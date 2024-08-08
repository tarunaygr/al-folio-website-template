---
layout: post
title: n00bzunit3d 2024 Pwn/Think Outside the Box
date: 2024-08-05 
description: Writeup for Pwn/Think Outside the Box in n00bzunit3d 2024
tags: n00bzunit3d2024
categories: writeup
---
Welcome to my series of writeups for n00bzunit3d 2024 capture-the-flag competition. In this post, we look at the `Pwn/Think Outside the Box` challenge. The challenge is a binary that lets us play tic tac toe against it. Beat it and get the flag. The catch is that the bot goes first and anyone who has played tic tac toe knows that the player who goes first has an advantage. That means we can only draw at best.

Our first instinct was to look inside the binary and see what the code is actually doing. We opened it in <a href="https://ghidra-sre.org/">Ghidra</a>, a static analysis tool. This allows to look at the decompiled code of the program and see how it operates. Decompiles are usually only an educated guess on what the original program actually meant so they may look a bit messy.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/n00bzunit3d/ghidra.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

In lines 25-27, the code only checks to see if the input values are less than 3 and that no other symbol is placed in that particular tile. It means, there is no check for negative numbers. Additionally, in each move, the bot's move are based on hardcoded values of the player's input. If we are able to circumvent one of these checks, we are freely allowed to make moves that do not meet any of the conditions for the bot to make a move and get a free win.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/n00bzunit3d/ghidra2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The winning game
```
Move: 0,1
 O | O | _ 
---|---|---
 _ | X | _
---|---|---
 _ | _ | _
Bot turn!
 O | O | _
---|---|---
 _ | X | _
---|---|---
 _ | _ | _
Move: 0,2
Returning!
 O | O | O
---|---|---
 _ | X | _
---|---|---
 _ | _ | _
You won! Flag: REDACTED
```