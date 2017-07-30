# 流量记录

## 收集频率
- 每分钟节点上传用户流量信息及客户端IP数
  - 服务ID，使用者IP
  - 便于显示折线图？
  - @TODO 具体细节需要讨论
- 每周压缩一次
  - 最大压缩按周计算是因为一年52周似乎按照数据库100w条储存是够的
  - 需要商议


## 保留细节
- 使用者IP
- 流量（废话）
- （等待补充）

## 储存结构
 - 是否需要累计到`Service Model`?
 - 按照 Day,Week 创建 Table
 -

## 问题
- 由于目前并行两种计费规则（流量，Coin），储存流量信息应该保留到什么程度？
  - Immediate Table必须最详细
  - Day 不压缩
  - 每周压缩至天
     - 参考 Nico.one 保留24H 按照每小时记录;30天每天总量
  - 那就每天压缩？直接Event Driven， **FailOrCreate**
  - 每天执行一次清理就好了，删掉三十天前的记录
  - 整合记录的时候直接存JSON,数据库就是Longtext，拿出来解包好了，不然太麻烦了……
  - 等等这样子就不能 Fail Or Create 了，因为数据格式不支持
    - Hour 分条存，Day 打包无所谓？
    - IP 链接频率可以试试在打包时 直接关联到 Service 本身
     - 限制就按照每小时/天进行，多的就不做限制了

## Summery

Immediate->Hour->Day  
IP 从 Immediate 的时候打包到 Hour 和 Day   
Great！

- @TODO 清理手册
- 还可以测试一下（不过还是算了）
