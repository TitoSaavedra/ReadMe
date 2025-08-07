## 🎨 Introducción

Esta propuesta corresponde a una **maqueta funcional y navegable** de la plataforma de tokenización. El objetivo de este entregable es **representar visualmente y técnicamente cada módulo** principal del sistema, incluyendo **transacciónes en la testnet de Ethereum**.

La maqueta está diseñada con componentes visuales modernos (Tailwind, React, etc.) e integra la biblioteca **ethers.js** para la interacción con wallets y contratos inteligentes en una red de pruebas. La finalidad es validar la lógica completa de una operación comercial: desde la firma de un contrato hasta la liberación de pagos, reflejada en la blockchain de prueba.

No se contempla la integración con una red blockchain real, la gestión de fondos en vivo, la creación de contratos inteligentes reales ni la implementación de un sistema productivo, Ademas está diseñada para usarse en un backend no productivo, por lo que no se contempla la integración de funciones en AWS, documentación de APIs, ni pruebas unitarias o de integración.

> ⚠️ **Este entregable no es un sistema productivo**. Las operaciones se limitan al entorno de testnet, sin gestión de fondos reales, pero con lógica funcional y trazabilidad verificable en blockchain.

---

---

## Detalle Técnico por Módulo - Plataforma de Tokenización Minera

### 1. Gestión de Usuarios y Login

#### Módulo Opcional: Registro de Usuarios

Es un módulo opcional que permite a los usuarios registrarse en la plataforma. Incluye la creación de un formulario de registro, validaciones de campos y lógica para el manejo de diferentes tipos de usuarios (minera o comprador). Este módulo es útil para establecer una base de usuarios antes de iniciar las operaciones comerciales, pero para la maqueta funcional, se puede simular el registro y login con datos predefinidos.

| Área         | Tarea                                            | Descripción detallada                                                                                  | Horas estimadas | Comentario                                | Opcional           |
| ------------ | ------------------------------------------------ | ------------------------------------------------------------------------------------------------------ | --------------- | ----------------------------------------- | ------------------ |
| **Frontend** | Diseño UI Registro/Login                         | Formularios con campos dinámicos (tipo empresa chilena/extranjera, datos empresa, datos representante) | 4 h             | Tailwind + validaciones básicas           | No                 |
|              | Validación de formularios                        | Validaciones de campos requeridos, rut/ID válidos, correo electrónico, etc.                            | 0.5 h           | JS / React Hook Form                      | No                 |
|              | Lógica de registro por rol                       | Diferente flujo visual para minera o comprador                                                         | 2 h             | Condiciones dinámicas por tipo de usuario | Sí                 |
|              | Panel de perfil editable (frontend)              | Vista para modificar datos de empresa o representante legal                                            | 2 h             | Con fetch de datos previos                | Sí                 |
|              | Adaptación de layout por rol (UI)                | Cambiar visualización según si el usuario es admin, minera o comprador                                 | 3 h             | Sidebar y accesos                         | No                 |
| **Backend**  | API de registro con lógica por tipo              | Endpoint que recibe el tipo de empresa, datos del representante y crea registros en cascada            | 2 h             | Uso de Node.js + Express                  | No                 |
|              | API de login con generación de JWT               | Autenticación con verificación de credenciales y firma de tokens JWT con expiración                    | 0.3 h           | JWT con refresh opcional                  | No                 |
|              | API para actualizar perfil                       | Endpoint para modificar datos de empresa o representante legal                                         | 2 h             | Validaciones similares a registro         | Sí                 |
|              | Middleware para roles                            | Protección de rutas y lógica condicional por rol                                                       | 2.5 h           | Express middleware                        | Sí                 |
|              | DB: modelo de usuarios, empresas, representantes | Definición y relación de modelos con ORM (ej: Mongoose o Sequelize)                                    | 3.5 h           | Incluye validaciones únicas               | No                 |
| **Horas**    |                                                  |                                                                                                        | **Totales**     |                                           | **Sin Opcionales** |
|              |                                                  |                                                                                                        | **21.8 h**      |                                           | **13.3 h**         |

