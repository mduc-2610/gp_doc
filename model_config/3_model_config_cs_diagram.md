# Class Diagram - Cấu hình Model

```mermaid
classDiagram
    class User {
        - id: UUID
        - username: String
        - email: String
        - phone: String
        - birthDay: LocalDate
        - firstName: String
        - lastName: String
        - avatar: String
        - roles: Set~Role~
        - userCode: String
        - userPrefix: String
        - isPremium: Boolean
        - providers: List~UserIdentity~
        + getId(): UUID
        + isAdmin(): Boolean
        + hasRole(roleName): Boolean
    }

    class Role {
        - name: RoleType
        - users: List~User~
        + getName(): RoleType
    }

    class RoleType {
        <<enumeration>>
        STUDENT
        LECTURER
        ADMIN
    }

    class ModelConfig {
        - id: UUID
        - user_id: UUID
        - name: String
        - wrapper_id: UUID
        - model_type: ModelType
        - api_key: String | null (encrypted)
        - extra_config: Dict~String, Any~
        - is_used: Boolean
        - lang: Lang
        - created_at: DateTime
        - updated_at: DateTime
        + getId(): UUID
        + getName(): String
        + getWrapper(): ModelWrapper
        + getApiKey(): String | null (masked)
    }

    class ModelWrapper {
        - id: UUID
        - name: String
        - task_type: TaskType
        - instruction: String | null
        - is_active: Boolean
        - created_at: DateTime
        - updated_at: DateTime
        + getId(): UUID
        + getName(): String
        + getTaskType(): TaskType
        + getInstruction(): String | null
    }

    class ModelUsage {
        - id: UUID
        - model_id: UUID
        - entity_type: EntityType
        - entity_id: UUID
        - created_at: DateTime
        + getModelId(): UUID
    }

    class ModelConfigService {
        + getModels(): ServiceResult~ModelConfig[]~
        + createModel(): ServiceResult~ModelConfig~
        + updateModel(): ServiceResult~ModelConfig~
        + deleteModel(): ServiceResult~void~
        + preloadModels(): ServiceResult~ModelConfig[]~
        - parseErrorResponse(): MessageResponse
        - getAuthHeaders(): Record~String, String~
    }

    class ModelConfigBackendService {
        - db_session: Session
        + get_models_by_user(): ModelConfig[]
        + create_model(): ModelConfig
        + update_model(): ModelConfig
        + delete_model(): void
        + get_wrappers(): ModelWrapper[]
        + create_wrapper(): ModelWrapper
        + update_wrapper(): ModelWrapper
        + delete_wrapper(): void
        + preload_models_for_user(): ModelConfig[]
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

    class ModelType {
        LOCAL
        REMOTE
    }

    class TaskType {
        GENERATION
        EMBEDDING
        PARSE_DOC
        PARSE_AUDIO
        PARSE_IMAGE
    }

    class Lang {
        EN
        VI
    }

    class EntityType {
        SESSION
        QUESTION
        OTHER
    }

    %% Relationships
    User "*" -- "*" Role : has
    Role "*" -- "1" RoleType : defines
    
    User "1" -- "*" ModelConfig : creates
    User "1" -- "*" ModelConfigService : uses
    User "1" -- "*" ModelWrapper : manages (if ADMIN role)
    
    ModelConfig "*" -- "1" ModelWrapper : uses
    ModelConfig "*" -- "*" ModelUsage : tracked_by
    
    ModelWrapper "1" -- "*" ModelConfig : has
    ModelWrapper "*" -- "1" TaskType : defines
    
    ModelUsage "*" -- "1" ModelConfig : references
    ModelUsage "*" -- "1" EntityType : tracks

    ModelConfigService --> ModelConfig : manages
    ModelConfigService --> ModelWrapper : manages
    ModelConfigService --> ServiceResult : returns
    ModelConfigService --> MessageResponse : uses

    ModelConfigBackendService --> ModelConfig : manages_db
    ModelConfigBackendService --> ModelWrapper : manages_db
    ModelConfigBackendService --> MessageResponse : uses

    ServiceResult --> ResponseStatus : has_status
    ServiceResult --> MessageResponse : has_error
    
    ModelConfig "*" -- "1" ModelType : has
    ModelConfig "*" -- "1" Lang : has
```

**Mô tả quan hệ:**
- **User**: Đại diện cho User của hệ thống. Mọi User đều có thể tạo và quản lý các Model Config của mình, xem danh sách Model Wrapper. User có nhiều Role (STUDENT, LECTURER, ADMIN).
- **Role**: Đại diện cho vai trò của User trong hệ thống, được định nghĩa bởi enum RoleType.
- **RoleType**: Enum định nghĩa các loại vai trò: STUDENT, LECTURER, ADMIN. User có role ADMIN sẽ có quyền quản lý (tạo, sửa, xóa) Model Wrapper.
- **ModelConfig**: Đại diện cho một cấu hình Model của User, chứa thông tin như tên, wrapper, model type, API key (mã hóa), extra config. Được sử dụng để đóng gói dữ liệu từ cơ sở dữ liệu.
- **ModelWrapper**: Đại diện cho một định nghĩa cách tương tác với một mô hình AI cụ thể, chứa tên, task type, và hướng dẫn sử dụng.
- **ModelUsage**: Theo dõi việc sử dụng Model cho các entity khác nhau trong hệ thống.
- **ModelConfigService** (Frontend): Lớp service Frontend xử lý các API call liên quan đến Model Config.
- **ModelConfigBackendService** (Backend): Lớp service Backend xử lý logic nghiệp vụ của Model Config, truy vấn cơ sở dữ liệu và đóng gói dữ liệu thành Model.
- **MessageResponse** & **ServiceResult**: Các lớp dùng để trả về kết quả API.
- **ModelType**: Enum định nghĩa kiểu Model (LOCAL hoặc REMOTE).
- **TaskType**: Enum định nghĩa loại tác vụ mà Wrapper hỗ trợ.
- **Lang**: Enum định nghĩa ngôn ngữ của Model.
- **EntityType**: Enum định nghĩa loại entity sử dụng Model.
