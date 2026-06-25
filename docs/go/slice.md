# [Go](../golang.md) - Slice

## Sumário
- [O que é um Slice?](#o-que-é-um-slice)
- [Declarando um slice](#declarando-um-slice)
- [Acessando um elemento](#acessando-um-elemento)
- [O operador ":"](#o-operador-)
  - [Omitindo o início](#omitindo-o-início)
  - [Omitindo o fim](#omitindo-o-fim)
  - [Copiando tudo](#copiando-tudo)
- [Compartilhamento de memória](#compartilhamento-de-memória)
- [Um exemplo prático](#um-exemplo-prático)
- [Len e Cap](#len-e-cap)
- [Sintaxe completa (menos usada)](#sintaxe-completa-menos-usada)
- [Como o slice é representado internamente](#como-o-slice-é-representado-internamente)
- [Append](#append)
  - [Cenário 1 - Ainda existe capacidade](#cenário-1---ainda-existe-capacidade)
  - [Cenário 2 - Capacidade esgotada](#cenário-2---capacidade-esgotada)
  - [Um erro clássico](#um-erro-clássico)
  - [Pré-alocando memória](#pré-alocando-memória)
- [Código com as explicações e os exemplos](#código-com-as-explicações-e-os-exemplos)

### O que é um Slice?
> Um slice é uma estrutura que referencia uma parte de um array.  
> Ele não possui os dados; apenas descreve uma janela sobre um array existente.  
> É uma das estruturas mais utilizadas da linguagem Go.

### Declarando um slice
Para slices, não definimos tamanho.  
Internamente todo slice referencia um array.
```go
numeros := []int{10, 20, 30, 40, 50}
```

Visualmente:
```sh
Array real na memória
+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
  0    1    2    3    4

Slice
  |
  +--> aponta para o array
```

### Acessando um elemento
```go
fmt.Println(numeros[2])
```

Resultado:
```sh
30
```

### O operador ":"
A sintaxe é:
```go
slice[inicio:fim]
```

> [!IMPORTANT]
> - inclui o índice inicial
> - exclui o índice final

Exemplo:
```go
numeros := []int{10, 20, 30, 40, 50}
fmt.Println(numeros[1:4])
```

Resultado:
```sh
[20 30 40]
```

Porque:
```sh
Indice:   0   1   2   3   4
Valor:   10  20  30  40  50
              ↑           ↑
            inicio       fim

O elemento do índice 4 não entra
```

#### Omitindo o início
Se não informar o início:
```go
numeros[:3]
```

é igual a:
```go
numeros[0:3]
```

Resultado:
```sh
[10 20 30]
```

#### Omitindo o fim
Se não informar o fim:
```go
numeros[2:]
```

é igual a:
```go
numeros[2:5]
```

Resultado:
```sh
[30 40 50]
```

#### Copiando tudo
```go
numeros[:]
```

Resultado:
```sh
[10 20 30 40 50]
```

> [!NOTE]
> Muito usado quando se quer passar o slice inteiro.

### Compartilhamento de memória
> [!IMPORTANT]
> O slice NÃO copia os dados.  
> Este é o comportamento que mais costuma confundir quem está começando.

Veja:
```go
numeros := []int{10, 20, 30, 40, 50}
parte := numeros[1:4]
fmt.Println(parte)
```

Resultado:
```sh
[20 30 40]
```

Mas agora:
```go
parte[0] = 999

fmt.Println(parte)
fmt.Println(numeros)
```

Resultado:
```sh
[999 30 40]
[10 999 30 40 50]
```

Por quê?

Porque `parte` e `numeros` apontam para o mesmo array.

Visualmente:
```sh
Array
+----+-----+----+----+----+
| 10 | 999 | 30 | 40 | 50 |
+----+-----+----+----+----+
       ^
       |
     parte[0]
```

### Um exemplo prático
Imagine que você esteja processando um arquivo gigante.

Você lê um lote de registros:
```go
lote := registros[0:1000]
```

Depois:
```go
lote = registros[1000:2000]
```

Depois:
```go
lote = registros[2000:3000]
```

> [!NOTE]
> - Nenhuma cópia é feita.
> - Você apenas cria novas `"janelas"` sobre o mesmo conjunto de dados.
> - Por isso slices são extremamente eficientes para processar grandes volumes de dados.

### Len e Cap
Todo slice possui:
  - `len` (quantos elementos ele vê)
  - `cap` (quantos elementos existem a partir da posição inicial)

Exemplo:
```go
numeros := []int{10, 20, 30, 40, 50}

parte := numeros[1:4]

fmt.Println(parte)
fmt.Println(len(parte))
fmt.Println(cap(parte))
```

Temos:
```sh
[20 30 40]
3
4
```

Porque?
```sh
Indice:   0   1   2   3   4
Valor:   10  20  30  40  50
              ^
            parte começa aqui
```

Da posição 1 até o final do array existem:

```sh
20 30 40 50
```

4 elementos, logo:
```sh
fmt.Println(len(parte)) // 3
fmt.Println(cap(parte)) // 4
```

### Sintaxe completa (menos usada)
Existe uma terceira posição:
```go
slice[inicio:fim:capacidade]
```

Exemplo:
```go
numeros := []int{10,20,30,40,50}
parte := numeros[1:4:4]
```

Aqui você controla a capacidade do novo slice.  
Isso é usado quando quer evitar que um append() modifique o array original.


### Como o slice é representado internamente
Um slice não é exatamente um ponteiro.

O slice é uma estrutura pequena (header) composta por:
  - ponteiro para o primeiro elemento;
  - quantidade de elementos visíveis (len);
  - capacidade restante (cap).

Internamente ele é uma estrutura parecida com:
```go
type slice struct {
    ptr *int
    len int
    cap int
}
```

> [!NOTE]
> Não é exatamente isso, mas ajuda a entender.

Então para:
```go
completo := []int{10, 20, 30, 40, 50}
parte := completo[3:4]
```

Teríamos algo como:
```sh
ptr -> posição do valor 40
len = 1
cap = 2
```

Visualmente:
```sh
Array original
+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
                 ^
                 |
                ptr

len = 1
cap = 2
```

> [!IMPORTANT]
> - Perceba que ele NÃO aponta para o início do array.
> - Ele aponta para o primeiro elemento visível do slice.

### Append
#### Cenário 1 - Ainda existe capacidade
```go
numeros := []int{10, 20, 30, 40, 50}
parte := completo[3:4]
```

Visualmente:
```sh
Array
+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
                 ^
                 |
               parte
len = 1
cap = 2
```

Agora:
```go
parte = append(parte, 999)
```

O Go analisa: ainda tenho espaço dentro da capacidade do slice?

Então ele simplesmente escreve no array existente.

Resultado:
```sh
Array
+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+
```

Se printarmos os valores:
```go
fmt.Println(numeros)
fmt.Println(parte)
```

Resultado:
```sh
[10 20 30 40 999]
[40 999]
```

#### Cenário 2 - Capacidade esgotada
```go
numeros := []int{10, 20, 30, 40, 50}
parte := completo[3:4]
```

Temos:
```sh
len = 1
cap = 2
```

Primeiro append:
```go
parte = append(parte, 999)
```

Agora:
```sh
len = 2
cap = 2
```

Capacidade cheia.

Antes de fazermos um novo append, vamos imprimir o endereço para onde `parte` está apontando:
```go
fmt.Printf("%p\n", &parte[0])
```

Resultado:
```sh
0x1071890060a8
```

Se fizer outro append:
```go
parte = append(parte, 888)
```

Então ele:
  - Cria um novo array maior.
  - Copia os dados.
  - Faz o slice apontar para o novo array.

Antes:
```sh
numeros
+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+
```

Depois:
```sh
numeros
+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+

parte
+----+-----+-----+
| 40 | 999 | 888 |
+----+-----+-----+
```

Agora são arrays diferentes.

Vamos imprimir novamente o endereço para ver onde `parte` está apontando agora:
```go
fmt.Printf("%p\n", &parte[0])
```

Resultado:
```sh
0x107189010000
```

**Detalhe MUITO importante**

Muita gente escreve:
```go
append(parte, 123)
```

Mas o correto é:
```go
parte = append(parte, 123)
```

Porque o append pode:
  - continuar usando o mesmo array
  - ou criar um array novo

Como você não sabe qual dos dois aconteceu, ele devolve o slice atualizado.

#### Um erro clássico
```go
func adiciona(s []int) {
    s = append(s, 999)
}

numeros := []int{1,2,3}
adiciona(numeros)
fmt.Println(numeros)
```

Muita gente espera:
```sh
[1 2 3 999]
```

Mas pode aparecer:
```sh
[1 2 3]
```

Porque a função recebeu uma cópia da estrutura do slice.

Se o append precisou realocar memória, somente a variável local foi atualizada.

O slice original continuou apontando para o array antigo.

O correto:
```go
func adiciona(s []int) []int {
    return append(s, 999)
}

numeros = adiciona(numeros)
```

Resumo:
```sh
append()
↓
Existe capacidade?

SIM
↓
Escreve no mesmo array

NÃO
↓
Cria novo array
Copia os dados
Atualiza o ponteiro
```

### Pré-alocando memória
Imagine que você sabe que vai carregar 1 milhão de registros.

Ao invés de deixar o Go ficar aumentando o array várias vezes:
```go
var registros []Registro
```

Você pode fazer:
```go
registros := make([]Registro, 0, 1000000)
```

Resultado:
```sh
len = 0
cap = 1000000
```

Ou seja:

- Espaço reservado para 1 milhão
- Elementos utilizados = 0

Agora os append() vão ser muito mais eficientes.

Como a capacidade já foi reservada:
- menos alocações  
- menos cópias  
- menos pressão no garbage collector  
- mais velocidade  

> [!NOTE]
> Em importações gigantes (100 GB, 200 GB+) isso faz diferença real.

### Código com as explicações e os exemplos
```go
package main

import "fmt"

func main() {
	// Um slice é uma "janela" para um array
	titulo(`Um slice é uma "janela" para um array`)
	fmt.Printf("\n")

	// Declarando um slice
	titulo(`Declarando um slice
Para slices, não definimos tamanho
Internamente o Go cria um array e o slice aponta para ele.`)
	fmt.Println(`numeros := []int{10, 20, 30, 40, 50}

Visualmente:

Array real na memória

+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
  0    1    2    3    4

Slice
  |
  +--> aponta para o array`)
	fmt.Printf("\n")

	numeros := []int{10, 20, 30, 40, 50}

	// Acessando um elemento
	titulo("Acessando um elemento")
	fmt.Println(`fmt.Println(numeros[2])`)
	fmt.Printf("\n")

	fmt.Println("Resultado:")
	fmt.Println(numeros[2])
	fmt.Printf("\n")

	// O operador ":"
	titulo(`O operador ":"`)
	fmt.Println(`A sintaxe é:
slice[inicio:fim]

Importante:
  - inclui o índice inicial
  - exclui o índice final

Exemplo:
numeros := []int{10, 20, 30, 40, 50}
fmt.Println(numeros[1:4])

Resultado:`)
	fmt.Println(numeros[1:4])
	fmt.Printf("\n")

	fmt.Println(`Porque:

Indice:   0   1   2   3   4
Valor:   10  20  30  40  50
              ↑           ↑
            inicio       fim

O elemento do índice 4 não entra`)
	fmt.Printf("\n")

	// O operador ":" - Omitindo o início
	titulo(`O operador ":" - Omitindo o início`)
	fmt.Println(`Se não informar o início:
numeros[:3]

é igual a:
numeros[0:3]

Resultado:`)
	fmt.Println(numeros[0:3])
	fmt.Printf("\n")

	// O operador ":" - Omitindo o fim
	titulo(`O operador ":" - Omitindo o fim`)
	fmt.Println(`Se não informar o fim:
numeros[2:]

é igual a:
numeros[2:5]

Resultado:`)
	fmt.Println(numeros[2:5])
	fmt.Printf("\n")

	// O operador ":" - Copiando tudo
	titulo(`O operador ":" - Copiando tudo`)
	fmt.Println(`numeros[:]

Resultado:`)
	fmt.Println(numeros[:])
	fmt.Println(`Muito usado quando se quer passar o slice inteiro.`)
	fmt.Printf("\n")

	// O ponto que mais confunde
	titulo(`O ponto que mais confunde`)
	fmt.Println(`O slice NÃO copia os dados.

Veja:

numeros := []int{10, 20, 30, 40, 50}
parte := numeros[1:4]
fmt.Println(parte)

Resultado:`)
	parte := numeros[1:4]
	fmt.Println(parte)
	fmt.Printf("\n")

	fmt.Println(`Mas agora:
parte[0] = 999

fmt.Println(parte)
fmt.Println(numeros)

Resultado:`)
	parte[0] = 999
	fmt.Println(parte)
	fmt.Println(numeros)
	fmt.Printf("\n")

	fmt.Println(`Por quê?

Porque parte e numeros apontam para o mesmo array.

Visualmente:

Array

+----+-----+----+----+----+
| 10 | 999 | 30 | 40 | 50 |
+----+-----+----+----+----+
       ^
       |
     parte[0]`)
	fmt.Printf("\n")

	// Um exemplo prático
	titulo(`Um exemplo prático`)
	fmt.Println(`Imagine que você esteja processando um arquivo gigante.

Você lê um lote de registros:
lote := registros[0:1000]

Depois:
lote = registros[1000:2000]

Depois:
lote = registros[2000:3000]

Nenhuma cópia é feita.

Você apenas cria novas "janelas" sobre o mesmo conjunto de dados.

Por isso slices são extremamente eficientes para processar grandes volumes de dados.`)
	fmt.Printf("\n")

	// Len e Cap
	titulo(`Len e Cap`)
	fmt.Println(`Todo slice possui:
  - len (quantos elementos ele vê)
  - cap (quantos elementos existem a partir da posição inicial)

Exemplo:

numeros := []int{10, 20, 30, 40, 50}

parte := numeros[1:4]

fmt.Println(parte)
fmt.Println(len(parte))
fmt.Println(cap(parte))

Temos:`)
	numeros = []int{10, 20, 30, 40, 50}
	parte = numeros[1:4]

	fmt.Println(parte)
	fmt.Println(len(parte))
	fmt.Println(cap(parte))
	fmt.Printf("\n")

	fmt.Println(`Porque?
Indice:   0   1   2   3   4
Valor:   10  20  30  40  50
              ^
            parte começa aqui

Da posição 1 até o final do array existem:

20 30 40 50

4 elementos, logo:

fmt.Println(len(parte)) // 3
fmt.Println(cap(parte)) // 4`)
	fmt.Printf("\n")

	// Sintaxe completa (menos usada)
	titulo(`Sintaxe completa (menos usada)`)
	fmt.Println(`Existe uma terceira posição:

slice[inicio:fim:capacidade]	

Exemplo:

numeros := []int{10,20,30,40,50}
parte := numeros[1:4:4]

Aqui você controla a capacidade do novo slice.
Isso é usado quando quer evitar que um append() modifique o array original.`)
	fmt.Printf("\n")

	// Append
	raw_string := `fmt.Printf("%p\n", &parte[0])`

	// Append - Cenário 1 - Ainda existe capacidade
	titulo(`Append - Cenário 1 - Ainda existe capacidade`)
	fmt.Println(`numeros := []int{10, 20, 30, 40, 50}
parte := numeros[3:4]

Visualmente:
Array

+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
                 ^
                 |
               parte

len = 1
cap = 2

Agora:
parte = append(parte, 999)

O Go analisa: ainda tenho espaço dentro da capacidade do slice.

Então ele simplesmente escreve no array existente.

Array

+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+

Se printarmos os valores:

fmt.Println(numeros)
fmt.Println(parte)

Resultado:`)

	numeros = []int{10, 20, 30, 40, 50}
	parte = numeros[3:4]
	parte = append(parte, 999)

	fmt.Println(numeros)
	fmt.Println(parte)
	fmt.Printf("\n")

	// Append - Cenário 2 - Capacidade esgotada
	titulo(`Append - Cenário 2 - Capacidade esgotada`)
	fmt.Println(`numeros := []int{10, 20, 30, 40, 50}
parte := numeros[3:4]

Temos:
len = 1
cap = 2

Primeiro append:
parte = append(parte, 999)

Agora:
len = 2
cap = 2

Capacidade cheia.

Antes de fazermos um novo append, vamos imprimir o endereço para onde parte está apontando:`)
	fmt.Println(raw_string)
	fmt.Println(`

Resultado:`)
	numeros = []int{10, 20, 30, 40, 50}
	parte = numeros[3:4]
	parte = append(parte, 999)
	fmt.Printf("%p\n", &parte[0])
	fmt.Printf("\n")

	fmt.Println(`Se fizer outro append:
parte = append(parte, 888)

Então ele:
  - Cria um novo array maior.
  - Copia os dados.
  - Faz o slice apontar para o novo array.

Antes:
numeros

+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+

Depois:
numeros

+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+

parte

+----+-----+-----+
| 40 | 999 | 888 |
+----+-----+-----+

Agora são arrays diferentes.

Vamos imprimir novamente o endereço para ver onde parte está apontando agora:`)
	fmt.Println(raw_string)
	fmt.Println(`

Resultado:`)
	parte = append(parte, 888)
	fmt.Printf("%p\n", &parte[0])
	fmt.Printf("\n")

	// Append - Detalhe MUITO importante
	titulo(`Append - Detalhe MUITO importante`)
	fmt.Println(`Muita gente escreve:
append(parte, 123)

Mas o correto é:
parte = append(parte, 123)

Porque o append pode:
  - continuar usando o mesmo array
  - ou criar um array novo

Como você não sabe qual dos dois aconteceu, ele devolve o slice atualizado.`)
	fmt.Printf("\n")

	// Append - Um erro clássico
	titulo(`Append - Um erro clássico`)

	raw_string = `func adiciona(s []int) {
    s = append(s, 999)
}

numeros := []int{1,2,3}
adiciona(numeros)
fmt.Println(numeros)`
	fmt.Println(raw_string)
	fmt.Printf("\n")

	fmt.Println(`Muita gente espera:
[1 2 3 999]

Mas pode aparecer:
[1 2 3]

Porque a função recebeu uma cópia da estrutura do slice.

Se o append precisou realocar memória, somente a variável local foi atualizada.

O slice original continuou apontando para o array antigo.

O correto:`)

	raw_string = `func adiciona(s []int) []int {
    return append(s, 999)
}

numeros = adiciona(numeros)`

	fmt.Println(raw_string)
	fmt.Printf("\n")

	// Pré-alocando memória
	titulo(`Pré-alocando memória`)
	fmt.Println(`Imagine que você sabe que vai carregar 1 milhão de registros.

Ao invés de deixar o Go ficar aumentando o array várias vezes:

var registros []Registro

Você pode fazer:

registros := make([]Registro, 0, 1000000)

Resultado:

len = 0
cap = 1000000

Ou seja:

Espaço reservado para 1 milhão
Elementos utilizados = 0

Agora os append() vão ser muito mais eficientes.

Como a capacidade já foi reservada:
  - menos alocações
  - menos cópias
  - menos pressão no garbage collector
  - mais velocidade

Em importações gigantes (100 GB, 200 GB+) isso faz diferença real.`)
	fmt.Printf("\n")

}

func titulo(texto string) {
	fmt.Println("========================================")
	fmt.Println(texto)
	fmt.Println("========================================")
}

```

Resultado:
```sh
========================================
Um slice é uma "janela" para um array
========================================

========================================
Declarando um slice
Para slices, não definimos tamanho
Internamente o Go cria um array e o slice aponta para ele.
========================================
numeros := []int{10, 20, 30, 40, 50}

Visualmente:

Array real na memória

+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
  0    1    2    3    4

Slice
  |
  +--> aponta para o array

========================================
Acessando um elemento
========================================
fmt.Println(numeros[2])

Resultado:
30

========================================
O operador ":"
========================================
A sintaxe é:
slice[inicio:fim]

Importante:
  - inclui o índice inicial
  - exclui o índice final

Exemplo:
numeros := []int{10, 20, 30, 40, 50}
fmt.Println(numeros[1:4])

Resultado:
[20 30 40]

Porque:

Indice:   0   1   2   3   4
Valor:   10  20  30  40  50
              ↑           ↑
            inicio       fim

O elemento do índice 4 não entra

========================================
O operador ":" - Omitindo o início
========================================
Se não informar o início:
numeros[:3]

é igual a:
numeros[0:3]

Resultado:
[10 20 30]

========================================
O operador ":" - Omitindo o fim
========================================
Se não informar o fim:
numeros[2:]

é igual a:
numeros[2:5]

Resultado:
[30 40 50]

========================================
O operador ":" - Copiando tudo
========================================
numeros[:]

Resultado:
[10 20 30 40 50]
Muito usado quando se quer passar o slice inteiro.

========================================
O ponto que mais confunde
========================================
O slice NÃO copia os dados.

Veja:

numeros := []int{10, 20, 30, 40, 50}
parte := numeros[1:4]
fmt.Println(parte)

Resultado:
[20 30 40]

Mas agora:
parte[0] = 999

fmt.Println(parte)
fmt.Println(numeros)

Resultado:
[999 30 40]
[10 999 30 40 50]

Por quê?

Porque parte e numeros apontam para o mesmo array.

Visualmente:

Array

+----+-----+----+----+----+
| 10 | 999 | 30 | 40 | 50 |
+----+-----+----+----+----+
       ^
       |
     parte[0]

========================================
Um exemplo prático
========================================
Imagine que você esteja processando um arquivo gigante.

Você lê um lote de registros:
lote := registros[0:1000]

Depois:
lote = registros[1000:2000]

Depois:
lote = registros[2000:3000]

Nenhuma cópia é feita.

Você apenas cria novas "janelas" sobre o mesmo conjunto de dados.

Por isso slices são extremamente eficientes para processar grandes volumes de dados.

========================================
Len e Cap
========================================
Todo slice possui:
  - len (quantos elementos ele vê)
  - cap (quantos elementos existem a partir da posição inicial)

Exemplo:

numeros := []int{10, 20, 30, 40, 50}

parte := numeros[1:4]

fmt.Println(parte)
fmt.Println(len(parte))
fmt.Println(cap(parte))

Temos:
[20 30 40]
3
4

Porque?
Indice:   0   1   2   3   4
Valor:   10  20  30  40  50
              ^
            parte começa aqui

Da posição 1 até o final do array existem:

20 30 40 50

4 elementos, logo:

fmt.Println(len(parte)) // 3
fmt.Println(cap(parte)) // 4

========================================
Sintaxe completa (menos usada)
========================================
Existe uma terceira posição:

slice[inicio:fim:capacidade]

Exemplo:

numeros := []int{10,20,30,40,50}
parte := numeros[1:4:4]

Aqui você controla a capacidade do novo slice.
Isso é usado quando quer evitar que um append() modifique o array original.

========================================
Append - Cenário 1 - Ainda existe capacidade
========================================
numeros := []int{10, 20, 30, 40, 50}
parte := numeros[3:4]

Visualmente:
Array

+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
                 ^
                 |
               parte

len = 1
cap = 2

Agora:
parte = append(parte, 999)

O Go analisa: ainda tenho espaço dentro da capacidade do slice.

Então ele simplesmente escreve no array existente.

Array

+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+

Se printarmos os valores:

fmt.Println(numeros)
fmt.Println(parte)

Resultado:
[10 20 30 40 999]
[40 999]

========================================
Append - Cenário 2 - Capacidade esgotada
========================================
numeros := []int{10, 20, 30, 40, 50}
parte := numeros[3:4]

Temos:
len = 1
cap = 2

Primeiro append:
parte = append(parte, 999)

Agora:
len = 2
cap = 2

Capacidade cheia.

Antes de fazermos um novo append, vamos imprimir o endereço para onde parte está apontando:
fmt.Printf("%p\n", &parte[0])


Resultado:
0x1fc3129821c8

Se fizer outro append:
parte = append(parte, 888)

Então ele:
  - Cria um novo array maior.
  - Copia os dados.
  - Faz o slice apontar para o novo array.

Antes:
numeros

+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+

Depois:
numeros

+----+----+----+----+-----+
| 10 | 20 | 30 | 40 | 999 |
+----+----+----+----+-----+

parte

+----+-----+-----+
| 40 | 999 | 888 |
+----+-----+-----+

Agora são arrays diferentes.

Vamos imprimir novamente o endereço para ver onde parte está apontando agora:
fmt.Printf("%p\n", &parte[0])


Resultado:
0x1fc3129880a0

========================================
Append - Detalhe MUITO importante
========================================
Muita gente escreve:
append(parte, 123)

Mas o correto é:
parte = append(parte, 123)

Porque o append pode:
  - continuar usando o mesmo array
  - ou criar um array novo

Como você não sabe qual dos dois aconteceu, ele devolve o slice atualizado.

========================================
Append - Um erro clássico
========================================
func adiciona(s []int) {
    s = append(s, 999)
}

numeros := []int{1,2,3}
adiciona(numeros)
fmt.Println(numeros)

Muita gente espera:
[1 2 3 999]

Mas pode aparecer:
[1 2 3]

Porque a função recebeu uma cópia da estrutura do slice.

Se o append precisou realocar memória, somente a variável local foi atualizada.

O slice original continuou apontando para o array antigo.

O correto:
func adiciona(s []int) []int {
    return append(s, 999)
}

numeros = adiciona(numeros)

========================================
Pré-alocando memória
========================================
Imagine que você sabe que vai carregar 1 milhão de registros.

Ao invés de deixar o Go ficar aumentando o array várias vezes:

var registros []Registro

Você pode fazer:

registros := make([]Registro, 0, 1000000)

Resultado:

len = 0
cap = 1000000

Ou seja:

Espaço reservado para 1 milhão
Elementos utilizados = 0

Agora os append() vão ser muito mais eficientes.

Como a capacidade já foi reservada:
  - menos alocações
  - menos cópias
  - menos pressão no garbage collector
  - mais velocidade

Em importações gigantes (100 GB, 200 GB+) isso faz diferença real.
```