---

### 2. Vinculación y Firma con MetaMask

#### Módulo Clave: Integración con MetaMask

Este módulo es esencial para la plataforma, ya que permite a los usuarios vincular su wallet de MetaMask y firmar transacciones. Incluye la creación de un botón de conexión, la firma de login y la validación de la firma en el backend. Es fundamental para las operaciones Web3 y la validación de identidad con wallet.

| Área         | Tarea                          | Descripción detallada                                                  | Horas estimadas | Comentario                           | Opcional           |
| ------------ | ------------------------------ | ---------------------------------------------------------------------- | --------------- | ------------------------------------ | ------------------ |
| **Frontend** | Conectar wallet con botón      | Componente de conexión con MetaMask. Mostrar dirección activa.         | 3 h             | Ethers.js / Web3Modal                | No                 |
|              | Firma de login con MetaMask    | Solicitar firma criptográfica al iniciar sesión.                       | 3 h             | Login no requiere password           | No                 |
| **Backend**  | Validar firma digital          | Validar firma enviada desde el frontend con dirección pública y nonce. | 4 h             | Ethers.js para recuperación de firma | No                 |
|              | Asociar wallet a empresa       | Endpoint para vincular dirección a cuenta validada                     | 3 h             | Una wallet por empresa               | No                 |
|              | Verificación de login seguro   | Lógica de nonce, expiración y validez de la firma.                     | 3 h             | Login sin credenciales adicionales   | No                 |
|              | DB: almacenar wallet vinculada | Modelo para guardar dirección de wallet asociada a empresa             | 3 h             | Incluye validación de unicidad       | No                 |
| **Horas**    |                                |                                                                        | **Totales**     |                                      | **Sin Opcionales** |
|              |                                |                                                                        | **19 h**        |                                      | **19 h**           |

---

### 3. Layout General

#### Módulo Clave: Estructura Base

Este módulo define la estructura base de la plataforma, incluyendo el sidebar dinámico, el header con la wallet conectada y la protección de rutas según el rol del usuario. Es fundamental para la navegación y el control de acceso por vistas y roles.

| Área         | Tarea                                      | Descripción detallada                                                            | Horas estimadas | Comentario                     | Opcional           |
| ------------ | ------------------------------------------ | -------------------------------------------------------------------------------- | --------------- | ------------------------------ | ------------------ |
| **Frontend** | Sidebar dinámico según rol                 | Cambiar opciones del sidebar según el tipo de usuario (admin, minera, comprador) | 4 h             | Renderizado condicional        | No                 |
|              | Header con wallet conectada y logout       | Mostrar dirección de wallet conectada y opción de cerrar sesión                  | 3 h             | Ethers.js + contexto de sesión | Sí                 |
|              | Modularización de vistas                   | Dividir estructura en vistas reutilizables por rol                               | 4 h             | Componentes reutilizables      | Sí                 |
|              | Protección de rutas (authContext + router) | Redirección de rutas si el usuario no está autenticado o no tiene permisos       | 4 h             | React Router + Context API     | No                 |
| **Backend**  | Middleware de autorización por token y rol | Verifica JWT y extrae permisos para acceso a rutas protegidas                    | 4 h             | Uso de Express middleware      | Sí                 |
|              | Redirección por rol                        | Controlador para enviar al usuario a su dashboard correspondiente                | 2 h             | Según claims del JWT           | Sí                 |
|              | DB: sesiones activas (opcional)            | Modelo para guardar sesiones activas con tokens y expiración                     | 4 h             | Permite invalidar sesiones     | Sí                 |
| **Horas**    |                                            |                                                                                  | **Totales**     |                                | **Sin Opcionales** |
|              |                                            |                                                                                  | **25 h**        |                                | **11 h**           |

---

