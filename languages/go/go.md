# Go

<a href="/README.md">Base de Conhecimento</a>

## Sumário

<details open>
<summary><strong>Conceitos</strong></summary>

- [Características](#características)
- [O que Go NÃO é](#o-que-go-não-é)
- [Motivação](#motivação)
- [Principais Diferenciais](#principais-diferenciais)
- [Casos de Uso](#casos-de-uso)
- [Filosofia da Linguagem](#filosofia-da-linguagem)
- [Instalação](#instalação)
- [Comandos](#comandos)
	- [Declaração e atribuição](#declaração-e-atribuição)
- [Biblioteca Padrão](/languages/go/standard-library/standard-library.md)
	- [fmt](/languages/go/standard-library/fmt.md)
- [Módulos e Pacotes](#módulos-e-pacotes) 
- [Exemplos](#exemplos)
	- [Hello](#hello)
- [Referências](#referências)
	- [Cursos](#cursos)
	- [Documentação Oficial](#documentação-oficial)
	- [Ferramentas Online](#ferramentas-online)
</details>

<details open>
<summary><strong>Estruturas de Dados</strong></summary>

- [Array](go/array.md)
- [Slice](go/slice.md)
</details>

<br>

### Características
- Open Source
- Foco em produtividade (faz muito com pouco)
- Expressiva, concisa, limpa e eficiente
- Moderna, aproveita ao máximo recursos multicore e de rede
- Rápida compilação e ao mesmo tempo trabalha com garbage collection
- Estaticamente tipada, compilada e ao mesmo tempo parece até uma linguagem interpretada
- Compilada em apenas um arquivo binário
- Nasceu na Google (começou a ser projetada em Sep - 2007)
- Versão 1.0 nasceu em 2012
- A partir da v1.5 - Próprio compilador foi feito em GO
- Forte compromisso com compatibilidade retroativa desde a versão 1 (o software feito na 1.15 vai funcionar na 1.16)
- Criada por:
  - Rob Pike - UNIX, Plan 9 e UTF-8
  - Robert Griesemar - V8 (motores dos browser, node.js)
  - Ken Thompson - UNIX, B e UTF-8

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### O que Go NÃO é
- Linguagem de programação que resolverá todos os problemas
- Não é dinâmica (é estaticamente tipada)
- Não é uma linguagem interpretada (é compilada)
- Não é uma linguagem com muitos recursos / firulas
- Não é uma linguagem orientada a objetos

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Motivação
- Limitações nas principais linguagens utilizadas na Google (Python, Java e C++)
- Python: problemas com lentidão
- C/C++: muita complexidade e demorado para compilar
- Java: complexidade gerada ao longo do tempo / verbosidade da linguagem
- Multithreading e concorrência: não nasceram pensando nisso
- Simplicidade
- Framework de testes e profiling nativos
- Detecção de Race Conditions
- Deploy simples (apenas um arquivo binário)
- Baixa curva de aprendizado

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Principais Diferenciais
- Compilação extremamente rápida
- Binário único para distribuição
- Garbage Collector nativo
- Concorrência através de Goroutines
- Comunicação entre processos através de Channels
- Biblioteca padrão muito completa
- Ferramentas nativas de testes
- Ferramentas nativas de profiling
- Ferramentas nativas para detecção de Race Conditions

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Casos de Uso
- APIs REST
- Microserviços
- Ferramentas de linha de comando (CLI)
- ETLs
- Sistemas distribuídos
- Aplicações de rede
- Containers e infraestrutura

Tecnologias conhecidas escritas em Go:
- Docker
- Kubernetes
- Terraform
- Consul
- Vault
- Prometheus

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Filosofia da Linguagem
- Simplicidade acima de flexibilidade
- Legibilidade acima de abstrações complexas
- Composição acima de herança
- Convenção acima de configuração
- Menos recursos para reduzir complexidade

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Instalação
Instalando no Ubuntu via WSL:
```sh
sudo apt update
sudo apt install golang-go
```

Conferindo a versão instalada:
```sh
go version
```

Resultado esperado:
```sh
go version goX.Y.Z linux/amd64
```

Exibindo as variáveis de ambiente que o Go utiliza:
```sh
go env
```

Algumas variáveis importantes:
- `GOPATH` → diretório de trabalho do Go, utilizado para armazenar binários, cache e arquivos relacionados ao desenvolvimento
- `GOMODCACHE` → armazena os arquivos do gerenciador de dependências

Adicionando o Go no PATH do sistema:
```sh
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc
source ~/.bashrc
```

> **Observação:** o primeiro comando adiciona a configuração permanentemente ao arquivo `.bashrc`.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Comandos
#### Declaração e atribuição
```go

```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Módulos e Pacotes
Quando instalamos o Go, algumas variáveis de ambiente são configuradas. Uma delas é a `GOPATH`, que representa o diretório de trabalho do Go. Atualmente, com o uso de módulos (`go.mod`), ela deixou de definir onde os pacotes do projeto ficam, sendo utilizada principalmente para armazenar binários, cache e dependências baixadas.

```sh
go env GOPATH
```

Resultado:
```sh
/home/dalq/go
```

Cada projeto Go moderno é organizado como um módulo. Um módulo é criado através do comando `go mod init` e define o caminho base utilizado pelos imports do projeto.

Para isso, vamos criar uma pasta onde ficará o nosso projeto:

```sh
cd /home/dalq/go
mkdir packaging
cd packaging
```

Agora, dentro da pasta `/home/dalq/go/packaging` digitamos:

```sh
go mod init <nome_do_projeto>
```

Por convenção, usamos como nome do projeto o caminho do repositório no GitHub. Esse caminho não precisa existir no GitHub naquele momento. Ele apenas será utilizado como identificador do módulo. Para esse exemplo, ficará assim:

```sh
go mod init github.com/danielsantello/packaging
```

Será criado um arquivo chamado `go.mod` com o seguinte conteúdo:

```sh
module github.com/danielsantello/packaging

go 1.25.0
```

#### Diferença entre módulo e pacote
```sh
Módulo
└── github.com/danielsantello/packaging

Pacotes do módulo
├── math
├── crypto
├── network
└── wallet
```

Um módulo pode conter vários pacotes:
```sh
Projeto
│
├── Módulo
│   └── definido no go.mod
│
├── Pacotes
│   └── um por diretório
│
└── Arquivos
    └── pertencem a um pacote
```

Agora vamos criar um pacote chamado `math` e importá-lo em outro pacote (`main`).

Segue abaixo uma visão das pastas:
```sh
packaging/
│
├── go.mod
│
├── math/
│   └── math.go
│
└── cmd/
    └── main.go
```

Primeiro, criaremos uma pasta chamada `math` na raiz do projeto. Dentro dela, um arquivo chamado `math.go` com o seguinte conteúdo:

```sh
package math

type Math struct {
	A int
	B int
}

func (m Math) Add() int {
	return m.A + m.B
}
```

Agora, vamos criar o arquivo principal na pasta `cmd` chamado `main.go` com o seguinte conteúdo:

```sh
package main

import (
	"fmt"
	"github.com/danielsantello/packaging/math"
)

func main() {
	m := math.Math{A: 1, B: 2}
	fmt.Println(m.Add())
}
```

O caminho utilizado no import sempre começa pelo nome do módulo definido no `go.mod` e termina com o pacote que desejamos importar.

Exemplo:
```sh
github.com/danielsantello/packaging/math
│──────────────────────────────────│└── pacote
           módulo
```

Resumo:
```sh
go mod init
    ↓
cria um módulo

go.mod
    ↓
define o módulo do projeto

package
    ↓
organiza o código

import
    ↓
utiliza outros pacotes
```

> [!NOTE]
> Em Go, cada diretório normalmente representa um pacote.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Exemplos
#### Hello
Criar um arquivo chamado `main.go` com o seguinte conteúdo:
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello")
}
```

Executar:
```sh
go run main.go
```

Resultado esperado:
```sh
Hello
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Referências
#### Cursos
- [Go Expert - Full Cycle](https://github.com/devfullcycle/goexpert)  
  Repositório oficial do curso de Pós-Graduação Go Expert da Full Cycle.

#### Documentação Oficial
- [Go.dev](https://go.dev/)  
  Site oficial da linguagem Go.

- [Tour of Go](https://go.dev/tour/)  
  Tutorial interativo oficial.

#### Ferramentas Online
- [Go Playground](https://go.dev/play/)  
  The Go Playground.

- [Go by Example](https://gobyexample.com/)  
  Exemplos práticos da linguagem.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>
