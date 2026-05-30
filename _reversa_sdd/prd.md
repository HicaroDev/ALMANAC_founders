# PRD: Almanac

> Selo 🟡 PLANEJADO. Documento gerado a partir de ideation + personas.

**Versão:** 1.0
**Data:** 2026-05-30
**Autor:** reversa-drafter
**Status:** rascunho

---

## 1. Problema

🟡 Designers e equipes de produto enfrentam lentidão e dificuldade na colaboração e revisão de mockups HTML. O processo tradicional envolvia enviar arquivos HTML soltos por Slack, forçando cada membro a baixar e abrir manualmente no navegador. Para dar feedback, precisavam tirar print da tela e explicar verbalmente onde era a mudança. Cada nova versão reiniciava esse ciclo, gerando atrito e perda de produtividade.

### Quem sente
🟡 **Mary (Designer de Produto):** sente o problema no momento em que finaliza um mockup e precisa compartilhar com a equipe para aprovação, e a cada nova versão do design que exige re-envio e re-download do arquivo HTML.

---

## 2. Personas-alvo

🟡 Referência completa em [`personas.md`](./personas.md). Resumo:

- **Mary (Designer de Produto):** 🟡 Designer que cria mockups HTML e precisa compartilhá-los com a equipe para receber feedbacks visuais e aprovações sem o atrito de gerenciar arquivos soltos.

---

## 3. Métricas de sucesso

🟡 A métrica do hackathon é binária: a SEED.md deve ser funcional. Ao ser colada em um agente de IA, o software deve ser completamente construído com deploy na Vercel, banco no Supabase e login Google funcionando perfeitamente. Não há métrica comercial de longo prazo definida.

| Métrica | Unidade | Alvo | Prazo |
|---|---|---|---|
| 🟡 SEED funcional | Binário (sim/não) | Sim | 30/05/2026 23:59 |
| 🟡 Deploy Vercel + Supabase + Google Auth | Binário (sim/não) | Sim | 30/05/2026 23:59 |

---

## 4. Escopo (in)

🟡 Funcionalidades obrigatórias (jornada do usuário):

- 🟡 Login com conta Google (via Supabase Auth)
- 🟡 Upload de página HTML e renderização na tela
- 🟡 Histórico de projetos criados
- 🟡 Compartilhamento via link para outros usuários
- 🟡 Feedback ancorado (pins) em pontos exatos do HTML
- 🟡 Persistência dos pins (não somem ao recarregar)
- 🟡 Diferenciais competitivos: conversas/threads, reações emoji, editar/deletar comentários, marcar como resolvido/reaberto, pins arrastáveis, agrupamento de pins próximos, histórico de versões, presença ao vivo, dashboard com contadores e status

---

## 5. Não-objetivos (out)

🟡 Tudo que não está nas regras obrigatórias do hackathon está fora do escopo desta versão:

- 🟡 App mobile nativo
- 🟡 Integração com ferramentas externas (Figma, Jira, Notion)
- 🟡 Onboarding tutorial extenso
- 🟡 SEO e otimização para mecanismos de busca
- 🟡 Internacionalização (i18n) multi-idioma
- 🟡 Modo offline / PWA

---

## 6. Restrições

| Tipo | Descrição |
|---|---|
| 🟡 Técnica | Stack de código livre (IA escolhe) |
| 🟡 Técnica | Banco de dados obrigatoriamente no Supabase (PostgreSQL) |
| 🟡 Técnica | Deploy obrigatoriamente na Vercel |
| 🟡 Técnica | Autenticação obrigatoriamente com Google |
| 🟡 Prazo | Submissão até 30/05/2026 às 23:59 |
| 🟡 Compliance | Nenhuma exigência regulatória identificada |
| 🟡 Orçamento | Projeto de hackathon sem orçamento definido |

---

## 7. Dependências externas

🟡
- Supabase (banco PostgreSQL + Auth + Storage)
- Google OAuth (provedor de identidade via Supabase Auth)
- Vercel (plataforma de deploy)

---

## 8. Riscos

🟡 Derivado das premissas do ideation.md e restrições do hackathon.

| Risco | Impacto | Probabilidade | Mitigação proposta |
|---|---|---|---|
| 🟡 SEED.md mal estruturada falha ao gerar o app | Alto | Média | Usar RFC 2119 e SPEC.md do Symphony como referência |
| 🟡 Stack Vercel+Supabase+Google Auth não integra corretamente | Alto | Média | Documentar passo a passo da integração na SEED |
| 🟡 Prazo insuficiente para implementar todos os diferenciais | Médio | Alta | Priorizar funcionalidades obrigatórias primeiro |
| 🟡 Vídeo de demonstração não mostra todas as funcionalidades | Médio | Baixa | Roteiro de gravação checklist-based |

---

## 9. Critérios de aceite (alto nível)

🟡
- **Dado** que Mary fez login com Google, **Quando** ela faz upload de um arquivo HTML, **Então** o mockup é renderizado na tela e um link de compartilhamento é gerado.
- **Dado** que Mary compartilhou o link com um colega, **Quando** o colega clica em um ponto do mockup, **Então** um pin de comentário é criado e fica visível para todos com acesso ao link.
- **Dado** que existem pins no mockup, **Quando** a página é recarregada, **Então** os pins permanecem salvos e na mesma posição.
- **Dado** que Mary criou múltiplas versões do mesmo projeto, **Quando** ela acessa o histórico, **Então** ela pode navegar e comparar as versões.

---

## Pendências de cobertura

🟡 Nenhuma pendência identificada. Todas as seções preenchidas com base nas fontes disponíveis.

---
Gerado por reversa-drafter em 2026-05-30
Fontes: ideation.md, personas.md
