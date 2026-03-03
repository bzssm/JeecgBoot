# Architecture Diagram

JeecgBoot is a Java-based low-code development platform built on Spring Boot 3 with a Vue3 frontend, supporting both monolithic and microservices deployment modes.

## Application Architecture

```mermaid
flowchart TB
    subgraph Client["Presentation Layer"]
        UI["Vue3 Frontend\njeecgboot-vue3\nVite / Ant Design Vue"]
    end

    subgraph Backend["Backend Services (Spring Boot 3.5.5 / Java 17)"]
        GW["API Gateway\nSpring Cloud Gateway\nJWT + Apache Shiro Auth"]
        subgraph Modules["Business Modules"]
            SYS["System Module\njeecg-system-biz\nUser / Role / Permission / Dict / Log"]
            DEMO["Demo Module\njeecg-module-demo\nSample Business Logic"]
            AIRAG["AI-RAG Module\njeecg-boot-module-airag\nVector Search / Embeddings"]
        end
        subgraph Core["Core & Infrastructure"]
            CORE["Base Core\njeecg-boot-base-core\nCommon Utils / AutoPOI / Knife4j"]
            REPORT["JimuReport\nLow-Code Report Engine\nBI Dashboard"]
            SCHED["Scheduler\nQuartz + XXL-Job"]
            MQ["Message Queues\nRabbitMQ / RocketMQ"]
        end
    end

    subgraph Data["Data Storage"]
        MYSQL["MySQL\nPrimary Database\nDruid + MyBatis-Plus"]
        REDIS["Redis\nCache / Session\nRedisson Distributed Lock"]
        PG["PostgreSQL\nAI-RAG Vector Store\npgvector Embeddings"]
        MONGO["MongoDB\nDocument Store\nOptional"]
    end

    subgraph Cloud["External Services"]
        AI["AI Services\nDeepSeek / ChatGPT\nNatural Language Processing"]
        OSS["Object Storage\nAliyun OSS / MinIO\nQiniu / Local File"]
        SMS["SMS Gateway\nAliyun SMS / Tencent SMS"]
        OAUTH["Third-Party OAuth\nGitHub / WeChat / DingTalk"]
        NACOS["Nacos\nService Discovery\nConfig Center"]
    end

    UI -->|"REST API / WebSocket"| GW
    GW --> SYS
    GW --> DEMO
    GW --> AIRAG
    GW --> REPORT
    SYS --> CORE
    DEMO --> CORE
    AIRAG --> CORE
    REPORT --> CORE
    SCHED --> SYS
    MQ --> SYS

    SYS --> MYSQL
    SYS --> REDIS
    DEMO --> MYSQL
    AIRAG --> PG
    REPORT --> MYSQL
    REPORT --> MONGO

    SYS --> OSS
    SYS --> SMS
    SYS --> OAUTH
    AIRAG --> AI
    Backend -.->|"Microservices Mode"| NACOS
```
