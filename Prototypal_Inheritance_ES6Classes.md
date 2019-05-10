# Prototypal Inheritance in JS

## Prototypes

Whenever a function foo() {} is declared, the JS engine creates 2 objects:
1. Object foo (since a function is also an object with a call method)
2. Object prototype

the foo object contains a property called 'prototype' which points to the prototype object

```
foo = {
  prototype: --------------------------> prototype = {}
  call:
  .
  .
  .
  }
```

this ```prototype``` object comes into play when the ```new``` keyword is used

Ex.
```js
function Human(name){
  this.name = name
 }
 
let elessar = new Human('Elessar Aragorn')
```
the ```new``` keyword does 2 things:
1. inserts ```var this = {}``` before line 26 and ```return this``` after line 26
2. the elessar object will contain an extra ```__proto__``` object which will point to the ```prototype``` object of Human

so now you have:
```
  Human = {
    prototype: -------------------------> prototype = {}
    call:                                     ^
    .                                         |  
    .                                         |
    .                                         |
    }                                         |
                                              |
  elessar = {                                 |
    name: 'Elessar Aragorn',                  |
    __proto__: --------------------------------
  }
```
