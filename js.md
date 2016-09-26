# Javascript

## Functions

- Functions are first class objects. Meaning functions are objects as much as arrays or numbers and they work like any other value. And even though functions are objects, we'll review functions first as they will help understand some of the concepts of objects. 
- Inherit from object, can store name value pairs
- Called lambda in other languages.
- Inner functions have access to the params of the functions it is contained in. This is called **Static Scoping**
- Functions are **closures**, meaning the function still has access to the parent's values, even if the parent doesn't exist anymore (has already returned). The values on functions are references, **not copies of values**


#### Invocation: Arguments

- functions receive parameters called arguments
- Functions contain invocation args in an array-like object with length.

```js
function printArgsArray(a, b, c){
  console.log(arguments) // [1, 3, 5]
  console.log(b) // 2
}
printArgsArray(1, 3, 5) // 9
```



### Function Invocation + This

Function invocation is the calling of a function. There are many ways of calling a function, and the way of invoking it defines the contents of `this`. It is a placeholder for the current object. It is defined at the time of the call of the function. 

```javascript 
var alien {
	name: "Mark",
	sayHi : function(){
    	return "Hi, my name is: " + this.name;
    } 
 }
 alien.sayHi(); // Hi, my name is: Mark
```



**1. Function Form**

If a function is ran alone, `this` will be assigned to the global variable. If we call the sayThis function

```javascript
sayThis(){ console.log(this) }
```

using `sayThis();`, the global object will be the this, which is not very useful, and it will be printed in the console. This is set to the global obj, not very useful

*Self = this*

This pattern can have unpredictable behavior with helper functions because they don't have access to outer this. So, we use `var self = this` in the inner function;

```javascript
function SeatReservation(name, initialMeal) {
    var self = this;
    self.name = name;
    self.meal = ko.observable(initialMeal);
    setTimeout(function () { alert(self.name) }, 100);
} // otherwise, this is window in setTimeout.
```

**2. Method Form**

A method is a function that is assiociated to an object. Like method speak on a `Cat` instance: `myCat.speak()`.It will be the object on which the function is being called upon. `myCat.speak() // "meow"` this inside `speak()` will be `myCat`.

**3. Constructor Form**

A constructor (considered like a `class` alternative in JS) is a function that creates objects. Inside a constructor called with the keyword `new`, `this` is the newly created instance.

```js
new functionObject(args)
```

- A new object is created and assigned to this. 
- if no return is especified, this will be returned.

**4. Bind + Apply + Call**

**Bind**

`bind` copies the function with the same body but with `this` and some arguments predefined. It is usually used when you want to pass a function to an event handler or other async callback. It doesn't change the original function.

```javascript
var add = function(a, b) { return a + b };
var addFive = add.bind(null, 5);
console.log(addFive(3)); // 8
```

**Apply / Call**

Both set the first function's this to the fisrt argument (can be set to null) and then pass any other arguments to the rest of the function.

**Call** calls the function with the specified arguments.

**Apply** calls the function with args specified in an array. 

```js
function speak(line) {
  console.log("The " + this.type + " rabbit says '" + line + "'");
}
var fatRabbit = {type: "fat", speak: speak};

speak.apply(fatRabbit, ["Burp!"]); // → The fat rabbit says 'Burp!'
speak.call({type: "old"}, "Oh my."); // → The old rabbit says 'Oh my.'
```



## Objects

- Evertything on JS is an object, except null and undefined. Including numbers ```(2).toString();```
- Objects have properties ``` foo.1234; // SyntaxErrorfoo```   ```['1234']; // works ```
- **delete** is the only way to remove properties. Setting them to ```null``` or ```undefined``` maintains the property with ```null``` or ```undefined``` instead of making the object not have that property.
- There's no equality on objects. Doing ```===``` only checks if references to the same object (pointers) are the same. 
- Objects are passed by ref.

### Prototype

JS uses protoypyal inheritance. Lets say you have a constructor ```Car``` and its instance ```redCar```. The prototype is a field of the instance ( `redCar`) that points to another object. When calling a variable or function objects look up on themselves, if not available, it looks through the protototype. If you try to look for a key in ```redCar``` and it is not found, it will look for it in its prototype, and on the prototype's prototype. Until it finds it or returns undefined. That is why its called the *Prototype Chain*. *A prototype is a property that all Javascript objects have, that stores all methods that objects of that class share.*

