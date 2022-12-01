![1_ZWNsHHSQNrU5jibNr-8Giw](https://miro.medium.com/max/864/1*3PlcINh0Epr30YKMIr3KTg.png)


# Guide-in-Bifrost-network-validator-
In this guide you will find a detailed description of how to create a validator on the Bifrost network 
# Official Links
[Website](https://bifrost.finance/) [Twitter](https://twitter.com/bifrost_finance) [Discord](https://discord.gg/bifrost-finance)

# Explorer
[Explorer](https://telemetry.polkadot.io/#list/0x9f28c6a68e0fc9646eff64935684f6eeeece527e37bbe1f213d22caa1d9d6bed)
# Install Node Guide 
### Setting up variables
First you need to download the binary file of the current version
```bash
wget https://github.com/bifrost-finance/bifrost/releases/download/bifrost-v0.9.64/bifrost
```
### Setup the Service

### The following commands will set up everything regarding running the service.

### 1. Create a service account to run the service:
```bash
adduser bifrost_service --system --no-create-home
```
### 2. Create a directory to store the binary and data
```bash
mkdir /var/lib/bifrost-data
```
### 3. Move the binary built in the last section to the created folder
```bash
mv ./bifrost /var/lib/bifrost-data
```
### 4. Make sure you set the ownership and permissions accordingly for the local directory that stores the chain data:
```bash
sudo chown -R bifrost_service /var/lib/bifrost-data
```
### Create the Configuration File
```bash
nano /etc/systemd/system/bifrost.service
```
### Insert the configuration below into the created file and make the necessary corrections
```bash
[Unit]
After=network.target
StartLimitIntervalSec=0

[Service]
Restart=on-failure
RestartSec=10
User= root
SyslogIdentifier=bifrost
SyslogFacility=local7
KillSignal=SIGHUP
ExecStart=/var/lib/bifrost-data/bifrost \
        --name YOUR NAME \
        --collator \
        --force-authoring \
        --base-path /var/lib/bifrost-data\
        --ws-port=9944 \
        --port=30333 \
        --prometheus-external \
        --state-cache-size 0 \
        -- \
        --execution wasm

[Install]
WantedBy=multi-user.target
```
### Then go to the directory where the binary is located and grant permissions
```bash
cd /var/lib/bifrost-data
```
```bash
chmod +x bifrost
chown bifrost_service bifrost
```
### Go back to original directory
```bash
cd 
```
### Run the Service
Almost there! Register and start the service by running:
```bash
systemctl enable bifrost.service
systemctl start bifrost.service
```
### And lastly, verify the service is running:
```bash
systemctl status bifrost.service
```
### You can also check the logs by executing:
```bash
journalctl -f -u bifrost.service
```
### Update the Client
If you want to update your client, you can keep your existing chain data in tact, and only update the binary by following these steps:
### 1. Stop the systemd service:
```bash
sudo systemctl stop bifrost.service
```
### 2. Remove the old binary file:
```bash
rm  /var/lib/bifrost-data/bifrost
```
### 3. Get the latest version of Moonbeam from the [Bifrost GitHub Release page](https://github.com/bifrost-finance/bifrost/releases)

### 4. If you're using the release binary, update the version and run:
```bash
 wget https://github.com/bifrost-finance/bifrost/releases/download/<NEW VERSION TAG HERE>/bifrost
```
### 5. Move the binary to the data directory:
```bash
mv ./bifrost /var/lib/bifrost-data
```
### 6. Then go to the directory where the binary is located and grant permissions
```bash
cd /var/lib/bifrost-data
```
```bash
chmod +x bifrost
chown bifrost_service bifrost
```
###  7. Go back to original directory
```bash
cd 
```

### 8. Register and start the service by running:
```bash
systemctl enable bifrost.service 
systemctl start bifrost.service 
```
### 9. You can also check the logs by executing:
```bash
journalctl -f -u bifrost.service
```



