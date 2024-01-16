# DataCatalogue 🐱 - Guide d'annotation

**DataCatalogue** (2021-2024) est un projet de recherche conjointement mené par l'équipe **ALMAnaCH** (Inria), la **Bibliothèque nationale de France** (BnF), et l'**Institut National d'Histoire de l'Art** (INHA). 

Ce guide présente le schéma d'annotation utilisé pour annoter plusieurs collections de catalogues de vente provenant de la BnF et l'INHA. Ce corpus annoté sera ensuite utilisé pour affiner un modèle YOLOv8 qui servira à analyser automatiquement la mise en page des catalogues de vente. 

Cette tâche automatique résultera dans la segmentation de la **macro-structure des catalogues de vente**, permettant ensuite une analyse fine de leur contenu. 

## Présentation du corpus d'entraînement

Le corpus d'entraînement a été assemblé à partir de 8 collections de catalogues de vente provenant de la BnF et l'INHA : 

### BnF

* Bienaimé-Feuardent (BnF)
* Bourgey (BnF)
* Dubois (BnF)
* Naville (BnF)
* Rollin (BnF)
* XVIIIe siècle (BnF)

### INHA

* Desvouges (INHA)
* Lair-Dubreuil (INHA)

Deux pages aléatoires ont été sélectionnées dans chaque catalogue téléchargé depuis Gallica et la bibliothèque numérique de l'INHA, et ont été ajouté à la plateforme d'annotation Roboflow.

Selon les résultats de l'affinage du modèle YOLOv8, nous nous attendons à une sur-représentation des catalogues provenant de certaines collections de la BnF pour lesquels la masse documentaire est plus importante, ce qui pourrait impacter la segmentation d'autres collections de catalogue. Le dataset devra donc peut-être être équilibré dans le futur. 

## Classes utilisées pour l'annotation

