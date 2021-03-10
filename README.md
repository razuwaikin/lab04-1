# Part 1
> Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.

Задача простейшая
* Скачиваем сам cmake
* Создаем CMakeLists.txt
```
cmake_minimum_required(VERSION 2.8) 

add_library(formatter STATIC formatter.h formatter.cpp)
```
* Подключаем их друг к другу:
```
cmake ~/workspace/projects/lab03/formatter_lib/
```
* Все на выходе мы получили файл библиотеки formatter.a

# Part 2

> У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeList.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeList.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter.

Тут тоже ничего сложного
* Копируем папку с проектом formatter_lib в formatter_ex_lib
* Cоздаем и подключаем CMakeLists.txt, как было описано в предыдущей части
```
cmake_minimum_required(VERSION 2.8)
project(formatter_ex)
include_directories(formatter_lib) #Подключаем директорию с заголовочными файлами
add_subdirectory(formatter_lib) Подключаем директорию с библиотекой
# которая соберется сама благодоря
# своему собственному CMakeLists.txt 
add_library(formatter_ex STATIC formatter_ex.h formatter_ex.cpp)
target_link_libraries(formatter_ex formatter) #подключаем библиотку
```

# Part 3
## Application 1
* Перемещаем библиотеку formatter_ex в папку с проектом hello_world
* Пишем и подключаем cmake
```
cmake_minimum_required(VERSION 2.8)
project(hello_world)
include_directories(formatter_ex_lib)
add_subdirectory(formatter_ex_lib)
add_executable(hello_world hello_world.cpp)
target_link_libraries(hello_world formatter_ex)
```
Никаких новых комад тут нет, так что поясненияя не требуются
* Запускаем, получаем файл hello_world, запускаем его получаем:
```
world 
-------------------------
hello, world!
-------------------------
```

## Application 2
Тут все тоже самое но нужно написать 2 симэйка для библиотеки solver_lib и для самого приложения

* Первый для библиотеки 
```
cmake_minimum_required(VERSION 2.8) 
add_library(solver_lib STATIC solver.h solver.cpp)
```

* Второй для самого прилрвжения
```
cmake_minimum_required(VERSION 2.8)
project(solver)
add_executable(solver equation.cpp)
include_directories(formatter_ex_lib)
add_subdirectory(formatter_ex_lib)
include_directories(solver_lib)
add_subdirectory(solver_lib)
target_link_libraries(solver formatter_ex)
target_link_libraries(solver solver_lib)
```

Собием, запускаем и получаем:
```
1
4
4
-------------------------
x1 = -2.000000
-------------------------
-------------------------
x2 = -2.000000
-------------------------
```
