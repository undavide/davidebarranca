---
title: " Sviluppare per Creative Suite e Photoshop\t\t"
url: 531.html
id: 531
comments: false
category:
  - Coding @it
  - Photoshop @it
date: 2011-12-12 23:11:31
tags:
---

Dopo aver letto e condiviso le [riflessioni di Gabe Harbs](http://in-tools.com/article/thoughts-on-extending-the-creative-suite/ "Thoughts on Extending the Creative Suite") (noto sviluppatore InDesign) sull'argomento, mi piacerebbe aggiungere qui un paio di considerazioni personali, incentrate su Photoshop.

Estensioni Creative Suite (CS)
------------------------------

Qualche informazione per inquadrare l'argomento: sviluppare per CS oggi è incredibilmente più agevole di quanto non potessimo immaginare solo qualche anno fa. La tecnologia di Macromedia (acquisita da Adobe) è stata inserita nella Creative Suite, si è accoppiata con le Flex SDK, per maturare finalmente in un prodotto che si chiama Extension Builder (un plugin di Flash Builder / Eclipse), basato sulle nuove CS SDK. Siamo ora in grado di scrivere pannelli che:

1.  si fondono quasi perfettamente con la UI (interfaccia utente) delle applicazioni;
    
2.  guidano Photoshop, InDesign ecc. esattamente come lo scripting;
    
3.  sono basati su un linguaggio OOP come ActionScript 3, e su servizi Flex per il data management.
    
4.  usano componenti Flash per il design della UI.
    

Strumenti di incredibile potenza. In più, il [team CS SDK](http://blogs.adobe.com/cssdk/ "Adobe CS SDK official blog") guidato dal PM [Gabriel Tavridis](http://twitter.com/#!/gtavridis "Gabriel Tavridis on Twitter") sembra essere veramente motivato nel far crescere a ritmo sostenuto la piattaforma; ascoltano attentamente gli sviluppatori, sia per quel che riguarda l'aspetto tecnico che strategico e commerciale; sebbene io abbia già menzionato altrove i miei [dubbi su Adobe come azienda](http://localhost:8888/2011/11/adobe-al-bivio-quali-cambiamenti-ci-aspettano/?lang=it "Adobe al bivio"), questo sembra un caso davvero speciale. Ovviamente non tutto è rose e fiori: da quando (con la CS4) è stato introdotto, il supporto ai pannelli Flash è andato migliorando (forse perché non poteva peggiorare :-) ), però il debug a distanza sui clienti è sempre un lavoraccio.

*   Su Mac, problemi apparentemente irrilevanti (Font di Sistema, preferenze corrotte) portano a pannelli senza contenuto.
    
*   Su PC (da qui in avanti mi riferisco a Photoshop come l'applicazione host per le estensioni CS), lo stesso script lanciato _tout-court_ o incorporato in un pannello, talvolta [si comporta in modo diverso](http://www.ps-scripts.com/bb/viewtopic.php?f=9&t=4489&sid=a77983ed5a8f6f8aebe91ed7790229c4 "PS-Scripts forum").
    
*   Adobe Extension Manager sembra essere particolarmente ostile agli utenti meno esperti - la mia richiesta di supporto #1 riguarda l'installazione via AEM. I [recenti problemi con OSX Lion](http://localhost:8888/2011/11/problemi-con-adobe-extension-manager-ed-osx-lion/?lang=it "Problemi con Adobe Extension Manager ed OSX Lion"), fortunatamente risolti con un upgrade ufficiale a Dicembre 2011, confermano che di spazio per migliorare ce n'è in abbondanza.
    

Detto ciò, so per certo che il team sta attualmente lavorando per risolvere questi problemi, per far restare le CS SDK una piattaforma nella quale sia vantaggioso e piacevole lavorare.

Tre strade
----------

Uno sviluppatore che voglia scrivere estensioni per Creative Suite può seguire tre vie (che possono anche incrociarsi):

1.  C++
    
2.  Scripting (multi-piattaforma: JavaScript)
    
3.  CS SDK
    

La prima è per programmatori hardcore (cioè non il sottoscritto). Ma dato che il mio caro amico [Marco Olivotto](http://www.marcoolivotto.com "Marco Olivotto") ha in cantiere un plugin per Photoshop, ho sentito da lui una delle più ampie raccolte di maledizioni rimostranze della storia, perlopiù rivolte alla documentazione: oscura, mancante, fuorviante, obsoleta, ecc. ecc. Per quanto JS possa essere usato sia come linguaggio procedurale (e veloce), oppure come uno strumento per costruire sistemi più complessi, personalmente trovo che le CS SDK ed un linguaggio più fortemente Object Oriented come Actionscript si adattino meglio alle esigenze di sviluppatori (come il sottoscritto) mediamente abili sia nella programmazione che nella gestione del Photoshop DOM (Document Object Model). Segui un corso su Java come il meraviglioso, gratuito, introduttivo ["Programming Methodology" della Stanford University](http://see.stanford.edu/see/courseinfo.aspx?coll=824a47e1-135f-4508-a5aa-866adcae1111 "Stanford University - Programming methodology") (tenuto da Mehram Sahami: vulcanico!) e sei pronto per cominciare a sviluppare estensioni con AS, mxml e CS SDK.

Estendere Photoshop
-------------------

Personalmente, vedo almeno un paio di aree nelle quali potrebbe evolvere in modo interessante l'estensibilità delle applicazioni CS, ed in particolare Photoshop.

### Integrazione

Semplificando, il CS SDK mette a disposizione (tra l'altro) gli strumenti per accedere al DOM di Photoshop da ActionScript. Ci sono però un paio di problemi:

1.  Alcune funzioni non sono tradotte in AS. Ad esempio, il metodo _.suspendHistory()_ di Photoshop esiste in AS (mutuato da JS) ma non funziona. Almeno: non funziona _ancora_.
    
2.  Questione più cruciale: lo sviluppo delle CS SDK è _slegato_ dall'evoluzione dello scripting delle varie applicazioni CS.
    

La comunità PS-Scripts raccoglie continuamente[ feature requests](http://www.ps-scripts.com/bb/viewtopic.php?f=36&t=3518&sid=986a4d430efdc3de891785c10bbd01a5 "PS-Scripts features wishlist") per Photoshop, anche se il supporto allo scripting non pare essere molto in alto nella lista delle priorità per il team di sviluppo: l'accesso ad una buona parte di strumenti è al di fuori del DOM e richiede codice Action Manager (non proprio user-friendly). Altre opzioni come l'area media di campionatura del contagocce o la posizione del cursore tanto per citarne un paio (vedi il mio progetto [Power Info Palette](http://blog.rbg.bigano.com/2011/05/22/work-in-progress-power-info-palette/ "Power Info Palette")), sono totalmente al di fuori della nostra portata: informazioni che non possono essere raggiunte, punto.

> Lo so che non è semplice, ma a mio modo di vedere sarebbe _molto più produttivo_ se il team CS SDK potesse collaborare con i team delle varie applicazioni (PS, ID, ecc) per sviluppare e integrare più strettamente a livello di ActionScript le nuove (e vecchie) features che gli utenti richiedono. Per dirla in altri termini, ancora più chiaramente: se Adobe ha intenzione di promuovere il CS SDK come la via preferenziale per estendere la Creative Suite, dovrebbe dare al team la possibilità di _guidare_ l'evoluzione del supporto allo scripting per ogni applicazione - non solo consentire di incorporare (e tradurre in AS) quel che già c'è, cosa che potremmo dare per scontata.

So bene che le estensioni sono un grande passo in avanti di per sé - ciononostante credo che rendere il team di Tavridis il punto di riferimento per gli sviluppatori, anche per quel che riguarda l'integrazione con le applicazioni, sia un modo certamente migliore di sfruttarne le potenzialità sul lungo periodo.

### Accesso a basso livello

Questo è uno dei miei chiodi fissi. Sviluppando per InDesign (del quale ho scarsissima esperienza) è possibile manipolare un documento e ogni tipo di oggetto che contiene - anche, da quel che so, il testo contenuto in un frame. Essendo InDesign un software che (semplificando molto) è fatto per manipolare testo, uno sviluppatore ha quindi accesso agli elementi di base: i caratteri. Traducendo per Photoshop, è come se potessimo avere accesso agli elementi costitutivi di un'immagine, i suoi atomi, cioè: i pixel. Purtroppo no. Possiamo duplicare un documento, un livello, applicare filtri ad una selezione o un canale - ma un livello bitmap in PS non può essere direttamente accessibile alle estensioni come una BitmapImage ActionScript. L'unica alternativa è:

1.  salvare una copia temporanea dell'immagine su disco;
    
2.  farla caricare dal pannello (ed elaborarla);
    
3.  salvarla di nuovo su disco;
    
4.  caricarla in PS e incollarla nel documento originale come un nuovo livello.
    

Un disastro. Sapevo che un passaggio diretto tra Photoshop ed una "estensione" (nelle due direzioni, come un JPG a livelli uniti) è permesso - dalle Photoshop Touch SDK (_stranamente scomparse dall'Adobe Devnet?!_). Ovvero, le applicazioni Touch che tanto Adobe sembra spingere ora. Sapevo anche che [Daniel Koestler](http://blogs.adobe.com/koestler/2011/05/using-the-photoshop-touch-sdk-creating-a-project.html "Daniel Koestler Adobe blog") e [Renaun Erickson](http://renaun.com/blog/2011/04/photoshop-touch-sdk-contains-an-actionscript-3-library-too/ "Renaun Erickson blog") hanno scritto le API in Actionscript (nelle Touch SDK, che ho scaricato in Maggio 2011) - Mi chiedo, sperandolo disperatamente, se questo piccolo passagio sarà permesso prima o poi anche dalle CS SDK.

> Lasciare che un'estensione abbia _accesso diretto ad un livello in Photoshop come BitmapImage_ sarebbe eccezionale: implicherebbe la possibilità di scrivere veri plugin di Digital Image Processing, con UI in Flash, nessun problema di memory management, usando kernel binari di PixelBender (il cui codice è quindi protetto), e tutti gli strumenti e shader già implementati in Actionscript. Certo, le performance rispetto ad un plugin C++ sarebbero inferiori ma... che importa.

Pensavo di essere l'unico a richiedere questa feature, ma dopo aver ricevuto un paio di email da altri sviluppatori, mi sono deciso: Adobe, per piacere... ;-)

Il futuro dell'estensibilità per Creative Suite
-----------------------------------------------

Mentre ragionavo su questo post, ho scritto a Gabriel Tavridis di alcuni dubbi che mi sono sorti dopo aver letto della [faccenda di Flash e Flex](http://blogs.adobe.com/flex/2011/11/your-questions-about-flex.html "Adobe Flex blog"). E' perfettamente chiaro che, sebbene a breve Flex non possa essere rimpiazzato da HTML5, l'interesse sul lungo periodo di Adobe per questa tecnologia è forte. Dunque, qual è la roadmap per le estensioni CS? Domandavo. Gabriel mi ha risposto molto gentilmente che avrebbe scritto in merito un post nel blog ufficiale - [ora online](http://blogs.adobe.com/cssdk/2011/12/cs_extensibility_and_flex_next.html "CS SDK future") e... ti suggerisco di leggerlo! Concludendo, non importa quale tecnologia useremo negli anni a venire, è importante però che Adobe sia trasparente nei confronti dei suoi utenti: come sviluppatore, noto con piacere che da questo punto di vista il team CS SDK è all'avanguardia.