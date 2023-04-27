#### Оновлення пакетів Ubuntu
```bash
sudo apt-get update && sudo apt-get upgrade
```

#### Встановлення та налаштування Prometheus
```bash
# Завантажте останню версію Prometheus з офіційного сайту
wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz

# Розпакуйте архів
sudo tar -xvf prometheus-2.43.0.linux-amd64.tar.gz -C /usr/local/bin/ && rm prometheus-2.43.0.linux-amd64.tar.gz

# Перейменуйте папку для зручності
cd /usr/local/bin/ && sudo mv prometheus-2.43.0.linux-amd64 prometheus && cd $HOME

# Створіть користувача prometheus та надайте йому права sudo
sudo useradd -m -s /bin/bash prometheus && sudo usermod -aG sudo prometheus

# Змініть власника та групу виконуваного файлу Node Exporter на користувача prometheus
sudo chown prometheus:prometheus /usr/local/bin/prometheus

# Створіть папку для зберігання даних Prometheus
sudo mkdir -p /var/lib/prometheus/data

# Змініть власника папки даних на користувача prometheus
sudo chown prometheus:prometheus /var/lib/prometheus/data

# Створіть папку для конфігураційного файлу Prometheus
sudo mkdir /etc/prometheus

# Змініть власника папки конфігураційного файлу на користувача prometheus
sudo chown prometheus:prometheus /etc/prometheus

# Скопіюйте конфігураційний файл з прикладу в папку конфігураційного файлу
cp /usr/local/bin/prometheus/prometheus.yml /etc/prometheus/

# Змініть власника конфігураційного файлу на користувача prometheus
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml

# Створіть службу для Prometheus
sudo nano /etc/systemd/system/prometheus.service

```

#### Вставте наступний код в файл
```bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/data

```

#### Запускаємо сервіс Prometheus
```bash
sudo systemctl daemon-reload && \
sudo systemctl enable prometheus && \
sudo systemctl start prometheus

```

#### Перевіряємо статус
```bash
sudo systemctl status prometheus
```

#### Перевірити логи
```bash
journalctl -u prometheus.service -f -o cat
```

#### Рестарт
```bash
systemctl restart prometheus.service
```

#### Відкрийте файл конфігурації Prometheus
```bash
sudo nano /etc/prometheus/prometheus.yml

```

#### Додайте наступний блок коду до файлу prometheus.yml, вказавши адресу та порт сервера, на якому запущений Node Exporter
```bash
scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['<IP-адрес>:<порт-Node-Exporter>']

```
