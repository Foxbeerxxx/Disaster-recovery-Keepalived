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
4. `Редактирую на обоих ВМ conf файл`
```
sudo nano /etc/keepalived/keepalived.conf
```
`Содержимое файла yf ppc1`
```
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
vrrp_instance VI_1 {
        state BACKUP
        interface enp0s8
        virtual_router_id 15
        priority 255
        advert_int 1

        virtual_ipaddress {
              192.168.111.15/24
        }

}
```
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


---

### Задание 3

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

### Задание 4

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
