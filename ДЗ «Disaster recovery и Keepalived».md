### Задание 1
- Дана для Cisco Packet Tracer, рассматриваемая в лекции.
- На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)
- Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).
- Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.
- На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.

### Решение
https://github.com/model15/netology_homework/blob/b4d3c5cfe0234b695fcbaf3e6a0f1bd94d1fbc6d/1/hsrp_advanced.pkt

![Снимок экрана_20241219_150419](https://github.com/user-attachments/assets/10304757-32c6-421e-a2c6-605b896ec8ab)
![Снимок экрана_20241219_150606](https://github.com/user-attachments/assets/5617d041-de27-4bc1-9de8-d43e07aea537)
![Снимок экрана_20241219_151601](https://github.com/user-attachments/assets/5f4a49ff-7a5f-4ac7-aad5-c7f4a6d6e475)



------


### Задание 2
- Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного [файла](1/keepalived-simple.conf).
- Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах
- Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.
- Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script
- На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html

http_check.sh
```
#! /bin/bash

IP="localhost:80"
HTTP_CODE=$(curl -LI "$IP" -o /dev/null -w '%{http_code}\n' -s)

if [[ $HTTP_CODE -eq 200 ]]; then
    exit 0
else
    exit 1
fi

```
keepalived.conf - master
```
global_defs {
enable_script_security
script_user model
}

vrrp_script http_check {
    script "/etc/keepalived/http_check.sh"
    interval 3
}


vrrp_instance VI_1 {
        state MASTER
        interface enp0s3
        virtual_router_id 100
        priority 95
        advert_int 1


        virtual_ipaddress {
              192.168.1.100/24
        }

        track_script {
            http_check
        }
}        

```
keepalived.conf - backup
```
global_defs {
enable_script_security
script_user model
}

vrrp_script http_check {
    script "/etc/keepalived/http_check.sh"
    interval 3
}


vrrp_instance VI_1 {
        state BACKUP
        interface enp0s3
        virtual_router_id 100
        priority 90
        advert_int 1


        virtual_ipaddress {
              192.168.1.100/24
        }

        track_script {
            http_check
        }
}     
```
![Screenshot_20241222_123627](https://github.com/user-attachments/assets/6a12fd84-ecef-4e1c-bd0c-8e5770f577a7)

![Screenshot_20241222_123734](https://github.com/user-attachments/assets/8d1e0724-c606-4548-bed4-fbae1c158e8c)
