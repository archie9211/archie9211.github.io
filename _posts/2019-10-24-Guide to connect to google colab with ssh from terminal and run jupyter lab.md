---
layout: post
title: "Guide to connect to google colab with ssh from terminal and run jupyter lab"
date: 2019-10-24 23:38
category: [guide, how-to, how to]
author: Nageen
tags: []
summary: 
published: true 
image: assets/images/colab-ssh-jupyter.jpeg
featured: true

---
Google colab is a good option to experiment and try heavy computational tasks on a high end computer for free. Notebook interface provided by google is perfect python programs.

I have made a script to connect to colab instance with just few comands.

## What you need ?

 1. ngrok Account

 2. ssh public key

Go to [https://ngrok.com](https://ngrok.com/) and create an account and copy the authtoken only

``` 
./ngrok authtoken ZT64bWYnXsdTAdfdassJej4FNFT_2auAQqKqZHn2Sh4g2sfAD 
```

we need only  last argument of command from above.

for second option : run ssh-keygen and just press enter 3–4 times if it asks for anything

and then copy the public key

run cat .ssh/id_rsa.pub

reference is here

![](https://cdn-images-1.medium.com/max/2732/1*a7pz8ctR0CSOLVVz5fVcBQ.png)

now open google colab, connect to desired runtime type and execute following commands in notebook cell:

    ! wget https://gist.githubusercontent.com/archie9211/ae3c8411da88ae8b2a05b0ee1a4fd412/raw/ee1a6e78fc498ad6d4830cd2eb6033839235ea8a/colab-ssh-jupyter.sh

    !bash colab-ssh-jupyter.sh

Enter the authtoken and ssh public key when prompt asks respectively.

it will print the ssh command to connect to colab instance. e.g

    ssh root@0.tcp.ngrok.io -p 16826 # it will be different for your case

Open terminal in your local pc and run the command you get on colab.

you will get a bash shell logged in to colab instance. for jupyterlab, first install following on the terminal :

    pip3 install jupyterlab && apt install tmux

now run tmux and tun following command in it: (chose port of your choice {I prefer 5 digit random number})

    jupyter lab --ip 0.0.0.0 --port 56784

now press ctrl + b and then “ to split split window and then run follow in new window: (use the same port you used for jupyter lab)

    ssh -R 80:localhost:56784 serveo.net 

You should get a URL after this on terminal open this in browser and you should get your jupyter lab on browser connected to google colab instance.

To minimize tmux on terminal, press ctrl+b and then d.

Script used :

<script src="https://gist.github.com/archie9211/ae3c8411da88ae8b2a05b0ee1a4fd412.js"></script>
For any query/doubt or correction, please comment below.

Also read this post at : [here](https://medium.com/@archie9211/guide-to-connect-to-google-colab-with-ssh-from-terminal-and-run-jupyter-lab-7ed60258368)

Thanks.


