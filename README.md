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
