#### Steps to Install and set up the InfluxDB on Ubuntu 20.04 and 22.04
```bash
# -- Step 1: Update your Ubuntu system using the following command:
sudo apt-get update

# -- Step 2: Getting packages on Ubuntu distributions:
wget -q https://repos.influxdata.com/influxdata-archive_compat.key
echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list

# -- Step 3: Install the InfluxDB version 2 package using the following commands:
sudo apt-get update
sudo apt-get install influxdb2
#  - Check InfluxDB version:
influx version

# -- Step 4: Start the InfluxDB service using the following command:
sudo systemctl enable influxdb
sudo systemctl start influxdb
#  - Verify your InfluxDB installation
sudo service influxdb status

# -- Step 5: Allow InfluxDB TCP port 8086 in the Firewall:
sudo ufw allow 8086/tcp
# InfluxDB is now running on http://<server ip>:8086.

# -- Step 6: Set up your initial user:
influx setup
#  - Enter a Username for your initial user
#  - Enter a Password and Confirm the Password for your user
#  - Enter your initial Organization Name
#  - Enter your initial Bucket Name

# That’s it! You have now set up InfluxDB and can begin using it to store and analyze your data.
```
