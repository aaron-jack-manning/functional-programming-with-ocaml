## The Built in List Type

We have put all this work into implementing a linked list as it is a useful exercise in learning OCaml, but the standard library actually already has a very similar implementation in it already. From this point on we're going to use the implementation in the standard library, so let's just quickly get familiar with that implementation and how it works. Some parts of it are quite elegant and neat, and others a bit messy.

The first great thing about the linked list implementation from the standard library is that it has a special status in the language, and therefore we have nice syntax for list literals. This is actually the syntax this text has been using in comments. For example the following is perfectly valid code:

```
let test_list = [1; 4; 3; 8]
```

The other different thing about the standard library implementation is the use of `[]` to represent the `Nil` case to be aligned with the literal syntax, and the `(::)` operator for the `Cons` case. Hence the type is declared as follows:

```
type 'a list =
    | []
    | (::) of 'a * 'a list
```

To show what pattern matching looks like for this, here is our original implementation of `map` with the new type:

```
let rec map (f : 'a -> 'b) (ls : 'a list) : 'b list =
    match ls with
    | [] ->
    | x :: xs -> (f x) :: (map f xs)
```

The only special thing here is that the case of the variant, which we said was a function that constructs an instance of the type, here is an infix operator. This is why we can use (::) as a prepending operator, and why we can pattern match with the form `x :: xs`.

We also have the power to pattern match with list literals, so we could write our `join` function from before as follows:

```
let rec join first_list second_list =
    match first_list with
    | [] ->
        second_list
    | [x] ->
        x :: second_list
    | x :: xs ->
        x :: (join xs second_list)
```


More complex techniques with pattern matching to follow.