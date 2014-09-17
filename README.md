This will give you a local VM with a single node Kafka installation
(+supporting zookeeper). I tested this on a Mac, 10.9.2.

1. install [vagrant >= 1.6.1](https://docs.vagrantup.com/v2/installation/index.html)
2. install [ansible](http://docs.ansible.com/intro_installation.html)
3. build and configure the vm: `cd kafka-meetup-demo; vagrant up`

At that point, you should be able to connect from your local machine to
the Kafka broker which is running on the VM. You first need to download
and untar Kafka (you don't need to install anything, this is just to get
the client programs on your machine):
    
    curl -O http://apache.petsads.us/kafka/0.8.1.1/kafka_2.9.2-0.8.1.1.tgz
    tar -zxvf kafka_2.9.2-0.8.1.1.tgz
    cd kafka_2.9.2-0.8.1.1

And now you can connect to the Kafka broker which is running on the VM:     

    # list topics 
    bin/kafka-topics.sh --list --zookeeper 192.168.33.10:2181
    # produce
    bin/kafka-console-producer.sh --broker-list 192.168.33.10:9092 --topic test
    # consume
    bin/kafka-console-consumer.sh --zookeeper 192.168.33.10:2181 --topic test --from-beginning

At this point I ran into problems on my work machine (mac, 10.9.2).
Trying to connect to the kafka broker I was getting "can't resolve host"
errors. If you run into it, read
[this](http://biomedicalontologies.com/2012/11/14/fixing-java-net-local-
host-name-unknown-error-on-mac-os-x/) or change your `/etc/hosts` to map
the name you get from `scutil --get HostName` to `127.0.0.1`; my host
name is `FOOBAR`, so my hosts file has a line in it like this: 

    127.0.0.1	localhost FOOBAR

