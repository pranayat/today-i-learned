# clsures in async callbacks
```JS
var i
var print = function(){
  console.log(i)
}

for(i = 0;i<10;i++){
  setTimeout(print,0)
  console.log("hi")
}
console.log('end')
```
outputs 
```
hi X10
end
10 X10
```

* print is pushed to the timers queue in everyloop, when the queue is popped after executing other code
it executes the print() function and looks for the value of i
* because of closure every instance of 'print' in the queue has saved the referenfec to 'i' which is in the global
scope
* at the end of the loop the value of this global variable i is 10

# use functions to create multipe copies of variable i

* everytime you wrap a variable declaration in a function, at every execution of that function it will be created anew
* let this variable declaration be of variable 'j'
* let the wrapper function be an IIFE

```JS
var i

for(i = 0;i<10;i++){
  (function(){
    var j = i
    setTimeout(function(){
                  console.log(j)
                },1000)
  })()
}
```
