# Reparaciones Flash

Sistema web para gestionar las órdenes de trabajo de un taller de reparación de equipos (laptops, PCs de escritorio, consolas, etc.), desde la recepción hasta la entrega y la garantía.

**Universidad Privada Domingo Savio – Facultad de Ingenierías**
**Materia:** Desarrollo de Sistemas II
**Docente:** Ing. Eddy Roger Calderón Caquiva
**Actividad 1:** Casos de uso, actores, requerimientos funcionales y no funcionales
**Tarija, noviembre 2026**

## Integrantes

- Romeo Lancelot Chavez Cuellar
- Pablo Cesar Maigua Ortiz
- Samuel Narciso Vilte Tarraga
- Manuel Ignacio Aramayo Hoyos
- Guerrero Riera Rodrigo Sebastián
---

## 1. Actores

**Internos** (pertenecen al taller y usan el sistema autenticados) y **externos** (participan de forma limitada o indirecta).

| N.º | Actor | Tipo | Descripción |
|---|---|---|---|
| 1 | Recepcionista | Interno | Registra clientes, abre órdenes de trabajo por cada equipo, asigna técnicos, registra el presupuesto previsto y el pago, marca el equipo como «Entregado», genera el recibo en PDF y cierra la orden. |
| 2 | Técnico | Interno | Cambia el estado de la orden (hasta «Reparado»), registra notas técnicas, lista los repuestos necesarios con sus costos y registra la mano de obra. |
| 3 | Administrador / dueño | Interno | Consulta el panel de control con reportes semanales y mensuales, exporta reportes y gestiona usuarios y roles. |
| 4 | Cliente | Externo | Consulta el estado de su equipo y el presupuesto con su carnet o código de ticket, y acepta o rechaza el presupuesto en línea. |
| 5 | Entidad bancaria / medio de pago | Externo | Procesa los pagos con tarjeta o transferencia; el sistema solo registra el método y la referencia. |
| 6 | Proveedor de repuestos | Externo (indirecto) | Abastece los repuestos; no accede al sistema y sus costos los registra el técnico. |

---

## 2. Casos de uso

Los diagramas están en [`docs/casos-de-uso`](docs/casos-de-uso).

| N.º | Módulo | Casos de uso principales |
|---|---|---|
| 1 | Gestión de órdenes | Registrar cliente, buscar y editar cliente, abrir orden de trabajo, registrar datos del equipo y falla, generar código de ticket, asignar o reasignar técnico, ver órdenes asignadas, cambiar estado de la orden, registrar historial de estados, registrar notas técnicas, consultar estado y presupuesto, aceptar o rechazar presupuesto, registrar pago y entrega, procesar pago con tarjeta o transferencia, generar recibo PDF, cerrar orden, calcular garantía de 90 días y fecha de fin |
| 2 | Gestión de servicios y precios | Registrar presupuesto previsto, listar repuestos necesarios, registrar costo de mano de obra, modificar listado de repuestos, calcular presupuesto estimado, verificar si el costo supera el presupuesto previsto, consultar presupuesto estimado, aceptar o rechazar presupuesto en línea |
| 3 | Autenticación y seguridad | Iniciar sesión, cerrar sesión, verificar contraseña cifrada, validar permiso por rol, cerrar sesión por inactividad, gestionar usuarios y roles (crear, editar, desactivar, asignar rol), registrar log de auditoría |
| 4 | Canales de notificaciones | Notificar decisión del cliente a técnico y recepcionista, notificar cambio de estado al cliente, notificar equipo listo para retirar, enviar notificación por correo electrónico, enviar notificación por SMS / WhatsApp, consultar notificaciones internas |
| 5 | Reportes y monitoreo | Ver panel de control, filtrar por semana, mes o rango de fechas, reporte de equipos atendidos, ranking de técnicos más productivos, ingresos totales semanales y mensuales, repuestos más solicitados, exportar reportes a CSV, consultar log de auditoría, verificar copias de seguridad y disponibilidad |

---

