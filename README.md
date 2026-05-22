# 🚀 Frontend Stitch - GitOps & FluxCD

[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io)
[![FluxCD](https://img.shields.io/badge/FluxCD-%230070f3.svg?style=for-the-badge&logo=flux&logoColor=white)](https://fluxcd.io)
[![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org)
[![TailwindCSS](https://img.shields.io/badge/tailwindcss-%2338B2AC.svg?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com)

Este repositorio es una plantilla de **GitOps declarativo** configurada para desplegar y reconciliar de manera automática una Landing Page premium sobre **FluxCD** en un clúster de Kubernetes (`expo-flux`).

El clúster está configurado para auto-sincronizarse con este repositorio utilizando Flux v2, asegurando que cualquier cambio en la configuración se aplique de inmediato sin intervención manual.

---

## 📂 Estructura del Proyecto

El repositorio está estructurado siguiendo las mejores prácticas de GitOps y la modularidad de Flux:

```text
├── apps/
│   └── nginx.yaml             # Despliegue de Nginx, Servicio y ConfigMap con la Landing Page
├── clusters/
│   └── expo-flux/
│       ├── apps.yaml          # Kustomization que apunta al directorio /apps
│       └── flux-system/       # Componentes del sistema generados por Flux
│           ├── gotk-components.yaml
│           ├── gotk-sync.yaml
│           └── kustomization.yaml
├── index.html                 # Código fuente local de la landing page para desarrollo
└── README.md                  # Documentación del proyecto
```

---

## 🏗️ Flujo de GitOps (Arquitectura)

```mermaid
graph TD
    A[Desarrollador] -->|Push a Main| B(Repositorio GitHub)
    B -->|Sincronización SSH| C[Flux Source Controller]
    C -->|Reconciliación Kustomize| D[Kustomize Controller]
    D -->|Despliega / Actualiza| E[Nginx Deployment]
    E -->|Sirve| F[Landing Page de FluxCD]
```

### Componentes Desplegados:
1. **Nginx Deployment:** Mantiene dos réplicas altamente disponibles del servidor Nginx.
2. **Kubernetes Service:** Expone el puerto 80 del servidor web en el clúster.
3. **ConfigMap (`nginx-html`):** Contiene el código HTML/Tailwind inyectado directamente en el directorio `/usr/share/nginx/html` de Nginx.

---

## ⚡ ¿Cómo Replicar o Desplegar este Clúster?

Para inicializar tu propio clúster utilizando este repositorio de GitOps, asegúrate de tener instalado el CLI de `flux` y acceso a tu clúster de Kubernetes, y ejecuta:

```bash
flux bootstrap github \
  --owner=galeyro \
  --repository=frontend-stich \
  --branch=main \
  --path=./clusters/expo-flux \
  --personal
```

Este comando:
1. Instalará los controladores de Flux en el espacio de nombres `flux-system`.
2. Configurará una llave de despliegue SSH en tu repositorio de GitHub para permitir la sincronización automática y segura.
3. Comenzará a monitorear la ruta `./clusters/expo-flux` para desplegar las aplicaciones.

---

## 🎨 Características de la Landing Page

La landing page incluida en el ConfigMap y en `index.html` es una interfaz premium y ultra-moderna que incluye:
* **Estilo Glassmorphic y Oscuro:** Diseñado con una paleta de colores HSL refinada.
* **Glow Interactivo:** Un efecto de iluminación dinámica que sigue el movimiento del mouse.
* **Sistema de Partículas:** Animaciones de fondo premium renderizadas dinámicamente con Canvas.
* **Responsive Completo:** Optimizada para dispositivos móviles y pantallas de alta definición mediante Tailwind CSS.
