# Architecture Diagram

JeecgBoot is a full-stack enterprise low-code platform built on Spring Boot 3 and Vue 3, supporting both monolithic and microservices deployment modes with AI integration capabilities.

## Application Architecture

```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        VUE["Vue 3 Frontend\nTypeScript · Vite · Pinia\nAnt Design Vue"]
        MOBILE["Mobile / App\nUni-App"]
    end

    subgraph Gateway["API Gateway Layer"]
        GW["Spring Cloud Gateway\nPort 9999\nSentinel Rate Limiting · CORS · Auth Filter"]
    end

    subgraph Backend["Backend Services"]
        direction TB
        SYS["System Service\njeecg-system-start  Port 8080\nSpring Boot 3.5 · Java 17"]
        AIRAG["AI RAG Service\njeecg-boot-module-airag\nLangChain4j · Vector Search"]
        DEMO["Demo Service\njeecg-module-demo\nSample Business Modules"]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        SYSTEMBIZ["System Business Layer\njeecg-system-biz\nUser · Role · Permission · Menu · Dict · Workflow"]
        BASECORE["Base Core\njeecg-boot-base-core\nJWT Auth · MyBatis-Plus · Druid · Shiro"]
    end

    subgraph Infra["Infrastructure  Spring Cloud Alibaba"]
        NACOS["Nacos\nService Discovery\nConfig Management"]
        SENTINEL["Sentinel\nCircuit Breaker\nRate Limiting"]
        XXLJOB["XXL-Job\nDistributed Task\nScheduling"]
        MONITOR["Spring Boot Admin\nService Monitoring"]
        QUARTZ["Quartz Scheduler\nJob Management"]
        SEATA["Seata\nDistributed Transactions"]
    end

    subgraph DataLayer["Data Layer"]
        MYSQL[("MySQL\nPrimary Database\nMulti-Datasource")]
        REDIS[("Redis\nCache · Sessions\nDistributed Lock")]
        PGVECTOR[("PostgreSQL\nAI Vector Store\nEmbeddings")]
        MINIO["MinIO · Aliyun OSS\nFile Storage"]
    end

    subgraph ExternalServices["External Services"]
        AI["DeepSeek AI\nAI Chat Integration"]
        SMS["Aliyun SMS\nTencent SMS"]
        SSO["OAuth2 SSO\nGitHub · WeChat\nDingTalk · CAS"]
        MAP["Amap API\nGeo Services"]
        REPORT["JiMu Report\nBI Dashboard\nBig Screen"]
    end

    VUE -->|"REST API · WebSocket"| GW
    MOBILE -->|"REST API"| GW
    GW -->|"Route"| SYS
    GW -->|"Route"| AIRAG
    GW -->|"Route"| DEMO

    SYS --> SYSTEMBIZ
    SYSTEMBIZ --> BASECORE
    AIRAG --> BASECORE

    SYS <-->|"Register · Config"| NACOS
    GW <-->|"Register · Config"| NACOS
    GW <-->|"Flow Control"| SENTINEL

    BASECORE -->|"Read · Write"| MYSQL
    BASECORE -->|"Cache · Lock"| REDIS
    AIRAG -->|"Vector Search"| PGVECTOR
    SYS -->|"Upload · Download"| MINIO

    SYS <-->|"Schedule"| XXLJOB
    SYS <-->|"Schedule"| QUARTZ
    SYS <-->|"Monitor"| MONITOR
    SYS <-->|"Distributed TX"| SEATA

    SYS -->|"AI Chat"| AI
    SYS -->|"Notifications"| SMS
    SYS <-->|"Login"| SSO
    SYS -->|"Map Data"| MAP
    SYS -->|"Reports"| REPORT
```
