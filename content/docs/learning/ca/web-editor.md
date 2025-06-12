---
title: "editor web"
date: 2023-04-04T15:10:36+02:00
draft: false
author: "Flor de Fuego i Olivia Jack / Traducció: Arnau Casanoves"
weight: 3
---

# editor web

Ús bàsic de l'editor del navegador a [hydra.ojack.xyz](https://hydra.ojack.xyz)

---

## Dreceres de teclat

* CTRL+Enter: executa una línia de codi  
* CTRL+Shift+Enter: executa tot el codi de la pantalla  
* ALT+Enter: executa un bloc de codi  
* CTRL+Shift+H: mostra o amaga el codi  
* CTRL+Shift+F: dona format al codi amb [Prettier](https://prettier.io/)  
* CTRL+Shift+S: desa una captura de pantalla com a fitxer local  
* CTRL+Shift+G: comparteix a Mastodon (ara no està disponible, però encara pots desar esbossos)

---

## Barra d'eines

A la cantonada superior dreta trobaràs una barra d'eines amb aquests botons:  
![](https://i.imgur.com/iCG8Lrq.png)  
{.center}

1. **executa tot el codi** Executa tot el codi a la pàgina (equivalent a *ctrl+shift+enter*)  
2. **puja a la galeria** Puja un esbós a la galeria d'Hydra i genera un enllaç curt  
3. **neteja tot** Reinicia l'entorn i esborra el text de l’editor  
4. **mostra esbós aleatori** Carrega exemples aleatoris. Una bona manera d'aprendre és veure codi d'altres  
5. **canvi aleatori** **daus** Modifica valors automàticament. Prova-ho amb qualsevol esbós  
6. **mostra informació** Obre una finestra amb ajuda i enllaços útils

---

## Desa els teus esbossos

### Com a enllaç

Quan executes tot el codi amb el ***botó d’execució*** o amb `Shift + Ctrl + Enter`, Hydra genera automàticament una URL amb l’última versió del teu esbós. Pots copiar l’enllaç per desar-lo o compartir-lo. També pots utilitzar les fletxes del navegador (`enrere` i `endavant`) per navegar per versions anteriors.  
![](https://i.imgur.com/lV0rmoh.png)

### A internet

També pots pujar l’esbós a la base de dades d’Hydra des de l’editor. Això és útil per a esbossos llargs. Els esbossos i les seves captures es comparteixen públicament a Mastodon. *(NOTA: el compte de Mastodon ara no funciona, per tant no es comparteixen automàticament).*

### Comparteix amagant el codi per defecte

L’etiqueta `showCode=false` a la URL permet compartir un esbós amb el codi amagat.

#### Exemple

Aquest és un esbós i el seu enllaç:

```javascript
osc(10, 0.1, 1.2).modulateScale(noise(3)).out()
```
```
https://hydra.ojack.xyz/?code=b3NjKDEwJTJDJTIwMC4xJTJDJTIwMS4yKS5tb2R1bGF0ZVNjYWxlKG5vaXNlKDMpKS5vdXQoKQ%3D%3D
```


I aquest és el mateix, però amb el codi amagat:
```
https://hydra.ojack.xyz/?code=b3NjKDEwJTJDJTIwMC4xJTJDJTIwMS4yKS5tb2R1bGF0ZVNjYWxlKG5vaXNlKDMpKS5vdXQoKQ%3D%3D&showCode=false

```

Pots tornar a veure el codi prement `Ctrl+Shift+H`.

---

## Què és un error?
De vegades, en executar una línia de codi, no passa res. Si hi ha un error, apareixerà un text en vermell a la part inferior esquerra de la pantalla. Per exemple: ‘Unexpected token ‘.’ (in red) 
No afecta el teu codi permanentment, però no podràs continuar fins que ho arreglis. Sol ser un error de tecleig o de sintaxi.

## Què és un comentari?

```javascript
// Hola! Sóc un comentari. No canviaré el teu codi. Pots escriure anotacions, el teu nom, un manifest o fins i tot un poema.
```

---

By Flor de Fuego, Olivia Jack
Traducció al català per [Arnau Casanoves](https://www.arnaucasanoves.com)
