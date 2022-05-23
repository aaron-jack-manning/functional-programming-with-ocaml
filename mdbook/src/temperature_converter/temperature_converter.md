## Temperature Converter

As a short program to introduce some more ideas in OCaml and functional programming in general, let's write a simple tool in the command line to convert temperatures from celcius to fahrenheit and vice versa.

We're going to start with a pretty simple, and somewhat imperative program, and slowly analyse it for how to change it to make it better, and use more of a functional style.

The first thing we are going to do is implement our conversion functions, which are the core part of the program.

Here are some simple implementations:

```
let fahrenheit_to_celcius x =
    (5.0 /. 9.0) *. (x -. 32.0)

let celcius_to_fahrenheit x =
    (9.0 /. 5.0) *. x + 32.0
```

A function in ocaml is declared as follows:

```
let <name> <arguments> =
    <body>
```

Notice how in our functions we didn't ever tell the OCaml compiler that `x` was a floating point number. This is because OCaml can infer the type for us, without sacrificing static and strong typing. If we want, we can put type annotations on to make it easier to read.

```
let fahrenheit_to_celcius (x : float) : float =
    (5.0 /. 9.0) *. (x -. 32.0)

let celcius_to_fahrenheit (x : float) : float =
    (9.0 /. 5.0) *. x +. 32.0
```

Now let's look at the bodies. As mentioned earlier, in OCaml the last line of a function is its return value. In this case, we only have one line per functions to the function returns whatever the single line expression evaluates to.

