---
name: beta-movidesk
description: Consultar tickets, pessoas e serviços autorizados do Movidesk por meio do MCP Beta MOV, com orientação sobre filtros, paginação, OAuth/PKCE e diagnóstico. Usar quando o usuário pedir consultas ou explicações sobre a integração Movidesk.
---

# Beta MOV

Use esta skill quando o usuário invocar `@beta-movidesk` ou `$beta-movidesk` para consultar o Movidesk.

## Regras essenciais

- Operar somente em modo leitura; não prometer alterações de tickets, status, atribuições ou comentários.
- Escolher a ferramenta MCP mais específica: `movidesk_get_ticket`, `movidesk_search_tickets`, `movidesk_list_open_tickets`, `movidesk_search_people` ou `movidesk_list_services`.
- Informar os filtros e a paginação aplicados, sem inventar campos ausentes.
- Usar filtros nomeados antes de filtros OData livres; respeitar `top` de 1 a 100 e `skip` de 0 a 10000.
- Em falhas de autenticação, orientar nova autorização OAuth. Nunca solicitar ou expor tokens, segredos, Bearer tokens ou códigos de autorização.

## Conexão MCP

- Endpoint: `https://movidesk-oauth-proxy-pkce.thngrns.chatgpt.site/mcp/`
- Manter a barra final de `/mcp/`.
- Cliente: `movidesk-mcp-app`.
- Escopo: `movidesk:read`.
- Fluxo: Authorization Code com PKCE S256.

Se a consulta falhar, diferenciar ausência de resultados de erro de autenticação, permissão ou indisponibilidade do serviço. Para detalhes operacionais, consultar a documentação do projeto Movidesk quando ela estiver disponível no contexto.

