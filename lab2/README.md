## Часть 1. Логирование

1. Создадим первые два Yaml файла конфигурации: `docker-compose.yml` и `promtail_config.yml`
   ![alt text](assets/image.png)
2. Поднимем контейнеры командой `docker compose up -d`
   ![alt text](assets/image-1.png)
3. Подключаемся к веб-интерфейсу nextcloud через `localhost:8080`
   ![alt text](assets/image-2.png)
4. Регистрируем учетку пользователя в системе и проверяем логи

```bash
docker exec -it ea32d8a1744b /bin/bash
cat data/nextcloud.log
```

![alt text](assets/image-3.png)  
5. Проверяем, что promtail подцепил `nextcloud.log`
![alt text](assets/image-5.png)

## Часть 2. Мониторинг

1. Откроем веб-интерфейс zabbix через `localhost:8082`
   ![alt text](assets/image-4.png)
2. Создадим файл template.yml
3. В разделе data collection / templates / import импортируем созданный файл
   ![alt text](assets/image-6.png)
   ![alt text](assets/image-7.png)
4. Разрешим короткое имя в некстклауде
   ![alt text](assets/image-8.png)
5. В разделе data collection / hosts / new host создадим новый хост и выберем импортированный шаблон
   ![alt text](assets/image-12.png)
6. В разделе monitoring / latest data проверим, что появились данные
   ![alt text](assets/image-11.png)

## Часть 3. Визуализация

1. В терминале выполнить команду `docker exec -it grafana bash -c "grafana cli plugins install alexanderzobnin-zabbix-app"`, затем `docker restart grafana`
   ![alt text](assets/image-13.png)
2. Установим плагин Zabbix через веб-интерфейс grafana
   ![alt text](assets/image-14.png)
3. Подключаем Loki
   ![alt text](assets/image-15.png)
4. Удостовермися что прошли тесты
   ![alt text](assets/image-16.png)
5. Аналогично настраиваем Zabbix
   ![alt text](assets/image-17.png)
   ![alt text](assets/image-18.png)
6. Изначально логи zabbix nextcloud не отображались в Grafana, так как в конфигурации Promtail использовалась маска \*.log, и файл nextcloud.log не подхватывался. После замены маски на точный путь /opt/nc_data/nextcloud.log логи начали корректно поступать в Loki и отображаться в Grafana.
   ![alt text](assets/image-20.png)
   ![alt text](assets/image-19.png)

## Задание

1. Дешборд для просмотра uptime некстклауда
   ![alt text](assets/image-21.png)
2. Табличка с логами loki
   ![alt text](assets/image-22.png)
3. Итоговый вид
   ![alt text](assets/image-23.png)

## Ответы на вопросы

### 1.SLO vs SLA

**SLO** - внутренняя цель качества сервиса
**SLA** - формальное соглашение с клиентом с обязательствами и штрафами

### 2. Инкрементальный vs дифференциальный бэкап

**Инкрементальный** - сохраняет изменения с момента последнего бэкапа любого типа
**Дифференциальный** - сохраняет изменения с момента последнего полного бэкапа

### 3. Мониторинг vs observability

**Мониторинг** - показывает, что не работает
**Observability** - помогает понять, почему это не работает

## Вывод

В ходе работы успешно настроена комплексная система мониторинга Nextcloud. Реализовано централизованное логирование через Promtail+Loki, сбор метрик с помощью Zabbix и визуализация данных в Grafana. Решена проблема с корректным сбором логов Nextcloud, созданы информативные дашборды для отслеживания состояния системы и анализа событий.
