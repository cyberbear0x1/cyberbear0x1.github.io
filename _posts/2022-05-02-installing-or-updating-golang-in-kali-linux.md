---
title: How to install or Update GoLang in Kali Linux 
author: Cyber Bear
date: 2022-05-02 20:10:00 UTC+1
categories: [Go Tutorial]
tags: [Kali Linux, Golang, Go, Update, gopls, VSCODE]
render_with_liquid: false
toc: true
---

This post will guide you on how to install or upgrade to GO 1.81 in Kali Linux.

>If this is your first time installing kindly skip to [Installing Go.](#installing-go)
{: .prompt-info }



* TOC
{:toc}

# What is Go

Go (or GoLang) is a modern programming language originally developed
by Google that uses high-level syntax similar to scripting languages. It is
popular for its minimal syntax and innovative handling of concurrency, as
well as for the tools it provides for building native binaries on foreign
platforms.

Installing Go is a quite straight-forward process.

Let's begin....

# Getting ready to Update Go

>Please skip this part if you don't have Go configured on your computer.
{: .prompt-info }

In this part, we will removing or renaming existing Go files. 

This will prevent any file collision.

Firstly let's find where the go binary was previously installed:

```bash
which go
# /usr/share/go-1.14/bin/go 
```
>You can either choose to delete or rename this file; I recommend you just rename it should things go wrong, you can just revert back by renaming it to it's initial name.
{: .prompt-tip }

To **delete** this file:

```bash
sudo rm -rf /usr/share/go-1.14/bin/go
```
To **rename** this file:

```bash
sudo mv /usr/share/go-1.14/bin/go /usr/share/go-1.14/bin/go.bak
# This file will be renamed to go.bak
```

Now you are ready to begin installation of the latest Go version.

# Installing Go

To install the latest Go binary kindly visit the [official Go download page](https://go.dev/dl/).

You are to choose the Linux file *go1.18.1.linux-amd64.tar.gz*
![go download image](/assets/UpdatingGO/Screenshot_2022-05-01_09-12-13.jpg)

once downloaded, go into the directory the file was downloaded into.

If you don't know where this:

```bash
locate go1.18.1.linux-amd64.tar.gz
#/home/kali/Downloads/go1.18.1.linux-amd64.tar.gz
```

Now go into the directory:

```bash
cd /home/kali/Downloads/
```

Next, Let's extract the file and save it in `/usr/local/`:
```bash
tar -C /usr/local/ -xzf go1.18.1.linux-amd64.tar.gz
```

Now let's create the necessary environment variables required for Go to run smoothly.

To do this:

Open your `.zshrc` file with the nano editor

```bash
nano ~/.zshrc
```
now go down at the end of the file and add:

```bash
#go variables
export GOPATH=$HOME/go
export GOROOT="/usr/local/go"
PATH="$PATH:$GOROOT/bin:$GOPATH/bin"
```
![variables](/assets/UpdatingGO/Screenshot_2022-05-03_13-10-17.png)

`CTRL + O` and hit the Enter Key to save the file:

Finally, activate your new `.zshrc` configuration.

To do this:
```bash
source ~/.zshrc
```
You should have Go installed.

Let's quickly confirm by checking it's version.

To do this:

```bash
go version
#go version go1.18.1 linux/amd64
```
# Possible Bugs and Fixes

If you use VSCODE, you may encounter this error

`undeclared name: any (compile)`
![error](/assets/UpdatingGO/go.png)

This issue is caused by GOPLS

To fix this, you can

1. Update the VSCode extension by:

`CTRL + P` and typing "Go: Install/update Tools"

2. Update it manually by first deleting the old GOPLS and reinstalling manually

To **delete**:

```bash
rm $GOPATH/bin/gopls
rm -r $GOPATH/src/golang.org/x/tools/gopls
```


To **reinstall**:
```bash
go get -v golang.org/x/tools/gopls
```

Now everything should work fine.

Happy Coding!!!


