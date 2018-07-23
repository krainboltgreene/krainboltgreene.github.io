---
title: "Postgres Client interface"
draft: false
---

``` ruby
require "postgresql"

client = PostgreSQL::Client.new(url: ENV["POSTGRESQL_URL"])

table = PostgreSQL::Table.new(client: client, name: "accounts")

query = PostgreSQL::Query::Select.new(fields: ["id", "name"], table: table, where: { "name" => "?" }, parameters: ["%urtis%"])
# query.to_s #=> SELECT id, name FROM accounts WHERE name = "%urtis%";

request = PostgreSQL::Request.new(client: client, query: query)
# request.to_a #=> [{ id: 4, name: "Kurtis Rainbolt-Greene"}, { ... }]

query2 = PostgreSQL::Query::Update.new(fields: { "name" => "James Rainbolt-Greene", "age" => 42 }, table: table, where: { "id" => 4 })
# query2.to_s #=> UPDATE accounts SET name = "James Rainbolt-Greene", age = 42 WHERE id = 4;
```
