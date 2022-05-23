# Further Reading

For this chapter, I, the author will just speak directly to you, the reader. This book is designed to introduce and explain how a different tool like OCaml can be used to write programs in this new and different way. It does not however cover all of OCaml's features, or all that you can use them for. I have aimed though to give you the tools required to do learn other functional languages more confidently, to port some of these ideas to your imperative or object oriented code, and to get you to understand the mindset of functional programming so that you can use it for your own projects, even if you don't understand everything you can do in OCaml just yet.

### Other Languages

Now that you can program in OCaml and have overcome the initial hurdle of functional programming, there are many other functional programming languages you can learn far more easily. The most noteworthy ones are F# and Standard ML. Standard ML predates OCaml and is very similar in many ways, yet it has somewhat simpler syntax and is defined by a standard rather than a compiler. F# came out after OCaml and is heavily inspired by it, but has since gained more popularity due to it running on the .Net platform and its interoperability with all other .Net languages. F# was the first functional language I learned and the breadth of libraries (including all of those available to C# programmers) make it a great language. The most noteworthy changes between F# and OCaml are operator overloading, header files replaced with a `private` keyword and summary comments and the ability to do light parametric polymorphism on traits like equality and addition. In all honesty, F# is a more well rounded language, but the fact that it runs on .Net is a dealbreaker for many  who want to produce standalone executables and not have a dependency on a VM. Any other languages you come across that claim to be in the ML family will probably have a lot you recognise from OCaml.

Then there are other functional languages like Haskell or Agda. Haskell is notorious for being difficult to learn if you don't have a background in functional programming, but your knowledge of OCaml will make it less of a barrier if you now want to tackle it. Some concepts like Monads are crucial to the way that Haskell handles side effects so now that you have implemented them yourself, they shouldn't be too difficult.

Unfortunately, there is not a lot in the way of systems programming languages in functional languages. The most noteworthy example is Rust, but I'm not really convinced that's a functional language. The great thing about functional programming though is that as it takes off (it's still a bit of a niche) some of the great ideas get ported from it to other languages. For example, Rust has immutability by default and pattern matching (not quite as powerful as F# and OCaml). Another option for low level programming is ATS but it comes with a bit of a warning. ATS is hard! The syntax is confusing, the error messages are confusing, there's not much documentation and the way it uses proofs at the type level gives a lot of powers, but also makes code complicated. Morevoer, the fusion between functional programming ideas and C interop cause for some oddities like curried functions intrinsically leaking memory. Despite all that, ATS has some fantastic ideas, that I hope get ported to other, more mainstream languages over time.

### Other Features

There are some features of OCaml or of the OCaml standard library that we didn't cover in this book. This is because I felt like they detracted from the overall point of what I aimed to teach. That said, here are some that might be interesting to look into if you're curious:

- Catching Exceptions
- Promises
- Lazy evaluation and the 'Lazy' module
- Sequences

### Other Data Structures

If you're interested in more data structures than just the list, stack, queue and map/set I covered in this text, the best book on data structures in functional languages is in my opinion *Purely Functional Data Structures* by Chris Okasaki. As a matter of fact, we actually used Okasaki's algorithm for inserting nodes into a red black tree.

### Other Standard Libraries

One commonly noted problem with OCaml is its standard library. Inconsistencies on if it is exception throwing or returns option, as well as some missing core features, leave many programmers looking elsewhere. There are other options for full replacement standard libraries people have come up with, or simple extensions to add missing features. Here are the most noteworthy ones:

- [Core (Jane Street)](https://github.com/janestreet/core)
- [Batteries Included](https://github.com/ocaml-batteries-team/batteries-included)
- [Containers](https://github.com/c-cube/ocaml-containers)

I have also created my own standard library, which, while not a full replacement of the existing standard library by any means, contains all the things I need to be productive in OCaml. The source code for that is [here](https://github.com/aaron-jack-manning/ocaml-standard-library) which I recommend reading if you want to implement your own, as there are some confusing things about what OCaml automatically opens in your files, how to prevent it and what you can't get rid of.