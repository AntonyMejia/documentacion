---
id: docrew-incidencia-cibersec
title: Incidencia 1 - Emisión de tokens
---

# Hallazgo 1 – Emisión de tokens con alcance privilegiado (`aws.cognito.signin.user.admin`)

## 1. Resumen ejecutivo

El pentest identificó que, tras autenticarse contra el **User Pool de AWS Cognito**, la aplicación recibe un **access token** con:

- `scope: "aws.cognito.signin.user.admin"`.
- Tiempo de vida (`ExpiresIn`) aproximado de **24 horas**.
- Emisión de **ID Token** y **Refresh Token** adicionales.

El reporte lo clasifica como:

- **CWE-266 – Incorrecta asignación de privilegios.**
- **CVSSv3 9.9 – Alta criticidad.**

Tras el análisis técnico se concluye que:

1. El scope `aws.cognito.signin.user.admin` es el **ámbito reservado por defecto de Cognito** para tokens emitidos por la API de User Pools, y **autoriza únicamente operaciones de autoservicio del propio usuario** (consulta/actualización de sus atributos, MFA, dispositivos, etc.).  
2. El nivel real de acceso **está limitado por los permisos de lectura/escritura de atributos configurados en el App Client** del User Pool.  
3. El riesgo principal se origina en:
   - La posibilidad de que **atributos sensibles (por ejemplo roles/permisos)** estén configurados como **editables** para el App Client.
   - Un **TTL elevado del access token (24 h)** y la existencia de refresh tokens de larga duración.  

No se trata de un fallo en la dependencia `aws-amplify` (versión 4.3.30 o similar), sino de **configuración de Cognito** y de **política de tokens**.

Este documento describe el análisis, las medidas de mitigación y el plan de tratamiento.

---

## 2. Contexto técnico de la aplicación

### 2.1. Arquitectura de autenticación

- **Proveedor de identidad:** AWS **Cognito User Pool**.  
- **Cliente de aplicación:** Frontend Angular/Ionic.
- **SDK utilizado en el front:** `aws-amplify` (`Amplify.configure({ Auth: environment.cognito })` y `Auth.signIn(username, password)`).
- **Flujo observado:**
  1. El usuario envía credenciales al front.
  2. El front llama a la API de Cognito (operaciones `InitiateAuth` / `RespondToAuthChallenge` encapsuladas por Amplify).
  3. Cognito devuelve:
     - `AccessToken` (con scope `aws.cognito.signin.user.admin`).
     - `IdToken`.
     - `RefreshToken`.
  4. El access token se almacena en una cookie (p. ej. `token`) para su uso posterior.

### 2.2. Uso de tokens en la aplicación

- El **access token** se utiliza para consumir APIs protegidas.
- El **Id Token** se utiliza para identificar al usuario en el front.
- El **Refresh Token** permite obtener nuevos tokens sin pedir credenciales, mientras no expire.  

---

## 3. Análisis del alcance `aws.cognito.signin.user.admin`

### 3.1. Definición del scope

Según la documentación oficial de AWS:

- El **token de acceso** de un usuario con el scope `aws.cognito.signin.user.admin` es el permiso para:
  - Leer y escribir atributos del usuario.
  - Enumerar factores de autenticación.
  - Configurar preferencias de MFA.
  - Gestionar dispositivos recordados.  
- Este scope **autoriza operaciones de autoservicio del usuario actual** en la API de Cognito (por ejemplo `GetUser` y `UpdateUserAttributes`).  
- Cuando la autenticación se realiza directamente contra la **API de User Pools**, **éste es el único scope que se devuelve en el access token**.  

En otras palabras: el scope **no otorga privilegios administrativos sobre otros usuarios ni sobre servicios de AWS**; sólo aplica al **propio usuario autenticado** y está limitado por los permisos de atributos.

### 3.2. Permisos de atributos y alcance real

AWS Cognito permite configurar, por cada App Client:

- Para cada atributo (estándar o `custom:`):
  - **Permiso de lectura** (incluible en tokens/consultas).
  - **Permiso de escritura** (actualizable vía API).  

El nivel de acceso del scope `aws.cognito.signin.user.admin` **depende de estos permisos**:

- Si un atributo está marcado como:
  - **Readable + Writable**, el usuario puede leerlo y modificarlo.
  - **Readable only**, el usuario sólo puede consultarlo.
  - **No readable / no writable**, el atributo ni se devuelve ni se puede actualizar (se producirá `NotAuthorizedException` en caso contrario).

Esto implica que **atributos críticos** (roles, flags de administrador, etc.) deben estar configurados como **no escribibles** para el App Client público, o gestionados únicamente desde backend.

### 3.3. Relación con la dependencia `aws-amplify`

