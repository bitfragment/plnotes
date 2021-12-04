---
title: 'Go: input and output'
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Sources

Alan A. A. Donovan and Brian W. Kernighan, *[The Go Programming Language].*
Addison-Wesley, 2016, Chapter 1: Tutorial

[The Go Programming Language]: http://www.gopl.io/


## Read stdin

Note, does not handle errors from `input.Scan()`, but should.

```go
func dup1() {
    counts := make(map[string]int)
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
        counts[input.Text()]++
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```


## Read file in streaming mode

A second version of `dup()` accepting a list of file names as command-line
argument, e.g. `go run myscript.go file1 file2 file3` and otherwise
reading stdin.

```go
func countLines(f *os.File, counts map[string]int) {
    input := bufio.NewScanner(f)
    for input.Scan() {
        counts[input.Text()]++
    }
}

func dup2() {
    counts := make(map[string]int)
    files := os.Args[1:]
    if len(files) == 0 {
        countLines(os.Stdin, counts)
    } else {
        for _, arg := range files {

            // `err` == nil on success.
            f, err := os.Open(arg)
            if err != nil {

                // The verb `%v` displays a value of any type.
                fmt.Fprintf(os.Stderr, "dup2: %v\n", err)
                continue
            }
            countLines(f, counts)
            f.Close()
        }
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```


## Read file into memory

`dup1()` and `dup2()` operate in streaming mode and "in principle
[...] can handle an arbitrary amount of input" (Donovan and Kernighan,
*The Go Programming Language* 12).

Version that reads input into memory, then processes it. Uses
`io/ioutil/ReadFile` and `strings.Split`. This version no longer
handles stdin.

> Under the covers, `bufio.Scanner`, `ioutil.ReadFile`, and 
> `ioutil.WriteFile` use the `Read` and `Write` methods of `*os.File`,
> but it's rare that most programmers need to access those lower-level
> routines directly. The higher-level functions like those from `bufio`
> and `io/ioutil` are easier to use.
—Donovan and Kernighan, *The Go Programming Language* 13

```go
func dup3() {
    counts := make(map[string]int)
    for _, filename := range os.Args[1:] {
        data, err := ioutil.ReadFile(filename)
        if err != nil {
            fmt.Fprintf(os.Stderr, "dup3: %v\n", err)
            continue
        }

        // "`ReadFile` returns a byte slice that must be converted into
        // a `string` so it can be split by `strings.Split`" (Donovan and 
        // Kernighan, *The Go Programming Language* 12).
        for _, line := range strings.Split(string(data), "\n") {
            counts[line]++
        }
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
``` 


## Network I/O

Demonstrated using the package `net/http` and a variation on the 
`curl` utility.

```go
func fetchurl() {
    for _, url := range os.Args[1:] {
        resp, err := http.Get(url)
        if err != nil {
            fmt.Fprintf(os.Stderr, "fetch: %v\n", err)
            os.Exit(1)
        }
        b, err := ioutil.ReadAll(resp.Body) // body is readable stream
        resp.Body.Close()
        if err != nil {
            fmt.Fprintf(os.Stderr, "fetch: reading %s: %v\n", url, err)
            os.Exit(1)
        }
        fmt.Printf("%s", b)
    }
}
```

Fetch URLs concurrently.

```go
func fetchall() {
    start := time.Now()
    ch := make(chan string) // create a channel of strings
    for _, url := range os.Args[1:] {
        go fetch(url, ch) // goroutine calls fetch() asynchronously
    }
    for range os.Args[1:] {
        fmt.Println(<-ch) // receive the summary line from channel ch
    }
    fmt.Printf("%.2fs elapsed\n", time.Since(start).Seconds())
}

func fetch(url string, ch chan<- string) {
    start := time.Now()
    resp, err := http.Get(url)
    if err != nil {
        ch <- fmt.Sprint(err) // send to channel ch
        return
    }

    // Read the response and discard it by writing to ioutil.Discard
    // output stream, retaining only the byte count.
    nbytes, err := io.Copy(ioutil.Discard, resp.Body)
    resp.Body.Close() // don't leak resources
    if err != nil {
        ch <- fmt.Sprintf("while reading %s: %v", url, err)
        return
    }
    secs := time.Since(start).Seconds()

    // Send a summary line on the channel ch
    ch <- fmt.Sprintf("%.2fs %7d %s", secs, nbytes, url)
}
```


## Minimal web server

Returns the path component of the server access URL.

Usage:
```sh
$ go run main.go &
$ open http://localhost:8000
Result:
	 URL.Path = "/"
```

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)
```

Connect handler() to incoming URLs and start a server listening on
port 8000. WHen a request arrives it is passed to handler().

```go
func server() {
	http.HandleFunc("/", handler)
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```

Request is a struct of type http.Request, whose fields include one
for the URL of the incoming request. Extract the path component
from the request URL and send it back as the response.

```go
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}

func main() {
	server()
}
```
