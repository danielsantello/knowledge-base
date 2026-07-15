# Array

<a href="../../README.md">Base de Conhecimento</a> › <a href="../go.md">Go</a>

## Sumário
- [Declarando um array sem valores iniciais](#declarando-um-array-sem-valores-iniciais)
- [Atribuindo valores ao array](#atribuindo-valores-ao-array)
- [Acessando valores de um array](#acessando-valores-de-um-array)
- [Percorrendo o array e imprimindo todos os valores](#percorrendo-o-array-e-imprimindo-todos-os-valores)
- [Exibindo o tamanho de um array](#exibindo-o-tamanho-de-um-array)
- [Declarando e definindo valores ao mesmo tempo](#declarando-e-definindo-valores-ao-mesmo-tempo)

<br>

> [!IMPORTANT]
> No array, o tamanho é fixo

### Declarando um array sem valores iniciais
```go
var meuArray [3]int
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Atribuindo valores ao array
```go
meuArray[0] = 10
meuArray[1] = 20
meuArray[2] = 30
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

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

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

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

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Exibindo o tamanho de um array
```go
fmt.Println(len(meuArray))
```

Resultado:
```sh
3
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Declarando e definindo valores ao mesmo tempo
```go
var meuArray [5]int = [5]int{10, 20, 30, 40, 50}")

// versão mais curta:
meuArray := [5]int{10, 20, 30, 40, 50}")
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>
