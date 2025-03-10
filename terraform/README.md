## Задание 1
Это задание для самостоятельной отработки навыков и не предполагает обратной связи от преподавателя. Его выполнение не влияет на завершение модуля. Но мы рекомендуем не откладывать его выполнение, так как в следующем домашнем задании вы будете использовать Terraform для решения рабочей задачи.
Задание
Установите Terraform на компьютерную систему (виртуальную или хостовую), используя лекцию или инструкцию.
В связи с недоступностью ресурсов для загрузки Terraform на территории РФ, вы можете воспользоваться VPN или использовать зеркало из репозитория по ссылке.

## Решение 1

#### Установка Terraform
Инструкция по установке Terraform на Ubuntu server 22.04
#### Обновить систему
Обновить систему
```
sudo apt update && sudo apt upgrade -y
```
#### Загрузить Terraform (с зеркала)
Создаем дирректорию и открываем ее
```
mkdir terraform
cd terraform/
```
Загружаем terraform с зеркала
```
wget https://hashicorp-releases.yandexcloud.net/terraform/1.6.3/terraform_1.6.3_linux_amd64.zip
```
Распаковать скачанный архив
Установим (при необходимости) unzip и распакуем скачанный архив
```
sudo apt install unzip
unzip terraform_1.6.3_linux_amd64.zip
```
Удалить архива
Удалим распакованный архив
```
rm terraform_1.6.3_linux_amd64.zip
```
Добавить в переменную PATH путь к папке с исполняемым файлом Terraform
После распаковки добавим путь к папке, в которой находится исполняемый файл Terraform, в переменную PATH:
```
export PATH=$PATH:/home/adrin/terraform
```
Проверка версии Terraform
```
terraform --version
```
![terraform-version](img/terraform-version.png)