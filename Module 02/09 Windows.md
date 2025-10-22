---
Практическая работа:
    Название: 'Мониторинг Windows'
    Действие: 'Модуль второй'
    Задача: ' Кастомные проверки'
---
# **Расширение сбора информации**

В данной практической работе мы добавим расширенный сбор данных для Windows.

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

### **Задача 1: Внешние проверки с использованием ExternalScripts**



Создание элемента мониторинга 

В веб интерфейсе zabbix открывает "Data collection" - 'Host'

```
Host: Windows
...
  Items
    Name: Processor total
    Type: agent zabbix active
    Key: perf_counter_en["\Processor(_Total)\% Processor Time",60]
```
```
Host: Windows
...
  Items
    Name: Event 50 
    Type: agent zabbix active
    Key: eventlog[System,,"Warning",,50,,skip]
```
```
Host: Windows
...
  Items
    Name: Error drive
    Type: agent zabbix active
    Key: eventlog[System,,"Warning",,153,,skip]
```
```
Host: Windows
...
  Items
    Name: WMI object disk freespace
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT FreeSpace FROM Win32_LogicalDisk WHERE DeviceID='C:']

```
```
Host: Windows
...
  Items
    Name: WMI object version bios
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT version FROM Win32_bios]

```
```
Host: Windows
...
  Items
    Name: WMI object product baseboard
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT product FROM Win32_baseboard]
    Populates host inventory field: model
```
```
Host: Windows
...
  Items
    Name: WMI object serial number baseboard
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT serialnumber FROM Win32_baseboard]
    Populates host inventory field: Chassis
```

Создание пользовательских макросов
Глобальные макросы: 

Создание глобальных макросов, перейдите в меню Administration >  Macros 

Нажимаем Add
```
Макрос: {$DISK_NAME}
Значение: c:
Описание: Макрос для диска с:
```
```
Макрос:{$THRESHOLDCPU_WAR} 
Значение: 50
Описание: Предупреждение значение cpu
```
```
Макрос:{$THRESHOLDCPU_CRIT} 
Значение: 70
Описание: Максимальное значение cpu
```
```
Макрос:{$THRESHOLDDISK_LOW} 
Значение: 5
Описание: Минимальное свободное место в процентах
```


Создание элемента с использованием макроса 
```
Host: Windows
...
  Items
    Name: WMI object disk c: freespace
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT FreeSpace FROM Win32_LogicalDisk WHERE DeviceID='{$DISK_NAME}']

```
