---
Практическая работа:
    Название: 'Создание среды для практических работ.'
    Действие: 'Модуль первый'
    Задача: 'Настройка DHCP'
---

# **Создание сервисов и сети предприятия**

В данной практической работе мы создадим стенд в рамках, которого будем выполнять наш курс .

Этапы создания стенда:

- Импорт виртуальных машин

- Настройка виртуальных машин и сети

- Развертывания сервисов

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


### **Задача 1: Установка DHCP**

Установка DHCP
```
apt install isc-dhcp-server
```

### **Задача 2: Настройка сетевого инетерфейса для DHCP**
Настройка Интерфейса

1. Добавить настройку сетевого интерфейса

```
nano /etc/default/isc-dhcp-server
```

```
INTERFACESv4="enp0s8"
```

### **Задача 3: Настройка Scope для DHCP**

1. Настройка scope
```
nano /etc/dhcp/dhcpd.conf
```
```
log-facility local7;
```
```
shared-network LAN1 {
  subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.101 192.168.10.128;
    option routers 192.168.10.1;
    option domain-name "corp.local";
    option domain-name-servers 192.168.10.1;
    default-lease-time 600;
    max-lease-time 7200;
  }
}
```
### **Задача 4: Проверка DHCP**

1. Проверка DHCP
```
dhcpd -t
```
2. Перезапуск службы DHCP
```
service isc-dhcp-server restart
```
3. Проверка статуса DHCP
```
service isc-dhcp-server status
```

4. Мониторинг работы DHCP
```
dhcp-lease-list
```
```
less /var/lib/dhcp/dhcpd.leases
```
```
grep dhcp /var/log/syslog
```
