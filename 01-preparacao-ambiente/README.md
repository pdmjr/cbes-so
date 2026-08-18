# 01 — Preparação do Ambiente

Nesta aula será configurado o ambiente utilizado nas atividades práticas da disciplina de **Sistemas Operacionais**.

A proposta é trabalhar com uma máquina virtual executando **Ubuntu Linux**, acessada remotamente por meio de **SSH**. O código-fonte será editado utilizando o **Visual Studio Code**, enquanto a compilação e execução dos programas serão realizadas diretamente no terminal da máquina Linux.

Ao final desta atividade, o ambiente deverá possuir a seguinte estrutura:

```text
Computador do aluno
      │
      │ SSH
      ▼
Máquina Virtual Ubuntu
      │
      ├── GCC
      ├── Make
      ├── Git
      ├── Zsh
      └── Oh My Zsh
```

O VS Code será executado no computador do aluno, mas os arquivos e comandos estarão efetivamente sendo executados na máquina virtual Ubuntu.

---

# 1. Máquina Virtual Ubuntu

Durante as aulas práticas utilizaremos uma máquina virtual com **Ubuntu Linux** previamente instalado.

A máquina virtual será utilizada como ambiente principal para:

* executar comandos Linux;
* desenvolver programas em C;
* compilar programas;
* criar e manipular processos;
* analisar memória;
* utilizar chamadas de sistema;
* experimentar mecanismos de comunicação entre processos;
* observar recursos internos do sistema operacional.

## 1.1 Verificando a versão do Ubuntu

Abra o terminal da máquina virtual e execute:

```bash
lsb_release -a
```

Outra opção é:

```bash
cat /etc/os-release
```

Exemplo de saída:

```text
NAME="Ubuntu"
VERSION="24.04 LTS (Noble Numbat)"
ID=ubuntu
ID_LIKE=debian
```

Também podemos verificar informações sobre o kernel:

```bash
uname -a
```

Uma saída típica seria semelhante a:

```text
Linux ubuntu 6.8.0-31-generic #31-Ubuntu SMP x86_64 GNU/Linux
```

Para visualizar apenas a versão do kernel:

```bash
uname -r
```

Esses comandos serão úteis ao longo da disciplina para identificar características do ambiente no qual os programas estão sendo executados.

---

# 2. Atualizando o Sistema

Antes de instalar novas ferramentas, é recomendável atualizar a lista de pacotes disponíveis.

Execute:

```bash
sudo apt update
```

Em seguida:

```bash
sudo apt upgrade
```

O comando `sudo` permite executar uma operação com privilégios administrativos.

Durante a primeira utilização de `sudo`, será solicitada a senha do usuário.

---

# 3. Instalando Ferramentas Básicas

Algumas ferramentas serão utilizadas durante praticamente toda a disciplina.

Podemos instalá-las utilizando:

```bash
sudo apt install build-essential git curl wget vim
```

O pacote `build-essential` instala ferramentas importantes para desenvolvimento em C/C++, incluindo:

```text
gcc
g++
make
```

Podemos verificar a instalação utilizando:

```bash
gcc --version
```

```bash
make --version
```

```bash
git --version
```

---

# 4. Identificando o Endereço IP da Máquina Virtual

Para acessar a máquina virtual remotamente precisamos conhecer seu endereço IP.

Execute:

```bash
ip addr
```

Ou, de forma mais simples:

```bash
hostname -I
```

Exemplo:

```text
192.168.56.101
```

Esse endereço dependerá da configuração de rede utilizada pela solução de virtualização.

Também podemos utilizar:

```bash
ip a
```

Procure uma interface como:

```text
enp0s3
```

ou:

```text
ens33
```

Exemplo:

```text
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.56.101/24
```

Nesse caso, o endereço IP da máquina virtual é:

```text
192.168.56.101
```

> O computador hospedeiro precisa conseguir alcançar esse endereço para que a conexão SSH funcione.

---

# 5. Testando a Comunicação com a Máquina Virtual

No computador hospedeiro, abra um terminal e execute:

```bash
ping 192.168.56.101
```

Substitua o endereço pelo IP da sua máquina virtual.

