# Guia de Resumo: Comandos Linux para Engenharia de Dados
---
* **Estrutura do Prompt de Comando:**
  `usuario@debian:~$` (ou `#`)
  * `usuario`: Nome do usuario conectado.
  * `@debian`: Nome da maquina na rede (hostname).
  * `:`: Separador de contexto.
  * `~`: Pasta pessoal local (`/home/usuario`).
  * `$`: Prompt indicando usuario comum.
  * `#`: Prompt indicando superusuario (root).
---
* **Interpretadores de Comandos (Shells):** `bash`, `zsh`, `ksh` (KornShell), `tcsh` (C Shell). 
  * `su` / `su root`: Alterna para o usuario root mantendo as variaveis do usuario anterior.
  * `su -`: Alterna para o usuario root carregando o perfil completo e limpo do root.
  * `exit`: Encerra a sessao de terminal ou desloga do usuario atual.
  * `clear`: Limpa a tela do terminal.

* **Gerenciamento de Privilegios:**
  * `usermod -aG sudo <usuario>`: Adiciona o usuario ao grupo sudo para conceder direitos administrativos.
---
* **Comandos principais**
* `apt` (Advanced Package Tool):Gerenciador de alto nivel do Debian/Ubuntu.
* `sudo apt update`: Atualiza a lista dos repositorios oficiais.
* `sudo apt-get install <pacote>`: Sintaxe classica de instalacao.
* `sudo apt-cache search <termo>`: Procura por pacotes nos repositorios locais.
* `apt-get source <pacote>`: Baixa os arquivos fontes de um determinado pacote.
* `dpkg`: Gerenciador de baixo nivel para arquivos `.deb` locais (`dpkg -i arquivo.deb`).
* `aptitude`: Interface avançada de gerenciamento (`install`, `upgrade`, `safe-upgrade`).
* `help`, `help alias`, `help cd`, `ls --help`: comandos de ajuda
* `apropos <termo>`: Procura por palavras-chave na base de manuais do sistema.
* `man man`, `man 1 passwd`, `man -k <termo>`, `man -w <comando>`: consultas de manuais
* `mkdir` (`-p`): criacao de pastas e para subpastas em cadeia, uso de aspas para nomes com espaco. 
* `rmdir`, `rm -rv`: remover
* `cp -v`, `cp -rv`, `cp -iv`: copia de diretorios
* `mv -v`: movimentacao
* `diff <dir1> <dir2>`, `rsync -r <dir1/> <dir2>`: Sincronizacao e Comparacao.
* `stat <arquivo>`: Exibe metadados detalhados (inodes, acessos, permissoes).
* `ln <origem> <link>`: Cria um Link Fisico (aponta direto para o inode).
* `ln -s <origem> <link>`: Cria um Link Simbolico / Atalho (aponta para o caminho do arquivo).
* `find <diretorio> -name "<padrao>"`: Busca arquivos recursivamente.
* `cat <arquivo>`: Exibe todo o conteudo de um arquivo na tela.
* `head -n <N>`: Mostra as primeiras N linhas de um arquivo de dados.
* `tail -n <N>`: Mostra as ultimas N linhas (`tail -f` acompanha logs em tempo real).
* `wc -l`: Conta o total de linhas de um arquivo.
* `curl`: Realiza requisicoes web ou download de arquivos via linha de comando.

* **Navegaçao de Diretorios:**
  * `cd <pasta>`: Entra em um diretorio especificado.
  * `cd ..`: Sobe um nivel na arvore de pastas (vai para a pasta pai).
  * `cd -`: Retorna ao diretorio anterior.
  * `cd ~` ou `cd`: Vai diretamente para a pasta pessoal do usuario (`/home/usuario`).
  * `pwd`: Mostra o caminho absoluto do diretorio atual (Print Working Directory).
  * `echo $OLDPWD`: Imprime o caminho da pasta onde o usuario estava anteriormente.

* **Leitura e Manipulacao de Texto:** 
* `cat`: exibe todo conteudo de um arquivo de texto direto no terminal
* `more`: leitor de texto paginado, ele exibe o conteudo do arquivo tela por tela, Limitacao: nao consegue voltar, apenas navegar para frente. 
* `less`: leitor mais moderno que o more, ele permite navegar para frente e para trás. 
* `wc`: conta elementos de um arquivo: 
    * `wc -l`: conta apenas o numero de linhas. 
    * `wc -w`: conta as palavras
    * `wc -c`: conta o numero de bytes (caracteres)
