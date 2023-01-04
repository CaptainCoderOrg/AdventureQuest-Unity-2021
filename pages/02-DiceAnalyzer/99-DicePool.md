---
layout: page
title: "Ch ??: Dice Pool"
nav_order: 99
parent: "Part 2: Dice Analyzer"
---

# Chapter ??: Dice Pool
{: .no_toc }

In [Chapter 5], you implemented a `DiceGroup` which allows you to simulate
rolling a group of like-sided dice. For example, `3d6`. However, what if you
would like to roll multiple `DiceGroup`s together? For example, what if **The
Sword of Fire and Ice** deals `2d4 + 2d6` damage? 

In this chapter, you will define a `DicePool` **class** which models a
collection of `DiceGroup`s that are rolled together.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# 01. The DicePool Class

1. Create a `DicePool` **class** in your `Scripts/Model` folder.
2. The `DicePool` class should be in the `AdventureQuest.Dice` name space.

When you're done, your `DicePool.cs` file should match the code below:

```csharp
namespace AdventureQuest.Dice
{
    public class DicePool
    {
        
    }
}
```

The `DiceGroup` class does not need the `UnityEngine`, `System.Collections`,
or `System.Collections.Generic` name spaces.
{: .note }

# 02. Properties of a DicePool

Just as you did with the `DiceGroup`, start by thinking about the properties that
will define a `DicePool`. 

Think about the following questions, then expand the section below.

What are the important observable features you would like to **expose** as
**properties**? What **fields** will you need that will be part of your
**implementation** that are **encapsulated** using the **private** modifier?


Did you think through these questions? Did you write down your answers? You will
gain so much more from this project if you try to come up with your own
solutions first. 
{: .tip }

<details markdown="block">
<summary>
<h3 style="display:inline">DicePool Properties (Click to Expand)</h3>
</summary>

Just as with the `DiceGroup`, there are many ways to implement a `DicePool`. 
If you chose a different set of **properties**, I'd love to hear about them.

I won't try to claim that the **properties** I've chosen here are the best
possible set of **properties**. But, I have attempted to choose properties that
**expose** only the necessary pieces for analyzing the possible outcomes of
rolling a `DicePool`.

```csharp
private readonly DiceGroup[] _dice;

/// <summary>
/// The minimum value that can be rolled by this <see cref="DicePool"/>.
/// </summary>
public int Min { get; }

/// <summary>
/// The maximum value that can be rolled by this <see cref="DicePool"/>.
/// </summary>
public int Max { get; }

/// <summary>
/// A formatted string representing this <see cref="DicePool"/>. For example,
/// "2d6 + 1d4 + 3d10".
/// </summary>
public string DiceFormat { get; }
```

I feel there is a really good argument that could be made to include a
**property** that exposes the underlying `DiceGroup[]`. However, this would
allow the individual `DiceGroup`s to be rolled. Just as with the `DiceGroup`, I
decided that a `DicePool` should act as a single "pool" and should only be
accessed as a whole. However, to be able to examine what each internal
`DiceGroup` looks like, I've included a `string` **property** `DiceFormat`.

Additionally, I have chosen to create a `private readonly DiceGroup[]` **field**
`_dice` to track the internal state of the `DicePool`.

</details>

# 03. DicePool Constructor

Just as with the `DiceGroup`, you can derive each of the **properties** of
`DicePool` using the `_dice` **field**. However, to make it easier to test,
let's start by implementing a **constructor**.

A `DicePool` needs to contain at least 1 `DiceGroup` but may contain any
number of `DiceGroup`s. One way you might accomplish this is to write
the following constructor:

