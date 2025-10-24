---
Практическая работа:
    Название: 'Действия'
    Действие: 'Модуль четвертый'
    Задача: ' Настройка дополнительной информации'
---
# **Дополнительние элементы**

В данной практической работе мы добавим скрипты и действия.

Этапы создания стенда:

- Добавление скриптов
- настройка действий с триггерами


Имена пользователей и пароли:
```
Для debian

login: student 
password: 111

login: root 
password: 111
```
```
Для Windows

login: Student 
password: password
```
### **Практическая работа**

### **Задача 1: Добавленеи скриптов **

Добпаление скрипта для перезапуска apache2
```
Alerts->Scripts
  Name: Restart Apache
  Scope: Action operation
  Type: Script
  Execute on: Zabbix agent
  sudo /etc/init.d/apache2 restart
```
Дабавить прав пользователю zabbix на хосте Zabbix 

```
visudo
```
Добавить в конец строку
```
zabbix ALL=(ALL) NOPASSWD: /etc/init.d/apache restart
```
### **Задача 2: Добавленеи в действие выполнения скрипта **

Добавление действия
```
Alerts → Actions → Trigger actions
Create action
  Name: Error Apache
    Condition:
        Value of tag Service equals Apache
        Trigger severity is greater than or equals Average
    Operations:
        Add:
          Operation: Restart Apache
          Host: Zabbix server
```
