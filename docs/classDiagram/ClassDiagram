# Class Diagram — بازتاب (Baztab)

نمودار کلاس فاز اول (MVP) بر اساس SRS، ERD و نمودارهای فعالیت.

```mermaid
classDiagram
    direction TB

    %% ===== Entities =====
    class User {
        +int userId
        +string personnelCode
        +string fullName
        -string passwordHash
        +Role role
        +int unitId
        +isEmployee() bool
        +isGeneralManager() bool
    }

    class Role {
        <<enumeration>>
        EMPLOYEE
        GENERAL_MANAGER
    }

    class OrganizationalUnit {
        +int unitId
        +string unitName
    }

    class Issue {
        +int issueId
        +int submitterId
        +int unitId
        +string title
        +string description
        +int priorityLevel
        +datetime submittedAt
    }

    class AuthToken {
        <<transient>>
        +string token
        +int userId
        +Role role
        +isValid() bool
    }

    %% ===== Services =====
    class AuthService {
        +login(personnelCode, password) AuthToken
        +logout(token) void
        +validateToken(token) User
    }

    class IssueService {
        +submitIssue(user, title, description, unitId, priorityLevel) Issue
        +getMyIssues(user, unitId) List~Issue~
        +getAllIssues(manager, unitId) List~Issue~
        +getIssueDetails(user, issueId) Issue
    }

    class AccessPolicy {
        +canViewIssue(user, issue) bool
        +canViewAllIssues(user) bool
    }

    %% ===== Relations =====
    User --> Role : has
    User "1" --> "0..*" Issue : submits
    OrganizationalUnit "1" --> "0..*" Issue : categorizes
    OrganizationalUnit "0..1" <-- "0..*" User : unitId (Phase 2)

    AuthService ..> User : authenticates
    AuthService ..> AuthToken : creates / revokes
    IssueService ..> Issue : creates / queries
    IssueService ..> OrganizationalUnit : validates / filters
    IssueService ..> AccessPolicy : checks access
    AccessPolicy ..> User
    AccessPolicy ..> Issue

    note for User "unitId nullable است (NFR-MAINT-01). در MVP خالی است"
    note for Issue "priorityLevel بین 1 تا 5. فیلد وضعیت در فاز اول وجود ندارد"
    note for OrganizationalUnit "5 مقدار ثابت: HR، مالی، IT، پشتیبانی، اداری"
```

## ردیابی به اپیک‌ها

| اپیک | کلاس / متد |
|---|---|
| EPIC-AUTH | `AuthService.login / logout / validateToken`، `AuthToken`، `Role` |
| EPIC-ISSUE | `IssueService.submitIssue`، `Issue`، `OrganizationalUnit` |
| EPIC-MY-ISSUES | `IssueService.getMyIssues`، `IssueService.getIssueDetails` |
| EPIC-GM-DASH | `IssueService.getAllIssues` |
| FR-PRIV-01 | `AccessPolicy.canViewIssue / canViewAllIssues` |
| NFR-MAINT-01 | `User.unitId` (nullable) |

## تصمیم‌های طراحی

- `Role` به‌صورت enum است تا افزودن «مدیر واحد» در فاز ۲ ساده باشد.
- `AccessPolicy` قاعده‌ی محرمانگی (FR-PRIV-01) را در یک نقطه متمرکز می‌کند.
- `AuthToken` ذخیره نمی‌شود، چون ERD جدول نشست ندارد.
- فیلد وضعیت و رابطه‌ی `managed_by` عمداً خارج از MVP هستند.
