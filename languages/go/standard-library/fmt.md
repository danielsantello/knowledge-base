# fmt

<a href="/README.md">Base de Conhecimento</a> › <a href="/languages/go/go.md">Go</a> › <a href="/languages/go/standard-library/standard-library.md">Biblioteca Padrão</a>

## Sumário
- [Introdução](#introdução)
- [Principais comandos](#principais-comandos)
  - [fmt.Print()](#fmtprint)
  - [fmt.Println()](#fmtprintln)
  - [fmt.Printf()](#fmtprintf)
  - [fmt.Sprintf()](#fmtsprintf)
  - [fmt.Errorf()](#fmterrorf)
- [Strings de formatação](#strings-de-formatação)
- [Verbos para Strings](#verbos-para-strings)
- [Verbos para Inteiros](#verbos-para-inteiros)
- [Verbos para Float](#verbos-para-float)
- [Verbos para Boolean](#verbos-para-boolean)
- [Verbos genéricos](#verbos-genéricos)
- [Ponteiros](#ponteiros)
- [Sequências especiais](#sequências-especiais)
- [Raw Strings](#raw-strings)
- [Funções variádicas](#funções-variádicas)
- [Resumo](#resumo)
  - [Escolha do comando](#escolha-do-comando)
  - [Verbos mais utilizados](#verbos-mais-utilizados)
- [Boas práticas](#boas-práticas)

<br>

### Introdução

O pacote `fmt` pertence à **Go Standard Library** e fornece funções para:

- Imprimir informações no terminal.
- Formatar valores para exibição.
- Construir strings formatadas.
- Criar mensagens de erro formatadas.
- Ler dados da entrada padrão (teclado).

Documentação oficial:

https://pkg.go.dev/fmt

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Principais comandos
#### fmt.Print()

Imprime exatamente os valores informados.

Características:

- Não adiciona quebra de linha.
- Não adiciona espaços automaticamente.
- Aceita um ou vários argumentos.

```go
fmt.Print("Olá")
fmt.Print(" Mundo")
```

Resultado:
```text
Olá Mundo
```

Também é possível passar vários argumentos:

```go
nome := "Daniel"
idade := 30

fmt.Print(nome, " tem ", idade, " anos.")
```

Resultado:
```text
Daniel tem 30 anos.
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

#### fmt.Println()

Semelhante ao `Print()`, porém:

- adiciona espaços entre os argumentos;
- adiciona uma quebra de linha ao final.

```go
fmt.Println("Olá", "Mundo")
```

Resultado:

```text
Olá Mundo
```

Outro exemplo:

```go
nome := "Daniel"
idade := 30

fmt.Println("Nome:", nome, "Idade:", idade)
```

Resultado:

```text
Nome: Daniel Idade: 30
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

#### fmt.Printf()

Permite imprimir valores formatados utilizando verbos de formatação.

```go
nome := "Daniel"
idade := 30

fmt.Printf("Nome: %s | Idade: %d\n", nome, idade)
```

Resultado:
```text
Nome: Daniel | Idade: 30
```

Diferentemente do `Print()` e `Println()`, o `Printf()` utiliza uma **string de formatação**, onde cada verbo (`%d`, `%s`, `%v`, etc.) será substituído pelo respectivo argumento informado.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

#### fmt.Sprintf()

Possui exatamente o mesmo comportamento do `Printf()`, porém retorna uma string ao invés de imprimir.

```go
texto := fmt.Sprintf("Nome: %s", nome)

fmt.Println(texto)
```

Resultado:
```text
Nome: Daniel
```

Muito utilizado para montar mensagens.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

#### fmt.Errorf()

Cria um erro formatado.

```go
err := fmt.Errorf("cliente %d não encontrado", id)
```

Muito utilizado para retornar erros.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Strings de formatação

As strings de formatação são utilizadas principalmente pelo `fmt.Printf()`, `fmt.Sprintf()` e `fmt.Errorf()`.

Exemplo:
```go
fmt.Printf("Nome: %s | Idade: %d\n", nome, idade)
```

Nesse exemplo:

- `%s` será substituído pelo valor da variável `nome`;
- `%d` será substituído pelo valor da variável `idade`;
- `\n` adicionará uma quebra de linha.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Verbos para Strings

| Verbo | Descrição | Exemplo |
|--------|-----------|----------|
| `%s` | String | Daniel |
| `%q` | String entre aspas | `"Daniel"` |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Verbos para Inteiros

| Verbo | Descrição | Exemplo |
|--------|-----------|----------|
| `%d` | Decimal | 30 |
| `%b` | Binário | 11110 |
| `%o` | Octal | 36 |
| `%x` | Hexadecimal | 1e |
| `%X` | Hexadecimal maiúsculo | 1E |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Verbos para Float

| Verbo | Descrição | Exemplo |
|--------|-----------|----------|
| `%f` | Float | 123.456000 |
| `%.2f` | Float com duas casas decimais | 123.46 |
| `%.4f` | Float com quatro casas decimais | 123.4560 |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Verbos para Boolean

| Verbo | Descrição |
|--------|-----------|
| `%t` | Boolean |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Verbos genéricos

| Verbo | Descrição |
|--------|-----------|
| `%v` | Valor padrão |
| `%+v` | Exibe structs com o nome dos campos |
| `%#v` | Exibe structs utilizando a sintaxe Go |

Exemplo:

```go
fmt.Printf("%v\n", cliente)
fmt.Printf("%+v\n", cliente)
fmt.Printf("%#v\n", cliente)
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Ponteiros

| Verbo | Descrição |
|--------|-----------|
| `%p` | Endereço de memória |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Sequências especiais

| Sequência | Descrição |
|------------|-----------|
| `\n` | Nova linha |
| `\t` | Tabulação |
| `\\` | Barra invertida |
| `\"` | Aspas |
| `%%` | Imprime o caractere `%` |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Raw Strings

Strings delimitadas por crases (**`**) não interpretam caracteres especiais.

```go
codigo := `fmt.Printf("Valor %d\n", valor)`

fmt.Println(codigo)
```

Resultado:
```text
fmt.Printf("Valor %d\n", valor)
```

Muito útil para:

- SQL
- JSON
- HTML
- Expressões regulares
- Exemplos de código

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Funções variádicas

As funções `Print()`, `Println()` e `Printf()` aceitam uma quantidade variável de argumentos.

Isso é possível porque sua assinatura utiliza o operador `...`.

Simplificando, elas são declaradas da seguinte forma:

```go
func Print(a ...any)

func Println(a ...any)

func Printf(format string, a ...any)
```

O trecho:
```go
...any
```

significa:

> "A função pode receber zero, um ou vários argumentos."

Exemplos:
```go
fmt.Print("A")
fmt.Print("A", "B")
fmt.Print("A", "B", "C")
```

Todos são válidos.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Resumo
#### Escolha do comando

| Comando | Quando utilizar |
|----------|-----------------|
| `Print()` | Texto simples, sem quebra de linha |
| `Println()` | Texto simples com quebra de linha |
| `Printf()` | Texto formatado |
| `Sprintf()` | Criar uma string formatada |
| `Errorf()` | Criar um erro formatado |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

#### Verbos mais utilizados

| Verbo | Tipo |
|--------|------|
| `%s` | String |
| `%d` | Inteiro |
| `%f` | Float |
| `%t` | Boolean |
| `%v` | Valor padrão |
| `%+v` | Struct detalhada |
| `%#v` | Struct em sintaxe Go |
| `%p` | Ponteiro |

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Boas práticas

- Utilize `Println()` para mensagens simples no terminal.
- Utilize `Printf()` quando precisar formatar valores.
- Utilize `Sprintf()` quando precisar montar uma string.
- Utilize `%+v` e `%#v` durante a depuração de structs.
- Utilize Raw Strings (crases) para SQL, JSON e exemplos de código.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>
