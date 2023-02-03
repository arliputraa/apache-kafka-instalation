# Installation Apache Kafka in Ubuntu Server

## Running:

    sudo apt-get update 
    and 
    sudo apt-get upgrade
    
## Install Java

    sudo apt install openjdk-11-jre-headless -y

## Download kafka.tar

    wget https://downloads.apache.org/kafka/3.3.2/kafka_2.12-3.3.2.tgz
    
## Extract kafka.tar.gz

    tar -xvf kafka_2.12-3.3.2.tgz
    
 ##### change name directory kafka_2.12-3.3.2.tgz (SKIP)
    mv kafka_2.12-3.3.2.tgz kafka
    
## Edit server.properties of kafka

    nano ~/kafka/config/server.properties
    
## change the logs dir to :

    log.dirs=/home/arliddi/kafka/logs
    
## append this to EOF

    delete.topic.enable = true

## Create zookeeper run as a services

    sudo nano /etc/systemd/system/zookeeper.service
    
 zookeeper.service
  
    [Unit]
    Requires=network.target remote-fs.target
    After=network.target remote-fs.target
    
    [Service]
    Type=simple
    User=root
    ExecStart=/home/arliddi/kafka/bin/zookeeper-server-start.sh /home/arliddi/kafka/config/zookeeper.properties
    ExecStop=/home/arliddi/kafka/bin/zookeeper-server-stop.sh
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user.target
    
## Create kafka run as a services

    sudo nano /etc/systemd/system/kafka.service
    
kafka.service

    [Unit]
    Requires=zookeeper.service
    After=zookeeper.service
    
    [Service]
    Type=simple
    User=root
    ExecStart=/bin/sh -c '/home/arliddi/kafka/bin/kafka-server-start.sh /home/arliddi/kafka/config/server.properties > /home/arliddi/kafka/logs/kafka.log 2>&1'
    ExecStop=/home/arliddi/kafka/bin/kafka-server-stop.sh
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user./root

## Create Kafka connect run as a service

    sudo nano /etc/systemd/system/kafka_connect.service
    
kafka_connect.service

   [Unit]
    Requires=kafka.service
    After=kafka.service
    
    [Service]
    Type=simple
    User=root
    ExecStart=/bin/sh -c '/home/arliddi/kafka/bin/connect-distributed.sh /home/arliddi/kafka/config/testing.properties > /home/arliddi/kafka/logs/kafka_connect.log 2>&1'
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user.target
    
## Start kafka 

    sudo systemctl start kafka
    sudo systemctl status kafka_connect
    
![image](https://user-images.githubusercontent.com/110078907/216553538-5ccdff87-7b97-4333-8c21-c2d45a213b82.png)
    
## Check status of kafka

    sudo systemctl status kafka
    
![image](https://user-images.githubusercontent.com/110078907/216553031-94780b21-e956-4d6b-b6a5-b15e7cf1e4b4.png)








    
