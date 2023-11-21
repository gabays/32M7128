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

Numériser le patrimoine I: standards et bonnes pratiques

# L'analyse de mise en page

Simon Gabay
Genève

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Licence Creative Commons" style="border-width:0;float:right;\" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a>

---

# _Pre-processing_

---

## Rotation

| Original 									  | Résultat							   |
|---------------------------------------------|----------------------------------------|
| ![w:250](images/1919_01_LAD_N504_6_rot.jpg) | ![w:250](images/1919_01_LAD_N504_6.jpg)|

---
## Niveau de gris

| Original 								| Résultat							   		|
|---------------------------------------|-------------------------------------------|
| ![w:250](images/1919_01_LAD_N504_6.jpg) | ![w:250](images/1919_01_LAD_N504_6_grey.jpg)|

---
## Binarisation

| Original 									 | Résultat							   		|
|--------------------------------------------|------------------------------------------|
| ![w:250](images/1919_01_LAD_N504_6_grey.jpg) | ![w:250](images/1919_01_LAD_N504_6_bin.jpg)|

---
## Binarisations
Le principe de la binarisation d'une image esrt de déterminer un seuil: les pixels dont le niveau de gris est en dessous du seuil deviennent noirs, et ceux au dessus deviennent blanc.

On n'utilise plus de seuil global (i.e. pour la page) mais par exemple des seuils contextuels calculés à partir de la moyenne et de la cote z d'une fenêtre centrée sur le pixel. Il existe une multitude de techniques plus ou moins efficaces comme Niblack ou son amélioration par Sauvola (cf. image).

![w:950](images/binarisation.png)


---
## Segmentation I

| Original 									| Résultat							   		   |
|-------------------------------------------|----------------------------------------------|
| ![w:250](images/1919_01_LAD_N504_6_bin.jpg) | ![w:250](images/1919_01_LAD_N504_6_bin_seg.jpg)|

---
## Segmentation: mise en page

![w:250](images/Pascal1663_Equilibre_btv1b86221002_0038_layout.jpg)

---
## Segmentation: lignes

![55%](images/1919_01_LAD_N504_6_bin_seg2.jpg)

L'OCR fonctionnant au niveau des lignes, il est fondamental de les extraire au mieux. L'utilisation d'outil dédié à la segmentation, et non d'un segmenteur intégré à l'OCR, peut être intéressant

---
## Segmentation: la décoration

![w:350](images/Sales1641_introduction_bpt6k1041711s_0025_ornement.jpg)

Le segmenteur peut extraire plus que des lignes: il peut extraire, par exemple, des ornements (bandeaux, initiales, culs-de-lampe…)

---

# Principes d'apprentissage machine

---
## L'apprentissage automatique

L'apprentissage machine (ou automatique) se fonde sur des approches mathématiques et statistiques pour donner aux ordinateurs la capacité d'« apprendre » à partir de données, c'est-à-dire d'améliorer leur performance à résoudre des tâches sans être explicitement programmés pour chacune.

Il existe plusieurs méthodes:
* Machines à vecteurs de support
* Méthodes statistiques
* Les réseaux de neurones
* …

---

## Apprendre pour une machine

Pour apprendre à une machine à faire quelque chose, on a deux possibilités:
1. Donner des règles à la machine: _suivons_ a pour lemme _suivre_. Le problème c'est qu'il existe des cas ambigus: _suis_? Il faut alors rajouter des règles toujours complexes
2. Donner des exemples à la machine, qui va déduire des règles à partir des exemples: _je suis un homme_ -> _être_ vs _je suis le cours_ -> _suivre_. Le problème c'est qu'il faut beaucoup d'exemple

Nous allons ici suivre la seconde méthode, qui est plus efficace. Il va donc nous falloir des exemples.

---
## Préparer un dataset
Il va nous falloir trois jeux de données:
* Pour l'entraînement: les données sont regardées par la machine pour déduire les règles
* Pour la validation: les données sont utilisées pour contrôler l'aprentissage sur les données d'entraînement à chaque itération, avec le risque que la machine se mette à bachoter
* Pour le test final: les données, qui n'ont jamais été vues pendant l'entraînement, vont permettre de contrôler la qualité finale

