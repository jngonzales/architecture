# Current Project Architecture (15% Complete)

## System Architecture

```mermaid
flowchart TB
    subgraph Client["CLIENT"]
        Browser["Browser"]
    end

    subgraph Vercel["VERCEL"]
        Next["Next.js 15 App"]
    end

    subgraph Supabase["SUPABASE"]
        Auth["Auth Service"]
        DB[(PostgreSQL)]
        Storage["File Storage"]
    end

    Browser <-->|HTTPS| Next
    Next <-->|API| Auth
    Next <-->|Queries| DB
    Next <-->|Files| Storage
```

## User Roles

```mermaid
flowchart LR
    subgraph Roles["USER ROLES - 3 Total"]
        Agent["Agent"]
        UW["Underwriter"]
        Admin["Admin"]
    end

    subgraph AgentPerms["Agent Can"]
        A1["Submit Deals"]
        A2["View Own Deals"]
        A3["Add Comments"]
        A4["Upload Photos"]
    end

    subgraph UWPerms["Underwriter Can"]
        U1["View All Deals"]
        U2["Run Underwriting"]
        U3["Update Status"]
        U4["Assign Tasks"]
    end

    subgraph AdminPerms["Admin Can"]
        AD1["Everything Above"]
        AD2["Manage Users"]
        AD3["Change Roles"]
    end

    Agent --> AgentPerms
    UW --> UWPerms
    Admin --> AdminPerms
```

## Database Schema - 6 Tables

```mermaid
erDiagram
    profiles ||--o{ deals : submits
    profiles ||--o{ properties : creates
    profiles ||--o{ underwriting_records : creates
    profiles ||--o{ deal_comments : writes
    profiles ||--o{ attachments : uploads
    
    properties ||--o{ deals : has
    properties ||--o{ attachments : has
    
    deals ||--o{ underwriting_records : has
    deals ||--o{ deal_comments : has
    deals ||--o{ attachments : has

    profiles {
        uuid id PK
        text email
        text full_name
        text role
        timestamp created_at
    }

    properties {
        uuid id PK
        uuid created_by FK
        text address
        text city
        text state
        text property_type
        int sqft
    }

    deals {
        uuid id PK
        uuid property_id FK
        uuid agent_id FK
        uuid assigned_to FK
        text status
        numeric asking_price
        numeric offer_price
        text seller_name
    }

    underwriting_records {
        uuid id PK
        uuid deal_id FK
        numeric arv
        numeric repair_estimate
        numeric max_offer
        text status
    }

    deal_comments {
        uuid id PK
        uuid deal_id FK
        uuid user_id FK
        text content
    }

    attachments {
        uuid id PK
        uuid deal_id FK
        text file_name
        text storage_path
    }
```

## Deal Pipeline - 9 Stages

```mermaid
flowchart LR
    S1["Submitted"]
    S2["Needs Info"]
    S3["Underwriting"]
    S4["Offer Prepared"]
    S5["Offer Sent"]
    S6["In Contract"]
    S7["Funding"]
    S8["Closed"]
    S9["Rejected"]

    S1 --> S2
    S1 --> S3
    S2 --> S3
    S3 --> S4
    S3 --> S9
    S4 --> S5
    S5 --> S6
    S5 --> S9
    S6 --> S7
    S6 --> S9
    S7 --> S8
```

## Page Structure

```mermaid
flowchart TD
    subgraph Public["PUBLIC"]
        Login["/login"]
    end

    subgraph Dashboard["DASHBOARD"]
        Home["/dashboard"]
        Deals["/dashboard/deals"]
        DealDetail["/dashboard/deals/id"]
        Underwriting["/dashboard/deals/id/underwriting"]
        Submit["/dashboard/submit"]
        Settings["/dashboard/settings"]
        Admin["/dashboard/admin"]
    end

    Login -->|Auth| Home
    Home --> Deals
    Home --> Submit
    Home --> Settings
    Home --> Admin
    Deals --> DealDetail
    DealDetail --> Underwriting
```

## Tech Stack

```mermaid
flowchart TB
    subgraph Frontend["FRONTEND"]
        Next["Next.js 15"]
        React["React 19"]
        TW["Tailwind CSS"]
        Shadcn["shadcn/ui"]
    end

    subgraph Backend["BACKEND"]
        ServerActions["Server Actions"]
        Middleware["Auth Middleware"]
    end

    subgraph Database["DATABASE"]
        Supa["Supabase"]
        PG["PostgreSQL"]
        RLS["Row Level Security"]
    end

    subgraph Tools["TOOLS"]
        TS["TypeScript"]
        Zod["Zod Validation"]
        RHF["React Hook Form"]
    end

    Frontend --> Backend
    Backend --> Database
    Tools -.-> Frontend
    Tools -.-> Backend
```

## Features Built

