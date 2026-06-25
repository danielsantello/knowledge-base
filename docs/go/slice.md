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

### Código com as explicações e os exemplos
```go
```

Resultado:
```sh
```
