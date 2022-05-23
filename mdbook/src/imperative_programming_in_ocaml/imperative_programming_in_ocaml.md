# Imperative Programming in OCaml

OCaml is a functional language at its core, but it does not enforce functional programming upon the user. Imperative programming features such as mutable data, loops and if statements are allowed. Morever effectful functions with file IO for example are totally fine. In languages like Haskell which attempt to enforce a certain style upon the programmer these things are very difficult. OCaml discourages but allows them without too much difficulty. This chapter examines some of the imperative programming features of OCaml. While this might seem counter to the goal of this textbook, in teaching functional programming, it is the belief of the author that the best programs come from a place of a functional design where imperative features are used sparingly if necessary. Hence these tools are presented as options when for mutability provides benefits in performance critical scenarios, or conditional IO helps with debugging.