# Домашнее задание к занятию "`Очереди RabbitMQ`" - `Шафиков Ринат`

### Задание 1. Установка RabbitMQ

Используя Vagrant или VirtualBox, создайте виртуальную машину и установите RabbitMQ. Добавьте management plug-in и зайдите в веб-интерфейс.
Итогом выполнения домашнего задания будет приложенный скриншот веб-интерфейса RabbitMQ.

### Решение 1

Vagrant файл
```
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.network "private_network", ip: "192.168.56.100"
  config.vm.disk :disk, size: "20GB", primary: true
  config.vm.provider "virtualbox" do |vb|
    vb.name = "rabbitmq"
    vb.gui = false
    vb.memory = "6144"
    vb.cpus = "2"
end
config.vm.hostname = "rabbitmq"
config.vm.network :forwarded_port, guest: 5672, host: 5672
config.vm.network :forwarded_port, guest: 15672, host: 15672

  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    sudo apt update
    sudo apt upgrade
    sudo apt install openssh-server 
    sudo apt install rabbitmq-server -y
    sudo systemctl start rabbitmq-server
    sudo systemctl enable rabbitmq-server
    sudo rabbitmq-plugins enable rabbitmq_management
    sudo systemctl restart rabbitmq-server
    sudo ufw allow ssh
    sudo ufw enable
    sudo ufw allow 5672,15672,4369,25672/tcp
    echo -e "192.168.56.100\tubuntu-jammy\tubuntu-jammy" >> /etc/hosts
  SHELL
end
```
Установка системы и настройка доступа пользователя
```
vagrant up
vagrant ssh
sudo rabbitmqctl add_user adrin password
sudo rabbitmqctl set_permissions -p / adrin ".*" ".*" ".*"
sudo rabbitmqctl set_user_tags adrin administrator
```

![rabbitmq_install](img/rabbitmq_install.png)

---

### Задание 2

`Задание 2. Отправка и получение сообщений
Используя приложенные скрипты, проведите тестовую отправку и получение сообщения. Для отправки сообщений необходимо запустить скрипт producer.py.

Для работы скриптов вам необходимо установить Python версии 3 и библиотеку Pika. Также в скриптах нужно указать IP-адрес машины, на которой запущен RabbitMQ, заменив localhost на нужный IP.

```pip install pika```

Зайдите в веб-интерфейс, найдите очередь под названием hello и сделайте скриншот. После чего запустите второй скрипт consumer.py и сделайте скриншот результата выполнения скрипта

В качестве решения домашнего задания приложите оба скриншота, сделанных на этапе выполнения.

Для закрепления материала можете попробовать модифицировать скрипты, чтобы поменять название очереди и отправляемое сообщение.`

### Решение 2

Измененный файл producer.py

```
#!/usr/bin/env python
# coding=utf-8
import pika
credentials = pika.PlainCredentials('adrin', 'password')
connection = pika.BlockingConnection(pika.ConnectionParameters('192.168.56.100',
                                                            5672,
                                                            '/',
                                                            credentials))
channel = connection.channel()
channel.queue_declare(queue='hello')
channel.basic_publish(exchange='', routing_key='hello', body='Hello Netology!')
connection.close()
```

![rabbitmq_hello](img/rabbitmq_hello.png)

```
#!/usr/bin/env python
# coding=utf-8
import pika
credentials = pika.PlainCredentials('adrin', 'password')
connection = pika.BlockingConnection(pika.ConnectionParameters('192.168.56.100',
                                                            5672,
                                                            '/',
                                                            credentials))
channel = connection.channel()
channel.queue_declare(queue='hello')


def callback(ch, method, properties, body):
    print(" [x] Received %r" % body)


# channel.basic_consume(callback, queue='hello', no_ack=True)
channel.basic_consume('hello', callback, auto_ack=True)
channel.start_consuming()
```

![rabbitmq_consumers](img/rabbitmq_consumers.png)

---

### Задание 3

Задание 3. Подготовка HA кластера
Используя Vagrant или VirtualBox, создайте вторую виртуальную машину и установите RabbitMQ. Добавьте в файл hosts название и IP-адрес каждой машины, чтобы машины могли видеть друг друга по имени.

Пример содержимого hosts файла:

```
cat /etc/hosts
192.168.0.10 rmq01
192.168.0.11 rmq02
```

После этого ваши машины могут пинговаться по имени.

Затем объедините две машины в кластер и создайте политику ha-all на все очереди.

В качестве решения домашнего задания приложите скриншоты из веб-интерфейса с информацией о доступных нодах в кластере и включённой политикой.

Также приложите вывод команды с двух нод:

```
rabbitmqctl cluster_status
```

Для закрепления материала снова запустите скрипт producer.py и приложите скриншот выполнения команды на каждой из нод:

```
rabbitmqadmin get queue='hello'
```
После чего попробуйте отключить одну из нод, желательно ту, к которой подключались из скрипта, затем поправьте параметры подключения в скрипте consumer.py на вторую ноду и запустите его.

Приложите скриншот результата работы второго скрипта.

### Решение 3

![Название скриншота3](ссылка на скриншот3)

---

### Задание 4. Ansible playbook

Напишите плейбук, который будет производить установку RabbitMQ на любое количество нод и объединять их в кластер. При этом будет автоматически создавать политику ha-all.

Готовый плейбук разместите в своём репозитории.

### Решение 4

---
