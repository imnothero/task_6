Проект по заданию 6.

Этот проект позволяет запускать нагрузочные тесты в JMeter, собирать метрики в InfluxDB и визуализировать их в Grafana.

## Быстрый старт

1. Запуск InfluxDB

**Через Docker:**
docker run -d --name influxdb -p 8086:8086 \
  -e DOCKER_INFLUXDB_INIT_MODE=setup \
  -e DOCKER_INFLUXDB_INIT_USERNAME=admin \
  -e DOCKER_INFLUXDB_INIT_PASSWORD=admin123 \
  -e DOCKER_INFLUXDB_INIT_ORG=my-org \
  -e DOCKER_INFLUXDB_INIT_BUCKET=jmeter \
  influxdb:2.7

1. Запуск Grafana
docker run -d --name grafana -p 3000:3000 grafana/grafana

3. Настройка JMeter
Открой файл теста в JMeter.
Добавь Backend Listener:

Backend Listener implementation: org.apache.jmeter.visualizers.backend.influxdb.InfluxdbBackendListenerClient

Пример параметров:

influxdbUrl=http://localhost:8086/api/v2/write?org=my-org&bucket=jmeter&precision=ms
influxdbToken=ТВОЙ_ТОКЕН_ОТ_INFLUXDB
application=loadtest
measurement=jmeter
summaryOnly=false
samplersRegex=.*
*Скопируй токен доступа в поле influxdbToken из панели InfluxDB.*

4. Запуск теста
Нажми Start в JMeter.

Метрики отправляются в InfluxDB автоматически.

5. Настройка Grafana
Открой Grafana: http://localhost:3000 (по умолчанию admin/admin)

Перейди в Configuration → Data sources и добавь InfluxDB:

URL: http://host.docker.internal:8086 (или http://localhost:8086)

Organization: my-org

Token: тот же, что и для JMeter

Bucket: jmeter

Импортируй дашборд JMeter (например, JMeter Dashboard ID: 1152):

Dashboards → Import → вставь ID → выбери источник InfluxDB