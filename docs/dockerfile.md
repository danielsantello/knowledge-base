# Dockerfile

## Sumário

- [Criando imagens](#criando-imagens)
- [Adicionando tags](#adicionando-tags)
- [Publicando imagens](#publicando-imagens)
- [Instruções](#instruções)
- [Exemplos](#exemplos)
	- [Hello World](#hello-world)
	- [Nginx com VIM](#nginx-com-vim)
	- [Nginx com Arquivos Locais](#nginx-com-arquivos-locais)
	- [Laravel](#laravel)
	- [Laravel utilizando Nginx como Proxy Reverso](#laravel-utilizando-nginx-como-proxy-reverso)
 	- [Node](#node)

## Criando imagens
> por padrão, cria-se um arquivo chamado Dockerfile na pasta principal do projeto.

**Sintaxe**

`docker build .`  
`docker build -t <namespace>/<nome-da-imagem>:<versão> .`  
`docker build -f <nome-do-arquivo-dockerfile> -t <namespace>/<nome-da-imagem>:<versão> .`  
`docker build --no-cache -f <nome-do-arquivo-dockerfile> -t <namespace>/<nome-da-imagem>:<versão> .`

> **Observação:** o parâmetro `--no-cache` força a execução de todas as instruções do Dockerfile novamente, ignorando o cache de camadas utilizado em builds anteriores. A imagem base somente será baixada novamente caso não exista localmente ou esteja desatualizada.

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

## Adicionando tags
**Sintaxe**

`docker tag <imagem-origem>:<tag> <imagem-destino>:<tag>`

**Exemplos de uso:**
```sh
docker tag danielsantello1982/app:latest danielsantello1982/app:1.0
```

> **Observação:** o comando não cria uma nova imagem. Ele apenas adiciona um novo nome apontando para a mesma imagem.

**Exemplo:**

```text
danielsantello1982/app:latest
        ↓
      IMAGE ID
      abc123
```

Após executar:

```sh
docker tag danielsantello1982/app:latest danielsantello1982/app:1.0
```

teremos:

```text
danielsantello1982/app:latest
                 ↓
               abc123

danielsantello1982/app:1.0
                 ↓
               abc123
```


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
docker build -t danielsantello1982/nginx-arquivos-locais:latest .
docker run --rm -d -p 8080:80 --name nginx danielsantello1982/nginx-arquivos-locais:latest
```

> **Resultado:**  
> - criará uma imagem baseada no NGINX  
> - definirá `/app` como diretório de trabalho (criando-o caso não exista)  
> - instalará o programa vim  
> - copiará o conteúdo da pasta `html` da máquina local para `/usr/share/nginx/html`

### Laravel
```dockerfile
FROM php:8.4-cli

WORKDIR /var/www

RUN apt-get update && \
    apt-get install -y libzip-dev unzip git && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

RUN composer create-project --prefer-dist laravel/laravel laravel

WORKDIR /var/www/laravel

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

> **Observações:**  
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

### Laravel utilizando Nginx como Proxy Reverso
Fluxo do exemplo:
```text
Browser
↓
Nginx
↓
Container Laravel
↓
php artisan serve
↓
Laravel
```

Para esse exemplo, vamos criar dois arquivos Dockerfiles.

O primeiro arquivo chamado `Dockerfile.laravel`:
```dockerfile
FROM php:8.4-cli

WORKDIR /var/www

RUN apt-get update && \
    apt-get install -y libzip-dev unzip git && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

RUN composer create-project --prefer-dist laravel/laravel laravel

WORKDIR /var/www/laravel

ENTRYPOINT [ "php", "artisan", "serve" ]

CMD [ "--host=0.0.0.0" ]
```

O segundo arquivo chamado `Dockerfile.nginx`:
```dockerfile
FROM nginx:1.15.0-alpine

RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d

RUN mkdir -p /var/www/public && \
    touch /var/www/public/index.php
```

Criar também um outro arquivo chamado `nginx.conf`:
```nginx
server {
    listen 80;
    index index.php index.html;
    root /var/www/laravel/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    charset utf-8;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass laravel:9000;
        fastcgi_param SCRIPT_FILENAME /var/www/public/index.php;
        include fastcgi_params;
    }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

Criar uma rede do tipo `bridge` chamada `net01`:
```sh
docker network create --driver bridge net01
```

> [!IMPORTANT]
> Os containers precisam estar na mesma rede Docker para que o Nginx consiga localizar o container Laravel através do nome:
>
> ```text
> fastcgi_pass laravel:9000;
> ```
>
> Nesse caso, `laravel` é o nome atribuído ao container durante sua criação.

Fazer o build das duas imagens:
```sh
docker build -f Dockerfile.laravel -t danielsantello1982/laravel:latest .
docker build -f Dockerfile.nginx -t danielsantello1982/nginx:latest .
```

Rodar as duas imagens:
```sh
docker run --rm -d --network net01 --name laravel danielsantello1982/laravel:latest
docker run --rm -d --network net01 -p 8080:80 --name nginx danielsantello1982/nginx:latest
```

Depois, basta acessar: `http://localhost:8080`

O Nginx atuará como proxy reverso, encaminhando as requisições para o container Laravel.

> [!NOTE]
> Este exemplo tem finalidade didática e demonstra a comunicação entre containers utilizando Nginx como proxy reverso para uma aplicação Laravel executada através do comando php artisan serve. Em ambientes de produção é mais comum utilizar Nginx em conjunto com PHP-FPM.

### Node
#### Passo 1 - Criar o projeto

Executar o seguinte container:
```sh
docker run --rm -it -v $(pwd)/:/usr/src/app -p 3000:3000 --name node node:22 bash
```

> [!NOTE]
>
> O parâmetro -v cria um mapeamento entre uma pasta da máquina local e uma pasta do container.
>
> Nesse exemplo:
> 
> - `$(pwd)` → diretório atual da máquina local
> - `/usr/src/app` → diretório dentro do container
>
> Dessa forma, os arquivos criados dentro do container ficam persistidos na máquina local.

Dentro do container, executar:
```sh
cd /usr/src/app
npm init -y
npm install express
```

> **Resultado:**  
> - criará o arquivo `package.json`  
> - criará o arquivo `package-lock.json`  
> - instalará a dependência `express`  
> - criará a pasta `node_modules`

Criar um arquivo chamado `index.js` na pasta do projeto:
```javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
    res.send('<h1>Rodando pelo Node</h1>')
})

app.listen(port, ()=> {
    console.log('Rodando na porta ' + port)
})
```

#### Passo 2 - Criar a imagem

Criar o Dockerfile:
```dockerfile
FROM node:22

WORKDIR /usr/src/app

COPY . /usr/src/app

EXPOSE 3000

CMD ["node","index.js"]
```

> **Observações:**  
> o comando `COPY` copiará todo o conteúdo do diretório atual para dentro da imagem, incluindo os arquivos `index.js`, `package.json` e demais arquivos do projeto.  
> o comando `EXPOSE` documenta a porta utilizada pela aplicação dentro do container. Ele não publica automaticamente a porta para a máquina host.

Fazer o build da imagem e executar o container:
```sh
docker build -t danielsantello1982/node:latest .
docker run --rm -p 3000:3000 --name node danielsantello1982/node:latest
```

Depois, basta acessar: `http://localhost:3000`

> **Resultado:**
> - criará uma imagem baseada em Node.js  
> - copiará os arquivos da aplicação para a imagem  
> - exporá a porta 3000  
> - iniciará a aplicação através do comando `node index.js`  
> - disponibilizará a aplicação em `http://localhost:3000`
