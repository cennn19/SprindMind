```mermaid
graph TD
    %% Styling
    classDef layerBox fill:#f8f9fa,stroke:#dee2e6,stroke-width:2px,rx:10,ry:10;
    classDef clientBox fill:#f3f0ff,stroke:#d0bfff,stroke-width:2px,rx:8,ry:8;
    classDef gatewayBox fill:#e6fcf5,stroke:#96f2d7,stroke-width:2px,rx:8,ry:8;
    classDef coreBox fill:#e7f5ff,stroke:#a5d8ff,stroke-width:2px,rx:8,ry:8;
    classDef aiBox fill:#fff4e6,stroke:#ffd8a8,stroke-width:2px,rx:8,ry:8;
    classDef infraBox fill:#fff0f6,stroke:#ffdeeb,stroke-width:2px,rx:8,ry:8;
    classDef extBox fill:#f4fce3,stroke:#d8f5a2,stroke-width:2px,rx:8,ry:8;
    classDef nodeItem fill:#ffffff,stroke:#ced4da,stroke-width:1px,rx:5,ry:5;

    %% 1. Client Layer
    subgraph Client_Layer ["Client Layer"]
        direction LR
        C1["Next.js 14+ (SSR)"]:::nodeItem
        C2["React Query + Zustand"]:::nodeItem
        C3["Tailwind + Framer"]:::nodeItem
        C4["Recharts / D3.js"]:::nodeItem
    end
    class Client_Layer clientBox;

    %% 2. API Gateway & Auth
    subgraph API_Gateway ["API Gateway & Auth"]
        direction LR
        G1["Rate limiting - OAuth 2.0 - RBAC - TLS 1.3"]:::nodeItem
        G2["Socket.io (real-time WebSocket)"]:::nodeItem
    end
    class API_Gateway gatewayBox;

    %% Flow 1
    Client_Layer -- "HTTPS / WS" --> API_Gateway

    %% 3. Core Services
    subgraph Core_Services ["Core Services (Node.js + Express)"]
        direction TB
        CS1["Task Service<br/>(CRUD, intake, bulk import)"]:::nodeItem
        CS2["Workload Service<br/>(Capacity, sprint balancer)"]:::nodeItem
        CS3["Skill Service<br/>(Mapping, proficiency level)"]:::nodeItem
        CS4["Empathy Engine<br/>(Mood check-in, SOS relay)"]:::nodeItem
        CS5["Notification Service<br/>(Private SOS, alerts)"]:::nodeItem
        CS6["Webhook Service<br/>(GitHub sync, signature verify)"]:::nodeItem
    end
    class Core_Services coreBox;

    %% Flow 2
    API_Gateway --> Core_Services

    %% 4. AI & Data Layers (Side by side mapping)
    subgraph Middle_Layer [" "]
        direction LR
        subgraph AI_Stack ["AI Stack"]
            direction TB
            A1["Gemini 1.5 Flash<br/>(Main LLM free tier)"]:::nodeItem
            A2["Claude / OpenAI<br/>(Fallback NLP engine)"]:::nodeItem
            A3["Custom System Prompt Engine → JSON output"]:::nodeItem
        end
        class AI_Stack aiBox;

        subgraph Data_Layer ["Data Layer"]
            direction TB
            D1[("Supabase (PostgreSQL)")]:::nodeItem
            D2[("Redis (priority cache)")]:::nodeItem
        end
        class Data_Layer nodeItem;
    end
    style Middle_Layer fill:none,stroke:none;

    %% Flow 3
    Core_Services -- "AI call" --> AI_Stack
    AI_Stack --> Data_Layer
    Core_Services --> Data_Layer

    %% 5. Infrastructure & DevOps
    subgraph Infra_DevOps ["Infrastructure & DevOps"]
        direction LR
        I1["Vercel (frontend)"]:::nodeItem
        I2["Render (backend)"]:::nodeItem
        I3["Cloudflare CDN"]:::nodeItem
        I4["GitHub Actions"]:::nodeItem
        I5["Sentry"]:::nodeItem
    end
    class Infra_DevOps infraBox;

    %% Flow 4
    Middle_Layer --> Infra_DevOps

    %% 6. External Integrations
    subgraph External_Integrations ["External Integrations"]
        direction LR
        E1["GitHub<br/>(Issues, PRs, webhook)"]:::nodeItem
        E2["GitLab<br/>(MR, CI/CD)"]:::nodeItem
        E3["Jira<br/>(Two-way sync)"]:::nodeItem
        E4["Linear<br/>(Issues, cycles)"]:::nodeItem
        E5["Notion / Asana<br/>(DB sync, tasks)"]:::nodeItem
    end
    class External_Integrations extBox;

    %% Flow 5
    Infra_DevOps -- "webhook" --> External_Integrations