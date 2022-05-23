# Stacks

Another useful data structure is a stack. In most cases, a linked list can be used just like a stack by only prepending to the stack, and removing from the front. That said, the implementation of a specifically designed stack can be useful to distinguish the usage in code. The implementation of the standard stack functions also let's us put some features of OCaml in action that we haven't used much up until now.

### Structure and Type

A stack is a data structure where elements that are added first, are the last to be removed. This is just like if you were to stack dishes for example. When adding new dishes they should go at the top of the stack, and when removing them, they should also be removed from the top.

Let's first construct the type to use for our stack. This will be similar to our linked list.

```
type 'a stack =
    | Empty
    | Node of 'a * 'a stack
```

### Implementing Stack Functions

Any good stack implementation should implement the following functions:

```
(* Returns the first element of the stack *)
peek: 'a stack -> 'a option

(* Removes the top element of a stack, returning that's elements value and a new stack  *)
pop: 'a stack -> ('a option * 'a stack)

(* Adds an element to a stack, returning the new stack *)
push: 'a -> 'a stack -> 'a stack
```

So let's implement them in order.

For peeking the top element of a stack, we just need to use pattern matching to deconstruct it and return the top element.

```
let peek (st : 'a stack) : 'a stack =
    match st with
    | Empty ->
        None
    | Node (x, xs) ->
        Some x
```

Notice here that because we may attempt to peek an empty stack, we need to handle this case in some way. In many imperative or object oriented language we could return `null`, or fail the program there, treating it like an index out of bounds error in an array. Ultimately though, the programmer using our implementation should be forced to handle this case, and should be allowed to handle this case without an exception. This is the perfect case to use `option` type, since it's possible no data will be returned.

Now, `pop` is a similar function in the sense that it returns the top element. So let's use that as a basis for our implementation. The only difference now is we also need to return the new stack, comprised of the remaining elements. A tuple is the perfect data type for this.

```
let pop (st : 'a stack) : ('a option * 'a stack) =
    match st with
    | Empty ->
        (None, Empty)
    | Node (x, xs) ->
        (Some x, xs)
```

Finally, let's implement the push function. To push to the top of the stack, we need a new stack which "points to" the existing stack. This is just like prepending to a list.

```
let push (new : 'a) (st : 'a stack) : ('a stack) =
    Node (new, st)
```

Fantastic, we new have an implementation of a stack data structure to peek, pop and push all with time complexity of ***O(1)***, just as we would expect.