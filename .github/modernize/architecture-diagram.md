# Architecture Diagram

JeecgBoot is a full-stack enterprise low-code platform built on Spring Boot 3 and Vue 3, supporting both monolithic and microservices deployment modes.

## Application Architecture

```mermaid
flowchart TB
    subgraph Frontend["Frontend Layer"]
        VUE["Vue 3 + Vite + TypeScript\nAnt Design Vue\nPinia State Management"]
    end

    subgraph Gateway["API Gateway Layer"]
        GW["Spring Cloud Gateway\nPort 9999\nSentinel Rate Limiting\nCORS Handling"]
    end

    subgraph ServiceDiscovery["Service Discovery and Config"]
        NACOS["Nacos\nPort 8848\nService Registry\nConfiguration Center"]
    end

    subgraph BusinessServices["Business Services Layer"]
        SYSTEM["System Service\njeecg-system\nPort 7001 / 8080\nUser, Role, Permission\nCode Generation"]
        DEMO["Demo Service\njeecg-demo\nSample Business Modules"]
        AIRAG["AI RAG Service\nPort 7008\nLangChain4j\nLiteFlow Orchestration"]
    end

    subgraph Security["Security and Auth"]
        SHIRO["Apache Shiro\nJWT Authentication\nOAuth2 via JustAuth"]
    end

    subgraph ReportBI["Reporting and BI"]
        JIMU["JimuReport\nJimubi BI\nAuto POI Excel Export"]
    end

    subgraph Observability["Observability and Scheduling"]
        SENTINEL["Sentinel Dashboard\nPort 9000\nTraffic Control"]
        XXLJOB["XXL-JOB\nPort 9080\nDistributed Task Scheduling"]
        MONITOR["Spring Boot Admin\nService Monitoring"]
    end

    subgraph DataLayer["Data Layer"]
        MYSQL["MySQL\nPort 3306\nPrimary Database"]
        REDIS["Redis\nPort 6379\nCache and Sessions"]
        PGVECTOR["pgvector on PostgreSQL\nPort 5432\nVector Embeddings for AI"]
        MINIO["MinIO\nObject Storage\nFile Uploads"]
        MYBATISPLUS["MyBatis-Plus\nDruid Connection Pool\nDynamic Datasource"]
    end

    subgraph ExternalAPIs["External Integrations"]
        LLM["LLM Providers\nOpenAI / Ollama\nAI Inference"]
        SMS["SMS Providers\nAliyun / Tencent\nOSS / Qiniu Storage"]
        THIRDAUTH["Third-party OAuth\nWeChat / GitHub / etc\nvia JustAuth"]
    end

    VUE -->|"HTTP/REST"| GW
    GW -->|"Route"| SYSTEM
    GW -->|"Route"| DEMO
    GW -->|"Route"| AIRAG
    SYSTEM --- NACOS
    DEMO --- NACOS
    AIRAG --- NACOS
    GW --- NACOS

    SYSTEM --- SHIRO
    SYSTEM --- JIMU
    SYSTEM --- MYBATISPLUS
    AIRAG --- MYBATISPLUS

    MYBATISPLUS --> MYSQL
    SYSTEM --> REDIS
    AIRAG --> PGVECTOR
    SYSTEM --> MINIO

    AIRAG -->|"LLM calls"| LLM
    SYSTEM -->|"SMS / Storage"| SMS
    SYSTEM -->|"Social Login"| THIRDAUTH

    GW --- SENTINEL
    SYSTEM --- XXLJOB
    SYSTEM --- MONITOR
```
