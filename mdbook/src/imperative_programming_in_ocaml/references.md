## References

References are a feature of the OCaml standard library which allow for mutable variables. In this subchapter we are going to re-implement all the code required for working with references.

The first thing we need to cover is mutable record fields. This is something we didn't cover when first introducing records, but OCaml allows the creation of records with mutable fields. For example, in the following type declaration all three fields can be mutated.

```
type colour =
    {
        mutable red : int;
        mutable green : int;
        mutable blue : int;
    }
```

To update the value of a mutable field, we do the following:

```
let colour =
    {
        red = 50;
        green = 20;
        blue = 30;
    }

colour.red <- 60
```

So why in a language that encourages immutability is this possible? Well, as usual, this is generally something that should be avoided. However, there are circumstances where small modifications need to be made to a record and copying the data to make the modification for a new instance of the record is expensive. This brings us to a general rule of mutability, which we will see more with arrays: mutability often makes code more subtle, and harder to understand, but sometimes it simply provides better performance, and the trade off must be made.

So let's see how we can implement the general idea of mutable variables using mutable record fields. For this we are going to use a type which we will call a `ref`.

```
type 'a ref =
    { mutable contents : 'a }
```

The `ref` type has one type parameter and is a record with a single mutable field. If we create an instance of this ref type, with the data we need to mutate in the contents field, we can then change it throughout our program. It is handy to have some functions to make that easier though.

Let's start with a function also called `ref` to create a `ref`.

```
let ref a =
    { contents = a }
```

Now `let number = ref 5` for example creates a reference with the value 5 stored inside.

What if we wish to derefence, we can create an operator for that as well.

```
let ( ! ) (cell : 'a ref) =
    cell.contents
```

Now using `!` on a reference will extract the value from inside it.

Finally, it will be convenient to be able to update a reference without accessing the contents field as well. So let's create an operator which we can use to update our reference:

```
let ( := ) (cell : 'a ref) (contents : 'a) =
    cell.contents <- contents
```

Now the following valid code can be used to declare a mutable variable, update it, and then display the result:

```
let number = ref 5
number := !number + 1

let _ = Printf.printf "%d\n" (!number)
(* Outputs 6 *)
```

Notice how we have created a system of *referencing* mutable data, not mutating the data itself, which means that mutable variables and non mutable variables have distinct types. This is especially nice as it allows it means the type signature of a function gives away if it can change the value of any variables in the above scope.