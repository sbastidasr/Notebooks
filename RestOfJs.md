### object.assign

Takes an object and assigns the properties of the other passed objects

object.assign({}, meower(state)) 



### Looping through Objects

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

​```js
  String.prototype.trim = function () {
    return this.replace(
      /^s*(S*(s+S+)*)s*$/, "$1");
  }
```

**Augmenting Strings**

​```js
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

**false** values (not objects?)

 undefined (not defined or can be set as undefined )

 0

 NaN

 false

 null (no value

 ""





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



#### RegExp

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



Functions in Loops**

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

## promises

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



**assign**

**Composition:** What objects do

**Inheritance:** modeled around what they are

**currying** Function that takes arguments. Uses the first arg and returns a new function and calls it with the secnd arg and so on.

**

------

## Don't Use



**for** or **for in** because order is not known. Use: **for each**

```js
Object.keys(objet).for each
```

------

## Use

JsLint

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






## Steams

Pipes move data from one program to another.

    var stream = fs.createReadStream(__dirname + '/data.txt');
    //stream.pipe(res);
   stream.pipe(oppressor(req)).pipe(res); //double piping to compress and repsond


##pipe 
.pipe() is just a function that takes a readable source stream src and hooks the output to a destination writable stream dst:

src.pipe(dst)

`.pipe(dst)` returns `dst` so that you can chain together multiple `.pipe()` calls together:

```
a.pipe(b).pipe(c).pipe(d)
```

which is the same as:

```
a.pipe(b);
b.pipe(c);
c.pipe(d);
```





## Promises

```


Tre

```



Real setTimeout example

```javascrip
var timer = new Promise(function(resolve, reject) { 
	// A mock async action using setTimeout
	setTimeout(function() { resolve(10); }, 3000);
})
ß
timer
.then(function(num) { console.log('first then: ', num); return num +10; })
.then(function(num) { console.log('second then: ', num); return num +10; })
.then(function(num) { console.log('last then: ', num);});

// From the console:
// first then:  10
// second then:  20
// last then:  30

```



Catch Example

```javascript
new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() { reject('rejectvalue!'); }, 3000);
})
.then(function(e) { console.log('done', e); })
.catch(function(e) { console.log('catch: ' + e); });

/*SAME AS DOING*/
.then(
  function(e) { console.log('done', e); }, 
  function(e) { console.log('catch: ' + e); }
);

// From the console:
// 'catch: Done!'


```





Promises 2.0 

```javascript
// ES6
function printAfterTimeout(string, timeout){
  return new Promise((resolve, reject) => {
    setTimeout(function(){
      resolve(string);
    }, timeout);
  });
}
printAfterTimeout('Hello ', 2e3).then((result) => {
  console.log(result);
  return printAfterTimeout(result + 'Reader', 2e3);
}).then((result) => {
  console.log(result);
});
```



# Awesome ES6 Features

## Arrow functions

```javascript
//ES6
$('.btn').click((event) =>  this.sendData()); // this will reference the outer this
// implicit returns
const ids = [291, 288, 984];
const messages = ids.map(value => `ID is ${value}`);

```



### Iterating through arrays ES6 

```javascript
// ES6
const array = ['a', 'b', 'c', 'd'];
for (const element of array) {
    console.log(element);
}
```



### Default Params

```javascript
ES6

function point(x = 0, y = -1, isFlag = true){
  console.log(x,y, isFlag);
}
point(0, 0) // 0 0 true
point(0, 0, false) // 0 0 false
point(1) // 1 -1 true
point() // 0 -1 true
```

### Rest Operator

```javascript
// ES6
function printf(format, ...params) {
  console.log('params: ', params);
  console.log('format: ', format);
}
printf('%s %d %.2f', 'adrian', 321, Math.PI);

```

### Spread Operator

```javascript
Math.max(...[2,100,1,6,43]) // 100
```





Promise.then()

- calls either successs or reject after completing the work(kinda like the callbacks)
- You can return values from callbacks and chain them to other promises
- YOu can apass error handling fungi\tion on the las call
- ​