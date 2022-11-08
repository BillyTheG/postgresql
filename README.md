# postgresql
Dieses Repo konfiguriert die Postgres-Datenbank in einem Dockercontainer

## Installation

Damit zwischen Container eine Kommunikation stattfinden kann, muss ein IP4-Forwarding ermöglicht werden.
```shell
sudo  sysctl net.ipv4.conf.all.forwarding=1
sudo iptables -P FORWARD ACCEPT
```
Ein Netzwerk, in welchem der Webserver sich auch befinden muss
```shell
docker network create --driver bridge apps
```

Danach in den /docker Ordner wechseln und das Image bauen und taggen.
```shell
docker build -f Dockerfile -t postgres .
```

Mit dem run-Befehl kann der Service in das richtige Netzwerk übertragen werden.
```shell
docker run -dit  --name postgres --network apps -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres:latest 
```

## Usage
in Jupyterlab kann die Einbindung mit:

```python
from sqlalchemy import create_engine
 
engine = create_engine('postgresql://postgres:postgres@localhost:5432/fake')

engine.execute("INSERT INTO toastbrot VALUES('2020-12-12','asdfada',True) ;")

engine.execute('SELECT text FROM toastbrot;')

```

oder durch
````python
import psycopg2

connection = psycopg2.connect(user = "postgres",  password = "postgres",                     host = "127.0.0.1",
                              port = "5432",
                              database = "fake") 

cursor = connection.cursor()                                


cursor.execute("Select * from news ;") 

```

geschehen.
