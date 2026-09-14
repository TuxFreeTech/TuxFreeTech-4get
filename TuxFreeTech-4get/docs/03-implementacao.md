# 03. Implementação

## Visão geral

A implementação foi realizada sobre uma instalação funcional do 4get executada em Docker.

O objetivo foi personalizar a aplicação para o ambiente TuxFreeTech sem alterar sua função principal como buscador.

A estratégia utilizada foi manter os arquivos personalizados no servidor e fazer com que o container utilizasse esses arquivos através do Docker Compose.

A estrutura final ficou baseada em três partes:

```text
4get
  ↓
Docker Compose
  ↓
Arquivos personalizados
```

---

## Estrutura utilizada no servidor

O projeto foi organizado em:

```text
/AppData/4get/
│
├── docker-compose.yml
│
└── custom/
    ├── favicon.ico
    ├── wallpaper.jpg
    ├── style.css
    └── banner/
        └── 4get-default.png
```

Os caminhos acima representam a organização utilizada no ambiente original.

Eles devem ser adaptados caso o projeto seja reproduzido em outro servidor.

---

## Container

O container utilizado recebeu o nome:

```text
4get
```

A imagem utilizada foi:

```text
luuul/4get:latest
```

A aplicação foi disponibilizada através de uma porta definida no Docker Compose.

No ambiente original, a porta utilizada foi `9090`.

Para uma instalação diferente, a porta pode ser alterada conforme a necessidade.

---

## Configuração inicial do Docker Compose

A configuração utilizada como base continha:

```yaml
services:
  4get:
    image: luuul/4get:latest
    container_name: 4get
    restart: unless-stopped
    ports:
      - "9090:80"
```

A partir dessa configuração foram adicionadas as personalizações.

---

## Personalização do nome

O nome apresentado pela aplicação foi alterado através de uma variável de ambiente do Docker Compose:

```yaml
environment:
  - FOURGET_SERVER_NAME=MeuServidor
```

No ambiente TuxFreeTech, esse valor foi definido como:

```text
TuxFreeTech
```

Essa abordagem permitiu alterar o nome sem editar diretamente o arquivo interno de configuração do 4get.

---

## Backup da configuração

Antes das alterações mais importantes, foi criada uma cópia de segurança do arquivo Docker Compose:

```text
docker-compose.yml.bak
```

Essa cópia permitiu retornar rapidamente à configuração anterior caso alguma alteração causasse problema.

---

## Favicon personalizado

O favicon original utilizado pelo 4get foi substituído por um arquivo personalizado.

A configuração ficou:

```yaml
volumes:
  - ./custom/favicon.ico:/var/www/html/4get/favicon.ico:ro
```

O arquivo personalizado permaneceu no servidor, enquanto o container recebeu acesso somente para leitura.

---

## Banner personalizado

O banner original foi substituído por um banner preparado para o TuxFreeTech.

A configuração utilizada foi:

```yaml
volumes:
  - ./custom/banner/4get-default.png:/var/www/html/4get/banner/4get-default.png:ro
```

O arquivo foi mantido dentro da pasta `custom`.

Isso permite alterar o banner sem precisar modificar diretamente a imagem do container.

---

## Verificação do funcionamento do banner

Durante a personalização foi necessário verificar como o 4get escolhia o banner.

A aplicação utiliza arquivos presentes na pasta:

```text
banner/
```

Por isso, o wallpaper não foi colocado nessa pasta.

Se o wallpaper fosse colocado junto dos banners, poderia ser tratado como uma das imagens disponíveis para a página.

---

## Wallpaper

A imagem escolhida para o fundo foi mantida separadamente:

```text
custom/wallpaper.jpg
```

Ela foi disponibilizada dentro do container através de:

```yaml
volumes:
  - ./custom/wallpaper.jpg:/var/www/html/4get/wallpaper.jpg:ro
```

O arquivo ficou na raiz da aplicação dentro do container.

Essa escolha evitou misturar o wallpaper com os arquivos utilizados como banners.

---

## Personalização do CSS

Em vez de alterar a estrutura da página inicial, foi utilizada uma cópia do CSS original do 4get.

O arquivo original foi copiado do container para:

```text
custom/style.css
```

A cópia permitiu preservar o CSS original e fazer a alteração no servidor.

