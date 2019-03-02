---
title: 'HTML Panel Tips #22: Protecting your Code'
date: 2016-07-27T15:21:18+01:00
author: Davide Barranca
excerpt: "How to protect your code (especially, but not only, ExtendScript) in the context of HTML Panels, and keep it away from prying eyes."
layout: post
permalink: /2016/07/html-panel-tips-22-protecting-your-code/
description: "How to protect your code (especially, but not only, ExtendScript) in the context of HTML Panels, and keep it away from prying eyes."
image: /wp-content/uploads/2016/07/obfuscation.jpg
category:
  - CEP
tags:
  - obfuscation
  - HTML Panels Tips
---

About one year ago I had a so-called _aha moment_ and decided to write a book. I had two or three subjects in mind, first choice was _HTML Panels' Licensing Solutions_ – i.e. how to build trial versions, anti-piracy systems, and the like. Luckily, and I really mean: luckily, I changed my mind and tackled a topic appealing to a slightly broader audience: the [HTML Panels Development course](http://htmlpanelsbook.com) was born.

Still, licensing systems in the context of HTML Panels are a soft spot of mine (see my old post about [partial serial number verification](/2015/07/partial-serial-number-verification-system-in-javascript/)), and I wish I had time to write that book! I did build, from my biased point of view, very good prototypes back then: for instance implementing RSA encryption, or server-side automatic licensing files delivery to be used in conjunction with e-commerce providers. Whatever you choose to do, you're protection system must rely on **secured code that nobody can look at** – which is what this article is all about.

![obfuscated](/wp-content/uploads/2016/07/obfuscated.jpg)

## Sealed enough

As a due preamble, there's no such a thing as unbreakable software. Let's admit however that you and I, we're possibly not building stuff that has very much appeal to the international crackers community, no? For whatever reason we like to keep our code hidden (privacy, licensing systems as a form of respect to paid users...), chances are that the malice level of our wannabe pirates is not worldwide top-notch. Neither we want to die in the process of keeping our code protect, and spend too many sleepless nights over that.

As an engineer I collaborate with uses to say, "I would rather spend time building and earning money on new products". So, a fair compromise in my opinion is to do the best that we can, being aware of both **the _context_ our products belong to** (their price, spread, number of users, etc.) and **the _resources_ needed to protect them**. So my own goal is to make it _"sealed enough"_. Where to put the bar, it's up to you.

## The tale of JsxBin

One year ago, I based all my researches and demos around licensing systems upon a very crucial feature of Panels: JsxBin code. HTML and Javascript are commonly considered inherently insecure, due to the discoverability / readability of the code. Not the case of JsxBin, a blob of gibberish nobody's able to understand. So I carefully diverted sensible operations like http calls, cryptography etc. to the ExtendScript layer; a bit of a hassle for several reasons (main one: ExtendScript is stuck, and will probably be 'till the end of the times, to ECMAScript 3 compatibility), but worth the effort. **JsxBin was the closest thing to binary code we had**.

To make things clear, it's not binary at all but more like an advanced form of cyphertext – a sealed box we could trust. In fact we trusted it so much that (despite being the task utterly boring, and illegal too) _one_ would have tried to reverse-engineer it, either (a) because of a very twisted idea of fun (b) to enjoy his/her own failures as the JsxBin safeness confirmation. I know you know JsxBin if you're reading this, but here it is what it looks like:

{% highlight text %}
@JSXBIN@ES@2.0@MyBbyBn0ACJAnASzCjNjFByBWzGiPjCjKjFjDjUCEzEjOjBjNjFDFePiEjBjWjJjE
jFhAiCjBjSjSjBjOjDjBzDjBjHjFEFdhIzHjDjPjVjOjUjSjZFFeFiJjUjBjMjZzFjHjSjFjFjUGNyBn
AMEbyBn0ABJEnAEXzHjXjSjJjUjFjMjOHfjzBhEIfRBFeFiDjJjBjPhBff0DzAJCEnftJGnAEXGfVBfy
BnfABB40BiAABAJByB
{% endhighlight %}

