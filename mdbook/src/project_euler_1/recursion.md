## Recursion

Writing a recursive approach to the problem, solves our problem of the lack of loops, and will inadvertantly remove the need for a mutable index and accumulator.

### Shadowing

Before talking about recursion though, we need to discuss shadowing. Shadowing in OCaml is just the fact that a variable of the same name can be declared twice, with the new definition shadowing, or overwriting the previous definition. For example, the following is perfectly valid OCaml code.

```
let number = 5

let number = 8
```

In fact, we can even use the previous definition of number in the new definition like this:

```
let number = 5

let number = number + 3
```

This may look suspiciously like mutability, but it's not. Why? Because if the definition is changed within a higher scope, then it will only change it within that scope. For example, after the following code has run, the value of number is still 5. The change in value of number only occurs in the higher scope of the `add_seven` function.

```
let number = 5

let add_seven x =
    let number = 7 in
    x + number
```

This means we can't change the definition within a loop and have it be changed in the next instance of the loop running.

In general, code like those in the above blocks becomes a bit subtle and obvuscated because of the shadowing, and hence shadowing is generally not advised. If no variables are shadowed, it becomes way easier to track the value of variables across a program, one of the great things about immutability.

### Recursion

So if shadowing is not a solution to our problem of immutability in this Project Euler problem, why mention it now? Well because what if you want to have a recursive function. Let's consider a factorial function defined recursively.

```
let fact n =
    if n = 0 then
        1
    else
        n * fact (n - 1)
```

In the above snippet, once the recursive call is met, the compiler can't find a definition for the `fact` function. This is because the top level declaration is not complete, so the compiler doesn't know it's full definition. So why isn't this a problem in other languages? Well because in OCaml we have shadowing. It's perfectly possible for there to be a previous definition of `fact` that is used in the call within fact.

So how do we write a recursive function, we just need to notify the compiler that the function will be recursive with the `rec` keyword. A correct factorial function would look like this:

```
let rec fact n =
    if n = 0 then
        1
    else
        n * fact (n - 1)
```