# Loki & Grafana & Promtail & Nginx
## Инфаструктура
Сбор логов с docker-контейнера, на котором запущен сервер nginx.
+ Компонент для сборов и отправки логов `Promtail`
+ Компонент для хранения, просмотров и анализ логов `Loki`
+ Компонент для визуализации логов `Grafana`
___
## Конфигурации
`docker-composer.yml` содержит сценарции для запуска контейнеров с образами всех компонентов стека.
Запущены все контейнеры в одной сети - `app`.

В контейнер с образом `Promtail` в файловую систему вмонтирован каталог с текущими логами `nginx` для отслеживания.

В `Grafana` вмонтирован `provisioning` c готовым конфигом для дашборда.

В `loki-config.yaml` указаны обычные рабочие параметры для сбора логов.

В `promtail.yaml` указано, куда отправлять логи и откуда их собирать.

В `nginx.conf` рабочий конфиг для запуска `nginx`
___
## Запуск
Запуск стека командой `docker compose up -d`
![](https://github.com/Morody/GrafanaLoki/blob/4c85200b691c663b63bbc8811ed7750facd3d14a/img/img.png)

Для входа в `Grafana` - `admin/admin`

По адресу `http://localhost:3000/explore` можно попасть в интерфейс `Grafana` с уже готовым дашбордом.
В качестве источника данных нужно выбрать `Loki`
В отображении текущих логов `nginx` нам поможет LogQL запрос

```
{filename="/opt/nginx_access.log"} |= `` 
```

Отображаются логи, которые можно увидеть в `./logs/nginx/access.log`

![](https://github.com/Morody/GrafanaLoki/blob/4c85200b691c663b63bbc8811ed7750facd3d14a/img/img_1.png)
![](https://github.com/Morody/GrafanaLoki/blob/4c85200b691c663b63bbc8811ed7750facd3d14a/img/img_2.png)


