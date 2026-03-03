# Architecture Diagram

JeecgBoot is a full-stack enterprise rapid development platform built on Spring Boot 3 (Java 17) for the backend and Vue 3 for the frontend, supporting both monolithic and microservices deployment modes.

## Application Architecture

```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        Browser["Browser / Desktop\n(Electron)"]
        Mobile["Mobile App\n(UniApp)"]
    end

    subgraph Frontend["Frontend - jeecgboot-vue3\n(Vue 3, Vite, TypeScript, Ant Design Vue)"]
        UI["UI Components\n(Ant Design Vue, VXE Table, ECharts)"]
        State["State Management\n(Pinia, Vue Router)"]
        AIChat["AI Chat Interface\n(@jeecg/aiflow)"]
        LowCode["Low-Code Designer\n(@jeecg/online, LogicFlow)"]
    end

    subgraph Gateway["API Gateway\n(Spring Cloud Gateway)"]
        GW["Route & Load Balance\nAuth Filter"]
    end

    subgraph Backend["Backend - jeecg-boot (Spring Boot 3.5, Java 17)"]
        subgraph Modules["Application Modules"]
            SysModule["jeecg-module-system\nUsers, Roles, Menus,\nPermissions, Org"]
            AIModule["jeecg-boot-module-airag\nAI RAG, Vector Search,\nDeepSeek Integration"]
            DemoModule["jeecg-module-demo\nDemo & Examples"]
        end

        subgraph Core["jeecg-boot-base-core"]
            Security["Security\n(Shiro + JWT)"]
            ORM["ORM Layer\n(MyBatis-Plus, Druid)"]
            Report["JimuReport\nBI Dashboard"]
            Scheduler["Scheduler\n(Quartz, XXL-Job)"]
            CodeGen["Low-Code\nCode Generator"]
        end
    end

    subgraph Config["Config and Discovery\n(Alibaba Nacos)"]
        ConfigCenter["Config Center"]
        ServiceDiscovery["Service Discovery"]
    end

    subgraph Storage["Data Storage"]
        MySQL[(MySQL\nPrimary DB)]
        Redis[(Redis\nCache and Sessions)]
        PGVector[(PostgreSQL + pgvector\nAI Embeddings)]
        FileStorage["File Storage\n(Local / MinIO / Alibaba OSS)"]
    end

    subgraph MQ["Message Queue"]
        RabbitMQ["RabbitMQ"]
        RocketMQ["RocketMQ"]
    end

    subgraph External["External Services"]
        DeepSeek["DeepSeek AI API"]
        AliyunSMS["Alibaba Cloud SMS"]
        TencentSMS["Tencent Cloud SMS"]
        SocialLogin["Social Login\n(WeChat, DingTalk, GitHub)"]
        CAS["CAS SSO"]
        UniPush["UniPush Notifications"]
        BaiduOCR["Baidu OCR"]
        GaodeMap["Gaode Map API"]
    end

    Browser --> Frontend
    Mobile --> Frontend
    Frontend -- "HTTP REST / SSE" --> Gateway
    Frontend -- "HTTP REST / SSE" --> Backend
    Gateway -- "Route" --> Backend
    Backend -- "Register and Fetch Config" --> Config

    SysModule --> Core
    AIModule --> Core
    DemoModule --> Core

    Core --> MySQL
    Core --> Redis
    AIModule --> PGVector
    Core --> FileStorage
    Core --> MQ

    AIModule -- "LLM API" --> DeepSeek
    Core -- "SMS" --> AliyunSMS
    Core -- "SMS" --> TencentSMS
    Core -- "OAuth2" --> SocialLogin
    Core -- "SSO" --> CAS
    Core -- "Push" --> UniPush
    Core -- "OCR" --> BaiduOCR
    Core -- "Map" --> GaodeMap
```
