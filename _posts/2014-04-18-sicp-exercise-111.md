---
layout: post
title: "SICP exercise 1.11"
description: ""
category: 
tags: []
---
{% include JB/setup %}

<h4 id="exercise-1.11">Exercise 1.11</h4>
<p>A function <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f" alt="f" title="f" /> is defined by the rule that <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f%28n%29%3Dn" alt="f(n)=n" title="f(n)=n" /> if <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=n%3C3" alt="n&lt;3" title="n&lt;3" /> and <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f%28n%29%3Df%28n-1%29%2B2f%28n-2%29%2B3f%28n-3%29" alt="f(n)=f(n-1)+2f(n-2)+3f(n-3)" title="f(n)=f(n-1)+2f(n-2)+3f(n-3)" /> if <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=n%5Cgeq%7B3%7D" alt="n\geq{3}" title="n\geq{3}" />. Write a procedure that computes <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f" alt="f" title="f" /> by means of a recursive process. Write a procedure that computes <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f" alt="f" title="f" /> by means of an iterative process.</p>
<h4 id="answer">Answer</h4>
<p>So the definition of <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f" alt="f" title="f" /> is:</p>
<p><br /><img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=%0A%5C%5B%20f%28n%29%20%3D%20%5Cleft%5C%7B%20%5Cbegin%7Barray%7D%7Bll%7D%0An%20%26%20%5Cmbox%7Bif%20%24n%3C3%24%7D%5C%5C%0Af%28n-1%29%2B2f%28n-2%29%2B3f%28n-3%29%5Cend%7Barray%7D%20%5Cright.%20%5C%5D%0A" alt="
\[ f(n) = \left\{ \begin{array}{ll}
n &amp; \mbox{if $n&lt;3$}\\
f(n-1)+2f(n-2)+3f(n-3)\end{array} \right. \]
" title="
\[ f(n) = \left\{ \begin{array}{ll}
n &amp; \mbox{if $n&lt;3$}\\
f(n-1)+2f(n-2)+3f(n-3)\end{array} \right. \]
" /><br /></p>
<p>This translates to the following procedure which computes <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f" alt="f" title="f" /> by using a recursive process:</p>
<pre><code>(define (f n)
    (if (&lt; n 3) n)
        (+ (f (- n 1))
            (* 2 (f (- n 2)))
            (* 3 (f (- n 3)))))</code></pre>
<p>To use an iterative process we can use a triplet of integers <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=a" alt="a" title="a" /> <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=b" alt="b" title="b" /> and <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=c" alt="c" title="c" />, initialized as <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f%282%29%3D2" alt="f(2)=2" title="f(2)=2" />, <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f%281%29%3D1" alt="f(1)=1" title="f(1)=1" /> and <img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=f%280%29%3D0" alt="f(0)=0" title="f(0)=0" />, and repeatedly apply the transformations</p>
<p><br /><img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=a%5Cleftarrow%7Ba%2B2%2Ab%2B3%2Ac%7D" alt="a\leftarrow{a+2*b+3*c}" title="a\leftarrow{a+2*b+3*c}" /><br /> <br /><img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=b%5Cleftarrow%7Ba%7D" alt="b\leftarrow{a}" title="b\leftarrow{a}" /><br /> <br /><img style="vertical-align:middle" src="http://chart.apis.google.com/chart?cht=tx&amp;chl=c%5Cleftarrow%7Bb%7D" alt="c\leftarrow{b}" title="c\leftarrow{b}" /><br /></p>
<p>This translates to the following procedure which uses an iterative process:</p>
<pre><code>(define (f n)
    (f-iter 2 1 0 n))

(define (f-iter a b c count)
    (if (= count 0)
        c
        (f-iter (+ a (* 2 b) (* 3 c)) a b (- count 1))))</code></pre>
