# taskflow

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Learn Flutter](https://docs.flutter.dev/get-started/learn-flutter)
- [Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Flutter learning resources](https://docs.flutter.dev/reference/learning-resources)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

## Alcance de TaskFlow

Problema: necesito organizar actividades académicas y saber cuáles faltan.
Usuario objetivo: estudiante que planifica entregas semanales.

### Historias de usuario y criterios de aceptación

1. Como estudiante, quiero listar mis tareas para ver qué debo hacer.
   Criterio: la lista muestra título, prioridad y estado de cada tarea.
2. Como estudiante, quiero abrir una tarea para leer su descripción.
   Criterio: al tocar una tarjeta aparece el detalle de la tarea elegida.
3. Como estudiante, quiero crear una tarea para registrar un pendiente.
   Criterio: el formulario guarda una tarea válida y la lista la muestra.
4. Como estudiante, quiero editar una tarea para corregir sus datos.
   Criterio: los cambios guardados aparecen en lista y detalle.
5. Como estudiante, quiero eliminar una tarea terminada para limpiar la lista.
   Criterio: después de confirmar, la tarea desaparece de la lista.

### Modelo de datos

Task: id (entero único), title (texto), description (texto),
priority (Alta, Media o Baja), isCompleted (verdadero/falso),
userId (entero que identifica al usuario de prueba).

### Pantallas previstas

- Lista: tarjetas de tareas y acción para crear.
- Detalle: información completa y futuras acciones de editar o eliminar.
- Formulario: campos de título, descripción y prioridad.

### Requisitos no funcionales

- La navegación debe tener textos comprensibles.
- Un id inexistente debe mostrar un mensaje en vez de cerrar la app.

### Contrato de API que necesitaremos

- GET /tasks: listar tareas
- GET /tasks/:id: consultar detalle
- POST /tasks: crear tarea
- PATCH /tasks/:id: actualizar tarea
- DELETE /tasks/:id: eliminar tarea

## API de tareas

TaskFlow consulta tareas de JSONPlaceholder, una API pública de prueba.

- URL: `GET https://jsonplaceholder.typicode.com/todos?userId=2`
- Parámetro `userId`: filtra las tareas de un usuario (en el código: `TaskApiService.userId`).
- Campos que llegan de la API: `id`, `userId`, `title` y `completed`.
- Valores por defecto que completa TaskFlow: `description` ("Sin descripción (API)")
  y `priority` ("Sin prioridad (API)"); no vienen del servidor.
- Marcar una tarea como completada solo cambia la copia local en memoria;
  la API no guarda el cambio y al reiniciar vuelve al valor original.

  ## Estados de la lista

TaskFlow distingue cuatro estados al consultar las tareas:

- **Carga:** se muestra un indicador mientras la petición está pendiente.
- **Éxito:** se muestra una tarjeta por cada tarea recibida.
- **Vacío:** la API respondió bien pero sin tareas. Se muestra el mensaje y el botón "Actualizar".
  Cómo provocarlo: cambiar `userId` a `9999` en `TaskApiService` y reiniciar.
- **Error:** la petición falló. Se muestra el mensaje y el botón "Reintentar".
  Cómo provocarlo: poner la red en modo Offline (F12 → Network) y pulsar actualizar.

"Reintentar" vuelve a consultar la API sin reiniciar la app.