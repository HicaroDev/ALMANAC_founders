# Requirements: Feedback Ancorado (Pins)

> Identificador: `001-feedback-ancorado-pins`
> Data: `2026-05-30`
> Pasta da extração reversa: `_reversa_sdd/`
> Confidência: 🟢 CONFIRMADO, 🟡 INFERIDO, 🔴 LACUNA / DÚVIDA

## 1. Resumo Executivo

Feature central do Almanac. Usuários clicam em pontos exatos do mockup HTML renderizado, criam pins de comentário que persistem no banco. Visitantes com link veem os pins mas só usuários logados podem interagir. Inclui notificações in-app, cluster adaptativo e gerenciamento de versões de mockup. Elimina o atrito de "print + explicação textual" do feedback tradicional.

## 2. Contexto a partir do legado

| Fonte | Trecho relevante | Confidência |
|-------|------------------|-------------|
| `_reversa_sdd/sdd/feedback-ancorado.md#5` | RF-01 a RF-08: requisitos funcionais completos de pins | 🟡 |
| `_reversa_sdd/sdd/feedback-ancorado.md#6` | Modelo de dados: `pins` e `pin_comments` com campos e FKs | 🟡 |
| `_reversa_sdd/sdd/feedback-ancorado.md#7` | Edge cases: elemento que sumiu, criação simultânea, comentário vazio, scroll | 🟡 |
| `_reversa_sdd/prd.md#4` | Feedback ancorado como funcionalidade obrigatória do escopo | 🟡 |
| `_reversa_sdd/inventory.md` | Stack alvo: Supabase, Vercel, Google Auth | 🟢 |

## 3. Personas e cenários de uso

| Persona | Objetivo | Cenário-chave |
|---------|----------|---------------|
| Mary (Designer de Produto) | Apontar problemas visuais em mockups sem sair do navegador | Clica no ponto exato do HTML, escreve o feedback, salva. O colega vê o pin ao abrir o link. |
| Carlos (Desenvolvedor) | Responder feedbacks e marcar como resolvido | Abre o projeto, navega pelos pins, responde em thread, marca como resolvido. |

## 4. Regras de negócio novas ou alteradas

1. **RN-01:** Todo pin pertence a um projeto e a uma versão específica. 🟡
   - Tipo: nova
2. **RN-02:** Coordenadas do pin são sempre percentuais (x%, y%) relativas ao documento HTML, não ao viewport. 🟡
   - Tipo: nova
3. **RN-03:** Um pin pode estar em um dos status: `open`, `resolved`, `reopened`. 🟡
   - Tipo: nova
4. **RN-04:** Comentários em um pin podem ter replies (thread aninhada, profundidade 1). 🟡
   - Tipo: nova
5. **RN-05:** Visitantes com link público veem pins mas não podem criar, editar ou responder. 🟢
   - Tipo: nova
6. **RN-06:** Ao re-enviar HTML, pins da versão anterior ficam na versão antiga. O sistema tenta migrá-los via seletor CSS fallback; se encontrar correspondência, oferece ao usuário manter ou reposicionar. 🟢
   - Tipo: nova
7. **RN-07:** Cluster de pins reduz o tamanho do círculo individual em até 30% para evitar sobreposição. 🟢
   - Tipo: nova

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade | Critério de aceite | Confidência |
|----|-----------|------------|--------------------|-------------|
| RF-01 | Usuário clica no iframe e o sistema calcula coordenada (x%, y%) | Must | Coordenadas relativas ao documento, persistidas no banco | 🟡 |
| RF-02 | Sistema exibe marcador visual no ponto clicado | Must | Bolinha colorida (vermelha/laranja) na posição exata | 🟡 |
| RF-03 | Sistema abre formulário de comentário ao criar pin | Must | Input de texto + botão salvar visível após clique | 🟡 |
| RF-04 | Sistema salva pin no Supabase e persiste entre recarregamentos | Must | GET após refresh retorna todos os pins do projeto/versão | 🟡 |
| RF-05 | Sistema carrega todos os pins ao abrir o projeto | Must | Pins renderizados no iframe assim que o projeto abre | 🟡 |
| RF-06 | Usuário pode reposicionar pin arrastando o marcador | Should | Drag do marcador atualiza coordenadas no banco ao soltar | 🟡 |
| RF-07 | Sistema agrupa pins muito próximos (< 30px) em cluster expansível | Should | Indicador de grupo com contador, clique expande | 🟡 |
| RF-08 | Sistema mantém âncora resiliente via seletor CSS como fallback | Should | Se o elemento-alvo mudar de posição, o pin usa o seletor | 🟡 |
| RF-09 | Usuário pode marcar pin como resolvido | Should | Botão "Resolvido" altera status para `resolved` | 🟡 |
| RF-10 | Usuário pode reabrir pin resolvido | Should | Botão "Reabrir" altera status para `reopened` | 🟡 |
| RF-11 | Usuário pode editar ou deletar seu próprio comentário | Should | Menu de opções no pin: editar texto, deletar com confirmação | 🟡 |
| RF-12 | Usuário pode reagir com emoji a um pin ou comentário | Could | Seleção de emoji (like, risada, coração) persistida | 🟡 |
| RF-13 | Sistema notifica usuários sobre novos pins e respostas via notificação in-app (sininho/badge) | Should | Badge visível no header, lista de notificações ao clicar | 🟢 |
| RF-14 | Ao criar/compartilhar projeto, usuário define quem pode acessar por e-mail | Should | Campo de input de e-mail + lista de convidados persistida | 🟢 |
| RF-15 | Cluster de pins reduz tamanho do círculo individual em até 30% em áreas densas | Should | Círculos menores gradualmente conforme densidade aumenta | 🟢 |

