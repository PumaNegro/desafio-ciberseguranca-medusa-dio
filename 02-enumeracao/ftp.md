# Enumeração e validação FTP

## Contexto
Nmap identificou `vsftpd 2.3.4` em TCP/21 e `ProFTPD 1.3.1` em TCP/2121.

## Credenciais – CRED-003
**Comando:** `medusa -h 192.168.0.15 -U user.txt -P spray_senhas.txt -M ftp -t 10`

**Resultado recebido:**
```text
ACCOUNT FOUND: [ftp] Host: 192.168.0.15 User: msfadmin Password: msfadmin [SUCCESS]
```

## Validação manual – CRED-004
**Comando:** `ftp 192.168.0.15`

**Resultado recebido:**
```text
220 (vsFTPd 2.3.4)
331 Please specify the password.
230 Login successful.
Remote system type is UNIX.
ftp> ls
drwxr-xr-x 6 1000 1000 4096 Apr 28 2010 vulnerable
226 Directory send OK.
```

## Conclusão
Foi comprovada autenticação FTP com a conta `msfadmin` e listagem do diretório `vulnerable`. Não há evidência fornecida de escrita, download, upload ou execução de comandos via FTP.

**Nota:** a versão vsftpd 2.3.4 é associada publicamente a um backdoor histórico, mas nenhuma exploração desse backdoor foi demonstrada neste conjunto de evidências. O achado deve permanecer como hipótese de validação, não como comprometimento confirmado.
