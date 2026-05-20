# Домашнее задание к занятию Система мониторинга Zabbix - Трофимов Алексей Ильич

## Задание 1

Ниже приведен набор команд для установки postgresql+zabbix

```bash
apt update
apt install postgresql
wget https://repo.zabbix.com/zabbix/8.0/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_8.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_8.0+ubuntu24.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

Создание пользователя базы данных и самой бд:

```bash
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix # импорт начальной схемы
```

В /etc/zabbix/zabbix_server.conf вбить пароль от бд (DBPassword=password)
И в конце концов перезапустить сервисы.

![Окно авторизации zabbix](/images/loginscreen_zabbix.png)

![Веб интерфейс zabbix](/images/loginnig_zabbix.png)

## Задание 2

Из 2х машин с агентами одна из них - сервер. Агент на нее поставлен в задании 1.
Ниже описан процесс установки агента на вторую машину:

```bash
wget https://repo.zabbix.com/zabbix/8.0/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_8.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_8.0+ubuntu24.04_all.deb
apt update
apt install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

Далее в /etc/zabbix/zabbix_agentd.conf меняем параметр "server=" на сервер забикс.

Через некторое время мы видим что агент появился в сети и данные идут на сервер:

![Активный агент zabbix](/images/hosts-zabbix.png)
![Данные агента zabbix](/images/latest-date-zabbix.png)
