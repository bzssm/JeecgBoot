# Architecture Diagram

JeecgBoot is a Spring Boot 3 based low-code platform with a multi-module Maven structure, supporting both monolithic and microservice deployment modes with a Vue3 frontend.

## Application Architecture

```mermaid
flowchart TD
    subgraph Frontend["Frontend Layer"]
        VUE["Vue3 Frontend\n(jeecgboot-vue3)"]
    end

    subgraph Gateway["Gateway Layer"]
        GW["Spring Cloud Gateway\n(jeecg-cloud-gateway)"]
    end

    subgraph AppLayer["Application Layer - Spring Boot 3.5 / Java 17"]
        subgraph Core["Core Modules"]
            SYS["System Module\n(jeecg-module-system)\nUser, Role, Permission, Menu"]
            DEMO["Demo Module\n(jeecg-module-demo)"]
            AIRAG["AI RAG Module\n(jeecg-boot-module-airag)\nChatGPT Integration"]
        end
        subgraph Infra["Infrastructure"]
            BASE["Base Core\n(jeecg-boot-base-core)\nShiro + JWT Security"]
            REPORT["JimuReport\nLow-Code Reporting"]
            JOB["XXL-Job\nScheduled Tasks"]
        end
    end

    subgraph MicroSvc["Microservice Layer (Spring Cloud Alibaba)"]
        NACOS["Nacos\nService Discovery + Config"]
        CLOUD_SYS["Cloud System Service\n(jeecg-system-cloud-start)"]
        MONITOR["Cloud Monitor\n(Spring Boot Admin)"]
    end

    subgraph DataLayer["Data Layer"]
        MYSQL[("MySQL\nPrimary Database")]
        REDIS[("Redis\nCache + Session + Distributed Lock")]
        MONGO[("MongoDB\nDocument Storage")]
    end

    subgraph MQ["Messaging"]
        RABBIT["RabbitMQ"]
        ROCKET["RocketMQ"]
    end

    subgraph Storage["Object Storage"]
        MINIO["MinIO"]
        OSS["Aliyun OSS"]
        QINIU["Qiniu Cloud Storage"]
    end

    subgraph ExternalSvc["External Services"]
        OAUTH["OAuth / JustAuth\nThird-party Login"]
        SMS["SMS\nAliyun / Tencent"]
        WECHAT["WeChat / DingTalk\nEnterprise IM"]
        BAIDU["Baidu AI\nOCR Services"]
    end

    VUE -->|"REST API / WebSocket"| GW
    VUE -->|"Direct (Monolithic)"| BASE
    GW --> CLOUD_SYS
    GW --> SYS

    SYS --> BASE
    DEMO --> BASE
    AIRAG --> BASE
    REPORT --> BASE
    JOB --> BASE

    NACOS -->|"Service Registry + Config"| CLOUD_SYS
    NACOS -->|"Service Registry + Config"| GW
    CLOUD_SYS --> MONITOR

    BASE -->|"MyBatis-Plus + Druid"| MYSQL
    BASE -->|"Spring Data Redis"| REDIS
    BASE -->|"Spring Data MongoDB"| MONGO

    SYS --> RABBIT
    SYS --> ROCKET

    BASE --> MINIO
    BASE --> OSS
    BASE --> QINIU

    SYS --> OAUTH
    SYS --> SMS
    SYS --> WECHAT
    AIRAG --> BAIDU
```