Le schéma d'annotation utilisée dans le cadre du projet DataCatalogue se base en majorité sur l'ontologie [SegmOnto](https://segmonto.github.io/), et sur la campagne d'annotation du projet COLAF (Inria). Certaines classes ont été adaptées aux cas d'usages définis par les catalogues de vente et à la modélisation TEI employée pour représenter ces documents, et d'autres ont été ajoutées.

### MainZone:Entry

### MainZone:Entry#Continued

### MainZone:CatalogueDesc

MainZone:CatalogueDesc correspond à une zone de texte habituellement située juste après un niveau de titre dans un catalogue de vente, et précédant les notices. Elle donne des informations sur un ensemble de notices, régies elle-même par un niveau de titre. On y retrouve des informations de chronologie, de bibliographie, des commentaires sur la vente, etc.

Fichier exemple : rollin_12148-bpt6k9780581w_f122.jpg

![Fichier exemple pour illustrer la classe MainZone:CatalogueDesc](https://s3.hedgedoc.org/demo/uploads/cd72bcaa-11c3-456f-a19e-06785075b12c.png)

Ici, en rouge et sous les niveaux de titre en orange, un exemple de deux MainZone:CatalogueDesc donnant des informations temporelles.


### MainZone:P

### RunningTitleZone

Voir la classe [RunningTitleZone](https://segmonto.github.io/gd/gdZ/RunningTitleZone/) telle qu'elle est définie par SegmOnto. 

>> RunningTitleZone: is a zone containing a running title, traditionally at the top of the page or of the double page. It can be the title (or the abbreviated title) of a document or of the current section.

Fichier exemple : bienaimé-feuardent_12148-bpt6k9777643f_f60.jpg

![Fichier exemple pour illustrer la classe RunningTitleZone](https://s3.hedgedoc.org/demo/uploads/db9e0d06-4ded-4edd-a788-95f02418ab90.png)

Ici, en rouge et en haut de la page et centrée, un exemple d'un segment de texte de classe **RunningTitleZone**.

### NumberingZone

Voir la classe [NumberingZone](https://segmonto.github.io/gd/gdZ/NumberingZone/) telle qu'elle est définie par SegmOnto.

>> NumberingZone: is a zone containing the page, the folio, or the document number, with no regard for the mark's origin (scribe, curator, etc). The zone usually is at the top of the page.

Fichier exemple : desvouges_CV05380_19200510_f5.jpg

![Fichier exemple pour illustrer la classe NumberingZone](https://s3.hedgedoc.org/demo/uploads/1255b888-ad68-4d85-8289-094347474a4a.png)

Ici en violet, en haut de la page et centré, une **NumberingZone**.

Une NumberingZone peut être accompagnée d'un RunningTitle.


### PageTitleZone

Voir la classe [TitlePageZone](https://segmonto.github.io/gd/gdZ/TitlePageZone/) telle qu'elle est définie par Segmonto.

>> TitlePageZone: characterises the entire page, rather than a section within a page, that contains for instance headings (chapter title, act or scene number, etc.). It is distinct from other pages and is traditionally the first page of a document, especially in the case of prints. It provides bibliographic or identifying information, such as the title of the work, the production date, the names of the printer(s), publisher(s) and author(s), etc.  [...]

Fichier exemple : bourgey_12148-bpt6k97777550_f5.jpg

![Fichier exemple pour illustrer la classe PageTitleZone](https://s3.hedgedoc.org/demo/uploads/7c8bfb4b-13ae-45d1-9909-83ae7734d992.png)

Ici en bleu marine, une **PageTitleZone** qui encadre les segments de texte constituant une page de titre. En l'occurencee, un titre, un sous-titre, et l'indication du lieu et de la date de la vente aux enchères. 

La segmentation intérieure des PageTitleZone n'intervient pas dans ce niveau de segmentation de la macro-structure, mais pourra faire l'objet d'une annotation plus fine dans le futur. 


### PageTitleZone:Index

Fichier exemple : bienaimé-feuardent_12148-bpt6k97782456_f15.jpg 

![Fichier exemple pour illustrer la classe PageTitleZone:Index](https://s3.hedgedoc.org/demo/uploads/397bdd9c-eaa7-43df-afd6-9a701e5e0591.png)



### MainZone:Head

Voir la classe [HeadingLine](https://segmonto.github.io/gd/gdL/HeadingLine/) telle qu'elle est définie par SegmOnto.

>> HeadingLine: is a line containing any type of heading, which is defined as a string with a distinctive typesetting (font, size, colour, capitalisation…) different than that seen in the body of the text. It is not limited to the header. Rather the HeadingLine indicates the beginning of a new unit, no matter the unit's size, such as a medieval rubric, a speaker's name in a play, or the title of a poem in a collection. It is also used in the TitlePageZone.

Fichier exemple : Lair-Dubreuil_CV10613_19260204_f8.jpg

![Fichier exemple pour illustrer la classe MainZone:Head](https://s3.hedgedoc.org/demo/uploads/f98fb7d9-b8d7-4246-931c-642f43b5f42a.png)

Ici en orange, deux **MainZone:Head** représentant chacune un niveau de titre. L'information contenue dans le premier niveau de titre, donc hiérarchiquement placé au-dessus du second, impacte celui-ci. Les informations contenues dans le premier niveau de titre et le second niveau de titre impactent les entrées de catalogues venant hiérarchiquement après ceux-ci.

### MarginTextZone:ManuscriptAddendum

Cette classe permet d'annoter les segments de texte manuscrits. Le nom de la classe a été repris de la campagne d'annotation menée par le projet COLAF (Inria).

Fichier exemple : bienaimé-feuardent_12148-bpt6k9780628v_f57.jpg

![Fichier exemple pour illustrer la classe MarginTextZone:ManuscriptAddendum](https://s3.hedgedoc.org/demo/uploads/a6c624db-fa95-4cf6-a3fe-3931a03ade69.png)

Ici en violet plusieurs **MarginTextZone** qui signalent la présence de segments de textes manuscrits, clairement distinguables du texte imprimé. Dans les catalogues de vente, ces segments mansucrits sont courts, comme en témoigne l'exemple où une personne a indiqué les prix auxquels ont été vendus les objets dans la marge à droite du texte.

Il arrive que les segments soient plus longs et contiennent plus d'information, sur l'objet-même par exemple, ou indiquent le nom de l'acheteur, mais cela reste rare.

### MarginTextZone:Decoration

Cette classe permet d'annoter les segments de texte manuscrits. Le nom de la classe a été repris de la campagne d'annotation menée par le projet COLAF (Inria).

Fichier exemple : Lair-Dubreuil_CV07148_19220502_f11.jpg

![Fichier exemple pour illustrer la classe MarginTextZone:Decoration](https://s3.hedgedoc.org/demo/uploads/ac2b4e7b-d2d1-4aa5-bbb7-6be6b9c05d21.png)

Ici en bleu violet et en bas de l'image un exemple de **MarginTextZone:Decoration**. Cette classe a généralement peu d'importance sémantique. Elle peut être comme ici une illustration d'un détail d'un objet vendu dans le catalogue, ou simplement être un séparateur contenant des motifs élaborés. Cette classe permet de distinguer les **GraphicZone**, les illustrations -majoritairement celles des objets mis aux enchères-, des éléments décoratifs. 

**MarginTextZone:Decoration** apparaît la majorité du temps dans le corps du texte, et non sur des planches où sont placées les illustrations.

### GraphicZone

Voir la classe [GraphicZone](https://segmonto.github.io/gd/gdZ/GraphicZone/) telle qu'elle est définie par SegmOnto.

>> GraphicZone is a zone containing any type of graphic element, from purely ornamental information to information consubstantial to the text (e.g. full-page paintings, line-fillers, marginal drawings, figures, etc.). Captions, if there are any, is part of this zone, and its text line is labelled HeadingLine. If an image contains text, it is possible to label the lines as DefaultLine.

Fichier exemple : bourgey_12148-bpt6k97781327_f77.jpg

![Fichier exemple pour illustrer la classe GraphicZone](https://s3.hedgedoc.org/demo/uploads/c03d4930-944b-4a39-8737-4553fe00393e.png)

Ici en bleu clair plusieurs **GraphicZone**. Il est important de souligner qu'une illustration peut contenir une ou plusieurs images. Dans l'exemple ci-dessus, des pièces de monnaies sont illustrées par leurs recto et leurs verso. L'image du recto et l'image du verso font donc référence à un seul et même objet, et sont unies par une légende, ici un numéro. On annote donc les deux images comme une seule **GraphicZone**. 

### GraphicZone:Legend

Cette classe permet d'annoter les segments de texte manuscrits. Le nom de la classe a été repris de la campagne d'annotation menée par le projet COLAF (Inria).

Fichier exemple : bourgey_12148-bpt6k97781327_f77.jpg

![Fichier exemple pour illustrer la classe GraphicZone:Legend](https://s3.hedgedoc.org/demo/uploads/c03d4930-944b-4a39-8737-4553fe00393e.png)

Ici en jaune plusieurs **GraphicZone:Legend**. Ces zones permettent de signaler la présence de légendes associées à leurs illustrations. 

Comme précisé dans SegmOnto, il est préférable que **GraphicZone** englobe également la **GraphicZone:Legend**, comme illustré dans l'exemple ci-dessus et ci-dessous.

Fichier exemple : bourgey_12148-bpt6k97782634_f29.jpg

![Fichier exemple pour illustrer la classe GraphicZone:Legend, contenant un nombre conséquent d'illustrations](https://s3.hedgedoc.org/demo/uploads/e4fa632c-178f-4ba1-81e4-c5ecb8d14913.png)


Cependant, des exceptions, telle qu'une légende placée trop loin de son illustration, existent, et il n'est donc pas toujours possible d'imbriquer **GraphicZone:Legend** dans **GraphicZone**. 

### MainZone:Notes

Cette classe permet d'annoter les notes de bas de page. Le nom de la classe a été repris de la campagne d'annotation menée par le projet COLAF (Inria).

Fichier exemple : bienaimé-feuardent_12148-bpt6k9777364k_f11.jpg

![Fichier exemple pour illustrer la glasse MainZone:Notes](https://s3.hedgedoc.org/demo/uploads/89b0585a-ad15-4584-8b5d-020e1fc64fb9.png)

Figurent ici en rouge la note de bas de page tapuscrite ainsi que son appel de note, annotées avec **MainZone:Notes**.

### MainZone:Table

### MainZone:TableHeader

### MainZone:QuireMark

### MainZone:Other

## Annotateur-ices

* Sarah Bénière (Inria)
* Hugo Scheithauer (Inria)