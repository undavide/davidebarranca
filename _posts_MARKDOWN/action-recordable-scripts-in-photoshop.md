---
title: " Action recordable scripts in Photoshop\t\t"
tags:
  - Action
  - GUI
  - recordable
  - script
  - tutorial
url: 1341.html
id: 1341
category:
  - Coding
  - ExtendScript / Javascript
date: 2012-11-07 23:16:55
---

Scripts, by default, leave traces in the History palette; a common practice is to pack and hide into a labeled, single history step some code (either few commands or entire scripts) this way:

var lotOfCode = function() {
  // ... perform needed tasks
}

// suspendHistory()
// String: Label that will appear in the History panel
// String: commands to be executed
app.activeDocument.suspendHistory("Secret stuff happening...", "lotOfCode();" );

You can record such a script into an action (using the _File - Scripts - Browse..._ menu item), and play it later. Things get more complicate when your script requires some kind of user interaction, say a GUI that asks for an input value. Take the following as an example (this will be the starting point that I'll be elaborating throughout this post):

// GBlur.jsx

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
				alignChildren: \['fill', 'top'\], \
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
rGB.runDialog();

It doesn't do anything exciting - it just pops up a ScriptUI modal window asking for a value that will be used as a radius in pixels for a Gaussian Blur filter. [![ExampleScript](http://localhost:8888/wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34-300x215.png "ExampleScript")](http://localhost:8888/wp-content/uploads/2012/11/DB_-2012-11-04-at-17.24.34.png)But... when you try to embed this into an action (say you've input 32 as the value while recording it), **the action won't remember the used value**. Instead, it will keep popping up the GUI asking for a number, each time you play it. Not exactly what you wanted. In order to build an "action-aware", user-input required, recordable script you need to implement a specific logic, that also requires a set of specific instructions: I'll be using this very code for the GUI, adding and commenting the bits of code required to make it fully working into Actions.

Javascript Resource
-------------------

Section 3 of the Photoshop CS6 Javascript Reference deals with the Javascript Resource, a set of instruction that includes, among the rest:

*   the ability to specify **a menu the script appears in** as a command \[_that is, a new item in the Filter / Automate / Help menu_\]
*   a terminology resource so the script can function with the Action Manager, which **allows your script to record and be automated** by scripting parameters \[_that is: Actions_\];

Third feature should be the possibility to group commands within menus, but it seems to be buggy. I won't duplicate the Reference here, let me just report a comment from the Adobe's `Fit Image.jsx` script:

// Special properties for a JavaScript to enable it to behave like an automation plug-in, 
// the variable name must be exactly as the following example and the variables 
// must be defined in the top 1000 characters of the file

That said, the needed code for my example is as follows:

/*
@@@BUILDINFO@@@ RecordableGBlur.jsx 1.0
*/
/*
// BEGIN\_\_HARVEST\_EXCEPTION_ZSTRING
<javascriptresource>
<name>$$$/RBG/recordableGBlur=Recordable Gaussian Blur...</name>
<menu>filter</menu>
<enableinfo>true</enableinfo>
<eventid>07d2f0b1-653d-11e0-ae3e-0800200c9a66</eventid>
<terminology><!\[CDATA\[<<  /Version 1
                          /Events <<
                            /07d2f0b1-653d-11e0-ae3e-0800200c9a66 \[($$$/RBG/recordableGBlur=Recordable Gaussian Blur) 
                            /imageReference <<
                              /radius \[($$$/Actions/Key/GaussianBlur/Radius=Radius) /uint\]
                            >>\]
                          >>
                      >\> \]\]></terminology>
</javascriptresource>
// END\_\_HARVEST\_EXCEPTION_ZSTRING
*/

