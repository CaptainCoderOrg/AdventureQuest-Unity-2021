---
layout: page
title: "Chapter 5: Dice Group"
nav_order: 5
parent: "Part 2: Dice Analyzer"
---

# Chapter 5: Dice Group
{: .no_toc }

It isn't uncommon to roll a single die in a table top role playing game. For
example, in Dungeons & Dragons rolling a `d20` is one of the most common ways to
determine the success or failure of a character's action. However, it is so much
more fun (and satisfying) to roll a group of dice!

For example, a common way to create a character is to roll 3 six-sided dice
together. The sum of these 3 dice is the resulting ability score. When a person
does this, we say they rolled `3d6`.

In this chapter, we will create a `DieGroup` **class** to model a a group of
like-sided dice.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# 00. Create a Feature Branch

You're about to start a new feature! Before beginning, you should create a
feature branch named `{username}/dice-analyzer`.

{% include GitHub/NewBranch.md branch_name="username/dice-analyzer" %}


# 01. The DiceGroup Class

1. Create a `DiceGroup` **class** in your `Scripts/Model` folder.
2. The `DiceGroup` class should be in the `AdventureQuest.Dice` name space. 

When you're done, your `DiceGroup.cs` file should look match the code below:

```csharp
namespace AdventureQuest.Dice
{
    public class DiceGroup
    {

    }
}
```

The `DiceGroup` class does not need the `UnityEngine`, `System.Collections`,
or `System.Collections.Generic` name spaces.
{: .note }

# 02. Properties of a DiceGroup

Just as we did with the `Die`, let's first think about the properties that will
define a `DiceGroup`. 

Think about the following questions, then expand the section below.

For example, if you have 3 six-sided dice (`3d6`) what can be observed about
that group? What about `4d8`? Are there any interesting properties that can be
derived from the group that might be useful or interesting? Which should have
`set`ters?


Did you think through these questions? Did you write down your answers? You will
gain so much more from this project if you try to come up with your own
solutions first. 
{: .tip }

<details markdown="block">
<summary>
<h3 style="display:inline">DiceGroup Properties (Click to Expand)</h3>
</summary>

One of the best (and worst) parts of programming is that there are many
different ways to solve the same problem. This gives you room for creativity!
However, it also gives you room to cause yourself (and your team) an infinite
amount of pain. That said, if your proposed properties don't match those in this
project, that is okay! On a team, you would have the opportunity to discuss this
with your peers, learn, and grow! 

I won't try to claim that the **properties** I've chosen here are the best
possible set of **properties**. But, I have attempted to choose properties that
If you came up with something different (or don't like a choice I've made), I'd
love to hear about it (you can leave a comment at the bottom of this chapter).
Convince me why I should change them. Maybe I'll update the project! 

## Properties of a DiceGroup

Below are the **properties** of the `DiceGroup` **class** that I feel should be **exposed** publicly:

```csharp
/// <summary>
/// An array containing each <see cref="Die"/> in this <see cref="DiceGroup"/>
/// </summary>
private readonly Die[] _dice;



/// <summary>
/// The number of dice in this <see cref="DiceGroup"/>
/// </summary>
public int Amount { get; }
/// <summary>
/// The number of sides on each die in this <see cref="DiceGroup"/>.
/// </summary>
public int Sides { get; }
/// <summary>
/// The minimum value that can be rolled by this <see cref="DiceGroup"/>.
/// </summary>
public int Min { get; }
/// <summary>
/// The maximum value that can be rolled by this <see cref="DiceGroup"/>.
/// </summary>
public int Max { get; }
```

**Notice:** I have chosen **NOT** to provide any `set`ters for the `DiceGroup`.

Additionally, I have chose **NOT** to include a **property** which exposes
any individual `Die`. This was intentional. It could be argued that a `DiceGroup`
should have an array (or list) containing instances of `Die`. So, why did I choose
not to include such a **property**?

I decided that a `DiceGroup` should act as a "group" and should only be accessed
as a whole. That is, we don't want to expose the ability to roll an individual
die that is part of a `DiceGroup`. 

That said, I have chosen to create a `private readonly Die[]` **field** `_dice`
to track the internal state of the `DiceGroup`.
</details>

# 03. Derived Properties

Something you may have noticed is that the **properties** of a `DiceGroup` are 
related. For example, `Amount` and `Min` will return the same value. In many
ways, it is simply a convenience to have a **property** labeled `Min`. In `C#`
we can write a `get`ter as a derived value using the `=>` operator. For example,
you can replace:

