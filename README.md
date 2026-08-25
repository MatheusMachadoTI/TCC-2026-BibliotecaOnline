# Biblioteca Online Gamificada

## **Projeto em desenvolvimento **

Um sistema de biblioteca online com gamificação que permite aos usuários ler livros, ouvir audiobooks, deixar avaliações e ganhar XP.

## Funcionalidades

### Para Usuários
- 📚 Leitura de livros digitais
- 🎧 Audiobooks
- ⭐ Sistema de avaliações e comentários
- 🎯 Gamificação com XP e níveis
- 📊 Acompanhamento de progresso
- 👤 Perfil personalizado com foto
- 🔥 Sistema de streak diário

### Para Administradores
- 👥 Gerenciamento de usuários
- 📖 Adição de novos livros/audiobooks
- 📈 Estatísticas do sistema
- 👑 Controle administrativo

## Tecnologias Utilizadas

- **Backend**: PHP 7.4+
- **Banco de Dados**: MySQL
- **Frontend**: HTML5, CSS3, JavaScript
- **Framework CSS**: Tailwind CSS
- **Ícones**: Font Awesome
- **Animações**: Canvas Confetti

## Instalação

1. **Pré-requisitos**
   - XAMPP ou similar com PHP 7.4+ e MySQL
   - Navegador web moderno

2. **Configuração**
   ```bash
   # Clone ou baixe os arquivos para htdocs
   cd /caminho/para/xampp/htdocs
   # Importe o banco.sql no phpMyAdmin
   ```

3. **Acesso**
   - URL: `http://localhost/Teixeira/sistema`
   - Login admin: `admin@biblioteca.com` (senha: 123)

## Estrutura do Banco de Dados

### Tabelas Principais
- `usuarios` - Dados dos usuários
- `acervo` - Livros e audiobooks
- `progresso` - Progresso de leitura
- `avaliacoes` - Avaliações e comentários

## Funcionalidades Técnicas

### Sistema de Gamificação
- Ganha 50 XP por livro concluído
- Níveis baseados em XP acumulado
- Streak diário para engajamento

### Segurança
- Senhas criptografadas com password_hash()
- Prepared statements para prevenir SQL Injection
- Validação de entrada de dados
- Sessões seguras

### Interface Responsiva
- Design moderno com Tailwind CSS
- Layout adaptável para mobile e desktop
- Animações suaves e interativas

## Desenvolvimento

### Arquivos Principais
- `dashboard.php` - Página principal
- `login.php` - Autenticação
- `profile.php` - Perfil do usuário
- `admin.php` - Painel administrativo
- `biblioteca.php` - Biblioteca completa
- `comentarios.php` - Página de comentários

### Estrutura de Pastas
```
/
├── css/           # Estilos CSS
├── includes/      # Funções PHP e modais
├── img/           # Imagens
├── uploads/       # Arquivos enviados
└── banco.sql      # Script do banco de dados
```

## Contribuição

Para contribuir com o projeto:
1. Faça fork do repositório
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## Setup e Operações (demo/TCC)

Este projeto foi preparado para demonstração em ambiente local (XAMPP). Abaixo estão passos práticos para setup, export do banco, proteção de arquivos e backup de uploads.

### Exportar banco (atualizar arquivo SQL)
- Use o script `export_db.ps1` para gerar `biblioteca_online.sql` a partir do banco atual:
```powershell
cd C:\xampp\htdocs\sistema
powershell -NoProfile -ExecutionPolicy Bypass -File .\export_db.ps1
```

### Backup dos uploads
- Use `backup_uploads.ps1` para criar um ZIP com as pastas `img/` e `files/`:
```powershell
cd C:\xampp\htdocs\sistema
powershell -NoProfile -ExecutionPolicy Bypass -File .\backup_uploads.ps1
```
- Parâmetros: `-OutputDir`, `-ProjectDir`, `-RetentionDays`.

### Servir arquivos com proteção
- Implementado `serve_file.php` para servir arquivos somente a usuários autenticados. Ele aceita `?item_id=ID` e retorna o arquivo do registro `acervo` correspondente.

### Integração com S3 (opcional)

- O projeto inclui uma abstração de armazenamento em `includes/storage.php` que usa o sistema de arquivos local por padrão e pode enviar arquivos para S3 quando a AWS SDK estiver instalada e as variáveis de ambiente estiverem configuradas.
- Para ativar S3:
  1. Instale dependências com Composer (se `composer` estiver disponível):
     ```powershell
     cd C:\xampp\htdocs\sistema
   composer install
     ```
  2. Copie `.env.example` para `.env` e configure as variáveis:
     - `USE_S3=1`
     - `S3_BUCKET` — nome do bucket
     - `AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`
  3. `includes/storage.php` usará S3 automaticamente quando `USE_S3=1` e a SDK estiver disponível.

