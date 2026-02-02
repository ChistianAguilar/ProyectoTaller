# Taller Mecánico – ProyectoTaller

Aplicación web construida con Django y Django REST Framework para demostrar, como trabajo práctico de carrera, el diseño e implementación completa de un sistema de gestión para talleres automotrices. Permite registrar clientes, vehículos y servicios, programar citas, asignar reparaciones, coordinar tareas del equipo y generar reportes operativos. La interfaz expone paneles diferenciados por rol (jefe, encargado, mecánico), mientras que la API REST abre la puerta a integraciones futuras.@taller_mecanico/gestion/views.py#511-914 @taller_mecanico/gestion/urls.py#30-80

## ¿Qué hace el sistema?
- Centraliza la **gestión de clientes, vehículos y servicios** conectando cada reparación con la información necesaria.@taller_mecanico/gestion/models.py#63-212
- Orquesta el **flujo operativo diario**: recepción del vehículo, asignación a un mecánico, seguimiento de estados y entrega final con notas y reportes.@taller_mecanico/gestion/views.py#511-747
- Proporciona **dashboards y métricas específicas** para cada rol del taller (KPIs, tareas, citas, reparaciones pendientes, ingresos, etc.).@taller_mecanico/gestion/views.py#511-914
- Incluye **agenda de citas y reportes financieros** para planificar la capacidad del taller y analizar ingresos por servicio.@taller_mecanico/gestion/views.py#775-914

## Objetivos principales
1. Permitir que el personal gestione citas y reparaciones de principio a fin (crear, reasignar, actualizar estados, cerrar).
2. Mantener un registro actualizado de clientes, vehículos y servicios solicitados.
3. Facilitar la asignación de turnos y la distribución de carga de trabajo del equipo.
4. Exponer una API REST básica que sirva como punto de integración para bots, apps móviles u otros sistemas.

## Características destacadas
- **Gestión integral** de clientes, empleados, servicios, vehículos y reparaciones con estados detallados y notas enriquecidas.@taller_mecanico/gestion/models.py#63-212
- **Dashboards según rol** (jefe, encargado, mecánico) con KPIs, tareas, reparaciones asignadas y paneles filtrados.@taller_mecanico/gestion/views.py#511-914
- **Agenda y reportes** para visualizar citas próximas, ingresos mensuales y estadísticas clave.@taller_mecanico/gestion/views.py#775-914
- **Permisos centralizados** mediante funciones de ayuda inyectadas como contexto global en todas las plantillas, simplificando la lógica de acceso.@taller_mecanico/taller_mecanico/settings.py#141-158
- **Exportación a Excel** y otros reportes operativos soportados por librerías como `openpyxl` y `xlsxwriter`.@requirements.txt#1-15
- **API REST** alineada con las vistas/serializers de la app `gestion`, lista para integraciones.@taller_mecanico/gestion/urls.py#30-80

## Tecnologías
- Python 3.11 (proyecto probado en esta versión; funciona desde 3.10+).
- Django 5.2.8 como framework principal.@requirements.txt#1-15
- Django REST Framework 3.16.1 para las APIs.@requirements.txt#1-15
- SQLite como base de datos por defecto (MySQL/MariaDB opcional).
- HTML, CSS y JavaScript con plantillas Django para la UI.
- Librerías adicionales: `openpyxl`, `xlsxwriter` (reportes), `python-telegram-bot` (integraciones futuras).@requirements.txt#1-15

## Requisitos previos
Asegúrate de disponer de:
```
Git >= 2.0
Python 3.10+
pip
virtualenv (opcional, pero recomendado)
```

## Instalación y ejecución (desarrollo)
1. **Clona el repositorio (o copia el proyecto en tu entorno):**
   ```bash
   git clone https://github.com/ChistianAguilar/ProyectoTaller.git
   cd ProyectoTaller
   ```
2. **Crea y activa un entorno virtual:**
   ```bash
   python -m venv .venv
   # Linux / macOS
   source .venv/bin/activate
   # Windows (PowerShell)
   .venv\Scripts\Activate.ps1
   # Windows (cmd)
   .venv\Scripts\activate.bat
   ```
3. **Instala dependencias base:**
   ```bash
   python -m pip install -U pip
   pip install -r requirements.txt
   ```
4. **(Opcional) instala dependencias para MySQL/MariaDB:**
   ```bash
   pip install -r requirements-optional.txt
   ```
5. **Configura variables de entorno (opcional, recomendado para prod):**
   Crea un archivo `.env` o exporta variables como:
   ```
   SECRET_KEY=tu_clave_secreta
   DEBUG=True
   ALLOWED_HOSTS=localhost,127.0.0.1
   DATABASE_URL=sqlite:///db.sqlite3  # O cadena para MySQL/Postgres
   ```
6. **Aplica migraciones y crea un superusuario (opcional):**
   ```bash
   cd taller_mecanico
   python manage.py migrate
   python manage.py createsuperuser
   ```
7. **Inicia el servidor de desarrollo:**
   ```bash
   python manage.py runserver
   ```
   Visita http://127.0.0.1:8000/ y el panel admin en http://127.0.0.1:8000/admin/

