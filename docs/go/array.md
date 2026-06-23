
# [Go](../golang.md) - Array

## Sumário
- [Declarando um array sem valores iniciais](#declarando-um-array-sem-valores-iniciais)
- [Atribuindo valores ao array](#atribuindo-valores-ao-array)
- [Acessando valores de um array](#acessando-valores-de-um-array)
- [Percorrendo o array e imprimindo todos os valores](#percorrendo-o-array-e-imprimindo-todos-os-valores)
- [Exibindo o tamanho de um array](#exibindo-o-tamanho-de-um-array)
- [Declarando e definindo valores ao mesmo tempo](#declarando-e-definindo-valores-ao-mesmo-tempo)
- [Código com as explicações e os exemplos](#código-com-as-explicações-e-os-exemplos)

> [!IMPORTANT]
> No array, o tamanho é fixo

### Declarando um array sem valores iniciais
```go
var meuArray [3]int
```

### Atribuindo valores ao array
```go
meuArray[0] = 10
meuArray[1] = 20
meuArray[2] = 30
```

### Acessando valores de um array
```go
fmt.Println(meuArray[0])
fmt.Println(meuArray[1])
fmt.Println(meuArray[2])
```

Resultado:
```sh
10
20
30
```

### Percorrendo o array e imprimindo todos os valores
```go
for i, v := range meuArray {
    fmt.Printf("O valor do indice %d é %d\n", i, v)
}
```

Resultado:
```sh
O valor do indice 0 é 10
O valor do indice 1 é 20
O valor do indice 2 é 30
```

### Exibindo o tamanho de um array
```go
fmt.Println(len(meuArray))
```

Resultado:
```sh
3
```

### Declarando e definindo valores ao mesmo tempo
```go
var meuArray [5]int = [5]int{10, 20, 30, 40, 50}")

// versão mais curta:
meuArray := [5]int{10, 20, 30, 40, 50}")
```

### Código com as explicações e os exemplos
```go
package main

import "fmt"

func main() {
	// No array, o tamanho é fixo
	titulo(`No array, o tamanho é fixo`)
	fmt.Printf("\n")

	// Declarando um array sem valores iniciais
	titulo(`Declarando um array sem valores iniciais`)
	fmt.Println("var meuArray [3]int")
	fmt.Printf("\n")

	var meuArray [3]int

	// Atribuindo valores ao array
	titulo(`Atribuindo valores ao array`)
	fmt.Println(`meuArray[0] = 10
meuArray[1] = 20
meuArray[2] = 30`)
	fmt.Printf("\n")

	meuArray[0] = 10
	meuArray[1] = 20
	meuArray[2] = 30

	// Acessando valores de um array
	titulo(`Acessando valores de um array`)
	fmt.Println(`fmt.Println(meuArray[0])
fmt.Println(meuArray[1])
fmt.Println(meuArray[2])

Resultado:`)
	fmt.Println(meuArray[0])
	fmt.Println(meuArray[1])
	fmt.Println(meuArray[2])
	fmt.Printf("\n")

	// Percorrendo o array e imprimindo todos os valores
	titulo(`Percorrendo o array e imprimindo todos os valores`)
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
	titulo(`Exibindo o tamanho de um array`)
	fmt.Println(`fmt.Println(len(meuArray))

Resultado:`)
	fmt.Println(len(meuArray))
	fmt.Printf("\n")

	// Declarando e definindo valores ao mesmo tempo
	titulo(`Declarando e definindo valores ao mesmo tempo:`)
	fmt.Println(`var meuArray [5]int = [5]int{10, 20, 30, 40, 50}
ou
meuArray := [5]int{10, 20, 30, 40, 50}`)
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
No array, o tamanho é fixo
========================================

========================================
Declarando um array sem valores iniciais
========================================
var meuArray [3]int

========================================
Atribuindo valores ao array
========================================
meuArray[0] = 10
meuArray[1] = 20
meuArray[2] = 30

========================================
Acessando valores de um array
========================================
fmt.Println(meuArray[0])
fmt.Println(meuArray[1])
fmt.Println(meuArray[2])

Resultado:
10
20
30

========================================
Percorrendo o array e imprimindo todos os valores
========================================
for i, v := range meuArray {
    fmt.Printf("O valor do indice %d é %d\n", i, v)
}

Resultado:
O valor do indice 0 é 10
O valor do indice 1 é 20
O valor do indice 2 é 30

========================================
Exibindo o tamanho de um array
========================================
fmt.Println(len(meuArray))

Resultado:
3

========================================
Declarando e definindo valores ao mesmo tempo:
========================================
var meuArray [5]int = [5]int{10, 20, 30, 40, 50}
ou
meuArray := [5]int{10, 20, 30, 40, 50}
```
