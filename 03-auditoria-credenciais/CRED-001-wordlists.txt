# Wordlists e auditoria de credenciais

## Preparação – CRED-001
Foram criados os arquivos:

**Passw.txt**
```text
admin
root
gopher
password
passwd
123456
msfadmin
```

**Usuars.txt**
```text
administrador
administrator
passwd
passw0rd
msfadmin
@test12
admin
```

O operador informou que as listas foram direcionadas ao laboratório, considerando o contexto do sistema operacional e dos serviços avaliados.

**Observação de qualidade:** os nomes dos arquivos e seu conteúdo devem ser revisados antes de reutilização. `Usuars.txt` contém entradas que parecem senhas (`passwd`, `passw0rd`), enquanto `Passw.txt` contém termos que podem ser nomes de usuário (`root`, `admin`). Em execuções posteriores foram citados `user.txt`, `users.txt` e `spray_senhas.txt`; não foi fornecido o conteúdo integral desses arquivos, portanto não se presume que sejam idênticos às listas acima.

## Resultados de autenticação
- SSH: Medusa reportou `msfadmin` / `msfadmin` como sucesso; em seguida houve login interativo SSH.
- FTP: Medusa reportou `msfadmin` / `msfadmin` como sucesso; login manual e listagem FTP confirmados.
- SMB: Medusa reportou `msfadmin` / `msfadmin` como sucesso, com `ADMIN$ - Access Allowed`; listagem de compartilhamentos confirmada.

**Tratamento de credenciais:** a senha foi incluída no anexo técnico restrito para rastreabilidade do laboratório. Em versões destinadas à circulação externa, recomenda-se mascarar a senha e distribuir segredos por canal separado.