```csharp
/// <summary>
/// Instantiates a <see cref="DicePool"/> with the specified <paramref name="diceGroups"/>.
/// </summary>
/// <exception cref="System.ArgumentNullException">If <paramref name="diceGroups"/> is null.</exception>
/// <exception cref="System.ArgumentException">If <paramref name="diceGroups"/> has fewer than 1 element.</exception>
/// 
public DicePool(DiceGroup[] diceGroups)
{
    if (diceGroups == null) throw new System.ArgumentNullException($"DicePool must contain at least 1 dice set.");
    if (diceGroups.Length < 1) throw new System.ArgumentException($"DicePool must have at least 1 dice set.");
    _dice = diceGroups;
}
```

However, there is a flaw with this logic that may not be obvious. Below I've
provided a situation that will compile and run but has a subtle bug. Can you
spot it?

# Challenge: Spot the Bug

```csharp
DiceGroup[] group = new DiceGroup[3];
group[0] = new DiceGroup(3, 6);
group[1] = new DiceGroup(2, 4);
DicePool oops = new DicePool(group);
```

<details markdown="block">
<summary>Think about it. Can you spot the bug? (Click to Expand)</summary>

If you were to examine the contents of `DiceGroup`, you would find that
`group[2]` is storing the **default** value of `null`. This will almost
certainly rear its ugly head when you attempt to use the `DicePool`.

```csharp
DiceGroup[] group = new DiceGroup[3]; // <-- This array has 3 elements
group[0] = new DiceGroup(3, 6);
group[1] = new DiceGroup(2, 4);
// group[2] = null <-- The 2nd index is never set but is added to the DicePool
DicePool oops = new DicePool(group);
```
</details>

# Challenge: Fix the bug

Unfortunately, the **constructor** is currently more than willing to let the
`DiceGroup[]` argument contain `null` values. Ideally, you would like to catch
this problem during construction and [Fail Fast]. To do this, you should iterate
through the incoming array and check if any of the elements are `null`. If they
are, you should throw a `System.NullReferenceException`.

* Add the constructor below to your `DicePool` class and complete the TODO.

<details markdown="block">
<summary>Constructor Declaration with TODOs (Click to Expand)</summary>

```csharp
/// <summary>
/// Instantiates a <see cref="DicePool"/> with the specified <paramref name="diceGroups"/>.
/// </summary>
/// <exception cref="System.ArgumentNullException">If <paramref name="diceGroups"/> is null.</exception>
/// <exception cref="System.ArgumentException">If <paramref name="diceGroups"/> has fewer than 1 element.</exception>
/// <exception cref="System.NullReferenceException">If any of the elements in <paramref name="diceGroups"/> are null.</exception>
public DicePool(DiceGroup[] diceGroups)
{
    if (diceGroups == null) throw new System.ArgumentNullException($"DicePool must contain at least 1 dice set.");
    if (diceGroups.Length < 1) throw new System.ArgumentException($"DicePool must have at least 1 dice set.");
    // TODO: Validate that all of the DiceGroups are non-null
    _dice = diceGroups;
}
```
</details>

To help you test your solution, you should add the following `DicePoolTest.cs`
to your `Tests/Model` folder.

<details markdown="block">
<summary>DicePoolTest.cs (Click to Expand)</summary>

```csharp
using NUnit.Framework;

namespace AdventureQuest.Dice
{

    public class DicePoolTest
    {

        [Test, Timeout(5000), Description("Tests that the Constructor doesn't allow a null array.")]
        public void ConstructorFailsOnNullArray()
        {
            Assert.Throws<System.ArgumentNullException>(() => new DicePool(null));
        }

        [Test, Timeout(5000), Description("Tests that the Constructor doesn't allow an empty array.")]
        public void ConstructorFailsOnEmptyArray()
        {
            DiceGroup[] emptyArray = {};
            Assert.Throws<System.ArgumentException>(() => new DicePool(emptyArray));
        }

        [Test, Timeout(5000), Description("Tests that the Constructor doesn't allow any null DiceGroups.")]
        public void ConstructorFailsOnNullDiceGroup()
        {
            DiceGroup[] arrayWithNull = { new DiceGroup(3, 6), null, new DiceGroup(2, 4) };
            Assert.Throws<System.NullReferenceException>(() => new DicePool(arrayWithNull));

            DiceGroup[] arrayWithNull2 = { new DiceGroup(3, 6), new DiceGroup(2, 4), new DiceGroup(1, 20), null };
            Assert.Throws<System.NullReferenceException>(() => new DicePool(arrayWithNull2));

            DiceGroup[] arrayWithNull3 = { null, new DiceGroup(2, 4) };
            Assert.Throws<System.NullReferenceException>(() => new DicePool(arrayWithNull3));
        }
    }
}
```
</details>

