
In C++, always use new instead of malloc if possible.

Compare these two submissions:

https://leetcode.com/submissions/detail/22554071/
https://leetcode.com/submissions/detail/22553962/

The only difference here is new/malloc.

malloc only allocate enough memory. It doesn't call the constructor of the struct, which leaves
the vector in undefined state.
