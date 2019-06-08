# CheckPoint Solutions and Explanations
### Table of Contents
#### Orientation & Precourse
[Scopes and Closures](#scopes-and-closures) \
[1](#scope-1-what-variables-does-somefunction-have-access-to)
[2](#scope-2-what-is-the-value-of-player)
[3](#scope-3-what-is-the-value-of-result)
[4](#scope-4-what-is-the-value-of-result)
[5](#scope-5-what-is-the-top-level-scope)
[6](#scope-6-what-is-the-value-of-result1)
[7](#scope-7-how-do-you-get-c)
[8](#scope-8-what-is-result4)
[9](#scope-9-what-is-the-final-value-of-result)
[10](#scope-10-what-does-bar-return)

[this](#this) \
[1](#this-1-what-message-will-be-alerted)
[2](#this-2-what-is-the-value-of-result)
[3](#this-3-what-is-the-value-of-result1-and-result2)
[4](#this-4-what-will-be-the-value-of-result)
[5](#this-5-what-is-the-value-of-result)
[ ](#)

[Classes](#classes) \
[1](#classes-1-what-will-be-the-value-of-result)
[2](#classes-2-what-will-be-the-value-of-result)
[3](#classes-3-what-is-the-value-of-result)
[4](#4)
[5](#5)


[](#) \
[2](#2)
[3](#3)
[4](#4)
[5](#5)
---
---


## Scopes and Closures
### Scope 1: What variables does someFunction have access to?
```javascript {.line-numbers}
var globalVariable = "Hello world!";

function someFunction(){
  var localVariable = 'Hello local scope!';
  function nestedFunction() {
    var nestedLocalVariable = 'Hello nested local scope!';
  }
}
```
#### Answer
someFunction has access to localVariable and globalVariable. 
#### Explanation
nestedLocalVariable is within the function nestedFunction, and thus the outer function someFunction cannot access it.
However, for localVariable, it is declared in someFunction thus someFunction has access to it. globalVariable is declared globally, so everything has access to it.


### Scope 2: What is the value of player?
```javascript {.line-numbers}
var player = { score: 3 };

function doStuff(obj) {
  obj = {};
}

player = doStuff(player);
```

#### Answer
undefined
#### Explanation
The function doStuff never returns anything, thus player will be assigned the value of undefined, which is the result of a function which does not have a return statement.

### Scope 3: What is the value of result?
``` javascript {.line-numbers}
var x = 10;

function outer () {
  var x = 20;
  function inner () {
    return x;
  }
  return inner();
}

var result = outer();
```
#### Answer
20
#### Explanation
When result is assigned on the last line it invokes the function outer, which returns the result of invoking inner, which in turn returns the locally scoped x, which in this case is 20, because the line just above the inner function declaration declares x to be 20, which is a locally scoped variable.

### Scope 4: What is the value of result?
```javascript {.line-numbers}
var x = 30;

function get (x) { return x; }
function set (value) { x = value; }

set(10);
var result = get(20);
```
#### Answer
20
#### Explanation
The function get simply returns any value it is given, and thus will return 20 on the last line. The global variable x and the function set do not affect result at all.

### Scope 5: What is the top level scope?
#### Answer
Global scope
#### Explanation
Global scope is the correct term for this concept. The other options are not terms that are used in the industry. Window scope is close, but window is specific to a browser, whereas global is universally applicable.

### Scope 6: What is the value of result1?
```javascript {.line-numbers}
var a = 0;
var b = 0;

function makeIncrementer() {
  var c = 0;
  a += 1;
  return function() {
    b += 1;
    c += 1;
    return c;
  }
}

var myIncrementerX = makeIncrementer();
myIncrementerX();
myIncrementerX();

var myIncrementerY = makeIncrementer();
myIncrementerY();
myIncrementerY();
myIncrementerY();

var result1 = myIncrementerX();
var result2 = myIncrementerY();
var result3 = a;
var result4 = b;
```
#### Answer
3
#### Explanation
result1 is equal to returned value of myIncrementerX. myIncrementerX was created with makeIncrementer and then is invoked three times (twice after being declared, and then again when result1 is assigned). Each time myIncrementerX is invoked it increases b and c by one, and then returns c. Since c is locally declared within makeIncrementer and is part of the myIncrementerX closure, when myIncrementerX is first declared c is set to 0. Each time myIncrementerX is invoked (which, we'll remember, is three times) c is incremented by one. Thus result1 is 3.

### Scope 7: How do you get c?
```javascript {.line-numbers}
function makeIncrementer() {
  var c = 0;
  return function() {
    c += 1;
    return c;
  }
}
```
#### Answer
Via makeIncrementer()()

#### Explanation
When makeIncrementer is invoked it returns a function, and then invoking that function returns c. Incidentally the following code would also work, but is not one of the possible answers (and is just provided for explanation purposes):
```javascript {.line-numbers}
let myFunc = makeIncrementer();
let theRealCValue = myFunc();
```

### Scope 8: What is result4? 
```javascript {.line-numbers}
var a = 0;
var b = 0;

function makeIncrementer() {
  var c = 0;
  a += 1;
  return function() {
    b += 1;
    c += 1;
    return c;
  }
}

var myIncrementerX = makeIncrementer();
myIncrementerX();
myIncrementerX();

var myIncrementerY = makeIncrementer();
myIncrementerY();
myIncrementerY();
myIncrementerY();

var result1 = myIncrementerX();
var result2 = myIncrementerY();
var result3 = a;
var result4 = b;
```
#### Answer
7
#### Explanation
Since b is a global variable, and because both myIncrementerX and myIncrementerY increment b, then each time either of them is invoked b increases by one. Together myIncrementerX and myIncremeterY are invoked seven times (the assignation of result1 and result2 count as invocations).

### Scope 9: What is the final value of result?
```javascript {.line-numbers}
var result = 0;

for (var i=0; i < 3; i++) {
  setTimeout(function() {
    result += i;
  }, 1000);
}
```
#### Answer
9
#### Explanation
Each time the loop runs, the anonymous function passed into setTimeout is added to the queue, but the anonymous function is NOT invoked. After the third loop i is increased from 2 to 3, and the ending condition is met so the loop ends, but the value of i is still 3. Once the loop is finished, which is the only synchronous part of the code, the asynchronous part can now complete. Thus the anonymous function (which had been added to the queue three times, once for i=0, once for i=1, once for i=2) is now invoked three times. For each invocation i is added to result, but we'll remember that i = 3 after the end of the loop. Thus 3+3+3 = 9 and 9 is the result.

### Scope 10: What does bar return?
```javascript {.line-numbers}
function add(x) {
  return function(y) {
    return x + y;
  }
}

var foo = add(1);
var bar = add(3);

foo(3);
// what does this return?
bar(2);
```
#### Answer
5
#### Explanation
bar is a function which will add whatever number is given to it when it was declared (in this case 3) to whatever number is given to it when it is invoked (in this case 2). Thus 2 + 3 = 5, which is the answer.

---

## this
### this 1: What message will be alerted?
```javascript {.line-numbers}
var name = "Window";
var alice = {
  name: "Alice",
  sayHi: function() {
    alert(this.name + " says hi");
  }
};
var bob = { name: "Bob" };

setTimeout(alice.sayHi.call(bob), 1000);
```
#### Answer
Bob says hi, immediately

#### Explanation
The call method immediately invokes the function it is being used with, so the setTimeout essentially doesn't matter.

### this 2: What is the value of result?
```javascript {.line-numbers}
var deckOfCards = {
  cardCount: 52,
  draw: function (number) {
    this.cardCount -= number
  }
}

deckOfCards.draw(10)
deckOfCards.draw(10)
var result = deckOfCards.cardCount
```
#### Answer
32
#### Explanation
Each time deckOfCards.draw is invoked it subtracts 10 from cardCount. It is invoked twice, so 52 - 10 - 10 is 32.

### this 3: What is the value of result1 and result2? 
```javascript {.line-numbers}
function isObject (other) {
  return this === other;
}

var alice = { name: 'Alice', f: isObject };
var bob   = { name: 'Alice', g: isObject };

var result1 = alice.f(alice);
var result2 = alice.f(bob);
```
#### Answer
result1 is true; result2 is false
#### Explanation
The variable result1 is equal to the method f in the alice object being called to compare the alice object to this. In this case, this is the alice object, since the method f is in the alice object. result2, on the other hand, is still calling the method f from the alice object, so this will still be alice, but comparing it to the bob object, which is being passed into function as an argument. The bob object is not equal to the alice object (they have differently named methods as well as occupying different places in memory).

### this 4: What will be the value of result? 
```javascript {.line-numbers}
var x = 10;

var puzzle = function (amount) {
  this.x += amount;
};

var alice = { x: 10, f: puzzle };

puzzle(5);
alice.f(5);
alice.g = alice.f;
alice.g(5);

var result = alice.x;
```
#### Answer
20
#### Explanation
The variable result is assigned to alice.x. alice.x originally starts out as 10, and then when alice.f(5) is invoked it adds 5 to x, and x is then 15. Then the method g on the alice object is assigned to the same function as the f method on the alice object. When alice.g(5) is invoked once again 5 is added to x, which results in 20. The puzzle(5) invocation does nothing to the alice object, because when puzzle is invoked this is the global scope, not the alice object.

### this 5: What is the value of result?
```javascript {.line-numbers}
window.name = 'window'

var alice = {
  name: 'Alice',
  greet: function () {
    return "Hi I am " + this.name
  }
}

var bob = {
  name: 'Bob',
  greet: alice.greet
}

var result = alice.greet()
```
#### Answer
"Hi I am Alice"
#### Explanation
When result is assigned, it is given the value of the greet method on the alice object. As such, this is equal to the alice object, and will return Alice as the 'this.name'. Remember that the value of this is always the context left of the period at the moment of invocation.


---
## Classes
### Classes 1: What will be the value of result?
```javascript {.line-numbers}
var pie = {
  rating: 6
};

var cherryPie = Object.create(pie);
var applePie = Object.create(cherryPie);

applePie.rating = 8;

var result = cherryPie.rating;
```
#### Answer
6
#### Explanation
Even though cherryPie was created using pie just as applePie was, both have pie as the precedent in their prototypal chain and are not related to each other. Thus when the rating of appliePie is changed it does not affect cherryPie.

### Classes 2: What will be the value of result?
```javascript {.line-numbers}
var MyFunc = function () {};
MyFunc.prototype.x = 10;

var obj1 = new MyFunc();
var obj2 = new MyFunc();

var result = obj1.x + 10;
```
#### Answer
20
#### Explanation
Both of the objects are created using MyFunc(), and MyFunc has an x value in its prototype, thus when obj1.x is invoked it looks up obj1's prototypal chain until it finds MyFunc's x value of 10 and returns that.

### Classes 3: What is the value of result?
```javascript {.line-numbers}
var obj1 = { x: 10 };

var obj2 = Object.create(obj1);
var obj3 = Object.create(obj2);

obj2.x = 20;

var result = obj3.x + 10;
```
#### Answer
30
#### Explanation
obj3 is created with obj2 as its prototype, and obj2 in turn was created with obj1 as its protoype. Thus the prototype chain is: obj3 >>> obj2 >>> obj1. Originally obly obj1 had an x key, but then obj2 is assigned an x key. So when obj3.x is invoked it goes up the prototype chain one step, to obj2, and finds that obj2.x = 20, so it returns that. 20 + 10 = 30.


### Classes 4: What will be logged to the console?
```javascript {.line-numbers}
var obj = {
  foo: function(){
    console.log(this);
  }
}

var obj2 = {
  foo: obj.foo
}

obj.foo.call(obj2);
```
#### Answer
obj2
#### Explanation
The method call immediately invokes the method obj.foo but with this = obj2, so console will log obj2.

### Classes 5: How do objects inherit from other objects in Javascript?

#### Answer
An object delegates to the prototype object to inherit properties and methods of another object.
#### Explanation

---
##
### 1
```javascript {.line-numbers}
```
#### Answer
#### Explanation

---
##
### 1
```javascript {.line-numbers}
```
#### Answer
#### Explanation
