# MySQL

## 1. table management

### querying table's information (columns, data types of each column)

```sql
SELECT 
TABLE_CATALOG,
TABLE_SCHEMA,
TABLE_NAME, 
COLUMN_NAME, 
DATA_TYPE,
CHARACTER_MAXIMUM_LENGTH
FROM INFORMATION_SCHEMA.COLUMNS
where TABLE_NAME = 'your table name here'
```