Uma resposta típica será:

```text
PING 192.168.56.101
64 bytes from 192.168.56.101: icmp_seq=1 ttl=64 time=0.432 ms
64 bytes from 192.168.56.101: icmp_seq=2 ttl=64 time=0.391 ms
```

Interrompa o comando utilizando:

```text
Ctrl + C
```

Caso não exista comunicação, será necessário verificar a configuração de rede da máquina virtual.

---

# 6. Instalação do Servidor SSH

O protocolo **SSH — Secure Shell** permite acessar remotamente outra máquina por meio de uma conexão segura.

No Ubuntu, instale o servidor SSH:

```bash
sudo apt install openssh-server
```

Depois da instalação, verifique o estado do serviço:

```bash
sudo systemctl status ssh
```

Uma saída esperada deverá indicar:

```text
Active: active (running)
```

Caso o serviço não esteja executando, podemos iniciá-lo:

```bash
sudo systemctl start ssh
```

Para configurá-lo para iniciar automaticamente com o sistema:

```bash
sudo systemctl enable ssh
```

Podemos verificar novamente:

```bash
systemctl status ssh
```

---

# 7. Primeiro Acesso SSH

No computador hospedeiro, execute:

```bash
ssh usuario@IP_DA_VM
```

Por exemplo:

```bash
ssh aluno@192.168.56.101
```

Na primeira conexão poderá aparecer uma mensagem semelhante a:

```text
The authenticity of host '192.168.56.101' can't be established.

Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Digite:

```text
yes
```

Em seguida será solicitada a senha do usuário da máquina virtual.

Depois da autenticação, teremos acesso ao terminal remoto.

Podemos verificar em qual máquina estamos executando comandos com:

```bash
hostname
```

Também podemos verificar o usuário:

```bash
whoami
```

Exemplo:

```text
aluno
```

Para encerrar a conexão SSH:

```bash
exit
```

ou:

```text
Ctrl + D
```

---

# 8. Chaves SSH

Embora seja possível utilizar senha para acessar a máquina, uma alternativa mais prática e segura é utilizar **criptografia de chave pública**.

O mecanismo utiliza duas chaves:

```text
Chave privada
    │
    │ permanece no computador do usuário
    │
    ▼

Chave pública
    │
    │ instalada no servidor
    ▼

Máquina Virtual
```

A chave privada **não deve ser compartilhada**.

---

# 9. Criando um Par de Chaves SSH

No computador hospedeiro, execute:

```bash
ssh-keygen
```

Uma opção recomendada atualmente é utilizar o algoritmo Ed25519:

```bash
ssh-keygen -t rsa -b 4096
```

O comando solicitará o local onde a chave será armazenada:

```text
Enter file in which to save the key (/home/YOU/.ssh/id_ALGORITHM):[Press enter]
```

Edite conforme sugestão abaixo e pressione `Enter`.

```text
/home/SEUUSURARIO/.ssh/VMs_rsa
```

Normalmente as chaves são armazenadas em:

```text
~/.ssh/
```

Serão criados dois arquivos:

```text
VMs_rsa
VMs_rsa.pub
```

Onde o primeiro é a **chave privada**, enquanto o segundo é a **chave pública**.

Podemos verificar:

```bash
ls -la ~/.ssh
```

Inicie o ssh-agent em segundo plano.

```bash
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

Adicione sua chave SSH privada ao ssh-agent.

```bash
ssh-add ~/.ssh/VMs_rsa
```

---

# 10. Visualizando a Chave Pública

Para visualizar a chave pública:

```bash
cat ~/.ssh/VMs_rsa.pub
```

A chave pública pode ser compartilhada e instalada em servidores, mas a chave privada **NÃO DEVE SER COMPARTILHADA**.

---

# 11. Copiando a Chave Pública para a Máquina Virtual

Em Linux, macOS ou alguns ambientes Windows que possuam o utilitário disponível, podemos utilizar:

```bash
ssh-copy-id usuario@IP_DA_VM
```

Por exemplo:

```bash
ssh-copy-id aluno@192.168.56.101
```

Será solicitada a senha do usuário uma última vez.

Depois disso, tente acessar novamente:

```bash
ssh aluno@192.168.56.101
```

Caso a configuração tenha funcionado corretamente, o acesso deverá ocorrer sem solicitar a senha do usuário remoto.

---

# 12. Como a Autenticação por Chave Funciona

Na máquina virtual, as chaves públicas autorizadas ficam armazenadas em:

```bash
~/.ssh/authorized_keys
```

Podemos visualizar:

```bash
cat ~/.ssh/authorized_keys
```

Quando uma conexão é realizada, o cliente demonstra possuir a chave privada correspondente a uma das chaves públicas presentes nesse arquivo.

Assim, a chave privada nunca precisa ser enviada para o servidor.

---

# 13. Criando um Alias para a Conexão SSH

Para evitar digitar constantemente:

```bash
ssh aluno@192.168.56.101
```

podemos criar uma configuração local.

No computador hospedeiro, edite:

```bash
~/.ssh/config
```

Adicione:

```text
Host sovm
    HostName 192.168.56.101
    User aluno
    IdentityFile ~/.ssh/VMs_rsa
```

Agora será possível conectar utilizando apenas:

```bash
ssh sovm
```

Essa configuração também será utilizada posteriormente pelo Visual Studio Code.

---

# 14. Instalando o Zsh

Por padrão, muitas distribuições Linux utilizam o **Bash** como shell.

Podemos verificar o shell atual utilizando:

```bash
echo $SHELL
```

Exemplo:

```text
/bin/bash
```

Durante as aulas podemos utilizar o **Zsh**, que oferece diversas funcionalidades adicionais.

Instale:

```bash
sudo apt install zsh
```

Verifique:

```bash
zsh --version
```

Para alterar o shell padrão:

```bash
chsh -s $(which zsh)
```

Será necessário realizar logout e login novamente.

Depois, verifique:

```bash
echo $SHELL
```

A saída deverá ser semelhante a:

```text
/usr/bin/zsh
```

---

# 15. Instalando o Oh My Zsh

O **Oh My Zsh** é um framework para configuração do Zsh que permite utilizar temas, plugins e diferentes melhorias de produtividade no terminal.

Primeiro certifique-se de que `curl` está instalado:

```bash
sudo apt install curl
```

Depois execute o instalador oficial:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Ao final da instalação, abra um novo terminal.

O arquivo principal de configuração será:

```bash
~/.zshrc
```

Podemos abri-lo com:

```bash
nano ~/.zshrc
```

ou:

```bash
vim ~/.zshrc
```

---

# 16. Configurando o Oh My Zsh

Dentro do arquivo:

```text
~/.zshrc
```

é possível encontrar a configuração de tema:

```text
ZSH_THEME="robbyrussell"
```

Geralmente troco pelo tema `gianu`.

```bash
sed -i 's/^ZSH_THEME="robbyrussell"$/# &\nZSH_THEME="gianu"/' ~/.zshrc
cat ~/.zshrc | grep ZSH_THEME
source ~/.zshrc
```

Também podemos habilitar plugins.

Por exemplo:

```text
plugins=(git)
```

Uma configuração simples pode utilizar:

```text
plugins=(
    git
    sudo
)
```

Depois de modificar o arquivo, recarregue as configurações:

```bash
source ~/.zshrc
```

---

# 17. Histórico de Comandos

Uma funcionalidade importante do shell é o histórico.

Execute alguns comandos:

```bash
pwd
```

```bash
ls
```

```bash
uname -a
```

Depois:

```bash
history
```

O terminal mostrará os comandos executados anteriormente.

Também podemos utilizar:

```text
↑
```

e:

```text
↓
```

para navegar pelo histórico.

Outra alternativa muito útil é:

```text
Ctrl + R
```

para pesquisar comandos anteriores.

---

# 18. Terminal no Windows

Caso o computador hospedeiro utilize **Windows**, existem várias alternativas para executar comandos SSH e ferramentas semelhantes às disponíveis em sistemas Unix.

Entre elas:

* Windows Terminal;
* PowerShell;
* Git Bash / MINGW64;
* Cygwin;
* WSL.

Para as atividades da disciplina, uma opção simples é utilizar **Git Bash**, que disponibiliza um terminal baseado em MINGW64.

