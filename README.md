Magento 2 docker environment
---
This repo provide a light Docker environment for Magento 2 using Nginx, Php7.2 and Mysql 5.7.26

## Set up an existing Magento2 project
- copy your magento2 project into **php/src* folder
- run `docker-compose up -d`
- add in your **/etc/hosts** file `127.0.0.1 magento2.local`
- open your browser and go to [magento2.local](http://magento2.local)

## set up a new Magento2 project
- `docker-compose up --build -d`
- `
   docker run --rm --interactive --tty  \ 
   --volume ${PWD}/php/src:/app \
   --user $(id -u):$(id -g) \
   composer create-project --ignore-platform-reqs --repository-url=https://repo.magento.com/ magento/project-community-edition ./
   `
- Provide public key and private key for repo.magento.com and wait for composer to create the project 
- add in your **/etc/hosts** file `127.0.0.1 magento2.local`
- open your browser and go to [magento2.local](http://magento2.local/setup)
- Follow the step to install magento2
