## Exercises

Most functional programming languages have a linked list implementation in their standard libraries, OCaml included. There are many functions that are useful for a linked list implementation that we haven't implemented in this section. Below are a bunch of those functions described by what they should do and their type signature. Implementations are left as exercises, and as such test cases are provided in the form of a series of statements which should evaluate to true, and solutions are given at the back of the book. As specified in the previous section, from this point we will use the list type from the standard library, so complete the following exercises using that.

```
(* This function should determine if the specified value is in the provided list. *)
contains : 'a -> 'a list -> bool

(* Test cases:
true = contains 5 [1; 2; 5; 9]
false = contains 1 []
true = contains "hello" ["goodbye"; "hello"]
*)
```

```
(* This function should take as input the length of a desired list and a function from the index of each element in the list to the desired value at that index, and produce such a list. Assume the index starts at 0. *)
initialise : int -> (int -> 'a) -> 'a list

(* Test cases:
[1; 2; 3; 4; 5] = initialise 5 (fun x -> x + 1)
[true; false] = initialise 2 (fun x -> x = 0)
[0; 2; 4] = initialise 3 (fun x -> x * 2)
*)
```

```
(* Takes two list and returns a list of tuples, pairing values at equivalent indexes together in a tuple. You may assume the two input lists are equal in length. *)
zip : 'a list -> 'b list -> ('a * 'b) list

(* Test cases:
[(1, 4); (2, 5); (3, 6)] = zip [1; 2; 3] [4; 5; 6]
[('a', 'c'); ('b', 'd')] = zip ['a'; 'b'] ['c'; 'd']
*)
```

```
(* Returns a list of tuples of all pairs of adjascent elements, and the first and last element paired together. Check the test cases for specific implementation details. *)
pairwise : 'a list -> ('a * 'a) list 

(* Test cases:
[(1, 2); (2, 3); (3, 4); (1, 4)] = pairwise [1; 2; 3; 4]
[true, false] = pairwise [true; false]
*)
```