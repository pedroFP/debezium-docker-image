# Run

```
$ docker compose up --build
```

# Add a new connection
You can use the Debezium UI web app on the port 8080 (not very good app honestly :/)

![debezium-ui](readme-files/debezium_ui.png)

I prefer making the connection request manually

>By default the connection starts on `snapshot.mode` => `inital`
>
> `initial`: Takes a snapshot of the structure and data of captured tables. Useful if topics have to be populated with a complete representation of the data from the captured tables.



```bash
curl --location --request POST 'localhost:8083/connectors/' --header 'Accept: application/json' --header 'Content-Type: application/json' --data-raw '{
    "name": "your-debezium-connection-name",
    "config": {
        "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
        "tasks.max": "1",

        "database.hostname":    "your-hostname",
        "database.port":        "5432",
        "database.user":        "user",                
        "database.password":    "password",
        "database.dbname":      "dbname",
        "database.server.name": "localhost",            

        "plugin.name": "pgoutput",
        "topic.prefix": "your-kafka-topic-prefix",
        "snapshot.mode": "no_data",
        "schema.include.list": "public",

        "transforms": "unwrap",
        "transforms.unwrap.type": "io.debezium.transforms.ExtractNewRecordState",
        "transforms.unwrap.drop.tombstones": "true",
        "transforms.unwrap.delete.handling.mode": "drop"
    }
}'


```

# Images

```
REPOSITORY                  TAG      SIZE
postgres                    16       451MB
docker-smaia-vpn            latest   16.5MB
debezium/connect            2.6      1.35GB
debezium/debezium-ui        latest   467MB
confluentinc/cp-kafka       7.5.0    849MB
confluentinc/cp-zookeeper   7.5.0    849MB
```
