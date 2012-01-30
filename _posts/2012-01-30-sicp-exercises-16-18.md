---
layout: post
title: "SICP exercises 1.6 - 1.8"
category: SICP - wizard book
tags: [ SICP solutions, WIP ]
---
{% include JB/setup %}
<h4 id="exercise-1.6">Exercise 1.6</h4>
<p>Alyssa P. Hacker doesn't see why <code>if</code> needs to be provided as a special form. &quot;Why can't I just define it as an ordinary procedure in terms of <code>cond</code>?&quot; she asks. Alyssa's friend Eva Lu Ator claims this can indeed be done, and she defines a new version of <code>if</code>:</p>
<pre><code>(define (new-if predicate then-clause else-clause)
    (bond (predicate then-clause)
    (else else-clause)))
</code></pre>
<p>Eva demonstrates the program for Alyssa:</p>
<pre><code>(new-if (= 2 3) 0 5)
5

(new-if (= 1 1) 0 5)
0
</code></pre>
<p>Delighted, Alyssa uses <code>new-if</code> to rewrite the square-root program:</p>
<pre><code>(define (sqrt-iter guess x)
    (new-if (good-enough? guess x)
    guess
    (sqrt-iter (improve guess x)
        x)))
</code></pre>
<p>What happens when Alyssa attempts to use this to compute square roots? Explain.</p>
<h4 id="answer">Answer</h4>
<p>What happens is that the program is thrown into an endless recursion, until it runs out of memory.</p>
<p><code>if</code> and <code>cond</code> are special in the way that they evaluate consequent expressions <em>only</em> when a predicate has evaluated to true, and in the same way evaluates alternative expressions only when the predicate is false (or in the case of <code>cond</code>, when all predicates are false).</p>
<p>This is commonly referred to as ``short-circuiting'', and demonstrated by a quick test:</p>
<pre><code>(define (p) (p))

(cond ((= 0 0) 0)
      (else (q)))
=&gt; 0

(if (= 0 0) 0
    (p))
=&gt; 0
</code></pre>
<p><code>(p)</code> is never evaluated since the predicate is true in both cases (see also exercise 1.5). By replacing special form <code>if</code> with regular procedure <code>new-if</code>, Alyssa has lost this short-circuit behavior.</p>
<p>Call to special form <code>if</code>:</p>
<pre><code>(if (good-enough? guess x)
    guess
    (sqrt-iter (improve guess x)
        x))
</code></pre>
<p>Is short circuiting so when the guess is good enough, the alternative expresson <code>(sqrt-iter (improve guess x))</code> is not evaluated.</p>
<p>Call to ordinary procedure <code>new-if</code>:</p>
<pre><code>(new-if (good-enough? guess x)
    guess
    (sqrt-iter (improve guess x)
        x))
</code></pre>
<p>Is not special form and is therefore evaluated according to the &quot;evaluate the arguments, and then apply&quot; method of applicative order evaluation. Which means that the argument <code>(sqrt-iter (improve guess x))</code> will be evaluated every time <code>new-if</code> is called, even when the guess is good enough, thus never finishing.</p>
<p>If there was no special form <code>if</code> that was short circuiting, I guess recursion could not be used in this way since no base case could be used to break the recursive calls.</p>
<h4 id="exercise-1.7">Exercise 1.7</h4>
<p>The <code>good-enough?</code> test used in computing square roots will not be very effective for finding the square roots of very small numbers. Also, in real computers, arithmetic operations are almost always performed with limited precision. This makes our test inadequate for very large numbers. Explain these statements, with examples showing how the test fails for small and large numbers. An alternative strategy for implementing <code>good-enough?</code> is to watch how <code>guess</code> changes from one iteration to the next and to stop when the change is a very small fraction of the guess. Design a square-root procedure that uses this kind of end test. Does this work better for small and large numbers?</p>
<h4 id="answer-1">Answer</h4>
<p>Example of when this test fails for small numbers:</p>
<pre><code>(square 0.016)
==&gt; 0.000256
</code></pre>
<p>So we expect <code>(sqrt 0.000256)</code> to give 0.016.</p>
<pre><code>(sqrt 0.000256)
==&gt; 0.033931660284039225
</code></pre>
<p>The reason why this happens is due to the arbitrary fixed tolerance of 0.001 for determining if the guess is good enough. When the numbers get smaller a difference by 0.001 can be way off, resulting in a poor approximation of the square root.</p>
<pre><code>(good-enough? 0.033931660284039225 0.000256)
==&gt; #t
</code></pre>
<p><strong>TODO: Also, give example of failing test for large numbers, and explain how this is due to limited precision of the <em>arithmetic operations</em>.</strong></p>
<p><strong>Have tried to make this fail for very large numbers, unsuccessfully. It seems to be working fine until it overflows, when the result becomes infinity and it gets stuck in an infinite loop</strong></p>
<p>Anyhow, doing as suggested and changing <code>good-enough?</code> to consider the guess good enough when the improved guess is only different by a small fraction of the old guess.</p>
<p>Old code:</p>
<pre><code>(define (good-enough? guess x)
    (&lt; (abs (- (square guess) x)) 0.001))

(define (sqrt-iter guess x)
    (if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x) x)))
</code></pre>
<p>New code:</p>
<pre><code>(define (good-enough? guess improved-guess)
    (&lt; (abs (- guess improved-guess)) (* guess 0.0001)))

(define (sqrt-iter guess x)
    (if (good-enough? guess (improve guess x))
        guess
        (sqrt-iter (improve guess x) x)))
</code></pre>
<p>Now:</p>
<pre><code>(square 0.016)
==&gt; 0.000256

(sqrt 0.000256)
==&gt; 0.01600000244941255
</code></pre>
<h4 id="exercise-1.8">Exercise 1.8</h4>
<p>Newton's method for cube roots is based on the fact that if <em>y</em> is an approximation to the cube root of <em>x</em>, then a better approximation is given by the value</p>
<p><img src="http://chart.apis.google.com/chart?cht=tx&amp;chl=%5Cfrac%7Bx%2Fy%5E%7B2%7D%2B2y%7D%7B3%7D" alt="\frac{x/y^{2}+2y}{3}" title="\frac{x/y^{2}+2y}{3}" /></p>
<p>Use this formula to implement a cube-root procedure analogous to the square-root procedure.</p>
<h4 id="answer-2">Answer</h4>
<p>Re-using <code>square</code> and <code>good-enough?</code>:</p>
<pre><code>(define (cube x) (* x x x))

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
</code></pre>
