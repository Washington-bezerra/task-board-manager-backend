# Anotações Backend Let

## Decisões Tomadas

- **Colunas são parte do objeto lista:** facilita edição em lote (nome da lista, adicionar/excluir/editar colunas) com uma única requisição.
- **Cards ficam dentro das colunas:** cada coluna tem um array de cards.
- **Template de card:** cada lista define um template de campos para os cards daquela lista.
- **IDs estáveis:** use UUID/nanoid para listas, colunas e cards.
- **Atualização otimista:** o frontend atualiza o estado local antes da resposta da API para UX rápida (tipo Notion).
- **REST como API principal:** mais simples para frontend web, gRPC pode ser considerado para backend-backend.
- **JWT obrigatório:** o frontend envia o JWT no header `Authorization`, backend valida e extrai o usuário.
- **CORS restrito:** só aceita requisições do domínio do frontend.
- **Rate limiting e HTTPS em produção.**
- **BFF:** interessante para projetos grandes/multiplataforma, mas pode ser adiado.

## Endpoints RESTful sugeridos

- `GET /lists` — Listar todas as listas do usuário autenticado.
- `POST /lists` — Criar nova lista.
- `GET /lists/{listId}` — Buscar uma lista específica.
- `PUT /lists/{listId}` — Editar nome, colunas, template de card.
- `DELETE /lists/{listId}` — Excluir lista.
- `POST /lists/{listId}/columns/{columnId}/cards` — Criar card.
- `PUT /lists/{listId}/columns/{columnId}/cards/{cardId}` — Editar card.
- `DELETE /lists/{listId}/columns/{columnId}/cards/{cardId}` — Excluir card.
- `PATCH /lists/{listId}/columns/{columnId}/cards/{cardId}/move` — Mover card para outra coluna.
- `GET /me` — Dados do usuário autenticado.

## Exemplo de payload de lista

```json
{
  "id": "list-123",
  "name": "Minha Lista",
  "columns": [
    {
      "id": "col-1",
      "name": "A Fazer",
      "cards": [
        {
          "id": "card-1",
          "name": "Comprar pão",
          "fields": [
            { "name": "Assunto", "type": "text", "value": "Mercado" }
          ],
          "order": 1
        }
      ]
    }
  ],
  "cardTemplate": [
    { "name": "Assunto", "type": "text" }
  ]
}
