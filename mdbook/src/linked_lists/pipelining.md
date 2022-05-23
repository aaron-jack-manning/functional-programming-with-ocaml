## Pipelining

One of the nicest things about OCaml is that its syntax and style makes well written OCaml programs incredibly easily to quickly read and understand, especially compared to imperative programming languages. One simple idea that contributes to this benefit greatly is the pipelining operator (`|>`) build into the standard library.

Even though this operator exists in the standard library, writing an implementation of it ourself is actually incredibly easy.

```
let (|>) f g = g f
```

This operator does one simple thing: reverses function application.

Here is a very simple example of how this operator can be used with a function of a single input.

```
let increment x = x + 1

let number = 5

5 |> increment (* 6 *)
```

For functions with multiple inputs, normal function application takes predence over this operator. This can be thought of as taking the last input variable to a curried function and putting it before the pipeline operator. Here is a simple example with a function of two variables.

```
let divide a b = a / b

let number = 10

let result = 2 |> divide number (* 5 *)
```

### Repeated Application with Lists

Many readers may now be wondering why this section on this reversed function application operator is in the chapter on lists. The reason for this is how this operator makes repeated operators on lists significantly easier to read. Here is one example of taking a list of integers, incrementing it, filtering out the elements that are divisible by 2 and summing the result.

```
let numbers = [2; 6; 3; 9]

numbers
|> map (fun x -> x + 1)
|> filter (fun x -> x % 2)
|> sum
```

This now gives some understanding as to why all of the previously implemented list functions take the list as the final input, because each function takes in all the inputs to the right of the function name, and the result is a function which is then applied backwards to whatever was before the `|>` operator.

That result is then the input given last to the next function.

This order of application is a little clearer while learning if the extra set of brackets are included as below:

```
let numbers = [2; 6; 3; 9]

numbers
|> (map (fun x -> x + 1))
|> (filter (fun x -> x % 2))
|> (sum)
```

If the key variable which should be included in the pipelining sequence is not taken as the final curried input to the function, brackets around the pipelining operator could be used to disambiguate.

The overall design decision around the pipelining operator is that it allows the order that functions are applied to be the same as the order they are applied in code.

If we write the above code without the pipelining operator it would look like this.

```
let numbers = [2; 6; 3; 9]

sum (
    filter (fun x -> x % 2) (
        map (fun x -> x + 1) numbers
    )
)
```

Getting used to writing functions in the order they should be applied has the benefit long term of writing significantly more readable code, and is one of the simple yet brilliant ideas from OCaml that has yet to take off in a lot of other programming languages, partially due to the dependency on currying.

### Function Composition

As an extension of the above idea, we can define a function application operator as follows:

```
let (>>) f g x = g (f x)
```

This is the definition of the function application operator in F#, a language heavily based on OCaml, and is a useful operator to include in your own code.

Here is an example of its usage:

```
let increment x = x + 1
let double x = 2 * x

let increment_then_double = increment >> double
```

This operator allows the building of complexity from composing simple functions together, and is one of the key design philosophies of many functional languages. In many other languages however, such as Haskell, function application is written in the opposite order, as it is in standard mathematical notation.

This idea of writing functions in the order they are applied makes for significantly more readable code.

