# Bloomberg-connect, useful concepts
---
## Index

### A
### B
- [BPKBL](#bpkbl)
- [BSID](#bsid)
### C
- [Chains](#chains)
- [Correlation ID](#correlation-id)
### D
### E
- [EID](#eid)
- [EMRS](#emrs)
### F
- [FIGI](#figi)
- [FSM](#fsm)
### G
- [gRPC](#grpc)
### H
### I
### J
### K
### L
### M
### N
### O
### P
### Q
### R
- [RTI](#rti)
### S
- [Security](#security)
### T
- [Ticker](#ticker)
### U
- [Underlying](#underlying)
### V
### W
### X
### Y
### Z

---
## BPKBL

El BPKBL (Bloomberg Parsekeyable Identifier) es un identificador alfanumérico utilizado en Bloomberg-Connect para identificar valores financieros en la terminal de Bloomberg.
Es una representación estructurada de un instrumento financiero que incluye:
- Ticker (Ejemplo: ``AAPL``)
- Fuente de precios (Ejemplo: ``US``)
- Sector de mercado (Ejemplo: ``Equity``)

### Características del BPKBL
1. Identificación legible y estandarizada
    - Se usa en terminales Bloomberg y en sus API de datos de mercado
    - Facilita la identificación de instrumentos financieros en distintos mercados

2. Útil para suscripciones en Bloomberg B-Pipe
    - Se puede utilizar para suscribirse a datos de mercado en ``//blp/mktlist``

3. Distinto del BSID
    - Mientras que el BSID es un identificador numérico único, el BPKBL es una representación estructurada y legible

4. Usado en cadenas de instrumentos
    - Se emplea para recuperar listas de valores relacionados como opciones o futuros

En conclusión, BPKBL es un identificador estructurado de Bloomberg que permite identificar activos de manera legible. Se usa en suscripciones a datos de mercado y consultas de listas de valores (chains). Es clave para acceder a instrumentos financieros en ``//blp/mktlist`` y ``//blp/mktdata``.

---
## BSID
El BSID (Bloomberg Security Identifier) es un identificador numérico único que Bloomberg asigna a cada instrumento financiero dentro de su plataforma B-Pipe.

Es el principal identificador que utiliza Bloomberg-Connect para suscribirse a datos de mercado y estructurar la información de activos como:
- Acciones
- Bonos
- Futuros
- Opciones
- Índices

---
## Chains

En `bloomberg-connect`, una "chain" (cadena) se refiere a una suscripción a una lista dinámica de valores, utilizada principalmente para obtener instrumentos financieros relacionados, como opciones o futuros de un activo subyacente.

### Características principales de una Chain en Bloomberg-Connect
1. Se utiliza para recuperar listas de instrumentos relacionados
    - Ejemplo: Obtener todas las opciones disponibles para una acción
    - Ejemplo: Obtener todos los futuros de un índice bursátil

2. Se basa en la suscripción al servicio `//blp/mktlist`
    - Permite recuperar los BSID (Bloomberg Security Identifier) de cada instrumento en la cadena

3. Es dinámica
    - A medida que Bloomberg actualiza la información, la chain se actualiza automáticamente

4. Estructura de la suscripción
    - Se suscribe con la estructura:
    ```
    //blp/mktlist/secids/bpkbl/<Underlying_Ticker>
    ```
   - Ejemplo para opciones de AAPL US Equity:
   ```
   //blp/mktlist/secids/bpkbl/AAPL US Equity
   ```

### Tipos de Chains en Bloomberg
1. Market List Chain (``//blp/mktlist``)
    - Devuelve la lista de instrumentos disponibles para un activo subyacente
    - Ejemplo: Lista de todas las opciones para AAPL US Equity

2. Security Chain (``//blp/mktdata``)
    - Devuelve datos de mercado para cada valor dentro de la chain
    - Ejemplo: Último precio de cada opción de AAPL en la cadena

En conclusión, una chain en Bloomberg-Connect permite recuperar listas dinámicas de instrumentos financieros relacionados con un activo subyacente. Se usa el servicio ``//blp/mktlist`` para obtener la lista de valores y //blp/mktdata para obtener datos de mercado. Es una herramienta poderosa para manejar opciones, futuros y derivados financieros en tiempo real.

---
## Correlation ID
El Correlation ID (Correlation Identifier) es un identificador único que se usa en ``bloomberg-connect`` para rastrear y asociar solicitudes y respuestas en las comunicaciones con Bloomberg B-Pipe.

### Características del Correlation ID
1. Identificador único para cada solicitud
    - Se asigna a cada petición enviada a Bloomberg (solicitudes de datos, suscripciones, autenticaciones, etc.)

2. Permite rastrear respuestas asincrónicas
    - Como Bloomberg-Connect usa un modelo basado en eventos, cada respuesta puede llegar en cualquier momento
    - Con el Correlation ID, es posible saber a qué solicitud corresponde cada respuesta

3. Puede ser generado por el usuario o el sistema
    - Si el usuario lo asigna, permite personalizar el rastreo
    - Si no se asigna manualmente, Bloomberg lo genera automáticamente

4. Se usa en sesiones y suscripciones
    - Cada sesión y suscripción tiene un Correlation ID para identificarla
    - También se usa en múltiples conexiones simultáneas a Bloomberg

Correlation ID es de gran importancia en `bloomberg-connect` porque:
- Facilita el rastreo de solicitudes y respuestas en aplicaciones con múltiples peticiones simultáneas
- Es esencial en arquitecturas asincrónicas como gRPC, Kafka y BLPAPI
- Permite depuración y monitoreo eficiente de datos de mercado en tiempo real

---
## EID
El EID (Entitlement Identifier) es un identificador numérico que Bloomberg asigna a los usuarios y aplicaciones para gestionar permisos de acceso a datos financieros en Bloomberg B-Pipe.

### Características del EID
1. Control de acceso a datos de mercado
    - Un usuario o aplicación solo puede suscribirse a datos para los cuales está autorizado

2. Asignado por Bloomberg
    - Cada usuario o empresa tiene un EID único que determina qué información puede acceder

3. Usado en autenticación y permisos
    - Se verifica durante la autenticación con ``//blp/apiauth``
    - Antes de recibir datos de mercado, Bloomberg verifica si el EID tiene derechos de acceso

4. Aplicable a datos en tiempo real y referencia
    - Datos de mercado (``//blp/mktdata``): Controla acceso a precios en tiempo real
    - Datos de referencia (``//blp/refdata``): Controla acceso a información estática de valores

EID tiene bastante importancia en `bloomberg-connect` porque:
- Permite controlar el acceso a datos financieros en tiempo real
- Garantiza que solo usuarios autorizados puedan suscribirse a valores específicos
- Es un requisito en la autenticación de Bloomberg B-Pipe (``//blp/apiauth``)

---
## EMRS
EMRS se refiere a Entitlement Management and Reporting System (Sistema de Gestión de Derechos y Reportes). Es un sistema utilizado en entornos financieros y tecnológicos, especialmente en empresas que gestionan acceso y permisos a datos y servicios de manera controlada.

### Características principales de EMRS
1. Gestión de derechos de acceso: EMRS permite gestionar qué usuarios o aplicaciones tienen acceso a ciertos datos o servicios. En otras palabras, se encarga de controlar qué recursos están disponibles para cada usuario o sistema dentro de una organización.

2. Control de permisos: Además de gestionar el acceso, EMRS también controla los niveles de permiso para esos usuarios. Esto incluye definir qué pueden hacer con esos datos, como visualizar, modificar, o compartir información.

3. Reportes y auditoría: EMRS proporciona funcionalidades de reporte y auditoría, lo que permite a las organizaciones rastrear quién accedió a qué datos y qué acciones realizaron. Esto es fundamental para mantener la seguridad y cumplir con regulaciones.

4. Integración con otras plataformas: Este sistema generalmente se integra con otras plataformas de software para gestionar el acceso a los datos de manera centralizada, como sistemas de bases de datos o APIs.

En resumen, EMRS es un sistema crucial para gestionar, auditar y controlar los derechos de acceso y permisos de usuarios y aplicaciones a diversos recursos dentro de una organización.

---
## FIGI
El FIGI (Financial Instrument Global Identifier) es un identificador global único asignado a valores financieros, diseñado para proporcionar una identificación estandarizada y no propietaria de instrumentos en los mercados globales.

Este identificador fue creado por Bloomberg y es administrado por la Object Management Group (OMG) como un estándar abierto.

### Características del FIGI
1. Identificador único a nivel mundial
    - Se asigna a acciones, bonos, derivados, futuros, divisas y otros instrumentos financieros
    - No cambia a lo largo del tiempo, incluso si el activo cambia de nombre o mercado

2. Estructura alfanumérica de 12 caracteres
    - Formato:
    ```
    <Prefijo>G<8-caracteres-alfa-numéricos><Dígito de control>
    ```
    - Ejemplo: BBG000PF3JB1 (FIGI de Apple Inc.)

3. Alternativa a identificadores propietarios
    - A diferencia del BSID (interno de Bloomberg) o del ISIN (regulado por agencias nacionales), el FIGI es libre y abierto

4. Asociado a diferentes niveles del instrumento, puede aplicarse a:
    - Instrumentos específicos (Acción de Apple en NASDAQ)
    - Empresas emisoras (Apple Inc.)
    - Clases de activos (Bonos corporativos de Apple)

En conclusión, FIGI es un identificador global único y abierto para instrumentos financieros. Es independiente de los sistemas propietarios y se puede usar en cualquier plataforma. Bloomberg-Connect permite consultas de datos de mercado y referencia usando FIGI.

---
## FSM
El término FSM (Finite State Machine o Máquina de Estados Finitos) en Bloomberg-Connect hace referencia a una estructura que gestiona los estados y transiciones de los procesos internos del microservicio, como las sesiones y suscripciones a Bloomberg B-Pipe.

### Características del FSM en Bloomberg-Connect
1. Define estados y transiciones de procesos asincrónicos
    - Se usa para gestionar la conexión con Bloomberg y la suscripción a datos de mercado
2. Controla el flujo de eventos en tiempo real
    - Maneja sesiones (Session FSM) y suscripciones (Subscription FSM)
    - Cada evento recibido desde Bloomberg activa una transición a otro estado
3. Evita estados inconsistentes o bloqueos
    - Si una conexión falla, el FSM define cómo reintentar o cerrar la sesión de forma segura
4. Utiliza eventos de Bloomberg para tomar decisiones
    - Bloomberg envía eventos como ``SessionStarted``, ``SubscriptionStarted`` o ``MarketDataUpdate``
    - El FSM cambia de estado en función de estos eventos

### Tipos de FSM en Bloomberg-Connect
1. Session FSM (Gestión de Sesiones)
    - Maneja la conexión con Bloomberg B-Pipe
    - Estados principales:
        - ``Disconnected`` → No hay sesión activa
        - ``Connecting`` → Intentando conectar con Bloomberg
        - ``Connected`` → Sesión activa y operativa
        - ``Error`` → Se detectó un problema en la conexión
    - Transiciones:
        - Si la conexión a Bloomberg falla → ``Error`` → ``Reconnecting``
        - Si la conexión se establece → ``Connecting`` → ``Connected``

2. Subscription FSM (Gestión de Suscripciones)
    - Maneja las suscripciones a datos de mercado
    - Estados principales:
        - ``AwaitingSubscription`` → Esperando la confirmación de Bloomberg
        - ``Subscribed`` → Suscripción activa y recibiendo datos
        - ``Failed`` → Error en la suscripción
        - ``Unsubscribed`` → Se cerró la suscripción
    - Transiciones:
        - Si Bloomberg acepta la suscripción → ``AwaitingSubscription`` → ``Subscribed``
        - Si la suscripción falla → ``AwaitingSubscription`` → ``Failed``
        - Si se recibe un error de ``Bloomberg`` → ``Subscribed`` → ``Unsubscribed``
    
En conclusión, FSM en Bloomberg-Connect gestiona las sesiones y suscripciones de datos. Cada evento de Bloomberg genera una transición de estado. Evita bloqueos y mejora la estabilidad del microservicio.

---
## gRPC

En `bloomberg-connect`, gRPC es un protocolo de comunicación de alto rendimiento basado en RPC (Remote Procedure Call) que permite la comunicación eficiente entre servicios distribuidos. Se utiliza para interactuar con el microservicio y recuperar datos financieros en tiempo real.

### Características de gRPC en Bloomberg-Connect
1. Protocolo de Comunicación Eficiente
    - Utiliza HTTP/2, lo que lo hace rápido y escalable
    - Soporta transmisión en tiempo real (streaming bidireccional)
    - Se usa para consultas rápidas de datos de mercado

2. API basada en Protocol Buffers (`.proto`)
    - Define los servicios de Bloomberg-Connect en archivos `.proto`
    - Reduce el uso de ancho de banda en comparación con JSON
    - Garantiza compatibilidad entre versiones

3. Integración con Kubernetes y Docker
    - gRPC se implementa dentro del contenedor de `bloomberg-connect`
    - Se expone mediante un puerto en Docker/Kubernetes
    - Se configura en `docker-compose.yml` o `devcontainer.json`

4. Seguridad y Autenticación
    - Puede utilizar TLS/SSL para cifrar las comunicaciones
    - Requiere autenticación con tokens o claves API

En conclusión, gRPC en Bloomberg-Connect se usa para consultar datos de mercado en tiempo real. Utiliza HTTP/2, Protocol Buffers, y permite streaming bidireccional. Se implementa en Docker/Kubernetes y se comunica a través del puerto 6565.

---
## RTI

En `bloomberg-connect`, RTI (Real-Time Index) es una funcionalidad que permite recibir actualizaciones de mercado en tiempo real de índices financieros. Está diseñado para manejar altos volúmenes de datos y garantizar una latencia mínima en la entrega de información crítica del mercado.

### Características de RTI en Bloomberg-Connect
1. Actualizaciones en Tiempo Real
    - Recibe datos de índices bursátiles como S&P 500, Dow Jones, Nasdaq, EURO STOXX 50, etc
    - Se conecta a Bloomberg B-Pipe para recibir datos de mercado en microsegundos

2. Basado en Suscripciones gRPC & Kafka
    - RTI se implementa mediante gRPC para baja latencia
    - Los datos en tiempo real se publican en Kafka, permitiendo consumo masivo por múltiples servicios

3. Integración con Market Data Metrics
    - Expone métricas en Prometheus/Grafana para monitoreo en tiempo real
    - Se puede visualizar la frecuencia de actualizaciones y la latencia de datos

4. Compatible con Bloomberg B-Pipe & API Services
    - Puede obtener datos desde:
        - ``//blp/mktdata`` → Datos de mercado en streaming
        - ``//blp/mktlist`` → Listado de componentes de un índice

En conclusión, RTI (Real-Time Index) en Bloomberg-Connect permite recibir índices bursátiles en tiempo real. Se basa en gRPC, Kafka y Prometheus para ofrecer latencia mínima y escalabilidad. Compatible con Bloomberg B-Pipe (``//blp/mktdata``), permitiendo actualizaciones en microsegundos.

---
## Security

En el ámbito de la API de Bloomberg, un "security" se refiere a un instrumento financiero específico, como una acción, un bono, una opción o cualquier otro tipo de valor que se pueda negociar. Los "securities" son los activos financieros sobre los cuales se solicita información o se realiza un seguimiento. En las solicitudes de datos, se utiliza el término "security" para hacer referencia a la entidad o activo sobre el cual se buscan datos específicos, como precios, volúmenes de transacción o dividendos.

---
## Ticker

El "ticker" es el símbolo único que identifica un valor específico dentro de un mercado o plataforma financiera. En Bloomberg, los tickers se utilizan para identificar de manera única las acciones o valores dentro de una bolsa o mercado específico. Un ejemplo podría ser el ticker "IBM US Equity" que hace referencia a las acciones de IBM en el mercado de EE. UU. Los tickers son clave para acceder a los datos del mercado en tiempo real y a los datos históricos de los valores que se están negociando​.

---
## Underlying

Este término se refiere al activo subyacente de un instrumento financiero derivado. Por ejemplo, en el caso de las opciones o futuros, el "underlying" es el activo sobre el cual se basa el contrato derivado (como una acción, una moneda, un índice, etc.). En las opciones sobre acciones, el activo subyacente sería la acción que puede ser comprada o vendida. En el contexto de la API de Bloomberg, "underlying" se usa para describir el activo que está relacionado con otros productos financieros, como un índice o un contrato de futuros​.

---

<!-- Botón flotante para volver al índice -->
<a href="#index" style="
    position: fixed;
    bottom: 20px;
    right: 20px;
    background-color:#0052a9;
    color: white;
    padding: 10px 15px;
    border-radius: 5px;
    text-decoration: none;
    font-weight: bold;
    box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
">
⬆
</a>