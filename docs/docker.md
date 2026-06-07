# Docker

## Sumário

- [Service](#service)
- [Help](#help)
- [Testando](#testando)
- [Listando containers](#listando-containers)
  

## Service
**Checando se o serviço do Docker está ativo**

```sh
sudo service docker status
```

```sh
dalq@Daniel:~$ sudo service docker status
○ docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
     Active: inactive (dead) since Sun 2026-06-07 19:56:07 -03; 5s ago
	 ...
```

**Ativando o serviço do Docker**

```sh
sudo service docker start
```

```sh
dalq@Daniel:~$ sudo service docker status
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-06-07 19:58:06 -03; 2s ago
     ...
```

**Parando o serviço do Docker**

```sh
sudo service docker stop
```

## Help
```sh
docker run --help
docker ps --help
```

## Testando
```sh
docker run hello-world
```

```sh
dalq@Daniel:~$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Listando containers
```sh
docker ps
```
por padrão, docker ps mostra apenas os containers em execução

```sh
docker ps -a
```
lista containers em execução e também os que já finalizaram

```sh
docker ps -a -q
```
lista apenas os id dos containers


---------------------
-- MODO INTERATIVO --
---------------------
-- Criar o container e já rodar em modo interativo
docker run -it ubuntu bash (sempre que precisar passar parâmetros, informar depois do run)

-- Explicação de cada comando:
docker run -> executar um container
-it => parâmetros, mesmo que -i -t
	-i => modo interativo, vai manter o stdin ativo, para manter o processo rodando. Vincula o terminal ao container
 	-t => tty, permite rodar comandos no terminal
ubuntu => nome da imagem que será executada
bash => comando que será executado


----------------------------
-- EXECUTANDO E REMOVENDO --
----------------------------
-- Cria o container e exclui quando terminar a execução
docker run -it --rm ubuntu bash (sempre que precisar passar parâmetros, informar depois do run)


-------------------------------
-- PUBLICANDO PORTAS NO HOST --
-------------------------------
docker run -p 8080:80 nginx
-- O comando acima subirá o nginx que responderá na porta 80 por padrão
-- Porém, essa porta 80 é a do container, ou seja, se tentar acesssar http://localhost, não vai conseguir
-- Precisamos fazer um mapeamento da máquina para a máquina do container
-- O parâmetro -p 8080:80 fará esse mapeamento
-- Ao acessar a porta 8080 da máquina local, a requisição será encaminhada pra porta 80 do container


---------------------------------------------------
-- CRIANDO UM CONTAINER EM BACKGROUND (DETACHED) --
---------------------------------------------------
docker run -d -p 8080:80 nginx (estamos fazendo um redirecionamento da porta local 8080 para a porta 80 do container)
-- o container ficará rodando em background, não mais associado (attached) ao terminal

-- Para ver a saida em modo background, podemos usar:
docker logs server (exibe o log e volta pro terminal)
docker logs -f server (exibe o log e trava o terminal)


-------------------------------------------------
-- ASSOCIANDO (ATTACH) O TERMINAL AO CONTAINER --
-------------------------------------------------
docker attach nginx


----------------------------------
-- CRIANDO UM CONTAINER NOMEADO --
----------------------------------
docker run -d -p 8080:80 --name nginx-server nginx


---------------------------
-- RENOMEANDO CONTAINERS --
---------------------------
docker rename nginx nginx-server


------------------------
-- CRIANDO CONTAINERS --
------------------------
docker create --name nginx-server nginx

-- O comando create apenas cria o container, sem executar. Diferente do docker run que cria e executa


--------------------------
-- REMOVENDO CONTAINERS --
--------------------------
docker rm <container_id>
docker rm <container_name>

OSB.: Não é possível remover um container em execução. Se necessário, digitar -f para forçar (force) a remoção:
docker rm <container_id> -f
docker rm <container_name> -f

-- Para remover vários containers
docker rm 49b000cf2fdb 1ed329fa2fb2 28ab953b6465
docker rm $(docker ps -qa) -f


-------------------------------------
-- EXECUTANDO E PARANDO CONTAINERS --
-------------------------------------
docker start <container_id>
docker start <container_name>
docker stop <container_id>
docker stop <container_name>

-- Parar e executar todos os containers
docker start $(docker ps -qa)
docker stop $(docker ps -qa)


----------------------------------------------------------
-- EXECUTAR COMANDOS DENTRO DE UM CONTAINER EM EXECUÇÃO --
----------------------------------------------------------
docker exec <container_id> ls
docker exec <container_name> ls
docker exec nginx-server ls


-------------------------------------------
-- ACESSAR O BASH DENTRO DE UM CONTAINER --
-------------------------------------------
docker exec -it nginx-server bash


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
