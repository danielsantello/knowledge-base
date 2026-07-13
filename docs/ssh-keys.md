# SSH - Autenticação por Chaves (SSH Keys)

## Sumário

- [O que é?](#o-que-é)
- [Quando utilizar?](#quando-utilizar)
- [Conceitos](#conceitos)
	- [Chave privada](#chave-privada)
	- [Chave pública](#chave-pública)
	- [authorized_keys](#authorized_keys)
 - [Estrutura da pasta .ssh (Windows)](#estrutura-da-pasta-ssh-windows)
	- [Arquivo config](#arquivo-config)
	- [Arquivo known_hosts](#arquivo-known_hosts)
 - [Listando as chaves existentes](#listando-as-chaves-existentes)
 - [Exibindo o conteúdo do arquivo config](#exibindo-o-conteúdo-do-arquivo-config)
 - [Exibindo a chave pública](#exibindo-a-chave-pública)
 - [Comentário da chave](#comentário-da-chave)
 - [Gerando um novo par de chaves](#gerando-um-novo-par-de-chaves)
 - [Obtendo a chave pública a partir da chave privada](#obtendo-a-chave-pública-a-partir-da-chave-privada)
 - [Criando a estrutura SSH no Linux](#criando-a-estrutura-ssh-no-linux)
 - [Copiando a chave pública para o servidor](#copiando-a-chave-pública-para-o-servidor)
 - [Testando uma conexão](#testando-uma-conexão)
 - [Como funciona internamente](#como-funciona-internamente)
 - [Fluxo completo](#fluxo-completo)
 - [Boas práticas](#boas-práticas)
 - [Problemas comuns](#problemas-comuns)
 	- [Continua pedindo senha](#continua-pedindo-senha)

<br>

### O que é?
A autenticação por chaves permite acessar servidores SSH sem utilizar a senha da conta remota.

> [!NOTE]
>
> A chave privada pode ser protegida por uma passphrase. Nesse caso, a passphrase protege o arquivo da chave e não corresponde à senha do usuário no servidor.

Ela utiliza um par de chaves:

- Chave privada (fica apenas com você)
- Chave pública (é instalada no servidor)

Durante a conexão, o cliente utiliza a chave privada para provar sua identidade.

O servidor verifica essa prova utilizando a chave pública cadastrada no arquivo `authorized_keys`. Quando a validação é bem-sucedida, o acesso é liberado sem solicitar a senha do usuário.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Quando utilizar?

A autenticação por chaves é recomendada para:

- acesso a servidores Linux via SSH;
- desenvolvimento remoto utilizando VS Code Remote SSH;
- autenticação no GitHub, GitLab e outros serviços Git;
- automações (CI/CD);
- scripts que necessitam acessar servidores sem interação humana.

Em praticamente todos os cenários, a autenticação por chaves é mais segura do que utilizar senhas.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Conceitos
#### Chave privada
Exemplo:
```sh
dalq_api_ed25519
```

- Nunca deve ser compartilhada.
- Fica somente na máquina cliente.
- É utilizada para provar sua identidade.

> [!WARNING]
>
> Nunca copie sua chave privada (id_ed25519, dalq_api_ed25519, etc.) para servidores Linux.
> 
> Apenas a chave pública (*.pub) deve ser instalada no servidor.

#### Chave pública
Exemplo:
```sh
dalq_api_ed25519.pub
```

Pode ser compartilhada livremente.

Ela será instalada nos servidores que você deseja acessar.

> [!NOTE]
> A mesma chave pública pode ser instalada em diversos servidores.

#### authorized_keys
No Linux:
```sh
~/.ssh/authorized_keys
```

Este arquivo contém todas as chaves públicas autorizadas.

Exemplo:
```sh
ssh-ed25519 AAAAC3Nz..... notebook-pessoal

ssh-ed25519 AAAAC3Nz..... desktop

ssh-ed25519 AAAAC3Nz..... servidor-ci
```

> [!NOTE]
>
> O arquivo `authorized_keys` pode conter dezenas ou centenas de chaves públicas.
> Cada linha representa um cliente autorizado a acessar aquele usuário.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Estrutura da pasta .ssh (Windows)
Local:
```powershell
C:\Users\Daniel\.ssh
```

Exemplo:
```sh
config
dalq_api_ed25519
dalq_api_ed25519.pub
known_hosts
```

#### Arquivo config
Arquivo responsável por armazenar aliases e configurações das conexões SSH.

Exemplo:
```sshconfig
Host dalq-api
    HostName 192.168.15.103
    User dalq
    Port 22
    IdentityFile C:\Users\Daniel\.ssh\dalq_api_ed25519
    IdentitiesOnly yes
```

Explicação:
- `Host` → nome (apelido) da conexão
- `HostName` → IP ou hostname real
- `User` → usuário utilizado na conexão
- `Port` → porta do SSH
- `IdentityFile` → caminho da chave privada
- `IdentitiesOnly` → utiliza apenas a chave especificada

#### Arquivo known_hosts
Armazena a identidade dos servidores já acessados.

Serve para evitar ataques do tipo "man in the middle".

Normalmente não deve ser editado manualmente.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Listando as chaves existentes
PowerShell:
```powershell
dir $HOME\.ssh
```

ou
```powershell
Get-ChildItem $HOME\.ssh
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Exibindo o conteúdo do arquivo config
PowerShell:
```powershell
Get-Content $HOME\.ssh\config
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Exibindo a chave pública
PowerShell:
```powershell
Get-Content $HOME\.ssh\dalq_api_ed25519.pub
```

Saída:
```sh
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... notebook-pessoal
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Comentário da chave
A estrutura da chave pública é:
```sh
tipo_da_chave chave comentário
```

Exemplo:
```sh
ssh-ed25519 AAAAC3Nza... dalq-api
```

O comentário é apenas identificador.

Pode ser alterado sem invalidar a chave.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Gerando um novo par de chaves
Algoritmo recomendado atualmente:
```powershell
ssh-keygen -t ed25519
```

> [!TIP]
> 
> O algoritmo ED25519 é uma escolha moderna e recomendada para novas chaves por oferecer chaves compactas, bom desempenho e segurança robusta.

Personalizando o nome:
```powershell
ssh-keygen -t ed25519 -f $HOME\.ssh\dalq_api_ed25519
```

Definindo comentário:
```powershell
ssh-keygen -t ed25519 -C "Notebook Pessoal"
```

Ou tudo junto:
```powershell
ssh-keygen `
    -t ed25519 `
    -C "Notebook Pessoal" `
    -f $HOME\.ssh\dalq_api_ed25519
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Obtendo a chave pública a partir da chave privada
```powershell
ssh-keygen -y -f $HOME\.ssh\dalq_api_ed25519
```

Útil para confirmar que a chave privada corresponde à chave pública.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Criando a estrutura SSH no Linux
```sh
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Explicação:
```sh
mkdir -p
```

Cria o diretório caso não exista.

```sh
chmod 700
```

Permissões:
- dono: leitura, escrita e execução
- grupo: nenhum acesso
- outros: nenhum acesso

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Copiando a chave pública para o servidor
Windows:
```powershell
type $HOME\.ssh\dalq_api_ed25519.pub | ssh dalq-api "cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

Explicação:
```sh
type
```

Lê o conteúdo da chave pública.

```sh
|
```

Envia a saída para outro comando.

```sh
cat >>
```

Acrescenta o conteúdo ao arquivo authorized_keys.

Se o arquivo não existir, ele será criado.

```sh
chmod 600
```

Permissões do arquivo:

- dono: leitura e escrita
- grupo: nenhum acesso
- outros: nenhum acesso

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Testando uma conexão
```sh
ssh dalq-api
```

O alias e as demais configurações da conexão são obtidos do arquivo `config`.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Como funciona internamente
```text
Cliente (Windows)
│
├── Chave privada
│
│  (prova criptograficamente sua identidade)
│
▼
Servidor (Linux)
│
├── authorized_keys
│
│  (contém as chaves públicas autorizadas)
│
▼
As chaves correspondem?
│
├── Sim → acesso liberado
└── Não → solicita senha (ou nega o acesso)
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Fluxo completo
```text
1. Gerar o par de chaves
↓
2. Instalar a chave pública no servidor
↓
3. Configurar o arquivo config
↓
4. Testar a conexão
↓
5. Utilizar normalmente
```

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Boas práticas
- Nunca compartilhe a chave privada.  
- Faça backup da chave privada.  
- Utilize nomes descritivos para as chaves.  
- Utilize aliases no arquivo `config`.  
- Prefira o algoritmo `ed25519`.  
- Mantenha permissões corretas em `~/.ssh`.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>

### Problemas comuns
#### Continua pedindo senha
Possíveis causas:

- chave pública não copiada para `authorized_keys`;
- permissões incorretas em `~/.ssh`;
- permissões incorretas em `authorized_keys`;
- usuário incorreto;
- `IdentityFile` apontando para outra chave;
- serviço SSH configurado para não aceitar autenticação por chave.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>
