---
id: 11z7ek6j577qpffsamw9ygx
title: Go
desc: ''
updated: 1706038231594
created: 1706037596484
---

___
<!-- TOC -->

1. [Introduction](#introduction)  
    * [About Go](#about-go)
    * [Installation](#installation)
2. []()
<!-- /TOC -->
___

# Introduction

## About Go

It is an open-source programming language developed and published by Google.

1. Focus on simplicity, clarity and scalability
    * inspired by languages like Python
    * aims to provide a clean, understandable syntax
2. High performance and focus on concurrency
    * similar to C or C++
    * popular for tasks that benefit from multi-threading
3. Batteries included
    * Go comes with a standard library
    * many core features are built-in
4. Statically typed
    * it is type-safe language
    * allows you to catch many errors early

Popular uses of Go include:

- Networking and APIs
- Microservices
- CLI Tools

## Installation

* Download the latest version of Go from their [website](https://go.dev/doc/install).

* Remove any previous Go installation by deleting the `/usr/local/go` folder (if it exists), then extract the archive you just downloaded into `/usr/local`, creating a fresh Go tree in `/usr/local/go`.  Do not untar the archive into an existing `/usr/local/go` tree. This is known to produce broken Go installations.

```bash
sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go<version>linux-amd64.tar.gz
```      

* Add /usr/local/go/bin to the PATH environment variable. You can do this by adding the following line to your $HOME/.profile or /etc/profile (for a system-wide installation):

```bash
export PATH=$PATH:/usr/local/go/bin
```          

* **Note:** Changes made to a profile file may not apply until the next time you log into your computer. To apply the changes immediately, just run the shell commands directly or execute them from the profile using a command such as source $HOME/.profile.

* Verify that you've installed Go by opening a command prompt and typing the following command:

```bash
go version
```          

* Confirm that the command prints the installed version of Go.

