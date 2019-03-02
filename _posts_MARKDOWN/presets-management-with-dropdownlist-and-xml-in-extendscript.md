---
title: " Presets management with DropDownList and XML in ExtendScript\t\t"
tags:
  - CoffeeScript
  - dropdownlist
  - Extendscript
  - presets
  - xml
url: 1763.html
id: 1763
category:
  - Coding
date: 2013-03-02 23:06:46
---

ScriptUI windows can contain several controls (checkboxes, sliders, etc), and to setup a Preset system is a very handy way to allow users save and retrieve their own preferred configurations easily. In this tutorial I'll show you how to implement Presets with a DropDownList menu based upon XML data. ![XML DropDownList Preset demo](http://localhost:8888/wp-content/uploads/2013/03/XML_DropDownList_Preset_demo.jpg)

Starting point
--------------

I've set up a simple demo dialog using a resource string - you can use it either in InDesign or Photoshop for instance: this is the **base** which I'll be using throughout the post **adding functional bits of code**. (I've used [CoffeeScript](http://www.coffeescript.org "CoffeeScript"), a nice language that compiles to JavaScript, to write the whole script; in this tutorial I'm going to show the JS only, but you can find the entire CoffeScript code at the bottom of the page if you're curious).

#target photoshop;

var win, windowResource;

windowResource = "dialog {  \
    orientation: 'column', \
    alignChildren: \['fill', 'top'\],  \
    size:\[410, 210\], \
    text: 'XML DropDownList Presets demo - www.davidebarranca.com',  \
    margins:15, \
    \
    controlsPanel: Panel { \
        orientation: 'column', \
        alignChildren:'right', \
        margins:15, \
        text: 'Controls', \
        controlsGroup: Group {  \
        orientation: 'row', \
        alignChildren:'center', \
        st: StaticText { text: 'Amount:' }, \
        mySlider: Slider { minvalue:0, maxvalue:500, value:300, size:\[220,20\] }, \
        myText: EditText { text:'300', characters:5, justify:'left'} \
        } \
    }, \
    presetsPanel: Panel { \
        orientation: 'row', \
        alignChildren: 'center', \
        text: 'Presets', \
        margins: 14, \
        presetList: DropDownList {preferredSize: \[163,20\] }, \
        saveNewPreset: Button { text: 'New', preferredSize: \[44,24\]}, \
        deletePreset: Button { text: 'Remove', preferredSize: \[60,24\]}, \
        resetPresets: Button { text: 'Reset', preferredSize: \[50,24\]} \
    }, \
    buttonsGroup: Group{\
        alignChildren: 'bottom',\
        cancelButton: Button { text: 'Cancel', properties:{name:'cancel'},size: \[60,24\], alignment:\['right', 'center'\] }, \
        applyButton: Button { text: 'Apply', properties:{name:'apply'}, size: \[100,24\], alignment:\['right', 'center'\] }, \
        }\
    }";

win = new Window(windowResource);
// binds mySlider.value with myText.text
win.controlsPanel.controlsGroup.myText.onChange = function() {
  this.parent.mySlider.value = Number(this.text);
};
win.controlsPanel.controlsGroup.mySlider.onChange = function() { 
  this.parent.myText.text = Math.ceil(this.value); }; win.show();&nbsp;

This scheme lets you:

*   **Save** new custom presets.
*   **Delete** custom presets.
*   **Reset** to the default group of presets (which are _protected_ and cannot be deleted).

The presets are stored as **XML data** in a file (in the example there's a single value only, but they can be as many as you need)

<presets>
  <preset default="true">
    <name>select...</name>
    <value/>
  </preset>
  <preset default="true">
    <name>Default value</name>
    <value>100</value>
  </preset>
  <preset default="true">
    <name>Low value</name>
    <value>10</value>
  </preset>
  <preset default="true">
    <name>High value</name>
    <value>400</value>
  </preset>
</presets>

The `default="true"` attribute marks this presets as read-only (i.e. the user can't delete them)

Logic
-----

This is how I've decomposed the problem (click to open a bigger version)

[![Presets Demo Workflow](http://localhost:8888/wp-content/uploads/2013/03/PresetsDemoWorkflow.png)](http://localhost:8888/wp-content/uploads/2013/03/PresetsDemoWorkflow.png)

Building the Presets system
---------------------------

Basically the core is an **initialization routine** that reads XML data from an XML file (or creates a default one if it's missing) and fills the DropDownList (DDL from now on) with labels, so let's start with it.

### 1\. Initialization routine

I've set few globals that will be useful for other functions too - the code is commented so it should be quite self-explanatory.

// the XML object holding the XML data
var xmlData = null;

// the array holding the preset labels, used later on 
// to check for label duplicates when adding a new preset
var presetNamesArray = \[\];

// the preset file (which lives alongside this script)
resPath = File($.fileName).parent;
var presetFile = new File("" + resPath + "/presets.xml");

// Initialization routine
var initDDL = function() {
  var i, nameListLength;

  if (!presetFile.exists) {
    createDefaultXML();
    // recursive call, needed to read and fill the DDL
    initDDL(); 
  }

  // retrieves XML Data
  xmlData = readXML();
  // empties the DDL before adding new content
  if (win.presetsPanel.presetList.items.length !== 0) {
    win.presetsPanel.presetList.removeAll();
  }

  // the number of preset labels
  nameListLength = xmlData.preset.name.length();
  presetNamesArray.length = 0;

  // for each preset XML element, retrieve the <name> label
  // and write it in the Labels array, then add it to the DDL
  i = 0;
  while (i < nameListLength) {
    presetNamesArray.push(xmlData.preset.name\[i\].toString());
    win.presetsPanel.presetList.add("item", xmlData.preset.name\[i\]);
    i++;
  }

  // set as the default the first preset
  // which is the placeholder "select..."
  win.presetsPanel.presetList.selection = win.presetsPanel.presetList.items\[0\];
  return true;
};

As you've seen I've mentioned some utility functions like `readXML()` and `writeXML()`, alongside `createDefaultXML()`, here they are:

// holds the XML object containing the 
// Default set of Presets
var defaultXML = <presets>
		   <preset default="true">
		   <name>select...</name>
		   <value></value>
	        </preset>
	        <preset default="true">
		   <name>Default value</name>
		   <value>100</value>
	        </preset>
	        <preset default="true">
		   <name>Low value</name>
		   <value>10</value>
	        </preset>
	        <preset default="true">
	       	   <name>High value</name>
		   <value>400</value>
                </preset>
	     </presets>;

// writes an XML object to file
writeXML = function(xml, file) {
  if (file == null) {
    // global, the preset.xml file already declared
    file = presetFile;
  }
  try {
    file.open("w");
    file.write(xml);
    file.close();
  } catch (e) {
    alert("" + e.message + "\\nThere are problems writing the XML file!");
  }
  return true;
};

// reads an XML object from a file
readXML = function(file) {
  var content;
  if (file == null) {
    // global, the preset.xml file already declared
    file = presetFile;
  }
  try {
    file.open('r');
    content = file.read();
    file.close();
    return new XML(content);
  } catch (e) {
    alert("" + e.message + "\\nThere are problems reading the XML file!");
  }
  return true;
};

// creates a preset.xml file
// using the default set of presets
createDefaultXML = function() {
  // global, the preset.xml file already declared
  if (!presetFile.exists) {
    writeXML(defaultXML);
    return true;
  } else {
    presetFile.remove();
    // recursive call
    createDefaultXML();
  }
  return true;
};

So far the script detects whether a `preset.xml` file exists: if it doesn't, it creates a default one - otherwise it reads it and use it to fill the DDL.

### 2\. Save a new preset

I've setup the following `onClick()` function which makes use of `createPresetChild()`, another utility that grabs values from the GUI and creates a node from them. Which node, in turn, will be appended to the existing XML data.

// used to workaround the lack of indexOf in ExtendScript
var __indexOf = \[\].indexOf || function(item) { for (var i = 0, l = this.length; i < l; i++) { if (i in this && this\[i\] === item) return i; } return -1; };

// creates a XML <preset> node 
var createPresetChild = function(name, value) {
  var child;
  // mind you, each custom node has the default attribute
  // set to "false", meaning that it can be deleted
  return child = <preset default="false">
		    <name>{name}</name>
		    <value>{value}</value>
		 </preset>;
};

// onClick handler for the Save New Preset button
win.presetsPanel.saveNewPreset.onClick = function() {
  var child, presetName;

  // ask for a preset name
  presetName = prompt("Give your preset a name!\\nYou'll find it in the preset list.", "User Preset", "Save new Preset");
  if (presetName == null) {
    return;
  }

  // in case the name already exists
  if (__indexOf.call(presetNamesArray, presetName) >= 0) {
    alert("Duplicate name!\\nPlease find another one.");
    // make a recursive call to this onClick function
    win.presetsPanel.saveNewPreset.onClick.call();
  }

  // creates a <preset> node with values grabbed 
  // from the window controls
  child = createPresetChild(presetName, win.controlsPanel.controlsGroup.myText.text);

  // appends the XML node to the existing XML data (global variable)
  xmlData.appendChild(child);

  // writes the XML object to the preset.xml file
  writeXML(xmlData);

  // you need to initialize again, in order to populate 
  // the DDL with new values
  initDDL();

  // selects the last preset in the DDL
  win.presetsPanel.presetList.selection = win.presetsPanel.presetList.items\[win.presetsPanel.presetList.items.length - 1\];
};

Please notice the lack of `indexOf` in ExtendScript (the used `__indexOf` is courtesy of CoffeeScript)

###  3\. Delete preset

That's pretty straightforward, you just need to check whether the `default` attribute of the preset is false (which means that the preset is not protected and can be deleted). Again, each preset's modification needs to write the changes to disk in the `presets.xml` file and re-initialize the DDL in order to populate the menu properly.

// onClick handler for the Delete preset button
win.presetsPanel.deletePreset.onClick = function() {

  // mind you: typeof xmlData.preset\[\].@default = XML
  if (xmlData.preset\[win.presetsPanel.presetList.selection.index\].@default.toString() === "true") {
    alert("Can't delete \\"" + xmlData.preset\[win.presetsPanel.presetList.selection.index\].name + "\\"\\nIt's part of the default set of Presets");
    return;
  }

  // ask for confirmation
  if (confirm("Are you sure you want to delete \\"" + xmlData.preset\[win.presetsPanel.presetList.selection.index\].name + "\\" preset?\\nYou can't undo this.")) {
    // delete the <preset> element
    delete xmlData.preset\[win.presetsPanel.presetList.selection.index\];
  }

  // as usual, write to disk and initialize again
  writeXML(xmlData);
  return initDDL();
};

###  4\. Reset presets

Another easy task, just replace `presets.xml` with a default one (which is hardcoded inside the script).

// onClick handler for the Reset presets button
win.presetsPanel.resetPresets.onClick = function() {
  if (confirm("Warning\\nAre you sure you want to reset the Preset list?", true)) {
    createDefaultXML();
    initDDL();
  }
};

###  5\. Apply preset

The last thing we've to add is a handler for the DDL `onChange` event - that is, the user selects the preset and window controls must be updated with values coming from it.

// DDL onChange handler
win.presetsPanel.presetList.onChange = function() {
  if (this.selection !== null && this.selection.index !== 0) {
    // sets GUI controls
    win.controlsPanel.controlsGroup.myText.text = xmlData.preset\[this.selection.index\].value;
    win.controlsPanel.controlsGroup.mySlider.value = Number(xmlData.preset\[this.selection.index\].value);
  }
};

Completed script
----------------

Let's put everything together! As follows both versions (JS and CoffeeScript). Speaking of the latter, XML management in ExtendScript made me use more than I wanted the backticks (which embed regular JS code), but that's fine - I'm a big fan of CoffeeScript anyway :-)

### Javascript

#target photoshop;
var createDefaultXML, createPresetChild, defaultXML, initDDL, presetFile, presetNamesArray, readXML, resPath, win, windowResource, writeXML, xmlData,
  __indexOf = \[\].indexOf || function(item) { for (var i = 0, l = this.length; i < l; i++) { if (i in this && this\[i\] === item) return i; } return -1; };

windowResource = "dialog {  \
    orientation: 'column', \
    alignChildren: \['fill', 'top'\],  \
    size:\[410, 210\], \
    text: 'DropDownList Demo - www.davidebarranca.com',  \
    margins:15, \
    \
    controlsPanel: Panel { \
        orientation: 'column', \
        alignChildren:'right', \
        margins:15, \
        text: 'Controls', \
        controlsGroup: Group {  \
            orientation: 'row', \
            alignChildren:'center', \
            st: StaticText { text: 'Amount:' }, \
            mySlider: Slider { minvalue:0, maxvalue:500, value:300, size:\[220,20\] }, \
            myText: EditText { text:'300', characters:5, justify:'left'} \
    	} \
    }, \
    presetsPanel: Panel { \
    	orientation: 'row', \
    	alignChildren: 'center', \
    	text: 'Presets', \
    	margins: 14, \
    	presetList: DropDownList {preferredSize: \[163,20\] }, \
    	saveNewPreset: Button { text: 'New', preferredSize: \[44,24\]}, \
    	deletePreset: Button { text: 'Remove', preferredSize: \[60,24\]}, \
    	resetPresets: Button { text: 'Reset', preferredSize: \[50,24\]} \
    }, \
    buttonsGroup: Group{\
        alignChildren: 'bottom',\
        cancelButton: Button { text: 'Cancel', properties:{name:'cancel'},size: \[60,24\], alignment:\['right', 'center'\] }, \
        applyButton: Button { text: 'Apply', properties:{name:'apply'}, size: \[100,24\], alignment:\['right', 'center'\] }, \
    }\
}";

win = new Window(windowResource);
xmlData = null;
presetNamesArray = \[\];
resPath = File($.fileName).parent;
presetFile = new File("" + resPath + "/presets.xml");
defaultXML = <presets>
		<preset default="true">
			<name>select...</name>
			<value></value>
		</preset>
		<preset default="true">
			<name>Default value</name>
			<value>100</value>
		</preset>
		<preset default="true">
			<name>Low value</name>
			<value>10</value>
		</preset>
		<preset default="true">
			<name>High value</name>
			<value>400</value>
		</preset>
	</presets>;

writeXML = function(xml, file) {
  if (file == null) {
    file = presetFile;
  }
  try {
    file.open("w");
    file.write(xml);
    file.close();
  } catch (e) {
    alert("" + e.message + "\\nThere are problems writing the XML file!");
  }
  return true;
};

readXML = function(file) {
  var content;
  if (file == null) {
    file = presetFile;
  }
  try {
    file.open('r');
    content = file.read();
    file.close();
    return new XML(content);
  } catch (e) {
    alert("" + e.message + "\\nThere are problems reading the XML file!");
  }
  return true;
};

createDefaultXML = function() {
  if (!presetFile.exists) {
    writeXML(defaultXML);
    void 0;
  } else {
    presetFile.remove();
    createDefaultXML();
  }
  return true;
};

createPresetChild = function(name, value) {
  var child;
  return child = <preset default="false">
				<name>{name}</name>
				<value>{value}</value>
			</preset>;
};

initDDL = function() {
  var i, nameListLength;
  if (!presetFile.exists) {
    createDefaultXML();
    initDDL();
  }
  xmlData = readXML();
  if (win.presetsPanel.presetList.items.length !== 0) {
    win.presetsPanel.presetList.removeAll();
  }
  nameListLength = xmlData.preset.name.length();
  presetNamesArray.length = 0;
  i = 0;
  while (i < nameListLength) {
    presetNamesArray.push(xmlData.preset.name\[i\].toString());
    win.presetsPanel.presetList.add("item", xmlData.preset.name\[i\]);
    i++;
  }
  win.presetsPanel.presetList.selection = win.presetsPanel.presetList.items\[0\];
  return true;
};

win.controlsPanel.controlsGroup.myText.onChange = function() {
  return this.parent.mySlider.value = Number(this.text);
};

win.controlsPanel.controlsGroup.mySlider.onChange = function() {
  return this.parent.myText.text = Math.ceil(this.value);
};

win.presetsPanel.presetList.onChange = function() {
  if (this.selection !== null && this.selection.index !== 0) {
    win.controlsPanel.controlsGroup.myText.text = xmlData.preset\[this.selection.index\].value;
    win.controlsPanel.controlsGroup.mySlider.value = Number(xmlData.preset\[this.selection.index\].value);
  }
  return true;
};

win.presetsPanel.resetPresets.onClick = function() {
  if (confirm("Warning\\nAre you sure you want to reset the Preset list?", true)) {
    createDefaultXML();
    return initDDL();
  }
};

win.presetsPanel.saveNewPreset.onClick = function() {
  var child, presetName;
  presetName = prompt("Give your preset a name!\\nYou'll find it in the preset list.", "User Preset", "Save new Preset");
  if (presetName == null) {
    return;
  }
  if (__indexOf.call(presetNamesArray, presetName) >= 0) {
    alert("Duplicate name!\\nPlease find another one.");
    win.presetsPanel.saveNewPreset.onClick.call();
  }
  child = createPresetChild(presetName, win.controlsPanel.controlsGroup.myText.text);
  xmlData.appendChild(child);
  writeXML(xmlData);
  initDDL();
  return win.presetsPanel.presetList.selection = win.presetsPanel.presetList.items\[win.presetsPanel.presetList.items.length - 1\];
};

win.presetsPanel.deletePreset.onClick = function() {
  if (xmlData.preset\[win.presetsPanel.presetList.selection.index\].@default.toString() === "true") {
    alert("Can't delete \\"" + xmlData.preset\[win.presetsPanel.presetList.selection.index\].name + "\\"\\nIt's part of the default set of Presets");
    return;
  }
  if (confirm("Are you sure you want to delete \\"" + xmlData.preset\[win.presetsPanel.presetList.selection.index\].name + "\\" preset?\\nYou can't undo this.")) {
    delete xmlData.preset\[win.presetsPanel.presetList.selection.index\];
  }
  writeXML(xmlData);
  return initDDL();
};

initDDL();

win.show();

###  CoffeeScript

`#target photoshop`

windowResource = "dialog {  \
    orientation: 'column', \
    alignChildren: \['fill', 'top'\],  \
    size:\[410, 210\], \
    text: 'DropDownList Demo - www.davidebarranca.com',  \
    margins:15, \
    \
    controlsPanel: Panel { \
        orientation: 'column', \
        alignChildren:'right', \
        margins:15, \
        text: 'Controls', \
        controlsGroup: Group {  \
            orientation: 'row', \
            alignChildren:'center', \
            st: StaticText { text: 'Amount:' }, \
            mySlider: Slider { minvalue:0, maxvalue:500, value:300, size:\[220,20\] }, \
            myText: EditText { text:'300', characters:5, justify:'left'} \
    	} \
    }, \
    presetsPanel: Panel { \
    	orientation: 'row', \
    	alignChildren: 'center', \
    	text: 'Presets', \
    	margins: 14, \
    	presetList: DropDownList {preferredSize: \[163,20\] }, \
    	saveNewPreset: Button { text: 'New', preferredSize: \[44,24\]}, \
    	deletePreset: Button { text: 'Remove', preferredSize: \[60,24\]}, \
    	resetPresets: Button { text: 'Reset', preferredSize: \[50,24\]} \
    }, \
    buttonsGroup: Group{\
        alignChildren: 'bottom',\
        cancelButton: Button { text: 'Cancel', properties:{name:'cancel'},size: \[60,24\], alignment:\['right', 'center'\] }, \
        applyButton: Button { text: 'Apply', properties:{name:'apply'}, size: \[100,24\], alignment:\['right', 'center'\] }, \
    }\
}";

\# Create Dialog
win = new Window windowResource

\# globals
xmlData = null
presetNamesArray = \[\]
resPath = File($.fileName).parent
presetFile = new File "#{resPath}/presets.xml"
defaultXML = `<presets>
					<preset default="true">
						<name>select...</name>
						<value></value>
					</preset>
					<preset default="true">
						<name>Default value</name>
						<value>100</value>
					</preset>
					<preset default="true">
						<name>Low value</name>
						<value>10</value>
					</preset>
					<preset default="true">
						<name>High value</name>
						<value>400</value>
					</preset>
				</presets>`

\# writes an XML object to a file
writeXML = (xml, file=presetFile) ->
	try
		file.open "w"
		file.write xml
		file.close()
	catch e
		alert "#{e.message}\\nThere are problems writing the XML file!"
	true

\# reads a file (default=presets.xml) and returns an XML object
readXML = (file=presetFile) ->
	try  
		file.open 'r'
		content = file.read()
		file.close()
		return new XML content
	catch e
		alert "#{e.message}\\nThere are problems reading the XML file!"
	true

\# creates a Default XML file
createDefaultXML = () ->
	# if file doesn't exist
	if !presetFile.exists 
		writeXML defaultXML
		undefined
	else
		presetFile.remove()
		createDefaultXML()
	true

\# creates and returns an XML <preset> element
createPresetChild = (name, value) ->
	child = `<preset default="false">
				<name>{name}</name>
				<value>{value}</value>
			</preset>`

\# Initialize DropDownList
initDDL = () ->
	# if file doesn't exist
	if !presetFile.exists
		createDefaultXML()
		initDDL()

	# file exists
	xmlData = readXML() 
	# clean DropDownList
	win.presetsPanel.presetList.removeAll() if win.presetsPanel.presetList.items.length isnt 0
	# how many presets?	 
	nameListLength = xmlData.preset.name.length()
	# empties the names array
	presetNamesArray.length = 0
	# fills the DDL and the presetNamesArray
	i = 0
	while i < nameListLength
		# toString() otherwise its an array of XML objects!
		presetNamesArray.push xmlData.preset.name\[i\].toString()
		win.presetsPanel.presetList.add "item", xmlData.preset.name\[i\]
		i++
	win.presetsPanel.presetList.selection = win.presetsPanel.presetList.items\[0\]
	true

\# binds mySlider.value and myText.text
win.controlsPanel.controlsGroup.myText.onChange = () ->
	@parent.mySlider.value = Number @text

\# binds mySlider.value and myText.text
win.controlsPanel.controlsGroup.mySlider.onChange = () ->
	@parent.myText.text = Math.ceil @value

win.presetsPanel.presetList.onChange = () ->
	if @selection isnt null and @selection.index isnt 0
		win.controlsPanel.controlsGroup.myText.text = xmlData.preset\[@selection.index\].value
		win.controlsPanel.controlsGroup.mySlider.value = Number xmlData.preset\[@selection.index\].value
	true

win.presetsPanel.resetPresets.onClick = () ->
	if confirm "Warning\\nAre you sure you want to reset the Preset list?", true
		# remove existing preset file, creates a Default XML file
		createDefaultXML()
		# refresh the DropDownList!
		initDDL()

win.presetsPanel.saveNewPreset.onClick = () ->
	presetName = prompt "Give your preset a name!\\nYou'll find it in the preset list.", "User Preset", "Save new Preset"
	if !presetName? then return
	if presetName in presetNamesArray
		alert "Duplicate name!\\nPlease find another one."
		# recursion again?!
		win.presetsPanel.saveNewPreset.onClick.call()
	# create a <preset> xml child
	child = createPresetChild presetName, win.controlsPanel.controlsGroup.myText.text
	# append the child to the xmlData
	xmlData.appendChild child
	writeXML xmlData
	# re-initialize the DDL in order to make changes appear
	initDDL()
	# select the last preset
	win.presetsPanel.presetList.selection = win.presetsPanel.presetList.items\[win.presetsPanel.presetList.items.length - 1\]

win.presetsPanel.deletePreset.onClick = () ->
	# "true" is a string in the XML, not a boolean!
	if \`xmlData.preset\[win.presetsPanel.presetList.selection.index\].@default.toString()\` is "true"
		alert "Can't delete \\"#{xmlData.preset\[win.presetsPanel.presetList.selection.index\].name}\\"\\nIt's part of the default set of Presets"
		return
	if confirm "Are you sure you want to delete \\"#{xmlData.preset\[win.presetsPanel.presetList.selection.index\].name}\\" preset?\\nYou can't undo this."
		\`delete xmlData.preset\[win.presetsPanel.presetList.selection.index\]\`
	writeXML xmlData
	# re-initialize the DDL in order to make changes appear
	initDDL()

\# Init the DropDownList
initDDL()

\# Show the window
win.show()