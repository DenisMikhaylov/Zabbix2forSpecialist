---
Практическая работа:
    Название: 'Устновка Zabbix.'
    Действие: 'Модуль Второй'
    Задача: ' Настройка безопасного подключения агента'
---
# **Настройка безопасного подключения агента**

В данной практической работе мы настроим безопасное подключение агента .

Этапы создания стенда:

- Настройка безопасного подключения агента

Имена пользователей и пароли:
```
Для denian
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

### **Задача 1: Настройка безопасного подключения агента на Debian **

1. Запустите терминул для виртуальной машины Debian под учетной записью студент указанной сверху.

2. Получите права супер пользователя.

```
su -
```
```
< Введите пароль пользователя student >
```



3. Cгенерируем на своем компе psk фразу командой и записываем ее в файл
```
openssl rand -hex 32 > /etc/zabbix/zabbix_agent.psk
```
4. Dыводим этот ключ на экран командой 
```

tail /etc/zabbix/zabbix_agent.psk
```
5. Сохраняем у себя данный ключ он нам пригодиться при настройки серверной части Zabbix

6. Настройка агента

```
nano /etc/zabbix/zabbix_agentd.conf
```

7. Меняем следующие значения:
```
Server=192.168.10.10
Hostname=Debian
TLSAccept=psk
TLSConnect=psk
TLSPSKFile=/etc/zabbix/zabbix_agent.psk
TLSPSKIdentity=Debian   такое же должно использоваться на серверной части
```
8. Перезагружаем агента
```
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```
9. Проверка работы с zabbix server
10. Подключиться к терминалу Server
11. Проверить получение данных для Debian агента

```

zabbix_get -s IP/DNSNAME  -p 10050 -k agent.version --tls-connect=psk --tls-psk-identity="Debian" --tls-cipher='ключ'

```
или если файл адеиный и скачен на сервер
```
zabbix_get -s IP/DNSNAME  -p 10050 -k agent.version --tls-connect=psk --tls-psk-identity="Debian" --tls-psk-file=<путь до psk файлв>
```