<details markdown="block">
<summary>Hint (Click to Expand)</summary>

1. Write a for loop that accesses each element of the array.
2. Check `if (diceGroup[i] == null) { }`
3. If it is `null`, throw the appropriate exception.

</details>

# 04. Derive Min/Max Properties

As previously stated, it is possible to derive each of the **properties** of
`DicePool` using the `_dice` **field** without using a `set`ter. However, it is
not as simple as using a [Expression body definition] (`=>`). Instead, you
can specify a **body** of code to execute when the `get`ter is accessed:

```csharp
/// <summary>
/// The minimum value that can be rolled by this <see cref="DicePool"/>.
/// </summary>
public int Min
{
    get
    {
        int min = 0;
        foreach (DiceGroup group in _dice)
        {
            min += group.Min;
        }
        return min;
    }
}
```

The code above uses a `foreach` loop to **iterate** through each `DiceGroup` in
`_dice` summing their `Min` values. Finally, the sum of the minimums is
returned.

You are not required to provide a `set`ter. By doing this, you ensure the `Min`
is **immutable**. That is, the value of `Min` cannot be changed. In general, you
should favor **immutable** values as it reduces the complexity of the program.
{: .note }

# Challenge: Implement the Max property

Can you implement the `Max` **property** using a `get`ter?

<details markdown="block">
<summary>Additional Test (Click to Expand)</summary>

The test below will help give you confidence that your `Min` and `Max`
**properties** are working.

```csharp
[Test, Timeout(5000), Description("Tests the Min and Max properties")]
public void TestMinMax()
{
    
    DicePool pool1d20 = new (new DiceGroup[]{new DiceGroup(1, 20)});
    Assert.AreEqual(1, pool1d20.Min);
    Assert.AreEqual(20, pool1d20.Max);

    DicePool pool2d41d6 = new (new DiceGroup[]{new DiceGroup(2, 4), new DiceGroup(1, 6)});
    Assert.AreEqual(3, pool2d41d6.Min);
    Assert.AreEqual(14, pool2d41d6.Max);

    DicePool pool1d61d41d8 = new (new DiceGroup[]{new DiceGroup(1, 6), new DiceGroup(2, 4), new DiceGroup(1, 8)});
    Assert.AreEqual(4, pool1d61d41d8.Min);
    Assert.AreEqual(22, pool1d61d41d8.Max);
}
```
</details>

# 05. Derive DiceFormat

The `DiceFormat` **property** is a little tricky because you need to accomplish two
things:

1. Transform each `DiceGroup` to a `string` (e.g. `3d6`)
2. Put a `" + "` between each of those strings.

Below is a **helper method** `DiceGroupStrings()` that accomplishes part 1 by
iterating over each `DiceGroup` in `_dice`.

* Add the `DiceGroupStrings()` **method** to the `DicePool` class:

<details markdown="block">
<summary>DiceGroupStrings() Declaration (Click to Expand)</summary>

```csharp
private string[] DiceGroupStrings()
{
    string[] groups = new string[_dice.Length];
    for (int i = 0; i < _dice.Length; i++)
    {
        DiceGroup group = _dice[i];
        groups[i] = $"{group.Amount}d{group.Sides}";
    }
    return groups;
}
```

The method above creates a new `string[] groups` that is the same length as `_dice`.
Next, it populates `groups` with the correct `"{amount}d{sides}"` string.
Finally, it returns the array of strings.
</details>


# Challenge: Implement DiceFormat

