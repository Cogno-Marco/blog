
# The difference between High and Low level programming languages
### Or at least a better way to define them





Most of the times when people talk about a low level language, they talk about assembly in particular. 

From wikipedia "A low-level programming language is a programming language that provides little or no abstraction from a computer's instruction set. [...] Generally, this refers to either machine code or assembly language."

And that's a totally reasonable definition, when you compare *any* language to assembly they truly are higher level. Many constructs that we have created don't exist in assembly, so when we compile/interpret any language we have to "lower them" to that level. Stuff like variables, functions, lambdas, etc.

But this definition has a few problems. Yeah every language is higher level than Assembly, but are they the same when compared with each other? Would you confidently say that C and Java are really the same "level"? Do you use them as if it where the same language or do you use them in different ways? What about Rust and Python? Lisp and Javascript?

I believe it should be time to define some new levels of programming languages.

## Relative vs Absolute scales

Here's the central idea when comparing languages. You could talk about a language in an absolute sense ("X is high level") or in a relative sense ("X is *higher level* than Y").

Saying a language is high level is much different than saying a language is *higher* level than another one. You can directly compare 2 languages and put them in order, one after the other, from low to high level, basically composing a spectrum.
Admittedly, that's a subjective ordering, but we'll get into that later.

Moreover comparing very different languages becomes basically trivial. One could argue that C++ is higher level than Rust, or vice versa, but basically anyone would agree that Python is higher level than C.

Remember we're talking about comparing them in a *relative* sense, you might say that "Python and C are *both* high level", but that would be talking in an *absolute* sense. We're directly comparing them, so yeah they're both high level, but you should also agree that Python is "higher level" than C.

Given all that you should be able to construct your own ordered list of programming languages, starting from lowest to highest level. But that wouldn't be really productive, it would just be mindless arguing. We can define some better rules to distinguish different levels, and the relative scale can actually help us.

## New separations

Alright so, let's start simple. We agree that Machine Code and Assembly are their own level of programming. But we also want more levels above that, instead of just one. We'll call the lowest "machine level", since Machine Code and Assembly give the highest level of control over what happens on the processor.

We'll then take "high level" programming languages and split them into 3 new categories, which I'll respectively call "low", "medium" and "high" level.
There's still a distinction between machine level and the others, but there's also a distinction between low, medium and high levels.

## Low Level Languages

As we said before there's a high difference between very distant languages, when compared relative to each other, like C and Python. You don't work the same way in C as you do in Python. That's because there exist a set of language features specifically made to "simplify" your work when using the language. 

The same way we have invented variables and functions to simplify working with Assembly, we've also invented other constructs, like garbage collectors, to simplify working with C-like languages.
When you use Java you (normally) don't have to think about memory in the same way you have to when you work in C. To me that makes them part of two different levels.

To better formalize this concept, I claim that the are 2 rules which distinguish low level languages from others. Only languages that respect all rules are low level.
1. The language is *directly* compiled into machine level.
2. The language is a "*system* programming language"
Let's see why I believe these should be the main rules, starting with the second one.

System programming languages are languages that make interaction with the hardware and the OS on which they run its main strength.
If a language is truly to be considered low level, having as much control as possible is exactly what you want. If your language doesn't let you control the hardware on which it runs, is it really as powerful? Would you use it in the same way if it did? Would the software that you create be identical? When a language hides all that away, it makes you interact with the system in a different way. That's a difference worthy enough to distinguish languages with each other. That's not something only I think, we already have a different name for those kind of languages, they're "system programming languages". You might recognize some of these languages: C, C++, Rust, Swift, Zig, Nim, etc. They're also all compiled, so they're all low level.

As a quick side note, to me if the language hides memory complexity behind something like a garbage collector, but still gives you the option to disable it or control memory in some way, that doesn't necessarily make a low level language. That's just a low level *construct* in a *higher* level language. It gives the option to lower the language. It's like having a language where you can write in assembly. That doesn't make the *whole* language "machine level", it's still a higher level language, just with some machine level mixed in when you really need to. 

Having maximum control of the system is not enough to make a language low level, you also need it to be compiled. If the language is interpreted instead of compiled, that makes a higher level language.
If you've used both a compiled and an interpreted language you can very much *feel* the difference. You interact differently with the language. Some stuff becomes much easier to do. Dynamic typing is really really hard in lower level languages, while you get it basically for free in dynamic languages.

Same goes if the language is still compiled, but to an intermediate language instead of machine code, which is then interpreted by a VM of some sort. You still use a higher level language, but not in the same way. To me that makes different level of languages. Compiled languages are also faster than interpreted ones, that's a big separation too. You wouldn't use Java/C#/Python as a language to make an Operating System and also expect it be as fast as one created in C/C++/Rust, so they must be at different levels.

Given all of that we can see some languages which can truly be considered to be low level.
We know C and C++ respect all rules, so they definitely should be low level. We also know Java, C#, Javascript, Typescript and Python to break either one or the other rule, so they are *higher* level still. I also believe Rust is still a low level language, since you have to still basically handle memory yourself. The borrow checker and smart pointers like `Box`, `Rc` and `Arc` obviously help you, but you still have to do the work yourself. It's definitely lower than languages where you have a garbage collector, you still need to think about memory, you can't just forget about it.


## Mid and High Level languages

So we know that if a language doesn't respect at least one of those 2 rules it's *not* low level. But how do we know if a language is mid or high level?

Thanks to those 2 rules it's incredibly simple. If it break only one of those rules it's mid level, otherwise it's high level.
Said another way, mid level languages are either *interpreted* system languages or compiled *non-system* languages. High level languages are instead neither system-oriented, nor compiled.

This quickly means that C# and Java are mid level languages (since both are a system language but not directly compiled, they have an intermediate language), while Python, Javascript and Typescript are high level (since they're neither compiled, nor system languages, stop using them as such it's a very bad idea!).


## Final Comments

So there we have it, we've separated programming languages into 4 distinct categories.
Now when you look at it, you can see that each new language tries to create a higher and higher level one every time.

Now, an interesting observation. Have you noticed how the difference between machine level and low level is **much** higher than low level and both mid and high levels? To me that's a result of a very interesting phenomenon. There's much less evolution in programming languages than there was in the past.

What do you think? Will programming languages continue to move into higher and higher level or will there be a change of pace? Will we have truly unique languages in the future or will we just have the ones we have now?
