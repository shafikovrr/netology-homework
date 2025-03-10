# Домашнее задание к занятию "`Кластеризация и балансировка нагрузки`" - `Шафиков Ринат`

### Задание 1

1. Запустите два simple python сервера на своей виртуальной машине на разных портах
2. Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
3. Настройте балансировку Round-robin на 4 уровне.
4. На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

### Решение

1. Запустите два simple python сервера на своей виртуальной машине на разных портах
```
mkdir http1
cd http1
nano index.html
```
```
Server 1 :8888
```
```
python3 -m http.server 8888 --bind 0.0.0.0 
```

```
mkdir http2
cd http2
nano index.html
```
```
Server 2 :9999
```
```
python3 -m http.server 9999 --bind 0.0.0.0
```

2 Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
```
sudo apt install nginx
curl http://localhost
sudo apt install haproxy
sudo nano /etc/haproxy/haproxy.cfg
```
3 Настройте балансировку Round-robin на 4 уровне.

```
listen stats # веб-страница со статистикой
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example # секция фронтенд
        mode    http
        bind    :8088
        #default_backend        web_servers

backend web_servers # секция бэкенд
        mode    http
        balance roundrobin
        option  httpchk
        http-check      send meth GET uri /index.html
        server  s1 127.0.0.1:8888 check
        server  s2 127.0.0.1:9999 check

listen web_tcp
        bind    :1325

        server s1 127.0.0.1:8888 check inter 3s
        server s2 127.0.0.1:9999 check inter 3s
```

```
sudo systemctl reload haproxy
```
4 На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

[Конфигурационный файл HaProxy](files/haproxy.cfg)

![HaProxy_L4](img/HaProxy_L4.png)

---

### Задание 2
1 Запустите три simple python сервера на своей виртуальной машине на разных портах
2 Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
3 HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
4 На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

### Решение 2

1 Запустите три simple python сервера на своей виртуальной машине на разных портах

Создаем директории 
```
mkdir http1 http2 http3
```
```
cd http1
nano index.html
```
```
Server 1 :7777
```
```
cd http2
nano index.html
```
```
Server 2 :8888
```
```
cd http3
nano index.html
```
```
Server 3 :9999
```
2 Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
```
sudo nano /etc/haproxy/haproxy.cfg 
```
```
listen stats # веб-страница со статистикой
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example # секция фронтенд
        mode    http
        bind    :8088
        default_backend        web_servers
       
backend web_servers # секция бэкенд
        mode    http
        balance roundrobin
        option  httpchk
        http-check      send meth GET uri /index.html
        server  s1 127.0.0.1:7777 weight 2 check
        server  s2 127.0.0.1:8888 weight 3 check
        server  s3 127.0.0.1:9999 weight 4 check
```
3 HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
```
listen stats # веб-страница со статистикой
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example # секция фронтенд
        mode    http
        bind    :8088
        #default_backend        web_servers
        acl ACL_example.local hdr(host) -i example.local
        use_backend web_servers if ACL_example.local

backend web_servers # секция бэкенд
        mode    http
        balance roundrobin
        option  httpchk
        http-check      send meth GET uri /index.html
        server  s1 127.0.0.1:7777 weight 2 check
        server  s2 127.0.0.1:8888 weight 3 check
        server  s3 127.0.0.1:9999 weight 4 check
```
4 На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

[Конфигурационный файл HaProxy](files/haproxy2.cfg)

![HaProxy_L4](img/HaProxy_L7.png)


