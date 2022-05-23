## If Statements

We have already covered if expressions, and the fact that an if statement is a function which returns the same type on all branches. But what about in circumstances where your if statement has effectful code in it, performing IO or updating a variable? In these cases we need an if statement. This is the same OCaml construct as an if expression but with one small detail worth mentioning. Let's say you have a variable in a program which should never be negative, and you wish to check that with a debugging statement that gives a warning if it is.

Up until this point, we have said that all branches must return the same value in an if statement, and functions like those in the print family (`print_*`) return `unit`, since they don't need to give back any useful information, so we just need to have an else case that returns `unit` right?

```
if number < 0 then
    print_string "Warning, number is negative"
else
    ()
```

This is totally valid OCaml code and does exactly what we need it to. We can however actually omit the else case here.

```
if number < 0 then
    print_string "Warning, number is negative"
```

This else case is implicitly still there. The if statement will return `unit` if the condition is not satisfied. The OCaml compiler doesn't force us to write it in though. Hence, if statements do not need to have an else case if their return type is `unit`.