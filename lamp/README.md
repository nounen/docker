docker pull nginx:1.17 
docker pull php:7.3-fpm 
docker pull mysql:5.7
docker pull redis:5

docker run --name lamp-nginx \
	-p 80:80 \
	-v $PWD/www:/usr/share/nginx/html \
	-v $PWD/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
	-v $PWD/nginx/logs:/var/log/nginx \
	-v $PWD/nginx/conf.d:/etc/nginx/conf.d \
	-v /etc/localtime:/etc/localtime \
	-d nginx:1.17 

docker run --name lamp-php \
	-v $PWD/www:/var/www/html \
	-v $PWD/php:/usr/local/etc/php \
	-v $PWD/php/php-fpm.conf：/usr/local/etc/php-fpm.conf \
	-v /etc/localtime:/etc/localtime \
	-d php:7.3-fpm 

docker run --name lamp-mysql \
	-p 3306:3306 \
	-v $PWD/mysql/conf.d:/etc/mysql/conf.d \
	-v $PWD/mysql/logs:/var/log/mysql \
	-v $PWD/mysql/data:/var/lib/mysql \
	-v /etc/localtime:/etc/localtime \
	-e MYSQL_ROOT_PASSWORD=mysql111111 \
	-d mysql:5.7 \
	--character-set-server=utf8mb4 \
	--collation-server=utf8mb4_unicode_ci

docker run --name lamp-redis \
	-p 6379:6379 \
	-v $PWD/redis/redis.conf:/usr/local/etc/redis/redis.conf \
	-v $PWD/redis/data:/data \
	-v /etc/localtime:/etc/localtime \
	-d redis:5 \
	redis-server /usr/local/etc/redis/redis.conf --appendonly yes