Technically, the actual prototype used to go up the chain is ```__proto__	``` (which is a pointer to an object) but it shouldn't be used for performance issues. To get the prototype, we use ```var b = Object.getPrototypeOf(a)``` .

There are a couple of ways of creating an object that inherits from another (through its `__proto__`). The faster one is `var redCar = new Car()` and the more modern `var redCar = Object.create(Car.prototype)`. These two acheive almost the same effect, by setting the `__proto__` to `Car.prototype`.

**Note:**  ```__proto__	``` is **not** the same as `.prototype` which is available only on Functions (Constructors). And is the object assigned to proto and shared by all instances. So:

```javascript 
var o = {};
o.__proto__ // Object.prototype, all objects inherit from it.

Cat = function(){} 
Cat.__proto__ // Function.prototype all functions inherit from it.

var myCat = new Cat();
myCat.__proto__ // Cat.prototype
myCat.__proto__.__proto__ // Object.prototype

Cat.prototype.meow // -> function to meow available to cat instances
myCat.meow // Same as Cat.prototype.meow
myCat.__proto__.meow // Same as myCat.meow

myCat instanceof Cat; //true
myCat instanceof Object; //true
```



**New / updated ** properties on an instance are assigned to the object, not to the prototype

```javascript
var person = { kind: 'person' }
var zack = {}
zack.__proto__ = person
zack.kind = 'zack'
console.log(zack.kind); //=> 'zack'
console.log(person.kind); //=> 'person',not modified
```



### Constructors

Constructors in JS are the most used way to do prototype chains. Constructors are Functions, which are highly optimized on JS engines. Constructors on JS are **not** classes!

```javascript 
var Pet() = function(){}
```

**Note** Don't forget to use `new` when creating objects with constructors, otherwise, its properties will be assigned to the global object when using `this` 

#### Constructor Property

Functions have an automatically created `prototype` property. When the interpreter creates a `prototype` of a function, it makes a new object and a `constructor` property that references the function. For example: `Pet.prototype.constructor == Pet // true`

All instances inherit a constructor property from their prototype via `__proto__` 

```javascript
var Pet() = function(){}
Pet.prototype.constructor // => Pet(){} Created by default.
var cat = new Pet()
cat.constructor // Pet(){} function used to build cat 
cat.__proto__.constructor // Pet(){} the property is actually in __proto__
```

The `constructor` property is assigned to function `prototype`. Then the interpreter forgets about it. You have to manually keep it from being overwritten if changing the prototype.

The constructor property serves three purposes:

1. Get the class of an object. (Functions considered classes). Now you can check if both objects are built with the same constructor.
2. Create a new instance: Given an object, you can create a new instance that has the same class.
3. Invoking a super-constructor.



### New 

Creates a new object (instance) from a **Constructor** and assigns the instance's  __proto__ to the function's prototype. Returns the object or a value of running the constructor function.

`var doge = new Pet()`

1. Creates a new object doing: `var doge = new Object()` 
2. Sets `doge.__proto__` to `Pet.prototype`
3. `return Pet.call(doge) || doge; // normally doge is returned but constructors can also return a value`

```javascript
function Foo() {
    console.log("This runs!"); // does run!
    this.petProperty="cat"; // instance property
    this.catFunction = function(){ return "cate"} // Created for each instance
}
Foo.prototype.protoPetProperty = 'dog'; // One function, shared by all instances.

var foo = new Foo();
console.log(foo.petProperty) //"cat"
console.log(foo.protoPetProperty) // "dog"
console.log(foo.__proto__.protoPetProperty) // "dog", same as above
console.log(foo.catFunction()) //"cate"

Foo.isPrototypeOf(foo);// true
```



**Properties on the constructor or on the Prototype?**

When adding properties or methods to objects, there are two ways to do it. All properties inside the constructor, in this case `Foo(){ // properties }` are instance specific. Each instance has a separate version of the method. 

Adding the methods via `Foo.prototype.run = function(){ console.log("run!") }  ` adds the method to the prototype and thus, is shared by all instances. This occupies less memory and is faster. 



