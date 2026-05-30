# Spec: Presenca ao Vivo

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Indicador de presenca em tempo real — mostra quem esta visualizando o mesmo projeto no momento, com avatares e feed de atividades recentes.

---

## 2. Contexto e Motivação

🟡 **Problema:** Em equipes colaborativas, saber quem mais esta vendo o mesmo mockup evita retrabalho e coordena revisoes.

🟡 **Por que agora:** Diferencial competitivo citado nas regras do hackathon.

---

## 3. Goals

- [ ] G-01: Usuarios ativos aparecem como avatares no header
- [ ] G-02: Feed de atividades mostra acoes recentes (criou pin, resolveu, etc)

---

## 4. Non-Goals

- NG-01: 🟡 Cursor position ao vivo (apenas presenca)
- NG-02: 🟡 Chat ao vivo
- NG-03: 🟡 Video/audio chamada

---

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade |
|---|---|---|
| RF-01 | 🟡 Sistema detecta quando usuario entra na pagina do projeto | Must |
| RF-02 | 🟡 Sistema exibe avatares dos usuarios ativos no header | Must |
| RF-03 | 🟡 Sistema remove avatar quando usuario sai | Must |
| RF-04 | 🟡 Sistema mantem feed de atividades (ultimas 20 acoes) | Should |
| RF-05 | 🟡 Atualizacao em tempo real via Supabase Realtime | Must |

### Fluxo Principal

1. 🟡 Usuario abre pagina do projeto
2. 🟡 Sistema registra presenca via Supabase Realtime
3. 🟡 Avatares dos usuarios ativos aparecem no canto superior
4. 🟡 Acoes (criou pin, resolveu, comentou) aparecem no feed lateral
5. 🟡 Ao sair, avatar desaparece em < 5s

### Modelo de Dados

```
presence {
  project_id: uuid
  user_id: uuid
  last_seen: timestamp
}

activity_feed {
  id: uuid PK
  project_id: uuid FK
  user_id: uuid FK
  action: string (pin_created, resolved, commented, version_created)
  target: string
  created_at: timestamp
}
```

---

## 6. Edge Cases

| Cenario | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Usuario abre 2 abas do mesmo projeto | Multiplas abas | Contar como 1 presenca |
| EC-02: 🟡 Conexao Realtime cai | Internet instavel | Tentar reconectar, remover apos 10s sem heartbeat |
| EC-03: 🟡 50 usuarios no mesmo projeto | Muitos avatares | Mostrar avatares + "+N" overflow |

---
