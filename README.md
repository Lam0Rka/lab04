## Laboratory work IV [![Build Status](https://travis-ci.com/Lam0Rka/lab04.svg?branch=main)](https://travis-ci.com/Lam0Rka/lab04)

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```sh
$ open https://travis-ci.org
```

## Tasks

- [x] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [x] 2. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [x] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [x] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [x] 7. Выполнить инструкцию учебного материала
- [x] 8. Составить отчет и отправить ссылку личным сообщением в **Slack**


## Homework

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры на различных платформах:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;
* используйте [AppVeyor](https://www.appveyor.com/) для сборки на операционной системе **Windows**.

ИНСТРУКЦИЯ ПРО ВСТАВКУ ЗНАЧКА:

1. Перейти на https://travis-ci.org/{username or org}/{repository}
2. Нажать на the Status Image (статус значка) или же соответств. окошко всплывет.
3. Копипэйстнуть код для нашего README.md
4. Выполнить такие простые команды как: git add, git commit & git push
5. Обновить GitHub и увидеть значок



1. Cоздание CMakeLists.txt для сборки.
```sh
cmake_minimum_required(VERSION 3.4)
project(formatter)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter STATIC formatter_lib/formatter.cpp)
include_directories(formatter_lib)

add_library(formatter_ex STATIC formatter_ex_lib/formatter_ex.cpp)
target_link_libraries(formatter_ex formatter)
include_directories(formatter_ex_lib)

add_executable(hello_world hello_world_application/hello_world.cpp)
target_link_libraries(hello_world formatter formatter_ex)

add_library(solver_lib STATIC solver_lib/solver.cpp)
include_directories(solver_lib)

add_executable(solver solver_application/equation.cpp)
target_link_libraries(solver formatter formatter_ex solver_lib)
```
2. Заполнение и создание файла .travis.yml
```sh
$ cat > .travis.yml <<EOF
language: cpp

compiler:
- gcc
- clang
os:
 - linux
EOF
```
2. Создание скрипта для того, чтобы было возможным собрать по проекту в каждой директории.
```sh
cmake formatter_lib/CMakeLists.txt -Bformatter_lib/_build -DCMAKE_CURRENT_SOURCE_DIR=/home/bogdan/bogdan1/workspace/projects/lab04
cmake --build formatter_lib/_build
cmake formatter_ex_lib/CMakeLists.txt -Bformatter_ex_lib/_build -DCMAKE_CURRENT_SOURCE_DIR=/home/bogdan/bogdan1/workspace/projects/lab04
cmake --build formatter_ex_lib/_build
cmake hello_world_application/CMakeLists.txt -Bhello_world_application/_build -DCMAKE_CURRENT_SOURCE_DIR=/home/bogdan/bogdan1/workspace/projects/lab04
cmake --build hello_world_application/_build
cmake solver_application/CMakeLists.txt -Bsolver_application/_build -DCMAKE_CURRENT_SOURCE_DIR=/home/bogdan/bogdan1/workspace/projects/lab04
cmake --build solver_application/_build
```
3. Установка пользовательского сценария запуска с помощью CMake. 
```sh
$ cat >> .travis.yml <<EOF
jobs:
  include:
  - name: "all projects"
    script:
    - cmake -H. -B_build
    - cmake --build _build
  - name: "each CMakeLists.txt"
    script:
    - source ./script
EOF
```
4. Установка доп. файлов.
```sh
$ cat >> .travis.yml <<EOF
addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```
5. Проверка .travis.yml 

```sh
$ travis lint
Hooray, .travis.yml looks valid :)
```
УРА УРА УРА! все работает :)




## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2015-2021 The ISC Authors
```
