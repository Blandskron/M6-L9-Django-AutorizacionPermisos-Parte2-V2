# User API Project

Este proyecto es una API desarrollada con **Django**, **Django REST Framework** (DRF) y **drf-spectacular** para la generación de documentación automática. Permite crear, leer, actualizar y eliminar usuarios del sistema, además de gestionar un perfil asociado con una foto. También incluye endpoints para consultar la documentación en formatos Swagger y Redoc.

## Características

- **CRUD completo de usuarios**: Alta, baja, modificación y listado de usuarios.
- **Perfil de usuario con foto**: Cada usuario tiene un perfil asociado con una imagen que puede ser redimensionada.
- **Documentación automática**: Utilizando `drf-spectacular`, se generan esquemas OpenAPI y se ofrecen vistas Swagger UI y Redoc.
- **Arquitectura escalable**: Separación de apps (`user_api` y `docs`) para mantener el código organizado.

## Requisitos

- Python 3.7+
- Django 3.2+ (o la versión que se haya utilizado en el proyecto)
- Django REST Framework
- drf-spectacular
- Pillow para manejo de imágenes

Asegúrate de contar con un entorno virtual y de haber instalado todos los paquetes requeridos.

## Instalación

1. Clona el repositorio:

   ```bash
   git clone https://github.com/tu-usuario/tu-repo.git
   cd tu-repo
   ```

2. Crea y activa un entorno virtual (opcional pero recomendado):

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # Linux/macOS
   venv\Scripts\activate     # Windows
   ```

3. Instala las dependencias:

   ```bash
   pip install -r requirements.txt
   ```

   Asegúrate de que en `requirements.txt` estén definidas las librerías:
   - Django
   - djangorestframework
   - drf-spectacular
   - Pillow

4. Realiza las migraciones:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

5. Inicia el servidor de desarrollo:

   ```bash
   python manage.py runserver
   ```

## Uso

### Endpoints Principales

- **CRUD de usuarios** (`user_api/users/`):
  - `GET /user_api/users/` – Listar todos los usuarios
  - `POST /user_api/users/` – Crear un nuevo usuario (requiere `username`, `password`, y opcional `profile.photo`)
  - `GET /user_api/users/{id}/` – Ver detalle de un usuario específico
  - `PUT /user_api/users/{id}/` – Actualizar un usuario completo
  - `PATCH /user_api/users/{id}/` – Actualizar parcialmente un usuario (por ejemplo, solo la foto)
  - `DELETE /user_api/users/{id}/` – Eliminar un usuario

Los datos del perfil se envían como un sub-objeto `profile`, por ejemplo:

```json
{
  "username": "nuevo_user",
  "password": "passwordsegura",
  "email": "email@ejemplo.com",
  "profile": {
    "photo": "archivo adjunto (multipart/form-data)"
  }
}
```

### Documentación

La documentación se encuentra disponible en las siguientes rutas:

- **Schema JSON:**  
  `http://localhost:8000/docs/schema/`

- **Swagger UI:**  
  `http://localhost:8000/docs/swagger/`

- **Redoc:**  
  `http://localhost:8000/docs/redoc/`

Estas vistas permiten navegar por la documentación generada dinámicamente y consultar información sobre todos los endpoints, modelos y parámetros soportados.

### Manejo de Imágenes

Las imágenes subidas se almacenan en la carpeta configurada en `MEDIA_ROOT` y son accesibles a través de `MEDIA_URL`. En modo desarrollo, se sirven directamente con el `runserver`. En producción se recomienda un servidor estático o almacenamiento en la nube.

La aplicación redimensiona las imágenes de perfil a un máximo de 300x300 píxeles si exceden esas dimensiones.

## Estructura del Proyecto

```
myproject/
│
├─ myproject/
│  ├─ settings.py
│  ├─ urls.py
│  └─ ...
│
├─ user_api/
│  ├─ models.py          # UserProfile y lógica de imagen
│  ├─ serializers.py     # UserSerializer, UserProfileSerializer
│  ├─ views.py           # UserViewSet con CRUD de usuarios
│  ├─ urls.py            # Rutas basadas en router para el UserViewSet
│  ├─ signals.py         # Señales para crear y guardar perfiles
│  ├─ apps.py
│  └─ ...
│
├─ docs/
│  ├─ urls.py            # Rutas de esquema, swagger y redoc
│  └─ ...
│
├─ media/                # Carpeta para fotos de perfil (creada en runtime)
│
└─ requirements.txt
```

## Recomendaciones

- Ajustar permisos y autenticación según necesidades (por ejemplo, solo dueños pueden editar sus propios datos).
- Agregar validaciones extras en el serializador (unicidad de email, formato de usuario, etc.).
- Añadir tests unitarios e integrales para garantizar calidad y evitar regresiones.
- Configurar un almacenamiento de archivos externo para entornos productivos (S3, Azure Blob, GCP Storage, etc.).

## Licencia

Este proyecto se distribuye bajo la licencia [MIT](LICENSE).
