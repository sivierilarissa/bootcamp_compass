# Guia Completo: Git e GitHub do Basico ao Avancado

## 1. Conceitos Fundamentais

* **Repositorio (Repo):** Local onde o codigo fonte e o historico de versoes de um projeto sao armazenados.
  * Cada projeto geralmente possui seu proprio repositorio.
  * Pode ser hospedado em servidores remotos especializados (ex: GitHub, Bitbucket).
  * Os desenvolvedores da equipe podem clonar o repositorio localmente para trabalhar em versoes independentes.
* **Inicializacao (`git init`):** Transforma o diretorio atual em um projeto reconhecido pelo Git.
  * Cria a pasta oculta `.git`, responsavel por armazenar os arquivos de configuracao e a estrutura de versionamento.
* **README:** Arquivo em formato Markdown (`.md`) que contem a descricao completa, instrucoes de instalacao e documentacao do projeto.
* **`.gitignore`:** Arquivo onde sao inseridos os padroes de nomes de arquivos e pastas que o Git nao deve monitorar (ex: credenciais sensiveis, logs ou dependencias compiladas).
* **Licencas de Software:** Definem as permissoes e restricoes de uso do codigo fonte.
  * **Licenca MIT:** Uma das licencas mais simples e permissivas do mercado. Isenta o autor de garantias e responsabilidade legal, permitindo reuso livre.

---

## 2. Visao Geral da Interface do GitHub

* **Aba Code:** Exibe o codigo fonte, a documentacao (`README.md`), os detalhes da licenca e o menu para selecao/criacao de branches. Fornece a URL para clonagem via HTTPS ou SSH.
* **Aba Issues:** Sistema de catalogacao de tarefas, melhorias e bugs.
  * Suporta formacao em Markdown no titulo e descricao.
  * Permite atribuir responsaveis (**Assignees**), categorias com cores em hexadecimal (**Labels**), vincular a projetos (**Projects**) e definir marcos de entrega (**Milestones**).
  * Ao ser concluida e fechada (**Closed**), a issue sai da lista de problemas abertos.
* **Aba Pull Requests (PR):** Ambiente de revisao onde colaboradores enviam codigo novo ou correcoes a partir de uma branch secundaria para ser integrado a branch principal (`main`).
  * Evita envios diretos na branch principal sem inspecao previa.
  * Em caso de divergencia no codigo, gera conflitos de merge que devem ser resolvidos pelas liderancas de desenvolvimento.
* **Aba Actions:** Plataforma para automacao de fluxos de trabalho (Workflows) de CI/CD (Continuous Integration / Continuous Deployment).
  * Permite criar rotinas automaticas para compilar (build), testar e fazer deploy do codigo em servidores remotos.
* **Aba Projects:** Quadro de organizacao visual de tarefas baseado na metodologia Kanban (ex: colunas de Backlog, Desenvolvimento, Teste e Concluido).
* **Aba Wiki:** Repositorio de conhecimento para documentacoes extensas e detalhadas do projeto, permitindo a criacao de multiplas paginas.
* **Aba Insights:** Paineis analiticos que detalham o progresso do projeto, incluindo estatisticas de commits, distribuicao de contribuidores, volume de tráfego de visitantes e contagem de **Forks** (copias espelhadas do repositorio feitas por outros usuarios).
* **Aba Settings:** Painel de configuracoes do repositorio.
  * Permite renomear ou excluir o repositorio, gerenciar acessos de colaboradores, alterar a branch padrao e acessar a **Danger Zone**.
* **GitHub Gists:** Recursos para armazenar e compartilhar pequenos blocos ou trechos de codigo isolados (podem ser publicos ou secretos).

---

## 3. Comandos Essenciais do Git

### Inicializacao, Monitoramento e Salvamento
* `git init`: Inicializa o repositorio Git no diretorio atual.
* `git status`: Exibe o estado dos arquivos no projeto.
  * **U (Untracked):** Arquivo novo que ainda nao esta sendo monitorado pelo Git.
  * **A (Added):** Arquivo adicionado a area de staging, pronto para ser comitado.
  * **M (Modified):** Arquivo monitorado que sofreu alteracoes locais.
* `git add <arquivo>`: Adiciona um arquivo especifico a area de staging.
* `git add .`: Adiciona todos os arquivos modificados e nao monitorados do diretorio atual de uma so vez.
* `git commit -m "<mensagem>"`: Salva um ponto da historia do projeto com uma mensagem descritiva.
* `git commit -a -m "<mensagem>"`: Adiciona automaticamente os arquivos modificados monitorados e realiza o commit.
* `git log`: Exibe o historico cronologico dos commits realizados no projeto (pressione a tecla `q` para sair).