## 3. Requerimientos funcionales (RF)

| Código | Requerimiento |
|---|---|
| RF-001 | El sistema debe permitir a la recepcionista registrar un cliente proporcionando nombre completo, número de carnet, teléfono y correo electrónico. |
| RF-002 | El sistema debe permitir buscar y editar clientes por carnet o nombre, evitando registros duplicados con el mismo número de carnet. |
| RF-003 | El sistema debe permitir a la recepcionista abrir una orden de trabajo por cada equipo recibido (laptop, PC de escritorio, consola u otro), asociada a un cliente. |
| RF-004 | El sistema debe registrar en cada orden la marca, modelo, número de serie, accesorios incluidos y la falla reportada por el cliente. |
| RF-005 | El sistema debe generar automáticamente un código de ticket único al abrir una orden y asignarle el estado inicial «En diagnóstico». |
| RF-006 | El sistema debe permitir a la recepcionista asignar un técnico a cada orden y reasignarlo cuando sea necesario. |
| RF-007 | El sistema debe mostrar al técnico solo las órdenes que tiene asignadas, con filtro por estado. |
| RF-008 | El sistema debe permitir al técnico cambiar el estado de la orden entre «En diagnóstico», «Esperando repuestos» y «Reparado»; el estado «Entregado» solo puede asignarlo la recepcionista al registrar la entrega (RF-016). |
| RF-009 | El sistema debe registrar el historial de cada cambio de estado con la fecha, la hora y el usuario que lo realizó. |
| RF-010 | El sistema debe permitir al técnico registrar notas técnicas durante la reparación y guardarlas con fecha, hora y autor. |
| RF-011 | El sistema debe permitir al técnico listar los repuestos necesarios indicando nombre, cantidad y costo unitario. |
| RF-012 | El sistema debe calcular automáticamente el presupuesto estimado sumando repuestos y mano de obra (RF-028) cada vez que se modifique el listado de repuestos o la mano de obra. |
| RF-013 | El sistema debe permitir al cliente consultar su orden ingresando su número de carnet o código de ticket, sin crear una cuenta, y mostrar el estado del equipo y el presupuesto estimado; si busca por carnet y tiene varias órdenes, debe mostrar la lista de todas ellas. |
| RF-014 | El sistema debe permitir al cliente aceptar o rechazar el presupuesto en línea cuando el presupuesto estimado supere el presupuesto previsto registrado en la orden (RF-028). |
| RF-015 | El sistema debe registrar la decisión del cliente con fecha y hora, notificar al técnico y a la recepcionista, y detener la reparación si el presupuesto es rechazado. |
| RF-016 | El sistema debe permitir a la recepcionista registrar el pago al momento de la entrega, indicando el método (efectivo, tarjeta o transferencia), y marcar la orden como «Entregado». |
| RF-017 | El sistema debe generar una nota de venta o recibo digital imprimible en formato PDF con los datos del cliente, equipo, servicios, repuestos y monto pagado. |
| RF-018 | El sistema debe permitir cerrar una orden únicamente cuando exista un pago registrado. |
| RF-019 | El sistema debe registrar automáticamente la fecha de fin al cerrar la orden. |
| RF-020 | El sistema debe calcular una garantía de 90 días sobre el servicio realizado a partir de la fecha de fin y mostrarla en el recibo. |
| RF-021 | El sistema debe mostrar al administrador un panel de control con reportes semanales y mensuales del número de equipos atendidos, filtrables por rango de fechas. |
| RF-022 | El sistema debe mostrar al administrador el ranking de técnicos más productivos, según la cantidad de órdenes completadas en el período seleccionado. |
| RF-023 | El sistema debe calcular y mostrar al administrador los ingresos totales por semana y por mes, a partir de los pagos registrados. |
| RF-024 | El sistema debe mostrar al administrador los repuestos más solicitados en el período seleccionado, con su cantidad total. |
| RF-025 | El sistema debe permitir al administrador exportar los reportes del panel en formato CSV (RNF-017). |
| RF-026 | El sistema debe permitir a los usuarios internos iniciar y cerrar sesión con usuario y contraseña, mostrando solo las opciones correspondientes a su rol. |
| RF-027 | El sistema debe permitir al administrador crear, editar y desactivar usuarios y asignarles un rol (recepcionista, técnico o administrador). |
| RF-028 | El sistema debe permitir registrar en cada orden el presupuesto previsto (por la recepcionista) y el costo de mano de obra (por el técnico). |

