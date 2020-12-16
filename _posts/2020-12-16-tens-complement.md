---
layout: post
title:  "A Trick for Subtraction"
date:   2020-12-16 09:59:00 +0000
categories: Arithmetic
---

The first series of my blogs is to explain how we build a digital circuit for
addition and subtraction -- the most fundamental arithmetic operations. I will
start with some theory of the maths and then discuss the circuitry. The
discussion is a background for us to understand the 
[signed multiplication](https://wctangcse.github.io/arithmetic/2020/12/13/signed-multiplication-1.html) that I wrote earlier on.

To begin with, I would like to show a little trick on doing subtraction without
really doing any subtraction at all! The example is as follows: I am asked to do 

$$5 - 3$$

But suppose I do not know (or want) to subtract, so I rewrite the expression as 

$$5 + (-3)$$

Then the *negative* of 3 is formed by taking the partner of 3 in this table, and
plus 1. 

| 0 | 9 |
| 1 | 8 |
| 2 | 7 |
| 3 | 6 |
| 4 | 5 |

Therefore, we have

$$5 - 3 = 5 + (-3) := 5 + (6 + 1) = \cancel{1}2$$

By dropping the carry (based on the fact that the difference is always less than 10),
we found the difference **2** without really subtracting!

Let's try again with another example:

$$4 - 1 = 4 + (-1) := 4 + (8 + 1) = \cancel{1}3$$

We can extend the method for bigger numbers too:

$$25 - 18 = 25 + (-18) := 25 + (81 + 1) = \cancel{1}07$$

where each of the digits of 18 had its partner looked up.

# Why does it work?

The idea behind this trick is rather simple -- we borrow a fixed but enough amount so that
the subtraction can be turned into an easy table look up (and plus one). If you
look at the table and the examples again you will notice:

$$\begin{aligned}
(-3) &:= 10 - 3 = 9 - 3 + 1 &= 6 + 1\\
(-1) &:= 10 - 1 = 9 - 1 + 1 &= 8 + 1\\
(-18) &:= 100 - 18 = 99 - 18 + 1 &= 81 + 1
\end{aligned}$$

Since the table is pairing bonds of 9, the actual subtraction is replaced (or
intuitively done) by the table look up. And the addition of patching 1 is to
make 9 or 99 back to 10 or 100, such that the calculation is correct once the
carry that we are expecting is being ignored (or discarded).

You will be now tempted to try an example when you do not have enough to
subtract:

$$3 - 4 = 3 + (-4) := 3 + (5 + 1) = 9$$

There are two points to note: (1) the borrow of 10 when we look up the table is
not returned in the end, so we know there is not enough to subtract; (2) 9 can
be viewed as (-1) if you accept negative numbers (I will explain this further
later). Anyway we can now generalise this method.

# The 10's Complement Method for Subtraction (Unsigned)

For now, I consider only unsigned numbers (i.e. zero and natural numbers).
Given a number $X$ of $N$ decimal digits, we define the 10's complement of
$$X$$ as:

$$Tenscomp(X) = 10^N - X = (10^N) - X + 1 = 9\ldots99 - X + 1$$

So the 10's complement can be found by looking up the table above for each of
the digits of $$X$$ followed by adding an extra one. 
By definition, the sum of $$X$$ and its 10's complement is exactly $$10^N$$. And
then the subtraction between two numbers $$X$$ and $$Y$$ can be expressed as:

$$X - Y = X + (10^N - Y) - 10^N = X + Tenscomp(Y) - 10^N$$

When $$X\geq Y$$, the sum $$X + Tenscomp(Y)$$ will be greater or equal to $$10^N$$ (i.e. we
can return the borrow by ignoring the carry); otherwise we can tell that $$X < Y$$.

> Example: 741 - 258
>
> $$Tenscomp(258) = 741 + 1 = 742$$
>
> $$741 - 258 := 741 + 742 = \cancel{1}483$$
>
> We return $$10^3 = 1000$$ in the end of the calculation.

# Isn't that it complicates the calculation?

Yes indeed, on a human perspective. But if we are to design a machine for both
addition and subtraction, then we can avoid subtraction by doing a table look up
followed by addition. Furthermore, the fewer digits you use the easier the look up
process is. In digital circuits, as you may know, we use only two digits 0 and 1. 
So the table look up is now further simplified as a digit (bit) flipping ($$0\rightarrow 1$$
or $$1\rightarrow 0$$ that can be realised efficiently using an inverter. In the
next blog, I will explain the application of the method in binary systems and
see how to build a circuit to add or subtract.
