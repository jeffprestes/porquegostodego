# Porque gosto de Go

Vou a muitos eventos de desenvolvedores em geral, muitos estranham a minha motivação tão grande por Go e me fazem essa pergunta. Daí encontrei o artigo do [Freeformz](https://github.com/freeformz/) e achei a ideia de deixar isso no Github muito boa. Ao ler o artigo dele encontrei muitas coisas que concordo então resolvi fazer um fork e escrever a versão em português. O intuíto é deixar claro as vantagens de Go e ajudar desenvolvedores a convencerem seus gerentes e outros colaboradores a iniciarem a adoção de Go.

## Goroutines

A primeira metade da história de Concorrência em Go. Execução leve de funções concorrentes. Você pode criar e executar toneladas dessas funções, se necessário, e o runtime do Go faz a multiplexação delas num numero configurado de CPU/Threads do sistema operacional à medida que for necessário. Elas inicial com uma minúscula stack que pode crescer (e diminuir) via alocação dinâmica (e liberação) de memória. Elas são tão simples quanto `go f(x)` onde `f()` é uma funcão.

## Channels

A outra parte da história de Concorrência em Go. Channels ou canais são uma espécie de "conduíte" tipado usado para receber e enviar valores entre as Goroutines. Elas são, por padrão, não bufferizadas, ou seja síncronas e bloqueantes. Elas também podem ser bufferizadas, permitindo que sejam enfileiradas internamente para processamento. Dessa maneira, multiplas Goroutines podem ler/escrever nos canais ao mesmo tempo sem "travar". Go também tem tipos primitivos para leitura de multiplus canais simultâneamente via comando `select`. 

## Compilado

Go compila seu programa num **binário estático**. Sim, você leu corretamente: **binário estático**. Isso faz que deploy de aplicações Go sejam extremamente simples, só sobreescrever o binário existente no servidor ou máquina cliente. Sem mais bundler, virtualenv, etc. Tudo isso é gerenciado em tempo de compilação. Isso simplifica **MUITO** o deploy.

## Runtime

Go é uma linguagem compilada mas ainda sim tem um runtime. Ele gerencia todos os detalhes do gerenciamento de memória a você. Memória usada por variáveis duram enquanto as variáveis são referenciadas, que usualmente é só dentro do escopo da funcão. Go tem garbage collector.

## Passagem por valor

Tudo é passado por valor, mas também há ponteiros. Vale esclarecer que um ponteiro é uma locação de memória então ele age como passando por referência. Isso significa que, por padrão, não há estado compartilhado de memória entre as funções. Entretanto, se você realmente precisar, você pode passar um ponteiro por questões de performance e quantidade de memória. O Go faz a coisa certa por padrão. Ah! Não tem cálculo de ponteiro, por padrão, ou seja, você não poderá fazer besteira contra si mesmo sem querer.

Como mencionado em [HN](http://news.ycombinator.com/item?id=5196498), você até pode fazer cálculo de ponteiro com o pacote ["unsafe"](http://golang.org/pkg/unsafe/) `unsafe.Pointer`. 

## Sistema de tipagem

Go tem structs (estruturas de dados) e interfaces. Structs em Go podem ter métodos *associados* a eles. Structs podem ser anonimamente incluídos em outras structs, permitindo assim que as variávies e métodos dessas structs sejam parte dessa outra struct maior. As interfaces em Go são um conjunto de assinatura de metodos. Structs implementam uma interface somente implementando os métodos descritos na definição da interface. Funções podem receber valores pelas interfaces, como [em](http://play.golang.org/p/KfKLiAClQE). O compilador checa tudo isso em tempo de compilação.

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
