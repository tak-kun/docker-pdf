# INSTALLING AND SETUP DOCKER SERVICE #

Перед тем как переходить к установке самой программы, нужно обновить систему до актуального состояния. Для этого выполните:

`sudo apt update && sudo apt upgrade`

Затем необходимо установить дополнительные пакеты ядра, которые позволяют использовать Aufs для контейнеров Docker. С помощью этой файловой системы мы сможем следить за изменениями и делать мгновенные снимки контейнеров:

`sudo apt install linux-image-extra-virtual`

После того как все приготовления завершены и вы убедились что ваша система полностью готова, можно перейти к установке. Мы будем устанавливать программу из официального репозитория разработчиков. Чтобы установить Docker Ubuntu 16.04 выполните в терминале:

`sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D`

`sudo apt-add-repository 'deb https://apt.dockerproject.org/repo ubuntu-xenial main'`

`sudo apt update && apt-cache policy docker-engine`

И установка Docker на Ubuntu:

`sudo apt install -y docker-engine`

Чтобы завершить установку осталось добавить нашего пользователя в группу docker. Иначе при запуске утилиты вы будете получать ошибку подключения к сокету:

sudo usermod -aG docker %%USERNAME%%

Затем проверяем запущен ли сервис:

`sudo systemctl status docker`

Все готово к работе. Теперь рассмотрим подробнее использование Docker.
-----------------------------------------------

## УТИЛИТА DOCKER ##

Все действия с контейнерами выполняются утилитой docker. Ее можно запускать от имени вашего пользователя после того, как он был добавлен в группу программы. Синтаксис утилиты очень прост:

`docker опции команда опции_команды аргументы`

Давайте сначала рассмотрим основные опции утилиты их всего несколько:

-D - включить режим отладки;
-H - подключиться к серверу, запущенному на другом компьютере;
-l - изменить уровень ведения логов, доступно: debug,info,warn,error,fatal;
-v - показать версию;
--help вывести справку по команде или утилите в целом;
Команд намного больше, ниже приведены все команды, которые вы можете использовать в своих программах:

attach - подключиться к запущенному контейнеру;
build - собрать образ из инструкций dockerfile;
commit - создать новый образ из изменений контейнера;
cp - копировать файлы между контейнером и файловой системой;
create - создать новый контейнер;
diff - проверить файловую систему контейнера;
events - посмотреть события от контейнера;
exec - выполнить команду в контейнере;
export - извлечь содержимое контейнера в архив;
history - посмотреть историю изменений образа;
images - список установленных образов;
import - создать контейнер из архива tar;
info - посмотреть информацию о системе;
inspect - посмотреть информацию о контейнере;
kill - остановить запущенный контейнер;
load - загрузить образ из архива;
login - авторизация в официальном репозитории Docker;
logout - выйти из репозитория Docker;
logs - посмотреть логи контейнера;
pause - приостановить все процессы контейнера;
port - подброс портов для контейнера;
ps - список запущенных контейнеров;
pull - скачать образ контейнера из репозитория;
push - отправить образ в репозиторий;
restart - перезапустить контейнер;
rm - удалить контейнер;
run - выполнить команду в контейнере;
save - сохранить образ в архив tar;
search - поиск образов в репозитории по заданному шаблону;
start - запустить контейнер;
stats - статистика использования ресурсов контейнером;
stop - остановить контейнер;
top - посмотреть запущенные процессы в контейнере;
unpause - проложить выполнение процессов в контейнере.
В этой статье мы будем часто использовать команду run, рассмотрим ее опции:

-e - переменные окружения для команды;
-h - имя хоста контейнера;
-i - интерактивный режим, связывающий stdin терминала с командой;
-m - ограничение памяти для команды;
-u - пользователь, от имени которого будет выполнена команда;
-t - связать tty с контейнером для работы ввода и вывода;
-v - примонтировать директорию основной системы в контейнер.
Теперь, когда мы рассмотрели все основы, приведем несколько примеров работы с контейнерами. Это очень просто.

## ИСПОЛЬЗОВАНИЕ DOCKER В UBUNTU ##

Чтобы убедиться что все работает давайте запустим тестовый образ. Для этого наберите:

`docker run hello-world`

Больше ничего не нужно, программа сама скачает образ, и выполнит оболочку в нем. Вы увидите сообщение Hello from Docker.

## ПОИСК И УСТАНОВКА КОНТЕЙНЕРОВ ##

Использование docker очень простое по своей сути. Если вы не знаете название нужного пакета, можете воспользоваться поиском, например, найдем Ubuntu:

`docker search ubuntu`

Утилита выведет список всех доступных для загрузки образов из репозитория Docker, которые содержат такое слово. Колонка Official означает, что образ поддерживается официальным разработчиком, а Stars - это количество пользователей, которым этот образ понравился.
Для загрузки образа на локальный компьютер используйте команду pull:

`docker pull ubuntu`

После завершения загрузки вы можете посмотреть список установленных образов:

`docker images`

## ЗАПУСК КОНТЕЙНЕРA ## 

Теперь, давайте запустим командную оболочку контейнера с помощью команды run, для получения интерактивного доступа используйте опции -i и -t:

`docker run -it ubuntu`

## СОХРАНЕНИЕ ИЗМЕНЕНИЙ ##

Вы можете изменять контейнер как захотите, добавлять и удалять программы и многое другое. Но когда вы его удалите, все изменения будут потеряны. Вы можете создать новое образ из модифицированного контейнера, для этого используется команда commit. Сначала смотрим список запущенных контейнеров:

`docker ps`

Отсюда возьмите id контейнера, затем выполните такую команду для создания нового образа:

`docker commit -m "изменения" -a "автор" ид_контейнера repository/имя`

Например:

`docker commit -m "Zenity" -a "Seriyyy95" d034b794a3bf repository/ubuntu-zenity`

Новый образ был сохранен на вашем компьютере и вы можете увидеть его в списке образов:

`docker images`





