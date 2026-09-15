# Configuração

Esta pasta contém arquivos de configuração de exemplo para reproduzir a personalização do 4get.

## Docker Compose

O arquivo `docker-compose.example.yml` apresenta uma configuração de exemplo baseada na estrutura utilizada no projeto.

Ele demonstra:

- definição do container;
- porta de acesso;
- nome personalizado do servidor;
- montagem dos arquivos de personalização;
- uso de arquivos externos ao container.

Os valores apresentados são exemplos e devem ser adaptados ao ambiente onde o 4get será instalado.

## Arquivo de exemplo

O arquivo:

docker-compose.example.yml

pode ser utilizado como base para criar a configuração do próprio ambiente.

Antes de iniciar o container, verifique principalmente:

- caminho da pasta de personalização;
- porta utilizada;
- nome desejado para o servidor;
- arquivos que serão montados no container.

## Importante

Este arquivo não é uma cópia da configuração privada utilizada no servidor TuxFreeTech.

A intenção é fornecer uma estrutura genérica que permita entender e reproduzir a técnica sem depender da configuração específica do ambiente original.