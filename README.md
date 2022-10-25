# Цель проекта:
1. автоматическая установка VM с помощью terraform
2. автоматическая установка Wordpress на ранее созданную VM с помощью ansible

## Порядок запуска
для работы с terraform необходимо перейти ~/Projects/homework/terraform и выполнить команду:
> terraform apply
В lamp/env/dev/inventory перечислены настройки подключения к VM, перед запуском плейбука необходимо указать <IP-адрес> подключения в ansible_host=
для запуск плейбука ansbile необхоимо перейти ~/Projects/homework/lamp и выполнить команду:
> ansible-playbook -i env/dev/inventory site.yml
Заходим на сайт http://<IP-адрес> в браузере, производим первичную настройку wordpress:

## /keys
Тут храним .pub ключи для terraform и ansible

## 1. /terraform ~ автоматическая установка VM с помощью terraform
Создание ВМ в облаке VKcloud
### Создание VM:
> terraform plan
> terraform apply
### Удаление VM:
> terraform destroy

## 2. /lamp ~ автоматическая установка Wordpress на ранее созданную VM с помощью ansible
### В проекте есть три основных плейбука: 
#### wait Ubuntu обновляется после создания VM до 20 минут, поэтому ждём завершения, пьём кофе.
роли:
    wait: Ubuntu обновляется после создания VM до 20 минут, поэтому ждём завершения, пьём кофе.
#### db - установка базы mysql
роли: 
    mysql:  1. apt-install mysql-server mysql-client python3-mysqldb
            2. установка root_password для mysql
#### front - установка фронта (nginx + php7.2-fpm)
роли:
    php: apt-install php7.2 php7.2-cli php7.2-fpm php7.2-json php7.2-pdo php7.2-mysql php7.2-zip php7.2-gd php7.2-mbstring php7.2-curl php7.2-xml php-pear
    nginx   1 . apt-install nginx 
            2. отключаем дефолтный сайт
#### app - установка самого wordpress, его настройка
роли:
    wordpress:
        1. create bd,users in mysql
        2. create nginx config (from templates) + activate for wordpress
        3. download latest wordpress from http://wordpress.org/latest.tar.gz
        4. first config wordpress with: wp-config.php

### В проекте есть три группы хостов: 
Разделение формальное, проект рассчитан на установку всех трёх плейбуков на один и тот же хост.
    db ~ куда ставим базу, 
    front ~ куда ставим фронт, 
    app ~ куда ставим сам сайт.
