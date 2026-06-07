#### Imagens

![Logo](../logo.png)

<p align="center">
  <a href="https://dalqsistemas.com.br/" target="blank"><img src="https://fullcycle.com.br/wp-content/themes/fullcycle/assets/images/logo-fc.svg"/></a>
</p>

dockerize ![version v0.12.0](https://img.shields.io/badge/version-v0.12.0-brightgreen.svg) ![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)
=============

# JWT Token Manager

[![CI](https://github.com/dev-toolbelt/jwt-token-manager/actions/workflows/ci.yml/badge.svg)](https://github.com/dev-toolbelt/jwt-token-manager/actions/workflows/ci.yml)
[![Coverage](https://codecov.io/gh/dev-toolbelt/jwt-token-manager/branch/main/graph/badge.svg)](https://codecov.io/gh/dev-toolbelt/jwt-token-manager)
[![Latest Stable Version](https://poser.pugx.org/dev-toolbelt/jwt-token-manager/v/stable)](https://packagist.org/packages/dev-toolbelt/jwt-token-manager)
[![Total Downloads](https://poser.pugx.org/dev-toolbelt/jwt-token-manager/downloads)](https://packagist.org/packages/dev-toolbelt/jwt-token-manager)
[![License](https://poser.pugx.org/dev-toolbelt/jwt-token-manager/license)](https://packagist.org/packages/dev-toolbelt/jwt-token-manager)
[![PHP Version](https://img.shields.io/packagist/php-v/dev-toolbelt/jwt-token-manager)](https://packagist.org/packages/dev-toolbelt/jwt-token-manager)

# Node Version Manager [![Tests](https://github.com/nvm-sh/nvm/actions/workflows/tests-fast.yml/badge.svg?branch=master)][3] [![nvm version](https://img.shields.io/badge/version-v0.40.5-yellow.svg)][4] [![CII Best Practices](https://bestpractices.dev/projects/684/badge)](https://bestpractices.dev/projects/684)

#### Títulos

# Título 1
## Título 2
### Título 3
#### Título 4

#### Listagens

- Item 1
- Item 2

* 1. Item 1
* 2. Item 2

- Item 1
  + Sub-item 1
  + Sub-item 2

- Item 2
  - Sub-item 1
  - Sub-item 2

#### Links

Projeto [desafio Full Cycle](https://github.com/danielsantello/full-cycle-rocks)

#### Destaques

Destacando uma `palavra` em uma frase

Destacando uma *palavra* em uma frase

Destacando uma **palavra** em uma frase

Destacando uma <kbd>palavra</kbd> em uma frase

> Destacando uma frase

> [!WARNING]
> io.js was a [fork of Node.js](https://en.wikipedia.org/wiki/Node.js#History), created in 2014 and merged back in 2015. io.js shipped v1, v2, and v3 release lines; post-merge, node.js began releasing with v4.

#### Separadores

---

#### Códigos
``` sh
brew tap ktr0731/evans
brew install evans
```

``` proto
message FullName {
  string first_name = 1;
  string last_name = 2;
}

message Request {
  string nickname = 1;
  FullName full_name = 2;
}
```

``` json
{
  "nickname": "myamori",
  "fullName": {
    "firstName": "aoi"
  }
}
```

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

``` Dockerfile
ENV DOCKERIZE_VERSION v0.12.0

RUN apt-get update \
    && apt-get install -y wget \
    && wget -O - https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz | tar xzf - -C /usr/local/bin \
    && apt-get autoremove -yqq --purge wget && rm -rf /var/lib/apt/lists/*
```

```php
use DevToolbelt\JwtTokenManager\JwtConfig;
use DevToolbelt\JwtTokenManager\JwtTokenManager;

// Create configuration
$config = new JwtConfig(
    privateKey: file_get_contents('/path/to/private.key'),
    publicKey: file_get_contents('/path/to/public.key'),
    issuer: 'https://api.yourapp.com'
);

// Initialize the manager
$manager = new JwtTokenManager($config);

// Generate a token
$token = $manager->encode('user-123', [
    'name' => 'John Doe',
    'role' => 'admin'
]);

// Decode and validate the token
$payload = $manager->decode($token);

echo $payload->getSubject();      // "user-123"
echo $payload->getClaim('name');  // "John Doe"
echo $payload->getClaim('role');  // "admin"
```

``` javascript
Moip.Validator.isSecurityCodeValid("5105105105105100", "123");    //return true
Moip.Validator.isSecurityCodeValid("5105105105105100", "12");     //return false
```

#### Tabelas

| Algorithm | Type | Description |
|-----------|------|-------------|
| `HS256`, `HS384`, `HS512` | HMAC | Symmetric key algorithms |
| `RS256`, `RS384`, `RS512` | RSA | Asymmetric RSA algorithms |
| `ES256`, `ES384`, `ES512` | ECDSA | Elliptic Curve algorithms |
| `PS256`, `PS384`, `PS512` | RSA-PSS | RSA with PSS padding |
| `EdDSA` | EdDSA | Edwards-curve Digital Signature |

#### Ícones

⚠️  Warning

Made with ❤️ by [Dev Toolbelt](https://github.com/Dev-Toolbelt)

**[⬆ voltar ao topo](#table-of-contents)**


## Sumário

<details open>
  <summary>
    <strong>WSL</strong>
  </summary>

  - [O que é o WSL2](#o-que-é-o-wsl2)
  - [Requisitos mínimos](#requisitos-mínimos)
  - [Instalação do WSL 2](#instalação-do-wsl-2)
    - [Windows Update](#windows-update)
    - [Atualizar o WSL](#atualizar-o-wsl)
    - [(Opcional) Habilitando WSL em versões antigas do Windows 10](#opcional-habilitando-wsl-em-versões-antigas-do-windows-10)
    - [Atribuir a versão default do WSL para a versão 2](#atribuir-a-versão-default-do-wsl-para-a-versão-2)
    - [Instale o Ubuntu](#instale-o-ubuntu)
    - [(Opcional) Alterar a versão de uma distribuição Linux de WSL 1 para WSL 2](#opcional-alterar-a-versão-de-uma-distribuição-linux-de-wsl-1-para-wsl-2)
    - [Instalação do WSL 2 via Microsoft Store (alternativa)](#instalação-do-wsl-2-via-microsoft-store-alternativa)
    - [Integração com VSCode](#integração-com-vscode)
  - [Windows Terminal como terminal padrão de desenvolvimento para Windows](#windows-terminal-como-terminal-padrão-de-desenvolvimento-para-windows)
  - [O que o WSL 2 pode usar de recursos da minha máquina?](#o-que-o-wsl-2-pode-usar-de-recursos-da-sua-máquina)
  - [Systemd](#systemd)
</details>

<details close>
  <summary>
    <strong>WSLg</strong>
  </summary>

  - [O que é WSLg](#o-que-é-wslg)
  - [Arquitetura do WSLg](#arquitetura-do-wslg)
  - [Como ativar o WSLg](#como-ativar-o-wslg)
</details>

<details close>
  <summary>
    <strong>Docker</strong>
  </summary>

  - [O que é o Docker](#o-que-é-docker)
  - [Porque usar WSL 2 + Docker para desenvolvimento](#porque-usar-wsl-2--docker-para-desenvolvimento)
  - [Modo de usar Docker no Windows](#modos-de-usar-docker-no-windows)
    - [1 - (Obsoleto) Docker Toolbox](#1-obsoleto-docker-toolbox)
    - [2 - (Obsoleto) Docker Desktop com Hyper-V](#2-obsoleto-docker-desktop-com-hyper-v)
    - [3 - Docker Desktop com WSL 2](#3-docker-desktop-com-wsl2)
      - [Vantagens](#docker-desktop-vantagens)
      - [Desvantagens](#docker-desktop-desvantagens)
    - [4 - Docker Engine (Docker Nativo) diretamente instalado no WSL2](#4-docker-engine-docker-nativo-diretamente-instalado-no-wsl2)
      - [Vantagens](#docker-engine-vantagens)
      - [Desvantagens](#docker-engine-desvantagens)
  - [Qual modo de usar Docker no Windows escolher?](#qual-modo-de-usar-docker-no-windows-escolher)
  - [Instalação do Docker](#instalação-do-docker)
    - [1 - Instalar o Docker com Docker Desktop](#1---instalar-o-docker-com-docker-desktop)
      - [Ativar o Docker na distribuição Linux](#ativar-o-docker-na-distribuição-linux)
      - [Otimizar recursos do Docker Desktop](#otimizar-recursos-do-docker-desktop)
      - [Aplicar autoMemoryReclaim no WSL 2](#aplicar-automemoryreclaim-no-wsl-2)
    - [2 - Instalar o Docker com Docker Engine (Docker Nativo)](#instalar-o-docker-com-docker-engine-docker-nativo)
      - [Erro ao iniciar o Docker no Ubuntu 22.04](#erro-ao-iniciar-o-docker-no-ubuntu-2204)
      - [Iniciar o Docker automaticamente no WSL](#iniciar-o-docker-automaticamente-no-wsl)
      - [Docker com Systemd](#docker-com-systemd) 
</details>


<details close>
  <summary>
    <strong>Dicas e truques</strong>
  </summary>

  - [Dicas e truques básicos com WSL 2](#dicas-e-truques-básicos-com-wsl-2)
    - [Performance ao usar o WSL 2](#performance-ao-usar-o-wsl-2)
    - [Windows Terminal](#windows-terminal)
    - [Acessar disco e outros dispositivos do Windows](#acessar-disco-e-outros-dispositivos-do-windows)
    - [Acessar o WSL 2 pelo Windows](#acessar-o-wsl-2-pelo-windows)
    - [Integração com VSCode](#integração-com-vscode)
    - [Executáveis do Windows no WSL 2](#executáveis-do-windows-no-wsl-2)
    - [Listando as distribuições Linux instaladas no WSL 2](#listando-as-distribuições-linux-instaladas-no-wsl-2)
    - [Parando todas as distribuições Linux no WSL 2](#parando-todas-as-distribuições-linux-no-wsl-2)
    - [Backup e restauração de distribuições Linux no WSL 2](#backup-e-restauração-de-distribuições-linux-no-wsl-2)
    - [Compactação do disco virtual do WSL 2](#compactação-do-disco-virtual-do-wsl-2)
    - [Rede em modo LAN e VPN](#rede-em-modo-lan-e-vpn)
    - [Ativar BuildKit no Docker](#ativar-buildkit-no-docker)
    - [Iniciar o Docker automaticamente no WSL 2](#iniciar-o-docker-automaticamente-no-wsl-2)
    - [Liberar memória RAM do WSL 2](#liberar-memória-ram-do-wsl-2)
    - [Arquivos .wslconfig e wsl.conf](#arquivos-wslconfig-e-wslconf)
    - [Expandir o disco virtual do WSL 2](#expandir-o-disco-virtual-do-wsl-2)
</details>

<details close>
  <summary>
    <strong>Dúvidas</strong>
  </summary>

  - [Dúvidas](#dúvidas)
      - [Preciso de uma licença PRO do Windows 10/11 para usar o WSL 2?](#preciso-de-uma-licença-pro-do-windows-1011-para-usar-o-wsl-2)
      - [Posso continuar desenvolvendo no Windows sem usar o WSL 2?](#posso-continuar-desenvolvendo-no-windows-sem-usar-o-wsl-2)
      - [O WSL 2 funciona junto com outras máquinas virtuais como VirtualBox ou VMWare?](#o-wsl-2-funciona-junto-com-outras-máquinas-virtuais-como-virtualbox-ou-vmware)
      - [É possível acessar aplicações rodando no WSL 2 pelo Windows?](#é-possível-acessar-aplicações-rodando-no-wsl-2-pelo-windows)
      - [É possível rodar aplicações gráficas no WSL 2?](#é-possível-rodar-aplicações-gráficas-no-wsl-2)
      - [Posso usar o WSL em cenários de produção?](#posso-usar-o-wsl-em-cenários-de-produção)
      - [Posso rodar o Docker Engine junto com o Docker Desktop?](#posso-rodar-o-docker-engine-junto-com-o-docker-desktop)
      - [Dúvidas sobre o Docker Desktop](#dúvidas-sobre-o-docker-desktop)
      - [Quer configurar um ambiente mais produtivo no Windows?](#quer-configurar-um-ambiente-mais-produtivo-no-windows)

</details>


## <a name='translation'>Traduções</a>

  Este style guide está disponível em outros idiomas:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese(Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese(Simplified)**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)
