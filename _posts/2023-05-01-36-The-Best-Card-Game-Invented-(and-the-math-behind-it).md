---
layout: post
title: "36: The Best Card Game Invented (and the math behind it)"
categories: [Math, Computing Science]
---

## Introducing 24: The Predecessor

There is this card game called "24" that evolved out of Shanghai. It is incredibly simple and is endlessly fun (I have played hours and hours of 24). To play, you remove the face cards from a regular deck of cards and deal four cards face up. With these four cards, you must make the number "24" using only the four basic operations, addition, subtraction, multiplication, and division. However, you must use every card and you can only use each card once. You can play by yourself, one-on-one, or as a party game with an unlimited amount of players. The first person to find a way of making 24 gets a point and you deal four new cards.

***
### Example
**Cards Dealt:** 4, 8, 3, 2

For example, if we were dealt four random cards like above, we can make 24 by (4 + 2 - 3) * 8.

***

## The Problem With 24

The problem with 24 is that all of my friends *hate* it. Some of them actively get angry at the mention of the game. It is not a passive or understated disdain, they loathe it with **zeal**. 


## 36: The New and Exciting Card Game

In order to entice my friends to play with me again, I invented a new card game: 36 !

Unlike 24, the players try to make 36 out of the cards dealt. And you deal **five** cards instead of **four** per hand. One caveat of this is that you must have two decks of cards (with the face cards removed), as there are only four of each number in a standard deck (you have two decks so that you can deal five of a kind).

***
### Example

**Cards Dealt:** 3, 3, 3, 3, 3

In this example, we can make 36 by: 3 \* 3 \* 3 + 3 \* 3

***

## The Math Behind 36

If you were just interested in the card game, you can stop reading here.

A lot of people at this point wonder: "Can you make 24 (or 36) from every possible hand dealt?" It might not come as a surprise, but the answer is no. You need not look further than the hand [1, 1, 1, 1], you can't make a number big enough to approach 24. 

One interesting fact though, is that with 36, a higher percentage of hands dealt are viable compared to 24. The data is summarized in the following tables:


Table 1. The number of viable hands over the number of total possible hands, with the decimal representation in brackets. The columns represent the number of cards dealt and the rows represent the target number. Data is for standard deck of cards with the face cards removed.  

|    	| 4              	| 5                	| 6                	|
|----	|----------------	|------------------	|------------------	|
| 24 	|                	|                  	|                  	|
| 36 	| 457/715 (0.64) 	| 1822/2002 (0.91) 	| 4889/5005 (0.98) 	|
| 48 	| 410/715 (0.57) 	| 1768/2002 (0.88) 	| 4824/5005 (0.96) 	|
| 60 	| 299/715 (0.42) 	| 1643/2002 (0.82) 	| 4719/5005 (0.94) 	|

As an example of reading from the table, we can see that if we were to deal five cards with the aim of making 36, there are 2002 possible hands, 1822 of which can make 36. This is 91% of the possible hands. 

From the table above, we can already see that 36 is inherently more fun than its younger cousin because you spend less time staring at duds. Personally, I have never played a game with 6 cards dealt, so I will refrain from commenting on the matter.

I should also note that some people play 24 with the face cards removed AND the ten card removed. For those counting at home, this means a hand can only contain the numbers one through nine. The statistics of this are noted in the following table:

Table 2. The number of viable hands over the number of total possible hands, with the decimal representation in brackets. The columns represent the number of cards dealt and the rows represent the target number. Data is for standard deck of cards with the face cards **and the ten cards** removed.

|    | 4              | 5                | 6                 |
|----|----------------|------------------|-------------------|
| 24 |                |                  |                   |
| 36 | 375/495 (0.76) | 1254/1287 (0.97) | 2994/3003 (0.997) |
| 48 | 341/495 (0.69) | 1229/1287 (0.95) | 2982/3003 (0.993) |
| 60 | 276/495 (0.56) | 1194/1287 (0.93) | 2964/3003 (0.987) |

We can see that across the board removing the ten card makes more hands viable. For this reason, when I play 36 I usually play with the face cards *and* the ten cards removed.


## How I Obtained These Results

In short: I wrote C# code to brute-force every possible operations on a hand, for every possible permutation of hands.

