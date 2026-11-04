# Relatório Técnico de Teste de Intrusão
## Laboratório Metasploitable 2 e DVWA

**Classificação:** Confidencial – uso restrito ao laboratório  
**Versão:** 1.0 – consolidada a partir das evidências fornecidas  
**Data de elaboração:** 03/11/2026  
**Responsável técnico:** [Preencher nome do analista]  
**Organização/cliente:** [Preencher, se aplicável]

---

## 1. Sumário executivo
Foi realizada avaliação controlada do ativo `192.168.0.15`, identificado pelas evidências como ambiente Metasploitable, incluindo serviços de infraestrutura e a aplicação web DVWA. O reconhecimento identificou 23 portas TCP abertas. Foram comprovadas autenticações com a conta `msfadmin` nos serviços SSH, FTP e SMB. No SSH, houve login interativo ao sistema operacional; no FTP, houve listagem de diretório; no SMB, houve enumeração de compartilhamentos e retorno da ferramenta indicando acesso permitido ao recurso `ADMIN$`.

Os resultados demonstram risco relevante associado a credenciais fracas/reutilizadas, exposição de serviços legados e superfície de ataque extensa. A avaliação do DVWA permaneceu inconclusiva: foi fornecido um comando de teste automatizado e credenciais conhecidas, mas não foi apresentada saída que comprove sucesso do teste ou exploração de vulnerabilidades da aplicação.

## 2. Objetivo
Avaliar a exposição de serviços de rede e mecanismos de autenticação do laboratório, registrar evidências reproduzíveis, identificar riscos e recomendar medidas de correção.

## 3. Escopo
- **Alvo principal:** `192.168.0.15`.
- **Aplicação web:** `http://192.168.0.15/dvwa/`.
- **Endpoint de login:** `http://192.168.0.15/dvwa/login.php`.
- **Rede de descoberta informada:** `192.168.0.0/24` (comando apresentado como `192.168.0.15/24`).
- **Ativos excluídos:** demais hosts descobertos na rede, salvo autorização expressa.

## 4. Metodologia
1. Descoberta de hosts com Nmap (`-sn`).
2. Identificação de portas e versões com Nmap (`-sV`).
3. Verificação de disponibilidade do SSH com Netcat.
4. Preparação de wordlists direcionadas.
5. Auditoria de credenciais com Medusa nos módulos SSH, FTP e SMBNT.
6. Validação manual de acesso via clientes SSH e FTP e listagem SMB.
7. Identificação do endpoint de autenticação DVWA e preparação de teste HTTP.

A metodologia foi documentada a partir dos comandos e resultados transcritos pelo operador. Não foram recebidos arquivos nativos de saída, capturas de tela ou logs integrais para todas as etapas.

## 5. Resultados de reconhecimento
### 5.1 Descoberta de hosts
O comando `nmap -sn 192.168.0.15/24` identificou no trecho fornecido os endereços `192.168.0.1`, `.2`, `.3`, `.15`, `.16`, `.47` e `.51`. A saída foi truncada após `.51`, portanto a lista não é considerada exaustiva.

### 5.2 Portas e serviços
O comando `nmap -sV 192.168.0.15` reportou 23 portas TCP abertas e 977 fechadas. Serviços relevantes incluem FTP (21), SSH (22), Telnet (23), HTTP (80), SMB (139/445), bindshell identificado pelo Nmap (1524), NFS (2049), FTP adicional (2121), MySQL (3306), PostgreSQL (5432), VNC (5900), IRC (6667), AJP (8009) e Tomcat (8180).

## 6. Resultados de auditoria de credenciais
### 6.1 SSH
Medusa reportou sucesso para a conta `msfadmin`. Em seguida, o comando SSH estabeleceu sessão e apresentou o prompt `msfadmin@metasploitable:~$`. Isso comprova acesso interativo ao sistema sob essa conta. O material não comprova privilégios root.

### 6.2 FTP
Medusa reportou sucesso para `msfadmin`. A autenticação manual retornou `230 Login successful`, e o comando `ls` listou o diretório `vulnerable`. Não foi demonstrada escrita ou acesso a todos os arquivos.

