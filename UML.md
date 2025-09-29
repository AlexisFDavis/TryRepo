# Diagrama UML Actualizado - Sistema de Gestión de Usuarios

## Diagrama de Clases Principal

```mermaid
classDiagram
    %% Controladores
    class AuthController {
        -AuthServiceImpl authServiceImpl
        +loginInternal(LoginRequest) ResponseEntity~LoginResponse~
        +loginExternal(LoginRequest) ResponseEntity~LoginResponse~
        -mapToUserResponse(UserModel) UserResponse
    }
    
    class UserController {
        -UserService userService
        +registerExternal(ExternalUserRegisterRequestDTO) ResponseEntity~ExternalUserRegisterResponseDTO~
        +registerInternal(InternalUserRegisterRequestDTO) ResponseEntity~InternalUserRegisterResponseDTO~
        +getAll() ResponseEntity~List~UserDebugModel~~
    }
    
    %% Servicios
    class AuthServiceImpl {
        -AuthenticationManager authenticationManager
        -JwtUtil jwtUtil
        -UserServiceImpl usuarioService
        +loginInternal(String, String) LoginResult
        +loginExternal(String, String) LoginResult
        -mapToModel(User) UserModel
    }
    
    class UserServiceImpl {
        -UserRepository userRepository
        -ModelMapper modelMapper
        -RoleService roleService
        -PasswordEncoder passwordEncoder
        -UserRoleService userRoleService
        +findById(Long) UserModel
        +findByUsername(String) UserModel
        +findByEmail(String) UserModel
        +findAll() List~UserModel~
        +loadUserByUsername(String) CustomUserDetails
        +registerExternal(ExternalUserRegisterRequestDTO) ExternalUserRegisterResponseDTO
        +registerInternal(InternalUserRegisterRequestDTO) InternalUserRegisterResponseDTO
        +getAll() List~UserDebugModel~
        -buildExternalUser(ExternalUserRegisterRequestDTO, String, String) User
        -buildInternalUser(InternalUserRegisterRequestDTO, String, String, String) User
        -validationDocument(String) void
        -validationEmail(String) void
        -mapToModel(User) UserModel
    }
    
    %% Entidades
    class User {
        -Long id
        -String firstName
        -String lastName
        -String username
        -String email
        -String document
        -String passwordHash
        -Boolean isActive
        -Boolean isEmailVerified
        -Boolean isExternal
        -LocalDateTime createdAt
        -LocalDateTime updatedAt
        -Set~UserRole~ userRoles
    }
    
    class Role {
        -Long id
        -String name
        -String description
        -Set~UserRole~ userRoles
    }
    
    class UserRole {
        -UserRoleId id
        -User user
        -Role role
    }
    
    class UserRoleId {
        -Long userId
        -Long roleId
    }
    
    class EmailVerification {
        -Long id
        -User user
        -String token
        -LocalDateTime expiresAt
        -Boolean isUsed
    }
    
    class RecoveryToken {
        -Long id
        -User user
        -String token
        -LocalDateTime expiresAt
        -Boolean isUsed
    }
    
    %% DTOs de Request
    class LoginRequest {
        -String username
        -String email
        -String password
    }
    
    class ExternalUserRegisterRequestDTO {
        -String firstName
        -String lastName
        -String email
        -String document
        -String passwordHash
    }
    
    class InternalUserRegisterRequestDTO {
        -String firstName
        -String lastName
        -String document
        -String email
        -String username
        -String passwordHash
        -List~Long~ role
    }
    
    %% DTOs de Response
    class LoginResponse {
        -Boolean success
        -String message
        -UserResponse user
        -String token
    }
    
    class UserResponse {
        -Long id
        -String firstName
        -String lastName
        -String username
        -String email
        -String document
        -Boolean isActive
        -Boolean isEmailVerified
        -Boolean isExternal
        -LocalDateTime createdAt
        -LocalDateTime updatedAt
    }
    
    class ExternalUserRegisterResponseDTO {
        -Long id
        -String email
        -String message
    }
    
    class InternalUserRegisterResponseDTO {
        -Long id
        -String firstName
        -String lastName
        -String document
        -String email
        -String userName
        -Boolean isActive
        -String message
    }
    
    class UserDebugModel {
        -Long id
        -String firstName
        -String lastName
        -String username
        -String email
        -String document
        -String passwordHash
        -Boolean isActive
        -Boolean isExternal
        -LocalDateTime createdAt
        -List~String~ roles
        +fromEntity(User) UserDebugModel
    }
    
    %% Modelos
    class UserModel {
        -Long id
        -String firstName
        -String lastName
        -String username
        -String email
        -String document
        -Boolean isActive
        -Boolean isEmailVerified
        -Boolean isExternal
        -LocalDateTime createdAt
        -LocalDateTime updatedAt
    }
    
    class LoginResult {
        -UserModel user
        -String token
    }
    
    %% Seguridad
    class CustomUserDetails {
        -Long id
        -String username
        -String password
        -Collection authorities
    }
    
    class JwtUtil {
        -String secret
        -int expiration
        +generateToken(CustomUserDetails) String
        +extractUsername(String) String
        +validateToken(String, CustomUserDetails) Boolean
    }
    
    class JwtRequestFilter {
        -JwtUtil jwtUtil
        -UserServiceImpl userService
        +doFilterInternal(ServletRequest, ServletResponse, FilterChain) void
    }
    
    %% Repositorios
    class UserRepository {
        +findById(Long) Optional~User~
        +findByUsername(String) Optional~User~
        +findByEmail(String) Optional~User~
        +findByDocument(String) Optional~User~
        +existsByEmail(String) Boolean
        +save(User) User
        +findAll() List~User~
    }
    
    class RoleRepository {
        +findById(Long) Optional~Role~
        +findAllById(List~Long~) List~Role~
        +save(Role) Role
        +findAll() List~Role~
    }
    
    class UserRoleRepository {
        +save(UserRole) UserRole
        +saveAll(List~UserRole~) List~UserRole~
        +findByUser(User) List~UserRole~
        +findByRole(Role) List~UserRole~
    }
    
    %% Servicios de Negocio
    class RoleService {
        +findById(Long) Role
        +findAllById(List~Long~) List~Role~
        +save(Role) Role
        +findAll() List~Role~
    }
    
    class UserRoleService {
        +save(UserRole) UserRole
        +saveAll(List~UserRole~) List~UserRole~
        +findByUser(User) List~UserRole~
        +findByRole(Role) List~UserRole~
    }
    
    %% Interfaces
    class UserService {
        +registerExternal(ExternalUserRegisterRequestDTO) ExternalUserRegisterResponseDTO
        +registerInternal(InternalUserRegisterRequestDTO) InternalUserRegisterResponseDTO
        +getAll() List~UserDebugModel~
    }
    
    %% Relaciones principales
    AuthController --> AuthServiceImpl
    UserController --> UserService
    UserService <|-- UserServiceImpl
    AuthServiceImpl --> UserServiceImpl
    AuthServiceImpl --> JwtUtil
    UserServiceImpl --> UserRepository
    UserServiceImpl --> RoleService
    UserServiceImpl --> UserRoleService
    
    User --> UserRole
    Role --> UserRole
    User --> EmailVerification
    User --> RecoveryToken
    
    UserRepository --> User
    RoleRepository --> Role
    UserRoleRepository --> UserRole
    
    RoleService --> RoleRepository
    UserRoleService --> UserRoleRepository
    
    JwtRequestFilter --> JwtUtil
    JwtRequestFilter --> UserServiceImpl
    
    %% Conversiones
    UserModel <-- User
    UserResponse <-- UserModel
    LoginResult --> UserModel
    LoginResponse --> UserResponse
    UserDebugModel <-- User
```