Things to notice (besides the fact that everything is enclosed in comment marks): _\[Line 7\]_ The Label as it will appear in the menu (`$$$` should refer to localized strings - even though if no correspondence is found, it keeps using what's provided; `RBG` is a custom string, add your own initial for instance) _\[Line 8\]_ The menu the script will appear listed into (Filter in the example) _\[Line 9\]_ A conditional that is evaluated in order to determine when to list the script in the menu (for instance only if the document is CMYK, etc) - in the example set to true, i.e. always. _\[Line 10\]_ A unique string that is associated to your script in the Photoshop registry. You can throw in whatever you like, but in order to be sure it won't be duplicated, a UUID is recommended - find a [UUID generator here](http://www.famkruithof.net/uuid/uuidgen "UUID generator"). _\[Line 13\]_ The event as it will be listed in the recorded Action, with its associated string/UUID. \[Line 15\] The parameter(s) that will be listed within, and used in, the recorded Action. In the example the only parameter is the radius - which happens to be included in the localized strings set (otherwise I would have used a customized string as in line 7 that will not be translated). Basically, the code says: please, add this script as a new item in the Filter's menu and get ready to use the provided labels as the script and parameter's name within an Action. But it's not enough, we're just at the beginning.

 Script's logic
---------------

In meta-language the main routine has to:

/*
If the script is called from the menu
    pop-up the dialog 
    read the param values from the GUI
    run the routine
else
    read the param values from the Action
    run the routine
if the routine is done
    save the parameters
*/

That is, the GUI doesn't pop-up when the script is called by an action - it's the action itself that has to pass the parameters (actually, you can make an action display the dialogs, toggling the second column of icons in the Actions panel). This translates into actual code as follows:

Globals = {
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
}

The highlighted lines are new, compared to the starting script.

*   In the `Globals` object I define a new `isCancelled` key - which is a _toggle_ that's switched to false when the routine is finished; otherwise I assume that the script isn't done yet.
*   I instantiate a `paramHolder` object, which contains a `radius` property that I'll be using to store the value from the GUI. Of course in this example it is just a single parameter, but they can be as many as you need.
*   I'm checking the `app.playbackDisplayDialog` property: it controls whether to display the dialog (and what kind of dialogs are allowed) when the script is called by an action. If it's equal to `DialogModes.ALL` it means that either the Action is set to play with all the dialogs on, or the call comes from the menu item; as a result, the GUI must pop up.
*   The `.initParameters()` function retrieves the parameter from the Action and assign it to the `paramHolder` object - which is what the `.actualRoutine()` is then fed with.
*   When the `.actualRoutine()` is done, it switches to false the `isCancelled` key, so that the script saves the `paramHolder` object (the radius' container, in the example). If the Action is recording, the parameter is saved inside the Action.

Write the Parameter from the Action
-----------------------------------

The script communicates with Actions by means of **Action Descriptors**, special objects that store key-value pairs, used to drive Photoshop at a lower level: for instance when DOM commands aren't specified (the Action terms doesn't refer to recordable/playable actions, afaik). The so-called **Action Manager** code is kind of the Photoshop's voodoo equivalent - yet when you start to scratch the surface of such an unfriendly mechanism, it's less scaring than it appears. Anyway: the key point is to pass a descriptor to `app.playbackParameters` (which is what is going to be stored within the Action itself in order to be played later). This is done in Adobe's own scripts by means of the `objectToDescriptor()` function:

RecordableGB = function() {
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
   for (var i = 0; i < l; i++ ) {
      var k = o.reflect.properties\[i\].toString();
      if (k == "\_\_proto\_\_" || k == "\_\_count\_\_" || k == "\_\_class\_\_" || k == "reflect")
         continue;
      var v = o\[ k \];
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
               uc\["px"\] = charIDToTypeID("#Pxl"); // pixelsUnit
               uc\["%"\] = charIDToTypeID("#Prc"); // unitPercent
               d.putUnitDouble(k, uc\[v.type\], v.value);
            } else {
               throw( new Error("Unsupported type in objectToDescriptor " + typeof(v) ) );
            }
         }
      }
   }
    return d;
}