### Object.create()    [Personal favorite, when possible]

`Object.create` builds an object that inherits directly from the object passed as its first argument. Meaning that to create an object based on a Constructor prototype, you would do `Object.create(Car.prototype)` which is the same as  ```var redCar = new Car();```  You can create an object that doesn't inherit from anything with `Object.create(null);`

`var doge = Object.create( Pet.prototype )`

1. Creates a new object doing `var doge = new Object()` 
2. Sets `doge.__proto__` to `Pet.prototype`
3. `returns doge;`

So basically `Object.create()` doesn't execute the constructor, but still does prototypal inheritance.

```javascript 
function Foo() {
    console.log("This doesn't run!"); //does not run!
    this.petProperty="cat";
    this.catFunction = function(){ return "cate" }
}
Foo.prototype.protoPetProperty = 'dog';

var foo = Object.create(Foo.prototype);
console.log(foo.petProperty) //undefined
console.log(foo.protoPetProperty) // "dog"
console.log(foo.__proto__.protoPetProperty) // "dog"
console.log(foo.catFunction()) //"error"

Foo.prototype.isPrototypeOf(foo);// true
```

**Example using Object.Create to create with prototype  (ES6)**

```js
const Food = {
  name:"BaseFood",
  init: function (type) {
    this.type = type
  },
  eat: function () {
    console.log('You ate the ' + this.type)
  }
}
const waffle = Object.create(Food)
waffle.init('waffle')
waffle.eat() // Output: "You ate the waffle"
Food.isPrototypeOf(waffle)//true
console.log(waffle.name) // Falls back to the prototypes name = "BaseFood";
waffle.name="waffle"
console.log(waffle.name) // now it has its name: "waffle";
```

```javascript
var zack = Object.create(person, {age: {value:  13} });
console.log(zack.age); // => ‘13’
```

**Creating with additional properties** 

`Object.create` takes an additional map of property descriptors. It adds the new properties to the newly created instance. 

```
var myCat = Object.create(Pet.prototype, {
        name: {
                value: "Catename", 
                writable: true, 
                configurable: true, 
                enumerable: true
            },
            age: {
                value: 10, 
                writable: true, 
                configurable: true, 
                enumerable: true
            }
});
```



### Multi-level Inheritance with new

```javascript
// 1. level constructor
var Dog = function ( name ) {
    this.name = name;
};
Dog.prototype.bark = function () { /* ... */ };

// 2. level constructor
var TrainedDog = function ( name, level ) {
    Dog.apply( this, arguments ); // calling the super constructor
    this.level = level;
};

// set up two-level inheritance
TrainedDog.prototype = Object.create( Dog.prototype ); // Inherit from prototype to get all Dog's methods
TrainedDog.prototype.constructor = TrainedDog;
TrainedDog.prototype.rollOver = function () { /* ... */ }; // Extra candy

// instances
var dog1 = new Dog( 'Rex' );
var dog2 = new TrainedDog( 'Rock', 3 );
```

### Multi-level Inheritance with Object.create

Multi-level inheritance can also be done through Object.create(), although a bit less powerful because constructos are not run.

```javascript
const Musician = {
    solo : function(length) {
      var solo = "";
      for (var i=0; i<length; i++) {
          solo += this.sounds[i % this.sounds.length] + " ";
      }
      console.log(solo);
  }
};

var Guitarist = Object.create(Musician)
Guitarist.sounds = ['Twang', 'Thrumb', 'Bling'],
Guitarist.strings = 6;
Guitarist.tune = function() {
    console.log('Be with you in a moment, tuning');
};

var nigel = Object.create(Guitarist)
nigel.solo(5); // "Twang Thrumb Bling Twang Thrumb "
console.log(nigel.strings) // 6
nigel.tune(); // "Be with you in a moment, tuning"
```



```
### Factory functions

Functions that return an object. consts are encapsulated by closure. Only talk is available, and it has access to sound. Sound is not available. 

​```js
const dog = () => {
  const sound = 'woof'
  return {
    talk: () => console.log(sound)
  }
}
const sniffles = dog()
sniffles.talk() // Outputs: "woof"
```

### 

###Looping through Objects

```javascript
var user = {
  name:"Brendan Eich",
  profession:"programmer",
  invented:"JavaScript"
};
for(var prop in user){
   console.log(user[prop]); // Brendan Eich programmer JavaScript
}
```





# NOT YET DONE 

## Augmenting Built in types.

- For example add **trim** to string

```js
  String.prototype.trim = function () {
    return this.replace(
      /^s*(S*(s+S+)*)s*$/, "$1");
  }