## Diagrama de Secuencia - Proceso de Login Interno

```mermaid
sequenceDiagram
    participant Client as Cliente
    participant AuthController as AuthController
    participant AuthService as AuthServiceImpl
    participant UserService as UserServiceImpl
    participant UserRepo as UserRepository
    participant JwtUtil as JwtUtil
    participant AuthManager as AuthenticationManager
    
    Client->>AuthController: POST /api/v1/auth/internal/login
    Note over Client,AuthController: LoginRequest{username, password}
    
    AuthController->>AuthService: loginInternal(username, password)
    
    AuthService->>AuthManager: authenticate(UsernamePasswordAuthenticationToken)
    AuthManager-->>AuthService: Authentication result
    
    AuthService->>UserService: loadUserByUsername(username)
    UserService->>UserRepo: findByUsername(username)
    UserRepo-->>UserService: User entity
    UserService-->>AuthService: CustomUserDetails
    
    AuthService->>JwtUtil: generateToken(userDetails)
    JwtUtil-->>AuthService: JWT Token
    
    AuthService->>UserService: findByUsername(username)
    UserService->>UserRepo: findByUsername(username)
    UserRepo-->>UserService: User entity
    UserService-->>AuthService: UserModel
    
    AuthService->>AuthService: Validar isActive y isExternal
    Note over AuthService: Verificar que usuario esté activo<br/>y sea interno
    
    AuthService-->>AuthController: LoginResult{user, token}
    
    AuthController->>AuthController: mapToUserResponse(user)
    AuthController-->>Client: LoginResponse{success, message, user, token}
    Note over Client,AuthController: 200 OK con datos del usuario y token
```

