# 一、kafka压测

用Kafka官方自带的脚本，对Kafka进行压测。Kafka压测时，可以查看到哪个地方出现了瓶颈（CPU，内存，网络IO）。

使用的两个脚本为kafka-consumer-perf-test.sh和kafka-producer-perf-test.sh，脚本的位置在$KAFKA_HOME/bin目录下。

# 二、脚本使用

2.1 kafka producer压力测试(kafka-producer-perf-test.sh)

（1）help

```
usage: producer-performance [-h] --topic TOPIC --num-records NUM-RECORDS [--payload-delimiter PAYLOAD-DELIMITER] --throughput THROUGHPUT
                            [--producer-props PROP-NAME=PROP-VALUE [PROP-NAME=PROP-VALUE ...]] [--producer.config CONFIG-FILE] [--print-metrics]
                            [--transactional-id TRANSACTIONAL-ID] [--transaction-duration-ms TRANSACTION-DURATION] (--record-size RECORD-SIZE |
                            --payload-file PAYLOAD-FILE)
```

（2）参数说明

```
optional arguments:
           -h, --help             查看帮助
          --topic TOPIC           topic
          --num-records NUM-RECORDS  产生的消息数量
          --payload-delimiter PAYLOAD-DELIMITER  当指定--payload-file参数时，可以提供分割符，注意：如果没有--payload-file                                                                                     参数，该参数不会生效，默认是\n
          --throughput THROUGHPUT  每秒的吞吐量，即每秒多少条消息
          --producer-props PROP-NAME=PROP-VALUE [PROP-NAME=PROP-VALUE ...]   kafka生产者相关的配置属性，比如                                                                         bootstrap.servers，client.id 等等，该参数指定的配置参数比--producer.config指定的                                                                       配置参数优先级高
         --producer.config CONFIG-FILE  生产者的配置文件
         --print-metrics        在最后输出测试的指标 (默认: false)

         --record-size RECORD-SIZE  消息的字节大小，注意：只能提供 --record-size 和--payload-file其中的一个参数，不可以全                                                           部都指定
        --payload-file PAYLOAD-FILE   读取消息的文件，必须是 UTF-8编码的文本文件。
                       注意： --record-size和--payload-file参数，是必须要指定的，但只能指定其中的一个参数，不可以全部指定。

```

（3）开始测试

```
 bin/kafka-producer-perf-test.sh  --topic test  --print-metrics --record-size 100 --num-records 100000 --throughput 1000  --producer-props bootstrap.servers=kms-2:9092,kms-3:9092,kms-4:9092  
 
 
说明：record-size是一条信息有多大，单位是字节。num-records是总共发送多少条信息。throughput 是每秒多少条信息。

```

（4）结果说明

分析：本例中一共写入10w条消息，每秒向Kafka写入了**0.10MB**的数据，平均是999.99条消息/秒，每次写入的平均延迟为13.84毫秒，最大的延迟为793毫秒。

2.2 Kafka Consumer压力测试(kafka-consumer-perf-test.sh)

Consumer的测试，如果这四个指标（IO，CPU，内存，网络）都不能改变，考虑增加分区数来提升性能。

（1）参数说明

```
--broker-list <String: host>             REQUIRED: kafka  server 
            --consumer.config <String: config file>  消费者配置文件      
            --date-format <String: date format>      日期格式 见 java.text.SimpleDateFormat . (默认: yyyy-MM-dd HH:mm:ss:SSS)   
            --fetch-size <Integer: size>             一次请求提取的数据量   (默认: 1048576)   
           --from-latest                           如果消费者还没有建立偏移量，则从最新的消息开始消费，而不是最早的消息                   
           --group <String: gid>                    消费者组id (默认:  perf-consumer-69057)                 
           --help                                  帮助信息                        
           --hide-header                            如果设置，将不会打印状态头信息
            --messages <Long: count>                 REQUIRED: 发送或者消费的消息数量
            --num-fetch-threads <Integer: count>     抓取的线程数量 (default: 1)
            --print-metrics                          打印指标                            
          --threads <Integer: count>               处理的线程数  (default: 10)                        
         --timeout [Long: milliseconds]          允许返回数据的最大超时时间(default: 10000)            
           --topic <String: topic>                  REQUIRED: 消费的主题

```

（2）开始测试

```
 bin/kafka-consumer-perf-test.sh  --broker-list kms-2:9092,kms-3:9092,kms-4:9092    --topic test --fetch-size 10000 --messages 10000000 --threads 1
 
--topic 指定topic的名称
--fetch-size 指定每次fetch的数据的大小
--messages 总共要消费的消息个数

```

（3）结果分析

开始测试时间start.time：2019-08-07 23:10:20:560

结束时间end.time：2019-08-07 23:10:34:785 

最大吞吐率data.consumed.in.MB： 9.5367MB/s

平均每秒消费MB.sec：0.6704MB/s

最大每秒消费100000条 data.consumed.in.nMsg：100000条

平均每秒消费21722.4153条 nMsg.sec：7029.8770条

 rebalance.time.ms：192

fetch.time.ms：14033

fetch.MB.sec：0.6796

fetch.nMsg.sec：7126.0600


原文链接：https://blog.csdn.net/jmx_bigdata/article/details/98788916