## Fold

Perhaps the most important function for operating on a linked list is called `fold`. One of the main reasons for its importance is the fact that many other list functions, including `map` and `filter` which we already implemented, can be written in terms of `fold`.

The idea of fold is that we have a function which takes as input a given element of the list, and some accumulating value over the list. The signature of `fold` is as follows:

```
fold : ('a -> 'b -> 'b) -> 'b -> ('a linked_list) -> 'b
```

For this function, the `'a` given as input to the provided function is each element in the `'a linked_list`. The `'b` provided as input is the initial term given to the provided function, for the output to be then fed back in to that function. If this is all a bit confusing, it should make a bit more sense as we write the implementation and do some examples.

```
let rec fold (f : 'a -> 'b -> 'b) (acc : 'b) (ls : 'a linked_list) : 'b =
    match ls with
    | Nil -> acc
    | Cons(x, xs) ->
        fold f (f x acc) xs
```

Notice how in this implementation we already use constant stack space by default.

Let's carefully consider the code here, specifically what happens in the `Cons(x, xs)` case. In this case, `fold` is recursively called, where the same function is passed down, the rest of the list is passed down, and the new value of `acc` is the result of the function applied to the start of the list and the current value  of `acc`.

To best demonstrate the usefullness of this function let's implement some other list functions which can be easily expressed in terms of `fold`.

### Sum and Concat

Here are functions to sum the values of an `int` list and concatenate the values of a `linked_list` respectively (note that the `string` concatenation operator in OCaml is `^`).

```
let sum (ls : int linked_list) =
    fold (fun a b -> a + b) 0 ls
```

```
let concat (ls : string linked_list) =
    fold (fun a b -> a ^ b) "" ls
```

### Map

The previously described list function `map` can be expressed in terms of `fold`. Notice that this implementation is identical in function to the tail recursive implementation which reverses the list.

```
let map (f : 'a -> 'b) (ls : 'a linked_list) : 'b linked_list =
    fold (fun a b -> prepend (f a) b) Nil ls
```
