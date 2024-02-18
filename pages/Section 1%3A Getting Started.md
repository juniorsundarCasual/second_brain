# Introduction
	- ## About Go
		- It is an open-source programming language developed and published by Google.
			- Focus on simplicity, clarity and scalability
			  logseq.order-list-type:: number
				- inspired by languages like Python
				  logseq.order-list-type:: number
				- aims to provide a clean, understandable syntax
				  logseq.order-list-type:: number
			- High performance and focus on concurrency
			  logseq.order-list-type:: number
				- similar to C or C++
				  logseq.order-list-type:: number
				- popular for tasks that benefit from multi-threading
				  logseq.order-list-type:: number
			- Batteries included
			  logseq.order-list-type:: number
				- Go comes with a standard library
				  logseq.order-list-type:: number
				- many core features are built-in
				  logseq.order-list-type:: number
			- Statically typed
			  logseq.order-list-type:: number
				- it is type-safe language
				  logseq.order-list-type:: number
				- allows you to catch many errors early
				  logseq.order-list-type:: number
		- Popular uses of Go include:
			- Networking and APIs
			- Microservices
			- CLI Tools
	- ## Installation
		- Download the latest version of Go from their [website](https://go.dev/doc/install).
		- Remove any previous Go installation by deleting the `/usr/local/go` folder
		  (if it exists), then extract the archive you just downloaded into
		  `/usr/local`, creating a fresh Go tree in `/usr/local/go`.  Do not untar
		  the archive into an existing `/usr/local/go` tree. This is known to
		  produce broken Go installations.
		- ```bash
		  sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go<version>linux-amd64.tar.gz
		  ```
		- Add `/usr/local/go/bin` to the `PATH` environment variable. You can do this by
		  adding the following line to your `$HOME/.profile` or `/etc/profile` (for a
		  system-wide installation):
		- ```bash
		  export PATH=$PATH:/usr/local/go/bin
		  ```
		- *Note:* Changes made to a profile file may not apply until the next time
		  you log into your computer. To apply the changes immediately, just run the
		  shell commands directly or execute them from the profile using a command
		  such as source `$HOME/.profile`.
		- Verify that you've installed Go by opening a command prompt and typing the
		  following command:
		  ```bash
		  go version
		  ```
		- Confirm that the command prints the installed version of Go.
	- ## Simple Program
		- You can initialise a Go module as follows:
		- ```bash
		  go mod init example.com/first-package
		  ```
		- A simple go program can be written as follows:
		  
		  ```go
		  // app.go
		  package main
		  // "package" clause
		  // main is a special package name as it tells the compiler that it is the
		  // entry point
		  // Every project needs to have a main package in place.
		  
		  import "fmt" // importing the "fmt" package from the standard library
		  
		  func main() {
		    fmt.Print("Hello World")
		  }
		  ```
		- We should also declare a module file and call it `go.mod`:
		  
		  ```gomod
		  module example.com/first-app
		  
		  go 1.21.6
		  ```
		- In terminal, running the following command will then run this code:
		- ``` bash
		  $ go run app.go
		  > Hello World
		  ```
- # Packages and Modules
	- Go projects are also called modules.
	- A package is a collection of similarly themed functions and .go files.
	- A module consists of multiplee packages.