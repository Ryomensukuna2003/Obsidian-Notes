- JavaScript is synchronous single threaded language 
- Everything happens inside Execution context
- Think Execution context as a big box with 2 components in which one is Memory component (Variable environment) and there's the code part where code is executed one at a time also know as  Thread of execution

When JS code is ran it creates a Execution Context and happens in 2 phases
First phase : memory allocation
	JS skims through code for memory allocation
	All variables are given a value of **undefined** and for functions JS stores **entire function** in the memory space

Second Phase : Code Execution
	In second phase JS executes code one by one as it is synchronous
	For variables its values are assigned and when a function is called then the function behaves like a mini JS program and creates a whole new execution context

Remember the all new execution context being made when any function is called... it goes into something known as call stack and at bottom of call stack there's a **Global execution context**  

#### Undefined VS Not Defined

- Undefined in JS means that JS don't know if it is defined in future or not like he's not sure and JS just assigns undefined to variables in hoisting part
- Not Defined in JS means that JS is sure that variable is left unchecked and has no value which gives a reference error

#### The Scope Chain, Scope & Lexical Environment
1. Scope of a variable is directly dependent on the lexical environment. 
2. Whenever an execution context is created, a lexical environment is created. Lexical environment is the local memory along with the lexical environment of its parent. Lexical as a term means in hierarchy or in sequence. 
3. Having the reference of parent's lexical environment means, the child or the local function can access all the variables and functions defined in the memory space of its lexical parent. 
4. The JS engine first searches for a variable in the current local memory space, if its not found here it searches for the variable in the lexical environment of its parent, and if its still not found, then it searches that variable in the subsequent lexical environments, and the sequence goes on until the variable is found in some lexical environment or the lexical environment becomes NULL. 
5. The mechanism of searching variables in the subsequent lexical environments is known as Scope Chain. If a variable is not found anywhere, then we say that the variable is not present in the scope chain.

#### Hoisting in JS
  
1. Hoisting: Mechanism in JS where the variable declarations are moved to the top of the scope before execution. Therefore it is possible to call a function before initializing it.
2. Variable and Arrow functions (because they act as variables) declarations are scanned and are made **undefined**
3. Function declarations are scanned and get copied as they are and made available

There are two key benefits of Hoisting:

1. **Allow you to use function declarations before they are defined**. This allows you to organize where you put your function code regardless of where they are called as long as they are within the same scope.
2. **Improve the performance of the interpreter**. Because variable declarations are moved to the top of their scope during compilation this may reduce the time the JavaScript engine needs to parse the code.
##### Special case for let and const in Hoisting
let and const are hoisted but they are hoisted in Temporal dead zone and Temporal dead zone only occurs until code execution is not started. As soon as execution of code starts let and const variables gives reference error if not handled properly.



#### Var, Let, Const

1. The keyword 'Var' has a function scope. Anywhere in the function, the variable specified using var is accessible but in ‘let’ the scope of a variable declared with the 'let' keyword is limited to the block in which it is declared. Let's start with a Block Scope
2. **Shadowing in Var** - Here shadowing is referenced as overriding as value of var will be last assigned value of any scope
```js
var a=100;
{
	var a=1000;
}
console.log(a); // <- 1000
```
1. let and const are hoisted but not initialized. Referencing the variable in the block before the variable declaration results in a ReferenceError because the variable is in a "temporal dead zone" from the start of the block until the declaration is processed. Also let and const are not available in window or this object. they have their separate storage  

#### Block Vs Scope

1. Block is a compound statement and used to combine multiple statement
```js
	{
		const h=10;  // <- statement 1
		console.log("Helo"); // <- statement 2
	}
```
2. Everything we can access in block is known as scope of the block. 
	1. Why this statement? isn't this obvious?
		No, as let and const has scope only of the block, Var has global scope and can be accessed anywhere in the code


#### Closures

A function bundled with its lexical environment is known as closures.
```js
function temp(){
	var a=7;             // --
	function inner(){    //  |
		console.log(a);  //  |---> entire of this is closure (function with its lexical enviornment)
	}                    //  |
	return inner;        // --
}
var newFunction=temp;  // <- Here newFunction will hold inner function along with its lexical envirenment
z(); // <- This will print a all thanks to closures
```

Why this bitch ass Closure?
- Used in **once functions** (function that only runs once) as we can store in closure that how many time function has been called
- setTimeouts 
- many more idk...

#### Closures with setTimeout

