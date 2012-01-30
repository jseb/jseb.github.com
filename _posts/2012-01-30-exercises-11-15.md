---
layout: post
title: "SICP exercises 1.1 - 1.5"
category: SICP
tags: [ SICP solutions, done-and-done ]
---
{% include JB/setup %}
<h4 id="exercise-1.1">Exercise 1.1</h4>
<p>Below is a sequence of expressions. What is the result printed by the interpreter in response to each expression? Assume that the sequence is to be evaluated in the order in which it is presented.</p>
<pre><code>10
=&gt; 10

(+ 5 3 4)
=&gt; 12

(- 9 1)
=&gt; 8

(/ 6 2)
=&gt; 3

(+ (* 2 4) (- 4 6))
=&gt; 6

(define a 3)
=&gt; &lt;implementaiton specific&gt;

(define b (+ a 1))
=&gt; &lt;implemetaiton specific&gt;

(+ a b (* a b))
=&gt; 19

(= a b)
=&gt; #f

(if (and (&gt; b a) (&lt; b (* a b)))
    b
    a)
=&gt; 4

(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25))
=&gt; 16

(+ 2 (if (&gt; b a) b a))
=&gt; 6

(* (cond ((&gt; a b) a)
         ((&lt; a b) b)
         (else -1))
   (+ a 1))
=&gt; 16
</code></pre>
<h4 id="exercise-1.2">Exercise 1.2</h4>
<p>Translate the following expression into prefix form</p>
<p><img src="http://chart.apis.google.com/chart?cht=tx&amp;chl=%5Cfrac%7B5%2B4%2B%282-%283-%286%2B%5Cfrac%7B4%7D%7B5%7D%29%29%29%7D%7B3%286-2%29%282-7%29%7D" alt="\frac{5+4+(2-(3-(6+\frac{4}{5})))}{3(6-2)(2-7)}" title="\frac{5+4+(2-(3-(6+\frac{4}{5})))}{3(6-2)(2-7)}" /></p>
<h4 id="answer">Answer</h4>
<pre><code>(/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 3))))) (* 3 (- 6 2) (- 2 7)))
</code></pre>
<h4 id="exercise-1.3">Exercise 1.3</h4>
<p>Define a procedure that takes three numbers as arguments and returns the sum of the squares of the two larger numbers.</p>
<h4 id="answer-1">Answer</h4>
<pre><code>(define (square x) (* x x))

(define (sum-of-squares x y) (+ (square x) (square y)))

(define (max x y) (if (&gt; x y) x y))

(define (sum-of-squares-largest-pair x y z)
    (cond ((&gt; x y) (sum-of-squares x (max y z)))
          (else (sum-of-squares y (max x z)))))
</code></pre>
<h4 id="exercise-1.4">Exercise 1.4</h4>
<p>Observe that our model of evaluation allows for combinations whose operators are compound expressions. Use this observation to describe the behavior of the following procedure:</p>
<pre><code>(define (a-plus-abs-b a b)
  ((if (&gt; b 0) + -) a b))
</code></pre>
<h4 id="answer-2">Answer</h4>
<p>The result of the evaluated conditional <code>(if (&gt; b 0) + -)</code> will be the operator + or the operator -, so the method body turns into either <code>(+ a b)</code> or <code>(- a b)</code>.</p>
<h4 id="exercise-1.5">Exercise 1.5</h4>
<p>Ben Bitdiddle has invented a test to determine whether the interpreter he is faced with is using applicative-order-evaluation or normal-order evaluation. He defines the following two procedures:</p>
<pre><code>(define (q) (q))

(define (test x y)
  (if (= x 0)
      0
      y))
</code></pre>
<p>Then then evaluates the expression</p>
<pre><code>(test 0 (q))
</code></pre>
<p>What behavior will Ben observe with an interpreter that uses applicative-order evaluation? What behavior will he observe with an interpreter that uses normal-order evaluation? Explain your answer.</p>
<h4 id="answer-3">Answer</h4>
<p>Using the substitution model and applicative-order evaluation &quot;evaluate the arguments, and then apply&quot;:</p>
<pre><code>1. retrieve the body of test
==&gt; (if (= x 0) 0 y)

2. replace the parameters by the arguments
==&gt; (if (= 0 0) 0 (q))

3. evaluate the arguments
3a. 0 ==&gt; 0
3b. retrieve the body of q
==&gt; (q) ==&gt; (q) ==&gt; (q) ==&gt; (q) ...
</code></pre>
<p>This is stuck in an endless recursion since q refers to itself.</p>
<p>Using the substitution model and normal-order evaluation &quot;fully expand, and then reduce&quot;:</p>
<pre><code>1. retrieve the body of test
==&gt; (if (= x 0) 0 y)

2. replace the parameters by the arguments
==&gt; (if (= 0 0) 0 (q))

3. keep expanding, so evaluating the conditional, and since 0 = 0 the result is
==&gt; (0)

4. then evaluate
==&gt; 0
</code></pre>
<p>Ben's test will determine what method of evaluation the interpreter uses, by observing if this test finishes or gets stuck in an endless recursion. Since normal-order evaluation does not evaluate the parameters until they are actually needed, (q) will never be evaluated, thus avoiding the endless recursion.</p>
