```mermaid
erDiagram
    USER {
        int user_id PK
        string personnel_code UK
        string full_name
        string password_hash
        string role
        int unit_id FK "Nullable (NFR-MAINT-01)"
    }

    ORGANIZATIONAL_UNIT {
        int unit_id PK
        string unit_name UK
    }

    ISSUE {
        int issue_id PK
        int submitter_id FK
        int unit_id FK
        string title
        text description
        int priority_level
        timestamp submitted_at
    }

    USER ||--o{ ISSUE : "submits"
    ORGANIZATIONAL_UNIT ||--o{ ISSUE : "categorizes"
    ORGANIZATIONAL_UNIT }o--o{ USER : "managed_by (Phase 2 - skip for MVP)"
