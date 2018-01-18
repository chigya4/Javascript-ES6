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

**let vs var**
```
function foo(x,y) {
    var z = x * 2;

    if (x > y) {
        let tmp = x;
        x = y;
        y = tmp;
    }

    for(let i=0; i<10; i++) {
        // ..
    }
}
```
> In the above code z can be var as it tells z can be used anywhere.
> Use let if the variable is to be used in the block.

> Its not let is the new var, its let and var.

```
try {
    let z = bar ( x * 2 )
} catch (err) {
    // ..
}
```
> An error may be z is undefined even though it is declared.
> Can be fixed by declaring it before try block or using var.

> Make variable declaration to use as close as possible when used the first time.

```
for (let i = 0; i<10; i++) {
    // ..
}
```
> Here the let i variable gets intialized 10 times each i with incremented value.

**const**
> A variable that cannot be reassigned
```
const x = [1,2,3];
x[0] = 42; // x = [42, 2, 3]
```
> To make x immutable use Object.freeze([1,2,3]) along with this var could be used.

> Use const only for constant, const PI = 3.14

**Default values**

***imperative way***
```
function foo(x) {
    x = x !== undefined ? x : 10;
}

foo(); // 10
foo(0); // 0 .. WTH!!
```

***declaraive way***
```
function foo(x = 10) {
    console.log(x);
}
foo(); // 10
foo(0); // 0 .. WTH!!
foo(undefined); // 10
foo.apply(null, []); // 10
foo.apply(null, [,]); // 10
```

**Lazy Expression**
```
var x = 1;

function foo( x = 2, f = function() { return x; } ){
 	var x = 5;	
  	console.log( f() );
}
foo(); // 2 How?? Closure??
```
