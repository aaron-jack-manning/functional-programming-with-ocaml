# Linked List

One data structure commonly used in computer programming is a linked list. Notice how we're going to start with a linked list as the first collection to learn about, whereas arrays would often be the first in imperative languages. The reason we aren't starting with arrays, is that the memory allocations required to implement them are fundamentally effectful. We will look at arrays later on, as some data structures can't be efficiently implemented without them, but until then, we will look at some quite creative implementations in a purely functional way.

Linked lists are very easy to implement in a functional language like OCaml. Let's first outline how they will be different to an imperative linked list and why functional programming languages lend themselves to this data structure nicely.

First it is important to note that memory allocations are effectful. In OCaml, we don't manually allocate and free memory, there is a garbage collector which handles memory for us.

We also will not be manipulating lists in place, since our variables are immutable. This means that appending to the front of a list, is the process of creating a new node which points to the old list, and appending to the back of a list can only be done by coping each node all the way down.

With these ideas in mind, let's implement a functional linked list.
