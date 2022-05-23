## Filter

Another very useful function for working with lists is a function we will call `filter`. This function similary to map, will demonstrate the usefulness of first class functions. We are going to write `filter` to have the following signature:

```
filter : ('a -> bool) -> 'a linked_list -> 'a linked_list
```

The idea of this function is that we provide a linked list and a function which takes each element of the linked list and returns a boolean. If the boolean is `true`, then the examined element is included within the output list, otherwise it is not.

Let's first start with a simple recursive implementation, which deconstructs the list, and behaves differently depending on if the first element satisfies the function or not.

```
let rec filter (f : 'a -> bool) (ls : 'a linked_list) : 'a linked_list =
    match ls with
    | Nil -> Nil
    | Cons(x, xs) ->
        if f x then
            prepend x (filter f xs)
        else
            filter f xs
```

Here our base case is simply when the list is empty, in which case no filtering needs to occur, and then if the function evaluates to `true` on the first element then it gets included within the output list, otherwise we simply make the recursive call without prepending it.

Let's test out this implementation on a a sample list:

```
(* The list: [4; 3; 1; 0] *)
let test_list = Cons(4, Cons(3, Cons(1, Cons(0))))

let new_list = filter (fun x -> x > 2) test_list
```

Where the above outputs the list:

```
[4; 3]
```

However you may notice, based on our study of the `map` function, that on a large enough list, such a function would store too much data on the stack, and thus result in a stack overflow. We can solve this problem in the same way we did with `map`, with tail recursion. That is, we have a helper function which passes the accumulator down as one of the arguments. Such an implementation would be as follows:

```
let rec filter_helper (f : 'a -> bool) (ls : 'a linked_list) (acc : 'a linked_list) : 'a linked_list =
    match ls with
    | Nil -> acc
    | Cons(x, xs) ->
        if f x then
            filter_helper f xs (prepend x acc)
        else
            filter_helper f xs acc

let filter f ls = filter_helper f ls Nil
```

We can also use the flexibility of `if` statements in OCaml to be used inline to optionally rewrite the above as:

```
let rec filter_helper (f : 'a -> bool) (ls : 'a linked_list) (acc : 'a linked_list) : 'a linked_list =
    match ls with
    | Nil -> acc
    | Cons(x, xs) ->
        filter_helper f xs (if f x then prepend x acc else acc)

let filter f ls = filter_helper f ls Nil
```

Notice how just as happened with the implementation of map, our tail recursive implementation of filter reverses the list. Due to the nature of the function filter, this is less likely to matter but if preservation of order is important, then we can similarly reverse the list as we did in the previous example.
