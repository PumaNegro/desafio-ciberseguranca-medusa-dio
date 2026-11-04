# Descoberta de hosts ativos

**ID:** REC-002  
**Ferramenta:** Nmap 7.991  
**Comando:** `nmap -sn 192.168.0.15/24`  
**Objetivo:** identificar hosts ativos na rede informada.

## Resultado recebido
| IP | MAC | Fabricante/observação |
|---|---|---|
| 192.168.0.1 | 38:91:48:4D:17:15 | ZTE |
| 192.168.0.2 | 8A:6D:61:DC:E2:91 | Desconhecido |
| 192.168.0.3 | 22:5F:46:25:A4:A6 | Desconhecido |
| 192.168.0.15 | 08:00:27:4A:C7:99 | Oracle VirtualBox virtual NIC |
| 192.168.0.16 | 96:31:B6:7D:89:C6 | Desconhecido |
| 192.168.0.47 | 1E:2A:11:1F:6F:0F | Desconhecido |
| 192.168.0.51 | Não apresentado | Saída truncada após o endereço |

**Limitação:** a transcrição termina após `192.168.0.51`; não é possível afirmar que este seja o último host encontrado. O comando `-sn` realiza descoberta de hosts, não varredura de portas.

**Escopo:** o alvo explicitamente investigado nas evidências subsequentes é `192.168.0.15`. Os demais endereços não devem ser testados sem autorização específica.