### Sincronizacao com Repositorio Remoto
* `git remote add origin <URL>`: Conecta o repositorio local a um repositorio remoto no GitHub.
* `git remote -v`: Lista as URLs dos servidores remotos configurados.
* `git remote rm origin`: Remove a conexao com o repositorio remoto configurado.
* `git push`: Envia os commits locais para o servidor remoto.
* `git push -u origin main`: Define o servidor `origin` e a branch `main` como padrao para os envios futuros.
* `git pull`: Baixa e mescla as alteracoes do repositorio remoto diretamente na sua maquina local.
* `git clone <URL>`: Faz a copia completa de um repositorio remoto para a sua maquina local.
* `git fetch`: Mapeia e atualiza as referencias de branches e tags do servidor remoto sem alterar os seus arquivos locais imediatamente.

### Manipulacao de Arquivos e Descarte de Mudancas
* `git mv <origem> <destino>`: Renomeia um arquivo ou o move para outra pasta mantendo o historico do Git.
* `git rm <arquivo>`: Remove um arquivo do disco e exclui seu monitoramento no Git.
* `git checkout -- <arquivo>`: Desfaz as alteracoes locais de um arquivo nao comitado, retornando-o ao estado original.
* `git reset --hard origin/main`: Apaga todas as alteracoes locais nao enviadas e força a sincronizacao exata com o ultimo commit da branch remota.

---

## 4. Trabalhando com Branches (Ramificacoes)

* **Conceito:** As branches permitem isolar o desenvolvimento de novas funcionalidades sem afetar a versão estavel do codigo na branch principal (`main`).

* `git branch`: Lista todas as branches locais existentes (a branch ativa e sinalizada com um asterisco `*`).
* `git branch <nome_branch>`: Cria uma nova branch com o nome especificado.
* `git branch -d <nome_branch>`: Deleta a branch informada.
* `git checkout <nome_branch>`: Alterna a sessao de trabalho para a branch informada.
* `git checkout -b <nome_branch>`: Cria a nova branch e alterna para ela imediatamente.
* `git merge <nome_branch>`: Une as alteracoes da branch especificada a branch onde voce esta posicionado atualmente.

---

## 5. Armazenamento Temporario (`git stash`)

* **Conceito:** O stash salva temporariamente as alteracoes locais nao comitadas na memoria de trabalho, permitindo limpar o diretorio para mudar de branch sem perder o progresso.

* `git stash`: Guarda as modificacoes pendentes na pilha de stash e limpa o diretorio de trabalho.
* `git stash list`: Exibe a lista de todos os stashs armazenados.
* `git stash apply`: Aplica as alteracoes do stash mais recente de volta ao codigo sem remove-lo da pilha.
* `git stash show -p <numero>`: Exibe as diferencas em formato diff das alteracoes salvas naquele stash especifico.
* `git stash drop <numero>`: Remove um stash especifico da pilha pelo seu indice.
* `git stash clear`: Apaga completamente todos os stashs salvos na memoria.

---

## 6. Marcas de Versao (Tags)

* **Conceito:** As tags funcionam como marcos ou pontos de checagem imutaveis no historico do desenvolvimento (ex: `v1.0`, `v2.0`).

* `git tag -a <nome_tag> -m "<mensagem>"`: Cria uma nova tag anotada vinculada ao commit atual.
* `git show`: Exibe a lista e os detalhes das tags criadas no projeto.
* `git show <nome_tag>`: Mostra as informacoes e as alteracoes especificas de uma determinada tag.
* `git checkout <nome_tag>`: Aponta o estado do projeto para o ponto exato da historia marcado pela tag.
* `git push origin <nome_tag>`: Envia uma tag especifica para o repositorio remoto.
* `git push origin --tags`: Envia todas as tags locais pendentes para o repositorio remoto de uma so vez.

---

## 7. Submodulos

* **Conceito:** Permite incluir e gerenciar um repositorio Git externo como uma pasta dentro de outro repositorio pai, mantendo os seus historicos de commit independentes.

* `git submodule add <URL_repositorio>`: Adiciona um novo submodulo ao projeto atual.
* `git submodule`: Lista todos os submodulos configurados no repositorio.
* `git push --recurse-submodules=on-demand`: Envia os commits do projeto pai e garante que as alteracoes dos submodulos sejam atualizadas no servidor remoto.