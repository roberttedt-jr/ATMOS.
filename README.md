# ATMOS. 🌦️ | Weather Dashboard & PWA

[![Live Demo](https://img.shields.io/badge/Demo-Live%20App-00f2fe?style=for-the-badge&logo=vercel&logoColor=white)](https://roberttedt-jr.github.io/ATMOS./app.html)
[![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla%20ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/es/docs/Web/JavaScript)
[![PWA](https://img.shields.io/badge/PWA-Ready-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)](https://web.dev/progressive-web-apps/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

**ATMOS.** es una Progressive Web App (PWA) de meteorología en tiempo real centrada en la precisión del dato, la fluidez visual y el rendimiento óptimo. Diseñada bajo una filosofía *zero-bloatware* (Vanilla JavaScript), ofrece predicciones hiperlocales, métricas ambientales detalladas (AQI, índice UV, punto de rocío), pronósticos horarios graficados dinámicamente y visualizaciones cartográficas sin la sobrecarga ni dependencias pesadas de frameworks modernos.

---

## 🎯 Retos y Solución Técnica

Muchas aplicaciones meteorológicas modernas sufren de sobrecarga de dependencias (paquetes pesados, bundles sobredimensionados y tiempos de carga lentos en conexiones móviles inestables). 

**ATMOS.** se diseñó para abordar tres desafíos esenciales:
1. **Rendimiento e independencia de frameworks:** Implementar una SPA interactiva con gestión de estado, renderizado reactivo del DOM y navegación por capas utilizando únicamente **Vanilla JS (ES6+)**, logrando tiempos de carga casi instantáneos (LCP < 1.2s).
2. **Consumo y normalización de APIs meteorológicas abiertas:** Integración con la API de **Open-Meteo** (sin límites estrictos de tasa de consulta ni necesidad de exponer tokens privados en cliente) combinada con capas visuales de **Windy.com**.
3. **Persistencia local y experiencia offline (PWA):** Gestión de ubicaciones favoritas, ajustes de unidades (°C/°F, km/h/mph) y personalización de usuario mediante `localStorage` y soporte para instalación nativa en dispositivos móviles.

---

## ✨ Características Principales

- 📍 **Geolocalización Automática y Búsqueda Global:** Detección de ubicación inmediata mediante la Geolocation API del navegador y buscador reactivo de ciudades y coordenadas.
- 📊 **Telemetría Ambiental Completa:**
  - Temperatura actual, sensación térmica, mínimas y máximas diarias.
  - Viento (velocidad y dirección), ráfagas y presión atmosférica.
  - Humedad relativa, punto de rocío y visibilidad.
  - Calidad del aire (AQI) y nivel de radiación ultravioleta (UV Index).
  - Horarios astronómicos: amanecer, atardecer y ciclo solar interactivo.
- 📈 **Gráfico Horario en Tiempo Real:** Visualización visual de la progresión de temperatura y probabilidad de lluvia hora a hora renderizada sobre `<canvas>`.
- 📅 **Pronóstico Extendido a 7 Días:** Desglose con rangos térmicos dinámicos y estados de nubosidad/precipitación normalizados según códigos WMO.
- 🗺️ **Radar Meteorológico Integrado:** Vista de mapas satelitales y de capas de viento impulsados por la tecnología de Windy.
- 🔔 **Sistema de Alertas de Lluvia:** Notificaciones contextuales según umbrales de precipitación.
- ⭐ **Gestión de Favoritos:** Persistencia sin base de datos externa para guardar y conmutar rápidamente entre múltiples ciudades.

---

## 🛠️ Stack Tecnológico y Decisiones de Arquitectura

| Capa / Módulo | Tecnología | Justificación Técnica |
| :--- | :--- | :--- |
| **Core / Lógica** | JavaScript Vanilla (ES6+) | Mantenimiento de bundle size mínimo, cero overhead de virtual DOM y control directo de ciclo de vida. |
| **Interfaz y Estilos** | HTML5 semántico + CSS3 (Flexbox/Grid, Custom Properties) | Soporte nativo para modo oscuro/claro, transiciones aceleradas por hardware (GPU) y diseño mobile-first responsivo. |
| **Renderizado Gráfico** | HTML5 Canvas API | Renderizado eficiente de curvas de temperatura/precipitación horarias sin librerías externas de gráficos. |
| **Datos Meteorológicos** | [Open-Meteo API](https://open-meteo.com/) | API abierta, de baja latencia, con modelado numérico de precisión (GFS, ECMWF, DWD ICON) y sin rate-limits agresivos. |
| **Cartografía / Radar** | [Windy.com Embed](https://windy.com/) | Visualización fluida de capas de radar meteorológico y corrientes de viento en tiempo real. |
| **Almacenamiento Local** | Web Storage API (`localStorage`) | Persistencia en cliente para caché de última ubicación, ciudades guardadas, preferencias de unidad y avatar. |
| **Capacidades PWA** | Web App Manifest + Viewport meta tags | Experiencia de app nativa en iOS/Android: icono en pantalla de inicio, modo *standalone* y eliminación de barras del navegador. |

---

## 🏛️ Flujo de Datos

```mermaid
flowchart TD
    A[Usuario / Navegador] -->|Permiso Geolocation| B(Geolocation API)
    A -->|Búsqueda manual| C(Buscador de Ciudades)
    
    B -->|Latitud, Longitud| D{Manejador de Estado JS}
    C -->|Coordenadas Geocoding| D
    
    D -->|Fetch Asíncrono| E[Open-Meteo API]
    E -->|JSON Response: Hourly, Daily, Current| D
    
    D -->|Normalización WMO & Unidades| F[Render Engine DOM]
    D -->|Dataset Horario| G[Canvas 2D - Gráfico Térmico]
    D -->|Coordenadas| H[Windy Map Embed]
    
    D <-->|Lectura / Escritura| I[(localStorage: Favoritos / Config)]
