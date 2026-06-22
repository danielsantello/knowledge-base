
# [Go](../golang.md) - Arrays

> [!NOTE]
> No array, o tamanho é fixo

## Declarando um array sem valores iniciais
```go
var meuArray [3]int
```

## Atribuindo valores ao array
```go
meuArray[0] = 10
meuArray[1] = 20
meuArray[2] = 30
```

## Acessando valores de um array
```go
fmt.Println(meuArray[0])
fmt.Println(meuArray[1])
fmt.Println(meuArray[2])
```

Resultado:
```go
10
20
30
```

## Percorrendo o array e imprimindo todos os valores
```go
for i, v := range meuArray {
    fmt.Printf("O valor do indice %d é %d\n", i, v)
}
```

Resultado:
```go
O valor do indice 0 é 10
O valor do indice 1 é 20
O valor do indice 2 é 30
```

## Exibindo o tamanho de um array
```go
fmt.Println(len(meuArray))
```

Resultado:
```go
3
```

## Declarando e definindo valores ao mesmo tempo
```go
var meuArray [5]int = [5]int{10, 20, 30, 40, 50}")

// versão mais curta:
meuArray := [5]int{10, 20, 30, 40, 50}")
```
