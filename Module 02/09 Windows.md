



Создание элемента мониторинга 

В веб интерфейсе zabbix открывает "Data collection" - 'Host'

```
Host: CLIENTN
...
  Items
    Name: Processor total
    Type: agent zabbix active
    Key: perf_counter_en["\Processor(_Total)\% Processor Time",60]
```
```
Host: CLIENTN
...
  Items
    Name: Event 50 
    Type: agent zabbix active
    Key: eventlog[System,,"Warning",,50,,skip]
```
```
Host: CLIENTN
...
  Items
    Name: Error drive
    Type: agent zabbix active
    Key: eventlog[System,,"Warning",,153,,skip]
```
```
Host: CLIENTN
...
  Items
    Name: WMI object disk freespace
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT FreeSpace FROM Win32_LogicalDisk WHERE DeviceID='C:']

```
```
Host: CLIENTN
...
  Items
    Name: WMI object version bios
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT version FROM Win32_bios]

```
```
Host: CLIENTN
...
  Items
    Name: WMI object product baseboard
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT product FROM Win32_baseboard]
    Populates host inventory field: model
```
```
Host: CLIENTN
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
Создание макросов для host, перейдите в меню data colliction >  hosts

Выбрать host Gate, перейти на вкладку Macros

Нажать add:
```
Макрос:{$THRESHOLD_WMI_LOW} 
Значение: 10
```

Создание элемента с использованием макроса 
```
Host: CLIENTN
...
  Items
    Name: WMI object disk c: freespace
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT FreeSpace FROM Win32_LogicalDisk WHERE DeviceID='{$DISK_NAME}']

```
