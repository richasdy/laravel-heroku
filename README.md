# laravel-heroku

## Deploying laravel in heroku

This is boilerplate Laravel 5.5

## Configuration
### laravel configuration
```sh
laravel new laravel-heroku
composer require predis/predis
php artisan migrate
chmod -fr 777 storage/logs
chmod -fr 777 storage/framework/sessions
chmod -fr 777 storage/framework/views
```

### heroku configuration
```sh
app_name=my-laravel-heroku
heroku apps:create $app_name
heroku addons:create heroku-postgresql:hobby-dev --app $app_name
heroku addons:create heroku-redis:hobby-dev --app $app_name
heroku config:set APP_KEY=$(php artisan --no-ansi key:generate --show) --app $app_name
heroku config:set APP_LOG=errorlog --app $app_name
heroku config:set QUEUE_DRIVER=redis SESSION_DRIVER=redis CACHE_DRIVER=redis --app $app_name
heroku config:set APP_ENV=development APP_DEBUG=true APP_LOG_LEVEL=debug --app $app_name
```

### heroku configuration
open your heroku app in deploy tab, connect github in deployment method ???????? 
```sh
heroku config:set BUILDPACK_URL=https://github.com/richasdy/laravel-heroku.git
```

### additional laravel configuration
add code below in 
```sh
config/database.php
```
```php
if (getenv('REDIS_URL')) {
    $redis_url = parse_url(getenv('REDIS_URL'));

    putenv('REDIS_HOST='.$redis_url['host']);
    putenv('REDIS_PORT='.$redis_url['port']);
    putenv('REDIS_PASSWORD='.$redis_url['pass']);
}

if (getenv('DATABASE_URL')) {
    $db_url = parse_url(getenv('DATABASE_URL'));

    putenv('DB_HOST='.$db_url['host']);
    putenv('DB_PORT='.$db_url['port']);
    putenv('DB_DATABASE='.substr($db_url['path'], 1));
    putenv('DB_USERNAME='.$db_url['user']);
    putenv('DB_PASSWORD='.$db_url['pass']);
}
```