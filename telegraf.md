#### Конфігурація telegraf (на робочих серверах)
```bash
# -- Step 1: install Telegraf from the InfluxData repository with the following commands:
curl -s https://repos.influxdata.com/influxdata-archive_compat.key > influxdata-archive_compat.key
echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list
sudo apt-get update && sudo apt-get install telegraf

# -- Step 2: run the following command to create a configuration file:
telegraf --sample-config --input-filter cpu:mem:net:swap --output-filter influxdb_v2 > telegraf.conf

# -- Step 3: set INFLUX_TOKEN environment variable:
INFLUX_TOKEN=YourAuthenticationToken

# -- Step 4: set environment variables for your telegraf.conf:
urls=http://23.88.34.128:8086
token=$INFLUX_TOKEN
organization=UAnodes
bucket=UAnodes
timeout=5s

# -- Step 5: automatically modify your telegraf.conf file:
sed -i "s|urls = \[\".*\"\]|urls = \[\"$urls\"\]|g" /etc/telegraf/telegraf.conf
sed -i "s/token = \".*\"/token = \"$token\"/g" /etc/telegraf/telegraf.conf
sed -i "s/organization = \".*\"/organization = \"$organization\"/g" /etc/telegraf/telegraf.conf
sed -i "s/bucket = \".*\"/bucket = \"$bucket\"/g" /etc/telegraf/telegraf.conf
sed -i 's/# timeout = "5s"/timeout = "'"$timeout"'"/' /etc/telegraf/telegraf.conf

# -- Step 6: start the telegraf service
sudo service telegraf start
```


