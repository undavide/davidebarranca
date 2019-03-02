---
title: " iBooks Author - Compressione delle immagini\t\t"
url: 905.html
id: 905
comments: false
category:
  - Digital Publishing @it
  - iBooksAuthor @it
date: 2012-04-06 23:07:15
tags:
---

![IBooksAuthor opening](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-opening1.jpg) Se sei seriamente interessato alla qualità delle immagini per l'editoria elettronica, vuoi sapere quello che accade loro quando iBooks Author esporta un file .ibooks: non solo quale profilo ICC è meglio usare, che tipo di file (JPG e PNG), ma anche che genere di conversione, compressione JPG e ridimensionamento viene automaticamente applicato dal software alle immagini che finiscono su iPad. Ho condotto test specifici e alcuni risultati sono piuttosto sorprendenti.  Il formato degli iBook è proprietario, codificato da Apple. Non è uno standard (lamentano i sostenitori degli standard), ma è eccezionalmente versatile e semplice da realizzare, pur producendo risultati tutt'altro che amatoriali. Eppure iBooks Author, l'applicazione gratuita di Apple per comporre iBooks, non è priva di bug: alcuni effetti sono poco intuitivi (vedi il mio post precedente sulle [immagini a pieno schermo e senza bordi](http://localhost:8888/2012/04/ibooks-author-immagini-pieno-schermo-fullscreen-senza-bordi/?lang=it "iBooks Author – Immagini a pieno schermo")), ma siamo solamente alla versione 1.1 e possiamo aspettarci numerosi ed interessanti aggiornamenti.

### Formati di file, profili ICC e dimensioni

