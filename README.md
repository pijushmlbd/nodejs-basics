# nodejs-basics
# What is node.js
##### From official website:
> Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.

Lets understand the definition. 
Javascript runtime-Javascript creators designed and created the language to achieve dynamic functionality in browsers but they need something to execute the code. This something which executes the code is called **javascript runtime**. They created the javascript engine . Whenever a browser sees javascript content ,it passes it to the javascript engine and the engine executes it.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![Markdown Logo](https://github.com/pijushmlbd/nodejs-basics/blob/main/Screenshot%202022-04-11%20at%201.11.56%20AM.png)


Different browsers have different javascript  engines and implementations are different. And the javascript engine inside the chrome browser is called **V8 engine.**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![Markdown Logo](https://github.com/pijushmlbd/nodejs-basics/blob/main/Screenshot%202022-04-11%20at%201.12.05%20AM.png)

But this runtime could only work with DOM context and manipulates only  the browser’s element.  From this point, 
What if we take the V8 engine out of the browser and use it for something more than manipulating DOM elements like file reading, calling network api,access system resources etc similar to other languages. 


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![Markdown Logo](https://github.com/pijushmlbd/nodejs-basics/blob/main/Screenshot%202022-04-11%20at%201.12.17%20AM.png)


### Node REPL
REPL is a  command line tool where we can try out node.js expressions. 
Start the prompt by using command prompt and type node. 

### When to use node.js : EventLoop
 Node.js shines in the following kind of operations 
 <!-- UL -->
           . Non-blocking
           . Event-driven
           . Data-intensive
           . I/O intensive

 To understand why it shines in these areas, it is important to understand one aspect of node.js is that
>                  Node.js is single threaded.!!


That is there is only one application thread. The single processing thread just executes each of every event one by one . No two things are happening in parallel. If we pass something to execute node.js it will add behind the event queue. 


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![Markdown Logo](https://github.com/pijushmlbd/nodejs-basics/blob/main/Screenshot%202022-04-11%20at%201.12.29%20AM.png)


Event loop is basically a loop of execution that picks up an event from the event queue and executes it. 

But what if the event is time consuming  like I/O intensive, network call ? How is this handled in a single thread?does the thread wait for the execution to finish? No. because 
 >          Javascript is asynchronous 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![Markdown Logo](https://github.com/pijushmlbd/nodejs-basics/blob/main/Screenshot%202022-04-11%20at%201.29.53%20AM.png)

The node.js api  does not wait for the response . rather it will deal with response later. When it gets the response , it is added to the back of the event queue. Node.js does this using something called **callbacks** . 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![Markdown Logo](https://miro.medium.com/max/1400/1*iHhUyO4DliDwa6x_cO5E3A.gif)

The processing of functions continues until the stack is once empty. Then, the event loop will process the next event in the queue (if there is one).

Due to this event loop model node.js shines in non-blocking, event-driven, data-intensive,i/o intensive operations.

In a multi-threaded model , there would have been threads for each request and the thread could wait for something to complete. So more resource intensive. 

## When not to use node.js
 <!-- UL -->
   . Data calculation
   . Blocking operation

But what if the operation is time consuming which does not depend on external api, rather the application thread needs to calculate it by itself. Such as finding the nth prime etc. In this case the node.js can not move on and execute the next event because the thread itself needs to complete the work.

## Node Modules

Node modules provide a way to reuse code in your Node application . Every javascript file in node.js can be considered as  a module itself. 

add.js
```javascript
  function add(num1, num2) {
    return num1 + num2;
  }
  console.log(‘add function file’);
```

main.js
```javascript
  console.log(‘Hello’);
  //use the add function
```

Let’s say we want to use the add function of the add.js file  in the main.js file. We can do that using a built-in function named **require** . It will execute the file where it has been called.

main.js
```javascript
  require(‘./add.js’)
  console.log(‘Hello’);
  //use the add function
    add(1,2);
```
But the add function is not called? Because

           `Every node module is encapsulated by default.`

Any function,variable do not get passed over by simply calling require function by default. So,we need to export them to tell node.js that we want to use it in other files. We can do that by using 
> 'module.exports'

Anything we want to take out from the encapsulated module and access it, need to mention it.
>                                 'dodule.exports =add;'

add.js

```javascript
  function add(num1, num2) {
    return num1 + num2;
  }
  console.log(‘add function file’);
  module.exports=add. 
```

main.js
```javascript
  var addFn=require(./add.js); // var {add }
  console.log(‘Hello’);
  //use the add function
   addFn(10,20);
```

If multiple exports needed, 

```javascript
   module.exports ={
      add,
      subtract 
}
```

Similarly we can use any builtin node module <!-- Links -->https://nodejs.org/api/index.html by require function


## Typescript

 <!-- UL -->
 -Very handy to use in a framework written in Javascript 
 <!-- UL -->
 -created by Microsoft.

### Why Typescript?

Javascript has evolved  over the years . Some of the problems while developing with javascript. 

 <!-- UL -->
- No type checks and no enforcement in typing 
```javascript
   var a ;
   a=10;
   a=’Hello’; 
```
All works fine. No way can we restrict a variable or object to contain only one type. Javascript only complains in runtime when something should have been a particular type , say a number , not very helpful in developing .

- Problems related to function arguments

```javascript
  Function add(a,b){
             Return a+b;
         }
     add (1,2)// 3 
     add (1) //undefined
     add (1,4,3) //7 , 3 is ignored!!
```
        
    
 Works in all cases.Still Javascript does not complain.

- Object related problems

```javascript
  var Person ={
firstName: ‘John ’
lastName : ‘David’
 }
 
 
person.foo=10;
console.log(person);

var person={
    firstName : ‘John’,
    lastName : ‘David’,
    foo : 10 //!!!
}
```
No way to prevent a developer from doing that. Loosely structured object. 


> But Browsers support only javascript !!

What if : 

![image](https://user-images.githubusercontent.com/17767188/162637211-49492bc3-f56d-41b9-96f0-db8baf793b98.png)

Typescript comes as the solution:


![image](https://github.com/pijushmlbd/nodejs-basics/blob/main/Screenshot%202022-04-11%20at%2012.04.30%20AM.png)


Types:

```javascript
var a : number;
var b : string;
var str : number[ ];
var mstr: [number,boolean];

a=10;
a=true; //show errors
mstr={10,true}
```





