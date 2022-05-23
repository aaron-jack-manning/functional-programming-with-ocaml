# Debugging

For the following sections, as we work with lists, the outputs of demo code will be shown, but if following along, it would be handy to have a function which displays a list to the terminal for debugging. So let's implement that function first, in this case specifically for lists of integers. We will cover how to implement a similar function generically later on, but for now, let's create a function which displays an integer list as a semi-colon separated list of values.

For this function, we are going to process the list by pattern matching, displaying the front of the list and recursively calling the display function on the rest of the list.

The first case for a list which is `Nil` does not need to display anything, so we can just return unit.

For the other case, we can call `printf` to display the current value of the list and a semi-colon separating it from the rest, and then recursively call display on the remaining values in the list.

```
let rec display (ls : int linked_list) =
    match ls with
    | Nil -> ()
    | Cons(x, xs) ->
        Printf.printf "%d; " x;
        display xs;
        ()
```

This function works alright, but it does have the disadvantage that it leaves a trailing semi-colon at the end and doesn't display a new line. To solve this problem we can use some of the powers of pattern matching to pattern match another layer deep, to identify the last element of the list.

The modified code is as follows:

```
let rec display (ls : int linked_list) =
    match ls with
    | Nil -> ()
    | Cons(x, Nil) ->
        Printf.printf "%d\n" x;
    | Cons(x, xs) ->
        Printf.printf "%d; " x;
        display xs;
        ()
```

Notice how now that we have this new case, the code should only reach the first case in the pattern match if it does in the first recursive call, otherwise the end of the list will be identified ahead of time in the second case.

Finally, it might be useful to group the values in the list so that an empty list is meaningful. To do this, let's rename the above function to `display_helper`, remove the new line from the end and display some brackets on either side.

```
let rec display_helper (ls : int linked_list) =
    match ls with
    | Nil -> ()
    | Cons(x, Nil) ->
        Printf.printf "%d" x;
    | Cons(x, xs) ->
        Printf.printf "%d; " x;
        display_helper xs;
        ()

let display (ls : int linked_list) =
    print_string "[";
    display_helper ls;
    print_string "]\n";
    ()
```

The output of this function will now look something like this:

```
[1; 2; 3; 4]
```
