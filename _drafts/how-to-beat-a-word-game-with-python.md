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

This was pretty straightforward, and it is obvious we need the english dictionary to start with. There were a lot of different sources one could go with, here are a few examples to start with:
- [http://www-personal.umich.edu/~jlawler/wordlist](http://www-personal.umich.edu/~jlawler/wordlist)
- [http://www.mit.edu/~ecprice/wordlist.10000](http://www.mit.edu/~ecprice/wordlist.10000)
- [https://github.com/en-wl/wordlist](https://github.com/en-wl/wordlist)
- [https://github.com/dwyl/english-words](https://github.com/dwyl/english-words])

Using these examples, you can get yourslef a simple words.txt file. Mine had ~25K words, each on a separate line. Reading them into a list was not that difficult:  
<pre><code data-trim class="python">wordfile = open("words.txt")
words = list(x for x in wordfile)
</code></pre>

Newline characters can sometimes creep up in the words, so we should strip them which is a good practise when taking in any string input.
<pre><code data-trim class="python">wordfile = open("words.txt")
words = list(x.strip() for x in wordfile)
</code></pre>
Since we are going to perform lookups on this list, it is better to store it as a hash table, or a *set* in python.
<pre><code data-trim class="python">wordfile = open("words.txt")
words = set(x.strip() for x in wordfile)
</code></pre>

Well, now we need a user input, so  
<pre><code data-trim class="python">+input_word = raw_input("Enter the word to be checked: ").strip()
</code></pre>

Now we need to get all the possible combinations of word pairs that can be formed with the given jumbled word. To get these word pairs we can first iterate over all the possible lengths of the word pairs, and then given the lengths of the the word pairs, find all the possible permutations of the letters (we use permutations instead of combinations since the ordering of letters matters in words!). An example should clear things out.

Given a word, *iount*, which has 5 letters, we first iterate over all the possible word pair lengths, which are:

* **1+4** (same as **4+1**) *and*

* **2+3** (same as **3+2**)

Then for each of the length pairs, we need to iterate over all the possible permutations. For example, in the 1+4 case:

* i + ount, *or*
* i + uont, *or*
* t + nuoi, *or*
* t + unoi, **_etc_**

You get the hint.

So, to first iterate over the different word pair length, we can do:
<pre><code data-trim class="python">for n in xrange(2,len(input_word)/2):
</code></pre>
We can start from 2 directly since there arent very any words in English that are single letters *and* have an antonym

To find all the permutations of a given string, we can import a nifty little function called *permutations* form the module **_itertools_**.
<pre><code data-trim class="python">from itertools import permutations
</code></pre>
and use it to get a list containing all the permutations of a given length
<pre><code data-trim class="python">perm_list = list(permutations(input_word, n))
</code></pre>
This gives us the unique first words of a given length n in the word pair. Now we first need to check if they are English words before even trying to make the second word with the remaining letters in the input_word.

This can be done by simply iterating over the list of permutations and checking if they are in the list of English words we initialised earlier.
<pre><code data-trim class="python">for perm in perm_list:
  perm = ''.join(perm)
  if perm in words:
</code></pre>
Note that the list the *permutations()* function returns isnt a list of words, it contains individual letters that we need to join to meaningfully compare them. Hence, we used the *join()* function first.

For the words in the list of permutaions that are actually in the English language, we now have to make another English word with the remaining letters in the input word. We can carefully remove all the letters of the first word from the input word to get the second word by replacing them with empty characters.
<pre><code data-trim class="python">input_word_copy = input_word[:]
for ch in perm:
  input_word_copy = input_word_copy.replace(ch, '', 1)
</code></pre>
Note that we copy the original word first before doing any manipulations to preserve the input word for future iterations with different word pairs

We are almost there! Now that we have the remaining letters of the word to form the second word in the word pair, all we need to do is to get all the permutations and then check if they are in the English language as well. This is easily done by:
<pre><code data-trim class="python">opp_perm_list = list(permutations(input_word_copy))
for opp_perm in opp_perm_list:
    opp_perm = ''.join(opp_perm)
    if opp_perm in words:
        print perm, opp_perm
</code></pre>

Ta Da, Done! For now, though. We are assuming here that as long as both the words are in the english language, they have to be antonyms (which is true for most cases).

We can further enhance this by actually comparing them against a database like the Stanford NLP database. There are also a lot of opportunities for multithreaded speedup, so stay tuned for another post detailing both these optimizations!
