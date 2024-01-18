# Pandas

## 1. DataFrame

### 간략한 기록들

 - chain of slice -> Dataframe의 값을 변경할 때 어떤 기존 값을 찾아서 바꾸려면
     - df.loc[df['some column']=='target'] 가 row 여러개를 가리키므로
     - df.loc[df['search column']=='keyword','update column'] = x 형식으로 값을 찾아서 업데이트할 수 있다. (해당 row가 유일하게 존재할 때)
 - list처럼 .loc[::-1]로 row를 뒤집을 수 있다
 - head나 tail은 추출 길이를 1로 해도 결과가 single row가 아니라 DF다
   - 그냥 iloc을 쓰자