---
id: 531
title: Sviluppare per Creative Suite e Photoshop
date: 2011-12-12T23:11:31+01:00
author: Davide Barranca
excerpt: 'Considerazioni personali sullo sviluppo di estensioni per Creative Suite - Photoshop in particolare.'
layout: post
guid: http://www.davidebarranca.com/?p=531
permalink: /2011/12/sviluppare-estensioni-creative-suite-photoshop/
category:
  - Coding @it
  - Photoshop @it
---
<div class="pf-content">
  <p>
    Dopo aver letto e condiviso le <a title="Thoughts on Extending the Creative Suite" href="http://in-tools.com/article/thoughts-on-extending-the-creative-suite/" target="_blank">riflessioni di Gabe Harbs</a> (noto sviluppatore InDesign) sull&#8217;argomento, mi piacerebbe aggiungere qui un paio di considerazioni personali, incentrate su Photoshop.
  </p>

  <h2>
    Estensioni Creative Suite (CS)
  </h2>

  <p>
    Qualche informazione per inquadrare l&#8217;argomento: sviluppare per CS oggi è incredibilmente più agevole di quanto non potessimo immaginare solo qualche anno fa. La tecnologia di Macromedia (acquisita da Adobe) è stata inserita nella Creative Suite, si è accoppiata con le Flex SDK, per maturare finalmente in un prodotto che si chiama Extension Builder (un plugin di Flash Builder / Eclipse), basato sulle nuove CS SDK.
  </p>

  <p>
    Siamo ora in grado di scrivere pannelli che:
  </p>

  <ol>
    <li>
      <p>
        si fondono quasi perfettamente con la UI (interfaccia utente) delle applicazioni;
      </p>
    </li>

    <li>
      <p>
        guidano Photoshop, InDesign ecc. esattamente come lo scripting;
      </p>
    </li>

    <li>
      <p>
        sono basati su un linguaggio OOP come ActionScript 3, e su servizi Flex per il data management.
      </p>
    </li>

    <li>
      <p>
        usano componenti Flash per il design della UI.
      </p>
    </li>
  </ol>

  <p>
    Strumenti di incredibile potenza.
  </p>

  <p>
    <!--more-->
  </p>

  <p>
    In più, il <a title="Adobe CS SDK official blog" href="http://blogs.adobe.com/cssdk/" target="_blank">team CS SDK</a> guidato dal PM <a title="Gabriel Tavridis on Twitter" href="http://twitter.com/#!/gtavridis" target="_blank">Gabriel Tavridis</a> sembra essere veramente motivato nel far crescere a ritmo sostenuto la piattaforma; ascoltano attentamente gli sviluppatori, sia per quel che riguarda l&#8217;aspetto tecnico che strategico e commerciale; sebbene io abbia già menzionato altrove i miei <a title="Adobe al bivio" href="/2011/11/adobe-al-bivio-quali-cambiamenti-ci-aspettano/?lang=it" target="_blank">dubbi su Adobe come azienda</a>, questo sembra un caso davvero speciale.
  </p>

  <p>
    Ovviamente non tutto è rose e fiori: da quando (con la CS4) è stato introdotto, il supporto ai pannelli Flash è andato migliorando (forse perché non poteva peggiorare 🙂 ), però il debug a distanza sui clienti è sempre un lavoraccio.
  </p>

  <ul>
    <li>
      <p>
        Su Mac, problemi apparentemente irrilevanti (Font di Sistema, preferenze corrotte) portano a pannelli senza contenuto.
      </p>
    </li>

    <li>
      <p>
        Su PC (da qui in avanti mi riferisco a Photoshop come l&#8217;applicazione host per le estensioni CS), lo stesso script lanciato <em>tout-court</em> o incorporato in un pannello, talvolta <a title="PS-Scripts forum" href="http://www.ps-scripts.com/bb/viewtopic.php?f=9&t=4489&sid=a77983ed5a8f6f8aebe91ed7790229c4" target="_blank">si comporta in modo diverso</a>.
      </p>
    </li>

    <li>
      <p>
        Adobe Extension Manager sembra essere particolarmente ostile agli utenti meno esperti &#8211; la mia richiesta di supporto #1 riguarda l&#8217;installazione via AEM. I <a title="Problemi con Adobe Extension Manager ed OSX Lion" href="/2011/11/problemi-con-adobe-extension-manager-ed-osx-lion/?lang=it" target="_blank">recenti problemi con OSX Lion</a>, fortunatamente risolti con un upgrade ufficiale a Dicembre 2011, confermano che di spazio per migliorare ce n&#8217;è in abbondanza.
      </p>
    </li>
  </ul>

  <p>
    Detto ciò, so per certo che il team sta attualmente lavorando per risolvere questi problemi, per far restare le CS SDK una piattaforma nella quale sia vantaggioso e piacevole lavorare.
  </p>

  <h2>
    Tre strade
  </h2>

  <p>
    Uno sviluppatore che voglia scrivere estensioni per Creative Suite può seguire tre vie (che possono anche incrociarsi):
  </p>

  <ol>
    <li>
      <p>
        C++
      </p>
    </li>

    <li>
      <p>
        Scripting (multi-piattaforma: JavaScript)
      </p>
    </li>

    <li>
      <p>
        CS SDK
      </p>
    </li>
  </ol>

  <p>
    La prima è per programmatori hardcore (cioè non il sottoscritto). Ma dato che il mio caro amico <a title="Marco Olivotto" href="http://www.marcoolivotto.com" target="_blank">Marco Olivotto</a> ha in cantiere un plugin per Photoshop, ho sentito da lui una delle più ampie raccolte di <del>maledizioni</del> rimostranze della storia, perlopiù rivolte alla documentazione: oscura, mancante, fuorviante, obsoleta, ecc. ecc.
  </p>

  <p>
    Per quanto JS possa essere usato sia come linguaggio procedurale (e veloce), oppure come uno strumento per costruire sistemi più complessi, personalmente trovo che le CS SDK ed un linguaggio più fortemente Object Oriented come Actionscript si adattino meglio alle esigenze di sviluppatori (come il sottoscritto) mediamente abili sia nella programmazione che nella gestione del Photoshop DOM (Document Object Model).
  </p>

  <p>
    Segui un corso su Java come il meraviglioso, gratuito, introduttivo <a title="Stanford University - Programming methodology" href="http://see.stanford.edu/see/courseinfo.aspx?coll=824a47e1-135f-4508-a5aa-866adcae1111" target="_blank">&#8220;Programming Methodology&#8221; della Stanford University</a> (tenuto da Mehram Sahami: vulcanico!) e sei pronto per cominciare a sviluppare estensioni con AS, mxml e CS SDK.
  </p>

  <h2>
    Estendere Photoshop
  </h2>

  <p>
    Personalmente, vedo almeno un paio di aree nelle quali potrebbe evolvere in modo interessante l&#8217;estensibilità delle applicazioni CS, ed in particolare Photoshop.
  </p>

  <h3>
    Integrazione
  </h3>

  <p>
    Semplificando, il CS SDK mette a disposizione (tra l&#8217;altro) gli strumenti per accedere al DOM di Photoshop da ActionScript. Ci sono però un paio di problemi:
  </p>

  <ol>
    <li>
      <p>
        Alcune funzioni non sono tradotte in AS. Ad esempio, il metodo <em>.suspendHistory()</em> di Photoshop esiste in AS (mutuato da JS) ma non funziona. Almeno: non funziona <em>ancora</em>.
      </p>
    </li>

    <li>
      <p>
        Questione più cruciale: lo sviluppo delle CS SDK è <em>slegato</em> dall&#8217;evoluzione dello scripting delle varie applicazioni CS.
      </p>
    </li>
  </ol>

  <p>
    La comunità PS-Scripts raccoglie continuamente<a title="PS-Scripts features wishlist" href="http://www.ps-scripts.com/bb/viewtopic.php?f=36&t=3518&sid=986a4d430efdc3de891785c10bbd01a5" target="_blank"> feature requests</a> per Photoshop, anche se il supporto allo scripting non pare essere molto in alto nella lista delle priorità per il team di sviluppo: l&#8217;accesso ad una buona parte di strumenti è al di fuori del DOM e richiede codice Action Manager (non proprio user-friendly). Altre opzioni come l&#8217;area media di campionatura del contagocce o la posizione del cursore tanto per citarne un paio (vedi il mio progetto <a title="Power Info Palette" href="http://blog.rbg.bigano.com/2011/05/22/work-in-progress-power-info-palette/" target="_blank">Power Info Palette</a>), sono totalmente al di fuori della nostra portata: informazioni che non possono essere raggiunte, punto.
  </p>

  <blockquote>
    <p>
      Lo so che non è semplice, ma a mio modo di vedere sarebbe <em>molto più produttivo</em> se il team CS SDK potesse collaborare con i team delle varie applicazioni (PS, ID, ecc) per sviluppare e integrare più strettamente a livello di ActionScript le nuove (e vecchie) features che gli utenti richiedono. Per dirla in altri termini, ancora più chiaramente: se Adobe ha intenzione di promuovere il CS SDK come la via preferenziale per estendere la Creative Suite, dovrebbe dare al team la possibilità di <em>guidare</em> l&#8217;evoluzione del supporto allo scripting per ogni applicazione &#8211; non solo consentire di incorporare (e tradurre in AS) quel che già c&#8217;è, cosa che potremmo dare per scontata.
    </p>
  </blockquote>

  <p>
    So bene che le estensioni sono un grande passo in avanti di per sé &#8211; ciononostante credo che rendere il team di Tavridis il punto di riferimento per gli sviluppatori, anche per quel che riguarda l&#8217;integrazione con le applicazioni, sia un modo certamente migliore di sfruttarne le potenzialità sul lungo periodo.
  </p>

  <h3>
    Accesso a basso livello
  </h3>

  <p>
    Questo è uno dei miei chiodi fissi.<br /> Sviluppando per InDesign (del quale ho scarsissima esperienza) è possibile manipolare un documento e ogni tipo di oggetto che contiene &#8211; anche, da quel che so, il testo contenuto in un frame.<br /> Essendo InDesign un software che (semplificando molto) è fatto per manipolare testo, uno sviluppatore ha quindi accesso agli elementi di base: i caratteri.
  </p>

  <p>
    Traducendo per Photoshop, è come se potessimo avere accesso agli elementi costitutivi di un&#8217;immagine, i suoi atomi, cioè: i pixel.<br /> Purtroppo no.<br /> Possiamo duplicare un documento, un livello, applicare filtri ad una selezione o un canale &#8211; ma un livello bitmap in PS non può essere direttamente accessibile alle estensioni come una BitmapImage ActionScript. L&#8217;unica alternativa è:
  </p>

  <ol>
    <li>
      <p>
        salvare una copia temporanea dell&#8217;immagine su disco;
      </p>
    </li>

    <li>
      <p>
        farla caricare dal pannello (ed elaborarla);
      </p>
    </li>

    <li>
      <p>
        salvarla di nuovo su disco;
      </p>
    </li>

    <li>
      <p>
        caricarla in PS e incollarla nel documento originale come un nuovo livello.
      </p>
    </li>
  </ol>

  <p>
    Un disastro. Sapevo che un passaggio diretto tra Photoshop ed una &#8220;estensione&#8221; (nelle due direzioni, come un JPG a livelli uniti) è permesso &#8211; dalle Photoshop Touch SDK (<em>stranamente scomparse dall&#8217;Adobe Devnet?!</em>). Ovvero, le applicazioni Touch che tanto Adobe sembra spingere ora. Sapevo anche che <a title="Daniel Koestler Adobe blog" href="http://blogs.adobe.com/koestler/2011/05/using-the-photoshop-touch-sdk-creating-a-project.html" target="_blank">Daniel Koestler</a> e <a title="Renaun Erickson blog" href="http://renaun.com/blog/2011/04/photoshop-touch-sdk-contains-an-actionscript-3-library-too/" target="_blank">Renaun Erickson</a> hanno scritto le API in Actionscript (nelle Touch SDK, che ho scaricato in Maggio 2011) &#8211; Mi chiedo, sperandolo disperatamente, se questo piccolo passagio sarà permesso prima o poi anche dalle CS SDK.
  </p>

  <blockquote>
    <p>
      Lasciare che un&#8217;estensione abbia <em>accesso diretto ad un livello in Photoshop come BitmapImage</em> sarebbe eccezionale: implicherebbe la possibilità di scrivere veri plugin di Digital Image Processing, con UI in Flash, nessun problema di memory management, usando kernel binari di PixelBender (il cui codice è quindi protetto), e tutti gli strumenti e shader già implementati in Actionscript. Certo, le performance rispetto ad un plugin C++ sarebbero inferiori ma&#8230; che importa.
    </p>
  </blockquote>

  <p>
    Pensavo di essere l&#8217;unico a richiedere questa feature, ma dopo aver ricevuto un paio di email da altri sviluppatori, mi sono deciso: Adobe, per piacere&#8230; 😉
  </p>

  <h2>
    Il futuro dell&#8217;estensibilità per Creative Suite
  </h2>

  <p>
    Mentre ragionavo su questo post, ho scritto a Gabriel Tavridis di alcuni dubbi che mi sono sorti dopo aver letto della <a title="Adobe Flex blog" href="http://blogs.adobe.com/flex/2011/11/your-questions-about-flex.html" target="_blank">faccenda di Flash e Flex</a>. E&#8217; perfettamente chiaro che, sebbene a breve Flex non possa essere rimpiazzato da HTML5, l&#8217;interesse sul lungo periodo di Adobe per questa tecnologia è forte. Dunque, qual è la roadmap per le estensioni CS? Domandavo. Gabriel mi ha risposto molto gentilmente che avrebbe scritto in merito un post nel blog ufficiale &#8211; <a title="CS SDK future" href="http://blogs.adobe.com/cssdk/2011/12/cs_extensibility_and_flex_next.html" target="_blank">ora online</a> e&#8230; ti suggerisco di leggerlo!
  </p>

  <p>
    Concludendo, non importa quale tecnologia useremo negli anni a venire, è importante però che Adobe sia trasparente nei confronti dei suoi utenti: come sviluppatore, noto con piacere che da questo punto di vista il team CS SDK è all&#8217;avanguardia.
  </p>
</div>
