---
layout: post
title:  "Catch Fish problem from SPOJ: MFISH"
date:   2016-01-23 16:04:33
categories: spoj dynamic_programming
tags:
- jekyll
- code
---

Analysis and solution of the problem: http://www.spoj.com/problems/MFISH/

<!--more-->

## Table of contents

* <a href="#description-of-the-problem"> Description of the problem </a>
* <a href="#understanding-the-problem"> Understanding the problem </a>
* <a href="#modelling-the-problem"> Modelling the problem </a>
* <a href="#brute-force-solution"> Brute Force solution </a>
* <a href="#dynamic-programming-solution-with-o-n-m-complexity"> Dynamic Programming solution with O(N * M) complexity </a>
* <a href="#dynamic-programming-solution-with-o-n-complexity"> Dynamic Programming solution with O(N) complexity </a>
  * <a href="#additional-analysis-of-the-problem"> Additional analysis of the problem </a>
  * <a href="#o-n-algorithm"> O(N) algorithm </a>
* <a href="#other-optimizations"> Other optimizations </a>
  * <a href="#counting-boat-coverage-with-o-1-complexity"> Counting boat coverage with O(1) complexity </a>
  * <a href="#sorting-boats-with-o-n-complexity"> Sorting boats with O(N) complexity </a>

## [Description of the problem](#description-of-the-problem)

> During his childhood, Mirko liked to play “Sea battle” a lot, but once he got bored of it and invented a new game imaginatively called “Ships catch fish on a river”.
>
> The game board consists of N fields lined up in a row and numbered 1 to N in ascending order from left to right. These fields represent a river on which M ships are to be placed. For each field the amount of fish swimming in that part of the river is known. Every ship has a specific length, i.e. it occupies the specific number of consecutive fields. A ship must somewhere drop an anchor but is allowed to do that in a certain field only. That means that one of the fields a ship will occupy is predetermined.
> 
> There can be only one ship or a part of only one ship on a single field of the board. We define the total amount of fish caught as the sum of amounts of fish swimming in all fields occupied by ships. Goal of the game is to place all ships on the river in a way that the total amount of fish caught is maximal.
>
> While playing an instance of this game, Mirko is in doubt whether the way he placed ships is optimal. Therefore asked you to help him calculate the maximum possible amount of fish caught.
>

### Input  

> First line of the input file contains an integer N, the number of fields on board, 1 ≤ N ≤ 100000.
> 
> In the next line there are N integers, separated by whitespaces, which represent the amounts of fish swimming in appropriate field, given in kilograms. These numbers will not be less than 1 nor greater than 100.
>
> The next line contains an integer M, number of the ships, 1 ≤ M ≤ N.
>
> Each of the following M lines consists of two integers B and D separated by a whitespace. That means that the appropriate ship should drop the anchor in a field numbered with B and that its length is D.
> 
> Note: input data will always be such that the solution will exist
>

### Output

>
> The only line of the output file should contain the maximum possible total amount of fish caught.
>

### Sample

>
> #### Input
>> 11  
>> 2 5 3 4 7 6 2 1 3 8 5  
>> 2  
>> 8 3  
>> 3 2  
>
> #### Output
>> 20 
>
> #### Input
>> 13   
>> 3 2 4 7 2 1 3 6 1 2 6 4 1   
>> 2  
>> 5 7   
>> 11 4  
>
> #### Output  
>> 38  
>
> #### Input
>> 11  
>> 1 1 6 4 4 1 1 3 10 1 1  
>> 3  
>> 2 3  
>> 6 4  
>> 10 2
>
> #### Output
>> 31

## [Understanding the problem](#understanding-the-problem)

Below provided a graphical representation of the problem's description:

![Illustration to the problem](/images/spoj/mfish/river.png)

Each boat has to put its anchor, strictly, into its own position. Positions of anchors are fixed - which means, that at least one segment of every boat has to be located in segment of the river with a boat's anchor. 

Apart from that - it is possible to vary alignment of the boats (boats are not allowed to overlap). Each alignment can be characterized by total coverage of fish. 

So, we have to find the largest possible coverage of fish (with respect to predefined anchor positions, and lengths of the boats). Below presented few possible alignments of the boats:

![Example of alignments](/images/spoj/mfish/alignments_example.png)

## [Modelling the problem](#modelling-the-problem)