```csharp
public int Min { get; }
```

with

```csharp
public int Min => Amount;
```

This is called an [Expression body definition] and is short hand for writing a
**method** which returns the value of `Amount`'s `get`ter. It is incredibly useful
for defining `get`ters that are derived from other **fields** and **properties**.

# Challenge: Derive each property

Can you derive each of the remaining **properties** using the `_dice` **field**,
another **property**, or a combination of **properties**?

<details markdown="block">
  <summary>Hint 1: Amount</summary>

  How many dice are in the `_dice` array?

  <details markdown="block">
  <summary><h3 style="display:inline">Solution</h3></summary>

You can determine the number of dice in the array using the `Length` property.

```csharp
public int Amount => _dice.Length;
```    
  </details>
</details>

<details markdown="block">
  <summary>Hint 2: Sides</summary>

Based on the definition of `DiceGroup` all of the dice in the `_dice` array should have the same number of sides.

  <details markdown="block">
  <summary><h3 style="display:inline">Solution</h3></summary>
You can access the the first element of the `_dice` array and return the number if `Sides` it has.

```csharp
public int Sides => _dice[0].Sides;
```    
  </details>
</details>

<details markdown="block">
  <summary>Hint 3: Max</summary>

  What is the maximum value of each die? How many dice are there?

  <details markdown="block">
  <summary><h3 style="display:inline">Solution</h3></summary>

`Sides` represents the maximum value an individual die can roll.
If you multiply this with `Amount`, you find the maximum possible
roll.

```csharp
public int Max => Amount * Sides;
```    
  </details>
</details>

# Challenge: Write a DiceGroup Constructor

Next, define a constructor for `DiceGroup`.

* What information is needed to construct a `DiceGroup`?

<details markdown="block">
  <summary>Hint 1: Parameters</summary>
  You can define a `DiceGroup` with two integers: `{amount}d{sides}`
</details>


<details markdown="block">
  <summary>Spoiler: Constructor Declaration and Comment</summary>
```csharp
/// <summary>
/// Instantiates a DiceSet containing <paramref name="amount"/> dice
/// each with the specified number of <paramref name="sides"/>.
/// </summary>
/// <exception cref="System.ArgumentException">
/// If amount is less than 1 or sides is less than 2.
/// </exception>
public DiceGroup(int amount, int sides)
{
    if (amount < 1) throw new System.ArgumentException($"DiceGroup must contain at least 1 die but was {amount}.");
    if (sides < 2) throw new System.ArgumentException($"DiceGroup must have at least 2 sides but was {sides}.");
    // TODO: Initialize _dice
    // TODO: Populate _dice
}
```
</details>

## DiceGroupTest

With a constructor defined, now would be a good time to add a `DiceGroupTest` to
your `Tests/Model` folder. Below are 2 tests that will help validate your solution
to the next challenge:

<details markdown="block">
  <summary><h3 style="display:inline">DiceGroupTest.cs (Click to Expand)</h3></summary>

```csharp
using NUnit.Framework;

namespace AdventureQuest.Dice
{
    public class DiceGroupTest
    {

        [Test, Timeout(5000), Description("Tests the DiceGroup(amoutn, sides) Constructor")]
        public void TestConstructor()
        {
            DiceGroup group3d6 = new(3, 6);
            Assert.AreEqual(3, group3d6.Amount);
            Assert.AreEqual(6, group3d6.Sides);
            Assert.AreEqual(3, group3d6.Min);
            Assert.AreEqual(18, group3d6.Max);

            DiceGroup group1d20 = new(1, 20);
            Assert.AreEqual(1, group1d20.Amount);
            Assert.AreEqual(20, group1d20.Sides);
            Assert.AreEqual(1, group1d20.Min);
            Assert.AreEqual(20, group1d20.Max);
        }

        [Test, Timeout(5000), Description("Tests the DiceGroup Constructor validates parameters")]
        public void TestConstructorArgumentException()
        {
            Assert.Throws<System.ArgumentException>(() => new DiceGroup(0, 6));
            Assert.Throws<System.ArgumentException>(() => new DiceGroup(-1, 6));
            Assert.Throws<System.ArgumentException>(() => new DiceGroup(3, 1));
            Assert.Throws<System.ArgumentException>(() => new DiceGroup(3, -1));
            Assert.Throws<System.ArgumentException>(() => new DiceGroup(1, 0));
        }

    }
}
```
</details>

## Challenge: Implement the DiceGroup Constructor