Theory says that, like with any other cyphertext, given an infinite number of plain text and cypher pairs (say, Jsx and corresponding JsxBin), you can find the key to decode it. Which is what ExtendScript Toolkit (aka ESTK) provides you: just start with `var a;` and inspect the resulting JsxBin code, then keep adding complexity and write down your findings. On and on and on... until you're bored to death (unless you're a cryptography enthusiast).

You will understand me if I'm vague here – we're at the dangerous intersection of legal and privacy issues – but lately JsxBin proved to be... not as sealed as we liked to think. I'm not saying that everybody can flip and turn it back into readable code in a snap (can you? can somebody you know?), but **the probability that somebody can decypher your JsxBin isn't zero anymore**. I'd say quite small, but not zero.

## Obfuscation

So what? Well, if your paranoid side isn't zero either, you must think about some other (I'd say "extra") form of protection, namely: obfuscation. Let me address this once and for all: **javascript obfuscation is getting better and better** and, with the balance between efforts, results and context we operate within – that I talked about before – I find it a very good fit. I want to make a couple of important points, though.

First, obfuscation is not going to substitute JsxBin: think about **obfuscation strategies as layers**, that you can overlay one on top of the other. E.g. you can obfuscate your Jsx, then JsxBin it (it can get more complex than that, but it gives you the idea). Second, **beware of free obfuscation services**. It turns out that "Javascript obfuscation" is such a popular query on Google that people are starting to take advantage of it. You can find [articles online](https://blog.sucuri.net/2015/03/why-a-free-obfuscator-is-not-always-free.html) about strange calls made by websites who use JS code that has been obfuscated for free: guess what, _malicious code_ has been injected during the minification/optimization/obfuscation process (go figure where) and this opens a horrid can of worms in terms of security that you don't want to mess with, period.

But again, while Javascript in HTML Panels relies on CEF (the Chromium Embedded Framework) engine to be parsed – and that's the modern Google V8 engine we all love – ExtendScript is an entirely different beast. [I did test minification systems](/2013/08/testing-minified-js-libraries-in-extendscript/) in the past, and my heart broke seeing how almost all the efforts to compress code resulted in no-longer-working code. That is, possibly due to the fact it doesn't know about modern JS syntax, **ExtendScript is remarkably sensitive when it comes to being massaged**.

### Minifiy, Optimize, Obfuscate?

I've been using these terms loosely, but they express different ideas.

**Minification** is the process of compressing your code, usually to be delivered faster over the internet, consuming less bandwidth; depending on the minifier you pick up, variable names can be changed as well, so that:

{% highlight js %}
function f1(arg1, arg2, arg3) {}
function f2() {
   var var1, var2, var3;
}

// becomes:

function f1(a,b,c){}function f2(){var a,b,c;}
{% endhighlight %}


**Optimization**, instead, means that code analysis is performed, and various techniques are applied – such as, and I'm quoting from a paid compression service: "Dictionary compression, a lossless data compression algorithm that uses as dictionary your own JavaScript source code to replace duplicates by a reference to the existing match thus reducing even more its raw byte size." Depending on the service you use, code can be not even shorter, but run faster too.

**Obfuscation** finally is the process to transform the source code to make it harder to understand. As a funny example, here it's what `alert("Hello, JavaScript")` looks like when you express it using [Japanese textual emoticons](http://utf-8.jp/public/aaencode.html?src=alert(%22Hello%2C%20JavaScript%22)

{% highlight text %}
ﾟωﾟﾉ= /｀ｍ´）ﾉ ~┻━┻ //*´∇｀*/ \['_'\]; o=(ﾟｰﾟ) =_=3; c=(ﾟΘﾟ) =(ﾟｰﾟ)-(ﾟｰﾟ); (ﾟДﾟ) =(ﾟΘﾟ)= (o^_^o)/ (o^_^o);(ﾟДﾟ)={ﾟΘﾟ: '_' ,ﾟωﾟﾉ : ((ﾟωﾟﾉ==3) +'_') \[ﾟΘﾟ\] ,ﾟｰﾟﾉ :(ﾟωﾟﾉ+ '_')\[o^_^o -(ﾟΘﾟ)\] ,ﾟДﾟﾉ:((ﾟｰﾟ==3) +'_')\[ﾟｰﾟ\] }; (ﾟДﾟ) \[ﾟΘﾟ\] =((ﾟωﾟﾉ==3) +'_') \[c^_^o\];(ﾟДﾟ) \['c'\] = ((ﾟДﾟ)+'_') \[ (ﾟｰﾟ)+(ﾟｰﾟ)-(ﾟΘﾟ) \];(ﾟДﾟ) \['o'\] = ((ﾟДﾟ)+'_') \[ﾟΘﾟ\];(ﾟoﾟ)=(ﾟДﾟ) \['c'\]+(ﾟДﾟ) \['o'\]+(ﾟωﾟﾉ +'_')\[ﾟΘﾟ\]+ ((ﾟωﾟﾉ==3) +'_') \[ﾟｰﾟ\] + ((ﾟДﾟ) +'_') \[(ﾟｰﾟ)+(ﾟｰﾟ)\]+ ((ﾟｰﾟ==3) +'_') \[ﾟΘﾟ\]+((ﾟｰﾟ==3) +'_') \[(ﾟｰﾟ) - (ﾟΘﾟ)\]+(ﾟДﾟ) \['c'\]+((ﾟДﾟ)+'_') \[(ﾟｰﾟ)+(ﾟｰﾟ)\]+ (ﾟДﾟ) \['o'\]+((ﾟｰﾟ==3) +'_') \[ﾟΘﾟ\];(ﾟДﾟ) \['_'\] =(o^_^o) \[ﾟoﾟ\] \[ﾟoﾟ\];(ﾟεﾟ)=((ﾟｰﾟ==3) +'_') \[ﾟΘﾟ\]+ (ﾟДﾟ) .ﾟДﾟﾉ+((ﾟДﾟ)+'_') \[(ﾟｰﾟ) + (ﾟｰﾟ)\]+((ﾟｰﾟ==3) +'_') \[o^_^o -ﾟΘﾟ\]+((ﾟｰﾟ==3) +'_') \[ﾟΘﾟ\]+ (ﾟωﾟﾉ +'_') \[ﾟΘﾟ\]; (ﾟｰﾟ)+=(ﾟΘﾟ); (ﾟДﾟ)\[ﾟεﾟ\]='\\\'; (ﾟДﾟ).ﾟΘﾟﾉ=(ﾟДﾟ+ ﾟｰﾟ)\[o^_^o -(ﾟΘﾟ)\];(oﾟｰﾟo)=(ﾟωﾟﾉ +'_')\[c^_^o\];(ﾟДﾟ) \[ﾟoﾟ\]='\\"';(ﾟДﾟ) \['_'\] ( (ﾟДﾟ) \['_'\] (ﾟεﾟ+(ﾟДﾟ)\[ﾟoﾟ\]+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ (ﾟｰﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+((ﾟｰﾟ) + (ﾟΘﾟ))+ (c^_^o)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟｰﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟΘﾟ)+ (c^_^o)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ ((ﾟｰﾟ) + (o^_^o))+ (ﾟДﾟ)\[ﾟεﾟ\]+((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟｰﾟ)+ (c^_^o)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟΘﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ ((o^_^o) +(o^_^o))+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (o^_^o)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (o^_^o)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟΘﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ (c^_^o)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ (ﾟｰﾟ)+ (ﾟДﾟ)\[ﾟεﾟ\]+(ﾟｰﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)\[ﾟεﾟ\]+((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟΘﾟ)+ (ﾟДﾟ)\[ﾟoﾟ\]) (ﾟΘﾟ)) ('_');
{% endhighlight %}

Don't ask me how it works, but it does – if you don't believe me, copy & paste that in ESTK and run it. Now imagine the face of somebody opening your JSX and finding there a bunch of Jap emoticons `^_^`

## Services evaluation

First, when testing a service, try to _beautify _the _uglified_ code. You may find out that apparently excellent services such as [this one](http://www.danstools.com/javascript-obfuscate/), which compresses some code from a [blogpost of mine](/2012/11/action-recordable-scripts-in-photoshop/) this way:

{% highlight js %}
eval(function(p,a,c,k,e,d){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--){d[e(c)]=k[c]||e(c)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('1 a,N,J,g,b;N=6(){m h.O=a.X};J=6(){1 j,4;4=1w;h.j=6(1d){5.1e.1y("z A Q","5.1e.1A.1C(1d.O)");a.r=18;m 18};j=h.j;h.Y=6(){1 M;M="1t {      p: 1z {       1W: \'1Q\',        1P: [\'1S\', \'1T\'],       n: \'K\',       E: 1N {},       w: 1M {         1i: 19 {n: \'P\', u: {1p: \'P\'}},          1j: 19 {n: \'1F\', u: {1p: \'1I\'}}       }     }   }";4=7 1L(M);4.n=a.U;4.p.E.1K=11;4.p.w.1i.1k=6(){m 4.12()};4.p.w.1j.1k=6(){g.O=1R(4.p.E.n);j(g);4.12()}};h.Z=6(){5.1H();4.1E();4.1G()};h.14=6(){1 B;B=!!5.x.V;9(B){C(g,5.x,a.F)}};h.10=6(W){1 I;I=D(W,a.F);5.x=I}};a={1U:"1V-1D-1O-1s-1u",X:1B,1v:1x("$$$/1J/26/2r/K=K"),U:"z A Q",F:"z A Q 2s 2t",r:11};g=7 N();b=7 J();9(5.2q===2p.2l){b.Y();b.Z()}15{b.14();b.j(g)}9(!a.r){b.10(g)}r?\'P\':R;6 D(o,s,f){9(R!=f){o=f(o)}1 d=7 2m;1 l=o.y.u.2n;d.13(5.e(\'S\'),s);16(1 i=0;i<l;i++){1 k=o.y.u[i].1X();9(k=="2o"||k=="2u"||k=="2v"||k=="y")2C;1 v=o[k];k=5.2D(k);1n(T(v)){2"2E":d.2B(k,v);c;2"2A":d.13(k,v);c;2"2w":d.2x(k,v);c;1b:{9(v 2y 1r){1 8=7 17;8["H"]=e("#1f");8["%"]=e("#1c");d.2z(k,8[v.G],v.2k)}15{1a(7 1g("1h G 1q D "+T(v)))}}}}m d}6 C(o,d,s,f){1 l=d.V;9(l){1 L=5.e(\'S\');9(d.24(L)&&(s!=d.1o(L)))m}16(1 i=0;i<l;i++){1 k=d.23(i);1 t=d.1Y(k);q=5.1Z(k);1n(t){2 3.20:o[q]=d.21(k);c;2 3.28:o[q]=d.1o(k);c;2 3.29:o[q]=d.2g(k);c;2 3.2h:{1 8=7 17;8[e("#2i")]="H";8[e("#1c")]="%";8[e("#1f")]="H";1 1l=d.2f(k);1 1m=d.2a(k);o[q]=7 1r(1m,8[1l])}c;2 3.2c:2 3.2d:2 3.2j:2 3.2b:2 3.2e:2 3.22:2 3.27:2 3.25:1b:1a(7 1g("1h G 1q C
// Etc. etc.
{% endhighlight %}

...the very same website provides a twin [service](http://www.danstools.com/javascript-beautify/) able to perfectly restore / beautify its own obfuscated code, making the whole obfuscation page... quite pointless – no? Second, always do test your code thoroughly. ExtendScript may seem to work, but in fact it may not.

### Javascript2img

A website giving terrific output – but that you cannot really use with ExtendScript, alas – is [Javascript2img](http://javascript2img.com/). Their idea is to transform your code into something that is also a valid PNG image, so a simple `alert()` gets an extreme makeover:

{% highlight js %}
v58d1dde603be30842b27fd17614124a0=\[ function(vb4604041b2c258763d25f88597bc71e0){return 'e1c1bfebab6bf67d6a890159995b9edf156ac725d1b4829149132045b0d6b1e6f9c02b68';}, function(vb4604041b2c258763d25f88597bc71e0){return v18db97631c1cc5e4fee9f5e108b11b86.createElement(vb4604041b2c258763d25f88597bc71e0);}, function(vb4604041b2c258763d25f88597bc71e0){return vb4604041b2c258763d25f88597bc71e0\[0\].getContext(vb4604041b2c258763d25f88597bc71e0\[1\]);}, function(vb4604041b2c258763d25f88597bc71e0){return vb4604041b2c258763d25f88597bc71e0\[0\].text=vb4604041b2c258763d25f88597bc71e0\[1\];}, function(vb4604041b2c258763d25f88597bc71e0){return null;}, function(vb4604041b2c258763d25f88597bc71e0){'d06b6c54863ac33d12419dd04f7acb85c696f722acdf7b0f35bb848a9f48c54d61c3515d';}, function(vb4604041b2c258763d25f88597bc71e0){return '8d163d76c569f509002af1a489436be60492eb8ef2be9d748f8769c7663a95c6f8055c6d';}, function(vb4604041b2c258763d25f88597bc71e0){vb4604041b2c258763d25f88597bc71e0.style.display='none';return vb4604041b2c258763d25f88597bc71e0;}, function(vb4604041b2c258763d25f88597bc71e0){v9caf9b339af1b9f364bcb2c035c44bbd.onload=vb4604041b2c258763d25f88597bc71e0}, function(vb4604041b2c258763d25f88597bc71e0){v9caf9b339af1b9f364bcb2c035c44bbd.src=vb4604041b2c258763d25f88597bc71e0;}, new Function("vb4604041b2c258763d25f88597bc71e0","return unescape(decodeURIComponent(window.atob(vb4604041b2c258763d25f88597bc71e0)))"), function(vb4604041b2c258763d25f88597bc71e0){vd6f4885c38c918428c6ea3b8b8a687bf=new Function('vb4604041b2c258763d25f88597bc71e0',v58d1dde603be30842b27fd17614124a0\[10\](v0a63761d20b234e464ed87e282c4eec3\[vb4604041b2c258763d25f88597bc71e0\]));return vd6f4885c38c918428c6ea3b8b8a687bf;}\]; vce94aec448332eef9b14d81fb54c7458=\[0,255,0\]; v0a63761d20b234e464ed87e282c4eec3=\[ 'cmV0dXJuJTIwJ2NhbnZhcyclM0I=', 'cmV0dXJuJTIwJ25vbmUnJTNC', 'cmV0dXJuJTIwJzJkJyUzQg==', 'cmV0dXJuJTIwJ3NjcmlwdCclM0I=', '', 've64b33e0a0c89a624e9ea3b58c2594de', 'v78bf4435ddbf6b19616fb171ba4230bd', 'cmV0dXJuJTIwJ2RhdGElM0FpbWFnZSUyRnBuZyUzQmJhc2U2NCUyQyclM0I=', '', 'iVBORw0KGgoAAAANSUhEUgAAAAcAAAAHCAIAAABLMMCEAAAAd0lEQVQImQXBvQpBcQCH4dffSb1JJyWTDM5mk1s69+gWbJRJicEmm/iRjzxPp21bYQ9L2cafWYRSywF/uoIpKXiD6h4b8ohdGWpNdlCO8sSRTPCeFJxp6YYe+UIFVzwlg6Q08IJ3uJAx6ctGqz6p5JTMcS0fOCd/vQ8w1VAqjQYAAAAASUVORK5CYII=', 'cmV0dXJuJTIwdjE4ZGI5NzYzMWMxY2M1ZTRmZWU5ZjVlMTA4YjExYjg2LmdldEVsZW1lbnRCeUlkKHZiNDYwNDA0MWIyYzI1ODc2M2QyNWY4ODU5N2JjNzFlMCklM0I=', 'cmV0dXJuJTIwZG9jdW1lbnQ=',
// Etc. etc.
{% endhighlight %}

And this is the resulting PNG: 

![js2img.png](/wp-content/uploads/2016/07/vQ8w1VAqjQYAAAAASUVORK5CYII.png)

The ExtendScript engine can't really figure the above code out and breaks – I did try also prepending it with all ES5 / ES6 shims ([extendscriptr](https://github.com/ExtendScript/extendscriptr) included), so if you want to try yourself to make it work, let me know in the comments if/how you succeed! Thanks. Nothing prevents you, although, to use javascript2img to obfuscate any other JS code within your panel (in case, be aware that some frameworks such as Angular may require [some extra care](https://docs.angularjs.org/tutorial/step_05#a-note-on-minification)).

### Javascript Obfuscator

This is a paid service, with a free tier – I'll let you decide whether based on your standards it is cheap or expensive: [Javascript Obfuscator](https://javascriptobfuscator.com/) goes from $19 up to $79 _per month_. It has a web interface, but extra options (also from the free tier) are accessible only using their Windows only Gui:

![Javascript Obfuscator](/wp-content/uploads/2016/07/DB-2016-07-27-at-11.55.29.png)

Above, all the settings that I've been able to use in the free tier. Good news: at least with them, ExtendScript code is perfectly fine and happy, so this is definitely an option. According to them, that security level is low – they provide [more advanced features](https://javascriptobfuscator.com/premium-membership.aspx) – yet again, it depends on who you think you're dealing with when protecting your code.

### Js Scrambler

This one offers lot of interesting features, such as Domain Lock and Expiration Date, but it's definitely _pricey_. They offer a time limited demo mode, and plans goes from $35 up to $255 per month (minimum payments accepted: three months). The code works in ExtendScript as well, so it's a good option for your JSX – I'm not in the position to tell you how it compares to Javascript Obfuscator, but to my purposes it ranks OK.

### JsxBlind

Made by the very talented InDesign developer Marc Autret, [JsxBlind](http://www.indiscripts.com/post/2015/12/jsxblind-first-jsxbin-obfuscator-for-extendscript) tackles the issue of code protection from a different perspective. It presupposes JsxBin as the source and outputs JsxBin as well, so you need to:

1.  Export to binary the original, plain Jsx using ESTK. You get a JsxBin file (nothing different from what we're used to).
2.  Process the above JsxBin through JsxBlind. You get another JsxBin file.

According to Marc, _if_ somebody decoded the JsxBlind-ed JsxBin, it would get scrambled variables and function names:

<figure >
	<img src="http://www.indiscripts.com/blog/public/data/jsxblind-first-jsxbin-obfuscator-for-extendscript/en02.png" />
	<figcaption>Image (c) Marc Autret</figcaption>
</figure>

JsxBlind is provided for free – refer to the original blogpost for all the details. Please note that, depending on your coding style, function names obfuscation (which is optional) may or may not work. Moreover, ExtendScript support varies among Creative Cloud applications: Marc is an InDesign developer so that is his platform of choice, Photoshop seems to work as well, but do test your obfuscated code before shipping.

## Protection strategies

I've shown here 4 valid alternatives – let me sum up here my own operative and strategic suggestions.

*   **Keep in mind the reason why you're protecting your code**, and balance the resources you're putting into this process, versus the results you're getting and your actual needs.
*   **JsxBin isn't dead**. The large majority of those who might be involved into the _evil business_ of spying your code don't have any key to open that lock.
*   **Obfuscation methods and providers can be layered**: you could use Javascript Obfuscator, then export as JsxBin, then use JsxBlind.
*   **Do always test your obfuscated code**. ESTK non outputting parsing errors doesn't mean that your scripts will work as expected. For instance don't hardwire variable params inside strings, always use String interpolation (I faced this very issue myself calling `suspendHistory()`)

Please let me know your thoughts in the comments below!

{% include_relative cepBook.md %}
