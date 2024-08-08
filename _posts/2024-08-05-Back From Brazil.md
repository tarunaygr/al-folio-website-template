---
layout: post
title: n00bzunit3d 2024 programming/Back from Brazil
date: 2024-08-05 
description: Writeup for programming/Back from Brazil in n00bzunit3d 2024
tags: n00bzunit3d2024
categories: writeup
---
Welcome to my series of writeups for n00bzunit3d 2024 capture-the-flag competition. In this post, we look at the `programming/Back from Brazil` challenge. First, let's look at the challenge. The challenge runs on a server and requires to connect remotely to access the challenge. The server code is provided and is given below.


{% highlight python linenos %}
import random, time

def solve(eggs):
    redactedscript = """REDACTED"""
    return sum([ord(c) for c in redactedscript])

n = 1000

start = time.time()

for _ in range(10):
    eggs = []
    for i in range(n):
        row = []
        for j in range(n):
            row.append(random.randint(0, 696969))
            print(row[j], end=' ')
        eggs.append(row)
        print()

    solution = solve(eggs)
    print("optimal: " + str(solution) + " ðŸ¥š")
    inputPath = input()
    inputAns = eggs[0][0]
    x = 0
    y = 0

    for direction in inputPath:
        match direction:
            case 'd':
                x += 1
            case 'r':
                y += 1
            case _:
                print("ðŸ¤”")
                exit()

        if x == n or y == n:
            print("out of bounds")
            exit()

        inputAns += eggs[x][y]



    if inputAns < solution:
        print(inputAns)
        print("you didn't find enough ðŸ¥š")
        exit()
    elif len(inputPath) < 2 * n - 2:
        print("noooooooooooooooo, I'm still in Brazil")
        exit()

    if int(time.time()) - start > 60:
        print("you ran out of time")
        exit()

print("tnxs for finding all my ðŸ¥š")
f = open("/flag.txt", "r")
print(f.read())
{% endhighlight %}

The challenge starts you off at the top left of a matrix. You can move either right (r) or down (d) while collecting the most amount of eggs possible on the way to the bottom right. The challenge runs 10 times, if you do not collect the maximum amount of eggs for each run, you fail else you get the flag.

The problem is a variation of the classic dynamic programming problem <a href="https://leetcode.com/problems/minimum-path-sum/"> Minimum Path Sum </a>. Solving the problem with a similar strategy gets us the flag.

```
READING
tnxs for finding all my 🥚
n00bz{REDACTED}
```