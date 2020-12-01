Formation Edition numérique

# Langages du web: HTML et CSS

Simon Gabay

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Licence Creative Commons" style="border-width:0;float:right;\" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a>

---
## HTML
---

### HTML

HTML, pour _Hypertext Markup Language_, est un langage de balisage inventé en 1997 conçu pour représenter les pages web.

Plus d'informations sur [Wikipedia](https://fr.wikipedia.org/wiki/Hypertext_Markup_Language)

Si le XML est le fond, HTML est la forme.

---

### Un document HTML

```HTML
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"/>
    <meta name="keywords"
          content="key1, key2"/>
    <title>Titre</title>
  </head>
  <body>
    <h1>This is the h1</h1>
    <div>
      <h2>This is the h2</h2>
      <p>First paragraph.</p>
      <p>Another
        <br/>paragraph.</p>
    </div>
  </body>
</html>
```

---

### HTML vs XML: points communs
- Structure arborescente
- Une balise ouverte doit être fermée
- Il est possible d'utiliser des balises autofermantes

---

### Principaux éléments HTML

C'est un vocabulaire non-extensible. Il a quelques éléments importants:
- L'élément ```<span>``` est un conteneur générique en ligne (_inline_) pour les contenus phrasés.
```XML
<div>Ici commence une div dont je veux <span>encadrer
un passage</span> pour telle et telle raison</div>
```

- L'élément ```<img>``` permet d'insérer une image. Le chemin vers le fichier est spécifié avec ```src``` et ```alt``` permet de fournir une description sommaire de l'image (si le navigateur n'arrive pas à l'ouvrir)

```HTML
<img src="chemin.jpg" alt="description"/>
```

---

### Principaux éléments HTML (suite)

- L'élément ```<ul>``` (unordered list) est une liste non numérotée
```HTML
<ul>
  <li>un item</li>
  <li>un autre item</li>
</ul>
```
On obtient une liste avec des puces :

```HTML
• un item
• un autre item
```

---

### Principaux éléments HTML (suite)


- L'élément ```<ol>``` (_ordered list_) fonctionne de la même manière
```HTML
<ol>
  <li>un item</li>
  <li>un autre item</li>
</ol>
```
On obtient une liste numérotée :

```HTML
1. un item
2. un autre item
```
---

### Principaux éléments HTML (suite)

- L'élément ```<a>``` permet de créer un lien. Il est utilisé avec ```href``` pour encoder l'adresse du lien

```HTML
<a href="www.unine.ch">uni de Neuch</a>
```

On obtient un lien

[uni de Neuch](www.unine.ch)

- L'élément ```<meta>``` permet d'ajouter des metadonnées au document

```HTML
<meta charset="utf-8"/>
<meta name="keywords"
       content="key1, key2"/>
```

---

### Deux attributs importants

- ```class``` permet de catégoriser l'élément (utile pour javascript et CSS).

```HTML
<div>Ici commence une div où j'identifie une ville comme
  <span class="lieu">Paris</span> mais aussi beaucoup
  d'autres comme <span class="lieu">Berne</span> ou bien
  <span class="lieu">Berlin</span> parce que ça
  m'intéresse.</div>
```

- ```id``` permet d'identifier un élément (utile pour javascript et CSS).

```HTML
<div>Ici commence une div dans laquelle je veux
  identifier le nom <span id="simon">Simon</span> parce
  que, encore une fois, ça m'intéresse.</div>
```


---

## Exercice

Fabriquez votre CV en HTML
