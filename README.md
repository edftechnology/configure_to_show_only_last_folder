# Como instalar/configurar/usar para `exibir somente a última pasta` no `Terminal Emulator` do `Linux Ubuntu`

## Resumo

Neste documento estão contidos os principais comandos e configurações para instalar/configurar para mostrar somente a última pasta no terminal no `Linux Ubuntu`.

## _Abstract_

_This document contains the main commands and settings to install/configure to show only the last folder in the terminal in `Linux Ubuntu`._


## Descrição

### `shell`

Um _shell_ é uma _interface_ de linha de comando que permite aos usuários interagirem com um sistema operacional ou _software_ por meio de comandos de texto. Ele atua como uma camada intermediária entre o usuário e o núcleo do sistema operacional, permitindo a execução de tarefas, manipulação de arquivos, gerenciamento de processos e configuração do sistema. Os _shells_ são comumente encontrados em sistemas `Unix-like`, como o `Linux`, e também estão disponíveis em outros sistemas operacionais, como o `Windows`. Eles podem variar em complexidade e recursos, desde _shells_ simples que fornecem apenas funcionalidades básicas até _shells_ avançados, como o `Bash`, o `PowerShell` e o `Zsh`, que oferecem automação avançada, _scripting_ e recursos de personalização. Os _shells_ desempenham um papel fundamental na automação de tarefas, na administração de sistemas e na programação de _scripts_, tornando-se uma ferramenta essencial para administradores de sistemas, desenvolvedores e usuários avançados.


## 1. Configurar/Instalar para exibir somente a última pasta no `Terminal Emulator` do `Linux Ubuntu` [1]

Para fazer com que o `Terminal Emulator` do `Linux Ubuntu` mostre apenas o nome da última pasta (ao invés do caminho completo), você precisa modificar o arquivo de configuração do `shell` que você está usando. Para o `bash`, este arquivo é geralmente `~/.bashrc`. Para o `zsh`, é `~/.zshrc`.

Vou mostrar como fazer isso para o `bash`, mas o processo é similar para outros _shells_.

1. Abrir o `Terminal Emulator`. Você pode fazer isso pressionando:

    ```bash
    Ctrl + Alt + T
    ```


2. Certifique-se de que seu sistema esteja limpo e atualizado.

    2.1 Limpar o `cache` do gerenciador de pacotes `apt`. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo `apt` e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando:
    ```bash
    sudo apt clean
    ```

    2.2 Remover pacotes `.deb` antigos ou duplicados do `cache` local. É útil para liberar espaço, pois remove apenas os pacotes que não podem mais ser baixados (ou seja, versões antigas de pacotes que foram atualizados). Digite o seguinte comando:
    ```bash
    sudo apt autoclean
    ```

    2.3 Remover pacotes que foram automaticamente instalados para satisfazer as dependências de outros pacotes e que não são mais necessários. Digite o seguinte comando:
    ```bash
    sudo apt autoremove -y
    ```

    2.4 Buscar as atualizações disponíveis para os pacotes que estão instalados em seu sistema. Digite o seguinte comando e pressione `Enter`:
    ```bash
    sudo apt update
    ```

    2.5 **Corrigir pacotes quebrados**: Isso atualizará a lista de pacotes disponíveis e tentará corrigir pacotes quebrados ou com dependências ausentes:
    ```bash
    sudo apt --fix-broken install
    ```

    2.6 Limpar o `cache` do gerenciador de pacotes `apt` novamente:
    ```bash
    sudo apt clean
    ```

    2.7 Para ver a lista de pacotes a serem atualizados, digite o seguinte comando e pressione `Enter`:
    ```bash
    sudo apt list --upgradable
    ```

    2.8 Realmente atualizar os pacotes instalados para as suas versões mais recentes, com base na última vez que você executou `sudo apt update`. Digite o seguinte comando e pressione `Enter`:
    ```bash
    sudo apt full-upgrade -y
    ```

3. Abrir o arquivo `~/.bashrc` em um editor de texto. Você pode fazer isso diretamente do `Terminal Emulator`:

    ```bash
    sudo nano ~/.bashrc
    ```

4. Encontre as linhas que define os `PS1`, que é o `prompt` do `shell`. As linhas podes se parecer com:

    ```bash
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
    ```

    Veja o exemplo do código inteiro abaixo, perceber que, neste caso, existem 3 (três) linhas a serem alteradas como segue no próximo Item:

    ```bash
    if [ "$color_prompt" = yes ]; then
        PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\W>
    else
        PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W\$ '
    fi
    unset color_prompt force_color_prompt

    # If this is an xterm set the title to user@host:dir
    case "$TERM" in
    xterm*|rxvt*)
        PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \W\a\]$PS1"
        ;;
    *)
        ;;
    esac
    ```

5. Modifique esta linha para mostrar apenas o nome da última pasta. Uma forma de fazer isso é:

    ```bash
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W\$ '
    ```

    Aqui, `\W` faz com que apenas o nome da última pasta seja exibido.

6. Depois de fazer a alteração, salve o arquivo e saia do editor:

    6.1 **Salvar o arquivo:** Pressione para salvar as mudanças:

    ```bash
    Ctrl + O
    ``` 

    Isso abrirá uma linha na parte inferior do editor perguntando o nome do arquivo para salvar. 

    6.2 Se você deseja manter o mesmo nome, simplesmente pressione `Enter`.

    6.3 **Sair do nano:** Para sair do editor nano após salvar o arquivo, pressione: `Ctrl + X`

7. Aplique as mudanças com o comando:

    ```bash
    source ~/.bashrc
    ```

    Agora, seu _prompt_ do `Terminal Emulator` deve mostrar apenas o nome da última pasta.

### 1.1 Explicação dos códigos

- **`nano ~/.bashrc`:** Abre o arquivo de configuração `.bashrc` usando o editor de texto `nano`.

- **`PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '`:** Esta é uma linha típica que você encontrará no arquivo `.bashrc`. Ela configura a aparência do _prompt_ do `Terminal Emulator`.

    - **`\u`:** Nome do usuário

    - **`\h:`** Nome do `host`

    - **`\w:`** Caminho completo do diretório atual

    - **`\$:`** Símbolo do _prompt_ (geralmente `$` para usuários normais e `#` para o `root`)

- **`PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W\$ '`:** Modificamos `\w` para `\W` para mostrar apenas o nome do último diretório.

    - **`source ~/.bashrc`:** Este comando aplica as alterações feitas no arquivo `.bashrc` sem a necessidade de reiniciar o `Terminal Emulator`.


## 2. Código completo para configurar/instalar

Para instalar o `Telegram Desktop` no `Linux Ubuntu` sem precisar digitar linha por linha, você pode seguir estas etapas:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando:

    ```bash
    Ctrl + Alt + T
    ```

2. Digite o seguinte comando e pressione `Enter`:

    ```bash
    **NÃO** há.
    ```

## Referências

[1] OPENAI. ***Instalar o `exibir somente a última pasta` no `terminal emulator` do `linux ubuntu`*** Disponível em: <https://chat.openai.com/c/6f1089c0-b7c9-4e65-a481-be2cb145bb46> (texto adaptado). Acessado em: 24/10/2023 16:46.

[2] OPENAI. ***Vs code: editor popular.*** Disponível em: <https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42> (texto adaptado). ChatGPT. Acessado em: 06/02/2024 09:28.

