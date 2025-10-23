# Reto Práctico: Configuración de SonarQube

## Objetivo
Configurar e implementar SonarQube para analizar la calidad del código de un proyecto de tu stack tecnológico favorito.

## Instrucciones

### 1. Configuración Inicial
1. Instala Docker y Docker Compose en tu sistema si aún no los tienes.
2. Crea un archivo `docker-compose.yml` con la configuración de SonarQube y PostgreSQL.
3. Inicia los contenedores de SonarQube y verifica que el servidor esté funcionando correctamente.

### 2. Configuración del Proyecto
1. Crea un nuevo proyecto en SonarQube para tu aplicación.
2. Genera un token de acceso para autenticar los análisis.
3. Configura el archivo de propiedades de SonarQube para tu proyecto.

### 3. Integración con tu Stack
Configura el análisis de código para un proyecto existente en tu stack tecnológico (Node.js, Python, Java, etc.). Asegúrate de:
- Especificar correctamente los directorios de código fuente y pruebas
- Configurar las reglas de calidad según las mejores prácticas para tu lenguaje
- Incluir la cobertura de pruebas si es posible

### 4. Análisis de Código
1. Ejecuta el análisis de código localmente.
2. Verifica los resultados en el dashboard de SonarQube.
3. Identifica y documenta al menos 3 problemas de calidad encontrados.

## Requisitos de Entrega
1. Archivo `docker-compose.yml` configurado.
2. Captura de pantalla del dashboard de SonarQube mostrando el análisis de tu proyecto.
3. Documentación breve (máx. 1 página) que incluya:
   - Stack tecnológico utilizado
   - Problemas principales encontrados
   - Soluciones implementadas o planeadas

## Siguientes Pasos
En la próxima sesión, integraremos este análisis en un pipeline de CI/CD para ejecutarse automáticamente con cada cambio en el repositorio.

## Recursos Adicionales
- [Documentación oficial de SonarQube](https://docs.sonarqube.org/latest/)
- [Guía de instalación](https://docs.sonarqube.org/latest/setup/install-server/)
- [Análisis de código fuente](https://docs.sonarqube.org/latest/analysis/overview/)

---
*Fecha de entrega: 1 semana*
*Formato de entrega: Enlace a repositorio con documentación adjunta*
