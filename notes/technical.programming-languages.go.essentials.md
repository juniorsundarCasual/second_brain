---
id: 1qfuw35ce1n4eftzb22prjk
title: Essentials
desc: ''
updated: 1706124035734
created: 1706123862491
---

# Simple Program

A simple go program can be written as follows:

```go
// app.go
package main
// "package" clause
// main is a special package name as it tells the compiler that it is the entry point
// Every project needs to have a main package in place.

import "fmt" // importing the "fmt" package from the standard library

func main() {
	fmt.Print("Hello World")
}
```

In terminal, running teh following command will then run this code:

```bash
$ go run app.go
> Hello World
```

# Packages and Modules

- Go projects are also called modules.
- A package is a collection of similarly themed functions and .go files.
- A module consists of multiplee packages.