```

**Augmenting Strings**

```js
var data = {
  name: 'Carl',
  state: 'Happy'
};
var template = 'someone named {name} is {state}'
var myStr = template.supplant(data);

String.prototype.supplant = function(o) {
  return this.replace(/{([^{}]*)}/g,function(a, b) {
    var r = o[b];
    return typeof r === 'string' || typeof r === 'number' ? r : a;
  });
};
```







### Functor

A functor is a function, given a value and a function, unwraps the values to get to its inner value(s), calls the given function with the inner value(s), wraps the returned values in a new structure, and returns the new structure.

Examples of functors: map

**Monad:** Is a functor (like stream) that implements flatmap in addition to map. If the callback returns a monad, it will be flattened into its containing value before being passed on.

**Flatmap:** takes a return in stream. waits for it to resolve(flattens it) and takes the value and does something

Promises in JS ES6 implement Flatmap, but called .then, bind, chain.

Ways of instantiating an object:

- Factories(DEFAULT): Functions that create and return objects
- More flexible and easier
- Have private properties
- Don't use this
- Prototypes
- Better performance
- Less memory
- Class: instanciate with new (fast but dont use because this is not correctly bound)



## JSON**

```js
var string = JSON.stringify({name: "X", born: 1980});
console.log(string); // → {"name":"X","born":1980}
console.log(JSON.parse(string).born); // → 1980
```

**Filter**

It filters out the elements in an array that don’t pass a test.

```js
console.log(ancestry.filter(function(person) {
  return person.father == "Carel Haverbeke";
}));
```

**Map**

The map method transforms an array by applying a function to all of its elements and building a new array from the returned values. The new array will have the same length as the input array, but its content will have been “mapped” to a new form by the function.

```js
function plus1(value){
  return value+1
}
[3,4].map(plus1) //[4,5]

// Transforms the array into only an array of names
const dragons = [
  {name:'Fluffykins', health:70},
  {name:'Munchkin', health:50},
]
const names = dragons.map((dragon) => dragon.name)
console.log(names) //outputs only the names
const names = dragons.map((dragon, i) => dragon.name)


//maps an array of peopke to only an array of names
console.log(map(overNinety, function(person) {
  return person.name;
}));
```

**Reduce**

computing a single value from arrays: adding numbers or finding the person with the earliest year of birth in the data set. The parameters to the reduce function are, apart from the array, a combining function and a start value.

```js
console.log(reduce([1, 2, 3, 4], function(a, b) {
  return a + b;
}, 0));
// → 10
console.log(ancestry.reduce(function(min, cur) {
  if (cur.born < min.born) return cur;
  else return min;
}));
```

**Getters and setters**

```js
var pile = {
  elements: ["eggshell", "orange peel", "worm"],
  get height() {
    return this.elements.length;
  },
  set height(value) {
    console.log("Ignoring attempt to set height to", value);
  }
};
console.log(pile.height);
// → 3
pile.height = 100;
// → Ignoring attempt to set height to 100
```

--

# Douglas Crockford the Javascript programming language

Javascript definitive guide Oreilly (rhinoceros)

Loosely typed: any type can be used anywhere

**names**

- all functions, vars, params lowercase
- Constructors uppercase
- _ initial for implementation
- $ initial for machines

**lambda:** Functions as first class objects

**logical** Operands

 && And

- if first operand is truthy, returns second.
- else returns first.
- Can be used to avoid null refs.

```js
return a && a.member;
```

|| Or

- if first operand is truthy, returns first.
- else returns second.
- Can be used to fill defaults

```js
var last = input || nr_items;
```

**string to number** +"42" = 42

**strings** are immutable

**false** values

 undefined (not defined or can be set as undefined )

 0

 NaN

 false

 null (no value

 ""

- everything else is objects

**Objects** An object is a hashtable (name, value) pairs

return; retue=rns undefined.

constructors will return this.

**maker function**

```js
function maker(name, grade){
  var it = {};
  it.name=name;
  it.grade=grade;
  return it;  
}
myObject = maker('Jack', 2);
```

## 



## arrays

- DO NOT USE FOR IN
- use traditional for
- mutable
- use [] to create neew array.
- linked to array.prototype
- check if is array: value instanceof Array;
- Dont use arrays as prototypes.
- augment array or array.prototype;

**methods**

- concat: merges arrys
- join: outputs a single string
- pop + push
- slice sort .
- delete will leave an undefined hole in the middle use **splice** instead.
- splice(a,b) starting at a delete b number of stuff.

**splice**

```js
myarr=['a','b','c','d'];
  myarr.splice(1,1); //['a','c','d']