## Diagrama de Secuencia - Proceso de Registro Externo

```mermaid
sequenceDiagram
    participant Client as Cliente
    participant UserController as UserController
    participant UserService as UserServiceImpl
    participant UserRepo as UserRepository
    participant PasswordEncoder as PasswordEncoder
    
    Client->>UserController: POST /api/v1/user/external/register
    Note over Client,UserController: ExternalUserRegisterRequestDTO
    
    UserController->>UserService: registerExternal(request)
    
    UserService->>UserService: validationEmail(email)
    UserService->>UserRepo: existsByEmail(email)
    UserRepo-->>UserService: Boolean
    
    UserService->>UserService: validationDocument(document)
    UserService->>UserRepo: findByDocument(document)
    UserRepo-->>UserService: Optional<User>
    
    UserService->>UserService: buildExternalUser(request)
    Note over UserService: Crear entidad User con:<br/>- isActive = false<br/>- isExternal = true<br/>- username = document
    
    UserService->>PasswordEncoder: encode(passwordHash)
    PasswordEncoder-->>UserService: Encoded password
    
    UserService->>UserRepo: save(user)
    UserRepo-->>UserService: Saved User
    
    UserService-->>UserController: ExternalUserRegisterResponseDTO
    UserController-->>Client: 201 Created
    Note over Client,UserController: {id, email, message}
```

## Diagrama de Secuencia - Proceso de Registro Interno

```mermaid
sequenceDiagram
    participant Client as Cliente
    participant UserController as UserController
    participant UserService as UserServiceImpl
    participant UserRepo as UserRepository
    participant RoleService as RoleService
    participant UserRoleService as UserRoleService
    participant PasswordEncoder as PasswordEncoder
    
    Client->>UserController: POST /api/v1/user/internal/register
    Note over Client,UserController: InternalUserRegisterRequestDTO
    
    UserController->>UserService: registerInternal(request)
    
    UserService->>UserService: validationEmail(email)
    UserService->>UserRepo: existsByEmail(email)
    UserRepo-->>UserService: Boolean
    
    UserService->>UserService: validationDocument(document)
    UserService->>UserRepo: findByDocument(document)
    UserRepo-->>UserService: Optional<User>
    
    UserService->>UserRepo: findByUsername(username)
    UserRepo-->>UserService: Optional<User>
    
    UserService->>RoleService: findAllById(roleIds)
    RoleService-->>UserService: List<Role>
    
    UserService->>UserService: buildInternalUser(request)
    Note over UserService: Crear entidad User con:<br/>- isActive = true<br/>- isExternal = false<br/>- username personalizado
    
    UserService->>PasswordEncoder: encode(passwordHash)
    PasswordEncoder-->>UserService: Encoded password
    
    UserService->>UserRepo: save(user)
    UserRepo-->>UserService: Saved User
    
    UserService->>UserService: Crear UserRole entities
    UserService->>UserRoleService: saveAll(userRoles)
    UserRoleService-->>UserService: List<UserRole>
    
    UserService-->>UserController: InternalUserRegisterResponseDTO
    UserController-->>Client: 201 Created
    Note over Client,UserController: {id, firstName, lastName, document, email, userName, isActive, message}
```

