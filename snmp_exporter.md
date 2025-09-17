## Snmp exporter для prometheus

1. Забираем с репозитория нужную версию:
```
git clone https://github.com/prometheus/snmp_exporter.git
or
wget https://github.com/prometheus/snmp_exporter/releases/tag/v0.29.0
```

Распаковываем:

```
tar -xzf snmp_exporter
```

Копируем бинарь:

sudo cp snmp_exporter /usr/local/bin/

Создаем директорию для конфигурации:
```
sudo mkdir /etc/snmp_exporter/
```

Создаем системного пользователя для службы:
```
sudo useradd --system --no-create-home --shell /bin/false snmp_exporter
```
Права для работы с бинарником:
```
sudo chown snmp_exporter:snmp_exporter /usr/local/bin/snmp_exporter
```

Файл для создания службы:

```
sudo tee /etc/systemd/system/snmp_exporter.service << EOF
[Unit]
Description=SNMP Exporter
After=network-online.target

[Service]
User=snmp_exporter
Restart=on-failure
ExecStart=/usr/local/bin/snmp_exporter --config.file='/etc/snmp_exporter/snmp.yaml'

[Install]
WantedBy=multi-user.target
EOF
```

Перечитываем unit файлы, добавляем в автозагрузку и запускаем службу:


```
sudo systemctl daemon reload

sudo systemctl enable --now snmp_exporter.service
```

Далее на сервере prometheus добавляем раздел в главный конфигурационный файл /etc/prometheus/prometheus.yaml:

```
scrape_configs:
  - job_name: 'snmp'
    static_configs:
      - targets: ["192.168.0.147"]  # SNMP device.
    metrics_path: /snmp
    params:
      module: [NAME_MODULE_IN_SNMP_YAML]
      auth:
      -  NAME_YOUR_COMMUNITY
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 192.168.0.120:9116
```
