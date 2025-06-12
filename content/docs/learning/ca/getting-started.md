---
title: "començant"
date: 2023-04-04T15:10:36+02:00
author: "Flor and Olivia"
weight: 3
---

# Començant amb _Hydra_

Aquest document és una introducció a la creació de visuals en directe amb Hydra. Cobreix els conceptes bàsics per escriure codi al navegador per generar i mesclar fonts de vídeo en directe. No cal experiència prèvia en codi ni en vídeo!

<!-- Si només vols començar en 60 segons també pots mirar: -->
<!-- * [Versió ràpida per començar](https://hackmd.io/@r08UjGF3QMCfvNmdjuY7iQ/rJCpsbNNc) -->

Aquest tutorial està pensat per ser utilitzat al costat de l’[editor web de Hydra](https://hydra.ojack.xyz/). També és interactiu: pots modificar directament el codi de cada bloc per veure com afecta les visuals.

---

## Coneix l’editor del navegador

Per començar, obre l’[editor web de Hydra](https://hydra.ojack.xyz/) en una finestra separada. Tanca la finestra superior clicant la [x] a la cantonada superior dreta.

![](https://i.imgur.com/ZfgVjJZ.gif)
{.center}

Veureu visuals acolorides al fons i text al damunt a l’esquerra. Aquest text és el codi que genera les visuals del fons.

A la cantonada superior dreta trobaràs una barra d’eines amb aquests botons:
![](https://i.imgur.com/iCG8Lrq.png)
{.center}

1. **run all code** Executa tot el codi a la pàgina (com prémer *ctrl+shift+enter*).
2. **upload to gallery** Pujar l’esbós a la galeria i crear un enllaç curt.
3. **clear all** Reinicia l’entorn i esborra el text de l’editor.
4. **show random sketch** Carrega un esbós aleatori. Bona manera d’aprendre estudiant codi d’altres persones.
5. **make random change** Els **daus** modifiquen automàticament un valor. Prova-ho amb un esbós carregat.
6. **show info window** Mostra una finestra amb ajuda i enllaços útils.

---

## Primera línia de codi

Fes servir el ***botó de clear all*** <img src="https://i.imgur.com/zQLjhBs.png" alt="drawing" width="40" style="display:inline;vertical-align:middle;"/>
per esborrar l’esbós anterior.

Ara escriu o enganxa el següent:

```javascript
osc().out()
```
Prem el botó de run <img src="https://i.imgur.com/sm5d3VX.png" alt="drawing" width="40" style="display:inline;vertical-align:middle;"/> per executar el codi i actualitzar les visuals. Hauries de veure unes ratlles que es desplacen.

```hydra
osc().out()
```

Això crea un oscil·lador visual. Prova de modificar els paràmetres afegint un número dins de osc(), per exemple: ```osc(10).out()```.

Torna a prémer el botó de run per veure els canvis. Afegeix més valors per controlar la `frequency`, `sync`, i `color offset`.

```hydra
osc(5, -0.126, 0.514).out()
```

*Truc: també pots prémer **‘ctrl + shift + enter’** per executar el codi.*

---

## Transformacions
Podem afegir una transformació a l’oscil·lador amb una funció com `rotate()`:
```hydra
osc(5,-0.126,0.514).rotate().out()
```

### Modularitat

Com veus, primer generem una font amb `osc()` i després hi afegim funcions (`rotate()` i `out()`) amb un punt ‘.’
Hydra s’inspira en la [modular synthesis](https://en.wikipedia.org/wiki/Modular_synthesizer): en lloc de cables, 
connectem funcions de JavaScript amb punts i parèntesis.

![](https://i.imgur.com/RBRxeiL.jpg)
{.center}

###### Source: [Sandin Image Processor](https://en.wikipedia.org/wiki/Sandin_Image_Processor)

Podem seguir afegint transformacions, per exemple:  
```hydra
osc(5,-0.126,0.514).rotate(0, 0.2).kaleid().out()
```

O repetir:
```hydra
osc(5,-0.126,0.514).rotate(0, 0.2).kaleid().repeat().out()
```


Troba més fonts i transformacions a [interactive function reference](https://hydra.ojack.xyz/api).
La lògica de Hydra consisteix a començar amb una ***source*** (com `osc()`, `shape()`, o `noise()`), afegir-hi transformacions de ***geometry*** i ***color*** (com `.rotate()`, `.kaleid()`, `.pixelate()` ), i finalment connectar tota la cadena de transformacions a la pantalla amb `.out()` .


```hydra
noise(4).color(-2, 1).colorama().out()
```

```hydra
shape(3).repeat(3, 2).scrollX(0, 0.1).out()
```

---

## Conceptes de codi

### Què és un error?
SDe vegades, intentaràs executar una línia de codi i no passarà res. Si hi ha un error, apareixerà un text en vermell a la part inferior esquerra de la pantalla. Per exemple, pot sortir un missatge com ara ‘Unexpected token ‘.’’ (en vermell).

Això no malmet el teu codi, però no podràs continuar codificant fins que l’error estigui corregit. Normalment, es tracta d’un error de tecleig o de sintaxi.

### Què és un comentari??

```javascript
// Hola! Soc una línia de comentari. Soc un text que no afecta el codi. Pots escriure anotacions, el teu nom, un manifest o fins i tot un poema aquí.
```

---

## Desa els teus esbossos

### Com a enllaç

Quan executes tot el codi amb el ***run button*** o amb `shift + ctrl + enter`, Hydra genera automàticament una URL que conté els darrers canvis del teu esbós. Pots copiar i enganxar aquesta URL des de la barra d’adreces del navegador per desar-la o compartir-la amb altres persones. També pots utilitzar les fletxes de navegació del navegador `endavant` i `enrere` per recuperar versions anteriors del teu esbós.
![](https://i.imgur.com/lV0rmoh.png)

### A internet

Pots fer servir el botó de pujar a l’editor per penjar el teu esbós a la base de dades de Hydra. Això és especialment útil per esbossos amb codi llarg. Els esbossos i les seves captures de pantalla es comparteixen públicament a Mastodon.
(NOTA: el compte de Mastodon no funciona actualment, així que els esbossos no es publiquen).

---

## Fer servir la càmera web
A més de fer servir fonts internes de Hydra (com ara `osc()` i `shape()`), també pots processar fonts de vídeo externes com una càmera web. Per inicialitzar la càmera web, escriu aquest codi:
```javascript
s0.initCam()
```

Això activa la càmera web i assigna la imatge a una variable anomenada `s0`, Veuràs que s’encén el llum de la teva càmera. 
Tot i això, encara no apareixerà la imatge a la pantalla. Per veure-la dins d’un esbós de Hydra, cal fer-la servir amb la funció `src()`.

```hydra
s0.initCam() // inicialitza la càmera com a font externa 's0'
src(s0).out() // utilitza la font externa 's0' dins Hydra
```

Igual que abans, pots afegir transformacions de color i geometria a la imatge de la càmera, concatenant funcions:

```hydra
s0.initCam()
src(s0).color(-1, 1).out()
```

```hydra
s0.initCam()
src(s0).color(-1, 1).kaleid().out()
```

Si tens més d’una càmera web, pots accedir-hi canviant el número dins `initCam`, per exemple: `s0.initCam(1)` o `s0.initCam(2)`.

---

## Sortides múltiples

Per defecte, Hydra conté quatre sortides virtuals independents que poden mostrar visuals diferents i que es poden combinar entre elles per crear composicions més complexes. Les variables `o0`, `o1`, `o2`, i `o3` corresponen a aquestes diferents sortides.

Per veure les quatre sortides alhora, pots utilitzar la funció `render()`. Aquesta funció divideix la pantalla en quatre parts, mostrant cada sortida en un quadrant diferent.

![](https://i.imgur.com/m5Q0Na6.jpg)
{.center}

Fer servir una variable diferent dins la funció `.out()` envia la cadena de transformacions a una sortida concreta. Per exemple `.out(o1)` mostrarà els gràfics al buffer `o1`.


```hydra
gradient(1).out(o0) // mostra un degradat a la sortida o0
osc().out(o1) // mostra un oscil·lador a la sortida o1
voronoi().out(o2) // mostra un voronoi a la sortida o2
noise().out(o3)  // mostra un soroll a la sortida o3

render()  // mostra totes les sortides
```

Per defecte, només la sortida `o0` es mostra a la pantalla. La funció `render()` divideix la pantalla i mostra totes les sortides alhora.
També pots mostrar només una sortida concreta afegint-la dins la funció `render()`, com ara `render(o2)` per veure només el buffer `o2`.


```hydra
gradient(1).out(o0) // mostra un degradat a la sortida o0
osc().out(o1) // mostra un oscil·lador a la sortida o1
voronoi().out(o2) // mostra un voronoi a la sortida o2
noise().out(o3)  // mostra un soroll a la sortida o3

render(o2)  // mostra només la sortida o2
```


*Truc: prova de crear diferents esbossos i canviar entre ells durant una actuació en directe, o fins i tot combinar-los.*


```hydra
gradient(1).out(o0)
osc().out(o1)
render(o0) // canvia la sortida mostrada
// render(o1)
```

---

## Mescla textures 
Pots fer servir funcions de ***blend*** per combinar diverses fonts visuals. `.blend()` combina els colors de dues fonts per crear-ne una de nova.

```hydra
s0.initCam()

src(s0).out(o0) // mostra la webcam a la sortida o0

osc(10).out(o1) // mostra un oscil·lador a la sortida o1

src(o0).blend(o1).out(o2) // comença amb o0, la barreja amb o1, i envia-ho a o2

render() // mostra totes les sortides
```

Prova d’afegir transformacions a les fonts (com ara `osc(10).rotate(0, 0.1).out(o1)`) per veure com afecta la imatge final. També pots ajustar la intensitat de la mescla afegint un segon paràmetre a  `.blend()`, per exemple `.blend(o1, 0.9)`.

Hi ha diversos [blend modes](https://en.wikipedia.org/wiki/Blend_modes) a Hydra, similars als que es poden trobar en programes com Photoshop o GIMP. Consulta [the function reference](https://hydra.ojack.xyz/api/) per descobrir més possibilitats.

```hydra
s0.initCam()

src(s0).out(o0) // mostra la webcam a o0

osc(10).out(o1) // oscil·lador a o1

src(o0).diff(o1).out(o2) // combina per diferència de color (les parts fosques es negativen)

render() // mostra totes les sortides
```

---

## Modulació
Mentre que les funcions de ***blend*** mescla combinen colors de dues fonts, les funcions de ***modulate*** utilitzen una font per afectar la ***geometry*** d’una altra. Això crea un efecte de distorsió o deformació. Seria com mirar a través d’un vidre texturat.
`modulate()` no canvia el color o la lluminositat, sinó que distorsiona una font visual utilitzant una altra.

Amb les mateixes fonts anteriors, podem utilitzar un oscil·lador per modular o deformar la imatge de la càmera:

```hydra
s0.initCam()

src(s0).out(o0) // mostra la webcam a o0
osc(10).out(o1) // oscil·lador a o1

src(o0).modulate(o1).out(o2) // utilitza o1 per deformar o0

render() // mostra totes les sortides
```

Pots controlar la intensitat de la distorsió afegint un segon paràmetre:`modulate(o1, 0.9)`. En aquest cas, els canals vermell i verd de l’oscil·lador afecten el desplaçament en X i Y de la imatge de la càmera.

Totes les transformacions de ***geometry*** tenen una versió ***modulate*** Per exemple, `.modulateRotate()` es com `.rotate()`, però permet diferents rotacions segons les parts de la imatge. Consulta [the function reference](https://hydra.ojack.xyz/api/) per més exemples.

```hydra
s0.initCam()

src(s0).out(o0)
osc(10).out(o1)

src(o0).modulateRotate(o1, 2).out(o2)

render()
```

## Més combinació i modulació

A més d'utilitzar sortides múltiples per combinar visuals, també pots fer-ho dins d'una sola cadena de funcions, sense enviar-les a sortides separades.

```hydra
osc(10, 0.1, 1.2).blend(noise(3)).out(o0)

render(o0) // mostra la sortida o0
```

Això et permet fer servir moltes fonts, modes de mescla i modulacions dins d’una sola línia de codi.

```hydra
osc(10, 0.1, 1.2).blend(noise(3)).diff(shape(4, 0.6).rotate(0, 0.1)).out()
```

*Truc: usa `ctrl + shift + f` a l'editor web per autoformatar el codi.*

#### Modula amb la càmera
```hydra
s0.initCam() // activa la càmera

shape().modulate(src(s0)).out() // forma modulada per la càmera
```
```hydra
s0.initCam() // activa la càmera

src(s0).modulate(shape()).out() // la càmera modulada per una forma
```






```hydra

noise().out(o1)
shape().out(o3)

src(o1).add(src(o3)).out(o2) // suma de llum (les imatges s’il·luminen)

render()
```

```hydra
osc(10).out(o0)
shape().out(o1)

src(o0).diff(o1).out(o2) // diferència de color (efecte negatiu/invertit)

render()
```
```hydra
osc().mult(src(o1)).out() // multiplica les fonts
shape(5).out(o1)

```


Ara ja hem cobert tots els tipus bàsics de funcions a Hydra: ***sources***, ***geometry***, ***color***, ***blending***, i ***modulation***! Prova a combinar-los i crea els teus propis visuals..


#### Gaudeix com mai!

---
By Flor de Fuego, Olivia Jack
Traducció al català per [Arnau Casanoves](https://www.arnaucasanoves.com)
