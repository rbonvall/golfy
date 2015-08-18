First draft
===========

Ideas
-----

Dynamically typed. Available types will be, at least:
function, number, string, list.

Have separated types for ints, floats, rationals and bools?
Have dicts? Dunno yet.

Anonymous function of two parameters that returns the first one::

    λxy.x     // same as (lambda (x y) x) in Scheme

Define a function whose name is ``A``
that does the same as the one above::

    Axy.x     // same as (define (A x y) x) in Scheme

Assigments::

    B.5       // same as (define B 5) in Scheme

Function application doesn't have parentheses nor commas::

    A23       // → 2

Tokens are one char long, unless they're preceded by whitespace,
or by an underscore at the beginning of the line::

    (λxy.x)12                        // → 1
    (λ first second . first) 11 22   // → 11
    _First x y . x                   // defines function First

Operators look like they're prefix; they're just functions::

    +12        // → 3
    + 11 22    // → 33

Maybe use ``_``, ``²`` and ``³`` as implicit function parameters::

    A.-²_     // same as Axy.-yx

``$`` is low-precedence function application like in Haskell,
so one can use it to save parentheses::

    +3(*4(+56))  // → 3 + (4 * (5 + 6)) = 47
    +3$*4$+56    // same as above

...or maybe we should just assign
convenient precedences to all functions.

Syntax for strings::

    "hello world"
    'x       // → Same as "x"

Syntax for lists::

    [4 1 3 9]
    :4139      // same as above
    :'a'b'c    // same as ["a" "b" "c"]

All functions are curried::

    A.+
    B.+2
    C.+23
    A98        // → 17
    B9         // → 11
    C          // → 5

Some builtin functional goodies::

    ι5         // → [0 1 2 3 4] (range)
    μ(*2)(ι5)  // → [0 2 4 6 8] (map)
    φ(<2)(ι5)  // → [0 1] (filter)
    ρ\+(ι5)    // → 10 (reduce), \+ means reference to +, not application
    °:fghi     // → λx.f(g(h(ix))) (function composition)

Everything behaves like a function.
Integers are functions in the `Church sense`_::

    Fx.&x'!    // function F appends a ! to a string
    3F"hello"  // → "hello!!!"

.. _Church sense: https://en.wikipedia.org/wiki/Church_encoding#Church_numerals

Maybe Church-encoded booleans are also a good idea,
so we don't need ``if`` expressions::

    S.<x0'n'p  // if x is negative then S is "n", else it's "p"

Lists also behave like functions,
with distinct meanings depending on the argument type::

    [7 4 8]1      // → 4 (index)
    [7 4 8][4 1]  // → [7 4 8 4 1] (concatenate)
    [7 4 8](*2)   // → [14 8 16] (map)
    [7 4 8]',     // → "7,4,8" (join)

``«`` reads a line from stdin,
``»`` writes to stdout::

    »"a:"
    A.«
    »"b:"
    B.«
    »$"a + b ="$+AB

Source encoding is `ISO-8859-7`_,
so we can have ``λ`` and all the weird symbols shown above
without wasting more than one byte.

.. _ISO-8859-7: http://en.wikipedia.org/wiki/ISO/IEC_8859-7

Steal as many good (and bad) ideas
from Haskell, Racket, APL and lambda calculus
as possible.


Useless trivia
--------------

Name is pronounced *golfy* but it's written *gоλφ*.
That's neither a latin *o* nor an omicron:
it's a cyrillic *о*.

The λ pays homage to lambda calculus.
The φ is a nod to φunctional programming. 
The о is just to be a nuisance.

Having an interpreter written in Go would make the name even cuter,
but that's not a requirement.

