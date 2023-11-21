---
title: Tópicos Adicionais
---

# Tópicos Adicionais

## live coding: avaliar linhas ou blocos de código separado

Pressione `ctrl+enter` para executar uma linha de código.
Pressione `shift+ctrl+enter` para avaliar um bloco de código.
Dica: podemos alternar entre diferentes linhas de código para uma performance de live coding.

```javascript
osc().out() // executar esta primeira

noise().mult(osc(10,0.1,10)).out() // experimente este
```

## arrays

Arrays (listas) em Hydra são uma coleção sequenciada de valores. Podem ser usadas para alterar vários parâmetros no tempo.

```javascript
osc(10,0.1,[10,0,2,0.5]).out()

shape([3,4,200,2]).out()
```

## audio

Faça visuais reativos a áudio. O sinal de áudio funciona como um parâmetro de entrada e podemos multiplicar este valor a fim de alterar a quantidade de mudanças.

```javascript
osc(20,0.1, ()=>a.ff[0]*10).out()
```


## funções glsl personalizadas: https://hydra-book.glitch.me/#/glsl (inglês)


## do documento antigo:

## Passando funções como variáveis
Cada parâmetro pode ser definido como uma função ao invés de uma variável estática. Por exemplo,
```javascript
osc(function(){return 100 * Math.sin(time * 0.1)}).out()
```
modifica a frequência do oscilador em uma função do tempo (tempo é uma variável global que representa os segundos que passaram desde o carregamento da página). Isto pode ser escrito de forma mais concisa usando a sintaxe es6:
```javascript
osc(() => (100 * Math.sin(time * 0.1))).out()
```

## Captura da área de trabalho
Abra um diálogo para selecionar uma aba de tela a ser usada como textura de entrada:
```javascript
s0.initScreen()
src(s0).out()
```

## Conectando a streams remotos
Qualquer instância de Hydra pode usar outras instâncias/janelas contendo Hydra como fontes de entrada, desde que estejam conectadas à internet e não bloqueadas por um firewall. Hydra usa a webrtc (webstreaming em tempo real) em segundo plano para compartilhar fluxos de vídeo entre janelas abertas. O módulo incluso rtc-patch-bay gerencia conexões entre janelas conectadas e também pode ser usado como um módulo autônomo para converter qualquer website em uma fonte dentro de Hydra (veja fonte de câmera autônoma abaixo, por exemplo).

Para começar, abra Hydra simultaneamente em duas janelas separadas.
Em uma das janelas, defina um nome para a fonte de patch-bay dada:
```javascript
pb.setName("myGraphics")
```
O título da janela deve mudar para o nome inserido em `setName()`.

A partir da outra janela, inicie "myGraphics" como uma fonte de stream.
```javascript
s0.initStream("myGraphics")
```
renderize para a tela:
```javascript
s0.initStream("myGraphics")
src(s0).out()
```
As conexões às vezes levam alguns segundos para serem estabelecidas; abra o console do navegador para ver o progresso.
Para listar as fontes disponíveis, digite o seguinte no console:
```javascript
pb.list()
```

## Usando p5.js com Hydra

```javascript
// Inicializa uma nova instância p5. Só é necessário chamar uma vez
p5 = new P5() // {width: window.innerWidth, height:window.innerHeight, mode: 'P2D'}

// desenha um retângulo no ponto 300, 100
p5.rect(300, 100, 100, 100)

// Note que P5 roda em modo instância, portanto, todas as funções precisam começar com a variável onde P5 foi inicializado (neste caso p5)
// referência para P5: https://P5js.org/reference/ (inglês)
// explicação do modo de instância: https://github.com/processing/P5.js/wiki/Global-and-instance-mode (inglês)

// Durante o livecoding, a função "setup()" do P5.js basicamente não tem utilidade; qualquer coisa que for chamada em setup pode simplesmente ser chamada fora de qualquer função.

p5.clear()

for(var i = 0; i < 100; i++){
  p5.fill(i*10, i%30, 255)
  p5.rect(i*20, 200, 10,200)
}

// Para programar animações ao vivo, você pode redefinir a função de desenho do P5 da seguinte forma:
// (um retângulo que segue o mouse)
p5.draw = () => {
  p5.fill(p5.mouseX/5, p5.mouseY/5, 255, 100)
  p5.rect(p5.mouseX, p5.mouseY, 30, 150)
}

// Para usar P5 como entrada para Hydra, basta usar o canvas como fonte:
s0.init({src: p5.canvas})

// Depois, renderize o canvas
src(s0).repeat().out()
```

## Carregando scripts externos
A função `await loadScript()` permite carregar outras bibliotecas empacotadas Javascript dentro do editor da Hydra. Qualquer código Javascript pode ser executado no editor Hydra.

Aqui está um exemplo usando o Three.js no editor web:
```javascript
await loadScript("https://threejs.org/build/three.js")

scene = new THREE.Scene()
camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)

renderer = new THREE.WebGLRenderer()
renderer.setSize(width, height)
material = new THREE.MeshBasicMaterial({color: 0x00ff00})
geometry = new THREE.BoxGeometry()
cube = new THREE.Mesh(geometry, material);
scene.add(cube)
camera.position.z = 1.5

// 'update' é uma função reservada que será executada toda vez que o contexto de renderização principal de Hydra for atualizado
update = () => {
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render( scene, camera );
}

s0.init({ src: renderer.domElement })

src(s0).repeat().out()
```

E aqui está um exemplo carregando a biblioteca Tone.js:
```javascript
await loadScript("https://unpkg.com/tone")

synth = new Tone.Synth().toDestination();
synth.triggerAttackRelease("C4", "8n");
```



## Responsividade a áudio
A funcionalidade FFT está disponível por meio de um objeto de áudio acessado via "a". O editor usa https://github.com/meyda/meyda para análise de áudio. Para mostrar os compartimentos fft,
```
a.show()
```
Defina o número de compartimentos fft:
```
a.setBins(6)
```
Acesse o valor do compartimento mais à esquerda (frequência mais baixa):
```
a.fft[0]
```
Use o valor para controlar uma variável:
```
osc(10, 0, () => (a.fft[0]*4))
  .out()
```
É possível calibrar a responsividade alterando o valor mínimo e máximo detectado (representado por linhas de desfoque sobre o fft). Para definir o valor mínimo detectado:
```
a.setCutoff(4)
```

Definir a escala altera o intervalo detectado.
```
a.setScale(2)
```
`fft[<índice>]` retornará um valor entre 0 e 1, onde 0 representa o corte e 1 corresponde ao máximo.

Você pode definir a suavização entre as leituras de nível de áudio (valores entre 0 e 1). 0 corresponde a nenhuma suavização (mais saltos, tempo de reação mais rápido), enquanto 1 significa que o valor nunca mudará.
```
a.setSmooth(0.8)
```
Para ocultar a onda de áudio:
```
a.hide()
```
## MIDI (experimental)

Controladores MIDI podem trabalhar com Hydra via WebMIDI. Um exemplo pode ser visto em [/docs/midi.md](https://github.com/ojack/hydra/blob/master/docs/midi.md).

*Tradução por [A1219](https://github.com/a-1219) e [Vagné L.](https://github.com/muziekmutantti)*