```js
function x(){
	for(var i=1;i<=5;i++){
		setTimeout((function temp(){
			console.log(i);          // <- Here this should print 1,2,3,4,5 at intervals of i
		}),i*1000);
	}
}
```

 - Here many of us would think that it should behave normally but how setTimeouts works internally is that it stores entire function with its closure that is inside setTimeout in different memory space and attaches a timer to it and once timer expires it pushes entire function into call stack to execute.
 - Now the catch here is for loop is synchronous and will execute without waiting for setTimeout and var i will be incremented to 6 and as setTimeout is refereeing to same memory space it will print 6.
 - So what's the fix here? Use let instead of var as it is block scoped and will create a new variable when loop is incremented
 - Well done but its not over my nigga. Don't use let use var.
 ```js
 function x(){
	for(var i=1;i<=5;i++){
		function close(x){
			setTimeout((function temp(){
				console.log(x);
			}),x*1000);
		}
		close(i); // This will create a closure and closure will make copy every time loop is called hence giving new memory space to setTimeout
	}
}
```

#### Data encapsulation with Closures

```js
function counter(){
	var counter=0;
	return function incrementCounter(){
		counter++;
		console.log(counter);
	}
}

var counter1=counter(); // Here counter will only return incrementCounter() function with its variable as closure and we can't access variables inside closure hence encapsulating variable and only giving access to its function 
counter1();
var counter2=counter(); // This will create a new counter 
```

#### Function statement vs Function expression
```js
// Function statement
function a(){
	console.log("a called");
}

// Function expression 
let b= function(){
	console.log("b called"); 
}

// So what's the diffrence here?
// Diffrence is in Hoisting like function expression acts a variable so b will throw an error of undefined and a will be executed as while function is copied in-memory
```

#### Anonymous Functions
```js
// Anonymous function
function (){

}
// Above statement will result in syntax error and Anonymous functions are used when assigning fucntion to a variable also known as function expression
```

#### Named Function Expression
```js
let x=function xyz(){

}
// Works fine until you call xyz() instead of x(); { Refrence error }
```

#### First class functions/citizens?
```js
// Ability to pass functions inside of function as argument is known as first class functions
let b=function(param1)){
	return function(){
		console.log("Returned a function")
	}
}
function text(){
}

b(test());

```

#### Callbacks
A callback function is a function passed on as a argument inside a function intended to be executed after completion of some event
```js
function performOperation(data, callback) {
  // Simulate an asynchronous operation
  setTimeout(() => {
    const processedData = data.toUpperCase();
    callback(processedData); // Execute the callback with the result
  }, 1000);
}

function displayResult(result) {
  console.log("Processed data:", result);
}

performOperation("hello world", displayResult); // Pass displayResult as the callback
```

Okay so create a JS program that prints count of button clicked
```js
let count=0;
document.getElementById("clickMe").addEventListener("click",function(){
	console.log(count++);
})
// Common man you know that we should not use any global variables as they can be easily modified by another program
// Oky then let me use diffrent approach
function attachEventListener(){
	let count=0;
	document.getElementById("clickMe").addEventListener("click",function(){
		console.log(count++);
	})
}
attachEventListener();
// Here we are using callbacks and closure property to hide count variable and do a count++;
```


#### Event Loop and Queues

- We know that for async tasks we have separate memory in Browser and when they finish their work it gets pushed into callback queue and there's a event loop that continuously listen to callback queue so that it can enter into call stack and get executed. Set-timeout and addEventListeners works the same way.
- Now comes fetch() into picture. Instead of pushed into callback queue it gets pushed into `MicroTask queue` that has higher priority then callback queues
- All callbacks that comes from `promises` and `Mutation Observer` goes into `MicroTask queue` 


#### Interpreter vs Compiler

- Interpreter executes code line by line and it dose not know what will come next
- Compiler on the other hand compiles whole code, optimise it and then run the code with improved performance

So what dose JS use?
It uses Just in time compiler that uses best of both the worlds for speed and efficiency


#### SetTimeoutes

It dose not guarantee that callback will run after given time as it will only get chance to run when main thread is empty.


#### Higher-order functions

Higher-order functions are functions that operate on other functions like map and filter
What if we want to make our own Higher order function?
	We can use Array.Prototype.${array_name};

```js
Array.Prototype.calculate=function(logic){
	let output=[];
	for(let i=0;i<this.length;i++){
		output.push(logic(this[i]));
	}
	return output;
}

console.log(radius.calculate(area))
```

