# DataCatalogue 🐱 — Guide d'annotation

**DataCatalogue** (2021-2024) est un projet de recherche conjointement mené par l'équipe **ALMAnaCH** (Inria), la **Bibliothèque nationale de France** (BnF), et l'**Institut National d'Histoire de l'Art** (INHA). 

Ce guide présente le schéma d'annotation utilisé pour annoter plusieurs collections de catalogues de vente provenant de la BnF et l'INHA. Ce corpus annoté sera ensuite utilisé pour affiner un modèle YOLOv8 qui servira à analyser automatiquement la mise en page des catalogues de vente. 

Cette tâche automatique résultera dans la segmentation de la **macro-structure des catalogues de vente**, permettant ensuite une analyse fine de leur contenu. 


## Présentation du corpus d'entraînement
Le corpus d'entraînement a été assemblé à partir de 8 collections de catalogues de vente provenant de la BnF et l'INHA : 

#### BnF
* Bienaimé-Feuardent (BnF)
* Bourgey (BnF)
* Dubois (BnF)
* Naville (BnF)
* Rollin (BnF)
* XVIIIe siècle (BnF)  

#### INHA
* Desvouges (INHA)
* Lair-Dubreuil (INHA)  

### Méthodologie
Deux pages aléatoires ont été sélectionnées dans chaque catalogue, téléchargés depuis Gallica et la bibliothèque numérique de l'INHA, et ont été ajoutées à la plateforme d'annotation Roboflow.  

Selon les résultats de l'affinage du modèle YOLOv8, nous nous attendons à une sur-représentation des catalogues provenant de certaines collections de la BnF pour lesquels la masse documentaire est plus importante, ce qui pourrait impacter la segmentation d'autres collections de catalogue. Le _dataset_ devra donc peut-être être équilibré dans le futur. 



