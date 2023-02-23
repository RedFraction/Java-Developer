[Вопросы для собеседования](README.md)

https://badtry.net/docker-tutorial-dlia-novichkov-rassmatrivaiem-docker-tak-iesli-by-on-byl-ighrovoi-pristavkoi/
https://eternalhost.net/base/vps-vds/docker-zapusk-konteynera

# Docker
+ [Что такое _Docker_?](#Что-такое-Docker)
+ [Зачем _Docker_?](#Зачем-Docker)
+ [Что такое _Docker Image (образ)_?](#Что-такое-Docker-Image-(образ))
+ [Что такое _Docker контейнер_?](#Что-такое-Docker-контейнер)
+ [Как менять контейнер?](#Как-менять-контейнер)
+ [Что такое _Dockerfile_?](#Что-такое-Dockerfile)
+ [Как пробрасывать локальную папку в контейнер Докера (монтирование папки)?](#Как-пробрасывать-локальную-папку-в-контейнер-Докера-(монтирование папки))
+ [Что такое Docker Volumes?](#Что-такое-Docker-Volumesr)
+ [Как работают и пробрасываются Docker порты?](#Как-работают-и-пробрасываются-Docker-порты)
+ [Слои Docker образа, прослойка данных и кеширование](#Слои-Docker-образа,-прослойка-данных-и-кеширование)
+ [Что такое Docker-compose?](#Что такое Docker-compose)
+ [Как писать Микро Сервисы с Docker?](#Как-писать-Микро-Сервисы-с-Docker?)

## Что такое _Docker_?
На сайте Докера можно найти статью (https://www.docker.com/), в которой подробно рассказывается, что такое Docker. Из их слов - это стандартизированное ПО для разработки и развёртывания проектов.

__Если говорить проще:__ Давайте на секунду забудем про Докер, и вспомним про такую ностальгическую штуку, как GameBoy Color. Если вы помните, игры для этой приставки поставлялись в виде картриджей. И я уверен в том, что производители видео игр пользуются успехом из-за своей простоты:

+ Когда ты хочешь поиграть, ты просто вставляешь картридж в приставку, и игра сразу же работает.
+ Ты можешь поделиться своей игрой с друзьями, передав всего лишь картридж, который они вставят в приставку, и сразу же смогут играть.

Docker следует похожему принципу - позволяет запускать своё ПО настолько просто, что это соизмеримо с вставкой картриджа и нажатием кнопки ON на приставке. Это основная суть, почему Docker настолько полезен - теперь кто угодно, у кого установлен Docker может запустить ваше приложение, выполнив для этого всего несколько команд. Раньше, вы, создавая приложения, к примеру на PHP, устанавливали локально PHP, MySql, возможно, NodeJs, при этом устанавливая зависимости в виде нужных расширений и библиотек. И, в случае передачи вашего скрипта какому-то знакомому, ему требовалось настраивать аналогичное окружение, аналогичных версий, иметь аналогичные расширения и конфигурацию, чтобы успешно запустить ваше приложение. Сейчас же, при использовании Докера, такой проблемы не возникнет впринципе. Теперь вам достаточно иметь установленную программу Docker, которая по одной вашей команде установит окружение, описанное в конфиге для запуска вашего приложения.

Какое программное обеспечение можно запустить с помощью докера? В техническом плане, Docker чем-то похож на виртуальную машину:

Докер - это движок, который запускает виртуальную операционную систему, имеющую чрезвычайно маленький вес (в отличие от Vagrant-а, который создаёт полноценную виртуальную ОС, Докер, имеет особые образы ПО, запускающиеся в виртуальной среде, не создавая полную копию ОС). Docker позволяет запустить ОС Linux в изолированной среде очень быстро, в течение нескольких минут.

[к оглавлению](#Docker)

## Зачем _Docker_?
__Кошмар при установке ПО__, с которым приходится сталкиваться. У вас когда-нибудь было такое, что вы пытаетесь установить ПО на ваш компьютер, а оно отказывается работать? Вы получаете несколько непонятных вам ошибок, из-за которых ничего не запускается. И после нескольких часов гугления, на десятой странице гугла...и на каком-то форуме, до этого неизвестного вам, вы наконец-то находите случайный комментарий, который помогает исправить вашу проблему.

Аналогично, что делает написание PC игр более сложным, чем написание Game Boy игр - это то, что приходится проектировать систему с учётом большого множества существующих PC девайсов и спецификаций. Так как разные компьютеры имеют различные операционные системы, драйвера, процессоры, графические карты, и т.д.
И потому задача разработчика - написать приложение совместимое со всеми популярными системами, является достаточно затруднительной и трудоёмкой.

Docker, как и Game Boy приставка, __берёт стандартизированные части программного обеспечения и запускает их__ так, как Game Boy запускал бы игру.

В этом случае вы не должны беспокоиться об операционной системе, на которой пользователь будет запускать ваше приложение. Теперь, когда пользователи будут запускать приложение через Docker - конфигурация будет собрана автоматически, и код будет выполняться ВСЕГДА.

__Как разработчик__, теперь вы не должны волноваться о том, на какой системе будет запущено ваше приложение.
__Как пользователь__, вам не нужно волноваться о том, что вы скачаете неподходящую версию ПО (нужного для работы программы). В Докере эта программа будет запущена в аналогичных условиях, при которых это приложение было разработано, потому, исключается факт получить какую-то новую, непредвиденную ошибку. Для пользователя все действия сводятся к принципу подключи и играй.

[к оглавлению](#Docker)

## Что такое _Docker Image (образ)_?
Docker образ (он же Docker Image), похож на Game Boy картридж - это просто программное обеспечение. Это стандартизированное программное обеспечение, которое запускается на любой приставке Game Boy. Вы можете дать игру вашему другу, и он сможет просто вставить картридж в приставку, и играть.

Как в случае с картриджами, бывают различные игры, так и Docker имеет различные образы ПО: ubuntu, php (который наследуется от оригинального образа Ubuntu), nodejs, и т.д.

```java
docker pull IMAGE_NAME // где <IMAGE_NAME> - имя скачиваемого образа

docker pull ubuntu:18.10
```
Эта команда сообщает Докеру о том, что нужно скачать образ Ubuntu 18.10 с Dockerhub.com - основной репозиторий Docker-образов, на котором вы и можете посмотреть весь их список и подобрать нужный образ для вашей программы. Представленные в хабе образы можно найти при помощи команд docker и search. К примеру, найти образ Ubuntu можно следующим образом:
````java
docker search ubuntu
````

Теперь, для того, чтобы посмотреть список всех загруженных образов, нужно выполнить:
````java
docker images
````
Проводя аналогии, команда docker images выглядит как коллекция картриджей от приставки, которые у вас есть сейчас

[к оглавлению](#Docker)


## Что такое _Docker контейнер_?
Теперь представьте, что мы обновили нашу приставку с Game Boy на GameCube. Игры хранятся на диске, который предназначен только для чтения самого образа игры. А прочие файлы (сохранения, кеш и т.д.) сохраняются внутри самой приставки, локально.

Так же, как и игра на диске, исходный Docker Image (образ) - __неизменяемый__.

Docker контейнер - это экземпляр запущенного образа. Аналогично тому, что вы вставляете диск в приставку, после чего игра начинается. А сам образ игры никак не модифицируется, все файлы, содержащие изменения хранятся где-то локально на приставке.

Запуск Docker контейнера соответствует тому, что вы играете в свою Gamecube игру. Docker запускает ваш образ в своей среде, аналогично тому, как Gamecube запускает игру с диска, не модифицируя оригинальный образ, а лишь сохраняя изменения и весь прогресс в какой-то песочнице.

Для запуска контейнера существует команда: 
````java
docker run <image> <опциональная команды, которая выполнится внутри контейнера>
````

__Давайте запустим наш первый контейнер Ubuntu__
````java
docker run ubuntu:18.10 echo 'hello from ubuntu'
````
Команда echo 'hello from ubuntu' была выполнена внутри среды Ubuntu. Другими словами, эта команда была выполнена в контейнере ubuntu:18.10.

Теперь выполним команду для проверки списка запущенных контейнеров:
````java
docker ps
````
Здесь пустота... это потому что docker ps показывает только список контейнеров, которые запущены в данный момент (наш же контейнер выполнил одну команду echo 'hello from ubuntu' и завершил свою работу).

А для того, чтобы посмотреть список всех контейнеров без исключения, нужно добавить флаг -a, выполним:
````java
docker ps -a
````

После выполнения нужных операций внутри контейнера, то Docker-контейнер завершает работу. Это похоже на режим сохранения энергии в новых игровых консолях - если вы не совершаете действий какое-то время, то система выключается автоматически.

Каждый раз, когда вы будете выполнять команду docker run, будет создаваться новый контейнер, на каждую из выполненных команд.

__Выполнение неограниченное количество команда внутри контейнера__

Давайте добавим немного интерактивности в наше обучение. Мы можем подключиться к консоли виртуальной ОС (Ubuntu 18.10), и выполнять любое количество команд без завершения работы контейнера, для этого, запустим команду:
````java
docker run -it ubuntu:18.10 /bin/bash
````
Опция -it вместе с /bin/bash даёт доступ к выполнению команд в терминале внутри контейнера Ubuntu.

Теперь, внутри этого контейнера можно выполнять любые команды, применимые к Ubuntu. Вы же можете представлять это как мини виртуальную машину, условно, к консоли которой мы подключились по SSH.

__Узнаём ID контейнера__
Иногда является очень полезным узнать ID контейнера, с которым мы работаем. И как раз-таки, при выполнении команды docker run -it <IMAGE> /bin/bash, мы окажемся в терминале, где все команды будут выполняться от имени пользователя root@<containerid>.

Теперь, все команды буду выполняться внутри операционной системы Ubuntu. Попробуем, например, выполнить команду ls, и посмотрим, список директорий, внутри этого образа Ubuntu.docker-ubuntu-ls

Docker контейнер является полностью независимым от системы хоста, из которой он запускался. Как изолированная виртуальная машина. И в ней вы можете производить любые изменения, которые никак не повлияют на основную операционную систему.

Это аналогично тому, как, если бы вы играли в Mario Kart на приставке Gamecube, и неважно, что вы делаете в игре, вы никак не сможете изменить само ядро игры, или изменить информацию, записанную на диске.

Контейнер является полностью независимым и изолированным от основной операционной системы, аналогично виртуальной операционной системе. Вы можете вносить любые изменения внутри виртуалки, и никакие из этих изменений не повлияют на основную операционную систему.

Теперь откройте новое окно терминала (не закрывая и не отключаясь от текущего), и выполните команду docker ps
docker-ps-new-window

Только на этот раз вы можете увидеть, что контейнер с Ubuntu 18.10 в текущий момент запущен.

Теперь вернёмся назад к первому окну терминала (который находится внутри контейнера), и выполним:
````java
mkdir /truedir   //создаст папку truedir
exit             //выйдет из контейнера, и вернётся в основную ОС
````
Выполнив команду exit, контейнер будет остановлен (чтобы убедиться, можете проверить командой docker ps). Теперь, вы так же знаете, как выйти из Docker контейнера.

Теперь, попробуем ещё раз просмотреть список всех контейнеров, и убедимся, что новый контейнер был создан docker ps -adocker-ps--a2

Так же, для того, чтобы запустить ранее созданный контейнер, можно выполнить команду docker start <CONTAINER_ID>,

где CONTAINER_ID - id контейнера, который можно посмотреть, выполнив команду docker ps -a (и увидеть в столбце CONTAINER_ID)

В моём случае, CONTAINER_ID последнего контейнера = 7579c85c8b7e (у вас же, он будет отличаться)

Запустим контейнер командой:
````java
docker start 7579c85c8b7e    //ваш CONTAINER_ID
docker ps
docker exec -it 7579c85c8b7e /bin/bash  //ваш CONTAINER_ID
````
И теперь, если внутри контейнера выполнить команду ls, то можно увидеть, что ранее созданная папка truedir существует в этом контейнереdocker-exex-truedir

Команда exec позволяет выполнить команду внутри запущенного контейнера. В нашем случае, мы выполнили /bin/bash, что позволило нам подключиться к терминалу внутри контейнера.

Для выхода, как обычно, выполним exit.

Теперь остановим и удалим Docker контейнеры командами:
````java
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
docker ps a                // просмотрим список активных контейнеров
docker stop aa1463167766   // остановим активный контейнер
docker rm aa1463167766     // удалим контейнер
docker rm bb597feb7fbe     // удалим второй контейнер
````
В основном, нам не нужно, чтобы в системе плодилось большое количество контейнеров. Потому, команду docker run очень часто запускают с дополнительным флагом --rm, который удаляет запущенный контейнер после работы:
```java
docker run -it --rm ubuntu:18.10 /bin/bash
```

[к оглавлению](#Docker)

## Как менять контейнер?
Во время запуска контейнера из существующего образа у пользователя есть возможность создавать или удалять файлы, аналогично работе на виртуальной машине. При этом изменения будут распространяться только в определенном контейнере. Доступна и возможность запуска с последующей остановкой контейнера, но после его удаления с помощью docker rm будут утеряны внесенные изменения.

Соответственно, следует ознакомиться со способом сохранения текущего контейнера как нового образа.

По завершении инсталляции Node.js в контейнере Ubuntu, на компьютере работает загруженный из образа контейнер. При этом он будет отличаться от использованного для его создания образа. В свою очередь, пользователю может понадобиться уже контейнер Node.js, чтобы использовать его при создании для новых образов.

Соответственно, следует сохранить результаты в текущем образе предложенной ниже командой:
```java
docker commit -m "What you did to the image" -a "Author Name" container_id repository/new_image_name
```
Добавление опции -m дает возможность указать сообщение подтверждения. Это позволит будущим пользователям образа понять, что именно было изменено. Что касается параметра -a — с его помощью можно указать, кто его создатель. container_id является тем же идентификатором, который был использован ранее, во время запуска интерактивной сессии в Docker.

К примеру, с именем пользователя admin и идентификатором 2c8ec46adae1 команда должна иметь следующий вид:
```java
docker commit -m "added Node.js" -a "admin" 2c8ec46adae1 admin/ubuntu-nodejs
```
Как запустить Docker контейнер на Linux

После того, как образ будет подтвержден (commit) он сохраняется на компьютере локально.

Завершающий этап — сохранение созданных образов в базу Docker Hub или другой репозиторий, откуда их может скачать любой желающий. Чтобы получить такую возможность, предварительно нужно создать аккаунт.

Отправка образов в репозиторий начинается с авторизации на Docker Hub.
```java
docker login -u docker-registry-username
```

Чтобы вход был успешно осуществлен, потребуется ввести пароль Docker Hub. Если он правильный, авторизация пройдет успешно.

Здесь важно знать, что если в реестре Docker имя пользователя отличается от локального, используемого при создании образа, обязательно нужно привязать этот образ к имени учетной записи в хабе. На примере контейнера с NodeJS команда привязки будет выглядеть так:
```java
docker tag admin/ubuntu-nodejs docker-registry-username/ubuntu-nodejs
```

После чего можно приступать к загрузке образа на сервер:
```java
docker push docker-registry-username/docker-image-name
```

__Автозагрузка контейнеров__

Часто встречается ситуация, когда контейнеры останавливаются вследствие определенных факторов. Простейший пример – произошла перезагрузка сервера. Чтобы избавиться от необходимости вручную запускать их, можно настроить автозапуск контейнеров. Для этого следует создать текстовые файлы со специальным форматом для сервиса systemcmd. Рассмотрим пример их создания на примере контейнера my-db, введя в терминал команду:
```java
cat /etc/systemd/system/my-db.service
```
В пустой файл необходимо добавить следующий код и сохранить его:
```java
[Unit]
Description=MY DB (PG) docker container
Requires=docker.service
After=docker.service
[Service]
Restart=always
ExecStart=/usr/bin/docker start -a my-db
ExecStop=/usr/bin/docker stop -t 2 my-db
TimeoutSec=30
[Install]
WantedBy=multi-user.target
```

После этого остается перезапустить демон systemcmd и включить автозагрузку контейнера mydb, набрав в терминале поочередно команды:
```java
systemctl daemon-reload
systemctl start my-db.service
systemctl enable my-db.service
```

[к оглавлению](#Docker)

## Что такое _Dockerfile_?
Docker позволяет вам делиться с другими средой, в которой ваш код запускался и помогает в её простом воссоздании на других машинах.

Dockerfile - это обычный конфигурационный файл, описывающий пошаговое создание среды вашего приложения. В этом файле подробно описывается, какие команды будут выполнены, какие образы задействованы, и какие настройки будут применены. А движок Docker-а при запуске уже распарсит этот файл (именуемый как Dockerfile), и создаст из него соответствующий образ (Image), который был описан.

К примеру, если вы разрабатывали приложение на php7.2, и использовали ElasticSearch 9 версии, и сохранили это в Dockerfile-е, то другие пользователи, которые запустят образ используя ваш Dockerfile, получат ту же среду с php7.2 и ElasticSearch 9.

С Dockerfile вы сможете подробно описать инструкцию, по которой будет воссоздано конкретное состояние. И делается это довольно-таки просто и интуитивно понятно.

Если вам нужно воспроизвести среду (образ) на другом ПК, с докером вы так же имеете два варианта при создании образа:

+ Вы можете запаковать ваш контейнер, создать из него образ (аналогично тому, что вы запишите на диск новую игру с собственными модификациями). Это похоже на способ, когда вы делитесь сохранениями напрямую.
+ Или же, можно описать Dockerfile - подробную инструкцию, которая приведёт среду к нужному состоянию.

Я склоняюсь ко второму варианту, потому что он более подробный, гибкий, и редактируемый (вы можете переписать Dockerfile, но не можете перемотать состояние образа в случае прямых изменений).

Пришло время попрактиковаться на реальном примере. Для начала, создадим файл cli.php в корне проекта с содержимым:
````php
<?php
$n = $i = 5;

while ($i--) {
    echo str_repeat(' ', $i).str_repeat('* ', $n - $i)."\n";
}
````
Пример Dockerfile:
````java
FROM php:7.2-cli
COPY cli.php /cli.php
RUN chmod +x /cli.php
CMD php /cli.php
````
Имена команд в Dockerfile это синтаксис разметки Dockerfile. Эти команды означают:
+ FROM - это как буд-то вы выбираете движок для вашей игры (Unity, Unreal, CryEngine). Хоть вы и могли бы начать писать движок с нуля, но больше смысла было бы в использовании готового. Можно было бы использовать, к примеру, ubuntu:18.10, в нашем коде используется образ php:7.2-cli, потому весь код будет запускаться внутри образа с предустановленным php 7.2-cli.
+ COPY - Копирует файл с основной системы в контейнер (копируем файл cli.php внутрь контейнера, с одноимённым названием)
+ RUN - Выполнение shell-команды из терминала контейнера (в текущем случае, присвоим права на выполнение скрипта /cli.php
+ CMD - Выполняет эту команду каждый раз, при новом запуске контейнера

Все команды https://docs.docker.com/engine/reference/builder/#from

При написании Dockerfile, начинать следует с наиболее актуального существующего образа, дополняя его в соответствии с потребностями вашего приложения.
К примеру, мы могли не использовать образ php:7.2-cli, а могли взять ubuntu:18.10, последовательно выполняя команды в RUN одна за одной, устанавливая нужное ПО. Однако, в этом мало смысла, когда уже есть готовые сборки.

Для создания образа из Dockerfile нужно выполнить:
````java
docker build <DOCKERFILE_PATH> --tag <IMAGE_NAME>
<DOCKERFILE_PATH> - путь к файлу Dockerfile (. - текущая директория),
<IMAGE_NAME> - имя, под которым образ будет создан
````

Выполним:
````java
docker build . --tag pyramid
````
При том, что имя файла Dockerfile при указывании пути упускается, нужно указывать только директорию, в которой этот файл находится (а . означает, что файл находится в той директории, из которой была запущена консоль)

После того, как команда выполнилась, мы можем обращаться к образу по его имени, которое было указано в <IMAGE_NAME>, проверим список образов: docker images

Теперь, запустим контейнер из нашего образа командой docker run pyramid

__Сначала мы скопировали файл cli.php в Docker образ, который создался с помощью Dockerfile. Для того, чтобы удостовериться в том, что файл действительно был проброшен внутрь контейнера, можно выполнить команду docker run pyramid ls, которая в списке файлов покажет и cli.php.__

Однако, сейчас этот контейнер недостаточно гибкий. Нам бы хотелось, чтобы можно было удобно изменять количество строк, из скольки состоит пирамида.

Для этого, отредактируем файл cli.php, и изменим, чтобы количество аргументов принималось из командной строки. Отредактируем вторую строку на:
````php
$n = $i = $argv[1] ?? 5; //а было $n = $i = 5
// это значит, что мы принимаем аргумент из консоли, а если он не получен, то используем по-умолчанию 5
````
После чего, пересоберём образ: docker build . --tag pyramid
И запустим контейнер: docker run pyramid php /cli.php 9, получив вывод ёлки пирамиды в 9 строк

Почему это работает?
Когда контейнер запускается, вы можете переопределить команду записанную в Dockerfile в поле CMD.

Наша оригинальная CMD команда, записанная в Dockerfile php /cli.php - будет переопределена новой php /cli.php 9.
Но, было бы неплохо передавать этот аргумент самому контейнеру, вместо переписывания всей команды. Перепишем так, чтобы вместо команды php /cli.php 7 можно было передавать просто аргумент-число.

Для этого, дополним Dockerfile:
````java
FROM php:7.2-cli
COPY cli.php /cli.php
RUN chmod +x /cli.php
ENTRYPOINT ["php", "/cli.php"]
## аргумент, который передаётся в командную строку
CMD  ["9"]
````
Мы немного поменяли формат записи. В таком случае, CMD будет добавлена к тому, что выполнится в ENTRYPOINT.

["php", "/cli.php"] на самом деле запускается, как php /cli.php. И, учитывая то, что CMD будет добавлена после выполнения текущей, то итоговая команда будет выглядеть как: php /cli.php 9 - и пользователь сможет переопределить этот аргумент, передавая его в командную строку, во время запуска контейнера.

Теперь, заново пересоберём образ
````java
docker build . --tag pyramid
````
И запустим контейнер с желаемым аргументом
````java
docker run pyramid 3
````
[к оглавлению](#Docker)


## Как пробрасывать локальную папку в контейнер Докера (монтирование папки)?
Монтирование директории в Docker контейнер - это предоставление доступа контейнеру на чтение содержимого вашей папки из основной операционной системы. Помимо чтения из этой папки, так же, контейнер может её изменять, и такая связь является двусторонней: при изменении файлов в основной ОС изменения будут видны в контейнере, и наоборот.

__Монтирование директории в контейнер позволяет ему читать и писать данные в эту директорию, изменяя её состояние.__

Для того, чтобы смонтировать папку из основной системы в контейнер, можно воспользоваться командой
docker run -v <DIRECTORY>:<CONTAINER_DIRECTORY> ...,
где DIRECTORY - это путь к папке, которую нужно смонтировать,
CONTAINER_DIRECTORY - путь внутри контейнера.

Только путь к монтируемой папке должен быть прописан полностью: C:\projects\docker-example, или на *nix-системах можно воспользоваться конструкцией $(pwd)

Выполним команду:
````java
docker run -it -v C:\projects\docker-example\cli:/mounted  ubuntu:18.10 /bin/bash
ls
ls mounted
touch mounted/testfile
````
При выполнении этой команды, указанная папка смонтируется в папку /mounted, внутри файловой системы контейнера, а команда touch mounted/testfile создаст новый файл под названием testfile, который вы можете увидеть из основной ОС.

Теперь вы можете увидеть, что после выполнения этой команды в текущей директории появился новый файл testfile. И это говорит о том, что двусторонняя связь работает - при изменении директории на основной ОС всё отразится на смонтированную папку внутри контейнера, а при изменениях изнутри контейнера всё отразится на основную ОС.

Монтирование папки позволяет вам изменять файлы вашей основной системы прямо во время работы внутри Docker контейнера.

Это удобная особенность, которая позволяет нам редактировать код в редакторе на основной ОС, а изменения будут сразу же применяться внутри контейнера.
[к оглавлению](#Docker)


## Что такое Docker Volumes?
Docker Volumes - что-то похоже на карты памяти для Game Cube. Эта карта памяти содержит данные для игры. Эти карты съемные, и могу работать, когда Gamecube приставка выключается. Вы так же можете подключить различные карты памяти, содержащие разные данные, а так же, подключать к разным приставкам.

Вы можете вставить вашу карту внутрь приставки, точно так же, как и Docker Volume может быть прикреплён к любому из контейнеров.

С Docker Volum-ами мы имеем контейнер, который хранит постоянные данные где-то на нашем компьютере (это актуально, потому что после завершения работы контейнер удаляет все пользовательские данные, не входящие в образ). Вы можете прикрепить Volume-данные к любому из запущенных контейнеров.

Вместо того, чтобы каждый раз, при запуске контейнера, писать, какие из папок вы хотите смонтировать, вы просто можете создать один контейнер с общими данными, который потом будете прикреплять.

Лично я, не использую это очень часто на практике, потому что есть много других методов по управлению данными. Однако, это может быть очень полезно для контейнеров, которые должны сохранять какие-то важные данные, или данные, которыми нужно поделиться между несколькими контейнерами.

[к оглавлению](#Docker)


## Как работают и пробрасываются Docker порты?

Docker позволяет нам получить доступ к какому-то из портов контейнера, пробросив его наружу (в основную операционную систему). По умолчанию, мы не можем достучаться к каким-либо из портов контейнера. Однако, в Dockerfile опция EXPOSE позволяет нам объявить, к какому из портов мы можем обратиться из основной ОС.

Для этого, на по-быстрому, запустим Docker-образ php-apache, который работает на 80 порту.

Для начала, создадим новую папку apache (перейдём в неё cd apache), в которой создадим файл index.php, на основе которого мы и поймём, что всё работает.
````php
<?php
echo 'Hello from apache. We have PHP version = ' . phpversion() . PHP_EOL;
````
А так же, в этой папке создадим файл Dockerfile:
```java
FROM php:7.2-apache
# Указываем рабочую папку
WORKDIR /var/www/html
# Копируем все файлы проекта в контейнер
COPY . /var/www/html
EXPOSE 80
```

Пробежимся по командам:
+ FROM: это вам уже знакомо, это образ с уже установленным php и apache
+ WORKDIR: создаст папку если она не создана, и перейдёт в неё. Аналогично выполнению команд mkdir /var/www/html && cd /var/www/html
+ EXPOSE: Apache по-умолчанию запускается на 80 порту, попробуем "прокинуть" его в нашу основную ОС (посмотрим как это работает через несколько секунд)

Для работы с сетью в Docker, нужно проделать 2 шага:

+ Прокинуть системный порт (Expose).
+ Привязать порт основной ОС к порту контейнера (выполнить соответствие).

+ Это что-то похоже на подключение вашей PS4 приставки к телевизору по HDMI кабелю. При подключении кабеля, вы явно указываете, какой HDMI-канал будет отображать видео. В этой аналогии наша основная ОС будет как телевизор, а контейнер - это игровая консоль. Мы должны явно указать, какой порт основной операционной системы будет соответствовать порту контейнера.

EXPOSE в Докерфайле разрешает подключение к 80 порту контейнера - как разрешение HDMI подключения к PS4.

Выполним первый шаг прокидывания порт. Сбилдим контейнер:
````java
docker build . --tag own_php_apache
````
И после этого, запустим контейнер:
````java
docker run own_php_apache
apache_call
````
После чего, попробуем перейти по адресу localhost:80

Но, это не сработало, потому что мы ещё не выполнили 2 шаг по маппингу портов.

Выйдите из контейнера, нажав CTRL+C. Если у вас проблемы с остановкой контейнера, в новом окне откройте терминал, выполните docker ps, найдите ID контейнера, который сейчас запущен, и выполните docker stop {CONTAINER_ID} (указав ваш ID контейнера)

Теперь, осталось сообщить нашему компьютеру, какой порт контейнера ему нужно слушать, и для этого формат записи будет такой:
````java
docker run -p <HOST_PORT>:<CONTAINER_PORT>
````
И мы можем указать любое соответствие портов, но сейчас просто укажем, что порт системы 80 будет слушать 80 порт контейнера:
````java
docker run -p 80:80 own_php_apache
````
Здесь, вы уже наверное заметили, что добавился новый параметр -p 80:80, который говорит Docker-у: я хочу, чтобы порт 80 из apache был привязан к моему локальному порту 80.

И теперь, если перейти по адресу localhost:80, то должны увидеть успешный ответ:
[к оглавлению](#Docker)


## Слои Docker образа, прослойка данных и кеширование
Каждый раз, когда вы собираете образ, он кешируется в отдельный слой. Ввиду того, что образы являются неизменяемыми, их ядро никогда не модифицируются, потому применяется система кеширования, которая нужна для увеличения скорости билдинга.

Каждая команда в Dockerfile сохраняется как отельный слой образа.

Рассмотрим это на примере нашего прошлого Dockerfile-а:
````java
FROM php:7.2-apache
# Копирует код ядра
COPY . /var/www/html
WORKDIR /var/www/html
EXPOSE 80
````
Когда вы пишите свой Dockerfile, вы добавляете слои поверх существующего основного образа (указанного в FROM), и создаёте свой собственный образ (Image).

+ FROM: говорит Докеру взять за основу этот существующий образ. А все новые команды будут добавлены слоями поверх этого основного образа.
+ COPY: копирует файлы с основной ОС в образ
+ WORKDIR: устанавливает текущую папку образа в /var/www/html

Слой Образа Докера это как точка сохранения в игре Super Mario. Если вы хотите изменить какие-то вещи, произошедшие до этой точки сохранения, то вам придётся перезапустить этот уровень полностью. Если вы хотите продолжить прогресс прохождения, вы можете начать с того места, где остановились.

Docker начинает кешировать с "того места, где остановился" во время билдинга Dockerfile. Если в Докерфайле не было никаких изменений с момента последнего билдинга, то образ будет взят полностью из кеша. Если же вы измените какую-то строку в Dockerfile - кеш будет взят только тех слоёв команд, которые находятся выше изменённой команды.

Для иллюстрации этого, добавим новые строки в Dockerfile:
````java
FROM php:7.2-apache
WORKDIR /var/www/html
# Copy the app code
COPY . /var/www/html
RUN apt-get update && apt-get upgrade -y && apt-get install -y curl php7.2-mbstring php7.2-zip php7.2-intl php7.2-xml php7.2-json php7.2-curl
RUN echo "Hello, Docker Tutorial"
EXPOSE 80
````
После чего, пересоберём образ:
````java
docker build . --tag own_php_apache
````
Выполнив эту команду, из вывода в консоль можете увидеть, что некоторые слои были взяты из кеша. Это как раз те команды, выше которых в Dockerfile не было добавлено/изменено содержимого.

И можно заметить, что в случае изменения Dockerfile, билдинг занимает больше времени, потому что не используется кеш. Где бы вы не написали команду, все закешированные команды, которые находятся ниже в Dockerfile, будут перебилжены заново. А те, что находятся выше, будут по-прежнему браться из кеша.

Когда вы используете команду COPY, она копирует указанную директорию в контейнер. И, в случае изменения содержимого любого из файлов этой директории, кеш команды COPY будет сброшен. Docker сверяет изменения во время билдинга в каждом из файлов. Если они были изменены, кеш будет сброшен, как и для всех последующих слоёв.

Если честно, то это действительно крутая функция. Docker следит за изменениями в файлах и использует кеш всегда, когда это нужно (когда были произведены изменения в каких-то из файлов). Изменение ваших файлов потенциально может затрагивать будущие команды, из-за чего, и все последующие слои билдятся заново, а не берутся из кеша.

Какие выводы из этого можно сделать:

+ Команды, которые вероятнее всего не будут меняться в будущем, нужно помещать как можно выше в Dockerfile.
+ Команды копирования данных нужно помещать ниже, потому что файлы при разработке изменяются довольно часто.
+ Команды, которые требуют много времени на билдинг, нужно помещать выше.

+ В заключение, так же хочу сказать, как можно уменьшить размер слоёв Docker образов.
В Dockerfile вы можете иметь несколько команд (RUN) на выполнение:
```java
RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y curl
```

В результате выполнения этой команды, будет создано 3 разных слоя в образе. Вместо этого, все команды стараются объединить в одну строку:
```java
RUN apt-get update && apt-get install -y wget curl
```
Если команда становится длинной, и нечитаемой, то для переноса на следующую строку делаем так:
```java
RUN apt-get update && apt-get install -y wget curl && \
&& apt-get clean -y \
&& docker-php-ext-install soap mcrypt pdo_mysql zip bcmath
```
Если же команда становится слишком большой, и неудобной для чтения, то можно создать новый shell скрипт, в который поместить длинную команду, и запускать этот скрипт одной простой командой RUN.

Технически, только команды ADD, COPY, и RUN создают новый слой в Docker образе, остальные команды кешируются по-другому (https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#minimize-the-number-of-layers)
[к оглавлению](#Docker)


## Что такое Docker-Compose?
Инструмент Docker Compose входит в комплект официального программного обеспечения для Docker. Он позволяет решать различные задачи, связанные с управлением одновременно несколькими контейнерами, составляющих в целом один проект.

Самый очевидный пример – веб-сайт, где для авторизации пользователей необходимо подключение к базе данных. Для такого проекта нужно два сервиса – один отвечает за функционирование сайта, а другой за базу данных.

Docker-compose написан в формате YAML который по своей сути похож на JSON или XML. Но YAML имеет более удобный формат для его чтения, чем вышеперечисленные. В формате YAML имеют значения пробелы и табуляции, именно пробелами отделяются названия параметров от их значений.

Создадим новый файл docker-compose.yml, для рассмотрения синтаксиса Docker Compose:
```java
version: '3'

services:
app:
build:
context: .
ports:
- 8080:80
```

Теперь, построчно разберёмся с заданными параметрами, и что они значат:
+ version: какая версия docker-compose используется (3 версия - самая последняя на даный момент).
+ services: контейнеры которые мы хотим запустить.
+ app: имя сервиса, может быть любым, но желательно, чтобы оно описывало суть этого контейнера.
+ build: шаги, описывающие процесс билдинга.
+ context: где находится Dockerfile, из которого будем билдить образ для контейнера.
+ ports: маппинг портов основной ОС к контейнеру.

[к оглавлению](#Docker)


## Как писать Микро Сервисы с Docker? Что такое микросервисы?

[к оглавлению](#Docker)