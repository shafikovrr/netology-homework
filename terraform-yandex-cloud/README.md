# Задача 1

## Выполните действия, приложите скриншот скриптов, скриншот выполненного проекта.

От заказчика получено задание: при помощи Terraform и Ansible собрать виртуальную инфраструктуру и развернуть на ней веб-ресурс.

В инфраструктуре нужна одна машина с ПО ОС Linux, двумя ядрами и двумя гигабайтами оперативной памяти.

Требуется установить nginx, залить при помощи Ansible конфигурационные файлы nginx и веб-ресурса.

Секретный токен от yandex cloud должен вводится в консоли при каждом запуске terraform.

Для выполнения этого задания нужно сгенирировать SSH-ключ командой ssh-kengen. Добавить в конфигурацию Terraform ключ в поле:

```
 metadata = {
    user-data = "${file("./meta.txt")}"
  }
```
В файле meta прописать:
```
 users:
  - name: user
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa  xxx
```
Где xxx — это ключ из файла /home/"name_ user"/.ssh/id_rsa.pub. Примерная конфигурация Terraform:
```
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
}

variable "yandex_cloud_token" {
  type = string
  description = "Введите секретный токен от yandex_cloud"
}

provider "yandex" {
  token     = var.yandex_cloud_token #секретные данные должны быть в сохранности!! Никогда не выкладывайте токен в публичный доступ.
  cloud_id  = "xxx"
  folder_id = "xxx"
  zone      = "ru-central1-a"
}

resource "yandex_compute_instance" "vm-1" {
  name = "terraform1"

  resources {
    cores  = 2
    memory = 2
  }

  boot_disk {
    initialize_params {
      image_id = "fd87kbts7j40q5b9rpjr"
    }
  }

  network_interface {
    subnet_id = yandex_vpc_subnet.subnet-1.id
    nat       = true
  }
  
  metadata = {
    user-data = "${file("./meta.txt")}"
  }

}
resource "yandex_vpc_network" "network-1" {
  name = "network1"
}

resource "yandex_vpc_subnet" "subnet-1" {
  name           = "subnet1"
  zone           = "ru-central1-b"
  network_id     = yandex_vpc_network.network-1.id
  v4_cidr_blocks = ["192.168.10.0/24"]
}

output "internal_ip_address_vm_1" {
  value = yandex_compute_instance.vm-1.network_interface.0.ip_address
}
output "external_ip_address_vm_1" {
  value = yandex_compute_instance.vm-1.network_interface.0.nat_ip_address
}
```
В конфигурации Ansible указать:
* внешний IP-адрес машины, полученный из output external_ ip_ address_ vm_1, в файле hosts;
* доступ в файле plabook *yml поля hosts.

```
- hosts: 138.68.85.196
  remote_user: user
  tasks:
    - service:
        name: nginx
        state: started
      become: yes
      become_method: sudo
```
Провести тестирование.


---


# Решение 1

## Установка Terraform

Инструкция по установке Terraform на Ubuntu server 22.04

### Обновить систему

Обновить систему

```
sudo apt update && sudo apt upgrade -y
```

### Загрузить Terraform (с зеркала)

Создаем дирректорию и открываем ее
```
mkdir terraform
cd terraform/
```
Загружаем terraform с зеркала

```
wget https://hashicorp-releases.yandexcloud.net/terraform/1.6.3/terraform_1.6.3_linux_amd64.zip
```

### Распаковать скачанный архив

Установим (при необходимости) unzip и распокуем скачанный архив

```
sudo apt install unzip
unzip terraform_1.6.3_linux_amd64.zip
```

### Удалить архива

Удалим распакованный архив

```
rm terraform_1.6.3_linux_amd64.zip
```
### Добавить в переменную PATH путь к папке с исполняемым файлом Terraform 

После распаковки добавим путь к папке, в которой находится исполняемый файл Terraform, в переменную PATH:

```
# действует до перезагрузки
export PATH=$PATH:/home/adrin/terraform 
```
### Проверка версии Terraform

```
terraform --version
```

### Создаем файл main.tf 

