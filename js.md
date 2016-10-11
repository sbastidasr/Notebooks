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

If a function is not a property of an object, it is invoked as a function. `this` will be assigned to the global variable.

```js
var sum = add(3, 4);    // sum is 7
function add(a, b){
  console.log(this) //this is the global object, not too useful
  return a+b
}
```



*Self = this*

This pattern can have unpredictable behavior with helper functions because they don't have access to outer this. So, we use `var self = this` in the inner function;



```js
myObject.double = function () {
  var self = this; // Workaround.
  var helper = function () {
    self.value = add(self.value, self.value);
  };
  helper(); // Invoke helper as a function.
};
// Invoke double as a method.
myObject.double( );
myObject.value(); // 6
```



**2. Method Form**

When a function is stored as a property of an object, we call it a method. When a method is invoked, this is bound to that object. (when invocation contains a dot `.` ). Like method speak on a `Cat` instance: `myCat.speak()`.It will be the object on which the function is being called upon. `myCat.speak() // "meow"` this inside `speak()` will be `myCat`.

```js
     var myObject = {
         value: 0,
         increment: function (inc) {
             this.value += typeof inc === 'number' ? inc : 1;
} };
myObject.increment( ); // myObject.value => 1
myObject.increment(2);// myObject.value => 3
```



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

speak.apply(fatRabbit, ["Hai!"]); // → The fat rabbit says 'Hai!'
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

**problem with new**

```js
class Dog {
  constructor() { this.sound = 'woof' }
  talk() { console.log(this.sound) }
}
const sniffles = new Dog()
sniffles.talk() // "woof"
$('button.myButton').click(sniffles.talk) //outputs stuff you dont want
```



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
    Dog.apply( this, arguments ); // calling the super constructor with same this + args.
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

### Factory functions

Functions that return an object. Variables are encapsulated by a closure, they become private. So they

- Have no leakage -> they only show what we want to return.
- makes all vars private.
- Also called **maker functions** or power constructors
- This is always bound correctly. 

Only talk is available, and it has access to sound. Sound is not available. 

```javascript
const dog = (age) => {
  const sound = 'woof'
  return {
    talk: () => console.log(sound),
    age: age
  }
}
const sniffles = dog(2)
sniffles.talk() // Outputs: "woof"
```



### object.assign ?????

Takes an object and assigns the properties of the other passed objects

object.assign({}, meower(state)) 



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


