# Docker

## Sumário

- [Verificando o serviço do Docker](#verificando-o-serviço-do-docker)
- [Iniciando o serviço do Docker](#iniciando-o-serviço-do-docker)
- [Parando o serviço do Docker](#parando-o-serviço-do-docker)
- [Consultando ajuda](#consultando-ajuda)
- [Testando a instalação](#testando-a-instalação)
- [Listando containers](#listando-containers)
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

## Listando containers
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

## Criando container em modo interativo
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

## Removendo o container ao término da execução
**Sintaxe**

`docker run -it --rm <nome-da-imagem> bash`

**Exemplo de uso:**
```sh
docker run -it --rm ubuntu bash
```

## Publicando portas no host
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


## Criando container em background
```sh
docker run -d -p 8080:80 nginx
```

> **Observação:** o parâmetro `-d` executa o container em modo **detached** (background).
>
> Nesse modo, o terminal é liberado imediatamente após a criação do container.

## Associando o terminal ao container
```sh
docker attach nginx
```
> [!IMPORTANT]
> O comando `attach` conecta seu terminal ao processo principal do container.
>
> Para abrir um terminal dentro de um container já em execução, normalmente utiliza-se:
>
> `docker exec -it <nome-do-container> bash`

## Criando um container nomeado
**Sintaxe**

`docker run -d -p 8080:80 --name <nome-do-container> <nome-da-imagem>`

**Exemplo de uso:**
```sh
docker run -d -p 8080:80 --name nginx-server nginx
```

## Renomeando containers
**Sintaxe**

`docker rename <nome-antigo> <nome-novo>`

**Exemplo de uso:**
```sh
docker rename nginx nginx-server
```

## Criando containers
**Sintaxe**

`docker create --name <nome-do-container> <nome-da-imagem>`

**Exemplo de uso:**
```sh
docker create --name nginx-server nginx
```
> O comando **create** apenas cria o container, sem executar. Diferente do docker run que cria e executa

## Removendo container
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

## Removendo vários containers
**Exemplos de uso:**
```sh
docker rm 49b000cf2fdb 1ed329fa2fb2 28ab953b6465
```

```sh
docker rm -f $(docker ps -qa)
```

## Executando containers
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

## Parando containers
**Sintaxe**

`docker stop <container_id>`  
`docker stop <container_name>`

**Exemplos de uso:**
```sh
docker stop app
```

```sh
docker stop $(docker ps -qa)
```

> **Observação:** esse último exemplo, para todos os containers em execução

## Executando comandos dentro de um container em execução
**Sintaxe**

`docker exec <container_id> <comando>`  
`docker exec <container_name> <comando>`

**Exemplo de uso:**
```sh
docker exec app ls
```

## Acessando o terminal dentro de um container
```sh
docker exec -it app bash
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


----------------
-- BIND MOUNT --
----------------
docker run -d --name nginx-server -p 8080:80 -v "$(pwd)"/html:/usr/share/nginx/html nginx
docker run -d --name nginx-server -p 8080:80 --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html nginx
docker run -d --name nginx-server -p 8080:80 --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html,ro nginx (somente leitura)

Uma das diferenças entre os dois comandos é que, o comando -v, ele cria a pasta na origem, mesmo que ela não exista


-------------
-- VOLUMES --
-------------
docker volume ls (lista os volumes criados)
docker volume create meuvolume (cria um volume com o nome meuvolume)
docker volume inspect meuvolume (retorna um json com os metadados do volume)
docker volume prune (limpa os volumes locais que não estão sendo usados)

-- Para usar um volume no container
docker run -d --name nginx-server -p 8080:80 --mount type=volume,source=meuvolume,target=/app nginx


-------------
-- IMAGENS --
-------------
docker images (exibe as imagens que tem salvas no computador)
docker pull ubuntu (apenas baixa a imagem do container register da DockerHub para o computador)
docker rmi ubuntu (remove a imagem ubuntu)


-------------
-- NETWORK --
-------------
-- Tipos de networks:
--	bridge (default, quando um container precisa se comunicar com outro)
--	host (mesma rede do host, onde está executando o docker. O Host consegue acessar os containers sem o mapeamento)
--	overlay (usado quando precisa de vários container para escalonamento - Swarm)
--	maclan (mac address, quando precisa isolar um container)
--	none (container sem interface de rede)

docker network
docker network ls (lista redes existentes)
docker network prune (remove redes que não estão em utilização)
docker network inspect bridge (retorna json com dados da network)
docker network inspect f293d0c2b0e5

docker network create --driver bridge dalq (criar a rede em modo brigde chamada dalq)

docker run -dit --name ubuntu1 --network dalq ubuntu bash
docker run -dit --name ubuntu2 --network dalq ubuntu bash
docker run -dit --name ubuntu3 ubuntu bash

-- No exemplo acima, a terceira maquina (ubuntu3) estará em uma rede diferente das outras duas
-- Além disso, quando crio um container apontando pra uma network, eu consigo fazer um ping pelo nome da máquina

docker network connect dalq ubuntu3

-- Ao fazer isso, eu conecto a máquina ubuntu3 a mesma rede das demais, sendo possível comunicar entre elas

docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ubuntu1
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' f293d0c2b0e5

-- Exemplo: criando um container na rede host:
docker run --rm -d --name nginx --network host nginx

-- Desse modo, já consigo acessar direto assim: http://localhost

-- Caso o container esteja em modo bridge e precisar acessar o host, pode ser feito dessa forma também:
curl http://host.docker.internal:8000 (imaginando que tenho um servidor web na maquina local rodando na porta 8000)


----------
-- LOGS --
----------
docker logs [container_id]
docker logs [container_name]
docker logs -f laravel (fica em modo de exibição dos logs)

-- Para ver a saida em modo background, podemos usar:
docker logs server (exibe o log e volta pro terminal)
docker logs -f server (exibe o log e trava o terminal)