With the `DiceGroupStrings()` **helper method** in place, it is possible to
derive the `DiceFormat` **property** using an [Expression body definition]
(`=>`). The trick is to utilize the `string.Join` **method**. 

Can you implement the `DiceFormat` **property**?

The best developers in the world RTFM (read the friendly manual)! It is full
of helpful methods and classes that have been tested and are designed to help
manage your programs complexity! To complete the next challenge, I highly
recommend reading about the [string.Join] method.
{: .tip }

Add the test below to your `DicePoolTest` to give yourself confidence that your
solution works.

<details markdown="block">
<summary>Additional Test (Click to Expand)</summary>

```csharp
[Test, Timeout(5000), Description("Tests the DiceFormat property")]
public void TestDiceFormat()
{
    DicePool pool1d20 = new (new DiceGroup[]{new DiceGroup(1, 20)});
    Assert.AreEqual("1d20", pool1d20.DiceFormat);

    DicePool pool2d41d6 = new (new DiceGroup[]{new DiceGroup(2, 4), new DiceGroup(1, 6)});
    Assert.AreEqual("2d4 + 1d6", pool2d41d6.DiceFormat);

    DicePool pool1d62d41d8 = new (new DiceGroup[]{new DiceGroup(1, 6), new DiceGroup(2, 4), new DiceGroup(1, 8)});
    Assert.AreEqual("1d6 + 2d4 + 1d8", pool1d62d41d8.DiceFormat);
}
```
</summary>

<details markdown="block">
<summary>Hint (Click to Expand)</summary>
You should use the separator " + " and the array returned by `DiceGroupStrings()`.
</details>

<details markdown="block">
<summary>Solution (Click to Expand)</summary>

```csharp
public string DiceFormat => string.Join(" + ", DiceGroupStrings());
```
</details>


# Challenge: Another Constructor Bug

Add the following test to your `DicePoolTest` **class** but don't run it yet.

<details markdown="block">
<summary>Additional Test (Click to Expand)</summary>

```csharp
[Test, Timeout(5000), Description("Tests that DiceGroup is not mutable")]
public void TestDiceGroupImmutable()
{
    DiceGroup[] group = new DiceGroup[2];
    group[0] = new DiceGroup(3, 6);
    group[1] = new DiceGroup(2, 4);
    DicePool pool3d6plus2d4 = new (group);

    group[0] = new DiceGroup(1, 8);
    group[1] = new DiceGroup(1, 10);
    DicePool pool1d8plus1d10 = new (group);

    Assert.AreEqual("3d6 + 2d4", pool3d6plus2d4.DiceFormat);
    Assert.AreEqual("1d8 + 1d10", pool1d8plus1d10.DiceFormat);
}
```
</details>

Just as before, the code above will compile and run. However, there is a logical
bug that occurs during the above test. Can you spot the bug?

<details markdown="block">
<summary>Think about it. Can you spot the bug? (Click to Expand)</summary>

The code successfully initializes two `DicePool`s. However, when you assign
`_dice = diceGroup;` in the constructor, it is referencing the array `group`
that was declared outside of the **class**. Then, the following code runs:

```csharp
group[0] = new DiceGroup(1, 8);
group[1] = new DiceGroup(1, 10);
```

Because `_dice` is referencing the same array as `group`, the values within the
first `DicePool` are modified! Ooops!

This bug could be incredibly annoying and tricky to track down because it
doesn't cause any exceptions to occur. This is one reason you should favor
**immutable** data. The only reason this bug is possible is because the values
of `group` can be modified.

 Imagine your player has the **ULTIMATE** weapon which should deal `3d20 + 6d6`
but it is accidentally changed to `1d4 + 1d6` damage! The game wouldn't crash
when the weapon is used. Instead, it would result in a pitiful damage result.

</details>

# Challenge: Fix the bug

To fix this bug, you need to that modifying the array that was passed to
the **constructor** will not modify the `_dice` array. This can be accomplished
by initializing a `new` array and copying the values.

