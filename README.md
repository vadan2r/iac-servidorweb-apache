# Script de Provisionamento de Servidor Web (Apache)

Este documento descreve o processo para criar um script Bash que automatiza o provisionamento de um servidor web Apache, incluindo a instalação do Apache, a instalação do `unzip`, o download e a instalação de uma aplicação web.

## Pré-requisitos

*   Sistema Linux (preferencialmente Debian/Ubuntu para este exemplo) com acesso ao terminal.
*   Acesso de `root` ou permissões `sudo`.
*   Editor de texto para criar o script Bash (ex: `nano`, `vim`, `gedit`).
*   Cliente Git instalado e configurado (para subir o script no GitHub).
*   VirtualBox with a snapshot created and ready to be restored.

## Passo a Passo

1.  **Criação do Arquivo Bash:**

    *   Abra o terminal.
    *   Crie um novo arquivo chamado `provisiona_web.sh` (ou qualquer nome de sua preferência) com o editor de texto de sua escolha:
        ```bash
        nano provisiona_web.sh
        ```

2.  **Estrutura do Script:**

    *   Adicione o shebang no início do arquivo para indicar que é um script Bash:
        ```bash
        #!/bin/bash
        ```
    *   Adicione comentários para organizar e documentar o script.

3.  **Restaurar Snapshot (VirtualBox):**
   *   **Important:** This step will depend of the command line tool installed to access virtual box.

        ```bash
        # Restaurar o snapshot (VirtualBox)
        VBoxManage snapshot "<nome_da_VM>" restore "<nome_do_snapshot>"
        ```

        *   Replace `<nome_da_VM>` with the actual name of your Virtual Machine.
        *   Replace `<nome_do_snapshot>` with the actual name of the snapshot you wish to restore.

4.  **Atualizar o Servidor:**

    *   Adicione o comando para atualizar os pacotes do sistema:
        ```bash
        # Atualizar o servidor
        apt update && apt upgrade -y
        ```
        *   `-y` assume "yes" para todas as perguntas durante a atualização.

5.  **Instalar o Apache2:**

    *   Adicione o comando para instalar o Apache2:
        ```bash
        # Instalar o Apache2
        apt install apache2 -y
        ```

6.  **Instalar o Unzip:**

    *   Adicione o comando para instalar o `unzip`, necessário para descompactar arquivos .zip:
        ```bash
        # Instalar o unzip
        apt install unzip -y
        ```

7.  **Baixar a Aplicação:**

    *   Adicione o comando para baixar a aplicação do GitHub para o diretório `/tmp`:
        ```bash
        # Baixar a aplicação
        wget https://github.com/denilsonbonatti/linux-site-dio/archive/refs/heads/main.zip -P /tmp
        ```
        *   `-P` especifica o diretório de destino.

8.  **Copiar os Arquivos da Aplicação:**

    *   Descompacte o arquivo zip e copie os arquivos para o diretório padrão do Apache (geralmente `/var/www/html`):
        ```bash
        # Descompactar a aplicação e copiar para o diretório do Apache
        unzip /tmp/main.zip -d /tmp
        cp -r /tmp/linux-site-dio-main/* /var/www/html/
        chown -R www-data:www-data /var/www/html
        ```
        *   `-d` especifica o diretório de destino para descompactar.
        *   `cp -r` copia recursivamente todos os arquivos e diretórios.
        *   `chown -R www-data:www-data` change the owner and group to the Apache User and group.

9.  **Restart Apache Server**

    *   Restart the Apache Server to load the new content.

    ```bash
     systemctl restart apache2
    ```

10. **Script Completo de Exemplo:**

    ```bash
    #!/bin/bash

    # Restaurar o snapshot (VirtualBox)
    #VBoxManage snapshot "<nome_da_VM>" restore "<nome_do_snapshot>"

    # Atualizar o servidor
    apt update && apt upgrade -y

    # Instalar o Apache2
    apt install apache2 -y

    # Instalar o unzip
    apt install unzip -y

    # Baixar a aplicação
    wget https://github.com/denilsonbonatti/linux-site-dio/archive/refs/heads/main.zip -P /tmp

    # Descompactar a aplicação e copiar para o diretório do Apache
    unzip /tmp/main.zip -d /tmp
    cp -r /tmp/linux-site-dio-main/* /var/www/html/
    chown -R www-data:www-data /var/www/html

    systemctl restart apache2

    echo "Servidor web provisionado com sucesso!"
    ```

11. **Tornar o Script Executável:**

    *   Dê permissão de execução ao script:
        ```bash
        chmod +x provisiona_web.sh
        ```

12. **Executar o Script:**

    *   Execute o script como `root` (ou com `sudo`):
        ```bash
        sudo ./provisiona_web.sh
        ```

13. **Verificação:**

    *   Após a execução, verifique se o Apache está rodando e se a aplicação web está acessível no navegador (geralmente `http://localhost` ou `http://<ip_do_servidor>`).

14. **Adicionar o Script ao Git e Subir para o GitHub:**

    *   Inicialize um repositório Git (se ainda não tiver um):
        ```bash
        git init
        ```
    *   Adicione o script ao repositório:
        ```bash
        git add provisiona_web.sh
        ```
    *   Faça um commit:
        ```bash
        git commit -m "Adicionado script de provisionamento do servidor web"
        ```
    *   Crie um repositório no GitHub.
    *   Adicione o remote ao seu repositório local:
        ```bash
        git remote add origin <URL_DO_SEU_REPOSITORIO_GITHUB>
        ```
    *   Suba o script para o GitHub:
        ```bash
        git push -u origin main
        ```

## Observações

*   Este script assume que você está executando-o em um ambiente Debian/Ubuntu. Se estiver usando outra distribuição Linux, ajuste os comandos de instalação de pacotes (ex: `yum install` para CentOS/RHEL).
*   Altere o link do `wget` e o diretório de destino da aplicação caso necessário.
*   Adapte o script conforme necessário para atender às suas necessidades específicas, como configurar virtual hosts, habilitar módulos Apache, etc.
*   Considere adicionar tratamento de erros e logs para facilitar a depuração e o monitoramento.
*   Ao subir o script para o GitHub, considere torná-lo parte de um repositório maior com outros scripts de IaC e documentação.

Este README fornece um guia detalhado para criar e executar o script de provisionamento.
