# Project Euler 1

In line with the project based aim of this textbook, we're going to learn some more of the basic concepts of programming in OCaml by completing a simple problem, Problem 1 of the [Project Euler](https://projecteuler.net/) problem set.

```
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.
```

To solve this problem we are going to need a few new programming concepts, but let's first outline the approach.

If programming in an imperative language, we would typically do this by declaring two variables to manipulate throughout a loop, one to be our index which we will increment by one in each case, and another to count the sum of all of the multiples. Then in each run of our loop we just need to check if the index is divisible by 3 or 5 and if so, add it to the accumulator. A solution in C would look something like this:

```
int total = 0;

for (int i = 0; i < 1000; i++)
{
    if (i % 3 || i % 5)
    {
        total += i;
    }
}

printf("%d\n", total);
```

This is where we come across a few problems with a similar implementation. While it is totally possible to translate the above code into OCaml in a similar way, using techniques we will learn in a later chapter, we want to solve this program in a functional way, not an imperative way. This is because the differences in style with a functional approach will allow us to reason about far more complex problems more easily later on. The most notable problems with trying to translate the code directly are the fact that the total is updated and changed, but we said variables were immutable in OCaml, and the use of a loop, which we haven't learned yet and won't talk about for quite some time. So let's go through the new concepts we will need.