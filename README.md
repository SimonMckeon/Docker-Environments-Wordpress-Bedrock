# Wordpress & Bedrock & Docker

A barebones development environment for Wordpress with Bedrock.

## Included containers:
- WordPress with XDebug  
- NGINX  
- MariaDB 10.2  

## Getting Started

**Create a new Bedrock project in a `wordpress` directory.**  

```
composer create-project roots/bedrock wordpress
```

To place the project in a different directory, replace the volumes for `wordpress` and `nginx` in `docker-compose.yml` and replace `wordpress` in the composer command.

**Run the containers**  
```
docker-compose up --build
```

**Create `wordpress/.env`**
```
DB_NAME='wordpress'
DB_USER='wordpress'
DB_PASSWORD='wordpress'

# Optionally, you can use a data source name (DSN)
# When using a DSN, you can remove the DB_NAME, DB_USER, DB_PASSWORD, and DB_HOST variables
# DATABASE_URL='mysql://database_user:database_password@database_host:database_port/database_name'

# Optional database variables
DB_HOST='mysql'
# DB_PREFIX='wp_'

WP_ENV='development'
WP_HOME='http://localhost:8080'
WP_SITEURL="${WP_HOME}/wp"

# Specify optional debug.log path
# WP_DEBUG_LOG='/path/to/debug.log'

# Generate your keys here: https://roots.io/salts.html
AUTH_KEY='generateme'
SECURE_AUTH_KEY='generateme'
LOGGED_IN_KEY='generateme'
NONCE_KEY='generateme'
AUTH_SALT='generateme'
SECURE_AUTH_SALT='generateme'
LOGGED_IN_SALT='generateme'
NONCE_SALT='generateme'
```

**To stop the containers**
```
docker-compose down
```

**To stop the containers and delete volumes. *Warning: deletes all data***
```
docker-compose down --volumes
```

## Import database
Place an SQL file in the root (alongside `docker-compose.yml`) called `db.sql` to automatically import when first running the containers.

## Composer
```
docker-compose exec wordpress composer <command>
```

## WP Cli
```
docker-compose exec wordpress wp --allow-root <command>
```

## XDebug
XDebug is installed in the WordPress container with sensible defaults found in `/docker/wordpress/xdebug.ini`

To get started with VSCode, ensure you have the [PHP Debug](https://marketplace.visualstudio.com/items?itemName=xdebug.php-debug) extension installed, and create the following config for debugging.

```
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for XDebug",
      "type": "php",
      "request": "launch",
      // A custom port for Xdebug
      "port": 9003,

      // server -> local
      "pathMappings": {
        "/var/www/html": "${workspaceRoot}/wordpress"
      }
    }
  ]
}
```