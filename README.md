## 📌 Descripción del Proyecto

Este proyecto consiste en una aplicación web desarrollada con Nuxt (modo CSR - Client Side Rendering) que simula una galería virtual de arte, tomando como fuente de datos la API pública del Museo de Arte de Cleveland. El objetivo principal fue construir una interfaz interactiva que permita explorar obras de arte de manera dinámica, aplicando conceptos modernos de desarrollo frontend, consumo de APIs y organización de componentes.

La aplicación funciona completamente del lado del cliente, lo que significa que todas las solicitudes a la API se realizan en tiempo real desde el navegador, permitiendo una experiencia fluida e interactiva para el usuario. A través de estas consultas, se obtienen datos relevantes como imágenes de las obras, títulos, autores, descripciones y otros metadatos asociados.

## 🧩 Arquitectura y Estructura

El proyecto está organizado siguiendo la estructura estándar de Nuxt:

- pages/index.vue: Página principal donde se renderiza la galería de obras. Aquí se realiza la lógica principal de consumo de la API y el despliegue de resultados.
- components/: Contiene componentes reutilizables que mejoran la modularidad del proyecto:
- NavBar.vue: Barra de navegación principal de la aplicación.
- ArtDetailsModal.vue: Modal interactivo que muestra información detallada de cada obra seleccionada.
- app.vue: Componente raíz que define la estructura base de la aplicación.
- nuxt.config.ts: Configuración del framework.
- public/: Archivos estáticos.

## ⚙️ Funcionalidades Principales

- 🎨 Galería dinámica de obras de arte: Se muestran múltiples piezas obtenidas desde la API.
- 🔍 Consulta de API en tiempo real: Los datos se cargan dinámicamente utilizando peticiones HTTP.
- 🪟 Modal de detalles: Al seleccionar una obra, se despliega información adicional en un componente modal.
- 🧭 Navegación intuitiva: Implementación de una barra de navegación para mejorar la experiencia del usuario.
- ⚡ Renderizado en cliente (CSR): Toda la lógica se ejecuta en el navegador, optimizando la interacción directa con el usuario.

## 💡 Objetivo del Proyecto

El propósito de este desarrollo fue aplicar conocimientos en:

- Consumo de APIs REST
- Manejo de estados en el frontend
- Componentización en frameworks modernos
- Renderizado del lado del cliente (CSR)
- Diseño de interfaces interactivas

Además, se buscó simular un caso real de aplicación web cultural, donde el usuario puede explorar contenido artístico de forma accesible y visualmente atractiva.

## 🚀 Tecnologías Utilizadas

- Nuxt 3
- Vue 3
- JavaScript / TypeScript
- HTML5 & CSS3
- API pública del Cleveland Museum of Art

