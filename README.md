# 🚀 Proyecto Semestral - Arquitectura de Microservicios Innovatech

**Autor:** Brayan Ignacio Allendes Casanova  
**Sección:** 003D  
**Asignatura:** Introducción a Herramientas DevOps  

---

## 📖 Descripción del Proyecto
Este repositorio contiene la contenedorización y orquestación de la arquitectura orientada a microservicios para el proyecto "Innovatech". Se implementó la separación del Frontend (React) y dos servicios de Backend (Despachos y Ventas en Spring Boot), conectados a una base de datos MySQL, asegurando un despliegue ágil, seguro y persistente mediante **Docker** y **Docker Compose**.

## 🏗️ Arquitectura y Contenedorización

### 1. Backend (Despachos y Ventas)
Se implementó un enfoque **Multi-stage build** en los `Dockerfile`:
* **Etapa 1 (Build):** Uso de `maven:3.9.6-eclipse-temurin-17` para compilar el código fuente y generar el artefacto `.jar` omitiendo los tests para mayor velocidad.
* **Etapa 2 (Run):** Uso de la imagen ligera `eclipse-temurin:17-jre-jammy` para producción.
* **Seguridad:** Se creó un usuario sin privilegios root (`appuser`) para la ejecución del servicio, cumpliendo con estrictos estándares de ciberseguridad.

### 2. Frontend (React/Vite)
* **Etapa 1 (Build):** Uso de `node:18-alpine` para instalar dependencias y compilar los estáticos (`npm run build`).
* **Etapa 2 (Run):** Uso de un servidor web `nginx:alpine` para servir los archivos estáticos de manera eficiente, exponiendo el puerto 80.

### 3. Base de Datos (MySQL)
* Se utilizó la imagen oficial de `mysql:8.0`.
* **Persistencia de Datos:** Se configuró un Named Volume (`db_data:/var/lib/mysql`) para asegurar que la información transaccional de las órdenes no se pierda ante reinicios o destrucción de los contenedores.

## ⚙️ Orquestación (Docker Compose)
El archivo `docker-compose.yml` levanta toda la infraestructura bajo una red interna. 

**Mapeo de Puertos:**
* **Frontend (Nginx):** `localhost:80`
* **Backend Despachos:** `localhost:8080`
* **Backend Ventas:** `localhost:8081`
* **Base de Datos:** `localhost:3306`

**Inyección de Dependencias:** Se utilizaron variables de entorno (`DB_ENDPOINT`, `DB_PORT`, `DB_NAME`, etc.) para que los microservicios descubran dinámicamente el contenedor de la base de datos sin hardcodear IPs.

## 🚀 Instrucciones de Despliegue Local

Para levantar el proyecto completo en tu entorno local, asegúrate de tener instalado Docker y Docker Compose, y ejecuta el siguiente comando en la raíz del proyecto:

```bash
docker-compose up --build -d
