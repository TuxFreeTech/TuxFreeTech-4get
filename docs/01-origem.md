
# 01. Origem do projeto

## De onde surgiu a ideia

Este projeto nasceu de uma necessidade simples: ter uma página de busca própria dentro do ambiente TuxFreeTech.

O 4get já estava funcionando no TuxServer e oferecia a base necessária para esse objetivo. Em vez de desenvolver um buscador do zero ou procurar outra aplicação apenas para criar uma identidade visual própria, decidimos trabalhar sobre aquilo que já funcionava.

A ideia era adaptar o 4get ao ambiente TuxFreeTech de maneira simples, mantendo sua função principal e evitando alterações desnecessárias na aplicação.

O objetivo não era transformar o 4get em outra aplicação.

Era fazer com que ele tivesse a nossa identidade.

---

## O que precisávamos

A solução precisava atender principalmente aos seguintes pontos:

- funcionar dentro do TuxServer;
- manter o funcionamento original do 4get;
- ter uma identidade visual própria;
- permitir manutenção simples;
- manter as personalizações separadas dos arquivos originais sempre que possível;
- evitar alterações profundas na estrutura da aplicação.

Também queríamos evitar:

- modificar desnecessariamente o funcionamento do buscador;
- criar uma solução mais complicada do que o problema;
- depender de alterações difíceis de reproduzir;
- transformar uma personalização visual em uma intervenção grande na aplicação.

A ideia desde o começo foi seguir um princípio simples:

> Se uma alteração visual pode ser feita de uma maneira menor e mais segura, não existe motivo para complicá-la.

---

## Primeiros passos

Começamos pelas alterações mais simples e de menor risco.

A primeira delas foi substituir o nome padrão apresentado pela aplicação por:

```text
TuxFreeTech
```

Essa alteração foi feita através da configuração do ambiente, sem modificar diretamente o arquivo interno de configuração do 4get.

Depois trabalhamos na identidade visual da página.

O favicon padrão foi substituído pelo favicon do TuxFreeTech, fazendo com que a identidade também aparecesse na aba do navegador.

Em seguida, substituímos o banner original por um banner personalizado.

Essas alterações foram feitas sem interferir no funcionamento da busca.

---

## A ideia do wallpaper

Depois que o nome, favicon e banner estavam funcionando, surgiu a ideia de utilizar uma imagem do TuxFreeTech como fundo da página.

A primeira impressão foi positiva, mas logo apareceu uma questão de design:

já havia bastante identidade visual concentrada no mesmo espaço.

Ter o Tux no favicon, no banner e ainda apresentar uma imagem grande do mascote como fundo poderia deixar a página exagerada.

Em vez de reforçar a identidade, poderíamos acabar fazendo justamente o contrário e tirar a atenção do buscador.

Foi nesse momento que a ideia mudou.

---

## Menos é mais

Decidimos que o wallpaper não deveria ser o protagonista.

A imagem continuaria presente, mas seria utilizada como uma espécie de marca d'água.

A intenção passou a ser:

> Você percebe o Tux quando presta atenção, mas ele não fica disputando espaço com o buscador.

Para conseguir esse efeito, a imagem foi escurecida através de uma camada sobreposta e utilizada como fundo da página.

O resultado manteve o centro da interface livre para o banner, o campo de busca e os links.

Essa decisão acabou definindo a direção visual da personalização.

---

## A primeira tentativa que não funcionou

Durante a implementação do wallpaper, inicialmente tentamos carregar um CSS personalizado através de uma alteração no arquivo `home.html` do 4get.

A ideia parecia simples: adicionar uma referência ao nosso CSS e deixar o restante da personalização separado.

Porém, durante o teste, descobrimos que o `home.html` não era apenas uma página HTML estática.

O 4get utiliza campos que são preenchidos pelo próprio sistema antes de entregar a página ao navegador.

Quando nossa cópia do arquivo foi colocada no lugar do original, esses campos deixaram de ser processados corretamente.

O resultado foi uma página mostrando literalmente elementos como:

```text
%search%
%searchbutton%
%server_name%
```

O banner também deixou de funcionar corretamente.

---

## O que fizemos quando deu errado

Em vez de insistir na primeira abordagem, fizemos o rollback.

A alteração do `home.html` foi retirada da configuração e o container do 4get foi recriado.

A página voltou imediatamente ao estado funcional anterior, preservando as personalizações que já estavam funcionando.

Esse episódio ajudou a definir uma regra importante para o projeto:

> Uma solução que funciona não precisa ser substituída por outra apenas porque existe uma maneira diferente de fazer a mesma coisa.

---

## A mudança de abordagem

Depois de entender o problema, abandonamos a alteração do `home.html`.

Percebemos que o 4get já carregava seu próprio arquivo de estilos. Portanto, não havia necessidade de alterar a estrutura da página apenas para adicionar nosso visual.

A nova abordagem foi mais simples:

1. preservar uma cópia do CSS original;
2. acrescentar apenas as regras necessárias;
3. disponibilizar o wallpaper separadamente;
4. substituir o CSS utilizado pelo container através da configuração do Docker.

Essa solução atingiu o mesmo objetivo sem interferir no sistema de templates da página.

---

## Onde chegamos

A personalização terminou com o 4get funcionando normalmente e incorporado à identidade TuxFreeTech.

O resultado final reúne:

- nome personalizado;
- favicon próprio;
- banner próprio;
- tema escuro;
- wallpaper discreto;
- configuração através do Docker Compose;
- arquivos personalizados mantidos fora da imagem original da aplicação.

O mais importante, porém, não foi simplesmente fazer a página ficar diferente.

Foi encontrar um equilíbrio entre personalização e simplicidade.

O TuxFreeTech está presente na página, mas o buscador continua sendo o elemento principal.

Essa acabou sendo a direção visual e técnica escolhida para o projeto:

> **A identidade deve estar presente sem precisar gritar.**