# Complexity Notations for Programmers
### Opening a black box on big O, Omega and Theta notations

You're not a good programmer unless you know what big O notation is.
That's why we'll look not only into what that is, but we'll also add 2 other very useful notations: big Omega and big Theta notations.

99% of programmer will be satisfied with knowing only big O notation. Not us though, we're not content with 99%, we want to be in the top 1% percent of programmers. Ideally top 0.01%.

## Why are these useful?

They're used to know and talk about algorithms complexity. They let you know if you've done a good job or not. And also if others have done a good job or not. That's why if you don't know about big O you're not a good programmer, because you've been programming blindly for all this time. Until now.

Basically, if they say your algorithm "is $O(n)$" (read as "big O of n"), it means that in the worst case if you double the size of the input, your algorithm will *at most* about 2 times more work. And if your algorithm is $O(n^2)$ doubling the input means 4 times more work. The ideal case is $O(1)$ where if you double the algorithm input, you will still do the same amount of work. 

There's a problem though. If you have two algorithms in the same class, then you won't be able to compare them, you'll need a more in depth analysis. You know both algorithms are fast, but you don't get to know which is faster.

The other two notations are very similar. Big O tells you the *worst* case. big Omega tells you the *best* case. And big Theta tells you *every* case.
You don't need the average case because it's redundant. You can still say that "in the average case it will do *at most* this work", so you can use big O notation.

## Big O notation

Let's start with the most important one.

Using non-rigorous mathematical language, we say that a function $f(n) \in O(g(n))$ if $\exists c>0 $ and $k > 0$ such that $f(n) \leq c \cdot g(n) \forall n \geq k$.

What this is basically saying is "constants don't matter" and also "weaker functions don't matter".

Say you have a function $f(n) = 2n + 3$. Thanks to big O notation you can immediately throw away numbers and say that $f(n) \in O(n)$

If you have $f(n) = 3n^2 + 5n - 1$ you can still throw everything away and say that it's $O(n^2)$.

If you carefully look at the definition you might realize that a function inside $O(n)$ is also $O(n^2)$ and $O(n^3)$ and so on. You would be correct, though you wouldn't get anything out of it, we usually stay as close as possible to the actual complexity. We want to do a *good* job, not just our job.

You can see that we can use this to talk about our algorithms complexity.
Say you want to find the max number in an array, how many cells do you have to look into? Since you have to look at the entire array the algorithm is $O(n)$. Done, that's it, you've analyzed the complexity of an algorithm without making any calculations.

What happens if you double the array? Exactly, you have to look at double the number of cells. That's what $O(n)$ means.

Congratulations, you're into the 99% of programmers. You can go analyze your code complexity and talk with them about it very easily.
Now, you actually could stop reading this blog post, but again, we're not fine with that, we want to get into the 1% of *actually good* programmers.
To do so we'll add 2 new tools under our belt, big Omega and big Theta notations.

## The other notations

Again, first using math language:
We say that a function $f(n) \in \Omega(g(n))$ if $\exists c>0$ and $k>0$ such that $f(n) \geq c \cdot g(n) \forall n \geq k$

If you look closely it's basically the same definition as big O, except for one thing, the direction of that inequality.

That's because, as I previously said, if big O is used for the *worst* case, big Omega is used for the *best* case. Big O basically says "your function is at *worst* like this other one" and big Omega says "your function is at *best* like this other one".

And finally we have big theta.
We say that a function $f(n) \in \Theta(g(n))$ if $f(n) \in O(g(n))$ and $f(n) \in \Omega(g(n))$.
That's it. If your function has the same lower and upper bounds, then it's inside big Theta.

Do you remember the "find the max" example? As I said before, you *have* to look at every cell of the array to find the max. So yes, the algorithm is $O(n)$ but it's also $\Omega(n)$, which means it's actually $\Theta(n)$. It will *always* look at every cell of the array.

## Closing

You now have 3 very important tools to be able to analyze and talk about code complexity. You can look at the code's worst and best cases. And if they're the same you also know the average case.
You are now in the 1% of programmers that know about these. Go and put these tools into good use.