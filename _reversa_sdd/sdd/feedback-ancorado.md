# Spec: Feedback Ancorado (Pins)

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Sistema de pins — usuários clicam em pontos exatos do HTML renderizado, criam comentários ancorados que persistem e ficam visíveis para todos com acesso ao link.

---

## 2. Contexto e Motivação

🟡 **Problema:** Feedback tradicional exigia prints + explicação textual. Pins eliminam ambiguidade — o comentário está exatamente onde o problema está.

🟡 **Por que agora:** Feature central do Almanac. Diferencial competitivo versus compartilhar arquivos por Slack.

---

## 3. Goals

- [ ] G-01: Usuario clica em qualquer ponto do iframe e cria um pin
- [ ] G-02: Pin persiste apos recarregar a pagina
- [ ] G-03: Pin mostra comentario em tooltip/modal ao clicar

**Métricas:**
| Métrica | Target |
|---|---|
| Criação de pin | < 2 cliques |
| Persistencia apos refresh | 100% |

---

## 4. Non-Goals

- NG-01: 🟡 Pin com desenho/annotation (apenas texto)
- NG-02: 🟡 Pin com anexo de imagem
- NG-03: 🟡 Pin em elementos nao-HTML (video, canvas)

---

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade | Criterio |
|---|---|---|---|
| RF-01 | 🟡 Usuario clica no iframe e sistema calcula coordenada (x,y,%) | Must | Coordenadas relativas ao viewport |
| RF-02 | 🟡 Sistema exibe marcador visual no ponto clicado | Must | Bolinha colorida na posicao |
| RF-03 | 🟡 Sistema abre formulario de comentario ao criar pin | Must | Input de texto + botao salvar |
| RF-04 | 🟡 Sistema salva pin no Supabase e persiste | Must | GET apos refresh retorna pins |
| RF-05 | 🟡 Sistema carrega todos os pins ao abrir projeto | Must | Lista de pins carregada com o projeto |
| RF-06 | 🟡 Usuario pode reposicionar pin arrastando | Should | Drag do marcador atualiza coordenadas |
| RF-07 | 🟡 Sistema agrupa pins muito proximos | Should | Cluster de pins vira grupo expansivel |
| RF-08 | 🟡 Sistema mantem ancora resiliente a mudancas de layout | Should | Xpath/seletor como fallback |

### Fluxo Principal

1. 🟡 Usuario visualiza mockup renderizado no iframe
2. 🟡 Usuario clica em ponto exato do mockup
3. 🟡 Sistema calcula coordenada percentual (x%, y%)
4. 🟡 Marcador vermelho/laranja aparece no local
5. 🟡 Modal de comentario abre para o usuario digitar
6. 🟡 Usuario salva → pin persistido no Supabase
7. 🟡 Outros usuarios veem o pin ao acessar o link

### Fluxos Alternativos

**Reposicionar:**
1. 🟡 Usuario arrasta o marcador do pin
2. 🟡 Sistema atualiza coordenadas em tempo real
3. 🟡 Ao soltar, sistema salva nova posicao

**Agrupamento:**
1. 🟡 Sistema detecta 2+ pins em raio < 30px
2. 🟡 Exibe indicador de grupo com contador
3. 🟡 Usuario clica no grupo para expandir e ver individuais

---

## 6. Modelo de Dados

```
pins {
  id: uuid PK
  project_id: uuid FK
  version: int
  x_percent: float
  y_percent: float
  selector: string (opcional, fallback)
  created_by: uuid FK -> users.id
  status: enum (open, resolved, reopened)
  created_at: timestamp
  updated_at: timestamp
}

pin_comments {
  id: uuid PK
  pin_id: uuid FK -> pins.id
  user_id: uuid FK -> users.id
  content: text
  parent_id: uuid (nullable, para replies)
  created_at: timestamp
}
```

---

## 7. Edge Cases

| Cenário | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Pin em elemento que sumiu | HTML atualizado, seletor nao encontra | Pin aparece na coordenada % sem ancora visual |
| EC-02: 🟡 Dois usuarios clicam no mesmo pixel | Criacao simultanea | Ambos salvos, coordenadas quase identicas → agrupamento |
| EC-03: 🟡 Comentario vazio | Usuario clica sem digitar | Botao salvar desabilitado ate digitar |
| EC-04: 🟡 iframe com scroll | Pin em area fora do viewport | Coordenada % relativa ao documento inteiro |

---
