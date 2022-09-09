# Does code rot?
### No but it gets obsolete

I had an interesting discussion with a programmer friend of mine about old code.

Basically the question was, "we know that code can smell, but can it also rot?"
This led us into a rabbit hole about what we actually believe rot to mean when talking about code.
To be fair, it also questions what we mean with "code smell". Does code *really* smell? When do you say it actually smells?
I mean, a common code smell is the so called ["god object"](https://refactoring.guru/smells/large-class), many people would call it a code smell, but is it *really*?
If the class has to do many complex things it *has* to be big and complex right? Is separating it into smaller pieces gonna do *any* good? I don't think it does.

But that's a discussion for another time, the question was about code rot, so what do we mean when we say rot?
I started with a simple reasoning. If code smell is code that *could* be improved, code rot is *old* code that *has* to improve. Basically code gets from smelling to rotting.
If I leave a piece of code unattended, not only it *could* get worse over time, but I say that over time it actually ***will*** get worse. To me that is unavoidable, when you were working on the problem you didn't have a full picture of the problem you where trying to solve. Over time you either understand that what you did was bad or could be done better, so it should be changed. To me this is true even across projects. Even if you did the best code in the world, over time something new *will* come out. One algorithm could be improved, a new language could come out, a new way to solve the same problem, anything.

This is were my friend had a different point of view. He agreed that it could be done better, but did the performance actually change over time? Well if you think about it, no it doesn't get slower, in fact if the hardware improves it actually gets faster. But you could still write it better right? This is orthogonal to what I said before. Yes your code did get faster, but you still could make it better. Then what if you couldn't?
I mean, what if you actually did create the *best* code ever? What if you actually built the most efficient code you could *ever* write? What if the best algorithm was already found?
You'd think it would matter right? If you did the mathematical best, then there's no way around it right? You wouldn't be able to still make it better, right?. 

Here's the interesting thing, I still think you could make it faster. Yes your algorithm *did not* rot, as in it did not get slower, your algorithm actually *did* get faster. But the technology and the hardware around it *did* change, so your code could *still* be improved because of it. 
Your code did *not* rot, it became *obsolete*.
This is what we agreed upon. **No, your code does not rot, it gets obsolete**.
