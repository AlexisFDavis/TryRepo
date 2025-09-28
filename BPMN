# Diagrama BPMN - Sistema de Gestión de Usuarios

## Proceso Principal de Autenticación y Registro

```mermaid
flowchart TD
    Start([Inicio]) --> Decision{¿Qué operación desea realizar?}
    
    %% Proceso de Login
    Decision -->|Login| LoginProcess[Proceso de Login]
    LoginProcess --> LoginType{¿Tipo de usuario?}
    LoginType -->|Interno| InternalLogin[Login Interno]
    LoginType -->|Externo| ExternalLogin[Login Externo]
    
    InternalLogin --> ValidateInternal[Validar credenciales internas]
    ValidateInternal --> CheckActiveInternal{¿Usuario activo?}
    CheckActiveInternal -->|No| ErrorInactive[Error: Usuario inactivo]
    CheckActiveInternal -->|Sí| CheckTypeInternal{¿Es usuario interno?}
    CheckTypeInternal -->|No| ErrorTypeInternal[Error: Usuario no es interno]
    CheckTypeInternal -->|Sí| GenerateTokenInternal[Generar token JWT]
    
    ExternalLogin --> ValidateExternal[Validar credenciales externas]
    ValidateExternal --> CheckActiveExternal{¿Usuario activo?}
    CheckActiveExternal -->|No| ErrorInactive
    CheckActiveExternal -->|Sí| CheckTypeExternal{¿Es usuario externo?}
    CheckTypeExternal -->|No| ErrorTypeExternal[Error: Usuario no es externo]
    CheckTypeExternal -->|Sí| GenerateTokenExternal[Generar token JWT]
    
    GenerateTokenInternal --> SuccessLogin[Login exitoso]
    GenerateTokenExternal --> SuccessLogin
    SuccessLogin --> EndLogin([Fin - Token generado])
    
    %% Proceso de Registro
    Decision -->|Registro| RegisterProcess[Proceso de Registro]
    RegisterProcess --> RegisterType{¿Tipo de registro?}
    RegisterType -->|Externo| ExternalRegister[Registro Externo]
    RegisterType -->|Interno| InternalRegister[Registro Interno]
    
    %% Registro Externo
    ExternalRegister --> ValidateEmailExt[Validar email único]
    ValidateEmailExt --> EmailExistsExt{¿Email existe?}
    EmailExistsExt -->|Sí| ErrorEmailExists[Error: Email ya existe]
    EmailExistsExt -->|No| ValidateDocExt[Validar documento único]
    ValidateDocExt --> DocExistsExt{¿Documento existe?}
    DocExistsExt -->|Sí| ErrorDocExists[Error: Documento ya existe]
    DocExistsExt -->|No| ValidateFormatExt[Validar formato de datos]
    ValidateFormatExt --> FormatValidExt{¿Formato válido?}
    FormatValidExt -->|No| ErrorFormat[Error: Formato inválido]
    FormatValidExt -->|Sí| CreateUserExt[Crear usuario externo]
    CreateUserExt --> SetExternalProps[Establecer propiedades externas]
    SetExternalProps --> HashPasswordExt[Encriptar contraseña]
    HashPasswordExt --> SaveUserExt[Guardar usuario]
    SaveUserExt --> SuccessExt[Registro externo exitoso]
    SuccessExt --> EndExt([Fin - Usuario externo creado])
    
    %% Registro Interno
    InternalRegister --> ValidateEmailInt[Validar email único]
    ValidateEmailInt --> EmailExistsInt{¿Email existe?}
    EmailExistsInt -->|Sí| ErrorEmailExists
    EmailExistsInt -->|No| ValidateDocInt[Validar documento único]
    ValidateDocInt --> DocExistsInt{¿Documento existe?}
    DocExistsInt -->|Sí| ErrorDocExists
    DocExistsInt -->|No| ValidateUsernameInt[Validar username único]
    ValidateUsernameInt --> UsernameExists{¿Username existe?}
    UsernameExists -->|Sí| ErrorUsernameExists[Error: Username ya existe]
    UsernameExists -->|No| ValidateRoles[Validar roles existentes]
    ValidateRoles --> RolesValid{¿Roles válidos?}
    RolesValid -->|No| ErrorRoles[Error: Roles inválidos]
    RolesValid -->|Sí| ValidateFormatInt[Validar formato de datos]
    ValidateFormatInt --> FormatValidInt{¿Formato válido?}
    FormatValidInt -->|No| ErrorFormat
    FormatValidInt -->|Sí| CreateUserInt[Crear usuario interno]
    CreateUserInt --> SetInternalProps[Establecer propiedades internas]
    SetInternalProps --> HashPasswordInt[Encriptar contraseña]
    HashPasswordInt --> SaveUserInt[Guardar usuario]
    SaveUserInt --> AssignRoles[Asignar roles]
    AssignRoles --> SaveUserRoles[Guardar relaciones usuario-rol]
    SaveUserRoles --> SuccessInt[Registro interno exitoso]
    SuccessInt --> EndInt([Fin - Usuario interno creado])
    
    %% Errores
    ErrorInactive --> EndError([Fin - Error])
    ErrorTypeInternal --> EndError
    ErrorTypeExternal --> EndError
    ErrorEmailExists --> EndError
    ErrorDocExists --> EndError
    ErrorUsernameExists --> EndError
    ErrorFormat --> EndError
    ErrorRoles --> EndError
    
    %% Estilos mejorados para mejor contraste
    classDef startEnd fill:#2E7D32,stroke:#1B5E20,stroke-width:3px,color:#FFFFFF
    classDef process fill:#1976D2,stroke:#0D47A1,stroke-width:2px,color:#FFFFFF
    classDef decision fill:#F57C00,stroke:#E65100,stroke-width:2px,color:#FFFFFF
    classDef error fill:#D32F2F,stroke:#B71C1C,stroke-width:2px,color:#FFFFFF
    classDef success fill:#388E3C,stroke:#2E7D32,stroke-width:2px,color:#FFFFFF
    
    class Start,EndLogin,EndExt,EndInt,EndError startEnd
    class LoginProcess,RegisterProcess,ValidateInternal,ValidateExternal,CreateUserExt,CreateUserInt process
    class Decision,LoginType,CheckActiveInternal,CheckTypeInternal,CheckActiveExternal,CheckTypeExternal,RegisterType,EmailExistsExt,DocExistsExt,FormatValidExt,EmailExistsInt,DocExistsInt,UsernameExists,RolesValid,FormatValidInt decision
    class ErrorInactive,ErrorTypeInternal,ErrorTypeExternal,ErrorEmailExists,ErrorDocExists,ErrorUsernameExists,ErrorFormat,ErrorRoles error
    class SuccessLogin,SuccessExt,SuccessInt success
```