```mermaid
flowchart TB
    subgraph Done["COMPLETED"]
        F1["Authentication"]
        F2["3 User Roles"]
        F3["Deal Submission"]
        F4["Photo Upload"]
        F5["Kanban Board"]
        F6["List View"]
        F7["Deal Details"]
        F8["Underwriting Calculator"]
        F9["Comments"]
        F10["Task Assignment"]
        F11["User Settings"]
        F12["Basic Admin"]
    end

    subgraph NotDone["NOT BUILT"]
        N1["Investor Portal"]
        N2["PDF Generator"]
        N3["Analytics"]
        N4["Notifications"]
        N5["External APIs"]
    end
```

# Full Platform Architecture (100% Complete)

## System Architecture

```mermaid
flowchart TB
    subgraph Clients["CLIENTS"]
        Web["Web App"]
        Mobile["Mobile App"]
        InvestorPortal["Investor Portal"]
    end

    subgraph Vercel["VERCEL"]
        Next["Next.js 15"]
        API["API Routes"]
        Cron["Cron Jobs"]
        Webhooks["Webhooks"]
    end

    subgraph Supabase["SUPABASE"]
        Auth["Auth + RLS"]
        DB[(PostgreSQL)]
        Storage["File Storage"]
        Realtime["Realtime"]
    end

    subgraph External["INTEGRATIONS"]
        PropStream["PropStream API"]
        DocuSign["DocuSign"]
        Twilio["Twilio SMS"]
        SendGrid["SendGrid Email"]
        Stripe["Stripe Payments"]
        Maps["Google Maps"]
    end

    Clients <--> Vercel
    Vercel <--> Supabase
    Vercel <--> External
    Supabase -->|Push| Clients
```

## User Roles - 4 Total

```mermaid
flowchart TB
    subgraph Roles["ALL ROLES"]
        Agent["Agent"]
        UW["Underwriter"]
        Admin["Admin"]
        Investor["Investor"]
    end

    subgraph AgentFeatures["AGENT"]
        AF1["Submit Deals"]
        AF2["Upload Docs"]
        AF3["Track Pipeline"]
        AF4["Receive Notifications"]
    end

    subgraph UWFeatures["UNDERWRITER"]
        UF1["Analyze Deals"]
        UF2["Calculate MAO"]
        UF3["Risk Scoring"]
        UF4["Generate Offers"]
    end

    subgraph AdminFeatures["ADMIN"]
        ADF1["User Management"]
        ADF2["Analytics Dashboard"]
        ADF3["Audit Logs"]
        ADF4["System Settings"]
    end

    subgraph InvestorFeatures["INVESTOR"]
        IF1["View Approved Deals"]
        IF2["Fund Requests"]
        IF3["Portfolio Tracking"]
        IF4["ROI Reports"]
    end

    Agent --> AgentFeatures
    UW --> UWFeatures
    Admin --> AdminFeatures
    Investor --> InvestorFeatures
```

## Database Schema - 12+ Tables

```mermaid
erDiagram
    profiles ||--o{ deals : submits
    profiles ||--o{ properties : creates
    profiles ||--o{ underwriting_records : analyzes
    profiles ||--o{ deal_comments : writes
    profiles ||--o{ notifications : receives
    profiles ||--o{ audit_logs : creates
    profiles ||--o{ investor_funding : funds
    
    properties ||--o{ deals : has
    properties ||--o{ attachments : has
    
    deals ||--o{ underwriting_records : has
    deals ||--o{ deal_comments : has
    deals ||--o{ attachments : has
    deals ||--o{ deal_activities : has
    deals ||--o{ offer_guidelines : has
    deals ||--o{ investor_funding : receives

    profiles {
        uuid id PK
        text email
        text role
        jsonb notification_preferences
        boolean is_active
    }

    deals {
        uuid id PK
        serial deal_number
        uuid investor_id FK
        text status
        text sub_status
        text priority
        numeric final_price
        date closing_date
    }

    underwriting_records {
        uuid id PK
        int version
        jsonb arv_comps
        jsonb repair_breakdown
        int risk_score
        jsonb risk_factors
    }

    notifications {
        uuid id PK
        uuid user_id FK
        text type
        text title
        boolean is_read
        boolean email_sent
        boolean sms_sent
    }

    audit_logs {
        uuid id PK
        uuid user_id FK
        text action
        text entity_type
        jsonb old_values
        jsonb new_values
    }

    investor_funding {
        uuid id PK
        uuid deal_id FK
        uuid investor_id FK
        numeric requested_amount
        numeric funded_amount
        text status
    }

    offer_guidelines {
        uuid id PK
        uuid deal_id FK
        text template_type
        text pdf_url
        text docusign_envelope_id
    }

    deal_activities {
        uuid id PK
        uuid deal_id FK
        text activity_type
        text description
        jsonb metadata
    }
```

## Feature Map

