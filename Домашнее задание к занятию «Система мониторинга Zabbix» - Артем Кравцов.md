### Задание 1
Установите Zabbix Server с веб-интерфейсом.


```
Сервер

sudo apt update
sudo apt install postgresql
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
sudo dpkg -i zabbix-release_latest_7.0+debian12_all.deb
sudo apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
sudo zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

*магия с файлом конфигураций*

systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2

```
![Screenshot_20241209_185202](https://github.com/user-attachments/assets/bcbea8fe-152b-4473-bd6b-d20f80a0c5a1)



---

### Задание 2
Установите Zabbix Agent на два хоста.

```
Агент

sudo apt update
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
sudo dpkg -i zabbix-release_latest_7.0+debian12_all.deb
sudo apt update
apt install zabbix-agent

*магия с файлом конфигураций*

systemctl restart zabbix-agent 
systemctl enable zabbix-agent
```
![Screenshot_20241210_013540](https://github.com/user-attachments/assets/c4c9a11e-dbe0-49c3-aa46-6d816cf9b22b)
![Screenshot_20241210_014030](https://github.com/user-attachments/assets/22aae791-04dd-438e-8ff9-bc2239b0afdd)
![Screenshot_20241210_014239](https://github.com/user-attachments/assets/66465e43-6a2a-42b9-9762-8a567d08e404)



