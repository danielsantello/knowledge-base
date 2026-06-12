
# Dockerfile

## Sumário

- [Criando imagens](#criando-imagens)
- [Publicando imagens](#publicando-imagens)
- [Exemplos](#exemplos)
	- [Hello World](#hello-world)

## Criando imagens
> por padrão, cria-se um arquivo chamado Dockerfile na pasta principal do projeto.

**Sintaxe**

`docker build -t <namespace>/<nome-da-imagem>:<versão> .`  
`docker build -t <namespace>/<nome-da-imagem>:<versão> . -f <nome-do-arquivo-dockerfile`

**Exemplos de uso:**
```sh
docker build -t danielsantello1982/app:latest .
```

```sh
docker build -t danielsantello1982/app:latest . -f Dockerfile.prod
```

> **Observação:** o parâmetro `-f` deve ser usado quando o nome do dockerfile não segue o padrão.

## Publicando imagens
```sh
docker push danielsantello1982/app:latest
```

> **Observação:** o comando acima publicará a imagem no Docker Hub Registry.

## Exemplos
### Hello World
```dockerfile
FROM ubuntu:latest

CMD [ "echo", "Hello World" ]
```

```sh
docker build -t danielsantello1982/hello-world:latest .
docker run --rm danielsantello1982/hello-world:latest
```

