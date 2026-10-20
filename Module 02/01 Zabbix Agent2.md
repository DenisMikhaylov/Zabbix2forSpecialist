
# Лабораторная работа: Установка и настройка Zabbix Agent 2 (Zabbix) в Debian 12 с кастомной инвентаризацией ПО

## Цель работы
Изучить установку Zabbix Agent 2 в Debian 12, освоить механизм `UserParameter` и настроить сбор инвентаризационных данных об установленном ПО с привязкой к полю инвентаря хоста.

## Теоретическая справка
Zabbix Agent 2 (пакет `zabbix-agent2`) — это агент нового поколения на Go, поддерживающий плагины для мониторинга Docker, PostgreSQL, MongoDB, Redis и других сервисов «из коробки». Для нестандартных проверок используется `UserParameter`. Полученные данные можно связать с полем **Software** в инвентаризации хоста.

---

## Часть 1. Установка Zabbix Agent 2 в Debian 12

> **Важно:** Пакеты Zabbix отсутствуют в стандартных репозиториях Debian. Необходимо подключить официальный репозиторий Zabbix 7.0 для Debian 12 (Bookworm).

1. **Подключите репозиторий Zabbix 7.0 для Debian 12**
   
2. **Проверьте, что репозиторий добавлен корректно**
   ```bash
   apt update
   ```
  
3. **Установите Zabbix Agent 2 вместе с плагинами**
   ```bash
   sudo apt install -y zabbix-agent2 zabbix-agent2-plugin-*
   ```
   Плагины (`zabbix-agent2-plugin-*`) добавляют возможности мониторинга баз данных, очередей сообщений и контейнеров без внешних скриптов.

4. **Запустите и включите автозапуск**
   ```bash
   sudo systemctl enable zabbix-agent2
   sudo systemctl start zabbix-agent2
   sudo systemctl status zabbix-agent2 
   ```
   Убедитесь, что служба активна (`active (running)`).

---

## Часть 2. Базовая настройка Agent 2

Отредактируйте `/etc/zabbix/zabbix_agent2.conf`:

```bash
sudo nano /etc/zabbix/zabbix_agent2.conf
```

Установите параметры (пример для сервера Zabbix `192.168.1.100` посмотреть ваш ip zabbix server):

```ini
Server=192.168.1.100
ServerActive=192.168.1.100
Hostname=debian12
```

- `Server` — разрешает пассивные проверки с указанного IP.
- `ServerActive` — адрес для активных проверок.
- `Hostname` — должен точно совпадать с именем хоста в веб-интерфейсе Zabbix.

Перезапустите агент:
```bash
sudo systemctl restart zabbix-agent2
```

> **Порт:** Для пассивных проверок используется `10050/TCP`, для активных — `10051/TCP`.

---

## Часть 3. Кастомный скрипт для инвентаризации ПО

### 3.1. Скрипт сбора списка пакетов (адаптирован для Debian 12)

Создайте каталог и скрипт:

```bash
sudo mkdir -p /etc/zabbix/scripts
sudo nano /etc/zabbix/scripts/list_installed_soft.sh
```

Содержимое (используется `dpkg-query`, так как в Debian 12 это основной пакетный менеджер):

```bash
#!/bin/bash
# Сбор списка установленных пакетов в Debian 12
dpkg-query -W -f='${Package}, ' | sed 's/, $//'
```

Сделайте скрипт исполняемым и проверьте:
```bash
sudo chmod +x /etc/zabbix/scripts/list_installed_soft.sh
sudo /etc/zabbix/scripts/list_installed_soft.sh | head -c 200
```

### 3.2. Настройка UserParameter

Создайте файл в каталоге `zabbix_agent2.d`:

```bash
sudo nano /etc/zabbix/zabbix_agent2.d/custom_soft_inventory.conf
```

Добавьте строку:
```ini
UserParameter=soft.inventory,/etc/zabbix/scripts/list_installed_soft.sh
```

Перезапустите агент:
```bash
sudo systemctl restart zabbix-agent2
```

Проверьте локально:
```bash
sudo zabbix_agent2 -t soft.inventory
```
Команда должна вывести список пакетов, начинающийся с `soft.inventory [s|...]`.

---

## Часть 4. Настройка Zabbix Server (веб-интерфейс)

### 4.1. Добавление хоста
1. **Monitoring → Hosts → Create host**.
2. **Host name** — точно как в `Hostname` агента (например, `debian12-host-01`).
3. Добавьте интерфейс **Agent** с IP-адресом хоста и портом `10050`.
4. Привяжите шаблон **Linux by Zabbix agent 2**.
5. Сохраните.

### 4.2. Создание элемента данных
1. Откройте хост → **Items → Create item**.
2. Заполните:
   - **Name:** `Installed Software Inventory`
   - **Type:** `Zabbix agent`
   - **Key:** `soft.inventory`
   - **Type of information:** `Text`
   - **Update interval:** `1d` (инвентаризация не требует частого обновления)
   - **History:** `0` или `90d` по желанию.
3. Сохраните.

### 4.3. Привязка к полю инвентаря «Software»
1. В настройках хоста (**Host → Inventory**) установите режим **Automatic**.
2. В настройках элемента `Installed Software Inventory` найдите раздел **Host inventory**.
3. Выберите поле **Software**.
4. Сохраните.

Теперь при каждом обновлении элемента список пакетов будет автоматически записываться в инвентарь хоста. Просмотреть его можно в **Inventory → Overview**.

---

## Часть 5. Проверка и диагностика

1. **Latest data**  
   В веб-интерфейсе: **Monitoring → Latest data** → выберите хост → элемент `Installed Software Inventory`. Убедитесь, что приходит непустое значение.

2. **Проверка инвентаря**  
   **Inventory → Overview** → выберите хост → поле **Software** должно быть заполнено.

3. **Диагностика с сервера (`zabbix_get`)**
   ```bash
   zabbix_get -s <IP_АГЕНТА> -p 10050 -k soft.inventory
   ```
   Команда должна вернуть тот же список, что и локальный тест.

4. **Логи агента**
   ```bash
   sudo journalctl -u zabbix-agent2 -f
   ```

---
