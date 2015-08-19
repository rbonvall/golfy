Second draft
============

Available characters
--------------------
ISO-8859-7 symbols,
removing the ones that are too similar to others
(e.g. greek alpha with latin a)
or the ones that are too confusing
(e.g. soft hyphen)::

    _
      ! " # $ % & ' ( ) * + , - . /
    0 1 2 3 4 5 6 7 8 9 : ; < = > ?
    @ A B C D E F G H I J K L M N O
    P Q R S T U V W X Y Z [ \ ] ^ _
    ` a b c d e f g h i j k l m n o
    p q r s t u v w x y z { | } ~

      ‘ ’ £ € ₯ ¦ § ¨ ©   « ¬     ―
    ° ± ² ³ ΄ ΅ Ά · Έ Ή Ί » Ό ½ Ύ Ώ
    ΐ     Γ Δ     Η Θ     Λ     Ξ
    Π     Σ     Φ   Ψ Ω Ϊ Ϋ ά έ ή ί
    ΰ α β γ δ ε ζ η θ ι κ λ μ ν ξ
    π ρ ς σ τ υ φ χ ψ ω ϊ ϋ ό ύ ώ

Types
-----

* ``@f``: function
* ``@n``: number
* ``@l``: list
* ``@s``: string

Comments
--------

Comments start with ``§``
and end at the end of the line.


Definitions
-----------

Anonymous function with two parameters, returns the first::

    λxy.x

Same as above, but named ``A``::

    Axy.x

Call function with parameters ``3`` and ``5``::

    A35        §§§ → 3

Call function with parameters ``12`` and ``34``::

    A 12 34    §§§ → 12

Pattern matching::

    Bxy{
      1  2  . 9    §§§ B12 → 9
      @n @n . *xy  §§§ B23 → 6, B42 → 8, etc.
      @l _  . 8    §§§ B[1]2 → 9, B41 → 9, etc.
    }

A function's arity is fixed.
It can be overloaded only on parameter types.

Booleans
--------

Booleans are not a type.
They are represented as functions,
like in lambda calculus::

    Txy.x
    Fxy.y

    &ab{
      T T . T
      T F . F
      F T . F
      F F . F
    }

    |ab{
      T T . T
      T F . T
      F T . T
      F F . F
    }

    ¬a.aFT

    =ab{
      T T . T
      F F . T
      T F . F
      F T . F
    }


We don't need *if* expressions,
just pass both cases to the boolean::

    x.20
    (<x0)"negative""positive" §§§ → "positive"

Let's define the sign function::

    ±x{
      0 . 0
      @n. (<0)_11
    }

Note that negative one is represented as ``_1``,
since ``-`` will be parsed as a binary function.

Linked lists::

    ©lrf.flr     §§§ Cons
    `.           §§§ Nil

    §§§ :123 → [1 2 3] → (((©1)©2)©3)©`

You get the car and the cdr of the list
by passing resp. true and false as an argument::

    a.:9876
    aT       §§§ → 9
    aF       §§§ → :876

Other list functions::

    §§§ equals
    =xy{
      `  ` . T
      `  @l. F
      @l ` . F
      @l @l. & (=(xT)(yT)) (=(xF)(yF))
    }

    §§§ length
    #x{
      ` . 0
      @l. +1 $ # $ xF
    }

    §§§ concatenate
    &ab{
      `  @l . b
      @l @l . © $ aT ¦ & $ aF ¦ b
    }

``$`` means "everything from now on is a parameter",
like it is usually used for in Haskell.
If there is more than one parameter,
we can separate them with ``¦``.

Overload the plus function
for numbers, lists and functions::

    +ab{
      @n @n . _builtinAdd a b
      @l @l . μ \+ $ ζ a b
      @f @f . λ x . + (a x) (b x)
    }

    §§§ +12 → 2
    §§§ +:123:456 → :579
    §§§ +$λx.*2x¦λx.±x → λx.+$*2x¦±x

Here, ``μ`` stands for map and ``ζ`` for zip.
The backslash in ``\+`` indicates that the function ``+``
should be taken as a value and not be called.

