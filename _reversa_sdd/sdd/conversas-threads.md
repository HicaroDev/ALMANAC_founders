# Spec: Conversas e Threads

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Sistema de threads nos pins — usuarios podem responder comentarios, reagir com emoji, editar, deletar e marcar como resolvido/reaberto.

---

## 2. Contexto e Motivação

🟡 **Problema:** Feedback sem thread gera comentarios soltos sem contexto de conversa. Resolver/reabrir permite rastrear o status de cada ponto.

🟡 **Por que agora:** Diferencial competitivo citado nas regras do hackathon.

---

## 3. Goals

- [ ] G-01: Usuario responde comentario e cria thread
- [ ] G-02: Usuario reage com emoji a comentario
- [ ] G-03: Autor marca pin como resolvido ou reaberto
- [ ] G-04: Usuario edita/deleta seu proprio comentario

---

## 4. Non-Goals

- NG-01: 🟡 Mencionar usuarios com @
- NG-02: 🟡 Notificacoes push/email
- NG-03: 🟡 Anexos em respostas

---

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade |
|---|---|---|
| RF-01 | 🟡 Usuario pode responder comentario em thread | Must |
| RF-02 | 🟡 Respostas aparecem aninhadas abaixo do pin | Must |
| RF-03 | 🟡 Usuario pode reagir com emoji (like, thumbs) | Should |
| RF-04 | 🟡 Autor do pin pode marcar como resolvido | Must |
| RF-05 | 🟡 Qualquer membro pode reabrir pin resolvido | Should |
| RF-06 | 🟡 Usuario pode editar seu proprio comentario | Should |
| RF-07 | 🟡 Usuario pode deletar seu proprio comentario | Must |

### Fluxo Principal

1. 🟡 Usuario clica em pin para abrir card de comentario
2. 🟡 Sistema exibe comentario principal + respostas existentes
3. 🟡 Usuario digita resposta e confirma
4. 🟡 Resposta aparece aninhada na thread
5. 🟡 Autor clica "Resolver" → pin muda para verde/check
6. 🟡 Outro usuario clica "Reabrir" → pin volta ao estado aberto

### Modelo de Dados

```
pin_reactions {
  id: uuid PK
  pin_id: uuid FK
  user_id: uuid FK
  emoji: string
}
```

---

## 6. Edge Cases

| Cenario | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Editar comentario antigo | Usuario edita apos 1h | Permitido, mostra indicador "editado" |
| EC-02: 🟡 Resolver pin com respostas nao lidas | Marcar resolvido com replies abertas | Permitido, replies ficam visiveis no historico |
| EC-03: 🟡 Deletar comentario raiz | Usuario deleta comentario principal | Thread inteira e mantida, mostra "[deletado]" |

---
