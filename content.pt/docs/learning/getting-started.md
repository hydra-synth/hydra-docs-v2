---
title: Começando
weight: 1
---

# Começando

Este documento é uma introdução à criação de visuais ao vivo usando o Hydra. Abrange o básico da escrita de código no navegador para gerar e combinar fontes de vídeo em tempo real. Nenhuma experiência com programação ou vídeo é necessária!

Quem quiser apenas começar em 60 segundos, também pode visitar:
* [Getting started short version (inglês)](https://hackmd.io/@r08UjGF3QMCfvNmdjuY7iQ/rJCpsbNNc)

Este tutorial é pensado para ser usado dentro do [editor web hydra](https://hydra.ojack.xyz/). Também é interativo -- podemos alterar o código em cada bloco para ver como isso afeta os visuais.  

### Conhecendo o editor do navegador
Para começar, abra o [editor web hydra](https://hydra.ojack.xyz/) em uma janela separada. Feche a janela em destaque clicando no [x] no canto superior direito.

![](https://i.imgur.com/ZfgVjJZ.gif)

Você verá alguns visuais coloridos em segundo plano com textos no canto superior esquerdo da tela. Este é o código que gera os visuais ao fundo.

No canto superior direito, você encontrará uma barra de ferramentas com estes botões:
![](https://i.imgur.com/iCG8Lrq.png)
1. **Executar tudo** executa todo o código na página (tecla de atalho *ctrl+shift+enter).
2. **Fazer upload para a galeria** envia o sketch para a galeria da Hydra e cria um curto URL.
3. **Limpar editor** redefine o ambiente e limpa o texto do editor.
4. **Mostrar um sketch aleatório** carrega exemplos de esboços aleatórios. Sempre é uma boa maneira de aprender Hydra estudando o código de outra pessoa.
5. **Aplicar mudanças aleatórias** modifica valores automaticamente. Experimente com alguns dos exemplos de esboço.
6. **Mostrar janela de informação** mostra uma janela em sobreposição com texto de ajuda e links


## Primeira linha de código

Use o botão ***clear all*** <img src="https://i.imgur.com/zQLjhBs.png" alt="drawing" width="40" style="display:inline;vertical-align:middle;"/>
para apagar o sketch anterior.

Em seguida, digite ou cole o seguinte no editor:
```javascript
osc().out()
```
Aperte o botão ***run***  <img src="https://i.imgur.com/sm5d3VX.png" alt="drawing" width="40" style="display:inline;vertical-align:middle;"/> para executar este código e atualizar os visuais. Você verá algumas listras rolando na tela.

```hydra
osc().out()
```

Isso cria um oscilador visual. Tente modificar os parâmetros do oscilador colocando um número dentro dos parênteses de `osc()`, por exemplo ```osc(10).out()```.

Execute novamente o código pressionando o botão ***run*** novamente e vendo a atualização do visual. Tente adicionar outros valores para controlar os parâmetros `frequency` (frequência), `sync` (sincronização) e `color offset` (deslocamento de cor) do oscilador.

```hydra
osc(5, -0.126, 0.514).out()
```


*Dica: você também pode usar o atalho* **‘ctrl + shift + enter’** *para ter o mesmo efeito do botão executar.*


## Adicionando transformações
Podemos adicionar outra transformação ao oscilador acima, adicionando a função `rotate()` depois do oscilador:
```hydra
osc(5,-0.126,0.514).rotate().out()
```

Como você pode ver, temos primeiro uma fonte de entrada `osc()` ae as coisas que vêm depois (`rotate()` and `out()`) são conectadas com um ponto '.'
Nesse sentido, Hydra é inspirado por [síntese modular](https://en.wikipedia.org/wiki/Modular_synthesizer) (inglês).
Em vez de conectar cabos, você conecta diferentes tipos de funções javascript.  
![](https://i.imgur.com/RBRxeiL.jpg)
###### fonte [Sandin Image Processor](https://en.wikipedia.org/wiki/Sandin_Image_Processor)

Podemos continuar adicionando transformações a esta cadeia de funções. Por exemplo:  
```hydra
osc(5,-0.126,0.514).rotate(0, 0.2).kaleid().out()
```

Repetir:
```hydra
osc(5,-0.126,0.514).rotate(0, 0.2).kaleid().repeat().out()
```


Para obter mais fontes e transformações disponíveis, consulte a [referência interativa de funções (inglês)](https://hydra.ojack.xyz/api).
A lógica é começar com uma ***source*** (fonte) (como `osc()`, `shape()`, ou `noise()`), e depois adicionar transformações a ***geometry*** (geometria) e ***color*** (cor) (como `.rotate()`, `.kaleid()`, `.pixelate()`), e no final sempre conecte a cadeia de transformações à tela de saída `.out()`.


```hydra
noise(4).color(-2, 1).colorama().out()
```

```hydra
shape(3).repeat(3, 2).scrollX(0, 0.1).out()
```


## O que é um erro?
Às vezes, você tentará executar uma linha de código e nada acontecerá. Se você tiver um erro, notará um texto em vermelho na parte inferior esquerda da tela. Algo como ‘Unexpected token ‘.’ (em vermelho) aparecerá. Isso não afeta seu código, mas você não poderá continuar codificando até corrigir o erro. Geralmente é um erro de digitação ou algo relacionado à sintaxe.

## O que é um comentário?

```javascript
// Olá, sou uma linha de comentários. Eu sou um texto que não vai mudar o seu código. Você pode escrever anotações, seu nome ou até mesmo um poema aqui.
```

## Salve seu esboço na internet


Ao avaliar todo o código com o botão ***run*** ou com `shift + ctrl + enter`, o Hydra gera automaticamente uma URL que contém as últimas alterações do seu esboço. Podemos copiar e colar o url da barra de URL para salvá-lo ou compartilhá-lo com outras pessoas. É possível também usar as setas `voltar` e `avançar` do navegador para navegar para versões anteriores de seu esboço.


## Usando a webcam
Além de usar fontes de dentro do Hydra (como `osc()` e `shape()`), podemos usar o Hydra para processar fontes de vídeo externas, como uma webcam.
Para inicializar a webcam, execute o seguinte código:
```javascript
s0.initCam()
```

Isso ativa a fonte da webcam dentro de uma variável chamada `s0`, e você deve ver a luz da sua webcam acender. No entanto, você ainda não verá a imagem da webcam na tela. Para usar a câmera em um esboço Hydra, você precisa usá-la dentro da função `src()`.

```hydra
s0.initCam() // inicializa a webcam como fonte externa 's0'
src(s0).out() // usa a fonte externa 's0' dentro do Hydra
```

Semelhante à adição de transformações acima, podemos adicionar transformações de cor e geometria à saída da câmera, adicionando funções à cadeia:

```hydra
s0.initCam()
src(s0).color(-1, 1).out()
```

```hydra
s0.initCam()
src(s0).color(-1, 1).kaleid().out()
```

Caso tenha várias webcams, poderá acessá-las separadamente adicionando um número dentro de `initCam`, por exemplo `s0.initCam(1)` ou `s0.initCam(2)`.




## Saídas múltiplas

Por padrão, o Hydra contém quatro saídas virtuais separadas que podem renderizar visuais diferentes e ser misturadas entre si para criar visuais mais complexos. As variáveis `o0`, `o1`, `o2` e `o3` correspondem às diferentes saídas.

Para ver todas as quatro saídas de uma vez, use a função `render()`. Isso dividirá a tela em quatro, mostrando cada saída em uma seção diferente da tela.

![](https://i.imgur.com/m5Q0Na6.jpg)

Usar uma variável diferente dentro da função `.out()` renderiza a cadeia para uma saída diferente. Por exemplo, `.out(o1)` renderizará uma cadeia de funções para o buffer gráfico `o1`.


```hydra
gradient(1).out(o0) // renderiza um gradiente para saída o0
osc().out(o1) // renderiza voronoi para saída o1
voronoi().out(o2) // renderiza voronoi na saída o2
noise().out(o3) // renderiza ruído na saída o3

render()  // mostra todas as saídas
```

Por padrão, apenas a saída `o0` é renderizada na tela, enquanto o comando `render()` divide a tela em quatro. Mostre uma saída específica na tela adicionando-a dentro de `render()`, por exemplo, `render(o2)` para mostrar o buffer `o2`.


```hydra
gradient(1).out(o0) // renderiza um gradiente para saída o0
osc().out(o1) // renderiza voronoi para saída o1
voronoi().out(o2) // renderiza voronoi na saída o2
noise().out(o3) // renderiza ruído na saída o3

render(o2)  // mostra apenas a saída o2
```


*Dica: tente criar esboços diferentes e trocá-los em sua performance ao vivo ou até mesmo combiná-los.*


```hydra
gradient(1).out(o0)
osc().out(o1)
render(o0) //switch render output
// render(o1)
```

## Combinando várias fontes juntas
Podemos usar funções ***blend*** (mescla/mistura) para combinar várias fontes visuais. `.blend()` combina as cores de duas fontes para criar uma terceira fonte.

```hydra
s0.initCam()

src(s0).out(o0) // renderiza a webcam na saída o0

osc(10).out(o1) // renderiza um oscilador na saída o1

src(o0).blend(o1).out(o2) // começa com o0, mistura com o1 e envia para o2

render() // renderiza todas as quatro saídas de uma vez
```

Tente adicionar transformações às fontes acima (como `osc(10).rotate(0, 0.1).out(o1)`) para ver como isso afeta a imagem combinada. Podemos também especificar a quantidade de mistura adicionando um parâmetro separado a `.blend()`, por exemplo `.blend(o1, 0.9)`.

Há vários [modos de mescla (inglês)](https://en.wikipedia.org/wiki/Blend_modes) em Hydra, semelhantes aos modos de mesclagem que você pode encontrar em um programa gráfico como Photoshop ou GIMP. Veja a [referência de funções (inglês)](https://hydra.ojack.xyz/api/) para mais possibilidades.

```hydra

s0.initCam()

src(s0).out(o0) // renderiza a webcam na saída o0

osc(10).out(o1) // renderiza um oscilador na saída o1

src(o0).diff(o1).out(o2) // combina sinais diferentes por diferença de cor (as partes escuras ficam invertidas).

render() // renderiza todas as quatro saídas de uma vez
```


## Modulação
Enquanto funções ***blend*** combinam as cores de duas fontes visuais, as funções ***modulate*** (modulação) usam as cores de uma fonte para afetar a geometria da segunda fonte. Isso cria uma espécie de efeito de deformação ou distorção. Uma analogia no mundo real seria olhar através de uma janela de vidro texturizada. `modulate()` não altera cor ou luminosidade, mas distorce uma fonte visual usando outra fonte visual.

Usando as mesmas fontes acima, podemos usar um oscilador para modular ou distorcer a imagem da câmera:

```hydra
s0.initCam()

src(s0).out(o0) // renderiza a webcam na saída o0
osc(10).out(o1) // renderiza um oscilador na saída o1

src(o0).modulate(o1).out(o2) // usa a fonte o1 para distorcer a fonte o0, áreas mais claras são distorcidas mais

render() // renderiza todas as quatro saídas de uma vez
```

Você pode adicionar um segundo parâmetro à função `modulate()` para controlar a quantidade de distorção: `modulate(o1, 0.9)`. Neste caso, os canais vermelho e verde do oscilador estão sendo convertidos em deslocamento x e y da imagem da câmera.

Todas as transformações de geometria têm funções de modulação correspondentes que permitem usar uma fonte para distorcer outra fonte. Por exemplo, `modulateRotate()` é semelhante a `rotate()`, porém nos permite aplicar diferentes intensidades de rotação em diferentes partes da fonte visual. Veja a [referência de funções](https://hydra.ojack.xyz/api/) para mais exemplos.

```hydra
s0.initCam()

src(s0).out(o0) // renderiza a webcam na saída o0
osc(10).out(o1) // renderiza um oscilador na saída o1

src(o0).modulateRotate(o1, 2).out(o2)

render() // renderiza todas as quatro saídas de uma vez
```

## Mais combinações e modulações

Além de usar várias saídas para juntar imagens, podemos também pode combinar várias fontes dentro da mesma cadeia de funções, sem transformá-las em saídas separadas.

```hydra
osc(10, 0.1, 1.2).blend(noise(3)).out(o0)

render(o0) // renderiza a saída o0
```

Isto permite utilizar muitas fontes, modos de combinação e modulação, tudo a partir da mesma cadeia de funções.

```hydra
osc(10, 0.1, 1.2).blend(noise(3)).diff(shape(4, 0.6).rotate(0, 0.1)).out()
```

*Dica: use `ctrl + shift + f` no editor web para formatar automaticamente seu código.*

#### Modulando com a câmera
```hydra
s0.initCam() // carrega uma câmera

shape().modulate(src(s0)).out() // forma modulada por uma câmera
```
```hydra
s0.initCam() // carrega uma câmera

src(s0).modulate(shape()).out() //câmera modulada por uma forma
```






```hydra

noise().out(o1)
shape().out(o3)

src(o1).add(src(o3)).out(o2) // luz aditiva. A cor apenas fica mais brilhante

render()
```

```hydra
osc(10).out(o0)

shape().out(o1)

src(o0).diff(o1).out(o2) // combine diferentes sinais por diferença de cor (cor negativa/invertida/oposta).  

render()
```
```hydra
osc().mult(src(o1)).out() // multiplica as fontes juntas
shape(5).out(o1)
```


Cobrimos agora todos os tipos básicos de funções dentro de Hydra: ***source***, ***geometry***, ***color***, ***blending***, e ***modulation***! Descubra onde você pode chegar combinando elas.


#### Aproveite!

*Tradução por [A1219](https://github.com/a-1219) e [Vagné L.](https://github.com/muziekmutantti)*
