# Inventario do Projeto - Hackaton

> Gerado pelo Reversa Scout em 2026-05-30

## Estrutura de Diretorios

```
/
├── .agents/skills/         # Skills do Reversa (framework)
├── .claude/skills/         # Skills do Reversa (framework)
├── .reversa/               # Nucleo do framework Reversa
├── _reversa_sdd/           # Saida das especificacoes
├── 01.md                   # Regras e requisitos do Hackathon
├── AGENTS.md               # Configuracao de agentes Reversa
├── CLAUDE.md               # Configuracao Claude/Cline
└── GEMINI.md               # Configuracao Gemini CLI
```

## Arquivos do Projeto

| Arquivo | Extensao | Descricao |
|---------|----------|-----------|
| `01.md` | .md | Regras do 2 Hackathon Relampago (Seed Edition) - Serial Founders. Desafio de recriar o app Almanac via SEED.md |
| `AGENTS.md` | .md | Configuracao dos agentes do Reversa |
| `CLAUDE.md` | .md | Configuracao do Claude |
| `GEMINI.md` | .md | Configuracao do Gemini CLI |

## Observacoes

- **Nao ha codigo de aplicacao** no projeto. O projeto e uma instalacao limpa do framework Reversa com o documento de regras do hackathon.
- **Nao ha dependencias**, gerenciadores de pacotes, entry points, CI/CD, Docker, banco de dados ou testes.
- **O unico artefato de dominio** e o arquivo `01.md` que descreve o desafio de recriar o app **Almanac** - uma ferramenta de colaboracao com feedback ancorado em mockups HTML.
- **Stack alvo definida no desafio:** Supabase (banco), Vercel (deploy), Google Auth (autenticacao).
