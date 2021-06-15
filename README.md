# lab08_tutorial
[![Build Status](https://travis-ci.com/Ko71k/lab08_tutorial.svg?branch=master)](https://travis-ci.com/Ko71k/lab08_tutorial)

```
export GITHUB_USERNAME=ko71k
```
Рабочая папка:
```
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
Клонирование репозитория:
```
$ git clone https://github.com/${GITHUB_USERNAME}/lab07_tutorial lab08_tutorial
$ cd lab08_tutorial
$ git submodule update --init
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08_tutorial
```
Создание докера у указывание базового образа:
```
$ cat > Dockerfile <<EOF
FROM ubuntu:18.04
EOF
```
Установка через докер
```
$ cat >> Dockerfile <<EOF
RUN apt update
RUN apt install -yy gcc g++ cmake
EOF
```
Копирование файла каталога и выбор рабочей папки докера:
```
$ cat >> Dockerfile <<EOF

COPY . print/
WORKDIR print
EOF
```
cmake в докере:
```
RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install
EOF
```
Установка значения LOG_PATH:
```
$ cat >> Dockerfile <<EOF
ENV LOG_PATH /home/logs/log.txt
EOF
```
Передача докеру директории с файлами:
```
$ cat >> Dockerfile <<EOF
VOLUME /home/logs
EOF
```
Переход к папке:
```
$ cat >> Dockerfile <<EOF
WORKDIR _install/bin
EOF
```
Создание точки входа:
```
$ cat >> Dockerfile <<EOF
ENTRYPOINT ./demo
EOF
```
Command docker not found, значит нужно установаить:
```
sudo apt install docker.io #version 20.10.2-0ubuntu1~20.04.2
```
После этого при вводе docker build -t logger . не даёт разрешения, значит нужно с sudo:
```
sudo docker build -t logger .
```
Обращение к докеру:
```
sudo docker images
```
Создание папки logs и интерактивного процесса logger
```
$ mkdir logs
$ sudo docker run -it -v "$(pwd)/logs/:/home/logs/" logger
text1
text2
text3
<C-D>
```
выход через ^C

Вывод информации о контейнере:
```
sudo docker inspect logger
```
Проверка файлов:
```
cat logs/log.txt
```
```
gsed -i 's/lab07/lab08/g' README.md
```
Изменение файла .travis.yml
```
$ cat >>.travis.yml <<EOF
/lang<CR>o
services:
- docker<ESC>
jVGdo
script:
- docker build -t logger
EOF
```
Загрузка на гитхаб:
```
$ git add Dockerfile
$ git add .travis.yml
$ git commit -m"adding Dockerfile"
$ git push origin master
```
Проверка файла:
```
$ travis enable
$ travis lint
```
После этой строчки вышла ошибка, и нужно было исправить файл .travis.yml, чтобы в нём было только 
```
/lang<CR>o
services:
- docker<ESC>
jVGdo
script:
- docker build -t logger
```
После этого всё заработало.