---
## Répartition du dataset

![w:800](images/dataset.png)

---
## Le règne de la quantité

L'IA est extrêmement gourmande en données: le plus c'est le mieux, il en faut donc un maximum. Il peut donc être utile de mutualiser les données, comme le propose le projet _HTR United_:

https://htr-united.github.io

Mais pour partager des données, il faut qu'elles soient compatibles entre elles, tant du point de vue du format que de l'annotation:
* On ne peut pas mélanger des données en ALTO et en PAGExml
* Si certains transcrivent _v_ et d'autres _u_ c'est potentiellement un problème
* Si certains distinguent les colonnes et d'autre pas ça ne va pas
* …

---
## Roadmap pour l'entraînement

![w:700](images/model.png)

---
## Petit lexique

* Modèle: somme de ce qui a été appris par la machine au cours de l'apprentissage
* Itération: cycle d'entrainement pendant le quel l'ordinateur voit toutes les données d'entraînement
* Surentraînement (_overfitting_): à force d'être évaluée sur le même jeu de validation à chaque itération, la machine se met à n'apprendre que pour passer ce test
* Généralisation: capacité du modèle à traiter des données qu'il n'a jamais vues.
* Arrêt prématuré (_early stopping_): arrêt de l'entraînement avant le surentraînement
* _Fine tuning_: on repart depuis un modèle déjà entraîné, auquel on ajoute une couche d'information

---
## Le résultat

On utilise pour évaluer un modèle un score dit "F1", calculé à partir de la précision (_precision_) et du rappel (_recall_):
* Le rappel calcule le nombre de positifs bien prédits par notre modèle.
* La précision calcule le nombre de prédictions positives bien effectuées.

On parle donc:
* De faux négatif, quand le résultat est déclaré positif mais est en réalité négatif. La machine s'est trompée.
* De vrai négatif, quand le résultat est déclaré négatif mais est en réalité positif. La machine a oublié quelque chose.

---
## Score F1

![w:320](images/f1.png)

---
## IoU

On utilise aussi l'intersection sur l'union (_Intersection over union_), plus adapté pour des zones (sur une page par exemple).

![w:800](images/iou.png)

---

![w:1200](images/iou_ex.png)

---
## Sur le test
Le score du test n'a pas de valeur en soi, il dépend:
* Des données d'entraînement
* Des données dans le jeu de test

99% sur 10 000 images ne vaut pas (forcément) 99% sur 1000 images

Pour le test, on peut utiliser des données:
* _In domain_: elles sont proches du jeu d'entraînement (tirées du même livre par exemple)
* _Out of domain_: elles sont différentes du jeu d'entraînement -- la question étant de savoir jusqu'à quel point différentes

---
## Exercice

Regardez le dossier `Exercices` et tenter de trouver un dénominateur commun à toutes ces images

---

# _SegmOnto_

---
## Intuition


![w:395](images/btv1b84259980_f29_ann.jpg)![w:350](images/btv1b86070385_f140_ann.jpg)

---
## Principes

<img src="images/btv1b84259980_f29_ann.jpg" style="float:right;" height="350" width="280"/>

Titre courant, numéro de page, corps du texte, initiale, rubrique...

Approche générale plutôt que spécifique

Mise en page plutôt qu’analyse sémantique

Une standardisation est cruciale pour la numérisation massive des documents qui s’annonce

---
## Double objectif

- Partager les données (_upstream_): l’intelligence artificielle a besoin de données: les équipes de recherche ont besoin de partager des documents annotés pour améliorer les résultats de l’HTR en augmentant la quantité de données d’entraînement
- Partager les données (_downstream_): les équipes de recherche ont aussi besoin de mettre en commun des post-traitement de données pour l’exploration des corpus et la production/transformation automatisée de documents (TEI, RDF...)

<span style="color:red;">→ Trouver un vocabulaire partagé et contrôlé pour décrire la page</span>

---
## Trouver un vocabulaire

Plutôt de créer un vocabulaire, il est préférable de s’appuyer sur de l’existant
- Vocabulaire PAGE xml, avec très peu de catégories
- Vocabulaire _Codicologia_, un vocabulaire multilingue pour la description des manuscrits, avec énormément de catégories, et adapté pour le RDF (Vocabulaire international de la codicologie - SKOS)