- Observação: em modo S3, o `serve_file.php` pode ser adaptado para gerar URLs pré-assinadas ou fazer streaming via `GetObject`. Posso aplicar essa alteração se quiser que arquivos sejam servidos diretamente do S3 de forma privada.


### PHP.ini (limites de upload)
- Para uploads grandes, editei `C:\xampp\php\php.ini` (backup em `php.ini.bak`) e aumentei `upload_max_filesize` e `post_max_size` para `200M`. Reinicie o Apache após alterações.

### Recomendações para apresentação
- Faça um backup completo das pastas `img/` e `files/` antes da demo.
- Inclua `biblioteca_online.sql` atualizado no repositório ou zip de entrega.
- Considere usar `serve_file.php` para proteger conteúdo durante a apresentação.

## Alterações recentes e instruções rápidas

- **Senha admin:** : admin foi resetado para `123` para uso em ambiente local (arquivo SQL atualizado).
- **Autoload Composer:** `includes/functions.php` agora carrega `vendor/autoload.php` quando presente (habilita AWS SDK).
- **Armazenamento:** `includes/storage.php` implementa abstração local/S3. Por padrão usa o sistema de arquivos (`img/`, `files/`).
- **Servir arquivos com proteção:** `serve_file.php` centraliza autorização e streaming/presigned URLs para arquivos registrados em `acervo`.
- **Uploads/admin:** `admin.php` aceita `capa` (salva em `img/`) e `arquivo` (salva em `files/` ou S3). Exclusão de livro remove arquivos associados.
- **Scripts de automação (PowerShell):**
   - `export_db.ps1` — exporta o banco para `biblioteca_online.sql`.
   - `backup_uploads.ps1` — cria ZIP de `img/` + `files/` em `backups/` com retenção.
   - `schedule_tasks.ps1` — cria tarefas agendadas no Windows (opcional).
   - `test_upload.ps1` — gera PDF grande e testa o fluxo de upload (usado para validar `post_max_size`).
- **Composer & AWS SDK:** Composer foi utilizado para instalar dependências (AWS SDK). Se `vendor/autoload.php` existir, a integração S3 está disponível.
- **php.ini:** `upload_max_filesize` e `post_max_size` foram aumentados para `200M` em `C:\xampp\php\php.ini` (backup criado). Reinicie o Apache após editar.

### Comandos úteis

- **Reiniciar Apache (PowerShell elevado):**
```powershell
Get-Process -Name httpd -ErrorAction SilentlyContinue | Stop-Process -Force -ErrorAction SilentlyContinue
& 'C:\xampp\apache\bin\httpd.exe' -k restart
netstat -ano | findstr /R ":80 |:443 "
```

- **Executar export do banco:**
```powershell
cd C:\xampp\htdocs\sistema
powershell -NoProfile -ExecutionPolicy Bypass -File .\export_db.ps1
```

- **Criar backup dos uploads:**
```powershell
cd C:\xampp\htdocs\sistema
powershell -NoProfile -ExecutionPolicy Bypass -File .\backup_uploads.ps1
```

- **Testar upload grande (automático):**
```powershell
cd C:\xampp\htdocs\sistema
powershell -NoProfile -ExecutionPolicy Bypass -File .\test_upload.ps1
```

### Como ativar S3 (opcional)

1. Instale dependências com Composer (se ainda não):
```powershell
cd C:\xampp\htdocs\sistema
composer install
```
2. Copie `.env.example` → `.env` e defina:
    - `USE_S3=1`
    - `S3_BUCKET`, `AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`
3. Faça upload via `admin.php`; registros em `acervo.arquivo_url` usarão `s3://{key}` e `serve_file.php` gerará presigned URL ou fará streaming conforme configuração.

### Commit & entrega

- Para salvar mudanças localmente, instale o Git e rode o `commit_changes.ps1` ou faça manualmente:
```powershell
git init
git add .
git commit -m "Atualizações: uploads, storage abstraction, backups, S3-ready"
```

Nota: scripts auxiliares foram movidos para a pasta `tools/`. Use os caminhos abaixo quando necessário:

- Export DB: `tools\export_db.ps1`
- Backup uploads: `tools\backup_uploads.ps1`
- Test upload: `tools\test_upload.ps1`
- Composer installer: `tools\install_composer.ps1` (cria `tools\composer.phar`)
- Commit helper: `tools\commit_changes.ps1`

Se quiser, eu faço o commit automático (preciso que o Git esteja instalado no sistema). 


