# Proxy Objects
https://hackernoon.com/introducing-javascript-es6-proxies-1327419ab413

There are 3 key terms we need to define before we proceed:

  * handler — the placeholder object which contains the trap(s)
  * traps — the method(s) that provide property access
  * target — object which the proxy virtualizes
  
  ```
  let handler = {
    get: function(target, name) {
            return name in target ?
                target[name] :
                'key does not exist';
         }
   };
   
   let o = {
      reason: 'reason',
      code: 'code'
  }
  
  let p = new Proxy(o, handler);
  
  console.log(p.reason)
  console.log(p.code)
  console.log(p.beer) // key does not exist
  ```
  here:
  ```get``` is the trap,
  ```o``` is the target,
  ```handler``` is the handler
  
