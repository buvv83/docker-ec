# docker-ec
Docker for symfony nginx+mysql+composer+php-fpm+rabit
=====================================================

Настройки для конфигурации докера создаются в .env для этого переименуйте файл .env.dist в .env и создайте папки из конфигурации
PATH_TO_PROJECT - переименуйте в папку вашего проекта

```
DB_PATH_HOST=~/www/PATH_TO_PROJECT/mysql # Путь до хранения файлов БД в вашем проекте

APP_PATH_HOST=~/www/PATH_TO_PROJECT/www # Путь до файлов в вашем проекте

CONTAINER_PATH_HOST=/var/www # Путь до файлов в контейнере

APP_PATH_NGINX_CONFIG=~/www/PATH_TO_PROJECT/nginx/core # Путь до файлов с настройками nginx в вашем проекте

APP_PATH_NGINX_LOG=~/www/PATH_TO_PROJECT/nginx/log # Путь до логов в вашем проекте

#MYSQL

MYSQL_PASSWORD=root # Пароль до БД

#PORT

APP_PORT=80 # Порт по которому будет доступен ваш сайт

CONTAINER_PORT=80 # Порт с которого прокидываем

APP_IP=127.0.0.5 # IP который будет доступен для просмотра сайта
```

После запуска в файле /etc/hosts добавляем строчку

```
127.0.0.5       NAME_OF_SITE.loc
```


### Запуск контейнеров
Первый запуск 

Соберет и запустит ваш билд
```
docker-compose up --build
```

В последующем достаточно запускать только
```
docker-compose up
```

### Перзапуск сервисов
```
docker-compose restart NAME_OF_SERVICE
```

### Зайти в сервис
```
docker-compose exec NAME_OF_SERVICE bash
```

### Удалить не нужные контейнеры
```
docker system prune
```

### Пример конфигурации NGINX
```
server {
    listen 80;
    server_name localhost;

    set  $root_path     '/var/www/public';
    root $root_path;

    index index.php index.html index.htm;
    
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;

    location / {
        # try to serve file directly, fallback to index.php
        try_files $uri /index.php$is_args$args;
    }

    location ~ \.php$ {
	    try_files $uri =404;
	    fastcgi_split_path_info  (.+?\.php)(/.*)$;
	    fastcgi_pass php:9000;
	    fastcgi_index index.php;
	    include fastcgi_params;
	    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	    fastcgi_param PATH_INFO $fastcgi_path_info;
    }

    # return 404 for all other php files not matching the front controller
    # this prevents access to other php files you don't want to be accessible.
    location ~ \.php$ {
        return 404;
    }
}

```
