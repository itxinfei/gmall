
<div align="center">

<h1 style="font-size: 36px;">🛒 Gmall 电商平台</h1>
**一站式电商解决方案 | 支持商品管理、订单处理、支付集成、用户系统**  

🚀 项目地址：[https://gitee.com/itxinfei/gmall](https://gitee.com/itxinfei/gmall)  
👥 QQ交流群：[661543188](https://qm.qq.com/cgi-bin/qm/qr?k=9yLlyD1dRBL97xmBKw43zRt0-6xg8ohb&jump_from=webapi)  
📧 邮箱支持：[747011882@qq.com](http://mail.qq.com/cgi-bin/qm_share?t=qm_mailme&email=f0hLSE9OTkdHTT8ODlEcEBI)  

<p align="center">
  <img alt="JDK" src="https://img.shields.io/badge/JDK-1.8%2B-brightgreen">
  <img alt="Maven" src="https://img.shields.io/badge/maven-3.3%2B-yellowgreen">
  <img alt="License" src="https://img.shields.io/badge/license-Apache-green">
  <img alt="Build" src="https://img.shields.io/badge/build-passing-brightgreen">
</p>

</div>



## 📌 项目简介  
**Gmall** 是一个面向中小型企业的开源电商平台，支持从商品上架、订单处理到支付结算的全流程管理。  
系统设计目标：  
- **高可用性**：基于微服务架构支持水平扩展  
- **业务灵活**：模块化设计适配多种电商模式（B2C/B2B/团购）  
- **性能优化**：Redis缓存、异步消息队列、分布式事务保障  

### 🎯 核心价值  
- **开箱即用**：提供完整电商功能模块，降低二次开发成本  
- **技术前沿**：采用Spring Cloud Alibaba、RocketMQ、Seata等主流技术栈  
- **数据驱动**：集成ECharts可视化报表，支持实时销售分析  

---

## 🧩 用户端口  
| 端口类型       | 功能描述                                                                 | 技术实现                  |
|----------------|--------------------------------------------------------------------------|---------------------------|
| **后台管理端** | 商品管理、订单审核、库存监控、促销配置                                    | Vue3 + Element Plus       |
| **用户端App**  | 商品浏览、下单支付、订单追踪、售后申请                                      | Weex + WebSocket实时通知  |
| **商家入驻端** | 店铺管理、商品发布、财务结算                                                | React + Ant Design        |

---

## 📐 项目架构  
### 系统架构全景  
![系统架构](docs/%E8%B0%B7%E7%B2%92%E5%95%86%E5%9F%8E-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E5%9B%BE.jpg)
**核心设计原则**：  
- **微服务化**：基于Spring Cloud Alibaba拆分为8大业务域  
- **数据分层**：OLTP（MySQL）与OLAP（ClickHouse）分离  
- **多级缓存**：Redis热点缓存 + Caffeine本地缓存  

### 微服务架构详解  
![微服务架构](docs/%E8%B0%B7%E7%B2%92%E5%95%86%E5%9F%8E-%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E5%9B%BE2.png)
**服务划分**：  
1. **gmall-auth**：统一鉴权中心（JWT + OAuth2）  
2. **gmall-gateway**：API网关（路由/限流/熔断）  
3. **gmall-product**：商品服务（Elasticsearch全文检索）  
4. **gmall-order**：订单服务（Seata分布式事务）  
5. **gmall-payment**：支付服务（支付宝沙箱/微信支付SDK）  
6. **gmall-wms**：仓储服务（库存扣减/锁定）  
7. **gmall-report**：数据报表（ECharts可视化）  
8. **gmall-config**：配置中心（Nacos动态配置）  

---

## 🧱 技术架构体系  

### 核心技术栈  
| 层级         | 技术选型                                                                 |
|--------------|--------------------------------------------------------------------------|
| **前端**     | Vue3 + React + Weex + ECharts + Vant                                    |
| **网关层**   | Spring Cloud Gateway + Sentinel                                         |
| **服务层**   | Spring Boot 2.7 + MyBatis Plus + MapStruct                              |
| **消息队列** | RocketMQ 4.9 + Kafka 3.0                                                |
| **数据层**   | MySQL 8.0 + Redis 6.2 + ClickHouse 22.3                                |
| **中间件**   | Nacos 2.1 + Seata 1.6 + XXL-JOB                                        |
| **监控**     | Prometheus + Grafana + SkyWalking                                       |

---

## 📊 整体业务流程  

**核心流程解析**：  
1. **商品上架**：商家上传商品 → Elasticsearch同步索引  
2. **下单流程**：库存预扣 → 订单生成 → 异步消息通知支付  
3. **支付回调**：支付宝/微信回调 → RocketMQ异步处理订单状态  
4. **售后流程**：用户申请退款 → 审核通过后触发Seata分布式事务回滚  

---

## 📁 模块功能详解  
### 核心业务模块  
#### 1. **商品服务（gmall-product）**  
- 支持SKU多规格管理（颜色/尺寸/库存）  
- Elasticsearch实现商品搜索（分词/聚合查询）  
- 文件上传整合MinIO（支持图片/视频存储）  

#### 2. **订单服务（gmall-order）**  
- Seata保障下单、扣库存、支付的事务一致性  
- 订单分表策略（按用户ID哈希分片）  
- 订单超时自动关闭（RabbitMQ延迟队列）  

#### 3. **支付服务（gmall-payment）**  
- 支付宝沙箱/微信支付SDK集成  
- 支付记录持久化（MySQL + Redis双写）  
- 支付对账系统（定时任务校验差异订单）  

#### 4. **仓储服务（gmall-wms）**  
- 库存扣减策略（乐观锁防超卖）  
- 库存预警机制（阈值提醒）  
- 仓库分仓管理（支持多地库存同步）  

---

## 💾 数据库设计  
| 数据库名          | 数据量级     | 核心表设计                                                                 |
|-------------------|--------------|----------------------------------------------------------------------------|
| `gmall_product`   | 500万+       | 商品表（字段含SPU/SKU结构、分类、属性）                                     |
| `gmall_order`     | 1亿+         | 订单主表+子表（按月份拆分，ShardingSphere实现）                             |
| `gmall_payment`   | 实时写入     | 支付记录表（支付宝/微信交易号索引）                                         |
| `clickhouse_log`  | 10亿+        | 用户行为日志（埋点数据实时分析）                                            |

---

## 🧰 部署与依赖  
### 快速部署指南  
```bash
# 1. 安装依赖中间件
docker-compose up -d

# 2. 初始化数据库
mysql -u root -p gmall_product < sql/init_product.sql

# 3. 启动微服务
mvn clean install && java -jar gmall-gateway.jar
```

### 自定义Maven依赖  
```xml
<!-- 商品搜索模块 -->
<dependency>
  <groupId>com.gmall</groupId>
  <artifactId>gmall-es-sdk</artifactId>
  <version>1.0.0</version>
</dependency>

<!-- 支付工具类 -->
<dependency>
  <groupId>com.gmall</groupId>
  <artifactId>gmall-payment-utils</artifactId>
  <version>1.0.0</version>
</dependency>
```

---

## 📦 关联仓库协同  
1. **Gmall-通用权限**  
   - 提供RBAC权限模型实现  
   - 支持商家角色权限隔离  

2. **Gmall-数据分析平台**  
   - 基于Flink实时计算用户行为  
   - ClickHouse支撑大屏可视化  

---

## 📱 关注微信公众号  
![微信公众号二维码](docs/心飞为你飞.jpg)  
**获取最新更新动态与技术支持文档**

---

## 📝 注意事项  
1. **部署依赖**：  
   - 需安装JDK1.8+、Maven3.3+、Docker  
   - 中间件依赖：Nacos、Redis、RocketMQ  

2. **数据初始化**：  
   ```bash
   # 初始化基础数据
   mysql -u root -p gmall_product < sql/init_base.sql
   ```

3. **日志排查**：  
   - 日志目录：`logs/gmall/*.log`  
   - 关键指标监控：`/actuator/prometheus`  

---

