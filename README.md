# Guía Teórica de SonarQube
## Introducción a SonarQube

SonarQube es una plataforma de código abierto desarrollada por SonarSource para la inspección continua de la calidad del código. Su principal objetivo es detectar y analizar automáticamente problemas en el código fuerte para mejorar su calidad y seguridad.

## Características Principales

### 1. Análisis Estático de Código

- **Detección temprana** de bugs, vulnerabilidades y olores de código
- Soporte para **múltiples lenguajes** de programación
- **Reglas personalizables** según las necesidades del proyecto

### 2. Métricas de Calidad

- **Cobertura de código**
- **Deuda técnica** cuantificada
- **Duplicación de código**
- **Complejidad ciclomática**
- **Mantenibilidad** del código

### 3. Integración Continua

- Soporte para **CI/CD**
- Integración con **GitHub Actions, Jenkins, Azure DevOps**, etc.
- Análisis en tiempo real

## Arquitectura de SonarQube

### Componentes principales

1. **SonarQube Server**
   - Procesa los informes de análisis
   - Almacena resultados en la base de datos
   - Proporciona la interfaz web

2. **SonarQube Database**
   - Almacena la configuración
   - Guarda los resultados del análisis
   - Mantiene el historial de métricas

3. **SonarScanners**
   - Ejecutan el análisis del código
   - Se ejecutan en máquinas de integración continua o en local
   - Generan informes que se envían al servidor

## Métricas Clave

1. **Fiabilidad**
   - Bugs
   - Tasa de fallos
   - Tiempo medio entre fallos

2. **Seguridad**
   - Vulnerabilidades
   - Puntos de inyección
   - Controles de acceso

3. **Mantenibilidad**
   - Código duplicado
   - Complejidad ciclomática
   - Cumplimiento de estándares

4. **Cobertura**
   - Cobertura de pruebas unitarias
   - Cobertura de condiciones
   - Cobertura de líneas

## Flujo de Trabajo Típico

1. **Configuración inicial**
   - Instalación del servidor
   - Configuración de la base de datos
   - Definición de reglas de calidad

2. **Integración con el flujo de desarrollo**
   - Configuración de análisis automático
   - Integración con herramientas de CI/CD
   - Definición de umbrales de calidad

3. **Monitoreo continuo**
   - Revisión de informes
   - Identificación de tendencias
   - Toma de decisiones basada en métricas

## Beneficios de Usar SonarQube

- **Mejora continua** de la calidad del código
- **Detección temprana** de problemas
- **Estandarización** del código
- **Reducción** de la deuda técnica
- **Documentación automática** de la calidad del código

## Recursos Adicionales

- [Documentación Oficial de SonarQube](https://docs.sonarqube.org/)
- [Guías de Buenas Prácticas](https://docs.sonarqube.org/latest/user-guide/concepts/)
- [Foro de la Comunidad](https://community.sonarsource.com/)

## Contribuir

SonarQube es un proyecto de código abierto. Las contribuciones son bienvenidas a través de [GitHub](https://github.com/SonarSource/sonarqube).

---
*Última actualización: Octubre 2023*


---

## Cómo Ejecutar SonarQube con Docker

### Requisitos Previos

- Docker y Docker Compose instalados en tu sistema
- Al menos 4GB de RAM disponibles (8GB recomendado)
- Al menos 2 núcleos de CPU

### Pasos para la Ejecución

1. **Crea un archivo `docker-compose.yml`** con la siguiente configuración:

```yaml
version: '3.8'

services:
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    ports:
      - "9000:9000"
    environment:
      - SONAR_JDBC_URL=jdbc:postgresql://sonarqube_db:5432/sonar
      - SONAR_JDBC_USERNAME=sonar
      - SONAR_JDBC_PASSWORD=sonar
    depends_on:
      - sonarqube_db
    networks:
      - sonarnet

  sonarqube_db:
    image: postgres:14
    container_name: sonarqube_db
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
      - POSTGRES_DB=sonar
    volumes:
      - sonarqube_db_data:/var/lib/postgresql/data
    networks:
      - sonarnet

networks:
  sonarnet:
    driver: bridge

volumes:
  sonarqube_db_data:
```

2. **Inicia los contenedores** con el siguiente comando:

```bash
docker-compose up -d
```

3. **Accede a la interfaz web** de SonarQube en tu navegador:
   - URL: http://localhost:9000
   - Usuario: `admin`
   - Contraseña: `admin` (se te pedirá cambiarla en el primer inicio)

4. **Configura tu primer proyecto**:
   - Haz clic en "Create Project"
   - Sigue el asistente para configurar el análisis de tu código

### Comandos Útiles

- **Detener los contenedores**:
  ```bash
  docker-compose down
  ```

- **Ver logs de los contenedores**:
  ```bash
  docker-compose logs -f
  ```

- **Reiniciar SonarQube**:
  ```bash
  docker-compose restart sonarqube
  ```

### Solución de Problemas Comunes

- **Problemas de memoria**: Si SonarQube no inicia correctamente, verifica que tengas suficiente memoria RAM disponible.
- **Puerto en uso**: Si el puerto 9000 está en uso, modifica el puerto en el archivo `docker-compose.yml`.
- **Persistencia de datos**: Los datos de la base de datos se guardan en un volumen de Docker llamado `sonarqube_db_data`.