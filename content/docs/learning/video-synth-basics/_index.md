---
weight: 3
bookFlatSection: false
title: "Fonaments del sintetitzador de vídeo"
bookCollapseSection: true
draft: false
---

# Fonaments del sintetitzador de vídeo

## Modularitat

Hydra s'inspira en la [síntesi modular](https://ca.wikipedia.org/wiki/Sintetitzador_modular).  
En lloc de connectar mòduls amb cables, connectes diferents tipus de funcions JavaScript utilitzant punts (`.`) i crides de funcions. Els arguments numèrics dins de les funcions són anàlegs a la posició dels potenciòmetres en els mòduls.

![](https://i.imgur.com/RBRxeiL.jpg)  
{.center}  
###### font: [Processador d’imatge Sandin](https://en.wikipedia.org/wiki/Sandin_Image_Processor)

La lògica consisteix a començar amb una ***font*** (com `osc()`, `shape()` o `noise()`), i després afegir-hi transformacions de ***geometria*** i de ***color*** (com `.rotate()`, `.kaleid()`, `.pixelate()`...), i finalment sempre cal connectar la cadena de transformacions a la pantalla de sortida amb `.out()`.

Per exemple, el següent codi mostra un oscil·lador amb els paràmetres de freqüència, sincronització i desplaçament RGB:

```hydra
osc(5, -0.126, 0.514).out()
```

Podem afegir una altra transformació a l’oscil·lador anterior afegint la funció `rotate()`:
```hydra
osc(5,-0.126,0.514).rotate().out()
```

Podem ampliar el patch aplicant un efecte de pixelació al resultat:
```hydra
osc(5,-0.126,0.514).rotate().pixelate().out()
```

---

## Glossari de funcions

Consulta la [interactive function reference](../../reference) per accedir al glossari de totes les funcions disponibles.
