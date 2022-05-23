## Map

There are other functions that might be handy to have for our list. For example, what about a function to add 1 to each element of an integer list. To do this we are going to destruct the list as we construct a new list with the desired function applied.

```
let rec add_one_list int_list =
    match int_list with
    | Nil ->
        Nil
    | Cons (x, xs) ->
        prepend (x + 1) (add_one_list xs)
```

Or a function to divide each element of an integer list by 2.

```
let rec divide_two_list int_list =
    match int_list with
    | Nil ->
        Nil
    | Cons (x, xs) ->
        prepend (x / 2) (divide_two_list xs)
```

Looks great! But look at the similarity of the above functions though. We're doing a very similar thing in each case in terms of the high level processing of the list. The only difference here is that the function we are applying differs slightly. Therefore, it might be helpful to use this similarity to write a general function that works in both cases, and uses the function that we apply to each element as a parameter. Fortunately, OCaml let's us do this very easily because functions are first class.

### First Class Functions

OCaml functions are first class. This means they have no special status, and aren't treated in any special way. This may have been made clear by the fact that there is no special keyword to define a function, unlike many other languages that have special syntax for a function. You can even think of an ordinary variable as just a function that doesn't take an input. What this means for the purposes of actually writing code, is we can take functions as inputs, and return them as outputs, easily and conveniently, unlike languages like C where you use function points, C# where you use delegates, etc.

So let's refactor our `add_one` and `divide_two` functions to take our mapping function generically.

```
let rec ls (f : 'a -> 'b) (ls : 'a linked_list) : 'b linked_list =
    match map with
    | Nil ->
        Nil
    | Cons (x, xs) ->
        prepend (f x) (map f xs)
```

Notice how the function `f` returns type `'b` rather than `'a`, meaning we get a `'b list`. This is so that we can use our mapping to change the type of the list if we so choose.

Now if we want to use the incrementing function from before on a list, we can just use map, and curry the function by providing an anonymous function for our incrementing:

```
let add_one x = x + 1

let add_one_list = map add_one
```

This is also a great time to introduce another feature of OCaml, anonymous functions.

### Anonymous Functions

If we have a simple function to pass into map, it may be a lot neater to write the function inline, and not bother assigning it a name in a declaration, especially if we only need it once. To do this we use an anonymous function. Anonymous functions use the `fun` keyword in their declaration and use an arrow to show what the function returns. For example a function to increment a number would look like this:

```
fun x -> x + 1
```

This allows us to write a function without giving it a name and assigning it to a variable. Just as `1` is an `int` literal, or `"hello"` is a `string` literal `fun x -> x + 1` is a function literal, specifically an `int -> int` literal.

That said, we can still assign anonymous functions to a variable name in a let declaration, meaning that the following are equivalent:

```
let increment = fun x -> x + 1
```

```
let increment x = x + 1
```

Using this with our increment example from before:

```
let add_one_list = map (fun x -> x + 1)
```

### Tail Recursion

Our linked list implementation is looking pretty neat. It does however have one issue. Let's try running our map function to increment all values on a list of length 100:

```
let test_list = new_list 100 1

let new_list = map (fun x -> x + 1) test_list
```

Great, that works. What if we try an even bigger list?

```
let test_list = new_list 999 1

let new_list = map (fun x -> x + 1) test_list
```

Oh oh! We get a stack overflow error. Let's first figure out what this even means.

Let's think about how the computer actually runs our code, by examining our implementation of `map` more closely:

```
let rec map (f : 'a -> 'b) (ls : 'a linked_list) : 'b linked_list =
    match ls with
    | Nil ->
        Nil
    | Cons (x, xs) ->
        prepend (f x) (map f xs)
```

When we first run this function we get the `Cons` case, and apply `f` to the first element, before applying `map` to the rest of the list. This means that the computer needs to store memory `f x` temporarily until it knows the result of `map f xs` to return the result. However, when `map` is run in the recursive call we have the same thing again, for the next element. That means `f x` is stored for each and every element in the list until it gets to the end and can then return the whole list. This data is stored on the stack, and there's a limit to how much data the stack can hold. The error we got means we overflowed the stack... we stored too much on it.

So how can we fix it? What we really need to do is avoid the dependency on calculating the result of the recursive call to then do the prepend operation. We need some way to do the prepend before we calculate the result of the recursive call. So how can we do this? What we can do is build the new list as we go by passing it into the next function call, as a variable we will call `acc` for accumulator.

```
let rec map (f : 'a -> 'b) (ls : 'a linked_list) (acc : 'b linked_list) : 'b linked_list =
    match ls with
    | Nil ->
        acc
    | Cons (x, xs) ->
        map f xs (prepend (f x) acc)
```

This way if we call this new `map` function with `acc` starting as an empty list, each time we call the function we append the mapped value to the front of the accumulator, meaning it is passed down into the next function call, and nothing needs to be stored in memory from the above scope.

There is one minor improvement that we should make to this though. It seems kinda silly to force the user of our function to always pass `Nil` as the third argument, so let's change the name of our `map` function and write a wrapper around it which puts that in for us. Such a change would look like this:

```
let rec map_helper (f : 'a -> 'b) (ls : 'a linked_list) (acc : 'b linked_list) : 'b linked_list =
    match ls with
    | Nil ->
        acc
    | Cons (x, xs) ->
        map_helper f xs (prepend (f x) acc)

let map f ls = map_helper f ls Nil
```

Now we can call our `map` function on a huge list without any problems. There is however one quirk about our new implementation. It doesn't actually behave the same as the original implementation. You might be able to spot how, if not, I encourage you to pause and ponder why for a moment.

Let's run it and see how it differs. Consider the following code:

```
(* The list: [1; 2; 3; 4] *)
let test_list = Cons(1, Cons(2, Cons(3, Cons(4, Nil))))

let new_list = map (fun x -> x + 1) test_list
```

For which the output is:

```
[5; 4; 3; 2]
```

In the process of applying our mapping our function has reversed the list. This happens because the first value to be prepended to the accumulator is the first value of the list, then we prepend the second element, which makes it closer to the front of the new list. This continues until the last element to prepend to the new list is the last element of the original list, which becomes the first of the new list because the are added to the front.

This gives us a bit of a compromise. No need to worry about a stack overflow error, but we get a reversed list. Sometimes the order of the list is unimportant, and sometimes the size of the list is small, we just need to make a choice of which to use accordingly. We also might have cases where we use a function that reverses our list an even number of times in sequence, in which case it won't matter.

But let's be practical, if we need to map some function to a huge list and preserve order, we need some function that's going to reverse our list, so let's write that now.

Since we've seen a way to tail recursively reverse a list, we just need to do the same thing we did above but without applying any function to each value. This gives us an implementation like this:

```
let rec reverse_helper (ls : 'a linked_list) (acc : 'a linked_list) : 'a linked_list =
    match ls with
    | Nil ->
        acc
    | Cons (x, xs) ->
        reverse_helper xs (prepend x acc)

let reverse ls = reverse_helper ls Nil
```