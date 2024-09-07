# Задание 1
#### ВАЖНО
Задание является сквозным и составлено на основе практической задачи из домашней работы [“L3-сеть”].

Описание задания
Перед вами стоит задача настроить IPv6-адреса на устройствах в локальной сети.

#### Требование к результату
    1. Вы должны отправить файл .pkt с выполненным заданием
    2. К выполненной задаче добавьте скриншоты с доступностью устройств между собой и ответы на вопросы.
#### Процесс выполнения
    1. Запустите программу Cisco Packet Tracer
    2. В программе Cisco Packet Tracer загрузите предыдущую практическую работу.
    3. На маршрутизаторе локальной сети (где три коммутатора в кольце), настройте выдачу IPv6-адресов по DHCP.
    4. Все устройства должны получить IPv6-адреса.
    5. Проверьте доступность каждого типа устройств с маршрутизатора куда они подключены, командой ping.
# Решение 1
    1) На маршрутизаторе локальной сети (где три коммутатора в кольце), настроим выдачу IPv6-адресов по DHCP.
#### Настройка маршрутизатора
```
Router>en
Router#conf t
Router(config)#interface gigabitEthernet 0/0/1
Router(config-if)#ipv6 enable
Router(config-if)#ipv6 add 2001:db8:acad:a::1/64
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#ipv6 unicast-routing 
Router(config)#interface GigabitEthernet 0/0/1.10
Router(config-subif)#ipv6 address 2001:db8:acad:10::1/64
Router(config-subif)#exit
Router(config)#interface GigabitEthernet 0/0/1.20
Router(config-subif)#ipv6 address 2001:db8:acad:20::1/64
Router(config-subif)#exit
Router(config)#interface GigabitEthernet 0/0/1.30
Router(config-subif)#ipv6 address 2001:db8:acad:30::1/64
Router(config-subif)#exit
Router(config)#end
Router#wr mem
```

#### Настройка свича 1-3

```
Switch#conf t
Switch(config)#sdm PREfer DUal-ipv4-and-ipv6 default 
Switch(config)#end
Switch#wr mem 
Switch#reload
```

![image1](img/image1.png)
