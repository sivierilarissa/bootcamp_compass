# Treinamento PostgreSQL e SQL

## 1. Comando SELECT
O comando `SELECT` é utilizado para selecionar colunas de tabelas.

### Sintaxe
```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1;
```

### Exemplos
* Listar os e-mails dos clientes da tabela `sales.customers`:
  ```sql
  SELECT email
  FROM sales.customers;
  ```
* Listar os e-mails e nomes dos clientes da tabela `sales.customers`:
  ```sql
  SELECT first_name, last_name, email
  FROM sales.customers;
  ```
* Listar todas as informações dos clientes da tabela `sales.customers`:
  ```sql
  SELECT *
  FROM sales.customers;
  ```

### Resumo
1. Comando usado para selecionar colunas de tabelas.
2. Quando selecionar mais de uma coluna, elas devem ser separadas por vírgula, sem conter vírgula antes do comando `FROM`.
3. Pode-se utilizar o asterisco (`*`) para selecionar todas as colunas da tabela.

---

## 2. Comando DISTINCT
O comando `DISTINCT` é utilizado para remover linhas duplicadas e mostrar apenas linhas distintas. É muito utilizado na etapa de exploração de bases de dados.

### Sintaxe
```sql
SELECT DISTINCT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1;
```

### Exemplos
* Listar as marcas de carros que constam na tabela `sales.products`:
  ```sql
  SELECT DISTINCT brand
  FROM sales.products;
  ```
* Listar as marcas e anos de modelo distintos que constam na tabela `sales.products`:
  ```sql
  SELECT DISTINCT brand, model_year
  FROM sales.products;
  ```

### Resumo
1. Comando usado para remover linhas duplicadas e mostrar apenas linhas distintas.
2. Muito utilizado na etapa de exploração dos dados.
3. Caso mais de uma coluna seja selecionada, o comando `SELECT DISTINCT` retornará todas as combinações distintas.

---

## 3. Comando WHERE
O comando `WHERE` é utilizado para filtrar linhas de acordo com uma condição lógica.

### Sintaxe
```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1
WHERE condicao_x = TRUE;
```

### Exemplos
* Listar os e-mails dos clientes que moram no estado de Santa Catarina:
  ```sql
  SELECT email, state
  FROM sales.customers
  WHERE state = 'SC';
  ```
* Listar os e-mails dos clientes que moram no estado de Santa Catarina ou Mato Grosso do Sul:
  ```sql
  SELECT email, state
  FROM sales.customers
  WHERE state = 'SC' OR state = 'MS';
  ```
* Listar os e-mails dos clientes que moram em Santa Catarina ou Mato Grosso do Sul e têm mais de 30 anos (nascidos após 1996-11-10):
  ```sql
  SELECT email, state, birth_date
  FROM sales.customers
  WHERE (state = 'SC' OR state = 'MS') AND birth_date > '1996-11-10';
  ```

### Resumo
1. Comando utilizado para filtrar linhas de acordo com uma condição.
2. No PostgreSQL, utilizam-se aspas simples para delimitar strings (sequência de caracteres / texto).
3. Pode-se combinar mais de uma condição utilizando operadores lógicos.
4. No PostgreSQL, as datas são escritas no formato `'YYYY-MM-DD'` ou `'YYYYMMDD'`.

---

## 4. Comando ORDER BY
O comando `ORDER BY` é utilizado para ordenar a seleção de acordo com uma regra definida pelo usuário.

### Sintaxe
```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1
WHERE condicao_x = TRUE
ORDER BY coluna_1;
```

### Exemplos
* Listar produtos da tabela `products` em ordem crescente com base no preço:
  ```sql
  SELECT *
  FROM sales.products
  ORDER BY price;
  ```
* Listar os estados distintos da tabela `customers` em ordem crescente:
  ```sql
  SELECT DISTINCT state
  FROM sales.customers
  ORDER BY state;
  ```

### Resumo
1. Comando utilizado para ordenar a seleção de acordo com uma regra definida.
2. Por padrão, o comando ordena em ordem crescente. Para mudar para a ordem decrescente, utilize o modificador `DESC`.
3. No caso de strings, a ordenação seguirá a ordem alfabética.

---

## 5. Comando LIMIT
O comando `LIMIT` é utilizado para limitar o número de linhas retornadas em uma consulta. É muito utilizado na etapa de exploração de dados e em conjunto com `ORDER BY` para obter registros do tipo *TOP N* (ex: "N pagamentos mais recentes", "N produtos mais caros").

### Sintaxe
```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1
LIMIT N;
```

### Exemplos
* Listar as 10 primeiras linhas da tabela `funnel`:
  ```sql
  SELECT *
  FROM sales.funnel
  LIMIT 10;
  ```
* Listar os 10 produtos mais caros da tabela `products`:
  ```sql
  SELECT *
  FROM sales.products
  ORDER BY price DESC
  LIMIT 10;
  ```

---

## 6. Operadores Aritméticos
Servindo para executar operações matemáticas, são muito utilizados para criar colunas calculadas.

