Why NodeJs was built?
At time when node js was not found there was a Apache Http server that was blocking so Ryan founder of Node wanted to build a non blocking HTTP server resulting Nodejs

NodeJS was build in 2009
NPM was build in 2010
NPM is a online registry in which people can contribute their own code 

Node Js internal working

- At First main Node Process is created on main thread and then Project is initialised
- Then all top level code is executed like console.log() and const x=10 etc...
- Top Level code --> Require modules --> Event callbacks register --> Starts Event loop
- if there is any CPU intensive task like FS module, Crypto, or compression then it is Offloaded to Thread Pool where multi threading comes into picture
- Else if other then Top level code is present then sequence of execution is like --
    
    - Expired Timer callbacks [ Set timeout( () => {} ) ]
    - IO Polling [ FS module like reading and Writing ]
    - Set-immediate callbacks
    - Close Callbacks
    - Condition ( is there any task remaining ? if Yes then restart event loop else exit );
    - Promises are handled in-between transition of Phases