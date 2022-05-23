## Tuples

Sometimes we want something a little more lightweight to group our data together. Maybe you have a function where you want to return two variables. In these cases we generally would use tuples.

One of the great things about tuples is we don't need to write any specific type definition, we can just use them.

To create a tuple, all we need to do is group multiple variables using parenthesis and separating them by commas. For example:

```
let tuple = (4, "hello")
```

Here we have created a tuple with 2 values, but tuples can have as many values as you want... well technically the limit is 4194303, but it is even exceptional to create tuples with more than about 5 values.

So what is the type of our tuple, well the type for the above tuple is written as:

```
(int * string)
```

This is because the type of the two fields are integers. This means that the type of a tuple is dependent on the number of fields, and the types of each field. As such, tuples are not designed to be collections used in the same way a list or array would. Tuples are simply a convenient way of grouping data together, and are most useful as a lightweight way to return multiple values from a function, for example.

```
let divide a b =
    let remainder = a mod b in
    let quotient = a / b in
    (quotient, remainder)
```

This function calculates the quotient and remainder upon division for the two input variables, and returns a tuple accordingly. Let's see an example of calling this function:

```
let result = divide 5 2
```

So how do we get the values out of the tuple? Well we can use pattern matching to deconstruct our tuple as follows:

```
let _ = match result with
    | (quotient, remainder) ->
        Printf.printf "The quotient is: %d\n" quotient;
        Printf.printf "The remainder is: %d\n" remainder;
        ()
```

In the case within our match expression the identifiers `quotient` and `remainder` have the values within the tuple bound to them.

We can also pattern match in a more compact way since there is only one case as follows:

```
let (quotient, remainder) = result
```

This then shows the simple syntax that we can use from the start to return two variables from a function:

```
let (quotient, remainder) = divide 5 2
```