* How will you validate the arguments passed to the constructor? Which exceptions should be thrown?
* How will you use that information to initialize the `_dice` field?
* How will you use that to populate the `_dice` **field**?

Because all of the **properties** are derived using the `_dice` **field**
there is no need to initialize them in the constructor.
{: .note }


<details markdown="block">
  <summary>Hint 1: Validation</summary>
  A `DiceGroup` should have at least 1 `Die` and a `Die` must have at least 2 sides.
</details>


<details markdown="block">
  <summary>Hint 2: Initializing _dice</summary>
You should initialize the `_dice` **field** to be an array with enough space for
each `Die`.
</details>

<details markdown="block">
  <summary>Hint 3: Populating _dice</summary>
You must iterate `amount` times to populate the `_dice` array. Each time,
you will need to construct a `new Die(sides)` with the specified number of
sides.
</details>

<details markdown="block">
  <summary>Spoiler: Solution</summary>

```csharp
public DiceGroup(int amount, int sides)
{
    if (amount < 1) throw new System.ArgumentException($"DiceSet must contain at least 1 die but was {amount}.");
    if (sides < 2) throw new System.ArgumentException($"DiceSet must have at least 2 sides but was {sides}.");
    _dice = new Die[amount];
    for (int i = 0; i < amount; i++)
    {
        _dice[i] = new Die(sides);
    }
}
```
</details>

# Challenge: Add a Roll() method to the 

Just like the `Die` class the `DiceGroup` class will have a `Roll()` method
which rolls all of the dice in the group and returns the result. To do this, you
can write a loop which iterates over all of the dice and calls the
`_dice[i].Roll()` method. However, you will need to keep track of the each
resulting die and sum them together.

<details markdown="block">
  <summary><h3 style="display:inline">Roll() method declaration (Click to Expand)</h3></summary>

```csharp
/// <summary>
/// Rolls all of the dice and returns the sum.
/// </summary>
public int Roll()
{
    int sum = 0;
    // TODO: Calculate the sum
    return sum;
}  
```
</details>

<details markdown="block">
  <summary><h3 style="display:inline">Additional Tests (Click to Expand)</h3></summary>

You can test your solution using the tests below. Feel free to add additional test as well!

```csharp
        [Test, Timeout(5000), Description("Tests the result of rolling a 3d6 10,000 times.")]
        public void TestRoll3d6()
        {
            DiceGroup group3d6 = new(3, 6);

            // Roll the die pool 1000 times ensuring the bounds
            int[] values = new int[10_000];
            for (int i = 0; i < 10_000; i++)
            {
                int result = group3d6.Roll();
                Assert.LessOrEqual(result, group3d6.Max);
                Assert.GreaterOrEqual(result, group3d6.Min);
                values[i] = result;
            }

            // Result should contain all values from 3 to 18
            for (int i = group3d6.Min; i <= group3d6.Max; i++)
            {
                Assert.Contains(i, values);
            }
        }

        [Test, Timeout(5000), Description("Tests the result of rolling a 4d4 10,000 times.")]
        public void TestRoll2d4()
        {
            DiceGroup group2d4 = new(2, 4);

            // Roll the die pool 1000 times ensuring the bounds
            int[] values = new int[10_000];
            for (int i = 0; i < 10_000; i++)
            {
                int result = group2d4.Roll();
                Assert.LessOrEqual(result, group2d4.Max);
                Assert.GreaterOrEqual(result, group2d4.Min);
                values[i] = result;
            }

            // Result should contain all values from 3 to 18
            for (int i = group2d4.Min; i <= group2d4.Max; i++)
            {
                Assert.Contains(i, values);
            }
        }

    }
```
</details>

# Good Time to Commit

Now would be a good time to make a `git` commit. You just finished a feature.
More specifically, you just implemented a `DiceGroup` class which models a group
of like-sided dice.

{% include GitHub/CreateCommit.md %}

# What's Next

With a `DiceGroup` in place, we can simulate rolling a group of like-sided dice.
However, what if we would like to roll multiple `DiceGroup`s together? For
example, what if **The Sword of Fire and Ice** deals `2d4 + 2d6` damage? In the
[Chapter 6: Dice Pool], you will define a **class** `DicePool` which models a 
collection of `DiceGroup`s that are rolled together.

---
[Expression body definition]: https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties#expression-body-definitions

[Chapter 6: Dice Pool]: {% link pages/02-DiceAnalyzer/06-DicePool.md %}