# Javascript-ES6 Notes

## 1. The Arrow Function

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
foo(0); // 0
```

***declarative way***
```
function foo(x = 10) {
    console.log(x);
}
foo(); // 10
foo(0); // 0
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

**Gather and Spread Operators**
```
function foo( ...args ) {
    bar(10, ...args); // args.unshift(10)
}
```

```
function foo(x, y, ...rest){
    return rest;
}
var a = [1, 2, 3];
var b = [4, 5, 6];

foo(...a, ...b); // x = 1, y = 2 .... 
```

**Babel**

[Babel](https://babeljs.io/)

**Array Destructuring**
```
function foo() {
    return [1, 2, 3];
}

var [a, b, c] = foo();
```
> LHS and RHS need not be of same size, can be more or less

```
Defualt values

function foo() {
    return null;
}

var [
    a,
    b = 10,
    c
] = foo() || []; // expect array type return
```

Dumping variables
```
swaping made easy
var x = 10, y = 20;
[x,y] = [y,x]
```

```
function foo() {
    return [1, 2, 3, [4, 5, 6] ];
}

var a,b,c,args;

[
    a,
    b = 42,
    c,
    ...args
] = foo() || []; //args will have [ [4, 5, 6] ]
```

nested array destructuring
```
function foo() {
    return [1, 2, 3, [4, 5, 6] ];
}

var a,b,c,d,e;

[
    a,
    b = 42,
    c,
    [
        d,
        ,
        e
    ]
] = foo() || []; 
```
> or
```
function foo() {
    return [1, 2, 3, [4, 5, 6] ];
}

//var a,b,c,d,e;

var [
    a,
    b = 42,
    c,
    [
        d,
        ,
        e
    ]
] = foo() || []; 
```
> This freaked me out
```
function foo() {
    return [1, 2, 3, [4, 5, 6] ];
}

var a,b;

var x = [a,b] = foo();

x; // [1, 2, 3, [4, 5, 6] ]
```
Multiple destructuring
```
var a,b,vals,c,d;

[
    ,,,
    [c, d]
] =

[a, b, ...vals] =

foo()
```

Object destructuring
```
function foo() {
    return { a:1, b:2, c:3 };
}

var obj;

var {
    a = 10,
    b: X = 42,
    c,
    d
} = obj = foo() || {}; // a=1, c=3, X=2
```

Nested Object destructuring
```
function foo() {
    return { a:1, b:2, c:3,
        d: {
            e: 4
        } 
    };
}

var {
    a = 10,
    b: X = 42,
    c,
    d: {      // can have multiple d properties d:X,d:Y..
        e
    } = {}
} = foo() || {}; // e=4
```
> left-right-left processing - { a: {} = 2 } = ..

> If declaring variables should wrap object in ( )

>> 1[0] is legal because in JS does boxing (something to do with wraping in object)

Default values
```
function foo( [a,b,c] = []) {
    console.log(a, b, c);
}

foo( [1,2,3] );
```

```
function foo( {a,b,c} = {}) {
    console.log(a, b, c);
}

foo( {
    a:1,
    b:2,
    c:3
} );
```