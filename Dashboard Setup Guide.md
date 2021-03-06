# Step-by-step guide to setup and access node dashboard

1. Enable prometheus option
2. Install node exporter
3. Open ports for prometheus server and node exporter (Firewall settings configuration)
4. Fill in the google form

## 1. Enable prometheus option
Prometheus enables you to monitor all of the "MINA blockchain" related statistics.
Include the option to enable your mina node to send the informaiton to performance dashboard with this flag: `-metrics-port 6060`
You'll need to stop the node you're currently running and restart the node using the following three options including the metrics flag.
Just change the password section for daemon and docker option, for systemd you'll need to add your password in the environment file.

  1. __Daemon__
```
coda daemon -peer-list-file ~/peers.txt \
  -block-producer-key ~/keys/my-wallet \
  -block-producer-password "YOUR PASSWORD HERE" \
  -generate-genesis-proof true \
  -metrics-port 6060
```

  2. __Docker__ (Updated 12/4)
```
sudo docker run --name mina -d \
-p 8301-8305:8301-8305 \
-p 6060:6060 \
--restart=always \
--mount "type=bind,source=`pwd`/keys,dst=/keys,readonly" \
--mount "type=bind,source=`pwd`/.coda-config,dst=/root/.coda-config" \
--mount type=bind,source="`pwd`/peers.txt,dst=/root/peers.txt",readonly \
-e CODA_PRIVKEY_PASS="YOUR PASSWORD HERE" \
minaprotocol/mina-daemon-baked:4.1-turbo-pickles-mina9652f8e-autod61d39d6 \
-peer-list-file /root/peers.txt \
-block-producer-key /keys/my-wallet \
-insecure-rest-server \
-file-log-level Debug \
-log-level Info \
-metrics-port 6060
```

   Please don't forget to finish key settings before running docker  
    chmod 700 ~/keys  
    chmod 600 ~/keys/my-wallet  

  3. __Systemd__  
  
  
    1. Make .mina-env file: sudo nano .mina-env  
    
    2. Copy and paste the contents below with your password typed  
    
    CODA_PRIVKEY_PASS="your password here"
    EXTRA_FLAGS="-metrics-port 6060" 
    
    [Exit with ctrl+x]  
    
    3. Reload daemon and start mina  
    
    systemctl --user daemon-reload
    systemctl --user start mina  
    
    4. Check your logs whether the service is working  
    
    journalctl --user -u mina -n 1000 -f 
  

- How to check whether your metrics port is open: http://YOUR_NODE_IP_ADDRESS:6060/metrics

## 2. Install node exporter
The node exporter allows you to monitor all of the "System" related statistics (source: https://github.com/prometheus/node_exporter#using-docker)
- Command (You need to run a seperate docker container for node exporter) :
```
docker run -d \
  --net="host" \
  --pid="host" \
  -v "/:/host:ro,rslave" \
  quay.io/prometheus/node-exporter \
  --path.rootfs=/host
 ```
- How to check whether your node exporter port is open: http://YOUR_NODE_IP_ADDRESS:9100/metrics

## 3. Open ports for prometheus server and node exporter (Firewall settings configuration)
- TCP Port 6060 (from 101.101.216.236) : mina daemon metrics port
- TCP Port 9100 (from 101.101.216.236) : node_exporter port
- If you are not sure how to open firewall settings, please follow the guide here: https://github.com/dsrvlabs/mina/blob/master/How%20to%20open%20ports.md

## 4. Fill in the google form
- Link: https://forms.gle/VQPzKvoZpK5jud838
(We DO NOT use your email and private information other than servicing purposes and DSRV does not collect, store or share your personal information to 3rd party)
(All of the information will be deleted immediately after the testnet period.)

## What next?
- You will receive the URL of the link for the dashboard along with your own ID and Password.
- If you have any technical problems or questions, feel free to reach out  at contact@dsrvlabs.com or at the discord channel.
- Discord : MINA Discord > Node dashboard
@Jongkwang @Seok Hyun (Danny) will be happy to help you out.

Thank you and happy staking! 
