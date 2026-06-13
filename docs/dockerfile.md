
# Dockerfile

## Sumário

- [Criando imagens](#criando-imagens)
- [Publicando imagens](#publicando-imagens)
- [Instruções](#instruções)
- [Exemplos](#exemplos)
	- [Hello World](#hello-world)
	- [Nginx com VIM](#nginx-com-vim)
	- [Nginx com Arquivos Locais](#nginx-com-arquivos-locais)
	- [Laravel](#laravel)

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
- `ENTRYPOINT` → define o comando principal executado ao iniciar o container

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
docker run --rm -d -p 8080:80 --name nginx danielsantello1982/nginx-vim:latest
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

### Laravel
```dockerfile
FROM php:8.4-cli

WORKDIR /var/www/laravel

RUN apt-get update && \
    apt-get install -y libzip-dev && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

RUN composer create-project --prefer-dist laravel/laravel laravel

ENTRYPOINT [ "php", "artisan", "serve" ]

CMD [ "--host=0.0.0.0" ]
```

```sh
docker build -t danielsantello1982/laravel:latest .
docker run --rm -d --name laravel -p 8000:8000 danielsantello1982/laravel:latest
```

**Exemplo alterando a porta padrão:**
```sh
docker run --rm -d --name laravel -p 8001:8001 danielsantello1982/laravel:latest --host=0.0.0.0 --port=8001
```

> **Observação:**  
> O comando definido em `ENTRYPOINT` será sempre executado.  
> Os parâmetros definidos em `CMD` podem ser substituídos ao executar o container.  
> Os parâmetros informados após o nome da imagem substituem os valores definidos em `CMD`.

> **Resultado:**  
> - criará uma imagem baseada em PHP  
> - instalará a extensão ZIP  
> - instalará o Composer  
> - criará um projeto Laravel  
> - iniciará o servidor embutido do Laravel  
> - permitirá alterar host e porta através dos parâmetros informados no `docker run`

