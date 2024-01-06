# Relational DB

## 1. Background

RDBMS를 사용할 때는 설계가 복잡해지면 테이블 사이의 연관관계, join기능이 필요

JPA에서 이를 보완하는 방법을 알아보고자 함

### 연관관계 매핑의 종류
 - 일대일, 일대다, 다대일, 다대다
  
다대다는 실무에서는 거의 쓰이지 않음

### 연관관계 매핑 방향
단방향과 양방향
 - 단방향은 한 엔티티가 다른 엔티티를 참조하지만 반대 방향의 참조는 일어나지 않는 관계 의미
 - ***단방향에서는 보통 외래키를 가진 테이블이 Owner가 된다***

## 2. One-to-one Directional Mapping

상품 엔티티 Product에 대해 ProductDetail이 Product를 참조하는 외래키를 가졌다고 하자
ProductDetail의 구현은 아래와 같이 이루어진다.

```java
@Entity
@Table(name="product_detail")
@Getter
@Setter
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
public class ProductDetail extends BaseEntity{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String description;

    @OneToOne
    @JoinColumn(name="product_number")
    private Product product;

    public ProductDetail(String description, Product product) {
        this.description = description;
        this.product = product;
    }
}
```

주목해야할 점은 @OneToOne과 @JoinColumn 어노테이션이다.

@OneToOne은 다른 엔티티 객체를 필드로 선언했을 때 일대일 연관관계로 매핑하게 해준다.

@JoinColumn은 매핑할 외래키를 설정한다. 기본값 대신 name을 직접 설정해주는 것이 좋다. 없으면 중간에 조인 테이블이 새로 생긴다.

ProductDetail 선언 후에는 이용을 위한 ProductDetailRepository를 선언해줘야 한다.
구현은 생략

```java
public void saveAndReadTest1(){
    Product product = new Product("pineapple",20,2);
    productRepository.save(product);

    ProductDetail productDetail = new ProductDetail("nice pen", product);
    productDetailRepository.save(productDetail);

    System.out.println(productDetailRepository.findById(productDetail.getId()).get());
}
```

위와 같이 예제를 작성하면 HeidiSQL을 통해 실제로 productDetail 테이블이 생기고 product_number가 product 테이블의 기본키처럼 Bigint형태로 생성되는 것을 확인할 수 있다.

위 예제에서 Hibernate에 의해 생성되는 쿼리를 보면 `product_detail` left outer join `product`로 from 부분이 작성되어 있는데, 이는 ProductDetail클래스에서 필드 product가 nullable이고 @OneToOne의 fetch 속성의 기본값이 즉시 로딩을 의미하는 Eager 이기 때문이다.

@OneToOne(optional=false) 로 수정하면 outer join에서 inner join으로 쿼리가 바뀌게 된다.

## 3. One-to-one Bidirectional Mapping

위의 예제에서 ProductDetail은 그대로 두고 Product 엔티티에 아래와 같이 필드를 추가해주면 된다.
```java
@OneToOne
@ToString.Exclude
private ProductDetail productDetail;
```

@ToString.Exclude는 순환참조를 막는 역할을 하는 필수적인 부분이다.

## 4. Many-to-one Directional Mapping

각 product마다 공급하는 provider 엔티티가 있어서 product와 provider가 다대일의 관계를 이룬다고 가정하자

product 엔티티에 다음과 같이 필드를 추가하면 product에서 provider를 참조할 수 있게 된다.

```java
@ManyToOne
@JoinColumn(name = "provider_id")
@ToString.Exclude
private Provider provider;
```

이렇게 하면 product 테이블에 provider_id라는 Bigint타입 칼럼이 생긴다.

## 5. Many-to-one Bidirectional Mapping

이번에는 반대로 provider 엔티티에 product리스트를 필드로 추가해서 양방향 매핑을 구현해보았다. Provider 엔티티의 구현 예제는 아래와 같다.

```java
@Entity
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
@Table(name = "provider")
public class Provider extends BaseEntity{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String name;

    @OneToMany(mappedBy = "provider", fetch = FetchType.EAGER)
    @ToString.Exclude
    private List<Product> productList = new ArrayList<>();

    public Provider(String name) {
        this.name = name;
    }
}
```

