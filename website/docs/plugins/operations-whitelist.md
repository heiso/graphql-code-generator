---
id: operations-whitelist
title: Operations whitelist
---

`operations-whitelist` plugin can generate two types of files. One dedicated to the server, containing a mapping of queries by ids. The other is a mapping of ids by operation name to be consumed by the clients.

The purpose of these files is to increase security between a graphql server and its dedicated clients.

The client should use the generated json to only send a hash to the server, the server will compare the received hash to its generated json and use the associated query/mutation

{@import ../generated-config/operations-whitelist.md}

## Examples

Generate a client side whitelist

```yaml
# ...
generates:
  path/to/file-client.json:
    documents: path/to/documents.graphql
    config:
      output: client
    plugins:
      - operations-whitelist
```

Result would be like

```json
{
 "findUser": "09e78e289d08d46294048ec8f86d68b1be8316b2dab8ceeb8a84473904ce9ae6"
}
```

Generate a server side whitelist

```yaml
# ...
generates:
  path/to/file-server.json:
    schema: path/to/schema.json
    documents: path/to/documents.graphql
    config:
      output: server
    plugins:
      - operations-whitelist
```

Result would be like

```json
{
 "09e78e289d08d46294048ec8f86d68b1be8316b2dab8ceeb8a84473904ce9ae6": "fragment UserFields on User {\n  id\n  username\n  role\n}query findUser($userId: ID!) {\n  user(id: $userId) {\n    ...UserFields\n  }\n}"
}
```
