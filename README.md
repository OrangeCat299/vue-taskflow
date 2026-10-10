# TaskFlow — Gestor de tareas con Vue.js

Aplicación web responsive para gestionar tareas personales, académicas y laborales. Permite añadir tareas, asignar prioridad y categoría, marcarlas como completadas, eliminarlas, buscar, filtrar y revisar el porcentaje de avance.

## Enlace público de producción
https://vue-taskflow.vercel.app

> Reemplaza el marcador anterior por tu URL real antes de entregar la actividad. El proyecto no está desplegado por defecto.

## Tecnologías

- Vue.js 3 (Composition API)
- Vite 7
- JavaScript, HTML y CSS
- `localStorage` del navegador, sin backend

## Requisitos

- Node.js 22.18+ o 24.12+
- npm

## Ejecutar localmente

```bash
npm install
npm run dev
```

Abre la dirección local mostrada por Vite (normalmente `http://localhost:5173`).

## Compilar para producción

```bash
npm run build
npm run preview
```

El directorio final es `dist/`.

## Estructura del proyecto

```text
vue-taskflow/
├── public/
├── src/
│   ├── components/
│   │   ├── TaskForm.vue     # Formulario y eventos para crear tareas
│   │   └── TaskItem.vue     # Tarea individual y eventos para cambiar/eliminar
│   ├── App.vue              # Estado compartido, filtros y progreso
│   ├── main.js              # Punto de entrada
│   └── style.css            # Diseño adaptable a celular y escritorio
├── index.html
├── vite.config.js
├── package.json
└── README.md
```

## Conceptos de Vue implementados

| Concepto | Implementación |
| --- | --- |
| Reactividad | `ref()` para tareas, búsqueda y filtro actual |
| Estado derivado | `computed()` para contadores, progreso y resultados filtrados |
| Componentes | `TaskForm.vue` y `TaskItem.vue` |
| Props | `TaskItem` recibe el objeto `task` |
| Eventos | `@submit`, `@click`, `@change` y `emit()` hacia el componente padre |
| Directivas | `v-model`, `v-for`, `v-if`, `:class` y `:style` |
| Persistencia | `watch()` guarda tareas en `localStorage` |

**Nota sobre almacenamiento:** los datos solo se conservan en el navegador y dispositivo donde se crean. No hay sincronización entre dispositivos ni cuentas.

## Subir el código a GitHub

1. Crea un repositorio **público** vacío llamado `vue-taskflow` en GitHub.
2. Abre la terminal dentro de la carpeta del proyecto y ejecuta:

```bash
git init
git add .
git commit -m "Proyecto Vue TaskFlow"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/vue-taskflow.git
git push -u origin main
```

Reemplaza `TU-USUARIO` por tu usuario real. Si GitHub solicita autenticación, inicia sesión por el método recomendado (navegador/GitHub CLI o token); no escribas tu contraseña en la URL.

## Publicar gratis con Vercel

1. Entra a [vercel.com](https://vercel.com/) y accede con tu cuenta.
2. Selecciona **Add New → Project** e importa el repositorio de GitHub.
3. Comprueba la configuración: framework `Vite`, build command `npm run build`, output directory `dist`.
4. Presiona **Deploy** y espera la URL pública.
5. Prueba la URL en una ventana de incógnito.
6. Actualiza el enlace de producción en este README y vuelve a subir el cambio a GitHub (`git add README.md`, `git commit -m "Agregar URL de producción"`, `git push`).

## Autor

Proyecto académico de Programación Web. Agrega aquí tu nombre y grupo antes de entregar.
