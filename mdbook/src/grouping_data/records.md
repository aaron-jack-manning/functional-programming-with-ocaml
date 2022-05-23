## Records

Records are a data type for combining other data types together, giving names to each piece of data. This is very similar to structs in C.

To declare a record, since we are declaring a new type, we use the `type` keyword. We then provide the names of each field and the data types. Note that OCaml enforces the name of the types and fields do not start with an uppercase letter. Here are some examples of record declarations to show off the syntax, and their potential uses:

```
type colour =
    {
        red : int;
        green : int;
        blue : int;
    }
```

```
type person =
    {
        name : string;
        age : int;
        height: float;
    }
```

To create an instance of a record, we just make sure to provide each of the fields as follows:

```
let purple = { red = 128; green = 0; blue = 128; }
```

Then to access any values from the record, we use dot notation.

```
let red_of_purple = purple.red
```

If we want to create a new record but use most of the fields with an existing record, we can create one from the other.

```
let grey = { purple with green = 128; }