## Configuración de base de datos
El proyecto viene configurado con SQLite:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
Para usar MySQL/MariaDB actualiza `taller_mecanico/settings.py` con `django.db.backends.mysql` y las credenciales correspondientes, asegurándote de tener instaladas las librerías nativas (`libmariadb-dev`, `mariadb-libs`, etc.).@taller_mecanico/README.md#26-85

## Datos iniciales y usuarios de prueba
Después de migrar la base de datos, puedes cargar usuarios demo:
```bash
python manage.py setup_test_users
# o para recrearlos
python manage.py setup_test_users --reset
```
Se crean automáticamente:
- Jefe: `jefe` / `jefe123`
- Encargado: `encargado` / `encargado123`
- Mecánico: `mecanico` / `mecanico123`

También puedes usar `python manage.py crear_usuario ...` para generar perfiles personalizados con roles específicos.@taller_mecanico/README.md#98-125

## Flujo de trabajo típico
1. Inicia sesión en `/login/`.@taller_mecanico/gestion/views.py#103-140
2. El sistema redirige al dashboard correspondiente al rol (jefe, encargado, mecánico).@taller_mecanico/gestion/views.py#103-140
3. Desde el panel puedes registrar clientes, crear vehículos, asignar reparaciones, actualizar estados, registrar informes y monitorear KPIs diarios.@taller_mecanico/gestion/views.py#511-914
4. El módulo de agenda permite coordinar citas; los reportes muestran ingresos y métricas recientes.@taller_mecanico/gestion/views.py#775-914

## API REST (ejemplos)
La app `gestion` expone endpoints basados en Django REST Framework. Ajusta prefijos según `taller_mecanico/urls.py`. Ejemplos:
- `GET/POST /api/clientes/` – lista o crea clientes.
- `GET/PUT/PATCH/DELETE /api/clientes/{id}/` – CRUD sobre clientes existentes.
- `GET/POST /api/vehiculos/` – gestiona vehículos asociados.
- `GET/POST /api/citas/` – crea y lista citas con filtros por fecha, cliente o estado.
- `GET/POST /api/reparaciones/` – registra reparaciones y permite cambiar estados.
Revisa `gestion/urls.py` y los serializers para rutas concretas y permisos.@taller_mecanico/gestion/urls.py#30-80

## Roles y permisos
- **Jefe:** control total de clientes, empleados, servicios y reportes financieros.
- **Encargado:** alta/baja/edición de clientes, agenda de citas y visualización de catálogos.
- **Mecánico:** acceso a tareas asignadas, toma de reparaciones pendientes y registro de avances.
La lógica de permisos se centraliza en funciones (`es_jefe`, `es_encargado`, `es_mecanico`, etc.) reutilizadas tanto en vistas como en plantillas mediante el contexto global de Django.@taller_mecanico/gestion/views.py#40-140 @taller_mecanico/taller_mecanico/settings.py#141-158

## Comandos útiles
| Propósito | Comando |
|-----------|---------|
| Crear superusuario | `python manage.py createsuperuser` |
| Ejecutar pruebas | `python manage.py test` |
| Poblar usuarios demo | `python manage.py setup_test_users [--reset]` |
| Exportar ingresos a Excel | Usar la UI en `/reportes/ingresos/` (genera archivo usando DRF + openpyxl).@taller_mecanico/gestion/urls.py#58-67

## Estructura del repositorio
```
/
├── taller_mecanico/
│   ├── gestion/             # App principal (modelos, vistas, serializers, urls, templates específicos)
│   └── taller_mecanico/     # Configuración global: settings, urls, asgi/wsgi
├── requirements.txt         # Dependencias base
├── requirements-optional.txt# Dependencias opcionales (MySQL, extras)
├── README.md                # Este documento
├── ieee830.md               # Especificación de requisitos
├── ROLES_USUARIOS.md        # Detalle extendido de perfiles
└── docs/*.md                # Otros documentos de soporte (cambios, análisis, etc.)
```

## Despliegue (recomendaciones)
1. Configura `DEBUG=False`, `ALLOWED_HOSTS` y credenciales sensibles via variables de entorno.
2. Usa una base de datos robusta (PostgreSQL o MySQL) y ejecuta migraciones automáticas en el pipeline de despliegue.
3. Sirve archivos estáticos usando WhiteNoise o a través de Nginx/Apache.
4. Ejecuta la app con Gunicorn/Uvicorn detrás de un proxy inverso.
5. Implementa CI/CD, backups de la base de datos y monitoreo básico de logs.

## Documentación complementaria
- `CAMBIOS_DASHBOARD_MECANICO.txt`: historial de iteraciones recientes del dashboard mecánico.
- `RESUMEN_CAMBIOS_IMPLEMENTADOS.md`: registro de hitos funcionales.
- `ROLES_USUARIOS.md`: definiciones extendidas de cada perfil.
- `ieee830.md`: plantilla IEEE 830 de requisitos del sistema.

## Contribuir
Si quieres colaborar:
```bash
git checkout -b feature/mi-cambio
# realiza ajustes
git commit -m "Descripción del cambio"
git push origin feature/mi-cambio
```
Abre un Pull Request describiendo el cambio, capturas si aplica y pasos para probarlo.
