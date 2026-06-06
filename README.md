# Documentação

## WSL - Windows Subsystem for Linux

Para trabalhar com o WSL, acessar o PowerShell como administrador

### Atualizar o WSL
```sh
wsl --update
```

Para selecionar versão
wsl --set-default-version 2

Para exibir as distribuições existentes
wsl --list --online

Para instalar uma outra distribuição
wsl --install -d nome-da-distribuicao
wsl --install -d Ubuntu
wsl --install -d Debian

Para acessar:
wsl.exe -d Debian
wsl.exe -d Ubuntu

Para ver as versões instaladas
wsl --list -v

Caso seja necessário mudar para a WSL 1, digitar:
wsl --set-version Debian 1

OBS.: Até o momento temos apenas duas versões da WSL. Docker só funciona na versão WSL 2

Para desativar todas as instâncias ativas
wsl --shutdown

Para parar somente uma distribuição Linux específica
wsl --t <distribution name>

Para remover uma distribuição
wsl --unregister <distribution name>
wsl --unregister Ubuntu
wsl --unregister Debian

Limitendo recursos da máquina principal
Editar o arquivo C:\Users\<seu_usuario>\.wslconfig
[wsl2]
memory=8GB
processors=4
swap=1GB

Pacotes básicos para instalar nas distribuições Ubuntu:
apt-get update && \
apt-get install iproute2 net-tools iputils-ping dnsutils traceroute curl wget

Gerando chaves públicas e privadas para github
ls -al ~/.ssh
ssh-keygen -t ed25519 -C "daniel@grupotwo.com.br"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub

git remote set-url origin git@github.com:danielsantello/full-cycle-rocks.git
