## Side Effects

When writing functions that are *pure*, we do not modify the inputs, or mutate data elsewhere. This is contrary to imperative and object oriented languages, where mutating state is crucial. So let's deconstruct each of the ideas of pure functions.

### Immutability

When declaring a variable in C, we would write the following:

```
int x;
```

This variable `x` is then declared, but not initialised. We could initialise in the same step, or do so later, but the point is that `x` is a *variable*. We can change its value whenever we want. In functional programming that is not the case.

If we wish to declare a variable named `x`, we *are required to* initialize it at that step, and it is not really a variable, because it is immutable, meaning that it cannot be changed.

```
let x = 5
```

Now `x` will be equal to 5 for the entire length of our program.

At first, it is natural to think this would make it difficult to have your program really do anything. This is no reason to fear functional programming. We will see very quickly why this isn't limiting in terms of our capabilities. So what are the benefits though? In general, it makes code easier to read and understand as when you look at a piece of code, you don't have to think through what the order of execution is and how it effects each variables value is at each point. This is also because we don't generally use loops, or complex branches in functional programming. Furthermore, we don't generally write if statements without their else case. An if statement is just a function.

### Side Effects

In an object oriented program it is common to create a class, of which many instances can be created, where they have data and methods associated with them. We can then have a method which is a *member of the instance* (non static) and modifies the internal data of that type.

This means we can have a method which takes no inputs and returns no outputs. In a functional programming language the idea of this is nonsensical. This is because we try to write our functions to have no side effects. This means that they will take an input, do some calculators, and return an output, without doing anything else.

Some examples of side effects are:
- Reading and writing to files
- Displaying text in the console
- Modifying data on the heap (follow by a pointer)
- Making a memory allocation

Some readers may now be thinking *"how on earth do you write a program to do something useful without side effects?* This is a very fair point. As we start out, because OCaml is a functional language but doesn't enforce its functional nature upon us, we will simply be very careful about where we use side effects. In languages like Haskell, we use things called monads to manage side effects. Designing programs around monads makes managing side effects very safe, and makes it easier to reason about the program as it becomes more complicated, with the up front cost of some extra thought in design. For now, there is no reason to worry about monads and the notion of managing side effects with them, as in OCaml we can do things like IO simply and easily. We will however cover them later on as a design pattern to improve our code. That said, mutating data is a side effect we will avoid at (almost) all cost.

It is also crucial at this point to explicitly mention that the whole idea of functional programming is to provide abstractions upon effectful code. Obviously our code has to allocate memory to do something useful, but when writing a functional program, we can simply think about data being passed around between our functions, without significant thinking dedicated to how the program handles it internally. This abstraction does come with disadvantages when it comes to performance analysis, and the ability to do systems programming, we will discuss these further later on.

All of this is about managing complexity. One of the biggest benefits of pure functions should be clear already. If I call a function, I know that its just doing calculations on the inputs and that's it. I don't have to worry about calling a function on data and not knowing if its going to modify that data I'm giving to it. This is especially important when using a library written by someone else, or returning to your own code you haven't looked at in a while, when you're less familiar with it.

The key idea here is that *pure functions make it easy to reason about complex algorithms, without tracking the state of your program*.