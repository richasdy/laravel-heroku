# laravel-heroku

## Deploying laravel in heroku

This is boilerplate Laravel 5.5

## Configuration
```sh
laravel new laravel-heroku
php artisan migrate

app_name=my-laravel-heroku
heroku apps:create $app_name
heroku addons:create heroku-postgresql:hobby-dev --app $app_name
heroku addons:create heroku-redis:hobby-dev --app $app_name
heroku config:set APP_KEY=$(php artisan --no-ansi key:generate --show)
heroku config:set APP_LOG=errorlog
heroku config:set QUEUE_DRIVER=redis SESSION_DRIVER=redis CACHE_DRIVER=redis
heroku config:set APP_ENV=development APP_DEBUG=true APP_LOG_LEVEL=debug
```

open your heroku dashboard and link this guthub to the project