## Proceso Detallado de Autenticación

```mermaid
flowchart TD
    StartAuth([Inicio Autenticación]) --> ReceiveCredentials[Recibir credenciales]
    ReceiveCredentials --> ValidateFormat[Validar formato de credenciales]
    ValidateFormat --> FormatOK{¿Formato válido?}
    FormatOK -->|No| ReturnFormatError[Retornar error de formato]
    FormatOK -->|Sí| AuthenticateUser[Autenticar usuario]
    AuthenticateUser --> AuthSuccess{¿Autenticación exitosa?}
    AuthSuccess -->|No| ReturnAuthError[Retornar error de autenticación]
    AuthSuccess -->|Sí| LoadUserDetails[Cargar detalles del usuario]
    LoadUserDetails --> CheckUserStatus[Verificar estado del usuario]
    CheckUserStatus --> UserActive{¿Usuario activo?}
    UserActive -->|No| ReturnInactiveError[Retornar error: usuario inactivo]
    UserActive -->|Sí| CheckUserType[Verificar tipo de usuario]
    CheckUserType --> TypeMatch{¿Tipo coincide con endpoint?}
    TypeMatch -->|No| ReturnTypeError[Retornar error: tipo incorrecto]
    TypeMatch -->|Sí| GenerateJWT[Generar token JWT]
    GenerateJWT --> CreateResponse[Crear respuesta de login]
    CreateResponse --> ReturnSuccess[Retornar éxito con token]
    
    ReturnFormatError --> EndAuth([Fin])
    ReturnAuthError --> EndAuth
    ReturnInactiveError --> EndAuth
    ReturnTypeError --> EndAuth
    ReturnSuccess --> EndAuth
    
    classDef startEnd fill:#2E7D32,stroke:#1B5E20,stroke-width:3px,color:#FFFFFF
    classDef process fill:#1976D2,stroke:#0D47A1,stroke-width:2px,color:#FFFFFF
    classDef decision fill:#F57C00,stroke:#E65100,stroke-width:2px,color:#FFFFFF
    classDef error fill:#D32F2F,stroke:#B71C1C,stroke-width:2px,color:#FFFFFF
    classDef success fill:#388E3C,stroke:#2E7D32,stroke-width:2px,color:#FFFFFF
    
    class StartAuth,EndAuth startEnd
    class ReceiveCredentials,ValidateFormat,AuthenticateUser,LoadUserDetails,CheckUserStatus,CheckUserType,GenerateJWT,CreateResponse process
    class FormatOK,AuthSuccess,UserActive,TypeMatch decision
    class ReturnFormatError,ReturnAuthError,ReturnInactiveError,ReturnTypeError error
    class ReturnSuccess success
```

