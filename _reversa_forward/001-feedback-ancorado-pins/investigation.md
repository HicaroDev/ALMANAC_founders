# Investigation: Feedback Ancorado (Pins)

> Apoio técnico para o roadmap.

## Stack confirmada

| Componente | Tecnologia | Fonte |
|------------|-----------|-------|
| Banco | Supabase PostgreSQL | `_reversa_sdd/inventory.md` |
| Autenticação | Google OAuth via Supabase Auth | `_reversa_sdd/inventory.md` |
| Deploy | Vercel | `_reversa_sdd/inventory.md` |
| Frontend | Livre (a critério da SEED) | `_reversa_sdd/dependencies.md` |

## Alternativas avaliadas

### Renderização de pins

| Alternativa | Prós | Contras | Veredito |
|-------------|------|---------|----------|
| Overlay SVG sobre iframe | Escala nativamente, interativo por elemento, cluster fácil | Nenhum relevante | ✅ **Escolhido** |
| Canvas HTML | Performance para muitos pins | Sem eventos por pin, hit-test manual | ❌ Descartado |
| Posicionamento absoluto com divs | Simples de implementar | Quebra em zoom/resize, cluster complexo | ❌ Descartado |

### Clusterização

| Alternativa | Prós | Contras | Veredito |
|-------------|------|---------|----------|
| Client-side (JS, raio dinâmico) | Resposta imediata, sem latência | Consumo de CPU no navegador | ✅ **Escolhido** |
| Server-side (SQL, PostGIS) | Preciso, escala para milhares | Latência de rede, sem feedback visual instantâneo | ❌ Descartado |

### Persistência de coordenadas

| Alternativa | Prós | Contras | Veredito |
|-------------|------|---------|----------|
| Percentual (x%, y%) | Resiliente a redimensionamento | Menos preciso em viewports extremas | ✅ **Escolhido** |
| Pixel absoluto (px) | Simples de capturar | Quebra em janelas diferentes / zoom | ❌ Descartado |
| Seletor CSS + offset | Resiliente a mudanças de layout | Complexo de calcular e manter | 🔄 Fallback (D-05) |

## Padrões aplicáveis

- **Overlay Pattern:** Camada transparente sobre conteúdo para interação adicional
- **Observer Pattern:** Realtime subscriptions para notificações
- **Command Pattern:** Ações de pin (criar, reposicionar, resolver) como comandos
