---
id: 2582
title: 'HTML Panels Tips: #10 Packaging / ZXP Installers'
date: 2014-05-22T21:33:23+01:00
author: Davide Barranca
layout: post
guid: http://www.davidebarranca.com/?p=2582
permalink: /2014/05/html-panels-tips-10-packaging-zxp-installers/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - How to build customized ZXP installers for CC HTML Extensions (standard, flash-compatible, hybrid) using ZXPSignCmd, ucf.jar and MXI files.
image: /wp-content/uploads/2014/05/mxi.png
categories:
  - Coding
  - HTML Panels
tags:
  - installer
  - MXI
  - tip
  - ucf.jar
  - ZXP
  - ZXPSignCmd
---
<div class="pf-content">
  <p>
    While there is a more user friendly app such as <a title="Adobe Creative Cloud Packager" href="http://blogs.adobe.com/oobe/2014/04/creative-cloud-packager-1-5-is-live.html" target="_blank">Adobe Packager</a> which is a great simple app for simple needs, I will show you how to custom build ZXP installers for:
  </p>
  
  <ol>
    <li>
      <strong>HTML Extensions</strong>
    </li>
    <li>
      Bundled <strong>Flash + HTML Extensions</strong>
    </li>
    <li>
      <strong>Hybrid Extensions</strong> (Panel + Plug-ins / Assets, etc)
    </li>
  </ol>
  
  <p>
    using the <code>ZXPSignCmd</code>, the <code>ucf.jar</code> command line tools, and custom MXI files.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    As a primer on Extension installers, you must know that:
  </p>
  
  <ol>
    <li>
      in order to deploy your products (either Panels, Scripts but also assets such as Brushes, Textures, etc) you have to build a ZXP package &#8211; to be submitted to <a title="Adobe Add Ons" href="https://creative.adobe.com/addons/" target="_blank">Adobe Add-ons</a> (the Adobe Exchange web store) or distributed in other ways.
    </li>
    <li>
      ZXP must be signed with either a Paid Certificate (see a list of <a title="Paid Certificates providers" href="https://www.adobeexchange.com/resources/7#signcert" target="_blank">supported providers</a>) or a free, Self-Signed certificate.
    </li>
    <li>
      ZXP must be <a title="Timestamping FAQ" href="https://www.eldos.com/security/articles/5731.php?page=all" target="_blank">timestamped</a>. You can avoid it if you plan to sell your own stuff directly, but for some <a title="Timestamp or not timestamp" href="http://cameronmcefee.com/guideguide-post-mortem-what-i-learned-about-code-signing/" target="_blank">good reasons</a> I strongly suggest you to timestamp. A table showing various scenarios is as follows:
    </li>
  </ol>
  
  <p>
    <img class="aligncenter size-full wp-image-2585" src="http://localhost:8888/wp-content/uploads/2014/05/Signature_timestamp.png" alt="Signature and Timestamping" width="620" height="350" srcset="http://localhost:8888/wp-content/uploads/2014/05/Signature_timestamp.png 620w, http://localhost:8888/wp-content/uploads/2014/05/Signature_timestamp-150x84.png 150w, http://localhost:8888/wp-content/uploads/2014/05/Signature_timestamp-300x169.png 300w" sizes="(max-width: 620px) 100vw, 620px" />
  </p>
  
  <p>
    More info and resources links below, so let&#8217;s start diving.
  </p>
  
  <h2>
    1. Create a Certificate
  </h2>
  
  <p>
    First, grab the <a title="CC Extensions Signing Toolkit" href="http://labs.adobe.com/downloads/extensionbuilder3.html" target="_blank">CC Extensions Signing Toolkit</a>, that is to say the <code>ZXPSignCmd</code> executable file.
  </p>
  
  <p>
    Myself, I will go with a free, self-signed certificate: Adobe Extension Manager will fire a couple of warning popups, but Adobe Add-ons is perfectly fine with it and won&#8217;t complain.
  </p>
  
  <p>
    So: fire the Terminal (or the Win Command line), get to the directory where you&#8217;ve moved the ZXPSignCmd file (if you don&#8217;t know how to do this, just type &#8220;cd &#8221; &#8211; mind you there&#8217;s a space &#8211; in the terminal and drag and drop the folder, then hit Enter) and create a certificate using this pattern:
  </p>
  
  <pre class="theme:solarized-light lang:js highlight:0 decode:true">ZXPSignCmd -selfSignedCert &lt;countryCode&gt; &lt;stateOrProvince&gt; &lt;organization&gt; &lt;commonName&gt; &lt;password&gt; &lt;outputPath.p12&gt;</pre>
  
  <p>
    An actual example with fake data can be:
  </p>
  
  <pre class="theme:solarized-light lang:js highlight:0 decode:true">./ZXPSignCmd -selfSignedCert IT BO DBCompany "Davide Barranca" OcaMorta selfDB.p12</pre>
  
  <p>
    Mind you, <code>./</code> at the beginning instructs the shell to look for the executable in the current directory. If everything went OK you should find a newly created <code>selfDB.p12</code>. That&#8217;s your self signed cert.
  </p>
  
  <h2>
    2. Pack and Sign an HTML Extension
  </h2>
  
  <p>
    <img class="alignleft size-full wp-image-2594" src="http://localhost:8888/wp-content/uploads/2014/05/HTML.png" alt="HTML" width="198" height="65" srcset="http://localhost:8888/wp-content/uploads/2014/05/HTML.png 198w, http://localhost:8888/wp-content/uploads/2014/05/HTML-150x49.png 150w" sizes="(max-width: 198px) 100vw, 198px" />Let&#8217;s assume the extension you&#8217;ve made has the ID <em>com.example.helloworld</em> and it&#8217;s contained within a directory named accordingly.
  </p>
  
  <p>
    Gather together the Extension with the ZXPSignCmd and the selfDB.p12 in one folder, then build the ZXP using this pattern:
  </p>
  
  <pre class="theme:solarized-light lang:js highlight:0 decode:true">ZXPSignCmd -sign &lt;inputDirectory&gt; &lt;outputZxp&gt; &lt;p12&gt; &lt;p12Password&gt; -tsa &lt;timestampURL&gt;</pre>
  
  <p>
    Which in our example (I&#8217;ve put a fake password too) translates in:
  </p>
  
  <pre class="theme:solarized-light lang:js highlight:0 decode:true">./ZXPSignCmd -sign com.example.helloworld com.example.helloworld.zxp selfDB.p12 OcaMorta -tsa https://timestamp.geotrust.com/tsa</pre>
  
  <p>
    If everything&#8217;s OK you should find a newly created <code>com.example.helloworld.zxp</code> file. That&#8217;s your self signed, timestamped installer; submit it to the Add Ons website and/or privately and enjoy.
  </p>
  
  <h2>
    3. Pack and Sign HTML + Flash Extensions
  </h2>
  
  <p>
    Here comes the fun. I assume you have (for the same product) already coded and packed an HTML version, plus the Flash version, so you have:
  </p>
  
  <ul>
    <li>
      <code>com.example.helloworld.zxp</code> (HTML version, packed and signed as shown above)
    </li>
    <li>
      <code>com.example.helloworld.zxp</code> (Flash version, packed and signed with Flash Builder 4.5 as we used to do back in the Flex days)
    </li>
  </ul>
  
  <p>
    Mind you, I use IDs for the ZXP names because in the past Adobe Exchange was a bit picky in that regard &#8211; so it&#8217;s not just <code>hello.zxp</code>. I don&#8217;t know whether things have changed now, try and let me know.
  </p>
  
  <p>
    <img class="alignright size-full wp-image-2598" src="http://localhost:8888/wp-content/uploads/2014/05/HTML_Flash_Folders.png" alt="HTML_Flash_Folders" width="277" height="174" srcset="http://localhost:8888/wp-content/uploads/2014/05/HTML_Flash_Folders.png 277w, http://localhost:8888/wp-content/uploads/2014/05/HTML_Flash_Folders-150x94.png 150w" sizes="(max-width: 277px) 100vw, 277px" />Please grab the <a title="ucf.jar" href="http://www.adobe.com/devnet/creativesuite/sdk/eula_cs6-signing-toolkit.html" target="_blank">ucf.jar command line tool</a> and arrange things in the way shown in the screenshot at right.
  </p>
  
  <p>
    That is to say, put into a folder:
  </p>
  
  <ol>
    <li>
      the selfDB.p12
    </li>
    <li>
      the ucf.jar
    </li>
    <li>
      a 46x46px icon.png (to be used as the icon in Extension Manager)
    </li>
    <li>
      a new MXI folder. Inside the MXI there&#8217;s: <ul>
        <li>
          an HTML folder containing the HTML panel ZXP installer,
        </li>
        <li>
          a FLASH folder containing the Flash panel ZXP installer,
        </li>
        <li>
          a MXI file, which I&#8217;m going to talk about right now.
        </li>
      </ul>
    </li>
  </ol>
  
  <h3>
    MXI file
  </h3>
  
  <p>
    Basically it&#8217;s an XML file containing instruction for the files deployment and lots of metadata. Please refer to this <a title="MXI Reference" href="https://helpx.adobe.com/extension-manager/kb/configuration-file-reference.html" target="_blank">Extension Manager CC Configuration File Reference</a> for extra info and best practices.
  </p>
  
  <p>
    As follows the MXI needed to correctly deploy both Flash and HTML extensions. Mind you, PS CS6 (13.x) and current CC (14.x) are supporting Flash, while HTML is for Photoshop-next (which I assume that is going to be 15).
  </p>
  
  <pre class="lang:xhtml decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;macromedia-extension 
	id="com.example.helloworld"
	icon="icon.png"
	name="Hello World" 
	requires-restart="true"
	version="1.0.0"&gt;

	&lt;author name="Davide Barranca"/&gt;

	&lt;description href="http://localhost:8888"&gt;
		&lt;![CDATA[Here goes the extension description]]&gt;
	&lt;/description&gt;

	 &lt;ui-access&gt;
		&lt;![CDATA[Here you tell the user where she will find your product (in the Window - Extensions menu, or elsewhere)]]&gt;
	&lt;/ui-access&gt;

	&lt;license-agreement&gt;
		&lt;![CDATA[Here goes the EULA]]&gt;
	&lt;/license-agreement&gt;

	&lt;products&gt;
		&lt;product familyname="Photoshop" version="13"/&gt;
	&lt;/products&gt;

	&lt;files&gt;
		&lt;!-- The HTML panel, for 15.0 onwards --&gt;
		&lt;file source="HTML/com.example.helloworld.zxp"
			  destination=""
			  file-type="CSXS"
			  products="Photoshop,Photoshop32,Photoshop64"
			  minVersion="15.0" /&gt;
		
		&lt;!-- The FLASH panel, for 13.0 to 14.9 --&gt;
		&lt;file source="FLASH/com.example.helloworld.zxp"
			  destination=""
			  file-type="CSXS"
			  products="Photoshop,Photoshop32,Photoshop64"
			  minVersion="13.0" maxVersion="14.9" /&gt;

		&lt;!-- Extension Manager icon --&gt;
		&lt;file source="icon.png" destination="$ExtensionSpecificEMStore" /&gt;
	&lt;/files&gt;
&lt;/macromedia-extension&gt;</pre>
  
  <p>
    Couple of things to notice:
  </p>
  
  <ul>
    <li>
      In the <code>product</code> tag, the version is actually the minimum version supported.
    </li>
    <li>
      When the <code>file-type="CSXS"</code> there&#8217;s no need to specify the destination.
    </li>
  </ul>
  
  <p>
    Now, fire the Terminal and:
  </p>
  
  <pre class="theme:solarized-light lang:js highlight:0 decode:true">java -jar ucf.jar -package -storetype PKCS12 -keystore ./selfDB.p12 -tsa http://timestamp.entrust.net/TSS/JavaHttpTS com.sample.helloworld.zxp -C "./MXI/" .</pre>
  
  <p>
    It will ask you for the p12 password, and eventually the <code>com.sample.helloworld.zxp</code> file is built.
  </p>
  
  <p>
    Some other notes:
  </p>
  
  <ul>
    <li>
      You might need to download Java from Oracle
    </li>
    <li>
      I&#8217;ve changed the timestamp url (that&#8217;s another option)
    </li>
    <li>
      There&#8217;s a final dot you have to keep!
    </li>
  </ul>
  
  <p>
    You may ask why the <code>ucf.jar</code> and not <span style="color: rgba(0, 0, 0, 0.74902);"><code>ZXPSignCmd</code>: the reason is that, in order to be CS6 compatible, <code>ucf.jar</code> is the tool of choice.</span>
  </p>
  
  <h2>
    4. Pack and Sign Hybrid Extensions
  </h2>
  
  <p>
    <img class="alignleft size-full wp-image-2604" src="http://localhost:8888/wp-content/uploads/2014/05/hybrid.png" alt="Hybrid MXI" width="268" height="336" srcset="http://localhost:8888/wp-content/uploads/2014/05/hybrid.png 268w, http://localhost:8888/wp-content/uploads/2014/05/hybrid-150x188.png 150w, http://localhost:8888/wp-content/uploads/2014/05/hybrid-239x300.png 239w" sizes="(max-width: 268px) 100vw, 268px" />Hybrid extensions let you install extra content, which can be platform (Mac/Win) and/or version (CS6/CC) and/or bit version (32/64bit) targeted: either Plugins, PDFs, etc.
  </p>
  
  <p>
    In fact the Flash+HTML extension above qualifies as an Hybrid extension too.
  </p>
  
  <p>
    In this example I will bundle a lot of extra content &#8211; keep in mind that all the paths are relative to the MXI file location.
  </p>
  
  <p>
    For the completeness sake, I will then use the <span style="color: rgba(0, 0, 0, 0.74902);"><code>ZXPSignCmd</code> since we will pretend that this one is a CC only Extension.</span>
  </p>
  
  <pre class="lang:xhtml decode:true">&lt;files&gt;
    &lt;!-- The HTML panel, for 15.0 onwards --&gt;
    &lt;file   source="HTML-cs-extensions/com.example.helloworld.zxp"
            destination=""
            file-type="CSXS"
            products="Photoshop,Photoshop32,Photoshop64"
            minVersion="15.0" /&gt;

    &lt;!-- the entire DOCS/ folder content --&gt;
    &lt;file   source="DOCS/"
            destination="$userhomefolder/Documents/HelloWorld_DOCS/" 
            file-type="ordinary" 
            products="Photoshop,Photoshop32,Photoshop64" /&gt;

    &lt;!-- JSX file goes in the Photoshop/Presets/Scripts folder --&gt;
    &lt;file   source="SCRIPTS/"
            destination="$scripts/HelloWorldFolder" 
            file-type="ordinary" 
            products="Photoshop,Photoshop32,Photoshop64" /&gt;

    &lt;!-- MAC plugin --&gt;
    &lt;file   source="MAC/"
            destination="$pluginsfolder/HelloWorldFolder" 
            file-type="ordinary" 
            products="Photoshop,Photoshop32,Photoshop64" 
            platform="mac"/&gt;

    &lt;!-- WIN plugin, 64bit --&gt;
    &lt;file   source="WIN/"
            destination="$pluginsfolder/HelloWorldFolder" 
            file-type="ordinary" 
            products="Photoshop,Photoshop64" 
            platform="win"/&gt;

    &lt;!-- WIN plugin, 32bit --&gt;
    &lt;file   source="WIN/"
            destination="$pluginsfolder/HelloWorldFolder" 
            file-type="ordinary" 
            products="Photoshop,Photoshop64" 
            platform="win"/&gt;

    &lt;!-- Extension Manager icon --&gt;
    &lt;file   source="icon.png"
            destination="$ExtensionSpecificEMStore" /&gt;
&lt;/files&gt;</pre>
  
  <p>
    Then you have to fire the Terminal and as usual:
  </p>
  
  <pre class="theme:solarized-light lang:js highlight:0 decode:true crayon-selected">./ZXPSignCmd -sign MXI com.sample.helloworld.zxp selfDB.p12 OcaMorta -tsa https://timestamp.geotrust.com/tsa</pre>
  
  <p>
    That should hopefully create a ZXP, deploying:
  </p>
  
  <ol>
    <li>
      The HTML Panel
    </li>
    <li>
      General assets (in my case a PDF and an HTML file) in a User&#8217;s Home subfolder
    </li>
    <li>
      C/C++ Plugins (targeting Mac/Win versions, 32 and 64 bits)
    </li>
    <li>
      JSX Scripts in the Photoshop CC/Presets/Scripts custom named subfolder
    </li>
  </ol>
  
  <p>
    Please refer to <a title="Path Tokens" href="https://helpx.adobe.com/extension-manager/kb/path-tokens-extension-manager.html" target="_blank">this page</a> for a description of General and Product Specific Path Tokens.
  </p>
  
  <h2>
    Resources
  </h2>
  
  <p>
    This brief guide is far from being exhaustive &#8211; I&#8217;ve only described things that I know first hand. For a better understanding of the topic, please refer to the following official documentation:
  </p>
  
  <ul>
    <li>
      <a title="ZXPSignCmd download" href="http://labs.adobe.com/downloads/extensionbuilder3.html" target="_blank">ZXPSignCmd</a> download
    </li>
    <li>
      ZXPSignCmd &#8211; <a title="ZXPSignCmd Instruction" href="http://download.macromedia.com/pub/labs/extensionbuilder3/ccextensions.pdf" target="_blank">Instruction</a>
    </li>
    <li>
      ZXPSignCmd &#8211; <a title="ZXPSignCmd Tech Note" href="http://wwwimages.adobe.com/content/dam/Adobe/en/devnet/creativesuite/pdfs/SigningTechNote_CC.pdf" target="_blank">Packaging and Signing Technical Note</a>
    </li>
    <li>
      <a title="ucf.jar download" href="http://www.adobe.com/devnet/creativesuite/sdk/eula_cs6-signing-toolkit.html" target="_blank">ucf.jar</a> download
    </li>
    <li>
      ucf.jar &#8211; <a title="ucf.jar" href="https://www.adobeexchange.com/resources/7#packman" target="_blank">Packaging instructions</a>
    </li>
    <li>
      ucf.jar &#8211; <a title="ucf.jar tech note" href="http://wwwimages.adobe.com/content/dam/Adobe/en/devnet/creativesuite/pdfs/CS_SDK_Guide.pdf" target="_blank">Technical Note</a> (see from pag. 23 onwards)
    </li>
    <li>
      <a title="MXI Reference" href="https://helpx.adobe.com/extension-manager/kb/configuration-file-reference.html" target="_blank">MXI Reference</a> (with path tokens, etc)
    </li>
    <li>
      <a title="EULA" href="https://www.adobeexchange.com/resources/7#eula" target="_blank">EULA Template</a>
    </li>
  </ul>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/05/html-panels-tips-10-packaging-zxp-installers/" myshare\_title="HTML Panels Tips: #10 Packaging / ZXP Installers" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->