### 4. Panel del Cliente

#### Módulo Clave: Principal de Interacción

Este módulo es el principal punto de interacción para el comprador de minerales.

| Área         | Tarea                                 | Descripción detallada                                                        | Horas estimadas | Comentario                             | Opcional           |
| ------------ | ------------------------------------- | ---------------------------------------------------------------------------- | --------------- | -------------------------------------- | ------------------ |
| **Frontend** | Visualización de intenciones de venta | Tabla con datos públicos de minerales disponibles para compra                | 3 h             | Filtro por tipo, volumen, pureza       | No                 |
|              | Crear contrato de compra              | Formulario para iniciar la compra de un lote publicado                       | 2.5 h           | Incluye selección y confirmación       | No                 |
|              | Ver contratos y estados               | Tabla con seguimiento de los contratos iniciados                             | 2 h             | Estados: pendiente, firmado, entregado | No                 |
|              | Firmar contrato y liberar pagos       | Interfaz para firmar contrato y mostrar botones para liberar pagos (40%/60%) | 4 h             | Integrado con MetaMask                 | Sí                 |
| **Backend**  | API para listar ofertas de venta      | Endpoint público para consultar ofertas activas                              | 2 h             | Con paginación y filtros               | No                 |
|              | API para crear contratos              | Guardar datos del comprador, lote e iniciar contrato                         | 3 h             | Verificación de disponibilidad         | Sí                 |
|              | Validar firma y liberar pagos         | Lógica para validar acciones de pago y firma según el estado del contrato    | 4 h             | Simulación blockchain                  | No                 |
|              | DB: modelos de contratos y ofertas    | Definición de esquemas para contratos y ofertas de venta                     | 4 h             | Relaciones entre modelos               | No                 |
| **Horas**    |                                       |                                                                              | **Totales**     |                                        | **Sin Opcionales** |
|              |                                       |                                                                              | **24.5 h**      |                                        | **17.5 h**         |

---

### 5. Panel de Administración

#### Módulo Opcional: Proveedor

Este módulo permite a la minera registrar ventas e inventario, gestionar entregas y validar contratos. Es opcional porque la maqueta puede simular estas acciones sin necesidad de un backend completo, se pueden cargar data estaticamente en donde se muestren los lotes disponibles y sus estados.

| Área         | Tarea                                        | Descripción detallada                                                                    | Horas estimadas | Comentario                                | Opcional           |
| ------------ | -------------------------------------------- | ---------------------------------------------------------------------------------------- | --------------- | ----------------------------------------- | ------------------ |
| **Frontend** | Registrar intención de venta                 | Formulario para que la minera registre mineral disponible: tipo, volumen, pureza, precio | 3.5 h           | Inputs validados                          | No                 |
|              | Subir certificado asociado                   | Adjuntar PDF/imagen con análisis de mineral                                              | 2.0 h           | Input tipo file con vista previa opcional | Sí                 |
|              | Confirmar entrega de lote vendido            | Botón para confirmar que el mineral fue entregado                                        | 1.5 h           | Visible solo si el contrato está firmado  | Sí                 |
|              | Ver estado del contrato asociado             | Tabla con estados del contrato: firmado, en espera, entregado, completado                | 2.0 h           | Estado conectado a backend                | Sí                 |
| **Backend**  | API para registrar intención                 | Endpoint que guarda lote con datos de venta + certificación                              | 2.5 h           | Asocia lote a empresa minera              | No                 |
|              | Confirmar entrega si contrato firmado        | Validación condicional para cambiar estado del contrato                                  | 2.0 h           | Cambia a “entregado”                      | Sí                 |
|              | API para listar contratos de minera          | Endpoint para que la minera vea sus contratos y estados                                  | 2.0 h           | Filtros por estado                        | Sí                 |
|              | DB: modelo de lotes y relación con contratos | Definición de esquema para lotes y su relación con contratos                             | 2.5 h           | Incluye validaciones                      | No                 |
| **Horas**    |                                              |                                                                                          | **Totales**     |                                           | **Sin Opcionales** |
|              |                                              |                                                                                          | **18 h**        |                                           | **8.5 h**          |