---

# 19. Git Bash e MINGW64

Ao instalar o **Git for Windows**, também é disponibilizado o **Git Bash**.

Ao abrir o terminal, normalmente será exibido algo semelhante a:

```text
usuario@notebook MINGW64 ~
$
```

Nesse ambiente podemos executar:

```bash
ssh
```

```bash
ssh-keygen
```

```bash
git
```

Por exemplo:

```bash
ssh aluno@192.168.56.101
```

Também podemos verificar as chaves:

```bash
ls ~/.ssh
```

---

# 20. Alternativa: Cygwin

Outra opção para usuários Windows é o **Cygwin**, que fornece diversas ferramentas típicas de ambientes Unix.

Depois da instalação, podemos utilizar comandos como:

```bash
ssh
```

```bash
scp
```

```bash
ls
```

```bash
grep
```

```bash
cat
```

No entanto, para esta disciplina não é obrigatório utilizar Cygwin. O Git Bash/MINGW64 ou o terminal SSH nativo do Windows geralmente são suficientes.

---

# 21. Testando o SSH no Windows

Abra o terminal utilizado no Windows e execute:

```bash
ssh -V
```

Uma saída típica será:

```text
OpenSSH_for_Windows_9.x
```

Depois teste:

```bash
ssh usuario@IP_DA_VM
```

Por exemplo:

```bash
ssh aluno@192.168.56.101
```

Se o login remoto funcionar, o computador já está preparado para acessar a VM.

---

# 22. Instalando o Visual Studio Code

Utilizaremos o **Visual Studio Code (VS Code)** como editor principal durante as atividades práticas.

O VS Code será instalado no **computador hospedeiro**, e não necessariamente dentro da máquina virtual.

Depois da instalação, abra o programa.

---

# 23. Extensões Recomendadas

Abra o painel de extensões:

```text
Ctrl + Shift + X
```

Procure e instale:

```text
Remote - SSH
```

Também são recomendadas:

```text
C/C++
```

e:

```text
C/C++ Extension Pack
```

A extensão de SSH permitirá que o VS Code abra diretórios e arquivos que estão dentro da máquina virtual.

---

# 24. Conectando o VS Code à Máquina Virtual

Abra a paleta de comandos:

```text
Ctrl + Shift + P
```

Procure por:

```text
Remote-SSH: Connect to Host...
```

Caso tenha sido criada anteriormente a configuração:

```text
Host sovm
    HostName 192.168.56.101
    User aluno
```

será exibida a opção:

```text
sovm
```

Selecione-a.

Uma nova janela do VS Code será aberta.

No canto inferior da janela deverá aparecer algo semelhante a:

```text
SSH: sovm
```

Isso indica que o VS Code está conectado à máquina virtual.

---

# 25. Abrindo uma Pasta Remota

Na janela conectada via SSH, selecione:

```text
File → Open Folder
```

Podemos criar um diretório específico para a disciplina.

No terminal da VM:

```bash
mkdir -p ~/sistemas-operacionais
```

Depois abra:

```text
/home/aluno/sistemas-operacionais
```

pelo VS Code.

---

# 26. Utilizando o Terminal Integrado do VS Code

Abra o terminal integrado:

```text
Terminal → New Terminal
```

ou utilize:

