# Workshop 1: Ciclo de Vida do Dado, Linux e Terminal

## 1. O Papel do Engenheiro de Dados

* **O que faz um Engenheiro de Dados:**
  * Conecta as fontes de dados.
  * Monta as pipelines de dados.
  * Cuida da qualidade e do custo.
  * Entrega dados confiaveis e no horario certo.
* **Visao Sistemica:** Se o dado nao chega limpo e no horario certo, os analistas e cientistas de dados ficam parados.

---

## 2. O Ciclo de Vida do Dado (Camadas)

O fluxo do dado percorre diferentes camadas de refinamento, da origem ate o consumo final por BI, Machine Learning e APIs:

| Camada | Nome Comercial | Descricao e Objetivo |
| :--- | :--- | :--- |
| **Fonte** | Source | Sistema de origem de onde o dado e extraido (fora do data lake). |
| **Bronze** | Raw | Copia fiel da origem, mantendo o dado bruto e sem tratamentos. |
| **Prata** | Refined / Trusted | Dados limpos, deduplicados e padronizados. |
| **Ouro** | Curated / Business | Modelado para as regras de negocio, pronto para uso analitico. |
| **Consumo** | Serving | Consumido por ferramentas de BI, modelos de Machine Learning e APIs. |

---

## 3. Data Lake vs Data Hub

* **Data Lake (Armazena):**
  * Coexistencia de varios formatos de dados.
  * Armazenamento de objetos baseado em nuvem: baixo custo e alta escalabilidade.
  * *Ponto de Atencao:* A qualidade varia, pois o dado entra sem validacao previa e precisa de tratamento.
* **Data Hub (Conecta e Distribui):**
  * Dados tratados e com dono definido, prontos para consumo corporativo.
  * Recebe dados de varias fontes, aplica regras e entrega para sistemas e times.
* **Evolucao do Hub:** Tratamento do dado como produto, suporte a multinuvem, reducao de redundancias e menor custo operacional.

---

## 4. O Impacto da Inteligencia Artificial no Dia a Dia

* **Qualidade:** Erros descobertos pelos usuarios geram alertas de anomalia preventivos antes de chegarem aos dashboards.
* **Textos e PDFs:** Dados nao estruturados que ficavam de fora das pipelines sao processados por LLMs que extraem campos e criam fontes estruturadas.
* **Cuidados e Governanca:**
  * **LGPD:** Risco de expor dados pessoais ao enviar informacoes sensiveis para treino ou prompts de LLMs.
  * **Custo:** Consultas geradas sem filtros podem varrer tabelas inteiras na nuvem, gerando custos elevados.
  * **Revisao:** Codigo gerado por IA que o engenheiro nao entende nao deve ir para producao.

---

## 5. Linux e o Terminal na Engenharia de Dados

* **Por que o Linux domina:** Roda a infraestrutura de servidores, containers e a maioria da nuvem. As principais ferramentas nascem la (Spark, Airflow, Kafka e Docker).
* **Ambientes e Distribuicoes:**
  * **Debian / Ubuntu:** Estaveis, base muito comum em servidores corporativos.
  * **Red Hat / Rocky / Alma:** Foco em ambientes corporativos e empresariais.
  * **Amazon Linux:** O padrao oficial utilizado nos servicos EC2 da AWS.
  * **Alpine:** Distribuicao minimalista e leve, muito comum no Docker.

---

## 6. Comandos Essenciais do Terminal

### Navegacao e Inspecao Basica
* `pwd`: Exibe o caminho absoluto do diretorio atual.
* `ls`: Lista arquivos e pastas do diretorio atual (`-la` para ocultos; `-lh` para tamanhos legiveis).
* `cd`: Navega entre diretorios (`cd ..` sobe um nivel; `cd ~` vai para a home).
* `cat`: Exibe todo o conteudo de um arquivo de texto na tela.
* `head`: Mostra o inicio de um arquivo (`head -n 1000` exibe as primeiras 1000 linhas).
* `tail`: Mostra o final de um arquivo (`tail -f` acompanha logs em tempo real).

### Filtros, Pipes e Processamento
* `|` (Pipe): Envia a saida padrao de um comando diretamente como entrada para o proximo.
* `grep`: Procura por padroes ou palavras-chave em arquivos ou logs.
* `wc -l`: Conta o numero de linhas de um arquivo ou fluxo de dados.
* **Exemplo de Pipeline Analitica:**
  ```bash
  head -n 1000 vendas.csv | grep "SP" | wc -l