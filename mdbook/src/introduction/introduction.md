# Introduction

OCaml, which stands for Object Categorical Abstract Machine Language, is a compiled, statically typed, functional programming language originally from 1996, and based on Caml, it's predecessor of about 10 years.

### Why OCaml?

OCaml is a programming language that fully supports and encourages a functional programming style, including currying, algebraic data types and pattern matching, immutability by default and first class functions, without enforcing those upon the programmer. What this means is that in OCaml, writing functional programs is the way to get the best code, but for data structures which fundamentally cannot be purely functional, using references and side effects can be done in a much simpler way than a language like Haskell. Eager evaluation and less aggressive optimization by the compiler make it way easier to understand the way the computer runs the code you write than Haskell as well.

### Design Principles and Structure

This book is an introduction to functional programming, using OCaml as the language through which to do that. That said, many of the skills taught in this book will apply to other functional programming languages, most notable Standard ML and F#, but also languages Erlang, Agda and Haskell for example.

Chapter 2, entitled *A Mathematical Approach to Programming* will provide an introduction to the paradigm of functional programming, and how it differs from imperative or object oriented programming, and what advantages come from those disadvantages.

After that point, as much as possible, the concepts of functional programming and OCaml will be taught through solving problems, working on projects or implementing common data structures and algorithms to manipulate them. Ultimately this will make this book a terrible reference if you're trying to look up a standalone explanation of any given concept. That cost comes with the benefit of teaching things in a order that is more natural and intuitive.

### Prerequisites

This book is not designed to be a general introduction to programming, but rather an introduction to functional programming for people already familiar with imperative programming.

This is partially a matter of convenience, but also a design decision. In general, I believe the best first programming language to learn is C. Readers coming from a background in C should find this book to be an introduction to how to program many things they are already familiar with but in a new style. As such data general programming concepts will be explained in terms of how OCaml does them, not how they work in the first place, and data structures like linked lists will be explained with reference to the imperative equivalent.
