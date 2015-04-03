---
layout: post
title: "SICP exercises 1.6 - 1.8"
category: SICP - wizard book
tags: [ SICP solutions, WIP ]
---
{% include JB/setup %}

#### Exercise 1.6
Alyssa P. Hacker doesn't see why `if` needs to be provided as a special form.
"Why can't I just define it as an ordinary procedure in terms of `cond`?" she
asks. Alyssa's friend Eva Lu Ator claims this can indeed be done, and she
defines a new version of `if`:

    (define (new-if predicate then-clause else-clause)
        (cond (predicate then-clause)
            (else else-clause)))

Eva demonstrates the program for Alyssa:

    (new-if (= 2 3) 0 5)
    5

    (new-if (= 1 1) 0 5)
    0

Delighted, Alyssa uses `new-if` to rewrite the square-root program:

    (define (sqrt-iter guess x)
        (new-if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x)
            x)))

What happens when Alyssa attempts to use this to compute square roots? Explain.

#### Answer
What happens is that the program is thrown into an endless recursion, until it
runs out of memory.

`if` and `cond` are special in the way that they evaluate consequent expressions
*only* when a predicate has evaluated to true, and in the same way evaluates
alternative expressions only when the predicate is false (or in the case of
`cond`, when all predicates are false).

This is commonly referred to as ``short-circuiting'', and demonstrated by a
quick test:

    (define (p) (p))

    (cond ((= 0 0) 0)
        (else (q)))
    => 0

    (if (= 0 0) 0
        (p))
    => 0

`(p)` is never evaluated since the predicate is true in both cases (see also
exercise 1.5). By replacing special form `if` with regular procedure `new-if`,
Alyssa has lost this short-circuit behavior.

Call to special form `if`:

    (if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x)
            x))

Is short circuiting so when the guess is good enough, the alternative expresson
`(sqrt-iter (improve guess x))` is not evaluated.

Call to ordinary procedure `new-if`:

    (new-if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x)
            x))

Is not special form and is therefore evaluated according to the "evaluate the
arguments, and then apply" method of applicative order evaluation. Which means
that the argument `(sqrt-iter (improve guess x))` will be evaluated every time
`new-if` is called, even when the guess is good enough, thus never finishing.

If there was no special form `if` that was short circuiting, I guess recursion
could not be used in this way since no base case could be used to break the
recursive calls.

#### Exercise 1.7
The `good-enough?` test used in computing square roots will not be very
effective for finding the square roots of very small numbers. Also, in real
computers, arithmetic operations are almost always performed with limited
precision. This makes our test inadequate for very large numbers. Explain these
statements, with examples showing how the test fails for small and large
numbers. An alternative strategy for implementing `good-enough?` is to watch how
`guess` changes from one iteration to the next and to stop when the change is a
very small fraction of the guess. Design a square-root procedure that uses this
kind of end test. Does this work better for small and large numbers? 

#### Answer

Example of when this test fails for small numbers:

    (square 0.016)
    ==> 0.000256

So we expect `(sqrt 0.000256)` to give 0.016.

    (sqrt 0.000256)
    ==> 0.033931660284039225

The reason why this happens is due to the arbitrary fixed tolerance of 0.001 for
determining if the guess is good enough. When the numbers get smaller a
difference by 0.001 can be way off, resulting in a poor approximation of the
square root.

    (good-enough? 0.033931660284039225 0.000256)
    ==> #t

**TODO: Also, give example of failing test for large numbers, and explain how
this is due to limited precision of the *arithmetic operations*.**

**Have tried to make this fail for very large numbers, unsuccessfully. It seems
to be working fine until it overflows, when the result becomes infinity and it
gets stuck in an infinite loop**

Anyhow, doing as suggested and changing `good-enough?` to consider the guess
good enough when the improved guess is only different by a small fraction of the
old guess.

Old code:

    (define (good-enough? guess x)
        (< (abs (- (square guess) x)) 0.001))

    (define (sqrt-iter guess x)
        (if (good-enough? guess x)
            guess
            (sqrt-iter (improve guess x) x)))

New code:

    (define (good-enough? guess improved-guess)
        (< (abs (- guess improved-guess)) (* guess 0.0001)))

    (define (sqrt-iter guess x)
        (if (good-enough? guess (improve guess x))
            guess
            (sqrt-iter (improve guess x) x)))

Now:

    (square 0.016)
    ==> 0.000256

    (sqrt 0.000256)
    ==> 0.01600000244941255

#### Exercise 1.8
Newton's method for cube roots is based on the fact that if *y* is an
approximation to the cube root of *x*, then a better approximation is given by
the value

$\frac{x/y^{2}+2y}{3}$

Use
 this formula to implement a cube-
r
oot procedure analogous to the square-root procedure.

#### Answer
Re-using `square` and `good-enough?`:

    (define (cube x) (* x x x))
    
    (define (average x y z)
        (/ (+ x y z) 3))

    (define (improve guess x)
        (/ (+ (/ x (square guess)) (* 2 guess)) 3))

    (define (cbrt-iter guess x)
        (if (good-enough? guess (improve guess x))
            guess
            (cbrt-iter (improve guess x) x)))

    (define (cbrt x)
        (cbrt-iter 1.0 x))
