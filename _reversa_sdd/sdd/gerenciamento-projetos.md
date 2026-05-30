# Spec: Gerenciamento de Projetos

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Gerenciamento de projetos — criar, listar, renomear, arquivar e excluir projetos. Cada projeto contém os mockups HTML e seus feedbacks associados.

---

## 2. Contexto e Motivação

🟡 **Problema:** Usuários precisam organizar múltiplos mockups em projetos separados. Sem gerenciamento, não há como distinguir entre diferentes designs ou versões.

🟡 **Por que agora:** O projeto é o contêiner principal de todo o conteúdo do Almanac. Essencial desde o primeiro uso.

---

## 3. Goals

- [ ] G-01: Usuário cria um projeto com nome e descrição opcional
- [ ] G-02: Usuário vê lista de todos os seus projetos
- [ ] G-03: Usuário arquiva/exclui projetos

**Métricas:**
| Métrica | Target |
|---|---|
| Criar projeto em < 3 cliques | Sim |
| Lista de projetos carrega em < 1s | Sim |

---

## 4. Non-Goals

- NG-01: 🟡 Projetos compartilhados com permissões (apenas dono gerencia)
- NG-02: 🟡 Pastas ou sub-organização dentro da lista de projetos
- NG-03: 🟡 Templates de projeto

---

## 5. Usuários e Personas

🟡 **Primário:** Mary — precisa organizar seus mockups em projetos separados por cliente ou sprint.

---

## 6. Requisitos Funcionais

### 6.1 Requisitos Principais

| ID | Requisito | Prioridade | Critério |
|---|---|---|---|
| RF-01 | 🟡 Usuário deve poder criar projeto com nome | Must | Formulário salva no Supabase |
| RF-02 | 🟡 Sistema deve listar projetos do usuário logado | Must | GET /projects retorna lista |
| RF-03 | 🟡 Usuário deve poder renomear projeto | Should | Inline edit no título |
| RF-04 | 🟡 Usuário deve poder arquivar projeto | Should | Projeto some da lista principal |
| RF-05 | 🟡 Usuário deve poder excluir projeto com confirmação | Must | Confirmação "Tem certeza?" antes de deletar |

### 6.2 Fluxo Principal

1. 🟡 Usuário clica "Novo Projeto" no dashboard
2. 🟡 Sistema exibe modal com campo "Nome do Projeto"
3. 🟡 Usuário digita nome e confirma
4. 🟡 Sistema cria projeto no Supabase e redireciona para tela do projeto
5. 🟡 Projeto aparece na lista do dashboard

### 6.3 Fluxos Alternativos

**Exclusão:**
1. 🟡 Usuário clica "Excluir" no card do projeto
2. 🟡 Sistema exibe confirmação: "Tem certeza? Todos os dados serão perdidos."
3. 🟡 Usuário confirma → sistema deleta projeto + pins + comentários

---

## 7. Requisitos Não-Funcionais

| ID | Requisito | Valor |
|---|---|---|
| RNF-01 | 🟡 Lista de projetos carrega em < 1s | < 1s |
| RNF-02 | 🟡 Dados do projeto são privados por usuário | RLS no Supabase |

---

## 8. Modelo de Dados

```
projects {
  id: uuid PK
  user_id: uuid FK -> users.id
  name: string
  status: enum (active, archived)
  created_at: timestamp
  updated_at: timestamp
}
```

---

## 9. Integrações

| Dependência | Tipo | Impacto |
|---|---|---|
| Supabase DB | Obrigatória | CRUD de projetos para |

---

## 10. Edge Cases

| Cenário | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Nome vazio | Usuário tenta criar sem nome | Botão desabilitado enquanto vazio |
| EC-02: 🟡 Projeto deletado com pins ativos | Usuário exclui projeto | Cascade delete: projeto + pins + comentários |
| EC-03: 🟡 2 projetos com mesmo nome | Usuário cria duplicata | Permitido (usei timestamp para diferenciar) |

---

## 11. Segurança

- 🟡 RLS no Supabase: usuário só vê seus próprios projetos
- 🟡 Exclusão lógica via campo `status` + exclusão física confirmada

---
