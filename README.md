
## Introdução

Para entender nosso objetivo, precisamos saber o que é cada ferramenta:

**WSL (Windows Subsystem for Linux):** É uma camada de compatibilidade criada pela Microsoft que permite executar binários Linux (como programas e ferramentas de linha de comando) diretamente no Windows. O WSL 2, versão mais recente, utiliza um kernel Linux real, oferecendo desempenho e compatibilidade quase nativos.
**Ubuntu:** É uma das distribuições Linux mais populares e amigáveis para iniciantes. Ao instalar o WSL, podemos escolher o Ubuntu como nosso "sistema operacional" Linux que rodará dentro do Windows.
**GCC (GNU Compiler Collection):** É o conjunto de compiladores padrão para a maioria dos sistemas Unix-like. Usaremos seu compilador C (também chamado `gcc`) para transformar nosso código-fonte `.c` em programas executáveis.
**VS Code (Visual Studio Code):** É um editor de código-fonte leve, gratuito e extremamente poderoso da Microsoft. Sua maior força são as extensões, e usaremos uma específica (a extensão "WSL") para que nosso editor, rodando no Windows, possa se comunicar diretamente com os arquivos e ferramentas dentro do Ubuntu.

## Instalação e Configuração

Vamos configurar o ambiente do zero.

### PASSO 1 Instalar o WSL e o Ubuntu
::: {.callout-important}
Para que essa VM funcione, é necessário que a virtualização por hardware esteja ativada no processador e na BIOS/UEFI.
No Windows:

Pressione Ctrl + Shift + Esc para abrir o Gerenciador de Tarefas.

Vá até a aba Desempenho → CPU.

Verifique a linha "Virtualização":

Se aparecer “Habilitada”, está tudo certo ✅

Se aparecer “Desabilitada”, será preciso ativar na BIOS/UEFI ⚙️

⚙️ Como ativar a virtualização na BIOS/UEFI

Reinicie o computador e pressione a tecla de acesso à BIOS (geralmente Del, F2, F10 ou Esc, dependendo da marca).
Procure por uma opção chamada:
Intel Virtualization Technology (VT-x) — para processadores Intel, ou
SVM Mode / AMD-V — para processadores AMD.
Ative a opção.

Salve as alterações e reinicie o computador.
:::   

1.  Abra o **PowerShell** ou **Prompt de Comando (CMD)** como **Administrador**.
2.  Digite o seguinte comando:

    ```bash
    wsl --install
    ```
3.  Este comando fará tudo automaticamente: habilitará os recursos necessários do Windows, baixará o kernel Linux mais recente e instalará a distribuição **Ubuntu** por padrão.
4. **Uma distribuição Linux instalada** (ex: Ubuntu).  
   Você pode instalar pela Microsoft Store ou via terminal.

5.  Após a conclusão, **reinicie o seu computador**.
6.  Ao reiniciar, o Ubuntu será iniciado pela primeira vez para finalizar a configuração. Você precisará criar um **nome de usuário** e uma **senha** para o seu ambiente Linux. (Nota: Esta senha não tem relação com sua senha do Windows).

### PASSO 2 Atualizar o Ubuntu e Instalar o GCC

Agora que temos o Ubuntu, precisamos instalar as ferramentas de desenvolvimento C.

1.  Abra o terminal do Ubuntu (pelo Menu Iniciar, procure por "Ubuntu").
2.  Primeiro, vamos atualizar os repositórios de pacotes:

    ```bash
    #Atualiza a lista de pacotes disponíveis
    sudo apt update
    
    #Atualiza os pacotes já instalados para suas últimas versões
    sudo apt upgrade
    ```
    *Comentário: **sudo** significa "Super User Do", e é usado para executar comandos com privilégios de administrador. Você precisará digitar a senha do Linux que acabou de criar.*

3.  Agora, instale o `build-essential`. Este pacote inclui o `gcc`, o `make` e outras ferramentas essenciais para compilação.

    ```bash
    # Instala o compilador GCC e outras ferramentas essenciais
    sudo apt install build-essential
    ```
4.  Para verificar se o GCC foi instalado corretamente, execute:

    ```bash
    gcc --version
    ```
    *Você deverá ver uma mensagem com a versão do GCC.*

### PASSO 3 Instalar e Configurar o VS Code

