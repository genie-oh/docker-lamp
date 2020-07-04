# docker-lamp
this is the LAMP environment built using Docker Compose.

## Thanks for
it is origined from [temori1919/docker-lamp](https://github.com/temori1919/docker-lamp)
thanks for your works.

## Consists
- php-fpm(php7.4)
- centos7
- apache2.4
- mariadb
- phpMyadmin
- nodejs (with Gulp)

## Installation

```bash
# clone this repository
https://github.com/genie-oh/docker-lamp.git
cd docker-lamp
# run docker compose
docker-compose up -d
```

## How to use
`code` dir is documentroot.

At first, you need to add below to `hosts` file for access.
```
127.0.0.1 lamplocal.me
127.0.0.1 test.lamplocal.me
```

try to access like below. can access only https.
```sh
https://lamplocal.me
https://test.lamplocal.me
```

## Constitution of documentroot.
if you want to add new subdomain home directory, put your contents to `code/home_sub/{yourapp}/public/`

```
|-- code                          # -- Mounted on /var/www/html
|   |-- config
|   |   `-- default.json.example
|   |-- gulpfile.js
|   |-- home_root                 # -- root home
|   |   `-- public                #    documentroot of lamplocal.jp
|   |       `-- index.html        #    can run only static contents.
|   |-- home_sub                  # -- virtual host home
|   |   `-- test                  #    you can add directory & use by subdomain
|   |       `-- public            #    documentroot of test.lamplocal.jp
|   |           `-- index.php     #    can run php script
|   `-- nodeapp
```

## Constitution about Docker Compose
```
docker-lamp/
|-- db
|   |-- Dockerfile
|   |-- logs            # For slow query log
|   `-- my.cnf          # Copy to /etc/mysql/conf.d
|-- node
|   `-- Dockerfile
|-- php
|   |-- Dockerfile
|   |-- logs            # For xdebug.log
|   `-- xdebug.ini      # Copy to /usr/local/etc/php/conf.d
|-- redis
|   |-- Dockerfile
|   |-- data            # for redis data
|   |-- logs            # for redis log
|   `-- redis.conf      # Copy to /usr/local/etc/redis/redis.conf
`-- web
|   |-- Dockerfile
|   |-- docker.conf     # Copy to /etc/httpd/conf.d
|   `-- logs            # for apache log
└── docker-compose.yml
```

## PHP
this contain below PHP extension
```
zip
imagick
gd
soap
xmlrpc
redis
xdebug
bcmath
pdo_mysql
```

If you want to install more PHP extension or remove,
add or remove it in the Dockerfile.

See more infomation

[PHP official repository](https://hub.docker.com/_/php/)

## phpMyAdmin
phpMyAdmin is [localhost:8080].

## Composer

You can run docker container composer.
```bash
# move your app dir
cd docker-lamp/code/[yourapp]
docker-compose run --rm -v $(pwd):/app composer install
```


If you need specific PHP extensions, 
you can avoid them with the following options.
```bash
docker-compose run --rm -v $(pwd):/app composer install --ignore-platform-reqs --no-scripts
```


See more infomation

[Composer official repository](https://hub.docker.com/_/composer/)

## Gulp
If you want to run PHPUnit with Gulp,
make new default.json from code/config/default.json.example,

then, rewrite like below.
```js
{
	"config": {
      # 'targets' is file of monitored.
  		"targets": [
  			"./your_app/config/**/*.php",
			"./your_app/database/**/*.php"
  		],
      # 'phpunitCmd' is commond path.
  		"phpunitCmd": "./your_app/vendor/bin/phpunit",
      # 'phpunitFile' is path to phpunit.
  		"phpunitFile": "./your_app/phpunit.*"
  	}
}
```

Run gulp.
```bash
docker-compose run -v --rm --no-deps node gulp watch
```

When you stop gulp,
```bash
# find container of "gulp watch".
docker ps

# remove container.
docker kill [container_id]
```