---

## Regra adicionada ao CSS

Ao final do CSS original foi acrescentada uma regra específica para o wallpaper:

```css
/* TuxFreeTech - wallpaper discreto */
body {
        background-color: #1d2021 !important;
        background-image:
                linear-gradient(rgba(29,32,33,.93), rgba(29,32,33,.93)),
                url('/wallpaper.jpg') !important;
        background-size: cover;
        background-position: center;
        background-attachment: fixed;
        background-repeat: no-repeat;
}
```

A imagem recebe uma camada escura antes de ser apresentada.

O objetivo não era destacar o mascote, mas criar uma presença visual discreta no fundo.

---

## Configuração final

A configuração final utilizada ficou:

```yaml
services:
  4get:
    image: luuul/4get:latest
    container_name: 4get
    restart: unless-stopped

    ports:
      - "9090:80"

    environment:
      - FOURGET_SERVER_NAME=MeuServidor

    volumes:
      - ./custom/favicon.ico:/var/www/html/4get/favicon.ico:ro
      - ./custom/banner/4get-default.png:/var/www/html/4get/banner/4get-default.png:ro
      - ./custom/style.css:/var/www/html/4get/static/style.css:ro
      - ./custom/wallpaper.jpg:/var/www/html/4get/wallpaper.jpg:ro
```

O valor `MeuServidor` é apenas um exemplo.

No ambiente original, o nome utilizado foi `TuxFreeTech`.

---

## Validação da configuração

Antes de recriar o container, a configuração foi validada com:

```bash
cd /caminho/do/4get && docker compose config
```

O comando apresentou a configuração consolidada sem erros.

Essa verificação foi feita antes de aplicar a alteração ao container.

---

## Aplicação das alterações

Depois da validação, o container foi iniciado ou atualizado utilizando:

```bash
cd /caminho/do/4get && docker compose up -d
```

Esse comando aplicou a nova configuração sem precisar remover manualmente o projeto.

---

## Verificação do container

Depois da aplicação das alterações, o estado do container foi verificado com:

```bash
docker ps --filter name=4get
```

O resultado esperado é que o container apareça com estado semelhante a:

```text
Up
```

---

## Teste visual

Depois que o container voltou a funcionar, a página foi aberta pelo navegador.

Foi feito um recarregamento completo da página para evitar que arquivos antigos permanecessem em cache.

O teste confirmou:

- banner TuxFreeTech funcionando;
- campo de busca funcionando;
- links funcionando;
- tema escuro preservado;
- favicon personalizado funcionando;
- wallpaper aparecendo como fundo;
- identidade visual sem excesso de elementos.

---

## A implementação que não foi mantida

Durante o processo também foi feita uma tentativa de personalização através do arquivo:

```text
template/home.html
```

Essa abordagem não fazia parte da implementação final.

Ela foi testada para carregar um CSS personalizado, mas acabou interferindo no funcionamento da página.

Depois do problema, o arquivo personalizado foi removido da configuração e o container foi recriado.

A implementação seguiu então por outro caminho, utilizando o CSS que o próprio 4get já carregava.

---

## Resultado da implementação

A solução final utiliza quatro elementos principais:

```text
Nome
  ↓
Favicon
  ↓
Banner
  ↓
CSS + Wallpaper
```

Todos eles são aplicados sem modificar o funcionamento principal do buscador.

A configuração fica concentrada no Docker Compose e os arquivos personalizados permanecem separados da aplicação original.

---

## Reproduzindo a ideia

Para reproduzir a solução em outro ambiente, é necessário:

1. possuir uma instalação funcional do 4get;
2. criar uma pasta para os arquivos personalizados;
3. preparar o favicon;
4. preparar o banner;
5. preparar o wallpaper;
6. criar uma cópia do CSS original;
7. acrescentar a regra visual ao CSS;
8. configurar os arquivos no Docker Compose;
9. validar a configuração;
10. atualizar o container;
11. testar a página.

Os caminhos, portas e nomes devem ser adaptados ao ambiente onde a solução será instalada.

---

## Princípio utilizado

A implementação seguiu uma regra simples:

> Alterar o mínimo necessário para obter o resultado desejado.

Sempre que foi possível manter o funcionamento original do 4get, essa opção foi priorizada.