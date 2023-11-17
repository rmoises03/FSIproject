# SEED Labs - SQL Injection

## Setup

Inicialmente adicionamos uma nova entrada nos hosts conhecidos pela máquina virtual, executando de seguida os containers fornecidos no lab e abrindo uma shell com acesso direto à base de dados. Utilizamos os seguintes comandos.

`sudo nano etc/hosts`:10.9.0.5 www.seed-server.com

`dcbuild`:docker-compose build

`dcup`:docker-compose up

`docksh f3 `:Abrir uma shell do MySQL container

`mysql -u root -pdees`:Executar o MySQL com utilizador root

`use sqllab_users;`:Selecionar o schema pretendido

## Task1
