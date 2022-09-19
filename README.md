# Запускаем
./configure
Выполнение этой команды у меня занимается около пятнадцати секунд. Последние пять строк вывода:

config.status: linking src/backend/port/tas/dummy.s to src/backend/port/tas.s
config.status: linking src/backend/port/posix_sema.c to src/backend/port/pg_sema.c
config.status: linking src/backend/port/sysv_shmem.c to src/backend/port/pg_shmem.c
config.status: linking src/include/port/linux.h to src/include/pg_config_os.h
config.status: linking src/makefiles/Makefile.linux to src/Makefile.port
Далее — сборка:

1
make
Опять же, если вы поставили и bizon, и flex — тогда ошибок не будет. Но, если этих программ не хватает, ошибки будут выглядеть следующим образом:

Не установлен bison:

ERROR: `bison' is missing on your system. It is needed to create the
file `gram.c'. You can either get bison from a GNU mirror site
or download an official distribution of PostgreSQL, which contains
pre-packaged bison output.
***
make[2]: *** [../../../src/Makefile.global:750: gram.c] Error 1
make[2]: Leaving directory '/home/test/postgres/src/backend/parser'
make[1]: *** [Makefile:137: parser/gram.h] Error 2
make[1]: Leaving directory '/home/test/postgres/src/backend'
make: *** [src/Makefile.global:388: submake-generated-headers] Error 2
Не установлен flex:
ERROR: `flex' is missing on your system. It is needed to create the
file `bootscanner.c'. You can either get flex from a GNU mirror site
or download an official distribution of PostgreSQL, which contains
pre-packaged flex output.
***
make[3]: *** [../../../src/Makefile.global:745: bootscanner.c] Error 1
make[3]: Leaving directory '/home/test/postgres/src/backend/bootstrap'
make[2]: *** [common.mk:39: bootstrap-recursive] Error 2
make[2]: Leaving directory '/home/test/postgres/src/backend'
make[1]: *** [Makefile:42: all-backend-recurse] Error 2
make[1]: Leaving directory '/home/test/postgres/src'
make: *** [GNUmakefile:11: all-src-recurse] Error 2
Если произошли такие ошибки, можно откатиться к начальному состоянию и запустить конфигурирование и установку заново. Для этого используется команда:

1
make distclean
После нее снова нужно запускать ./configure с указанием необходимых параметров (если они есть). И потом — опять make.


All of PostgreSQL successfully made. Ready to install.
Далее можно запустить команду make check. С помощью этой команды можно протестировать собранный сервер до его установки с помощью регрессионных тестов. Регрессионные тесты — это комплект тестов, проверяющих, что PostgreSQL работает на вашем компьютере так, как задумано разработчиками. Давайте проверим:

make check
 
=======================
 All 201 tests passed.
=======================
По видимому, всё хорошо (проверка длилась около минуты). Идем дальше.

Установка — запускаем команду:

1
sudo make install
И через пару секунд я получил сообщение:

1
PostgreSQL installation complete.
Ура! Установка PotsgreSQL 13-й версии успешно завершена!