## Proceso Detallado de Registro Externo

```mermaid
flowchart TD
    StartExtReg([Inicio Registro Externo]) --> ReceiveData[Recibir datos de registro]
    ReceiveData --> ValidateFirstName[Validar nombre]
    ValidateFirstName --> FirstNameOK{¿Nombre válido?}
    FirstNameOK -->|No| ReturnNameError[Retornar error de nombre]
    FirstNameOK -->|Sí| ValidateLastName[Validar apellido]
    ValidateLastName --> LastNameOK{¿Apellido válido?}
    LastNameOK -->|No| ReturnLastNameError[Retornar error de apellido]
    LastNameOK -->|Sí| ValidateEmail[Validar email]
    ValidateEmail --> EmailFormatOK{¿Formato de email válido?}
    EmailFormatOK -->|No| ReturnEmailFormatError[Retornar error de formato email]
    EmailFormatOK -->|Sí| CheckEmailUnique[Verificar email único]
    CheckEmailUnique --> EmailUnique{¿Email único?}
    EmailUnique -->|No| ReturnEmailExistsError[Retornar error: email existe]
    EmailUnique -->|Sí| ValidateDocument[Validar documento]
    ValidateDocument --> DocFormatOK{¿Formato de documento válido?}
    DocFormatOK -->|No| ReturnDocFormatError[Retornar error de formato documento]
    DocFormatOK -->|Sí| CheckDocUnique[Verificar documento único]
    CheckDocUnique --> DocUnique{¿Documento único?}
    DocUnique -->|No| ReturnDocExistsError[Retornar error: documento existe]
    DocUnique -->|Sí| ValidatePassword[Validar contraseña]
    ValidatePassword --> PasswordOK{¿Contraseña válida?}
    PasswordOK -->|No| ReturnPasswordError[Retornar error de contraseña]
    PasswordOK -->|Sí| CreateUserEntity[Crear entidad usuario]
    CreateUserEntity --> SetExternalProperties[Establecer propiedades externas]
    SetExternalProperties --> HashPassword[Encriptar contraseña]
    HashPassword --> SaveUser[Guardar usuario en BD]
    SaveUser --> SaveSuccess{¿Guardado exitoso?}
    SaveSuccess -->|No| ReturnSaveError[Retornar error de guardado]
    SaveSuccess -->|Sí| CreateResponse[Crear respuesta]
    CreateResponse --> ReturnSuccess[Retornar éxito]
    
    ReturnNameError --> EndExtReg([Fin])
    ReturnLastNameError --> EndExtReg
    ReturnEmailFormatError --> EndExtReg
    ReturnEmailExistsError --> EndExtReg
    ReturnDocFormatError --> EndExtReg
    ReturnDocExistsError --> EndExtReg
    ReturnPasswordError --> EndExtReg
    ReturnSaveError --> EndExtReg
    ReturnSuccess --> EndExtReg
    
    classDef startEnd fill:#2E7D32,stroke:#1B5E20,stroke-width:3px,color:#FFFFFF
    classDef process fill:#1976D2,stroke:#0D47A1,stroke-width:2px,color:#FFFFFF
    classDef decision fill:#F57C00,stroke:#E65100,stroke-width:2px,color:#FFFFFF
    classDef error fill:#D32F2F,stroke:#B71C1C,stroke-width:2px,color:#FFFFFF
    classDef success fill:#388E3C,stroke:#2E7D32,stroke-width:2px,color:#FFFFFF
    
    class StartExtReg,EndExtReg startEnd
    class ReceiveData,ValidateFirstName,ValidateLastName,ValidateEmail,CheckEmailUnique,ValidateDocument,CheckDocUnique,ValidatePassword,CreateUserEntity,SetExternalProperties,HashPassword,SaveUser,CreateResponse process
    class FirstNameOK,LastNameOK,EmailFormatOK,EmailUnique,DocFormatOK,DocUnique,PasswordOK,SaveSuccess decision
    class ReturnNameError,ReturnLastNameError,ReturnEmailFormatError,ReturnEmailExistsError,ReturnDocFormatError,ReturnDocExistsError,ReturnPasswordError,ReturnSaveError error
    class ReturnSuccess success
```

