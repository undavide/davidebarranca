---
id: 768
title: Media di più livelli in Photoshop
date: 2012-03-04T00:46:45+01:00
author: Davide Barranca
excerpt: 'Media di più livelli in Photoshop: la scala delle opacità o lo stack di Oggetti Avanzati.'
layout: post
guid: http://www.davidebarranca.com/?p=768
permalink: /2012/03/media-di-piu-livelli-in-photoshop-opacita-oggetto-avanzato/
categories:
  - Photoshop @it
---
<div class="pf-content">
  <div id="attachment_754" style="width: 580px" class="wp-caption aligncenter">
    <a href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it" target="_blank"><img aria-describedby="caption-attachment-754" class=" wp-image-754   " style="border-style: initial;border-color: initial;border-width: 0px" src="http://localhost:8888/wp-content/uploads/2012/03/Three_of_a_perfect_pair.jpg" alt="Three_of_a_perfect_pair" width="570" height="376" srcset="http://localhost:8888/wp-content/uploads/2012/03/Three_of_a_perfect_pair-150x99.jpg 150w, http://localhost:8888/wp-content/uploads/2012/03/Three_of_a_perfect_pair-300x199.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" /></a>
    
    <p id="caption-attachment-754" class="wp-caption-text">
      Tre versioni di una singola immagine, da sinistra: MO, DB, GA.
    </p>
  </div>
  
  <p>
    In Correzione Colore si è recentemente scoperto che produrre più versioni di una singola immagine allo scopo di unirle, fonderle in modo opportuno, è una strategia eccezionalmente efficace. Il che non esclude che l&#8217;autore della correzione debba essere una persona sola, come il mio caro amico Marco Olivotto ha scritto in un bel manifesto sulla Correzione Colore collaborativa intitolato <a title="Tre teste sono meglio di una - RBG blog" href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it" target="_blank">Tre teste sono meglio di una</a>.
  </p>
  
  <p>
    Sebbene maschere e modalità di fusione siano all&#8217;ordine del giorno, il blend più semplice (33-33-33 oppure 25-25-25-25) può non essere così intuitivo da ottenere, a meno che tu non sappia come fare!
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    Il blend 50-50 è banale: il primo livello in basso va lasciato al 100% di opacità, quello superiore al 50%. Le cose sono diverse per fondere tre livelli, 33-33-33. Saresti forse tentato di impostare la base al 100% e i due livelli sopra al 33% (o forse entrambi al 50%) &#8211; ma non è la strada corretta.
  </p>
  
  <p>
    Supponiamo di avere appunto tre livelli da fondere, e di voler dare ad ognuno di essi il medesimo peso.
  </p>
  
  <h3>
    Oggetti Avanzati e Stack
  </h3>
  
  <p>
    Un metodo è selezionare tutti i tre livelli, creare un Oggetto Avanzato che li contenga, e poi scegliere il menu <em>Livello > Oggetti avanzati > Metodo serie immagini > Media</em> (in inglese: <em>Layer > Smart Objects > Stack Mode > Mean</em>). Questa è senza dubbio la via più precisa, anche se gli Oggetti Avanzati possono essere mortalmente lenti/pesanti da gestire, e richiedono una versione Extended di Photoshop.
  </p>
  
  <h3>
    Sequenza di Opacità
  </h3>
  
  <p>
    La strada più veloce (perché siamo sempre di fretta, o magari vogliamo solo vedere se il blend funziona o no nell&#8217;economia della correzione) è impostare le varie opacità.
  </p>
  
  <p>
    <img class="alignleft size-full wp-image-756" style="border-style: initial;border-color: initial;border-width: 0px" src="http://localhost:8888/wp-content/uploads/2012/03/layers.png" alt="Layers palette" width="306" height="306" srcset="http://localhost:8888/wp-content/uploads/2012/03/layers.png 306w, http://localhost:8888/wp-content/uploads/2012/03/layers-150x150.png 150w, http://localhost:8888/wp-content/uploads/2012/03/layers-300x300.png 300w" sizes="(max-width: 306px) 100vw, 306px" />
  </p>
  
  <p>
    Non è così ovvio come assegnarle però. Nel caso di tre livelli soltanto i numeri sono (dal basso verso l&#8217;alto) 100%, 50%, 33% come indicato nello screenshot a sinistra &#8211; e la formula molto semplice da usare è:
  </p>
  
  <p>
    <strong>OPACITÀ = 100 / POSIZIONE</strong>
  </p>
  
  <p>
    Ovvero, il livello di sfondo è in posizione uno, per cui la sua opacità è 100/1 = 100. Il secondo livello è in posizione due, per cui 100/2 = 50. E quindi il terzo livello, sopra gli altri, in posizione tre ha un&#8217;opacità di 100/3 = 33.
  </p>
  
  <p>
    Per risparmiarti la fatica, la progressione delle opacità per 10 livelli è: 100, 50, 33, 25, 20, 17, 14, (12), 11, 10.
  </p>
  
  <p>
    In verità il 12 dovrebbe essere 13 (perché 100/8 = 12,5), ma non fa una gran differenza. In ogni modo: se hai bisogno di precisione matematica e hai da fondere otto livelli e più, usa il metodo degli Oggetti Avanzati; altrimenti, la scala delle opacità è un ottimo compromesso!
  </p>
  
  <p>
    Giusto un piccolo suggerimento che può tornare utile di tanto in tanto. E se vuoi sapere come Marco ha unito le tre versioni, leggi il <a title="Tre teste sono meglio di una - RBG blog" href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it" target="_blank">post originale</a> e scopri perché 1+1+1 > 3.
  </p>
  
  <p>
    <a href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it" target="_blank"><img class="alignleft  wp-image-762" style="border-style: initial;border-color: initial;border-width: 0px" src="http://localhost:8888/wp-content/uploads/2012/03/Layered-Stonehenge.jpg" alt="Layered Stonehenge" width="570" height="376" /></a>
  </p>
  
  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2012/03/media-di-piu-livelli-in-photoshop-opacita-oggetto-avanzato/" myshare\_title="Media di più livelli in Photoshop" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->