```mermaid
flowchart TB
    subgraph Core["CORE - Built"]
        C1["Authentication"]
        C2["Deal Submission"]
        C3["Kanban Pipeline"]
        C4["Underwriting Calc"]
        C5["Comments"]
        C6["Task Assignment"]
    end

    subgraph New["NEW MODULES"]
        N1["Analytics Dashboard"]
        N2["Investor Portal"]
        N3["PDF Generator"]
        N4["Notifications"]
        N5["Audit Logs"]
    end

    subgraph Integrations["INTEGRATIONS"]
        I1["PropStream"]
        I2["DocuSign"]
        I3["Twilio SMS"]
        I4["SendGrid"]
        I5["Stripe"]
    end

    Core --> New
    New --> Integrations
```

## Full Page Structure

```mermaid
flowchart TD
    subgraph Public["PUBLIC PAGES"]
        Landing["/"]
        About["/about"]
        Login["/login"]
        Register["/register"]
    end

    subgraph Dashboard["DASHBOARD"]
        Home["/dashboard"]
        
        subgraph Deals["DEALS"]
            DealsList["/deals"]
            DealNew["/deals/new"]
            DealDetail["/deals/id"]
            DealUW["/deals/id/underwriting"]
            DealDocs["/deals/id/documents"]
            DealOffer["/deals/id/offer"]
        end

        subgraph Analytics["ANALYTICS"]
            AnalyticsDash["/analytics"]
            Pipeline["/analytics/pipeline"]
            Agents["/analytics/agents"]
            Revenue["/analytics/revenue"]
        end

        subgraph AdminSection["ADMIN"]
            AdminDash["/admin"]
            Users["/admin/users"]
            Roles["/admin/roles"]
            AuditLogs["/admin/audit-logs"]
            SysSettings["/admin/settings"]
        end
    end

    subgraph InvestorSection["INVESTOR PORTAL"]
        InvDash["/investor"]
        InvDeals["/investor/deals"]
        Portfolio["/investor/portfolio"]
        Funding["/investor/funding"]
    end

    Public --> Dashboard
    Public --> InvestorSection
```

## Data Flow

```mermaid
sequenceDiagram
    participant A as Agent
    participant S as System
    participant U as Underwriter
    participant I as Investor
    participant E as External APIs

    A->>S: Submit Deal + Photos
    S->>E: Fetch Property Data
    S->>U: Notify New Deal
    U->>S: Run Underwriting
    U->>S: Generate PDF Offer
    S->>E: Send via DocuSign
    S->>A: Notify Offer Sent
    S->>I: Notify Investment Opportunity
    I->>S: Request Funding
    S->>U: Notify Funding Request
    U->>S: Approve Funding
    S->>I: Notify Approved
    S->>A: Notify Deal Closed
    S->>I: Update Portfolio
```

## Notification System

```mermaid
flowchart LR
    subgraph Triggers["TRIGGERS"]
        T1["Deal Status Change"]
        T2["New Comment"]
        T3["Assignment"]
        T4["Funding Request"]
        T5["Document Upload"]
    end

    subgraph System["NOTIFICATION SERVICE"]
        NS["Process Event"]
        Prefs["Check Preferences"]
    end

    subgraph Channels["CHANNELS"]
        InApp["In-App"]
        Email["Email"]
        SMS["SMS"]
        Push["Push"]
    end

    Triggers --> System
    System --> Channels
```

## Comparison: Current vs Full

```mermaid
flowchart LR
    subgraph Current["CURRENT 15%"]
        direction TB
        CC1["Auth 3 roles"]
        CC2["Deal CRUD"]
        CC3["Kanban"]
        CC4["Underwriting"]
        CC5["Comments"]
        CC6["Basic Admin"]
    end

    subgraph Full["FULL 100%"]
        direction TB
        FC1["Auth 4 roles"]
        FC2["Deal CRUD++"]
        FC3["Kanban++"]
        FC4["Underwriting++"]
        FC5["Comments++"]
        FC6["Full Admin"]
        FC7["Investor Portal"]
        FC8["Analytics"]
        FC9["PDF Generator"]
        FC10["Notifications"]
        FC11["Audit Logs"]
        FC12["6 Integrations"]
    end

    Current -->|Expand| Full
```

## Development Timeline

```mermaid
gantt
    title Roadmap to Full Platform
    dateFormat YYYY-MM-DD
    section Foundation
    Current Build           :done, 2025-01-01, 2d
    Database Enhancement    :2025-01-03, 7d
    Audit Logging          :2025-01-10, 5d
    section Investor
    Investor Role          :2025-01-15, 7d
    Funding Workflow       :2025-01-22, 7d
    section Documents
    PDF Generator          :2025-01-29, 7d
    DocuSign Integration   :2025-02-05, 7d
    section Analytics
    Dashboard              :2025-02-12, 10d
    Reports                :2025-02-22, 5d
    section Integrations
    PropStream             :2025-02-27, 5d
    Email SMS              :2025-03-04, 7d
    section Polish
    Testing                :2025-03-11, 7d
    Launch                 :milestone, 2025-03-18, 0d
```

## Feature Completion

```mermaid
pie title Feature Completion
    "Built 15%" : 15
    "To Build 85%" : 85
```
