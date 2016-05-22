---
layout: post
title:  "The tiny parser of the subset of German"
date:   2016-05-22 22:07:00
categories: nlp
tags:
- nlp
custom_css:
- deutsch_nlp_demo/jquery-ui.css
- deutsch_nlp_demo/style.css
custom_js:
- deutsch_nlp_demo/earley-oop.min.js
- deutsch_nlp_demo/jquery-2.1.3.min.js
- deutsch_nlp_demo/jquery-ui.min.js
- deutsch_nlp_demo/d3.v3.min.js
- deutsch_nlp_demo/demo.js
comments: true
body_onload: onload="init()"
---

I have combined the process of learning German with my hobby, and would like to present the prototype of the parser of the subset of German language.

<!--more-->

## Table of contents
* [Vorwort (Deutsch)](#vorwort-deutsch)
* [Preface (English)](#preface-english)
* [Demo](#demo)
* [Used Tools and Libraries](#used-tools-and-libraries)

![Illustration to the problem](/images/deutsch_nlp_demo/tree.png)

## [Vorwort (Deutsch)](#vorwort-deutsch)
Circa vor vier Monaten bin ich Deutsch lernen angefangen. Zuerst war es schwierig, weil ich viel zu klein Wortschatz hatte und nur einfache grammatikalische Regeln gekannt hat.

Trotzdem habe ich jetzt mehr Lust Deutsch zu lernen, weil ich den Lernprozess mit meinem Hobby kombiniert habe. Mein Hobby ist Computerlinguistik (sogenannt "Natural Language Processing" oder kurz "NLP"), und ich hatte eine Idee, um einen einfachen grammatikalischen Parser (Grammatik-Analyse-Tool) für die Teilmenge von Deutsch zu programmieren.

Vor einiger Zeit habe ich einen universellen Parsing Algorithm programmiert (gekannt als "Earley Parser"): https://github.com/lagodiuk/earley-parser-js, und deshalb muss ich nur grammatikalische Regeln von Deutsch als Context Free Grammar (CFG) bestimmen.

Damit möchte ich [den Prototype des grammatikalisches Parser präsentieren](#demo).

## [Preface (English)](#preface-english)
Approximately four months ago I have started to learn German. At first it was difficult, because I had too small vocabulary and knew only simple grammatical rules.

Nevertheless, now I have more desire to learn German, because I have combined the learning process with my hobby. My hobby is Computational Linguistics (so-called "Natural Language Processing" or shortly "NLP"), and I had an idea to develop a simple grammatical parser (grammar analysis tool) for the subset of German.

Some time ago I have implemented a universal parsing algorithm (known as "Earley parser"): https://github.com/lagodiuk/earley-parser-js, and therefore I must determine only grammatical rules of German as a Context Free Grammar (CFG).

So I would like to present [the prototype of grammatical parser](#demo).

## [Demo](#demo)

{% include deutsch_nlp_demo/deutsch_nlp_demo.html %}

## [Used Tools and Libraries](#used-tools-and-libraries)
- My implementation of the Earley Parser: https://github.com/lagodiuk/earley-parser-js
- Minimized variant of the Earley Parser was produced by [Google Closure Compiler Service](https://closure-compiler.appspot.com/home)
- [jQuery](https://jquery.com/)
- [D3.js](https://d3js.org/)

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
