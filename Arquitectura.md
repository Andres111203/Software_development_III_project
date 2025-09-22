# Documento de Arquitectura del Sistema de Reservas y Gestión de Turnos para Servicios Locales

## 1. Introducción  
Este documento describe la arquitectura inicial del **sistema de reservas y gestión de turnos para servicios locales**.  
El sistema busca digitalizar y optimizar la administración de citas en negocios como gimnasios, consultorios médicos, barberías, salones de belleza y coworkings.  

**Problema identificado:**  
Actualmente muchos negocios locales manejan sus reservas de forma manual o poco organizada (libretas, llamadas telefónicas, WhatsApp), lo que genera pérdidas de tiempo, confusiones y baja satisfacción de los clientes.  

**Objetivo del proyecto:**  
Construir una aplicación web moderna que permita a los clientes reservar citas en línea, recibir notificaciones y realizar pagos anticipados, mientras que los administradores gestionan la disponibilidad de horarios y recursos.  

**Equipo:** Grupo # 4  
**Integrantes:**  
- Náthalie Wilches Tamayo - 2569482  
- Juan David Camacho - 2266292  
- Andres Eduardo Narvaez - 2259545  
- Juan Gabriel Paredes - 2266183  
**Fecha:** 12/09/2025  

---

## 2. Requisitos Funcionales  

### **Principales funcionalidades**
- **Gestión de usuarios:** clientes, empleados y administradores con roles diferenciados.  
- **Agenda de turnos:** visualización de calendarios y horarios disponibles.  
- **Reservas en línea:** creación, modificación y cancelación de citas.  
- **Pagos anticipados:** integración con pasarela de pagos (Stripe/MercadoPago/PayPal sandbox).  
- **Notificaciones:** envío de confirmaciones y recordatorios.  
- **Optimización con caché:** almacenamiento en Redis de la agenda más consultada.  
- **Panel administrativo:** estadísticas de uso (turnos atendidos, ingresos, cancelaciones).  

### **Historias de Usuario**
| **ID**    | **Historia de Usuario**                                                                                                           | **Criterios de Aceptación**                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **US-01** | Como cliente quiero poder registrarme e iniciar sesión para acceder a la aplicación y reservar turnos. | - El cliente debe poder registrarse con correo y contraseña válidos.<br>- Al registrarse, recibirá un correo de confirmación.<br>- El inicio de sesión debe usar JWT para autenticación segura. |
| **US-02** | Como cliente quiero poder reservar y pagar un turno en línea para asegurar mi cita. | - El sistema debe permitir seleccionar un servicio y un horario disponible.<br>- Se debe integrar con una pasarela de pagos sandbox.<br>- Al confirmar la reserva, el cliente recibe un correo/notificación con los detalles. |
| **US-03** | Como administrador quiero gestionar la disponibilidad de horarios y recursos para optimizar el uso del negocio. | - El administrador puede agregar, modificar o bloquear horarios.<br>- El calendario debe actualizarse en tiempo real para evitar reservas duplicadas.<br>- El panel debe mostrar estadísticas básicas de uso. |

---

## 3. Requisitos de Calidad  

### **Historias de Calidad**
| **ID**    | **Fuente**         | **Estímulo**                                                                                         | **Artefactos**                               | **Entorno**         | **Respuesta**                                                        | **Medida de Respuesta**                                                                                 |
| --------- | ------------------ | ---------------------------------------------------------------------------------------------------- | -------------------------------------------- | ------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **RQ-01** | Cliente | Desea que el sistema esté disponible 24/7 sin interrupciones. | Aplicación y servidores | Producción | El sistema debe garantizar alta disponibilidad. | Disponibilidad mínima del **99.9% mensual** (máximo 40 min de caída). |
| **RQ-02** | Cliente | Quiere que la consulta de la agenda sea rápida incluso en horas pico. | Backend, caché Redis | Producción | Respuesta inmediata al consultar horarios. | Tiempo de respuesta < **300ms** en el 95% de las solicitudes. |
| **RQ-03** | Administrador | Se incrementa la demanda de reservas en un 100% en menos de 1 hora. | Infraestructura de microservicios | Horarios pico | El sistema debe escalar automáticamente. | Escalabilidad horizontal soportando hasta el **doble de carga** inicial manteniendo 70% de respuestas < 1s. |
| **RQ-04** | Desarrollador | Necesita que el sistema sea seguro para proteger datos sensibles. | Código fuente, BD y API | Desarrollo y Producción | Autenticación y cifrado de datos. | Implementar **JWT + cifrado AES-256** para contraseñas y datos sensibles. |

---

## 4. Restricciones del Sistema  

### **Lista de Restricciones**
| **Tipo de Restricción** | **Descripción** |
|------------------------|----------------|
| Tecnológica | El backend se desarrollará en **Spring Boot (Java)** y el frontend en **Angular** con Bootstrap 5.<br>Base de datos: **PostgreSQL**.<br>Mensajería: **RabbitMQ/Kafka**.<br>Caché: **Redis**.<br>Pagos: **Stripe/MercadoPago sandbox**. |
| Infraestructura | El despliegue inicial será con **Docker Compose**. Se debe considerar escalabilidad en la nube a futuro. |
| Regulatorias | Cumplir con la **Ley 1581 de 2012** (protección de datos personales en Colombia) y la **Ley 1273 de 2009** (protección de la información y los datos). |
| Negocio | El sistema debe priorizar la **usabilidad y satisfacción del cliente**. El tiempo de respuesta debe cumplir las métricas de SLA definidas. |

---

## 5. Formato de Entrega  
- **Formato:** Markdown (`.md`) en el repositorio de Codelabs.  
- **Nombre del documento:** `Arquitectura.md`.  
- **Extensión máxima:** 3 páginas.  

---