Liens:
- http://codicologia.irht.cnrs.fr/
- http://gams.uni-graz.at/archive/objects/o:voccod/methods/sdef:SKOS/get

---

<style scoped>
table {
    height: 100%;
    width: 100%;
    font-size: 27px;
}
</style>

## Zones

| Zones                      | Zones (suite)             |
|----------------------------|---------------------------|
| `CustomZone`               | `NumberingZone`           |
| `DamageZone`               | `QuireMarksZone`          |
| `DigitizationArtefactZone` | `RunningTitleZone`        |
| `DropCapitalZone`          | `SealZone`                |
| `GraphicZone`              | `StampZone`               |
| `MainZone`                 | `TableZone`               |
| `MarginTextZone`           | `TitlePageZone`           |
| `MusicZone`                |                           |


---
## Lignes

- `CustomLine`
- `DefaultLine`
- `DropCapitalLine`
- `HeadingLine`
- `InterlinearLine`
- `MusicLine`

---

<style scoped>
table {
    height: 100%;
    width: 100%;
    font-size: 20px;
    margin-bottom: -10px;
    padding-bottom: -10px;
}
div.twocols {
  margin-top: 35px;
  column-count: 2;
}
div.twocols p:first-child,
div.twocols h1:first-child,
div.twocols h2:first-child,
div.twocols ul:first-child,
div.twocols ul li:first-child,
div.twocols ul li p:first-child {
  margin-top: 0 !important;
}
div.twocols p.break {
  break-before: column;
  margin-top: 0;
}
</style>
<div class="twocols">

## `CustomZone`
- Definition: characterises any kind of zone not fitting in the other categories, according to any convenient typology the user chooses.
- Subtypes: Any
- Examples: encoding catalogue entries with
  - `CustomZone:entry#1`
  - `CustomZone:entry#2`

<p class="break"></p>

![right height:550px](SegmOnto/BNF_bpt6k1181800p_26.jpeg)
</div>

---

## `DamageZone`

* Definition: characterises any area containing damage to the source, such as holes in the material (parchment, paper…), blots, etc.
* Subtypes: suggested values include:
  * `DamageZone:corrosion` (_corrosion_)
  * `DamageZone:hole` (_trou_)
  * `DamageZone:mold` (_moisissure_)
  * `DamageZone:peeled` (_desquamé_)
  * `DamageZone:soaked` (_détrempé_)
  * `DamageZone:scuffed` (_eraillé_)
…

---
## `DamageZone` 2
Examples:

| `DamageZone:soaked` 									| `DamageZone:hole`							   		   |
|-------------------------------------------|----------------------------------------------|
| ![w:380](SegmOnto/BB57.png) | ![w:320](SegmOnto/e-codices_Mslitt-0010-1.jpg)|


Identifying damaged area might prove useful, as they can affect the result of text prediction.

---

## `DigitizationArtefactZone`

* Definition: it contains any type of item external to the document itself, but due to the process of digitisation, such as rulers or color tables added to help analyse the image.
* Subtypes: suggested values include:

| `DigitizationArtefactZone:testCard` 									| `DigitizationArtefactZone:ruler`							   		   |
|-------------------------------------------|----------------------------------------------|
| ![w:300](SegmOnto/Berlin_Stabi_Darmst_2m_1660.jpg) | ![w:99](SegmOnto/BGE_cl0269_bindingRulerS.png)|


---

## `DropCapitalZone`

* Definition: contains any type of initial letter, occupying a space corresponding to several lines of the main text or bearing significant ornementation, be they historiated, ornated, flourished or painted initials (and excluding the following text line).

* Subtypes: suggested values include:

  * `DropCapitalZone:historiated`
  * `DropCapitalZone:floriate`
  * `DropCapitalZone:flourish`
  * `DropCapitalZone:voided`
  * `DropCapitalZone:parted`

---

## `DropCapitalZone`

  * `DropCapitalZone:champ`
  * `DropCapitalZone:facetted`
  * `DropCapitalZone:plain`
  * `DropCapitalZone:interlocked`

