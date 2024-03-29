## Отчет по лабораторной работе №1 "Базовая настройка ip-телефонов в среде Сisco packet tracer"

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [IP-telephony](https://github.com/itmo-ict-faculty/ip-telephony)

Year: 2023/2024

Group: K34202

Author: Osipova Valeriya Vladimirovna

Lab: Lab1

Date of create: 15.02.2024

Date of finished: 20.02.2024

### Цель работы
Изучить рабочую среду Cisco Packet Tracer, ознакомиться с интерфейсами основных устройств, типами кабелей, научиться собирать топологию. Изучить построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer.

### Ход работы

#### 1 часть

1. Собираем схему соединения, указанную на рисунке

![схема drawio](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/e068d953-1b32-4213-8667-90387c9b1a86)

В Cisco Packet Tracer схема преобретает следующий вид:

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/9f5a7ee0-3358-4650-8349-d0e593a58c61)

2. Присвоим компьютерам статические IP-адреса 192.168.0.1 - 192.168.0.7, как показано на схеме выше, с маской 255.255.255.0.

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/131957d2-a943-44d5-8f26-abaaa91fab05)

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/3006be0b-4aff-4d16-aed5-14897fb3367e)

3. Выполняем команду ping на разных ПК, чтобы проверить связность. Убеждаемся, что любой компьютер посредством пинга успешно передает пакеты любому другому компьютеры на схеме. Пример пинга:

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/4acabaea-5b98-4b5d-9920-e8d766a291fe)

#### 2 часть

1. Собираем схему соединения, указанную на рисунке

![схема2 drawio](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/5cfa828a-ae1d-4eeb-878b-1cdd741e2dd9)

2. Изменяем имя маршрутизатора на CMERouter

```
Router(config)#hostname CMERouter
CMERouter(config)#
```

3. Настраиваем интерфейс fa0/0 на маршрутизаторе CMERouter, направленный на коммутатор. Присвоим ему IP-адрес 192.168.0.1 с маской 255.255.255.0

```
CMERouter(config)#interface FastEthernet0/0
CMERouter(config-if)#ip address 192.168.0.1 255.255.255.0
CMERouter(config-if)#no shutdown
```

4. На маршрутизаторе настраиваем DHCP-сервер, выделяем пул для IP–телефонов под названием phones:

```
CMERouter(config-if)#ip dhcp pool phones
CMERouter(dhcp-config)#network 192.168.0.0 255.255.255.0
CMERouter(dhcp-config)#default-router 192.168.0.1
CMERouter(dhcp-config)#option 150 ip 192.168.0.1
```

Появляется следующее сообщение:

```
%IPPHONE-6-REGISTER: ephone-1 IP:192.168.0.2 Socket:2 DeviceType:Phone has registered.
%IPPHONE-6-REGISTER: ephone-2 IP:192.168.0.3 Socket:2 DeviceType:Phone has registered.
```

Таким образом, двум IP-телефонам динамически присвоены адреса 192.168.0.2 и 192.168.0.3 соответственно.

5. Также на маршрутизаторе настроим VoIP параметры, указываем максимальное количество IP-телефонов и номеров для них - 10, шлюз - 3100, адрес интерфейса маршрутизатора 192.168.0.1:

```
CMERouter(config)#telephony-service
CMERouter(config-telephony)#max-dn 10
CMERouter(config-telephony)#max-ephones 10
CMERouter(config-telephony)#ip source-address 192.168.0.1 port 3100
CMERouter(config-telephony)#auto assign 1 to 19
```

6. На коммутаторе включим поддержку VoIP на интерфейсах:

```
Switch(config)#interface FastEthernet 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport voice vlan 1
Switch(config-if)#interface FastEthernet 0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport voice vlan 1
Switch(config-if)#interface FastEthernet 0/3
Switch(config-if)#switchport mode access
Switch(config-if)#switchport voice vlan 1
```

7. На маршрутизаторе присваиваем номера телефонам:

```
CMERouter(config)#ephone-dn 1
CMERouter(config-ephone-dn)#number 89111
CMERouter(config-ephone-dn)#ex
CMERouter(config)#ephone-dn 2
CMERouter(config-ephone-dn)#number 89222
```

8. Подключаем IP-телефоны к источнику питания:

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/5119ae06-5711-4894-89ed-cb4fe6154a97)

Таким образом, схема сети в Cisco Packet Tracer выглядит следующим образом:

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/a487eb20-a055-4c33-b847-536fbe693978)

При наведении мыши на IP-телефон можно увидеть присвоенный IP-адрес и номер:

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/06b91198-2abf-41ff-8088-913def9ff35c)

9. Проверим звонки между телефонами:

![image](https://github.com/Valeriya-Osipova/2023_2024-ip-telephony-k34202-osipova-v-v/assets/64967406/03943cd5-1d71-4e2a-990d-bbe1c9ca586d)

Звонки проходят успешно.

#### 3. Вывод

В результате выполнения данной лабораторной работы удалось изучить рабочую среду Cisco Packet Tracer, ознакомиться с интерфейсами основных устройств, типами кабелей, научиться собирать топологию, изучить построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer.
