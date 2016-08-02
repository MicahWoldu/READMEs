#JavaScript Functional Classes

###Introduction

JavaScript is a prototypal object-oriented language, meaning that objects can inherit properties directly from other objects. This will be a bit different for those who have learned to develop in other langauges like Java or C#, where a subclass inherits from a superclass, and objects are instances of classes.

If you want to create multiple objects with similar methods and properties in JavaScript, the best options are functional classes, or the constructor function. There are four styles of functional classes.

#####*Functional*
 
#####*Functional-shared*

#####*Prototypal*

#####*Pseudoclassical*

Each of these four is a different style of object construction; they all accomplish the same thing in a slightly different way.

Take for example, a street with many houses. Every house on the street is painted a `colour` and it will have a front `door` which can either be opened or closed. So Houses may be thought of as objects.

If we implement functional classes here, every instance created by the function will have the attributes of house colour and the method to open and close the door.

From here we can potentially create unlimited new `House` objects, which are similar. So each functional class performs the same basic steps to generate the new houses:

* 	generate an object
* 	assign properties
*	add methods
*	return that object

Each of the four styles has its pros and cons, which will now be outlined below.

###Functional

Firstly, the functional style is a fast, very simple and clear means of object creation. However it duplicates all of the methods for every single object created. This means a new function is stored in memory every time the object is generated.  If you are working with large numbers of objects, this may not be the best choice.



	var House = function(colour){
    var obj = {}; // generate object

    obj.colour = colour;
    obj.door = 'open'; //assign properties

    obj.openDoor = function(){
        obj.door = 'open';
    }; //explicitly define or borrow methods

    obj.closeDoor = function(){
        obj.door = 'close';
    }; //explicitly define or borrow methods

    return obj;
	}

	var house = House('red'); //instantiation pattern


###Functional-shared

Secondly there is functional-shared, a better approach for memory management as it utilises a single repository for the methods. Every time an object is created, pointers are generated, so every new object will point back to the houseMethods object, where the functions are stored one single time in memory.


	var House = function(colour){
    var obj = {}; // generate object

    obj.colour = colour;
    obj.door = 'open'; //assign properties

    obj.open = houseMethods.open;
    obj.close = houseMethods.close; //explicitly define or borrow methods

    return obj;
	};

	var houseMethods = {};

	houseMethods.openDoor = function(){
    this.door = 'open'; //Add method to delegate fallback object
	};

	houseMethods.closeDoor = function(){
    this.door = 'close'; //Add method to delegate fallback object

	};

	var house = House('red'); //instantiation pattern

`this` is not as efficient as using the properties of its prototype which is known as delegating through fallback.

###Prototypal

If a method or property is called on an object, where it does not exist for that object, JavaScript will check if it's defined on its fallback object. (See the README on [Inheritance and JavaScript](https://github.com/codingforeveryone/READMEs/blob/master/JavaScript/inheritance-and-javascript.md)).  Fallbacks are like a back-up plan for objects, they are the cornerstone of *prototypal* style. Every function has a property called `prototype` where the fallback methods are stored. So in the prototypal House function, the House function's property (`House.prototype`) is explicitly delegated as the fallback location for EVERY house object created by House. So every method in `House.prototype` is available to every object created by House. The advantage is that methods are not duplicated in memory. However, it utilises more code than the others.

	var House = function(colour){
    var obj = Object.create(House.prototype); // generate object

    obj.colour = colour;
    obj.door = 'open'; //assign properties

    return obj;
	};

	//(Automatically generated by interpreter)
	//House.prototype = {};

	House.prototype.openDoor = function(){
    this.door = 'open';
	};

	House.prototype.closeDoor = function(){
    this.door = 'close';
	};

	var house = House('red'); //instantiation pattern


###Pseudoclassical

Lastly we come to pseudoclassical. Instead of assigning the `Object.create(House.prototype)` to a new variable, it is assigned to `this` for the purpose of simple property assignment and method creation. The interpreter will do this automatically 'under the bonnet', and return the object, so long as the keyword `new` is initialised at instantiation `var house = new House('red')`. The disadvantage is that object creation is not that clear.

	var House = function(colour){
    //(Automatically generated by interpreter)
    //var this = Object.create(House.prototype);
    this.colour = colour;
    this.door = 'open';

    //(Automatically generated by interpreter)
    //return this;
	};

	//(Automatically generated by interpreter)
	//House.property = {}

	House.prototype.openDoor = function(){
    this.door = 'open';
	};

	House.prototype.closeDoor = function(){
    this.door = 'close';
	};

	var house = new House('red'); //instantiation pattern


Instantiation happens when a functional class is utilized to create a new object,

	var house = House('red')

Note that pseudoclassical is the only style that makes use of the `new` keyword, the other three classes are used like a function call.

###Summary

None of these four are better or worse than each other to use, they are just styles. It is up to you as the programmer to decide which your program would benefit the most from.

*Functional*

* Most transparent and easy to understand
 
* Higher memory cost because each instance retains its own properties

*Functional-shared*

* Offers memory cost advantage over functional

* A bit more complex than functional

*Prototypal*

* Shared properties in a separate object and not extended within the instatiated object

* Code is more complex to read

*Pseudoclassical*

* Often the most optimized pattern and very common

* The keyword *this* can be difficult to understand

###Related

[Objects](https://github.com/codingforeveryone/READMEs/blob/master/JavaScript/Objects.md)

[Inheritance and JavaScript](http://codingforeveryone.foundersandcoders.org/JavaScript/inheritance-and-javascript.html)

###References

[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript)

[Udacity Object-oriented JavaScript course](https://www.udacity.com/course/object-oriented-javascript--ud015)

[Instantiation patterns in JavaScript](http://callmenick.com/post/instantiation-patterns-in-javascript)

JavaScript: The Good Parts - Douglas Crockford Chapter 5