# What is an object really?
### A matter of definitions

Every code snippet in this post is to be treated as pseudo code.

I think everyone would agree that this makes an object:
```cpp
struct Foo {
    int data1;
    int data2;
    
    void bar();
}

Foo obj = new Foo();
```


But would you also agree that this is an object too?

```cpp
struct Foo {
    int data1;
    int data2;
}

void bar(Foo foo);
Foo obj = new Foo();
```


There's a simple reason why both of these are in fact objects.
Here's the code for the first example:

```cpp
void Foo::bar() {
    this.data1 = 10;
    this.data2 = 2 * this.data1;
}
```

And here's the second:

```cpp
void bar(Foo this) {
    this.data1 = 10;
    this.data2 = 2 * this.data1;
}
```

See any similarity? They're basically the same code!

You can clearly see that moving the function outside the struct had no impact on how the function is written.

What has actually changed is one thing: encapsulation, meaning `bar(...)` is no longer inside `Foo::`, but it's actually a standalone function.
Someone might argue that's a very big difference, because in the first example `bar(...)` is encapsulated inside the object, while in the second one it's not.
This means that I don't have to go search for a function somewhere, it's already inside the object, the IDE can just autocomplete when I write `Foo.` and show me both the data and functions of the `Foo` object.
The problem with that is, it's not an object that should do that, that's the job of a namespace/module. Namespaces and modules are what's actually used to separate functions and objects into different "portions" (or chunks, or blocks, call them whatever you want).

So what if I write
```cpp
struct Foo {
    int data1;
    int data2;
}

void bar(Foo this) {
    this.data1 = 10;
    this.data2 = 2 * this.data1;
}
```

and then use it with a namespace?

You would most likely get something like this:
(remember, this is all pseudocode)

```cpp
#import Code as Name

void main() {
    Name.Foo foo = new Name.Foo();
    Name.bar(foo);
}
```

In this example simply writing `Name.` would again trigger the autocomplete, achieving the same result as before, except even better, because you also get the Foo object with it.

Let's do a quick recap. We have shown that both a function inside a struct or outside the struct still makes an object. How much can we push this?

I mean, does this make an object?
```cpp
struct Foo {
    static int data1;
    static int data2;
}

void bar(Foo foo);
Foo obj = new Foo();
```

Yeah you can't have multiple objects due to that static but is it still **an** object? I would argue it does, I mean, you still access data in the same way.
Then would this be an object?

```cpp
namespace Foo {
    int data1;
    int data2;
}

void bar(int data1, int data2);
```

The memory footprint is the same, the access is the same, ...

Can we still go on?

```cpp
int data1;
int data2;

void bar(int data1, int data2);
```
Do these compose an object? Are these part of an "implied" object? An invisible object which *could* exist? Is this already an object that *actually* exists? I mean, you still have a collection of data and functions accessing it.
You might think this is totally different but if you write `Foo.data1` is `Foo` a struct name with a static variable or a namespace? Is encapsulation really a fundamental property of objects or is it a byproduct of namespaces and modules?

According to Alan Kay, which is *basically* the inventor of Object Oriented Programming, [Everything is an object.](https://wiki.c2.com/?AlanKaysDefinitionOfObjectOriented)

Would you agree with him? Would you not? What do you call an object? What *is* an object really?
