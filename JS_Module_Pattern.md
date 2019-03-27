# Module Pattern

Hide object properties using closure

```JS
  function createPerson(){
    var firstName = 'Burt'
    var lastName = 'Maclin'
    
    var obj = {
      getFirstName: function(){
        return firstName
       },
      getLastName: function(){
        return lastName
       }
     }
     
     return obj
  }
  
 var p1 = createPerson()
 // console.log(p1.firstName) wont work
 console.log(p1.getFirstName()) // works as obj.getFirstName() remembers scope of firstName because of closure
  ```
   
