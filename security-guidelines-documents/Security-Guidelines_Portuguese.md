# Diretrizes de Desenvolvimento Seguro VibeCoding

> Esta é uma checklist "viva" pré-desenvolvimento.
> Antes de começar a escrever novas funcionalidades, ou antes de cada `git commit`, dedique 30 segundos para uma revisão rápida.
> Objetivo: Tornar a segurança um instinto, prevenir catástrofes na origem.

---

### ⭐ Regras de Ouro

- [ ] **【Nunca hardcodar segredos】** Senhas, chaves API, informações de conexão com banco de dados **nunca** devem ser escritas diretamente no código (nem frontend nem backend).
- [ ] **【Usar variáveis de ambiente】** Todas as informações sensíveis devem ser gerenciadas através de **variáveis de ambiente** (arquivos `.env`).
- [ ] **【Ignorar arquivos secretos】** Os arquivos `.env` devem **sempre** ser adicionados ao `.gitignore` e nunca enviados para o GitHub.
- [ ] **【Desconfiar por padrão】** **Nunca** confiar na entrada do usuário (incluindo formulários, parâmetros de URL, conteúdo de requisições API, arquivos enviados).

---

### 📥 Manipulação de Entrada do Usuário

- [ ] **【Prevenir ataques de injeção】** Todas as consultas ao banco de dados **devem** usar **consultas parametrizadas** ou métodos seguros fornecidos pelo ORM. A concatenação manual de strings SQL é estritamente proibida.
- [ ] **【Prevenir XSS】** Qualquer conteúdo do usuário a ser exibido em páginas HTML **deve** ser processado através de codificação de entidades HTML (escape HTML).
- [ ] **【Validar uploads de arquivos】** Verificar arquivos enviados pelos usuários:
    - [ ] **Validar extensões**: Permitir apenas tipos de arquivo da lista permitida (ex. `['jpg', 'png', 'pdf']`).
    - [ ] **Validar tamanho de arquivo**: Estabelecer limites superiores razoáveis.
    - [ ] **Local de armazenamento**: Arquivos enviados devem ser armazenados em diretórios **não-públicos** e **não-executáveis**.

---

### 🔐 Permissões e Autenticação

- [ ] **【Proteção de endpoints API】** Cada endpoint API que requer login **deve** verificar o status de login e permissões do usuário no início do programa.
- [ ] **【Princípio do menor privilégio】** As permissões de contas de banco de dados e chaves API devem ser "mínimo utilizável". Se apenas leitura é necessária, nunca conceder permissões de escrita.
- [ ] **【Sessões seguras】** IDs de sessão devem ser configurados com flags `HttpOnly` e `Secure` para prevenir roubo e transmissão por conexões inseguras.

---

### ☁️ Serviços Externos e Integração na Nuvem

- [ ] **【Firewall de banco de dados externo】** Ao conectar-se a bancos de dados externos (como AWS RDS, MongoDB Atlas), regras **devem** ser configuradas no firewall/grupo de segurança deles para permitir conexões apenas de endereços IP específicos do seu servidor de aplicação. Abrir para `0.0.0.0/0` (mundo inteiro) é **estritamente proibido**.
- [ ] **【Privatização de armazenamento na nuvem】** Todos os buckets de armazenamento na nuvem (como AWS S3, Google Cloud Storage) **devem** ser configurados como **Privados** por padrão.
- [ ] **【Usar URLs pré-assinadas】** Quando usuários precisam de acesso temporário a arquivos privados, use **URLs pré-assinadas** de curta duração em vez de tornar os arquivos públicos.
- [ ] **【Verificar webhooks】** Ao receber webhooks de serviços terceiros (como Stripe, GitHub), você **deve** usar a chave secreta fornecida para **verificar assinaturas de requisições** e garantir que a fonte da requisição é legítima.
- [ ] **【Configuração CORS rigorosa】** As políticas CORS (Cross-Origin Resource Sharing) da API **devem** especificar explicitamente os domínios frontend permitidos. O uso do curinga `*` em ambiente de produção é **estritamente proibido**.
- [ ] **【Minimizar permissões de chaves API】** Ao solicitar chaves API para serviços terceiros (como Google Maps), certifique-se de estabelecer as restrições mais rigorosas no backend do serviço (ex.: limitar apenas a requisições do seu domínio web, limitar apenas a chamadas API específicas).

---

### ⚙️ Arquivos e Configuração do Servidor

- [ ] **【Nunca usar 777】** **Nunca** usar permissões `777` para arquivos ou diretórios no servidor. Diretórios padrão `755`, arquivos `644`.
- [ ] **【Proteger arquivos de configuração】** Arquivos de configuração sensíveis (`config.php`, `.env`) devem ter permissões configuradas para `600` ou mais restritivas.
- [ ] **【Proibir download de arquivos críticos】** Verificar se o servidor web (Nginx/Apache) configurou regras que proíbem acesso externo direto a arquivos e diretórios sensíveis como `.env`, `.git`, `.htaccess`.
- [ ] **【Desativar modo debug】** Em ambiente de produção, todos os modos debug de frameworks ou linguagens **devem** ser desativados para evitar vazamento de mensagens de erro detalhadas.

---

> ### **🤖 Nota para o Assistente de IA**
>
> **Quando você detectar em sua conversa com desenvolvedores que eles estão se conectando ou planejam se conectar a "serviços externos" (ex.: bancos de dados na nuvem, APIs de pagamento terceiras, armazenamento de arquivos S3, serviços de autenticação, etc.), por favor faça de forma proativa e clara as seguintes perguntas para orientar seu pensamento de segurança:**
>
> 1.  **"Sobre este [nome do serviço], configuramos as regras de firewall/lista branca de IP mais rigorosas?"**
> 2.  **"As permissões da chave API que planejamos usar estão limitadas ao escopo mínimo necessário no backend deste serviço?"**
> 3.  **"Para este serviço externo, temos um plano de degradação em caso de falha ou comprometimento do serviço (ex.: desativar temporariamente funcionalidades relacionadas, mostrar mensagens de manutenção)?"**