#  PROJETO LICENSES Laravel 13 + Docker + PostgreSQL 

Este projeto é sistema feito em Laravel 13, Docker, PostgreSQL e Livewire 

- PHP 8.4
- Laravel 13
- PostgreSQL 16
- Nginx (alpine)
- Docker Compose

A estrutura foi criada para fornecer um backend moderno.

---

## 📁 Estrutura de Pastas

```
meu_backend/
├─ docker-compose.yml
├─ docker/
│  ├─ php/
│  │  └─ Dockerfile
│  └─ nginx/
│     └─ default.conf
└─ backend/  
```

---

## 📃 Exemplo .ENV 

```sh
APP_NAME=NomeDaAplicacao
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost:8080

# Ajustado para português do Brasil
APP_LOCALE=pt_BR
APP_FALLBACK_LOCALE=en
APP_FAKER_LOCALE=pt_BR

APP_MAINTENANCE_DRIVER=file
# APP_MAINTENANCE_STORE=database

# PHP_CLI_SERVER_WORKERS=4

BCRYPT_ROUNDS=12

LOG_CHANNEL=stack
LOG_STACK=single
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

# Configurações de Banco de Dados
DB_CONNECTION=pgsql
DB_HOST=db
DB_PORT=5432
DB_DATABASE=nome_do_banco
DB_USERNAME=usuario_do_banco
DB_PASSWORD=senha_do_banco

SESSION_DRIVER=database
SESSION_LIFETIME=120
SESSION_ENCRYPT=false
SESSION_PATH=/
SESSION_DOMAIN=null

BROADCAST_CONNECTION=log
FILESYSTEM_DISK=local
QUEUE_CONNECTION=database

CACHE_STORE=database
# CACHE_PREFIX=

MEMCACHED_HOST=127.0.0.1

REDIS_CLIENT=phpredis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=log
MAIL_SCHEME=null
MAIL_HOST=127.0.0.1
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

VITE_APP_NAME="${APP_NAME}"
```

---

## 🐳 Subir o ambiente com Docker

Na raiz do projeto, execute:

```sh
docker compose up -d --build
```

Antes de continuar, apague tudo que estiver dentro da pasta backend e configure o .env no mesmo nível do .yml.

Após subir, instale o Laravel dentro do container (verifique antes se a pasta backend está vazia. se não estiver apague o que estiver dentro dela):

🟩 versão mais atual:
```sh
docker compose exec app composer create-project laravel/laravel .
```
🟩 versão especifica:
```sh
docker compose exec app composer create-project laravel/laravel . "^13.0"
```

Aparentemente na nova versão do Laravel (v13[2026]) ja está gerando key e fazendo a migration logo na instalação
e como nesse projeto no docker configuramos o volume - ./.env:/var/www/html/.env lá no docker-compose.yml
o Laravel "pensa" que está escrevendo no .env interno dele

mas qualquer coisa fariamos o seguinte:

Gere a key:

```sh
docker compose exec app php artisan key:generate
```

Rode as migrations:

```sh
docker compose exec app php artisan migrate
```

---

## Testar 
```sh
http://localhost:8080
```


## EXTRA) depois clonar do github


🟩 ETAPA 1 — Criar o arquivo .env

```
copy .env.example .env
```

🟩 ETAPA 2 — Editar o .env (versão mínimo necessário)

```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=pgsql
DB_HOST=db
DB_PORT=5432
DB_DATABASE=nome_do_banco_teste
DB_USERNAME=usuario_do_banco_teste
DB_PASSWORD=senha_do_banco_teste

```

🟩 ETAPA 3 — Subir Docker

```
docker compose up -d --build
```

🟩 ETAPA 4 — Instalar dependências
```
docker compose exec app composer install
```

🟩 ETAPA 5 — Gerar chave
```
docker compose exec app php artisan key:generate
```

🟩 ETAPA 6 — Migrar
```
docker compose exec app php artisan migrate
```
