
# Dockerfile

## Sumário

- [Criando imagens](#criando-imagens)
- [Publicando imagens](#publicando-imagens)
- [Instruções](#instruções)
- [Exemplos](#exemplos)
	- [Hello World](#hello-world)
	- [Nginx + VIM](#nginx-+-vim)
	- [Nginx + Mapeamento Local](#nginx-+-mapeamento-local)

## Criando imagens
> por padrão, cria-se um arquivo chamado Dockerfile na pasta principal do projeto.

**Sintaxe**

`docker build -t <namespace>/<nome-da-imagem>:<versão> .`  
`docker build -f <nome-do-arquivo-dockerfile> -t <namespace>/<nome-da-imagem>:<versão> .`

**Exemplos de uso:**
```sh
docker build -t danielsantello1982/app:latest .
```

```sh
docker build -f Dockerfile.prod -t danielsantello1982/app:latest .
```

> **Observações:**  
> o parâmetro `-f` deve ser usado quando o nome do dockerfile não segue o padrão.  
> o ponto (`.`) representa o contexto de build, indicando que os arquivos necessários para a construção da imagem estão no diretório atual.

## Publicando imagens
```sh
docker push danielsantello1982/app:latest
```

> **Observação:** o comando acima publicará a imagem no Docker Hub Registry.

## Instruções
- `FROM` → define a imagem base utilizada na construção da nova imagem
- `CMD` → define o comando executado quando o container for iniciado

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

### Nginx + VIM
```dockerfile
FROM nginx:latest

RUN apt-get update
RUN apt-get install vim -y
```

```sh
docker build -t danielsantello1982/nginx-vim:latest .
docker run -d -p 8080:80 --name nginx danielsantello1982/nginx-vim:latest
```

### Nginx + Mapeamento Local
```dockerfile
FROM nginx:latest

WORKDIR /app

RUN apt-get update &&\
    apt-get install vim -y

COPY html /usr/share/nginx
```

```sh
docker build -t danielsantello1982/nginx-local:latest .
docker run -d -p 8080:80 --name nginx danielsantello1982/nginx-local:latest
```

> **Resultado:**  
> - criará uma imagem com o NGINX  
> - criará uma pasta chamada /app  
> - instalará o programa vim  
> - copiará o conteúdo da pasta html da máquina local para o diretório /usr/share/nginx/html do container

