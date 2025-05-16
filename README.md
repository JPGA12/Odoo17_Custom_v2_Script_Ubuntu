# Script de Instalación de Odoo 17 con FECOL y Enterprise (Sin Módulos ACS)

Este script automatiza la instalación de Odoo 17 en sistemas Ubuntu, incluyendo la configuración completa del sistema y la clonación de los repositorios FECOL y Enterprise, eliminando específicamente los módulos ACS.

## Requisitos Previos

- Sistema operativo Ubuntu (preferiblemente Ubuntu 22.04 LTS)
- Privilegios de administrador (sudo)
- Acceso a Internet

## Características Principales

- Instalación completa de Odoo 17 desde GitHub
- Configuración automática de PostgreSQL
- Instalación de todas las dependencias necesarias
- Clonación de repositorios personalizados (FECOL y Enterprise)
- **Eliminación automática de todos los módulos ACS** del repositorio FECOL
- Configuración de entorno virtual Python
- Creación y configuración del servicio systemd

## Proceso de Instalación Detallado

El script realiza las siguientes operaciones:

1. **Actualización del sistema**:
   - Actualiza los repositorios
   - Actualiza los paquetes instalados

2. **Instalación de dependencias**:
   - Node.js 18.x (con eliminación previa de versiones existentes)
   - PostgreSQL y herramientas de desarrollo
   - Dependencias Python necesarias
   - Herramientas de compilación y bibliotecas del sistema

3. **Configuración de PostgreSQL**:
   - Verifica la instalación
   - Asegura que el servicio esté funcionando

4. **Creación de usuario para Odoo**:
   - Crea usuario de sistema 'odoo17'
   - Configura usuario de PostgreSQL con permisos adecuados

5. **Instalación de wkhtmltopdf**:
   - Descarga e instala wkhtmltopdf 0.12.6.1 para generar reportes PDF

6. **Clonación de repositorios**:
   - Odoo 17.0 oficial desde GitHub
   - Repositorio FECOL (para facturación electrónica colombiana)
   - Repositorio Enterprise de Odoo

7. **Procesamiento específico de repositorios**:
   - **Eliminación de todos los módulos ACS** del repositorio FECOL
   - Configuración de permisos adecuados

8. **Configuración del entorno Python**:
   - Creación de entorno virtual
   - Instalación de dependencias Python específicas
   - Manejo especial de greenlet y gevent para compatibilidad

9. **Configuración del servicio**:
   - Creación del archivo de configuración Odoo
   - Configuración del servicio systemd
   - Inicio y verificación del servicio

## Uso

1. Descargue el script de instalación
2. Otorgue permisos de ejecución:
   ```
   chmod +x install_odoo17_without_acs.sh
   ```
3. Ejecute el script con privilegios de administrador:
   ```
   sudo ./install_odoo17_without_acs.sh
   ```

## ¿Por qué sin módulos ACS?

Este script está específicamente diseñado para entornos donde los módulos ACS no son requeridos o pueden causar conflictos. Los módulos ACS (Advanced Customer Solutions) son un conjunto de soluciones adicionales que pueden tener licencias específicas o dependencias particulares que no son necesarias en todos los entornos de implementación.

## Acceso Post-Instalación

Una vez completada la instalación:
- Acceda a Odoo desde su navegador: http://localhost:8069
- Usuario por defecto: admin
- Contraseña: La especificada como admin_passwd en el archivo de configuración

## Directorios Importantes

- **Instalación de Odoo**: `/opt/odoo17/odoo`
- **Módulos adicionales** (sin ACS): `/opt/odoo17/odoo/extra-addons`
- **Módulos enterprise**: `/opt/odoo17/odoo/enterprise`
- **Archivo de configuración**: `/etc/odoo17.conf`

## Administración del Servicio

```bash
# Iniciar Odoo
sudo systemctl start odoo17

# Detener Odoo
sudo systemctl stop odoo17

# Reiniciar Odoo
sudo systemctl restart odoo17

# Comprobar estado
sudo systemctl status odoo17

# Ver logs
sudo journalctl -u odoo17 -f
```

## Consideraciones de Seguridad

- **IMPORTANTE**: Cambie la contraseña predeterminada `admin_password` en `/etc/odoo17.conf`
- Los tokens de acceso a GitHub están visibles en el script y deberían ser cambiados o eliminados después de la instalación
- Considere implementar HTTPS mediante un proxy inverso para entornos de producción

## Resolución de Problemas

Si encuentra problemas durante la instalación:

1. **Problema con la eliminación de módulos ACS**:
   - Verifique manualmente que no haya carpetas con "acs" en el nombre en `/opt/odoo17/odoo/extra-addons`
   - Si persisten, elimínelas manualmente: `sudo find /opt/odoo17/odoo/extra-addons -type d -name "*acs*" -exec rm -rf {} +`

2. **Problemas con los servicios**:
   - El script intenta detener servicios anteriores que podrían estar en conflicto
   - Verifique que no haya instancias de Odoo en ejecución: `ps aux | grep odoo`

3. **Errores de dependencias Python**:
   - Revise los logs: `sudo journalctl -u odoo17`
   - Verifique el archivo requirements_temp.txt y las instalaciones manuales de greenlet y gevent

## Personalización

Este script puede modificarse para:
- Cambiar las versiones de las dependencias específicas
- Modificar los puertos de Odoo (por defecto: 8069, 8072)
- Agregar repositorios adicionales
- Cambiar el número de workers según los recursos del servidor

---

**Nota**: Este script está optimizado para eliminar todos los módulos ACS. Si necesita una instalación con los módulos ACS incluidos, utilice una versión diferente del script de instalación.