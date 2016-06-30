# Cómo calendarizar respaldos en Pantheon.io


##Respaldos en Pantheon.io
Para programar los respaldos de código, archivos y base de datos en Pantheon se requiere que el sitio esté asignado a un plan (basic, professional, business u otro).

Para activar los respaldos en un sitio hospedado en Pantheon.io:
- Ingresar al dashboard de sitios de Pantheon
- Ingresar al sitio específico al que se quieren programar los respaldos
- En el tab ambiente correspondiente, seleccionar la pestaña vertical llamada “Backups”
- Seleccionar lo siguiente:
    - Backup Log:
        - Backup log for the Live environment: Keep backup for 6 months.
    - Backup Schedule:
        - Marcar enabled
        - Choose the weekly backup day: seleccionar un día. Puede ser viernes.
        - Aplicar los cambios con “Update weekly backup schedule”