| Initiale manuscrite 									| initiale imprimée							   		   |
|-------------------------------------------|----------------------------------------------|
| ![w:290](SegmOnto/btv1b84259980_f68.jpg) | ![w:500](SegmOnto/btv1b86070385_f78.jpg)|



---

## `GraphicZone`

* Definition: characterises a zone containing any type of graphic element, from purely ornamental  to consubstantial to the text (e.g., full page paintings, line-fillers, marginal drawings, figures, etc.).
* Subtypes: suggested values include:

  * `GraphicZone:illustration`
  * `GraphicZone:ornamentation`
  * `GraphicZone:figure`

---
## `GraphicZone`

| `GraphicZone:illustration` 								| `GraphicZone:ornamentation`							   		   |
|-------------------------------------------|----------------------------------------------|
| ![w:144](SegmOnto/btv1b84259980_f466.jpg) | ![w:290](SegmOnto/btv1b86070385_f65.jpg) |


| `GraphicZone:figure` 									    | `GraphicZone:figure`							   		   |
|-------------------------------------------|----------------------------------------------|
| ![w:300](SegmOnto/btv1b8601519p_f219.jpg) | ![w:198](SegmOnto/Latin_7295A__btv1b10027322j_23.jpeg) |

---

## `MainZone`

* Definition the main area (text column) designed to contain text, either as a single  or several columns (as designed in the conception of the layout: including eventually text, music notations, illumination, etc.).

* Subtypes: suggested values include:

| `MainZone:column` 								| `MainZone:column#1` and `MainZone:column#1`							   		   |
|-------------------------------------------|----------------------------------------------|
| ![w:110](SegmOnto/btv1b8601519p_f144.jpg) | ![w:110](SegmOnto/ex1.png) |




---

## `MarginTextZone`

* Definition: characterises any **text zone** contained in the margins (upper, lower, inner or outer), including the space between two columns, whatever their semantic status (gloss, additions, …).

* Subtypes: suggested values include:

  * `MarginTextZone:note`
  * `MarginTextZone:commentary`
  * `MarginTextZone:correction`
  * `MarginTextZone:variants`

---
## `MarginTextZone`

| `MarginTextZone` 								          | .                 							   		   |
|-------------------------------------------|------------------------------------------|
| ![w:260](SegmOnto/btv1b6000371s_f21.jpg) | ![w:180](SegmOnto/btv1b86070385_f144.jpg) |

| `MarginTextZone:variantes` 			            | `MarginTextZone:variantes`			   		      |
|---------------------------------------------|---------------------------------------------|
| ![w:110](SegmOnto/corpus_christianorum.jpg) | ![w:110](SegmOnto/corpus_christianorum.jpg) |

---

## `MusicZone`

* Definition: characterises an area containing musical notations, such as neumes, staves, etc., with the possible inclusions of text.

* Subtypes: none

* Examples:

| `MusicZone` imprimée 			                  | `MusicZone` manuscrite    			   		      |
|---------------------------------------------|---------------------------------------------|
| ![w:250](SegmOnto/btv1b8446952v_f33.jpg)    | ![w:230](SegmOnto/btv1b84192440_f58.jpg)    |


---

## `NumberingZone`

* Definitio: it characterises a zone containing the page number.

* Subtypes: suggested values include:
  * `NumberingZone:page`
  * `NumberingZone:other`

* Examples

| `NumberingZone:folio`             		      | `NumberingZone:page`    	    		   		    |
|---------------------------------------------|---------------------------------------------|
| ![w:190](SegmOnto/btv1b84192440_f45.jpg)    | ![w:190](SegmOnto/btv1b86070385_f135_p.jpg) |


---
## `QuireMarksZone`

* Definition: characterises a zone containing a quire signature (i.e., _a ii_), catchword, or any kind of element relative to the material organisation of the source, with the exclusion of page numbers.

* Subtypes: suggested values include:
  * `QuireMarksZone:signature`
  * `QuireMarksZone:catchwords`

---
## `QuireMarksZone`
* Examples
  * `QuireMarksZone:signature`

![w:420](SegmOnto/bpt6k1280589b_f17.jpg)

  * `QuireMarksZone:catchwords`