@OneToMany 어노테이션으로 product의 리스트를 필드로 선언하고 있다.

Eager로 즉시로딩을 선택하였고, List가 아니라 collection이나 map도 가능하다.

product와 provider의 연관 관계에서 주인은 product이기 때문에 실제 사용할 때는 아래 테스트와 같이 사용하게 된다. 

```java
@Test
public void test2(){
    Provider provider = new Provider("물산");
    providerRepository.save(provider);

    Product product1 = new Product("apple",20,2,provider);
    productRepository.save(product1);

    Product product2 = new Product("banana",20,2,provider);
    productRepository.save(product2);

    List<Product> l = providerRepository.findById(provider.getId()).get().getProductList();
    for(Product p : l){
        System.out.println(p);
    }
}
```
위와 다르게 provider.productlist.add(프로덕트) 형태로 구현하게 되면 이는 무시되게 되고 프로덕트는 실제로 테이블에 추가되지 않는다.

## 6. One-to-many Directional Mapping

One쪽이 주인이 되는 단방향 참조 관계로, 다대일과 다르게 주인의 list필드에 추가하는 것으로 update 쿼리가 실행된다.

category가 주인으로서 여러 개의 product를 갖는다고 할 때 구현은 아래와 같다.

```java
@Entity
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString(callSuper = true)
@EqualsAndHashCode
@Table(name = "category")
public class Category {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    @Column(unique = true)
    private String code;

    private String name;

    @OneToMany(fetch=FetchType.EAGER)
    @JoinColumn(name="category_id")
    private List<Product> products = new ArrayList<>();

    public Category(String code, String name) {
        this.code = code;
        this.name = name;
    }
}
```

@OneToMany와 @JoinColumn에 의해서 category_id라는 이름의 칼럼이 Product에 추가된다.

이는 각각의 Product가 product.category_id 에 속함을 나타낸다.

```java
@Test
void test1(){
    Product product = new Product("pen",10,20);
    productRepository.save(product);

    Product product2 = new Product("apple",102,20);
    productRepository.save(product2);

    Category category = new Category("02","objects");
    category.getProducts().add(product);
    category.getProducts().add(product2);
    categoryRepository.save(category);

    List<Product> l = categoryRepository.findById(category.getId()).get().getProducts();

    for(Product p : l){
        System.out.println(p);
    }
}
```

위 예제를 실행시켜보면 .add()별로 update가 실행됨을 볼 수 있다.

> 쿼리의 수를 줄이고 싶다면 다대일 관계를 쓰는 것이 바람직할 것이다.

일대다 양방향 매핑은 어느 엔티티 클래스도 주인이 될 수 없기 때문에 다루지 않는다.

## 7. Many-to-many Mapping

별로 권장되지는 않지만, 중간에 조인 테이블이 생성되는 방법으로 다대다 매핑을 구현할 수 있다.

## 8. 영속성 전이 (cascade)

어떤 엔티티의 영속성 상태가 변할 때 연관된 엔티티의 영속성도 함께 변하게 하는 것

종류
 - ALL : 모든 상태 변경에 대해 cascade
 - PERSIST : 엔티티가 영속화할 때 같이 영속화
 - MERGE : 영속성 컨텍스트에 병합할 때 같이 병합
 - REMOVE : 제거할 때 같이 제거
 - REFRESH : 엔티티를 새로고침할 때 연관 엔티티도 새로고침
 - DETACH : 영속성 컨텍스트에서 detach할 때 같이 detach

E.g. 일대다 매핑인 provider-product에서 provider의 속성을 `@OneToMany(mappedBy = "provider, cascade = CascadeType.PERSIST)` 로 하면 provider를 save할 때 연관된 product들이 모두 자동저장된다.

## 9. 고아 객체

JPA에서 고아(orphan)란 부모 엔티티와 연관관계가 끊어진 엔티티를 의미한다.  
JPA이는 고아 객체를 자동으로 삭제하는 기능이 있다.

주인 객체에서 orphanRemoval을 true로 설정하면 된다. e.g. `@OneToMany(mappedBy = "provider, cascade = CascadeType.PERSIST, orphanRemoval = true)`





