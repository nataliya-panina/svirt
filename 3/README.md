# Домашнее задание к занятию «Docker. Часть 2»

### Дополнительные примеры
Примеры различных композ проектов от разработчиков Docker: [https://github.com/docker/awesome-compose/blob/master/wireguard/compose.yaml](https://github.com/docker/awesome-compose/tree/master)

### Дополнительная документация:
  - блок networks: в compose: https://docs.docker.com/compose/compose-file/06-networks/
  - блок volumes: в compose: https://docs.docker.com/compose/compose-file/07-volumes/

## Задание 1

**Напишите ответ в свободной форме, не больше одного абзаца текста.**

Установите Docker Compose и опишите, для чего он нужен и как может улучшить лично вашу жизнь.

## Решение

Docker compose - это решение для запуска и управления несколькими контейнерами, вместе составляющими одно приложение, одновременно, при помощи файла конфигурации YAML. Один раз написанный и протестированный файл конфигурации исключает ошибки, которые можно допустить при запуске контейнеров вручную.

---
## Задание 2 

**Выполните действия и приложите текст конфига на этом этапе.** 

Создайте файл docker-compose.yml и внесите туда первичные настройки: 

 * version;
 * services;
 * volumes;
 * networks.

При выполнении задания используйте подсеть 10.5.0.0/16.
Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw.
Все приложения из последующих заданий должны находиться в этой конфигурации.

## Решение
```
https://hub.docker.com/r/prom/prometheus
mkdir prometheus
cd prometheus
nano compose.yml
```

Compose.yml:

```
version: '3'
services:

volumes:

networks:
  paninang-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
        gateway: 10.5.0.1
```
        
---
## Задание 3 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus. 
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории [6-04/prometheus](https://github.com/netology-code/sdvps-homeworks/tree/main/6-04/prometheus)).
3. Обеспечьте внешний доступ к порту 9090 c докер-сервера.

---

## Решение

compose.yml
```
services:
  prometheus:
    image: prom/prometheus:v2.53.1
    container_name: paninang-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - paninang-netology-hw
    restart: always
volumes:
  prometheus-data:
networks:
  paninang-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
        gateway: 10.5.0.1
```
```
docker compose up -d
```

![image](https://github.com/user-attachments/assets/fd52af99-b8d2-4549-a4bc-0a3e88b0ffa5)

![image](https://github.com/user-attachments/assets/bf804ed5-f362-4bb6-90eb-ef6c6ed5a73c)

---
## Задание 4 

**Выполните действия:**

1. Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway. 
2. Обеспечьте внешний доступ к порту 9091 c докер-сервера.

## Решение

---
## Задание 5 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana. 
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории [6-04/grafana](https://github.com/netology-code/sdvps-homeworks/tree/main/6-04/grafana)).
3. Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
4. Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.

## Решение

---
## Задание 6 

**Выполните действия.**

1. Настройте поочередность запуска контейнеров.
2. Настройте режимы перезапуска для контейнеров.
3. Настройте использование контейнерами одной сети.
5. Запустите сценарий в detached режиме.

## Решение

---
## Задание 7 

**Выполните действия.**
1. Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: ```echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:9091/metrics/job/netology```.
2. Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
3. Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://prometheus:9090" -> Save & Test).
4. Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.

В качестве решения приложите:

* docker-compose.yml **целиком**;
* скриншот команды docker ps после запуске docker-compose.yml;
* скриншот графика, постоенного на основе вашей метрики.

## Решение

---
## Задание 8

**Выполните действия:** 

1. Остановите и удалите все контейнеры одной командой.

В качестве решения приложите скриншот консоли с проделанными действиями.

## Решение

---
## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

## Задание 9* 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Alertmanager с именем контейнера <ваши фамилия и инициалы>-netology-alertmanager. 
2. Добавьте необходимые тома с данными и [конфигурацией](https://github.com/netology-code/sdvps-homeworks/tree/main/6-04/alertmanager), сеть, режим и очередность запуска.
3. Обновите конфигурацию Prometheus (необходимые изменения ищите в презентации или документации) и перезапустите его. 
4. Обеспечьте внешний доступ к порту 9093 c докер-сервера.

В качестве решения приложите скриншот с событием из Alertmanager.

## Решение


---
## Задание 10* 

Запустите свой сценарий на чистом железе без предзагруженных образов.

**Ответьте на вопросы в свободной форме:**

1. Опишите выполненный вами процесс развертывания сценария.
2. Как вы думаете зачем может понадобиться такой способ развертывания?

## Решение

