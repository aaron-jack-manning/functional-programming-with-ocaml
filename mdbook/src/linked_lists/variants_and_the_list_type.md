## Variants and the List Type

### Variants and Pattern Matching

Now let's create our linked list. The first thing we need is some custom data type that represents the nodes of the list. For this we will use a variant.

A variant is a custom type which contains a finite number of distinct cases. We can use variants to create types to represent data that enums would often be used for in other languages. Here are some simple examples of variants.

```
type boolean =
    | True
    | False
```

```
type day = Monday | Tuesday | Wednesday | Thursday | Friday | Saturday | Sunday
```

When declaring a custom type, we need to use the `type` keyword. That type is then given a name, which, by convension, we use *snake_case* for. That said, the fact that the types start with a lowercase letter and the names of the cases start with an uppercase letter is enforced by the language.

For a variant type, each case is separated by a `|`, and a `|` before the first case is optional. When we create an instance of each of these types, it will be of one of the cases. This is because *each case is a constructor for the type*. Hence to create an instance of the `day` type from above, we can write the following code:

```
let first_day = Monday
```

If we want to check if a variable of type `day` is a specific case, we can just check for equality with `=` and use an if statement.

There is however a better way of determining the case of a variant, called pattern matching. Pattern matching will be covered more thoroughly in it's own chapter, as it is a very powerful tool available to OCaml programmers. For now though, here is how we can write code for each case in the above variant using a pattern match:

```
let string_of_day (input_day : day) : string =
    match input_day with
    | Monday -> "monday"
    | Tuesday -> "tuesday"
    | Wednesday -> "wednesday"
    | Thursday -> "thursday"
    | Friday -> "friday"
    | Saturday -> "saturday"
    | Sunday -> "sunday"
```

The statement `match <variable> with` is followed by a series of possible cases that the variable may satisfy. Some code follows the arrow which will run if that is the case that is satisfied. All cases in a pattern match must return the same value, just like with an if and an else.

Variants can also have some data of any type associated with them. For example:

```
type colour =
    | Red
    | Green
    | Blue
    | Cyan
    | Magenta
    | Yellow
    | Custom of int * int * int
```

This variant represents a colour, as one of a series of different predefined colours. However, it also has a case which allows a custom colour specified by 3 integers as a tuple (benig the RGB values). When constructing an instance of this `colour` type, we can construct any of the first 6 cases as before, or we can provide a tuple of 3 integers when constructing a `colour` using the `Custom` case. This is because `Custom` is really a function with the signature:

```
Custom : (int * int * int) -> colour
```

So if we want to represent white we can do so with the following code:

```
let white = Custom (255, 255, 255)
```

So how do we pattern match on this? Well we just need to put a variable in place of the tuple here, which will be bound to the value within the `Custom` case. See below:

```
match white with
| Red -> (* do something *)
| Green -> (* do something *)
| Blue -> (* do something *)
| Cyan -> (* do something *)
| Magenta -> (* do something *)
| Yellow -> (* do something *)
| Custom x -> (* do something with x, which is a variable of type int * int * int *)
```

As we will see more of later on, pattern matching is even more powerful, in that we can deconstruct the tuple with the pattern match itself, by changing the bottom line to:

```
| Custom (r, g, b) -> (* do something with the variables r, g and b *)
```

We'll look more at pattern matching later with more complex data structures and using wildcards, but the above technique will be sufficient for the code we're about to write to construct and manipulate a linked list.

### List Type

In the case of the type of our list, we are going to use a variant which is recursive. One case for our list is when it is empty, and the other when there is at least one node.

```
type 'a linked_list =
    | Nil
    | Cons of 'a * 'a linked_list
```

The names `Nil` and `Cons` are very commonly used in functional programming with a linked list. If you come from a background in C, consider the `Nil` case as being the end of the list, where the pointer to the next node would be `null`. The Cons case is for all other nodes.

Note also the use of `'a` before the name of our type. This is because our type has a *type parameter*. Every time `'a` appears in our type definition, it is the same as the type parameter. This allows us to write code that works for lists of any type, since an `int linked_list` and `float linked_list` are distinct types that are both linked lists but parameterised on different types.

Now notice the type of the associated data: `'a * 'a linked_list`. Here `'a` is the data stored on the given node of the list, and `'a linked_list` is another instance of our linked list type, meaning that here we have a *recursive type*. In many other languages this would not be possible, but in OCaml the layer of abstraction just like a pointer in C, or `Box<>` in Rust is built in.

Now that we have our list structure, to better explain how it works let's create a list of the data [7; 9].

Our list has a starting node of the case `Cons` with data 7 and a linked list following it which can be represented by [9]. We then construct that list in the same way. This leaves us with a linked list of:

```
let list =
    Cons(7, Cons(9, Nil))
```