## Classes utilisées pour l'annotation
Le schéma d'annotation utilisé dans le cadre du projet DataCatalogue se base en majorité sur l'ontologie [SegmOnto](https://segmonto.github.io/), et sur la campagne d'annotation du projet [COLaF](https://colaf.huma-num.fr/) (Inria) pour le dataset [LADaS](https://github.com/DEFI-COLaF/LADaS/blob/main/AnnotationGuide.md). Certaines classes ont été adaptées aux cas d'usages définis par les catalogues de vente et à la modélisation TEI employée pour représenter ces documents, et d'autres ont été ajoutées.



### DigitizationArtefactZone
La classe `DigitizationArtefactZone` contient toutes les mentions résultant du processus de numérisation.  

Fichier exemple : bourgey_12148-bpt6k9778375p_f1.jpg  

![Fichier exemple DigitizationArtefactZone](/images_annotationguide/bourgey_12148-bpt6k9778375p_f1.jpg)  

Ici, en rose, la mention de Gallica (BnF).  



### GraphicZone
Une `GraphicZone` contient une illustration, généralement associée à une notice. Si l'image est accompagnée d'une **légende**, celle-ci est incluse dans la `GraphicZone` et étiquetée avec la classe `GraphicZone:Head`.  

Fichier exemple : bourgey_12148-bpt6k97781327_f77.jpg  

![Fichier exemple GraphicZone](/images_annotationguide/bourgey_12148-bpt6k97781327_f77.jpg)  

Ici, en bleu clair, plusieurs `GraphicZone`. Il est important de souligner qu'**une illustration peut contenir une ou plusieurs images**. Dans l'exemple ci-dessus, des pièces de monnaies sont illustrées par leur _recto_ et leur _verso_. Les images du _recto_ et du _verso_ font donc référence à **un seul et même objet**, et sont unies par une légende, ici un numéro. On annote donc les deux images comme une seule `GraphicZone`.  



### GraphicZone:Decoration
La classe `GraphicZone:Decoration` signale la présence d'un **élément graphique à but décoratif**, en opposition à une illustration signalée par `GraphicZone` qui correspond à l'image associée à une notice. Les `GraphicZone:Decoration` apparaissent la plupart du temps **dans le corps du texte** et non sur les planches d'illustrations.  

Fichier exemple : desvouges_CV03675_19170430_f59.jpg  

![Fichier exemple GraphicZone:Decoration](/images_annotationguide/desvouges_CV03675_19170430_f59.jpg)  

Ici, en orange, une décoration précédant un titre.  



### GraphicZone:Head
La classe `GraphicZone:Head` permet d'étiqueter la légende d'une illustration lorsque celle-ci correspond à une **numérotation** uniquement. Elle figure, autant que possible, à l'intérieur de la `GraphicZone` correspondante.  

Fichier exemple : bourgey_12148-bpt6k97782634_f29.jpg  

![Fichier exemple GraphicZone:Head](/images_annotationguide/bourgey_12148-bpt6k97782634_f29.jpg)  



### GraphicZone:Legend
La classe `GraphicZone:Legend` correspond à la **légende associée à une illustration**. Tout comme la classe `GraphicZone:Head`, la classe `GraphicZone:Legend` se place au sein de la `GraphicZone`. Cependant, des exceptions telles qu'**une légende placée trop loin de son illustration** existent, et il n'est donc pas toujours possible d'imbriquer `GraphicZone:Legend` dans `GraphicZone`. Lorsque la légende consiste uniquement en une numérotation, on utilisera la classe `GraphicZone:Head`.   

Fichier exemple : Lair-Dubreuil_CV05028_19200202_f11.jpg  

![Fichier exemple GraphicZone:Legend](/images_annotationguide/Lair-Dubreuil_CV05028_19200202_f11.jpg)  

Ici, la légende encadrée en marron contient, en plus du numéro de la notice correpondante, du texte : "Portrait de Mlle Zucchi, de l'Opéra".  



### MainZone:Entry et MainZone:Entry#Continued
La classe `MainZone:Entry` correspond à une **notice de catalogue**. Lorsque la notice continue sur la page suivante, la seconde partie est signalée avec la classe `MainZone:Entry#Continued`.  

Fichier exemple : bourgey_12148-bpt6k9777791v_f27.jpg  

![Fichier exemple MainZone:Entry](/images_annotationguide/bourgey_12148-bpt6k9777791v_f27.jpg)  

En violet, les différentes notices correspondant à l'entrée signalée par le titre "Meubles anciens".  


Fichier exemple : rollin_12148-bpt6k9777787z_f9.jpg  

![Fichier exemple MainZone:Entry#Continued](/images_annotationguide/rollin_12148-bpt6k9777787z_f9.jpg)  

Ici, en rose clair, la suite d'une notice qui commence sur la page précédente.  



### MainZone:Form
La classe `MainZone:Form` met en évidence un **formulaire**, une zone à remplir.  


Fichier exemple : desvouges_CV07685_19221216_f3.jpg  

![Fichier exemple MainZone:Form](/images_annotationguide/desvouges_CV07685_19221216_f3.jpg)  

Ici, en rose saumon en bas de la page, un bulletin de souscription à remplir.  



### MainZone:Head
La classe `MainZone:Head` signale un **niveau de titre**. Elle correspond à la classe [`HeadingLine`](https://segmonto.github.io/gd/gdL/HeadingLine/) telle qu'elle est définie par SegmOnto :  

>> **HeadingLine:** is a line containing any type of heading, which is defined as a string with a distinctive typesetting (font, size, colour, capitalisation...) different than that seen in the body of the text. It is not limited to the header. Rather the `HeadingLine` indicates the beginning of a new unit, no matter the unit's size, such as a medieval rubric, a speaker's name in a play, or the title of a poem in a collection. It is also used in the `TitlePageZone`.  

Fichier exemple : Lair-Dubreuil_CV10613_19260204_f8.jpg  

![Fichier exemple MainZone:Head](/images_annotationguide/Lair-Dubreuil_CV10613_19260204_f8.jpg)  

Ici, en orange, deux `MainZone:Head` représentant chacune un niveau de titre. L'information contenue dans le premier niveau de titre, donc hiérarchiquement placé au-dessus du second, impacte celui-ci. Les informations contenues dans le premier et le second niveau de titre impactent les notices de catalogue venant hiérarchiquement après ceux-ci.  



### MainZone:Other
La classe `MainZone:Other` est utilisée lorsqu'**aucune autre classe ne correspond**.  

Fichier exemple : desvouges_CV07850_19230212_f17.jpg  

![Fichier exemple MainZone:Other](/images_annotationguide/desvouges_CV07850_19230212_f17.jpg)  

Ici, une mention d'impression qui ne figure pas sur une page de titre.  



### MainZone:P et MainZone:P#Continued
La classe `MainZone:P` correspond à un **paragraphe de texte**. Lorsque le paragraphe continue sur la page suivante, la fin du paragraphe sera annotée avec la classe `MainZone:P#Continued`.  

Fichier exemple : bienaime-feuardent_12148-bpt6k9778158g_f14.jpg  

![Fichier exemple MainZone:P](/images_annotationguide/bienaime-feuardent_12148-bpt6k9778158g_f14.jpg)  

Ici, en bleu, figurent les paragraphes correspondant aux conditions de la vente.  



### MainZone:P@CatalogueDesc
La classe `MainZone:P@CatalogueDesc` correspond à une zone de texte habituellement située juste après un niveau de titre et précédant les notices. Elle donne des **informations sur un ensemble de notices**, régies elles-mêmes par un niveau de titre. On y trouve **informations de chronologie, de bibliographie, des commentaires sur la vente**, etc.  

Fichier exemple : rollin_12148-bpt6k9780581w_f122.jpg  

![Fichier exemple MainZone:CatalogueDesc](/images_annotationguide/rollin_12148-bpt6k9780581w_f122.jpg)  

Ici, les `MainZone:P@CatalogueDesc` figurent en rouge, tandis que les niveaux de titres sont en orange. Dans cet exemple, elles donnent des informations temporelles.  



### MarginTextZone:ManuscriptAddendum
La classe `MarginTextZone:ManuscriptAddendum` signale les **annotations manuscrites** inscrites au moment de la vente ou _a posteriori_. Elles peuvent apparaître sur tous types de pages. Cette classe a été reprise de la campagne d'annotation du projet COLaF (Inria) pour le jeu de données [LADaS](https://github.com/DEFI-COLaF/LADaS).  

Fichier exemple : bienaime-feuardent_12148-bpt6k9780628v_f57.jpg  
![Fichier exemple MarginTextZone:ManuscriptAddendum](/images_annotationguide/bienaime-feuardent_12148-bpt6k9780628v_f57.jpg)  

Ici, les annotations manuscrites apparaissent en rose vif. Il arrive que les segments soient plus longs et contiennent plus d'information, sur l'objet-même par exemple, ou indiquent le nom de l'acheteur, mais cela reste rare.  



### MarginTextZone:Notes et MarginTextZone:Notes#Continued
La classe `MarginTextZone:Notes` permet d'annoter les notes de bas de page. Lorsque la note continue sur la page suivante, elle sera signalée par l'étiquette `MarginTextZone:Notes#Continued`. Ces classes ont été reprises de la campagne d'annotation du projet COLaF (Inria) pour le jeu de données [LADaS](https://github.com/DEFI-COLaF/LADaS).  

Fichier exemple : dubois_12148-bpt6k9629016g_f75.jpg  

![Fichier exemple MarginTextZone:Notes](/images_annotationguide/dubois_12148-bpt6k9629016g_f75.jpg)  

Ici, en bleu cyan, les notes de bas de page. Pour l'instant, les appels de notes font partie de la classe dans laquelle ils se trouvent, ici, des `MainZone:Entry`.  



### NumberingZone
La classe `NumberingZone` permet d'étiqueter les **numéros de pages**. Elle est définie par [SegmOnto](https://segmonto.github.io/gd/gdZ/NumberingZone/) :  

>> **NumberingZone:* is a zone containing the page, the folio, or the document number, with no regard for the mark's origin (scribe, curator, etc). The zone usually is at the top of the page.  

Fichier exemple : desvouges_CV05380_19200510_f5.jpg  

![Fichier exemple NumberingZone](/images_annotationguide/desvouges_CV05380_19200510_f5.jpg)  

Ici en violet, en haut de la page et centré, une `NumberingZone`. Une `NumberingZone` peut être accompagnée d'une `RunningTitleZone`.  



### PageTitleZone
La classe `PageTitleZone` désigne l'ensemble d'une **page de titre**. Cette classe contient les segments qui la composent tels qu'un titre, un sous-titre, l'indication du lieu et de la date de la vente par exemple. La segmentation interne des `PageTitleZone` fera l'objet d'une annotation plus fine dans le future.  

La classe équivalente [`TitlePageZone`](https://segmonto.github.io/gd/gdZ/TitlePageZone/) définit par SegmOnto :  

>> **TitlePageZone:** characterises the entire page, rather than a section within a page, that contains for instance headings (chapter title, act or scnee number, etc.). It is distinct from other pages and is traditionally the first page of a document, especially in the case of prints. It provides bibliographic or identifying information, such as the title of the work, the production date, the names of the printer(s), publisher(s) and author(s), etc. [...]  

Fichier exemple : bourgey_12148-bpt6k97777550_f5.jpg  

![Fichier exemple PageTitleZone](/images_annotationguide/bourgey_12148-bpt6k97777550_f5.jpg)  

Ici, en bleu marine, une `PageTitleZone` qui encadre les segments de texte constituant une page de titre.



### PageTitleZone:Index
La classe `PageTitleZone:Index` contient chaque ligne d'une **table des matières** ou d'un **sommaire**.  

Fichier exemple : bienaime-feuardent_12148-bpt6k97782456_f15.jpg  

![Fichier exemple PageTitleZone:Index](/images_annotationguide/bienaime-feuardent_12148-bpt6k97782456_f15.jpg)  

Ici, les `PageTitleZone:Index` sont signalées en bleu foncé.  



### QuireMarkZone
Une `QuireMarkZone` contient un **repère** relatif à la structure de l'ouvrage, à l'exception des numéros de page et de folio. Elle correspond à la classe [`QuireMarksZone`](https://segmonto.github.io/gd/gdZ/QuireMarksZone/) définie par SegmOnto :  

>> **QuireMarksZone:** is a zone containing a quire signature (e.g. _a ii_), catchword, or any kind of element relative to the material organisation of the source, with the exclusion of page, folio, or item numbers. The zone usually is at the bottom of the page.  

Fichier exemple : naville_12148-bpt6k9779820r_f55.jpg  

![Fichier exemple QuireMarkZone](/images_annotationguide/naville_12148-bpt6k9779820r_f55.jpg)  

Ici, en bleu, la `QuireMarkZone` contient le repère "6*".  



### RunningTitleZone
La classe `RunningTitleZone` contient un **titre courant**. Il apparaît souvent en haut de la page. La classe [`RunningTitleZone`](https://segmonto.github.io/gd/gdZ/RunningTitleZone/) est définie par SegmOnto comme :  

>> **RunningTitleZone:** is a zone containing a running title, traditionally at the top of the page or of the double page. It can be the title (or the abbreviated title) of a document or of the current section.  

Fichier exemple : bienaime-feuardent_12148-bpt6k9777643f_f60.jpg  

![Fichier exemple RunningTitleZone](/images_annotationguide/bienaime-feuardent_12148-bpt6k9777643f_f60.jpg)  

Ici, en rouge, en haut de la page et centré, un exemple d'un segment de texte correspondant à un titre courant étiqueté `RunningTitleZone`.  



### StampZone et StampZone:Sticker
La classe `StampZone` signale un **tampon**, notamment celui du service de conservation (bibliothèque ou archives par exemple), et `StampZone:Sticker` l'utilisation d'un sticker dans le même but. Elle est définie par [SegmOnto](https://segmonto.github.io/gd/gdZ/StampZone/) comme :   

>> **StampZone:** is a zone containing a stamp, be it a library stamp or a mark from a postal service.  

Fichier exemple : bienaime-feuardent_12148-bpt6k9779231g_f58.jpg  

![Fichier exemple StampZone](/images_annotationguide/bienaime-feuardent_12148-bpt6k9779231g_f58.jpg)  

Ici, en vert, le tampon de la Bibliothèque nationale.  


Fichier exemple : desvouges_CV09509_19241208_f1.jpg  

![Fichier exemple StampZone:Sticker](/images_annotationguide/desvouges_CV09509_19241208_f1.jpg)  

En bordeaux, le sticker de la Bibliothèque d'art et d'archéologie Jacques Doucet.  



### TableZone
La classe `TableZone` indique la présence d'un **tableau**. La classe [`TableZone`](https://segmonto.github.io/gd/gdZ/TableZone/) est définie par SegmOnto :  

>> **TableZone:** is a zone containing a table of any kind. The table can be clearly drawn (with rows and columns) or not. The tables of contents are in the vast majority of cases not tables.  

Fichier exemple : naville_12148-bpt6k9780404r_f82.jpg  

![Fichier exemple TableZone](/images_annotationguide/naville_12148-bpt6k9780404r_f82.jpg)  

Ici, les notices sont présentées à l'intérieur d'un tableau, signalé en bleu.  



## Annotateur-ices
* Sarah Bénière (Inria)
* Hugo Scheithauer (Inria)