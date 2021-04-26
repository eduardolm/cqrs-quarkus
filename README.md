## API Orçamento

## A aplicação

Esta API simula o cenário de uma conta bancária, onde o usuário cadastra uma entrada ou saída de recursos.
A adição ou remoção de fundos da conta do usuário utiliza serviços distribuídos para calcular o novo saldo da conta.
A API permite ainda que o usuário consulte seu saldo em conta.

## Deploy dos serviços externos

```
docker-compose up -d --build
```
Faz o deploy de 4 containers Docker sendo MongoDB, PostgreSQL, Kafka e Zookeeper (necessário para o Kafka)

Após a realização do deploy, é preciso [criar o tópico no cluster Kafka](https://kafka.apache.org/quickstart).

## Testando a aplicação

#### Executando um request CURL para cadastrar uma entrada de recursos na conta
```
curl -X POST -H "Content-Type: application/json" -d @income-transaction.json http://localhost:8080/transactions
```
#### Executando um request CURL para cadastrar uma saída de recursos da conta
```
curl -X POST -H "Content-Type: application/json" -d @expense-transaction.json http://localhost:8080/transactions
```
#### Executando um request CURL para consulta de saldo
```
curl http://localhost:8081/balance\?accountId\=wesley | json_pp
```
#### Executando um teste de performance [K6's](https://k6.io) simples
````
k6 run --vus 10 --duration 60s performance-tests/income.js
k6 run --vus 10 --duration 60s performance-tests/expense.js
````
