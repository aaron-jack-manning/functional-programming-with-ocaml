## General Functions

Now that we have a type for a list, and a basic idea of how to use it, let's write some functions to make our list more convenient to use.

Let's start with the following simple functions to make building lists easier:

```
(* To prepend a value to the start of a list *)
prepend : 'a -> 'a linked_list -> 'a linked_list

(* To join two lists together in sequence *)
join : 'a list -> 'a linked_list -> 'a linked_list

(* To create a list of a given length, with all values being a specific value *)
new_list : int -> 'a -> 'a linked_list
```

Notice again the use of `'a` as a generic type parameter. This means if our functions doesn't apply any function to the variable of type `'a` which would restrict its type, our function can also work for any type, giving us a generic linked list implementation.

### Prepend

To prepend a value to a list, we just have to construct a new list from the value to prepend and the provided list, the latter of which will be the tail of the new list. Such an implementation would look like this:

```
let prepend (value : 'a) (list : 'a linked_list) : 'a linked_list =
    Cons (value, list)
```

### Join

To join two lists, we are going to deconstruct one list until we get to the end, constructing the new list along the way. This requires us to consider what is happening in each case. Let's pattern match on the first list and consider each of the following cases:

1) The first list is `Nil`. In this case we just return the second list.
2) The first list has only one element. In this case we just need to construct a list of this one element, and the second list.
3) The first list has more than one element. In this case we just need to create a new list from the first element of the first list, and the rest of the first list joined to the second. We do this with a recursive call.

Translating the above ideas into our pattern matching code we get the following:

```
let rec join first_list second_list =
    match first_list with
    | Nil ->
        second_list
    | Cons (x, Nil) ->
        prepend x second_list
    | Cons (x, xs) ->
        prepend x (join xs second_list)
```

If we want, we can also create an infix operator for join to make it more compact.

```
let ( @ ) = join
```

OCaml let's us create all sorts of operators like this, with certain restrictions on what symbols are valid. There are also some nuances to what symbols are and aren't valid and how associativety and precence works which won't be covered here, but is worth looking into if you wish to define many operators to use together.

### New List

To create a new list with all the same values, we just need to examine the cases where we have the length of 0, and the length being greater than 0, since we are just going to recursively construct the rest of the list. So, if the specified length is 0, we just return an empty list. Otherwise, we construct a list with the specified value, and use a recursive call to make the rest of the list.

```
let rec new_list length value =
    if length = 0 then
        Nil
    else
        prepend value (new_list (length - 1) value)
```

Here we have the obvious problem that negative values give an invalid list. We'll talk about error handling later on, but for now I'm just going to ammend the code so that negative values and 0 give an empty list.

```
let rec new_list length value =
    if length <= 0 then
        Nil
    else
        prepend value (new_list (length - 1) value)
```
