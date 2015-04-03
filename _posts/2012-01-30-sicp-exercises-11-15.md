---
layout: post
title: "SICP exercises 1.1 - 1.5"
category: SICP - wizard book
tags: [ SICP solutions, done-and-done ]
---
{% include JB/setup %}

#### Exercise 1.1
Below is a sequence of expressions. What is the result printed by the
interpreter in response to each expression? Assume that the sequence is to be
evaluated in the order in which it is presented.

    10
    => 10

    (+ 5 3 4)
    => 12

    (- 9 1)
    => 8

    (/ 6 2)
    => 3

    (+ (* 2 4) (- 4 6))
    => 6

    (define a 3)
    => <implementaiton specific>

    (define b (+ a 1))
    => <implemetaiton specific>

    (+ a b (* a b))
    => 19

    (= a b)
    => #f

    (if (and (> b a) (< b (* a b)))
        b
        a)
    => 4

    (cond ((= a 4) 6)
          ((= b 4) (+ 6 7 a))
          (else 25))
    => 16

    (+ 2 (if (> b a) b a))
    => 6

    (* (cond ((> a b) a)
             ((< a b) b)
             (else -1))
       (+ a 1))
    => 16

#### Exercise 1.2
Translate the following expression into prefix form

$$ \frac{5+4+(2-(3-(6+\frac{4}{5})))}{3(6-2)(2-7)} $$

#### Answer
    (/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 3))))) (* 3 (- 6 2) (- 2 7)))

#### Exercise 1.3
Define a procedure that takes three numbers as arguments and returns the sum of
the squares of the two larger numbers.

#### Answer
    (define (square x) (* x x))

    (define (sum-of-squares x y) (+ (square x) (square y)))

    (define (max x y) (if (> x y) x y))

    (define (sum-of-squares-largest-pair x y z)
        (cond ((> x y) (sum-of-squares x (max y z)))
              (else (sum-of-squares y (max x z)))))

#### Exercise 1.4
Observe that our model of evaluation allows for combinations whose operators are
compound expressions. Use this observation to describe the behavior of the
following procedure:

    (define (a-plus-abs-b a b)
      ((if (> b 0) + -) a b))

#### Answer
The result of the evaluated conditional `(if (> b 0) + -)` will be the operator
+ or the operator -, so the method body turns into either `(+ a b)` or
`(- a b)`.

#### Exercise 1.5
Ben Bitdiddle has invented a test to determine whether the interpreter he is
faced with is using applicative-order-evaluation or normal-order evaluation. He
defines the following two procedures:

    (define (q) (q))

    (define (test x y)
      (if (= x 0)
          0
          y))

Then then evaluates the expression

    (test 0 (q))

What behavior will Ben observe with an interpreter that uses applicative-order
evaluation? What behavior will he observe with an interpreter that uses
normal-order evaluation? Explain your answer.

#### Answer
Using the substitution model and applicative-order evaluation "evaluate the
arguments, and then apply":

    1. retrieve the body of test
    ==> (if (= x 0) 0 y)

    2. replace the parameters by the arguments
    ==> (if (= 0 0) 0 (q))

    3. evaluate the arguments
    3a. 0 ==> 0
    3b. retrieve the body of q
    ==> (q) ==> (q) ==> (q) ==> (q) ...

This is stuck in an endless recursion since q refers to itself.

Using the substitution model and normal-order evaluation "fully expand, and then
reduce":

    1. retrieve the body of test
    ==> (if (= x 0) 0 y)

    2. replace the parameters by the arguments
    ==> (if (= 0 0) 0 (q))

    3. keep expanding, so evaluating the conditional, and since 0 = 0 the result is
    ==> (0)

    4. then evaluate
    ==> 0

Ben's test will determine what method of evaluation the interpreter uses, by
observing if this test finishes or gets stuck in an endless recursion. Since
normal-order evaluation does not evaluate the parameters until they are actually
needed, (q) will never be evaluated, thus avoiding the endless recursion.
