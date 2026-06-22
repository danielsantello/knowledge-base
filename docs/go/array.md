
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

## Código completo
```go
package main

import "fmt"

func main() {
	// No array, o tamanho é fixo
	titulo("No array, o tamanho é fixo")
	fmt.Printf("\n")

	// Declarando um array sem valores iniciais
	titulo("Declarando um array sem valores iniciais")
	fmt.Println("var meuArray [3]int")
	fmt.Printf("\n")

	var meuArray [3]int

	// Atribuindo valores ao array
	titulo("Atribuindo valores ao array")
	fmt.Println("meuArray[0] = 10")
	fmt.Println("meuArray[1] = 20")
	fmt.Println("meuArray[2] = 30")
	fmt.Printf("\n")

	meuArray[0] = 10
	meuArray[1] = 20
	meuArray[2] = 30

	// Acessando valores de um array
	titulo("Acessando valores de um array")
	fmt.Println("fmt.Println(meuArray[0])")
	fmt.Println("fmt.Println(meuArray[1])")
	fmt.Println("fmt.Println(meuArray[2])")
	fmt.Printf("\n")

	fmt.Println("Resultado:")
	fmt.Println(meuArray[0])
	fmt.Println(meuArray[1])
	fmt.Println(meuArray[2])
	fmt.Printf("\n")

	// Percorrendo o array e imprimindo todos os valores
	titulo("Percorrendo o array e imprimindo todos os valores")
	raw_string := `for i, v := range meuArray {
    fmt.Printf("O valor do indice %d é %d\n", i, v)
}`
	fmt.Println(raw_string)
	fmt.Printf("\n")

	fmt.Println("Resultado:")
	for i, v := range meuArray {
		fmt.Printf("O valor do indice %d é %d\n", i, v)
	}
	fmt.Printf("\n")

	// Exibindo o tamanho de um array
	titulo("Exibindo o tamanho de um array")
	fmt.Println("fmt.Println(len(meuArray))")
	fmt.Printf("\n")

	fmt.Println("Resultado:")
	fmt.Println(len(meuArray))
	fmt.Printf("\n")

	// Declarando e definindo valores ao mesmo tempo
	titulo("Declarando e definindo valores ao mesmo tempo:")
	fmt.Println("var meuArray [5]int = [5]int{10, 20, 30, 40, 50}")
	fmt.Println("ou")
	fmt.Println("meuArray := [5]int{10, 20, 30, 40, 50}")
}

func titulo(texto string) {
	fmt.Println("========================================")
	fmt.Println(texto)
	fmt.Println("========================================")
}
```
