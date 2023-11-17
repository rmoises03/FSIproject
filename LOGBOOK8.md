# SEED Labs - SQL Injection

## Setup

Inicialmente adicionamos uma nova entrada nos hosts conhecidos pela máquina virtual, executando de seguida os containers fornecidos no lab e abrindo uma shell com acesso direto à base de dados. Utilizamos os seguintes comandos.

`sudo nano etc/hosts`:10.9.0.5 www.seed-server.com

`dcbuild`:docker-compose build

`dcup`:docker-compose up

## Task1

Começamos por correr o comando  `docker exec -it mysql-10.9.0.6 bash`, para abrir uma shell no container do MySQL. Em seguida usamos `mysql -u root -pdees` para login na base de dados. Para mostrar todas as tabelas dentro da mesma, usamos os seguintes comandos

```sql
use sqllab_users;
show tables;
```

Finalmente para obtermos os dados da Alice corremos `SELECT * FROM credential WHERE name='Alice';`

Resultando em
![Add screenshot 1 here]()

## Task2