## 6. Requisitos Não Funcionais

| Tipo | Requisito | Evidência ou justificativa | Confidência |
|------|-----------|----------------------------|-------------|
| Desempenho | Criação de pin em < 2 cliques | Experiência do usuário deve ser instantânea | 🟡 |
| Desempenho | Carregamento de pins em < 500ms | Projetos podem ter dezenas de pins | 🟡 |
| Segurança | Público com link vê pins (read-only). Apenas usuários logados e convidados criam/editam | RN-05: visibilidade híbrida | 🟢 |
| Confiabilidade | Pins persistem 100% após refresh | Perda de dados é inaceitável | 🟡 |

## 7. Critérios de Aceitação

```gherkin
Cenário: Criar pin com sucesso
  Dado que o usuário está visualizando um mockup HTML no iframe
  Quando o usuário clica em um ponto do mockup
  Então um marcador visual aparece no local clicado
  E um formulário de comentário é exibido
  E o usuário digita um texto e clica em Salvar
  Então o pin é persistido no banco de dados
  E o pin fica visível para todos os usuários com acesso ao projeto

Cenário: Pin persiste após recarregar
  Dado que existem pins salvos em um projeto
  Quando o usuário recarrega a página
  Então todos os pins são carregados e exibidos nas mesmas posições

Cenário: Comentário vazio não é permitido
  Dado que o formulário de criação de pin está aberto
  Quando o usuário tenta salvar sem digitar texto
  Então o botão Salvar permanece desabilitado

Cenário: Agrupamento de pins próximos
  Dado que existem dois pins a menos de 30px de distância
  Quando a página é carregada
  Então os pins são exibidos como um único cluster com contador
  E ao clicar no cluster, os pins individuais são exibidos
```

## 8. Prioridade MoSCoW

| Item | MoSCoW | Justificativa |
|------|--------|---------------|
| RF-01 a RF-05 (criar, exibir, persistir, carregar pins) | Must | Núcleo da feature, sem isso não há feedback ancorado |
| RF-06 (reposicionar), RF-07 (agrupar), RF-08 (fallback) | Should | Melhora significativa de UX, mas feature funciona sem |
| RF-09 (resolver), RF-10 (reabrir), RF-11 (editar/deletar) | Should | Ciclo de vida completo do feedback |
| RF-13 (notificações), RF-14 (compartilhar por e-mail), RF-15 (cluster densidade) | Should | Melhora colaboração e UX |
| RF-12 (reações emoji) | Could | Diferencial competitivo, baixo esforço |

## 9. Esclarecimentos

### Sessão 2026-05-30

- **Q1:** Como a visibilidade dos pins funciona entre públicos e logados?
  - **R:** Híbrido: público com link vê pins (read-only). Só usuários logados e convidados criam, editam e respondem.

- **Q2:** O que acontece com pins quando o HTML é atualizado (nova versão)?
  - **R:** Pins ficam na versão antiga. O sistema tenta migrar via seletor CSS fallback; se encontrar correspondência, oferece ao usuário manter ou reposicionar.

- **Q3:** Pins aparecem em tempo real?
  - **R:** Não em tempo real automático. O sistema envia notificação push/badge in-app para informar usuários sobre novos pins e respostas. O projeto tem uma seção de "Compartilhar" onde o dono adiciona e-mails dos colaboradores.

- **Q4:** Deve haver notificações?
  - **R:** Sim, in-app (sininho/badge no header). Apenas dentro do app, sem e-mail.

- **Q5:** Re-upload de HTML substituindo o anterior — destino dos pins?
  - **R:** Pins ficam na versão anterior. O sistema tenta migrar via seletor CSS fallback. Se achar correspondência, pergunta ao usuário se quer migrar. Pins órfãos (sem ancora) ficam marcados como "orphaned" visíveis na versão antiga.

## 10. Lacunas

Nenhuma lacuna pendente no momento.

## 11. Histórico de alterações

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-05-30 | Versão inicial gerada por `/reversa-requirements` | reversa |

---

**Feature dir:** `_reversa_forward/001-feedback-ancorado-pins/`
**Requirements:** `_reversa_forward/001-feedback-ancorado-pins/requirements.md`

0 marcadores `[DÚVIDA]` no documento.

**Próximo passo sugerido:** `/reversa-plan`

Digite **CONTINUAR** para prosseguir com `/reversa-plan`.
