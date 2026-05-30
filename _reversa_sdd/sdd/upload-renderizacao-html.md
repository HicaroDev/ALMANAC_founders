# Spec: Upload e Renderizacao HTML

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Usuário faz upload de arquivo HTML mockup, sistema armazena no Supabase Storage e renderiza em iframe seguro para visualização e ancoragem de feedbacks.

---

## 2. Contexto e Motivação

🟡 **Problema:** Sem renderização do HTML, não há mockup para inspecionar nem onde ancorar pins.

🟡 **Por que agora:** Core do produto — sem upload, o Almanac não tem função.

---

## 3. Goals

- [ ] G-01: Upload de arquivo .html em < 5s (arquivo medio)
- [ ] G-02: HTML renderizado fielmente em iframe sandbox
- [ ] G-03: Atualizar HTML mantendo pins existentes

**Métricas:**
| Métrica | Target |
|---|---|
| Upload completo | < 5s |
| Renderizacao fiel | Sim |

---

## 4. Non-Goals

- NG-01: 🟡 Upload de pastas ZIP
- NG-02: 🟡 Preview de imagens avulsas (apenas HTML)
- NG-03: 🟡 Editor HTML inline

---

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade | Criterio |
|---|---|---|---|
| RF-01 | 🟡 Usuario deve fazer upload de arquivo .html | Must | File picker aceita apenas .html |
| RF-02 | 🟡 Sistema deve armazenar HTML no Supabase Storage | Must | URL publica gerada apos upload |
| RF-03 | 🟡 Sistema deve renderizar HTML em iframe sandbox | Must | iframe com sandbox attribute |
| RF-04 | 🟡 Usuario deve poder substituir HTML por nova versao | Must | Upload button na tela do projeto |
| RF-05 | 🟡 Sistema deve preservar pins ao substituir HTML | Should | Pins relinkam por seletor/xpath |

### Fluxo Principal

1. 🟡 Usuario abre projeto e clica "Upload HTML"
2. 🟡 Sistema exibe file picker filtrado para .html
3. 🟡 Usuario seleciona arquivo → sistema faz upload ao Supabase Storage
4. 🟡 Sistema renderiza HTML em iframe na tela
5. 🟡 URL do projeto é gerada para compartilhamento

### Fluxos Alternativos

**Substituir HTML:**
1. 🟡 Usuario clica "Atualizar HTML" na tela do projeto
2. 🟡 Sistema substitui arquivo no Storage
3. 🟡 iframe recarrega com novo HTML
4. 🟡 Pins existentes permanecem nas mesmas coordenadas

---

## 6. Modelo de Dados

```
project_files {
  id: uuid PK
  project_id: uuid FK
  storage_path: string
  original_name: string
  version: int
  uploaded_at: timestamp
}
```

---

## 7. Edge Cases

| Cenário | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Arquivo maior que 10MB | Upload de HTML grande | Rejeitar com mensagem "Arquivo muito grande" |
| EC-02: 🟡 HTML com JavaScript malicioso | Upload com script tags | Sandbox do iframe bloqueia execucao |
| EC-03: 🟡 HTML invalido | Upload de arquivo corrompido | Tentar renderizar mesmo assim, mensagem se falhar |
| EC-04: 🟡 Storage Supabase fora do ar | Falha no upload | Exibir "Erro ao fazer upload. Tente novamente." |

---

## 8. Seguranca

- 🟡 iframe com `sandbox="allow-same-origin"` (sem scripts)
- 🟡 RLS no Storage: apenas dono do projeto pode fazer upload
- 🟡 Qualquer usuario com link pode visualizar (leitura publica)

---
