Third draft
===========

Let's consider pattern matching à la Haskell. Good ol' naïve factorial::

    f[0].1
    fx.*x(f$-x1)

Here we surround ``0`` with brackets to indicate it's a value,
since ``0`` will be a perfectly valid function parameter name.

That's not tail recursive,
so let's create a private auxiliary function that takes an accumulator::

    fx.{
      φa[0].a          § (a)
      φan.φ(-n1)(*na)  § (b)
      φ1x              § (c)
    }

The curly braces create a new scope,
evaluating all expressions in it and returning the last one.
Expressions ``a`` and ``b`` create
the auxiliary tail recursive function ``φ`` using pattern matching,
and expression ``c`` calls it and returns its result.

We could also create a cuter, point-freeish version
by partially evaluating ``φ``::

    f.{
      φa[0].a
      φan.φ$-n1¦*na
      φ1
    }

Now let’s begin with a straightforward implementation of the map function ``μ``::

    μf[`].`
    μfl.©$f(Tl)
         ¦μf(Fl)

Remember that `` ` `` is nil, ``©`` is cons,
and ``T`` and ``F`` work as car and cdr
(but they are actually true and false).

Instead of extracting the head and the tail from the linked list’s node,
we could pattern match against cons::

    μf[`].`
    μf[©ht].©$fh
             ¦μft

Through the recursion steps,
the mapped function doesn’t change,
so we can create an auxiliary function
that closes over the function
and only takes the list as a parameter::

    μf.{
      M[`].`
      M[©ht].©$fh
              ¦Mt
      M
    }

It’s fold’s turn now.
Let’s define how to fold a list ``l``
with operator ``+`` and initial value ``0``
(notice that these are cute variable names,
not the add operator nor the number zero)::

    f0+l.{
      Fa[`].`
      Fa[©ht].F$+ah
               ¦t
      F0l
    }

Thanks to currying,
we can omit list ``l`` altogether::

    f0+.{
      Fa[`].`
      Fa[©ht].F$+ah
               ¦t
      F0
    }

For convenience and as homage,
we can define a ``/`` reduce operator like APL’s,
in terms of ``f``::

    /+[©ht].f$h¦+¦t

Again, ``+`` can be any operator.
For example, if we take the function composition binary operator ``°``,
we can express a function pipeline like this::

    /°:fgh

Here, ``:fgh`` is syntactic sugar for ``[f g h]``,
a list of functions whose names are one-letter long.

To do: think about supporting guards.