Now there is one other key thing to mention which you may have noticed, being the use of `+.` instead of `+`, and similarly for all other operators. This is because of how OCaml handles types and polymorphism. Languages that allow `+` to work for multiple types either allow this because `+` has a special status in the language (in OCaml it's just a function like any other), the use of parametric polymorphism (which OCaml does not support) or the use of operator overloading (which OCaml also does not support). If those terms don't make sense to you, think about it this way: in OCaml functions either work for a specific single type, or any type at all. Since `+` doesn't make sense for all types, addition is implemented differently for each type. The same applies to all other numerical arithmetic operations.

Now let's write some of the code to handle user input and output the result.

Consider the following code:

```
let fahrenheit_to_celcius (x : float) : float =
    (5.0 /. 9.0) *. (x -. 32.0)

let celcius_to_fahrenheit (x : float) : float =
    (9.0 /. 5.0) *. x +. 32.0

let main () =
    print_string "What temperature type would you like to convert?\n";
    let temperature_type = read_line () in
    
    print_string "Enter a number of degrees: ";
    let number = read_line () in
    
    if temperature_type = "celcius" then
        print_float (celcius_to_fahrenheit number)
    else if temperature_type = "fahrenheit" then
        print_float (fahrenheit_to_celcius number)
    else
        failwith "Invalid temperature type."
    ;

    print_string "\n"

let _ = main ()
```

We're going to go over this line by line to explain what each thing is doing, and then compile and test if it works.

The first line of the function uses the `print_string` function from the standard library and displays the string to prompt the user to input a temperature type. Note the semi-colon on the end since this function returns `unit` which we don't need to store in a variable, so this line is a statement.

We then declare a variable called `temperature_type` which is the output of the `read_line` function from the standard library, which reads a line from the standard input channel. Note the structure of this expression:

```
let ... = ... in
```

The `in` keyword ends a variable declaration, and specifies the scope in which the variable will exist. In most practical purposes, the keyword `in` can be thought of as just a line terminator for this type of expression, but you can read such an expression as declaring the variable in the rest of the current scope. We don't need the `in` keyword when declaring on the top level, but do within a function. Also take note of the fact that the variable is bound when it is created. In OCaml, variables are *immutable* and cannot be changed after declaration. This means we cannot declare a variable without giving its value, and the language has no notion of null or uninitialised variables.

We then do the same thing to get the input of the number we're going to convert.

Then we have an if statement to handle the different cases for which conversion we are doing, with an else case which throws an exception using the function `failwith`. Again, the trailing semi-colon there is because this if statement does return a value, but we are not using it.

We then send a new line to the console since `print_float` does not.

Let's run our code and see what happens:

```
File "main.ml", line 18, characters 43-49:
18 |         print_float (celcius_to_fahrenheit number)
                                                ^^^^^^
Error: This expression has type string but an expression was expected of type
         float
```

It seems like there is an error. This is a type incompatibility error, telling us that `number` is a string, but the `celcius_to_fahrenheit` function expects a float. This is because `read_line` returns a string. We need to convert this first before we use it, so let's do that.

```
let fahrenheit_to_celcius (x : float) : float =
    (5.0 /. 9.0) *. (x -. 32.0)

let celcius_to_fahrenheit (x : float) : float =
    (9.0 /. 5.0) *. x +. 32.0

let main () =
    print_string "What temperature type would you like to convert?\n";
    let temperature_type = read_line () in
    
    print_string "Enter a number of degrees: ";
    let number_string = read_line () in

    let number =  float_of_string number_string in

    if temperature_type = "celcius" then
        print_float (celcius_to_fahrenheit number)
    else if temperature_type = "fahrenheit" then
        print_float (fahrenheit_to_celcius number)
    else
        failwith "Invalid temperature type."
    ;

    print_string "\n"

let _ = main ()
```

If we compile and run our program now, it should work as expected.

### Refactoring

There are many improvements we could make to our program, so let's make those.

The first thing to change is to neaten up our if statement a bit. In OCaml, an if statement is reall just a function, which returns whatever each branch returns depending on the case. So let's return the converted float from our if statement, and assign it to a variable. That way we can display that variable after the fact. We're also going to use the module `Printf` in the standard library which has a function called `printf` for easily displaying our output.

```
let fahrenheit_to_celcius (x : float) : float =
    (5.0 /. 9.0) *. (x -. 32.0)

let celcius_to_fahrenheit (x : float) : float =
    (9.0 /. 5.0) *. x +. 32.0

let main () =
    print_string "What temperature type would you like to convert?\n";
    let temperature_type = read_line () in
    
    print_string "Enter a number of degrees: ";
    let number_string = read_line () in

    let number =  float_of_string number_string in
    

    let converted_result =
        if temperature_type = "celcius" then
            celcius_to_fahrenheit number
        else if temperature_type = "fahrenheit" then
            fahrenheit_to_celcius number
        else
            failwith "Invalid temperature type."
    in

    Printf.printf "%f\n" converted_result

let _ = main ()
```

`printf` takes in a format string, which describes what we're going to output. In this case the `%f` is a for a float, and we then supply that argument after. This is also the first time we have used a function with more than one argument, and as you may expect, we can just put the next argument after the first with a space in between.

Notice how this code still satisfies type checking, even though `failwith` is not a float. This is because it is special in that it has polymorphic output, and will satisfy typechecking because the output will be whatever it needs to be given the context.

So how else can we improve our program? Well there are two noteworthy issues, being that if the input is not a float, `float_of_string` will throw an exception, and the fact that an exception is thrown if the temperature unit is invalid. While OCaml supports exceptions, they are generally considered bad practice in functional languages for similar reasons as any effectful code. Luckily OCaml provides a way to handle this called the `option` type. Option type is a type with two cases, and we can use a pattern matching expression to check it. To introduce the idea of pattern matching, we will first refactor our if statement:

```
let main () =
    print_string "What temperature type would you like to convert?\n";
    let temperature_type = read_line () in
    
    print_string "Enter a number of degrees: ";
    let number_string = read_line () in

    let number =  float_of_string number_string in
    
    let converted_result =
        match temperature_type with
        | "celcius" ->
            celcius_to_fahrenheit number
        | "fahrenheit" ->
            fahrenheit_to_celcius number
        | _ ->
            failwith "Invalid temperature type."
    in

    Printf.printf "%f\n" converted_result

let _ = main ()
```

This `match` expression checks the variable used in the match against each of the cases. The wildcard character (`_`) at the end matches against anything, so it acts as a catch all if the previous cases didn't match. This will look familiar to the `_` used in the final line to call the `main` function, because it is basically the same thing, being a pattern matching which only hase one case, the catch all.

Now let's change the `float_of_string` function to a function called `float_of_string_opt` and use a pattern match to deconstruct it. This will look like this:

```
let main () =
    print_string "What temperature type would you like to convert?\n";
    let temperature_type = read_line () in
    
    print_string "Enter a number of degrees: ";
    let number_string = read_line () in

    let number =
        match float_of_string_opt number_string with
        | Some data -> data
        | None ->
            print_string "Invalid numerical input, defaulted to 0.";
            0.0
    in
    
    let converted_result =
        match temperature_type with
        | "celcius" ->
            celcius_to_fahrenheit number
        | "fahrenheit" ->
            fahrenheit_to_celcius number
        | _ ->
            failwith "Invalid temperature type."
    in

    Printf.printf "%f\n" converted_result

let _ = main ()
```

`option` type has two cases, one with data associated and one without. `data` in the first case is a variable bound to the data stored within that case of the option type. We can then handle this error however we choose. Here a default value is chosen, which probably isn't the most sensible, but we'll cover more sophisticated error handling at a later point.

Also notice that this default value is written as `0.0` rather than `0`. This is no accident. OCaml's type system is quite strict, and `0` is of type `int`, not `float`. This means using `0` is a type error.

One thing you also may have noticed is that we have chosen to write the `main` function underneath the conversion functions. This is not just a stylistic choice. In OCaml, dependencies go down the file, meaning that we cannot call a function called further down. This idea even extends across files, where file order is important. Because of this, OCaml has specific syntax for mutual recursion. This design decision is annoying to many first time OCaml programmers, as it forces you to think about code in a different way. However, it does provide some benefits, forcing you to write clearer code without complex dependencies across files.

This tour into OCaml hopefully gives you somewhat of an idea of what OCaml's syntax is like, and what to expect. We will now take a deep dive into specific concepts of functional programming in OCaml.
