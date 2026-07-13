# SSH - Autenticação por Chaves (SSH Keys)

## Sumário

- [O que é?](#o-que-é)
- [Conceitos](#conceitos)
	- [Chave privada](#chave-privada)
	- [Chave pública](#chave-pública)
	- [authorized_keys](#authorized_keys)

<br>

### O que é?
A autenticação por chaves substitui o uso de senhas durante a conexão SSH.

Ela utiliza um par de chaves:

- Chave privada (fica apenas com você)
- Chave pública (é instalada no servidor)

Quando a chave pública do servidor corresponde à chave privada do cliente, o acesso é liberado sem necessidade de senha.

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

#### Chave pública
Exemplo:
```sh
dalq_api_ed25519.pub
```

Pode ser compartilhada livremente.

Ela será instalada nos servidores que você deseja acessar.

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

Cada linha representa uma chave autorizada.

<div align="right"><a href="#sumário">Sumário [↑]</a></div>
<div align="center">· · ·</div>




