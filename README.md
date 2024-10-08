﻿# Guia de Configuração e Uso do Workflow para Deploy via SFTP

Este guia detalha como configurar e utilizar um workflow no GitHub Actions que faz deploy seguro do seu código para um servidor SFTP por meio de uma conexão VPN. O workflow é acionado automaticamente sempre que mudanças são enviadas para a branch ***main***. 

Siga os passos abaixo para configurar e usar este workflow de forma eficaz.

## Secrets Configuration
Você precisa adicionar os seguintes segredos ao seu repositório no GitHub:

***VPN_CONFIG:*** Arquivo de configuração OpenVPN (.ovpn) codificado em base64. </br>
***VPN_AUTH:*** Credenciais de autenticação da VPN (username e password) codificadas em base64. </br>
***SFTP_PRIVATE_KEY:*** Chave privada SSH para autenticação no SFTP, convertida para o formato .pem e codificada em base64. </br>

## Convertendo .ppk para .pem e codificando em Base64 </br>
If you have an SSH key in .ppk format and need to convert it to .pem, follow the steps below:

## 1. Download PuTTYgen
Se você ainda não tem o PuTTYgen, baixe-o do site oficial do PuTTY [aqui](https://www.putty.org/).

## 2. Convert .ppk to .pem
1. Abra o PuTTYgen.
2. Carregue o arquivo da sua chave privada .ppk.
3. Vá em Conversions > Export OpenSSH key e salve o arquivo com a extensão .pem.

## 3. Encode .pem em Base64
Se preferir usar o terminal para converter a chave e codificá-la em base64, siga os comandos abaixo:

```
# Converter .ppk para .pem
puttygen key.ppk -O private-openssh -o key.pem

# Definir permissões para arquivo .pem
chmod 600 key.pem

# Codificar .pem em base64
base64 key.pem > key_base64.txt
```

## 4. Configure SFTP_PRIVATE_KEY
1. Abra o arquivo key_base64.txt.
2. Copie o conteúdo.
3. Adicione-o como um segredo no GitHub com o nome SFTP_PRIVATE_KEY.

***OBS:*** Você precisa observar que a última linha da chave privada é uma linha em branco. Você precisa manter essa linha ao adicionar segredos do Repositório, caso contrário, pode levar a um *invalid format* erro.


## Passo 5: Testando o Workflow

1. Faça uma alteração na branch main: Realize uma pequena modificação no código e faça o commit e o push para a branch ***main***. Isso acionará automaticamente o workflow configurado no GitHub Actions.
2. Verifique a execução no GitHub Actions:
   - Acesse a aba Actions no seu repositório GitHub e confira se o workflow foi executado corretamente.
   - Certifique-se de que não há erros nos logs e que todas as etapas foram concluídas com sucesso.
3. Confirme o Deploy:
   - Verifique se os arquivos foram transferidos corretamente para o servidor SFTP.

Pronto! Se tudo estiver funcionando, seu workflow está configurado e testado com sucesso.
