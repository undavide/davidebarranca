---
id: 639
title: 'Subclipse Version Control in Flash Builder: installazione e setup'
date: 2011-12-23T00:15:37+01:00
author: Davide Barranca
excerpt: 'Come installare e impostare un repository locale con Subclipse (plugin per Eclipse / Flash Builder che gestisce il supporto a Subversion, un sistema di controllo versione - VCS). '
layout: post
guid: http://www.davidebarranca.com/?p=639
permalink: /2011/12/version-control-in-flash-builder-subclipse-subversion-installazione-e-setup/
categories:
  - Coding @it
---
<div class="pf-content">
  <p>
    <img class="size-full wp-image-544 alignright" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/Folders.png" alt="Folders" width="204" height="216" srcset="http://localhost:8888/wp-content/uploads/2011/12/Folders.png 204w, http://localhost:8888/wp-content/uploads/2011/12/Folders-150x158.png 150w" sizes="(max-width: 204px) 100vw, 204px" />&#8220;<em>Salva con Nome</em>&#8221; è la più semplice ed usata forma di Version Control &#8211; ovvero la gestione delle revisioni di un documento. Tipo:
  </p>
  
  <ul>
    <li>
      BlogPost.txt
    </li>
    <li>
      BlogPost_V1.txt
    </li>
    <li>
      BlogPost_V2_Links.txt
    </li>
    <li>
      BlogPost_Final.txt
    </li>
    <li>
      BlogPost_Final_OK.txt
    </li>
  </ul>
  
  <p>
    Eccetera. Siccome lo screenshot a destra è una veritiera rappresentazione della mia cartella di sviluppo per <a title="ALCE - Advanced Local Contrast Enhancer for Photoshop" href="http://www.bigano.com/ALCE" target="_blank">ALCE</a>, ho deciso che è giunta l&#8217;ora di adottare una qualche forma di controllo versione, ovvero: <a title="Subclipse" href="http://subclipse.tigris.org/" target="_blank">Subclipse</a> (un plugin per Flash Builder 4.6 che gestisce il supporto a <a title="Subversion" href="http://subversion.apache.org/" target="_blank">Subversion</a>).
  </p>
  
  <p>
    Sto ancora imparando: in questo post ho raccolto una serie di informazioni che spero possano servire da riferimento per l&#8217;installazione ed il setup di un repository locale (cioè non su server) a chi, come me, sta giusto cominciando a mettere un po&#8217; di ordine nel caos.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <h2>
    Subversion e Subclipse
  </h2>
  
  <p>
    Se l&#8217;argomento ti è nuovo, ti consiglio di leggere questa chiarissima <a title="A Visual Guide to Version Control" href="http://betterexplained.com/articles/a-visual-guide-to-version-control/" target="_blank">Visual Guide to Version Control System (VCS)</a>. Spiega tutti i concetti del version control e la sua terminologia.
  </p>
  
  <blockquote>
    <p>
      In pratica, nel paradigma centralizzato (opposto a quello distribuito), definisci un <em>Repository:</em> una cartella (locale o su un server remoto) che contiene una copia di riferimento del tuo codice. Puoi fare <em>Check Out</em> (scaricarlo) sulla tua macchina per sviluppare; poi <em>Sincronizzi col Repository</em> e infine fai <em>Check In</em> (anche detto <em>Commit</em>), ovvero carichi nel repository la tua versione. Il sistema traccia i cambiamenti (come somma di differenze), definisce le revisioni, funge da backup; in più, puoi ramificare lo sviluppo (<em>Branch</em>) ovvero duplicare la versione principale del codice (<em>Trunk</em>) per sperimentare nuove implementazioni &#8211; per poi eventualmente riunirla (<em>Merge</em>). Più una lunga serie di altre funzioni il cui senso ancora mi sfugge.
    </p>
  </blockquote>
  
  <p>
    <a title="Subversion" href="http://subversion.apache.org/" target="_blank">Subversion</a> è un comune sistema di Version Control, conosciuto anche come svn (il tool a riga di comando). Se vuoi sentire parlar male di Subversion, <a title="Linus Torvalds on Git" href="http://www.youtube.com/watch?v=4XpnKHJAok8" target="_blank">Linus Torvalds è un&#8217;ottima fonte</a>. Se ti stai chiedendo perché scegliere Subclipse e non Subversive come plugin per Eclipse (e quindi Flash Builder), vale la pena leggere <a title="SVN plugins for Eclipse" href="http://stackoverflow.com/questions/61320/svn-plugins-for-eclipse-subclipse-vs-subversive" target="_blank">questa Q&A</a>.
  </p>
  
  <h2>
    Installazione
  </h2>
  
  <p>
    In Flash Builder (io uso la versione 4.6 ma sono propenso a credere che valga anche per la 4.5 e 4.0), vai al menu:
  </p>
  
  <p>
    <em>Help &#8211; Eclipse Marketplace</em>
  </p>
  
  <p>
    E nella finestra seguente cerca &#8220;Subclipse&#8221;:
  </p>
  
  <p>
    <img class="alignnone  wp-image-551" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/EclipseMarketplace.png" alt="Eclipse Marketplace" width="570" height="497" srcset="http://localhost:8888/wp-content/uploads/2011/12/EclipseMarketplace.png 609w, http://localhost:8888/wp-content/uploads/2011/12/EclipseMarketplace-150x130.png 150w, http://localhost:8888/wp-content/uploads/2011/12/EclipseMarketplace-300x261.png 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>
  
  <p>
    Clicca il pulsante <em>Install</em> e riavvia Flash Builder. Il che pare sufficiente per considerare Subclipse installato.
  </p>
  
  <h2>
    Setup di un Repository locale
  </h2>
  
  <p>
    Molti dei tutorial che ho letto a questo punto ti mostrano come creare un <em>repo</em> (conosciamo il gergo, eh? ;-)) su un server remoto. Peccato che a me non serva, né ho la più pallida idea di come impostare un server remoto. Tutto quello che mi occorre è una cartella sul mio disco. Dopo un paio di tentativi finiti male (uno dei quali basato su <a href="http://www.mamp.info/en/index.html" title="MAMP" target="_blank">MAMP</a>), ho capito che il processo è:
  </p>
  
  <ol>
    <li>
      <p>
        Identificare una cartella e dire a svn (non Subclipse: svn, il tool a riga di comando) di creare un repository.
      </p>
    </li>
    
    <li>
      <p>
        Puntare Subclipse a quello.
      </p>
    </li>
  </ol>
  
  <p>
    Il che ha senso &#8211; ma senza queste informazioni ti trovi a fissare Flash Builder (come ho fatto io) pensando: e ora che faccio?
  </p>
  
  <h3>
    Creare un repository
  </h3>
  
  <p>
    Subclipse non può creare un repository. Ma non è un grosso problema.
  </p>
  
  <p>
    Poniamo di volere la cartella &#8220;repo&#8221; (che conterrà i repository) nella Home &#8211; userò prima Finder per creare &#8220;repo&#8221; come una cartella qualsiasi. Poi, nel Terminale:
  </p>
  
  <p style="text-align: center">
    <img class="aligncenter size-full wp-image-565" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/Terminal.png" alt="Terminal" width="570" height="137" srcset="http://localhost:8888/wp-content/uploads/2011/12/Terminal.png 570w, http://localhost:8888/wp-content/uploads/2011/12/Terminal-150x36.png 150w, http://localhost:8888/wp-content/uploads/2011/12/Terminal-300x72.png 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>
  
  <p>
    Occorrono un paio di comandi:
  </p>
  
  <pre>cd repo</pre>
  
  <p>
    Il primo entra nella cartella &#8220;repo&#8221; appena creata
  </p>
  
  <pre>svnadmin create ALCE_repo</pre>
  
  <p>
    Il secondo dice a svn di creare, nella cartella &#8220;repo&#8221;, un vero repository chiamato &#8220;ALCE_repo&#8221; (che conterrà il mio progetto ALCE).
  </p>
  
  <div id="attachment_567" style="width: 312px" class="wp-caption alignright">
    <img aria-describedby="caption-attachment-567" class="size-full wp-image-567 " src="http://localhost:8888/wp-content/uploads/2011/12/Repository.png" alt="Repository structure" width="302" height="110" srcset="http://localhost:8888/wp-content/uploads/2011/12/Repository.png 302w, http://localhost:8888/wp-content/uploads/2011/12/Repository-150x54.png 150w, http://localhost:8888/wp-content/uploads/2011/12/Repository-300x109.png 300w" sizes="(max-width: 302px) 100vw, 302px" />
    
    <p id="caption-attachment-567" class="wp-caption-text">
      Questo è un repository
    </p>
  </div>
  
  <p>
    Infatti &#8220;repo&#8221; conterrà i vari repository, uno per ogni progetto che voglio monitorare. Nello screenshot puoi vedere che un repository è ben più di una semplice cartella.
  </p>
  
  <h3>
    Riempire un repository
  </h3>
  
  <p>
    Abbiamo un repo nuovo di zecca: il secondo passo è riempirlo con un progetto di Flash Builder (una CS extension, o qualsiasi altro codice Flash/AIR).
  </p>
  
  <div id="attachment_573" style="width: 117px" class="wp-caption alignleft">
    <a href="http://localhost:8888/wp-content/uploads/2011/12/Perspective.png" target="_blank"><img aria-describedby="caption-attachment-573" class=" wp-image-573" src="http://localhost:8888/wp-content/uploads/2011/12/Perspective-150x209.png" alt="SVN Repository Exploring Perspective" width="107" height="150" srcset="http://localhost:8888/wp-content/uploads/2011/12/Perspective-150x209.png 150w, http://localhost:8888/wp-content/uploads/2011/12/Perspective-214x300.png 214w, http://localhost:8888/wp-content/uploads/2011/12/Perspective.png 237w" sizes="(max-width: 107px) 100vw, 107px" /></a>
    
    <p id="caption-attachment-573" class="wp-caption-text">
      SVN Perspective
    </p>
  </div>
  
  <p>
    In Flash Builder, dal menu <em>Window &#8211; Open Perspective &#8211; Other&#8230;</em> scegli <em>SVN Repository Exploring</em>.
  </p>
  
  <p>
    Questa vista ti da accesso al pannello <em>SVN Repositories</em>, che dovrebbe essere vuoto. Per comunicare a Flash Builder l&#8217;esistenza del repository, fai click destro dentro al pannello (nello spazio bianco) e seleziona <em>New &#8211; Repository Location&#8230;</em> e quindi nella finestra seguente digita:
  </p>
  
  <pre>file:///Users/[your user]/repo/ALCE_repo</pre>
  
  <p>
    <img class="aligncenter size-full wp-image-580" src="http://localhost:8888/wp-content/uploads/2011/12/AddSVNRepository.png" alt="Add SVN Repository" width="568" height="216" srcset="http://localhost:8888/wp-content/uploads/2011/12/AddSVNRepository.png 568w, http://localhost:8888/wp-content/uploads/2011/12/AddSVNRepository-150x57.png 150w, http://localhost:8888/wp-content/uploads/2011/12/AddSVNRepository-300x114.png 300w" sizes="(max-width: 568px) 100vw, 568px" />
  </p>
  
  <p>
    Nota bene che &#8220;<em>file:///&#8221;</em> ha tre slash.
  </p>
  
  <p>
    <img class="alignright size-full wp-image-596" src="http://localhost:8888/wp-content/uploads/2011/12/CreatedRepo.png" alt="Created Repository" width="282" height="87" srcset="http://localhost:8888/wp-content/uploads/2011/12/CreatedRepo.png 282w, http://localhost:8888/wp-content/uploads/2011/12/CreatedRepo-150x46.png 150w" sizes="(max-width: 282px) 100vw, 282px" />
  </p>
  
  <p>
    Una nuova location dovrebbe essere creata, come vedi nello screenshot a destra &#8211; ovvero Flash Builder ora sa che esiste un repository.
  </p>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2011/12/ShareProject.png" target="_blank"><img class="alignleft size-thumbnail wp-image-598" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/ShareProject-150x186.png" alt="Share Project" width="150" height="186" srcset="http://localhost:8888/wp-content/uploads/2011/12/ShareProject-150x186.png 150w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject-241x300.png 241w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject.png 650w" sizes="(max-width: 150px) 100vw, 150px" /></a>Il prossimo ed ultimo passo è di collegare un progetto a questo repo. I comandi Subclipse nel Flash Builder IDE stanno nel menu <em>Team</em>. Per accedervi, torna alla vista standard (Flex o Extension Builder), e nel pannello <em>Package Explorer</em> fai click destro sul progetto che ti interessa aggiungere al repository, quindi seleziona:
  </p>
  
  <p>
    <em>Team &#8211; Share</em>
  </p>
  
  <p>
    Nella finestra seguente scegli <em>SVN</em> come tipo di repository:
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-612" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SVN.png" alt="Share Project - SVN" width="527" height="417" srcset="http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SVN.png 527w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SVN-150x118.png 150w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SVN-300x237.png 300w" sizes="(max-width: 527px) 100vw, 527px" />
  </p>
  
  <p>
    quindi <em>Use existing repository location:</em>
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-613" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelRep.png" alt="Share Project - select repository" width="527" height="417" srcset="http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelRep.png 527w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelRep-150x118.png 150w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelRep-300x237.png 300w" sizes="(max-width: 527px) 100vw, 527px" />
  </p>
  
  <p>
    ed infine <em>Use project name as folder name:</em>
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-614" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelName.png" alt="Share Project - select name" width="527" height="417" srcset="http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelName.png 527w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelName-150x118.png 150w, http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelName-300x237.png 300w" sizes="(max-width: 527px) 100vw, 527px" />
  </p>
  
  <p>
    Non ti preoccupare se la Console riporterà un messaggio di errore:
  </p>
  
  <pre><span style="color: #ff0000">Filesystem has no item svn: URL 'file:///Users/Davide/repo/ALCE_repo/myTest' non-existent in that revision</span></pre>
  
  <p>
    Puoi tranquillamente ignorarlo (il comando successivo, mkdir, creerà la cartella myTest mancante). Secondo la documentazione, a questo punto il progetto sarà <em>committed</em> &#8211; ovvero: i files copiati nel repository.
  </p>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2011/12/Commit.png" target="_blank"><img class="alignleft size-thumbnail wp-image-618" src="http://localhost:8888/wp-content/uploads/2011/12/Commit-150x150.png" alt="Commit" width="150" height="150" srcset="http://localhost:8888/wp-content/uploads/2011/12/Commit-150x150.png 150w, http://localhost:8888/wp-content/uploads/2011/12/Commit-300x300.png 300w, http://localhost:8888/wp-content/uploads/2011/12/Commit.png 800w" sizes="(max-width: 150px) 100vw, 150px" /></a>Per ragioni che mi sfuggono, a me questo non succede, devo farlo manualmente: click destro sul progetto (myTest in questo caso, vedi lo screenshot sulla destra) nel pannello <em>Package Explorer</em>, scegliendo:
  </p>
  
  <p>
    <em>Team &#8211; Commit</em>
  </p>
  
  <p>
    Nella finestra che segue, puoi selezionare quali risorse copiare nel repository &#8211; aggiungendo anche un commento:
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-624" style="border-width: 0px" src="http://localhost:8888/wp-content/uploads/2011/12/CommitComment.png" alt="Commit and comment" width="568" height="500" srcset="http://localhost:8888/wp-content/uploads/2011/12/CommitComment.png 568w, http://localhost:8888/wp-content/uploads/2011/12/CommitComment-150x132.png 150w, http://localhost:8888/wp-content/uploads/2011/12/CommitComment-300x264.png 300w" sizes="(max-width: 568px) 100vw, 568px" />
  </p>
  
  <p>
    E&#8217; tutto! Se il processo è andato a buon fine, la Console dovrebbe contenere alcuni messaggi di log.
  </p>
  
  <p>
    <img class="alignleft size-full wp-image-627" src="http://localhost:8888/wp-content/uploads/2011/12/RepositoryContent.png" alt="Repository content" width="309" height="190" srcset="http://localhost:8888/wp-content/uploads/2011/12/RepositoryContent.png 309w, http://localhost:8888/wp-content/uploads/2011/12/RepositoryContent-150x92.png 150w, http://localhost:8888/wp-content/uploads/2011/12/RepositoryContent-300x184.png 300w" sizes="(max-width: 309px) 100vw, 309px" />Inoltre, vai nel pannello <em>SVN repositories</em> e clicca sul pulsante di refresh (le due frecce gialle) per aggiornare la vista: il repository ora contiene la cartella myTest e tutti i file del progetto.
  </p>
  
  <p>
    Adesso puoi cominciare ad usare il Version Control System &#8211; scrivi il tuo codice e fai <em>Check in</em> (<em>Commit</em>) ogni volta che desideri definire una nuova revisione &#8211; che poi troverai nel pannello <em>History</em>.
  </p>
  
  <p>
    Spero che queste informazioni ti possano far risparmiare un po&#8217; di tempo! Nel prossimo post della serie, mi soffermerò più approfonditamente (ehi, sto ancora imparando!) su Tag, Branches, revisioni, ecc. Quindi&#8230; a presto!
  </p>
  
  <h2>
    Links
  </h2>
  
  <p>
    Cercando di capire come funziona Subclipse e un VCS, ho letto parecchio materiale &#8211; parte del quale può tornare utile anche a te:
  </p>
  
  <ul>
    <li>
      <a title="SVN book" href="http://svnbook.red-bean.com/en/1.7/svn-book.pdf" target="_blank">Version Control with Subversion</a>, pdf del manuale di Subversion (pubblicato anche da O&#8217;Reilly); esiste anche la versione in italiano, ma apparentemente meno aggiornata.
    </li>
    <li>
      Guy Rutemberg blog, <a title="Creating Local SVN Repository" href="http://www.guyrutenberg.com/2007/10/29/creating-local-svn-repository-home-repository/" target="_blank">Creating Local SVN Repository</a> (con il tool a riga di comando svn).
    </li>
    <li>
      Documentazione IBM: <a title="Subversion and Eclipse" href="http://www.ibm.com/developerworks/opensource/library/os-ecl-subversion/#resources" target="_blank">Subversion and Eclipse</a>.
    </li>
    <li>
      North Carolina State University, seminario <a title="Subclipse for Configuration Management" href="http://agile.csc.ncsu.edu/SEMaterials/tutorials/subclipse/index.html#section1_0" target="_blank">Subclipse for Configuration Management</a>.
    </li>
  </ul>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2011/12/version-control-in-flash-builder-subclipse-subversion-installazione-e-setup/" myshare\_title="Subclipse Version Control in Flash Builder: installazione e setup" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->