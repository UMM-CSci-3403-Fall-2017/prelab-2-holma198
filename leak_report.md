# Leak report

After running valgrind, it appears that 46 bytes of data are consumed
and never returned to the memory.  The culprit appears to be when the
strip function is run, specifically line 41 when result becomes a
pointer for saved characters.
A leak also occurs on line 62, which is when a new variable, cleaned
is being stored, cleaned, and pointing to the function strip, which
has the memory leak issue listed earlier.
Finally on line 87, the function is clean is run, this function
contains the variable assignment to strip, and strip contains a memory leak
issue on line 41.

This issue might be resolved if a line that frees the variable pointer is placed
somewhere in the strip function.

EDIT: This issue can be resolved by placing the free statement in the second function rather than the first one, since running the first function and storing it in a variable in the second function, the memory pointer can be accessed in this variable. An if statement was added to ensure that free() would not be run with an empty string.
