# Architecture Diagram

JeecgBoot is a cloud-native, microservices-based low-code platform built on Spring Boot 3 and Vue 3, supporting both monolithic and distributed deployment modes.

## Application Architecture

```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        VUE["Vue 3 Frontend\nVite · TypeScript · Ant Design Vue"]
    end

    subgraph Gateway["API Gateway Layer"]
        GW["Spring Cloud Gateway\nPort 9999 · Knife4j · Sentinel Rate Limiting"]
    end

    subgraph Services["Microservices Layer"]
        SYS["jeecg-system\nPort 7001 · System Management\nShiro · JWT · MyBatis-Plus"]
        AIRAG["jeecg-ai-rag\nPort 7008 · AI RAG Module\nLangChain4j · LLM Integration"]
        DEMO["jeecg-demo\nBusiness Demo Module"]
    end

    subgraph Infra["Infrastructure Layer"]
        NACOS["Nacos\nService Discovery · Config Center"]
        SENTINEL["Sentinel\nCircuit Breaker · Flow Control"]
        XXLJOB["XXL-Job\nDistributed Task Scheduler"]
        MONITOR["Spring Boot Admin\nService Monitor"]
    end

    subgraph Data["Data Storage Layer"]
        MYSQL["MySQL\nPrimary Database"]
        REDIS["Redis\nCache · Session · Token"]
        PG["PostgreSQL\nVector Store for AI RAG"]
    end

    subgraph External["External Services"]
        OSS["Aliyun OSS / MinIO / Qiniu\nFile Storage"]
        SMS["Aliyun SMS / Tencent SMS\nMessage Service"]
        OAUTH["OAuth2 Providers\nSocial Login"]
    end

    VUE -->|"HTTP / REST"| GW
    GW -->|"Service Route"| SYS
    GW -->|"Service Route"| AIRAG
    GW -->|"Service Route"| DEMO
    GW --- SENTINEL

    SYS --- NACOS
    AIRAG --- NACOS
    DEMO --- NACOS

    SYS --> MYSQL
    SYS --> REDIS
    AIRAG --> MYSQL
    AIRAG --> PG

    SYS --> OSS
    SYS --> SMS
    SYS --> OAUTH

    NACOS --> MYSQL
    XXLJOB --> MYSQL
    MONITOR --- NACOS
```
