# 06. Manutenção

## Objetivo

A personalização foi feita de forma a manter os arquivos personalizados separados dos arquivos originais do 4get.

Isso facilita ajustes futuros e reduz a necessidade de alterar diretamente o conteúdo do container.

---

## Arquivos personalizados

Os arquivos utilizados pela personalização ficam dentro da pasta `custom/`.

Os principais arquivos são:

custom/
├── favicon.ico
├── banner/
│   └── 4get-default.png
├── wallpaper.jpg
└── style.css

Esses arquivos são montados no container através do Docker Compose.

---

## Alterando a aparência

As alterações visuais podem ser feitas nos arquivos da pasta `custom/`.

Por exemplo:

- `favicon.ico`: ícone da página;
- `4get-default.png`: banner;
- `wallpaper.jpg`: imagem de fundo;
- `style.css`: alterações visuais.

Depois de alterar o CSS ou o Docker Compose, é importante validar a configuração antes de recriar o container.

Comando:

cd /AppData/4get
docker compose config

Se a configuração estiver correta, o container pode ser atualizado com:

docker compose up -d

---

## Atualizações do 4get

Uma atualização do 4get pode substituir os arquivos internos da aplicação.

Por isso, a personalização não deve depender de alterações feitas diretamente dentro do container.

Os arquivos personalizados ficam no host e são montados novamente pelo Docker Compose.

Depois de atualizar o 4get, a página deve ser testada para verificar se:

- o buscador continua funcionando;
- o banner continua sendo apresentado;
- o favicon continua carregando;
- o wallpaper continua aparecendo;
- o nome TuxFreeTech continua correto;
- a aparência geral continua como esperado.

---

## Backup

Antes de realizar alterações importantes no Docker Compose, é recomendável manter uma cópia da configuração que está funcionando.

Neste projeto foi utilizado:

`docker-compose.yml.bak`

O objetivo do backup é permitir um retorno rápido para a configuração anterior caso uma alteração cause algum problema.

---

## Se algo der errado

A primeira regra é simples:

**não começar a modificar várias coisas ao mesmo tempo.**

Se uma alteração causar um problema:

1. identificar a última mudança realizada;
2. voltar ao último estado funcional;
3. testar novamente;
4. só então tentar uma nova abordagem.

Esse processo torna mais fácil descobrir a causa do problema e evita transformar uma alteração pequena em vários problemas ao mesmo tempo.

---

## Manutenção dos arquivos personalizados

Os arquivos dentro de `custom/` devem ser tratados como parte do projeto.

Se algum deles for substituído ou alterado, é importante verificar o resultado no navegador.

Especialmente no caso do `style.css`, uma alteração incorreta pode afetar a aparência da página inteira.

Por isso, alterações grandes devem ser feitas com cuidado e, sempre que possível, de forma incremental.

---

## O que não fazer

Não é recomendado:

- editar arquivos diretamente dentro do container sem necessidade;
- colocar o wallpaper dentro da pasta de banners;
- substituir arquivos internos do 4get apenas para realizar uma alteração visual simples;
- fazer várias alterações simultaneamente sem testar;
- remover o backup antes de confirmar que a nova configuração está funcionando.

---

## Princípio de manutenção

A manutenção deste projeto segue a mesma ideia utilizada durante sua implementação:

> **Alterar o mínimo necessário.**

Quanto menos partes do 4get forem modificadas, menor será a chance de uma atualização ou alteração futura quebrar a personalização.

A identidade visual deve continuar sendo uma camada sobre o 4get, e não uma reconstrução do próprio 4get.