```
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
  required_version = ">= 0.13"
}

# вывод токена при каждом запуске terraform 
# https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb

variable "yandex_cloud_token" {
  type = string
  description = "y0_AgAAAAAGdhqDAATuwQAAAADxVkChy8lgcyHVSnChPj4Br75IEyvd9So" #секретный токе (публиковать нельзя!!!)
}

# Настройка провайдера yandex
provider "yandex" {
  token     = var.yandex_cloud_token  #секретные данные должны быть в сохранности!! Никогда не выкладывайте токен в публичный доступ.
  cloud_id  = "b1ghmbbcc16prrludk63"  #_https://console.cloud.yandex.ru/cloud/b1ghmbbcc16prrludk63
  folder_id = "b1gltt4aeqoofm7e2pnj"  #_https://console.cloud.yandex.ru/folders/b1gltt4aeqoofm7e2pnj
  zone      = "ru-central1-b"         #_https://cloud.yandex.ru/docs/overview/concepts/geo-scope
}

# Параметры машины vm-1
resource "yandex_compute_instance" "vm-1" {

  name                        = "terraform1"

  resources {
    cores         = 2
    memory        = 2
  }
# image_id берем с yandex cloud (при создании вм - при выборе операционной системы - нажимаем на вопрос справа от назвния)
  boot_disk {
    initialize_params {
      image_id = "fd87e3vsemiab8q1tl0h"
    }
  }
# настройка nat (внешняя сеть)
  network_interface {
    subnet_id = yandex_vpc_subnet.subnet-1.id
    nat       = true
  }

  metadata = {
    user-data = "${file("./meta.yaml")}"
  }
}

resource "yandex_vpc_network" "network-1" {
  name = "network1"
}
# настройка внутренней сети
resource "yandex_vpc_subnet" "subnet-1" {
  name           = "subnet1"
  zone           = "ru-central1-b"
  network_id     = yandex_vpc_network.network-1.id
  v4_cidr_blocks = ["192.168.10.0/24"]
}
# Вывод информаци по IP (внутренний ip и внешний ip)
output "internal_ip_address_vm_1" {
  value = yandex_compute_instance.vm-1.network_interface.0.ip_address
}
output "external_ip_address_vm_1" {
  value = yandex_compute_instance.vm-1.network_interface.0.nat_ip_address
}
```
### Создаем файл meta.yaml 

Генерируем параметры ssh содединения (ключи)
```
ssh-keygen 
```

Задаем имя пользователя, его группу и параметры подключения по ssh

```
cloud-config
users:
  - name: user
    groups: sudo
    shell: /bin/bash
    sudo: 'ALL=(ALL) NOPASSWD:ALL'
    ssh-authorized-keys:
      - ssh-rsa AAAAB3NzaC1yc2EAAA...xcLPs8= adrin@terraform-srv
# ssh-authorized-keys - id_rsa.pub
```

### Создаем виртуальную машину

Инициализируем провайдера, указанного в конфигурационном файле main.tf, что позволит работать с ресурсами и источниками данных провайдера.
```
terraform init
```

Произведем проверку

```
terraform plan
```

Создаем виртуальную машину

```
terraform apply
```
Проверяем резуьтат на yandex cloud

## Установка Ansible на хосте с terraform

Установить ansible 
```
sudo apt install ansible
```
Создать в папке с ansible файл hosts

```
cd .ansible/
nano hosts
```
Содержимое файла (достаточно указать внешний ip адресс созданной ранее виртуальной машины)
```
https://console.cloud.yandex.ru/folders/b1gltt4aeqoofm7e2pnj/compute/instances
```
Проверить соединение с хоста с управляемой (виртуальной) сашиной

```
ansible all -m ping -v
```

Если ansible.conf отсутствует в папке с ansible - сгенерировать его
```
ansible-config init --disabled > ansible.cfg
```
Произвести настройку ansible.conf

```
private_key_file=/home/adrin/.ssh/id_rsa
remote_user=user
inventory=/home/adrine/.ansible/hosts
```
При необходимости меняем значения в блоке
```
[privilege_escalation]
```
Создаем роль для установки nginx
```
ansible-galaxy init nginx
```
В дирректории ansible создаем файл nginx.yml
```
nano nginx.yml
```
следующего содержания

```
---
- hosts: 158.160.16.194
  remote_user: user
  gather_facts: no
  roles:
  - include_role:
    name: nginx
    tasks_from: main
    become: yes
    become_method: sudo
```
В созданной дирректори роли nginx в папке tasks отредактируем файл main.yml
```
---
# установка nginx
- name: Install nginx  (state=present is optional)
  become: yes
  ansible.builtin.apt:
    name: nginx
    state: present

# замена файла конфигурации nginx
- name: Replace nginx.conf
  template:
    src=templates/nginx.conf
    dest=/etc/nginx/nginx.conf

#создание начальной страницы index.html
- name: "Add Index page"
  template:
    src: "index.html.j2"
    dest: "/var/www/html/index.html"
    owner: root
    group: root
    mode: 0755
  notify: "Restart nginx"

```
В папку templates скопируем (согласно заданию) файлы index.html.j2 (стартовая страница c добавкой расширения "j2") и nginx.conf (конфигурация nginx)

Запускаем на выполнение созданную роль
```
ansible-playbook nginx.yml -v
```