---

### 6. Gestión de Contratos

#### Módulo Clave: Formalización de Transacciones

Este módulo es esencial para formalizar transacciones en la plataforma. Permite a los usuarios gestionar contratos.

| Área         | Tarea                                 | Descripción detallada                                                                              | Horas estimadas | Comentario                                    | Opcional           |
| ------------ | ------------------------------------- | -------------------------------------------------------------------------------------------------- | --------------- | --------------------------------------------- | ------------------ |
| **Frontend** | Visualización de contratos existentes | Tabla con filtros por estado, tipo de mineral, empresa y fecha. Pagina por rol.                    | 2.5 h           | Filtros dinámicos, paginación y ordenamiento  | No                 |
|              | Detalle del contrato individual       | Mostrar contrato completo, hash, participantes, condiciones y estado con diseño tipo “PDF viewer”. | 3.0 h           | Estética legal, estilo documento              | Sí                 |
|              | Acciones según rol y estado           | Botones condicionales: firmar, liberar pagos, aprobar entrega, anular, renovar, etc.               | 2.5 h           | Reglas de visibilidad según contexto y rol    | Sí                 |
| **Backend**  | API CRUD para contratos               | Endpoints para crear, obtener, modificar estado, eliminar (soft delete) contratos                  | 3.5 h           | Incluye validaciones y seguridad por rol      | No                 |
|              | Generar hash simulado del contrato    | Algoritmo para generar hash único (ej: SHA256) para trazabilidad simbólica                         | 1.0 h           | Simulación de blockchain, sin despliegue real | No                 |
|              | Estados del contrato                  | Definir lógica de transición de estados. Incluye validación de dependencias (firma, entrega, etc.) | 2.0 h           | Manejo de errores y condiciones por flujo     | Sí                 |
|              | DB: modelo de contratos               | Esquema con relaciones a usuarios, lotes, condiciones, firma, hash, historial de estados           | 3.0 h           | Normalización y relaciones                    | No                 |
| **Horas**    |                                       |                                                                                                    | **Totales**     |                                               | **Sin Opcionales** |
|              |                                       |                                                                                                    | **17.5 h**      |                                               | **10 h**           |

---

### 7. Inventario y Entrega

#### Módulo Clave: Inventario

| Área         | Tarea                                       | Descripción detallada                                                                                           | Horas estimadas | Comentario                          | Opcional           |
| ------------ | ------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | --------------- | ----------------------------------- | ------------------ |
| **Frontend** | Formulario de ingreso de mineral            | Formulario con campos controlados para ingresar tipo de mineral, volumen, contrato relacionado y observaciones. | 3.5 h           | Inputs controlados con validaciones | No                 |
|              | Subir certificado de entrega                | Adjuntar archivo PDF o imagen como comprobante de entrega de mineral.                                           | 2.0 h           | Vista previa opcional               | Sí                 |
|              | Estado de verificación del sistema          | Mostrar estado del ingreso: pendiente, validado, rechazado. Alerta visual tipo badge o label.                   | 1.5 h           | Integrado con backend               | Sí                 |
| **Backend**  | API para registrar ingreso de mineral       | Endpoint que guarda el lote entregado junto a los metadatos: volumen, tipo, fecha, contrato vinculado.          | 2.5 h           | Protección por rol                  | No                 |
|              | Asociar ingreso al contrato correspondiente | Validar que el contrato esté activo y se puedan agregar entregas al mismo.                                      | 2.0 h           | Verificación de condiciones         | No                 |
|              | Validación por admin/oráculo simulado       | Cambio de estado del lote a 'validado' tras aprobación por parte del administrador del sistema.                 | 2.0 h           | Lógica condicional y controlada     | Sí                 |
|              | DB: modelo de entregas e inventario         | Esquema para registrar entregas, asociando a contratos y lotes.                                                 | 2.5 h           | Incluye validaciones de unicidad    | No                 |
|              | Registro de historial de entregas           | Guardar cada entrega con fecha, lote, estado y hash simulado.                                                   | 1.5 h           | Trazabilidad de transacciones       | Sí                 |
| **Horas**    |                                             |                                                                                                                 | **Totales**     |                                     | **Sin Opcionales** |
|              |                                             |                                                                                                                 | **17.5**        |                                     | **10.5 h**         |

