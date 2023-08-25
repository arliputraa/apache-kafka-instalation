# Installation Apache Kafka in Ubuntu Server

## Running:

    sudo apt-get update
    and
    sudo apt-get upgrade
    
## Install Java

    sudo apt install openjdk-11-jre-headless -y

## Download kafka.tar

    wget https://downloads.apache.org/kafka/3.3.2/kafka_2.12-3.3.2.tgz
    tar -xvf kafka_2.12-3.3.2.tgz
    
 change name directory kafka_2.12-3.3.2.tgz
   
    mv kafka_2.12-3.3.2.tgz kafka
    
## Edit server.properties of kafka

   sudo nano ~/kafka/config/server.properties
    
change the logs dir to:

    log.dirs=/home/devarli/kafka/logs
    
append this to EOF:

    delete.topic.enable = true

## Create zookeeper run as a services

    sudo nano /etc/systemd/system/zookeeper.service
    
 Append this:
  
    [Unit]
    Requires=network.target remote-fs.target
    After=network.target remote-fs.target
    
    [Service]
    Type=simple
    User=root
    ExecStart=/home/devarli/kafka/bin/zookeeper-server-start.sh /home/devarli/kafka/config/zookeeper.properties
    ExecStop=/home/devarli/kafka/bin/zookeeper-server-stop.sh
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user.target
    
## Create kafka run as a services

    sudo nano /etc/systemd/system/kafka.service
    
Append this:

    [Unit]
    Requires=zookeeper.service
    After=zookeeper.service
    
    [Service]
    Type=simple
    User=root
    ExecStart=/bin/sh -c '/home/devarli/kafka/bin/kafka-server-start.sh /home/devarli/kafka/config/server.properties > /home/devarli/kafka/logs/kafka.log 2>&1'
    ExecStop=/home/devarli/kafka/bin/kafka-server-stop.sh
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user.target

## Create Kafka connect run as a service

    sudo nano /etc/systemd/system/kafka_connect.service
    
Append this:

    [Unit]
    Requires=kafka.service
    After=kafka.service
    
    [Service]
    Type=simple
    User=root
    ExecStart=/bin/sh -c '/home/devarli/kafka/bin/connect-distributed.sh /home/devarli/kafka/config/kafka-connect-properties > /home/devarli/kafka/logs/kafka_connect.log 2>&1'
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user.target

### Copy connect.properties to kafka-connect-properties in file path /home/kafka/config

    cp connect.properties kafka-connect-properties
    nano kafka-connect-properties

Append this:

    bootstrap.servers=(your ip):9092
    plugin.path=/home/devarli/kafka/connect
    
### Start kafka 

    sudo systemctl start kafka

![image](https://github.com/arliputraa/apache-kafka-instalation/assets/110078907/229f3422-c48a-4218-a62c-a1831de838e0)
    
### Check status of kafka

    sudo systemctl status kafka
    
![image](https://github.com/arliputraa/apache-kafka-instalation/assets/110078907/9fea027e-96d2-4fc2-a802-58b4ff0e1638)

### Start kafka_connect

    sudo systemctl start kafka_connect

![image](https://github.com/arliputraa/apache-kafka-instalation/assets/110078907/6fb6ebfa-23fa-472f-ba14-1f9c6c37d50f)

### Check status of kafka_connect

    sudo systemctl status kafka_connect

![image](https://github.com/arliputraa/apache-kafka-instalation/assets/110078907/aca7b588-81e0-48c2-b3c5-a5704cee8c6f)

### For more learning Apache Kafka in this link: https://github.com/arliputraa/kafka-cluster-configuration

### Optional add monitoring Kafdrop for gui kafka in this link: https://github.com/arliputraa/kafdrop-configuration-for-gui-kafka

![image](https://github.com/arliputraa/apache-kafka-instalation/assets/110078907/cd500506-c2f2-4b9a-936a-0dfccd9bdcea)




    
