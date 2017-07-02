---
layout: post
title:  "Tiny JavaScript parser of the subset of German"
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

* [Vorwort (Deutsch)]({{page.url}}#vorwort-deutsch)
* [Preface (English)]({{page.url}}#preface-english)
* [Demo]({{page.url}}#demo)
* [Used Tools and Libraries]({{page.url}}#used-tools-and-libraries)

![Illustration to the problem](/images/deutsch_nlp_demo/tree.png)

<!--more-->

## [Vorwort (Deutsch)](#vorwort-deutsch)
Circa vor vier Monaten habe ich angefangen Deutsch zu lernen. Zuerst war es schwierig, weil ich viel zu kleiner Wortschatz
hatte und nur einfache grammatikalische Regeln gekannt habe.

Trotzdem habe ich jetzt mehr Lust Deutsch zu lernen, weil ich den Lernprozess mit meinem Hobby kombiniert habe. Mein Hobby ist Computerlinguistik (sogenannt "Natural Language Processing" oder kurz "NLP"), und ich hatte eine Idee, um einen einfachen grammatikalischen Parser (Grammatik-Analyse-Tool) für die Teilmenge von Deutsch zu programmieren.

Vor einiger Zeit habe ich einen universellen Parsing Algorithm programmiert (gekannt als ["Earley Parser"](https://de.wikipedia.org/wiki/Earley-Algorithmus)): https://github.com/lagodiuk/earley-parser-js, und deshalb muss ich nur grammatikalische Regeln von Deutsch als [Kontextfreie Grammatik](https://de.wikipedia.org/wiki/Kontextfreie_Grammatik) bestimmen.

Damit möchte ich [den Prototype des grammatikalisches Parser präsentieren](#demo).

Ich auch denke, dass systematische Aufzählung der grammatikalischen Regeln mir hilft Deutsch zu lernen.

## [Preface (English)](#preface-english)
Approximately four months ago I have started to learn German. At first it was difficult, because I had too small vocabulary and knew only simple grammatical rules.

Nevertheless, now I have more desire to learn German, because I have combined the learning process with my hobby. My hobby is Computational Linguistics (so-called "Natural Language Processing" or shortly "NLP"), and I had an idea to develop a simple grammatical parser (grammar analysis tool) for the subset of German.

Some time ago I have implemented a universal parsing algorithm (known as ["Earley parser"](https://en.wikipedia.org/wiki/Earley_parser)): https://github.com/lagodiuk/earley-parser-js, and therefore I have to determine only grammatical rules of German as a [Context Free Grammar (CFG)](https://de.wikipedia.org/wiki/Kontextfreie_Grammatik).

So I would like to present [the prototype of grammatical parser](#demo).

I also think that systematic enumeration of grammatical rules (in the form of Context Free Grammar) helps me to learn German.

## [Demo](#demo)

**Note:** *as far as this is just a prototype, the grammar contains very small amount of nouns and verbs (mostly related to food) and very limited subset of grammatical rules (e.g. [modal verbs](https://de.wikipedia.org/wiki/Modalverb), limited capabilities of parsing [dependent clauses](https://de.wikipedia.org/wiki/Nebensatz), standard sentences in the [present tense](https://de.wikipedia.org/wiki/Pr%C3%A4sens), etc.).*

[Press here, if you would like to see the full-screen demo.](/nlp/deutsch.html)

{% include deutsch_nlp_demo/deutsch_nlp_demo.html %}

## [Used Tools and Libraries](#used-tools-and-libraries)
- My implementation of the Earley Parser: https://github.com/lagodiuk/earley-parser-js
- Minimized variant of the Earley Parser was produced by [Google Closure Compiler Service](https://closure-compiler.appspot.com/home)
- [jQuery](https://jquery.com/)
- [D3.js](https://d3js.org/)

<div id="disqus_thread"></div>
<script>

var disqus_config = function () {
this.page.url = "http://lagodiuk.github.io/nlp/2016/05/23/deutsch_nlp_demo.html";
this.page.identifier = "deutsch_nlp_demo";
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '//lahodiuk.disqus.com/embed.js';

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
