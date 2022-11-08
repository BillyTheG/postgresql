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