Per farla breve: usa JPG o PNG (che permette di usare la trasparenza), converti tutti in sRGB e tieni conto che praticamente tutte le immagini (salvo alcune eccezioni di cui ti dirò a breve) verranno ridimensionate a 2048 x 1536 pixel, la dimensione del nuovo iPad con retina display. Questo è quello che [Apple raccomanda](http://support.apple.com/kb/PH2838).

### Come ispezionare file .iba e .ibook

[![iBook Assets](http://localhost:8888/wp-content/uploads/2012/04/iBookAssets-132x300.png "iBook Assets")](http://localhost:8888/wp-content/uploads/2012/04/iBookAssets.png)iBooks Author salva i tuoi progetti come file .iba - mentre quando esporti per iPad viene prodotto un file .ibooks. Il pacchetto .iba contiene una copia di tutti gli elementi originali che hai importato, mentre nel pacchetto .ibooks le cose sono trasformate: ridimensionate, convertite, compresse. Entrambi possono essere ispezionati in questo modo:

1.  Duplica il file .iba oppure .ibooks (giusto per sicurezza)
    
2.  Rinominali in .zip
    
3.  Scompattali in una cartella (attenzione: OSX non porta a termine questa operazione con un doppio click, io ho dovuto usare Stuffit Expander)
    

Ecco fatto. Come vedi, ottieni una struttura di cartelle che contengono file xhtml, css e tutti gli elementi necessari, comprese le immagini.

### Comparazione: dimensione delle immagini

Per tutti i test che seguono, ho usato un'immagine di 2048 x 1536 pixel, salvati come JPG massima qualità, in sRGB. Quando importi un'immagine in iBooks Author puoi ridimensionarla e spostarla per costruire il design della pagina che hai in mente, e decidere se permetterne la visione a pieno schermo (per i dettagli vedi [il mio post precedente](http://localhost:8888/2012/04/ibooks-author-immagini-pieno-schermo-fullscreen-senza-bordi/?lang=it "iBooks Author – Immagini a pieno schermo")). Se l'immagine non è impostata per andare a pieno schermo, il pacchetto .ibooks conterrà una versione ridimensionata dell'immagine originale, corrispondente all'effettiva dimensione sulla pagina. Ovvero, se nel libro su iPad l'immagine (che originariamente è 2048 x 1536 px) occupa uno spazio di 300 x 400 px, verrà salvata nel file .ibooks come un JPG di 300 x 400 px. Diversamente, se l'immagine è parte di un widget (_Immagine_ oppure _Galleria_) le cose si complicano:

*   Se hanno un contorno (il default è una linea continua di 1 pt) verranno ridimensionate a **2040 x 1530** px.
*   Se sono prive di contorno, la dimensione è di **2048 x 1536** px.

![](http://localhost:8888/wp-content/uploads/2012/04/InspectorFullscreen1.png "InspectorFullscreen.png")Per cui applicare un contorno diminuisce l'effettiva dimensione delle immagini nel file .ibooks. Nota bene: **l'opzione _Full-screen only_ non ha nessun effetto**, sull'iPad un widget immagine andrà comunque a pieno schermo quindi ti consiglio di tenerla disattivata. Inoltre un widget _Immagine_ andrà al massimo ad una dimensione di **2008 x 1319** pixel ([vedi qui i dettagli](http://localhost:8888/2012/04/ibooks-author-immagini-pieno-schermo-fullscreen-senza-bordi/?lang=it "iBooks Author – Immagini a pieno schermo")), mentre  per le _Gallerie_ è consentita la visualizzazione a pieno schermo e _senza bordi_ (2048 x 1536). Come ultima opzione, se è parte di un widget _Immagine Interattiva_ (ovvero non solo a pieno schermo, ma anche ingrandibile ed esplorabile), la dimensione finale dipenderà dalla percentuale di zoom - cioè puoi dare in pasto ad iBooks Author un file 5000 x 5000 px ed aspettarti di trovarlo negli assets del file .ibooks..

### Comparazione: profili

\[caption id="attachment_866" align="alignleft" width="180" caption="Comparazione del gamut di iPad1/iPad2 ed il nuovo iPad © C. David Tobie"\][![iPad1/iPad2 over iPad3](http://localhost:8888/wp-content/uploads/2012/04/ipad1-2overiPad3-300x290.jpg "iPad1/iPad2 over iPad3")](http://cdtobie.wordpress.com/2012/03/21/more-answers-about-the-new-ipad-and-color/)\[/caption\] Se ti interessi di Gestione Colore e non conosci ancora il blog di [C. David Tobie](http://cdtobie.wordpress.com/) (è _Global Product Technology Manager - Imaging Color Solutions, [Datacolor inc.](http://www.datacolor.com/)_) ti suggerisco di leggere almeno la sua serie di post sull'iPad ([More answers about the new iPad and Color](http://cdtobie.wordpress.com/2012/03/21/more-answers-about-the-new-ipad-and-color/) contiene i link a tutti gli articoli). E' molto competente, e puoi leggere contenuti originali che non troverai altrove. I miei test hanno confermato che, sebbene entrambi i pacchetti .iba e .ibooks contengano immagini col medesimo tag sRGB (come l'originale), il file proveniente dall'esportazione in .ibooks appare visibilmente più chiaro. Segno che qualche tipo di conversione dietro le quinte viene effettuata dal software Apple. Ho misurato un [ColorChecker sintetico](http://www.babelcolor.com/main_level/ColorChecker.htm#ColorChecker_images), con questi risultati:

*   circa 4 punti nella _b_ di _Lab_ nei blu e ciano (l'originale è più satura)
*   circa 1 punto nella a di Lab per gialli e arancioni
*   uno spostamento verso il verde nella base (per il JPG del file .ibooks)

![ColorChecker sRGB Comparison](http://localhost:8888/wp-content/uploads/2012/04/ColorChecker_sRGB_COMPARISON.jpg "ColorChecker sRGB Comparison")A sinistra l'originale, a destra il file processato nel .ibooks - la differenza è visivamente trascurabile. Niente da cui farsi ossessionare, anche se il ColorChecker di per sé non è molto significativo: con fotografie _vere_ la versione .ibooks appare costantemente più chiara.

![iBooksAuthor Profile Comparison](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-ProfileComparison.jpg "iBooksAuthor Profile Comparison")

Ancora: a sinistra l'originale, a destra il file proveniente dal pacchetto .ibooks - ora si vede la differenza! Per simulare l'effetto, aggiungi un livello di regolazione Curve all'originale ed alza il punto centrale della curva composita di circa +20 punti (input: 127, output: 147), questo è quello che vedresti più o meno nel file .ibooks.

### Comparazione: compressione JPG

Ho importato in iBooks Author un JPG salvato per il wed in Photoshop CS6, massima qualità: quindi ho confrontato il JPG proveniente dagli assets del file .iba (identico all'originale) con quello proveniente dal file .ibooks. Come puoi vedere, gli artefatti di compressione sono molto evidenti nel file finale .ibooks: e non è strano, perché se il JPG del .iba pesa 1.2MB, quello del .ibooks è nemmeno la metà, 560KB. Che tipo di compressione JPG è stata usata? Per scoprirlo, ho scritto un piccolo Javascript per Photoshop che salva per il web 101 JPG dallo stesso originale, con _Qualità_ da 0 a 100. Credo di poter identificare la compressione .ibooks all'incirca poco sotto il livello 50: non è identica - ad esempio nella versione da .ibook c'è molto più dithering attorno ai bordi, il che mi fa pensare che sia stato usato qualche tipo di maschera per variare dinamicamente il livello di compressione - come si può anche fare in Photoshop con un canale alfa costruito appositamente, determinando il range massimo/minimo. \[caption id="attachment_876" align="aligncenter" width="570" caption="Sinistra, JPG importato originariamente (qualità 100); centro, JPG qualità 48; destra, il JPG contenuto nel file .ibooks (compressione JPG sconosciuta). Luminosità normalizzata. Clicca sull'immagine per ingrandire."\][![iBooksAuthor - JPG Compression Comparison](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison.jpg "iBooksAuthor - JPG Compression Comparison")](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison.jpg)\[/caption\] Certo che, ingrandendo, il JPG del .ibooks sembra Frankenstein comparato all'immacolata bellezza dell'originale a massima qualità.

### Conclusioni

Sii felice e produci i tuoi iBooks. Non usare niente di più di JPG a massima qualità, sRGB,  2048 x 1536 px (a meno che tu non preveda di usare il widget _Immagine Interattiva_, che può richiedere risoluzioni più alte). Non ti preoccupare delle conversioni occulte, non ci puoi fare niente - verifica le immagini su iPad ed eventualmente ricorreggile in Photoshop di conseguenza, se non ti piacciono. Si, certo: verranno ignobilmente compresse, e se le ispezioni al 400% vedrai un mare di orribili artefatti JPG, ma sai una cosa? Su un iPad con retina display non sono così evidenti (tantomeno su iPad di vecchie generazioni), possiamo vivere tranquilli. E se per caso ti va di scompattare il file .ibooks, sostituire gli assets JPG con file di maggiore qualità, ricomprimere il pachetto e spedirlo ad Apple per l'approvazione sullo store online, be', fammi sapere come va a finire! Non voglio essere il primo a fare un _hack_ degli iBooks, rischiando di far arrabbiare la grande A :-)