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

## Setup

```go
package main

import (
    "bufio"
    "fmt"
    "image"
    "image/color"
    "image/gif"
    "io"
    "io/ioutil"
    "log"
    "math"
    "math/rand"
    "net/http"
    "os"
    "strings"
    "sync"
    "time"
)
```


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


## Using standard image packages

```go
const (
	whiteIndex = 0 // first color in palette
	blackIndex = 1 // next color in palette
)

var palette = []color.Color{color.White, color.Black}

// Create a Lissajous figure.
func lissajous(out io.Writer) {
	const (
		cycles  = 5     // number of complex x oscillator revolutions
		res     = 0.001 // angular resolution
		size    = 100   // image canvas covers [-size..+size]
		nframes = 64    // number of animation frames
		delay   = 8     // delay between frames in 10ms units
	)

	freq := rand.Float64() * 3.0 // relative frequency of y oscillator
	anim := gif.GIF{LoopCount: nframes}
	phase := 0.0 // phase difference

	// Create `nframes` images, each representing a frame of the
	// animation, storing them in the struct `anim`, interspersed
	// with delays of the value of `delay` each.
	for i := 0; i < nframes; i++ {
		rect := image.Rect(0, 0, 2*size+1, 2*size+1)
		img := image.NewPaletted(rect, palette)

		// Run the two oscillators.
		for t := 0.0; t < cycles*2*math.Pi; t += res {

			// x oscillator is a sine function.
			x := math.Sin(t)

			// Frequency of y oscillator incorporates a random
			// value and the increasing value of `phase`.
			y := math.Sin(t*freq + phase)

			// Set the pixel x,y to a randomly chosen color.
			img.SetColorIndex(
				size+int(x*size+0.5),
				size+int(y*size+0.5),
				blackIndex)
		}
		phase += 0.1
		anim.Delay = append(anim.Delay, delay)
		anim.Image = append(anim.Image, img)
	}

	// Encode the sequence of frames and delays in `anim` and
	// write to output.
	gif.EncodeAll(out, &anim)
}
```


## Network I/O

The `lissajous` program, the following `fetch*` programs, and the minimal
web server use different types as output streams:

- `lissajous` copies data to `os.Stdout`, a file
- `fetchurl` copies HTTP response data to `os.Stdout`
- `fetchall` discards the response by writing it `ioutil.Discard`
- The web server uses `fmt.Fprintf` to write data to a
  `http.ResponseWriter` that represents the browser.

Fetch a URL.

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

Request is a struct of type http.Request, whose fields include one
for the URL of the incoming request. Extract the path component
from the request URL and send it back as the response.

```go
func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}
```

Connect handler() to incoming URLs and start a server listening on
port 8000. WHen a request arrives it is passed to handler().

```go
func server() {
    http.HandleFunc("/", handler)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```


## Server version 2: adding a request counter

```go
var mu sync.Mutex
var count int
```

Display the Path component of the requested URL.
`mu.Lock()` and `mu.Unlock()` ensure a race condition can't be caused by
concurrent requests trying to update `count` as the same time.

```go
func handler2(w http.ResponseWriter, r *http.Request) {
    mu.Lock()
    count++
    mu.Unlock()
    fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}
```

Display the accumulated number of calls.

```go
func counter(w http.ResponseWriter, r *http.Request) {
    mu.Lock()
    fmt.Fprintf(w, "Count %d\n", count)
    mu.Unlock()
}
```

Two handlers serve requests to two different URLs.
Each handler is run in a separate goroutine, so the server can
serve multiple requests simultaneously.

```go
func server2() {
    // Handle request to /
    http.HandleFunc("/", handler2)

    // Handler request to /count
    http.HandleFunc("/count", counter)
    
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```


## Server version 3: report headers and form data

Version that echoes the HTTP request.

```go
func handler3(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "%s %s %s\n", r.Method, r.URL, r.Proto)
    for k, v := range r.Header {
        fmt.Fprintf(w, "Header[%q] = %q\n", k, v)
    }
    fmt.Fprintf(w, "Host = %q\n", r.Host)
    fmt.Fprintf(w, "RemoteAddr = %q\n", r.RemoteAddr)

    // "Go allows a simple statement such as a local variable 
    // declaration to precede the `if` condition, which is particularly
    // useful for error handling as in this example [...] [this form] is
    // shorter and reduces the scope of the variable `err`, which is 
    // good practice." (22).
    if err := r.ParseForm(); err != nil {
        log.Print(err)
    }

    for k, v := range r.Form {
        fmt.Fprintf(w, "Form[%q] = %q\n", k, v)
    }
}


func server3() {
    http.HandleFunc("/", handler3)
    http.HandleFunc("/count", counter)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```


## Server version 4: write Lissajous figures

Version that writes animated GIFs to the HTTP client.

```go
func server4() {
    // The second argument here "is a *function literal*, that is, an
    // anonymous function defined at its point of use" (22).
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        lissajous(w)
    })
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```


## `main()`, invoking `server4()`

```go
func main() {
    server4()
}
```


## Execute this file

```txt
$ codedown go < input-output.md | grep . > /tmp/tmp.go && go run /tmp/tmp.go &
$ open http://localhost:8000
Result:
     URL.Path = "/"
```
