# Javascript-ES6

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