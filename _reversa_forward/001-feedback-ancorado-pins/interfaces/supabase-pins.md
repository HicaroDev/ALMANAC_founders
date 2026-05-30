# Interface: Supabase Pins API

> Contrato de comunicação com o banco Supabase para operações de pins.

## Configuração

| Parâmetro | Valor |
|-----------|-------|
| Base URL | `SUPABASE_URL/rest/v1` |
| Auth Header | `Authorization: Bearer SUPABASE_ANON_KEY` |
| Prefer Header | `return=representation` |

## Endpoints

### Listar pins de uma versão

```
GET /pins?project_id=eq.<uuid>&version_id=eq.<uuid>&select=*,pin_comments(*)
```

**Resposta 200:**
```json
[
  {
    "id": "uuid",
    "x_percent": 45.5,
    "y_percent": 72.1,
    "selector": null,
    "status": "open",
    "created_by": "uuid",
    "created_at": "2026-05-30T10:00:00Z",
    "pin_comments": [
      {
        "id": "uuid",
        "user_id": "uuid",
        "content": "Ajustar margem aqui",
        "parent_id": null
      }
    ]
  }
]
```

### Criar pin

```
POST /pins
```

**Body:**
```json
{
  "project_id": "uuid",
  "version_id": "uuid",
  "x_percent": 45.5,
  "y_percent": 72.1,
  "selector": "#header > .logo",
  "created_by": "uuid"
}
```

**Resposta 201:** O pin criado com `id` e timestamps.

### Atualizar pin (reposicionar, resolver)

```
PATCH /pins?id=eq.<uuid>
```

**Body:**
```json
{
  "x_percent": 50.0,
  "y_percent": 30.0,
  "status": "resolved"
}
```

### Deletar pin

```
DELETE /pins?id=eq.<uuid>
```

**Resposta 204:** Sem conteúdo.

## Realtime Subscription

```js
supabase
  .channel('pins')
  .on('postgres_changes',
    { event: 'INSERT', schema: 'public', table: 'pins',
      filter: `project_id=eq.${projectId}` },
    (payload) => { /* notificar usuários do projeto */ }
  )
  .subscribe()
```

## Erros

| Código | Significado |
|--------|-------------|
| 401 | Não autenticado, redirecionar para login |
| 403 | Sem permissão (não é convidado do projeto) |
| 404 | Projeto ou versão não encontrados |
| 422 | Dados inválidos (coordenadas fora de 0-100, texto vazio) |
