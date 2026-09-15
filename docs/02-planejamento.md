# 02. Planejamento

## Objetivo

O objetivo deste projeto era personalizar a página do 4get para o ambiente TuxFreeTech sem alterar o funcionamento do buscador.

A prioridade não era criar uma nova interface.

Era manter a aplicação funcionando como já funcionava e modificar apenas aquilo que fosse necessário para criar uma identidade visual própria.

---

## O que queríamos preservar

Antes de começar as alterações, definimos alguns pontos que deveriam permanecer intactos:

- funcionamento da busca;
- configurações dos mecanismos de pesquisa;
- tema escuro;
- estrutura principal da aplicação;
- funcionamento do container Docker;
- possibilidade de manter e atualizar o 4get no futuro.

Também decidimos evitar alterações profundas nos arquivos internos da aplicação sempre que houvesse uma alternativa mais simples.

---

## O que poderia ser alterado

As alterações planejadas eram principalmente visuais.

Os elementos considerados foram:

- nome apresentado pela aplicação;
- favicon;
- banner;
- identidade visual;
- wallpaper.

A ideia era fazer essas alterações utilizando a própria estrutura oferecida pelo 4get e pelo Docker, evitando modificar o funcionamento interno do buscador.

---

## Primeira estratégia

Começamos pelas alterações mais simples.

O nome do servidor foi personalizado através da configuração do Docker Compose.

Depois adicionamos o favicon e o banner personalizados através de arquivos mantidos fora do container.

Essa abordagem funcionou bem porque permitiu alterar a aparência sem modificar diretamente o funcionamento da aplicação.

---

## Planejamento do wallpaper

Depois das primeiras personalizações surgiu a ideia de utilizar uma imagem do TuxFreeTech como fundo da página.

A princípio, colocar a imagem diretamente como fundo parecia suficiente.

Porém, percebemos que o resultado poderia ficar visualmente exagerado.

A página já possuía o mascote no banner e no favicon.

Adicionar uma imagem grande e evidente ao fundo faria com que a identidade visual ocupasse espaço demais.

Por isso, o planejamento foi alterado.

O wallpaper deveria funcionar como um elemento secundário.

---

## A solução visual escolhida

A ideia final foi utilizar a imagem como uma espécie de marca d'água.

Para isso, o fundo recebeu uma camada escura sobre a imagem.

A intenção era reduzir bastante a presença do Tux e manter o foco nos elementos principais da página.

A prioridade visual ficou assim:

```text
Banner
   ↓
Campo de busca
   ↓
Links e informações
   ↓
Identidade visual no fundo
```

O wallpaper deveria ser percebido sem competir com os elementos principais.

---

## Primeira abordagem técnica

Para carregar o visual personalizado, inicialmente consideramos alterar o arquivo `home.html` do 4get.

A ideia era adicionar uma referência para um CSS próprio.

Isso permitiria manter a personalização separada do CSS original.

A abordagem parecia simples e, em teoria, permitiria controlar o visual da página sem modificar o restante da aplicação.

---

## O que descobrimos durante o teste

Durante a implementação percebemos que o `home.html` utiliza campos que são preenchidos pelo próprio 4get antes de a página ser apresentada ao navegador.

Entre esses campos estão:

```text
%server_name%
%banner%
%search%
%searchbutton%
```

Portanto, o arquivo não deveria ser tratado como uma página HTML comum.

Ao substituir o arquivo original por uma cópia personalizada, esses campos deixaram de ser processados corretamente.

A página passou a exibir os códigos literalmente e o banner deixou de funcionar.

---

## Mudança de estratégia

Depois desse teste, decidimos abandonar a alteração do `home.html`.

Em vez de tentar criar uma nova maneira de carregar nosso CSS, utilizamos o arquivo de estilos que o próprio 4get já carregava.

A estratégia passou a ser:

1. preservar uma cópia do CSS original;
2. acrescentar somente as regras necessárias;
3. manter o wallpaper separado;
4. disponibilizar o wallpaper para a aplicação;
5. substituir o CSS utilizado pelo container através do Docker Compose.

Essa abordagem exigia menos alterações na estrutura da aplicação.

---

## Por que escolhemos essa solução

A solução final foi escolhida principalmente por simplicidade e segurança.

Ela permitia:

- preservar o funcionamento da página;
- evitar alterações no sistema de templates;
- manter os arquivos personalizados fora da imagem do 4get;
- controlar a aparência através de um único arquivo;
- facilitar futuras alterações;
- permitir um rollback simples caso alguma mudança apresentasse problema.

A decisão seguiu um princípio que acabou se tornando importante durante o projeto:

> Não devemos modificar uma parte importante da aplicação quando podemos alcançar o mesmo resultado modificando uma parte menor e mais isolada.

---

## Resultado esperado

O planejamento final tinha um objetivo bastante simples:

manter o 4get funcionando como buscador e fazer com que ele parecesse pertencer ao TuxFreeTech.

O resultado esperado era uma interface:

- simples;
- escura;
- limpa;
- reconhecível;
- fácil de manter;
- e sem excesso de elementos visuais.

O wallpaper deveria complementar a página, não dominar a página.

---

## Critério de sucesso

Consideramos a personalização bem-sucedida se:

- a busca continuasse funcionando;
- o banner continuasse sendo exibido;
- o favicon fosse carregado corretamente;
- o nome TuxFreeTech fosse apresentado;
- o wallpaper fosse exibido de forma discreta;
- o container continuasse funcionando normalmente;
- e todas as alterações pudessem ser revertidas sem reconstruir o projeto do zero.

---

## Princípio adotado

O planejamento deste projeto acabou seguindo uma regra simples:

> **Se uma solução mais simples consegue o mesmo resultado, ficamos com a solução mais simples.**