### 6.3 SMB
Medusa reportou sucesso para `msfadmin` com a mensagem `ADMIN$ - Access Allowed`. O `smbclient -L` listou os compartilhamentos `print$`, `tmp`, `opt`, `IPC$`, `ADMIN$` e `msfadmin`. Não foi demonstrada execução administrativa ou escrita nos compartilhamentos.

## 7. Aplicação web DVWA
Foi informado o endpoint `/dvwa/login.php`, as credenciais `admin/password` e a URL pós-login `/dvwa/`. Também foi apresentado comando Medusa com módulo HTTP. Como a saída do comando e uma evidência de sessão autenticada não foram fornecidas, não se declara sucesso de brute force nem vulnerabilidade específica no DVWA.

## 8. Achados e severidade preliminar
| ID | Achado | Severidade preliminar | Situação |
|---|---|---|---|
| F-01 | Credencial fraca/reutilizada em SSH | Alta | Confirmado |
| F-02 | Credencial fraca/reutilizada em FTP | Alta | Confirmado |
| F-03 | Credencial fraca em SMB e acesso reportado a ADMIN$ | Alta | Confirmado, limitado ao retorno da ferramenta |
| F-04 | Superfície de ataque extensa e serviços legados | Alta (agregada) | Exposição confirmada; impacto individual requer validação |
| F-05 | Serviços com versões antigas | Média/Alta | Versões reportadas; CVEs não validadas |
| F-06 | Teste de autenticação DVWA sem saída | Não classificada | Inconclusivo |
| F-07 | Enumeração enum4linux sem saída | Não classificada | Evidência ausente |

As severidades são avaliações preliminares qualitativas. Não foi calculada pontuação CVSS porque faltam elementos de contexto e validação suficientes.

## 9. Recomendações
- Remover credenciais padrão e reutilizadas; estabelecer senhas únicas e robustas.
- Restringir SSH, SMB e FTP a redes administrativas e ativos autorizados.
- Substituir FTP e Telnet por protocolos criptografados; desativar serviços não necessários.
- Revisar ACLs, permissões de compartilhamentos SMB e diretórios FTP.
- Atualizar sistema operacional, Samba, Apache, bancos de dados e demais serviços para versões suportadas.
- Aplicar firewall com política de negação por padrão e segmentação de rede.
- Implementar monitoramento de autenticações, limitação de tentativas e alertas para padrões de brute force.
- Completar a avaliação do DVWA com evidências de requisições/respostas, nível de segurança e módulos efetivamente testados.
- Realizar reteste após remediação.

## 10. Conclusão
As evidências confirmam fragilidade de credenciais em três serviços de rede e autenticação efetiva no SSH e FTP, além de enumeração SMB com acesso reportado a `ADMIN$`. A quantidade e diversidade de serviços expostos aumentam a superfície de ataque. Recomenda-se priorizar a substituição das credenciais, restrição de acesso aos serviços e atualização do ambiente. A aplicação DVWA e algumas hipóteses associadas a versões específicas permanecem pendentes de validação.

## 11. Limitações e ressalvas
Este documento não substitui laudo baseado em coleta independente. Foi elaborado somente a partir das informações e transcrições fornecidas. Não foram disponibilizados: capturas de tela originais, arquivos de saída completos do Nmap/enum4linux/Medusa, versão e nível de segurança do DVWA, evidência do resultado HTTP, autorização formal assinada, janela de teste, nem reteste de correção. Os demais hosts da rede não integram o escopo de exploração. Segredos devem ser mascarados em cópias de circulação externa.

## 12. Anexos e rastreabilidade
- REC-001: Nmap – serviços e versões.
- REC-002: Nmap – descoberta de hosts.
- CRED-001: criação de wordlists.
- CRED-002: auditoria e login SSH.
- CRED-003: auditoria de credenciais FTP.
- CRED-004: login manual FTP e listagem.
- SMB-001: comando enum4linux (saída não recebida).
- SMB-002: Medusa SMBNT.
- SMB-003: listagem de compartilhamentos.
- WEB-001: comando Medusa HTTP (saída não recebida).
- WEB-002/003: credenciais e URL DVWA informadas.

---
**Fim do relatório.**
