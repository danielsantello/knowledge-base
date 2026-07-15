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

<details open>
<summary><strong>Biblioteca Padrão</strong></summary>

- [fmt](/languages/go/standard-library/fmt.md)
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