1.  **Instale o VS Code no Windows** (e não dentro do Ubuntu). Baixe-o diretamente do site oficial https://code.visualstudio.com/.
2.  Abra o VS Code.
3.  Vá até a aba de **Extensões** (ícone de blocos no menu lateral ou `Ctrl+Shift+X`).
4.  Procure e instale a extensão chamada **"WSL"** (publicada pela Microsoft).

### PASSO 4 (O "Hello, World!")

1.  **Feche** qualquer instância do VS Code que esteja aberta.
2.  Abra o seu **terminal do Ubuntu**.
3.  Vamos criar um diretório para nossos projetos C *dentro* do Linux (isso é importante para o desempenho):

    ```bash
    #Cria um diretório chamado 'projetos_c' na sua pasta 'home'
    mkdir ~/projetos_c
    
    #Entra no diretório recém-criado
    cd ~/projetos_c
    ```
4.  Agora, dentro deste diretório no terminal do Ubuntu, digite o comando mágico:

    ```bash
    code .
    ```
5.  *O que acontece?*
    * O Windows abrirá o VS Code.
    * O VS Code, usando a extensão WSL, se conectará ao seu ambiente Ubuntu. Você verá uma indicação "WSL: Ubuntu" no canto inferior esquerdo do editor.
    * O VS Code estará "enxergando" os arquivos de dentro do diretório `~/projetos_c` do Ubuntu.

6.  **Criando o "Hello, World!"**
    No explorador de arquivos do VS Code (menu lateral), crie um novo arquivo chamado `hello.c`.
    Digite o seguinte código:

    ```c
    // hello.c
    // Nosso primeiro programa em C no ambiente WSL
    
    #include <stdio.h> // Inclui a biblioteca de Input/Output Padrão
    
    int main() {
        // Imprime a string "Hello, WSL!" no console
        printf("Hello, WSL!\n");
        
        return 0; // Retorna 0 para indicar que o programa rodou com sucesso
    }
    ```

7.  **Compilando e Executando**
    * Abra o terminal integrado do VS Code (`Ctrl+'` ou `Terminal > Novo Terminal`).
    * **Observe:** Este *não* é um terminal do Windows. É um terminal `bash` rodando diretamente no seu Ubuntu!
    * Para compilar, digite:

    ```bash
    # O comando gcc compila o arquivo 'hello.c'
    # A flag '-o hello' diz para criar um arquivo executável chamado 'hello'
    gcc hello.c -o hello
    ```
    * Se você olhar no explorador de arquivos, verá um novo arquivo `hello` (com ícone de executável).
    * Para executar o programa, digite:

    ```bash
    # O './' é necessário para dizer ao terminal para executar o arquivo
    # 'hello' que está neste diretório atual.
    ./hello
    ```
    * **Saída:** Você verá a mensagem `Hello, WSL!` impressa no seu terminal.

## 4. Boas Práticas 

* **Armadilha do Sistema de Arquivos:** **Sempre** armazene seus projetos Linux (como seu código C) *dentro* do sistema de arquivos do Ubuntu (ex: `/home/seu_usuario/projetos_c`). **Não** os armazene no seu C: do Windows (ex: `/mnt/c/Users/...`). Acessar arquivos do Windows a partir do Linux é muito lento e pode causar problemas de permissão.
* **Terminal:** Use o terminal integrado do VS Code. Ele estará automaticamente conectado ao WSL, permitindo que você compile e execute sem sair do editor.
* **Mantenha Atualizado:** Lembre-se de rodar `sudo apt update && sudo apt upgrade` de vez em quando no seu terminal Ubuntu para manter tudo seguro e atualizado.

---

Nesta tutorial, você aprendeu

✅ Instalar o WSL e o Ubuntu

✅ Instalar e Configurar o VS Code

--- 

## 6. Links Úteis e Referências

* Documentação Oficial do WSL na Microsoft (https://docs.microsoft.com/pt-br/windows/wsl/)
* Documentação do VS Code para desenvolvimento em C++ (aplica-se ao C) (https://code.visualstudio.com/docs/cpp/config-wsl)
* Manual Oficial do GCC (https://gcc.gnu.org/onlinedocs/)
* Compilar Programas em C (https://brunofx-ufersa.github.io/reacomp/posts/compilation-clinux.html)
