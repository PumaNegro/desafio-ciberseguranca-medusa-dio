# Análise consolidada de vulnerabilidades

## Escopo e premissas
- Alvo técnico principal: `192.168.0.15` (identificado como Metasploitable).
- Aplicação web: DVWA no mesmo endereço.
- Ambiente descrito pelo operador como laboratório controlado.
- A autorização formal, janela de teste e limites de impacto não foram anexados; devem ser preenchidos antes de circulação corporativa.

## Achados

### F-01 – Credenciais fracas/reutilizadas em SSH
- **Ativo:** 192.168.0.15:22/TCP
- **Evidência:** Medusa reportou `ACCOUNT FOUND` para `msfadmin`; login interativo SSH concluído e prompt `msfadmin@metasploitable`.
- **Impacto demonstrado:** acesso autenticado ao sistema operacional sob a conta `msfadmin`.
- **Severidade preliminar:** Alta.
- **CVSS:** não atribuído nesta versão; requer confirmação de privilégios efetivos, exposição, requisitos de ataque e impacto no contexto do ativo.
- **Recomendação:** substituir credenciais padrão/fracas, aplicar política de senha robusta, restringir SSH por rede/ACL/VPN, usar autenticação por chave, limitar tentativas e monitorar autenticações.

### F-02 – Credenciais fracas/reutilizadas em FTP
- **Ativo:** 192.168.0.15:21/TCP (vsftpd 2.3.4)
- **Evidência:** Medusa reportou sucesso para `msfadmin`; cliente FTP retornou `230 Login successful` e listou `vulnerable`.
- **Impacto demonstrado:** acesso FTP autenticado e listagem de diretório.
- **Severidade preliminar:** Alta se o serviço estiver acessível a usuários não confiáveis; contextualizar exposição e dados acessíveis.
- **CVSS:** não atribuído.
- **Recomendação:** desativar FTP em texto claro ou substituí-lo por SFTP/FTPS adequadamente configurado; remover contas padrão; aplicar menor privilégio; restringir rede e auditar permissões.

### F-03 – Credenciais fracas e acesso reportado ao ADMIN$ via SMB
- **Ativo:** 192.168.0.15:139,445/TCP
- **Evidência:** Medusa reportou `SUCCESS (ADMIN$ - Access Allowed)` para `msfadmin`; `smbclient -L` listou `ADMIN$`, `IPC$`, `tmp`, `opt`, `print$` e `msfadmin`.
- **Impacto demonstrado:** autenticação SMB e enumeração de compartilhamentos; ferramenta reportou acesso permitido a ADMIN$.
- **Limite:** não foi demonstrada leitura/escrita ou execução administrativa no compartilhamento.
- **Severidade preliminar:** Alta.
- **CVSS:** não atribuído.
- **Recomendação:** trocar credenciais padrão, restringir SMB a redes administrativas, revisar ACLs e compartilhamentos, desabilitar SMBv1 quando possível, atualizar Samba e monitorar logons/acesso a shares.

### F-04 – Superfície de ataque ampla e serviços legados expostos
- **Ativo:** 192.168.0.15
- **Evidência:** Nmap identificou 23 portas TCP abertas, incluindo Telnet, FTP, r-services, SMB, bancos de dados, VNC, X11, Java RMI, NFS, IRC, AJP e Tomcat.
- **Impacto:** ampliação da superfície de ataque e exposição de protocolos/serviços potencialmente inseguros.
- **Severidade preliminar:** Alta como risco agregado de configuração; individualizar após validação.
- **Recomendação:** desativar serviços desnecessários, segmentar a rede, restringir portas por firewall, atualizar sistemas e serviços, exigir criptografia e autenticação forte.

### F-05 – Serviços e versões obsoletos
- **Evidência:** Nmap reportou, entre outros, OpenSSH 4.7p1, Apache 2.2.8, MySQL 5.0.51a, PostgreSQL 8.3.x, BIND 9.4.2 e vsftpd 2.3.4.
- **Impacto:** versões legadas podem conter vulnerabilidades conhecidas e não receber correções.
- **Severidade preliminar:** Média/Alta, dependente da confirmação de versão real, patch level e exposição.
- **Recomendação:** inventariar pacotes, aplicar atualizações suportadas ou migrar para versões mantidas; validar CVEs por versão exata e configuração.

### F-06 – Teste de autenticação DVWA sem resultado comprobatório
- **Ativo:** HTTP/80, `/dvwa/login.php`
- **Evidência:** comando Medusa informado; credenciais `admin/password` e URL pós-login informadas.
- **Impacto:** não determinado.
- **Severidade:** Não classificada – evidência insuficiente.
- **Recomendação:** coletar saída da ferramenta e evidência de sessão autenticada; verificar CSRF, cookies e critérios de sucesso/falha. Não relatar brute force bem-sucedido sem confirmação.

### F-07 – Enumeração SMB com saída ausente
- **Ativo:** SMB/NetBIOS
- **Evidência:** comando `enum4linux -a` informado, sem conteúdo do arquivo de saída.
- **Impacto:** não determinado para essa execução; compartilhamentos foram listados separadamente com smbclient.
- **Severidade:** Não classificada – saída ausente.
- **Recomendação:** anexar `enum_Saida.txt` original, caso disponível.

## Observações sobre hipóteses não confirmadas
- A versão `vsftpd 2.3.4` merece validação específica devido a histórico de backdoor conhecido, mas não foi apresentada prova de exploração.
- A identificação de `bindshell` na porta 1524 é uma detecção do Nmap; não foi apresentado teste de conexão ou validação do nível de privilégio.
- Não foram fornecidos resultados de exploração de vulnerabilidades web do DVWA, enumeração detalhada de NFS, bancos de dados, Tomcat/AJP, IRC ou demais serviços.

## Priorização de remediação
1. Trocar/remover credenciais padrão e reutilizadas em SSH, FTP e SMB.
2. Restringir imediatamente a exposição de SMB e FTP; eliminar protocolos em texto claro quando não necessários.
3. Desativar serviços não utilizados, especialmente Telnet e serviços remotos legados.
4. Atualizar sistema operacional e serviços para versões suportadas.
5. Validar permissões dos compartilhamentos SMB e diretórios FTP.
6. Completar evidências faltantes do DVWA e do enum4linux.
7. Reexecutar varredura após correções e documentar reteste.

## Limitações gerais
Este relatório é baseado exclusivamente nas transcrições fornecidas pelo operador. Não houve acesso independente ao laboratório, verificação dos arquivos brutos, captura de pacotes ou validação externa das versões. Horários, endereços e saídas foram reproduzidos conforme fornecidos. Severidades são preliminares e não equivalem a pontuação CVSS oficial.
