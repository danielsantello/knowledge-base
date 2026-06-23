# [Go](../golang.md) - Slice

## Sumário
- [Declarando um slice](#declarando-um-slice)
- [Acessando um elemento](#acessando-um-elemento)
- [O operador ":"](#o-operador-)
  - [Omitindo o início](#omitindo-o-início)
  - [Omitindo o fim](#omitindo-o-fim)
  - [Copiando tudo](#copiando-tudo)
- [O ponto que mais confunde](#o-ponto-que-mais-confunde)
- [Um exemplo prático](#um-exemplo-prático)
- [Len e Cap](#len-e-cap)
- [Código com as explicações e os exemplos](#código-com-as-explicações-e-os-exemplos)

> [!IMPORTANT]
> Um slice é uma `"janela"` para um array

### Declarando um slice
Para slices, não definimos tamanho.  
Internamente o Go cria um array e o slice aponta para ele.
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

### O ponto que mais confunde
O slice NÃO copia os dados.

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

### Código com as explicações e os exemplos
```go
```

Resultado:
```sh
```
