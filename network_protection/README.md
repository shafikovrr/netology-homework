# Домашнее задание к занятию "`Защита сети`" - `Шафиков Ринат`

---

### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

sudo nmap -sA < ip-адрес >

sudo nmap -sT < ip-адрес >

sudo nmap -sS < ip-адрес >

sudo nmap -sV < ip-адрес >

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.

В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.

### Решение 1

ip_kali 192.168.0.67
ip_host 192.168.0.9 eth0

/var/lib/suricata/rules/suricata.rules

```
sudo apt update && sudo apt upgrade -y
sudo apt install fail2ban
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
sudo systemctl status fail2ban
sudo apt install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt install suricata
sudo suricata-update
sudo systemctl status suricata
ip a
sudo nano /etc/suricata/suricata.yaml
sudo systemctl restart suricata
```
```
sudo nmap -sA 192.168.0.9
```

Сообщений в логах нет (nmap не смог определить операционную систему)

![k_sA](img/k_sA.png)

```
sudo nmap -sT 192.168.0.9
```
Обнаружен открытый порт ssh 22 (соединение к хосту по ssh)

![k_sT](img/k_sT.png)

Surikata реагирует на сканирование портов (mySQL port 3306, PostgreSQL port 5432, Oracle SQL port 1521) и класифицирует их как Potentially Bad Traffic.

![s_nmap_sT](img/s_nmap_sT.png)

```
sudo nmap -sS 192.168.0.9
```
Обнаружен открытый порт ssh 22 (соединение к хосту по ssh)

![k_sS](img/k_sS.png)

Surikata реагирует на сканирование портов (mySQL port 3306, PostgreSQL port 5432, Oracle SQL port 1521) и класифицирует их как Potentially Bad Traffic.

![s_nmap_sS](img/s_nmap_sS.png)

```
sudo nmap -sV 192.168.0.9
```

Получена более полная информация. Обнаружен открытый порт ssh 22 (соединение к хосту по ssh - Ubuntu, OS Linux)

![k_sV](img/k_sV.png)

suricata:

Surikata реагирует на сканирование портов (mySQL port 3306, PostgreSQL port 5432, Oracle SQL port 1521) и класифицирует их как Potentially Bad Traffic.

![*s_nmap_sV](img/s_nmap_sV.png)

Появилось сообщение о закрытии удаленным хостом (kalilinux) соединения во время процесса обмена ключами.

fail2ban:

![f2b_nmap_sV](img/f2b_nmap_sV.png)






---

### Задание 2

`Приведите ответ в свободной форме........`

Проведите атаку на подбор пароля для службы SSH:

hydra -L users.txt -P pass.txt < ip-адрес > ssh

1. Настройка hydra:
   - создайте два файла: users.txt и pass.txt;
   - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.
Дополнительная информация по hydra: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:
   - открыть файл /etc/fail2ban/jail.conf,
   - найти секцию ssh,
   - установить enabled в true.
Дополнительная информация по Fail2Ban:https://putty.org.ru/articles/fail2ban-ssh.html.

В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.

### Решение 2


Установим hydra на kalilinux

```
sudo apt install hydra
```

Произведем подбор пароля

```
sudo hydra -t 4 -L users.txt -P pass.txt 192.168.0.61 ssh
```
Пароль подобран

![k_pass_ok](img/k_pass_ok.png)

![tail_auth_log_pass_ok](img/tail_auth_log_pass_ok.png)

Hydra заблокирована fail2ban

![k_pass_no](img/k_pass_no.png)

![f2b_pass_block](img/f2b_pass_block.png)

---
