# Spec: Historico de Versoes

**Versão:** 1.0
**Status:** Rascunho
**Data:** 2026-05-30

---

## 1. Resumo

🟡 Sistema de versionamento — usuarios criam multiplas versoes do mesmo projeto e comparam diferencas entre elas.

---

## 2. Contexto e Motivação

🟡 **Problema:** Design evolui em iteracoes. Sem historico de versoes, nao da para comparar "antes vs depois" ou reverter para uma versao anterior.

🟡 **Por que agora:** Diferencial competitivo e essencial para o fluxo de revisao.

---

## 3. Goals

- [ ] G-01: Usuario cria nova versao a partir da atual
- [ ] G-02: Usuario navega entre versoes no seletor
- [ ] G-03: Usuario compara duas versoes lado a lado

---

## 4. Non-Goals

- NG-01: 🟡 Diff automatico de HTML (apenas visual)
- NG-02: 🟡 Merge de versoes
- NG-03: 🟡 Branching (apenas linear)

---

## 5. Requisitos Funcionais

| ID | Requisito | Prioridade |
|---|---|---|
| RF-01 | 🟡 Usuario pode criar nova versao a partir da atual | Must |
| RF-02 | 🟡 Sistema lista versoes em ordem cronologica | Must |
| RF-03 | 🟡 Usuario pode alternar entre versoes no visualizador | Must |
| RF-04 | 🟡 Sistema exibe versao atual no header do projeto | Must |
| RF-05 | 🟡 Usuario pode comparar duas versoes lado a lado | Should |
| RF-06 | 🟡 Pins sao preservados entre versoes (mesmas coordenadas) | Must |

### Fluxo Principal

1. 🟡 Usuario clica "Nova Versao" no projeto
2. 🟡 Sistema copia HTML atual como nova versao (v2, v3...)
3. 🟡 Usuario faz upload de novo HTML ou mantem o atual
4. 🟡 Versao aparece no seletor dropdown
5. 🟡 Usuario alterna entre versoes e ve pins de cada uma
6. 🟡 Modo comparacao mostra duas renderizacoes lado a lado

### Modelo de Dados

```
versions {
  id: uuid PK
  project_id: uuid FK
  version_number: int
  storage_path: string
  created_at: timestamp
  created_by: uuid FK
}
```

---

## 6. Edge Cases

| Cenario | Trigger | Comportamento |
|---|---|---|
| EC-01: 🟡 Versao sem HTML | Criar versao sem fazer upload | Impedir criacao, alerta "Upload necessario" |
| EC-02: 🟡 Deletar versao atual | Usuario tenta excluir versao em uso | Impedir, solicitar trocar para outra versao primeiro |
| EC-03: 🟡 Pins de versao anterior sumiram | Alternar para versao antiga | Cada versao tem seus proprios pins |

---
