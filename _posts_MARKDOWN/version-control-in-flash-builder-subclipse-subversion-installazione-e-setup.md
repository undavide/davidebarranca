---
title: " Subclipse Version Control in Flash Builder: installazione e setup\t\t"
url: 639.html
id: 639
comments: false
category:
  - Coding @it
date: 2011-12-23 00:15:37
tags:
---

![Folders](http://localhost:8888/wp-content/uploads/2011/12/Folders.png)"_Salva con Nome_" è la più semplice ed usata forma di Version Control - ovvero la gestione delle revisioni di un documento. Tipo:

*   BlogPost.txt
*   BlogPost_V1.txt
*   BlogPost\_V2\_Links.txt
*   BlogPost_Final.txt
*   BlogPost\_Final\_OK.txt

Eccetera. Siccome lo screenshot a destra è una veritiera rappresentazione della mia cartella di sviluppo per [ALCE](http://www.bigano.com/ALCE "ALCE - Advanced Local Contrast Enhancer for Photoshop"), ho deciso che è giunta l'ora di adottare una qualche forma di controllo versione, ovvero: [Subclipse](http://subclipse.tigris.org/ "Subclipse") (un plugin per Flash Builder 4.6 che gestisce il supporto a [Subversion](http://subversion.apache.org/ "Subversion")). Sto ancora imparando: in questo post ho raccolto una serie di informazioni che spero possano servire da riferimento per l'installazione ed il setup di un repository locale (cioè non su server) a chi, come me, sta giusto cominciando a mettere un po' di ordine nel caos.

Subversion e Subclipse
----------------------

Se l'argomento ti è nuovo, ti consiglio di leggere questa chiarissima [Visual Guide to Version Control System (VCS)](http://betterexplained.com/articles/a-visual-guide-to-version-control/ "A Visual Guide to Version Control"). Spiega tutti i concetti del version control e la sua terminologia.

> In pratica, nel paradigma centralizzato (opposto a quello distribuito), definisci un _Repository:_ una cartella (locale o su un server remoto) che contiene una copia di riferimento del tuo codice. Puoi fare _Check Out_ (scaricarlo) sulla tua macchina per sviluppare; poi _Sincronizzi col Repository_ e infine fai _Check In_ (anche detto _Commit_), ovvero carichi nel repository la tua versione. Il sistema traccia i cambiamenti (come somma di differenze), definisce le revisioni, funge da backup; in più, puoi ramificare lo sviluppo (_Branch_) ovvero duplicare la versione principale del codice (_Trunk_) per sperimentare nuove implementazioni - per poi eventualmente riunirla (_Merge_). Più una lunga serie di altre funzioni il cui senso ancora mi sfugge.

[Subversion](http://subversion.apache.org/ "Subversion") è un comune sistema di Version Control, conosciuto anche come svn (il tool a riga di comando). Se vuoi sentire parlar male di Subversion, [Linus Torvalds è un'ottima fonte](http://www.youtube.com/watch?v=4XpnKHJAok8 "Linus Torvalds on Git"). Se ti stai chiedendo perché scegliere Subclipse e non Subversive come plugin per Eclipse (e quindi Flash Builder), vale la pena leggere [questa Q&A](http://stackoverflow.com/questions/61320/svn-plugins-for-eclipse-subclipse-vs-subversive "SVN plugins for Eclipse").

Installazione
-------------

In Flash Builder (io uso la versione 4.6 ma sono propenso a credere che valga anche per la 4.5 e 4.0), vai al menu: _Help - Eclipse Marketplace_ E nella finestra seguente cerca "Subclipse": ![Eclipse Marketplace](http://localhost:8888/wp-content/uploads/2011/12/EclipseMarketplace.png) Clicca il pulsante _Install_ e riavvia Flash Builder. Il che pare sufficiente per considerare Subclipse installato.

Setup di un Repository locale
-----------------------------

Molti dei tutorial che ho letto a questo punto ti mostrano come creare un _repo_ (conosciamo il gergo, eh? ;-)) su un server remoto. Peccato che a me non serva, né ho la più pallida idea di come impostare un server remoto. Tutto quello che mi occorre è una cartella sul mio disco. Dopo un paio di tentativi finiti male (uno dei quali basato su [MAMP](http://www.mamp.info/en/index.html "MAMP")), ho capito che il processo è:

1.  Identificare una cartella e dire a svn (non Subclipse: svn, il tool a riga di comando) di creare un repository.
    
2.  Puntare Subclipse a quello.
    

Il che ha senso - ma senza queste informazioni ti trovi a fissare Flash Builder (come ho fatto io) pensando: e ora che faccio?

### Creare un repository

Subclipse non può creare un repository. Ma non è un grosso problema. Poniamo di volere la cartella "repo" (che conterrà i repository) nella Home - userò prima Finder per creare "repo" come una cartella qualsiasi. Poi, nel Terminale:

![Terminal](http://localhost:8888/wp-content/uploads/2011/12/Terminal.png)

Occorrono un paio di comandi:

cd repo

Il primo entra nella cartella "repo" appena creata

svnadmin create ALCE_repo

Il secondo dice a svn di creare, nella cartella "repo", un vero repository chiamato "ALCE\_repo" (che conterrà il mio progetto ALCE). \[caption id="attachment\_567" align="alignright" width="302" caption="Questo è un repository"\]![Repository structure](http://localhost:8888/wp-content/uploads/2011/12/Repository.png)\[/caption\] Infatti "repo" conterrà i vari repository, uno per ogni progetto che voglio monitorare. Nello screenshot puoi vedere che un repository è ben più di una semplice cartella.

### Riempire un repository

Abbiamo un repo nuovo di zecca: il secondo passo è riempirlo con un progetto di Flash Builder (una CS extension, o qualsiasi altro codice Flash/AIR). \[caption id="attachment_573" align="alignleft" width="107" caption="SVN Perspective"\][![SVN Repository Exploring Perspective](http://localhost:8888/wp-content/uploads/2011/12/Perspective-150x209.png)](http://localhost:8888/wp-content/uploads/2011/12/Perspective.png)\[/caption\] In Flash Builder, dal menu _Window - Open Perspective - Other..._ scegli _SVN Repository Exploring_. Questa vista ti da accesso al pannello _SVN Repositories_, che dovrebbe essere vuoto. Per comunicare a Flash Builder l'esistenza del repository, fai click destro dentro al pannello (nello spazio bianco) e seleziona _New - Repository Location..._ e quindi nella finestra seguente digita:

file:///Users/\[your user\]/repo/ALCE_repo

![Add SVN Repository](http://localhost:8888/wp-content/uploads/2011/12/AddSVNRepository.png) Nota bene che "_file:///"_ ha tre slash. ![Created Repository](http://localhost:8888/wp-content/uploads/2011/12/CreatedRepo.png) Una nuova location dovrebbe essere creata, come vedi nello screenshot a destra - ovvero Flash Builder ora sa che esiste un repository. [![Share Project](http://localhost:8888/wp-content/uploads/2011/12/ShareProject-150x186.png)](http://localhost:8888/wp-content/uploads/2011/12/ShareProject.png)Il prossimo ed ultimo passo è di collegare un progetto a questo repo. I comandi Subclipse nel Flash Builder IDE stanno nel menu _Team_. Per accedervi, torna alla vista standard (Flex o Extension Builder), e nel pannello _Package Explorer_ fai click destro sul progetto che ti interessa aggiungere al repository, quindi seleziona: _Team - Share_ Nella finestra seguente scegli _SVN_ come tipo di repository: ![Share Project - SVN](http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SVN.png) quindi _Use existing repository location:_ ![Share Project - select repository](http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelRep.png) ed infine _Use project name as folder name:_ ![Share Project - select name](http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelName.png) Non ti preoccupare se la Console riporterà un messaggio di errore:

Filesystem has no item svn: URL 'file:///Users/Davide/repo/ALCE_repo/myTest' non-existent in that revision

Puoi tranquillamente ignorarlo (il comando successivo, mkdir, creerà la cartella myTest mancante). Secondo la documentazione, a questo punto il progetto sarà _committed_ \- ovvero: i files copiati nel repository. [![Commit](http://localhost:8888/wp-content/uploads/2011/12/Commit-150x150.png)](http://localhost:8888/wp-content/uploads/2011/12/Commit.png)Per ragioni che mi sfuggono, a me questo non succede, devo farlo manualmente: click destro sul progetto (myTest in questo caso, vedi lo screenshot sulla destra) nel pannello _Package Explorer_, scegliendo: _Team - Commit_ Nella finestra che segue, puoi selezionare quali risorse copiare nel repository - aggiungendo anche un commento: ![Commit and comment](http://localhost:8888/wp-content/uploads/2011/12/CommitComment.png) E' tutto! Se il processo è andato a buon fine, la Console dovrebbe contenere alcuni messaggi di log. ![Repository content](http://localhost:8888/wp-content/uploads/2011/12/RepositoryContent.png)Inoltre, vai nel pannello _SVN repositories_ e clicca sul pulsante di refresh (le due frecce gialle) per aggiornare la vista: il repository ora contiene la cartella myTest e tutti i file del progetto. Adesso puoi cominciare ad usare il Version Control System - scrivi il tuo codice e fai _Check in_ (_Commit_) ogni volta che desideri definire una nuova revisione - che poi troverai nel pannello _History_. Spero che queste informazioni ti possano far risparmiare un po' di tempo! Nel prossimo post della serie, mi soffermerò più approfonditamente (ehi, sto ancora imparando!) su Tag, Branches, revisioni, ecc. Quindi... a presto!

Links
-----

Cercando di capire come funziona Subclipse e un VCS, ho letto parecchio materiale - parte del quale può tornare utile anche a te:

*   [Version Control with Subversion](http://svnbook.red-bean.com/en/1.7/svn-book.pdf "SVN book"), pdf del manuale di Subversion (pubblicato anche da O'Reilly); esiste anche la versione in italiano, ma apparentemente meno aggiornata.
*   Guy Rutemberg blog, [Creating Local SVN Repository](http://www.guyrutenberg.com/2007/10/29/creating-local-svn-repository-home-repository/ "Creating Local SVN Repository") (con il tool a riga di comando svn).
*   Documentazione IBM: [Subversion and Eclipse](http://www.ibm.com/developerworks/opensource/library/os-ecl-subversion/#resources "Subversion and Eclipse").
*   North Carolina State University, seminario [Subclipse for Configuration Management](http://agile.csc.ncsu.edu/SEMaterials/tutorials/subclipse/index.html#section1_0 "Subclipse for Configuration Management").