```

## typeof

object='object'

function='function'

***array='object'***

number='number'

string='string'

boolean='boolean'

***null='object'***

undefined='undefined'

## eval DONT USE Unless you can absolutely trust JSON.

- Compiles and returns the result
- Is what the browser uses to convert strings into actions.

```js
eval(string)
```

- same as New function Functtion

## (global) Object

- container for global vars and all built in objects
- On browsers window is the global.
- sometimes 'this' points to it. *var global = this;*

**this is evil**

- Functions and vars can crash with each other.
- try to reduce its use
- vars without var are global.
- Create ONE global object and place everything there.

## Encapsulation

- Has no leakage
- Only shows what we want to return.
- Uses closures
- makes all vars private.

```js
YAHOO.Trivia = function () {
  //define common vars & functions here
  return {
    getNextPoster: function (cat,diff){   /* ...Stuff..  */}
    showPoser: function () {   /* ...Stuff..  */}
    }
}
var trivia = YAHOO.Trivia();
}
```

### RegExp

-patterns enclosed in slashes



# End Douglas

------

#### This

**Object Literal**

```js
 var stooge = {
         "first-name": "Jerome",
         "last-name": "Howard"
};
stooge.first-name="asd";
stooge["first-name"]; //asd
```

**Prototype** Every object is linked to a prototype object from which it can inherit properties.

When we make changes to an object, the object’s prototype is not touched:

The prototype link is used only in retrieval. If we try to retrieve a property value from an object, and if the object lacks the property name, then JavaScript attempts to retrieve the property value from the prototype object. And if that object is lacking the property, then it goes to its prototype, and so on. This is called ***delegation***.

So object.prototype points to another object.

This example is to illustrate prototypes. DONT use new.

```js
var man = {};  
man.sex="male";

var yehuda = Object.create(man);  
yehuda.firstName="Yehuda";  
yehuda.lastName="Katz";

console.log(yehuda.sex);       // "male"  
console.log(yehuda.firstName);  // "Yehuda"  
console.log(yehuda.lastName);   // "Katz"

var man2 = Object.getPrototypeOf(yehuda); // returns the man object  
console.log(man===man2);
```

### Enumeration

For in //doesnt guarantee order.

For each

for(var i; i<length;i++)

### delete

delete another_stooge.nickname;

### Global Vars

Weaken the resiliency of programs.

Use one single global var MyApp.

#### Functions

Functions in js are objects. Can be used as any other values. And can have other inside functions.

Invocation: Every call receives 'this' and args.

### Method Invocation

- When a function is stored as a property of an object, we call it a method. When a method is invoked, this is bound to that object. (when invocation contains a dot . )

```js
     var myObject = {
         value: 0,
         increment: function (inc) {
             this.value += typeof inc === 'number' ? inc : 1;
} };
myObject.increment( ); // myObject.value => 1
myObject.increment(2);// myObject.value => 3
```

### The Function Invocation

- When a function is not the property of an object, then it is invoked as a function:

```js
var sum = add(3, 4);    // sum is 7
```

When a function is invoked with this pattern, this is bound to the global object.

```js
myObject.double = function () {
  var that = this; // Workaround.
  var helper = function () {
    that.value = add(that.value, that.value);
  };
  helper(); // Invoke helper as a function.
};
     // Invoke double as a method.
