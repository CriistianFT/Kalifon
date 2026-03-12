- Acceso Administrativo mediante URL oculta
- Autenticación requerida para todos los roles internos (Dueño, Empleado, Admin, Contador). (mediante tokens JWT).
- Las contraseñas en la base de datos deben almacenarse hasheadas (Encriptadas de forma irreversible).
- El backend validara los permisos de cada rol antes de ejecutar cualquier accion (ej. si un token de "Empleado" intenta borrar un usuario, el servidor lo bloquea).

password Project ADMA by SupaBase: OGgIKPedl0RVU9ub
admaProject54@._ GH