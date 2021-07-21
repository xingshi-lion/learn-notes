# Ip2region-准确率达99.9%的离线IP地址定位

## 1、Ip2region的Github仓库地址

https://github.com/lionsoul2014/ip2region

## 2、Ip2region是什么？

ip2region - 准确率99.9%的离线IP地址定位库，0.0x毫秒级查询，ip2region.db数据库只有数MB，提供了java,php,c,python,nodejs,golang,c#等查询绑定和Binary,B树,内存三种查询算法。

## 3、Ip2region特性

数据聚合了一些知名ip到地名查询提供商的数据，这些是他们官方的的准确率，经测试着实比经典的纯真IP定位准确一些。

ip2region的数据聚合自以下服务商的开放API或者数据(升级程序每秒请求次数2到4次):

01, >80%, 淘宝IP地址库, http://ip.taobao.com/

02, ≈10%, GeoIP, https://geoip.com/

03, ≈2%, 纯真IP库, http://www.cz88.net/

**备注：**如果上述开放API或者数据都不给开放数据时ip2region将停止数据的更新服务。

## 4、标准化的数据格式

_城市Id|国家|区域|省份|城市|ISP_

## 5、maven仓库地址

```xml
<dependency>
    <groupId>org.lionsoul</groupId>
    <artifactId>ip2region</artifactId>
    <version>1.7.2</version>
</dependency>
```

## 6、ip2region 并发使用

1. 全部binding的各个search接口都**不是**线程安全的实现，不同线程可以通过创建不同的查询对象来使用，并发量很大的情况下，binary和b-tree算法可能会打开文件数过多的错误，请修改内核的最大允许打开文件数(fs.file-max=一个更高的值)，或者使用持久化的memory算法。

2. memorySearch接口，在发布对象前进行一次预查询(本质上是把ip2region.db文件加载到内存)，可以安全用于多线程环境。