myObject.double( );
myObject.value(); // 6
```

### The Constructor Invocation Pattern **DO NOT USE THIS (new)**

- If a function is invoked with the *new* prefix, then a new object will be created with a hidden link to the value of the function’s prototype member, and this will be bound to that new object.
- Functions intended to use **new** prefix are called constructors.
- By convention, they are kept in variables with a capitalized name.
- *new* returns a new object with a pointer to the constructor's prototype

```js
// Create a constructor function called Quo. It makes an object with a status property.
var Quo = function (string) {
  this.status = string;
};
// Give all instances of Quo a public method called get_status.
Quo.prototype.get_status = function () {
  return this.status;
};

var myQuo = new Quo("confused");// Make an instance of Quo, with a link to the constructor.prototype.
document.writeln(myQuo.get_status()); // Get method from he constructor prototype (Quo)
```

### The Apply Invocation Pattern

functions can have methods.

The apply method lets us construct an array of arguments to use to invoke a function. It also lets us choose the value of this. The apply method takes two parame- ters. The first is the value that should be bound to this. The second is an array of parameters.

```js
// Make an array of 2 numbers and add them.
var array = [3, 4];
var sum = add.apply(null, array);    // sum is 7
// Make an object with a status member.
var statusObject = {
  status: 'A-OK'
};
// statusObject does not inherit from Quo.prototype,
// but we can invoke the get_status method on
// statusObject even though statusObject does not have
// a get_status method.
var status = Quo.prototype.get_status.apply(statusObject);
// status is 'A-OK'
```

#### Return

A function always returns a value. If no return specified, it returns udefined.

### Public methods

- method that uses this to access an object
- can be reused with many 'classes'

```js
function(string) { //can be used with any object
  return this.member + string;
}
```

### MOdule

- Varuables defined in a module are only visible in the module
- FUnctions can be used to contain modules

### Singletons

- Objects instantiated *once*
- can be created with object notation
- Another way of writing a singleton.(using closures)

```js
var singleton = function(){
  var privateVar;
  function provateFunc(){};

  return {
    firstmethod: function(a,b){
      //dostuff with private var & function
    }
  }
}(); //we assign the result of invoking the function to the singleton.

singleton.firstmethod(1,2);
```

### Power Constructor

- using a singleton module pattern in a constructor and we have a power constructor

```js
function powerConstructor(){
  var that=object(oldObject); //or use that={};
  var privateVar;
  function provateFunc(){};
  //Privileged methods, gettes, setters etc.
  that.firstmethod: function(a,b){
      //dostuff with private var & function  
  }
  return that;
}
myObject=power_constructor();
```

**Functions in Loops**

When assigning functions in a loop, all will be bound to the same closure.

~~

REALLY REMEMBER THIS.

# Debugging

if(something==='wrong){

 debugger;

}

~ Complete this Stuff

------

#### High Order Functions

A function that can accept other function as an argument. This is called **Composition:** of functions.

This works because in js functions are values.

```js
var isDog = function(animal){
	return animal.species==='dogs';
	}
var dogs = animals.filter(isdog);
var dogs = animals.filter(anml => anml.species==='dogs')
```

##### Map

Transforms the array.

```js
//returns names of the animals.
var names = animals.map(function(animal){
	return animal.name;
})

//map to return other val
var names = animals.map(function(animal){
	return animal.name+ 'is a '+ animal.species;
})
```

##### Reduce

See my [Reduce](moarCode/reduceExamples.md) page for details.

```js
var orders = [
  { amount: 250 },
  { amount: 400 }
]

