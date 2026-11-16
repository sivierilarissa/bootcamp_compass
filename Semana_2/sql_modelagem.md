# Banco de Dados - da Modelagem ao SQL com PostgreSQL

## Sumário

1. [Modelo Conceitual](#1-modelo-conceitual)
2. [Modelo Lógico](#2-modelo-lógico)
3. [Modelo Físico: SQL](#3-modelo-físico-sql)
4. [Conceitos Básicos de Dados](#4-conceitos-básicos-de-dados)
5. [Correlação e Causalidade](#5-correlação-e-causalidade)
6. [Como a IA Interpreta Dados Brutos](#6-como-a-ia-interpreta-dados-brutos)
7. [Fontes de Dados e Dicionário de Dados](#7-fontes-de-dados-e-dicionário-de-dados)
8. [Formatos de Arquivo](#8-formatos-de-arquivo)
9. [Guia Básico de SQL + Modelagem de Dados](#9-guia-básico-de-sql--modelagem-de-dados)

---

## 1. Modelo Conceitual

### 1.1 Introdução

Um projeto de BD (banco de dados) deve avaliar a necessidade das informações que serão armazenadas e conceber um BD **eficaz** e **eficiente**.

Um projeto de BD passa por **4 etapas**:

| Etapa | Descrição |
|---|---|
| **1. Modelo Conceitual** | Definição dos dados, objetos e relacionamentos, em alto nível |
| **2. Normalização** | Evitar redundâncias |
| **3. Modelo Lógico** | Visão, em forma de diagrama, do que será o modelo físico |
| **4. Modelo Físico** | Tabelas criadas via comandos SQL |

#### Primeira etapa: construção do modelo conceitual

- Deve ser construído **com a ajuda do usuário**; por isso, precisa ser de **fácil entendimento**, ou seja, um modelo de **alto nível**.
- Busca uma definição certeira dos dados, sendo necessário especificar os **objetos** presentes no projeto e seus **relacionamentos**.
- As particularidades de implementação desses objetos e relacionamentos devem ser **postergadas**.
- **Não pode ser ambíguo**, pois servirá de meio de comunicação entre usuários e desenvolvedores.
  - Isso obriga o entendimento e o atendimento dos requisitos pelos dois grupos.
  - É muito mais valioso para os desenvolvedores reconhecer e validar as reais necessidades do usuário.

### 1.2 Criando o Modelo Conceitual

#### Levantamento de requisitos

- Para criar o modelo conceitual, a equipe responsável já deve ter em mãos o **levantamento de requisitos**.
- O levantamento de requisitos deve ser feito pela equipe de **engenharia de software**.
- **Requisitos** de um sistema são descrições dos **serviços fornecidos** pelo sistema e de suas **restrições operacionais**.
- Com o levantamento em mãos, a equipe deve aplicar técnicas para transformar os requisitos em um projeto apropriado às necessidades do sistema.

**Itens do levantamento de requisitos:**

1. Cliente
2. Projeto
3. Sinopse do cliente
4. O que fazer

#### Componentes do modelo conceitual

Estão presentes no modelo conceitual:

- **Entidades**: retratam um objeto do mundo real (professor, aluno) ou conceitual (disciplina, nota). Uma entidade pode ser definida como um **substantivo**.
- **Atributos**: propriedades das entidades (nome, idade etc.).
- **Relacionamentos**: associações entre entidades.

---

### 1.3 Entidades

#### Tipo Entidade × Entidade

Um **tipo entidade** define uma coleção ou conjunto de entidades que possuem os mesmos atributos. Cada tipo de entidade no banco de dados é descrito por seu **nome** e seus **atributos**.

#### Como detectar possíveis entidades

1. Leia o levantamento de requisitos extraindo dele todas as possíveis entidades.
2. Lembre-se: uma entidade pode ser vista como **substantivo**.
3. Nem todo substantivo é uma entidade, mas, na primeira etapa, liste todos.
4. Se um substantivo aparecer mais de uma vez, desconsidere as repetições.
5. Se um substantivo aparecer com designação diferente, mas representando o mesmo ser, também deve ser desconsiderado.

#### Como eliminar entidades

| Critério de eliminação | Explicação |
|---|---|
| **Entidades que descrevem o domínio** | Elementos que servem apenas para descrever o domínio do negócio e do problema. Se não é necessário guardar informações sobre eles, provavelmente estão na lista só para descrever o domínio. |
| **Referências ao aplicativo** | Referências, funcionalidades, configurações e design do futuro software devem ser eliminadas. São informações importantes, mas não pertencem ao modelo conceitual, que representa o armazenamento de fatos significativos para um propósito específico. |
| **Entidades com instância única** | Elementos que, após análise, serão um conjunto unitário (apenas uma instância armazenada) devem ser eliminados. |
| **Entidades com atributo único** | Revise as entidades restantes: se alguma possui apenas um atributo, provavelmente ela é um atributo de outra entidade. |

---

### 1.4 Relacionamentos

#### Como detectar possíveis relacionamentos

- Na representação gráfica, o relacionamento é um **losango**.
- A associação entre entidades se dá por conexões chamadas **relacionamentos**.
- São identificados por **verbos** ou **preposições** que conectam duas ou mais entidades.
  - **Verbo**: indica ação, estado ou fenômeno da natureza.
  - **Preposição**: liga palavras, estabelecendo uma relação entre elas.
  - Exemplos: *de, referente, gera, pertence, possui*.

#### Cardinalidade

A cardinalidade define o **grau de relacionamento** existente entre as entidades. Dependendo das regras do negócio, pode ser:

| Cardinalidade | Nome | Observação |
|---|---|---|
| **1:1** | Um para um | O relacionamento vira uma **chave estrangeira** em uma futura tabela. |
| **1:N** | Um para muitos | A chave estrangeira é sempre criada do **lado N**. |
| **N:N** ou **M:N** | Muitos para muitos | Exige tabela associativa no modelo lógico. |

> **Dica:** para identificar a cardinalidade correta, coloque-se no lugar de uma entidade e tente identificar **quantas entidades do outro lado** do relacionamento é possível visualizar.

---

### 1.5 Atributos

#### Como detectar atributos

- São **propriedades ou características** de uma entidade.
- Ao ler o texto, é possível detectá-los por **adjetivos** conectados, direta ou indiretamente, às entidades.
- Atributos também podem ser vistos como **substantivos**.

#### Tipos de atributos

**Quanto à quantidade de informações**

| Tipo | Descrição | Exemplo | Representação |
|---|---|---|---|
| **Monovalorado** | Formado por um único valor | Nome | Linha contínua |
| **Multivalorado** | Formado por um conjunto de valores | Vários telefones | Linha pontilhada |

**Quanto à atomicidade**

| Tipo | Descrição | Exemplo |
|---|---|---|
| **Simples (atômico)** | Indivisível | Nome |
| **Composto** | Pode ser dividido em partes menores | Endereço |

**Quanto à unicidade**

Chave é o conjunto mínimo de atributos que garante que a entidade seja única. Cria-se o `id` (identificador) para garantir a unicidade das entidades.

| Tipo | Descrição | Exemplo |
|---|---|---|
| **Chave simples** | Um atributo garante a unicidade | `id` |
| **Chave composta** | Um conjunto de atributos garante a unicidade | — |

**Quanto ao armazenamento**

| Tipo | Descrição |
|---|---|
| **Armazenado** | Tem seu valor armazenado no BD |
| **Derivado** | Tem seu valor calculado a partir de outros valores armazenados no BD |

---

## 2. Modelo Lógico

*(Elaborado após o modelo conceitual.)*

- Oferece à equipe de desenvolvimento uma **visão abstrata** do BD.
- O coerente é só trabalhar no modelo lógico quando já se tem o **modelo conceitual normalizado**.
- Deve considerar o **SGBD relacional (BDR)** que será utilizado, para facilitar uma visão mais precisa do modelo físico final.
- Para ser mais eficiente, além de normalizado, é necessário ter as **chaves estrangeiras bem definidas**.
- O modelo lógico **não tem tipos de relacionamento**: o relacionamento entre tabelas é garantido pelas chaves estrangeiras.
- A criação da chave estrangeira depende da **cardinalidade** existente entre os tipos entidade do modelo conceitual.

### Chave estrangeira (FK)

- É um ou mais atributos que **fazem referência a um atributo de outra tabela**.
- Garante a **consistência e a integridade** dos dados por meio desse relacionamento.
- É importante para que os SGBDs garantam a **integridade referencial**.

### Tipos de dados mais comuns

| Tipo | Descrição |
|---|---|
| `CHAR(n)` | Caracteres de tamanho **fixo** |
| `VARCHAR(n)` | Caracteres de tamanho **variável** |
| `NUMBER(p,e)` | Números de ponto flutuante com precisão de `p` dígitos e escala de `e` dígitos (após a vírgula). *No PostgreSQL, o equivalente é `NUMERIC(p,e)`.* |
| `INTEGER` | Números inteiros |
| `DATE` | Data |
| `TIME` | Hora |
| `TIMESTAMP` | Data e hora |

---

## 3. Modelo Físico: SQL

**SQL (Structured Query Language)** significa "Linguagem Estruturada de Consulta".

- Linguagem **declarativa**.
- Desenvolvida para os bancos de dados **relacionais**.
- Tornou-se **padrão** para bancos de dados relacionais.

---

## 4. Conceitos Básicos de Dados

### 4.1 Tipos de variáveis

```
Variáveis
├── Qualitativas (categorias ou qualidades)
│   ├── Nominais  → sem ordem (cores, países)
│   └── Ordinais  → com ordem implícita (escolaridade, satisfação)
└── Quantitativas (quantidades mensuráveis numericamente)
    ├── Discretas  → valores inteiros (número de filhos, quantidade de vendas)
    └── Contínuas  → valores fracionários/decimais (altura, peso, temperatura)
```

### 4.2 Tipos de dados

| Tipo | Características | Exemplos | Observação |
|---|---|---|---|
| **Estruturados** | Organizados em linhas e colunas | Planilhas financeiras, bases de clientes | Fácil armazenamento e consulta em bancos de dados relacionais |
| **Não estruturados** | Não seguem formato padronizado | Textos de e-mails, áudios, imagens, vídeos | Exigem técnicas de IA e aprendizado de máquina para análise |

### 4.3 Relação da IA com dados

- A IA consegue lidar tanto com dados **estruturados** (banco de dados) quanto **não estruturados** (reconhecimento de voz e de imagem).
- O tipo de variável e a forma do dado influenciam o tipo de modelo de IA usado.
- **Exemplos:**
  - Prever vendas futuras → dados **estruturados**.
  - Analisar sentimentos em redes sociais → dados **não estruturados**.

### 4.4 Estatísticas descritivas essenciais

- Ajudam a **resumir e interpretar** conjuntos de dados.
- O foco está em entender **o que os números significam**.
- Fornecem insights rápidos para apoiar decisões baseadas em dados.

| Medida | Definição | Pergunta que responde | Observação |
|---|---|---|---|
| **Média** | Valor central ou típico de um conjunto de dados | Qual o valor **típico**? | Útil para resumir grandes volumes em um único número. **Limitação:** pode ser influenciada por valores muito altos ou muito baixos. |
| **Mediana** | Valor que ocupa a posição central quando os dados estão ordenados | Qual o **ponto central**? | Menos sensível a valores extremos que a média. |
| **Moda** | Valor que mais se repete | Qual o **mais comum**? | Útil para identificar padrões de preferência ou comportamento (o mais popular). |
| **Desvio padrão** | Quanto os dados variam em relação à média | Quão **consistentes** são os dados? | Indica se os valores estão próximos ou muito espalhados. |

---

## 5. Correlação e Causalidade

São conceitos centrais em análise de dados.

### 5.1 Definições

| Conceito | Definição | Exemplos |
|---|---|---|
| **Correlação** | Relação estatística entre duas variáveis. Elas se movimentam juntas, **mas isso não significa que uma causa a outra**. Mostra padrões. | Aumento nas vendas de sorvete e aumento de afogamentos no verão (ambos influenciados pela temperatura). |
| **Causalidade** | Relação de causa e efeito: uma variável influencia **diretamente** a outra. Exige mais evidências que a correlação (experimentos controlados ou análises profundas). Explica mecanismos. | Aumento do consumo de calorias causa aumento de peso, se não houver compensação. Aplicar desconto em um item aumenta suas vendas. |

### 5.2 Por que é tão fácil confundir?

- A correlação muitas vezes **parece intuitiva** como causa.
- Dados incompletos ou vieses podem reforçar interpretações erradas.

### 5.3 Ferramentas para diferenciar

- **Experimentos controlados**: grupos de controle e de teste para isolar variáveis.
- **Análises estatísticas avançadas**: métodos que ajudam a identificar relações causais.
- **Uso de IA**: detecta variáveis ocultas que influenciam os resultados.

### 5.4 Riscos e benefícios

- **Risco da correlação:** tomar decisões baseadas apenas em correlação pode ser arriscado.
- **Benefício da causalidade:** fornece base mais sólida para estratégias de negócios, políticas públicas e inovações.

---

## 6. Como a IA Interpreta Dados Brutos

**Dados brutos** são dados coletados em seu formato original, sem tratamento.

> **Desafio:** esses dados, sozinhos, não têm significado até serem processados.

### 6.1 Primeira etapa: pré-processamento

| Passo | Descrição |
|---|---|
| **Limpeza** | Remoção de inconsistências e erros |
| **Normalização** | Padronização de formatos (datas, números, unidades) |
| **Estruturação** | Organização dos dados para que possam ser usados pelos modelos de IA |

### 6.2 Segunda etapa: interpretação de dados estruturados

- A IA lê tabelas, bancos de dados e planilhas.
- Identifica **padrões, correlações e tendências**.

### 6.3 Interpretação de dados não estruturados

A IA precisa converter dados não organizados em **representações numéricas**:

- **Processamento de Linguagem Natural (PLN):** análise de textos.
- **Reconhecimento de imagem:** fotos e vídeos, para identificar falhas e padrões visuais.
- **Reconhecimento de fala:** transforma áudio em texto.

### 6.4 Representações internas: embeddings

- Modelos de IA transformam dados em **vetores numéricos**.
- Esses vetores permitem comparar **similaridades e diferenças** entre informações.

**Fluxo geral:**

```
Dados brutos            →   Processamento              →   Insights
(sem processamento)         (interpretação e análise)      (classificações, previsões, recomendações)
```

> **Exemplo:** um modelo de IA pode recomendar produtos a clientes com base em compras anteriores e no comportamento de usuários semelhantes.

---

## 7. Fontes de Dados e Dicionário de Dados

### 7.1 Fontes para dados

| Fonte | Endereço |
|---|---|
| Portal de Dados Abertos do Governo Brasileiro | [dados.gov.br](https://dados.gov.br) |
| Banco Mundial | [data.worldbank.org](https://data.worldbank.org) |
| Kaggle Datasets | [kaggle.com/datasets](https://www.kaggle.com/datasets) |
| Google Dataset Search (indica onde estão os dados) | [datasetsearch.research.google.com](https://datasetsearch.research.google.com) |

### 7.2 Dicionário de dados

Documento que **explica o que são os dados**: significado, tipo, formato e regras de cada campo.

---

## 8. Formatos de Arquivo

### 8.1 Arquivos simples

#### `.txt`: arquivos de texto

- Armazena dados em **texto puro**, sem formatação especial ou estrutura complexa.
- Fácil de abrir em qualquer editor, mas **sem estrutura** para dados complexos.
- **Exemplos:** notas pessoais ou lembretes, logs de sistema, listas simples de tarefas, documentações básicas.

#### `.csv`: valores separados por vírgula

- Usado para **dados estruturados** em formato de tabela, com colunas separadas por vírgulas.
- Pode ser aberto no Excel, no Google Sheets e em programas de análise de dados.
- **Exemplos:** planilhas de vendas mensais, listas de clientes com contato, dados de inventário, relatórios financeiros básicos.

### 8.2 Arquivos para dados estruturados e complexos

#### JSON (JavaScript Object Notation)

- Formato **leve e eficiente**, ideal para troca de dados estruturados entre sistemas e aplicações web.
- **Legibilidade superior:** fácil leitura por humanos e máquinas; muito usado em programação moderna.
- **Exemplos:** dados de APIs web, configurações de aplicativos, respostas de serviços online.

```json
{
  "nome": "João Silva",
  "idade": 30,
  "cidade": "São Paulo",
  "habilidades": ["Python", "SQL", "Excel"]
}
```

#### XML (eXtensible Markup Language)

- Formato **hierárquico**, com estrutura em árvore para dados complexos e documentos estruturados.
- Mais verboso que o JSON, mas ainda amplamente usado em sistemas corporativos legados.
- **Casos de uso:** feeds RSS de notícias, documentos técnicos, configuração de sistemas.

#### XML vs. JSON

| Característica | JSON | XML |
|---|---|---|
| Sintaxe | Mais limpa e simples | Mais verbosa |
| Tamanho do arquivo | Menor | Maior |
| Velocidade de processamento | Mais rápido | Mais lento |
| Robustez | Menor | Mais robusto |
| Validação | Limitada | Validação em esquema |
| Metadados* | Limitado | Suporte a metadados |
| Padronização | Amplamente adotado | Padrão estabelecido |

\* *Metadados: informações detalhadas sobre outras informações.*

### 8.3 Planilhas

#### XLSX (Planilhas Excel)

Formato especializado que combina dados com capacidades de análise, cálculo e apresentação visual; essencial para relatórios e tomada de decisão.

- **Análise avançada:** formato popular para análise de dados e criação de relatórios profissionais.
- **Fórmulas complexas:** suporta cálculos automáticos, funções estatísticas e operações matemáticas avançadas.
- **Visualizações:** criação de gráficos, tabelas dinâmicas e dashboards interativos.
- **Múltiplas abas:** organiza diferentes conjuntos de dados em um único arquivo.

#### ODS (Planilhas LibreOffice)

- **Alternativa open source:** equivalente ao Excel, completamente gratuito e de código aberto.
- **Compatibilidade total:** pode ser facilmente convertido de e para Excel sem perda de funcionalidades.
- **Recursos completos:** suporta fórmulas, gráficos, macros e todas as funcionalidades essenciais.

### 8.4 SQL / SQLite Dumps

- O SQLite é um sistema de banco de dados cujo arquivo **não precisa estar dentro de um servidor**: pode estar na sua máquina.
- **Dump** é o processo de pegar o banco de dados e exportá-lo em um formato para trabalhar sobre esses dados.
- Contém a **estrutura completa** do banco, incluindo tabelas, índices e dados; fundamental para análise relacional e aprendizado de SQL.
- Alguns datasets vêm na forma de banco de dados completo exportado.
- **Treinamento em SQL:** útil para praticar consultas e manipulação de dados relacionais.
- **Importação fácil:** pode ser importado diretamente em sistemas de gerenciamento de banco de dados.

---

## 9. Guia Básico de SQL + Modelagem de Dados

### Classificação dos comandos

| Sigla | Nome | Comandos |
|---|---|---|
| **DDL** | Data Definition Language | `CREATE`, `ALTER`, `DROP` |
| **DML** | Data Manipulation Language | `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE` |
| **DQL** | Data Query Language | `SELECT` |

---

### 9.1 DDL: Definição de Dados

#### `CREATE TABLE`

**Sintaxe:**

```sql
CREATE TABLE nome_tabela (
    nome_atributo tipo_dado [restricao],
    nome_atributo tipo_dado [restricao]
);
```

**Exemplos:**

```sql
CREATE TABLE uf (
    id    INTEGER NOT NULL PRIMARY KEY,
    nome  VARCHAR(20),
    sigla CHAR(2) NOT NULL
);

CREATE TABLE municipio (
    id    INTEGER NOT NULL PRIMARY KEY,
    nome  VARCHAR(20),
    id_uf INTEGER,
    FOREIGN KEY (id_uf) REFERENCES uf(id)
);
```

#### `ALTER TABLE`

**Sintaxe:**

```sql
ALTER TABLE nome_tabela
    [ADD nome_atributo tipo_dado [restricao],]
    [ALTER nome_atributo TYPE novo_tipo_dado,]
    [RENAME [COLUMN] nome_atual_atributo TO novo_nome_atributo];
```

**Exemplos:**

```sql
-- Adicionar chaves e uma nova coluna
ALTER TABLE cliente
    ADD PRIMARY KEY (id),
    ADD FOREIGN KEY (id_cidade) REFERENCES municipio(id),
    ADD dt_nascimento DATE;

-- Alterar o tipo de dado de uma coluna
ALTER TABLE cliente
    ALTER dt_nascimento TYPE VARCHAR(10);

-- Renomear uma coluna
ALTER TABLE cliente
    RENAME id_cidade TO id_municipio;
```

#### `DROP TABLE`

**Sintaxe:**

```sql
DROP TABLE nome_tabela;
```

**Exemplo:**

```sql
DROP TABLE cliente;
```

---

### 9.2 DML: Manipulação de Dados

#### `INSERT`

**Sintaxe:**

```sql
INSERT INTO nome_tabela [(nome_atributo [, nome_atributo])]
VALUES (valor1, valor2, valor3);
```

**Exemplo:**

```sql
INSERT INTO uf (id, nome, sigla)
VALUES (1, 'Ceará', 'CE');
```

#### `UPDATE`

**Sintaxe:**

```sql
UPDATE nome_tabela
SET nome_atributo = valor [, nome_atributo = valor]
WHERE condicao [operador_logico condicao];
```

**Exemplo:**

```sql
UPDATE uf
SET nome = 'CEARÁ'
WHERE id = 1;
```

#### `DELETE`

**Sintaxe:**

```sql
DELETE FROM nome_tabela
WHERE condicao [operador_logico condicao];
```

**Exemplo:**

```sql
DELETE FROM uf
WHERE id = 1;
```

#### `TRUNCATE`

Remove **todos** os registros da tabela.

```sql
TRUNCATE TABLE nome_tabela;

-- Exemplos
TRUNCATE TABLE uf;
TRUNCATE TABLE uf CASCADE;  -- também limpa as tabelas que dependem de uf
```

---

### 9.3 DQL: Consultas com `SELECT`

#### `SELECT` básico

```sql
SELECT nome_atributo, nome_atributo
FROM nome_tabela;

-- Todos os atributos (*)
SELECT *
FROM uf;

-- Atributos específicos
SELECT id, sigla
FROM uf;
```

#### `SELECT` + `ORDER BY`

Ordena o resultado (`ASC` = crescente, `DESC` = decrescente).

```sql
SELECT nome_atributo
FROM nome_tabela
ORDER BY nome_atributo [tipo_ordenacao];

-- Exemplo
SELECT id, sigla, nome
FROM uf
ORDER BY sigla ASC, nome DESC;
```

#### `SELECT` + `WHERE`

```sql
SELECT *
FROM nome_tabela
WHERE nome_atributo [condicao];
```

**Operadores de comparação:**

| Operador | Significado |
|---|---|
| `=` | Igual |
| `<>` | Diferente |
| `>` | Maior |
| `<` | Menor |
| `>=` | Maior ou igual |
| `<=` | Menor ou igual |

#### `SELECT` + `IN` (lista predefinida)

```sql
SELECT sigla, nome
FROM uf
WHERE sigla IN ('CE', 'SP', 'PE');

SELECT sigla, nome
FROM uf
WHERE sigla NOT IN ('CE', 'SP', 'PE');
```

#### `SELECT` + `BETWEEN` + `AND`

```sql
SELECT sigla, nome
FROM uf
WHERE sigla BETWEEN 'CE' AND 'SP';

SELECT sigla, nome
FROM uf
WHERE sigla NOT BETWEEN 'CE' AND 'SP';
```

#### `SELECT` + `LIKE`

O símbolo `%` representa qualquer sequência de caracteres.

```sql
-- Nome termina com M
SELECT sigla, nome
FROM uf
WHERE nome LIKE '%M';

-- Nome NÃO começa com M
SELECT sigla, nome
FROM uf
WHERE nome NOT LIKE 'M%';
```

#### `SELECT` + `IS NULL`

Filtra campos com valores nulos ou não nulos.

```sql
SELECT sigla, nome
FROM uf
WHERE nome IS NULL;

SELECT sigla, nome
FROM uf
WHERE nome IS NOT NULL;
```

> **Atenção:** `nome = ''` (texto vazio, sem nenhum conteúdo) **não é o mesmo** que `nome IS NULL` (valor nulo).

#### `SELECT` + `AND` e `OR` (operadores lógicos)

```sql
-- AND: ambas as condições devem ser verdadeiras
SELECT sigla, nome
FROM uf
WHERE sigla LIKE 'P%' AND nome IS NOT NULL;

-- OR: basta uma condição ser verdadeira
SELECT sigla, nome
FROM uf
WHERE sigla LIKE 'P%' OR nome IS NOT NULL;
```

---

### 9.4 Manipulando a Saída de Dados

#### Concatenação de strings: `||`

```sql
SELECT nome,
       logradouro || ', ' || numero || ', ' || bairro AS endereco
FROM cliente;
```

- `', '` insere uma vírgula seguida de um espaço.
- `AS` cria um **atributo derivado** (apelido para a coluna calculada).

#### Operadores aritméticos

```sql
-- Soma (+)
SELECT descricao, valor + 10
FROM produto;

-- Subtração (-)
SELECT descricao, valor - 10
FROM produto;

-- Multiplicação (*)
SELECT qtd_item, vr_item, qtd_item * vr_item
FROM item_venda;

-- Divisão (/): pode gerar várias casas decimais
SELECT vr_doc, vr_doc / 2
FROM item_venda;

-- Divisão com controle de casas decimais (CAST)
SELECT vr_doc, CAST(vr_doc / 2 AS NUMERIC(5,2))
FROM item_venda;
```

#### Regras de precedência

| Ordem | Regra |
|---|---|
| **1º** | Expressões entre parênteses `( ... )` |
| **2º** | Operadores de manipulação (como na matemática: `*` e `/`, depois `+` e `-`) |
| **3º** | Operadores de comparação |
| **4º** | Operador lógico `AND` |
| **5º** | Operador lógico `OR` |

---

### 9.5 Funções de Agregação

| Função | O que faz | Exemplo |
|---|---|---|
| `MAX()` | Maior valor de um campo (não precisa ordenar) | `SELECT MAX(vr_doc) FROM documento;` |
| `MIN()` | Menor valor de um campo (não precisa ordenar) | `SELECT MIN(vr_doc) FROM documento;` |
| `COUNT()` | Contagem do número de registros de uma seleção | `SELECT COUNT(vr_doc) FROM documento;` |
| `SUM()` | Soma | `SELECT SUM(vr_doc) FROM documento;` |
| `AVG()` | Média | `SELECT AVG(vr_doc) FROM documento;` |

#### `GROUP BY`

Agrupa os registros pelo atributo informado.

```sql
SELECT id_cliente, SUM(vr_doc)
FROM documento
GROUP BY id_cliente;
```

#### `HAVING`

Condição usada junto com funções de agregação (filtra os grupos).

```sql
SELECT id_cliente, SUM(vr_doc)
FROM documento
GROUP BY id_cliente
HAVING SUM(vr_doc) > 100;
```

---

### 9.6 `SELECT` + `DISTINCT`

Evita a repetição de um mesmo valor em uma consulta.

```sql
SELECT DISTINCT bairro
FROM cliente;
```

> **Atenção:** ao usar **dois ou mais atributos**, o `DISTINCT` considera a **combinação** deles. Se o primeiro atributo se repete mas o segundo não, todos os registros são retornados.

---

### 9.7 Observações Importantes

- Os comandos `>`, `<` e `BETWEEN`, quando usados em siglas (`CHAR`), comparam os valores seguindo a **ordem alfabética** (e não o `id`).
- `MAX`, `MIN` e `COUNT` aplicados a textos seguem o mesmo critério de **ordenação do texto**.
- `::` converte o atributo para outro tipo de dado (específico do PostgreSQL).

```sql
SELECT vr_doc::NUMERIC(5,2)
FROM documento;
```

---

### 9.8 Junção de Tabelas (`JOIN`)

É comum **apelidar as tabelas** com `AS` para deixar o comando mais simples.

| Tipo de junção | O que retorna |
|---|---|
| `INNER JOIN` | Apenas os registros que existem em **ambas** as tabelas |
| `LEFT [OUTER] JOIN` | Todos os registros da tabela **à esquerda** + correspondentes da direita |
| `RIGHT [OUTER] JOIN` | Todos os registros da tabela **à direita** + correspondentes da esquerda |
| `FULL [OUTER] JOIN` | Todos os registros de **ambas** as tabelas |

#### `INNER JOIN`: registros em comum

```sql
SELECT cliente.id, cliente.nome,
       documento.vr_doc, documento.dt_venc
FROM cliente
INNER JOIN documento ON cliente.id = documento.id_cliente;
```

#### `FULL JOIN`: junção externa completa

```sql
SELECT cl.id, cl.nome, doc.vr_doc, doc.dt_venc
FROM cliente AS cl
FULL OUTER JOIN documento AS doc ON cl.id = doc.id_cliente;
```

#### `LEFT JOIN`: junção externa à esquerda

```sql
SELECT cl.id, cl.nome, doc.vr_doc, doc.dt_venc
FROM cliente AS cl
LEFT OUTER JOIN documento AS doc ON cl.id = doc.id_cliente;
```

#### `RIGHT JOIN`: junção externa à direita

```sql
SELECT cl.id, cl.nome, doc.vr_doc, doc.dt_venc
FROM cliente AS cl
RIGHT OUTER JOIN documento AS doc ON cl.id = doc.id_cliente;
```