* `grep`: filtro para busca de texto, busca por palavras chave. 
    * `grep -i`: busca o termo
    * `grep -v`: inverte a busca, exibe apenas o que nao contem o termo
* `nvim`/`vim` (`:syntax on/off`): editores de texto no terminal. sintax on ativa destaque de sintaxe colorido, off desativa as cores
* `dquote` (`" "`): protecao de textos no shell 
* `echo`: imprime textos, valores, na saida do terminal
*  `echo $SHELL`: exibe o caminho absoluto do interpretador de comandos (shell)
* `zsh`: (Z Shell) interpretador de comandos avancado e altamente customizavel que incorpora recursos do Bash, Ksh e Tcsh

* **Historico de Comandos:** 
* `history`, `!!` : reexecuta o ultimo. 
* `!comando`: busca pelo nome.
* `!<numero>`: executa por ID do historico.

* **Operadores de Controle:** 
* `&`: executa o comando em segundo plano. 
* `&&`: executa o primeiro, se der certo, executa o segundo. Se o primeiro não rodar ele trava.
* `|`: Canaliza a saída de um comando para a entrada do outro. 
* `||`: executa o primeiro comando, se não der certo, executa o próximo. Se o primeiro rodar, não executa o segundo comando. 
* `;`: Executa um comando após o outro. 
---
* **Alternar Shell Padrao (`chsh`):**
  * `chsh -s $(which zsh)`: Altera o shell padrao para `zsh`.
  * `chsh -s $(which bash)`: Altera o shell padrao de volta para `bash`.
---
* **Inspecao de Hardware:**
  * `lsblk`: Lista discos, partiçoes e unidades de bloco em estrutura de arvore.
  * `lscpu`: Detalhes completos do processador (nucleos, arquitetura, cache).
  * `lspci`: Lista dispositivos nos barramentos PCI/PCIe (`-v` verbose, `-vv` very verbose, `-k` exibe os drivers em uso).
  * `lsusb`: Lista todos os dispositivos USB conectados.

---
**Hierarquia do Sistema de Arquivos (FHS)** 
* `/bin`: Executaveis e comandos essenciais para todos os usuarios (`ls`, `cd`, `cat`).
* `/sbin`: Comandos de administracao do sistema exclusivos do usuario root.
* `/boot`: Arquivos necessarios para inicializacao do sistema (GRUB e Kernel Linux).
* `/dev`: Arquivos especiais que representam dispositivos de hardware.
* `/etc`: Arquivos de configuracao do sistema ("Painel de Controle").
* `/home`: Pastas pessoais dos usuarios comuns.
* `/root`: Diretorio pessoal isolado do superusuario.
* `/lib` e `/lib64`: Bibliotecas essenciais e drivers do Kernel.
* `/media` e `/mnt`: Pontos de montagem automaticos e manuais para discos e midias externas.
* `/opt`: Instalaçao de softwares adicionais de terceiros.
* `/usr`: Recursos, programas e arquivos compartilhados dos usuarios.
* `/proc` e `/sys`: Sistemas virtuais contendo informaçoes em tempo real do Kernel e processos.
* `/run`: Dados volateis desde a ultima inicializacao do sistema.
* `/tmp`: Arquivos temporarios (limpos a cada reinicializacao).
* `/var`: Dados variaveis de tamanho dinamico (logs em `/var/log`).
---
* **Caminho Absoluto vs Caminho Relativo:**
  * **Caminho Absoluto:** Inicia obrigatoriamente na raiz e percorre todo o caminho. 
  * **Caminho Relativo:** Inicia a partir do diretorio atual ate onde voce vai. 
---
* **Permissoes de Arquivos & Permissoes Octais**
* **Modo Simbolico:** `chmod o-x`, `chmod o+x`, `chmod -x <diretorio>`.
* **Modo Octal:** `chmod 755 script.sh` (para pastas e scripts), `chmod 644 dados.csv` (para arquivos de dados).

No Linux, as permissoes de arquivos e diretorios sao lidas da esquerda para a direita em 3 blocos de usuarios:
1. **u** (user / dono): O proprietario do arquivo.
2. **g** (group / grupo): O grupo associado ao arquivo.
3. **o** (others / outros): Qualquer pessoa que nao seja o dono e nao pertencao ao grupo.

