---
layout: post
title:  "Demonstrative comparison of the problem solving strategies"
date:   2017-03-26 10:00:00
categories: computer_science
tags:
- algorithms
- combinatorics
- math
comments: false
---

I would like to describe an interesting problem, which is useful for demonstration of various approaches of solving problems in the world of software engineering: *Brute-force*, *Dynamic programming* and *derivation of an explicit closed-form formula* using ideas from Combinatorics.
We will see, how the runtime efficiency of solutions can be improved from exponential complexity down to almost a constant time complexity and will discuss the pros and cons of each approach.

![Introduction image](/images/problem_solving_algorithms_and_math/img1.png)

## Table of contents
* [Description of the problem]({{page.url}}#description-of-the-problem)
* [Brute-force solution]({{page.url}}#brute-force-solution)
* [Dynamic programming solution]({{page.url}}#dynamic-programming-solution)
* [Explicit closed-form formula solution]({{page.url}}#explicit-closed-form-formula-solution)

<!--more-->

## [Description of the problem](#description-of-the-problem) ##


There is a hexagon and some kind of a creature, which moves between vertices of the hexagon. At every step the creature randomly picks one of adjacent vertices and moves there. The creature starts to move from the vertex, named "A". The question is: how many paths are there - which ends in the initial vertex after N moves?

Below is an illustration, which demonstrates two paths (out of six possible), which ends in the initial vertex after four moves (the arrows are indexed in the order of performed moves):

![Demonstration of the problem](/images/problem_solving_algorithms_and_math/img2.png)


## [Brute-force solution](#brute-force-solution) ##

Let's start with a Brute-force solution. 
As an auxiliary step, let's assign the indices from the interval *0 to 5* to the vertices of the hexagon.
We can observe that an amount of paths, which ends in some vertex, 
depends only on the amount of paths to the adjacent vertices (with an amount of available moves decreased by one):

![Demonstration of the Brute-force solution](/images/problem_solving_algorithms_and_math/img3.png)

So, based on the described observation, we can straightforwardly implement the solution as a recursive function:

{% highlight java %}
// Amount of vertices of the polygon
final int V = 6;

int solve(int moves) {
    // Start vertex is "A" (index 0)
    // Target vertex is also "A"
    return count(moves, 0);
}

// moves - amount of moves
// curr  - index of a current vertex
int count(int moves, int curr) {
	if(moves == 0) { return (curr == 0) ? 1 : 0; }
	int next = count(moves - 1, (curr + 1) % V);
	int prev = count(moves - 1, (curr > 0) ? curr - 1 : V - 1);
	return next + prev;
}
{% endhighlight %}

However, the analysis of the tree of recursive calls of the function `count` shows, that there are many identical subtrees:

![Demonstration of the problem of the Brute-force solution](/images/problem_solving_algorithms_and_math/img5.png)

The presence of identical subtrees of recursive calls, is a result of a fact, that it is possible to arrive in a vertex via different paths, using the same amount of moves:

![Demonstration of the problem of the Brute-force solution](/images/problem_solving_algorithms_and_math/img4.png)

So we see, that *the solution of the problem consists of the overlapping solutions of subproblems*.

As far as described thoughts are valid *for every value of N (amount of moves)* - we can conclude, that the solution of *every* subproblem consists of the overlapping solutions of its subproblems and so forth.
Hence, **the overall complexity of provided solution is exponential**.

## [Dynamic programming solution](#dynamic-programming-solution) ##

As far as the recursive solution consists of the overlapping problems, let's calculate *the cardinality of the space of all subproblems*. 
The function `count` has two arguments: 
- `int moves` - amount of available moves, and the value of `moves` varies *from 0 to N*
- `int curr` - index of a current vertex, and the value of `curr` varies *from 0 to (V - 1)* (where *V* - is an amount of vertices).  

So, as far as the combination of argument values uniquely determines a subproblem - the total amount of subproblems is `(N+1) * V`.

Hence, we can make use of the *memoization* table (with dimensions *(N+1) x V*) in order to cache the values of the already solved subproblems:

{% highlight java %}
// Amount of vertices of the polygon
final int V = 6;

// Needed for memoization
final int NOT_SOLVED_YET = -1;

int solve(int moves) {
	// Initialization of the memoization table
    int[][] memoized = new int[moves + 1][V];
	for(int[] row : memoized) { Arrays.fill(row, NOT_SOLVED_YET); }
    
	return count(moves, 0, memoized);
}

int count(int moves, int curr, int[][] memoized) {
    // Is the subproblem already solved?
	if(memoized[moves][curr] != NOT_SOLVED_YET) {
		return memoized[moves][curr];
	}
	
	int result;
	if(moves == 0) {
		result = (curr == 0) ? 1 : 0;
	} else {
		int next = count(moves - 1, (curr + 1) % V);
		int prev = count(moves - 1, (curr > 0) ? curr - 1 : V - 1);
		result = next + prev;
	}

    // Remember the solution of the subproblem
	memoized[moves][curr] = result;
	return result;
}
{% endhighlight %}

The provided recursive algorithm is a *Top-down* version of the Dynamic programming solution. 
It can be rewritten into the iterative algorithm (a *Bottom-up* version of the Dynamic Programming solution). 
Inside the iterative solution - it is not needed to keep the entire memoization table, and instead we can track only the previous row of the table, which helps to reduce the memory footprint of the solution.

As far as in a worst case it is needed to traverse an entire memoization table in order to solve the problem - the complexity of the Dynamic programming solution is `O(N * V)`. 
Taking into account that *V* (amount of vertices) is a constant - the complexity of solution is linear in terms of *N*: **`O(N)`**.

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

### [Explicit closed-form formula solution](#explicit-closed-form-formula-solution) ###

Is it possible to develop a solution with better runtime complexity?
It turns out that *yes*! However, we will need a bit of the extra math in order to reveal additional aspects of the problem.

Let's introduce an auxiliary notation (keeping in mind, that we consider only paths, which starts in the vertex "A"):

![Auxiliary notation](/images/problem_solving_algorithms_and_math/img6.png)

The presence of the symmetry axis (which goes through the starting vertex "A") of hexagon implies:

![Implication of the symmetry property](/images/problem_solving_algorithms_and_math/img7.png)

Let's write recurrence relations for calculations of amount of paths to the different vertices of the hexagon:

![Recurrence relations](/images/problem_solving_algorithms_and_math/img8.png)

Now, our goal is to reduce an amount of variables inside the recurrence relations:

![Simplified recurrence relations](/images/problem_solving_algorithms_and_math/img9.png)

So, we have obtained a system of two recurrence relations with two variables (6) and (7):

![System of two recurrence relations](/images/problem_solving_algorithms_and_math/img10.png)

Now, let's derive the recurrence relation for amount of paths, which ends in the vertex "A" after *n* moves:

![Solution of the system of two recurrence relations](/images/problem_solving_algorithms_and_math/img11.png)