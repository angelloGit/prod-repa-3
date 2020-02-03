
# Git репозиторий автообновлением

При коммите в ветку мастер содержимое ветки мастер должно обновлять папку /var/www

# Expected result:
- Ваш ubuntu сервер(*)
- Git репозиторий на сервере(*)
- Публичный репозиторий
- Склоненный с репозитория [https://github.com/MaksymSemenykhin/git-course-task/tree/task%233](https://github.com/MaksymSemenykhin/git-course-task/tree/task%233)
- Процедура будет работать с прод репой.
- Не использовать облачные git сервисы.
- Не использовать облачные teamcity, jenkins... сервисы.
- В README файле описано детали настройки и flow с этим репозиторием.
- Репозиторий может содержать доп скрипты гит репозитория.
- Доступ на сервер для ключа task3.pub без пароля.
```console
создал каталог для 3-й таски /var/www/3  
сам сайт - taska3.echo.dp.ua  и его рут - /var/www/taska3 -  симлинк на /var/www/3
смысл - подмена каталога на время chekout 

репа лежит у пользователя team  в каталоге ~/deploy/taska3.git
создал git init --bare taska3.git
потом post-receive 

#!/bin/sh

GIT_DIR=/home/teamcity/deploy/taska3.git/
GIT=/usr/bin/git

while read oldrev newrev ref
do
    case $ref in.
<------>refs/heads/master)
<------>    rm /var/www/taska3
<------>    ln -s /var/www/under_construction /var/www/taska3
<------>    echo "got update $newrev for $ref, deploying"
<------>    $GIT --work-tree=/var/www/3 --git-dir=$GIT_DIR checkout --force master
<------>    rm /var/www/taska3
<------>    ln -s /var/www/3 /var/www/taska3
<------>;;
<------>*)
<------>    echo "got update $newrev for $ref, exiting"
<------>;;
    esac
done
как тестил 
у себя в домашнике в репе 
git remote add deploy team@127.0.0.1:deploy/taska3.git
mcedit index.php ......
git add index.php && git commit -m 'test with nginx) ' && git push deploy

copy past c  консоли
angello@sandbox:~/work/prod-repa-3$ 
git add index.php && git commit -m 'test with nginx) ' && git push deploy
[master bb9063e] test with nginx)
 1 file changed, 1 insertion(+), 1 deletion(-)
Counting objects: 3, done.
Writing objects: 100% (3/3), 286 bytes | 143.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: got update bb9063ec7f60494686e3dcb2948c5696830b4e1b for refs/heads/master, deploying
remote: Already on 'master'
To 127.0.0.1:deploy/taska3.git
   cbdc337..bb9063e  master -> master

писал єто все дольше чем делал ))) 
```
