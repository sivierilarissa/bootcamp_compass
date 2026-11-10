# SQL — Structured Query Language

## 1. O que é SQL?

**SQL (Structured Query Language)** é a linguagem padrão utilizada para se comunicar com bancos de dados relacionais.

No dia a dia, trabalhamos principalmente com três famílias de comandos SQL:

- **DDL:** define a estrutura do banco de dados.
- **DQL:** consulta e recupera dados.
- **DML:** manipula os dados armazenados.

---
## 2. DDL — Data Definition Language

**Papel:** define o que existe no banco de dados, como tabelas, colunas, tipos de dados, chaves e índices.
É a família responsável por criar e modificar a estrutura do banco.

### Principais comandos

| Comando | Função |
|---|---|
| `CREATE` | Cria estruturas, como tabelas, índices e schemas. |
| `ALTER` | Altera uma estrutura existente. |
| `DROP` | Remove uma estrutura do banco. |
| `TRUNCATE` | Remove todos os registros de uma tabela, mantendo sua estrutura. |

---
## 3. DQL — Data Query Language

**Papel:** consulta e recupera dados sem alterar o conteúdo do banco.
Seu principal comando é o `SELECT`.
A DQL permite explorar informações, realizar filtros, agrupamentos e ordenações.

### Principais comandos e cláusulas

| Comando / Cláusula | Função |
|---|---|
| `SELECT` | Seleciona os dados desejados. |
| `FROM` | Define de qual tabela vêm os dados. |
| `JOIN` | Combina dados de diferentes tabelas. |
| `WHERE` | Filtra registros (linhas). |
| `GROUP BY` | Agrupa registros. |
| `COUNT()` / `SUM()` | Realiza operações de agregação. |
| `HAVING` | Filtra os resultados após o agrupamento. |
| `ORDER BY` | Ordena os resultados. |
| `LIMIT` | Limita a quantidade de registros retornados. |

---
## 4. DML — Data Manipulation Language

**Papel:** manipula o conteúdo das tabelas e seus registros, sem alterar a estrutura do banco.
É utilizada para inserir, atualizar e excluir dados.

### Principais comandos

| Comando | Função |
|---|---|
| `INSERT` | Insere novos registros (linhas). |
| `UPDATE` | Altera registros existentes. |
| `DELETE` | Remove registros de uma tabela. |

---
## 5. PostgreSQL

O **PostgreSQL** é um sistema gerenciador de banco de dados relacional (SGBD), gratuito e de código aberto (Open Source).

### Principais características

- Forte suporte a tipos de dados, integridade e SQL padrão.
- Muito utilizado em backends, APIs e projetos de Analytics.
- Suporta dados estruturados e semiestruturados.
- Possui recursos avançados, como JSON, pesquisa textual (Full-Text Search) e extensões.

---
## 6. Modelo mental de banco de dados relacional

Para entender a organização de um banco relacional, podemos fazer uma comparação com uma planilha:

| Banco de dados | Comparação com planilha |
|---|---|
| Tabela | Planilha |
| Linha | Registro |
| Coluna | Atributo tipado |
| Relação | Ligação entre tabelas por meio de chaves |

As tabelas podem se relacionar entre si, permitindo organizar informações sem precisar repetir os mesmos dados.

---
## 7. Tipos de dados no PostgreSQL

Os tipos de dados definem quais informações podem ser armazenadas em cada coluna.

| Tipo | Descrição |
|---|---|
| `INTEGER` / `BIGINT` | Números inteiros. |
| `SERIAL` | Inteiro com geração automática de valores sequenciais. |
| `NUMERIC` | Números decimais com precisão definida. |
| `TEXT` | Armazena textos. |
| `BOOLEAN` | Valores verdadeiro ou falso (`TRUE` / `FALSE`). |
| `TIMESTAMPTZ` | Data e hora com tratamento de fuso horário. |
| `DATE` | Armazena datas, sem horário. |
| `UUID` | Identificador único universal. |
| `JSONB` | Armazena dados semiestruturados no formato JSON, em representação binária otimizada. |

---

## 8. Chaves e restrições (Constraints)

As chaves e restrições garantem a integridade e a consistência dos dados armazenados.

### Principais conceitos

| Chave / Restrição | Descrição |
|---|---|
| `PRIMARY KEY (PK)` | Identifica exclusivamente cada registro. Não permite valores duplicados nem nulos. |
| `FOREIGN KEY (FK)` | Faz referência a uma chave de outra tabela, estabelecendo um relacionamento. |
| `UNIQUE` | Garante que os valores de uma coluna (ou combinação de colunas) não se repitam. |
| `NOT NULL` | Define que o campo é obrigatório e não pode ficar vazio (NULL). |
| `CHECK` | Define uma regra personalizada que os dados devem respeitar. |

---
## 9. Índices (INDEX)

Os índices ajudam a acelerar pesquisas e consultas em tabelas com grandes volumes de dados.

**Exemplo:** ao pesquisar frequentemente um cliente pelo CPF, um índice nessa coluna pode tornar a busca mais rápida.

**Atenção:** índices ocupam espaço de armazenamento e podem aumentar o custo de operações de inserção, atualização e exclusão.

---
## 10. Observações importantes

### TRUNCATE

O comando `TRUNCATE` remove todos os registros de uma tabela, mas mantém sua estrutura, incluindo colunas e restrições.

No PostgreSQL, para reiniciar também os valores de colunas com geração de sequência, pode-se utilizar:

```sql
TRUNCATE TABLE nome_da_tabela RESTART IDENTITY;
```

Também é possível utilizar `CASCADE` quando for necessário considerar tabelas que possuem dependências por chave estrangeira.

### Datas aleatórias — NOW() - X dias

No PostgreSQL, `NOW()` retorna a data e hora atuais.

É possível subtrair um intervalo de tempo para gerar datas retroativas.

**Exemplo:**

```sql
NOW() - INTERVAL '10 days'
```

Retorna a data e hora de 10 dias atrás.

Esse recurso pode ser útil para gerar datas variadas em registros de teste.

---
## 11. Resumo para memorização

| Família | Significado | Principal finalidade |
|---|---|---|
| DDL | Data Definition Language | Criar e modificar estruturas. |
| DQL | Data Query Language | Consultar dados. |
| DML | Data Manipulation Language | Inserir, atualizar e excluir dados. |

**Modelo mental:**

- DDL = estrutura.
- DQL = consulta.
- DML = conteúdo.

**Principal conceito:** SQL é a linguagem utilizada para interagir com bancos de dados relacionais, enquanto PostgreSQL é um dos sistemas que executam esses comandos.