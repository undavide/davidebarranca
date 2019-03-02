---
title: " Sharpening - uno studio\t\t"
url: 251.html
id: 251
comments: false
category:
  - Photoshop @it
date: 2011-11-23 17:00:31
tags:
---

L'ultimo esempio che ho presentato nella mia [lezione sullo Sharpening al CCC](http://localhost:8888/2011/09/sharpening-session-at-ccc/ "Sharpening session at CCC") in Ottobre è stata la correzione di un'immagine subacquea. Mi è stato domandato, giorni dopo, di mostrarla nuovamente in tutti i suoi passaggi - così ho chiesto al mio caro amico e fotografo [Davide D'Angelo](http://www.davidedangelo.com/ "Davide D'Angelo Photographer"): molto gentilmente mi ha messo in contatto col suo collega [Paolo Fossati, fotografo subacqueo](http://www.paolo-fossati.com/ "Paolo Fossati underwater photographer") e autore di questa immagine. Grazie mille ad entrambi! Eccola qui in forma di piccolo studio sullo sharpening. \[caption id="attachment_335" align="aligncenter" width="570" caption="Versione finale (clicca sull'immagine per ingrandire) - © Paolo Fossati"\][![Actinia and Anemonefish](http://localhost:8888/wp-content/uploads/2011/11/03-Final_L.jpg "Actinia and Anemonefish")](http://localhost:8888/wp-content/uploads/2011/11/03-Final_H.jpg)\[/caption\]  L’originale è un file RAW di una Nikon D80 (3872 x 2592 px, tanto per contestualizzare i raggi di maschera di contrasto che userò), e, per quel che ho potuto capire da Google, si tratta di una Actiniaria (o Attinia, l'animale rosso coi tentacoli) in compagnia di un pesce pagliaccio (il Nemo giallo). L'immagine proviene dal viaggio di Paolo Fossati in Arabia Saudita ai [Seven Reefs (galleria completa](http://www.paolo-fossati.com/Galleria%20Arabia/Galleria%20Arabia.html "Arabian Seven Reef - images by Paolo Fossati photographer")). Rispetto al CCC, ho un più tempo per soffermarmi sui vari passaggi, ma per brevità do’ per scontata la conoscenza di alcuni strumenti – se qualcosa non ti fosse chiaro, scrivimi nei commenti a fondo pagina! \[caption id="attachment_336" align="aligncenter" width="570" caption="Versione di partenza, azzerando tutto in Adobe Camera Raw - clicca per ingrandire"\][![Raw conversion](http://localhost:8888/wp-content/uploads/2011/11/raw_L.jpg "Raw conversion")](http://localhost:8888/wp-content/uploads/2011/11/raw_H.jpg)\[/caption\]

Parte 1 - Colore e contrasto
----------------------------

Ammetto di non avere particolare esperienza con questo tipo di immagini naturalistiche, zeppe di trabocchetti che Davide D'Angelo conosce certamente meglio di me: infatti oltre a fotografare [in scena](http://www.davidedangelo.com/Danza.html "Davide D'Angelo - on stage photography") ed essere specialista di [infrarosso](http://www.davidedangelo.com/Infrared_gallery/infrared_gallery.html "Davide D'Angelo - Infrared Landscape photography"), è un esperto subacqueo. Quindi ha una dimestichezza particolare nella correzione di queste immagini (che ha elaborato a migliaia) - meno ovvie di quel che sembrano a prima vista. \[caption id="attachment_339" align="alignleft" width="300" caption="Adobe Camera Raw default - clicca per ingrandire"\][![ACR Default](http://localhost:8888/wp-content/uploads/2011/11/ACR-Default.jpg "ACR Default")](http://localhost:8888/wp-content/uploads/2011/11/ACR_Default_H.jpg)\[/caption\] Al CCC di Bologna ho lavorato su un soggetto più semplice, così ho potuto descrivere anche tutta la parte di correzione colore. Cosa che purtroppo non posso fare qui - e non per nasconderti chissà quali segreti; a dire la verità non sapevo che strada prendere, e ho sperimentato liberamente. Comunque la mia versione corretta (che è un intermedio di lavorazione, e sarà la base di tutto lo sharpening che faremo) è decisamente lontana sia dal default che propone Camera Raw, sia dall'originale molto piatto dal quale ho deciso di partire. Comunque, grosso modo, la correzione prevedeva:

1.  Esportazione del RAW in ProPhoto RGB (spazio colore che in genere evito come la peste, ma che qui è opportuno per avere un canale del Rosso con più dettaglio, che mi servirà nei passaggi successivi).
    
2.  Una prematura conversione in Lab per lavorare sul contrasto nel canale _L_ (anche con channel blending).
    
3.  Curve di dubbia morale sui canali _a_ e _b_, ed azioni _MMM_ ed _Helmholtz-Kohlrausch_ dal [pannello PPW di Dan Margulis](http://www.ledet.com/margulis/ppw).
    
4.  Nel caso servisse RGB, useremo sRGB (che, per fortuna, sembra obliterare il terribile banding nel cielo nell'acqua, che è stato una penitenza dall'inizio).
    

Possiamo sgarrare, forse, su una delle regole base della correzione colore: cioè il punto di ombra neutro. Siamo sott'acqua, annegati di Blu ovunque eccetto per l'area di primo piano colpita dalla luce del flash; peraltro io non sono mai stato in immersione (e dubito ci andrò mai), quindi non ho la più pallida idea di quale sia il tipo di visione là sotto, né come appaiano queste creature dal vero ad un subacqueo a che esplori il fondale. \[caption id="attachment_342" align="aligncenter" width="570" caption="Versione intermedia - clicca per ingrandire"\][![Intermediate version - color corrected](http://localhost:8888/wp-content/uploads/2011/11/01-Intermediate_L.jpg "Intermediate version - color corrected")](http://localhost:8888/wp-content/uploads/2011/11/01-intermediate_H.jpg)\[/caption\] \[caption id="attachment_374" align="aligncenter" width="570" caption="Dettagli - clicca per aprire l'alta risoluzione in una nuova finestra"\][![Details - Base version](http://localhost:8888/wp-content/uploads/2011/11/Details_BASE.jpg "Details - Base version")](http://localhost:8888/wp-content/uploads/2011/11/Details_BASE.jpg)\[/caption\] Detto questo, la mia versione intermedia è forse troppo ricca di colore - che di per sé non è un difetto, credo. Se ti interessa comparare gli stili di correzione di diversi ritoccatori, dai un'occhiata alla [versione di Paolo Fossati / Davide D'Angelo](http://www.paolo-fossati.com/Galleria%20Arabia/slides/Ayla-22.jpg "Paolo Fossati / Davide D'Angelo version") \- più drammatica e tridimensionale. Io sono stato meno fotografico (del resto non sono un fotografo), e più descrittivo; per certi versi la mia versione richiama l'estetica di certi acquerelli del 18 secolo sugli uccelli del paradiso della Nuova Guinea, se la cosa non suonasse del tutto folle... E' la visione del fondale (immaginario) di chi non c'è mai stato - un po' come i libri di viaggio scritti senza viaggiare, un'interpretazione fantastica e non sempre corrispondente al vero - che di per sé, dicevo, può anche non essere un difetto.

Parte 2 - HiRaLoAm
------------------

E’ giunto il momento di pianificare la strategia di sharpening. In generale, non vorrei accentuare troppo lo sfondo, e mantenere invece l’attenzione sui soggetti in primo piano. Siccome siamo in Lab, il canale della _b_ (opponenti blu/giallo) ci tornerà utile per una maschera. Una passata di HiRaLoAm (High Radius Low Amount – ovvero il filtro Maschera di Contrasto con Raggio Alto e Fattore Basso) male non fa, per cui:

5.  Duplica il livello di sfondo e chiamalo HiRaLoAm.
    
6.  Attiva il canale _L_, ed applica la Maschera di Contrasto con Fattore: 500 - Raggio: 12px - Soglia: 0px.
    

\[caption id="attachment_356" align="aligncenter" width="570" caption="HiRaLoAm spinto - clicca per ingrandire"\][![HiRaLoAm Full Blast - crop](http://localhost:8888/wp-content/uploads/2011/11/HiRaLoAm_FULL_crop.jpg "HiRaLoAm Full Blast - crop")](http://localhost:8888/wp-content/uploads/2011/11/HiRaLoAm_FULL1.jpg)\[/caption\] Fa schifo, eh? Molto bene, perché:

7.  Crea una maschera di livello per HiRaLoAm, e applicaci il canale _L _dallo Sfondo. Ci siamo quasi.
    
8.  Abbassa l’opacità di HiRaLoAm al 40% circa..
    

[![Apply L to the layer mask](http://localhost:8888/wp-content/uploads/2011/11/Apply_l-150x89.png)](http://localhost:8888/wp-content/uploads/2011/11/Apply_l.png)Sapendo che avrei usato una maschera di livello per attenuare l’effetto (dopo), ho preferito avere la mano pesante col filtro (prima). Volendo, poi, c’è anche l’opacità che fornisce un ulteriore controllo – il flusso di lavoro così è più flessibile, a mio parere. Lo sharpening è andato solo sul canale _L_ per evitare guai sotto forma di aloni colorati. C’è ancora un problema – l’acqua è troppo dettagliata, in particolare i pescetti sullo sfondo sembrano radioattivi. Così, come dicevamo prima:

9.  Fai doppio click sull’icona del livello HiRaLoAm nella palette dei Livelli, per aprire la finestra degli _Stili di Livello – Opzioni di Fusione_ (_Layer Style – Blending Options_), e sposta come in figura gli slider _Fondi Se _(_Blend If_), per restringere l’effetto alle parti “calde” del canale _b_, tagliando quelle “fredde” (cioè l’acqua blu).
    

[![Blending Options - Blend If](http://localhost:8888/wp-content/uploads/2011/11/BlendIf-150x118.png)](http://localhost:8888/wp-content/uploads/2011/11/BlendIf.png) Le _Opzioni di Fusione_ sono un modo per limitare la visibilità di un livello (di qualsiasi tipo) sulla base del suo contenuto o del contenuto del livello sottostante. _Fondi Se_ (_BlendIf_) funziona come una soglia: prima selezioni quale/i canale/i usare come base, poi trascini gli slider (eventualmente, ALT+click separa in due uno slider e sfuma la transizione) per definire se e quando verrà limitata la visibilità del livello superiore: a seconda di quanto range del canale scelto gli slider non racchiudono.

10.  Premi contemporaneamente (_Command + Option + Shift + E_) se sei su un Mac, (_CTRL + ALT + Shift + E_) se sei su un PC. Un nuovo livello verrà creato e riempito col contenuto visibile. Butta pure il livello HiRaLoAm sotto assieme alla sua maschera, e rinomina il livello fresco di creazione come “HiRaLoAm”.
    

\[caption id="attachment_359" align="aligncenter" width="570" caption="Sharpening a Raggio Alto, Fattore Basso, con maschera L e Opzioni di Fusione - clicca per ingrandire"\][![HiRaLoAm intermediate version](http://localhost:8888/wp-content/uploads/2011/11/02-HiRaLoAm_L.jpg "HiRaLoAm intermediate version")](http://localhost:8888/wp-content/uploads/2011/11/02-HiRaLoAm_H.jpg)\[/caption\] \[caption id="attachment_379" align="aligncenter" width="570" caption="Dettagli - clicca per aprire l'alta risoluzione in una nuova finestra"\][![Details - HiRaLoAm](http://localhost:8888/wp-content/uploads/2011/11/Details_HIRALOAM.jpg "Details - HiRaLoAm")](http://localhost:8888/wp-content/uploads/2011/11/Details_HIRALOAM.jpg)\[/caption\]

Parte 3 - Ehi, dove siamo?
--------------------------

Ok, pausa. Fin qui, ho corretto il colore e contrasto in modi che non so dire più: il risultato sta felicemente come base (ed àncora di salvataggio) nel livello di Sfondo. Poi abbiamo un secondo livello chiamato “HiRaLoAm”, che è il risultato di un livello con sharpening HiRaLoAm con maschera e opzioni _Fondi Se_, trasformato per semplicità con un’operazione esoterica di “Unisci Visibili su Nuovo Livello” in un livello… normale. HiRaLoAm è usato per aggiungere una specie di effetto di contrasto locale (anche se non è un vero contrasto locale come [ALCE (conosci ALCE, vero?](http://www.bigano.com/ALCE "ALCE - Advanced Local Contrast Enhancer for Photoshop")). Manca una cosa e abbiamo finito.

Parte 4 - Sharpening tradizionale
---------------------------------

Senza esitazione:

11.  Duplica il livello HiRaLoAm e chiamalo “USM” (che sta per Unsharp Mask, ovvero Maschera di Contrasto).
    
12.  Attiva il canale _L_ e applica, indovina cosa, il filtro Maschera di Contrasto con Fattore: 500 – Raggio: 1.4px – Soglia: 0px.
    

Un po’ tanto, eh? Nessun problema, visto che il Maestro (Dan Margulis) insegna che gli aloni più fastidiosi sono quelli chiari - rispetto a quelli scuri. Ci occorre solo un modo per dimezzarli:

13.  Sempre tenendo il canale _L_ di USM come l’unico attivo, Applica il canale _L_ dal livello HiRaLoAm, modalità di fusione Scurisci (Darken), 50% di Opacità.
    

[![Apply L channel from HiRaLoAm darken 50% opacity](http://localhost:8888/wp-content/uploads/2011/11/Apply_ll-150x89.png)](http://localhost:8888/wp-content/uploads/2011/11/Apply_ll.png)In questo modo, gli aloni scuri di USM sono sempre lì, mentre quelli chiari sono stati ridotti del 50% grazie all’intervento del livello HiRaLoAm (che ne era privo). Ci sei fin qui? Ricorda che se hai domande, puoi scrivere nei commenti! L’ultimissimo passaggio è l’applicazione di una maschera che tagli l’effetto nell’acqua sullo sfondo. Potremmo usare il canale _b_ come abbiamo fatto prima, ma c’è un’alternativa che è più efficace - o ugualmente efficace, ma ha il pregio di mostrare come esista sempre più di una soluzione ad ogni problema. Riesci ad indovinare quale?

14.  Duplica il documento senza unire i livelli, chiama il nuovo documento “RGB”. Tieni solo il livello di sfondo, butta tutto il resto. Converti al buon vecchio sRGB.
    
15.  Torna al documento originale in Lab, aggiungi una maschera di livello ad USM, e applicaci il canale _R_ dal documento RGB.
    
16.  Curva la maschera di livello per schiarirla un po’ (quindi facendo passare di più l’effetto dello sharpening - le maschere funzionano così).
    

[![Apply R from RGB duplicate](http://localhost:8888/wp-content/uploads/2011/11/RGB-150x89.png)](http://localhost:8888/wp-content/uploads/2011/11/RGB.png) In questa particolare situazione, vogliamo una maschera che sia nera come la pece nell’acqua, e quindi il canale _R_ è a priori una buona scelta. Ma se prendi il canale _R_ di uno spazio RGB piccolo, uscirà già completamente nero e senza dettaglio nelle sue ombre (l’acqua) per cui qui ed ora: più piccolo è, meglio è. \[caption id="attachment_364" align="alignleft" width="300" caption="La maschera del livello USM: il canale R di un duplicato del documento, convertito in RGB"\]![Red channel as a mask](http://localhost:8888/wp-content/uploads/2011/11/R-mask2.jpg "Red channel as a mask")\[/caption\] \[caption id="attachment_365" align="alignleft" width="215" caption="La palette dei Livelli al termine di questa correzione"\]![Layers palette](http://localhost:8888/wp-content/uploads/2011/11/Layers_small2.png "Layers palette")\[/caption\] Ce l’abbiamo fatta! Terminiamo con un documento a tre livelli, con uno Sfondo che rappresenta la versione coi soli colori corretti, un livello di mezzo con un HiRaLoAm che agisce principalmente sul primo piano, e un livello in cima con la Maschera di Contrasto tradizionale mascherata opportunamente, e i cui aloni chiari sono stati dimezzati. E’ più lunga da scrivere che non da fare, credimi. Pensi che il risultato sia un po' troppo spinto? Sei probabilmente in ottima compagnia, ma la soluzione è a portata di mano: abbassa l'opacità del livello HiRaLoAm e/o USM a tuo piacere - è sempre meglio strafare, potendo aggiustare il tiro con precisione in seguito. Comunque, se per qualsiasi ragione ti fossi perso per strada, usa i commenti a fondo pagina per fare domande e chiedere spiegazioni: farò del mio meglio per farti nuotare in acque basse. \[caption id="attachment_335" align="aligncenter" width="570" caption="Versione finale (clicca sull'immagine per ingrandire) - © Paolo Fossati"\][![Actinia and Anemonefish](http://localhost:8888/wp-content/uploads/2011/11/03-Final_L.jpg "Actinia and Anemonefish")](http://localhost:8888/wp-content/uploads/2011/11/03-Final_H.jpg)\[/caption\] \[caption id="attachment_380" align="aligncenter" width="570" caption="Dettagli - clicca per aprire l'alta risoluzione in una nuova finestra"\][![Details - Final version](http://localhost:8888/wp-content/uploads/2011/11/Details_FINAL.jpg "Details - Final version")](http://localhost:8888/wp-content/uploads/2011/11/Details_FINAL.jpg)\[/caption\]   _**Credits**_ _Ringrazio infinitamente__ [Davide D'Angelo](http://www.davidedangelo.com "Davide D'Angelo Photographer") per il suo aiuto, e__ [Paolo Fossati](http://www.paolo-fossati.com "Paolo Fossati Photographer") che mi ha gentilmente concesso di scegliere ed usare una sua fotografia. Se posso permettermi un consiglio: visita i loro siti, contengono immagini che valgono la pena di essere viste__._