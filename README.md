# Gestor de Tareas - Svelte

Proyecto web para gestionar tareas. Construido con **Svelte** para aprender a crear aplicaciones reactivas y modernas. Permite añadir, organizar y completar tareas de forma sencilla.

## 🎯 Características

- ✅ **Añadir tareas** - Crea nuevas tareas rápidamente
- 📋 **Gestión de prioridades** - Reorganiza el orden de tus tareas pendientes
- ✔️ **Marcar como completada** - Traslada tareas al listado de completadas
- 🗑️ **Eliminar tareas** - Borra tareas completadas
- 💾 **Almacenamiento persistente** - Guarda tus tareas en localStorage
- 🎨 **Diseño moderno** - Interfaz atractiva con animaciones

## 📋 Requisitos previos

- **Node.js** (versión 18 o superior)
- **npm** o **yarn**

## 🚀 Instalación

1. **Clonar el repositorio**
```bash
git clone https://github.com/nachomf18/svelte-gestor-tareas.git
```

2. **Instalar dependencias**
```bash
npm install
```

3. **Iniciar el servidor de desarrollo**
```bash
npm run dev
```

La aplicación estará disponible en `http://localhost:5173` (o el puerto que indique Vite)

## 🏗️ Compilar para producción

Para crear una versión optimizada lista para despliegue:

```bash
npm run build
```

## 📖 Uso

1. **Añadir una tarea**: Escribe el nombre de la tarea en el campo de entrada y haz clic en "Añadir tarea"
2. **Cambiar prioridad**: Usa los botones ⬆️ y ⬇️ para mover tareas hacia arriba o abajo
3. **Completar tarea**: Haz clic en "Terminar" para marcar una tarea como completada
4. **Eliminar tarea**: En la sección de completadas, haz clic en "Eliminar" para borrar la tarea

## 🛠️ Tecnologías utilizadas

- **Svelte** - Framework reactivo
- **Vite** - Herramienta de build rápida
- **JavaScript** - Lógica de la aplicación
- **CSS** - Estilos y animaciones

## 📁 Estructura del proyecto

```
.
├── src/
│   ├── routes/
│   │   ├── +page.svelte      # Página principal
│   │   └── +layout.svelte    # Layout global
│   ├── app.html              # HTML base
│   └── lib/                  # Librerías compartidas
├── static/                   # Archivos estáticos
├── package.json
├── vite.config.js
├── svelte.config.js
└── README.md
```

## 👤 Autor

**Ignacio Martín Fernández**
- GitHub: [@nachomf18](https://github.com/nachomf18)