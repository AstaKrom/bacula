# bacula
- Установка тулзов:
sudo apt install net-tools

- установка bacula:
sudo apt install bacula -y

- Настройка папок:
cd /etc/bacula
sudo mkdir -p /bacula/{backup,restore}
sudo chown -R bacula:bacula /{backup,restore}

- Установка rsync:
sudo apt install rsync

- Настройка:
sudo nano /etc/default/rsync
RSYNC_ENABLE = true

- Файл конфигурации:
sudo nano /etc/rsyncd.conf

pid file = /var/run/rsyncd.pid
log file = /var/log/rsybcd.log
transfer logging = true
munge symlinks = yes
hosts allow = 192.168.56.14
use chroot = no
ignore errors = yes
[data]
path = /etc/default
uid = root
read only = yes
list = yes
comment = Data backup Dir
aith users = backup
secret file = /etc/rsyncd.scrt

- Файл с секретом:
sudo nano /etc/rsyncd.scrt

backup:12345

-Ограничить права
sudo chmod 0600 /etc/rsyncd.scrt

- ребут rsync
sudo systemctl restart rsync

- Проверка работы
sudo netstat -tulnp | grep rsync

- на второй машине создаем bash скрипт
nano script.sh

#!/bin/bash
date
# Папка, куда будем складывать архивы — ее либо сразу создать либо не создавать а положить в уже существующие
syst_dir=/backup/
# Имя сервера, который архивируем
srv_name=bacula
# Адрес сервера, который архивируем
srv_ip=192.168.56.13
# Пользователь rsync на сервере, который архивируем
srv_user=backup
# Ресурс на сервере для бэкапа
srv_dir=data
echo "Start backup ${srv_name}"
# Создаем папку для инкрементных бэкапов
mkdir -p ${syst_dir}${srv_name}/increment/
/usr/bin/rsync -avz --progress --delete --password-file=/etc/rsyncd.scrt ${srv_user}@${srv_ip}::${srv_dir} ${syst_dir}${srv_name}/current/ --backup --backup-dir=${syst_dir}${srv_name}/increment/`date +%Y-%m-%d`/
/usr/bin/find ${syst_dir}${srv_name}/increment/ -maxdepth 1 -type d -mtime +30 -exec rm -rf {} \;
#/usr/bin/rsync -avz --delete --password-file=/etc/rsyncd.scrt $srv_user@$srv_ip $srv_data$syst_dir
date
echo "Finish backup ${srv_name}"

- Созадем серкретный файл, указываем только пароль
sudo nano /etc/rsyncd.scrt
12345

- ограничиваем права
sudo chmod 0600 /etc/rsyncd.scrt

- Запускаем скрипт
