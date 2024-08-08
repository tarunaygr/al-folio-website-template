---
layout: post
title: n00bzunit3d 2024 web/passwordless
date: 2024-08-05 
description: Writeup for web/passwordless in n00bzunit3d 2024 
tags: n00bzunit3d2024
categories: writeup
---
Welcome to my series of writeups for n00bzunit3d 2024 capture-the-flag competition. In this post, we look at the `web/passwordless` challenge. 
We are given a link to a website that asks for a username field and the backend code of the server that does the verification of the user.

server.py
{% highlight python linenos%}
#!/usr/bin/env python3
from flask import Flask, request, redirect, render_template, render_template_string
import subprocess
import urllib
import uuid
global leet

app = Flask(__name__)
flag = open('/flag.txt').read()
leet=uuid.UUID('13371337-1337-1337-1337-133713371337')

@app.route('/',methods=['GET','POST'])
def main():
    global username
    if request.method == 'GET':
        return render_template('index.html')
    elif request.method == 'POST':
        username = request.values['username']
        if username == 'admin123':
            return 'Stop trying to act like you are the admin!'
        uid = uuid.uuid5(leet,username) # super secure!
        return redirect(f'/{uid}')

@app.route('/<uid>')
def user_page(uid):
    if uid != str(uuid.uuid5(leet,'admin123')):
        return f'Welcome! No flag for you :('
    else:
        return flag

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=1337)
{% endhighlight %}
The server uses `uuid5` and the `leet` object to do authentication. However, it prevents us from entering `admin123` in the username field. This is a easy workaround. We can find the `uuid5` value of the `admin123` username by ourself and enter that directly into the browser searchbar. Opening the correct webpage gives us the flag.