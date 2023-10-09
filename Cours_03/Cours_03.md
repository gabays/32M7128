---
marp: true
theme: default
paginate: true
---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
h1 {
  font-size: 46px;
  color: darkred;
}
h2 {
  font-size: 38px;
  color: darkred;
}
</style>

Bibliothèques numériques I. Gestion de projet (conception, partage, archivage)

# Markdown et LaTeX

Simon Gabay

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Licence Creative Commons" style="border-width:0;float:right;\" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a>

---
# Markdown

---
## Markdown

Markdown est un langage de balisage léger créé en 2004. 

Il existe untutoriel pour les spécialistes de sciences humaines: Sarah Simpkin, « Débuter avec Markdown », traduction par Sofia Papastamkou, _Programming Historian en français_ 2 (2020), https://doi.org/10.46430/phfr0007.

---
## Exemple

On utilise des balises et un syntaxe très simple pour encoder des informations:

| Code Markdown                      | Rendu                            |
|------------------------------------|----------------------------------|
| `du **gras** ici`                  | du **gras** ici                  |
| `de l'_italique_ ici`              | de l'_italique_ ici              |
| `Un [lien vers Google](https://www.google.ch)` | Un [lien vers Google](https://www.google.ch) |


---
## Compiler

Pour obtenir le résultat final, on convertit par exemple le markdown en HTML (mais pas uniquement). On dit que l'on "compile" le code (source) en autre chose.

![width:100%](images/markdown-flowchart.png)

(Originellement, la compilation concerne le passage du code source en code objet. Tous les langages ne sont pas compilés, certains sont interprétés, comme Python ou R, d'autres sont compilés, comme Java).

---
## Utiliser Markdown

Pour utiliser Markdown:

- De nombreux éditeurs proposent un compilateur intégré (comme Oxygen, Visual Studio Code…)
- Il existe des applications en ligne comme https://markdownlivepreview.com
- Pour convertir des documents en ligne de commande, vous avez [Pandoc](https://pandoc.org)

---
## Exercice

Markdown est notamment utilisé avec GitLab/Github:

- Aller sur le site du cours: https://github.com/gabays/32M7128 ;
- Sélectionner le [`README.md`](https://github.com/gabays/32M7128/blob/master/README.md);
- Cliquer sur `Raw` en haut à droite pour voir le code non compilé.




---

Deux options:
- Créer un compte sur [Overleaf](https://www.overleaf.com)
- Installer en local
  - Télécharger la bonne distribution [en suivant ce guide](https://www.latex-project.org/get/)
  - Télécharger un déiteur, comme [TexStudio](https://www.texstudio.org).

---
## Lecture

```bibtex
@book{rouquette:halshs-00924546,
  TITLE = {{(Xe)LaTeX appliqu{\'e} aux sciences humaines}},
  AUTHOR = {Rouquette, Ma{\"i}eul},
  URL = {https://halshs.archives-ouvertes.fr/halshs-00924546},
  PUBLISHER = {{Atramenta}},
  PAGES = {268},
  YEAR = {2012},
  KEYWORDS = {informatique ; bibliographique ; latex},
  PDF = {https://halshs.archives-ouvertes.fr/halshs-00924546/file/latex-sciences-humaines.pdf},
  HAL_ID = {halshs-00924546},
  HAL_VERSION = {v1},
}
```

---
## Tex
`TeX` est un système logiciel libre de composition de documents. `LaTeX` est un langage et un système de composition de documents qui permet de simplifier l'utilisation de `Tex`. Il existe plusieurs distributions:
- `MiKTeX` pour Windows
- `MacTeX` pour Mac OS
- `Tex Live` pour Mac OS

---
## Les moteurs de composition

Il est nécessaire de compiler les documents `LaTeX` pour obtenir un pdf. Pour cela il existe plusieurs moteurs de composition (qui ne sont pas parfaitement compatibles):
- `pdfTex` qui n'est plus développé (mais reste utilisable), et duquel dérive les deux suivants
- `XeTex` modifie le moteur `Tex` pour élargir le nombre de caractères disponibles (encodage unicode+gestion des fontes OpenTypes)
- `LuaTex` qui utilise Lua

Nous utiliserons XeLaTeX, qui permet d'utiliser des commandes `LaTeX` avec `XeTeX`.

---
## Les bases

1. Que les commandes commencent par `\`
2. avec des arguments entre `{`accolades`}`
3. des arguments optionnels entre `[`crochets`]`

```latex
\maketitle
\textit{du texte en italique}
\includegraphics[height=2cm]{images/logoLatex.png}
````


---
## Un document simple

```latex
\documentclass{article}% la classe
\usepackage{fontspec}%gestion des caractères spéciaux (notamment les accents)

\begin{document}
Bonjour tout le monde!

C'est mon premier document LaTeX.
\end{document}
```

On observe la présence
1. D'un préambule, qui contient toutes les commandes qui affectent le document entier.
2. Du corps du document

Attention `\usepackage{fontspec}` est spécifique à `XeLaTex`

---
## Quelques premières commandes

1. `\textit` pour l'italique
```latex
\textit{texte à mettre en italique}
```
2. `\textbf` pour le gras
```latex
\textit{texte à mettre en gras}
```
3. `\textsc` pour les petites capitales
```latex
\textit{texte à mettre en petites capitales}
```

---
## Commandes plus complexes

Dans certains cas, il n'est pas possible de tout mettre entre `{`accolades`}`: il faut ouvrir et fermer l'élément (pour reprendre une terminologie XML):
```latex
\begin{itemize}
  \item un
  \item deux
  \item trois
\end{itemize}
```

---
## Commandes encore plus complexes
Il est parfois utile d'appeler un fichier externe au document `LaTeX`, comme une image
```latex
\begin{figure}[!htb]
  %je centre l'image
  \centering
  %je précise la taille de mon image et le chemin vers elle
  \includegraphics[height=2cm]{images/logoLatex.png}
  %je rajoute une légende
  \caption{ici un logo}
  %je lui donne un identifiant pour pointer vers l'image par la suite
  \label{fig:logolatex}
\end{figure}
```



