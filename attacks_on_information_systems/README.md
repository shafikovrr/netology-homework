# Домашнее задание к занятию "`Уязвимости и атаки на информационные системы`" - `Шафиков Ринат`

---

### Задание 1

`Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.
Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.
Просканируйте эту виртуальную машину, используя nmap.
Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.
Сами уязвимости можно поискать на сайте` 

```
https://www.exploit-db.com/
```

`Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.
Ответьте на следующие вопросы:`
- `Какие сетевые службы в ней разрешены?`
- `Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)`

`Приведите ответ в свободной форме.`

### Решение 1

```
sudo apt update && sudo apt upgrate -y
sudo apt install nmap
```
```
sudo nmap -sV 192.168.0.78
```

![sudo_nmap_sV_ip](img/sudo_nmap_sV_ip.png)

Уязвимости

- 21/tcp   open  ftp         vsftpd 2.3.4

`vsftpd 2.3.4 - Backdoor Command Execution
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)`

- 139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
- 445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

`Samba 3.5.0 < 4.4.14/4.5.10/4.6.4 - 'is_known_pipename()' Arbitrary Module Load (Metasploit)`

- 2121/tcp open  ftp         ProFTPD 1.3.1

`ProFTPd IAC 1.3.x - Remote Command Execution`

---

### Задание 2

`Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.
Запишите сеансы сканирования в Wireshark.
Ответьте на следующие вопросы:`

- `Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?`
- `Как отвечает сервер?`
- `Приведите ответ в свободной форме.`

### Решение 2

1. Cканирование Metasploitable в режиме SYN

3306/tcp open  mysql

```
sudo nmap -sS 192.168.0.78
```
![SYN_nmap_-sS](img/SYN_nmap_-sS.png)

![W_SYN_nmap_-sS](img/W_SYN_nmap_-sS.png)

2. Cканирование Metasploitable в режиме FIN

```
sudo nmap -sF 192.168.0.78
```

![FIN_nmap_-sF](img/FIN_nmap_-sF.png)

3. Cканирование Metasploitable в режиме Xmas

```
sudo nmap -sX 192.168.0.78
```

![Xmas_nmap_-sX](img/Xmas_nmap_-sX.png)

4. Cканирование Metasploitable в режиме UDP.

```
sudo nmap -sU 192.168.0.78
```

![UDP_nmap_-sU](img/1UDP_nmap_-sU.png)














---
