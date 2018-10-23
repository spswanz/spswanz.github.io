---
layout: default
title: Spelling Bee Cheat
permalink: /SpellingBee.html
---

The _New York Times_ added a daily puzzle called ["Spelling Bee"](https://www.nytimes.com/puzzles/spelling-bee). The puzzle gives you seven letters in a hive (one in the center of the hive, surrounded by six other letters). You must make as many words as you can from those seven letters, as long as you comply with these rules:
 1. you must include the center letter;
 2. you can repeat letters as often as you want;
 3. the words must be at least four letters long (five letters long in Sunday's _A Little Variety_ puzzle pack). No 'cat' or 'if'.

You get one point for each valid word and three points for a word that uses all seven letters (a pangram in NYT-ese). Each puzzle contains at least one pangram.

Unlike the NYT crosswords, you can't check your answers right away; you have to wait until the following day. Furthermore, you can only check yesterday's answers, so if you want to go back to last week's answers, you can't. So I needed a way to check my answers. I especially needed a way to find the pangram, because no matter how many words I found, if I had not found the pangram, my day was ruined.

So I wrote a little Python script. It only works in the Terminal for now, but I'm teaching myself Flask to turn it into a web app. Stay tuned for updates. You can find it in my [Github repo](https://github.com/spswanz/NYT-Spelling-Bee-Cheat).   