(Side Note: I am not a C# programmer; I just wanted to try something different and I did not want to write brute-force code in Python. With that being said, I hated it. Never doing it again. My code is incredibly messy, so I will only show snippets and I can offer the compiled executable if requested. It's too embarassing otherwise.)

### Choosing The Targets
The original goal was to create a new card game similar to 24, but not 24, as my friends loathe 24. 

To begin, I needed to choose "targets." These are the numbers we aim to make with the cards dealt. In order to choose targets, I use a heuristic: I assume that any variant of 24 should target a **highly composite number,** a positive integer that has more divisors than any smaller positive integer. That is to say, we want a number that has more factors than any number before it. For reference, the first few HCN are 1, 2, 4, 6, 12, 24, 36, 48, 60, 120, 180. From this sequence, I elected to test 36, 48, and 60.

### Generating the Hands

With the targets in mind, we now need to generate every possible hand that can be dealt from a deck of cards. This is harder than it first sounds. We want to avoid duplicates at all cost, as if we just lazily take combinations without any considerations for duplicates, the order of the problem grows really fast. For reference, if we took 50 cards (say 5 cards of each number from 1 to 10) and chose 5 (dealing 5 cards at a time), then we say have ~2e6 possible hands. However, from Table 1., we can see that there are only 1287 possible hands when dealing 5 cards. This is a magnitude difference of 1e3. And we haven't even begun to try all possible operations on each hand between every number in the hand (i.e. checking if the hand is viable).

Luckily for us, this process of dealing cards has a name, Combinations with Repetition. And someone on the internet has already coded it:

Code Block 1. C# code for selecting *k* items from an array (adapted from [stackexchange](https://stackoverflow.com/questions/1952153/what-is-the-best-way-to-find-all-combinations-of-items-in-an-array)) 
```cs
static IEnumerable<IEnumerable<T>> GetKCombsWithRept<T>(IEnumerable<T> list, int length) where T : IComparable
    {
        if (length == 1) return list.Select(t => new T[] { t });
        return GetKCombsWithRept(list, length - 1)
            .SelectMany(t => list.Where(o => o.CompareTo(t.Last()) >= 0),
                (t1, t2) => t1.Concat(new T[] { t2 }));
    }
```

### Checking Each Hand

Now that we have every possible hand, we need to check if the hand can be made into the target. Again, this process is more difficult than it first seems. The problem is that there are an infinite number of parenthesis placements.

***
### Example

If you deal five cards: 1, 2, 3, 4, 5.

You can place the parenthesis in an infinite number of ways:

1 + (2 + 3) + 4 + 5

1 + ((2 + 3)) + 4 + 5

1 + (((2 + 3))) + 4 + 5

...

Obviously, these are all equivalent statements, but the problem remains
***

Luckily, there is a clever trick! We can avoid parenthesis all together by using Reverse Polish Notation (RPN). We only need to consider every possible (valid) permutations of RPN orderings. 

***
### Example

If we were to deal four cards, then all possible RPN expressions are
NNONONO,
NNNOONO,
NNONNOO,
NNNONOO,
NNNNOOO, where N indicates a number and O indicates an operation. This represents all possible ways we can operate on 4 numbers. 

E.g. we can write 1 + 2 + 3 + 4 + 5 as 1 2 + 3 + 4 + 5 +, which is in the form NNONONO. 

***

To generate every possible RPN expressions, there is another clever trick. RPN expressions are one-to-one with post-order traversals of binary trees. So we simply generate every possible binary tree with nodes representing operators and leaves representing numbers. We then post-order traverse each tree to form a RPN expression.

Code Block 2. C# code to evaluate an RPN expression (adapted from math.bas.bg/bantchev/place/rpn/rpn.c%23.html)
```cs
 public static double evalrpn(Stack<string> tks)
    {
        string tk = tks.Pop();
        double x, y;
        if (!Double.TryParse(tk, out x))
        {
            y = evalrpn(tks); x = evalrpn(tks);
            if (tk == "+") x += y;
            else if (tk == "-") x -= y;
            else if (tk == "*") x *= y;
            else if (tk == "/") x /= y;
            else throw new Exception();
        }
        return x;
    }
```

We can then brute-force each hand dealt by looping over every RPN expression with replacing *N* with the numbers in our hand, and *O* by +, -, / or *. This is more combinatorics of course, but it isn't very hard to do.

## Conclusion

This project was a fun afternoon...but it turns out my friends also hate 36. I'm not too sad, as 36 is also fun to play alone. It's more fun than 24.

Anyways, hopefully my writeup was clear and not too confusing. Feel free to email me if you want the code or just to clear something up.

Kenny