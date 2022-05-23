## If Expressions

To solve our problem we're also going to need to have branching paths for our code. To do this we are going to use an *if expression*. This is distinct from an *if statement* in a key way. Let's consider some simple code to construct a branching path on if a value is less than 0, 0, or greater than 0.

```
if number > 0 then
    (* do something *)
else if number < 0 then
    (* do something *)
else
    (* do something *)
```

This should look familiar to other programming languages, but there is something importantly different. In OCaml an if expression is a function, and thus returns a value which we can assign to another variable. Imagine each of the blocks of code within a series of cases like this as like a function, which has to return something at the end. These three cases then construct an overall function which returns the value from whichever case is run. So let's rewrite the above but fill out the cases by returning a string describing the number, and storing that as a variable.

```
let description =
    if number > 0 then
        "greater than 0"
    else if number < 0 then
        "less than 0"
    else
        "equal to 0"
```

There are a few implications of this fact about if statements. One of which is that because the overall expression makes up a function, all branches must return a value of the same type (string in this case). The following code is not valid:

```
let result = 
    if condition then
        0
    else
        "0"
```

It also means we have to have an else case, for example the following code is also not valid:

```
let result =
    if condition then
        0
```

Because what happens if the condition is not true? Well then we have no value to assign to result, and we have already established that in OCaml we don't have nulls, nor do we really want them.

What if however you want to do some IO or something in some circumstances, but not return anything meaningful? We will cover how to do that at the end of this chapter, for now just remember if statements are functions, and each branch returns the same type.

What if you know that all cases are covered by if else? The compiler will still force an else case, and while it is easy to just change the last case into an else case if you are confident that you have exhaustive conditions, we will see in a later section a way that the compiler can check for exhaustion of cases under many circumstances.

### Logical Operators and Booleans

The header of an if statement takes in a boolean type, which, as you'd expect, is either `true` or `false`.

There are some logical operators for working with booleans.

- Logical And (`&&`)
- Logical Or (`||`)

And a logical function:

- Logical Negation (`not`)

We will see some of these in action as we solve our initial problem.

### Polymorphic Comparison

OCaml also has polymorphic comparison operators. We will cover the strangeness of these operators in detail later on when talking about polymorphism more generally, but for now just know that we have the following functions available and they work for *any type*, including custom types.

- Equal (`=`)
- Not equal (`<>`)
- Less than (`<`)
- Greater than (`>`)
- Less than or equal (`<=`)
- Greater than or equal (`>=`)

Note that equality in OCaml here checks *structural equality*. We can give the OCaml compiler an instance of a complex type and it will check that every aspect of it is equal. This is unlike many languages which for data on the heap, just check that the pointers are the same, and this difference makes OCaml's equality check incredibly handy, but comes with some disadvantages when it comes to the other checks.

This is because the other operators work for every type as well, even if it doesn't give a sensible answer. For example the following lines of code all give an answer that is `true` or `false`.

```
true < false
"hello" >= "goodbye"
[1; 3; 5] > [0; 3] (* lists, to be discussed in more detail later *)
```

Moreover what answer is given is not obvious, and can't really be known without looking at the specific implementation of these operators.

Hence, in general, while equal and not equal can be used for any type, the other comparison operators should be used for numerical types only.
