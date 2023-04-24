PostgresSQL:

docker pull postgres
Criar o diretório de dados: mkdir ${HOME}/database-dados
cd ${HOME}/database-dados
mkdir postgres

Criar um container: sudo docker run -d --name postgres -e POSTGRES_PASSWORD=123456 -v ${HOME}/database-dados/postgres/:/var/lib/postgresql/data -p 5432:5432 postgres

Com isso já pode ser usado no DBeaver com os dados:
localhost:5432
user: postgres
pass: 123456

Você pode entrar no modo interativo:
Dentro do container: sudo docker exec -it postgres bash
Entrando no Postgres: psql -h localhost -U postgres
listando os databases: \l

---

Redis:

docker pull redis
docker run -d -p 6379:6379 -i -t redis

Para entrar no modo interativo: docker exec -it redis sh 
Depois executar: redis-cli 
Alguns comandos:
   - Limpar todas as keys: flushall
   - Retornar todas as keys: KEYS *
   - Retornar o valor de uma key: get
   - Setar o valor de uma key: set "valor"
   - Deletar uma(s) key(s): del key1 key2 key3 https://redis.io/commands/set

---

RabbitMQ:

mkdir -p /database_dados/rabbitmq
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 --hostname rabbitmq -v ~/database_dados/rabbitmq:/var/lib/rabbitmq rabbitmq:3-management

---

MSSQL

docker pull mcr.microsoft.com/mssql/server:2022-latest

docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=A54B#32x' \
--name 'mssql' -p 1433:1433 \
-v database-dados:/var/opt/mssql \
-d mcr.microsoft.com/mssql/server:2022-latest

Se desejar alterar senha do usuário SA:
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '<YourStrong!Passw0rd>' \
   -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"'

Restaurar um backup:
a) Criar a pasta de backup dentro do container:
docker exec -it mssql mkdir /var/opt/mssql/backup
b) Copiar o arquivo de backup para dentro do container
docker cp ~/backup/DES_EDUSOFTAPI_ICO.bak mssql:/var/opt/mssql/backup
c) Listar os nomes de arquivos lógicos e os caminhos dentro do backup: 
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd -S localhost \
   -U SA -P 'A54B#32x' \
   -Q 'RESTORE FILELISTONLY FROM DISK = "/var/opt/mssql/backup/DES_EDUSOFTAPI_ICO.bak"' \
   | tr -s ' ' | cut -d ' ' -f 1-2
d) Restaurando:
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P 'A54B#32x' \
   -Q 'RESTORE DATABASE DES_EDUSOFTAPI_ICO 
   FROM DISK = "/var/opt/mssql/backup/DES_EDUSOFTAPI_ICO.bak" 
   WITH 
   MOVE "ace_ico" TO "/var/opt/mssql/data/DES_EDUSOFTAPI_ICO.mdf", 
   MOVE "ace_ico_log" TO "/var/opt/mssql/data/DES_EDUSOFTAPI_ICO.ldf"'

Realizar um backup:
sudo docker exec -it mssql /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA -P 'A54B#32x' \
-Q "BACKUP DATABASE DES_EDUSOFTAPI_ICO TO DISK = N'/var/opt/mssql/backup/DES_EDUSOFTAPI_ICO_2.bak' WITH NOFORMAT, NOINIT, NAME = 'DES_EDUSOFTAPI_ICO-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"

---

Mongo:

docker pull mongo
docker run -d --name mongodb -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=123456 mongo
Connection String: mongodb://admin:123456@localhost:27017/admin 
Para uma ide client: https://www.mongodb.com/try/download/compass
Depois de baixado o arquivo .deb, basta instalar como o comando: sudo apt install ./mongodb-compass_1.36.3_amd64.deb

---

RabbitMQ

Criar uma pasta para armazenar os dados: 
mkdir -p /database-dados/rabbitmq


docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 --hostname rabbitmq -v ~/database-dados/rabbitmq:/var/lib/rabbitmq rabbitmq:3-management

Para acessar a interface: http://localhost:15672/ Usuário/Senha: guest

---

Zookeeper + Kafka + Confluent Center

Para subir:
docker compose -f docker_compose_files/docker_compose_kafka.yaml up

Para parar:
docker compose -f docker_compose_files/docker_compose_kafka.yaml down


Acesse http://localhost:9021 para manipular os tópicos do Kafka via Confluent Center

---

Nginx + Php + Mysql

Para subir:
cd php
docker compose -f docker_compose_php.yaml up –d
docker exec -it php-fpm docker-php-ext-install pdo pdo_mysql 
docker compose -f docker_compose_php.yaml restart
Depois, vamos criar um host para nosso site: sudo nano /etc/hosts 
Adicione no arquivo: 127.0.0.1 site.localhost
Para acessar: http://site.localhost/index.php

Para parar: 
docker compose -f docker_compose_php.yaml down 

Documentação: https://dev.to/jrnunes1993/como-criar-containers-com-php-mysql-e-nginx-utilizando-o-docker-compose-964