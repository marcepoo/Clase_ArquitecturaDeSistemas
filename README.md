# Aplicación de Gestión de Tareas - Comparativa de Arquitecturas

Este proyecto explora distintas formas de construir una aplicación de gestión de tareas personales, utilizando tres enfoques arquitectónicos distintos para ilustrar sus ventajas y desafíos:

1. **Arquitectura Monolítica**: Toda la lógica reunida en un solo archivo.
2. **Modelo Vista Controlador (MVC)**: Separación estructurada entre datos, lógica y presentación.
3. **Microservicios**: Componentes autónomos que colaboran mediante comunicación HTTP.

---

## Visión General

Se trata de una aplicación en consola que permite al usuario gestionar sus tareas. Entre sus funcionalidades se incluyen:
- **Creación de tareas**
- **Listado de tareas**
- **Marcar tareas como realizadas**
- **Eliminación de tareas**

Cada una de las implementaciones utiliza el mismo conjunto de funcionalidades, pero organizadas bajo distintas estructuras para resaltar sus diferencias.

---

## Enfoque 1: Arquitectura Monolítica

En este modelo, toda la aplicación está contenida en un solo archivo: desde la gestión de datos, la lógica del programa y la interfaz del usuario, hasta el almacenamiento.

**Aspectos clave:**
- Código centralizado en un único archivo.
- Fácil de leer y rápido para desarrollos simples.
- Puede dificultar el mantenimiento conforme el proyecto crece.

**Estructura del proyecto:**
```
monolitic/
│
├── main.py                # Punto de entrada que ejecuta la app   
│     
└── task.json              # Para almacenar las tareas
```
---

## Enfoque 2: MVC (Modelo-Vista-Controlador)

Este enfoque divide la aplicación en componentes bien definidos:
- **Modelo**: Representa los datos (tareas).
- **Vista**: Interacción con el usuario mediante consola.
- **Controlador**: Coordina el flujo entre modelo y vista.

### Ventajas:
- Código más organizado y reutilizable.
- Facilita el mantenimiento y la escalabilidad.
- Ideal para separar responsabilidades de forma clara.

### Estructura de carpetas:
```
mvc/
│
├── controllers/        
│   └── task_controller.py # Gestiona la lógica de negocio
│
├── models/        
│   └── task.py            # Define la clase de la tarea
│
├── views/            
│   └── task_view.py       # Muestra la interfaz de usuario
│
├── storage/        
│   └── task_repo.py       # Gestiona la persistencia de datos
│   └── task.json          # Almacena las tareas
│
├── assets/                 
│   └── logo.png
│
├── main.py                # Punto de entrada que ejecuta la app
├── requirements.txt        
└── README.md   
```
---

## Enfoque 3: Arquitectura Basada en Microservicios

Este diseño divide la lógica en servicios autónomos que se comunican entre sí usando peticiones HTTP. Cada servicio tiene una responsabilidad específica y opera de manera independiente.

### Características principales:
- Servicios ejecutándose en procesos separados.
- Aislamiento funcional por componente.
- Ideal para despliegues con contenedores (ej. Docker).
- Permite mayor flexibilidad y escalabilidad.

### Servicios disponibles:
| Servicio             | Función principal                                           | Puerto |
|----------------------|-------------------------------------------------------------|--------|
| **Client**           | Interfaz web (formulario + lista de tareas)                | 5000   |
| **Task Service**     | Lógica de negocio (crear, completar, eliminar tareas)      | 5001   |
| **Storage Service**  | Persistencia en `tasks.json`                               | 5002   |
| **Logging Service**  | Registro de eventos (log.txt)                              | 5003   |
| **Notification Service** | (Opcional) Notificaciones al completar tareas         | —      |

### Flujo de Interacción:
```
Usuario → Client (5000) → Task Service (5001)
         ↘                     ↓
       Logging (5003) ← Storage (5002)
```
### Estructura del proyecto:
```
microservice/
│
├── client/                    # Interfaz web que interactúa con el Task Service
│   └── app.py
│
├── services/
│   ├── logging_service/       # Registra acciones (crear, eliminar, completar)
│   │   └── app.py
│   │   └── log.txt
│
│   ├── storage_service/       # Gestiona el archivo tasks.json
│   │   └── app.py
│   │   └── tasks.json
│
│   ├── task_service/          # Lógica de negocio de las tareas
│   │   └── app.py
│
│   └── notification_service/  # (Opcional) Para notificaciones futuras
│       └── app.py
```

### Cómo ejecutar el sistema

1. Abre 4 terminales diferentes y en cada una ejecuta:

```bash
# Terminal 1
cd services/task_service
python app.py

# Terminal 2
cd services/storage_service
python app.py

# Terminal 3
cd services/logging_service
python app.py

# Terminal 4
cd client
python app.py
2. Accede a http://localhost:5000 desde el navegador para interactuar.

3. Puedes revisar los archivos tasks.json (almacenamiento) y log.txt (historial) para ver el estado de las tareas y el registro de eventos.