As you see from the commented code, you feed the `objectToDescriptor()` function with an object to be converted (in the example: `paramHolder`) and a String (in the example, coming from an updated version of the Globals object, "Recordable Gaussian Blur Action Settings"), which looks to me as a key that you'll be using to retrieve the object back from the descriptor in the next step.

Read the Parameter from the Action
----------------------------------

I'll make use of another Adobe made function, `descriptorToObject()`, to extract `paramHolder` from the stored descriptor (which is represented by the `app.playbackParameters`):

RecordableGB = function() {
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
   for (var i = 0; i < l; i++ ) {
      var k = d.getKey(i); // i + 1 ?
      var t = d.getType(k);
      strk = app.typeIDToStringID(k);
      switch (t) {
         case DescValueType.BOOLEANTYPE:
            o\[strk\] = d.getBoolean(k);
            break;
         case DescValueType.STRINGTYPE:
            o\[strk\] = d.getString(k);
            break;
         case DescValueType.DOUBLETYPE:
            o\[strk\] = d.getDouble(k);
            break;
         case DescValueType.UNITDOUBLE:
            {
            var uc = new Object;
            uc\[charIDToTypeID("#Rlt")\] = "px"; // unitDistance
            uc\[charIDToTypeID("#Prc")\] = "%"; // unitPercent
            uc\[charIDToTypeID("#Pxl")\] = "px"; // unitPixels
            var ut = d.getUnitDoubleType(k);
            var uv = d.getUnitDoubleValue(k);
            o\[strk\] = new UnitValue( uv, uc\[ut\] );
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
}

`descriptorToObject()` works in a peculiar way, since it takes as a parameter an instance of the object (`paramHolder`) that he's going to update - with the information extracted from the other parameter (the actual descriptor). You see that, in the `initParameters()` function I define an `isFromAction` boolean, casting the `app.playbackParameters.count` property to Boolean. If it's zero (i.e. false - the descriptor has no keys) the script isn't run from an Action. In this very case, the count is equal to two, and if you want to know what the keys look like you can:

var descParameters = app.playbackParameters.count;
for (var i = 0; i< descParameters; i++) {
  alert("Key\[" + i + "\]: " + app.typeIDToStringID(app.playbackParameters.getKey(i)) + "; ")
}

Mind you, the `javascriptresource` is used in the process to determine the label that will appear in the Actions palette - so they must match.

Final script
------------

The completed example's code is as follows (please find a note just below it regarding line 99):

// Written by Davide Barranca based on Adobe's FitImage
/*
@@@BUILDINFO@@@ RecordableGBlur.jsx 1.0
*/
/*
// BEGIN\_\_HARVEST\_EXCEPTION_ZSTRING
<javascriptresource>
<name>$$$/RBG/recordableGBlur=Recordable Gaussian Blur...</name>
<menu>filter</menu>
<enableinfo>true</enableinfo>
<eventid>07d2f0b1-653d-11e0-ae3e-0800200c9a66</eventid>
<terminology><!\[CDATA\[<<  /Version 1
                          /Events <<
                            /07d2f0b1-653d-11e0-ae3e-0800200c9a66 \[($$$/RBG/recordableGBlur=Recordable Gaussian Blur) 
                            /imageReference <<
                              /radius \[($$$/Actions/Key/GaussianBlur/Radius=Radius) /uint\]
                            >>\]
                          >>
                      >\> \]\]></terminology>
</javascriptresource>
// END\_\_HARVEST\_EXCEPTION_ZSTRING
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
    windowResource = "dialog {			valuePanel: Panel {				orientation: 'column',				alignChildren: \['fill', 'top'\],				text: 'Radius',				valueText: EditText {},				buttonsGroup: Group {					cancelButton: Button {text: 'cancel', properties: {name: 'cancel'}},					okButton: Button {text: 'Ok', properties: {name: 'ok'}}				}			}		}";
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
   for (var i = 0; i < l; i++ ) {
      var k = o.reflect.properties\[i\].toString();
      if (k == "\_\_proto\_\_" || k == "\_\_count\_\_" || k == "\_\_class\_\_" || k == "reflect")
         continue;
      var v = o\[ k \];
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
               uc\["px"\] = charIDToTypeID("#Pxl"); // pixelsUnit
               uc\["%"\] = charIDToTypeID("#Prc"); // unitPercent
               d.putUnitDouble(k, uc\[v.type\], v.value);
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
   for (var i = 0; i < l; i++ ) {
      var k = d.getKey(i); // i + 1 ?
      var t = d.getType(k);
      strk = app.typeIDToStringID(k);
      switch (t) {
         case DescValueType.BOOLEANTYPE:
            o\[strk\] = d.getBoolean(k);
            break;
         case DescValueType.STRINGTYPE:
            o\[strk\] = d.getString(k);
            break;
         case DescValueType.DOUBLETYPE:
            o\[strk\] = d.getDouble(k);
            break;
         case DescValueType.UNITDOUBLE:
            {
            var uc = new Object;
            uc\[charIDToTypeID("#Rlt")\] = "px"; // unitDistance
            uc\[charIDToTypeID("#Prc")\] = "%"; // unitPercent
            uc\[charIDToTypeID("#Pxl")\] = "px"; // unitPixels
            var ut = d.getUnitDoubleType(k);
            var uv = d.getUnitDoubleValue(k);
            o\[strk\] = new UnitValue( uv, uc\[ut\] );
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
};

Regarding line 99, the always helpful Mike Hale from [Ps-scripts](http://www.ps-scripts.com "PS-Scripts") pointed out the following:

> it's important that the function return the un-localized string 'cancel' if there is an error so an action that is recording the script will know something went wrong and not store the step in the action. It should return undefined if everything went ok and the step should be recorded.

Code Protection
---------------

Be aware that if you're willing to protect your work via binary encoding, **you must leave alone everything** from the beginning to the `#target photoshop` (included). Basically you have to:

1.  Put the code (without the `<javascriptresource>`) on a temporary file, and export it (_File - Export to Binary..._)
2.  Open the resulting .jsxbin and add a backslash to each line ending:

// From this:

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
//... etc.

3.   Copy the binary file's content and put everything inside an `eval("...")` function, then add **before it** the `<javascriptresource>`:

// Written by Davide Barranca based on Adobe's FitImage
/*
@@@BUILDINFO@@@ RecordableGBlur.jsx 1.0
*/
/*
// BEGIN\_\_HARVEST\_EXCEPTION_ZSTRING
<javascriptresource>
<name>$$$/RBG/recordableGBlur=Recordable Gaussian Blur...</name>
<menu>filter</menu>
<enableinfo>true</enableinfo>
<eventid>07d2f0b1-653d-11e0-ae3e-0800200c9a66</eventid>
<terminology><!\[CDATA\[<<  /Version 1
                          /Events <<
                            /07d2f0b1-653d-11e0-ae3e-0800200c9a66 \[($$$/RBG/recordableGBlur=Recordable Gaussian Blur) 
                            /imageReference <<
                              /radius \[($$$/Actions/Key/GaussianBlur/Radius=Radius) /uint\]
                            >>\]
                          >>
                      >\> \]\]></terminology>
</javascriptresource>
// END\_\_HARVEST\_EXCEPTION_ZSTRING
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
AiX4ge0AiAiW4gf0AiAjG4hA0AiAiZ4hB0AiAiF4hD0AiAie4hC0AiAAhEARByB");

_\[I'd like to thank Mike Hale and Xbytor from [ps-scripts.com](http://www.ps-scripts.com "Ps-Scripts forum") who helped me understanding how this stuff works!\]_