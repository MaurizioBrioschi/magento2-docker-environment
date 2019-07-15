Magento 2 docker environment
---
This repo provide a light Docker environment for Magento 2 using Nginx, Php7.2 and Mysql 5.7.26

## First of all
- it is a best practise to not run Docker like sudo user: for this reason we need to build out php container using the 
  user of our machine running `docker-compose build --build-arg UID=$(id -u) --build-arg GID=$(id -g) php-fpm`
- Set the UID and GID in *.env* file, related to our user id and group id of our machine.  
  Get the user id typing `id -u`. Get the group id typing id -g``
  **This will ensure that our php-fpm container will run with our host machine user.**

  
## Set up an existing Magento2 project
- copy your magento2 project into *php/src* folder
- run `docker-compose up -d`
- remove file *.gitignore* from *php/src* folder
- add in your **/etc/hosts** file `127.0.0.1 magento2.local`
- open your browser and go to [magento2.local](http://magento2.local)

## set up a new Magento2 project
- `docker-compose up --build -d`
- remove file *.gitignore* from *php/src* folder
- `
   docker run --rm --interactive --tty 
   --volume ${PWD}/php/src:/app 
   --user $(id -u):$(id -g) 
   composer create-project --ignore-platform-reqs --repository-url=https://repo.magento.com/ magento/project-community-edition ./
   `
- Provide public key and private key for repo.magento.com and wait for composer to create the project 
- add in your **/etc/hosts** file `127.0.0.1 magento2.local`
- open your browser and go to [magento2.local](http://magento2.local/setup)
- Follow the step to install magento2

### load sample data
- `docker-compose exec php-fpm php -dmemory_limit=6G bin/magento sampledata:deploy`
- `docker run --rm --interactive --tty --volume ${PWD}/php/src:/app --user $(id -u):$(id -g) composer update --ignore-platform-reqs`
- `docker-compose exec php-fpm php bin/magento setup:upgrade`

## Cache with redis
Enable cache with redis running on redis database 0

`
docker-compose exec php-fpm php bin/magento setup:config:set --cache-backend=redis --cache-backend-redis-server=redis --cache-backend-redis-db=0
`

Page cache over redis can be enabled, using db 1 with:
`
docker-compose exec php-fpm  php bin/magento setup:config:set --page-cache=redis --page-cache-redis-server=redis --page-cache-redis-db=1
`

## Useful commands
Set environment in Magento 2 like developer
`
docker-compose exec php-fpm php bin/magento deploy:mode:set developer
`