---

## 4. Requerimientos no funcionales (RNF)

### 4.1 Requisitos del producto

| Código | Requerimiento |
|---|---|
| RNF-001 | El sistema debe responder a cualquier búsqueda o consulta de ticket en menos de 2 segundos. |
| RNF-002 | El sistema debe soportar al menos 30 usuarios consultando sus tickets de forma simultánea sin degradación del rendimiento. |
| RNF-003 | El sistema debe almacenar las contraseñas cifradas con un algoritmo de hash con sal (bcrypt o Argon2), nunca en texto plano. |
| RNF-004 | El sistema debe controlar el acceso por roles, validando el permiso en el servidor en el 100 % de las rutas protegidas. |
| RNF-005 | El sistema debe proteger los datos de los clientes transmitiéndolos únicamente por HTTPS (TLS 1.2 o superior). |
| RNF-006 | El sistema debe visualizarse correctamente tanto en pantallas de escritorio como en teléfonos móviles de 360 px de ancho o más (diseño responsivo). |
| RNF-007 | El sistema debe funcionar en las dos últimas versiones de Chrome, Firefox, Edge y Safari, sin instalar software adicional. |
| RNF-008 | El sistema debe permitir a la recepcionista registrar una orden de trabajo completa en menos de 3 minutos. |
| RNF-009 | El sistema debe estar disponible el 99 % del tiempo en el horario de atención del taller. |
| RNF-010 | El sistema debe realizar una copia de seguridad automática diaria de la base de datos y conservarla durante 30 días. |
| RNF-011 | El sistema debe registrar un log de auditoría de las operaciones críticas (cambios de estado, pagos, cierres y cambios de usuarios) conservado durante al menos 1 año. |
| RNF-012 | El sistema debe cerrar automáticamente la sesión de un usuario interno tras 15 minutos de inactividad. |
| RNF-013 | El sistema debe generar el recibo en PDF en menos de 5 segundos. |
| RNF-014 | El sistema debe poder escalar hasta 100 usuarios concurrentes sin requerir un rediseño de la arquitectura. |

### 4.2 Requisitos de organización

| Código | Requerimiento |
|---|---|
| RNF-015 | El sistema debe desarrollarse con una arquitectura modular en capas y con código que siga los estándares de codificación acordados por el equipo, usando control de versiones (Git). |
| RNF-016 | El sistema debe cumplir con la normativa vigente de protección de datos personales, usando los datos de los clientes solo para la gestión de sus reparaciones. |

### 4.3 Requisitos externos

| Código | Requerimiento |
|---|---|
| RNF-017 | El sistema debe permitir exportar los reportes del panel de control (RF-021 a RF-024) en formato CSV compatible con Microsoft Excel. |
| RNF-018 | El sistema debe generar recibos en PDF estándar, abiertos en cualquier lector de PDF e imprimibles en tamaño carta o A4. |

---

## 5. Estructura del repositorio

```
reparaciones-flash/
├── README.md
├── docs/
│   ├── requisitos/        # Documento de requisitos y actores
│   ├── casos-de-uso/      # Diagramas de casos de uso (.drawio y .png)
│   └── diagrama-clases/   # Diagrama de clases (.drawio y .png)
└── src/                   # Código fuente (próximas etapas)
```

## 6. Flujo de trabajo con ramas

- `main`: versión entregable.
- `develop`: integración del trabajo del equipo.
- `docs/...` y `feature/...`: una rama por tarea o módulo, que se une a `develop` mediante Pull Request revisado por otro integrante.
