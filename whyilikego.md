# Porque gosto de Go

Vou a muitos eventos de desenvolvedores em geral, muitos estranham a minha motivação tão grande por Go e me fazem essa pergunta. Daí encontrei o artigo do [Freeformz](https://github.com/freeformz/) e achei a ideia de deixar isso no Github muito boa. Ao ler o artigo dele encontrei muitas coisas que concordo então resolvi fazer um fork e escrever a versão em português. O intuíto é deixar claro as vantagens de Go e ajudar desenvolvedores a convencerem seus gerentes e outros colaboradores a iniciarem a adoção de Go.

## Goroutines

The first 1/2 of Go's concurrency story. Lightweight, concurrent function execution. You can spawn tons of these if needed and the Go runtime multiplexes them onto the configured number of CPUs/Threads as needed. They start with a super small stack that can grow (and shrink) via dynamic allocation (and freeing). They are as simple as `go f(x)`, where `f()` is a function.

## Channels

The other 1/2 of Go's concurrency story. Channels are a typed conduit used to send and receive values. These are used to pass values to and from Goroutines for processing. They are, by default unbuffered, meaning synchronous and blocking. They can also be buffered, allowing values to queue up inside of them for processing. Multiple go routines can read/write to them at the same time w/o having to take locks. Go also has primitives for reading from multiple channels simultaneously via the `select` statement. 

## Compiled

Go compiles your program into a **static binary**. Yep, you read that correctly: **a static binary**. This makes deployment really simple, just copy over the binary. No bundler, no virtualenv, etc. All of that is handled at compile time. This simplifies deployment greatly.

## Runtime

Go is a complied language, but still has a runtime. It handles all of the details of mallocing memory for you, allocating/deallocating space for variables, etc. Memory used by variables lasts as long as the variables are referenced, which is usually the scope of a function. Go has a garbage collector.

## Pass By Value

Everything is passed by value, but there are pointers. It's just that the value of a pointer is a memory location, so it acts as pass by reference. This means that, by default, there is no shared state between functions. But, you can pass a pointer if you desire/need it for performance/memory utilization reasons. Go does the right thing by default, but doesn't shackle you. Oh, and there isn't any pointer math, so you won't screw yourself with it.

As pointed out on [HN](http://news.ycombinator.com/item?id=5196498), you can do pointer math with the ["unsafe"](http://golang.org/pkg/unsafe/) package and `unsafe.Pointer`. 

## Type System

Go has structs and interfaces. Go's structs can have methods *associated* with them. Structs can be anonymously included in other structs to make the inside struct's variables/methods available as part of the enclosting struct. Interfaces are sets of method signatures. Structs implement an interface by implementing the methods described by the interface definition. Functions can receive values by interface, like [so](http://play.golang.org/p/KfKLiAClQE). The compiler checks all of this at compile time.
## Package System

Or lack there of. Since Go compiles everything statically, you don't have to worry about packages at runtime. But how do you include libraries into your code? Simply by importing them via url, like so: `import "github.com/bmizerany/pq"`. Go's tooling knows how to look that up and clone the repo. Also works with Mercurial, Bazaar & Subversion.

## Anonymous Functions & Closures

Go supports anonymous functions that can form closures. These functions can then be passed around and retain the environment under which they were created, like [so](http://play.golang.org/p/4rYrqw4l7m). This can be super powerful when [combined](https://github.com/freeformz/filechan) with channels and go rountines.

## Built in profiling

It supports pprof as part of the standard library. And with a [very small bit of work](http://golang.org/pkg/net/http/pprof/), you can access profiling info via a http interface.

## Defer

Ever forget to close a file descriptor or free some memory? So have the designers of Go. The reason for this is that you usually have to perform those actions far away from where the resources were opened/allocated. Go solves this problem with the `defer` statement. Each `defer` statement pushes a function/expression onto a stack that get's executed in LIFO order after the enclosing function returns. This makes cleanup really easy, after opened a file, just `defer file.Close()`.

## Comprehensive Standard Library

Go's [standard library](http://golang.org/pkg/) is pretty comprehensive.

## It's Fast

Go compiles down to native code and it does it fast. Usually in just a few seconds.

## Comprehensive Tooling Out Of The Box

Go do a `go --help`. Some of my favorites: `go fmt`, `go doc`, `go fix` `go get` `go tool pprof` & `go test`.

## Straight Forward Testing

Super [straight foward](http://golang.org/pkg/testing/), no magic.

## Simple C Interface

By using build directives you can integrate with C libraries really [easily](https://gist.github.com/freeformz/4552031).

## Straight Forward Syntax / Standard Formatting

The syntax is pretty simple, C like w/o all the crazy of C. But what's really nice is `go fmt`, which re-writes your code into the Go standard format. No more arguments over coding style!

# Issues

With all of that said, it's not perfect...

## Runtime 

Go's runtime is not super [tuned](http://golang.org/doc/faq#Why_does_Go_perform_badly_on_benchmark_x) yet. By comparison the JVM has had over 18 years of development history behind it. But, for a 1.0.X runtime & language, Go is pretty damn good.

## Garbage Collector

Go programs can malloc a lot of ram at times, even when the program itself isn't using much. Most of this shows up as VIRT though, so most linux systems just won't care.

## 1 CPU by default

This is going to change over time, but the runtime will, by default, use only [one CPU](http://golang.org/doc/faq#Why_no_multi_CPU).