```text
Ctrl + `
```

Execute:

```bash
hostname
```

Depois:

```bash
whoami
```

e:

```bash
pwd
```

Exemplo:

```text
aluno
/home/aluno/sistemas-operacionais
```

Embora o VS Code esteja visualmente executando no computador hospedeiro, o terminal está sendo executado **dentro da máquina virtual**.

---

# 27. Primeiro Arquivo pelo VS Code

Crie um arquivo chamado:

```text
teste.txt
```

Adicione:

```text
Sistemas Operacionais
```

Salve o arquivo.

No terminal integrado execute:

```bash
ls
```

Deverá aparecer:

```text
teste.txt
```

Visualize o conteúdo:

```bash
cat teste.txt
```

Resultado:

```text
Sistemas Operacionais
```

Esse pequeno teste demonstra que o VS Code está manipulando diretamente os arquivos armazenados na VM.

---

# 28. Criando a Estrutura da Disciplina

Podemos criar uma estrutura inicial para os experimentos.

Execute:

```bash
mkdir -p ~/sistemas-operacionais/aulas
```

Entre no diretório:

```bash
cd ~/sistemas-operacionais
```

Verifique:

```bash
pwd
```

Resultado esperado:

```text
/home/aluno/sistemas-operacionais
```

Crie alguns diretórios:

```bash
mkdir 01-compilacao
mkdir 02-processos
mkdir 03-threads
```

Verifique:

```bash
ls
```

Exemplo:

```text
01-compilacao
02-processos
03-threads
aulas
```

---

# 29. Teste Final do Ambiente

Para verificar se o ambiente está preparado, execute os comandos abaixo.

### Sistema operacional

```bash
uname -a
```

### Usuário

```bash
whoami
```

### Diretório atual

```bash
pwd
```

### Compilador C

```bash
gcc --version
```

### Make

```bash
make --version
```

### Git

```bash
git --version
```

### Shell

```bash
echo $SHELL
```

### SSH

```bash
ssh -V
```

Se todos os comandos funcionarem, o ambiente básico está pronto.

---

# 30. Exercício Prático

Realize as atividades abaixo utilizando **apenas o terminal** sempre que possível.

1. Identifique o endereço IP da máquina virtual.
2. Verifique a versão do kernel Linux.
3. Descubra qual usuário está conectado.
4. Descubra qual é o diretório home desse usuário.
5. Acesse a máquina virtual via SSH.
6. Configure autenticação utilizando chave pública/privada.
7. Configure um alias chamado `sovm` no arquivo `~/.ssh/config`.
8. Teste o acesso utilizando:

```bash
ssh sovm
```

9. Instale e configure o Zsh.
10. Instale o Oh My Zsh.
11. Conecte o VS Code à máquina virtual utilizando **Remote - SSH**.
12. Crie pelo VS Code o arquivo:

```text
~/sistemas-operacionais/ambiente.txt
```

13. Insira nesse arquivo:

```text
Disciplina: Sistemas Operacionais
Ambiente: Ubuntu Linux
Acesso: SSH
```

14. No terminal, visualize o arquivo utilizando:

```bash
cat ~/sistemas-operacionais/ambiente.txt
```

---

# 31. Desafio

Sem utilizar a interface gráfica da máquina virtual, descubra as seguintes informações utilizando comandos Linux:

```text
Nome da máquina:
Usuário atual:
Endereço IP:
Versão do Ubuntu:
Versão do kernel:
Shell atual:
Quantidade de CPUs:
Quantidade de memória RAM:
Espaço disponível em disco:
```

Alguns comandos que podem ajudar:

```bash
hostname
```

```bash
whoami
```

```bash
hostname -I
```

```bash
uname
```

```bash
lscpu
```

```bash
free
```

```bash
df
```

Procure identificar quais opções de cada comando apresentam as informações de maneira mais legível.

---

# 32. Resultado Esperado

Ao final desta aula, o ambiente deverá permitir o seguinte fluxo de trabalho:

```text
┌───────────────────────────┐
│ Computador do aluno       │
│                           │
│ VS Code                   │
│ Git Bash / Terminal       │
└─────────────┬─────────────┘
              │
              │ SSH
              ▼
┌───────────────────────────┐
│ Máquina Virtual Ubuntu    │
│                           │
│ ~/sistemas-operacionais   │
│                           │
│ GCC                       │
│ Make                      │
│ Git                       │
│ Zsh + Oh My Zsh           │
└───────────────────────────┘
```

Em seguida, os códigos escritos no VS Code serão armazenados e executados diretamente nesse ambiente Linux.

---

# Próximo Assunto

➡️ [02a — Compilando um Código-Fonte em C](../02a-compilacao-c/README.md)

Na próxima aula serão abordados:

* estrutura básica de um programa em C;
* `Hello, World!`;
* processo de compilação;
* utilização do GCC;
* arquivos executáveis;
* argumentos de linha de comando;
* `argc` e `argv`;
* criação de um `Makefile` simples.

---
