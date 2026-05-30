# Data Delta: Feedback Ancorado (Pins)

> Diff conceitual do modelo de dados para suportar a feature.

## Novas tabelas

### pins

| Coluna | Tipo | Restrições | Notas |
|--------|------|------------|-------|
| id | uuid | PK, default gen_random_uuid() | |
| project_id | uuid | FK -> projects.id, NOT NULL | |
| version_id | uuid | FK -> versions.id, NOT NULL | Isola pins por versão de mockup |
| x_percent | float | NOT NULL (0-100) | Coordenada percentual X |
| y_percent | float | NOT NULL (0-100) | Coordenada percentual Y |
| selector | text | NULLABLE | Seletor CSS fallback (ex.: "#header > .logo") |
| created_by | uuid | FK -> users.id, NOT NULL | |
| status | text | NOT NULL, DEFAULT 'open' | Valores: open, resolved, reopened |
| created_at | timestamptz | NOT NULL, DEFAULT now() | |
| updated_at | timestamptz | NOT NULL, DEFAULT now() | |

**Índices:**
- `idx_pins_project_version` ON pins(project_id, version_id)
- `idx_pins_created_by` ON pins(created_by)

### pin_comments

| Coluna | Tipo | Restrições | Notas |
|--------|------|------------|-------|
| id | uuid | PK, default gen_random_uuid() | |
| pin_id | uuid | FK -> pins.id, NOT NULL, ON DELETE CASCADE | |
| user_id | uuid | FK -> users.id, NOT NULL | |
| content | text | NOT NULL | |
| parent_id | uuid | NULLABLE, FK -> pin_comments.id | Para replies (profundidade 1) |
| created_at | timestamptz | NOT NULL, DEFAULT now() | |

**Índices:**
- `idx_pin_comments_pin` ON pin_comments(pin_id)

### notifications (opcional)

| Coluna | Tipo | Restrições | Notas |
|--------|------|------------|-------|
| id | uuid | PK | |
| user_id | uuid | FK -> users.id, NOT NULL | Destinatário |
| type | text | NOT NULL | Valores: new_pin, new_reply, pin_resolved, pin_reopened |
| pin_id | uuid | FK -> pins.id, NULLABLE | |
| read | boolean | NOT NULL, DEFAULT false | |
| created_at | timestamptz | NOT NULL, DEFAULT now() | |

## Campos alterados em tabelas existentes

### projects (assumindo que já existe)

| Coluna | Mudança | Tipo | Notas |
|--------|---------|------|-------|
| shared_emails | NOVO | text[] | Lista de e-mails de convidados |
