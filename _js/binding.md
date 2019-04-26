---
title: 'JavaScript binding'
---

JavaScript variables are untyped, so you can assign and re-assign values of any type.

Assignment of a *primitive* value copies the value; assignment of an *object* type copies the reference.

Variables are declared with `var`, or more recently, with `let` and `const`. `var` has function scope, or else global scope; `let` and `const` have block scope only.

In non-strict mode, assigning a value to an undeclared variable in the global scope --- e.g., `x = 'foo'`, where no variable `x` has been declared with `var`, `let`, or `const` in the global scope --- **creates a variable in the global scope.** This is easy to do accidentally. If you do it deliberately, you have created a **property of the global object.** (Unlike a declared variable, it can be deleted using `delete`.)

## Shadowing and the scope chain

Inside the local scope of a function, declaring a variable with the same name as a variable in the enclosing scope hides the variable in the enclosing scope:

```js
> var x = 1
> function f() { var x = 2; return x; } // hides global variable `x`
> f() 
2
```

Evaluation follows the *scope chain* up to the global scope. Understanding this is essential to using `with` and function closures.

## Hoisting

Variables declared in the local scope of a function are *hoisted* to the top of a function, **where they are visible before being bound to their values**:

```js
> var x = 1
> function f() {
... console.log(x); // global `x` hidden; local `x` visible, unbound
... var x = 2;
... console.log(x); // local `x` is now bound
... }
> f()
undefined
2
```

The hoisted equivalent of

```js
function f() {
    console.log(x);
    var x = 2;
    console.log(x);
}
```

is thus:

```js
function f() {
    var x;
    console.log(x);
    x = 2;
    console.log(x);
}
```

## References

Flanagan, David. *Javascript: The Definitive Guide.* 6th ed, O'Reilly, 2011.