* **Tabela de Permissoes (rwx)**
* **r** (read / leitura): Valor octal **4**. Permite ler arquivos ou listar pastas.
* **w** (write / escrita): Valor octal **2**. Permite alterar arquivos ou criar/apagar itens em pastas.
* **x** (execute / execucao): Valor octal **1**. Permite rodar programas/scripts ou atravessar/entrar (`cd`) em diretorios.
* **-** (sem permissao): Valor octal **0**.

* **Calculo Octal por Bloco**
* **7** (4 + 2 + 1): `rwx` (Acesso total — padrao para diretorios e scripts executaveis).
* **6** (4 + 2 + 0): `rw-` (Leitura e escrita — padrao para arquivos de dados/texto).
* **5** (4 + 0 + 1): `r-x` (Leitura e execucao).
* **4** (4 + 0 + 0): `r--` (Apenas leitura).
* **0** (0 + 0 + 0): `---` (Sem acesso).

* **Permissoes Especiais, Operadores, Leitura & Atalhos**
SETUID (4 / `u+s`), SGID (2 / `g+s`), STICKY BIT (1 / `+t`).

* **SETUID** (Valor Octal **4** / Caractere `s` no Dono): Executa um arquivo binario com as permissoes do proprietario do arquivo, e nao do usuario que o chamou no terminal (ex: `-rwsr-xr-x`).
* **SGID** (Valor Octal **2** / Caractere `s` no Grupo): Executa com os privilegios do grupo proprietario ou faz novos arquivos criados em uma pasta herdarem o grupo pai (ex: `-rwxr-sr-x`).
* **STICKY BIT** (Valor Octal **1** / Caractere `t` em Outros): Garante permissao de exclusao/renomeacao de arquivos apenas ao dono do arquivo ou ao superusuario (`root`) dentro de diretorios publicos (ex: `drwxrwxrwt`, padrao da pasta `/tmp`).
---
* **Atalhos do Teclado:**
* `Ctrl + C`: Interrompe a execucao do processo.
* `Ctrl + E`: Move o cursor para o final da linha.
* `jobs`: Lista tarefas em segundo plano.
* `fg`: Traz o comando em background de volta para o primeiro plano.
* `!!` ou `Ctrl + P`: Recupera o comando anterior no historico.
* `Ctrl + S`: Pausa a exibicao visual da saida do terminal.
* `Ctrl + Q`: Descongela e retoma a exibicao pausada pelo Ctrl + S.
* `Ctrl + Shift + T`: Abre uma nova aba no terminal grafico.
* `Ctrl + D`: Envia o sinal de fim de arquivo (EOF) e encerra a sessao.
* Ctrl + L`: Limpa a tela do terminal.
* `Tab`: Autocompleta comandos, pastas e arquivos.
* `Ctrl + R`: Realiza busca reversa no historico de comandos.
---
* **Operadores & Redirecionadores**
* `>`: Redireciona a saida sobrescrevendo o conteudo do arquivo.
* `>>`: Redireciona a saida anexando ao final do arquivo (append).
* `2>`: Redireciona mensagens de erro (STDERR).
---
* **Leitura e Edicao de Texto**
* `less`: Leitor paginado de arquivos (pressione `q` para sair).
* `nano` / `vim`: Editores de texto direto no terminal.
* `grep '<termo>'`: Filtra linhas contendo o termo pesquisado.
---
*  **Terminais e Shells**
* `chsh`: (Change Shell): Comando utilizado para alterar o shell padrão do seu usuário no sistema Linux.
`chsh -s $(which zsh)`: Altera para o Zsh.
`chsh -s $(which bash)`: Altera para o Bash
---
* **Identificacao do Usuario e Maquina**
* `whoami`: Imprime o nome do usuario atualmente logado.
* `id`: Exibe o UID, GID e os grupos atribuidos ao usuario.
* `hostname`: Exibe o nome de rede da maquina.
* `uname -a`: Exibe detalhes completos sobre o Kernel Linux.
* `uptime`: Mostra o tempo de execucao do sistema e a carga media de trabalho.

* **Usuarios, Sessoes & Identificacao**
* `useradd`/`adduser`: criar novos usuarios
* `deluser`: remover uma conta ja existente
* `usermod -aG`: modificar as propriedades de um usuario, especificamente adicionando-o a grupos de permissoes adicionais
* `gpasswd -a`: administrar grupos no Linux, a opcao a adiciona um usuario existente a um grupo
* `addgroup`: cria um novo grupo de permissoes no sistema
* `groups`: exibe a lista de grupos
* `id`: exibe detalhes de identificacao numerica do usuario e dos grupos.
* `passwd`: alterar ou definir a senha de um usuario
* `passwd -S`: exibe detalhes da senha de uma conta
* `/etc/passwd` (dados de usuarios e diretorios home) 
* `/etc/shadow` (hashes de senhas criptografadas) - apenas root
---
* **Gerenciamento de Processos & Prioridades**
* `ps`: lista os processos em execucao
* `ps a`: Lista os processos de todos os usuarios que possuem um terminal associado
* `ps ax`: Lista absolutamente todos os processos em execucao no sistema, incluindo servicos que rodam em segundo plano.
* `ps ax | grep`: Combina a listagem completa de processos com o filtro grep para encontrar um processo especifico pelo nome.
* `pidof`: Retorna diretamente o numero de identificacao (PID) de um processo especifico buscando pelo seu nome exato.
* `pgrep`: Filtra e lista os PIDs dos processos cujos nomes correspondem ao termo ou padrao pesquisado.
* `top`: Exibe uma interface interativa em tempo real com os processos ativos no sistema, mostrando o consumo de CPU, memoria RAM, tempo de execucao e prioridades.
* `kill <PID>`: forca o encerramento.

* **Prioridades (Valores Nice):** 
Escala de **-20** (maxima prioridade) a **19** (minima prioridade). 
O valor padrao e **0**. Quanto maior o numero negativo, maior e a exigencia de recursos computacionais.

* **Alterar Prioridade:** 
* `nice -n <valor> <comando>`: Inicia um novo processo ja definindo a sua prioridade inicial.
* `renice -n <valor> -p <PID>`: Altera a prioridade de um processo que ja esta em execucao, identificando-o pelo seu numero de identificacao (PID).
---
*  **Gerenciamento de Servicos & Systemd**
* **O Gerenciador Systemd:** Sistema de inicializacao responsavel por controlar daemons e servicos no boot.
* **Comandos Systemctl:** 
* `systemctl status`: Exibe o estado atual de funcionamento de um servico (se esta ativo, parado ou com falhas).
* `systemctl start`: Inicia a execucao de um servico imediatamente na sessao atual do sistema.
* `systemctl stop`: Interrompe/para imediatamente a execucao de um servico que esteja em andamento.
* `systemctl restart`: Reinicia o servico (faz o stop seguido do start), util para aplicar novas configuracoes alteradas nos arquivos de servico.
* `systemctl enable`: Habilita o servico para que ele seja inicializado automaticamente durante o processo de boot (inicializacao) do sistema operacional.
* `systemctl disable`: Desabilita o carregamento automatico do servico durante o boot do sistema

 **Ferramentas e Estrutura:** 
 * `systemd-cgtop`: monitoramento de consumo em tempo real. * `ls /etc/systemd/system`: links simbolicos das unidades.
---
* **Des/compactadores, Des/empacotadores & Analise de Espaco**
* **Conceito de Empacotamento:** O comando `tar -cvf arq.tar /pasta` empacota os arquivos em um unico item antes da compactacao. Isso e necessario porque compactadores isolados (`gzip`, `bzip2`, `xz`) nao concatenam multiplos arquivos por conta propria.
* **Algoritmos de Compactacao Integrados ao Tar:**
  * `-z`: Usa o `gzip` (opcao `-k` no gzip mantem o arquivo original intacto).
  * `-j`: Usa o `bzip2` (taxa de compressao maior que o gzip).
  * `-J`: Usa o `xz` (maior taxa de compressao entre todos, utilizado no Kernel Linux).

* **Descompactando com Tar (Substituindo o `-c` por `-x`):** `tar -xzvf` (`.tar.gz`), `tar -xjvf` (`.tar.bz2`), `tar -xJvf` (`.tar.xz`).

* **Verificacao de Espaco:** `du -sh *` (exibe o tamanho dos itens em formato legivel KB, MB, GB).
---
* **Shell Script & Automaçao**
* **Estrutura Basica:**
  * **Shebang (`#!/bin/bash`):** Primeira linha obrigatoria que define o interpretador.
  * **Variaveis:** Declaradas sem espacos (`PESSOA=$(whoami)` ou `DATA=$(date)`). Chamadas com `$PESSOA`.
  * **Substituicao de Comando `$()`:** Executa o comando e armazena o retorno na variavel.
* **Funçoes em Scripts:**
  ```bash
  nome_da_funcao () {
      comando 1
      comando 2
  }
  nome_da_funcao # Chamada da funcao