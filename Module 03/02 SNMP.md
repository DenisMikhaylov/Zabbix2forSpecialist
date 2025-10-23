---
Практическая работа:
    Название: 'Мониторинг SNMP'
    Действие: 'Модуль второй'
    Задача: ' Кастомные проверки'
---
# **Расширение сбора информации**

В данной практической работе мы добавим расширенный сбор данных по SNMP.

Этапы создания стенда:

- Добавление расширенных проверок проверок


Имена пользователей и пароли:
```
Для denian
login: sa 
password: 111

login: root 
password: 111
```
```
Для Windows
login: Administrator 
password: Pa$$w0rd
```
### **Практическая работа**

### **Задача 1: SNMP**
Подключаемся к серверу server

Добавить в конец каждой строки в описание репозитариев слово non-free
```
nano /etc/apt/sources.list
```
```
deb http://deb.debian.org/debian/ bookworm main non-free-firmware non-free
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware non-free

deb http://security.debian.org/debian-security bookworm-security main non-free-firmware non>
deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware>

# bookworm-updates, to get updates before a point release is made;
# see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backpor>
deb http://deb.debian.org/debian/ bookworm-updates main non-free-firmware non-free
deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware non-free

```
Установка snmp консоли
```
apt update 
```
```
apt install snmp snmp-mibs-downloader
```
```
:> /etc/snmp/snmp.conf
```


Включение SNMP на Windows 

Добавить компонент Service SNMP и WMI for SNMP

Настроить службу SNMP

На вкладке безопасность добавить имя public

И выставить признак принимать запросы от люого узла

На вкладке Агент SNMP выставить чекбокс для всех служб

Получение списка всех параметров с хоста
```
snmpwalk -c public -v2c 192.168.1.104
```
Проверям на zabbix server

```
snmpwalk -On -c public -v2c <ip windows> sysName

```

Перебор всех значений OID устройства
```
snmpwalk -On -c public -v2c <ip windows> 1
```
Определение имени устройства
```
snmpget -c public -v2c <ip windows> .1.3.6.1.2.1.1.5.0
```
```
snmpget -c public -v2c <ip windows> 1.3.6.1.2.1.2.2.1.8
```
```
snmpget -c public -v2c <ip windows> 1.3.6.1.2.1.2.2.1.7
```
```
snmpget -c public -v2c <ip windows> 1.3.6.1.2.1.1.1.0
```
```
snmpwalk -c public -v2c <ip windows> 1.3.6.1.2.1.1.6.0
```
Определение uptime устройства
```
snmpget -c public -v2c <ip windows> 1.3.6.1.2.1.1.3.0
```
Получение mac address table
```
snmpwalk -On -v2c -c public@101 <ip windows> 1.3.6.1.2.1.2.2.1.6
```
Определение загрузки CPU устройства
```
snmpget -c public -v2c <ip windows> 1.3.6.1.2.1.25.3.3.1.2
```
Вывод списка интерфейсов устройства
```
snmpwalk -c public -v2c -On <ip windows> 1.3.6.1.2.1.2.2.1.8
```

Вывод количества байт, прошедших через порт устройства с момента его включения
```
snmpget -c public -v2c -On <ip windows> 1.3.6.1.2.1.2.2.1.10.7
```
```
snmpget -c public -v2c -On <ip windows> 1.3.6.1.2.1.2.2.1.16.7
```