---

### 8. Flujo Financiero

#### Módulo Clave: Simulación de Pagos

Este módulo debe simular la transacción de pago en la Testnet de Ethereum, reflejando el flujo financiero.
La logica de la liberación de pagos contra entrega es Opcional, debido a que la maqueta puede simular el proceso sin necesidad de una integración real con blockchain.

| Área         | Tarea                                        | Descripción detallada                                                                               | Horas estimadas | Comentario                      | Opcional           |
| ------------ | -------------------------------------------- | --------------------------------------------------------------------------------------------------- | --------------- | ------------------------------- | ------------------ |
| **Frontend** | Visualización de estado de pagos             | Componente visual que refleja progreso del contrato: 40% liberado, 60% pendiente, 100% completado.  | 2.0 h           | Uso de barras, badges o estados | No                 |
|              | Botón para liberar 40% inicial               | Visible tras firma de ambas partes. Ejecuta lógica de liberación simulada de fondos.                | 2.0 h           | Validación previa necesaria     | Sí                 |
|              | Botón para liberar 60% restante              | Aparece luego de entrega validada por la minera. Ejecuta transición a pago completo.                | 2.0 h           | Validación condicional          | Sí                 |
|              | Mostrar hash simulado de transacción         | Muestra hash tipo blockchain como referencia visual del pago registrado.                            | 1.5 h           | Elemento de trazabilidad visual | Sí                 |
| **Backend**  | Validar condiciones de pago                  | Verifica que se cumplan requisitos del contrato antes de liberar cada porcentaje (firma / entrega). | 2.5 h           | Control de flujo y condiciones  | No                 |
|              | Guardar historial de transacciones simuladas | Registro de cada pago con hash, monto, fecha y tipo de liberación en la base de datos.              | 2.0 h           | Auditoría de simulaciones       | No                 |
|              | Simulación de lógica blockchain              | Generar y asociar hash simulado para representar transacción en testnet o de forma local.           | 1.5 h           | No depende de red real          | Sí                 |
|              | DB: modelo de pagos                          | Esquema para almacenar historial de pagos: contrato, monto, porcentaje, estado, fecha, hash.        | 2.0 h           | Modelo relacional con contratos | No                 |
| **Horas**    |                                              |                                                                                                     | **Totales**     |                                 | **Sin Opcionales** |
|              |                                              |                                                                                                     | **15.5 h**      |                                 | **9.0 h**          |

---

### 9. Indicadores Comerciales

#### Módulo Opcional: Mejora Visual

Este módulo es opcional y se centra en mejorar la visualización de indicadores comerciales clave, como precios de mercado, volúmenes transaccionados y estadísticas de contratos. No es esencial para la lógica transaccional.

| Área         | Tarea                            | Descripción detallada                                                                 | Horas estimadas | Comentario                          | Opcional           |
| ------------ | -------------------------------- | ------------------------------------------------------------------------------------- | --------------- | ----------------------------------- | ------------------ |
| **Frontend** | Widgets de precios de mercado    | Componente visual que muestra precios de cobre, USD y ETH en tiempo real.             | 2.5 h           | Datos ilustrativos, estilo widget   | Sí                 |
|              | Autoactualización cada 1 minuto  | Lógica para refrescar datos desde endpoint mock o externo sin recargar la vista.      | 1.5 h           | Usar setInterval o fetch recurrente | Sí                 |
| **Backend**  | API simulada para precios        | Endpoint de prueba que devuelve precios estáticos o aleatorios en formato JSON.       | 1.5 h           | Mock de fácil implementación        | Sí                 |
|              | Posibilidad de socket o cron job | Base preparada para actualizar precios con WebSocket o cron job en producción futura. | 1.5 h           | Preparado para evolución escalable  | Sí                 |
| **Horas**    |                                  |                                                                                       | **Totales**     |                                     | **Sin Opcionales** |
|              |                                  |                                                                                       | **7.0 h**       |                                     | **0.0 h**          |

