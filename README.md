# docker-ec
Docker for symfony nginx+mysql+composer+php-fpm
===============================================

Настройки для конфигурации докера создаются в .env для этого переименуйте файл .env.dist в .env и создайте папки из конфигурации
PATH_TO_PROJECT - переименуйте в папку вашего проекта

```
DB_PATH_HOST=~/www/PATH_TO_PROJECT/mysql # Путь до хранения файлов БД в вашем проекте

APP_PATH_HOST=~/www/PATH_TO_PROJECT/www # Путь до файлов в вашем проекте
CONTAINER_PATH_HOST=/var/www # Путь до файлов в контейнере

APP_PATH_NGINX_CONFIG=~/www/PATH_TO_PROJECT/nginx/core # Путь до файлов с настройками nginx в вашем проекте
CONTAINER_PATH_NGINX_CONFIG=/etc/nginx/conf.d # Путь до файлов nginx в контейнере

APP_PATH_NGINX_LOG=~/www/PATH_TO_PROJECT/nginx/log # Путь до логов в вашем проекте
CONTAINER_PATH_NGINX_LOG=/var/log/nginx/ # Путь до логов в контейнере
```

#MYSQL
MYSQL_PASSWORD=root # Пароль до БД

#PORT
APP_PORT=8080 # Порт по которому будет доступен ваш сайт
CONTAINER_PORT=80 # Порт с которого прокидываем


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

### Пример конфигурации NGINX
```
server {
    listen 80;
    server_name localhost;

    set  $root_path     '/var/www';
    root $root_path;

    index index.php index.html index.htm;
    
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;

    location ~ \.php$ {
	    try_files $uri =404;
	    fastcgi_split_path_info  (.+?\.php)(/.*)$;
	    fastcgi_pass php:9000;
	    fastcgi_index index.php;
	    include fastcgi_params;
	    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	    fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```
