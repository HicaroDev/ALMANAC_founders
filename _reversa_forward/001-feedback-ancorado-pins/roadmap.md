# Roadmap: Feedback Ancorado (Pins)

> Identificador: `001-feedback-ancorado-pins`
> Data: `2026-05-30`
> Requirements: `_reversa_forward/001-feedback-ancorado-pins/requirements.md`
> Confidência: 🟢 CONFIRMADO, 🟡 INFERIDO, 🔴 LACUNA

## 1. Resumo da abordagem

Adicionar sistema de pins ao Almanac como feature client-side pesada com persistência em Supabase. O HTML renderizado no iframe recebe uma camada de overlay onde pins são desenhados como marcadores SVG absolutos. Criação usa coordenadas percentuais (x%, y%) calculadas a partir do clique. Persistência via tabela `pins` no Supabase PostgreSQL com suporte a Realtime subscriptions para notificações. Clusterização client-side com redução de raio em até 30% em áreas densas. Versões de mockup isolam pins por `version_id`.

## 2. Princípios aplicados

Nenhum princípio definido em `.reversa/principles.md`. Nada a reportar.

## 3. Decisões técnicas

| ID | Decisão | Justificativa | Alternativas descartadas | Confidência |
|----|---------|----------------|--------------------------|-------------|
| D-01 | Coordenadas percentuais (x%, y%) relativas ao documento | Resistentes a redimensionamento de viewport, ao contrário de px fixos | px absolutos (quebra em janelas diferentes) | 🟢 |
| D-02 | Overlay SVG sobre o iframe para renderizar pins | SVG escala nativamente com o container, sem perder qualidade | Canvas (perde interatividade por elemento), divs posicionadas (complexo para clusters) | 🟢 |
| D-03 | Clusterização client-side com raio dinâmico (redução até 30%) | UX fluida sem depender de banco para agrupar pins próximos | Cluster server-side (latência, sem resposta visual imediata) | 🟢 |
| D-04 | Notificações via Supabase Realtime + badge in-app | Stack já inclui Supabase, Realtime é nativo e evita WebSocket próprio | Polling (ineficiente), WebSocket próprio (mais infra) | 🟢 |
| D-05 | Pins vinculados a `version_id` (FK para versões de mockup) | Isolamento claro entre versões; migração opcional via seletor CSS | Pins soltos sem versão (perdem contexto ao re-upload) | 🟢 |

## 4. Premissas

Nenhuma — 0 marcadores `[DÚVIDA]` no requirements.

## 5. Delta arquitetural

| Componente | Arquivo de origem no legado | Tipo de mudança | Resumo |
|------------|------------------------------|-----------------|--------|
| PinOverlay | `_reversa_sdd/sdd/feedback-ancorado.md` | componente-novo | Camada SVG sobre iframe para exibir e interagir com pins |
| PinService | `_reversa_sdd/sdd/feedback-ancorado.md` | componente-novo | Lógica de CRUD de pins via Supabase client |
| NotificationService | `_reversa_sdd/prd.md` | componente-novo | Badge de notificações + subscription Realtime |
| ShareManager | `_reversa_sdd/prd.md` | componente-novo | Gerenciamento de convidados por e-mail |
| VersionService | `_reversa_sdd/sdd/historico-versoes.md` | componente-novo | Controle de versões de mockup para isolar pins |

## 6. Delta no modelo de dados

- Duas novas tabelas: `pins` e `pin_comments`
- Campo `shared_emails` adicionado à tabela de projetos
- Notificações como tabela `notifications` (opcional)
- Detalhe completo em: `_reversa_forward/001-feedback-ancorado-pins/data-delta.md`

## 7. Delta de contratos externos

| Contrato | Tipo | Arquivo de detalhe |
|----------|------|--------------------|
| Supabase Pins API | HTTP (REST) + Realtime | `interfaces/supabase-pins.md` |

## 8. Plano de migração

Projeto greenfield — sem migração de dados legados. As tabelas são criadas do zero.

## 9. Riscos e mitigações

| Risco | Impacto | Probabilidade | Mitigação |
|-------|---------|---------------|-----------|
| iframe CORS/Same-origin impede captura de clique | Alto | Baixa | Mockups são HTML puro hospedados no mesmo domínio via Vercel |
| Coordenadas percentuais perdem precisão em viewports muito pequenas | Médio | Baixa | Usar fallback com seletor CSS + coordenadas combinadas |
| Realtime congestiona com muitos pins | Baixo | Baixa | Paginação de pins (limite por versão) |

## 10. Critério de pronto

- [ ] Todas as ações do `actions.md` marcadas `[X]`
- [ ] `cross-check.md` (se executado) sem CRITICAL nem HIGH
- [ ] `regression-watch.md` gerado
- [ ] Re-extração reversa executada e sem regressão vermelha (recomendado, não obrigatório)

## 11. Histórico de alterações

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-05-30 | Versão inicial gerada por `/reversa-plan` | reversa |
