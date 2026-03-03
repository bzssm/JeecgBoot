# Architecture Diagram

JeecgBoot is a full-stack, cloud-native low-code platform built on Spring Boot 3 and Vue 3, featuring microservice capabilities, AI/RAG integration, and multi-database support.

## Application Architecture

```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        Browser["Browser / Electron\nVue 3 + Vite + Ant Design Vue 4\nPinia, Vue Router, Axios"]
    end

    subgraph Gateway["API Gateway (Port 9999)"]
        GW["Spring Cloud Gateway\nSentinel Rate Limiting\nCORS, JWT Validation"]
    end

    subgraph Registry["Service Registry and Config"]
        Nacos["Nacos\nService Discovery\nConfig Center"]
        Sentinel["Sentinel Dashboard\nCircuit Breaker"]
    end

    subgraph Services["Backend Microservices"]
        SysService["System Service (Port 7001)\nSpring Boot 3 + Spring Cloud\nShiro + JWT Auth\nMyBatis Plus + Druid\nCode Generator, Workflow"]
        AIService["AI RAG Service (Port 7008)\nSpring AI\nEmbedding Store"]
        DemoService["Demo Service\nBusiness Modules\nReport Engine"]
    end

    subgraph Messaging["Async Messaging"]
        RabbitMQ["RabbitMQ"]
        RocketMQ["RocketMQ"]
    end

    subgraph DataStorage["Data Storage"]
        MySQL["MySQL\nPrimary Database"]
        Redis["Redis\nCache and Sessions"]
        MongoDB["MongoDB\nDocument Store"]
        PgVector["PostgreSQL pgvector\nAI Embeddings"]
        MinIO["MinIO\nObject Storage"]
    end

    subgraph Scheduler["Task Scheduling"]
        XXLJob["XXL-Job\nDistributed Job Scheduler"]
    end

    subgraph ExternalAPIs["External Integrations"]
        Aliyun["Aliyun OSS and SMS"]
        OAuth["OAuth Providers\nJustAuth"]
        OpenAI["OpenAI / LLM APIs"]
    end

    Browser -->|"HTTPS"| GW
    GW -->|"Route"| SysService
    GW -->|"Route"| AIService
    GW -->|"Route"| DemoService
    GW <-->|"Discover"| Nacos
    SysService <-->|"Register and Config"| Nacos
    AIService <-->|"Register and Config"| Nacos
    DemoService <-->|"Register and Config"| Nacos
    SysService --> MySQL
    SysService --> Redis
    SysService --> MongoDB
    SysService --> MinIO
    AIService --> PgVector
    AIService --> MySQL
    AIService --> Redis
    DemoService --> MySQL
    DemoService --> Redis
    SysService <-->|"Events"| RabbitMQ
    SysService <-->|"Events"| RocketMQ
    SysService --> XXLJob
    SysService -->|"File Storage"| Aliyun
    SysService -->|"Social Login"| OAuth
    AIService -->|"LLM Inference"| OpenAI
```
