# ProyectoTaller

Descripción

ProyectoTaller es una aplicación web desarrollada con Django para la gestión de citas de clientes en talleres automotrices. El propósito de este proyecto es servir como trabajo práctico y apoyo para la defensa del proyecto de la carrera, demostrando diseño, implementación e integración de una solución informática completa para la gestión de un taller.

Objetivos principales

- Permitir a administradores y personal del taller gestionar citas de clientes (crear, modificar, cancelar).
- Mantener un registro de clientes, vehículos y servicios solicitados.
- Facilitar la asignación de turnos y la organización del taller.
- Proveer una interfaz web y una API REST básica para integraciones futuras.

Características

- Gestión de clientes (alta, edición, baja).
- Registro de vehículos asociados a clientes.
- Creación y gestión de citas (fecha, hora, servicio, mecánico asignado).
- Panel de administración (Django admin) para la gestión completa.
- API REST para operaciones básicas (Django REST Framework).
- Exportación básica a Excel (si aplica) y funcionalidades de reporte.

Tecnologías

- Lenguaje: Python 3.10+
- Framework backend: Django 5.2.8
- API: Django REST Framework 3.16.1
- Frontend: HTML / CSS / JavaScript (plantillas Django)
- Base de datos por defecto: SQLite (soporte opcional para MySQL/MariaDB)
- Dependencias principales: ver `requirements.txt`

Requisitos previos

Asegúrate de tener instaladas las herramientas necesarias:

```
Git >= 2.0
Python 3.10+
pip
virtualenv (opcional)
```

Instalación y ejecución (desarrollo)

1. Clona el repositorio:

```
git clone https://github.com/ChistianAguilar/ProyectoTaller.git
cd ProyectoTaller
```

2. Crea y activa un entorno virtual:

```
python -m venv venv
# Linux / macOS
source venv/bin/activate
# Windows (PowerShell)
venv\Scripts\Activate.ps1
# Windows (cmd)
venv\Scripts\activate.bat
```

3. Instala dependencias:

```
pip install -r requirements.txt
```

4. Configura variables de entorno (opcional)

Crea un archivo `.env` (o configura variables en el entorno) con al menos estas variables si usas base de datos externa o deseas ocultar la clave secreta:

```
SECRET_KEY=tu_clave_secreta
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1
DATABASE_URL=sqlite:///db.sqlite3  # o configuración para MySQL/Postgres
```

5. Aplica migraciones y crea usuario administrador:

```
python manage.py migrate
python manage.py createsuperuser
```

6. Ejecuta la aplicación en modo desarrollo:

```
python manage.py runserver
```

Accede a:
- Aplicación: http://127.0.0.1:8000
- Panel administrativo: http://127.0.0.1:8000/admin/

Base de datos

- Por defecto el proyecto usa SQLite (`db.sqlite3`).
- Para MySQL/MariaDB instala la dependencia opcional (por ejemplo `mysqlclient`) y configura `DATABASES` o `DATABASE_URL`. Revisa `requirements-optional.txt`.

Ejecución de pruebas

Ejecuta las pruebas automatizadas con:

```
python manage.py test
```

Estructura del repositorio (resumen)

- `taller_mecanico/` - Código principal del proyecto Django (apps, modelos, vistas, urls, etc.)
- `requirements.txt` - Dependencias principales (Django, DRF, etc.)
- `requirements-optional.txt` - Dependencias opcionales (MySQL, herramientas extra)
- `ieee830.md` - Especificación de requisitos
- `ROLES_USUARIOS.md` - Descripción de roles y permisos
- `README.md` - Este archivo

API (ejemplos)

El proyecto incluye una API REST básica usando Django REST Framework. Rutas de ejemplo (ajusta según `urls.py` del proyecto):

- Listar/crear clientes: GET/POST `/api/clientes/`
- Detalle/editar/eliminar cliente: GET/PUT/PATCH/DELETE `/api/clientes/{id}/`
- Listar/crear vehículos: GET/POST `/api/vehiculos/`
- Listar/crear citas: GET/POST `/api/citas/`
- Filtrar citas por fecha/cliente/estado: `/api/citas/?fecha=2026-02-01&cliente=3`

(Revisa `taller_mecanico/urls.py` y las vistas/serializers para rutas concretas.)

Despliegue (recomendaciones)

Para un entorno de producción:

- Desactiva `DEBUG` y configura `ALLOWED_HOSTS`.
- Usa una base de datos robusta (Postgres o MySQL).
- Sirve archivos estáticos con WhiteNoise o mediante Nginx.
- Usa un WSGI/ASGI server (Gunicorn/uvicorn).
- Configura variables sensibles vía entorno (SECRET_KEY, DB credentials).
- Considera usar un servicio de CI/CD y backups de base de datos.

Contribuir

Si quieres colaborar: crea un fork, abre una rama con tu feature/fix y realiza un Pull Request.

```
git checkout -b feature/mi-cambio
# realiza cambios
git commit -m "Descripción del cambio"
git push origin feature/mi-cambio
```
