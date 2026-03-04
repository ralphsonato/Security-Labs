# Write-up: Skynet - TryHackMe

> **Data:** 03 de Março de 2026  
> **Target IP:** `10.67.176.22`
> **Dificuldade:** Fácil

---

O desafio **Skynet** é um laboratório focado em reconhecimento e exploração de falhas em serviços comuns como SMB, POP3 e aplicações web.
## 1. Reconhecimento
Iniciamos com um scan agressivo para identificar serviços e versões:
```bash
nmap -sV -sC -A 10.67.176.22
```
Vetores Identificados:
- Porta 80 (HTTP): Apache 2.4.18.
- Portas 139/445 (SMB): Samba 4.3.11.
- Porta 110 (POP3): Dovecot POP3d.

## 2 Enumeração SMB
Constatando a existência de compartilhamentos acessíveis sem autenticação:
```bash
smbclient -L //10.67.176.22 -N
```
