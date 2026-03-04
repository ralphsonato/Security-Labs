# Write-up: Skynet - TryHackMe

> **Data:** 03 de Março de 2026  
> **Target IP:** `10.67.176.22`

---

O desafio **Skynet** é um laboratório focado em reconhecimento e exploração de falhas em serviços comuns como SMB, POP3 e aplicações web.
## 1. Reconhecimento
Iniciei com um scan para identificar serviços e versões:
```bash
nmap -sV -sC -A 10.67.176.22
```
Vetores Identificados:
- Porta 80 (HTTP): Apache 2.4.18.
- Portas 139/445 (SMB): Samba 4.3.11.
- Porta 110 (POP3): Dovecot POP3d.

### 1.1 Enumeração SMB
Constatando a existência de compartilhamentos acessíveis sem autenticação:
```bash
smbclient -L //10.67.176.22 -N
```
    Sharename       Type      Comment
    ---------       ----      -------
    print$          Disk      Printer Drivers
    anonymous       Disk      Skynet Anonymous Share
    milesdyson      Disk      Miles Dyson Personal Share
    IPC$            IPC       IPC Service (skynet server (Samba, Ubuntu))
No share anonymous, encontrei o arquivo log1.txt, que continha uma wordlist de senhas, e o attention.txt, que confirmou o nome do usuário alvo: milesdyson.

### 1.2 Webmail e Credenciais

Utilizei o diretório `/squirrelmail` (identificado via Gobuster) e a wordlist `log1.txt` para comprometer a conta de e-mail.
```
gobuster dir -u [http://10.67.176.22](http://10.67.176.22) -w /usr/share/wordlists/dirb/common.txt
```
**Credenciais de E-mail:** `milesdyson : cyborg007haloterminator`

Dentro da caixa de entrada, recuperei a senha do Samba em um e-mail de "Password Reset" e a localização de um diretório oculto.

