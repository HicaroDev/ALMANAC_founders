# Actions: Feedback Ancorado (Pins)

> Identificador: `001-feedback-ancorado-pins`
> Data: `2026-05-30`
> Roadmap: `_reversa_forward/001-feedback-ancorado-pins/roadmap.md`

## Resumo

| Métrica | Valor |
|---------|-------|
| Total de ações | 16 |
| Paralelizáveis (`[//]`) | 6 |
| Maior cadeia de dependência | 5 |

## Fase 1, Preparação

| ID | Descrição | Dependências | Paralelismo | Arquivo alvo | Confidência | Status |
|----|-----------|--------------|-------------|--------------|-------------|--------|
| T001 | Migration: criar tabela `pins` (colunas, FKs, índices) no Supabase | - | `[//]` | `seed.sql` | 🟢 | `[X]` |
| T002 | Migration: criar tabela `pin_comments` (colunas, FKs, ON DELETE CASCADE) | T001 | - | `seed.sql` | 🟢 | `[X]` |
| T003 | Migration: criar tabela `notifications` + campo `shared_emails` em `projects` | - | `[//]` | `seed.sql` | 🟢 | `[X]` |

## Fase 2, Testes

| ID | Descrição | Dependências | Paralelismo | Arquivo alvo | Confidência | Status |
|----|-----------|--------------|-------------|--------------|-------------|--------|
| T004 | Testes unitários do PinService (CRUD básico) | T001, T002 | - | `src/services/__tests__/pinService.test.ts` | 🟡 | `[ ]` |
| T005 | Testes de integração: criar pin via API e verificar persistência | T004 | - | `src/services/__tests__/pinService.integration.test.ts` | 🟡 | `[ ]` |

## Fase 3, Núcleo

| ID | Descrição | Dependências | Paralelismo | Arquivo alvo | Confidência | Status |
|----|-----------|--------------|-------------|--------------|-------------|--------|
| T006 | PinService: implementar `createPin()`, `listPins()`, `updatePin()`, `deletePin()` | T001, T002 | - | `src/lib/supabase.ts` | 🟢 | `[X]` |
| T007 | PinOverlay: componente SVG que renderiza marcadores no iframe a partir de lista de pins | - | `[//]` | `src/app/projeto/[id]/page.tsx` | 🟢 | `[X]` |
| T008 | Click handler: capturar clique no iframe, calcular x%/y%, abrir formulário de comentário, chamar createPin() | T006, T007 | - | `src/app/projeto/[id]/page.tsx` | 🟢 | `[X]` |
| T009 | Clusterização: agrupar pins com distância < 30px, reduzir raio do círculo individual em até 30% | T007, T008 | - | `src/utils/clusterPins.ts` | 🟢 | `[X]` |

## Fase 4, Integração

| ID | Descrição | Dependências | Paralelismo | Arquivo alvo | Confidência | Status |
|----|-----------|--------------|-------------|--------------|-------------|--------|
| T010 | VersionService: filtrar pins por `version_id`, migração via seletor CSS fallback | T006 | - | `src/app/projeto/[id]/page.tsx` | 🟢 | `[X]` |
| T011 | NotificationService: subscription Supabase Realtime em `pins` (INSERT), criar notificação no banco | T003, T006 | - | `src/app/projeto/[id]/page.tsx` | 🟡 | `[X]` |
| T012 | NotificationBadge: componente de sininho no header com contagem de não-lidas | T011 | `[//]` | `src/components/header.tsx` | 🟡 | `[X]` |
| T013 | ShareManager: campo de input de e-mail na tela do projeto, salvar em `shared_emails` | T003 | `[//]` | `src/app/projeto/[id]/page.tsx` | 🟢 | `[X]` |

## Fase 5, Polimento

| ID | Descrição | Dependências | Paralelismo | Arquivo alvo | Confidência | Status |
|----|-----------|--------------|-------------|--------------|-------------|--------|
| T014 | Visibilidade híbrida: público vê pins (read-only), logados podem interagir; redirecionar se não autenticado | T006, T007 | - | `src/app/compartilhado/[id]/page.tsx` | 🟢 | `[X]` |
| T015 | Edge cases: pins órfãos (orphaned) quando seletor não encontra elemento, feedback visual para erro de rede | T006, T008 | `[//]` | `src/app/projeto/[id]/page.tsx` | 🟡 | `[X]` |
| T016 | Onboarding: cobrir cenários do onboarding.md (comentário vazio, re-upload de HTML, cluster) | T009, T010, T014 | - | `src/app/projeto/[id]/page.tsx` | 🟡 | `[X]` |

## Notas de execução

_Reservado para /reversa-coding._

## Histórico de alterações

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-05-30 | Versão inicial gerada por `/reversa-to-do` | reversa |
