---
name: beta-movidesk
description: Consultar tickets, pessoas e serviços autorizados do Movidesk pelo MCP Beta MOV, escolhendo filtros compatíveis e respostas rápidas. Usar quando o usuário pedir consultas ou análises do Movidesk.
---

# Beta MOV

Quando o usuário invocar `@beta-movidesk` ou `$beta-movidesk`, use diretamente as ferramentas MCP disponíveis no ambiente. A conexão e o OAuth são responsabilidade do host: não tente descobrir endpoints com navegador, shell ou chamadas HTTP próprias.

## Escolha da ferramenta

- ID numérico de ticket: `movidesk_get_ticket`.
- Linha do tempo, resumo ou classificação: use a ferramenta específica, com `ticket_id` ou `protocol`; faça uma única consulta.
- Busca por assunto, categoria, status, datas ou filtros: `movidesk_search_tickets`.
- Tickets abertos em uma data: `movidesk_list_open_tickets`.
- Pessoas: `movidesk_search_people`.
- Serviços: `movidesk_list_services`.
- Busca dentro da descrição e interações: `movidesk_search_ticket_content`, somente quando o usuário pedir explicitamente conteúdo textual.

## Filtros corretos

- Em `movidesk_search_people`, `keyword` não é um filtro de nome enviado ao Movidesk. Para procurar uma pessoa por nome, use `filter` OData, por exemplo `contains(businessName, 'Gedore')`; combine com `active: true` somente se isso fizer sentido para o pedido.
- Em `movidesk_list_services`, forneça um critério real como `id`, `status`, `category`, `owner_team` ou `filter`; não use `keyword` isoladamente.
- Em `movidesk_search_ticket_content`, informe `keyword` e pelo menos um escopo fornecido pelo usuário: ticket, protocolo, datas, status, cliente, categoria, serviço, responsável ou `filter`. Não invente intervalos temporais para evitar perguntar.
- Para uma palavra que provavelmente está no assunto ou categoria, tente primeiro `movidesk_search_tickets` com `keyword` e `top: 25`. Use a busca de conteúdo somente se isso não atender ao pedido.
- Use `top: 25` por padrão; use até 100 somente quando o usuário pedir uma lista ampla. Use `skip` apenas para paginação solicitada.

## Velocidade e tentativas

- Faça a menor consulta que responde ao pedido. Não execute automaticamente buscas em pessoas, tickets e conteúdo ao mesmo tempo se o usuário não pediu todas.
- Consultas independentes e explicitamente pedidas podem ser feitas em paralelo, mantendo cada uma limitada.
- Após `search_criteria_required`, corrija adicionando o filtro compatível; não repita a mesma chamada.
- Após HTTP 502/503 ou indisponibilidade, informe a falha e pare. Não faça várias tentativas automáticas.
- Não anuncie plano, conexão, diagnóstico ou cada tentativa. Entregue apenas o resultado, os filtros relevantes e a limitação encontrada.

## Segurança e resposta

- Operar somente em leitura. Nunca prometer alteração de tickets, status, atribuições ou comentários.
- Se retornar 401/invalid_token, informe que é necessário reconectar o MCP pelo ChatGPT. Nunca solicite ou exponha tokens, segredos, Bearer tokens ou códigos OAuth.
- Diferencie claramente nenhum resultado, filtro inválido, falta de permissão e indisponibilidade.
- Não invente dados ausentes nem faça inferências além do conteúdo retornado pelo Movidesk.

## Configuração de referência

- Endpoint: `https://movidesk-oauth-proxy-pkce.thngrns.chatgpt.site/mcp/`
- Manter a barra final de `/mcp/`.
- Escopo: `movidesk:read`.
- Fluxo: Authorization Code com PKCE S256.
