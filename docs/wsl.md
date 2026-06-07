
# WSL
> Para trabalhar com o WSL (Windows Subsystem for Linux), execute o PowerShell como administrador.

## Sumário

- [Atualizando o WSL](#atualizando-o-wsl)
- [Selecionando a versão](#selecionando-a-versão)
- [Mudando a versão de uma distribuição](#mudando-a-versão-de-uma-distribuição)
- [Exibindo distribuições existentes](#exibindo-distribuições-existentes)
- [Instalando distribuições](#instalando-distribuições)
- [Acessando distribuições](#acessando-distribuições)
- [Exibindo as distribuições instaladas](#exibindo-as-distribuições-instaladas)
- [Parando todas as instâncias ativas](#parando-todas-as-instâncias-ativas)
- [Parando uma distribuição](#parando-uma-distribuição)
- [Removendo uma distribuição](#removendo-uma-distribuição)
- [Limitando recursos da máquina principal](#limitando-recursos-da-máquina-principal)
- [Pacotes básicos para instalar nas distribuições Ubuntu](#pacotes-básicos-para-instalar-nas-distribuições-Ubuntu)

## Atualizando o WSL
```sh
wsl --update
```

## Selecionando a versão
**Sintaxe**

`wsl --set-default-version <numero-da-versao>`

**Exemplo de uso:**
```sh
wsl --set-default-version 2
```

## Mudando a versão de uma distribuição
**Sintaxe**

`wsl --set-version <nome-da-distribuicao> <numero-da-versao>`

**Exemplo de uso:**
```sh
wsl --set-version Ubuntu 1
```
> **Observação:** Atualmente existem apenas duas versões do WSL. O Docker funciona apenas com WSL 2.

## Exibindo distribuições existentes
```sh
wsl --list --online
```

## Instalando distribuições
**Sintaxe**

`wsl --install -d <nome-da-distribuicao>`

**Exemplo de uso:**
```sh
wsl --install -d Ubuntu
```

## Acessando distribuições
**Sintaxe**

`wsl.exe -d <nome-da-distribuicao>`

**Exemplo de uso:**
```sh
wsl.exe -d Ubuntu
```

## Exibindo as distribuições instaladas
```sh
wsl --list -v
```

## Parando todas as instâncias ativas
```sh
wsl --shutdown
```

## Parando uma distribuição
**Sintaxe**

`wsl --t <nome-da-distribuicao>`

**Exemplo de uso:**
```sh
wsl --t Ubuntu
```

## Removendo uma distribuição
**Sintaxe**

`wsl --unregister <nome-da-distribuicao>`

**Exemplo de uso:**
```sh
wsl --unregister Ubuntu
```

## Limitando recursos da máquina principal
Editar o arquivo `C:\Users\<seu_usuario>\.wslconfig`

```sh
[wsl2]
memory=8GB
processors=4
swap=1GB
```

## Pacotes básicos para instalar nas distribuições Ubuntu
```sh
apt-get update && \
apt-get install iproute2 net-tools iputils-ping dnsutils traceroute curl wget
```
