# Nmap – identificação de serviços

**ID:** REC-001  
**Alvo:** `192.168.0.15`  
**Comando:** `nmap -sV 192.168.0.15`  
**Objetivo:** identificar portas TCP abertas, serviços e versões anunciadas.

## Resultado recebido

```text
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec?
513/tcp  open  login       OpenBSD or Solaris rlogind
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
```

**Resumo:** 23 portas TCP abertas; 977 portas TCP fechadas (conforme saída fornecida). MAC `08:00:27:4A:C7:99`, identificado como interface virtual Oracle VirtualBox. Hosts reportados: `metasploitable.localdomain` e `irc.Metasploitable.LAN`.

## Interpretação
A superfície de ataque observada é ampla e inclui serviços legados, protocolos sem criptografia e serviços de administração/banco de dados. Versões antigas são indicadores para investigação, não prova isolada de vulnerabilidade explorável.

**Limitações:** não foi fornecida a linha completa de execução (por exemplo, privilégios, opções adicionais ou arquivo XML), nem evidência de varredura UDP. A identificação de versão depende das respostas dos serviços e pode não refletir todos os componentes instalados.
