---
id: 1341
title: Action recordable scripts in Photoshop
date: 2012-11-07T23:16:55+01:00
author: Davide Barranca
excerpt: "In order to build a script that requires user input (from a GUI) and can be recorded into an Action, you must code a particular set of features - I'll be providing a commented example, to be used as a template."
layout: post
guid: http://www.davidebarranca.com/?p=1341
permalink: /2012/11/action-recordable-scripts-in-photoshop/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'How to code a script that requires user input from a GUI and can be recorded into an Action - an example is provided and commented.'
image: /wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34.png
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - Action
  - GUI
  - recordable
  - script
  - tutorial
---
<div class="pf-content">
  <p>
    Scripts, by default, leave traces in the History palette; a common practice is to pack and hide into a labeled, single history step some code (either few commands or entire scripts) this way:
  </p>
  
  <pre class="lang:js decode:true">var lotOfCode = function() {
  // ... perform needed tasks
}

// suspendHistory()
// String: Label that will appear in the History panel
// String: commands to be executed
app.activeDocument.suspendHistory("Secret stuff happening...", "lotOfCode();" );</pre>
  
  <p>
    You can record such a script into an action (using the <em>File &#8211; Scripts &#8211; Browse&#8230;</em> menu item), and play it later.
  </p>
  
  <p>
    Things get more complicate when your script requires some kind of user interaction, say a GUI that asks for an input value. Take the following as an example (this will be the starting point that I&#8217;ll be elaborating throughout this post):
  </p>
  
  <pre class="height-set:true lang:js decode:true" title="Example - GBlur.jsx">// GBlur.jsx

#target photoshop
var Globals, ParamHolder, RecordableGB, rGB;

// store useful values here
Globals = {
  defaultRadius: 10,
  stringTitle: "Recordable Gaussian Blur",
};

// main object
RecordableGB = function() {
  var actualRoutine, win;
  win = null;
  this.CreateDialog = function() {
    // String containing info needed to build the ScriptUI window
    windowResource = "dialog { \
			valuePanel: Panel { \
				orientation: 'column', \
				alignChildren: ['fill', 'top'], \
				text: 'Radius', \
				valueText: EditText {}, \
				buttonsGroup: Group { \
					cancelButton: Button {text: 'cancel', properties: {name: 'cancel'}}, \
					okButton: Button {text: 'Ok', properties: {name: 'ok'}} \
				} \
			} \
		}"
    win = new Window(windowResource);
    // further formatting
    win.text = Globals.stringTitle;
    win.valuePanel.valueText.active = true;
    // button callbacks
    win.valuePanel.buttonsGroup.cancelButton.onClick = function() {
      return win.close();
    };
    win.valuePanel.buttonsGroup.okButton.onClick = function() {
      actualRoutine(Number(win.valuePanel.valueText.text));
      win.close();
    };
  };
  this.runDialog = function() {
    app.bringToFront();
    win.center();
    win.show();
  };
  // routine's core - just a Gaussian Blur as an example
  actualRoutine = function(radius) {
    return app.activeDocument.activeLayer.applyGaussianBlur(radius);
  };
};

// main program
rGB = new RecordableGB();
rGB.CreateDialog();
rGB.runDialog();</pre>
  
  <p>
    It doesn&#8217;t do anything exciting &#8211; it just pops up a ScriptUI modal window asking for a value that will be used as a radius in pixels for a Gaussian Blur filter.
  </p>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34.png"><img class="alignleft size-medium wp-image-1345" title="ExampleScript" src="http://localhost:8888/wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34-300x215.png" alt="ExampleScript" width="300" height="215" srcset="http://localhost:8888/wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34-300x215.png 300w, http://localhost:8888/wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34-150x107.png 150w, http://localhost:8888/wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34.png 348w" sizes="(max-width: 300px) 100vw, 300px" /></a>But&#8230; when you try to embed this into an action (say you&#8217;ve input 32 as the value while recording it), <strong>the action won&#8217;t remember the used value</strong>. Instead, it will keep popping up the GUI asking for a number, each time you play it. Not exactly what you wanted.
  </p>
  
  <p>
    In order to build an &#8220;action-aware&#8221;, user-input required, recordable script you need to implement a specific logic, that also requires a set of specific instructions: I&#8217;ll be using this very code for the GUI, adding and commenting the bits of code required to make it fully working into Actions.
  </p>
  
  <h2>
    Javascript Resource
  </h2>
  
  <p>
    Section 3 of the Photoshop CS6 Javascript Reference deals with the Javascript Resource, a set of instruction that includes, among the rest:
  </p>
  
  <ul>
    <li>
      the ability to specify <strong>a menu the script appears in</strong> as a command [<em>that is, a new item in the Filter / Automate / Help menu</em>]
    </li>
    <li>
      a terminology resource so the script can function with the Action Manager, which <strong>allows your script to record and be automated</strong> by scripting parameters [<em>that is: Actions</em>];
    </li>
  </ul>
  
  <p>
    Third feature should be the possibility to group commands within menus, but it seems to be buggy. I won&#8217;t duplicate the Reference here, let me just report a comment from the Adobe&#8217;s <code>Fit Image.jsx</code> script:
  </p>
  
  <pre class="lang:default decode:true">// Special properties for a JavaScript to enable it to behave like an automation plug-in, 
// the variable name must be exactly as the following example and the variables 
// must be defined in the top 1000 characters of the file</pre>
  
  <p>
    That said, the needed code for my example is as follows:
  </p>
  
  <pre class="lang:default decode:true">/*
@@@BUILDINFO@@@ RecordableGBlur.jsx 1.0
*/
/*
// BEGIN__HARVEST_EXCEPTION_ZSTRING
&lt;javascriptresource&gt;
&lt;name&gt;$$$/RBG/recordableGBlur=Recordable Gaussian Blur...&lt;/name&gt;
&lt;menu&gt;filter&lt;/menu&gt;
&lt;enableinfo&gt;true&lt;/enableinfo&gt;
&lt;eventid&gt;07d2f0b1-653d-11e0-ae3e-0800200c9a66&lt;/eventid&gt;
&lt;terminology&gt;&lt;![CDATA[&lt;&lt;  /Version 1
                          /Events &lt;&lt;
                            /07d2f0b1-653d-11e0-ae3e-0800200c9a66 [($$$/RBG/recordableGBlur=Recordable Gaussian Blur) 
                            /imageReference &lt;&lt;
                              /radius [($$$/Actions/Key/GaussianBlur/Radius=Radius) /uint]
                            &gt;&gt;]
                          &gt;&gt;
                      &gt;&gt; ]]&gt;&lt;/terminology&gt;
&lt;/javascriptresource&gt;
// END__HARVEST_EXCEPTION_ZSTRING
*/</pre>
  
  <p>
    Things to notice (besides the fact that everything is enclosed in comment marks):
  </p>
  
  <p>
    <em>[Line 7]</em> The Label as it will appear in the menu (<code>$$$</code> should refer to localized strings &#8211; even though if no correspondence is found, it keeps using what&#8217;s provided; <code>RBG</code> is a custom string, add your own initial for instance)<br /> <em>[Line 8]</em> The menu the script will appear listed into (Filter in the example)<br /> <em>[Line 9]</em> A conditional that is evaluated in order to determine when to list the script in the menu (for instance only if the document is CMYK, etc) &#8211; in the example set to true, i.e. always.<br /> <em>[Line 10]</em> A unique string that is associated to your script in the Photoshop registry. You can throw in whatever you like, but in order to be sure it won&#8217;t be duplicated, a UUID is recommended &#8211; find a <a title="UUID generator" href="http://www.famkruithof.net/uuid/uuidgen" target="_blank">UUID generator here</a>.<br /> <em>[Line 13]</em> The event as it will be listed in the recorded Action, with its associated string/UUID.<br /> [Line 15] The parameter(s) that will be listed within, and used in, the recorded Action. In the example the only parameter is the radius &#8211; which happens to be included in the localized strings set (otherwise I would have used a customized string as in line 7 that will not be translated).
  </p>
  
  <p>
    Basically, the code says: please, add this script as a new item in the Filter&#8217;s menu and get ready to use the provided labels as the script and parameter&#8217;s name within an Action. But it&#8217;s not enough, we&#8217;re just at the beginning.
  </p>
  
  <h2>
     Script&#8217;s logic
  </h2>
  
  <p>
    In meta-language the main routine has to:
  </p>
  
  <pre class="lang:default decode:true">/*
If the script is called from the menu
    pop-up the dialog 
    read the param values from the GUI
    run the routine
else
    read the param values from the Action
    run the routine
if the routine is done
    save the parameters
*/</pre>
  
  <p>
    That is, the GUI doesn&#8217;t pop-up when the script is called by an action &#8211; it&#8217;s the action itself that has to pass the parameters (actually, you can make an action display the dialogs, toggling the second column of icons in the Actions panel). This translates into actual code as follows:
  </p>
  
  <pre class="lang:default mark:4,7-10,14,18,22-24 decode:true">Globals = {
  defaultRadius: 10,
  stringTitle: "Recordable Gaussian Blur",
  isCancelled = true,
};

var ParamHolder = function() {
  this.radius = undefined;
};
paramHolder = new ParamHolder();

rGB = new RecordableGB();

if (app.playbackDisplayDialogs === DialogModes.ALL) {
  rGB.CreateDialog();
  rGB.runDialog();
} else {
  rGB.initParameters();
  rGB.actualRoutine(paramHolder);
}

if (!Globals.isCancelled) {
  rGB.saveOffParameters(paramHolder);
}</pre>
  
  <p>
    The highlighted lines are new, compared to the starting script.
  </p>
  
  <ul>
    <li>
      In the <code>Globals</code> object I define a new <code>isCancelled</code> key &#8211; which is a <em>toggle</em> that&#8217;s switched to false when the routine is finished; otherwise I assume that the script isn&#8217;t done yet.
    </li>
    <li>
      I instantiate a <code>paramHolder</code> object, which contains a <code>radius</code> property that I&#8217;ll be using to store the value from the GUI. Of course in this example it is just a single parameter, but they can be as many as you need.
    </li>
    <li>
      I&#8217;m checking the <code>app.playbackDisplayDialog</code> property: it controls whether to display the dialog (and what kind of dialogs are allowed) when the script is called by an action. If it&#8217;s equal to <code>DialogModes.ALL</code> it means that either the Action is set to play with all the dialogs on, or the call comes from the menu item; as a result, the GUI must pop up.
    </li>
    <li>
      The <code>.initParameters()</code> function retrieves the parameter from the Action and assign it to the <code>paramHolder</code> object &#8211; which is what the <code>.actualRoutine()</code> is then fed with.
    </li>
    <li>
      When the <code>.actualRoutine()</code> is done, it switches to false the <code>isCancelled</code> key, so that the script saves the <code>paramHolder</code> object (the radius&#8217; container, in the example). If the Action is recording, the parameter is saved inside the Action.
    </li>
  </ul>
  
  <h2>
    Write the Parameter from the Action
  </h2>
  
  <p>
    The script communicates with Actions by means of <strong>Action Descriptors</strong>, special objects that store key-value pairs, used to drive Photoshop at a lower level: for instance when DOM commands aren&#8217;t specified (the Action terms doesn&#8217;t refer to recordable/playable actions, afaik). The so-called <strong>Action Manager</strong> code is kind of the Photoshop&#8217;s voodoo equivalent &#8211; yet when you start to scratch the surface of such an unfriendly mechanism, it&#8217;s less scaring than it appears.
  </p>
  
  <p>
    Anyway: the key point is to pass a descriptor to <code>app.playbackParameters</code> (which is what is going to be stored within the Action itself in order to be played later). This is done in Adobe&#8217;s own scripts by means of the <code>objectToDescriptor()</code> function:
  </p>
  
  <pre class="height-set:true lang:default decode:true">RecordableGB = function() {
// ...
// ... same code as before

  this.saveOffParameters = function(paramObject) {
    var desc;
    desc = objectToDescriptor(paramObject, Globals.stringMessage);
    app.playbackParameters = desc;
  };
}

///////////////////////////////////////////////////////////////////////////////
// Function: objectToDescriptor
// Usage: create an ActionDescriptor from a JavaScript Object
// Input: JavaScript Object (o)
//        object unique string (s)
//        Pre process converter (f)
// Return: ActionDescriptor
// NOTE: Only boolean, string, number and UnitValue are supported, use a pre processor
//       to convert (f) other types to one of these forms.
// REUSE: This routine is used in other scripts. Please update those if you
//        modify. I am not using include or eval statements as I want these
//        scripts self contained.
///////////////////////////////////////////////////////////////////////////////
function objectToDescriptor (o, s, f) {
   if (undefined != f) {
      o = f(o);
   }

   var d = new ActionDescriptor;
   var l = o.reflect.properties.length;
   d.putString( app.charIDToTypeID( 'Msge' ), s );
   for (var i = 0; i &lt; l; i++ ) {
      var k = o.reflect.properties[i].toString();
      if (k == "__proto__" || k == "__count__" || k == "__class__" || k == "reflect")
         continue;
      var v = o[ k ];
      k = app.stringIDToTypeID(k);
      switch ( typeof(v) ) {
         case "boolean":
            d.putBoolean(k, v);
            break;
         case "string":
            d.putString(k, v);
            break;
         case "number":
            d.putDouble(k, v);
            break;
         default:
         {
            if ( v instanceof UnitValue ) {
               var uc = new Object;
               uc["px"] = charIDToTypeID("#Pxl"); // pixelsUnit
               uc["%"] = charIDToTypeID("#Prc"); // unitPercent
               d.putUnitDouble(k, uc[v.type], v.value);
            } else {
               throw( new Error("Unsupported type in objectToDescriptor " + typeof(v) ) );
            }
         }
      }
   }
    return d;
}</pre>
  
  <p>
    As you see from the commented code, you feed the <code>objectToDescriptor()</code> function with an object to be converted (in the example: <code>paramHolder</code>) and a String (in the example, coming from an updated version of the Globals object, &#8220;Recordable Gaussian Blur Action Settings&#8221;), which looks to me as a key that you&#8217;ll be using to retrieve the object back from the descriptor in the next step.
  </p>
  
  <h2>
    Read the Parameter from the Action
  </h2>
  
  <p>
    I&#8217;ll make use of another Adobe made function, <code>descriptorToObject()</code>, to extract <code>paramHolder</code> from the stored descriptor (which is represented by the <code>app.playbackParameters</code>):
  </p>
  
  <pre class="height-set:true lang:default decode:true">RecordableGB = function() {
// ...
// ... same code as before

  this.initParameters = function() {
    var isFromAction;
    isFromAction = !!app.playbackParameters.count; // boolean casting
    if (isFromAction) {
      descriptorToObject(paramHolder, app.playbackParameters, Globals.stringMessage);
    }
  }
}

///////////////////////////////////////////////////////////////////////////////
// Function: descriptorToObject
// Usage: update a JavaScript Object from an ActionDescriptor
// Input: JavaScript Object (o), current object to update (output)
//        Photoshop ActionDescriptor (d), descriptor to pull new params for object from
//        object unique string (s)
//        JavaScript Function (f), post process converter utility to convert
// Return: Nothing, update is applied to passed in JavaScript Object (o)
// NOTE: Only boolean, string, number and UnitValue are supported, use a post processor
//       to convert (f) other types to one of these forms.
// REUSE: This routine is used in other scripts. Please update those if you
//        modify. I am not using include or eval statements as I want these
//        scripts self contained.
///////////////////////////////////////////////////////////////////////////////

function descriptorToObject (o, d, s, f) {
   var l = d.count;
   if (l) {
       var keyMessage = app.charIDToTypeID( 'Msge' );
        if ( d.hasKey(keyMessage) && ( s != d.getString(keyMessage) )) return;
   }
   for (var i = 0; i &lt; l; i++ ) {
      var k = d.getKey(i); // i + 1 ?
      var t = d.getType(k);
      strk = app.typeIDToStringID(k);
      switch (t) {
         case DescValueType.BOOLEANTYPE:
            o[strk] = d.getBoolean(k);
            break;
         case DescValueType.STRINGTYPE:
            o[strk] = d.getString(k);
            break;
         case DescValueType.DOUBLETYPE:
            o[strk] = d.getDouble(k);
            break;
         case DescValueType.UNITDOUBLE:
            {
            var uc = new Object;
            uc[charIDToTypeID("#Rlt")] = "px"; // unitDistance
            uc[charIDToTypeID("#Prc")] = "%"; // unitPercent
            uc[charIDToTypeID("#Pxl")] = "px"; // unitPixels
            var ut = d.getUnitDoubleType(k);
            var uv = d.getUnitDoubleValue(k);
            o[strk] = new UnitValue( uv, uc[ut] );
            }
            break;
         case DescValueType.INTEGERTYPE:
         case DescValueType.ALIASTYPE:
         case DescValueType.CLASSTYPE:
         case DescValueType.ENUMERATEDTYPE:
         case DescValueType.LISTTYPE:
         case DescValueType.OBJECTTYPE:
         case DescValueType.RAWTYPE:
         case DescValueType.REFERENCETYPE:
         default:
            throw( new Error("Unsupported type in descriptorToObject " + t ) );
      }
   }
   if (undefined != f) {
      o = f(o);
   }
}</pre>
  
  <p>
    <code>descriptorToObject()</code> works in a peculiar way, since it takes as a parameter an instance of the object (<code>paramHolder</code>) that he&#8217;s going to update &#8211; with the information extracted from the other parameter (the actual descriptor).
  </p>
  
  <p>
    You see that, in the <code>initParameters()</code> function I define an <code>isFromAction</code> boolean, casting the <code>app.playbackParameters.count</code> property to Boolean. If it&#8217;s zero (i.e. false &#8211; the descriptor has no keys) the script isn&#8217;t run from an Action. In this very case, the count is equal to two, and if you want to know what the keys look like you can:
  </p>
  
  <pre class="lang:default decode:true">var descParameters = app.playbackParameters.count;
for (var i = 0; i&lt; descParameters; i++) {
  alert("Key[" + i + "]: " + app.typeIDToStringID(app.playbackParameters.getKey(i)) + "; ")
}</pre>
  
  <p>
    Mind you, the <code>javascriptresource</code> is used in the process to determine the label that will appear in the Actions palette &#8211; so they must match.
  </p>
  
  <h2>
    Final script
  </h2>
  
  <p>
    The completed example&#8217;s code is as follows (please find a note just below it regarding line 99):
  </p>
  
  <pre class="height-set:true lang:default mark:99 decode:true">// Written by Davide Barranca based on Adobe's FitImage
/*
@@@BUILDINFO@@@ RecordableGBlur.jsx 1.0
*/
/*
// BEGIN__HARVEST_EXCEPTION_ZSTRING
&lt;javascriptresource&gt;
&lt;name&gt;$$$/RBG/recordableGBlur=Recordable Gaussian Blur...&lt;/name&gt;
&lt;menu&gt;filter&lt;/menu&gt;
&lt;enableinfo&gt;true&lt;/enableinfo&gt;
&lt;eventid&gt;07d2f0b1-653d-11e0-ae3e-0800200c9a66&lt;/eventid&gt;
&lt;terminology&gt;&lt;![CDATA[&lt;&lt;  /Version 1
                          /Events &lt;&lt;
                            /07d2f0b1-653d-11e0-ae3e-0800200c9a66 [($$$/RBG/recordableGBlur=Recordable Gaussian Blur) 
                            /imageReference &lt;&lt;
                              /radius [($$$/Actions/Key/GaussianBlur/Radius=Radius) /uint]
                            &gt;&gt;]
                          &gt;&gt;
                      &gt;&gt; ]]&gt;&lt;/terminology&gt;
&lt;/javascriptresource&gt;
// END__HARVEST_EXCEPTION_ZSTRING
*/;

#target photoshop
var Globals, ParamHolder, RecordableGB, paramHolder, rGB;

ParamHolder = function() {
  return this.radius = Globals.defaultRadius;
};

RecordableGB = function() {
  var actualRoutine, win;
  win = null;
  this.actualRoutine = function(param) {
    app.activeDocument.suspendHistory("Recordable Gaussian Blur", "app.activeDocument.activeLayer.applyGaussianBlur(param.radius)");
    Globals.isCancelled = false;
    return false;
  };
  actualRoutine = this.actualRoutine;
  this.CreateDialog = function() {
    var windowResource;
    windowResource = "dialog {			valuePanel: Panel {				orientation: 'column',				alignChildren: ['fill', 'top'],				text: 'Radius',				valueText: EditText {},				buttonsGroup: Group {					cancelButton: Button {text: 'cancel', properties: {name: 'cancel'}},					okButton: Button {text: 'Ok', properties: {name: 'ok'}}				}			}		}";
    win = new Window(windowResource);
    win.text = Globals.stringTitle;
    win.valuePanel.valueText.active = true;
    win.valuePanel.buttonsGroup.cancelButton.onClick = function() {
      return win.close();
    };
    win.valuePanel.buttonsGroup.okButton.onClick = function() {
      paramHolder.radius = Number(win.valuePanel.valueText.text);
      actualRoutine(paramHolder);
      win.close();
    };
  };
  this.runDialog = function() {
    app.bringToFront();
    win.center();
    win.show();
  };
  this.initParameters = function() {
    var isFromAction;
    isFromAction = !!app.playbackParameters.count;
    if (isFromAction) {
      descriptorToObject(paramHolder, app.playbackParameters, Globals.stringMessage);
    }
  };
  this.saveOffParameters = function(paramObject) {
    var desc;
    desc = objectToDescriptor(paramObject, Globals.stringMessage);
    app.playbackParameters = desc;
  };
};

Globals = {
  scriptUUID: "07d2f0b1-653d-11e0-ae3e-0800200c9a66",
  defaultRadius: 30,
  stringRadius: localize("$$$/Actions/Key/GaussianBlur/Radius=Radius"),
  stringTitle: "Recordable Gaussian Blur",
  stringMessage: "Recordable Gaussian Blur Action Settings",
  isCancelled: true
};

paramHolder = new ParamHolder();

rGB = new RecordableGB();

if (app.playbackDisplayDialogs === DialogModes.ALL) {
  rGB.CreateDialog();
  rGB.runDialog();
} else {
  rGB.initParameters();
  rGB.actualRoutine(paramHolder);
}

if (!Globals.isCancelled) {
  rGB.saveOffParameters(paramHolder);
}

isCancelled ? 'cancel' : undefined;

///////////////////////////////////////////////////////////////////////////////
// Function: objectToDescriptor
// Usage: create an ActionDescriptor from a JavaScript Object
// Input: JavaScript Object (o)
//        object unique string (s)
//        Pre process converter (f)
// Return: ActionDescriptor
// NOTE: Only boolean, string, number and UnitValue are supported, use a pre processor
//       to convert (f) other types to one of these forms.
// REUSE: This routine is used in other scripts. Please update those if you
//        modify. I am not using include or eval statements as I want these
//        scripts self contained.
///////////////////////////////////////////////////////////////////////////////
function objectToDescriptor (o, s, f) {
   if (undefined != f) {
      o = f(o);
   }

   var d = new ActionDescriptor;
   var l = o.reflect.properties.length;
   d.putString( app.charIDToTypeID( 'Msge' ), s );
   for (var i = 0; i &lt; l; i++ ) {
      var k = o.reflect.properties[i].toString();
      if (k == "__proto__" || k == "__count__" || k == "__class__" || k == "reflect")
         continue;
      var v = o[ k ];
      k = app.stringIDToTypeID(k);
      switch ( typeof(v) ) {
         case "boolean":
            d.putBoolean(k, v);
            break;
         case "string":
            d.putString(k, v);
            break;
         case "number":
            d.putDouble(k, v);
            break;
         default:
         {
            if ( v instanceof UnitValue ) {
               var uc = new Object;
               uc["px"] = charIDToTypeID("#Pxl"); // pixelsUnit
               uc["%"] = charIDToTypeID("#Prc"); // unitPercent
               d.putUnitDouble(k, uc[v.type], v.value);
            } else {
               throw( new Error("Unsupported type in objectToDescriptor " + typeof(v) ) );
            }
         }
      }
   }
    return d;
}

///////////////////////////////////////////////////////////////////////////////
// Function: descriptorToObject
// Usage: update a JavaScript Object from an ActionDescriptor
// Input: JavaScript Object (o), current object to update (output)
//        Photoshop ActionDescriptor (d), descriptor to pull new params for object from
//        object unique string (s)
//        JavaScript Function (f), post process converter utility to convert
// Return: Nothing, update is applied to passed in JavaScript Object (o)
// NOTE: Only boolean, string, number and UnitValue are supported, use a post processor
//       to convert (f) other types to one of these forms.
// REUSE: This routine is used in other scripts. Please update those if you
//        modify. I am not using include or eval statements as I want these
//        scripts self contained.
///////////////////////////////////////////////////////////////////////////////

function descriptorToObject (o, d, s, f) {
   var l = d.count;
   if (l) {
       var keyMessage = app.charIDToTypeID( 'Msge' );
        if ( d.hasKey(keyMessage) && ( s != d.getString(keyMessage) )) return;
   }
   for (var i = 0; i &lt; l; i++ ) {
      var k = d.getKey(i); // i + 1 ?
      var t = d.getType(k);
      strk = app.typeIDToStringID(k);
      switch (t) {
         case DescValueType.BOOLEANTYPE:
            o[strk] = d.getBoolean(k);
            break;
         case DescValueType.STRINGTYPE:
            o[strk] = d.getString(k);
            break;
         case DescValueType.DOUBLETYPE:
            o[strk] = d.getDouble(k);
            break;
         case DescValueType.UNITDOUBLE:
            {
            var uc = new Object;
            uc[charIDToTypeID("#Rlt")] = "px"; // unitDistance
            uc[charIDToTypeID("#Prc")] = "%"; // unitPercent
            uc[charIDToTypeID("#Pxl")] = "px"; // unitPixels
            var ut = d.getUnitDoubleType(k);
            var uv = d.getUnitDoubleValue(k);
            o[strk] = new UnitValue( uv, uc[ut] );
            }
            break;
         case DescValueType.INTEGERTYPE:
         case DescValueType.ALIASTYPE:
         case DescValueType.CLASSTYPE:
         case DescValueType.ENUMERATEDTYPE:
         case DescValueType.LISTTYPE:
         case DescValueType.OBJECTTYPE:
         case DescValueType.RAWTYPE:
         case DescValueType.REFERENCETYPE:
         default:
            throw( new Error("Unsupported type in descriptorToObject " + t ) );
      }
   }
   if (undefined != f) {
      o = f(o);
   }
};</pre>
  
  <p>
    Regarding line 99, the always helpful Mike Hale from <a title="PS-Scripts" href="http://www.ps-scripts.com" target="_blank">Ps-scripts</a> pointed out the following:
  </p>
  
  <blockquote>
    <p>
      it&#8217;s important that the function return the un-localized string &#8216;cancel&#8217; if there is an error so an action that is recording the script will know something went wrong and not store the step in the action. It should return undefined if everything went ok and the step should be recorded.
    </p>
  </blockquote>
  
  <h2>
    Code Protection
  </h2>
  
  <p>
    Be aware that if you&#8217;re willing to protect your work via binary encoding, <strong>you must leave alone everything</strong> from the beginning to the <code>#target photoshop</code> (included). Basically you have to:
  </p>
  
  <ol>
    <li>
      Put the code (without the <code>&lt;javascriptresource&gt;</code>) on a temporary file, and export it (<em>File &#8211; Export to Binary&#8230;</em>)
    </li>
    <li>
      Open the resulting .jsxbin and add a backslash to each line ending:
    </li>
  </ol>
  
  <pre class="lang:default decode:true">// From this:

@JSXBIN@ES@2.0@MyBbyBnACM2jKFbyBn0AGO2jLFby2jMFn0ABJ2jMFnASzBjPBGEVzBjGCfIRBVBfG
ffnffACzChBhdDjzJjVjOjEjFjGjJjOjFjEEfVCfInnnJ2jPFnASzBjEFAEjzQiBjDjUjJjPjOiEjFj
TjDjSjJjQjUjPjSGfntnftJ2jQFnASzBjMHBXzGjMjFjOjHjUjIIfXzKjQjSjPjQjFjSjUjJjFjTJfX
zHjSjFjGjMjFjDjUKfVBfGnftJ2jRFnAEXzJjQjVjUiTjUjSjJjOjHLfVFfARCEXzOjDjIjBjSiJiEi
//....

// To This:

@JSXBIN@ES@2.0@MyBbyBnACM2jKFbyBn0AGO2jLFby2jMFn0ABJ2jMFnASzBjPBGEVzBjGCfIRBVBfG\
ffnffACzChBhdDjzJjVjOjEjFjGjJjOjFjEEfVCfInnnJ2jPFnASzBjEFAEjzQiBjDjUjJjPjOiEjFj\
TjDjSjJjQjUjPjSGfntnftJ2jQFnASzBjMHBXzGjMjFjOjHjUjIIfXzKjQjSjPjQjFjSjUjJjFjTJfX\
zHjSjFjGjMjFjDjUKfVBfGnftJ2jRFnAEXzJjQjVjUiTjUjSjJjOjHLfVFfARCEXzOjDjIjBjSiJiEi\
//... etc.</pre>
  
  <ol start="3">
    <li>
       Copy the binary file&#8217;s content and put everything inside an <code>eval("...")</code> function, then add <strong>before it</strong> the <code>&lt;javascriptresource&gt;</code>:
    </li>
  </ol>
  
  <pre class="lang:default decode:true">// Written by Davide Barranca based on Adobe's FitImage
/*
@@@BUILDINFO@@@ RecordableGBlur.jsx 1.0
*/
/*
// BEGIN__HARVEST_EXCEPTION_ZSTRING
&lt;javascriptresource&gt;
&lt;name&gt;$$$/RBG/recordableGBlur=Recordable Gaussian Blur...&lt;/name&gt;
&lt;menu&gt;filter&lt;/menu&gt;
&lt;enableinfo&gt;true&lt;/enableinfo&gt;
&lt;eventid&gt;07d2f0b1-653d-11e0-ae3e-0800200c9a66&lt;/eventid&gt;
&lt;terminology&gt;&lt;![CDATA[&lt;&lt;  /Version 1
                          /Events &lt;&lt;
                            /07d2f0b1-653d-11e0-ae3e-0800200c9a66 [($$$/RBG/recordableGBlur=Recordable Gaussian Blur) 
                            /imageReference &lt;&lt;
                              /radius [($$$/Actions/Key/GaussianBlur/Radius=Radius) /uint]
                            &gt;&gt;]
                          &gt;&gt;
                      &gt;&gt; ]]&gt;&lt;/terminology&gt;
&lt;/javascriptresource&gt;
// END__HARVEST_EXCEPTION_ZSTRING
*/;

#target photoshop

eval("@JSXBIN@ES@2.0@MyBbyBnACM2jKFbyBn0AGO2jLFby2jMFn0ABJ2jMFnASzBjPBGEVzBjGCfIRBVBfG\
ffnffACzChBhdDjzJjVjOjEjFjGjJjOjFjEEfVCfInnnJ2jPFnASzBjEFAEjzQiBjDjUjJjPjOiEjFj\
TjDjSjJjQjUjPjSGfntnftJ2jQFnASzBjMHBXzGjMjFjOjHjUjIIfXzKjQjSjPjQjFjSjUjJjFjTJfX\
zHjSjFjGjMjFjDjUKfVBfGnftJ2jRFnAEXzJjQjVjUiTjUjSjJjOjHLfVFfARCEXzOjDjIjBjSiJiEi\
// ...
// ...
B4J0AiAiR4K0AiAjJ4M0AiAjI4N0AiAjC4O0AiAnO4P0AiAnG4Q0AiAnJ4R0AiA2OB4S0AiA2hDB4T0\
AiAia4U0AiAiS4V0AiAnS4W0AiAmQ4X0AiAlf4Y0AiAiH4Z0AiAiI4ga0AiA2kQB4gb0AiAiY4gc0Ai\
AiX4ge0AiAiW4gf0AiAjG4hA0AiAiZ4hB0AiAiF4hD0AiAie4hC0AiAAhEARByB");</pre>
  
  <p>
    <em>[I&#8217;d like to thank Mike Hale and Xbytor from <a title="Ps-Scripts forum" href="http://www.ps-scripts.com" target="_blank">ps-scripts.com</a> who helped me understanding how this stuff works!]</em>
  </p>
  
  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2012/11/action-recordable-scripts-in-photoshop/" myshare\_title="Action recordable scripts in Photoshop" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->