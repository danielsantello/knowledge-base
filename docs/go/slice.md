# [Go](../golang.md) - Slice

## Sumário
- [Declarando um slice](#declarando-um-slice)
- [Acessando um elemento](#acessando-um-elemento)
- [O operador ":"](#o-operador-)
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

### Código com as explicações e os exemplos
```go
```

Resultado:
```sh
```
