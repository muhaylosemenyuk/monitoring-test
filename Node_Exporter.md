#### Оновлення пакетів Ubuntu
```bash
sudo apt-get update && sudo apt-get upgrade
```

#### Встановлення та налаштування моніторингового агента Node Exporter
```bash
# Завантажте останню версію Node Exporter з офіційного сайту Prometheus
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz

# Розпакуйте архів
sudo tar -xvf node_exporter-1.5.0.linux-amd64.tar.gz -C /usr/local/bin/ && rm node_exporter-1.5.0.linux-amd64.tar.gz

# Перейменуйте папку для зручності
cd /usr/local/bin/ && sudo mv node_exporter-1.5.0.linux-amd64 node_exporter && cd $HOME

# Створіть користувача prometheus та надайте йому права sudo
sudo useradd -m -s /bin/bash prometheus && sudo usermod -aG sudo prometheus

# Змініть власника та групу виконуваного файлу Node Exporter на користувача prometheus
sudo chown prometheus:prometheus /usr/local/bin/node_exporter

# Створіть файл конфігурації сервісу
sudo nano /etc/systemd/system/node_exporter.service

```

#### Додайте наступний текст до файлу конфігурації сервісу
```bash
[Unit]
Description=Node Exporter

[Service]
User=prometheus
ExecStart=/usr/local/bin/node_exporter/node_exporter
Restart=always

[Install]
WantedBy=multi-user.target

```

#### Запускаємо сервіс
```bash
sudo systemctl daemon-reload
sudo systemctl enable node_exporter.service
sudo systemctl start node_exporter.service

```

#### Перевірити логи
```bash
journalctl -u node_exporter.service -f -o cat
```

#### Рестарт
```bash
systemctl restart node_exporter.service
```
