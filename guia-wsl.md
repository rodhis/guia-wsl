# Instalação de Windows subsystem Linux (WSL) 2 (v.1.0)

O WSL é uma ferramenta que permite uma distro Linux rodar como um subsistema dentro do seu Windows, lhe dando acesso ao seu terminal e todas as suas funcionalidades, como git, npm, yarn, node.js e por aí vai, sem precisar instalar arquivos adicionais em seu Windows.

O WSL2 é compatível com Windows 10 ou 11. É importante seguir os passos conforme escrevo aqui, pois existem diversas pequenas dificuldades que precisam ser evitadas ou vencidas para que tudo funcione corretamente.

**Passo a passo:**

1. Instale o WSL2 com a distro de escolha (para uso no bootcamp FS JS TEX, recomendo Ubuntu (sem versão) (NOTA: corresponde à versão 22.04.1, com última atualização em 14/03/2022, sendo a mais atualizada no momento que escrevi este documento). Para tanto, siga os passos do vídeo abaixo, que mostra a instalação completa, bem como o uso de diversos comandos que você precisará depois. É IMPORTANTE VERIFICAR ATUALIZAÇÕES TAMBÉM, como mostrado no vídeo!
   https://www.youtube.com/watch?v=o1_E4PBl30s

2. Ao abrir o terminal do Linux ele te pedirá um novo nome de usuário. Este precisa ser algo como “seunome_linux” ou “nome_ubuntu” (sem as aspas) . É necessário colocar “linux” ou o nome da distro no seu usuário por ser um padrão do WSL (nota: não testei todas as combinações para saber EXATAMENTE o que é necessário, mas sei que, com nome e distro, é o suficiente). Logo após ele pedirá uma senha, que será utilizada para quase tudo do Linux. Sugiro escolher algo fácil.

3. Verifique se existem atualizações da sua distro. No Ubuntu, use os comandos: <br/>
<pre>•   sudo apt update <br/>
•   sudo apt -y upgrade <br/></pre>

4. Instale o VSCode em seu **Windows**, com o instalador normal do site da Microsoft: https://code.visualstudio.com/download . **Não o abra após a instalação!**

5. Instale o Git em seu Linux, caso este já não venha instalado por padrão. Use o comando no terminal:

<pre>•	sudo apt install git-all</pre>

6. Faça toda a configuração de integração do seu Git com GitHub através do terminal. Ao fim, dê um git clone em algum repositório seu.

7. **Importante!** Vá no repositório clonado ainda no terminal do Linux e abra-o com o comando “code .” (sem as aspas). Isso fará o terminal baixar as dependências necessárias para concluir a integração do seu Git do WSL e VSCode do Windows.

8. No VSCode, abra um novo terminal, clique no + e vá em “select default profile”. Selecione wsl (que pode estar já como bash, se já fez todo o necessário para a configuração).
   
9.  Tudo estará ok se o seu perfil do GitHub estiver em Sync com seu VSCode e seu terminal abaixo estiver verde com “WSL:nomedadistro”, conforme imagem:

Cuidados importantes:
a) Evite usar comandos do Windows (Ctrl c, del, clique direito etc.) para mexer nas pastas que contém os diretórios do WSL. Isso costuma dar erros de permissão. Use o terminal do Linux.
b) Clone seus repositórios do GitHub ao invés de copiar e colar de um backup em disco ou nuvem. Isso evita erros de permissão com o Git também.
c) Sempre abra uma nova instância do VSCode ou uma pasta numa janela nova de WSL. Para fazer isso você pode dar “open folder” ou clicar no botão verde da imagem e ir em “new wsl Window”, onde pode, então, abrir sua repo com o VSCode integrado. Abrir diretamente pelo Windows não integra o Git WSL e gera diversos erros de permissão e untrusted files, então evite fazer assim!
d) Não é necessário instalar Git no Windows com esse método!

Eliminando arquivos Zone.identifier
Ao usar WSL muitos desses arquivos podem aparecer quando você baixa ou cola arquivos na sua pasta Linux. Eles servem para identificar a fonte de origem, mas, fora isto, apenas bagunçam tudo. Para resolver este problema:
a) Use o atalho do Windows “Win + R” e digite na caixa de executar “gpedit.msc”.
b) Siga o caminho: Configuração de Usuário -> Modelos administrativos -> Componentes do Windows -> Gerenciador de Anexos.
c) Clique duas vezes em “Não preservar informações de zona em anexos de arquivos”;
d) Marque a opção “habilitado”.
(OBS: Para acessar o editor de políticas de grupo é necessário possuir Windows 10 Pro ou superior.)

e) Para limpar arquivos Zone.identifier já existentes abra um terminal no Linux e use o comando:

• find “pasta alvo” -type f -name "\*:Zone.Identifier" -exec rm -f {} \;

(substitua pasta alvo pelo caminho onde estão seus arquivos, como a pasta padrão da T.EX, por exemplo).

Configuração necessária do Live Server:
Para utilizar a extensão Live Server, podem ser necessárias configurações adicionais para que esta funcione. Siga os passos:
a) Vá em Extension Settings do Live Server. Logo no início estará a opção “Advance Custom Broswer Cmd Line”. Clique em “Edit in settings.json”.

b) Na nova janela que se abrir, adicione ou edite as linhas antes do último fechamento com chave } :

• "liveServer.settings.CustomBrowser": "chrome",

• "liveServer.settings.AdvanceCustomBrowserCmdLine": "/mnt/c/Program Files/Google/Chrome/Application/chrome.exe",
Vai ficar algo assim:

Note que esta configuração é para abrir com o Chrome. Para outros navegadores, é necessário editar o CustomBrowser e indicar o caminho específico de seu executável na linha de baixo.
Gerenciador de pacotes, SASS, Vue, Node.js e outros:
Estes recursos de programação são instalados via gerenciadores de pacotes, como npm ou yarn. Aqui, usaremos npm. Para instalá-lo, utilize no terminal:
• curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

- Para instalar SASS:
  • npm install -g sass
- Para o Vue:
  • npm install vue@next
  • sudo npm install -g @vue/cli-
  Ver instruções detalhadas nas aulas 2.8.11 e 2.8.12
  Para node.js, utilizar este tutorial:
  https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl
  NOTA: Se acontecerem erros de console na instalação de alguns desses, use “sudo” antes dos comandos para remover as necessidades de permissão de acesso.
  Extras:
  a) Use o Windows Terminal como indicado no vídeo. Clique na setinha ao lado do indicador de janela do terminal que está aberto, vá em configurações. Em Inicialização, você pode colocar para abrir diretamente o terminal WSL. Em Padrões, pode colocar para sempre abrir como administrador, o que economiza tempo.
  b) Se quiser remover a maioria das requisições de senha do Linux, use o comando:
  • sudo visudo
  No arquivo que abrir, procure as linhas com %admin e %sudo, e troque o final das linhas:
  • ALL
  por
  • NOPASSWD: ALL
  Aperte Ctrl x, aperte y e dê um enter para salvar as configurações novas.
