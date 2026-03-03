# Architecture Diagram

JeecgBoot is a full-stack low-code development platform built on Spring Boot 3 and Vue 3, supporting both monolithic and microservice deployment modes.

## Application Architecture

```mermaid
flowchart TD
    subgraph Frontend["Frontend Layer"]
        VUE["Vue 3 + Vite\njeecgboot-vue3"]
    end

    subgraph Gateway["API Gateway Layer"]
        GW["Spring Cloud Gateway\njeecg-cloud-gateway\nPort 9999"]
    end

    subgraph Services["Backend Services Layer"]
        SYS["System Service\njeecg-module-system\nSpring Boot 3.5.5 / JDK 17\nPort 8080"]
        DEMO["Demo Service\njeecg-module-demo\nSpring Boot 3"]
        AIRAG["AI-RAG Service\njeecg-boot-module-airag\nSpring Boot 3"]
    end

    subgraph Infra["Infrastructure and Middleware"]
        NACOS["Nacos\nService Discovery\nConfig Center"]
        REDIS["Redis\nCache and Session"]
        MYSQL["MySQL 8\nPrimary Database"]
        QUARTZ["Quartz\nScheduled Jobs JDBC"]
        XXLJOB["XXL-JOB\nDistributed Job Scheduler"]
        SENTINEL["Sentinel\nRate Limiting and Circuit Breaker"]
        MONITOR["Spring Boot Admin\nService Monitor"]
    end

    subgraph Storage["Object Storage"]
        MINIO["MinIO\nLocal Object Storage"]
        OSS["Aliyun OSS\nCloud Object Storage"]
    end

    subgraph Security["Security Layer"]
        JWT["JWT Token Auth\njava-jwt 4.5"]
        SHIRO["Apache Shiro 2\nAuthorization"]
    end

    subgraph Persistence["Persistence Layer"]
        MP["MyBatis-Plus 3.5\nORM Framework"]
        DRUID["Druid 1.2\nConnection Pool"]
        DYNAMIC["Dynamic Datasource\nMulti-DB Support"]
    end

    VUE -->|"HTTP REST"| GW
    VUE -->|"HTTP REST direct"| SYS
    GW -->|"Route"| SYS
    GW -->|"Route"| DEMO
    GW -->|"Route"| AIRAG
    SYS --> NACOS
    DEMO --> NACOS
    AIRAG --> NACOS
    SYS --> REDIS
    SYS --> MYSQL
    SYS --> QUARTZ
    SYS --> JWT
    SYS --> SHIRO
    SYS --> MP
    MP --> DRUID
    DRUID --> DYNAMIC
    DYNAMIC --> MYSQL
    SYS --> MINIO
    SYS --> OSS
    XXLJOB --> NACOS
    SENTINEL --> NACOS
    MONITOR --> NACOS
```
