# Spec: Autenticação

**Versão:** 1.0
**Status:** Rascunho
**Autor:** reversa-spec-sdd
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Componente de autenticação via Google OAuth usando Supabase Auth. Permite que usuários façam login com conta Google, gerenciem sessão e façam logout.

---

## 2. Contexto e Motivação

🟡 **Problema:** A regra de ouro do hackathon exige autenticação com Google. Sem login, não há identificação do usuário, nem projetos, nem feedbacks atribuíveis.

🟡 **Evidências:** Regra obrigatória do desafio — "O sistema deve ter Autenticação com o Google para login."

🟡 **Por que agora:** O login é a porta de entrada para todas as funcionalidades do Almanac. Sem ele, nenhuma outra feature funciona.

---

## 3. Goals

- [ ] G-01: Usuário consegue fazer login com conta Google em até 2 cliques
- [ ] G-02: Sessão persiste entre recarregamentos de página
- [ ] G-03: Usuário consegue fazer logout e limpar sessão

**Métricas de sucesso:**
| Métrica | Baseline atual | Target | Prazo |
|---|---|---|---|
| Login funcional com Google | N/A | 100% funcional | 30/05/2026 |
| Sessão persiste após refresh | N/A | Sim | 30/05/2026 |

---

## 4. Non-Goals

- NG-01: 🟡 Suporte a outros provedores (GitHub, email/senha) — apenas Google OAuth
- NG-02: 🟡 Cadastro separado (signup) — fluxo integrado ao login do Google
- NG-03: 🟡 Recuperação de senha (inexistente para OAuth)
- NG-04: 🟡 RBAC ou permissões granulares — apenas autenticação básica

---

## 5. Usuários e Personas

🟡 **Usuário primário:** Mary (Designer de Produto) — precisa acessar o Almanac com sua conta Google

🟡 **Jornada atual (sem a feature):** Mary não consegue acessar o sistema. Não há projetos, uploads ou feedbacks sem identificação.

🟡 **Jornada futura (com a feature):** Mary clica em "Entrar com Google", autoriza, e é redirecionada ao dashboard.

---

## 6. Requisitos Funcionais

### 6.1 Requisitos Principais

| ID | Requisito | Prioridade | Critério de Aceite |
|---|---|---|---|
| RF-01 | 🟡 O sistema deve exibir botão "Entrar com Google" na tela inicial | Must | Botão visível e clicável sem necessidade de recarregar |
| RF-02 | 🟡 O sistema deve iniciar fluxo OAuth 2.0 ao clicar no botão | Must | Redireciona para tela de escolha de conta Google |
| RF-03 | 🟡 O sistema deve criar usuário no banco Supabase após primeira autenticação | Must | Usuário inserido na tabela `users` do Supabase |
| RF-04 | 🟡 O sistema deve manter sessão ativa via token JWT do Supabase Auth | Must | Recarregar a página não redireciona para login |
| RF-05 | 🟡 O sistema deve exibir nome e foto do perfil Google no header | Should | Avatar e nome do usuário visíveis no canto superior |
| RF-06 | 🟡 O sistema deve permitir logout com 1 clique | Must | Botão de logout limpa sessão e redireciona para tela de login |
| RF-07 | 🟡 O sistema deve redirecionar para tela de login se token expirou | Must | Request 401 → redirect automático para /login |

### 6.2 Fluxo Principal

1. 🟡 Usuário acessa o Almanac e vê tela de boas-vindas com botão "Entrar com Google"
2. 🟡 Usuário clica no botão → redirecionado para o consentimento Google
3. 🟡 Usuário autoriza → Supabase Auth cria/retorna sessão JWT
4. 🟡 Sistema redireciona para dashboard com usuário autenticado
5. 🟡 Nome e avatar do Google aparecem no header

### 6.3 Fluxos Alternativos

