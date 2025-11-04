# Uso do Visual Studio Code com o Subsistema do Windows para Linux (WSL)

**Autor:** JOÃO VICTOR FERREIRA DA SILVA E  FRANCISCO VINÍCIUS DE BRITO ALENCAR  
**Professor:** BRUNO FRANCISCO XAVIER  
**Data:** 03/11/2025

## Sumário
1. [Introdução](#introdução)
2. [O que é o WSL](#o-que-é-o-wsl)
3. [Por que usar o VS Code com o WSL](#por-que-usar-o-vs-code-com-o-wsl)
4. [Pré-requisitos](#pré-requisitos)
5. [Instalação do VS Code e Extensão WSL](#instalação-do-vs-code-e-extensão-wsl)
6. [Abrindo Projetos no WSL com o VS Code](#abrindo-projetos-no-wsl-com-o-vs-code)
7. [Arquitetura Cliente-Servidor do VS Code + WSL](#arquitetura-cliente-servidor-do-vs-code--wsl)
8. [Gerenciamento de Extensões no WSL](#gerenciamento-de-extensões-no-wsl)
9. [Uso do Git e Controle de Versão](#uso-do-git-e-controle-de-versão)
10. [Boas Práticas e Dicas](#boas-práticas-e-dicas)
11. [Conclusão](#conclusão)
12. [Referências](#referências)



## 1. Introdução

O **Visual Studio Code (VS Code)** é um editor de código-fonte leve e rápido. 
Com o **Subsistema do Windows para Linux (WSL)**, é possível executar um ambiente Linux dentro do Windows sem necessidade de máquina virtual ou dual boot.

A integração entre VS Code e WSL permite **desenvolver, depurar e executar projetos Linux diretamente no Windows**, mantendo o desempenho e as ferramentas nativas de ambos os sistemas.

> Referência oficial: [Microsoft Learn – Usar o VS Code com o WSL](https://learn.microsoft.com/pt-br/windows/wsl/tutorials/wsl-vscode)

---

## 2. O que é o WSL

O **Windows Subsystem for Linux (WSL)** é uma camada de compatibilidade que permite executar distribuições Linux nativamente no Windows.  
Com ele, você pode usar terminal, ferramentas, pacotes e bibliotecas do Linux sem sair do Windows.

Há duas versões principais:
- **WSL 1:** mais simples, usa tradução de chamadas do sistema.  
- **WSL 2:** usa um kernel Linux completo, oferecendo melhor compatibilidade e desempenho (recomendado e padrão atual).

Você pode instalar distribuições como **Ubuntu, Debian, Kali Linux, Fedora**, entre outras, diretamente pela Microsoft Store.


## 3. Por que usar o VS Code com o WSL

Usar o VS Code junto ao WSL oferece diversas vantagens:

- Desenvolvimento em ambiente **nativo Linux** com o conforto do Windows.  
- Acesso às **ferramentas Linux** (gcc, apt, bash, etc.).  
- Possibilidade de depuração e execução direta no WSL.  
- Evita problemas de compatibilidade entre caminhos de arquivos Windows/Linux.  
- Integração total com **Git** e controle de versão.  
- Terminal integrado que pode ser configurado para abrir diretamente no Linux.



## 4. Pré-requisitos

Antes de começar, você deve ter:

1. **WSL instalado** - Execute no PowerShell (como Administrador):
     ```bash
     wsl --install
     ```
   - Reinicie o computador após a instalação.

2. **Uma distribuição Linux instalada** (ex: Ubuntu).  
   O comando `wsl --install` geralmente instala o Ubuntu por padrão.

3. **Visual Studio Code instalado no Windows** - Baixe em: [https://code.visualstudio.com](https://code.visualstudio.com)



## 5. Instalação do VS Code e Extensão WSL

### 1. Instalar o VS Code
Baixe e instale o VS Code para **Windows**. Durante a instalação, na tela "Tarefas Adicionais", é crucial marcar a opção **"Adicionar ao PATH"**. Isso permite que o WSL chame o VS Code do Windows usando o comando `code`.

### 2. Instalar a extensão WSL
No VS Code, acesse o menu de **Extensões** (ícone de blocos ou `Ctrl+Shift+X`).
Na barra de pesquisa, digite `WSL` e instale a extensão oficial **"WSL"** publicada pela Microsoft.



Esta extensão faz parte do pacote "Remote Development", que é o que permite a arquitetura cliente-servidor.


## 6. Abrindo Projetos no WSL com o VS Code

Existem duas formas principais de iniciar a conexão.

### Método 1: Via Terminal WSL (Recomendado)

Este é o fluxo de trabalho mais rápido e comum.

1.  Abra o terminal da sua distribuição Linux (Ex: "Ubuntu" no Menu Iniciar).
2.  Navegue até a pasta do seu projeto **dentro do sistema de arquivos do Linux** (ex: `/home/seu_usuario/projetos/meu-app`).
    ```bash
    cd ~/projetos/meu-app
    ```
3.  Execute o comando:
    ```bash
    code .
    ```
4.  O VS Code (do Windows) será iniciado, e ele se conectará automaticamente ao "Servidor VS Code" rodando dentro do WSL, abrindo a pasta atual.

### Método 2: Via Paleta de Comandos do VS Code

1.  Abra o VS Code (no Windows).
2.  Pressione `Ctrl+Shift+P` para abrir a Paleta de Comandos.
3.  Digite `WSL: Connect to WSL` e selecione a opção.
4.  Uma nova janela do VS Code será aberta, já conectada ao WSL.
5.  Você pode então usar "Arquivo" > "Abrir Pasta..." para navegar até seu diretório de projeto no Linux (ex: `/home/seu_usuario/projetos`).

**Verificando a Conexão:** No canto inferior esquerdo do VS Code, você verá um indicador verde mostrando que está conectado ao WSL: `WSL: Ubuntu` (ou sua distro).



## 7. Arquitetura Cliente-Servidor do VS Code + WSL

A integração não é um simples "abrir arquivo". A extensão WSL divide o VS Code em uma arquitetura de dois componentes:

1.  **Cliente (Frontend):** É a interface de usuário do VS Code que você vê e interage, rodando no **Windows**.
2.  **Servidor (Backend):** É um "Servidor VS Code" leve que é instalado e executado **dentro do WSL (Linux)**.

Este servidor no Linux é quem de fato:
- Executa o terminal integrado (bash, zsh, etc.).
- Analisa o código (IntelliSense, linting).
- Gerencia e executa as extensões de linguagem (Python, Node.js, etc.).
- Executa o depurador.
- Lida com os comandos do Git.

Isso garante que todas as operações intensivas ocorram **nativamente no Linux**, proporcionando desempenho total, enquanto você usa a interface gráfica fluida no Windows.

---

## 8. Gerenciamento de Extensões no WSL

Com a arquitetura cliente-servidor, as extensões também são divididas:

- **Extensões de UI (Locais):** Coisas que mudam a aparência do editor, como **Temas de Cores** e **Pacotes de Ícones**. Elas são instaladas **uma vez, no VS Code do Windows**.
- **Extensões de Workspace (Remotas):** Coisas que interagem com seu código, como **suporte a linguagens (Python, Go)**, **linters (ESLint)** e **depuradores**. Elas precisam ser instaladas **"no WSL"**.

Ao se conectar ao WSL e abrir o painel de Extensões, você verá duas seções: "LOCAL - INSTALADAS" e "WSL: [SUA DISTRO] - INSTALADAS".

Se você tem uma extensão de linguagem (como "Python") instalada localmente, o VS Code mostrará um botão verde para "Instalar no WSL". Você deve clicar nele para que a extensão funcione corretamente no seu ambiente Linux.

---

## 9. Uso do Git e Controle de Versão

O VS Code se integra perfeitamente com o Git instalado *dentro* do WSL, não com o Git do Windows.

1.  **Instale (ou verifique) o Git no WSL:**
    A maioria das distribuições já vem com o Git, mas é bom garantir:
    ```bash
    sudo apt update
    sudo apt install git
    ```

2.  **Configure o Git no WSL:**
    É fundamental configurar seu nome e e-mail *dentro* do Linux, pois é este Git que o VS Code usará.
    ```bash
    git config --global user.name "Seu Nome"
    git config --global user.email "seu.email@exemplo.com"
    ```

3.  **Fluxo de Trabalho:**
    Pronto! Ao abrir a aba "Controle do Código-Fonte" (`Ctrl+Shift+G`) no VS Code, ele detectará automaticamente o repositório Git no WSL e todos os comandos (commit, push, pull, branch) funcionarão nativamente.

---

## 10. Boas Práticas e Dicas

**Armazenamento de Arquivos (Desempenho):** Sempre armazene os arquivos do seu projeto **dentro do sistema de arquivos do Linux** (ex: `/home/seu_usuario/projetos`). Evite trabalhar em arquivos que estão no sistema do Windows (ex: `/mnt/c/Users/SeuUsuario/Documentos`). O acesso entre os sistemas de arquivos (WSL 2 para Windows) é lento e pode causar gargalos.

**Integridade dos Arquivos:** **NUNCA** use o Windows File Explorer ou aplicativos do Windows (como Bloco de Notas) para modificar arquivos que estão *dentro* do WSL (ex: `/home/seu_usuario`). Isso pode corromper os metadados e permissões dos arquivos Linux. Use sempre o VS Code conectado ao WSL ou um editor de terminal (como `nano` ou `vim`) para editar esses arquivos.

**Comando `code .`:** Acostume-se a abrir seus projetos. É o método mais rápido e garante que você esteja no contexto correto.

**Terminal Integrado:** Use o terminal integrado do VS Code (`Ctrl+` \` ) para todos os seus comandos. Ele já estará 100% conectado ao shell da sua distribuição Linux no WSL.



## 11. Conclusão

A integração do VS Code com o WSL 2 oferece o ambiente de desenvolvimento Linux mais produtivo e de melhor desempenho disponível no Windows.

Ao adotar a arquitetura cliente-servidor e seguir as boas práticas de armazenamento de arquivos, o desenvolvedor obtém uma experiência fluida, rápida e totalmente integrada, combinando as melhores ferramentas de ambos os sistemas operacionais sem a sobrecarga de uma máquina virtual tradicional.


## 12. Referências

**Documentação Oficial (Referência Principal):** [https://learn.microsoft.com/pt-br/windows/wsl/tutorials/wsl-vscode](https://learn.microsoft.com/pt-br/windows/wsl/tutorials/wsl-vscode)

**Documentação do VS Code - Remote Development:** [https://code.visualstudio.com/docs/remote/wsl](https://code.visualstudio.com/docs/remote/wsl)

**Boas Práticas de Git no WSL:** [https://learn.microsoft.com/pt-br/windows/wsl/tutorials/wsl-git](https://learn.microsoft.com/pt-br/windows/wsl/tutorials/wsl-git)
