---
Практическая работа:
    Название: 'Шаблоны.'
    Действие: 'Модуль второй'
    Задача: ' Шаблоны Linux'
---
# **Добавлени нод**

В данной практической работе мы добавим шаблоны Linux .

Этапы создания стенда:

- Добавление шаблонов Linux


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
password: password
```
### **Практическая работа**

### **Задача 1: Настройка подключения агента **
1. открыть веб инетерфейс Zabbix


Применение  шаблона “Linux by Zabbix agent”

```
Host: Gate
  Template:
    Linux by Zabbix agent
 ```

Подождать 5 минут. Обновить экран.

Проверка

```
Monitoring->Hosts->Latest data
  Gate
  Tag: Gate
  Zoom: 1h
```
### **Задача 2: Добавление шаблонов Windows **

1. открыть веб инетерфейс Zabbix

Применение шаблона “Windows by Zabbix agent”

```
Host: С Виндовс операционной системой
  
  Template:
    Windows by Zabbix agent

```

Подождать 5 минут. Обновить экран.

Проверка

```
Monitoring->Hosts->Latest data
  Windows
  Tag: Windows
  Zoom: 1h
```

### **Задача 2: Создание шаблона**

1. Открыть веб интерфейс Zabbix

2. Создаем шаблон 

```
  Data collection -> Templates -> Create template

  Templates Name: Template App SSH Port Service SSH Port Service
  Groups: My Template
      Items
          Name: SSH service is running {HOST.NAME}
          Key: net.tcp.service[ssh,,{$SSH_PORT}]
          Update interval: 30s
      Macros
        {$SSH_PORT}=22
```

3. Добавляем шаблон на все линукс машины

4. Проверяем
