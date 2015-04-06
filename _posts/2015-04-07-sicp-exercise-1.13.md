---
layout: post
title: "SICP exercise 1.13"
category: SICP - wizard book
tags: [ SICP solutions ]
---
{% include JB/setup %}

#### Exercise 1.13
Prove that $$ Fib(n) $$ is the closest integer to $$ \varphi^n / \sqrt{5} $$,
where $$ \varphi = (1 + \sqrt{5}) / 2 $$. Hint: Let $$ \psi = (1 - \sqrt{5} / 2)
$$. Use induction and the definition of the Fibonacci numbers (see Section
1.2.2) to prove that $$ Fib(n) = (\varphi^n - \psi^n) / \sqrt{5} $$.

#### Answer
Looking at the two starting cases first to form an inductive hypothesis, when
$$ n = 0 $$ then

$$
    \begin{align*}
        \cfrac{\varphi^n - \psi^n}{\sqrt{5}}
        &= \cfrac{\left( \cfrac{1 + \sqrt{5}}{2} \right)^0
            - \left( \cfrac{1 - \sqrt{5}}{2} \right)^0}{\sqrt{5}}
        = \cfrac{1 - 1}{\sqrt{5}} = 0 = Fib(0)
    \end{align*}
$$

and when $$ n = 1 $$

$$
    \begin{align*}
        \cfrac{\varphi^n - \psi^n}{\sqrt{5}}
        &= \cfrac{\left( \cfrac{1 + \sqrt{5}}{2} \right)^1
            - \left( \cfrac{1 - \sqrt{5}}{2} \right)^1}{\sqrt{5}}
            = \cfrac{\cfrac{1 + \sqrt{5} - \left(1 -
            \sqrt{5}\right)}{2}}{\sqrt{5}} \\
        &= \cfrac{\cfrac{2\left(\sqrt{5}\right)}{2}}{\sqrt{5}}
            = \cfrac{\sqrt{5}}{\sqrt{5}} = 1 = Fib(1)
    \end{align*}
$$

Using strong induction for when $$ n $$ is arbitrary

$$
    \begin{align*}
        F(n) &= F(n - 1) + F(n - 2)
            && \text{from the definition of Fibonacci numbers} \\
        &= \cfrac{\varphi^{n - 1} - \psi^{n - 1}}{\sqrt{5}}
            + \cfrac{\varphi^{n - 2} - \psi^{n - 2}}{\sqrt{5}}
                && \text{using the inductive hypothesis} \\
            &= \cfrac{\left[ \varphi^{n - 2} + \varphi^{n - 1} \right]
            - \left[ \psi^{n - 2} + \psi^{n - 1} \right]}{\sqrt{5}}
                && \text{rearranging} \\
            &= \cfrac{\left[ \varphi^{n - 2}\left(1 + \varphi \right) \right]
                - \left[ \psi^{n - 2} \left(1 + \psi \right) \right]}{\sqrt{5}}
                && \text{factoring}
    \end{align*}
$$

Now note that

$$
    \begin{align*}
        1 + \varphi &= 1 + \cfrac{1 + \sqrt{5}}{2} = \cfrac{3 + \sqrt{5}}{2}
            = \cfrac{1 + 2\sqrt{5} + 5}{4} \\
        &= \cfrac{\left(1 + \sqrt{5}\right)\left(1 + \sqrt{5}\right)}{4}
            = \left( \cfrac{1 + \sqrt{5}}{2} \right)^2 = \varphi^2
    \end{align*}
$$

And similarly $$ 1 + \psi = \left( \frac{1 - \sqrt{5}}{2} \right)^2 = \psi^2 $$.


Using this in the formula above gives the last step:

$$
    \begin{align*}
        \cfrac{\left[ \varphi^{n - 2}\left(\varphi^2 \right) \right]
            - \left[ \psi^{n - 2} \left(\psi^2 \right) \right]}{\sqrt{5}}
            = \cfrac{\varphi^n - \psi^n}{\sqrt{5}}
    \end{align*}
$$
