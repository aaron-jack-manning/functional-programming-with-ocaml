# Hello World

As with any programming language, it's typical to start with a simple "Hello, World" program to get up and started with the language and compiler. In this case, we will go through such a program and then quickly move into a slighly more in depth program, to get straight into the language and its features. Throughout this chapter, don't stress if you don't fully understand everything. All the relavent concepts will be covered in depth later on. For now though, just read through, and try and soak up as much as you can about how OCaml is different from other languages you may have used, and how the process of writing programs in OCaml may differ.


## Getting Started

Let's begin by creating a file which we will call `main.ml`, to write our code in. The `.ml` extension is used for OCaml source code files.

Unlike many other programming languages, OCaml doesn't require a special function to be the entry point of the program, we just write code on the top level.

Open up `main.ml` and copy and paste the following code:

```
print_string "Hello, World!\n";
```

Now navigate to the directory with `main.ml` in your chosen terminal and run the following commands:

```
ocamlopt -o program main.ml
./program
```

You should see "Hello, World!" displayed in your terminal window.

OCaml has two compilers, `ocamlopt` and `ocamlc`. We will explore the differences between them, the different output files, compiler options and splitting up the compiling and linking stages when we get to work on more complex programs. For most of the code we're going to write early on though, we will just work in one file and you can compile and run using the above commands.

Let's first though just understand the single line of code we have written.

`print_string` is a function in an OCaml module called `Stdlib`, which is automatically included in all files by the compiler. The type signature of `print_string` is as follows:

```
val print_string : string -> unit
```

In OCaml, strings are specified with double quotes, and function application doesn't require any parenthesis, hence the syntax we used. Notice that the end of our line has a semi-colon. We'll actually talk about this a bit more later, as that semi-colon is less innocent that it would be in a language like C. Remember the type signature above? Well unit is a return value that is used in OCaml when there is no real data to return. You can kind of think about it like `void` in many other languages. The use of the semi-colon turns our *expression* of function application into a *statement*, which doesn't have any value.

As specified above, OCaml has no special entry point function. That said, writing code on the top level in between function declarations can make code more difficult to read. Hence, it is typical to write a main function which will be the entry point of the program, and then to call main. Here is a refactored "Hello, World!" program.

```
let main () =
    print_string "Hello, World!\n"


let _ = main ()
```

When declaring variables or functions, we use the `let` keyword, followed by the name of the variable or function. Notice the use of `()` after the function name of `main`. When writing a function in OCaml, arguments are separated by spaces after the function name. `()` is the way we represent that unit type from before. Just as a boolean has two cases: `true` and `false`, unit has one case: `()`.

What is the purpose of taking in an argument that provides no information then? Well because a consequence of code being written on the top level is that it is evaluated immediately. If we have a variable which has code in it that is effectful (prints to the console for example), then it is evaluated at the start of the program, because it already has all of its arguments. Here though, the `main` function only runs when the bottom line is reached.

So what's up with that bottom line. Well, there are some interesting consequences of OCaml's syntax which can sometimes be a bit irritating. OCaml is very light on syntax, and you will not see anywhere near the same number of brackets as you may see in languages with C style syntax, but OCaml is not whitespace significant. This means the compiler has to use a few key pieces of information to figure out what is what, and what falls in which scope. In that final line, a `let` binding is used to call the `main` function and actually display the result, but that function returns unit, so we don't care about the result. The `_` represents discarding that result. This is actually a case of a feature called pattern matching which will be discussed later. For now, when you see `_`, it means discard the variable rather than binding it.

One other interesting thing about OCaml is that there is no `return` statement. The final block of code in a function is whatever is returned. So in this case, `main` returns whatever `print_string` returns, which is `unit`.

Going back to the semi-colon from before, if we changed our code to:

```
let main () =
    print_string "Hello, World!";

let _ = main ()
```

We would get an error. This is because the semi-colon ends a *statement*, which has no return value, and our function must return something, hence its final line must be an *expression*.

The actual compiler error looks like this:

```
File "main.ml", line 6, characters 0-0:
Error: Syntax error
```

and is remarkably unhelpful. OCaml's compiler errors are fantastic when it comes to types, but useless when it comes to syntax, and regularly misreports the location. We will discuss this further later on, along with techniques on how to deal with it.
