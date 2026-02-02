# Guardería para empleados (Tarea 14)

Este módulo de Odoo implementa la **gestión de una guardería para los hijos de empleados** como parte de la Tarea 14.

## Descripción funcional

- Permite registrar servicios de guardería asociados a:
  - Un empleado (`hr.employee`).
  - Un evento/actividad (`event.event`).
- Define un modelo principal `modulotarea14.guarderia` que **hereda** de:
  - `hr.employee` (a través de `employee_id`).
  - `event.event` (a través de `event_id`).
- Gestiona información específica de la guardería:
  - Descripción del servicio.
  - Rango de edad de los niños (0-2, 3-5, 6-8, 9-11 años).

## Modelo principal

Modelo: `modulotarea14.guarderia`

Campos principales:

- `employee_id`: relación Many2one con `hr.employee` (empleado solicitante).
- `event_id`: relación Many2one con `event.event` (actividad/evento asociado a la guardería).
- `descripcion`: texto libre con la descripción del servicio de guardería.
- `rango_edad`: selección con los rangos de edad disponibles.

El modelo utiliza `_inherits` para reutilizar campos de empleado y de evento (por ejemplo, `job_title`, `work_email`, `date_begin`, `date_end`).

## Vistas

Archivo: `views/guarderia_views.xml`

- Vista formulario de guardería con:
  - Empleado.
  - Evento.
  - Rango de edad.
  - Descripción.
  - Sección "Datos del empleado" (solo lectura): puesto de trabajo, correo.
  - Sección "Datos del evento" (solo lectura): fechas de inicio y fin.
- Vista lista con columnas básicas:
  - Nombre (heredado del evento/empleado).
  - Rango de edad.
  - Fecha de inicio.
- Acción y menús:
  - Acción `action_guarderia` sobre el modelo `modulotarea14.guarderia`.
  - Menú raíz "Guardería" bajo el menú de `hr_custom`.
  - Submenú "Servicios" que abre la lista/formulario de guardería.

## Seguridad

Archivos:

- `security/security.xml`:
  - Grupo `Usuario Guardería`.
  - Grupo `Responsable Guardería` que implica al grupo de usuario.
- `security/ir.model.access.csv`:
  - Permisos de acceso para el modelo `modulotarea14.guarderia`:
    - Usuarios de guardería: solo lectura.
    - Responsables: lectura, creación, escritura y borrado.

## Dependencias del módulo

En `__manifest__.py` se definen las dependencias:

- `hr`
- `event`
- `hr_custom` (módulo personalizado donde cuelga el menú padre `hr_custom.menu_hr_custom_root`).

Asegúrate de tener estos módulos instalados antes de instalar este módulo de guardería.

## Instalación

1. Copiar la carpeta `Tarea14` dentro de la ruta de addons de Odoo.
2. Reiniciar el servidor de Odoo.
3. Activar el modo desarrollador.
4. Actualizar la lista de aplicaciones.
5. Buscar "moduloTarea14" o "Guardería" e instalar el módulo.

## Uso

1. Entrar al menú **Recursos Humanos → Guardería → Servicios** (según configuración del módulo `hr_custom`).
2. Crear un nuevo registro de guardería:
   - Seleccionar el empleado.
   - Seleccionar el evento.
   - Indicar el rango de edad.
   - Rellenar la descripción.
3. Guardar el registro.

Los usuarios asignados al grupo **Usuario Guardería** podrán consultar los servicios de guardería.
Los usuarios en el grupo **Responsable Guardería** podrán crear, modificar y eliminar registros.

## Autores

- Autor técnico del módulo: `BlueMichelle` (según manifest).
- Adaptaciones y prácticas realizadas por: Michelle para la Tarea 14 (Guardería).

  ## Imagen
  