## Proceso Detallado de Registro Interno

```mermaid
flowchart TD
    StartIntReg([Inicio Registro Interno]) --> ReceiveDataInt[Recibir datos de registro interno]
    ReceiveDataInt --> ValidateDataInt[Validar datos básicos]
    ValidateDataInt --> DataOK{¿Datos válidos?}
    DataOK -->|No| ReturnDataError[Retornar error de datos]
    DataOK -->|Sí| CheckEmailUniqueInt[Verificar email único]
    CheckEmailUniqueInt --> EmailUniqueInt{¿Email único?}
    EmailUniqueInt -->|No| ReturnEmailExistsErrorInt[Retornar error: email existe]
    EmailUniqueInt -->|Sí| CheckDocUniqueInt[Verificar documento único]
    CheckDocUniqueInt --> DocUniqueInt{¿Documento único?}
    DocUniqueInt -->|No| ReturnDocExistsErrorInt[Retornar error: documento existe]
    DocUniqueInt -->|Sí| CheckUsernameUnique[Verificar username único]
    CheckUsernameUnique --> UsernameUnique{¿Username único?}
    UsernameUnique -->|No| ReturnUsernameExistsError[Retornar error: username existe]
    UsernameUnique -->|Sí| ValidateRoles[Validar roles]
    ValidateRoles --> RolesExist{¿Roles existen?}
    RolesExist -->|No| ReturnRolesError[Retornar error: roles inválidos]
    RolesExist -->|Sí| CreateUserEntityInt[Crear entidad usuario interno]
    CreateUserEntityInt --> SetInternalProperties[Establecer propiedades internas]
    SetInternalProperties --> HashPasswordInt[Encriptar contraseña]
    HashPasswordInt --> SaveUserInt[Guardar usuario en BD]
    SaveUserInt --> SaveSuccessInt{¿Guardado exitoso?}
    SaveSuccessInt -->|No| ReturnSaveErrorInt[Retornar error de guardado]
    SaveSuccessInt -->|Sí| CreateUserRoles[Crear relaciones usuario-rol]
    CreateUserRoles --> SaveUserRoles[Guardar relaciones en BD]
    SaveUserRoles --> RolesSaved{¿Roles guardados?}
    RolesSaved -->|No| ReturnRolesSaveError[Retornar error: roles no guardados]
    RolesSaved -->|Sí| CreateResponseInt[Crear respuesta]
    CreateResponseInt --> ReturnSuccessInt[Retornar éxito]
    
    ReturnDataError --> EndIntReg([Fin])
    ReturnEmailExistsErrorInt --> EndIntReg
    ReturnDocExistsErrorInt --> EndIntReg
    ReturnUsernameExistsError --> EndIntReg
    ReturnRolesError --> EndIntReg
    ReturnSaveErrorInt --> EndIntReg
    ReturnRolesSaveError --> EndIntReg
    ReturnSuccessInt --> EndIntReg
    
    classDef startEnd fill:#2E7D32,stroke:#1B5E20,stroke-width:3px,color:#FFFFFF
    classDef process fill:#1976D2,stroke:#0D47A1,stroke-width:2px,color:#FFFFFF
    classDef decision fill:#F57C00,stroke:#E65100,stroke-width:2px,color:#FFFFFF
    classDef error fill:#D32F2F,stroke:#B71C1C,stroke-width:2px,color:#FFFFFF
    classDef success fill:#388E3C,stroke:#2E7D32,stroke-width:2px,color:#FFFFFF
    
    class StartIntReg,EndIntReg startEnd
    class ReceiveDataInt,ValidateDataInt,CheckEmailUniqueInt,CheckDocUniqueInt,CheckUsernameUnique,ValidateRoles,CreateUserEntityInt,SetInternalProperties,HashPasswordInt,SaveUserInt,CreateUserRoles,SaveUserRoles,CreateResponseInt process
    class DataOK,EmailUniqueInt,DocUniqueInt,UsernameUnique,RolesExist,SaveSuccessInt,RolesSaved decision
    class ReturnDataError,ReturnEmailExistsErrorInt,ReturnDocExistsErrorInt,ReturnUsernameExistsError,ReturnRolesError,ReturnSaveErrorInt,ReturnRolesSaveError error
    class ReturnSuccessInt success
```

## Proceso de Validación de Seguridad

