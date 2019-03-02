---
title: " Media di più livelli in Photoshop\t\t"
url: 768.html
id: 768
comments: false
category:
  - Photoshop @it
date: 2012-03-04 00:46:45
tags:
---

\[caption id="attachment_754" align="aligncenter" width="570" caption="Tre versioni di una singola immagine, da sinistra: MO, DB, GA."\][![Three_of_a_perfect_pair](http://localhost:8888/wp-content/uploads/2012/03/Three_of_a_perfect_pair.jpg)](http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it)\[/caption\] In Correzione Colore si è recentemente scoperto che produrre più versioni di una singola immagine allo scopo di unirle, fonderle in modo opportuno, è una strategia eccezionalmente efficace. Il che non esclude che l'autore della correzione debba essere una persona sola, come il mio caro amico Marco Olivotto ha scritto in un bel manifesto sulla Correzione Colore collaborativa intitolato [Tre teste sono meglio di una](http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it "Tre teste sono meglio di una - RBG blog"). Sebbene maschere e modalità di fusione siano all'ordine del giorno, il blend più semplice (33-33-33 oppure 25-25-25-25) può non essere così intuitivo da ottenere, a meno che tu non sappia come fare!  Il blend 50-50 è banale: il primo livello in basso va lasciato al 100% di opacità, quello superiore al 50%. Le cose sono diverse per fondere tre livelli, 33-33-33. Saresti forse tentato di impostare la base al 100% e i due livelli sopra al 33% (o forse entrambi al 50%) - ma non è la strada corretta. Supponiamo di avere appunto tre livelli da fondere, e di voler dare ad ognuno di essi il medesimo peso.

### Oggetti Avanzati e Stack

Un metodo è selezionare tutti i tre livelli, creare un Oggetto Avanzato che li contenga, e poi scegliere il menu _Livello > Oggetti avanzati > Metodo serie immagini > Media_ (in inglese: _Layer > Smart Objects > Stack Mode > Mean_). Questa è senza dubbio la via più precisa, anche se gli Oggetti Avanzati possono essere mortalmente lenti/pesanti da gestire, e richiedono una versione Extended di Photoshop.

### Sequenza di Opacità

La strada più veloce (perché siamo sempre di fretta, o magari vogliamo solo vedere se il blend funziona o no nell'economia della correzione) è impostare le varie opacità. ![Layers palette](http://localhost:8888/wp-content/uploads/2012/03/layers.png) Non è così ovvio come assegnarle però. Nel caso di tre livelli soltanto i numeri sono (dal basso verso l'alto) 100%, 50%, 33% come indicato nello screenshot a sinistra - e la formula molto semplice da usare è: **OPACITÀ = 100 / POSIZIONE** Ovvero, il livello di sfondo è in posizione uno, per cui la sua opacità è 100/1 = 100. Il secondo livello è in posizione due, per cui 100/2 = 50. E quindi il terzo livello, sopra gli altri, in posizione tre ha un'opacità di 100/3 = 33. Per risparmiarti la fatica, la progressione delle opacità per 10 livelli è: 100, 50, 33, 25, 20, 17, 14, (12), 11, 10. In verità il 12 dovrebbe essere 13 (perché 100/8 = 12,5), ma non fa una gran differenza. In ogni modo: se hai bisogno di precisione matematica e hai da fondere otto livelli e più, usa il metodo degli Oggetti Avanzati; altrimenti, la scala delle opacità è un ottimo compromesso! Giusto un piccolo suggerimento che può tornare utile di tanto in tanto. E se vuoi sapere come Marco ha unito le tre versioni, leggi il [post originale](http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it "Tre teste sono meglio di una - RBG blog") e scopri perché 1+1+1 > 3. [![Layered Stonehenge](http://localhost:8888/wp-content/uploads/2012/03/Layered-Stonehenge.jpg)](http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/?lang=it)