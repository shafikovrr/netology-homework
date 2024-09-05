# Домашнее задание к занятию "`Disaster recovery и Keepalived`" - `Шафиков Ринат`

### Задание 1

`Приведите ответ в свободной форме........`

1. `Дана схема для Cisco Packet Tracer, рассматриваемая в лекции.`
2. `На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)`
3. `Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).`
4. `Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.`
5. `На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.`
   
### Для Router1

```
en
conf t
int g0/1
standby version 2
standby 1 ip 192.168.1.1
standby 1 priority 50
exit
exit
write
```
```
show standby brief
```
### Для Router2

```
en
conf t
int g0/1
standby version 2
standby 1 ip 192.168.1.1
standby 1 priority preempt
exit
exit
write
```
```
show standby brief
```
![1hsrp](https://github.com/shafikovrr/disaster_recovery_keepalived/blob/main/img/1hsrp.png)
![2hsrp](https://github.com/shafikovrr/disaster_recovery_keepalived/blob/main/img/2hsrp.png)

---

### Задание 2

1. `Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного файла.`
2. `Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах.`
3. `Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.`
4. `Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script.`
5. `На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html`

Bash-скрипт
```
#!/bin/bash

if [[ $(netstat -tulpn | grep LISTEN | grep :80) ]] && [[ -f /var/www/html/index.nginx-debian.html ]]; then
  exit 0
else
  exit 1
fi
```
[Bash-скрипт](https://github.com/shafikovrr/disaster_recovery_keepalived/blob/main/check.sh)

Конфигурационный файл keepalived MASTER (srv1 192.168.0.60)
```
global_defs {
        script_user root
        enable_script_security
}

vrrp_script check {
        script "/etc/keepalived/check.sh"
        interval 3
}

vrrp_instance VI_1 {
        state MASTER
        interface ens18
        virtual_router_id 15
        priority 255
        advert_int 1

        virtual_ipaddress {
                192.168.0.99/24
                }
        track_script {
                check
                }
}
```
[Конфигурационный файл keepalived MASTER (srv1 192.168.0.60)](https://github.com/shafikovrr/disaster_recovery_keepalived/blob/main/keepalived.conf)

Конфигурационный файл keepalived BACKUP (srv2 192.168.0.94)
```
global_defs {
        script_user root
        enable_script_security
}
vrrp_script check {
        script "/etc/keepalived/check.sh"
        interval 3
}
vrrp_instance VI_1 {
        state BACKUP
        interface ens18
        virtual_router_id 15
        priority 200
        advert_int 1
        virtual_ipaddress {
                192.168.0.99/24
                }
        track_script {
                check
                }
}
```
---
![1keepalived_check](https://github.com/shafikovrr/disaster_recovery_keepalived/blob/main/img/1keepalived_check.png)
![2keepalived_check](https://github.com/shafikovrr/disaster_recovery_keepalived/blob/main/img/2keepalived_check.png)

```
sudo apt update && sudo apt upgrade -y
sudo reboot now
sudo nano /etc/hosts
sudo nano /etc/hostname
sudo reboot now
ip a
sudo apt install keepalived 
sudo nano /etc/keepalived/keepalived.conf
syastem start keepalived
systemctl status keepalived
sudo nano /etc/keepalived/check.sh
cd /etc/keepalived/
ls -la
chmod +x check.sh 
sudo apt install net-tools
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl status keepalived
```
