# Docker Compose

## Sumário

- [Subindo Containers](#subindo-containers)
- [Parando Containers](#parando-containers)
- [Exibindo Containers](#exibindo-containers)
- [Exibindo Logs](#exibindo-logs)
- [Resumo dos principais comandos](#resumo-dos-principais-comandos)

> [!TIP]
> por padrão, cria-se um arquivo chamado `docker-compose.yaml` na pasta principal do projeto.

## Subindo Containers
```sh
docker compose up
docker compose up -d
docker compose up --build
docker compose up -d --build
docker compose -f <nome-do-arquivo> up
```

> [!NOTE]
> - o comando `docker compose up` lê o arquivo `docker-compose.yaml` e cria/inicia os containers definidos nele.
> - o parâmetro `-d` executa os containers em segundo plano (detached mode).
> - o parâmetro `--build` força a reconstrução das imagens antes da inicialização dos containers.
> - o parâmetro `-f` permite utilizar um arquivo diferente de `docker-compose.yaml`.

## Parando Containers
```sh
docker compose down
docker compose down -v
docker compose down --rmi all
docker compose down -v --rmi all
```

> [!NOTE]
> - o comando `docker compose down` remove os containers e redes criados pelo compose.
> - o parâmetro `-v` também remove os volumes declarados no `docker-compose.yaml`.
> - o parâmetro `--rmi all` remove as imagens utilizadas pelos serviços definidos no compose.
> - o parâmetro `-v --rmi all` remove containers, redes, volumes e imagens.

## Exibindo Containers
```sh
docker compose ps
docker compose ps -a
```

> [!NOTE]
> - o comando `docker compose ps` exibe apenas os containers em execução.
> - o parâmetro `-a` exibe também os containers parados.

## Exibindo Logs
```sh
docker compose logs
docker compose logs -f
docker compose logs <nome-do-serviço>
docker compose logs -f <nome-do-serviço>
```

> [!NOTE]
> - o comando `docker compose logs` exibe os logs de todos os serviços definidos no `docker-compose.yaml`.
> - o parâmetro -f (follow) mantém a exibição aberta, mostrando novas mensagens conforme elas são geradas.
> - o parâmetro <nome-do-servico> filtra os logs de um único serviço.
> - o parâmetro `<nome-do-serviço>` corresponde ao nome definido na seção `services` do arquivo `docker-compose.yaml`.

## Resumo dos principais comandos
```sh
docker compose up
docker compose up -d --build
docker compose -f <nome-do-arquivo> up
docker compose ps
docker compose logs -f
docker compose down

# para recriar um ambiente, garantindo que tudo seja recriado do zero
docker compose down -v
docker compose up --build
```
