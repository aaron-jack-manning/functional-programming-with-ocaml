## Putting it all Together

Now we can put all these ideas together into our solution. Here is the question again to remind you:

```
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.
```

The first thing we need to do is have some recursive function that we can run for each of the numbers from 999 to 1 (since the criteria of the question says less than 1000). Let's construct that function, which I will call `loop`.

```
let rec loop index =
    if index = 1 then
        0
    else
        loop (index - 1)
```

Currently if we call `loop 999` our function will iterate over all the required numbers, doing nothing special and then always returning 0. So let's now update our function to do what we want it to, which is check if index is divisible by 3 or 5 using the `mod` operator, which, unlike many other languages which use `%`, is just `mod` in OCaml.

```
let rec loop index =
    if index = 1 then
        (* do something *)
    else if index mod 3 = 0 || index mod 5 = 0 then
        (* do something *)
    else
        (* do something *)
```

Here we have three cases. Obviously we need a case for if the number is divisible by 3 or 5, because we need to count the sum of values satisfying that condition. We also need a case for when index is 1, as that is when our recursion will terminate. We then have a case for all the other numbers, let's start by filling out that one.

```
let rec loop index =
    if index = 1 then
        (* do something *)
    else if index mod 3 = 0 || index mod 5 = 0 then
        (* do something *)
    else
        loop (index - 1)
```

In that case there's nothing special to do, so we just call the `loop` function on the next number to check.

Next we can fill out our first case. If the input is provided as 1, the sum of all multiples of 3 or 5 from 1 to 1, is just 0, since 1 isn't divisible by 3 or 5.

```
let rec loop index =
    if index = 1 then
        0
    else if index mod 3 = 0 || index mod 5 = 0 then
        (* do something *)
    else
        loop (index - 1)
```

Lastly we need to figure out what happens when we have a number divisible by 3 or 5. In that case we still need to call `loop` on the next number, but we also want to add the index to the result of that call.

```
let rec loop index =
    if index = 1 then
        0
    else if index mod 3 = 0 || index mod 5 = 0 then
        index + loop (index - 1)
    else
        loop (index - 1)
```

So when this runs the computer will keep adding the required value to the recursive call at each step, until we get to 1, return 0, and then add up all the instances of index added at the time.

Now let's try calling our `loop` function with `index` starting as 999, and displaying the result.

```
let rec loop index =
    if index = 1 then
        0
    else if index mod 3 = 0 || index mod 5 = 0 then
        index + loop (index - 1)
    else
        loop (index - 1)

let solution = loop 999

let _ = Printf.printf "%d\n" solution
```
