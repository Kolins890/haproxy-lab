Домашнее задание к занятию 2 «Кластеризация и балансировка нагрузки»- Николай Чернов

Задание 1

Запустите два simple python сервера на своей виртуальной машине на разных портах
Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
Настройте балансировку Round-robin на 4 уровне.
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

Решение

```haproxy
global
    log /dev/log local0
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
    user haproxy
    group haproxy
    daemon

defaults
    log global
    mode tcp  # 4-й уровень (TCP)
    option tcplog
    option dontlognull
    timeout connect 5000
    timeout client 50000
    timeout server 50000

frontend http_front
    bind *:8082
    default_backend http_back

backend http_back
    balance roundrobin
    server server1 127.0.0.1:8080 check
    server server2 127.0.0.1:8081 check

listen stats
    bind :9000
    mode http
    stats enable
    stats uri /stats
    stats auth admin:password


Задание 2

Запустите три simple python сервера на своей виртуальной машине на разных портах
Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

Решение
Запустил серию запросов, они будут распределяться между двумя серверами:

for i in {1..10}; do curl http://127.0.0.1/; sleep 1; done
<img width="1361" height="160" alt="image" src="https://github.com/user-attachments/assets/1902254c-fafb-4d50-a163-2895ae132d20" />

<img width="1366" height="515" alt="image" src="https://github.com/user-attachments/assets/26074e1e-7ee2-438b-a79e-d9ca3b8fe207" />

```haproxy
global
    log /dev/log local0
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
    user haproxy
    group haproxy
    daemon

defaults
    log global
    mode http  # 7-й уровень (HTTP)
    option httplog
    option dontlognull
    timeout connect 5000
    timeout client 50000
    timeout server 50000

frontend http_front
    bind *:80
    acl is_example_local hdr(host) -i example.local
    use_backend http_back if is_example_local
    default_backend http_default

backend http_back
    balance roundrobin
    server server1 127.0.0.1:8080 weight 2 check
    server server2 127.0.0.1:8081 weight 3 check
    server server3 127.0.0.1:8082 weight 4 check

backend http_default
    http-request deny deny_status 403

listen stats
    bind :9000
    mode http
    stats enable
    stats uri /stats
    stats auth admin:password
