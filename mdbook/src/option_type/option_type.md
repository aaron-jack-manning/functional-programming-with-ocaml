# Option Type

Now that we have worked with variants, let's explore how they can be used in error handling, to represent cases where functions may not return any meaningful data. For this, the standard library has a type built in called `option`, with a declaration like this.

```
type 'a option =
    | Some of 'a
    | None
```

Let's use this to refactor a simple function which throws an exception:

```
let rec find_first (f : 'a -> bool) (ls : 'a list) : 'a =
    match ls with
    | x :: xs ->
        if f x then
            x
        else
            find_first f xs
    | [] ->
        failwith "value not found"
```

This function returns the first value in a list for which the provided function returns true, and throws an exception if no value satisfies the function.

Throwing an exception here is an easy thing to do, and makes the implementation neat, but it's not really optimal. Catching exceptions slows down code, and by returning an object that could encapsulate the error we can force the user to actually handle the error. This design decision, to force the caller to handle the error produces far less error prone code.

So let's use this option type and return `Some` when there's a reasonable result, and `None` when there isn't.

```
let rec find_first (f : 'a -> bool) (ls : 'a list) : 'a option =
    match ls with
    | x :: xs ->
        if f x then
            Some x
        else
            find_first f xs
    | [] ->
        None
```

This way when a caller is using the function they can choose how to handle the error:

```
let test_list = [2; 4; -1; 8]

match find_first (fun x -> x < -2) test_list with
| Some result ->
    (* handle the success case *)
| None ->
    (* handle the error case *)
```

The great thing about doing this is that it forces the caller to handle the error case. Even if they choose to make it throw an exception, we can write library code that can sensibly return no data without halting the program.

Now if you want this to be exception throwing, it can help to have some simple syntax to do that, so let's write an unwrap function to do just that.

```
let unwrap (opt : 'a option) (message : string) : 'a =
    match opt with
    | Some result -> result
    | None -> failwith message
```

This is similar to `expect` if you have ever programmed in Rust.

In general though, `option` type is commonly used in cases where `null` would typically be used in an imperative program. OCaml also has a built in type called `result` for handling true errors are passing on debugging info, but as you have seen with lists and options, we can implement any of these things ourselves without too much trouble.

We'll see more cases where returning option type can be useful in the next section.