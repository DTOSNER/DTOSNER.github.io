---
layout: post
title: "Migrate Postgres database from Heroku to localhost"
date: 2022-04-22 19:00:00 -0000
categories: [Heroku]
tags: [rails, postgres, heroku, wsl2]
---

This topic covers how to transfer data from the Postgres database of you application hosted on Heroku to your local machine.

## Just commands
```console
# WSL2
cd rails/project/repo/path
sudo service postgresql restart
rails db:drop
rails db:drop DISABLE_DATABASE_ENVIRONMENT_CHECK=1
rails db:create
rails db:migrate
pg_restore --verbose --clean --no-acl --no-owner -h localhost -U user -d database_name '/location/of/downloaded/db/backup'
```

---
## Step by step

### Create backup on heroku
- Login to heroku
- Select project
- Go to resources
- Select Heroku Postgres Add-on
- Durability tab
- Create Manual Backup
- Wait

### Download database backup
- Click Download at newly created backup

### Delete content of actual database for project

#### Navigate to project
```console
# WSL2
cd rails/project/repo/path
```

#### Delete old database

> Following commands delete all data at actual database on localhost
{: .prompt-danger }

```console
# WSL2
sudo service postgresql restart
rails db:drop
rails db:drop DISABLE_DATABASE_ENVIRONMENT_CHECK=1
```

### Create empty database
```console
# WSL2
rails db:create
rails db:migrate
```

### Restore data from downloaded backup
```console
# WSL2
pg_restore --verbose --clean --no-acl --no-owner -h localhost -U user -d database_name '/location/of/downloaded/db/backup'
```



