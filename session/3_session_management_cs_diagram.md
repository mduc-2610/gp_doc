# Class Diagram - Quản lý các Session

```mermaid
classDiagram
    class User {
        - id: UUID
        - username: String
        - email: String
        - phone: String
        - firstName: String
        - lastName: String
        - userCode: String
        - userPrefix: String
        - isPremium: Boolean
        + getId(): UUID
        + getRoles(): Set~Role~
    }

    class Role {
        - name: RoleType
        + getName(): RoleType
    }

    class RoleType {
        <<enumeration>>
        STUDENT
        LECTURER
        ADMIN
    }

    class Session {
        - id: UUID
        - name: String
        - description: String | null
        - created_at: DateTime
        - updated_at: DateTime
        - created_by: UUID | null
        - is_active: Boolean
        - total_documents: Integer
        - auto_grant_invite: Boolean
        + getId(): UUID
        + getName(): String
        + getDescription(): String | null
        + getTotalDocuments(): Integer
    }

    class Document {
        - id: UUID
        - session_id: UUID
        - name: String
        - file_path: String
        + getSessionId(): UUID
        + getName(): String
    }

    class SessionAccessGrant {
        - id: UUID
        - session_id: UUID
        - actor_id: UUID
        - actor_type: ActorType
        - access_level: AccessLevel
        - is_active: Boolean
        - expires_at: DateTime | null
        + getSessionId(): UUID
        + getActorId(): UUID
    }

    class DocAgentService {
        + createSession(): ServiceResult~SessionResponse~
        + getSessions(): ServiceResult~Session[]~
        + getSessionDetail(): ServiceResult~Session~
        + updateSession(): ServiceResult~void~
        + deleteSession(): ServiceResult~void~
        - parseErrorResponse(): MessageResponse
        - getAuthHeaders(): Record~String, String~
    }

    class SessionService {
        - document_service: DocumentService
        + get_sessions(): Session[]
        + create_session(): Session
        + get_session(): Session
        + get_sessions_by_user(): Session[]
        + update_session(): Session
        + delete_session(): void
    }

    class MessageResponse {
        - translation_key: String
        - message: String
        - details: Dict~String, Any~ | null
        + create(translation_key, message, details): MessageResponse
    }

    class ServiceResult~T~ {
        - type: ResponseStatus
        - data: T | null
        - error: MessageResponse | null
    }

    class ResponseStatus {
        SUCCESS
        ERROR
    }

    class AccessLevel {
        MANAGER
        READER
        CONTRIBUTOR
    }

    class ActorType {
        USER
        SYSTEM
    }

    %% Relationships
    User "*" -- "*" Role : has
    Role "*" -- "1" RoleType : has_type
    User "1" -- "*" Session : creates
    User "1" -- "*" SessionAccessGrant : has
    Session "1" -- "*" Document : contains
    Session "1" -- "*" SessionAccessGrant : grants
    SessionAccessGrant "*" -- "1" AccessLevel : has
    SessionAccessGrant "*" -- "1" ActorType : has

    DocAgentService --> Session : manages
    DocAgentService --> ServiceResult : returns
    DocAgentService --> MessageResponse : uses

    SessionService --> Session : manages
    SessionService --> MessageResponse : uses

    ServiceResult --> ResponseStatus : has_status
    ServiceResult --> MessageResponse : has_error
```

**Mô tả quan hệ:**
- **User**: Đại diện cho User của hệ thống, có thể có nhiều Role (STUDENT, LECTURER, ADMIN). User có thể tạo và quản lý các Session.
- **Role**: Đại diện cho vai trò của User trong hệ thống, được định danh bởi RoleType enum.
- **RoleType**: Enum định nghĩa các loại role: STUDENT, LECTURER, ADMIN.
- **Session**: Đại diện cho một Session, chứa thông tin cơ bản như tên, mô tả, ngày tạo. Được sử dụng để đóng gói dữ liệu từ cơ sở dữ liệu.
- **Document**: Các tài liệu thuộc về một Session.
- **SessionAccessGrant**: Quản lý quyền truy cập vào Session cho các User khác.
- **DocAgentService** (Frontend): Lớp service Frontend xử lý các API call liên quan đến session.
- **SessionService** (Backend): Lớp service Backend xử lý logic nghiệp vụ của session, truy vấn cơ sở dữ liệu và đóng gói dữ liệu thành Model.
- **MessageResponse** & **ServiceResult**: Các lớp dùng để trả về kết quả API.
