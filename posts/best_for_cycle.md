
# Can we improve the for cycle?
### What I'd like to see in future languages

In the history of programming languages we have seen some evolutions of the for loop. What's the next step?

## For loop basics

There are basically 2 main ways to implement a for cycle in any language:

1. Index based
2. Iterator based

You might know the first one from languages like C/C++/C#/Java/...
```cs
float[] numbers = ...;
for(int i = 0; i < numbers.length(); i++) {
    float num = numbers[i];
    //...
}
```
Basically you work with a control variable which has a start and end values, then use it inside the loop as needed (for example accessing an array).

The other way to have a for loop is instead directly iterating over that array you want to work on.
You might know the second one from languages like Python/C#/...

```cs
float[] numbers = ...;
foreach (float num in numbers) {
    // ...
}
```

You can immediately see that the iterator based for loop is faster to write and easier to use. You can also realize that it has the same efficiency, since it's just the same amount of work written in a different more implicit way. This gives a first way to analyze for loops implementations: 
Given 2 versions of the same for loop, the fastest one is the best one, and if they have the same speed the shortest one is the best one (because it reduces development time).

Though there is also a different trick we can use to analyze for loops: converting them into a different form.

## For loop conversion

When you look at the previous examples, both are doing a very simple thing, getting each element in a list one at a time.
Here's how you might rewrite both of those for loops using a while loop:

```cs
float[] numbers = ...;
int i = 0;
while (i < numbers.length()) {
    float num = numbers[i];
    //...
    i++;
}
```

This is another key thing. We can analyze for loops using while loops and how they compare. In fact we can immediately see that in every current language implementation, iterator based for loops are actually *WORSE* than index based for loops. This is because you actually loose the index variable, and are now forced to use different constructs to gain it back. This in turn means having more boilerplate code and potentially a loss of performance.
We'll call this method *conversion* or *decomposition*.

Here is how you would commonly re-gain indices back in python:

```py
numbers = [...]

# indexless
for element in numbers:
    # ...

# indexed (most common)
for index, element in enumerate(numbers):
    # ...

# indexed (alternative)
for index in range(len(numbers)):
    element = numbers[index]
    # ...
```

And here's C#:

```cs
float[] numbers = ...;

//using LINQ, potentially loosing performance
//TODO: test performance
foreach (var (value, i) in Model.Select((value, i) => ( value, i )))
{
    //...
}
```

Using this while loop trick we can immediately see a better for loop construction, just an iterator for loop which keeps the index. Here's an example:

```cs
float[] numbers = ...;

for (index: int, value: float in numbers) {
    //...
}
```

This is exactly what [Jai's for loop](https://github.com/Jai-Community/Jai-Community-Library/wiki/Getting-Started#for-loops) lets you do.
This is because Jai uses this exact trick, [expanding a for loop into a while loop](https://github.com/Jai-Community/Jai-Community-Library/wiki/Getting-Started#for_expansion).

Moreover you can see another quick improvement, since most of the time you don't need them, you can just avoid defining those variables and have them be implicitly set and be ready to use.

```cs
float[] numbers = ...;

for numbers {
    //here you can access "index" and "value" automatically, or how Jai calls them "it_index" and "it".
}
```

## For loop conversion using iterators

There is another way to convert iterator based for loops: using iterators.
This enables for loops to work on virtually any data structure. It just needs to implement a proper iterator on that data structure.

A decomposition for that type of for loop would look something like this:

```java
BinaryTree huffman = ...;
Iterator iter = huffman.iterator();
while iter.has_next() {
    float value = iter.value;
    int index = iter.index;
    //...
    iter.next()
}
```

The closest way to have this using index based for loops is using that very iterator instead of the index, something like this

```java
BinaryTree huffman = ...;
for (Iterator iter = huffman.iterator(); iter.has_next(); iter.next()) {
    float value = iter.value;
    int index = iter.index;
    //...
}
```

Not really an improvement over the while loop, especially when compared to the range based for loop

```java
BinaryTree huffman = ...;
for value, index in huffman {
    //...
}
```

Up until now we have seen that iterator based for loops are actually more useful than the index based... except for one thing.

## Array slices

Let us look at this example

```java
float[] numbers = ...;

//assume numbers is at least length 10
for (int i = 5; i < numbers.length() - 5 ; i++) {
    float value = numbers[i];
}
```

The only way to have this using iterator based for loops is the following

```java
float[] numbers = ...;
for index, value in numbers {
    if index < 5 continue;
    if index > numbers.length() - 5 break;
    
    //...
}
```

This is a problem, not only we've added more boilerplate, but we've also lost performance due to those if statements. In the index based for loop we can just loop from one index to another, since we already know what those are.

Here's where the final improvement to for loops comes: slices.

What the original problematic code is doing is iterating on a portion of an array instead of the whole array. If we could have a way to do the same on the final for loop we would be set.

Let's say we have this:

```java
float[] numbers = ...;
for index, value in numbers[5:-5] {
    //...
}
```

Is this actually an improvement? Let's decompose both loops into a while loop to see:

```java
float[] numbers = ...;
int index = 5;
while (index < numbers.length() - 5) {
    float value = numbers[index];
    //...
    index++;
}
```

Yup, both loops would basically boil down to this, without loosing performance in any significant way.

# Closing

We have seen what I believe to be the best for loop we could have in current and future languages, at least in the close future.
As a quick recap here it is in a couple of examples:

```java
//1. iterating using indices/ranges (we might still need them for some stuff)
for 0..10 {
    //here you can use both "value" and "index", even though one is redundant
}

//2. iterating over an array
float[] numbers = ...;
for numbers {
    //here you can use "index" and "value"...
}

//3. while nesting you can fall back to named index/value pairs to avoid conflicts and problems
float[] even_numbers = ...;
for num_index, num in numbers {
    for even_index, even in even_numbers {
        // ...
    }
}

//4. complex data structures
BinaryTree huffman = ...;
for huffman {
    //you can still use "index" and "value", just the loop uses an iterator instead of an index
}

//5. slices
for numbers[5:-5] {
    //iterates over numbers from the fifth-from-start to the fifth-to-last
}
```

And here's the current index based version:
```java
//1. iterating using indices/ranges (we might still need them for some stuff)
for (int i = 0; i < 10; i++) {
    //...
}

//2. iterating over an array
float[] numbers = ...;
for (int i = 0; i < 10; i++) {
    float value = numbers[i];
    //...
}

//3. nesting
float[] even_numbers = ...;
for (int i = 0; i < 10; i++) {
    float num = numbers[i];
    for (int j = 0; i < 10; i++) {
        float even = even_numbers[i];
        // ...
    }
}

//4. complex data structures
BinaryTree huffman = ...;
for (Iterator iter = huffman.iterator(); iter.has_next(); iter.next()) {
    //...
}

//5. "slices"
for (int i = 5; i < numbers.length() - 5; i++) {
    //iterates over numbers from the fifth-from-start to the fifth-to-last
}
```

I think you can see that the iterator for loop is really really easy to solve.
The most common complaint I got was that the iterator for "is doing stuff in the background, hiding it away", but to me it's a nonsensical statement, a for loop is just a for loop, even when something is hidden it's still iterating over every element, so it's just redundant information.

If any language designer/maintainer is reading this and wants to implement these, it's now a good time to do so.
It's just *really* sad to know that we still don't have these into current languages. Like, how much time will it take to just have a good for loop? If we can't even do something as simple as this how can we expect to more complex stuff?