| Operador | Operação |
| :---: | :--- |
| `+` | Adição |
| `-` | Subtração |
| `*` | Multiplicação |
| `/` | Divisão |
| `^` | Exponencial |
| `%` | Módulo |
| `\|\|` | Concatenação / Soma de strings |

### Exemplos
* Criar uma coluna contendo a idade do cliente na tabela `sales.customers`:
  ```sql
  SELECT 
      email, 
      birth_date,
      (CURRENT_DATE - birth_date) / 365 AS idade_cliente
  FROM sales.customers;
  ```
* Criar a coluna `nome_completo` unindo nome e sobrenome:
  ```sql
  SELECT 
      first_name || ' ' || last_name AS nome_completo
  FROM sales.customers;
  ```

### Resumo
1. Usados para executar operações matemáticas e criar colunas calculadas.
2. *Aliases* (pseudônimos) com a palavra-chave `AS` são utilizados para nomear colunas calculadas.
3. Para criar pseudônimos que contêm espaços, utilizam-se aspas duplas (`" "` ).
4. Para strings, o operador `||` realiza a concatenação.

---

## 7. Operadores de Comparação
Servindo para comparar dois valores e retornar `TRUE` ou `FALSE`, são muito utilizados em conjunto com o `WHERE` ou na criação de colunas *flag*.

| Operador | Descrição |
| :---: | :--- |
| `=` | Igual a |
| `>` | Maior que |
| `<` | Menor que |
| `>=` | Maior ou igual a |
| `<=` | Menor ou igual a |
| `<>` | Diferente de |

### Exemplo
* Criar uma coluna que retorne `TRUE` sempre que um cliente for um profissional CLT:
  ```sql
  SELECT 
      customer_id,
      first_name,
      professional_status,
      (professional_status = 'clt') AS cliente_clt
  FROM sales.customers;
  ```

---

## 8. Operadores Lógicos
Utilizados para unir expressões simples em uma composta.

| Operador | Descrição |
| :--- | :--- |
| `AND` | `TRUE` se todas as condições separadas por `AND` forem verdadeiras |
| `OR` | `TRUE` se alguma das condições separadas por `OR` for verdadeira |
| `NOT` | Inverte o resultado booleano da condição |
| `BETWEEN` | `TRUE` se o operando estiver dentro do intervalo comparativo |
| `IN` | `TRUE` se o operando for igual a algum item de uma lista |
| `LIKE` | `TRUE` se o operando corresponder a um padrão determinado |
| `ILIKE` | Igual ao `LIKE`, mas desconsidera letras maiúsculas e minúsculas |
| `IS NULL` | `TRUE` se a condição for igual a `NULL` (nulo) |

### Exemplos
* Selecionar veículos que custam entre 100k e 200k na tabela `products`:
  ```sql
  SELECT *
  FROM sales.products
  WHERE price BETWEEN 100000 AND 200000;
  ```
* Selecionar nomes que começam com "ANA" (Case-Sensitive vs Case-Insensitive):
  ```sql
  -- Case-sensitive
  SELECT *
  FROM sales.customers
  WHERE first_name LIKE 'ANA%';

  -- Case-insensitive (ignora maiúsculas e minúsculas)
  SELECT *
  FROM sales.customers
  WHERE first_name ILIKE 'ANA%';
  ```

---

## 9. Funções Agregadas
Executam operações aritméticas e estatísticas nos registros de uma coluna. Não computam células vazias (`NULL`) como zero.

* `COUNT()`: Conta quantas linhas existem em determinada pesquisa (o uso de `*` conta todas as linhas; `COUNT(DISTINCT coluna)` conta valores exclusivos).
* `SUM()`: Soma os valores de uma coluna.
* `MIN()`: Retorna o menor valor.
* `MAX()`: Retorna o maior valor.
* `AVG()`: Calcula a média aritmética.

### Exemplos
* Contagem total de visitas na tabela `funnel`:
  ```sql
  SELECT COUNT(*)
  FROM sales.funnel;
  ```
* Contagem de produtos distintos visitados em janeiro de 2021:
  ```sql
  SELECT COUNT(DISTINCT product_id)
  FROM sales.funnel
  WHERE visit_page_date BETWEEN '2021-01-01' AND '2021-01-31';
  ```

---

## 10. Agrupamento (GROUP BY) e Filtro Agrupado (HAVING)

### GROUP BY
Utilizado para agrupar registros semelhantes de uma coluna, normalmente em conjunto com funções de agregação.

```sql
SELECT state, COUNT(*) AS contagem
FROM sales.customers
GROUP BY state
ORDER BY contagem DESC;
```

### HAVING
Utilizado para filtrar linhas da seleção com base em uma coluna **agrupada** (diferente do `WHERE`, que atua antes do agrupamento).

```sql
SELECT state, COUNT(*) AS contagem
FROM sales.customers
GROUP BY state
HAVING COUNT(*) > 100;
```

---

## 11. Junções de Tabelas (JOINS)
Utilizadas para combinar colunas de uma ou mais tabelas com base em uma condição de correspondência.

![Esquema dos Joins em SQL](./img/sql_join.jpq)