**Common known Higher order functions are Map, reduce, filter**

- **map**: This function transforms each element in an array by applying a given function to it and returns a new array containing the results
- **filter**: This function creates a new array containing only the elements from the original array that satisfy a given condition
- **reduce**: This function reduces an array of values down to a single value by applying a reducer function to each element. It iterates through the array, accumulating a single result from the elements. It typically takes an accumulator and the current value as arguments in its callback function, and often an initial value for the accumulator.
```js
    const numbers = [1, 2, 3];
    const doubledNumbers = numbers.map(num => num * 2); // [2, 4, 6]
    
    const numbers = [1, 2, 3, 4, 5];
    const evenNumbers = numbers.filter(num => num % 2 === 0); // [2, 4]
    
    const numbers = [1, 2, 3, 4];
    const sum = numbers.reduce((accumulator, currentValue) => accumulator + currentValue, 0); // 10
```

We can also do chaining of these Higher order functions
```js
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Example of chaining:
// 1. Filter out odd numbers
// 2. Double the remaining (even) numbers
// 3. Sum the doubled even numbers

const result = numbers
  .filter((num) => num % 2 === 0)
  .map((num) => num * 2)
  .reduce((sum, num) => sum + num, 0);

```

##### Reduce Example

```js
const people = [
  { name: 'Alice', age: 30 },
  { name: 'Bob', age: 25 },
  { name: 'Alice', age: 35 },
  { name: 'Charlie', age: 28 },
  { name: 'Bob', age: 40 },
];

const nameFrequencies = people.reduce((accumulator, currentPerson) => {
  const name = currentPerson.name;
  if (accumulator[name]) {
    // If the name already exists in the accumulator, increment its count
    accumulator[name]++;
  } else {
    // If the name doesn't exist, initialize its count to 1
    accumulator[name] = 1;
  }
  return accumulator; // Return the updated accumulator for the next iteration
}, {}); // Start with an empty object as the initial accumulator

```

#### Problems with Callbacks

Callbacks are cool and all and async js exists because of callbacks but there are 2 main reasons for these callbacks
- Callback Hell [code grows horizontally instead of vertically]
- Inversion of control [We lose control of code as we pass it down to another function call]


#### Promises

Promises are object that represents eventual completion or failure of async operation and its resulting value
```js
let myPromise= new Promise((resolve,reject)=>{
	if(true){
		resolve("Always true");
	}
	else {
		reject("Won't happen");
	}
})

myPromise
	.then((message)=>{
		console.log(message);
	})
	.catch((error)=>{
		console.log(error);
	})
```

So what's the difference here???
In this code we are not giving control to another function and callback is only ran after resolve or rejection of promise

```js
// Promise Chaining
myPromise
	.then((response)=>{
		return doSomething();
	})
	.catch((error)=>{
		console.log(error);
	})
	.then(()=>{
		return doAgain();
	});

// Here catch statement will only catch error of above then's and below that catch will still be executed
```

#### Promise API's

- **Promise.all()**
	- Takes array of promises and returns array of promises with data.
	- Time taken to resolve this promise will be max of all Promises.
	- If any of promise fails it will throw an error immediately.  
- **Promise.settled()**
	- Same as all but if an promise fails then it will still return Array of promises with error at place of those API calls that failed
- **Promise.race()**
	- The first one to settle will be returned
	- If first one fails then it will return error
- **Promise.any()**
	- First one to success will be returned
	- If all fails then it will give a error of all errors



#### async/await

Syntactic sugar over promises, making asynchronous code appear more synchronous
Error handling is done with `try...catch` blocks.
#### Promises vs async/await

async/await will wait promise to resolve and Promise will not
```js
const p=new Promise((resolve,reject)=>{
	setTimeout(()=>{
		resolve("Resolved after 5sec");
	},5000)
})

async function handlePromise(){
	const val=await p;
	console.log(p);
}
handlePromise();

function getData(){
	p.then((res)=>{
		console.log(res);
	})
	console.log("helo");
}

getData();
```


#### How dose fetch() works?

When we fetch("API_URL") then it gives a promise of Response which is a readable stream and to read that readable stream we have to use `res.json()` which itself is a promise and we have to use .then() method to fulfil that promise.


Immediately Invoked Function in JavaScript?

```js
(function x(){
  console.log("YES");
})(); //  <-- Immediately Invoked Function for less garbage in global context and clean code

```