The river can be modelled just by simple array `int[] fish` - where each cell represents amount of fish in corresponding location (according to problem's description, indexing of positions starts from 1, in contradiction to commonly used 0-based indexing in arrays - so we have to be careful).

For convenience, let's define class, which represents boat:

{% highlight java %}
class Boat {
    int anchor;
    int length;

    // let's introduce a variable, which represents
    // the leftmost allowed position of the boat
    int minPos;

    Boat(int anchor, int length) {
        this.anchor = anchor;
        this.length = length;
        this.minPos = (anchor - length) + 1;
        if (this.minPos < 1) {
            this.minPos = 1;
        }
    }
}
{% endhighlight %}

So, we have to develop some function, which returns the largest possible coverage for given array of boats and array of fish amounts:

{% highlight java %}
static int solve(int[] fish, Boat[] boats) {
    // ...
    // Our algorithm will be here
    // ...
    return result;
}
{% endhighlight %}

As far, as description of the problem does not tell anything about the order of the boats inside input data - let's sort the boats by location of anchors:

{% highlight java %}
static int solve(int[] fish, Boat[] boats) {
    Arrays.sort(boats, (b1, b2) -> Integer.compare(b1.anchor, b2.anchor));
    // ...
    // Our algorithm will be here
    // ...
    return result;
}
{% endhighlight %}

After analysis of restrictions of the problem (boats can't overlap, and must be connected with their anchors), we can notice an **important insight about the problem: (after sorting boats by anchor location) - the precedence order of the boats will be fixed for any possible alignment of boats.**

## [Brute force solution](#brute-force-solution)

Let's look closer into the instance of our problem, and try to trace the possible solution of the problem:
- imagine, that we put the `1st boat` into some *feasible position* `x` (such that distance to its anchor is smaller than length of the boat: `(x + boats[1].length) >= boats[1].anchor`)
- now, we must put `2nd boat` into its *feasible position* `y` - and, with respect to restrictions of our problem (boats can't overlap), we have to choose such `y`, that `y > (x + boats[1].length)`
- now, we have to put `3rd boat` into its *feasible position* `z`, such that   
`z > (y + boats[2].length)`
- and so on...

So, we can see that steps of our solution are very similar, and we just have to:
- track the index of the current boat: `b`
- track the first feasible position for curent boat on the river: `s` (such position, which doesn't overlap with previous boat)

Based on the observations from above - we can represent the solution of the instance of problem, through solutions of smaller subproblems:

![Recurrence relation](/images/spoj/mfish/recurrence_relation.png)

Let's simplify our recurrence relation a bit. To do this, let's consider two possible ways of implementing the function, which finds the maximum value inside array:

Iterative:
{% highlight java %}
static int max(int[] arr) {
    int result = Integer.MIN_VALUE;
    for(int i = 0; i < arr.length; i++) {
        result = Math.max(result, arr[i]);
    }
    return result;
}
{% endhighlight %}

Recursive:
{% highlight java %}
static int max(int[] arr) {
    return max(arr, 0);
}

static int max(int[] arr, int i) {
    if(i == arr.length) return Integer.MIN_VALUE;
    return Math.max(arr[i], max(arr, i + 1));
}
{% endhighlight %}

Recursive implementation allows to get rid of explicit loop routine, hence the same approach can be used for simplification of our recurrence relation.
So, the following expressions are equivalent:
![Loopless recurrence formula](/images/spoj/mfish/equivalent_max.png)

So, our final recurrence relation is following:
![Recurrence relation](/images/spoj/mfish/recurrence_relation_2.png)

Below presented recursive solution, which is based on recurrence relation from above:

{% highlight java linenos=table %}
import java.util.Arrays;

public class MFISH {

	static int solve(int[] fish, Boat[] boats) {
		Arrays.sort(boats, 
                    (b1, b2) -> Integer.compare(b1.anchor, b2.anchor));
                    
		int result = solve(boats, fish, 1, 0);
		return result;
	}

	static final int NEGATIVE_INFINITY = -100000000;

	static int solve(Boat[] boats, int[] fish, int start, int boat) {

		if (boat == boats.length) {
			// all boats successfully aligned
			return 0;
		}

		if (start > boats[boat].anchor) {
			// anchor of the boat is unreachable
			// from given start position
			return NEGATIVE_INFINITY;
		}

		if (((fish.length - start) + 1) < boats[boat].length) {
			// boat can't fit into the river 
            // (intersects the right boundary of the fish array)
			return NEGATIVE_INFINITY;
		}

		if ((start + boats[boat].length) <= boats[boat].anchor) {
			// given start position is too far from
			// anchor of current boat
			// so, the start position has to be closer
			return solve(boats, fish, start + 1, boat);
		}

		int subProblem1 = solve(boats, fish, start + 1, boat);

		int coverage = 
                countFish(fish, start, (start + boats[boat].length) - 1);
                
		int subProblem2 = coverage +
                solve(boats, fish, start + boats[boat].length, boat + 1);

		return Math.max(subProblem1, subProblem2);
	}

	static int countFish(int[] fish, int from, int to) {
		int coverage = 0;
		for (int i = from - 1; i <= (to - 1); i++) {
			coverage += fish[i];
		}
		return coverage;
	}
}
{% endhighlight %}

## [Dynamic Programming solution with O(N * M) complexity](#dynamic-programming-solution-with-o-n-m-complexity)

We can notice that solution of every subproblem depends only on two variables:
- `s` - position in the river
- `b` - index of current boat

Based on the information from the problem's description - we can see, that there are only `N` possible values of `s`, and only `M` possible values of `b`.   
Hence, there are only `M * N` possible combinations of `s` and `b` (let's call them - *states*).

So, we can make use of [Memoization technique](https://en.wikipedia.org/wiki/Memoization) - in order to transform brute force solution into **Top Down Dynamic Programming** solution.
Below presented code, which is very similar to the previous one - but, additionally, augmented with logic for memoization (highlighted lines of code):

{% highlight java linenos=table hl_lines="9 10 11 12 23 42 43 44 61 62" %}
import java.util.Arrays;

public class MFISH {

	static int solve(int[] fish, Boat[] boats) {
		Arrays.sort(boats, 
                    (b1, b2) -> Integer.compare(b1.anchor, b2.anchor));

		int[][] memoized = new int[fish.length + 1][boats.length];
		for (int[] row : memoized) {
			Arrays.fill(row, NOT_INITIALIZED);
		}

		int result = solve(boats, fish, 1, 0, memoized);
		return result;
	}

	static final int NEGATIVE_INFINITY = -100000000;
	static final int NOT_INITIALIZED = -1;

	static int solve(Boat[] boats, int[] fish,
			int start, int boat,
			int[][] memoized) {

		if (boat == boats.length) {
			// all boats successfully aligned
			return 0;
		}

		if (start > boats[boat].anchor) {
			// anchor of the boat is unreachable
			// from given start position
			return NEGATIVE_INFINITY;
		}

		if (((fish.length - start) + 1) < boats[boat].length) {
			// boat can't fit into the river
			// (intersects the right boundary of the fish array)
			return NEGATIVE_INFINITY;
		}

		if (memoized[start][boat] != NOT_INITIALIZED) {
			return memoized[start][boat];
		}

		if ((start + boats[boat].length) <= boats[boat].anchor) {
			// given start position is too far from
			// anchor of current boat
			// so, the start position has to be closer
			return solve(boats, fish, start + 1, boat, memoized);
		}

		int subProblem1 = solve(boats, fish, start + 1, boat, memoized);

		int coverage = 
                countFish(fish, start, (start + boats[boat].length) - 1);
                
		int subProblem2 = coverage +
        solve(boats, fish, start + boats[boat].length, boat + 1, memoized);

		memoized[start][boat] = Math.max(subProblem1, subProblem2);
		return memoized[start][boat];
	}

	// This method can be implemented with O(1) complexity,
    // using cumulative sum array
	static int countFish(int[] fish, int from, int to) {
		int coverage = 0;
		for (int i = from - 1; i <= (to - 1); i++) {
			coverage += fish[i];
		}
		return coverage;
	}
}
{% endhighlight %}

Provided code - contains implementation of method `countFish` with O(N) runtime complexity. 
But, actually, calculation of amount of fish on the interval - can be done with O(1) runtime complexity, using **cumulative sum array** (see: [Counting boat coverage with O(1) complexity](#counting-boat-coverage-with-o-1-complexity)).
So, for now, let's assume that runtime complexity of method `countFish` is O(1).

Hence, runtime complexity of each recursive step is O(1) - algorithm performs constant amount of opertaions.
The total amount of *states* is `M * N` - so, **overall complexity of solution is `O(N * M)`**.

To avoid the overhead of recursive calls mechanics (memory frame allocations), povided algorithm could be transformed into **Bottom Up Dynamic Programming** solution.

Unfortunately, the boundary conditions of the problem are pretty big: `M <= N <= 100000` - so, our `O(M * N)` solution is not sufficient for the instances of problem with large `M` and `N`.

Can we do better?

## [Dynamic Programming solution with O(N) complexity](#dynamic-programming-solution-with-o-n-complexity)

### [Additional analysis of the problem](#additional-analysis-of-the-problem)

Let's analyze the set of possible locations of *the leftmost border* of each boat.
Below presented the illustration, which helps to disclose the useful insight about the problem:

![Analysis of location of the leftmost borders of the boats](/images/spoj/mfish/leftmost_border_of_boats.png)

Which implies, that **there is 1:1 relation between position on the river - `s`, and index of boat - `b`, which can start at given position!**

Mathematically, it means that `b = g(s)` - index of the boat is just a function of the position on the river (`b` can be derived from given position on the river - `s`).

To be continued...

### [O(N) algorithm](#o-n-algorithm)

Will be soon...

## [Other optimizations](#other-optimizations)

Will be soon...

### [Counting boat coverage with O(1) complexity](#counting-boat-coverage-with-o-1-complexity)

Will be soon...

### [Sorting boats with O(N) complexity](#sorting-boats-with-o-n-complexity)

Will be soon...