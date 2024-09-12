# Oracle
Querys e rotinas que facilitam nosso dia a dia.
#### Pesquisar quais tabela tem um campo
```sql
SELECT  OWNER, TABLE_NAME, COLUMN_NAME, DATA_TYPE 
FROM ALL_TAB_COLUMNS
WHERE COLUMN_NAME like  '%EMPENHO%';
```

#### Listar Colunas de uma Tabela Específica
```sql
SELECT column_name, data_type, data_length, nullable
FROM all_tab_columns
WHERE table_name = 'NOME_DA_TABELA'
AND owner = 'NOME_DO_SCHEMA'
ORDER BY column_id;

```


####  Achar código em procedures (functions, triggers, packages, etc.)
```sql
SELECT name, type, text
FROM all_source
WHERE UPPER(text) LIKE '%TEXTO_PROCURADO%' AND owner = 'NOME_DO_SCHEMA';
```

####  Listar tabelas locadas (locked)
```sql
SELECT s.sid, s.serial#, lo.oracle_username, lo.os_user_name, lo.locked_mode, o.object_name
FROM v$locked_object lo, dba_objects o, v$session s
WHERE lo.object_id = o.object_id AND lo.session_id = s.sid;

```

####  Análise de Performance - Monitorar sessões ativas
```sql
SELECT sid, serial#, username, status, osuser, process, machine, program
FROM v$session
WHERE status = 'ACTIVE';

```

####  Análise de Performance - Identificar SQL com alto consumo de recursos
```sql
SELECT sql_id, sql_text, elapsed_time, cpu_time, buffer_gets, disk_reads, executions
FROM v$sql
ORDER BY elapsed_time DESC;

```

####  Análise de Performance - Analisar índices não utilizados
```sql
SELECT i.index_name, i.table_name
FROM dba_indexes i
JOIN dba_ind_usage u ON i.index_name = u.index_name
WHERE u.used = 'NO' AND i.owner = 'NOME_DO_SCHEMA';

```

####  Listar Relacionamentos entre Tabelas via Foreign Keys
```sql
SELECT a.constraint_name AS foreign_key,
       a.table_name AS child_table,
       a.column_name AS child_column,
       c_pk.table_name AS parent_table,
       c_pk.column_name AS parent_column
FROM all_cons_columns a
JOIN all_constraints c ON a.constraint_name = c.constraint_name
JOIN all_cons_columns c_pk ON c.r_constraint_name = c_pk.constraint_name
WHERE c.constraint_type = 'R'
AND a.owner = 'NOME_DO_SCHEMA'
ORDER BY a.table_name, a.column_name;


```

####  Listar todas as Foreign Keys em um Schema
```sql
SELECT constraint_name, table_name, r_constraint_name
FROM all_constraints
WHERE constraint_type = 'R'
AND owner = 'NOME_DO_SCHEMA'
ORDER BY table_name;

```

####  Obter Detalhes de Chaves Estrangeiras de uma Tabela Específica
```sql
SELECT a.constraint_name AS foreign_key,
       a.column_name AS child_column,
       c_pk.table_name AS parent_table,
       c_pk.column_name AS parent_column
FROM all_cons_columns a
JOIN all_constraints c ON a.constraint_name = c.constraint_name
JOIN all_cons_columns c_pk ON c.r_constraint_name = c_pk.constraint_name
WHERE c.constraint_type = 'R'
AND a.table_name = 'NOME_DA_TABELA'
ORDER BY a.column_name;


```

####  Listar Colunas Referenciadas e Referentes (Parent e Child Tables)
```sql
SELECT c.constraint_name AS foreign_key,
       c.table_name AS child_table,
       col.column_name AS child_column,
       r.table_name AS parent_table,
       rcol.column_name AS parent_column
FROM all_constraints c
JOIN all_cons_columns col ON c.constraint_name = col.constraint_name
JOIN all_constraints r ON c.r_constraint_name = r.constraint_name
JOIN all_cons_columns rcol ON r.constraint_name = rcol.constraint_name
WHERE c.constraint_type = 'R'
AND c.owner = 'NOME_DO_SCHEMA'
ORDER BY c.table_name;
```
