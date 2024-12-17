# Домашнее задание к занятию "`Disaster recovery и Keepalived`" - `Татаринцев Алексей`



---

### Задание 1


1. `Скачиваем готовый стенд `
2. `Заходим в интерфейс роутера 2`
3. `Прописываем настройки`
```
Router1# configure terminal
Router1(config)# interface GigabitEthernet0/0
Router1(config-if)# standby 0 ip 192.168.0.1    
Router1(config-if)# standby 0 priority 95        
Router1(config-if)# standby 0 preempt            
Router1(config-if)# exit

Router1(config)# interface GigabitEthernet0/1
Router1(config-if)# standby 1 ip 192.168.1.1     
Router1(config-if)# standby 1 priority 95        
Router1(config-if)# standby 1 preempt            
Router1(config-if)# exit

```
![1](https://github.com/Foxbeerxxx/Disaster-recovery-Keepalived/blob/main/img/img1.png)`

4. `Заходим на PC0 и начинаем пинговать сервер 192.168.1.50, рвем сеть к роутеру первой группы и включаем режим симуляции`
![2](https://github.com/Foxbeerxxx/Disaster-recovery-Keepalived/blob/main/img/img2.png)`

![3](https://github.com/Foxbeerxxx/Disaster-recovery-Keepalived/blob/main/img/img3.png)`

5. `Как итог переключение отработало.Пакеты побежали по группе второго роутра`


---

### Задание 2



1. `Запускаю две виртуальные машины ppc1 - 192.168.1.10 и ppc2 - 192.168.1.5`
2. `На каждую ммашину насчинаю устанавливать необзодимые сервисы `
```
sudo apt update
sudo apt install keepalived
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl start keepalived
```

3. `Проверяю сервисы и добавляю в автозагрузку`
![4](https://github.com/Foxbeerxxx/Disaster-recovery-Keepalived/blob/main/img/img4.png)`

4. `Редактирую на обоих ВМ conf файл`
```
sudo nano /etc/keepalived/keepalived.conf
```
`Содержимое файла yf ppc1`
```
vrrp_script check_webserver {
    script "/etc/keepalived/check_webserver.sh"
    interval 3
    weight -10
}
vrrp_instance VI_1 {
        state MASTER
        interface enp0s8
        virtual_router_id 15
        priority 255
        advert_int 1

        virtual_ipaddress {
              192.168.111.15/24
        }

}
```

5. `Редактирую на обоих ВМ conf файл`
```
sudo nano /etc/keepalived/keepalived.conf
```
`Содержимое файла yf ppc2`
```
vrrp_script check_webserver {
    script "/etc/keepalived/check_webserver.sh"
    interval 3
    weight -10
}
vrrp_instance VI_1 {
        state BACKUP
        interface enp0s8
        virtual_router_id 15
        priority 250
        advert_int 1

        virtual_ipaddress {
              192.168.111.15/24
        }

}
```
6. `Записываю скрипт в папку keepalived`

```
sudo nano  /etc/keepalived/check_webserver.sh
```
`Содержимое файла`
```
#!/bin/bash

WEB_SERVER_IP="127.0.0.1"
WEB_SERVER_PORT="80"
INDEX_FILE="/var/www/html/index.html"

# Проверка доступности порта
if ! nc -z $WEB_SERVER_IP $WEB_SERVER_PORT; then
    exit 1
fi

# Проверка существования файла index.html
if [[ ! -f $INDEX_FILE ]]; then
    exit 1
fi

exit 0

```
7. `Перезапускаю сервис Keepalived`

```
sudo systemctl restart keepalived
```
`Смотрим, что получилось, на двух машинах забиваем ip a`
![5](https://github.com/Foxbeerxxx/Disaster-recovery-Keepalived/blob/main/img/img5.png)`

`Плавающий ip находится на Мастере`

8. `Останавливаю службу nginx`
```
sudo systemctl stop nginx
```
9. `Плавающий IP уходит на Backup`

![6](https://github.com/Foxbeerxxx/Disaster-recovery-Keepalived/blob/main/img/img6.png)`

