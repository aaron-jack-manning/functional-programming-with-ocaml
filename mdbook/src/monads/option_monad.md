## Option Monad

The underlying type on which the option monad is based is the `option` type, which we have already seen.

```
type 'a option =
    | Some of 'a
    | None
```

In this case, we are going to use option type to handle division by 0 exceptions for integers in a non-exception throwing way. The first sensible thing to do is to implement our division function which divides integers but returns none if the denominator is 0. Here is such an implementation:


```
let ( / ) (x : int) (y : int) : int =
    if y <> 0 then
        Some (x / y)
    else
        None
```

Fantastic, problem solved right? We can just use this new function instead of the usual division operator. There are however, some issues with this. For example, the following expression is no longer valid.

```
5 + 2 / 3
```

This is because `+` expects an integer, but `2 / 3` is an `int option`.

One thing we can do to resolve this problem is implement a new version of `+` which allows inputs of `int option`. If we do this though, it makes sense to force both inputs to be options, and to reimplement the division operator so that it can also take in options. Let's start with `+`.

If either of the inputs to our operator are `None` then we are going to return `None`, thus propagating the error. That way, if we divide by 0 at any point, the whole expression becomes `None` wihtout the need for any intermediary pattern matches.

```
let ( + ) (x_opt : int option) (y_opt : int option) : int option =
    match x_opt with
    | Some x ->
        beginmatch y_opt with
        | Some y ->
            Some (x + y)
        | None -> None
        end
    | None -> None
```

And similarly for division:

```
let ( / ) (x_opt : int option) (y_opt : int option) : int option =
    match x_opt with
    | Some x ->
        beginmatch y_opt with
        | Some y ->
            if y <> 0 then
                Some (x / y)
            else
                None
        | None -> None
        end
    | None -> None
```

We end up with a lot of repeated boilerplate code though. So let's try and abstract some of it out. To do so we are going to implement a function called `bind`. This bind function is going to take in a function from an `int` to an `int option` and an `int option` to apply it to. Notice that we can't apply the function directly, we first have to unwrap the `int option`.

```
let bind (x_opt : int option) (f : int -> int option) : int option =
    match x_opt with
    | Some x -> f x
    | None -> None
```

Notice how if `x_opt` contains an integer in the `Some` case then we just apply the function, otherwise we propagate the `None`. Ultimately this is the kind of behaviour we want with our custom arithmetic operators, propagate the error if something went wrong, apply the provided function if it didn't. Let's implement our new version of `+` again using `bind`.

```
let ( + ) (x_opt : int option) (y_opt : int option) : int option =
    bind x_opt (fun x -> bind y_opt (fun y -> Some (x + y)))
```

Let's carefully consider what is happening here, because it can get a little confusing.

Let's first consider the case where both `x_opt` and `y_opt` are `Some` options. `bind x_opt (...)` is taking the integer associated with `x_opt` and passing it into the anonymous function. That means `x` in that function is just an integer. Then the same process occurs, unwrapping `y_opt` and then adding it to `x`. Now let's consider what happens if for example `x_opt` is actually none. Well if `bind` is called on `None` the function is ignored and `None` is returned. This means that if either `x_opt` or `y_opt` is `None`, the function returns `None`, the exact behaviour we were after.

Now let's do the same with division.

```
let ( / ) (x_opt : int option) (y_opt : int option) : int option =
    bind x_opt (fun x -> bind y_opt (fun y -> if y <> 0 then Some (x / y) else None))
```

This function is almost identical to the version for `+` except we make sure to check for division by 0 and return `None` or `Some` accordingly. There is still a bit of repeated code we can simplify though, by taking the binary operator at the end as a function.

```
let bind_binary (operator : int -> int -> int option) (x_opt : int option) (y_opt : int option) : int option =
    bind x_opt (fun x -> bind y_opt (fun y -> operator x y))
```

Then we can write our plus and divide functions to take inputs of `int`s and return an `int option`.

```
let plus_opt (x : int) (y : int) : int option =
    Some (x + y)
```

```
let divide_opt (x : int) (y : int) : int option =
    if y <> 0 then
        Some (x / y)
    else
        None
```

And override the operators by passing these functions into our `bind_binary` function.

```
let ( + ) = bind_binary plus_opt
let ( / ) = bind_binary divide_opt
```

Most importantly, we now have arithmetic operators which take in options and return options, and furthermore we have a mechanism to easily implement new operators. We just write a function from `int`s to an `int` option, and give it as the first argument to bind_binary. Hence doing the same for subtraction and division:

```
let ( - ) = bind_binary (fun x y -> x - y)
let ( * ) = bind_binary (fun x y -> x * y)
```

Now going back to our expression from before:

```
5 + 2 / 3
```

This doesn't technically work here, we first need to make sure we are passing in options like this:

```
Some 5 + Some 2 / Some 3
```

Unfortunately, as OCaml doesn't allow us to overload operators, we can't have an implementation to accept integers and one for integer options using the same symbol, but languages that allow this allow expressions to be written with integers or integer options mixed together just like the above. We can of course do this OCaml with seperate operators, but we're going to leave this for now.

### What does this have to do with Monads?

Now is the point where we can explain monads, and how we've implemented a monad as above.

A monad has three important things, a type with a single type parameter, and two functions called `bind` and `return`. Here are the signatures:

```
type 'a t
val return : 'a -> 'a t
val bind : 'a -> ('a -> 'b t) -> 'b
```

There are also some rules that a monad must follow called the monad laws. I'm not going to cover those thoroughly now, rather that to say it is crucial in the above function's implementations that `return` doesn't alter the data, it simply wraps it up on a box of type `'a t`, and that `bind` simply applies the provided function to its first argument where possible, otherwise it can pass on the data as is.

In our option monad above, the bind function is one we implemented and called bind. Here it is again:

```
let bind (x_opt : int option) (f : int -> int option) : int option =
    match x_opt with
    | Some x -> f x
    | None -> None
```

Note that in general, bind can have `f` with a signature `'a -> 'b t`, but in our case `'a` and `'b` are the same type. We would want to make this generic though if we were to use this option monad for types beyond integers.

So in our case, both `'a` and `'b` are `int`, and `t` is `option`.

As for return, well that is just `Some`, a function that wraps the input into the box of our type.

This section gave an explanation for how monads can be used with option type for arithmetic operators, but the definition provided here is quite general, and as you'll see in the next section we can use monads for handling errors in a slightly different way, with the result monad.

In languages like Haskell, due to the way that functional programming is enforced more strongly than in OCaml, monads are a means through which to handle effectful programming in as safe of a way as possible, and thus are necessary parts of the language, and required for things like IO.

In OCaml, it is most sensible to view monads as a design pattern, which is useful under certain circumstances such as error handling and promises (which are a way of handling concurrent programs).