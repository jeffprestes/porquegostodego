# Porque gosto de Go

Vou a muitos eventos de desenvolvedores em geral, muitos estranham a minha motivação tão grande por Go e me fazem essa pergunta. Daí encontrei o artigo do [Freeformz](https://github.com/freeformz/) e achei a ideia de deixar isso no Github muito boa. Ao ler o artigo dele encontrei muitas coisas que concordo então resolvi fazer um fork e escrever a minha versão em português. O intuíto é deixar claro as vantagens de Go e ajudar desenvolvedores a convencerem seus gerentes e outros colaboradores a iniciarem a adoção de Go.

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

Como mencionado no [HN](http://news.ycombinator.com/item?id=5196498), você até pode fazer cálculo de ponteiro com o pacote ["unsafe"](http://golang.org/pkg/unsafe/) `unsafe.Pointer`. 

## Sistema de tipagem

Go tem structs (estruturas de dados) e interfaces. Structs em Go podem ter métodos *associados* a eles. Structs podem ser anonimamente incluídos em outras structs, permitindo assim que as variávies e métodos dessas structs sejam parte dessa outra struct maior. As interfaces em Go são um conjunto de assinatura de metodos. Structs implementam uma interface simplesmente implementando os métodos descritos na definição da interface. Funções podem receber valores pelas interfaces, como [nesse exemplo aqui](http://play.golang.org/p/KfKLiAClQE). O compilador checa tudo isso em tempo de compilação.     
## Sistema de pacote

Ou a falta dele. Como o Go compila tudo estaticamemte, você não tem de se preocupar sobre os pacotes em tempo de execução. Porém, como você incluí bibliotecas no seu código? Simplesmente importando-as via URL: `import "github.com/bmizerany/pq"`. As ferramentas em Go sabem como localizá-la e clonar o repositório. Também funciona com Mercurial, Bazaar e Subversion.

## Funções Anonimas & Closures

Go suporta funções anonimas que podem formar closures. Essas funções podem então ser passadas e reter o ambiente (variáveis, por exemplo) sob o qual foram criadas, como [aqui](http://play.golang.org/p/4rYrqw4l7m). Isso pode ser muito poderoso quando [combinados](https://github.com/freeformz/filechan) com channels e goroutines. 

## Profiling nativo

Go suporta pprof como parte do pacote padrão. E com um [um pouquinho de trabalho](http://golang.org/pkg/net/http/pprof/) você pode acessar dados de profiling via HTTP.

## Defer

Quem nunca esqueceu de fechar um arquivo ou liberar outros recursos de I/O como conexão com Banco de Dados? O mesmo aconteceu com os designers do Go. A razão desse problema é que geralmente você precisa fechar/encerar essas recursos longe de onde esses arquivos/conexões foram abertos. Go resolve esse problema com o comando `defer`. Cada comando `defer` enfilera o comando de fechamento/encerramemto para ser executado logo após o comando de retorno da função, ou seja, quando a execução da função for encerrada. Ou seja, depois de abrir um arquivo só chamar `defer file.Close()`.

## Biblioteca padrão extensa e completa

[Biblioteca padrão](http://golang.org/pkg/) do Go é bem completa e extensa.

## É rápido!

Go compila para código nativo e isso o torna rápido. A execução chegando-se até a nanosegundos em alguns casos.

## Completo conjunto de comandos para ajudar no desenvolvimento

Execute `go --help` e dê uma lida. Alguns dos meus favoritos: `go fmt`, `go doc`, `go fix` `go get` `go tool pprof` & `go test`.

## Testes sem complicação

Super [simples](http://golang.org/pkg/testing/), sem mágicas.

## Interface com C de forma simples

Por utilizar diretivas de compilação você pode integrar sua aplicação Go com bibliotecas C muito [facilmente](https://gist.github.com/freeformz/4552031)

## Sintaxe simples / Formatação de código padronizado

A sintaxe é muito simples. Porém umas das coisas que gosto é o `go fmt`, assim não preciso me preocupar com a formatação e estilo do meu código. Ele reescreve meu código para o estilo padrão de Go. Nunca mais aquelas discussões de onde as chaves ( { } ) devem ficar depois do if ;)
