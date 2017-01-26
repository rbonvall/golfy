For all
-------

Let’s define the for-all function ``Apl``,
that returns true iff predicate ``p`` holds
for every element of list ``l``::

    Ap[`].T
    Ap[©ht].&$ph¦Apt

An alternative approach would be::

    Ap.{
      a[`].T
      a[©ht].&$ph¦at
      a
    }

Now let’s use the function ``ζl``,
that tells whether ``l`` is the empty list,
to write ``A`` as a single boolean expression
(equivalent Scheme code on the right)::

    Apl.|$ζl          § (or (null? l)
         ¦&$p(Tl)     §     (and (p (car l))
           ¦Ap(Fl)    §          (forall p (cdr l))))

Is divisible
------------

*n* is divisible by *d* if *n* mod *d* is zero.
As a goλφ function::

    Dnd.=0$%nd

Range
-----

The range of *n*, written as ``ιn``,
is the list of integers from 0 to *n* - 1::

    ι5      § → [0 1 2 3 4]

If we want to use 2 as the starting value,
we can call the cdr function ``F``
twice on the result::

    F$F$ι5  § → [2 3 4]

Since integers are also functions in the Church sense,
we can also write that as::

    2F$ι5   § → [2 3 4]


Is prime
--------

Now we have all the pieces in place
to write function ``Pn``,
that tells us whether ``n`` is prime::

    Pn.A$¬Dn
        ¦2F$ιn

We are taking advantage of the ``¬`` operator,
that not only works on booleans but also on functions,
and it does so by negating its return value.
If that wasn’t the case,
we would need to use a lambda expression for the predicate::

    Pn.A$λd.¬(Dnd)
        ¦2F$ιn

or to create a new one by composing ``¬`` and ``Dn``::

    Pn.A$°¬(Dn)
        ¦2F$ιn

