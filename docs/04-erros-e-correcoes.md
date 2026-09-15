# 04. Erros, tentativas e correções

## Por que documentar os erros?

Este projeto não funcionou perfeitamente na primeira tentativa.

Isso não foi tratado como um problema a ser escondido da documentação.

Registrar as tentativas que não funcionaram ajuda a entender como chegamos à solução final e também evita que outra pessoa percorra o mesmo caminho sem saber o que aconteceu.

Além disso, os erros ajudaram a mostrar quais partes do 4get poderiam ser personalizadas com segurança e quais deveriam permanecer intactas.

---

## Problema 1: alteração do `home.html`

### O que tentamos

Durante a personalização do wallpaper, criamos uma cópia do arquivo:

```text
template/home.html
```

A intenção era adicionar uma referência para um CSS personalizado e deixar a personalização visual separada do CSS original.

A ideia parecia simples porque permitiria controlar o visual da página sem alterar diretamente o restante da aplicação.

---

### O que aconteceu

Depois que o arquivo personalizado foi colocado no lugar do original, a página deixou de funcionar corretamente.

Em vez de apresentar os elementos normalmente, o navegador passou a mostrar literalmente campos que deveriam ser preenchidos pelo 4get.

Entre eles estavam:

```text
%search%
%searchbutton%
%server_name%
```

O banner também deixou de ser apresentado corretamente.

---

### O que descobrimos

O arquivo `home.html` não funciona apenas como uma página HTML estática.

O 4get utiliza campos dentro desse arquivo que são preenchidos pelo próprio sistema antes que a página seja entregue ao navegador.

Ao substituir o arquivo original por uma cópia personalizada, interferimos nesse processo.

O problema, portanto, não estava no CSS que queríamos adicionar.

O problema estava na forma escolhida para inserir esse CSS.

---

### Como corrigimos

Em vez de tentar consertar a página modificada, decidimos voltar ao estado anterior.

A alteração do `home.html` foi retirada da configuração do Docker Compose.

Depois disso, o container foi recriado.

A página voltou a funcionar normalmente.

O banner, a busca, os links, o favicon e o nome TuxFreeTech voltaram a ser apresentados corretamente.

---

### Resultado

O rollback foi concluído com sucesso.

A tentativa foi abandonada e o `home.html` deixou de fazer parte da implementação.

---

# Problema 2: excesso de identidade visual

## O que percebemos

Depois que o nome, favicon e banner estavam personalizados, surgiu a ideia de colocar uma imagem do Tux como wallpaper.

A primeira ideia era utilizar a imagem de maneira bastante visível.

Durante a análise visual, porém, percebemos que isso criaria um excesso de identidade.

A página já apresentava o Tux em outros elementos.

Um wallpaper muito evidente faria com que o mascote competisse com o próprio buscador.

---

## A correção

Em vez de abandonar o wallpaper, mudamos a forma como ele seria apresentado.

A imagem passou a ser utilizada como um elemento secundário.

Foi aplicada uma camada escura sobre o wallpaper para reduzir sua presença visual.

A intenção passou a ser criar uma espécie de marca d'água.

O usuário deveria perceber a identidade visual sem perder o foco no buscador.

---

## Resultado

Essa mudança produziu um resultado mais equilibrado.

O Tux continua presente no fundo, mas não domina a página.

A identidade visual passou a complementar a interface em vez de competir com ela.

---

# Problema 3: localização do wallpaper

## O que consideramos

Durante a implementação também foi necessário decidir onde disponibilizar o arquivo do wallpaper dentro do container.

A pasta utilizada pelo 4get para os banners não era uma boa opção.

O 4get procura imagens nessa pasta para utilizá-las como banners.

---

## O risco

Se o wallpaper fosse colocado junto dos banners, ele poderia ser interpretado como uma imagem disponível para utilização como banner.

Isso faria com que uma imagem criada para o fundo pudesse aparecer em um local onde não deveria.

---

## A solução

O wallpaper foi disponibilizado separadamente na raiz da aplicação:

```text
/var/www/html/4get/wallpaper.jpg
```

No Docker Compose, isso foi feito através de um arquivo montado somente para leitura:

```yaml
- ./custom/wallpaper.jpg:/var/www/html/4get/wallpaper.jpg:ro
```

O CSS passou então a utilizar:

```css
url('/wallpaper.jpg')
```

---

## Resultado

O wallpaper ficou disponível para a página sem fazer parte da coleção de banners.

---

# O que foi descartado

## `home.html` personalizado

A alteração do `home.html` foi descartada.

Motivo:

o arquivo participa do sistema utilizado pelo 4get para preencher os elementos da página.

Modificar esse arquivo para uma necessidade puramente visual introduziu um risco desnecessário.

---

## Wallpaper muito evidente

Também foi descartada a ideia de utilizar o wallpaper com grande destaque.

Motivo:

a identidade visual já estava presente em outros elementos da página.

A solução mais discreta apresentou um resultado melhor.

---

# O que aprendemos

## 1. Nem toda alteração precisa acontecer no arquivo mais óbvio

Queríamos adicionar um estilo à página e inicialmente pensamos em modificar o HTML.

Depois descobrimos que o próprio 4get já carregava um arquivo de estilos que poderia ser utilizado.

A segunda solução exigia menos alterações.

---

## 2. Testar uma ideia também serve para descobrir que ela não é adequada

A tentativa com o `home.html` não produziu o resultado esperado.

Mesmo assim, ela foi útil porque mostrou como aquela parte do 4get funciona.

Depois dessa descoberta, conseguimos escolher uma solução mais simples.

---

## 3. Fazer rollback também faz parte da implementação

Quando uma alteração interfere no funcionamento da aplicação, insistir nela nem sempre é a melhor escolha.

Neste projeto, o rollback permitiu recuperar rapidamente o estado funcional e continuar a partir de uma base segura.

---

## 4. Personalização visual também precisa de equilíbrio

Não basta conseguir colocar um elemento na tela.

Ele precisa fazer sentido dentro da interface.

No caso do wallpaper, reduzir a intensidade da imagem produziu um resultado melhor do que simplesmente aumentar sua presença.

---

# Principal aprendizado

A principal lição deste projeto foi:

> Uma solução tecnicamente possível não é necessariamente a melhor solução.

Quando uma abordagem começa a interferir em partes importantes da aplicação, vale a pena parar, voltar ao último estado funcional e procurar uma alternativa menor.

Neste projeto, a solução final surgiu justamente dessa mudança de postura.

Em vez de modificar mais partes do 4get, reduzimos a intervenção.

O resultado foi uma personalização mais simples, mais segura e mais fácil de manter.