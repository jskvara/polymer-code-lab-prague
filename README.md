# Notes Application Polymer Code Lab Prague 2014

The goal of this code lab is to create note-taking application in **Polymer**. We create simple application which can add notes, delete selected note or all notes in the list and we also try local storage to save the notes into the browser.

You will learn:
* how to write applications and Polymer
* how to use Polymer Core and Paper elements
* how to write your own Polymer elements
* how to integrate Polymer elements into your application

Download the prepared project from Github and open it in [Chrome Dev Editor](https://chrome.google.com/webstore/detail/chrome-dev-editor-develop/pnoffddplpippgcfjdhbmhkofpnaalpg). We prepared everything important for you so you don't have to solve any CSS styling and HTML structure, and you can directly explore how to write Dart application.

Project is divided into steps according to this Code Lab. Try to run the application from _step06/_ and see how it works.

![Notes application](notes_app.png)

## Step 1 - Empty project
Start in _step01/_ folder. This is a default Polymer project with HTML template and styles and only shows simple page with Polymer elements.

We update this code, add more functionality and at the end we will have nice notes application.

## Step 2 - Create new Polymer element + using Paper + Core elements

In this step we create our own element and we learn, how to use Polymer Paper and Core elements in our application.

First **create** `prague-notes.html` file.

Then you need to remove all imports and change it to our element in `index.html` file to:

```html
	<!-- import notes element -->
	<link rel="import" href="prague-notes.html">
```

And use our element in HTML body:

```html
	<body>
		<prague-notes></prague-notes>
	</body>
```

The whole `index.html` file looks like this:

```html
<!doctype html>

<html>
<head>
  <title>Note app</title>

  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">

  <script src="../bower_components/webcomponentsjs/webcomponents.js"></script>

  <!-- import notes element -->
  <link rel="import" href="prague-notes.html">
</head>

<body unresolved>
  <prague-notes></prague-notes>
</body>
</html>
```


In file `prague-notes.html` we need to make several changes. At first, we need to add element definition. Add `<polymer-element>` tag to `prague-notes.html` with `<template>` and `<script>` tags inside. The element name should be same as the name of file.

```html
<polymer-element name="prague-notes">
  <template>
	  ...
  </template>
  <script>
    ...
  </script>
</polymer-element>
```

Add some CSS styles inside `<template` element:
```html
	<style>
    #notes-delete-all {
      background: red;
    }
    core-toolbar h2 {
      color: white;
    }
    core-toolbar paper-input {
      width: 100%;
    }
  </style>
```

Now, we can start adding Polymer Core and Paper elements to our element. First, we need to import elements, which we want to use. We will use paper input, paper button, core toolbar and core icon elements.

At the beggining of the file, we need to add the following import statements:
```html
<link rel="import" href="../bower_components/paper-input/paper-input.html">
<link rel="import" href="../bower_components/paper-button/paper-button.html">
<link rel="import" href="../bower_components/core-icon/core-icon.html">
<link rel="import" href="../bower_components/core-toolbar/core-toolbar.html">
```

In our template body, we can now easily use these elements. We will have `core-toolbar` element with three sub-elements: title, input box for adding new note and button, which deletes all.

Last element in `prague-notes` will be list of all notes, but we will add this in step 3. For now, we will insert only empty `<div>` element.

HTML body of our element will be the following:

```html
    <core-toolbar>
      <h2>Notes app</h2>

      <span flex>
        <paper-input floatinglabel label="Enter note" id="newNoteInput"></paper-input>
      </span>

      <paper-button raised id="notes-delete-all">
        <core-icon icon="clear">
        </core-icon>
        Delete all
      </paper-button>
    </core-toolbar>

    <core-toolbar>
      <h2>Notes app</h2>

      <span flex>
        <paper-input floatinglabel label="Enter note"></paper-input>
      </span>

      <paper-button raised id="notes-delete-all">
        <core-icon icon="clear">
        </core-icon>
        Delete all
      </paper-button>
    </core-toolbar>

    <div class="content">

    </div>
```

Last thing, that is missing is JavaScript code for our element. Content of the `<script>` tag will be following:

```html
  <script type="text/javascript">
    Polymer({});
  </script>
```

We can also delete unused files `main.js` and `styles.css`.

You should be able to run the project now. If you select `index.html` file and click on the play button run. You should see our appplication with toolbar.

## Step 3 - Create new Polymer element and bind model with template

In this step we create another element `prague-note`, which will be single item in our list of notes. And we will use this element as a template for list of our notes.

At first, we create new element in file `prague-note.html`. We will use the following paper elements: paper button, papaer icon button, paper shadow and paper ripple.

Add these imports into our newly created file `prague-note.html`:

```html
<link rel="import" href="../bower_components/polymer/polymer.html">
<link rel="import" href="../bower_components/paper-button/paper-button.html">
<link rel="import" href="../bower_components/paper-icon-button/paper-icon-button.html">
<link rel="import" href="../bower_components/paper-shadow/paper-shadow.html">
<link rel="import" href="../bower_components/paper-ripple/paper-ripple.html">
```

We add element declaration with `<script>` tag:

```html
<polymer-element name="prague-note">
  <template>
    ...
  </template>
  <script type="text/javascript">
    Polymer({});
  </script>
</polymer-element>
```

We add some css styles:
```html
    <style>
      .note {
        padding: 0.5em;
        margin: 0.5em;
      }
      .note .note-text {
        margin: 0.5em;
        font-size: 2em;
      }
      paper-icon-button.delete {
        color: red;
        float: right;
        padding-top: 3ex;
      }
    </style>
```

And finally we can add Paper elements. We start with `<paper-shadow>` element, which will have horizontal layout:

```html
    <paper-shadow class="note" layout horizontal z="2">

    </paper-shadow>
```

Inside this element we can add `<div>` element with our note's text and with nice ripple effect. Next we add delete icon button.

```html
    <paper-shadow class="note" layout horizontal z="2">
      <paper-ripple fit></paper-ripple>
      <div flex class="note-text">
        {{note}}
      </div>
      <div>
        <paper-icon-button icon="clear" class="delete"></paper-icon-button>
      </div>
    </paper-shadow>
```

Now we need to add `note` attribute to our element. We need to update our element definition:

```html
<polymer-element name="prague-note" attributes="note">
```

And we also need to add JavaScript for this attribute to bind it with our `note` variable via property `publish`.

```html
  <script type="text/javascript">
    Polymer({
      publish: {
        note: ''
      }
    });
  </script>
```

Now we can use our new element `prague-note` in our html. We want to use this element for each item in notes list. So we can easily use polymer `template` tag with `repeat` attribute. In `prague-notes.html` you need to import our element:

```html
<link rel="import" href="prague-note.html">
```

Add inside content `<div>` in `prague-notes.html` we can use our element:

```html
      <template repeat="{{note in notes}}">
        <prague-note note="{{note}}"></prague-note>
      </template>
```

For every note object in notes array we render `prague-note` element, with `note` attribute, which sets the note object into our `note` property in `prague-notes` element.

In `prague-notes.html` file, we need to declare our `notes` collection and we can add some data. When we add property to Polymer object, that property is automatically available in our template and every change will be propragated to template.

```html
  <script type="text/javascript">
    Polymer({
      notes: ['Learn Polymer', 'Build Polymer App'],
    });
  </script>
```

## Step 4 - Adding new notes

In this step we add a method, that will add a new note to our list of notes.

In `prague-notes.html` file we add `on-change` and `value` attribute to `paper-input` element and , which will call `createNote` method and set `newNote` varaible in our JavaScript.

```html
        <paper-input floatinglabel label="Enter note" id="newNoteInput" on-change="{{createNote}}" value="{{newNote}}"></paper-input>
```

And in our JavaScript we set `notes` variable toempty array add new method `createNote`. This method uses `newNote` variable, which we declared in our template. When this method is run, we check if the value of input is not empaty and add data from our input to the list of our notes.

```javascript
    Polymer({
      notes: [],
      createNote: function() {
        if (this.newNote !== '') {
          this.notes.push(this.newNote);
          this.newNote = '';
        }
      }
    });
```

When you run your application, you should be able to add new notes to our list.

## Step 5 - Deleting notes

In this step we add two methods. First method deletes single note from our list and second method deletes all the notes.

We will start with `Delete all` button. We need to add `on-click` attribute to our delete button in `prague-notes.html` file:

```html
      <paper-button raised id="notes-delete-all" on-click="{{deleteAll}}">...</paper-button>
```

And our JavaScript we define new method `deleteAll`, which removes all data from our `notes` collection:

```javascript
      deleteAll: function() {
        this.notes = [];
      }
```

Now we need to add delete method for signle note. Before we start with this method, we need to add indexes to our notes in list, because we need to know, which note we want to delete.

We add indexes to our notes collection in template file `prague-notes.html`. We change our code to:

```html
      <template repeat="{{note, i in notes}}">
        <prague-note note="{{note}}" index="{{i}}"></prague-note>
      </template>
```

We also need to add this new `index` attribute to our `prague-note` element.

First add index to our attributes list in `prague-note` element definition:

```html
<polymer-element name="prague-note" attributes="note index">
```

And we also need to add our `index` variable to `publish` property.

```javascript
    Polymer({
      publish: {
        note: '',
        index: 0
      },
      ...
```

Now we can add `on-click` attribute to `paper-icon-button` element in `prague-note` to call new method `deleteNote`:

```html
        <paper-icon-button icon="clear" class="delete" on-click="{{deleteNote}}"></paper-icon-button>
```

And we declare this method. Because the list of our notes is in parent element, we need to fire our custom event with name `delete-note` and with index of deleted note.

```javascript
      deleteNote: function() {
        this.fire('delete-note', {'index': this.index});
      }
```

Now we need to catch this event. We can declare that our element `prague-notes` should listen for these events and when this event is fired, we can call our method in `prague-notes` element. We can use `on-delete-note` attribute in element definition. The syntax for this attribute is `on-` and name of event. When we catch any event, we can call our custom method from that element. We need to change our `prague-notes` element declaration:

```html
<polymer-element name="prague-notes" on-delete-note="{{deleteNote}}">
  ...
</polymer-element>
```

And we add new method `deleteNote` to `prague-notes.html` file. We get the index of note, which should be deleted and we can easily remove note on that index from our collection.

```javascript
      deleteNote: function(event, note) {
        var index = note.index;
        this.notes.splice(index, 1);
      }
```

## Step 6 - Saving to local storage

We can improve our application to save all data to brower local storage, so when user closes or refreshes browser, tha data are still saved in browser. And if user opens the browser again he doesn't loose the data.

We can easily use core localstorage element. We need to include it in `prague-notes.html` file:

```html
<link rel="import" href="../bower_components/core-localstorage/core-localstorage.html">
```

And now we can use the element and bind it to our `notes` variable. Now every time the notes varaible is changed, it's saved into local storage and when user reload the browser this element gets our notes from local storage and populate the `notes` varaible.

```html
<core-localstorage id="storage" name="codelab-app-storage" value="{{notes}}"></core-localstorage>
```

Now when you refresh your browser, you should see all your notes, that were previously saved in your browser.