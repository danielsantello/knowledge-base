# [Go](../golang.md) - Slice

## Sumário
- [Declarando um slice](#declarando-um-slice)
- [Código com as explicações e os exemplos](#código-com-as-explicações-e-os-exemplos)

> [!IMPORTANT]
> Um slice é uma `"janela"` para um array

### Declarando um array sem valores iniciais
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
