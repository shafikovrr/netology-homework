
Задание 1
1. Запустите два simple python сервера на своей виртуальной машине на разных портах
2. Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
3. Настройте балансировку Round-robin на 4 уровне.
4. На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

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


```
sudo apt install nginx
curl http://localhost
sudo apt install haproxy
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
[Конфигурационный файл HaProxy](https://github.com/shafikovrr/HAProxy/blob/main/haproxy.cfg)

![1hsrp](https://github.com/shafikovrr/disaster_recovery_and_keepalived/blob/main/img/1hsrp.png)

