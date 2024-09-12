# Oracle
Querys e rotinas que facilitam nosso dia a dia.
#### Pesquisar quais tabela tem um campo
```sql
SELECT  OWNER, TABLE_NAME, COLUMN_NAME, DATA_TYPE 
FROM ALL_TAB_COLUMNS
WHERE COLUMN_NAME like  '%EMPENHO%';
```