Can you update the constructor such that the `_dice` **field** is initialized to
a new array containing a copy of each value?

<details markdown="block">
<summary>Hint (Click to Expand)</summary>

Complete the **TODOs** in this **constructor** template:

```csharp
public DicePool(DiceGroup[] diceGroups)
{
    if (diceGroups == null) throw new System.ArgumentNullException($"DicePool must contain at least 1 dice set.");
    if (diceGroups.Length < 1) throw new System.ArgumentException($"DicePool must have at least 1 dice set.");
    // TODO: Initialize _dice to be the same length as diceGroup
    for (int i = 0; i < diceGroups.Length; i++)
    {
        if (diceGroups[i] == null) { throw new System.NullReferenceException($"DicePool cannot be initialized with null DiceGroup."); }
        // TODO: Copy the appropriate element to _dice
    }
}
```
</details>

# Challenge: Implement the Roll() method

Finally, you are ready to implement the **DicePool.Roll()** method which sums
the result of rolling each `DiceGroup`.

<details markdown="block">
<summary>Roll() Declaration (Click to Expand)</summary>

```csharp
// <summary>
/// Rolls all of the dice and returns the sum.
/// </summary>
public int Roll()
{
    int sum = 0;
    return sum;
}
```
</details>

<details markdown="block">
<summary>Additional Tests (Click to Expand)</summary>

```csharp
[Test, Timeout(5000), Description("Tests the result of rolling a 1d6 + 1d4 + 1d8 50,000 times.")]
public void TestRoll1d61d41d8()
{
    DicePool pool = new ( new []{new DiceGroup(1, 6), new DiceGroup(1, 4), new DiceGroup(1, 8)});

    // Roll the die pool 1000 times ensuring the bounds
    int[] values = new int[50_000];
    for (int i = 0; i < 50_000; i++)
    {
        int result = pool.Roll();
        Assert.LessOrEqual(result, pool.Max);
        Assert.GreaterOrEqual(result, pool.Min);
        values[i] = result;
    }

    // Result should contain all values from 3 to 18
    for (int i = pool.Min; i <= pool.Max; i++)
    {
        Assert.Contains(i, values);
    }
}

[Test, Timeout(5000), Description("Tests the result of rolling a 2d4 + 1d6 50,000 times.")]
public void TestRoll2d41d20()
{
    DicePool pool = new ( new []{new DiceGroup(2, 4), new DiceGroup(1, 20)});

    // Roll the die pool 1000 times ensuring the bounds
    int[] values = new int[50_000];
    for (int i = 0; i < 50_000; i++)
    {
        int result = pool.Roll();
        Assert.LessOrEqual(result, pool.Max);
        Assert.GreaterOrEqual(result, pool.Min);
        values[i] = result;
    }

    // Result should contain all values from 3 to 28
    for (int i = pool.Min; i <= pool.Max; i++)
    {
        Assert.Contains(i, values);
    }
}
```
</details>

# Good Time to Commit

If you have not already done so, now would be a good time to make a commit. You just
finished a feature. More specifically, you implemented a `DicePool` **class** which
models rolling one or more `DiceGroup`s together.

{% include GitHub/CreateCommit.md %}

# What's Next

Whew! That was a lot of work! In [Chapter 7: DicePool Roller], you will
implement a **Dice Analyzer Scene** that allows the user enter a string
representing a `DicePool` and roll simulate rolls. This will be your first step
toward analyzing the possible outcomes of various dice pools.

---
[Chapter 5]: {% link pages/02-DiceAnalyzer/05-DiceGroup.md %}
[Chapter 5: DicePool Roller]:
[Fail Fast]: https://en.wikipedia.org/wiki/Fail-fast
[string.Join]: https://learn.microsoft.com/en-us/dotnet/api/system.string.join?view=net-7.0#system-string-join(system-string-system-string())
[Expression body definition]: https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties#expression-body-definitions