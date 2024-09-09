# Задание 1
#### Описание задания
Перед вами стоит задача настроить DHCP-сервер в Linux.

#### Требование к результату
Вы должны отправить файл с выполненным заданием.
К выполненной задаче добавьте скриншоты настройки и работающего DHCP-сервера.
Для выполнения заданий вы можете использовать дистрибутив на ваш выбор (Deb-дистрибутив или CentOs).
#### Процесс выполнения
Запустите программу VirtualBox
В программе VirtualBox загрузите операционную систему Ubuntu, если она у вас не установлена в качестве основной системы.
Установите DHCP-сервер: sudo apt-get install isc-dhcp-server -y
Настройте DHCP-сервер так, чтобы клиенту выдавался ip-адрес, ip-адрес dns-сервера и максимальное время аренды адреса было 8 часов.
Запустите службу DHCP: sudo systemctl start isc-dhcp-server.service

# Решение 1
1) Установка
```
sudo apt install isc-dhcp-server -y
```
2) Настройка DHCP
```
sudo nano /etc/default/isc-dhcp-server
```
![alt text](img/image14.png)
```
sudo nano /etc/dhcp/dhcpd.conf
```
![alt text](img/image2.png)
![alt text](img/image13.png)

Проверка:
```
systemctl status isc-dhcp-server
```
![alt text](img/image4.png)
Получение IP через настроенный dhcp сервер
![alt text](img/image15.png)

# Задание 2
#### Описание задания
Перед вами стоит задача создать и настроить PXE-сервер.

#### Требование к результату
Вы должны отправить файлы с выполненным заданием
К выполненной задаче добавьте скриншоты с конфигурацией PXE-сервера и его работоспособность.
Для выполнения заданий вы можете использовать дистрибутив на ваш выбор (Deb-дистрибутив или CentOs).
#### Процесс выполнения
Запустите программу VirtualBox
В программе VirtualBox загрузите операционную систему Ubuntu, если она у вас не установлена в качестве основной системы.
Установите TFTP-сервер: sudo apt-get install tftpd-hpa
Создайте директорию для TFTP-сервера.
В файле “tftp-hpa” TFTP-сервера укажите выделенный ip-адрес или адрес loopback-интерфейса.
Также в дополнительных опциях TFTP-сервера разрешите создавать новые файлы.
Перезагрузите TFTP-сервер: service tftp-hpa restart
Создайте в директории TFTP-сервера какой-нибудь файл
Проверьте работоспособность PXE-сервера, либо загрузив с него файл по сети, либо подключившись TFTP-клиентом.
Выполните скриншоты и ответ внесите в комментарии к решению задания.

# Решение 2

Вывод команды 
```
ip a
```
![alt text](img/image5.png)

Установите TFTP-сервер: 
```
sudo apt-get install tftpd-hpa
```
![alt text](img/image7.png)

Создаем директорию fttp
```
mkdir -p /tftp
```
![alt text](img/image3.png)

назначим на нее права доступа
```
chmod -R 777 /tftp/
chown -R nobody:nogroup /tftp/
```
![alt text](img/image12.png)

Создаем копию файла конфигурации tftp и отредактируем его. Перезапускаем tftp сервер.
![alt text](img/image10.png)
![alt text](img/image6.png)
Проверяем статус tftp
![alt text](img/image11.png)
Создадим файл в директории tftp сервера
```
echo test tftp server > /tftp/netology-tftp
```
![alt text](img/image1.png)

Подключаемся tftp клиентом к tftp серверу
```
tftp 192.168.0.43
```
![alt text](img/image9.png)

Скачаем файл netology-tftp в домашнюю папку
```
get netology-tftp
```
![alt text](img/image8.png)