---

### 10. Panel Administrador

#### Módulo Clave: Supervisión y Validación

Este módulo es esencial para la supervisión y validación global del sistema.

| Área         | Tarea                                            | Descripción detallada                                                                 | Horas estimadas | Comentario                              | Opcional           |
| ------------ | ------------------------------------------------ | ------------------------------------------------------------------------------------- | --------------- | --------------------------------------- | ------------------ |
| **Frontend** | Validación de empresas nuevas                    | Tabla con empresas registradas pendientes. Acciones: aprobar / rechazar.              | 2.5 h           | Filtro por tipo de empresa y botones UI | No                 |
|              | Ver contratos pendientes y entregas no validadas | Vista resumen con filtros para revisar entregas y contratos pendientes.               | 2.5 h           | Badges y alertas por estado             | Sí                 |
|              | Aprobar entregas, certificados, ingresos         | Panel donde el admin puede aprobar documentos o rechazar entregas según estado.       | 2.5 h           | Acciones controladas por lógica         | Sí                 |
|              | Dashboard con reportes visuales                  | Visualización de métricas clave mediante gráficos sobre contratos, pagos y entregas.  | 3.5 h           | Uso de librerías como Chart.js          | No                 |
| **Backend**  | Endpoints restringidos a admin                   | Middleware y rutas protegidas por rol de administrador mediante JWT.                  | 2.0 h           | Autorización y seguridad por rol        | No                 |
|              | Cambiar estados de contratos y ventas            | Endpoints para actualizar el estado de contratos, entregas y transacciones.           | 2.5 h           | Validaciones de transición de estado    | No                 |
|              | Configuración global del sistema                 | Permitir modificar parámetros como tokens, porcentajes, límites desde el panel admin. | 2.0 h           | Ajustes del sistema desde la interfaz   | Sí                 |
|              | DB: modelos de configuración y auditoría admin   | Modelos para almacenar parámetros, cambios de configuración y logs administrativos.   | 2.0 h           | Control de trazabilidad y cambios       | Sí                 |
| **Horas**    |                                                  |                                                                                       | **Totales**     |                                         | **Sin Opcionales** |
|              |                                                  |                                                                                       | **19.5 h**      |                                         | **15.5 h**         |

---

### Resumen de Horas

| Módulo                           | Total Horas | Total Horas (Sin Opcionales) |
| -------------------------------- | ----------- | ---------------------------- |
| Gestión de Usuarios y Login      | 21.8 h      | 13.3 h                       |
| Vinculación y Firma con MetaMask | 19 h        | 19 h                         |
| Layout General                   | 25 h        | 11 h                         |
| Panel del Cliente                | 24.5 h      | 17.5 h                       |
| Panel de Administración          | 18 h        | 8.5 h                        |
| Gestión de Contratos             | 17.5 h      | 10 h                         |
| Inventario                       | 17.5 h      | 10.5 h                       |
| Flujo Financiero                 | 15.5 h      | 9 h                          |
| Indicadores Comerciales          | 7 h         | 0 h                          |
| Panel Administrador              | 19.5 h      | 15.5 h                       |
| **Total General**                | **185.3 h** | **92.5 h**                   |

Si solamente se hace lo necesario para una maqueta funcional y navegable, el total de horas conciderando solo los módulos clave y sin los opcionales es de **92,5 horas**.
