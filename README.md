Inquirer.js  [![Build Status](https://travis-ci.org/SBoudrias/Inquirer.js.png?branch=master)](http://travis-ci.org/SBoudrias/Inquirer.js)
=====================

A collection of common interactive command line user interfaces.


Goal and philosophy
---------------------

We strive at providing easily embeddable and beatiful command line interface for Node.js ;
some hope in becoming the CLI Xanadu.

_Inquirer_ should ease the process of asking end user questions, parsing, validating answers, and providing error feedback.

_Inquirer_ provide the user interface, and the inquiry session flow. If you're searching for a full blown command line program utility, then check out [Commander.js](https://github.com/visionmedia/commander.js) (inspired by) or [Charm](https://github.com/substack/node-charm) (used internally).


Documentation
=====================


Installation
---------------------

``` prompt
npm install inquirer
```

```javascript
var inquirer = require("inquirer");
inquirer.prompt([/* Pass your questions in here */], function( answers ) {
	// Use user feedback for... whatever!!
});
```


Examples (Run it and see it)
---------------------

Checkout the `examples/` folder for code and interface examples.

``` prompt
node examples/pizza.js
# etc
```


Methods
---------------------

### `inquirer.prompt( questions, callback )`

Launch the prompt interface (inquiry session)

+ **questions** (Array) contains [Question Object](#question)
+ **callback** (Function) first parameter is the [Answers Object](#answers)


Objects
---------------------

### Question
A question object is a `hash` containing question related values:

+ **type**: (String) Type of the prompt. Defaults: `input` - Possible values: `input`, `confirm`,
`list`, `rawlist`
+ **name**: (String) The name to use when storing the answer in the anwers hash.
+ **message**: (String) The question to print.
+ **default**: (String) Default value to use if nothing is entered
+ **choices**: (Array) Choices array.  
Values can be simple `string`s, or `object`s containing a `name` (to display) and a `value` properties (to save in the answers hash).
+ **validate**: (Function) Receive the user input and should return `true` if the value is valid, and an error message (`String`) otherwise. If no error message is returned, a default one is provided.
+ **filter**: (Function) Receive the user input and return the filtered value to be used inside the program. The value returned will be added to the `Answers` hash.

`validate` and `filter` function can be asynchronous using `this.async()`. You just have to pass the value you'd normally return to the callback option.

``` javascript
{
  validate: function(input) {
    var done = this.async();
    setTimeout(function() {
      if (typeof input !== "number") {
        done("You need to provide a number");
        return;
      }
      done(true);
    }, 3000);
  }
}
```

### Answers
A key/value hash containing the client answers in each prompt.

+ **Key** The `name` property of the _question_ object
+ **Value** (Depends on the prompt)
  + `confirm`: (Boolean)
  + `input` : User input (String)
  + `rawlist`, `list` : Selected choice value (or name if no value specified) (String)


Prompts
---------------------

### List - `{ type: "list" }`

Take `type`, `name`, `message`, `choices`[, `filter`] properties.

``` prompt
What about the toping?
  [X] Peperonni and chesse
  [ ] All dressed
  [ ] Hawaïan
(Use arrow key)
```

### Raw List - `{ type: "rawlist" }`

Take `type`, `name`, `message`, `choices`[, `filter`] properties.

``` prompt
You also get a free 2L liquor! Which one?
  1) Pepsi
  2) 7up
  3) Coke
  Default (1)
```

### Confirm - `{ type: "confirm" }`

Take `type`, `name`, `message`[, `default`] properties.

``` prompt
Hi, welcome to Node Pizza plaza. Is it for a delivery? (Y/n)
```

### Input - `{ type: "input" }`

Take `type`, `name`, `message`[, `default`, `filter`, `validate`] properties.

``` prompt
Any comments on your purchase experience? (default "Nope, all good!"):
```


News on the march (Release notes)
=====================

+ **0.1.3** : Add async support for validation and filtering functions.
+ **0.1.0** : First official release. There's 4 prompt types: `input`, `confirm`, `list` and
`rawlist`. There's functionnality to allow the validation of input, and the filtering of values.


License
=====================

Copyright (c) 2012 Simon Boudrias  
Licensed under the MIT license.
