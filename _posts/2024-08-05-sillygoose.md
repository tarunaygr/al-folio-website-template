---
layout: post
title: n00bzunit3d 2024 programming/sillygoose
date: 2024-08-05 
description: Writeup for programming/sillygoose in n00bzunit3d 2024
tags: n00bzunit3d2024
categories: writeup
---
Welcome to my series of writeups for n00bzunit3d 2024 capture-the-flag competition. In this post, we look at the `programming/sillygoose` challenge. First, let's look at the challenge. The challenge runs on a server and requires to connect remotely to access the challenge. The server code is provided and is given below.


{% highlight python linenos %}
from random import randint
import time
ans = randint(0, pow(10, 100))
start_time = int(time.time())
turns = 0
while True:
    turns += 1

    inp = input()

    if int(time.time()) > start_time + 60:
       print("you ran out of time you silly goose") 
       break

    if "q" in inp:
        print("you are no fun you silly goose")
        break

    if not inp.isdigit():
        print("give me a number you silly goose")
        continue

    inp = int(inp)
    if inp > ans:
        print("your answer is too large you silly goose")
    elif inp < ans:
        print("your answer is too small you silly goose")
    else:
        print("congratulations you silly goose")
        f = open("/flag.txt", "r")
        print(f.read())

    if turns > 500:
        print("you have a skill issue you silly goose")
{% endhighlight %}

The challenge chooses a number randomly between `0` and `10^100`. Our task is to guess that number within 500 tries. For every guess, the challenge tells us if our guess was too big, too small or correct. This challenge can be essentially be boiled down to a binary search where we try to find the correct number in an array contain `0` - `10^100`. Doing so gives us the correct guess in `<=500` tries and the challenge gives us the flag.

```
.
.
.
your answer is too small you silly goose

congratulations you silly goose
n00bz{REDACTED}


Correct guess: 9265085847743364256439545120524325888018315000388679627853879417829339611151938624173184616910518237
```