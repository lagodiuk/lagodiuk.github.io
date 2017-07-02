---
layout: post
title:  "Investigation of some probabilistic properties of the Langford pairing problem"
date:   2017-06-04 12:15:00
categories: computer_science
tags:
- combinatorics
- math
comments: true
---

In combinatorial mathematics, a [Langford pairing](https://en.wikipedia.org/wiki/Langford_pairing), also called a Langford sequence, is a permutation of the sequence of *2n* numbers *(1, 1, 2, 2, ..., n, n)* in which the two ones are one unit apart, the two twos are two units apart, and more generally the two copies of each number *k* are *k* units apart.
Langford pairings are named after C. Dudley Langford, who posed the problem of constructing them in 1958.

![Demonstration of the problem](/images/langford_pairing_problem/langford_pairing_picture.png)

* [When does the Langford pairing exist?]({{page.url}}#when-does-the-langford-pairing-exist)
* [Langford pairing and the random permutations]({{page.url}}#langford-pairing-and-the-random-permutations)
  * [Expectation of the amount of correct placements]({{page.url}}#expectation-of-the-amount-of-correct-placements)
  * [Variance of the amount of correct placements]({{page.url}}#variance-of-the-amount-of-correct-placements)
  * [Experimental evaluation]({{page.url}}#experimental-evaluation)
  * [Probabilistic bounds]({{page.url}}#probabilistic-bounds)
  * [Speculation around the Poisson distribution]({{page.url}}#speculation-around-the-poisson-distribution)

<!--more-->

## [When does the Langford pairing exist?](#when-does-the-langford-pairing-exist) ##

Let’s consider an arrangement of *n* pairs of natural numbers within a sequence of length *2n*, which satisfies conditions of the problem.
Is it possible to obtain a Langford sequence for all values of *n*?

Every number *i* can be associated with two numbers: *x<sub>i</sub>* and *y<sub>i</sub>*, which represent the indices of the first and the second occurrence of the number in the sequence.
Hence, according to the requirements of the problem, we can define the system of following constraints:

![img 1](/images/langford_pairing_problem/img_1.png)

The third and the second constraints mean, that all values of *x<sub>i</sub>* and *y<sub>i</sub>* must be unique.
Having the third constraint in mind, let’s calculate the sum of all *x<sub>i</sub>* and *y<sub>i</sub>*:

![img 2](/images/langford_pairing_problem/img_2.png)

On the other hand, having the first constraint in mind, the sum of all *x<sub>i</sub>* and *y<sub>i</sub>* can be represented in a
following way:

![ing 3](/images/langford_pairing_problem/img_3.png)

Hence, from equations (3) and (4) we have:

![img 4](/images/langford_pairing_problem/img_4.png)

As far as the sum of all *x<sub>i</sub>* is integer, then ![img 5](/images/langford_pairing_problem/img_5.png) must be integer as well.
It is possible iff ![img 6](/images/langford_pairing_problem/img_6.png).

Hence, in case if ![img 6](/images/langford_pairing_problem/img_6.png) the *n* pairs of natural numbers can be arranged into the sequence of
length *2n* (with respect to the constraints of the problem).

## [Langford pairing and the random permutations](#langford-pairing-and-the-random-permutations) ##

Let’s explore the possibility to obtain a Langford sequence (which will satisfy all constraints from equation (2)) as a result of the random permutation of the sequence of *n* pairs of natural numbers.

## [Expectation of the amount of correct placements](#expectation-of-the-amount-of-correct-placements) ##

Let’s consider a discrete valued random variable ![xi](/images/langford_pairing_problem/xi.png), which represents an amount of correct placements of pairs of numbers (which satisfy the first constraint from the system of constraints (2)).

In order to proceed, let’s represent ![xi](/images/langford_pairing_problem/xi.png) as a sum of indicator random variables:

![img 7](/images/langford_pairing_problem/img_7.png)

where ![xii](/images/langford_pairing_problem/xii.png) is an indicator random variable, 
which indicates - whether the pair of *i*-th numbers placed correctly (with respect to the constraints from equation (2)):

![img 8](/images/langford_pairing_problem/img_8.png)

Due to the linearity of the expectation:

![img 9](/images/langford_pairing_problem/img_9.png)

Where: ![img 10](/images/langford_pairing_problem/img_10.png) is a probability, that *x<sub>i</sub>* and *y<sub>i</sub>* are placed correctly (according to the constraints from equation (2)).

Let's calculate ![img 10](/images/langford_pairing_problem/img_10.png).
For every pair of numbers with index ![img 11](/images/langford_pairing_problem/img_11.png) out of ![img 12](/images/langford_pairing_problem/img_12.png) possible positions for *x<sub>i</sub>* and *y<sub>i</sub>* - there are only *(2n − i − 1)* positions, which satisfy constraints from equation (2).
For example:

![img 13](/images/langford_pairing_problem/img_13.png)

Hence:

![img 14](/images/langford_pairing_problem/img_14.png)

So, expectation of the amount of correct placements can be represented as:

![img 15](/images/langford_pairing_problem/img_15.png)

## [Variance of the amount of correct placements](#variance-of-the-amount-of-correct-placements) ##

Let’s calculate the variance of ![xi](/images/langford_pairing_problem/xi.png). 

By definition of the variance, and according to the equation (6):

![img 16](/images/langford_pairing_problem/img_16.png)

As far, as ![xii](/images/langford_pairing_problem/xii.png) is an indicator random variable, then:

![img 17](/images/langford_pairing_problem/img_17.png)

We can make use of the linearity property of expectation again:

![img 18](/images/langford_pairing_problem/img_18.png)

However, we need to calculate somehow the value of: ![img 19](/images/langford_pairing_problem/img_19.png).

Due to the symmetry, we can consider only the cases, when *i < j*:

![img 20](/images/langford_pairing_problem/img_20.png)

It turns out, that ![xiixij](/images/langford_pairing_problem/xiixij.png) is also an indicator random variable, which is defined in a following way:

![img 21](/images/langford_pairing_problem/img_21.png)

Hence, we have:

![img 22](/images/langford_pairing_problem/img_22.png)

![img 23](/images/langford_pairing_problem/img_23.png) is equal to a joint probability ![img 24](/images/langford_pairing_problem/img_24.png):

![img 25](/images/langford_pairing_problem/img_25.png)

Where:
* ![mij](/images/langford_pairing_problem/mij.png) - number of ways to place correctly the *i*-th and *j*-th pairs of numbers
* ![img 26](/images/langford_pairing_problem/img_26.png) - number of ways to choose values for: *x<sub>i</sub>*, *y<sub>i</sub>*, *x<sub>j</sub>* and *y<sub>j</sub>*

Let’s calculate ![mij](/images/langford_pairing_problem/mij.png).

The set of all correct relative arrangements of *x<sub>i</sub>*, *y<sub>i</sub>*, *x<sub>j</sub>* and *y<sub>j</sub>* can be divided into three disjoint sets with *interleaved* (![iij](/images/langford_pairing_problem/iij.png)), *nested* (![nij](/images/langford_pairing_problem/nij.png)) and *separate* (![sij](/images/langford_pairing_problem/sij.png)) relative placements.
Hence:

![img 27](/images/langford_pairing_problem/img_27.png)


1. **Interleaved arrangements:**  
![img 28](/images/langford_pairing_problem/img_28.png)  
Without loss of generality let’s consider only a first variant: ![img 29](/images/langford_pairing_problem/img_29.png) (the counting for the second variant will be exactly the same - so, we will just multiple an obtained result by 2).   
In order to produce the correct interleaved placement, we need to choose the proper positions of the leftmost and rightmost occupied cells (*x<sub>i</sub>* and *y<sub>j</sub>*), and afterwards we can solely define the positions of *y<sub>i</sub>* and *x<sub>j</sub>* inside:   
![img 30](/images/langford_pairing_problem/img_30.png)  
An amount of cells between the leftmost and rightmost occupied cells is:   
![img 31](/images/langford_pairing_problem/img_31.png)  
where: ![img 32](/images/langford_pairing_problem/img_32.png)     
As far as ![img 33](/images/langford_pairing_problem/img_33.png), then ![img 34](/images/langford_pairing_problem/img_34.png).
Hence the total amount of all possible interleaved placements is:    
![img 35](/images/langford_pairing_problem/img_35.png)   

2. **Nested arrangement:**     
![img 36](/images/langford_pairing_problem/img_36.png)    
In order to produce the correct nested placement, we need to choose the proper positions of the leftmost and rightmost occupied cells (*x<sub>j</sub>* and *<sub>yj</sub>*), and afterwards we should choose the proper positions for *x<sub>i</sub>* and *y<sub>i</sub>* inside:    
![img 37](/images/langford_pairing_problem/img_37.png)    
An amount of ways to choose the proper positions for *x<sub>j</sub>* and *y<sub>j</sub>* is: *(2n − j − 1)*.
Now, we need to choose the proper positions for *x<sub>i</sub>* and *y<sub>i</sub>* inside, and an amount of ways to do so is: *(j − i − 1)*.    
Hence:    
![img 38](/images/langford_pairing_problem/img_38.png)    

3. **Separate arrangements:**    
![img 39](/images/langford_pairing_problem/img_39.png)    
Without loss of generality let’s consider only a first variant: ![img 40](/images/langford_pairing_problem/img_40.png) (the counting for the second variant will be exactly the same - so, we will just multiple an obtained result by 2).   
In order to produce the correct interleaved placement, we need to choose the proper positions of the leftmost and rightmost occupied cells (*x<sub>i</sub>* and *y<sub>j</sub>*), and afterwards we can solely define the positions of *y<sub>i</sub>* and *x<sub>j</sub>* inside:    
![img 41](/images/langford_pairing_problem/img_41.png)    
An amount of cells between the leftmost and rightmost occupied cells is:     
![img 42](/images/langford_pairing_problem/img_42.png)    
where: ![img 43](/images/langford_pairing_problem/img_43.png)    
As far as ![img 44](/images/langford_pairing_problem/img_44.png), then ![img 45](/images/langford_pairing_problem/img_45.png).    
Hence, the total amount of all possible separate placements is:     
![img 46](/images/langford_pairing_problem/img_46.png)    
The tidy analysis shows, that in case if *i + j > 2n − 3* any separate arrangement of *i*-th and *j*-th pairs of characters is impossible within *2n* slots.
Hence, the complete formula for |![sij](/images/langford_pairing_problem/sij.png)| is:    
![img 47](/images/langford_pairing_problem/img_47.png)    

So, finally, using equations (20), (21) and (24) - we can express ![mij](/images/langford_pairing_problem/mij.png) in terms of *i*, *j* and *n*:   
![img 48](/images/langford_pairing_problem/img_48.png)    


After simplification, we can obtain the closed form for ![mij](/images/langford_pairing_problem/mij.png):    
![img 49](/images/langford_pairing_problem/img_49.png)    

Now, we are almost ready to calculate ![img 50](/images/langford_pairing_problem/img_50.png), using equations (16) and (17):    
![img 51](/images/langford_pairing_problem/img_51.png)    

In order to calculate the sum ![img 52](/images/langford_pairing_problem/img_52.png) let’s consider separately the cases where ![img 53](/images/langford_pairing_problem/img_53.png) and ![img 54](/images/langford_pairing_problem/img_54.png):    
![img 55](/images/langford_pairing_problem/img_55.png)    

After simplification, we can obtain the closed form:    
![img 56](/images/langford_pairing_problem/img_56.png)    

According to equations (14), (27) and (29) we can obtain the value of ![img 57](/images/langford_pairing_problem/img_57.png):    
![img 58](/images/langford_pairing_problem/img_58.png)    

So, using equations (10), (13) and (14) we can calculate the variance:     
![img 59](/images/langford_pairing_problem/img_59.png)    


## [Experimental evaluation](#experimental-evaluation) ##

In order to verify derived formulas for expectation and variance, I have developed an application for simulation of random permutations on the sequences with different amounts of pairs of natural numbers. 
For every *n* application simulates 500000 random permutations, and calculates the average amount (and variance) of correctly placed pairs of numbers.

Below is presented the code of a Java application for simulation:

{% highlight java %}
import java.util.*;

public class ExpectationVarianceCheck {

	public static void main(String[] args) {
		Random rnd = new Random(1);
		for (int size = 2; size < 46; size++) {
			evaluate(size, rnd);
		}
	}

	private static void evaluate(int size, Random rnd) {

		int trials_count = 500000;

		int correct_placements_cnt = 0;
		int correct_placements_cnt_sqr = 0;
		for (int i = 0; i < trials_count; i++) {
			List<Integer> characters = random_placement(size, rnd);
			int correct = calc_correct_placements(characters);
			correct_placements_cnt += correct;
			correct_placements_cnt_sqr += correct * correct;
		}

		double avg_correct_placements =
				(double) correct_placements_cnt / trials_count;
		double experimental_variance =
				(double) correct_placements_cnt_sqr / trials_count
						- Math.pow(avg_correct_placements, 2);

		System.out.printf("%d  %3.3f  %3.3f  %3.3f  %3.3f %n",
				size, avg_correct_placements, expectation(size),
				experimental_variance, variance(size));
	}

	private static double expectation(int n) {
		return (3.0 * n - 3) / (4 * n - 2);
	}

	private static double variance(int n) {
		return 3.0 / 4 + 4.0 / (n - 1) + 4.0 / (3 * n)
				- 127.0 / (96 * (2 * n - 3)) - 363.0 / (32 * (2 * n - 1))
				- 9.0 / (16 * Math.pow(2 * n - 1, 2));
	}

	private static int calc_correct_placements(List<Integer> characters) {
		Map<Integer, Integer> first_occurrence = new HashMap<>();
		int correct = 0;
		for (int i = 0; i < characters.size(); i++) {
			int c = characters.get(i);
			if (first_occurrence.get(c) == null) {
				first_occurrence.put(c, i);
			} else {
				int firstOccurrencePos = first_occurrence.get(c);
				if (i - firstOccurrencePos - 1 == c) {
					correct++;
				}
			}
		}
		return correct;
	}

	private static List<Integer> random_placement(int size, Random rnd) {
		List<Integer> characters = new ArrayList<>();
		for (int i = 1; i <= size; i++) {
			characters.add(i);
			characters.add(i);
		}
		Collections.shuffle(characters, rnd);
		return characters;
	}
}
{% endhighlight %}

Below is presented a table with comparison of the expectation and variance obtained experimentally and calculated, based on the derived formulas: 

| n (amount of pairs of numbers) | average amount of correct placements | theoretical expectation | experimental variance | theoretical variance |
|---|-------|-------|-------|-------|
| 2 | 0.502 | 0.500 | 0.250 | 0.250 |
| 3 | 0.601 | 0.600 | 0.462 | 0.462 |
| 4 | 0.643 | 0.643 | 0.520 | 0.520 |
| 5 | 0.669 | 0.667 | 0.562 | 0.560 |
| 6 | 0.681 | 0.682 | 0.590 | 0.589 |
| 7 | 0.691 | 0.692 | 0.611 | 0.611 |
| 8 | 0.699 | 0.700 | 0.630 | 0.628 |
| 9 | 0.708 | 0.706 | 0.644 | 0.641 |
| 10 | 0.713 | 0.711 | 0.653 | 0.651 |
| 11 | 0.716 | 0.714 | 0.660 | 0.660 |
| 12 | 0.720 | 0.717 | 0.670 | 0.667 |
| 13 | 0.719 | 0.720 | 0.674 | 0.674 |
| 14 | 0.723 | 0.722 | 0.679 | 0.679 |
| 15 | 0.724 | 0.724 | 0.684 | 0.684 |
| 16 | 0.725 | 0.726 | 0.688 | 0.688 |
| 17 | 0.728 | 0.727 | 0.695 | 0.691 |
| 18 | 0.727 | 0.729 | 0.692 | 0.695 |
| 19 | 0.730 | 0.730 | 0.699 | 0.698 |
| 20 | 0.730 | 0.731 | 0.700 | 0.700 |
| 21 | 0.733 | 0.732 | 0.703 | 0.703 |
| 22 | 0.734 | 0.733 | 0.705 | 0.705 |
| 23 | 0.732 | 0.733 | 0.705 | 0.707 |
| 24 | 0.735 | 0.734 | 0.711 | 0.708 |
| 25 | 0.733 | 0.735 | 0.710 | 0.710 |

![img 68](/images/langford_pairing_problem/img_68.png)   
![img 69](/images/langford_pairing_problem/img_69.png)   

As you can see, the values obtained via derived formulas of expectation and variance are consistent to the values obtained via computer simulation.  


## [Probabilistic bounds](#probabilistic-bounds) ##

Having the explicit expressions for expectation and variance we can make use of a couple of probabilistic inequalities, in order to estimate the probability of obtaining the correct placement of all *n* pairs of numbers:

* Markov's inequality:   
![img 60](/images/langford_pairing_problem/img_60.png)   

* Chebyshev's inequality:   
![img 61](/images/langford_pairing_problem/img_61.png)   

Unfortunately, these probabilistic bounds are still very weak.


## [Speculation around the Poisson distribution](#speculation-around-the-poisson-distribution) ##

Strictly speaking, the indicator random values of ![xii](/images/langford_pairing_problem/xii.png) are not independent, hence generally speaking, we can’t model the distribution of values of ![xi](/images/langford_pairing_problem/xi.png) using the Poisson distribution.

Nevertheless, let’s consider the values of expectation and variance for the large amounts of pairs *n*:

![img 62](/images/langford_pairing_problem/img_62.png)   

![img 63](/images/langford_pairing_problem/img_63.png)   

We can see, that ![img 64](/images/langford_pairing_problem/img_64.png).
Hence, to some extent, I assume that it is still viable to use a Poisson distribution as a rough approximation of the distribution of values of the random variable ![xi](/images/langford_pairing_problem/xi.png).
This assumption is also based on the observation, that occurrence of the correctly placed pair of numbers is a rare event.

The event rate of the given Poisson distribution is ![img 65](/images/langford_pairing_problem/img_65.png). 
Hence, the probability of occurrence of *k* correctly placed pairs is:

![img 66](/images/langford_pairing_problem/img_66.png)   

Let’s evaluate adequacy of the Poisson approximation experimentally (again, using the computer simulation):

{% highlight java %}
import java.util.*  ;

public class PoissonDistributionCheck {

	public static void main(String[] args) {
		Random rnd = new Random(1);
		for (int size = 20; size < 41; size += 5) {
			evaluate(size, rnd);
			System.out.println();
		}
	}

	private static void evaluate(int size, Random rnd) {
		int trialsCount = 500000;
		Map<Integer, Integer> correct_placements_count = new TreeMap<>();
		for (int i = 0; i < trialsCount; i++) {
			List<Integer> characters = random_placement(size, rnd);
			int correct = calc_correct_placements(characters);
			int count = correct_placements_count.getOrDefault(correct, 0);
			correct_placements_count.put(correct, count + 1);
		}
		for (int correct : correct_placements_count.keySet()) {
			int simulated_count = correct_placements_count.get(correct);
			double simulated_prob = (double) simulated_count / trialsCount;
			System.out.printf("%d %d %1.5f %1.5f %n", size, correct, simulated_prob, poisson_prob(correct));
		}
	}

	private static double poisson_prob(int correct) {
		long fact = factorial(correct);
		return Math.exp(-0.75) * Math.pow(0.75, correct) / fact;
	}

	private static long factorial(int n) {
		long fact = 1;
		for (int i = 1; i <= n; i++) {
			fact *= i;
		}
		return fact;
	}

	private static int calc_correct_placements(List<Integer> characters) {
		Map<Integer, Integer> first_occurrence = new HashMap<>();
		int correct = 0;
		for (int i = 0; i < characters.size(); i++) {
			int c = characters.get(i);
			if (first_occurrence.get(c) == null) {
				first_occurrence.put(c, i);
			} else {
				int firstOccurrencePos = first_occurrence.get(c);
				if (i - firstOccurrencePos - 1 == c) {
					correct++;
				}
			}
		}
		return correct;
	}

	private static List<Integer> random_placement(int size, Random rnd) {
		List<Integer> characters = new ArrayList<>();
		for (int i = 1; i <= size; i++) {
			characters.add(i);
			characters.add(i);
		}
		Collections.shuffle(characters, rnd);
		return characters;
	}
}
{% endhighlight %}

Below is presented an example of table (for *n = 40*) with comparison of observed distribution and the Poisson distribution:

| n  | k | Observed distribution of the values of ![xi](/images/langford_pairing_problem/xi.png) | Poisson distribution |
|----|---|---------|---------|
| 40 | 0 | 0.47407 | 0.47237 | 
| 40 | 1 | 0.35732 | 0.35427 | 
| 40 | 2 | 0.13153 | 0.13285 | 
| 40 | 3 | 0.03100 | 0.03321 | 
| 40 | 4 | 0.00533 | 0.00623 | 
| 40 | 5 | 0.00068 | 0.00093 | 
| 40 | 6 | 0.00008 | 0.00012 | 
| 40 | 7 | 0.00000 | 0.00001 | 

![img 67](/images/langford_pairing_problem/img_67.png)   

As you can see, the Poisson distribution can be used as an approximate upper bound of distribution of values of a random variable, which represents an amount of correctly placed pairs of numbers (after the random shuffling).

<div id="disqus_thread"></div>
<script>

var disqus_config = function () {
this.page.url = "http://lagodiuk.github.io/computer_science/2017/06/04/langford_pairing_problem.html";
this.page.identifier = "langford_pairing_problem";
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '//lahodiuk.disqus.com/embed.js';

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>

