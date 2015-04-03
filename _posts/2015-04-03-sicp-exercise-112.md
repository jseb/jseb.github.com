---
layout: post
title: "SICP exercise 1.12"
category: SICP - wizard book
tags: [ SICP solutions, done-and-done ]
---
{% include JB/setup %}

#### Exercise 1.9
The following pattern of numbers is called *Pascal's triangle*.

$$
\newcommand\T{\Rule{0pt}{1em}{.3em}}
    \begin{array}{c c c c c c c c c c }
            &   &   &   & 1 &   &   &   &   \\
            &   &   & 1 &   & 1 &   &   &   \\
            &   & 1 &   & 2 &   & 1 &   &   \\
            & 1 &   & 3 &   & 3 &   & 1 &   \\
          1 &   & 4 &   & 6 &   & 4 &   & 1 \\
    \end{array}
$$

The numbers at the edge of the triangle are all 1, and each number inside the
triangle is the sum of the two numbers above it. Write a procedure that computes
elements of Pascal's triangle by means of a recursive process.

#### Answer
Each row $$ n $$ of Pascal's triangle contains the Binominal Coefficients
$$ {n \choose k} $$ for $$ k = 0 \dots n $$.

$$
\newcommand\T{\Rule{0pt}{1em}{.3em}}
    \begin{array}{c c c c c c c c c c }
            &   &   &   & {0 \choose 0} &   &   &   &   \\
            &   &   & {1 \choose 0} &   & {1 \choose 1} &   &   &   \\
            &   & {2 \choose 0} &   & {2 \choose 1} &   & {2 \choose 2} &   &   \\
            & {3 \choose 0} &   & {3 \choose 1} &   & {3 \choose 2} &   & {3 \choose 3} &   \\
          {4 \choose 0} &   & {4 \choose 1} &   & {4 \choose 2} &   & {4 \choose 3} &   & {4 \choose 4} \\
    \end{array}
$$

So one element can be calculated using the recursive formula

$$
{n \choose k} = {n - 1 \choose k - 1} + {n - 1 \choose k} \quad \text{for }
1 \le k \le n - 1
$$

with the base case $$ {n \choose 0} = {n \choose n} = 1 $$.

    (define (bico n k)
      (if (and (>= k 0) (>= n 0) (>= n k)) ; function domain
          (if (or (= k 0) (= k n))         ; base case
              1
              (+ (bico (- n 1) (- k 1)) (bico (- n 1) k)))))
