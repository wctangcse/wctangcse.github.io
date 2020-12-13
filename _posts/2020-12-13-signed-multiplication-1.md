---
layout: post
title:  "Signed Multiplication -- Part 1"
date:   2020-12-13 10:02:07 +0000
categories: Arithmetic
---
Through the years my students repeatedly confuse with the theory of the signed
multiplication. They do not understand why a correction is needed when the
mutliplier (i.e. $$B$$ in $$A\times B$$) is negative. This is of course complicated
by the fact that negative numbers are expressed or represented by its 2's
complement so that only addition is required through the procedure.

In this short article, I am going to explain and derive the correction in
signed multiplication and then discuss the implications when the theory is
applied to multiplier circuit design.

# The Mathematics

Let's consider two natural numbers $$A$$ and $$B$$. We represent them in
binary using exactly $$N$$ bits, as bit strings 
$$a_{N-1}a_{N-2}\ldots a_0$$ 
and
$$b_{N-1}b_{N-2}\ldots b_0$$
respectively. 
Then these representations give different numerical values depending
whether it is positive or not:

$$b_{N-1}b_{N-2}\ldots b_0= \left\{
\begin{matrix}
B & \mathrm{when}\ B \geq 0\\
2^N - |B| = 2^N + B &\mathrm{when}\ B < 0\\
\end{matrix}
\right.$$

To avoid subtraction in binary, we convert subtraction into a 2's complement
operation (equivalent to take a negative of a number) followed by a normal
addition.

$$A - B = A + (-B)\rightarrow A + (2^N - B)$$

The carry out $$2^N$$ should be **ignored** to keep the answer numerically correct.

Now consider the binary multiplication $A\times B$ using the longhand method,
where each bit $$b_i$$ is multiplied (or logic AND) to $$a_{N-1}\ldots a_0$$ and
then shift $$i$$ bits to the left to give a signed subproduct. The subproducts
$$P_i$$ are added together to produce the final product $$P$$. In order to preserve
the numerical value of $$A$$, the additions of subproducts require $$N + 1$$ bit in
minimum and every time the partial sum should be signed extended. In other
words, the binary addition, under such conditions, is signed and safe with the
2's complement representation. We can therefore express $$P$$ as:

$$P = \sum_{i=0}^{N-1} A\times 2^ib_i = A\times\sum_{i=0}^{N-1}2^ib_i$$

When $$B$$ is positive, it is obvious that
$$P = A\times B$$
which is the product that we expect, in a total of $$2N$$ bits and in 2's complement
representation.

Nevertheless, when $$B$$ is negative, we can deduce that 

$$P = A\times (2^N - |B|) = A\times (2^N + B) = 2^NA + AB$$

So we need to correct the answer after the binary multiplication by subtracting
$$2^NA$$ from $$P$$:

$$\begin{align}
P - 2^NA &= AB\\
AB &= P + 2^N(2^N - A) = P + C
\end{align}$$

Subtraction is once again avoided by taking 2's complement of $$A$$ in $$N$$ bits and
shifting that $$N$$ bits to the left. The correction is in $$2N$$ bits and matches
the bit width of the product $$P$$. We let $$C = 2^N(2^N - A)$$ the correction value
that should be added to the product when $$B$$ is negative.

To conclude, the **first method** to correct the answer from the binary
multiplication is:

$$AB = \left\{
\begin{matrix}
P & \mathrm{when}\ B \geq 0\\
P + C &\mathrm{when}\ B < 0\\
\end{matrix}
\right.$$

If there is a carry at bit position $$2N$$ (i.e. $$2^{2N}$$), it should be ignored.

> Example 1: $$N = 4, A = -6, B = -3$$, then
>
> $$a = 10000_2 - 0110_2 = 1010_2 = 10$$, $$b = 10000_2 - 0011_2 = 1101_2 = 13$$
>
> $$P = 10110010_2$$ (carry discarded) which represents $$-78 = -6\times 2^4 + (-6)(-3)$$
> $$C = 2^4(2^4 - (-6)) = 16(16 + 6) = 352 = 256 + 92$$
> $$P + C = -78 + 352 = 256 + 18 = 2^8 + 18$$
> 
> So once the carry at bit 8 is discarded, the correct product is found.

Correcting the product after the binary multiplication requires a total of $$N +
2$$ additions ($$N + 1$$ for subproducts and a last one correction). 
This can be reduced back to $$N + 1$$ if we merge the correction with the last
addition of subproduct, noting that:

$$P = \sum_{i=0}^{N-1} A\times 2^ib_i = \sum_{i=0}^{N-2}2^ib_i + 2^{N-1}A$$

when $$B$$ is negative and $$b_{N-1} = 1$$. Then we have:

$$\begin{aligned}
AB  &= P + 2^N(2^N - A) \\
    &= \sum_{i=0}^{N-2}2^ib_i + 2^{N-1}A + 2^N(2^N - A)\\
    &= \sum_{i=0}^{N-2}2^ib_i + 2^{N-1}(A + 2(2^N - A))\\
    &= \sum_{i=0}^{N-2}2^ib_i + 2^{N-1}(2^{N+1} - A)\\
    &= P' + C'
\end{aligned}$$

$$C'$$ is the 2's complement of $$A$$ in $$(N+1)$$ bits then shifted $$(N-1)$$ bits to
the left. $$C'$$ is also $$2N$$ bits in width and matches that of the partial
products $$P'$$.

This gives the **second method** to correct the answer from the binary multiplication:

$$AB = \left\{
\begin{matrix}
P & \mathrm{when}\ B \geq 0\\
P' + C' &\mathrm{when}\ B < 0\\
\end{matrix}
\right.$$

Similarly, if there is a carry at bit position $$2N$$ (i.e. $$2^{2N}$$), it should be ignored.

> Example 2: $$N = 4, A = -6, B = -3$$
> 
> $$a = 10000_2 - 0110_2 = 1010_2 = 10$$, $$b = 10000_2 - 0011_2 = 1101_2 = 13$$
>
> $$P' = 11100010_2$$, $$C' = 2^3(2^5 - (-6)) = 00110000_2$$ 
> (if you convert from 560 then you need to drop the leading *10* bits and leave the least 8 bits)
> $$P' + C' = 11100010_2 + 00110000_2 = 1\,00010010_2$$ 
> which gives **18** when carry out is discarded.

In the next part, I will discuss what this mathematical analysis means to the multiplier design in digital electronics.
