## Atomic Extension

### No Nested LR/SC Allowed

> Chapter 17.1:
> 
> An LR instruction and an SC instruction are said to be paired if the LR precedes the SC in program
order and if there are **no other LR or SC instructions in between**; the corresponding memory
operations are said to be paired as well (except in case of a failed SC, where no store operation is
generated).
