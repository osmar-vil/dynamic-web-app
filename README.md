# Dynamic Web App

Aplicação web em PHP puro, com roteador simples, controllers e views, usando PDO para acessar o banco de dados.

## Requisitos

- PHP 8+ com as extensões `pdo_mysql` e `pdo_pgsql`
- Docker com o plugin Compose (`docker compose`)

## Configuração

1. Clone o repositório:

   ```bash
   git clone https://github.com/osmar-vil/dynamic-web-app.git
   cd dynamic-web-app
   ```

2. Crie o arquivo `.env` a partir do modelo e preencha as senhas:

   ```bash
   cp .env.example .env
   ```

   | Variável | Padrão | Descrição |
   |---|---|---|
   | `MYSQL_ROOT_PASSWORD` | — | Senha do usuário `root` do MySQL (obrigatória) |
   | `MYSQL_DATABASE` | `myapp` | Banco criado na primeira execução |
   | `MYSQL_PORT` | `3306` | Porta do MySQL na sua máquina |
   | `POSTGRES_USER` | `postgres` | Usuário do PostgreSQL |
   | `POSTGRES_PASSWORD` | — | Senha do PostgreSQL (obrigatória) |
   | `POSTGRES_DB` | `myapp` | Banco criado na primeira execução |
   | `POSTGRES_PORT` | `5432` | Porta do PostgreSQL na sua máquina |

3. Suba os bancos de dados:

   ```bash
   docker compose up -d
   ```

## Executando a aplicação

Use o servidor embutido do PHP:

```bash
php -S localhost:8000
```

E acesse <http://localhost:8000>.

## Conexão com o banco

Use `127.0.0.1` em vez de `localhost` no DSN. Com `localhost`, o PDO tenta conectar por um socket Unix local, que não existe quando o banco roda no Docker, e retorna o erro `SQLSTATE[HY000] [2002] No such file or directory`.

```php
// MySQL
$dsn = "mysql:host=127.0.0.1;port=3306;dbname=myapp;charset=utf8mb4";

// PostgreSQL
$dsn = "pgsql:host=127.0.0.1;port=5432;dbname=myapp";
```

## Comandos do Docker

```bash
docker compose up -d     # sobe os bancos
docker compose ps        # mostra o status
docker compose down      # para os bancos (os dados continuam salvos)
docker compose down -v   # para os bancos e APAGA os dados
```

As senhas do `.env` só são aplicadas na primeira vez que os dados são criados. Para trocar uma senha depois, rode `docker compose down -v` e suba de novo, ou altere a senha direto no banco.

## Estrutura

```
.
├── controllers/      # Lógica de cada página
├── views/            # Templates HTML
│   └── partial/      # Partes reutilizáveis (header, nav, footer)
├── functions.php     # Funções auxiliares (ex.: dd)
├── router.php        # Mapeia as URLs para os controllers
├── index.php         # Ponto de entrada
├── compose.yaml      # Serviços MySQL e PostgreSQL
└── .env.example      # Modelo das variáveis de ambiente
```

## Rotas

| URL | Controller |
|---|---|
| `/` | `controllers/index.php` |
| `/about` | `controllers/about.php` |
| `/contact` | `controllers/contact.php` |

Qualquer outra URL retorna a página 404.
