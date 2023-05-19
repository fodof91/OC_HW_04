# Задание № 3 «Сетевые взаимодействия с применением протокола TCP»
## Фирсов Федор БПИ219.

## Условие задчи:

Задача 15. O клумбе – 1. На клумбе растет 40 цветов, за ними непрерывно следят два процесса–садовника
и поливают увядшие цветы, при этом оба садовника очень боятся полить один и тот же цветок,
который еще не начал вянуть. <br/>
Создать приложение, моделирующее состояния цветков на клумбе и действия садовников.
Для изменения состояния цветов создать отдельный процесс (а не 40 процессов для каждого цветка),
который может задавать одновременное начало увядания для нескольких цветков.

*  На момент начала моделирования все цветы политы.
*  Каждое утро каждый политый цветок с вероятностью 1/2 начинают увядать.
*  Во избежание траура будем считать, что увядший цветок просто некрасивый и никгда не умрет
*  Днем два Садовника Alice и Bob просыпаются и идут слева направо вдоль клумбы и поливают увядшие цветы.
*  У садовником могут быть лейки разных размеров (задается как параметр) стандартно у Alice для 5-ти цветов, а у BOb-a только 4. 
*  Если цветок поливается одним садовникам, то другой не будет его поливать благодаря pthread семафору.
*  Садовники не стареют, а цветы никогда не умрут - процесс может длиться вечность)

##  Запуск и тестирование:
Для простоты запуска в каждой папке есть Make файл и Bash скрип. 

Скрипт компилирует и запускает всех клиентов и сервер с перенаправлением вывода в файл.

При запуске Bash скрипта процесс начинается автоматически, но для пояления выходных файлов необходимо его прервать. (CTRL + C)

В папке sample лежат примеры работы

##  Решение:
###  4-5:

1. [Код клиента-клумбы](https://github.com/fodof91/OC_HW_03/blob/master/4-5/client_flowerbed.c)
2. [Код клиента садовника](https://github.com/fodof91/OC_HW_03/blob/master/4-5/client_gardener.c)
3. [Код сервера](https://github.com/fodof91/OC_HW_03/blob/master/4-5/server.c)
4. [Код скрипта запуска](https://github.com/fodof91/OC_HW_03/blob/master/4-5/run.sh)
5. [Код Makefile](https://github.com/fodof91/OC_HW_03/blob/master/4-5/Makefile)

Система состоит из сервера и 3-х клиентов: клумба и 2 садовника.
Клумба постоянно отправляет запрос серверу с данными о завядших цветах на ней, а получает информацию о новых поливах.

Вся информация как в этом запросе так и во всех остальных это 40 char-ов, где i-й символ '1', если i-ый цветок необходимо полить, и '0' иначе.

Между садовниками и сервером существует 2 вида запросов: Настал новый день и просходит полив

При поливе обновляется информация на сервера, а садовник узнает не опередил ли его другой садовник для полива некоторых цветов.
А при запросе начала нового дня обновляется информация о завядших цветах у садовника.

Синхронизация всех запросов происходит благодаря pthread семафору на сервере.
