
# Code Compression Law
### Why one-liners are a bad idea

Every time I try to refactor some code I try to decrease the total number of lines. 
Every time I have at least 2 identical pieces of code I try to extract them into a method.
*Somehow* every time I try to do so i think the total code will decrease by like 10 or 20 lines, only to find out the number of lines is pretty much unchanged.
Why does that happen?

## Law of code compression

Let's say you have some code. Let's say some parts of that code are identical. You would like to take those lines of code, put them into a single function and call that function instead. Let's call this process "*code compression*". Some people call it *extract method*, but "compressing code" is actually more generic, you might be also *deleting* useless code. For this post though, "code compression" and "extract method" basically mean the same thing.

What happens when you compress code?
Ideally you would like the total number of lines to decrease, so you have to maintain less. But you also have to add some lines right? You have the function definition, maybe a line to close the function block, maybe some comments, some empty lines, maybe the code is not trivial to compress so you have to slightly edit it, all sorts of things. These are only some of the factors that fight against you when you try to compress it, and so I'll call them "waste".

So when is it actually good to compress code? Well, if you have $n$ lines which you can compress in $k$ places, but when doing so you create $\delta$ lines of waste, then you have

$$
(n-1)(k-1) \geq \delta + 1
$$

As long as this inequality holds true, then you'll actually reduce code instead of increasing it.

## Proof

Let's see where the formula actually comes from.

Again, we have $n$ lines of code in $k$ places to compress.
Say the rest of the code is $x$ lines long.

Before compressing it we have $x + k \cdot n$ total lines.
When we compress it, we have a function which is $n$ lines long. You'll then call that function in $k$ places. In doing so you create $\delta$ lines of waste. So the total is $x + n + k + \delta$. The keen observers among you might have seen that this is turning a roughly $O(n \cdot k)$ into an $O(n + k)$. That would be correct but we're still no-where close the formula from before, so let's go on.

We want the code to *decrease* in lines, or at least to stay the same, so we want
$$
\begin{align*}
\text{lines of code before compression} &\geq \text{lines of code after compression} \\
x + k \cdot n &\geq x + n + k + \delta
\end{align*}
$$


With some manipulations we have

$$
\begin{align*}
x + k \cdot n &\geq x + n + k + \delta \\ 
k \cdot n &\geq n + k + \delta \\ 
k \cdot n - k - n &\geq \delta \\
k \cdot n - k - n + 1 &\geq \delta + 1 \\
(n - 1)(k - 1) &\geq \delta + 1
\end{align*}
$$

Which is the formula from before.

## Analysis

Ok, so now we have a formula, but what is it actually telling us?

Let's try plugging in some numbers.
Say we want to compress some pieces of code, each 2 lines long.
When I write code a new function adds me 4 lines of waste (2 empty lines for readability, 1 for function declaration + function block start and 1 for function block end).
How many places do I need to compress to decrease the total number of lines? In other words, $k$ = ?

$$
\begin{align*}
(n - 1)(k - 1) &\geq \delta + 1 \\
(2 - 1)(k - 1) &\geq 4 + 1 \\
k - 1 &\geq 5 \\
k &\geq 6 \\
\end{align*}
$$

So I would need to call that function at least 6 times before it's actually worth it.
This is... strangely high right?
Let's see if it's correct.
Lines of code *before* compression = $x + 2 \cdot 6$ = $x + 12$
Lines of code *after* compression = $x + 2 + 6 + 4$ = $x + 12$
It seems to actually work, it actually IS that high.

Now the first realization. What if the piece of code I want to compress is 6 lines long? Well the formula is symmetric, I can switch $n$ with $k$ without any worry, so I'll only need to call that function twice.
Longer functions means calling them fewer times, while shorter functions means calling them many times.

What about single line functions?
Well here's the problem, you will **always** increase the number of lines.
$$
\begin{align*}
(n - 1)(k - 1) &\geq \delta + 1 \\
(1 - 1)(k - 1) &\geq \delta + 1 \\
0(k - 1) &\geq \delta + 1 \\
0 &\geq \delta + 1 \\
-1 &\geq \delta \\
\end{align*}
$$
Which is impossible since we know $\delta \geq 0$. In fact in most languages it's actually greater than 1.

So every time you have a function with 1 line of code, you are *increasing* the number of lines of the whole program. Maybe it's not such a good idea having many one-liners right?

I actually suggest trying this formula for your specific code. How many lines of waste do you create when you add a function? How many times do you have to call a 2-line function before the number of lines decreases? What can you change about your writing to improve the situation? You might find some really interesting results.