| `QuireMarksZone` manuscrite            		  | `QuireMarksZone` imprimée   	   		     |
|---------------------------------------------|------------------------------------------|
| ![w:270](SegmOnto/btv1b8451116z_f340.jpg)   | ![w:380](SegmOnto/bpt6k1280589b_f86.jpg) |



---

## `RunningTitleZone`

* Definition: characterises a zone containing a running title.

* Subtypes: none

* Examples

| `RunningTitleZone` imprimée            	  | `RunningTitleZone` manuscrite   	   	    |
|-------------------------------------------|-------------------------------------------|
| ![w:440](SegmOnto/bpt6k1280589b_f24.jpg)  | ![w:490](SegmOnto/btv1b84259980_f112.jpg) |


---

## `SealZone`

* Definition: it characterises a zone containing a seal.

* Subtypes: none

* Examples

![w:740](SegmOnto/btv1b10540051s_f273.jpg)![w:150](SegmOnto/btv1b550082631_f1.jpg)


---

## `StampZone`

* Definition: it characterises a zone containing a stamp, be it a library stamp or a mark from a postal service.

* Subtypes: suggested values include:
  * `StampZone:postal`
  * `StampZone:curatorial`

---

## `StampZone`

* Examples
  * `StampZone:curatorial`

![w:300](SegmOnto/bpt6k1520316t_f35.jpg)
![w:300](SegmOnto/btv1b6000371s_f21_stamp.jpg)

* `StampZone:postal`

![w:150](SegmOnto/wiki_Esternay_Carte_postale_Tampon.jpg)

---

## `TableZone`

* Definition: characterises a zone containing a table of any kind.

* Subtypes: none

* Examples

| `TableZone`                           	  | `TableZone`                      	   	    |
|-------------------------------------------|-------------------------------------------|
| ![w:160](SegmOnto/bpt6k106140h_2.jpeg)    | ![w:280](SegmOnto/btv1b10027322j_f41.jpg) |


---

## `TitlePageZone`

* Definition: it characterises a zone containing a title distinct from the main text. It is mainly used for prints.

* Subtypes: none

* Examples

| `TitlePageZone` manuscrite                | `TitlePageZone` imprimée              	  |
|-------------------------------------------|-------------------------------------------|
| ![w:100](SegmOnto/bpt6k111470b_f1.jpg)    | ![w:85](SegmOnto/bpt6k1520316t_f7.jpg)    |

---
## `CustomLine`

* Definition: characterises any kind of line not fitting in the other categories, according to any convenient typology the user chooses.

* Subtypes: Any

* Justification: all projects have specific needs regarding types of lines peculiar to their sources or their goals and  not covered by the standard types.

---
## `DefaultLine`

* Definition: it characterises any kind of standard text line, whether they are included in the `MainZone` text, in the `MarginZone`, in `MusicZone`, or in any type of zone.

* Subtypes: none

* Examples

| `DefaultLine` imprimée                    | `DefaultLine` manuscrite              	  |
|-------------------------------------------|-------------------------------------------|
| ![w:100](SegmOnto/btv1b8601519p_f144.jpg) | ![w:100](SegmOnto/btv1b84259980_f47.jpg)  |

---
## `DropCapitalLine`

