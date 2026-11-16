# Treinamento PostgreSQL + SQL

## Sumário

1. [Bases e Colunas Utilizadas](#1-bases-e-colunas-utilizadas)
2. [Comandos Básicos de Consulta](#2-comandos-básicos-de-consulta)
   - [SELECT](#21-select) · [DISTINCT](#22-distinct) · [WHERE](#23-where) · [ORDER BY](#24-order-by) · [LIMIT](#25-limit) · [Exercícios](#26-exercícios)
3. [Operadores](#3-operadores)
   - [Aritméticos](#31-operadores-aritméticos) · [Comparação](#32-operadores-de-comparação) · [Lógicos](#33-operadores-lógicos) · [Desafio](#34-desafio)
4. [Funções Agregadas, GROUP BY e HAVING](#4-funções-agregadas-group-by-e-having)
5. [JOINS](#5-joins)
6. [UNION](#6-union)
7. [Subquery](#7-subquery)
8. [Conversão de Unidades](#8-conversão-de-unidades)
9. [Tratamento de Dados](#9-tratamento-de-dados)
10. [Tratamento de Texto](#10-tratamento-de-texto)
11. [Tratamento de Data e Hora](#11-tratamento-de-data-e-hora)
12. [Funções](#12-funções)
13. [Manipulação de Tabelas](#13-manipulação-de-tabelas)

---

## 1. Bases e Colunas Utilizadas

Tabelas usadas nos exemplos e exercícios (colunas conforme citadas nas consultas):

| Schema.Tabela | Colunas citadas |
|---|---|
| `sales.customers` | `customer_id`, `first_name`, `last_name`, `email`, `state`, `city`, `birth_date`, `income`, `score`, `professional_status` |
| `sales.products` | `product_id`, `brand`, `model_year`, `price` |
| `sales.funnel` | `visit_id`, `customer_id`, `product_id`, `store_id`, `visit_page_date`, `paid_date`, `discount` |
| `sales.stores` | `store_id`, `store_name` |
| `temp_tables.regions` | `city`, `state`, `region`, `size`, `population` |
| `temp_tables.ibge_genders` | `first_name`, `gender` |
| `temp_tables.tabela_1` / `tabela_2` | `cpf`, `name`, `state` |
| `temp_tables.products_2` | mesma estrutura de `sales.products` |

---

## 2. Comandos Básicos de Consulta

### 2.1 SELECT

Serve para **selecionar colunas** de tabelas.

**Sintaxe**

```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1;
```

**Exemplos**

```sql
-- Liste os e-mails dos clientes da tabela sales.customers
SELECT email
FROM sales.customers;

-- Liste os nomes e e-mails dos clientes da tabela sales.customers
SELECT first_name, last_name, email
FROM sales.customers;

-- Liste todas as informações dos clientes da tabela sales.customers
SELECT *
FROM sales.customers;
```

**Resumo**

1. Comando usado para selecionar colunas de tabelas.
2. Ao selecionar mais de uma coluna, separe-as por vírgula, **sem** vírgula antes do `FROM`.
3. Pode-se usar o asterisco (`*`) para selecionar todas as colunas da tabela.

---

### 2.2 DISTINCT

Serve para **remover linhas duplicadas** e mostrar apenas linhas distintas. Muito usado na etapa de **exploração das bases**.

**Sintaxe**

```sql
SELECT DISTINCT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1;
```

**Exemplos**

```sql
-- Liste as marcas de carro que constam na tabela products
SELECT DISTINCT brand
FROM sales.products;

-- Liste as marcas e os anos de modelo distintos que constam na tabela products
SELECT DISTINCT brand, model_year
FROM sales.products;
```

**Resumo**

1. Remove linhas duplicadas e mostra apenas linhas distintas.
2. Muito utilizado na etapa de exploração dos dados.
3. Se mais de uma coluna for selecionada, retorna todas as **combinações distintas**.

---

### 2.3 WHERE

Serve para **filtrar linhas** de acordo com uma condição.

**Sintaxe**

```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1
WHERE condicao_x = TRUE;
```

**Exemplos**

```sql
-- Liste os e-mails dos clientes que moram em Santa Catarina
SELECT email, state
FROM sales.customers
WHERE state = 'SC';

-- Liste os e-mails dos clientes que moram em Santa Catarina ou Mato Grosso do Sul
SELECT email, state
FROM sales.customers
WHERE state = 'SC' OR state = 'MS';

-- Liste os e-mails dos clientes que moram em Santa Catarina ou Mato Grosso do Sul
-- e que têm mais de 30 anos (nascidos antes da data de corte)
SELECT email, state, birth_date
FROM sales.customers
WHERE (state = 'SC' OR state = 'MS')
  AND birth_date < '1996-11-10';
```

**Resumo**

1. Filtra linhas de acordo com uma condição.
2. No PostgreSQL, usam-se **aspas simples** para delimitar strings.
3. *String* = sequência de caracteres = texto.
4. É possível combinar mais de uma condição com operadores lógicos.
5. No PostgreSQL, as datas são escritas no formato `'YYYY-MM-DD'` ou `'YYYYMMDD'`.

---

### 2.4 ORDER BY

Serve para **ordenar** a seleção de acordo com uma regra definida pelo usuário.

**Sintaxe**

```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1
WHERE condicao_x = TRUE
ORDER BY coluna_1;
```

**Exemplos**

```sql
-- Liste os produtos da tabela products em ordem crescente de preço
SELECT *
FROM sales.products
ORDER BY price;

-- Liste os estados distintos da tabela customers em ordem crescente
SELECT DISTINCT state
FROM sales.customers
ORDER BY state;
```

**Resumo**

1. Ordena a seleção de acordo com uma regra definida.
2. Por padrão, a ordem é **crescente**. Para a ordem decrescente, use `DESC`.
3. Para strings, a ordenação segue a **ordem alfabética**.

---

### 2.5 LIMIT

Serve para **limitar o número de linhas** da consulta. Muito usado na etapa de exploração dos dados.

**Sintaxe**

```sql
SELECT coluna_1, coluna_2, coluna_3
FROM schema_1.tabela_1
LIMIT N;
```

**Exemplos**

```sql
-- Liste as 10 primeiras linhas da tabela funnel
SELECT *
FROM sales.funnel
LIMIT 10;

-- Liste os 10 produtos mais caros da tabela products
SELECT *
FROM sales.products
ORDER BY price DESC
LIMIT 10;
```

**Resumo**

1. Limita o número de linhas da consulta.
2. Muito utilizado na exploração dos dados.
3. Muito usado em conjunto com `ORDER BY` quando importam os **TOP N** (ex.: "N pagamentos mais recentes", "N produtos mais caros").

---

### 2.6 Exercícios

**Exercício 1.** Selecione os nomes de cidades distintas que existem no estado de Minas Gerais, em ordem alfabética (tabela `sales.customers`).

```sql
SELECT DISTINCT city
FROM sales.customers
WHERE state = 'MG'
ORDER BY city;
```

**Exercício 2.** Selecione o `visit_id` das 10 compras mais recentes efetuadas (tabela `sales.funnel`).

```sql
SELECT visit_id, paid_date
FROM sales.funnel
WHERE paid_date IS NOT NULL
ORDER BY paid_date DESC
LIMIT 10;
```

**Exercício 3.** Selecione todos os dados dos 10 clientes com maior `score` nascidos após 01/01/2000 (tabela `sales.customers`).

```sql
SELECT *
FROM sales.customers
WHERE birth_date >= '2000-01-01'
ORDER BY score DESC
LIMIT 10;
```

---

## 3. Operadores

### 3.1 Operadores aritméticos

Servem para executar **operações matemáticas**. Muito utilizados para criar **colunas calculadas**.

| Operador | Operação |
|---|---|
| `+` | Adição |
| `-` | Subtração |
| `*` | Multiplicação |
| `/` | Divisão |
| `^` | Exponencial |
| `%` | Módulo |
| `\|\|` | Soma (concatenação) de strings |

**Exemplos**

```sql
-- Crie uma coluna contendo a idade do cliente da tabela sales.customers
SELECT
    email,
    birth_date,
    (current_date - birth_date) / 365 AS idade_cliente
FROM sales.customers;

-- Liste os 10 clientes mais novos da tabela customers
SELECT
    email,
    birth_date,
    (current_date - birth_date) / 365 AS idade_cliente
FROM sales.customers
ORDER BY idade_cliente
LIMIT 10;

-- Crie a coluna "nome_completo" contendo o nome completo do cliente
-- (o operador || concatena as strings)
SELECT
    first_name || ' ' || last_name AS nome_completo
FROM sales.customers;
```

**Resumo**

1. Executam operações matemáticas.
2. Muito utilizados para criar colunas calculadas.
3. **Aliases** (pseudônimos) são muito usados para dar nome às colunas calculadas.
4. Aliases que contêm espaços no nome usam **aspas duplas**.
5. Com strings, o operador `||` **concatena**.
6. Consulte o Guia de comandos para ver os demais operadores do SQL.

---

### 3.2 Operadores de comparação

Servem para **comparar dois valores**, retornando `TRUE` ou `FALSE`. Muito usados com o `WHERE` para filtrar linhas.

| Operador | Significado |
|---|---|
| `=` | Igual a |
| `>` | Maior que |
| `<` | Menor que |
| `>=` | Maior ou igual a |
| `<=` | Menor ou igual a |
| `<>` | Diferente de |

**Exemplo**

```sql
-- Crie uma coluna que retorne TRUE sempre que o cliente for profissional CLT
SELECT
    customer_id,
    first_name,
    professional_status,
    (professional_status = 'clt') AS cliente_clt  -- compara e retorna booleano
FROM sales.customers;
```

**Resumo**

1. Comparam dois valores, retornando `TRUE` ou `FALSE`.
2. Muito usados com `WHERE` para filtrar linhas.
3. Úteis para criar colunas **flag** (`TRUE`/`FALSE`).
4. Consulte o Guia de comandos para ver os demais operadores.

---

### 3.3 Operadores lógicos

Usados para **unir expressões simples em uma composta**.

| Operador | Retorna `TRUE` quando... |
|---|---|
| `AND` | todas as condições separadas por `AND` forem `TRUE` |
| `OR` | alguma condição separada por `OR` for `TRUE` |
| `NOT` | a condição mencionada em seguida **não** for `TRUE` |
| `BETWEEN` | o operando estiver dentro do intervalo comparativo |
| `IN` | o operando for igual a algum item de uma lista |
| `LIKE` | o operando corresponder a um padrão determinado |
| `ILIKE` | igual ao `LIKE`, mas **desconsidera maiúsculas e minúsculas** |
| `IS NULL` | o valor for nulo |

**Exemplos**

```sql
-- Veículos que custam entre 100k e 200k
SELECT *
FROM sales.products
WHERE price BETWEEN 100000 AND 200000;

-- Veículos que custam abaixo de 100k ou acima de 200k
SELECT *
FROM sales.products
WHERE price NOT BETWEEN 100000 AND 200000;

-- Produtos das marcas HONDA, TOYOTA ou RENAULT
SELECT *
FROM sales.products
WHERE brand IN ('HONDA', 'TOYOTA', 'RENAULT');

-- Primeiros nomes distintos que COMEÇAM com ANA
SELECT DISTINCT first_name
FROM sales.customers
WHERE first_name LIKE 'ANA%';

-- Primeiros nomes distintos que TERMINAM com ANA
SELECT DISTINCT first_name
FROM sales.customers
WHERE first_name LIKE '%ANA';

-- Primeiros nomes distintos que começam com 'ana' (ILIKE ignora maiúsculas/minúsculas)
SELECT DISTINCT first_name
FROM sales.customers
WHERE first_name ILIKE 'ana%';

-- Linhas com valor nulo no campo "population" da tabela temp_tables.regions
SELECT *
FROM temp_tables.regions
WHERE population IS NULL;
```

**Resumo**

1. Unem expressões simples em uma composta.
2. `AND`: verifica se duas comparações são **simultaneamente** verdadeiras.
3. `OR`: verifica se **uma ou outra** comparação é verdadeira.
4. `BETWEEN`: verifica quais valores estão dentro do intervalo definido.
5. `IN`: funciona como **múltiplos `OR`**.
6. `LIKE` e `ILIKE`: comparam textos e são sempre usados com o operador `%`, que funciona como **coringa** (qualquer texto pode aparecer no lugar).
7. `ILIKE` ignora letras maiúsculas e minúsculas na comparação.
8. `IS NULL`: verifica se o campo é nulo.
9. Consulte o Guia de comandos para ver os demais operadores.

---

### 3.4 Desafio

**Exercício 1.** Calcule quantos salários mínimos cada cliente da tabela `sales.customers` ganha. Selecione `email`, `income` e a coluna calculada "salários mínimos". Considere o salário mínimo igual a R$ 1.200.

```sql
SELECT
    email,
    income,
    income / 1200 AS "salários mínimos"
FROM sales.customers;
```

**Exercício 2.** Na query anterior, acrescente uma coluna informando `TRUE` se o cliente ganha acima de 4 salários mínimos e `FALSE` caso contrário. Chame a nova coluna de "acima de 4 salários".

```sql
SELECT
    email,
    income,
    income / 1200 AS "salários mínimos",
    (income / 1200) > 4 AS "acima de 4 salários"
FROM sales.customers;
```

**Exercício 3.** Na query anterior, filtre apenas os clientes que ganham entre 4 e 5 salários mínimos, usando `BETWEEN`.

```sql
SELECT
    email,
    income,
    income / 1200 AS "salários mínimos",
    (income / 1200) > 4 AS "acima de 4 salários"
FROM sales.customers
WHERE (income / 1200) BETWEEN 4 AND 5;
```

**Exercício 4.** Selecione e-mail, cidade e estado dos clientes que moram em Minas Gerais e Mato Grosso.

```sql
SELECT email, state, city
FROM sales.customers
WHERE state IN ('MG', 'MT');
```

> Mato Grosso é `MT`. A sigla `MS` corresponde a Mato Grosso do Sul.

**Exercício 5.** Selecione e-mail, cidade e estado dos clientes que **não** moram em São Paulo.

```sql
SELECT email, state, city
FROM sales.customers
WHERE state NOT IN ('SP');
```

**Exercício 6.** Selecione os nomes das cidades que começam com a letra Z (tabela `temp_tables.regions`).

```sql
SELECT *
FROM temp_tables.regions
WHERE city ILIKE 'Z%';
```

---

## 4. Funções Agregadas, GROUP BY e HAVING

### 4.1 Funções agregadas

Servem para executar **operações aritméticas nos registros de uma coluna**.

| Função | O que faz |
|---|---|
| `COUNT()` | Conta quantas linhas existem em determinada pesquisa |
| `SUM()` | Soma os valores |
| `MIN()` | Menor valor |
| `MAX()` | Maior valor |
| `AVG()` | Média |

**Exemplos**

```sql
-- Contagem de todas as linhas de uma tabela
-- Conte todas as visitas realizadas ao site da empresa fictícia
SELECT COUNT(*)  -- o * conta as linhas
FROM sales.funnel;

-- Contagem das linhas de uma coluna
-- Conte todos os pagamentos registrados na tabela sales.funnel
SELECT COUNT(paid_date)
FROM sales.funnel;

-- Conte todos os produtos distintos visitados em jan/2021
SELECT COUNT(DISTINCT product_id)
FROM sales.funnel
WHERE visit_page_date BETWEEN '2021-01-01' AND '2021-01-31';

-- Calcule o preço mínimo, máximo e médio dos produtos da tabela products
SELECT
    MIN(price),
    MAX(price),
    AVG(price)
FROM sales.products;

-- Informe qual é o veículo mais caro da tabela products
SELECT *
FROM sales.products
WHERE price = (SELECT MAX(price) FROM sales.products);
```

**Resumo**

1. Executam operações aritméticas nos registros de uma coluna.
2. Funções agregadas **não computam células vazias (`NULL`) como zero**.
3. Em `COUNT()`, pode-se usar o asterisco (`*`) para contar os registros.
4. `COUNT(DISTINCT ...)` conta apenas os valores exclusivos.

---

### 4.2 GROUP BY

Serve para **agrupar registros semelhantes** de uma coluna. Normalmente é usado em conjunto com as funções de agregação.

**Exemplos**

```sql
-- Contagem agrupada de uma coluna
-- Calcule o nº de clientes da tabela customers por estado
SELECT state, COUNT(*) AS contagem
FROM sales.customers
GROUP BY state
ORDER BY contagem DESC;

-- Contagem agrupada de várias colunas
-- Calcule o nº de clientes por estado e status profissional
SELECT state, professional_status, COUNT(*) AS contagem
FROM sales.customers
GROUP BY state, professional_status
ORDER BY state, contagem DESC;

-- Seleção de valores distintos
-- Selecione os estados distintos da tabela customers usando GROUP BY
SELECT state
FROM sales.customers
GROUP BY state;
```

**Resumo**

1. Agrupa registros semelhantes de uma coluna.
2. Normalmente usado com funções de agregação.
3. Pode-se referenciar a coluna a ser agrupada pela **posição ordinal** no `SELECT` (ex.: `GROUP BY 1, 2, 3` agrupa pelas 3 primeiras colunas selecionadas).
4. O `GROUP BY` sozinho funciona como um `DISTINCT`, eliminando linhas duplicadas.

---

### 4.3 HAVING

Serve para **filtrar linhas da seleção por uma coluna agrupada**, diferente do `WHERE`.

**Exemplo**

```sql
-- Calcule o nº de clientes por estado, filtrando apenas estados com mais de 100 clientes
SELECT
    state,
    COUNT(*)
FROM sales.customers
GROUP BY state
HAVING COUNT(*) > 100;
```

**Resumo**

1. Tem a mesma função do `WHERE`, mas pode filtrar o resultado das **funções agregadas**, o que o `WHERE` não consegue fazer.
2. O `HAVING` também pode filtrar colunas não agregadas.

---

### 4.4 Exercícios

**Exercício 1.** Conte quantos clientes da tabela `sales.customers` têm menos de 30 anos.

```sql
SELECT COUNT(*)
FROM sales.customers
WHERE (current_date - birth_date) / 365 < 30;
```

**Exercício 2.** Informe a idade do cliente mais velho e do mais novo da tabela `sales.customers`.

```sql
SELECT
    MAX((current_date - birth_date) / 365),
    MIN((current_date - birth_date) / 365)
FROM sales.customers;
```

**Exercício 3.** Selecione todas as informações do cliente mais rico da tabela `sales.customers` (a resposta pode conter mais de um cliente).

```sql
SELECT *
FROM sales.customers
WHERE income = (SELECT MAX(income) FROM sales.customers);
```

**Exercício 4.** Conte quantos veículos de cada marca estão registrados na tabela `sales.products`. Ordene pelo nome da marca.

```sql
SELECT brand, COUNT(*)
FROM sales.products
GROUP BY brand
ORDER BY brand;
```

**Exercício 5.** Conte quantos veículos existem por marca e ano do modelo. Ordene pelo nome da marca e pelo ano do veículo.

```sql
SELECT brand, model_year, COUNT(*)
FROM sales.products
GROUP BY brand, model_year
ORDER BY brand, model_year;
```

**Exercício 6.** Conte quantos veículos de cada marca estão registrados e mostre apenas as marcas com **mais de 10** veículos.

```sql
SELECT brand, COUNT(*)
FROM sales.products
GROUP BY brand
HAVING COUNT(*) > 10
ORDER BY COUNT(*);
```

---

## 5. JOINS

Servem para **combinar colunas de uma ou mais tabelas**. A tabela da esquerda é declarada primeiro (`FROM`) e a da direita depois (`JOIN`).

**Sintaxe**

```sql
SELECT t1.coluna_1, t1.coluna_2, t2.coluna_1, t2.coluna_2
FROM schema.tabela_1 AS t1
[TIPO_DE_JOIN] JOIN schema.tabela_2 AS t2
    ON condicao_de_join;
```

| Tipo de join | O que retorna |
|---|---|
| `INNER JOIN` | Apenas os registros presentes em **ambas** as tabelas |
| `LEFT JOIN` | Todos os registros da tabela da **esquerda** + correspondentes da direita |
| `RIGHT JOIN` | Todos os registros da tabela da **direita** + correspondentes da esquerda |
| `FULL JOIN` | Todos os registros de **ambas** as tabelas |

**Exemplos**

```sql
-- LEFT JOIN entre temp_tables.tabela_1 e temp_tables.tabela_2
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
LEFT JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;

-- INNER JOIN
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
INNER JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;

-- RIGHT JOIN
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
RIGHT JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;

-- FULL JOIN
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
FULL JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;
```

**Resumo**

1. Combinam colunas de uma ou mais tabelas.
2. É possível chamar todas as colunas com `*`, mas **não é recomendado**.
3. É boa prática criar **aliases** para nomear as tabelas.
4. O `JOIN` sozinho funciona como `INNER JOIN`.

### 5.1 Exercícios

**Exercício 1.** Identifique qual é o status profissional mais frequente nos clientes que compraram automóveis no site.

```sql
SELECT
    cus.professional_status,
    COUNT(fun.paid_date) AS pagamentos
FROM sales.funnel AS fun
LEFT JOIN sales.customers AS cus
    ON fun.customer_id = cus.customer_id
GROUP BY cus.professional_status
ORDER BY pagamentos DESC;
```

**Exercício 2.** Identifique qual é o gênero mais frequente nos clientes que compraram automóveis no site (use a tabela `temp_tables.ibge_genders`).

```sql
SELECT
    ibge.gender,
    COUNT(fun.paid_date)
FROM sales.funnel AS fun
LEFT JOIN sales.customers AS cus
    ON fun.customer_id = cus.customer_id
LEFT JOIN temp_tables.ibge_genders AS ibge
    ON LOWER(cus.first_name) = ibge.first_name
GROUP BY ibge.gender;
```

**Exercício 3.** Identifique de quais regiões são os clientes que mais visitam o site (use a tabela `temp_tables.regions`).

```sql
-- Exploração das tabelas
SELECT * FROM sales.customers LIMIT 10;
SELECT * FROM temp_tables.regions LIMIT 10;

-- Consulta final
SELECT
    reg.region,
    COUNT(fun.visit_page_date) AS visitas
FROM sales.funnel AS fun
LEFT JOIN sales.customers AS cus
    ON fun.customer_id = cus.customer_id
LEFT JOIN temp_tables.regions AS reg
    ON LOWER(cus.city) = LOWER(reg.city)
   AND LOWER(cus.state) = LOWER(reg.state)
GROUP BY reg.region
ORDER BY visitas DESC;
```

### 5.2 Exercícios adicionais

**Exercício 1.** Identifique quais são as marcas de veículo mais visitadas na tabela `sales.funnel`.

```sql
SELECT
    pro.brand,
    COUNT(*) AS visitas
FROM sales.funnel AS fun
LEFT JOIN sales.products AS pro
    ON fun.product_id = pro.product_id
GROUP BY pro.brand
ORDER BY visitas DESC;
```

**Exercício 2.** Identifique quais são as lojas de veículo mais visitadas na tabela `sales.funnel`.

```sql
SELECT
    sto.store_name,
    COUNT(*) AS visitas
FROM sales.funnel AS fun
LEFT JOIN sales.stores AS sto
    ON fun.store_id = sto.store_id
GROUP BY sto.store_name
ORDER BY visitas DESC;
```

**Exercício 3.** Identifique quantos clientes moram em cada tamanho de cidade (o porte consta na coluna `size` da tabela `temp_tables.regions`).

```sql
SELECT
    reg.size,
    COUNT(*) AS contagem
FROM sales.customers AS cus
LEFT JOIN temp_tables.regions AS reg
    ON LOWER(cus.city) = LOWER(reg.city)
   AND LOWER(cus.state) = LOWER(reg.state)
GROUP BY reg.size
ORDER BY contagem;
```

---

## 6. UNION

Serve para **colar uma tabela sobre a outra**, desde que as duas tenham a **mesma quantidade de colunas** e os **mesmos tipos de dados**.

| Comando | Comportamento |
|---|---|
| `UNION` | Une as tabelas e **remove as linhas repetidas** após a conferência |
| `UNION ALL` | Une as tabelas e **mantém as linhas repetidas** (duplicadas) |

**Sintaxe**

```sql
SELECT coluna_1, coluna_2
FROM schema_1.tabela_1
UNION  -- ou UNION ALL
SELECT coluna_3, coluna_4
FROM schema_2.tabela_2;
```

**Exemplo**

```sql
-- Una a tabela sales.products com a tabela temp_tables.products_2
SELECT * FROM sales.products
UNION ALL
SELECT * FROM temp_tables.products_2;
```

---

## 7. Subquery

Servem para **consultar dados de outras consultas**, usando o resultado de uma query dentro de outra.

### 7.1 Tipos

| Tipo | Observações |
|---|---|
| **Subquery no `WHERE`** | Retorna sempre **um único valor** (uma linha e uma coluna). |
| **Subquery com `WITH`** | Forma mais recomendada: deixa a query mais legível. |
| **Subquery no `FROM`** | Pouco utilizada; prefira o `WITH`. |
| **Subquery no `SELECT`** | Pesada e pouco utilizada, pois roda **linha por linha**. Mas às vezes é o único meio de responder a uma pergunta (ex.: acumular dados do registro). |

### 7.2 Exemplos

**Subquery no `WHERE`:** informe qual é o veículo mais barato da tabela products.

```sql
SELECT *
FROM sales.products
WHERE price = (SELECT MIN(price) FROM sales.products);
```

**Subquery com `WITH`:** calcule a idade média dos clientes por status profissional.

```sql
WITH alguma_tabela AS (
    SELECT
        professional_status,
        (current_date - birth_date) / 365 AS idade
    FROM sales.customers
)
SELECT
    professional_status,
    AVG(idade) AS idade_media
FROM alguma_tabela
GROUP BY professional_status;
```

**Subquery no `FROM`:** mesma análise do exemplo anterior.

```sql
SELECT
    professional_status,
    AVG(idade) AS idade_media
FROM (
    SELECT
        professional_status,
        (current_date - birth_date) / 365 AS idade
    FROM sales.customers
) AS alguma_tabela
GROUP BY professional_status;
```

**Subquery no `SELECT`:** na tabela `sales.funnel`, crie uma coluna com o nº de visitas **acumuladas** que a loja visitada recebeu até o momento.

```sql
SELECT
    fun.visit_id,
    fun.visit_page_date,
    sto.store_name,
    (
        SELECT COUNT(*)
        FROM sales.funnel AS fun2
        WHERE fun2.visit_page_date <= fun.visit_page_date
          AND fun2.store_id = fun.store_id
    ) AS visitas_acumuladas
FROM sales.funnel AS fun
LEFT JOIN sales.stores AS sto
    ON fun.store_id = sto.store_id
ORDER BY sto.store_name, fun.visit_page_date;
```

**Resumo**

1. Servem para consultar dados de outras consultas.
2. Para funcionarem no `WHERE` e no `SELECT`, devem retornar **apenas um único valor**.
3. Não é recomendado usar subqueries diretamente no `FROM`, pois dificultam a legibilidade da query.

### 7.3 Exemplos com `WITH`

**Análise de recorrência dos leads:** calcule o volume de visitas por dia ao site, separado por 1ª visita e demais visitas.

```sql
WITH primeira_visita AS (
    SELECT customer_id, MIN(visit_page_date) AS visita_1
    FROM sales.funnel
    GROUP BY customer_id
)
SELECT
    fun.visit_page_date,
    (fun.visit_page_date <> primeira_visita.visita_1) AS lead_recorrente,
    COUNT(*)
FROM sales.funnel AS fun
LEFT JOIN primeira_visita
    ON fun.customer_id = primeira_visita.customer_id
GROUP BY fun.visit_page_date, lead_recorrente
ORDER BY fun.visit_page_date DESC, lead_recorrente;
```

**Análise do preço versus o preço médio:** calcule, para cada visita ao site, quanto o preço do veículo visitado estava acima ou abaixo do preço médio dos veículos daquela marca (considerando o desconto dado no veículo).

```sql
WITH preco_medio AS (
    SELECT brand, AVG(price) AS preco_medio_da_marca
    FROM sales.products
    GROUP BY brand
)
SELECT
    fun.visit_id,
    fun.visit_page_date,
    pro.brand,
    (pro.price * (1 + fun.discount)) AS preco_final,
    preco_medio.preco_medio_da_marca,
    ((pro.price * (1 + fun.discount)) - preco_medio.preco_medio_da_marca) AS preco_vs_media
FROM sales.funnel AS fun
LEFT JOIN sales.products AS pro
    ON fun.product_id = pro.product_id
LEFT JOIN preco_medio
    ON pro.brand = preco_medio.brand;
```

**Coluna calculada com o nº de visitas por cliente** (tabela `sales.customers`).

```sql
WITH numero_de_visitas AS (
    SELECT customer_id, COUNT(*) AS n_visitas
    FROM sales.funnel
    GROUP BY customer_id
)
SELECT
    cus.*,
    n_visitas
FROM sales.customers AS cus
LEFT JOIN numero_de_visitas AS ndv
    ON cus.customer_id = ndv.customer_id;
```

---

## 8. Conversão de Unidades

O SQL reconhece valores literais como **texto** automaticamente. O operador `::` seguido do tipo de dado converte o valor para o novo tipo, permitindo que a operação aconteça.

### 8.1 Tipos de conversão

| Forma | Observação |
|---|---|
| Operador `::` | Mais "limpo"; use sempre que possível |
| `CAST()` | Use quando o `::` não funcionar |

**Sintaxe do `CAST`**

```sql
SELECT CAST('PRIMEIRO VALOR' AS UNIDADE) - CAST('SEGUNDO VALOR' AS UNIDADE);
```

### 8.2 Exemplos

```sql
-- Conversão de texto em data (operador ::)
SELECT '2021-10-01'::date - '2021-02-01'::date;

-- Conversão de texto em número (operador ::)
SELECT '100'::numeric - '10'::numeric;

-- Conversão de número em texto (operador ::)
SELECT REPLACE(112122::text, '1', 'A');

-- Conversão de texto em data (função CAST)
SELECT CAST('2021-10-01' AS date) - CAST('2021-02-01' AS date);
```

> **`REPLACE`:** 1º argumento = coluna (ou valor); 2º = sequência de caracteres a ser substituída; 3º = o que entra no lugar do 2º.

**Resumo**

1. O operador `::` e o `CAST()` convertem um dado para a unidade desejada.
2. O `::` é mais "clean", mas em algumas ocasiões não funciona, sendo necessário usar `CAST()`.
3. Use o Guia de comandos para consultar a lista de unidades mais utilizadas.

---

## 9. Tratamento de Dados

### 9.1 Tipos

| Comando | Função |
|---|---|
| `CASE WHEN` | Cria respostas específicas para diferentes condições; muito usado para **agrupar dados** |
| `COALESCE()` | Procura nulos nos dados declarados; se não encontrar, passa ao próximo item e **preenche com o primeiro valor não nulo** da sequência |

**Estrutura do `CASE WHEN`**

```sql
CASE
    WHEN condicao_1 THEN 'retorno_1'
    WHEN condicao_2 THEN 'retorno_2'
    ELSE 'retorno_padrao'
END AS nome_coluna
```

### 9.2 Exemplos

**Agrupamento com `CASE WHEN`:** calcule o nº de clientes que ganham abaixo de 5k, entre 5k e 10k, entre 10k e 15k e acima de 15k.

```sql
WITH faixa_de_renda AS (
    SELECT
        income,
        CASE
            WHEN income < 5000 THEN '0-5000'
            WHEN income >= 5000 AND income < 10000 THEN '5000-10000'
            WHEN income >= 10000 AND income < 15000 THEN '10000-15000'
            ELSE '15000+'
        END AS faixa_renda
    FROM sales.customers
)
SELECT faixa_renda, COUNT(*)
FROM faixa_de_renda
GROUP BY faixa_renda;
```

**Tratamento de nulos com `COALESCE`:** crie a coluna `populacao_ajustada` na tabela `temp_tables.regions`, preenchida com `population`; se for nulo, use a população média geral das cidades do Brasil.

```sql
-- Exploração
SELECT * FROM temp_tables.regions LIMIT 10;

-- Consulta final
SELECT
    *,
    COALESCE(population, (SELECT AVG(population) FROM temp_tables.regions)) AS populacao_ajustada
FROM temp_tables.regions;
```

**Resumo**

1. `CASE WHEN`: cria respostas específicas para diferentes condições; muito usado para agrupamento de dados.
2. `COALESCE`: preenche campos nulos com o primeiro valor não nulo de uma sequência de valores.

---

## 10. Tratamento de Texto

### 10.1 Funções

| Função | Para que serve |
|---|---|
| `LOWER()` | Transforma todo o texto em **minúsculas** |
| `UPPER()` | Transforma todo o texto em **maiúsculas** |
| `TRIM()` | Remove os **espaços das extremidades** do texto |
| `REPLACE()` | **Substitui** uma string por outra (útil, por exemplo, para acentos) |

**Sintaxe do `REPLACE`**

```sql
REPLACE('texto original', 'trecho a substituir', 'trecho novo')
```

### 10.2 Exemplos

Todas as consultas abaixo devem retornar `TRUE`:

```sql
SELECT UPPER('São Paulo') = 'SÃO PAULO';
SELECT LOWER('São Paulo') = 'são paulo';
SELECT TRIM('SÃO PAULO     ') = 'SÃO PAULO';
SELECT REPLACE('SAO PAULO', 'SAO', 'SÃO') = 'SÃO PAULO';
```

---

## 11. Tratamento de Data e Hora

### 11.1 Tipos

| Recurso | Função |
|---|---|
| `INTERVAL` | Soma datas na unidade desejada |
| `DATE_TRUNC` | Trunca uma data no início do período |
| `EXTRACT` | Extrai unidades de uma data/timestamp |
| `DATEDIFF` | Diferença entre datas em uma unidade. **Não é nativa do PostgreSQL**: é criada pelo usuário (ver [seção 12](#12-funções)) |

### 11.2 Exemplos

**Soma de datas com `INTERVAL`:** calcule a data de hoje mais 10 unidades (dias, semanas, meses, horas).

```sql
SELECT current_date + (INTERVAL '10 weeks');  -- também: days, months, hours
```

**Truncagem de datas com `DATE_TRUNC`:** calcule quantas visitas ocorreram por mês no site.

```sql
SELECT
    DATE_TRUNC('month', visit_page_date)::date AS visit_page_month,
    -- 1º argumento: a unidade de truncagem; 2º argumento: a coluna de data
    COUNT(*)
FROM sales.funnel
GROUP BY visit_page_month
ORDER BY visit_page_month DESC;
```

**Extração com `EXTRACT`:** calcule qual é o dia da semana que mais recebe visitas.

```sql
SELECT
    EXTRACT('dow' FROM visit_page_date) AS dia_da_semana,
    -- unidade a extrair ('dow' = day of week), FROM, de onde extrair
    COUNT(*)
FROM sales.funnel
GROUP BY dia_da_semana
ORDER BY dia_da_semana;
```

> No PostgreSQL, `dow` retorna de 0 (domingo) a 6 (sábado).

**Diferença entre datas com o operador `-`:** calcule a diferença entre hoje e `'2018-06-01'` em dias, semanas, meses e anos.

```sql
SELECT (current_date - '2018-06-01');         -- dias
SELECT (current_date - '2018-06-01') / 7;     -- semanas
SELECT (current_date - '2018-06-01') / 30;    -- meses
SELECT (current_date - '2018-06-01') / 365;   -- anos

-- Usando a função DATEDIFF criada na seção 12
SELECT DATEDIFF('weeks', '2018-06-01', current_date);
```

**Resumo**

1. `INTERVAL` soma datas na unidade desejada. Se a unidade não for informada, o SQL entende que a soma é em **dias**.
2. `DATE_TRUNC` trunca uma data no início do período.
3. `EXTRACT` extrai unidades de uma data/timestamp.
4. A subtração de datas (`-`) retorna valores em **dias**. Para outras unidades, é preciso transformar a unidade (ou criar uma função para isso).
5. Use o Guia de comandos para consultar as unidades de data e hora do SQL.

---

## 12. Funções

Servem para criar **comandos personalizados** a partir de scripts usados recorrentemente.

**Sintaxe**

```sql
CREATE FUNCTION nome_da_funcao (variavel_entrada tipo, data_inicial date, data_final date)  -- variáveis de entrada
RETURNS tipo_de_saida  -- exemplo: integer
LANGUAGE sql           -- como queremos que a função seja lida
AS
$$
    -- script da função
$$;
```

**Exemplo:** crie uma função chamada `DATEDIFF` para calcular a diferença entre duas datas em dias, semanas, meses e anos.

```sql
CREATE FUNCTION datediff (unidade varchar, data_inicial date, data_final date)
RETURNS integer
LANGUAGE sql
AS
$$
    SELECT
        CASE
            WHEN unidade IN ('d', 'day', 'days')       THEN (data_final - data_inicial)
            WHEN unidade IN ('w', 'week', 'weeks')     THEN (data_final - data_inicial) / 7
            WHEN unidade IN ('m', 'month', 'months')   THEN (data_final - data_inicial) / 30
            WHEN unidade IN ('y', 'year', 'years')     THEN (data_final - data_inicial) / 365
        END AS diferenca;
$$;
```

**Excluindo a função:**

```sql
DROP FUNCTION datediff;
```

> **Correção em relação ao original:** no script original, a condição de anos dividia por 30 e havia uma segunda condição `'w'` dividindo por 365. Aqui foram separadas em meses (÷ 30) e anos (÷ 365).

**Resumo**

1. Para criar funções, usa-se `CREATE FUNCTION`.
2. É obrigatório informar (a) os tipos dos **inputs**, (b) o tipo do **output** e (c) a **linguagem** em que a função é escrita.
3. O script da função deve ser delimitado por `$$`.
4. Para deletar uma função, usa-se `DROP FUNCTION`.

---

## 13. Manipulação de Tabelas

### 13.1 Criação e exclusão de tabelas

Conteúdo: criação de tabela **a partir de uma query**, criação de tabela **do zero** e **deleção** de tabelas.

**Criação a partir de uma query:** crie a tabela `temp_tables.customers_age` com o id e a idade dos clientes.

```sql
SELECT
    customer_id,
    DATEDIFF('years', birth_date, current_date) AS idade_cliente
INTO temp_tables.customers_age
FROM sales.customers;

SELECT *
FROM temp_tables.customers_age;
```

> Requer a função `datediff` criada na [seção 12](#12-funções).

**Criação do zero:** crie a tabela `temp_tables.profissoes` com a tradução dos status profissionais dos clientes.

```sql
CREATE TABLE temp_tables.profissoes (
    professional_status varchar,
    status_profissional varchar
);

INSERT INTO temp_tables.profissoes
    (professional_status, status_profissional)
VALUES
    ('freelancer',    'freelancer'),
    ('retired',       'aposentado(a)'),
    ('clt',           'clt'),
    ('self_employed', 'autônomo(a)'),
    ('other',         'outro'),
    ('businessman',   'empresário(a)'),
    ('civil_servant', 'funcionário público(a)'),
    ('student',       'estudante');
```

**Deleção de tabelas:**

```sql
DROP TABLE temp_tables.profissoes;
```

**Resumo**

1. Para criar tabelas a partir de uma query, escreva a query normalmente e coloque `INTO` antes do `FROM`, informando o schema e o nome da tabela a ser criada.
2. Para criar tabelas do zero: (a) crie uma tabela vazia com `CREATE TABLE`; (b) informe os valores a inserir com `INSERT INTO` seguido do nome das colunas; (c) insira os valores em forma de lista após `VALUES`.
3. Para deletar uma tabela, use `DROP TABLE`.

---

### 13.2 Linhas

| Comando | Função |
|---|---|
| `INSERT INTO` | Inserção de linhas |
| `UPDATE` | Atualização de linhas |
| `DELETE FROM` | Deleção de linhas |

**Inserção:** insira os status "desempregado(a)" e "estagiário(a)" em `temp_tables.profissoes`.

```sql
INSERT INTO temp_tables.profissoes
    (professional_status, status_profissional)
VALUES
    ('unemployed', 'desempregado'),
    ('trainee',    'estagiario');
```

**Atualização:** corrija a tradução de "estagiário(a)" de `trainee` para `intern`.

```sql
UPDATE temp_tables.profissoes
SET professional_status = 'intern'
WHERE status_profissional = 'estagiario';
```

**Deleção:** delete as linhas dos status "desempregado(a)" e "estagiário(a)".

```sql
DELETE FROM temp_tables.profissoes
WHERE status_profissional = 'desempregado'
   OR status_profissional = 'estagiario';
```

**Resumo**

1. **Inserir** linhas: (a) informe os valores com `INSERT INTO` seguido do nome da tabela e das colunas; (b) insira os valores em forma de lista após `VALUES`.
2. **Atualizar** linhas: (a) informe a tabela com `UPDATE`; (b) informe o novo valor com `SET`; (c) delimite qual linha será modificada com o filtro `WHERE`.
3. **Deletar** linhas: (a) informe a tabela com `DELETE FROM`; (b) delimite qual linha será deletada com o filtro `WHERE`.

> **Atenção:** `UPDATE` e `DELETE` **sem `WHERE`** afetam todas as linhas da tabela.

---

### 13.3 Colunas

Conteúdo: inserção, alteração e deleção de colunas.

**Inserção de coluna:** insira em `sales.customers` uma coluna com a idade do cliente.

```sql
ALTER TABLE sales.customers
ADD customer_age int;

UPDATE sales.customers
SET customer_age = DATEDIFF('years', birth_date, current_date)
WHERE true;
```

**Alteração do tipo da coluna:** altere `customer_age` de inteiro para `varchar`.

```sql
ALTER TABLE sales.customers
ALTER COLUMN customer_age TYPE varchar;
```

**Renomeação da coluna:** renomeie `customer_age` para `age`.

```sql
ALTER TABLE sales.customers
RENAME COLUMN customer_age TO age;
```

**Deleção da coluna:** delete a coluna `age`.

```sql
ALTER TABLE sales.customers
DROP COLUMN age;
```

**Resumo**

1. Qualquer modificação nas colunas de uma tabela usa `ALTER TABLE` seguido do nome da tabela.
2. Para **adicionar** colunas, use `ADD` seguido do nome e do tipo da nova coluna.
3. Para **mudar o tipo** de uma coluna, use `ALTER COLUMN ... TYPE`.
4. Para **renomear** uma coluna, use `RENAME COLUMN ... TO`.
5. Para **deletar** uma coluna, use `DROP COLUMN`.