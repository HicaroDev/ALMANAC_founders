# Spec: Dashboard e Status

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Dashboard inicial com listagem de projetos, contadores de atividade, status (ativo/arquivado) e metricas rapidas.

---

## 2. Contexto e Motivação

🟡 **Problema:** Sem dashboard, o usuario nao tem visao geral dos seus projetos nem do progresso das revisoes.

🟡 **Por que agora:** Primeira tela apos login — essencial para navegacao.

---

## 3. Goals

- [ ] G-01: Dashboard carrega com lista de projetos
- [ ] G-02: Cards mostram status, contagem de pins, ultima atividade
- [ ] G-03: Filtros: ativos / arquivados / todos

---

## 4. Non-Goals

- NG-01: 🟡 Dashboard com graficos estatisticos avancados
- NG-02: 🟡 Widgets customizaveis

---

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade |
|---|---|---|
| RF-01 | 🟡 Dashboard lista todos os projetos do usuario | Must |
| RF-02 | 🟡 Cada card mostra nome, status, data, qtd pins | Must |
| RF-03 | 🟡 Usuario pode filtrar por status (ativos/arquivados) | Should |
| RF-04 | 🟡 Card mostra preview do mockup (thumbnail) | Should |
| RF-05 | 🟡 Dashboard mostra contador total de projetos e pins | Should |

### Fluxo Principal

1. 🟡 Usuario faz login → redirecionado ao dashboard
2. 🟡 Sistema carrega lista de projetos do Supabase
3. 🟡 Cards exibem nome, status (ativo/arquivado), data, contagem
4. 🟡 Usuario clica em card → abre tela do projeto
5. 🟡 Usuario filtra por status usando abas ou dropdown

### Modelo de Dados

Nenhum novo — reusa `projects` com campo `status`.

---

## 6. Edge Cases

| Cenario | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Usuario sem projetos | Primeiro login | Estado vazio com CTA "Criar primeiro projeto" |
| EC-02: 🟡 100+ projetos | Muitos projetos | Paginacao ou scroll infinito |
| EC-03: 🟡 Thumbnail nao carrega | Falha no storage | Placeholder generico com nome do projeto |

---