**Fluxo A — Login cancelado:**
1. 🟡 Usuário clica em "Entrar com Google"
2. 🟡 Usuário desiste na tela de consentimento Google
3. 🟡 Sistema redireciona de volta ao Almanac sem autenticação
4. 🟡 Nenhum erro é exibido — apenas retorna ao estado inicial

---

## 7. Requisitos Não-Funcionais

| ID | Requisito | Valor alvo | Observação |
|---|---|---|---|
| RNF-01 | 🟡 Redirecionamento OAuth deve ocorrer em < 2s | < 2s | Dependente do Google/ Supabase |
| RNF-02 | 🟡 Sessão JWT deve expirar conforme configurado no Supabase | Conforme config | Padrão Supabase: 1 hora |
| RNF-03 | 🟡 Token JWT deve ser armazenado com segurança (httpOnly cookie ou localStorage) | Seguro | Usar Supabase Client recomendado |

---

## 8. Design e Interface

🟡 **Componentes afetados:** Header (avatar + nome), LoginPage, DashboardPage

🟡 **Comportamento esperado:**
- Tela inicial: logotipo + nome "Almanac" + botão "Entrar com Google"
- Header: avatar circular (32px) + nome + dropdown de logout

🟡 **Estados da UI:**
- Estado vazio: Tela de login com botão Google
- Estado de carregamento: Spinner no botão durante redirect
- Estado de erro: Mensagem "Falha na autenticação. Tente novamente."
- Estado de sucesso: Dashboard com usuário logado

---

## 9. Modelo de Dados

```
users {
  id: uuid (PK, do Supabase Auth)
  email: string
  name: string
  avatar_url: string
  created_at: timestamp
}
```

**Migrações necessárias:** Sim — tabela `users` sincronizada com Supabase Auth (trigger on user created)

---

## 10. Integrações e Dependências

| Dependência | Tipo | Impacto se indisponível |
|---|---|---|
| Supabase Auth | Obrigatória | Login completamente indisponível |
| Google OAuth API | Obrigatória | Login completamente indisponível |

---

## 11. Edge Cases e Tratamento de Erros

| Cenário | Trigger | Comportamento esperado |
|---|---|---|
| EC-01: 🟡 Usuário revoga acesso Google | Revoga permissão no Google | Na próxima tentativa de login, pede re-autorização |
| EC-02: 🟡 Token JWT expirado durante uso | Request 401 | Redirect automático para /login, sessão limpa |
| EC-03: 🟡 Supabase Auth fora do ar | Falha na inicialização | Exibir "Serviço temporariamente indisponível. Tente novamente." |
| EC-04: 🟡 Conta Google sem e-mail verificado | Google retorna e-mail não verificado | Bloquear acesso com mensagem "Conta precisa de e-mail verificado" |

---

## 12. Segurança e Privacidade

- 🟡 **Autenticação:** Google OAuth 2.0 via Supabase Auth
- 🟡 **Autorização:** Qualquer usuário com conta Google pode acessar
- 🟡 **Dados sensíveis:** Nome, e-mail e avatar do Google — armazenados no Supabase
- 🟡 **Auditoria:** Log de login bem-sucedido e falhas

---

## 13. Plano de Rollout

- 🟡 **Estratégia:** Big bang (login é feature crítica, sem flag)
- 🟡 **Como reverter:** Desabilitar provedor Google no painel Supabase Auth
- 🟡 **Monitoramento pós-deploy:** Verificar logs de autenticação no Supabase

---

## 14. Open Questions

Nenhuma.

---

## 15. Decisões Tomadas

| Decisão | Alternativas consideradas | Racional |
|---|---|---|
| Supabase Auth | NextAuth.js, Clerk | Hackathon exige Supabase |
| Google OAuth como único provedor | Múltiplos providers | Regra do hackathon |

---

## Apêndice

### Referências
- PRD: `_reversa_sdd/prd.md`
- Supabase Auth Docs

### Histórico de Revisões
| Versão | Data | Autor | Mudanças |
|---|---|---|---|
| 1.0 | 2026-05-30 | reversa-spec-sdd | Criação inicial |
