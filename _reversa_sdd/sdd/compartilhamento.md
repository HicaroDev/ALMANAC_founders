# Spec: Compartilhamento

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Sistema de compartilhamento — gera link unico para cada projeto. Qualquer pessoa com o link pode visualizar o mockup e os feedbacks ancorados.

---

## 2. Contexto e Motivação

🟡 **Problema:** Sem compartilhamento via link, o Almanac seria apenas um visualizador pessoal. O valor esta em colaborar com a equipe.

🟡 **Por que agora:** Funcionalidade obrigatoria do hackathon e core do produto.

---

## 3. Goals

- [ ] G-01: Gerar link compartilhavel unico por projeto
- [ ] G-02: Usuario sem login consegue ver mockup + pins
- [ ] G-03: Copiar link com 1 clique

**Metricas:**
| Metrica | Target |
|---|---|
| Link gerado em | < 1s |
| Copiar link | 1 clique |

---

## 4. Non-Goals

- NG-01: 🟡 Permissoes granulares (leitura/escrita por link)
- NG-02: 🟡 Link com expiracao
- NG-03: 🟡 Protecao por senha no link

---

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade | Criterio |
|---|---|---|---|
| RF-01 | 🟡 Sistema deve gerar URL unica para cada projeto | Must | UUID ou slug na URL |
| RF-02 | 🟡 Sistema deve exibir botao "Copiar Link" | Must | Copia URL para clipboard |
| RF-03 | 🟡 Sistema deve permitir acesso sem login a pagina compartilhada | Must | Visitante ve mockup + pins (read-only) |
| RF-04 | 🟡 Visitante deve ver pins existentes no mockup | Must | Pins carregam mesmo sem auth |
| RF-05 | 🟡 Visitante nao pode criar/edit pins sem login | Must | Criar pin redireciona para login |

### Fluxo Principal

1. 🟡 Usuario abre projeto e clica "Compartilhar"
2. 🟡 Sistema gera URL: almanac.app/projeto/<id>
3. 🟡 Sistema copia URL para clipboard automaticamente
4. 🟡 Usuario envia link para equipe via Slack/email
5. 🟡 Colega abre link e ve mockup renderizado com pins

---

## 6. Edge Cases

| Cenário | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Link de projeto deletado | Abrir URL de projeto excluido | Pagina 404 "Projeto nao encontrado" |
| EC-02: 🟡 Visitante tenta criar pin | Clicar no mockup sem estar logado | Modal "Faca login para comentar" |
| EC-03: 🟡 Projeto com HTML corrompido | Link compartilhado mas HTML quebrado | Exibir mensagem "Erro ao renderizar" |

---
