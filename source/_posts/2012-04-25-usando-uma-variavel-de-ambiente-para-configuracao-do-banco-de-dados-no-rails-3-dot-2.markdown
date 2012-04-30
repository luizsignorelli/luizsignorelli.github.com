---
layout: post
title: "Usando uma variável de ambiente para configuração do banco de dados no Rails 3.2"
date: 2012-04-25 07:25
comments: true
published: true
categories: 
---

Já faz algum tempo que vi o [Heroku](http://heroku.com) utilizar uma variável de ambiente para fazer a configuração de banco de dados da sua aplicação.
Eles utilizam uma variável chamada `DATABASE_URL`, e o conteúdo dessa variável deve ser uma string de conexão com o banco, por exemplo: `postgres://username:password@host:port/database_name`.

A partir do Rails 3.2, você consegue configurar sua conexão com o banco dessa maneira também. Suponha que seu `database.yml` está assim:

```yaml
development:
  adapter: mysql2
  encoding: utf8
  reconnect: false
  database: my_db_development
  pool: 5
  username: root
  password: root_password
  host: localhost
```

Se você definir a variável de ambiente `DATABASE_URL` com uma string de conexão válida, o rails vai ignorar o arquivo `database.yml` e usar a string de conexão definida na variável. Para o exemplo acima, a string equivalente seria: `mysql2://root:root_password@localhost:3306/my_db_development`.

O principal benefício dessa abordagem é isolar a configuração do banco, assim cada desenvolvedor define a variável `DATABASE_URL` como preferir. Outra maneira de se atingir isso é ignorar o arquivo `database.yml` do seu repositório git, assim cada desenvolvedor deve criar o seu com suas configurações locais.



##Como descobri isso da pior maneira possível)

Há alguns meses eu estava desenvolvendo uma aplicação **Java** usando o [Play! Framwork](http://www.playframework.org/) e como o **Play** suporta esse tipo de configuração usando variáveis de ambiente, a equipe resolveu usar para a cofiguração de banco de dados.

 A nossa variável também se chamva `DATABASE_URL`, pois tinhamos visto essa configuração no Heroku e acabamos usando o mesmo nome. Como estavamos usando `mysql`, a nossa string era mais ou menos assim: `mysql://root@localhost:3306/my_database`.

Para não precisar ficar definindo essa variável toda hora, adicionei o comando para exportar no meu arquivo `.bash_profile`. O projeto acabou, mas eu esqueci de remover essa linha do meu `.bash_profile`, logo a variável ainda estava sendo definida.

Meses depois, o **Rails 3.2** é lançado, e fui criar uma aplicação usando essa nova versão. Toda vez que eu tentava usar a aplicação rails, no console ou subindo o servidor, o seguinte erro era lançado:
 
```
Please install the mysql adapter: `gem install activerecord-mysql-adapter` (mysql is not part of the bundle. Add it to Gemfile.) (LoadError)    
```
Detalhe: minha aplicação rails não usava `mysql` e sim `sqlite3`, e mesmo assim ele me dava esse erro pedindo o `activerecord-mysql-adapter`.

Depois de pesquisar na iternet e tentar resolver o problema, sem sucesso, por uma semana, resolvi debugar o rails para ver o que e diabos ele estava tentando fazer. Foi aí que descobri essa nova *feature*. O rails estava pegando a configuração do meu projeto **Play**, e por isso o erro. Foi só remover a variavel de ambiente que tudo voltou a funcionar normalmente.


A única menção que encontrei dessa nova *feature* do rails foi no último parágrafo 
[desse](http://rubypond.com/blog/activerecord-url-connections) blog.