---
layout: post
title:  "Is it possible to express all prime numbers using a regular grammar?"
date:   2016-04-30 21:15:00
categories: computer_science
tags:
- computer_science
comments: true
---

We will use the Pumping Lemma, in order to understand whether it is possible to describe the prime numbers using a Regular Grammar.

<!--more-->

## Table of contents
* [Problem Statement](#problem-statement)
* [Pumping Lemma](#pumping-lemma)
* [So, can we describe prime numbers using a regular grammar? (No)](#proof)

## [Problem Statement](#problem-analysis)

For simplicity let's consider the [unary numeral system](https://en.wikipedia.org/wiki/Unary_numeral_system) (number *N* is represented by word *X<sub>N</sub>*, which consists of N consecutive symbols **'1'**).  
Also, let's use the following notation: *|X<sub>i</sub>|* to express an amount of **'1'** symbols inside the word *X<sub>i</sub>*.

So, having the infinite set of words, which corresponds to prime numbers:

*X<sub>1</sub>* = 11  
*X<sub>2</sub>* = 111  
*X<sub>3</sub>* = 11111  
*X<sub>4</sub>* = 1111111  
*X<sub>5</sub>* = 11111111111  
...

where *|X<sub>i</sub>|* is a prime number, for every *i*,

we would like to understand, whether it is possible to express all words from this set using a regular grammar?  

In other words, is it possible construct the finite state machine, which will be able to recognize all prime numbers (only the prime numbers)?

## [Pumping Lemma](#pumping-lemma)

[The Pumping Lemma](https://en.wikipedia.org/wiki/Pumping_lemma_for_regular_languages) states, that *every sufficiently long word* of a regular language can be expanded in a special way, such that *every new word (obtained after expansion of the original word) will belong to this language as well*.

Formally saying, every word *X<sub>i</sub>* (such, that length of the word is greater than some threshold) can be splitted into 3 consecutive parts: *X*, *Y* and *Z*, and every new word (e.g. *XYYZ*, *XYYYZ*, ..., *XY<sup>n</sup>Z*) will belong to the same language as *XYZ*.

This lemma is a very convenient tool for proving, that the language is not regular.

> For example, in case if:  
> *W<sub>p</sub>* = 11111111111  
> 
> One of the possible ways to split *W<sub>p</sub>* is:  
> *W<sub>p</sub> = XYZ* = 11111 _ 11 _ 1111  
> (underscore characters are used to display the separation between *X*, *Y* and *Z*)
> 
> So, the parts of the word are:  
> *X* = 11111  
> *Y* = 11  
> *Z* = 1111

## [So, can we describe prime numbers using a regular grammar? (No)](#proof)

The unary representation *W<sub>p</sub>* of every prime number *p* (such that *|W<sub>p</sub>| = p*) can be splited into 3 parts: *X*, *Y* and *Z*.  

Which, in general case, implies that:  
*|W<sub>p</sub>| = |XYZ| = |X| + |Y| + |Z|*

*XY<sup>n</sup>Z* means, that we just should replicate *n* times the *Y* part of the word *W<sub>p</sub>*.

So, the amount of symbols **'1'** inside the generated word can be represented as:  
*|XY<sup>n</sup>Z| = |X| + n×|Y| + |Z| = (|X| + |Y| + |Z|) + (n - 1)×|Y| = |W<sub>p</sub>| + (n - 1)×|Y|*

We can choose the *n = |W<sub>p</sub>| + 1*  
Which implies, that:  
*|XY<sup>n</sup>Z| = |W<sub>p</sub>| + (n - 1)×|Y| = |W<sub>p</sub>| + |W<sub>p</sub>|×|Y| = (|Y| + 1)×|W<sub>p</sub>|*  
Hence, *XY<sup>n</sup>Z* **is a composite number**.

**Hence, for any arbitrary splitting of *W<sub>p</sub>* (any arbitrary prime number), we can choose such *n*, that *XY<sup>n</sup>Z* will be a composite number.**

Our implication contradicts to the Pumping Lemma, which leads to a conclusion, that the language, which consists of the prime numbers can’t be expressed using a Regular Grammar.

<div id="disqus_thread"></div>
<script>

var disqus_config = function () {
this.page.url = "http://lagodiuk.github.io/computer_science/2016/04/30/prime_numbers_regular_grammar.html";
this.page.identifier = "prime_numbers_regular_grammar";
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '//lahodiuk.disqus.com/embed.js';

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>