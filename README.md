# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Решетов Павел`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

### Требования к результатам

Прикрепите в файл README.md скриншот авторизации в админке.
Приложите в файл README.md текст использованных команд в GitHub.##


`При необходимости прикрепитe сюда скриншоты`
![web-server-zabbix](img/task-1.png)

### Использованные команд:
- sudo apt install postgresql postgresql-contrib - устанавливаем postgres
- sudo wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian12_all.deb - скачиваем архив с zabix
- sudo dpkg -i zabbix-release_latest_6.0+debian12_all.deb - распаковка архива
- sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.4-pgsql zabbix-apache-conf zabbix-sql-scripts устанавливаем все нужные пакеты
- start postgresql
- enable postgresql
- sudo -u postgres psql -c "CREATE DATABASE zabbix OWNER zabbix;" -создаем БД в zabbix
- sudo nano /etc/zabbix/zabbix_server.conf - можно тут редактировать данные БД
- sudo sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf - задаем пароль
- systemctl restart zabbix-server apache2
- systemctl enable zabbix-server apache2
- настройка веб-сервиса zabbix
---

### Задание 2

Установите Zabbix Agent на два хоста.

### Процесс выполнения

1. Установлены Zabbix Agent на две виртуальные машины:
   - одна из них — сервер Zabbix  
   - вторая — отдельный клиент
2. В конфигурации `zabbix_agentd.conf` на обоих хостах:
   - указан IP-адрес сервера Zabbix в параметрах `Server` и `ServerActive`
   - установлен уникальный `Hostname`
3. Агенты добавлены через `Configuration > Hosts` в веб-интерфейсе Zabbix
4. Привязаны шаблоны `Template OS Linux by Zabbix agent`
5. Проверено поступление метрик в `Monitoring > Latest Data`

Требования к результатам

Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
Приложите в файл README.md текст использованных команд в GitHub


### Скриншоты:

![Configuration Hosts](img/task-2-1.png)

![agent log](img/task-2-2.png)

![Latest data](img/task-2-3.png)


### Использованные команды: (на второй машине где только агент плюсом команды по установке скаиванию, распаковке архива zabbix для ubuntu)
- sudo apt install zabbix-agent
- sudo nano /etc/zabbix/zabbix_agentd.conf - прописываем нужный ip машины и имя хоста, для мониторинга агентом, на каждой машине свой ip и имя хоста
- sudo systemctl restart zabbix-agent
- sudo systemctl enable zabbix-agent
- настройки через веб-интерфейс, создание хостов и прописывание данных из файла zabbix_agentd.conf
- sudo cat /var/log/zabbix-agent/zabbix_agentd.log - просмотр логов агента
- 

---

