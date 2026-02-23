Домашнее задание к занятию 2 «Кластеризация и балансировка нагрузки»- Николай Чернов

Задание 1

Запустите два simple python сервера на своей виртуальной машине на разных портах
Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
Настройте балансировку Round-robin на 4 уровне.
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

Решение

https://github.com/Kolins890/haproxy-lab/blob/main/haproxy-1.cfg
https://github.com/Kolins890/haproxy-lab/blob/main/HAProxy%201.1.PNG?raw=true
https://github.com/Kolins890/haproxy-lab/blob/main/HAProxy%201.2.PNG?raw=true

Задание 2

Запустите три simple python сервера на своей виртуальной машине на разных портах
Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

Решение

https://github.com/Kolins890/haproxy-lab/blob/main/HAProxy%202.1.PNG?raw=true
https://github.com/Kolins890/haproxy-lab/blob/main/HAProxy%202.2.PNG?raw=true
https://github.com/Kolins890/haproxy-lab/blob/main/haproxy-2.cfg
