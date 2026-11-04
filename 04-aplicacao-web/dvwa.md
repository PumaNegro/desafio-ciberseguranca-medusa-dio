# Avaliação da aplicação web DVWA

**Alvo informado:** `http://192.168.0.15/dvwa/`  
**Endpoint de autenticação:** `http://192.168.0.15/dvwa/login.php`  
**Credenciais fornecidas pelo operador:** usuário `admin`; senha `password`.

## Teste automatizado – WEB-001
Comando informado:
```bash
medusa -h 192.168.0.15 -U user.txt -P spray_senhas.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'/username=^USER&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 10
```

**Status:** foi fornecido o comando, mas não foi fornecida a saída do Medusa. Não é possível concluir que houve descoberta automatizada de credenciais.

## Credenciais e URL pós-login – WEB-002 / WEB-003
O operador informou `admin` / `password` como credenciais para acesso ao DVWA e indicou `http://192.168.0.15/dvwa/` como URL após login.

**Limitação:** não foram apresentadas captura de tela, resposta HTTP, cookie de sessão ou saída que confirme a autenticação bem-sucedida. As credenciais são tratadas como informadas, não como validadas por evidência independente.

## Próximas validações recomendadas
1. Registrar versão do DVWA e nível de segurança selecionado.
2. Anexar captura da tela autenticada, removendo cookies, tokens e dados sensíveis.
3. Documentar cada módulo testado (por exemplo, SQL Injection, XSS, CSRF, File Upload) com requisição, resposta, impacto e evidência.
4. Para testes automatizados de login, validar método HTTP, parâmetros, token CSRF, cookies e critério de falha/sucesso antes de interpretar a saída da ferramenta.

Nenhuma vulnerabilidade específica de lógica ou de entrada do DVWA foi comprovada pelo material recebido.