## Diagrama de Secuencia - Proceso de Obtener Todos los Usuarios

```mermaid
sequenceDiagram
    participant Client as Cliente
    participant UserController as UserController
    participant UserService as UserServiceImpl
    participant UserRepo as UserRepository
    participant ModelMapper as ModelMapper
    
    Client->>UserController: GET /api/v1/user/
    
    UserController->>UserService: getAll()
    
    UserService->>UserRepo: findAll()
    UserRepo-->>UserService: List<User>
    
    UserService->>ModelMapper: map(user, UserDebugModel.class)
    ModelMapper-->>UserService: UserDebugModel
    
    UserService-->>UserController: List<UserDebugModel>
    UserController-->>Client: 200 OK
    Note over Client,UserController: Lista de usuarios con información de debug
```

## Arquitectura del Sistema

```mermaid
graph TB
    subgraph "Capa de Presentación"
        AC[AuthController]
        UC[UserController]
    end
    
    subgraph "Capa de Servicios"
        AS[AuthServiceImpl]
        US[UserServiceImpl]
        RS[RoleService]
        URS[UserRoleService]
    end
    
    subgraph "Capa de Datos"
        UR[UserRepository]
        RR[RoleRepository]
        URR[UserRoleRepository]
    end
    
    subgraph "Capa de Seguridad"
        JU[JwtUtil]
        CUD[CustomUserDetails]
        JRF[JwtRequestFilter]
        AM[AuthenticationManager]
    end
    
    subgraph "Capa de Utilidades"
        PE[PasswordEncoder]
        MM[ModelMapper]
    end
    
    subgraph "Base de Datos"
        DB[(Database)]
    end
    
    AC --> AS
    UC --> US
    AS --> US
    AS --> JU
    AS --> AM
    US --> UR
    US --> RS
    US --> URS
    US --> PE
    US --> MM
    RS --> RR
    URS --> URR
    UR --> DB
    RR --> DB
    URR --> DB
    JU --> CUD
    JRF --> JU
    JRF --> US
```

## Resumen de la Arquitectura Actualizada

### **Capas del Sistema:**

1. **Capa de Presentación (Controllers)**:
   - `AuthController`: Maneja autenticación (login interno/externo)
   - `UserController`: Maneja registro de usuarios y listado

2. **Capa de Servicios**:
   - `AuthServiceImpl`: Lógica de autenticación y generación de tokens JWT
   - `UserServiceImpl`: Lógica de negocio para gestión de usuarios
   - `RoleService`: Gestión de roles
   - `UserRoleService`: Gestión de relaciones usuario-rol

3. **Capa de Datos**:
   - `UserRepository`: Acceso a datos de usuarios
   - `RoleRepository`: Acceso a datos de roles
   - `UserRoleRepository`: Acceso a relaciones

4. **Capa de Seguridad**:
   - `JwtUtil`: Manejo de tokens JWT
   - `CustomUserDetails`: Detalles del usuario para Spring Security
   - `JwtRequestFilter`: Filtro para validación de tokens JWT
   - `AuthenticationManager`: Gestión de autenticación

5. **Capa de Utilidades**:
   - `PasswordEncoder`: Encriptación de contraseñas
   - `ModelMapper`: Mapeo entre entidades y DTOs

### **Flujos Principales:**

1. **Autenticación**: Cliente → Controller → Service → AuthenticationManager → Repository → Database
2. **Registro**: Cliente → Controller → Service → Repository → Database
3. **Validación**: Service → Repository → Database
4. **Seguridad**: JWT Token → Filter → Service → Database

