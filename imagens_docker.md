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

---

Redis:

docker pull redis
docker run -d -p 6379:6379 -i -t redis

----

RabbitMQ:

mkdir -p /database_dados/rabbitmq
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 --hostname rabbitmq -v ~/database_dados/rabbitmq:/var/lib/rabbitmq rabbitmq:3-management

----

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