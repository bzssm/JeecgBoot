# Architecture Diagram

JeecgBoot is a full-stack low-code development platform with a Vue3 frontend and a Spring Boot 3 backend, supporting both monolithic and microservice deployment modes.

## Application Architecture

```mermaid
flowchart TD
    subgraph Presentation["Presentation Layer"]
        UI["Vue3 Frontend\nAnt Design Vue · Vite · TypeScript\nPort 80 via Nginx"]
    end

    subgraph Gateway["API Gateway (Microservice Mode)"]
        GW["Spring Cloud Gateway\nNacos Service Discovery"]
    end

    subgraph Backend["Backend Layer - Spring Boot 3 / Java 17"]
        SYS["System Module\njeecg-system-biz\nPort 8080"]
        AIRAG["AI RAG Module\njeecg-boot-module-airag\nChatGPT Integration"]
        DEMO["Demo Module\njeecg-module-demo"]
        REPORT["JimuReport\nJimuBI Dashboard"]
    end

    subgraph Security["Security"]
        SEC["Apache Shiro + JWT\nShiro-Redis Session"]
    end

    subgraph DataAccess["Data Access Layer"]
        ORM["MyBatis-Plus 3.5\nDruid Connection Pool\nDynamic Datasource"]
    end

    subgraph Storage["Data Storage"]
        MYSQL["MySQL 8\nPrimary Database"]
        REDIS["Redis 5\nCache and Sessions"]
        PGVEC["PostgreSQL pgvector\nVector Database for AI"]
        MINIO["MinIO\nObject Storage"]
    end

    subgraph Messaging["Messaging and Scheduling"]
        MQ["RabbitMQ\nRocketMQ"]
        JOB["XXL-Job\nQuartz Scheduler"]
    end

    subgraph External["External Integrations"]
        CLOUD["Aliyun OSS\nQiniu Cloud\nAliyun SMS"]
        SOCIAL["WeChat\nDingTalk\nThird-Party OAuth"]
        AI["OpenAI\nBaidu OCR"]
    end

    UI -->|"REST API / WebSocket"| SYS
    UI -->|"REST API / WebSocket"| GW
    GW -->|"Route"| SYS
    GW -->|"Route"| AIRAG
    SYS --> SEC
    SYS --> ORM
    AIRAG --> ORM
    DEMO --> ORM
    REPORT --> ORM
    ORM --> MYSQL
    SEC --> REDIS
    ORM --> REDIS
    AIRAG --> PGVEC
    SYS --> MQ
    SYS --> JOB
    SYS --> MINIO
    SYS --> CLOUD
    SYS --> SOCIAL
    AIRAG --> AI
```