* `LEFT JOIN`: Retorna todos os registros da tabela da esquerda e os correspondentes da tabela da direita.
* `INNER JOIN`: Retorna apenas os registros que possuem correspondência em ambas as tabelas.
* `RIGHT JOIN`: Retorna todos os registros da tabela da direita e os correspondentes da tabela da esquerda.
* `FULL JOIN`: Retorna todos os registros quando há uma correspondência em qualquer uma das tabelas.

### Sintaxe
```sql
SELECT t1.coluna_1, t2.coluna_2
FROM schema.tabela_1 AS t1
LEFT JOIN schema.tabela_2 AS t2
    ON t1.chave = t2.chave;
```

---

## 12. União de Tabelas (UNION / UNION ALL)
Utilizado para empilhar consultas. As tabelas devem possuir a mesma quantidade de colunas e tipos de dados compatíveis.

* `UNION`: Une as tabelas e remove linhas duplicadas após a conferência.
* `UNION ALL`: Une as tabelas mantendo todas as linhas, inclusive as duplicadas.

---

## 13. Subconsultas (Subqueries)
Consultas aninhadas utilizadas para reutilizar resultados de uma query em outra.

* **No WHERE**: Retorna um único valor, linha ou coluna para filtro.
* **Com WITH (CTEs)**: Melhora a legibilidade criando tabelas temporárias nomeadas no escopo da consulta.
* **No FROM**: Utiliza o resultado de uma consulta como uma tabela derivada.
* **No SELECT**: Avaliada linha a linha (pode ter impacto de performance, mas útil para acumulados).

### Exemplo com WITH (CTE)
```sql
WITH idade_profissional AS (
    SELECT
        professional_status,
        (CURRENT_DATE - birth_date) / 365 AS idade
    FROM sales.customers
)
SELECT
    professional_status,
    AVG(idade) AS idade_media
FROM idade_profissional
GROUP BY professional_status;
```

---

## 14. Conversão de Dados (`::` e `CAST`)
O SQL interpreta tipos nativamente, mas conversões explícitas são necessárias para operações específicas.

* Operador `::`: Sintaxe limpa e direta (ex: `'2021-10-01'::date`).
* Função `CAST()`: Padrão ANSI SQL (ex: `CAST('100' AS numeric)`).

---

## 15. Tratamento de Dados e Texto

### CASE WHEN
Cria lógica condicional para categorização e agrupamento de dados.
```sql
SELECT
    income,
    CASE
        WHEN income < 5000 THEN '0-5000'
        WHEN income >= 5000 AND income < 10000 THEN '5000-10000'
        ELSE '10000+'
    END AS faixa_renda
FROM sales.customers;
```

### COALESCE
Retorna o primeiro valor não nulo de uma lista de expressões.
```sql
SELECT COALESCE(population, (SELECT AVG(population) FROM temp_tables.regions)) AS populacao_ajustada
FROM temp_tables.regions;
```

### Funções de Texto
* `LOWER()`: Converte texto para letras minúsculas.
* `UPPER()`: Converte texto para letras maiúsculas.
* `TRIM()`: Remove espaços das extremidades.
* `REPLACE(coluna, 'antigo', 'novo')`: Substitui sequências de caracteres.

---

## 16. Tratamento de Data e Hora
* `INTERVAL`: Utilizado para somar ou subtrair períodos de tempo (`CURRENT_DATE + INTERVAL '10 weeks'`).
* `DATE_TRUNC`: Trunca uma data no início do período especificado (ex: `'month'`).
* `EXTRACT`: Extrai subpartes de datas/timestamps (ex: `EXTRACT('dow' FROM visit_page_date)`).

---

## 17. Funções Personalizadas (`CREATE FUNCTION`)
Permitem encapsular blocos de lógica reutilizáveis no banco de dados.

```sql
CREATE FUNCTION calcular_diferenca_dias(data_inicial DATE, data_final DATE)
RETURNS INTEGER
LANGUAGE sql
AS $$
    SELECT (data_final - data_inicial);
$$;
```

---

## 18. Manipulação de Tabelas e Registros (DDL e DML)

### Criação e Exclusão de Tabelas
```sql
-- Criar tabela a partir de uma query
SELECT customer_id INTO temp_tables.customers_age FROM sales.customers;

-- Criar tabela do zero
CREATE TABLE temp_tables.profissoes (
    professional_status VARCHAR,
    status_profissional VARCHAR
);

-- Excluir tabela
DROP TABLE temp_tables.profissoes;
```

### Inserção, Atualização e Deleção (DML)
* `INSERT INTO`: Insere registros.
* `UPDATE ... SET`: Atualiza registros existentes com base em um filtro `WHERE`.
* `DELETE FROM`: Remove registros com base em um filtro `WHERE`.
```sql
UPDATE temp_tables.profissoes
SET professional_status = 'intern'
WHERE status_profissional = 'estagiario';
```
```

---

Your PostgreSQL training notes have been successfully consolidated and polished into a clean Markdown file. Let me know if you are ready to create the next topics or push this to your repository!