---
layout: post
section-type: post
title: Solving a mundane word game with Python
category: Code
tags: [ 'Python', 'word-puzzle', 'set' ]
---
This was one of the first few times I actually used Python out of work or school. It's a word puzzle which would be easy if youre an english genius but if you're someone who wants a machine to do the heavy lifting, a Python script would be just what you need!

So well the puzzle goes something like this:  
Given a set of jumbled letters, find all possible combinations of antonym-like word pairs from it.

#### example
iount = in + out

#### other inputs:
<pre><code>golyrbi  
gondylou  
wpnuOd  
naawmnom  
titlghfer  
egmooc  
ydignhat  
yyiannnurs  
mmweetrrinus  
ciiavelglyt
</code></pre>

This was pretty straightforward, and I knew I needed the english dictionary to start with. There were a lot of different sources I could go with, here are a few examples to start with:
- [http://www-personal.umich.edu/~jlawler/wordlist](http://www-personal.umich.edu/~jlawler/wordlist)
- [http://www.mit.edu/~ecprice/wordlist.10000](http://www.mit.edu/~ecprice/wordlist.10000)
- [https://github.com/en-wl/wordlist](https://github.com/en-wl/wordlist)
- [https://github.com/dwyl/english-words](https://github.com/dwyl/english-words])

I got myself a words.txt file with ~25K words, each on a separate line. Reading them into a list was not that difficult:  
<pre><code data-trim class="python">wordfile = open("words.txt")
words = list(x for x in wordfile)
</code></pre>

Somehow newline characters were creeping up in the words, so I decided to rstrip all of them.  
<pre><code data-trim class="python">wordfile = open("words.txt")
words = list(x.rstrip() for x in wordfile)
</code></pre>

Well, now we need a user input! So  
<pre><code data-trim class="python">+input_word = raw_input("Enter the word to be checked: ")
</code></pre>


