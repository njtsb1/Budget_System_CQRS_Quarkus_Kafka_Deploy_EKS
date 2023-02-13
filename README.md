# Daily Learning

## Creating a Budget System, Using CQRS, Quarkus, Kafka and Deploy in EKS

Project developed at Bootcamp Inter Java Developer with instruction from specialist [Wesley Fuchter](https://github.com/wesleyfuchter "Wesley Fuchter").</br>
Deploying an application written in Java/Kotlin on Amazon's Elastic Kubernetes Service. The application is an example of the CQRS pattern that includes two Quarkus services that communicate through an asynchronous bus using Kafka. Learning how to create Kubernetes manifests for deployment on EKS and what settings are needed to have the environment running in production.

[LICENSE](./LICENSE)

See [original repository](https://github.com/wesleyfuchter/cqrs-quarkus-kafka)

![Design](/images/design.png)

## Deploying the external services

  --docker-compose up -d --build

It will deploy four docker containers on your environment with MongoDB, PostgreSQL, Kafka and Zookepper (required by Kafka)

After deploying Kafka, you'll need to [create the topic on the Kafka cluster](https://kafka.apache.org/quickstart). For example:

docker exec -it bankaccount-kafka \
  ./bin/kafka-topics.sh --create \
  --topic transactions \
  --zookeeper bankaccount-zookeeper:2181 \
  --replication-factor 1 \
  --partitions 1

## Testing the application

### Running a CURL request to create a income transaction

  --curl -X POST -H "Content-Type: application/json" -d @income-transaction.json [localhost:8080/transactions](http://localhost:8080/transactions)

#### Running a CURL request to create a expense transaction

  --curl -X POST -H "Content-Type: application/json" -d @expense-transaction.json [http://localhost:8080/transactions](http://localhost:8080/transactions)

#### Running CURL request to fetch the balance

  --curl [http://localhost:8081/balance\?accountId\=nivaldo](http://localhost:8081/balance\?accountId\=nivaldo) | json_pp

#### Running [K6's](https://k6.io) simple performance test

  --k6 run --vus 10 --duration 60s performance-tests/income.js
  --k6 run --vus 10 --duration 60s performance-tests/expense.js
