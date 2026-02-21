# Aplicación de seguimiento y rastreo de asesores comerciales

## 1) Master Prompt para crear la aplicación

Copia y pega este prompt en tu asistente de IA preferido para generar el sistema completo:

```text
Actúa como un arquitecto de software senior + product manager + UX lead + tech lead full-stack.
Necesito que construyas una aplicación web y móvil para el seguimiento y rastreo de asesores comerciales en campo, incluyendo alertas por desviación de recorridos.

Objetivo del negocio:
- Supervisar en tiempo real la ubicación de asesores.
- Validar cumplimiento de rutas planificadas y visitas programadas.
- Alertar desviaciones operativas para actuar tempranamente.
- Mejorar productividad, trazabilidad y calidad de servicio.

Entregables que debes producir:
1. Arquitectura técnica completa (frontend, backend, móvil, base de datos, colas/eventos, observabilidad y seguridad).
2. Diseño de datos (modelo relacional + eventos de geolocalización).
3. API REST/GraphQL con especificación de endpoints y contratos JSON.
4. Motor de reglas de alertas de desviación de recorridos (configurable por zona/cliente/asesor).
5. UI/UX: wireframes textuales de pantallas clave (dashboard, mapa, detalle de asesor, alertas, configuración).
6. Historias de usuario priorizadas con criterios de aceptación y Definition of Done.
7. Plan de implementación por fases (MVP, v2, v3) con estimación por sprint.
8. Estrategia de pruebas (unitarias, integración, e2e, performance, seguridad).
9. Plan de despliegue CI/CD y runbook operativo.
10. Métricas y KPIs del negocio (cumplimiento de ruta, tiempo fuera de ruta, tasa de alertas atendidas, etc.).

Restricciones y lineamientos:
- Multi-rol: Administrador, Supervisor, Asesor, Auditor.
- Geolocalización cada 15-60 segundos con tolerancia a conectividad intermitente.
- Modo offline en móvil con sincronización diferida.
- Privacidad: consentimiento, minimización de datos, retención configurable y trazabilidad de accesos.
- Alertas configurables por geocerca, tiempo, distancia y secuencia de visitas.
- Escalabilidad para miles de asesores concurrentes.

Tecnologías sugeridas (puedes proponer alternativas justificadas):
- Frontend web: React + TypeScript.
- App móvil: Flutter o React Native.
- Backend: Node.js (NestJS) o Python (FastAPI).
- Base de datos: PostgreSQL + PostGIS.
- Mensajería/eventos: Kafka o RabbitMQ.
- Cache: Redis.
- Infraestructura: Docker + Kubernetes + observabilidad (Prometheus/Grafana + OpenTelemetry).

Salida esperada:
- Responde con secciones claras y accionables.
- Incluye ejemplos de payloads, tablas, reglas, y pseudocódigo donde aplique.
- No entregues generalidades: prioriza diseño implementable.
```

---

## 2) Funcionalidades de la aplicación

### 2.1 Módulo de usuarios y roles
- Registro y administración de usuarios.
- Roles: Administrador, Supervisor, Asesor, Auditor.
- Permisos granulares por módulo.
- Bitácora de accesos y cambios.

### 2.2 Planeación comercial y rutas
- Creación de rutas por asesor y por día.
- Asignación de clientes/paradas con ventanas horarias.
- Optimización sugerida de ruta (opcional en v2).
- Reprogramación manual por supervisor.

### 2.3 Geolocalización en tiempo real
- Captura de ubicación GPS en móvil.
- Visualización en mapa en dashboard web.
- Línea de tiempo de posiciones.
- Indicador de estado: en ruta, detenido, fuera de ruta, sin señal.

### 2.4 Sistema de alertas por desviación de recorridos
- Alerta por salida de corredor permitido.
- Alerta por distancia excedida del trayecto planificado.
- Alerta por omisión de visita programada.
- Alerta por permanencia excesiva en punto no planificado.
- Escalamiento automático si no se atiende en X minutos.
- Configuración de umbrales por zona/equipo/asesor.

### 2.5 Gestión de visitas y evidencia
- Check-in/check-out en cliente.
- Evidencias: fotos, notas, firma digital (opcional).
- Resultado de visita (efectiva/no efectiva, motivo).
- Coordenadas y timestamp de evidencias.

### 2.6 Reportes y analítica
- Cumplimiento de rutas por periodo.
- Ranking de productividad por asesor/equipo.
- Mapa de calor de zonas con mayor desviación.
- Exportación a PDF/Excel/CSV.

### 2.7 Notificaciones
- Push móvil para asesor y supervisor.
- Correo/WhatsApp/Slack (según integración).
- Centro de notificaciones dentro del sistema.

### 2.8 Seguridad y cumplimiento
- Cifrado de datos en tránsito y reposo.
- Políticas de retención y anonimización.
- Consentimiento de geolocalización.
- Trazabilidad completa para auditoría.

---

