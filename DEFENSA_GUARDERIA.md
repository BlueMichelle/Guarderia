# Defensa de la Tarea 14 – Módulo Guardería para empleados

## 1. Objetivo del módulo

El objetivo del módulo es **gestionar servicios de guardería para los hijos de empleados** dentro de Odoo, permitiendo:

- Relacionar un servicio de guardería con un **empleado**.
- Relacionar ese servicio con un **evento/actividad** (por ejemplo, una jornada, taller o turno especial).
- Controlar el **rango de edad** de los niños que atiende el servicio.
- Definir **permisos de acceso** diferenciados entre usuarios que solo consultan y responsables que gestionan la información.

## 2. Diseño del modelo de datos

### Modelo principal: `modulotarea14.guarderia`

- `_name = 'modulotarea14.guarderia'`
- `_description = 'Guardería para empleados'`
- `_inherits = {'hr.employee': 'employee_id', 'event.event': 'event_id'}`

Con `_inherits` conseguimos **reutilizar campos** de `hr.employee` y de `event.event` sin duplicar estructura:

- Desde el empleado se reutilizan campos como `name`, `job_title`, `work_email`, etc.
- Desde el evento se reutilizan campos como `name`, `date_begin`, `date_end`, etc.

Campos específicos definidos:

- `employee_id`: `Many2one('hr.employee')`, obligatorio.
- `event_id`: `Many2one('event.event')`, obligatorio.
- `descripcion`: `Text`, descripción libre del servicio.
- `rango_edad`: `Selection` con los rangos:
  - `0_2` → 0–2 años
  - `3_5` → 3–5 años
  - `6_8` → 6–8 años
  - `9_11` → 9–11 años

Este diseño permite que un registro de guardería esté **vinculado a un empleado concreto y a un evento concreto**, y que además muestre información de ambos sin duplicar datos.

## 3. Vistas y menús

### Vista formulario

En `views/guarderia_views.xml` se define una vista formulario con:

- Campos principales:
  - `employee_id` (sin crear empleados nuevos desde la vista).
  - `event_id` (sin crear eventos nuevos desde la vista).
  - `rango_edad`.
  - `descripcion`.
- Grupo "Datos del empleado" (solo lectura):
  - `job_title`.
  - `work_email`.
- Grupo "Datos del evento" (solo lectura):
  - `date_begin`.
  - `date_end`.

Esto demuestra el uso de `_inherits`, ya que se muestran campos del empleado y del evento dentro del formulario de guardería.

### Vista lista

También se define una vista tipo lista:

- Columnas:
  - `name` (heredado).
  - `rango_edad`.
  - `date_begin`.

### Acción y menús

- Acción `ir.actions.act_window` llamada `action_guarderia` que abre el modelo `modulotarea14.guarderia` en modo `list,form`.
- Menús:
  - Menú raíz `menu_guarderia_root` ("Guardería") bajo el menú padre de `hr_custom`.
  - Submenú `menu_guarderia` ("Servicios") que lanza `action_guarderia`.

Con esto, el usuario puede navegar desde el menú de Recursos Humanos (según `hr_custom`) hasta la gestión de servicios de guardería.

## 4. Seguridad y control de acceso

En `security/security.xml` se crean dos grupos:

- `group_guarderia_user` – Usuario Guardería.
- `group_guarderia_manager` – Responsable Guardería (incluye al grupo de usuario).

En `security/ir.model.access.csv` se asignan permisos sobre el modelo `modulotarea14.guarderia`:

- Usuarios Guardería:
  - Lectura: Sí.
  - Escritura: No.
  - Creación: No.
  - Borrado: No.
- Responsables Guardería:
  - Lectura: Sí.
  - Escritura: Sí.
  - Creación: Sí.
  - Borrado: Sí.

Esto asegura que solo los responsables puedan gestionar los registros, mientras que otros usuarios de guardería solo consultan la información.

## 5. Dependencias y contexto en Odoo

En `__manifest__.py` se definen las dependencias:

- `hr`: módulo de Recursos Humanos.
- `event`: módulo de eventos.
- `hr_custom`: módulo personalizado que aporta el menú padre `hr_custom.menu_hr_custom_root`.

El módulo se instala sobre este contexto para integrarse con la estructura existente de Recursos Humanos y eventos.

## 6. Flujo de uso típico

1. Un responsable accede al menú **Guardería → Servicios**.
2. Crea un nuevo registro de guardería:
   - Selecciona el empleado trabajador.
   - Selecciona el evento (por ejemplo, actividad de guardería en una fecha concreta).
   - Define el rango de edad de los hijos que se están cubriendo.
   - Añade la descripción del servicio.
3. Guarda el registro.
4. Cualquier usuario del grupo Guardería puede consultar los registros creados.
5. Solo los responsables pueden modificar o eliminar los registros.

## 7. Justificación técnica

- Se ha usado `_inherits` en lugar de `_inherit` para **componer** el modelo de guardería con los modelos `hr.employee` y `event.event`, manteniendo sus registros y reutilizando campos sin duplicar tablas.
- Se ha separado correctamente la configuración de **vistas**, **seguridad** y **datos de acceso**, siguiendo la estructura típica de un módulo Odoo (`views/`, `security/`, `models/`).
- El manifest incluye dependencias mínimas necesarias para que el módulo funcione en un entorno realista de Recursos Humanos.

Este documento sirve como guía de defensa para explicar el diseño funcional y técnico del módulo durante la presentación de la Tarea 14.
