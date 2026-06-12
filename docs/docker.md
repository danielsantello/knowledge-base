# Docker

## Sumário

- [Verificando o serviço do Docker](#verificando-o-serviço-do-docker)
- [Iniciando o serviço do Docker](#iniciando-o-serviço-do-docker)
- [Parando o serviço do Docker](#parando-o-serviço-do-docker)
- [Restartando o serviço do Docker](#parando-o-serviço-do-docker)
- [Consultando ajuda](#consultando-ajuda)
- [Testando a instalação](#testando-a-instalação)
- [Containers](#containers)
	- [Listando containers](#listando-containers)
 	- [Listando apenas os nomes dos containers](#listando-apenas-os-nomes-dos-containers)
	- [Criando container em modo interativo](#criando-container-em-modo-interativo)
	- [Removendo o container ao término da execução](#removendo-o-container-ao-término-da-execução)
	- [Publicando portas no host](#publicando-portas-no-host)
	- [Criando container em background](#criando-container-em-background)
	- [Associando o terminal ao container](#associando-o-terminal-ao-container)
	- [Criando um container nomeado](#criando-um-container-nomeado)
	- [Renomeando containers](#renomeando-containers)
	- [Criando containers](#criando-containers)
	- [Removendo container](#removendo-container)
	- [Removendo vários containers](#removendo-vários-containers)
	- [Executando containers](#executando-containers)
	- [Parando containers](#parando-containers)
	- [Executando comandos dentro de um container em execução](#executando-comandos-dentro-de-um-container-em-execução)
	- [Acessando o terminal dentro de um container](#acessando-o-terminal-dentro-de-um-container)
	- [Mapeando uma pasta local](#mapeando-uma-pasta-local)
 	- [Inspecionando containers](#inspecionando-containers)
- [Volumes](#volumes)
	- [Listando volumes criados](#listando-volumes-criados)
 	- [Criando um novo volume](#criando-um-novo-volume)
  	- [Exibindo dados de um volume](#exibindo-dados-de-um-volume)
  	- [Limpando volumes sem utilização](#limpando-volumes-sem-utilização)
  	- [Removendo volumes](#removendo-volumes)
  	- [Mapeando um volume no container](#mapeando-um-volume-no-container)
- [Imagens](#imagens)
	- [Listando imagens criadas](#listando-imagens-criadas)
 	- [Baixando imagens da DockerHub](#baixando-imagens-da-dockerhub)
  	- [Removendo imagens](#removendo-imagens)
  	- [Removendo imagens não utilizadas](#removendo-imagens-não-utilizadas)
- [Networks](#networks)
	- [Tipos de networks](#tipos-de-networks)
 	- [Criando uma network](#criando-uma-network)
 	- [Listando networks criadas](#listando-networks-criadas)
  	- [Exibindo dados de uma network](#exibindo-dados-de-uma-network)
    - [Limpando networks sem utilização](#limpando-networks-sem-utilização)
    - [Removendo networks](#removendo-networks)
    - [Definindo qual network usar no container](#definindo-qual-network-usar-no-container)
    - [Conectando uma network em um container existente](#conectando-uma-network-em-um-container-existente)
    - [Descobrindo o ip de um container](#descobrindo-o-ip-de-um-container)
- [Resumo dos principais comandos](#resumo-dos-principais-comandos)
  

## Verificando o serviço do Docker
```sh
sudo service docker status
```

**Resultado esperado:**
```text
○ docker.service - Docker Application Container Engine
     Loaded: loaded (...)
     Active: inactive (dead) ...
	 ...
```

## Iniciando o serviço do Docker
```sh
sudo service docker start
```

**Resultado esperado:**
```text
● docker.service - Docker Application Container Engine
     Loaded: loaded (...)
     Active: active (running) ...
     ...
```

## Parando o serviço do Docker
```sh
sudo service docker stop
```

## Restartando o serviço do Docker
**Sintaxe**

`docker restart <container_id>`
`docker restart <container_name>`

**Exemplo de uso:**
```sh
docker restart app
```

## Consultando ajuda
```sh
docker run --help
docker ps --help
```

## Testando a instalação
```sh
docker run hello-world
```

**Resultado esperado:**
```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

## Containers
### Listando containers
```sh
docker ps
```
> **Observação:** por padrão, `docker ps` mostra apenas os containers em execução.

```sh
docker ps -a
```
> **Observação:** lista containers em execução e também os que já finalizaram.

```sh
docker ps -a -q
```
> **Observação:** lista apenas os IDs dos containers.

### Listando apenas os nomes dos containers
```sh
docker ps --format "{{.Names}}"
```

### Criando container em modo interativo
**Sintaxe**

`docker run -it <nome-da-imagem> bash`

**Exemplo de uso:**
```sh
docker run -it ubuntu bash
```
> **Observação:** Os parâmetros do container devem ser informados após o comando `run`.

**Explicação dos parâmetros**

- `docker run` → executa um container
- `-i` → modo interativo, mantém o STDIN ativo
- `-t` → aloca um terminal (TTY)
- `ubuntu` → imagem que será executada
- `bash` → comando executado ao iniciar o container

### Removendo o container ao término da execução
**Sintaxe**

`docker run -it --rm <nome-da-imagem> bash`

**Exemplo de uso:**
```sh
docker run -it --rm ubuntu bash
```

### Publicando portas no host
```sh
docker run -p 8080:80 nginx
```

> [!NOTE]
> **Mapeamento de portas**
>
> O Nginx responde na porta `80` dentro do container.
>
> O parâmetro `-p 8080:80` cria um mapeamento:
>
> - `8080` → porta da máquina host
> - `80` → porta do container
>
> Ao acessar:
>
> `http://localhost:8080`
>
> a requisição será encaminhada para a porta `80` do container.


### Criando container em background
```sh
docker run -d -p 8080:80 nginx
```

> **Observação:** o parâmetro `-d` executa o container em modo **detached** (background).
>
> Nesse modo, o terminal é liberado imediatamente após a criação do container.

### Associando o terminal ao container
```sh
docker attach nginx
```
> [!IMPORTANT]
> O comando `attach` conecta seu terminal ao processo principal do container.
>
> Para abrir um terminal dentro de um container já em execução, normalmente utiliza-se:
>
> `docker exec -it <nome-do-container> bash`

### Criando um container nomeado
**Sintaxe**

`docker run -d -p 8080:80 --name <nome-do-container> <nome-da-imagem>`

**Exemplo de uso:**
```sh
docker run -d -p 8080:80 --name nginx-server nginx
```

### Renomeando containers
**Sintaxe**

`docker rename <nome-antigo> <nome-novo>`

**Exemplo de uso:**
```sh
docker rename nginx nginx-server
```

### Criando containers
**Sintaxe**

`docker create --name <nome-do-container> <nome-da-imagem>`

**Exemplo de uso:**
```sh
docker create --name nginx-server nginx
```
> O comando **create** apenas cria o container, sem executar. Diferente do docker run que cria e executa

### Removendo container
**Sintaxe**

`docker rm <container_id>`  
`docker rm <container_name>`

> [!IMPORTANT]
> Não é possível remover um container em execução.
>
> Para forçar a remoção:
>
> ```sh
> docker rm -f <container_id>  
> docker rm -f <container_name>
> ```

### Removendo vários containers
**Exemplos de uso:**
```sh
docker rm 49b000cf2fdb 1ed329fa2fb2 28ab953b6465
```

```sh
docker rm -f $(docker ps -qa)
```

### Executando containers
**Sintaxe**

`docker start <container_id>`  
`docker start <container_name>`

**Exemplos de uso:**
```sh
docker start app
```

```sh
docker start $(docker ps -qa)
```

> **Observação:** esse último exemplo executa todos os containers criados

### Parando containers
**Sintaxe**

`docker stop <container_id>`  
`docker stop <container_name>`

**Exemplos de uso:**
```sh
docker stop app
```

```sh
docker stop $(docker ps -q)
```

> **Observação:** esse último exemplo, para todos os containers em execução

### Executando comandos dentro de um container em execução
**Sintaxe**

`docker exec <container_id> <comando>`  
`docker exec <container_name> <comando>`

**Exemplo de uso:**
```sh
docker exec app ls
```

### Acessando o terminal dentro de um container
```sh
docker exec -it app bash
```

### Mapeando uma pasta local
**Exemplos de uso:**
```sh
docker run -d --name nginx-server -p 8080:80 -v "$(pwd)"/html:/usr/share/nginx/html nginx
```

```sh
docker run -d --name nginx-server -p 8080:80 --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html nginx
```

```sh
docker run -d --name nginx-server -p 8080:80 --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html,ro nginx
```

> **Observação:** no primeiro exemplo, o parâmetro `-v` cria automaticamente a pasta de origem caso ela ainda não exista. No último exemplo, abre a pasta no destino como somente leitura `(ro - readonly)`

### Inspecionando containers
**Sintaxe**

`docker inspect <nome-do-container>`

**Exemplo de uso:**
```sh
docker inspect app
```

> **Observação:** retorna um `JSON` com os metadados do container

## Volumes
### Listando volumes criados
```sh
docker volume ls
```

### Criando um novo volume
**Sintaxe**

`docker volume create <nome-do-volume>`

**Exemplo de uso:**
```sh
docker volume create datadir
```

### Exibindo dados de um volume
**Sintaxe**

`docker volume inspect <nome-do-volume>`

**Exemplo de uso:**
```sh
docker volume inspect datadir
```

> **Observação:** retorna um `JSON` com os metadados do volume

### Limpando volumes sem utilização
```sh
docker volume prune
```

### Removendo volumes
**Sintaxe**

`docker volume rm <nome-do-volume>`

**Exemplo de uso:**
```sh
docker volume rm datadir
```

> **Observação:** limpa os volumes locais que não estão sendo usados

### Mapeando um volume no container
**Sintaxe**

`docker run -d --name <nome-do-container> -p 8080:80 --mount type=volume,source=<nome-do-volume>,target=/app <nome-da-imagem>`

**Exemplo de uso:**
```sh
docker run -d --name nginx-server -p 8080:80 --mount type=volume,source=datadir,target=/app nginx
```

## Imagens
### Listando imagens criadas
```sh
docker images
```

### Baixando imagens da DockerHub
**Sintaxe**

`docker pull <nome-da-imagem>`

**Exemplo de uso:**
```sh
docker pull ubuntu
```

### Removendo imagens
**Sintaxe**

`docker rmi <nome-da-imagem>`

**Exemplo de uso:**
```sh
docker rmi ubuntu
```

### Removendo imagens não utilizadas
```sh
docker image prune
```

## Networks
### Tipos de networks

- `bridge` → default, quando um container precisa se comunicar com outro
- `host` → mesma rede do host, onde está executando o docker. O Host consegue acessar os containers sem o mapeamento
- `overlay` → usado quando precisa de vários container para escalonamento - Swarm
- `macvlan` → atribui um MAC/IP próprio ao container
- `none` → container sem interface de rede

### Criando uma network
**Sintaxe**

`docker network create --driver <nome-do-driver> <nome-da-network>`

**Exemplo de uso:**
```sh
docker network create --driver bridge net01
```

### Listando networks criadas
```sh
docker network ls
```

### Limpando networks sem utilização
```sh
docker network prune
```

### Removendo networks
**Sintaxe**

`docker network rm <nome-da-network>`

**Exemplo de uso:**
```sh
docker network rm net01
```

### Exibindo dados de uma network
**Sintaxe**

`docker network inspect <id-da-network>`  
`docker network inspect <nome-da-network>`

**Exemplos de uso:**
```sh
docker network inspect f293d0c2b0e5
```

```sh
docker network inspect bridge
```

> **Observação:** retorna um `JSON` com os metadados da network

### Definindo qual network usar no container
```sh
docker run -dit --name ubuntu1 --network net01 ubuntu bash
```

> **Observação:** quando se cria um container apontando pra uma network específica, é possível executar um ping pelo nome da máquina

```sh
docker run --rm -d --name nginx --network host nginx
```

> **Observação:** quando se cria um container apontando pra uma network `host`, é possível acessar direto assim:  
> `http://localhost`
>
> Caso o container esteja em modo bridge e precisar acessar o host, pode ser feito dessa forma também:  
> `http://host.docker.internal`

### Conectando uma network em um container existente
**Sintaxe**

`docker network connect <nome-da-network> <nome-do-container>`

**Exemplo de uso:**
```sh
docker network connect net01 ubuntu1
```

### Descobrindo o ip de um container
**Sintaxe**

`docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <nome-do-container>`  
`docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <id-do-container>`

**Exemplos de uso:**
```sh
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ubuntu1
```

```sh
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' f293d0c2b0e5
```

## Logs
**Sintaxe**

`docker logs [container_id]`  
`docker logs [container_name]`

**Exemplos de uso:**
```sh
docker logs ubuntu1
```

```sh
docker logs -f laravel
```

> **Observação:** o parâmetro -f mantém o terminal em modo de exibição

```sh
docker logs --tail 20 nginx
```

## Resumo dos principais comandos
```sh
docker ps
docker ps -a
docker run -it ubuntu bash
docker run -it --rm ubuntu bash
docker run -p 8080:80 nginx
docker run -d -p 8080:80 nginx
docker run -d -p 8080:80 --name nginx-server nginx
docker attach nginx

docker rm <container_id|container_name>
docker rm <container_id|container_name> -f
docker rm 49b000cf2fdb 1ed329fa2fb2 28ab953b6465
docker rm $(docker ps -qa) -f

docker exec <container_id|container_name> ls
docker exec -it <container_id|container_name> bash

docker run -d --name nginx-server -p 8080:80 -v "$(pwd)"/html:/usr/share/nginx/html nginx
docker run -d --name nginx-server -p 8080:80 --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html nginx
docker run -d --name nginx-server -p 8080:80 --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html,ro nginx (somente leitura)

docker volume ls
docker volume create meuvolume
docker run -d --name nginx-server -p 8080:80 --mount type=volume,source=meuvolume,target=/app nginx

docker network ls
docker network create --driver bridge dalq
docker run -dit --name ubuntu1 --network dalq ubuntu bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_id|container_name>

docker logs <container_id|container_name>
docker logs -f <container_id|container_name> (fica em modo de exibição dos logs)
```