## 3) Historias de usuario

### HU-01: Monitoreo en tiempo real
**Como** supervisor
**quiero** ver en un mapa la ubicación actual de mis asesores
**para** detectar retrasos o incidencias de forma inmediata.

**Criterios de aceptación:**
- El mapa muestra asesores activos con actualización <= 60 segundos.
- Se visualiza último timestamp por asesor.
- Se distingue visualmente estado normal vs. alerta.

### HU-02: Alerta por desviación de ruta
**Como** supervisor
**quiero** recibir alertas cuando un asesor se salga del recorrido planificado
**para** tomar acciones correctivas rápidas.

**Criterios de aceptación:**
- El sistema dispara alerta al exceder umbral de distancia/tiempo.
- La alerta incluye asesor, ubicación, ruta esperada y severidad.
- Se puede marcar como atendida, descartada o escalada.

### HU-03: Configuración de reglas
**Como** administrador
**quiero** configurar reglas de desviación por equipo o zona
**para** adaptar el sistema a distintos contextos operativos.

**Criterios de aceptación:**
- Se pueden crear reglas con condiciones combinadas.
- Las reglas tienen vigencia, prioridad y estado (activa/inactiva).
- Toda modificación queda en bitácora.

### HU-04: Registro de visita en campo
**Como** asesor
**quiero** registrar check-in y check-out con evidencia
**para** demostrar cumplimiento de visita.

**Criterios de aceptación:**
- El sistema registra geolocalización y hora automáticamente.
- Permite adjuntar foto y comentario.
- Funciona offline y sincroniza al recuperar señal.

### HU-05: Reporte de cumplimiento
**Como** gerente comercial
**quiero** ver reportes de cumplimiento por asesor
**para** evaluar desempeño y tomar decisiones.

**Criterios de aceptación:**
- Reportes por rango de fechas y filtros por región/equipo.
- Métricas clave: visitas efectivas, desviaciones, tiempo en ruta.
- Exportación a formato descargable.

### HU-06: Auditoría operativa
**Como** auditor
**quiero** consultar trazabilidad de eventos y cambios de reglas
**para** validar cumplimiento de políticas.

**Criterios de aceptación:**
- Registro inmutable de eventos críticos.
- Búsqueda por usuario, fecha y tipo de acción.
- Acceso de solo lectura para perfil auditor.

---

## 4) Alcance del proyecto

### 4.1 MVP (8-12 semanas)
- Login y gestión básica de usuarios/roles.
- App móvil con tracking GPS y modo offline básico.
- Dashboard con mapa en tiempo real.
- Planeación manual de rutas.
- Alertas básicas de desviación (distancia + tiempo).
- Reportes básicos de cumplimiento.

### 4.2 Fuera de alcance MVP (para v2+)
- Optimización avanzada de rutas con IA.
- Integraciones complejas ERP/CRM bidireccionales.
- Predicción de riesgo de incumplimiento con ML.
- Gamificación y scoring avanzado.

### 4.3 Supuestos
- Los asesores cuentan con smartphone con GPS activo.
- Existe conectividad parcial en la mayoría de zonas.
- El negocio definirá parámetros operativos iniciales (umbrales).

### 4.4 Riesgos
- Baja precisión GPS en interiores.
- Resistencia al cambio por parte de usuarios.
- Saturación de alertas por configuración deficiente.

---

## 5) Interfaz de la aplicación (UX/UI)

### 5.1 Pantalla: Dashboard (Supervisor)
- KPIs superiores: asesores activos, en ruta, fuera de ruta, alertas abiertas.
- Mapa central con ubicación en vivo y capas (rutas, geocercas, clientes).
- Panel lateral de alertas ordenadas por severidad.
- Filtros: fecha, zona, equipo, asesor.

### 5.2 Pantalla: Detalle de asesor
- Tarjeta resumen: estado actual, batería, conectividad.
- Timeline de posiciones y eventos del día.
- Ruta planificada vs. ruta real superpuesta.
- Botón de contacto rápido (llamada/mensaje).

### 5.3 Pantalla: Gestión de alertas
- Bandeja de alertas con estados (nueva, atendida, escalada, cerrada).
- Detalle con causa, ubicación y evidencia.
- Acciones: asignar responsable, comentar, cerrar.

### 5.4 Pantalla: Configuración de reglas
- Formulario de regla con condiciones (distancia, tiempo, geocerca).
- Alcance de la regla (global, por equipo, por asesor).
- Prioridad y simulación previa de impacto.

### 5.5 App móvil del asesor
- Inicio de jornada / fin de jornada.
- Lista de visitas del día con navegación.
- Check-in/check-out y captura de evidencia.
- Estado de sincronización y cola offline.

### 5.6 Principios de diseño
- Interfaz clara y accionable en menos de 3 clics para incidencias críticas.
- Semáforo de estados (verde/ámbar/rojo) para lectura rápida.
- Diseño responsive y accesible (WCAG AA).
