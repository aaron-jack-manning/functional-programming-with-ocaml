## Functions are Data

One of the most important ideas behind functional programming languages is that because variables are just data, and functions are just data, there's no reason we should treat them in a different or special way. This is counter to the way functions and methods are handled in imperative and object oriented programming languages.

In C for example, if we want to pass around a function into another function, we can't do this directly. Instead, we use a function pointer. This is because functions have a special status, and are treated differently in the language.

In functional programming languages this is not the case, we can pass functions into functions as much as we like.

Furthermore, all functions are closures and capture the variables within their environment before being returned. This forces functional languages to use either garbage collection or reference counting rather than manual memory management.