```mermaid
flowchart TD
    StartSecurity([Inicio Validación de Seguridad]) --> ReceiveToken[Recibir token JWT]
    ReceiveToken --> ValidateTokenFormat[Validar formato del token]
    ValidateTokenFormat --> TokenFormatOK{¿Formato válido?}
    TokenFormatOK -->|No| ReturnTokenFormatError[Retornar error de formato token]
    TokenFormatOK -->|Sí| ExtractUsername[Extraer username del token]
    ExtractUsername --> LoadUserFromToken[Cargar usuario desde token]
    LoadUserFromToken --> UserExists{¿Usuario existe?}
    UserExists -->|No| ReturnUserNotFound[Retornar error: usuario no encontrado]
    UserExists -->|Sí| ValidateTokenExpiry[Validar expiración del token]
    ValidateTokenExpiry --> TokenValid{¿Token válido?}
    TokenValid -->|No| ReturnTokenExpired[Retornar error: token expirado]
    TokenValid -->|Sí| CheckUserActive[Verificar usuario activo]
    CheckUserActive --> UserActiveSec{¿Usuario activo?}
    UserActiveSec -->|No| ReturnUserInactive[Retornar error: usuario inactivo]
    UserActiveSec -->|Sí| AuthorizeAccess[Autorizar acceso]
    AuthorizeAccess --> ReturnAuthorized[Retornar autorizado]
    
    ReturnTokenFormatError --> EndSecurity([Fin])
    ReturnUserNotFound --> EndSecurity
    ReturnTokenExpired --> EndSecurity
    ReturnUserInactive --> EndSecurity
    ReturnAuthorized --> EndSecurity
    
    classDef startEnd fill:#2E7D32,stroke:#1B5E20,stroke-width:3px,color:#FFFFFF
    classDef process fill:#1976D2,stroke:#0D47A1,stroke-width:2px,color:#FFFFFF
    classDef decision fill:#F57C00,stroke:#E65100,stroke-width:2px,color:#FFFFFF
    classDef error fill:#D32F2F,stroke:#B71C1C,stroke-width:2px,color:#FFFFFF
    classDef success fill:#388E3C,stroke:#2E7D32,stroke-width:2px,color:#FFFFFF
    
    class StartSecurity,EndSecurity startEnd
    class ReceiveToken,ValidateTokenFormat,ExtractUsername,LoadUserFromToken,ValidateTokenExpiry,CheckUserActive,AuthorizeAccess process
    class TokenFormatOK,UserExists,TokenValid,UserActiveSec decision
    class ReturnTokenFormatError,ReturnUserNotFound,ReturnTokenExpired,ReturnUserInactive error
    class ReturnAuthorized success
```

## Resumen de Procesos BPMN

### 1. **Proceso Principal de Autenticación y Registro**
- **Entrada**: Credenciales de usuario (username/email, password)
- **Procesos**: Validación, autenticación, verificación de estado y tipo
- **Salida**: Token JWT o error

### 2. **Proceso de Autenticación Detallado**
- **Validaciones**: Formato, credenciales, estado del usuario, tipo de usuario
- **Puntos de decisión**: 4 niveles de validación
- **Resultados**: Éxito con token o error específico

### 3. **Proceso de Registro Externo**
- **Validaciones**: 6 campos obligatorios con validaciones específicas
- **Verificaciones**: Unicidad de email y documento
- **Configuración**: Usuario inactivo, externo, username = documento

### 4. **Proceso de Registro Interno**
- **Validaciones**: 7 campos obligatorios + roles
- **Verificaciones**: Unicidad de email, documento y username
- **Configuración**: Usuario activo, interno, roles asignados

### 5. **Proceso de Validación de Seguridad**
- **Validaciones**: Token JWT, expiración, usuario activo
- **Autorización**: Verificación de permisos y estado

### Características Clave de los Procesos:

1. **Validación en Cascada**: Cada proceso valida datos en orden específico
2. **Manejo de Errores**: Puntos de decisión con rutas de error claras
3. **Seguridad**: Múltiples capas de validación y verificación
4. **Trazabilidad**: Cada paso está documentado y puede ser auditado
5. **Escalabilidad**: Procesos modulares que pueden extenderse fácilmente

### Flujos de Excepción:
- **Errores de Validación**: Retorno inmediato con mensaje específico
- **Errores de Negocio**: Verificación de reglas de negocio
- **Errores de Sistema**: Manejo de fallos de base de datos o servicios

### Puntos de Control:
- **Validación de Entrada**: Formato y estructura de datos
- **Validación de Negocio**: Reglas específicas del dominio
- **Validación de Seguridad**: Autenticación y autorización
- **Validación de Persistencia**: Operaciones de base de datos
