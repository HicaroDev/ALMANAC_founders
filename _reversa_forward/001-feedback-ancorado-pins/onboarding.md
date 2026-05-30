# Onboarding: Feedback Ancorado (Pins)

> Passo a passo para testar a feature manualmente.

## Pré-requisitos

- Projeto Almanac rodando (Vercel deploy ou dev local)
- Banco Supabase configurado com as migrations aplicadas
- Usuário logado com Google Auth

## Roteiro de teste

### 1. Criar um projeto

1. Acesse o Almanac e faça login com Google
2. Clique em "Novo Projeto"
3. Faça upload de um arquivo HTML
4. Confirme que o mockup renderizou no iframe

### 2. Criar um pin

1. Clique em qualquer ponto do mockup renderizado
2. Confirme que um marcador (bolinha) apareceu no local
3. Confirme que o formulário de comentário abriu
4. Digite um texto e clique em Salvar
5. Recarregue a página
6. Confirme que o pin ainda está visível na mesma posição

### 3. Testar comentário vazio

1. Clique em um ponto do mockup
2. Tente salvar sem digitar texto
3. Confirme que o botão Salvar permanece desabilitado

### 4. Testar notificação

1. Em outra aba, faça login com outro usuário (convidado do projeto)
2. Abra o mesmo projeto
3. Na aba original, crie um novo pin
4. Na aba do convidado, confirme que o badge de notificação aparece

### 5. Testar cluster

1. Crie 3 pins em um raio < 30px um do outro
2. Confirme que eles agruparam em um cluster com contador
3. Clique no cluster e confirme que os pins individuais são exibidos
4. Confirme que os círculos individuais estão menores (redução até 30%)

### 6. Testar versões

1. No mesmo projeto, faça upload de um novo HTML (nova versão)
2. Confirme que os pins da versão anterior não aparecem na nova
3. Navegue de volta à versão anterior
4. Confirme que os pins originais ainda estão lá

### 7. Testar permissões

1. Copie o link do projeto e abra em uma aba anônima (sem login)
2. Confirme que os pins são visíveis (read-only)
3. Tente clicar em um ponto para criar pin
4. Confirme que a interação é bloqueada (redirect para login ou overlay)