var totalAmount = orders.reduce( function(sum, order) {
  return sum + order.amount
}, 0) //takes an object as starting point. 0 here
//totalAmount=>650
```

## 3promises

Dealing with asyinc callbacks.

##### Arrow functions (ES6)

(args) => {code}

```js
var names = animals.map((x) => x.name)
```

The **create**

**Closures:** The function body has access to vars defined outside it. (All functions in JS are closures).

Example: Callback functions can access stuff availabele to them where they were declared.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

**pure functions**

**Call+apply**

**assign**

**Composition:** What objects do

**Inheritance:** modeled around what they are

**Factories:** Function that create objects and return the. Simpler than classes.

#### classes DO NOT USE

```js
class Dog {
  constructor() {
    this.sound = 'woof'
  }
  talk() {
    console.log(this.sound)
  }
}
const sniffles = new Dog()
sniffles.talk() // Outputs: "woof"
$('button.myButton').click(sniffles.talk) //outputs stuff you dont want
```

#### Factories USE

```js
const dog = () => {
  const sound = 'woof',
  return {
    talk: () => console.log(sound)
  }
}
const sniffles = dog()
sniffles.talk() // Outputs: "woof"
```

favor composition over inheritance

### Composition

Designing types around what they do

Composition example [here](moarCode/compositionExample.md)

### Inheritance

Designing types around what they are

You get the object but also a lot of extra stuff you dont need

**object.assign({}, meower(state)) **

Takes an object and assigns the properties of the other passed objects

**currying** Function that takes arguments. Uses the first arg and returns a new function and calls it with the secnd arg and so on.

**lodash**

**Bind**

------

## Don't Use

**new** Use **create**

**for** or **for in** because order is not known. Use: **for each**

```js
Object.keys(objet).for each
```

------

## Use

JsLint

------

------

## Angular.js

Directives

extensions to HTML like ng-app to star angular

ng-model="name" : adds model from a text box to the view model. Can be printed with {{ name }}

adds data array to viewModel

<body ng-init="names=[{name:'John Smith', city:'Phoenix'}, {name:'John Doe', city:'Philly'}]">

ng-repeat allows to list an array

ng-repeat="person in personArray"

Order:

ng-repeat="just in personArray | orderBy:'name'"

{{cust.name | uppercase}}

Filter/search

<input Type="text" ng-model="nameText" />

<li ng-repeat="just in personArray | filter:nameText '">

{{cust.name }} </li>

Scope is a view-Model. It is the glue between view and controller: $scope.

Controller sould't know the view because it may be different view for desktop, mobile etc.. View may know the controller.

$scope is the data of the view.

VController receives the $scope and passes the data in it.

function SimpleController($scope){

 $scope.data='asd';

}

view. Only this part gets the div,

<div ng-controller='SingleController'>

{{ data }}

<//div>

View Defines which controller it will use

ng-click="loginEmail()"

calls the function on the controller

------

## D3.js

```js
<script>

      var scale = d3.scale.linear()
        .domain([1, 5])   // Data space
        .range([0, 200]); // Pixel space

      var svg = d3.select("body").append("svg")
        .attr("width",  250)
        .attr("height", 250);

      function render(data, color){

        // Bind data
        var rects = svg.selectAll("rect").data(data);

        // Enter: Constructing the elements. only adds elements that weren't there and properties that won't change

        rects.enter().append("rect")
          .attr("y", 50)
          .attr("width",  20)
          .attr("height", 20);

        // Update: stuff that changes
        rects
          .attr("x", scale)
          .attr("fill", color);

        // Exit: delete unused
        rects.exit().remove();
      }

      render([1, 2, 2.5],     "red");
      render([1, 2, 3, 4, 5], "blue");
      render([1, 2],          "green");

    </script>


rects.enter().append("rect").data
```

------

# Videos

Asynchronous Programming at Netflix - @Scale 2014 - Web

https://www.youtube.com/watch?v=gawmdhCNy-A

James Coglan: Practical functional programming: pick two | JSConf EU 2014

https://www.youtube.com/watch?v=XcS-LdEBUkE

Bret Victor The Future of Programming

https://www.youtube.com/watch?v=8pTEmbeENF4

Philip Roberts: What the heck is the event loop anyway? | JSConf EU 2014

https://www.youtube.com/watch?v=8aGhZQkoFbQ

------

# Extras

cool guy tutorial videos: https://www.youtube.com/playlist?list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84

------

# Books

- Programming Javascript applications - Eric Elliot
- You Don't Know JS: Up & Going - Kyle Simpson

# jsconf

# STUFF YOU SHOULDN'T USE

## Constructor functions

Start with capital letter and create instances with new

The new object’s prototype will be the object found in the prototype property of the constructor function.

```js
function Rabbit(type) {
  this.type = type;
}
var blackRabbit = new Rabbit("black");
console.log(blackRabbit.type);  // → black

Rabbit.prototype.speak = function(line) {
  console.log("The " + this.type + " rabbit says '" + line + "'");
};
blackRabbit.speak("Doom..."); // → The black rabbit says 'Doom...'
```

