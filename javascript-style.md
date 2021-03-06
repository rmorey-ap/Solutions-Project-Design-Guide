AnyPresence Solutions JavaScript Style Guide
=================================

This style guide outlines the coding conventions of the JavaScript code at [AnyPresence](http://anypresence.com).

Table of Contents
---------
- [Background](#background)
- [Language](#language)
- [Code Organization](#code-organization)
- [Spacing](#spacing)
- [Semicolons](#semicolons)
- [Linting](#linting)
- [Comments](#comments)
- [Quotes](#quotes)
- [Variables](#variables)
  - [Global Variables](#global-variables)
- [Operators](#operators)
  - [Equality](#equality)
- [Conditionals](#conditionals)
- [Ternary Operator](#ternary-operator)
- [Prototypes](#prototypes)
- [Closures](#closures)
- [Classes](#classes)
- Backbone.js
- [Object Oriented vs Procedural Programming](#object-oriented-vs-procedural-programming)

Background
---------
Here are some of the documents that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

- [jQuery JavaScript Style Guide](http://contribute.jquery.org/style-guide/js/)
- [Node.js Style Guide](https://github.com/felixge/node-style-guide)
- [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)
- [Backbone Styles/Patterns](https://gist.github.com/liammclennan/2886952)


Language
---------
US English should be used throughout. 

**Preferred:**
```javascript
var myColor = "#FFF";
```

**Not Preferred:**
```javascript
var myColour = "#FFF";
```

Code Organization
---------
Organise modules first by type, then by function

```
models/
  shopping.js
  admin.js
views/
  shopping.js
  admin.js
```

This minimizes coupling and maximizes cohesion because dependencies tend to go the same way (ie routers depend on models and views, views depend on models) and cohesion tends to be high inside of a module containing models for a certain feature, or views for a certain feature.

Spacing
---------
- Indent using 2 spaces. Never indent with tabs.
- No whitespace at the end of line or on blank lines.
- Use UNIX-style newlines (`\n`), and a newline character as the last character of a file. Windows-style newlines (`\r\n`) are forbidden inside any repository.
- New line at the end of each file.
- Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
- No spaces *inside* parentheses.  i.e. `if (foo) {` but not `if ( foo ) {`.
- One space *outside* parentheses.  i.e. `function (bar) {` but not `function(bar){`.
- Function calls should have *no space* before the opening paren.  i.e. `myFunc('value');`

**Preferred:**
```javascript
if (user.isHappy) {
  // Do something
} else {
  // Do something else
}
```

**Not Preferred:**
```javascript
if (user.isHappy)
{
  // Do something
}
else 
{
  // Do something else
}
```

- There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.

Semicolons
---------
Use them. Never rely on ASI.

Linting
---------
Use JSHint to detect errors and potential problems. Every jQuery project has a Grunt task for linting all JavaScript files: `grunt jshint`.

Comments
---------
When they are needed, comments should be used to explain why a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations.

**All** class-like objects and methods should be commented using [YUI Doc](http://yui.github.io/yuidoc/syntax/) style comments.

Quotes
---------
**Always** use single quotes, unless you are writing JSON.

**Preferred:**
```javascript
var foo = 'bar';
```

**Not Preferred:**
```javascript
var foo = "bar";
```

Variables
---------
Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops. 


#### Global Variables
Each project may expose at most one global variable.  This variable may be used as a namespace.

```javascript
window.AP = {};

AP.MyClass = function () { ... 
```

Operators
---------
Always add a single space before and after the use of an operator. 

**Preferred:**
```javascript
var foo = 'bar';
var answer = 42;
answer += 9;
answer++;
answer = 40 + 2;
```

**Not Preferred:**
```javascript
var foo='bar';
var answer=42;
answer+= 9;
++answer;
answer= 40+2;
```

When incrementing a variable, the `++`, `--`, etc are preferred to be after the variable instead of before to be consistent with other operators. Operators separated should always be surrounded by spaces unless there is only one operand.

**Preferred:**
```javascript
answer++;
```

#### Equality
Strict equality checks (`===`) must be used in favor of abstract equality checks (`==`). The only exception is when checking for `undefined` and `null` by way of `null`.

Conditionals
---------
Conditionals should use the positive case if possible when using an `else` body. If there is no `else` body, it is fine to use the negative case.

**Preferred:**
```javascript
// If with else body
if (someVariable) {
	// Do Something
} else {
	// Do Something
}

// If with no else body
if (!someOtherVariable) {
	// Do Something
}
```

**Not Preferred:**
```javascript
if (!someVariable) {
	// Do Something
} else {
	// Do Something
}
```


Ternary Operator
---------
The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something, and parentheses are added for improved readability. If the variable being compared is a boolean type, then no parentheses are needed.

**Preferred:**
```javascript
result = (a > b) ? x : y;
```
**Not Preferred:**
```javascript
result = a > b ? x = c > d ? c : d : y;
```

Prototypes
---------
**Do not extend built-in prototypes.**

**Preferred:**
```javascript
var a = [];
if (!a.length) {
  console.log('winning');
}
```

**Not Preferred:**
```javascript
Array.prototype.empty = function () {
  return !this.length;
}

var a = [];
if (a.empty()) {
  console.log('losing');
}
```

Closures
---------
Closures should be named to produce better stack traces, heap and cpu profiles. Use closures, but do not nest them. 

**Preferred:**
```javascript
var afterConnect = function () {
  console.log('connected');
},
connect = function () {
  client.connect(afterConnect);
};

setTimeout(connect, 1000);

```
**Not Preferred:**
```javascript
setTimeout(function () {
  client.connect(function () {
    console.log('losing');
  });
}, 1000);
```


Classes
-------
Formal classes in JavaScript do not exist.  However, class-like objects may be closely approximated using the following patterns.

```javascript
var Animal, Cockatoo, gangGang,
  __hasProp = Object.hasOwnProperty,
  __extends = function (child, parent) { for (var key in parent) { if (__hasProp.call(parent, key)) child[key] = parent[key]; } function ctor() { this.constructor = child; } ctor.prototype = parent.prototype; child.prototype = new ctor(); child.__super__ = parent.prototype; return child; };

/*
  Base animal class provides attributes for names.
  @class Animal
*/
Animal = (function () {
  /*
    Genus name of this animal.
    @property genus
    @type String
  */
  Animal.prototype.genus = 'None';
  /*
    Species name of this animal, excluding genus.
    @property species
    @type String
  */
  Animal.prototype.species = 'none';
  /*
    Common name of this animal.
    @property commonName
    @type String
  */
  Animal.prototype.commonName = null;
  /*
    @constructor
    @param {String} genus genus name of this animal
    @param {String} species species name of this animal, excluding genus
    @param {String} commonName common name of this animal
  */
  function Animal (genus, species, commonName) {
    this.genus = genus;
    this.species = species;
    this.commonName = commonName;
  };
  /*
    Generates and returns the full species name of this animal.
    @method getSpeciesName
    @return {String} species name of this animal
  */
  Animal.prototype.getSpeciesName = function () {
    return "" + this.genus + " " + this.species;
  };
  /*
    Returns the common name of this animal.
    @method getCommonName
    @return {String} common name of this animal
  */
  Animal.prototype.getCommonName = function () {
    return this.commonName;
  };
  return Animal;
})();

/*
  Represents a Cockatoo animal.
  @class Cockatoo
  @extends Animal
*/
Cockatoo = (function (_super) {
  __extends(Cockatoo, _super);
  /*
    @constructor
  */
  function Cockatoo () {
    return Cockatoo.__super__.constructor.apply(this, arguments);
  };
  /*
    Generates and returns the common name of this cockatoo.
    @method getCommonName
    @return {String} common name of this cockatoo
  */
  Cockatoo.prototype.getCommonName = function () {
    return "" + Cockatoo.__super__.getCommonName.apply(this, arguments) + " Cockatoo";
  };
  return Cockatoo;
})(Animal);

// create an instance of a cockatoo
gangGang = new Cockatoo('Callocephalon', 'fimbriatum', 'Gang-Gang');

// outputs "Gang-Gang Cockatoo"
console.log(gangGang.getCommonName());
```


Backbone.js
---------
Adhere to all of the best practices outlined [here](https://gist.github.com/liammclennan/2886952#backbone-specific).


Object Oriented vs Procedural Programming
-----------------------------------------
While it may not seem apparent to beginner JavaScript developers, JavaScript has a rich object-oriented feature set.  These features should be maximized.  Avoid procedural programming conventions.  For example, instead of declaring orphaned variables and functions, use a class.

**Preferred, object-oriented:**
```javascript
AP.MyClass = (function () {
  MyClass.prototype.counter = 0;
  function MyClass (myRandomData) {
    this.myRandomData = myRandomData;
  };
  MyClass.prototype.increment = function () {
    return this.counter++;
  };
  return MyClass;
})();
```

**Not preferred, procedural:**
```javascript
  // orphaned variable, not part of any class or object
var counter = 0,
  // orphaned function, not part of any class or object
  increment = function () {
    return counter++;
  };
```
