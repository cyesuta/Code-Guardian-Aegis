# Explicação das Diretrizes de Segurança VibeCoding: Por Que Insistimos Tanto?

Este documento explica a importância de cada regra nas "Diretrizes de Segurança". Entender isso o ajudará a ver seu código da "perspectiva de um hacker" e prevenir problemas antes que ocorram.

---

## ⭐ Regras de Ouro

### 【Nunca hardcodar segredos】 & 【Usar variáveis de ambiente】 & 【Ignorar arquivos secretos】

**Por que é importante?**

Essas três regras são as leis supremas de sobrevivência do mundo digital. Seu código (especialmente ao usar Git) será copiado, compartilhado e armazenado em múltiplos lugares. Escrever senhas, chaves API e outros "segredos" diretamente no código é como tatuar a senha do cofre na sua testa - qualquer um que te veja (veja o código) pode facilmente abrir seu cofre.

**Cenário do Hacker 😈**
> Eu adoro procurar por `password`, `api_key` ou `db_connect` no GitHub. Encontrei seu projeto público e em um arquivo chamado `config.js` vi este código: `const db_password = 'Password123!';`. Perfeito! Nem preciso atacar seu site - agora posso tentar me conectar diretamente ao seu banco de dados com essa senha.

**Consequências Catastróficas 💥**

**Controle completo do serviço.** Hackers podem roubar, alterar, excluir todos os dados dos seus usuários, ou usar seus serviços API pagos para atividades ilegais, com todas as contas em seu nome.

---

## 📥 Manipulação de Entrada do Usuário

### 【Prevenir Ataques de Injeção SQL】

**Por que é importante?**

Imagine o banco de dados como um robô que só entende a linguagem SQL. Se você concatenar diretamente a entrada do usuário com seus comandos, os usuários têm a oportunidade de pronunciar seus próprios "comandos". Consultas parametrizadas dizem ao robô: "Escute, a próxima parte são **apenas dados** - não importa o conteúdo, não execute como comandos."

**Cenário do Hacker 😈**
> No campo de login do seu site, inseri `' OR '1'='1' --` como nome de usuário. Assumo que sua consulta SQL está escrita assim: `"SELECT * FROM users WHERE username = '" + userInput + "';`. Minha entrada a transformou em `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` é sempre verdadeiro, então contornei a verificação de senha e fiz login com sucesso na primeira conta de usuário (geralmente o administrador).

**Consequências Catastróficas 💥**

Atacantes podem contornar o login, roubar todos os dados do banco (listas de usuários, hashes de senhas), ou até mesmo deletar o banco de dados.

---

## 🔐 Permissões e Autenticação

### 【Proteção de Endpoints API】

**Por que é importante?**

Interfaces frontend (UI) podem esconder botões, mas hackers nunca confiam em interfaces. Eles chamam suas APIs backend diretamente com ferramentas (como Postman, curl). Você deve assumir que cada endpoint API será atacado diretamente, então cada endpoint deve ser um guarda independente, verificando por si só a identidade e permissões dos visitantes.

**Cenário do Hacker 😈**
> Descobri que para modificar informações pessoais, o frontend envia uma requisição para `/api/user/update`. Embora eu não possa ver os botões de edição de outras pessoas, assumo que esta API distingue usuários através de `userId`. Tentei enviar uma requisição para `/api/user/update?userId=1` (ID do administrador) com os dados que queria modificar. Meu Deus, o servidor não verificou se a requisição veio de mim pessoalmente e mudou com sucesso o email do administrador!

**Consequências Catastróficas 💥**

Usuários comuns podem alterar dados de outros usuários ou até administradores, ou executar permissões que não deveriam ter, causando caos sistêmico.