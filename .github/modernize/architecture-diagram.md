# Architecture Diagram

JeecgBoot is a full-stack low-code development platform built on Spring Boot 3 and Vue 3, supporting both monolithic and microservices deployment modes.

## Application Architecture

```mermaid
flowchart TB
    subgraph Frontend["Frontend Layer"]
        VUE["Vue 3 + Vite\njeecgboot-vue3"]
    end

    subgraph Gateway["API Gateway (Microservices Mode)"]
        GW["Spring Cloud Gateway\n+ Nacos Discovery\n+ Sentinel Rate Limiting"]
    end

    subgraph AppLayer["Application Layer"]
        SYS["System Service\njeecg-system\nSpring Boot 3.5 / Java 17"]
        DEMO["Demo Service\njeecg-module-demo"]
        AIRAG["AI RAG Service\njeecg-boot-module-airag"]
    end

    subgraph CoreLayer["Core Framework"]
        CORE["jeecg-boot-base-core\nShiro Auth / JWT\nMyBatis-Plus / Druid\nLow-Code Engine\nOnline Report / BI Dashboard"]
    end

    subgraph Storage["Data Storage"]
        MYSQL["MySQL\nPrimary Database"]
        REDIS["Redis\nCache + Session\n+ Distributed Lock"]
        PGVECTOR["PostgreSQL pgvector\nAI Embedding Store"]
        MINIO["MinIO / Aliyun OSS\nFile Storage"]
    end

    subgraph Infra["Infrastructure Services"]
        NACOS["Nacos\nConfig + Service Discovery"]
        QUARTZ["Quartz / XXL-Job\nScheduled Jobs"]
        MQ["RabbitMQ / RocketMQ\nMessage Queue"]
        SEATA["Seata\nDistributed Transactions"]
        SENTINEL["Sentinel\nCircuit Breaker"]
    end

    subgraph External["External Integrations"]
        AI["AI Services\nDeepSeek / OpenAI"]
        SMS["SMS Providers\nAliyun / Tencent"]
        OSS["Cloud Storage\nAliyun OSS / Qiniu"]
        SOCIAL["Social Login\nGitHub / WeChat\n/ DingTalk"]
        BAIDU["Baidu OCR API"]
    end

    VUE -->|REST API| GW
    VUE -->|Direct REST in monolith| SYS
    GW -->|Route| SYS
    GW -->|Route| DEMO
    GW -->|Route| AIRAG
    SYS --> CORE
    DEMO --> CORE
    AIRAG --> CORE
    CORE -->|JDBC via Druid| MYSQL
    CORE -->|Cache / Lock| REDIS
    AIRAG -->|Vector Search| PGVECTOR
    CORE -->|Upload / Download| MINIO
    SYS -->|Config / Discovery| NACOS
    CORE -->|Scheduled Tasks| QUARTZ
    SYS -->|Async Messaging| MQ
    SYS -->|Distributed TX| SEATA
    GW -->|Traffic Control| SENTINEL
    CORE -->|Chat Completion| AI
    CORE -->|Notifications| SMS
    CORE -->|File Upload| OSS
    SYS -->|OAuth2 Login| SOCIAL
    CORE -->|OCR| BAIDU
```