* Definition: characterises a line on which rests a [`DropCapital`](https://github.com/SegmOnto/examples/tree/main/zones/DropCapital).

* Subtypes: none

* Examples

| `DefaultLine` imprimée                    | `DefaultLine` manuscrite              	  |
|-------------------------------------------|-------------------------------------------|
| ![w:200](SegmOnto/btv1b84259980_f68.jpg) | ![w:400](SegmOnto/btv1b86070385_f78.jpg)  |

* Justification: drop capitals are both specific zones, and bear a text.

---
## `InterlinearLine`

* . Definition: characterises a line that is not a standard text line, but as been added between two of them, for instance to include a forgotten word.

* Subtypes: suggested values include:

  - `InterlinearLine:commentary`
  - `InterlinearLine:correction`

* Examples

  - `InterlinearLine:correction`

![w:600](SegmOnto/btv1b6000371s_f21_inter.jpg)

---
## `MusicLine`

* Definition: caracterises the central line of a musical stave.

* Subtypes: none

* Examples

![w:400](SegmOnto/btv1b8446952v_f33.jpg)![w:300](SegmOnto/btv1b84192440_f58.jpg)


* Justification: analysis or prediction of musical content can be as necessary as the text's.

---
## `HeadingLine`

* Definition: characterises a line containing a rubric, for instance signalling the beginning of a new text.

* Subtypes: suggested values include:

  - `HeadingLine:incipit`
  - `HeadingLine:explicit`
  - `HeadingLine:title`

* Examples
![w:400](SegmOnto/btv1b84259980_f29.jpg)![w:200](SegmOnto/bpt6k1280589b_f15.jpg)

---

# Exercice : créer un nouveau document

---
## La description du document (1)

Après avoir cliqué sur `Create new Document`, plusieurs éléments sont à préciser dans la partie *Description* :
- commencer par donner un nom à votre document ;
- *Main script* : Latin ;
- *Read direction* : *Left to Right* ;
- *Line offset* : *Baseline*
- *Metadata* : les métadonnées sont remplies automatiquement si vous choisissez de charger votre document via IIIF.

---
## La description du document

![w:1000](img_tuto/Description.png)

---
## Métadonnées remplies automatiquement via IIIF

![w:500](img_tuto/Metadata_iiif.png)

---
## La description du document (2)

La dernière étape consiste à mettre à jour l'ontologie du document afin qu'elle corresponde à celle des deux modèles précédemment chargés :

*Region types* :
- ajouter manuellement *CustomZone*, *DamageZone*, *DigitizationArtefactZone*, *DropCapitalZone*, *GraphicZone*, *MainZone*, *MarginTextZone*, *MusicZone*, *NumberingZone*, *QuireMarksZone*, *SealZone*, *StampZone*, *TableZone* et *TitlePageZone* (15 éléments au total).

*Line types* :
- ajouter manuellement *CustomLine*, *DefaultLine*, *DropCapitalLine*, *InterlinearLine*, *MusicLine* and *HeadingLine* (6 éléments au total).
- Enfin, appuyer sur `Create`.

---
## L'ontologie

![w:900](img_tuto/Ontology.png)

---

## La segmentation des images

---
## Images

![w:1000](img_tuto/Images.png)

---

## Regions

- Commencer par sélectionner l'ensemble des images que vous souhaitez segmenter ;
- Cliquer sur `Segment`;
- Le `Model` à utiliser est le modèle de segmentation par défaut (`blla.mlmodel`) ;
- Sélectionner `Regions` dans `Segmentation steps` ;
- Lancer la segmentation et patienter.

---
## Paramètres pour la segmentation par régions

![w:500](img_tuto/Segment_regions.png)

---
## Exemple de résultat

![w:500](img_tuto/Segment_sans_correction.png)

---
## Corrections

- Vous pouvez à présent consulter les résultats de la segmentation en cliquant sur `Edit`.
- Le modèle par défaut n'étant pas familier de l'ontologie que nous souhaitons utiliser, il reste à associer chacune des zones repérées à la bonne terminologie.
- Il se peut qu'il soit nécessaire de redéfinir certaines zones, pour cela cliquer sur le petit ciseau pour passer en mode *édition*.


---
## Lines

Il reste à présent à repérer les lignes du document :
- sélectionner les images ;
- cliquer sur `Segment` ;
- le `Model` à utiliser est le modèle par défaut ;
- sélectionner `Lines Baselines and Masks` dans `Segmentation steps` ;
- lancer la segmentation ;
- vérifier que les lignes ont bien été reconnues.

N.B. : il est important de commencer par segmenter les pages par régions puis par lignes afin que ces dernières soient automatiquement associées à leurs zones.

---
## Corrections

- Vous pouvez à présent consulter les résultats de la segmentation en cliquant sur `Edit`.
- Le modèle par défaut n'étant pas familier de l'ontologie que nous souhaitons utiliser, il reste à associer chacune des lignes repérées à la bonne terminologie.
- Il se peut qu'il soit nécessaire de retracer certaines lignes, pour cela cliquer sur le petit ciseau pour passer en mode *édition*.

