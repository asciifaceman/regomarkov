# REgomarkov
[![GoDoc](https://godoc.org/https://github.com/asciifaceman/regomarkov?status.svg)](https://godoc.org/https://github.com/asciifaceman/regomarkov)

Go implementation of markov chains for textual data.

You can find out more about markov chains [here](http://setosa.io/ev/markov-chains/) and [here](https://towardsdatascience.com/introduction-to-markov-chains-50da3645a50d)

# Notice
This is a rehost of [https://github.com/mb-14/gomarkov](https://github.com/mb-14/gomarkov) which was not receiving PR merges or updates. This version contains some PRs from the original project, modifications, and is updated to be `go mod` compatible.

## Installation

With go 1.13+

`go get github.com/asciifaceman/regomarkov`


## Usage
```go
package main

import (
	"github.com/mb-14/gomarkov"
	"fmt"
	"strings"
	"io/ioutil"
	"encoding/json"
)

func main() {
	//Create a chain of order 2
	chain := gomarkov.NewChain(2)

	//Feed in training data
	chain.Add(strings.Split("I want a cheese burger", " "))
	chain.Add(strings.Split("I want a chilled sprite", " "))
	chain.Add(strings.Split("I want to go to the movies", " "))

	//Get transition probability of a sequence
	prob, _ := chain.TransitionProbability("a", []string{"I", "want"})
	fmt.Println(prob)
	//Output: 0.6666666666666666

	//You can even generate new text based on an initial seed
	chain.Add(strings.Split("Mother should I build the wall?", " "))
	chain.Add(strings.Split("Mother should I run for President?", " "))
	chain.Add(strings.Split("Mother should I trust the government?", " "))
	next, _ := chain.Generate([]string{"should", "I"})
	fmt.Println(next)
	// or to have the chain provide a full sentence (if possible)
	txt, _ := chain.GenerateAll()
	fmt.Println(txt)

	//The chain is JSON serializable
	jsonObj, _ := json.Marshal(chain)
	err := ioutil.WriteFile("model.json", jsonObj, 0644)
	if err != nil {
		fmt.Println(err)
	}
}
```
## Examples

- [Gibberish username detector](/examples/gibberish)

- [Fake Hackernews post generator](/examples/fakernews)

- [Pokemon name generator](/examples/pokenamer)