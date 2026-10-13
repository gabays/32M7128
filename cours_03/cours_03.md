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

Introduction à la philologie computationnelle

# L'analyse de mise en page

Simon Gabay

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Licence Creative Commons" style="border-width:0;float:right;\" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a>


---
## Analyse de mise en page

L'analyse de mise en page est une tâche de vision par ordinateur (_computer vision_).

L'idée est de s'intéresser à la "mise en texte", c'est-à-dire ce qui "dans le livre n’est ni texte ni écriture, mais sans lequel ni l’un ni l’autre ne pourraient exister".

---

![w:1000](images/yolo.jpg)

Analyse avec YOLO.

---

## Machine readable

In recent years, scholars have created the tools to retrieve information, mainly the text from (historical) documents.

By doing so, they create **machine readable** data: rather than pixels on an image, they have characters in a text document.

---

![w:395](images/btv1b8608299v_f27_raw.JPEG)

---

![w:395](images/btv1b8608299v_f27_la.png)

---
```
DE I. A. DE BAIF.
21
Si est-ce que la ruine
N'est penchante que sus toy:
Car helas helas, Meline,
Plus plus ie ne suis a moy,
Et s'il te plaist que ie meure,
Mort encor tien ie demeure.
O Douce peinture amiable,
Peinture toute pitoyable,
Qui riant me prometz le bien,
Vers qui tout autre ne m'est rien.
O seul confort a ma detresse,
Mais pourquoy ma fiere Maitresse,
Las, douce & fiere, mais pourquoy
Ne me rit elle comme toy,
D'un ris montrant misericorde,
Lors que dauant toy ie recorde,
Vne harangue de pitié,
Pour adoucir sa mauuaitié?
O pleust a dieu, que dauant elle
Asseuré ie la fisse telle,
Comme a toy ie la fais ici:
Bien tost ie trouueroy merci.
Quoy que tu soys peinture morte,
Toutesfoys ma plainte est si forte,
Qu'helas tu m'en semble douloyr.
Et i'atendriroys son vouloyr,
Mais dauant elle faut ma langue
Au premier mot de sa harangue:
Tel est son œil eblouissant,
b. iij.
```
---
## Machine actionable
What we now need to do is not only to retrieve the text, but to \textbf{restructure} the whole document.

By doing so, we create **machine actionable** document: rather than raw text, we have semantic labels regarding the type of text (the footnote vs the main body)

---

![w:395](images/btv1b8608299v_f27_raw.JPEG)

---

![w:395](images/btv1b8608299v_f27.png)

---
```xml
<pb facs="btv1b8608299v_page_00027.xml"/>
<fw>DE I. A. DE BAIF.</fw>
<fw>21</fw>
<ab>
    <lb/>Si est-ce que la ruine
    <lb/>N'est penchante que sus toy:
    <lb/>Car helas helas, Meline,
    <lb/>Plus plus ie ne suis a moy,
    <lb/>Et s'il te plaist que ie meure,
    <lb/>Mort encor tien ie demeure.
    <lb/><hi>O</hi>Douce peinture amiable,
    <lb/>Peinture toute pitoyable,
    <lb/>Qui riant me prometz le bien,
    <lb/>Vers qui tout autre ne m'est rien.
    <lb/>O seul confort a ma detresse,
    <lb/>Mais pourquoy ma fiere Maitresse,
    <lb/>Las, douce &amp; fiere, mais pourquoy
    <lb/>Ne me rit elle comme toy,
    <lb/>D'un ris montrant misericorde,
    <lb/>Lors que dauant toy ie recorde,
    <lb/>Vne harangue de pitié,
    <lb/>Pour adoucir sa mauuaitié?
    <lb/>O pleust a dieu, que dauant elle
    <lb/>Asseuré ie la fisse telle,
    <lb/>Comme a toy ie la fais ici:
    <lb/>Bien tost ie trouueroy merci.
    <lb/>Quoy que tu soys peinture morte,
    <lb/>Toutesfoys ma plainte est si forte,
    <lb/>Qu'helas tu m'en semble douloyr.
    <lb/>Et i'atendriroys son vouloyr,
    <lb/>Mais dauant elle faut ma langue
    <lb/>Au premier mot de sa harangue:
    <lb/>Tel est son œil eblouissant,
</ab>
<gb>b. iij</gb>
```
---
# Pré-éditorialisation

**Editorialisation**: processus consistant à enrôler des ressources pour les intégrer dans une nouvelle publication.

**Pré-éditorialisation**: un état numérique pivot du texte, compris comme un dénominateur commun à un maximum de projets, anticipant une série de manipulations chronophages, et qu’il est possible de continuer à enrichir sans avoir rien à y retrancher.

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

![w:300](images/Sales1641_introduction_bpt6k1041711s_0025_ornement.jpg)

Le segmenteur peut extraire plus que des lignes: il peut extraire, par exemple, des ornements (bandeaux, initiales, culs-de-lampe…)


---

# Exercice de modélisation

---
## Analyser une image

![w:370](images/btv1b84259980_f29_orig.jpg)

---
## Analyser une image

![w:370](images/btv1b84259980_f29_orig.jpg)![w:330](images/btv1b86070385_f140_orig.jpg)

---
## Cas problématique

 ![h:200](images/1285_Fournival_btv1b84259980_f474_decoration.jpg) ![h:200](images/1230_ChretienTroyes_btv1b84272526_f67_InitialeHistoriee.jpg) ![h:200](images/1300_Bible_btv1b10052442z_f10_initialeOrnee.jpg)

Est-ce que
- Une vignette
- Une initiale historiée
- Une initiale ornée
sont une, deux ou trois choses différentes?

---
## Cas problématique II

![h:200](images/bpt6k15120824_f12.jpg) ![h:200](images/bpt6k15120824_f17.jpg) ![h:200](images/bpt6k15120824_f20.jpg) ![h:200](images/bpt6k15120824_f13.jpg) 

---
## Cas limite

![h:550](images/btv1b100737746_f2.jpg)

---
## Exercice

Regardez le dossier `Exercices` et tenter de trouver un dénominateur commun à toutes ces images

---

# SegmOnto

---
## Analyser une image

![w:370](images/btv1b84259980_f29_orig.jpg)![w:330](images/btv1b86070385_f140_orig.jpg)

---
## Intuition

![w:370](images/btv1b84259980_f29_ann.jpg)![w:330](images/btv1b86070385_f140_ann.jpg)

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
## Modéliser
- Tenir compte des limites posées par la machine (nombre de classes, quantité de données nécessaires pour détecter une nouvelle classe)
- Être capable de trouver des dénominateur commun en jouant sur la granularité:
  - Trop peu de classes, et l'on ne récupère pas assez d'information
  - Trop de classes et on risque des chevauchement des classes

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

# Les Zones

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

# Les lignes

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
![h:190](SegmOnto/btv1b84259980_f29.jpg) ![h:190](SegmOnto/bpt6k1280589b_f15.jpg)

---

# Extensions

---
## Le manuscrit moderne

![w:550](images/val_batch1_pred.jpg)

---
## LaDaS

![h:550](images/theatre_bd6t57417754_1637_f20-jpg_jpg.rf.c66797ff2e15c88acec9e423b5e32b17.png) ![h:550](images/these_1-n7vdcd_p44-1_jpg.rf.c7229636b396229e7f67e8703d414781.png)