- `aws-amplify` actúa como **cliente** de la API de Cognito:
  - Envía las credenciales.
  - Recibe los tokens emitidos por el User Pool.
- El SDK **no define scopes ni tiempos de expiración**, sólo consume lo que Cognito entrega.  
- Por tanto:
  - El scope `aws.cognito.signin.user.admin` y la expiración de 24h **no dependen de la versión de Amplify**, sino de la **configuración del User Pool**.

---

## 4. Evaluación de impacto para el aplicativo

### 4.1. Riesgo teórico (descrito en el pentest)

El reporte considera que:

- Con un token que tenga `aws.cognito.signin.user.admin`:
  - Un usuario podría llamar APIs como `GetUser` / `UpdateUserAttributes` y manipular información de su cuenta.
  - Si la aplicación usa atributos como `custom:rol` para permisos, y estos son *writable*, se podría producir **escalación de privilegios**.
- Con un **TTL de 24h** y refresh tokens sin revocación/rotación adecuada:
  - El abuso podría extenderse en el tiempo si un token es comprometido.

### 4.2. Riesgo real (dependiente de configuración)

El impacto efectivo sobre el aplicativo depende de:

1. **Uso de atributos de Cognito para autorización:**
   - Si los roles/permisos **no** están en atributos editables por el cliente (p. ej. se gestionan en BD interna o grupos de Cognito devueltos sólo vía backend el token no permite convertirse en administrador.
   - Si existen atributos `custom:` de rol/permisos marcados como *writable* para el App Client, sí existe potencial de escalación.

2. **TTL y manejo de tokens:**
   - Un **access token de 24h** amplía la ventana en caso de exfiltración.
   - El refresh token permite obtener nuevos tokens hasta su expiración (configurable de 1 hora a varios años).  

En consecuencia, el hallazgo se clasifica internamente como:

- **Riesgo de configuración / hardening**, cuya severidad final depende de:
  - Resultado de la revisión de atributos (si roles/permisos son editables o no).
  - Ajuste de tiempos de expiración de tokens.

---

## 5. Plan de tratamiento y medidas de mitigación

> **Nota:** las acciones están redactadas en tiempo futuro/presente. Pueden marcarse como *Completadas* en el seguimiento interno una vez implementadas.

### 5.1. Revisión y restricción de atributos de usuario

**Objetivo:** Garantizar que ningún usuario pueda modificar atributos que impacten permisos o modelo de seguridad a través del scope `aws.cognito.signin.user.admin`.

**Acciones:**

1. **Inventario de atributos utilizados para autorización**
   - Identificar atributos estándar y personalizados (`custom:role`, `custom:isAdmin`, etc.) que influyan en:
     - Acceso a módulos.
     - Niveles de privilegio.
     - Flags de administración.

2. **Reconfiguración de permisos de atributos en el App Client**
   - En Cognito Console:
     1. Ir a **User Pools** → seleccionar el User Pool de la aplicación.
     2. Menú **App clients** → seleccionar el cliente utilizado por la app.
     3. Pestaña **Attribute permissions** → **Editar**.  
   - Asegurar que:
     - Los atributos de rol/permisos estén configurados como:
       - **Sólo lectura (read)** para el App Client público, o
       - No expuestos al cliente (gestión únicamente servidor-a-servidor).
     - Sólo los atributos estrictamente necesarios para el funcionamiento del front estén marcados como *read/write*.

3. **Validación técnica**
   - Desde un cliente de prueba (por ejemplo Postman/Burp) utilizar un access token para llamar a `UpdateUserAttributes`.
   - Verificar que:
     - Intentos de escritura sobre atributos de rol/permisos devuelven `NotAuthorizedException`.
     - Sólo atributos de perfil permitidos son modificables.

### 5.2. Ajuste de tiempos de vida de tokens

**Objetivo:** Reducir la ventana de exposición en caso de compromiso del token.

**Acciones:**

1. **Access Token**
   - Modificar la configuración del App Client para que el **access token expire en un intervalo corto**, por ejemplo **10–15 minutos**, de acuerdo con las políticas internas y las recomendaciones de uso de Cognito.  
   - Ajustar el front para que:
     - No almacene tokens más tiempo que su vigencia real (cookies o storage deben caducar igual o antes).
     - Preferentemente, utilizar almacenamiento en memoria en lugar de cookies persistentes cuando sea viable.

2. **Refresh Token**
   - Establecer un periodo de expiración acordado (por ejemplo, **1–7 días** según criticidad y UX).  
   - Documentar el procedimiento de revocación/rotación de refresh tokens en incidentes de seguridad o revocación de acceso.

### 5.3. Separación de capacidades administrativas

**Objetivo:** Asegurar que ninguna operación administrativa de Cognito se encuentre accesible con tokens de usuario final.

**Acciones:**

1. Revisar el código backend para confirmar que las operaciones:
   - `AdminListUsers`, `AdminCreateUser`, `AdminDeleteUser`, etc.
   - Se ejecutan **únicamente** desde servicios backend autenticados con credenciales IAM (roles de ejecución de Lambda, usuarios de sistema, etc.), **no desde el front**.  

2. Verificar que **no existen endpoints expuestos al front** que actúen como “proxy transparente” de estas operaciones sin validación adicional de permisos.

3. Documentar en la arquitectura que:
   - Los tokens de usuario final (con `aws.cognito.signin.user.admin`) sólo se utilizan para **operaciones de autoservicio** y para autenticación/autorización de la app.
   - Las operaciones administrativas se realizan exclusivamente desde el backend.

### 5.4. Monitoreo y auditoría

**Objetivo:** Detectar y responder de forma temprana a un uso anómalo de los tokens / API de Cognito.

**Acciones:**

1. **Registro y análisis de logs**
   - Habilitar o revisar:
     - **CloudTrail** y **CloudWatch Logs** para eventos de Cognito (especialmente `UpdateUserAttributes`, `GetUser`, `Admin*`).  
   - Configurar alertas (por ejemplo, en CloudWatch / SIEM) para:
     - Volúmenes inusuales de llamadas `UpdateUserAttributes`.
     - Intentos repetidos de actualización de atributos de rol/permisos.

2. **Revisión periódica**
   - Establecer revisiones periódicas (trimestrales/semestrales) de:
     - Configuración de App Clients.
     - Permisos de atributos.
     - Tiempos de expiración de tokens.

---

## 6. Conclusión y clasificación final del hallazgo

1. El scope `aws.cognito.signin.user.admin` **no es un comportamiento anómalo del aplicativo ni de la librería Amplify**, sino el **scope estándar emitido por AWS Cognito** para la API de User Pools.  
2. El potencial de “incorrecta asignación de privilegios” depende de:
   - Que existan **atributos de rol/permisos** configurados como **writable** para el App Client.
   - Que la aplicación utilice dichos atributos para autorización sin controles adicionales.
3. La configuración de **TTL de 24 horas para el access token** y refresh tokens prolongados incrementa la ventana de exposición en caso de exfiltración, por lo que se considera una oportunidad clara de **hardening**.

Con la aplicación de las medidas descritas:

- **Se limita la capacidad de modificación de atributos sensibles** únicamente a canales administrativos controlados.
- **Se reduce de forma significativa la ventana de abuso**, ajustando tiempos de vida de Access/Refresh Tokens.
- Se refuerza la separación entre **operaciones de usuario final** y **operaciones administrativas**, alineando la solución con el principio de **mínimo privilegio**.

Por lo anterior, el hallazgo puede reclasificarse internamente como:

- **“Riesgo de configuración / mejora de seguridad”**, con **riesgo residual bajo** tras la implementación y verificación de los controles.

---

## 7. Referencias

- **Descripción del token de acceso – Amazon Cognito**  
  https://docs.aws.amazon.com/es_es/cognito/latest/developerguide/amazon-cognito-user-pools-using-the-access-token.html

- **Ámbitos, M2M y APIs con servidores de recursos (scope `aws.cognito.signin.user.admin`)**  
  https://docs.aws.amazon.com/es_es/cognito/latest/developerguide/cognito-user-pools-define-resource-servers.html

- **Uso de atributos de usuario y permisos de lectura/escritura (User Pool Attributes)**  
  https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html

- **Application-specific settings with app clients (permisos de atributos por app client)**  
  https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-client-apps.html

- **Gestión de expiración de tokens de User Pools**  
  https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-caching-tokens.html  
  y resumen de rangos de expiración:  
  https://aws.amazon.com/about-aws/whats-new/2020/08/amazon-cognito-user-pools-supports-customization-of-token-expiration/

- **UpdateUserAttributes – API de Amazon Cognito (requiere scope `aws.cognito.signin.user.admin`)**  
  https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserAttributes.html

- **Buenas prácticas de seguridad para Amazon Cognito user pools**  
  https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-security-best-practices.html

- **Refresh tokens – Amazon Cognito**  
  https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-the-refresh-token.html

- **Artículo externo: AWS Cognito Token Security – One step closer (Truesec)**  
  https://www.truesec.com/hub/blog/aws-cognito-token-security-one-step-closer

- **Artículo externo: AWS Cognito Privilege Escalation Using Custom Attributes Editing in User Pool**  
  https://medium.com/%40un1quely/aws-cognito-privilege-escalation-using-custom-attributes-editing-in-user-pool-2f8d6eaa3c7f
