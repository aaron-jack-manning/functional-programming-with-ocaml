## Persistance of Functional Structures

One interesting thing you may not have realised about our implementation of a linked list compared to an imperative one is that it is persistent by default.

Let's understand what this means in the context of some of the list functions and how the nodes are represented in memory.

Whenever we have made a modification to the list, we have simply taken in the previous list and returned a new list. This is true of functions like map for example which create a completely new set of nodes and do not touch the old list at all. Re-examining our map function from before, we used an empty list as the starting value of `acc`, and built the last as we mapped down.

```
let rec reverse_helper (ls : 'a linked_list) (acc : 'a linked_list) : 'a linked_list =
    match ls with
    | Nil ->
        acc
    | Cons (x, xs) ->
        reverse_helper xs (prepend x acc)

let reverse ls = reverse_helper ls Nil
```

The same is true of the non tail recursive implementation which built the resulting list from the empty list which was returned in the terminating case.

Some of our list functions did not rebuild the entire list from scratch but still returned a new list. One such example is prepend which simply takes the old list and stores it as the tail of the new list. In this case, the new list points to the old list, and therefore both lists are valid to be used and passed around even though they share many nodes because those nodes cannot be mutated.

This property of functional linked lists, and as we will say later on many other functional data structures is called persistence. When we make an operation on a list, without doing anything special, both the original list and the new list are valid lists and unchanged. Any operation which attempts to modify a list either copies and creates new nodes or shares data with the original list in a way that is completely safe.

Implementing a list in such a way in C is very difficult, because sharing references to the same data for multiple lists makes memory cleanup hard. In OCaml we don't have this problem because of garbage collection but other functional languages use a technique called reference counting where the number of references or pointers to any piece of data are counted and when the count hits 0, the memory is freed. Such an implementation is possible in languages which don't do reference counting automatically through the use of smart pointers such as in C++ or Rust. This benefit of functional linked lists comes at a disadvantage though. For example, if maintaining the old list is not crucial, then appending to the end of a list in C can be done without the need to copy all of the nodes, because the final node can be mutated. These tradeoffs make a functional linked list and imperative linked list two slightly different data structures which can be used in different contexts, and is one of the reasons why many newer programming languages offer the flexibility to conveniently work with both. That having been said, the primary goal of this text is to introduce the functional way of doing things, such that these ideas can be applied elsewhere, not just to say that the OCaml ways is better, and one of the problems with OCaml is that implementing a list which can be mutated is significantly more difficult.   


