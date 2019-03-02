---
id: 972
title: 'Dorsi digitali Phase One e Adobe Camera Raw &#8211; Paneling &#8220;bug&#8221;'
date: 2012-04-28T23:31:32+01:00
author: Davide Barranca
excerpt: |
  I files raw dei dorsi digitali Phase One non sono pienamente supportati da Adobe Camera Raw (7.0 e precedenti), che non interpreta i dati di calibrazione del sensore incorporati nei .IIQ. Questo paneling "bug" incide sull'uniformità dell'immagine - in questo post ti propongo una soluzione.
layout: post
guid: http://www.davidebarranca.com/?p=972
permalink: /2012/04/dorsi-digitali-phase-one-iiq-adobe-camera-raw-paneling-bug/
image: /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3.jpg
category:
  - Post-produzione
tags:
  - Adobe Camera Raw @it
  - Capture One @it
  - Dorso Digitale
  - IIQ
  - IQ180 @it
  - Oggetto Avanzato
  - P65+ @it
  - Phase One @it
  - raw
---
<div class="pf-content">
  <p>
    <img class="alignnone size-full wp-image-922" style="border-style: initial; border-color: initial; border-width: 0px;" title="PhaseOne IQ180 paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug.jpg" alt="PhaseOne IQ180 paneling bug" width="570" height="428" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug.jpg 570w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug-150x112.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug-300x225.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <p>
    Se usi un dorso digitale Phase One (serie IQ oppure P/P+) ti è stato detto di adoperare esclusivamente Capture One Pro per lo sviluppo dei raw. Qualità insuperabile, e dopo tutto se un&#8217;azienda produce sia hardware che software, sarà in grado di ottimizzare entrambi per lavorare in coppia come nessun altro.
  </p>

  <p>
    Personalmente, temo di non condividere lo stesso entusiasmo dei sostenitori di Capture One: da quando Adobe ha distribuito il nuovo Camera Raw 7.0 (con Photoshop CS6, la cui tecnologia è incorporata anche in Lightroom 4) finalmente esiste un&#8217;eccellente alternativa &#8211; direi superiore sotto molti aspetti chiave. Purtroppo, devi tenere conto di <strong>un problema fastidioso e nascosto</strong> &#8211; amichevolmente chiamato <em>il Paneling &#8220;bug&#8221;.</em>
  </p>

  <p>
    <!--more-->
  </p>

  <h3>
    Qual è la parte di <em>&#8220;non ufficialmente supportato&#8221;</em> che ti sfugge?
  </h3>

  <p>
    <img class="alignleft  wp-image-924" style="border-style: initial; border-color: initial; border-width: 0px;" title="IIQ icon" src="/wp-content/uploads/2012/04/IIQ_icon.png" alt="IIQ icon" width="135" height="169" />&#8230; Mi è stato detto una volta, ed avevano ragione in qualche modo. Aprire un file raw Phase One con estensione .IIQ in Adobe Camera Raw (ACR d&#8217;ora in poi) era teoricamente sconsigliato, perché mancava il <em>&#8220;supporto ufficiale&#8221;.</em> Cosa volesse dire era lecito domandarselo, perché ACR sembrava leggere ed aprire senza problemi gli IIQ proprio come qualsiasi altro file CR2, NEF, DNG.
  </p>

  <p>
    Errore: <strong> ogni file .IIQ contiene (meta)dati di calibrazione del sensore che ACR non è in grado di interpretare</strong>. Queste informazioni, da quel che vedo, sono usate per <strong>bilanciare la risposta del sensore</strong>, che è geometricamente diviso in otto sezioni (due righe per quattro colonne). Che siano otto elettroniche separate oppure otto sensori più piccoli messi assieme non ne ho idea, e obbiettivamente non mi interessa granché.
  </p>

  <div id="attachment_929" style="width: 560px" class="wp-caption aligncenter">
    <img aria-describedby="caption-attachment-929" class="size-full wp-image-929" title="PhaseOne P65+ paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug.jpg" alt="PhaseOne P65+ paneling bug" width="550" height="363" srcset="/wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug.jpg 550w, /wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug-150x98.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug-300x197.jpg 300w" sizes="(max-width: 550px) 100vw, 550px" />

    <p id="caption-attachment-929" class="wp-caption-text">
      PhaseOne P65+ Sinistra: versione RGB. Destra: il canale "a" di Lab equalizzato, evidenzia Paneling (griglia).
    </p>
  </div>

  <div id="attachment_932" style="width: 560px" class="wp-caption aligncenter">
    <img aria-describedby="caption-attachment-932" class="size-full wp-image-932" title="PhaseOne IQ180 paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21.jpg" alt="PhaseOne IQ180 paneling bug" width="550" height="831" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21.jpg 550w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21-150x226.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21-198x300.jpg 198w" sizes="(max-width: 550px) 100vw, 550px" />

    <p id="caption-attachment-932" class="wp-caption-text">
      PhaseOne IQ180. Sopra: versione RGB, debole dominante incrociata verde/magenta sull'asfalto. Sotto: il canale "a" di Lab equalizzato evidenzia il problema più chiaramente.
    </p>
  </div>

  <p>
    Il punto è che se questa calibrazione, diversa per ogni sensore, si perde (e ACR 7.0 e le versioni precedenti la perdono per strada di sicuro), la demosaicizzazione produce un&#8217;immagine <em>non corretta</em>. Le differenze tra ogni rettangolo della griglia possono venire accentuate piuttosto facilmente dalla color correction successiva: e credimi, tu non vuoi trovarti per le mani un file che ha <strong>dominanti verdi e magenta disposte a scacchiera nell&#8217;immagine</strong>.
  </p>

  <blockquote>
    <p>
      N.B. questo problema non si manifesta usando Capture One, che legge ed interpreta correttamente i tag proprietari dei file .IIQ.
    </p>
  </blockquote>

  <p>
    Ad oggi, sia Photoshop CS6 che Lightroom 4 <a title="Supported cameras for the Camera Raw plug-in and Lightroom" href="http://www.adobe.com/products/photoshop/extend.html" target="_blank">supportano ufficialmente l&#8217;intera gamma di dorsi Phase One</a>, eppure il problema persiste. Puoi facilmente controllare se i tuoi raw sono gestiti senza errori da ACR, senza essere affetti dal cosiddetto <strong>Paneling &#8220;bug&#8221;</strong>. Ti consiglio di provare almeno una volta su alcuni files (uno scatto test di un muro bianco ed un paio di immagini &#8220;vere&#8221;):
  </p>

  <ol>
    <li>
      <p>
        Apri il file .IIQ in Adobe Camera Raw, azzerando tutto eccetto il bilanciamento del bianco. Profondità di bit (8/16) e profilo RGB non contano, i tuoi valori di default andranno bene.
      </p>
    </li>

    <li>
      <p>
        In Photoshop, converti il file in Lab (<em>Immagine &#8211; Metodo &#8211; Colore Lab</em> oppure <em>Modifica &#8211; Converti in Profilo &#8211; Colore Lab</em>).
      </p>
    </li>

    <li>
      <p>
        Nella palette dei <em>Canali</em> rendi attivo il canale &#8220;a&#8221; (Command + 4 su Mac, CTRL + 4 su PC, Photoshop CS5 e CS6)
      </p>
    </li>

    <li>
      <p>
        <em>Immagine &#8211; Regolazioni &#8211; Equalizza</em>.
      </p>
    </li>

    <li>
      <p>
        Rendi attivo il canale &#8220;b&#8221; (Command + 5 su Mac, CTRL + 5 su PCs) e ripeti il passaggio #4.
      </p>
    </li>
  </ol>

  <p>
    Se nel canale &#8220;a&#8221; e/o &#8220;b&#8221; intravedi una griglia più o meno evidente, i tuoi files sono <em>(non ufficialmente) non supportati</em> in ACR! A volte la griglia non è così ovvia, e puoi trovare stranezze come quella nell&#8217;immagine qui sotto &#8211; brutta notizia comunque.
  </p>

  <div id="attachment_936" style="width: 560px" class="wp-caption aligncenter">
    <img aria-describedby="caption-attachment-936" class="size-full wp-image-936" title="PhaseOne IQ180 paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3.jpg" alt="PhaseOne IQ180 paneling bug" width="550" height="531" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3.jpg 550w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3-150x144.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3-300x289.jpg 300w" sizes="(max-width: 550px) 100vw, 550px" />

    <p id="caption-attachment-936" class="wp-caption-text">
      Phase One IQ180. Sopra: cielo con evidente Paneling (dominante magenta nella metà sinistra). Sotto: il canale "a" di Lab equalizzato evidenzia un pattern decisamente bizzarro.
    </p>
  </div>

  <div id="attachment_938" style="width: 117px" class="wp-caption alignleft">
    <a href="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART.jpg" target="_blank"><img aria-describedby="caption-attachment-938" class="size-medium wp-image-938 " style="border-style: initial; border-color: initial; border-width: 0px;" title="PhaseOne IQ180 paneling bug - particular" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART-107x300.jpg" alt="PhaseOne IQ180 paneling bug - particular" width="107" height="300" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART-107x300.jpg 107w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART-150x419.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART.jpg 257w" sizes="(max-width: 107px) 100vw, 107px" /></a>

    <p id="caption-attachment-938" class="wp-caption-text">
      Clicca per ingrandire.
    </p>
  </div>

  <p>
    Tra parentesi: so bene che i passaggi sopra descritti sono un modo estremo per brutalizzare un&#8217;immagine a fini di test &#8211; eppure l&#8217;aumento di contrasto nei canali &#8220;a&#8221; e &#8220;b&#8221; di Lab è piuttosto comune tra ritoccatori, ed il comando <em>Equalizza</em> è effettivamente usato nel cosiddetto <em>Modern Man from Mars</em>, una tecnica codificata da Dan Margulis, elemento fondamentale del suo <a title="The Picture Postcard Workflow Panel" href="http://www.ledet.com/margulis/ppw" target="_blank">Picture Postcard Workflow</a>, aka PPW.
  </p>

  <p>
    Mi è stato riferito da un utente Phase One che lo stesso tipo di Paneling &#8220;bug&#8221; potrebbe mostrarsi anche usando Capture One come raw processor. Nel qual caso la calibrazione interna del dorso digitale non corrisponde più alla vera risposta del sensore: che tradotto in termini pratici significa spedire tutto a revisionare in Danimarca (lui ha fatto così, ed ha risolto i suoi guai).
  </p>

  <h3 style="font-size: 1.17em;">
    Usa Adobe Camera Raw lo stesso!
  </h3>

  <p>
    Personalmente, Capture One Pro (C1 Pro) non mi piace per nulla. Non è gentile scriverlo in certi forum (le persone sono piuttosto suscettibili quando si tratta di religione, cibo e raw processor), ma siccome ora non mi sente nessuno posso confessarlo: è un gran brutto software, macchinoso, lento, involuto, inutilmente complesso, la UI (interfaccia utente) e i controlli sono progettati male (prova a muovere un punto su una curva dei canali senza perdere la pazienza), anche dal mero punto di vista degli algoritmi ACR 7.0 è superiore. Questo è il mio personale, opinabile punto di vista &#8211; e non sono un fan di Adobe, di solito.
  </p>

  <div id="attachment_947" style="width: 230px" class="wp-caption alignright">
    <img aria-describedby="caption-attachment-947" class="size-full wp-image-947" title="CaptureOne Curves" src="/wp-content/uploads/2012/04/CaptureOne_Curves.png" alt="CaptureOne Curves" width="220" height="165" srcset="/wp-content/uploads/2012/04/CaptureOne_Curves.png 220w, /wp-content/uploads/2012/04/CaptureOne_Curves-150x112.png 150w" sizes="(max-width: 220px) 100vw, 220px" />

    <p id="caption-attachment-947" class="wp-caption-text">
      Capture One, pannello Curve.
    </p>
  </div>

  <p>
    Nota bene: la qualità delle immagini di C1 è molto buona (per quanto più spesso io preferisca ACR nel confronto diretto) ma il prezzo in termini di usabilità è troppo alto. Se non avessi alternative, mi adatterei mestamente a C1. Ma ACR 7.0 restituisce una qualità eccezionalmente competitiva: la riduzione del rumore è senz&#8217;altro migliore, l&#8217;usabilità generale è di svariati ordini di grandezza superiore, il <a title="Local Laplacian Filters: Edge-aware Image Processing with a Laplacian Pyramid" href="http://people.csail.mit.edu/sparis/publi/2011/siggraph/" target="_blank">Local Laplacian filtering</a> (l&#8217;algoritmo, rivisto, che sta dietro a Clarity) è davvero molto efficace, adesso che Adobe ha implementato le curve sui singoli canali R, G, B (era ora!) non ci sono ragioni per non usarla.
  </p>

  <p>
    Quindi io apro i files .IIQ in ACR (perché sono un ritoccatore, i fotografi in genere adoperano Lightroom: che, tecnicamente parlando, è lo stesso motore in una macchina diversa). Come?
  </p>

  <h3>
    Evita la conversione in DNG
  </h3>

  <p>
    <img class="alignleft size-full wp-image-955" style="border-style: initial; border-color: initial; border-width: 0px;" title="DNG icon" src="/wp-content/uploads/2012/04/DNG.png" alt="DNG icon" width="150" height="150" /><strong>Non provare ad esportare gli .IIQ in DNG con Capture One</strong> (un processo stranamente veloce per gli standard di C1, peraltro). E&#8217; stato segnalato che precedenti versioni di C1 commettono piccoli errori sui metadati e pasticci sulla dimensione dei files (gonfiati nel DNG fino al doppio rispetto agli IIQ). Che questi problemi siano stati risolti o meno (non mi risulta, almeno per quel che riguarda il peso), anche usando una versione aggiornata di Capture One <strong>i DNG non sono calibrati</strong>, per cui quando vai ad aprirli in ACR si presenta lo stesso Paneling &#8220;bug&#8221;! Questo dal mio punto di vista è un problema non da poco, che dovrebbe essere preso in considerazione da Phase One: quando un utente converte in un formato standard come il DNG, si aspetta di ottenere dati corretti. Posso capire che alcune informazioni proprietarie di secondaria importanza, incorporate nei raw, possano non trovare spazio nelle specifiche DNG (penso ai punti usati dall&#8217;autofocus, che se la memoria non mi inganna sono recuperabili nei CR2 solo usando il Canon DPP). Qui però stiamo parlando dei canali dell&#8217;immagine, che sono intenzionalmente lasciati senza correzione!
  </p>

  <p>
    Nel caso ti venisse l&#8217;idea: lascia pure stare l&#8217;Adobe DNG Converter &#8211; al quale dei dati di calibrazione del sensore non interessa un fico secco. L&#8217;unico stratagemma che ho trovato non è particolarmente elegante, ma funziona &#8211; se sei disposto a perdere qualcosa per strada.
  </p>

  <h3>
    Livelli e Modalità di Fusione
  </h3>

  <p>
    Negli ultimi tre anni ho avuto l&#8217;opportunità di lavorare con due dorsi Phase One P65+ ed un IQ180 (60MP ed 80MP rispettivamente), per cui sono questi i modelli dei quali ho esperienza diretta &#8211; lo stesso concetto si può applicare su ogni modello P1 che soffra del Paneling &#8220;bug&#8221; (ti faccio notare che scrivo: &#8220;bug&#8221;. Perché so che <em>non è ufficialmente</em> un bug &#8211; per quanto il fatto che C1 esporti un DNG non corretto a me pare un dettaglio non da poco; usare le virgolette mi sembra un equo compromesso).
  </p>

  <p>
    Siccome il problema viene evidenziato nei canali &#8220;a&#8221; e &#8220;b&#8221; di Lab (più il primo, a cui corrisponde l&#8217;asse verde-magenta, che il secondo, blu-giallo: ovvero le componenti cromatiche dell&#8217;immagine) e non la &#8220;L&#8221;, è abbastanza facile trovare un escamotage processando il raw due volte e unendo i risultati come livelli in modalità di fusione Luminosità / Colore &#8211; nel modo seguente:
  </p>

  <ol>
    <li>
      <p>
        Apri il file .IIQ in ACR 7.0 (o qualunque altra versione di ACR che hai legalmente a disposizione).
      </p>
    </li>

    <li>
      <p>
        Aggiusta tutti i parametri come credi &#8211; senza perdere tempo sul rumore cromatico (che andrà perso) &#8211; e aprilo in Photoshop come Oggetto Avanzato (shift-click sul pulsante <em>Apri Immagine</em>).
      </p>
    </li>
  </ol>

  <div>
    <a href="/wp-content/uploads/2012/04/ACR_open_as_SmartObject.png"><img class="aligncenter size-full wp-image-950" style="margin-bottom: 15px;" title="ACR - open as SmartObject" src="/wp-content/uploads/2012/04/ACR_open_as_SmartObject.png" alt="ACR - open as SmartObject" width="483" height="108" srcset="/wp-content/uploads/2012/04/ACR_open_as_SmartObject.png 483w, /wp-content/uploads/2012/04/ACR_open_as_SmartObject-150x33.png 150w, /wp-content/uploads/2012/04/ACR_open_as_SmartObject-300x67.png 300w" sizes="(max-width: 483px) 100vw, 483px" /></a>
  </div>

  <ol start="3">
    <li>
      <p>
        Apri lo stesso .IIQ in Capture One.
      </p>
    </li>

    <li>
      <p>
        In Capture One, cerca di ottenere un risultato cromaticamente simile (mimando le stesse curve, bilanciamento del bianco, ecc. impostate in ACR, fatta eccezione per la rimozione del rumore di luminosità e lo sharpening). Incrocia le dita e salva un TIFF con la stessa profondità di bit e profilo ICC di output usati in ACR.
      </p>
    </li>

    <li>
      <p>
        Apri il TIFF in Photoshop, trascinagli sopra l&#8217;Oggetto Avanzato proveniente da ACR ed imposta la sua modalità di fusione in <em>Luminosità</em>. Se tieni premuto il tasto Shift durante il trascinamento, il livello si allineerà automaticamente al sottostante.
      </p>
    </li>

    <li>
      <p>
        [Opzionale] Seleziona entrambi i livelli e convertili in un singolo Oggetto Avanzato (una buona idea se hai intenzione di correggere la prospettiva dello scatto).
      </p>
    </li>
  </ol>

  <p>
    <img class="alignleft size-full wp-image-951" style="border-width: 0px;" title="Layers" src="/wp-content/uploads/2012/04/Layers.png" alt="Layers" width="351" height="262" srcset="/wp-content/uploads/2012/04/Layers.png 351w, /wp-content/uploads/2012/04/Layers-150x111.png 150w, /wp-content/uploads/2012/04/Layers-300x223.png 300w" sizes="(max-width: 351px) 100vw, 351px" />In questo modo ti ritrovi con un documento a due livelli: il sottostante è l&#8217;interpretazione di Capture One, il superiore è la versione ACR in <em>Luminosità</em>. Ovvero: mantieni il colore di Capture One (profilo ICC di input, curve, correzione delle aberrazioni e riduzione del rumore cromatici, ecc.) e la luminosità di Adobe Camera Raw (con la sua riduzione del rumore, Clarity, recupero delle luci ed ombre, ecc.).
  </p>

  <p>
    Puoi invertire l&#8217;ordine dei livelli (ACR sotto e C1 sopra, ma in modalità di fusione <em>Colore</em>), il risultato è il medesimo.
  </p>

  <p>
    Se ti preme il color management, puoi essere molto soddisfatto di poter scegliere tra i profili ICC di input di Capture One; o molto insoddisfatto, perché è più complicato usare il ColorChecker Passport per profilare uno scatto in C1. In entrambi i casi, non preoccuparti troppo; non mi sono mai sembrate soluzioni particolarmente accurate. Del resto, come ha detto una volta Dan Margulis: &#8220;se un&#8217;immagine ha un problema, correggilo &#8211; è il tuo mestiere, che problema c&#8217;è&#8221;.
  </p>

  <h3>
    Conclusioni
  </h3>

  <p>
    Adobe Camera Raw 7.0 è un sostanziale passo avanti rispetto alle versioni 6.x. La sua usabilità è molto, molto superiore rispetto a Capture One, restituisce immagini di qualità eccezionale ed algoritmi decisamente più avanzati (sono in fremente attesa di vedere nella prossima release ufficiale l&#8217;applicazione della <a title="Lightroom Journal - New Color Fringe Correction Controls" href="http://blogs.adobe.com/lightroomjournal/2012/04/new-color-fringe-correction-controls.html" target="_blank">nuova correzione sulle aberrazioni cromatiche assiali</a>). Eppure i raw provenienti dai dorsi digitali Phase One sono <em>(non ufficialmente) non supportati</em> a causa della mancata interpretazione dei dati di calibrazione del sensore incorporati negli .IIQ da parte di ACR (in qualsiasi sua versione fino alla corrente, la 7.0). Né i DNG esportati da Capture One vengono sottoposti ad alcuna normalizzazione (bug!). Quindi aprire un file raw Phase One in Adobe Camera Raw porta ad un&#8217;immagine che contiene una griglia, una scacchiera di dominanti incrociate &#8211; perlopiù sull&#8217;asse verde/magenta: in alcuni casi a prima vista invisibile (in altri molto ben presente da subito), che comunque è destinata a diventare molto evidente ed invasiva appena con la color correction vengono enfatizzati i colori.
  </p>

  <p>
    Un espediente (in pia attesa che Capture One corregga l&#8217;esportazione dei DNG e/o Adobe supporti <em>veramente</em> i raw Phase One) che suggerisco è di <strong>processare il raw due volte</strong> (una con C1, una con ACR) <strong>unendo i due files in Photoshop come livelli in Colore e Luminosità</strong>. Non è una soluzione particolarmente elegante, ma non ho trovato alternative e funziona. Forse in questo modo si unisce il meglio dei due mondi. Forse! 😉
  </p>
</div>
