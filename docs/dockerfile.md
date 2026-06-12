
# Dockerfile

## Sumário

- [Criando imagens](#criando-imagens)
- [Publicando imagens](#publicando-imagens)
- [Instruções](#instruções)
- [Exemplos](#exemplos)
	- [Hello World](#hello-world)
	- [Nginx com VIM](#nginx-com-vim)
	- [Nginx com Arquivos Locais](#nginx-com-arquivos-locais)

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

> **Observação:** antes de publicar uma imagem, é necessário estar autenticado no Docker Hub:
>
> ```sh
> docker login
> ```

## Instruções
- `FROM` → define a imagem base utilizada na construção da nova imagem
- `RUN` → executa comandos durante o processo de build
- `WORKDIR` → define o diretório de trabalho da imagem
- `COPY` → copia arquivos da máquina local (contexto de build) para dentro da imagem
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

### Nginx com VIM
```dockerfile
FROM nginx:latest

RUN apt-get update && \
    apt-get install -y vim
```

```sh
docker build -t danielsantello1982/nginx-vim:latest .
docker run -d -p 8080:80 --name nginx danielsantello1982/nginx-vim:latest
```

### Nginx com Arquivos Locais
```dockerfile
FROM nginx:latest

WORKDIR /app

RUN apt-get update && \
    apt-get install -y vim

COPY html /usr/share/nginx/html
```

```sh
docker build -t danielsantello1982/nginx-local:latest .
docker run -d -p 8080:80 --name nginx danielsantello1982/nginx-local:latest
```

> **Resultado:**  
> - criará uma imagem baseada no NGINX  
> - definirá `/app` como diretório de trabalho (criando-o caso não exista)  
> - instalará o programa vim  
> - copiará o conteúdo da pasta `html` da máquina local para `/usr/share/nginx/html`

