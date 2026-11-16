# Banco de Dados: Da Modelagem à SQL com PostgreSQL

## Projeto de Banco de Dados
Um projeto de banco de dados passa por quatro etapas principais:
1. **Modelo Conceitual:** Alto nível, construído com o usuário, focado na definição de entidades, atributos e relacionamentos, sem se prender a detalhes técnicos.
2. **Normalização:** Evitar redundâncias.
3. **Modelo Lógico:** Visão em forma de diagrama orientada ao modelo relacional, com chaves estrangeiras bem definidas.
4. **Modelo Físico:** Tabelas criadas via comandos SQL.

### Elementos do Modelo Conceitual
* **Entidade:** Objeto do mundo real ou conceitual (substantivo, ex.: professor, aluno).
* **Atributo:** Propriedade ou característica de uma entidade (ex.: nome, idade).
* **Relacionamento:** Associação entre entidades (identificado por verbos ou preposições).
  * **Cardinalidade:** Grau de relacionamento ($1:1$, $1:N$, $N:M$).

---

## Fluência em Dados (Data Literacy) e IA
* **Data Literacy:** Capacidade de ler, entender, criar e comunicar informações baseadas em dados no dia a dia.
  * *Dimensões:* Ler, Entender, Criar e Comunicar.
* **IA Generativa:** Tecnologia capaz de gerar novos conteúdos a partir de redes neurais massivas. Difere da IA tradicional (que classifica e prediz) por criar e simular.
  * *Uso prático:* Interpretação de textos, visualização assistida por prompts, análise de planilhas e geração de hipóteses.

### Tipos de Variáveis e Dados
* **Qualitativas:** Descrevem categorias ou qualidades (Nominais sem ordem, Ordinais com ordem).
* **Quantitativas:** Mensuráveis numericamente (Discretas inteiras, Contínuas fracionárias).
* **Estruturados vs. Não Estruturados:** Dados em tabelas/linhas/colunas vs. textos, áudios, imagens e vídeos.
* **Estatística Descritiva:** Média (valor típico), Mediana (ponto central), Moda (mais comum) e Desvio Padrão (consistência/dispersão).
* **Correlação vs. Causalidade:** Correlação indica que variáveis se movimentam juntas; causalidade exige comprovação de que uma variável causa diretamente o efeito na outra.

---

## Formatos de Arquivos de Dados
* `.txt`: Texto puro, sem formatação complexa.
* `.csv`: Valores separados por vírgula para tabelas simples.
* **JSON:** Formato leve e estruturado, ideal para APIs e aplicações web.
* **XML:** Formato hierárquico em árvore, usado em sistemas legados.
* `.xlsx / .ods`: Planilhas com suporte a cálculos, fórmulas, gráficos e múltiplas abas.
* **SQL/SQLite Dumps:** Arquivos contendo estruturas completas de banco de dados e dados para importação.

---

## Guia Prático SQL (PostgreSQL)

### DDL (Data Definition Language)
```sql
-- Criar tabela UF
CREATE TABLE uf (
    id INTEGER NOT NULL PRIMARY KEY,
    nome VARCHAR(20),
    sigla CHAR(2) NOT NULL
);

-- Criar tabela Município
CREATE TABLE municipio (
    id INTEGER NOT NULL PRIMARY KEY,
    nome VARCHAR(20), 
    id_uf INTEGER,
    FOREIGN KEY (id_uf) REFERENCES uf(id)
);

-- Alterar tabela
ALTER TABLE cliente
    ADD PRIMARY KEY (id),
    ADD FOREIGN KEY (id_cidade) REFERENCES municipio(id),
    ADD dt_nascimento DATE;

-- Excluir tabela
DROP TABLE cliente;
```

### DML (Data Manipulation Language)
```sql
-- Inserir dados
INSERT INTO uf (id, nome, sigla)
VALUES (1, 'Ceará', 'CE');

-- Atualizar dados
UPDATE uf
SET nome = 'CEARÁ'
WHERE id = 1;

-- Deletar dados
DELETE FROM uf
WHERE id = 1;

-- Truncar tabela
TRUNCATE TABLE uf CASCADE;
```

### DQL (Data Query Language) - Consultas e Filtros
```sql
-- Seleção básica com ordenação
SELECT id, sigla, nome
FROM uf
ORDER BY sigla ASC, nome DESC;

-- Filtro com WHERE e operadores
SELECT *
FROM uf
WHERE sigla IN ('CE', 'SP', 'PE')
  AND nome IS NOT NULL;

-- Uso de BETWEEN e LIKE
SELECT sigla, nome
FROM uf
WHERE sigla BETWEEN 'CE' AND 'SP'
  AND nome LIKE '%M';
```

### Manipulação de Saída e Funções de Agregação
```sql
-- Concatenação e operadores aritméticos
SELECT nome, logradouro || ', ' || numero AS endereco
FROM cliente;

-- Funções de Agregação com GROUP BY e HAVING
SELECT id_cliente, SUM(vr_doc) AS total_vendas
FROM documento
GROUP BY id_cliente
HAVING SUM(vr_doc) > 100;

-- Valores distintos
SELECT DISTINCT bairro
FROM cliente;
```

### Junções de Tabelas (Joins)
```sql
-- INNER JOIN
SELECT cl.id, cl.nome, doc.vr_doc, doc.dt_venc
FROM cliente AS cl
INNER JOIN documento AS doc ON cl.id = doc.id_cliente;

-- LEFT JOIN
SELECT cl.id, cl.nome, doc.vr_doc, doc.dt_venc
FROM cliente AS cl
LEFT OUTER JOIN documento AS doc ON cl.id = doc.id_cliente;
```