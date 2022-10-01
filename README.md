# bool.h: better booleans for C

### Why?  Because ANSI makes mistakes, too.

The *bool* type was added to C99 with the addition of the <stdbool.h> header,
promising the end to boilerplate *bool*s with a standard implementation
everybody could agree upon. Internally, *bool* is simply an integer value, in
line with the traditional use of integer truth values going back to K&R, and the
values are set by a simple macro **\#define**. **false** is set as **0**, as
expected by C for boolean logic, and **true** is set as **1**.

Unfortunately, the choice of integer value for **true** in <stdbool.h> was,
arguably, the wrong one. Using **1** as the value for **true** increases the
potential for some subtle bugs and, at best, is logically inconsistent. A
bitwise logical operation between **true** and another integer value will
succeed or fail depending only upon the bit in the 1's place. C treats *any*
non-zero integer value as **true**, so in some ways the choice of value for
**true** is arbitrary, but why pick a value which behaves inconsistently?

Instead, we propose the value **-1** for **true**. On any system which uses
two's complement arithmetic (which is, generally, every system designed since
the birth of the C language, and at the very least every major system in use
today), **-1** is interpreted on the binary level as a number with every bit set
to **1**. This means that a bitwise logical operation between **true** and *any*
non-zero number will function as expected. It also scales to any size integer,
from 1- to 64-bits (although the implementation will default to the standard
size of *int*, generally assumed to be 32-bits).

Such a simple improvement, but unfortunately expecting it to ever happen at the
glacial pace of ANSI C development is asking too much, especially now that
they've made their decision on the "standard".

### How?

Just drop the *bool.h* file into the includes path of your project, and include
<bool.h> with the rest of your headers. From there, the *bool* type should work
as expected, just like <stdbool.h> (except with sensible integer values).

<bool.h> also seemlessly works alongside any libraries which expect <stdbool.h>,
as it rewrites the preprocessor macros used by <stdbool.h> and mimics its
presence. Theoretically, any library which uses *bool* should happily accept
<bool.h>'s version as if it was the standard.

### Who do I have to thank for this?

This header file was created in 2022 by Auul, but it has been released into the
public domain.

### License

This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or distribute this
software, either in source code form or as a compiled binary, for any purpose,
commercial or non-commercial, and by any means.

In jurisdictions that recognize copyright laws, the author or authors of this
software dedicate any and all copyright interest in the software to the public
domain. We make this dedication for the benefit of the public at large and to
the detriment of our heirs and successors. We intend this dedication to be an
overt act of relinquishment in perpetuity of all present and future rights to
this software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE AUTHORS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF
CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org/>
