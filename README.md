# Javascript-ES6 Notes

## The Arrow Function

**Simple example**
```
function foo() {
    return 2;
}
```
converts to
```
foo = () => 2;
```

**Arrow Function Variations**
```
=> 3 // Headless arrow function
() => 3
x => 3
(...x) => 3
(x,y) => 3 // With two parameters x and y
x => { try { 3; } catch(e) {} }
x => { return 3; }
x => ({ y:x }) // to return an object should use () `common brackets`
```
**Promise and this**
```
p.then( function(v) {return v.id} )

p.then( v => v.id ) // here if v is undefined, in stacktrace it will be shown as anonymous function
better to use named function
p.then( function extractId(v) {return v.id} )
```
```
var obj = {
    id: 42,
    foo: function foo() {
        var context = this; // bad coding as this is non this coding
        setTimeout(function(){
            console.log( context.id );
        },100);
    }
};

obj.foo();
```
```
var obj = {
    id: 42,
    foo: function foo() {
        setTimeout(() =>
            console.log( this.id ); // this has id as arrow fucntion doesnt have this keyword
        ,100);
    }
};

obj.foo();
```
> Comma operator can be used to combine expressions to avoid {} braces and use () parantheses.
> Be careful while using .bind(), .call() or .apply() as arrow function wont have its own this.

