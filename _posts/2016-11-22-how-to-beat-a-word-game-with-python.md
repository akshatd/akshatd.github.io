---
layout: post
section-type: post
title: Solving a mundane word game with Python
category: Code
tags: [ 'Python', 'word-puzzle', 'set' ]
---
So one fine day I encounter one of these word-game-riddle-thingies that im normally not very good at. It was on a whatsapp group chat, so while everyone normally flexes their brain muslces, I slip into a silent corner. This time round though, it was different. I decided to participate. What was different? This time I had Python!

So well the puzzle goes something like this:  
Given a set of jumbled letters, find all possible combinations of antonym-like word pairs from it.

#### example  
<code>iount = in + out   
other inputs:  
- golyrbi  
- gondylou  
- wpnuOd  
- naawmnom  
- titlghfer  
- egmooc  
- ydignhat  
- yyiannnurs  
- mmweetrrinus  
- ciiavelglyt
</code>

This was pretty straightforward, and i knew i needed a the english dictionary to start with. There were a lot of different sources i could go with, here are a few examples to start with:  
<code>[http://www-personal.umich.edu/~jlawler/wordlist]
[http://www.mit.edu/~ecprice/wordlist.10000]
[https://github.com/en-wl/wordlist]
[https://github.com/dwyl/english-words]
</code>

I got myself a words.txt file with ~25K words, each on a separate line. Reading them into a list was not that difficult:  
<pre><code data-trim class="python">wordfile = open("words.txt")
words = list(x for x in wordfile)
</code></pre>

Somehow newline characters were cleeping up in the words, so i decided to rstrip all of them.  
<pre><code data-trim class="python">wordfile = open("words.txt")
words = list(x.rstrip() for x in wordfile)
</code></pre>

Well, now we need a user input! So  
<pre><code data-trim class="python">+input_word = raw_input("Enter the word to be checked: ")
</code></pre>

