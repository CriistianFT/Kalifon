---

mindmap-plugin: markdown

---


# 🏴‍☠️ Guía: Linux & Bandit (Niveles 0-15)


> [!abstract] Resumen
> Referencia técnica detallada. Cada comando incluye su tabla de "Flags" (parámetros) para entender exactamente qué hace cada opción.


---


## 🧠 1. Fundamentos Teóricos


### 1.1. Los Flujos Estándar (Standard Streams)


Vital para entender tuberías (`|`) y redirecciones.


| Stream | ID | Descripción | Símbolo |
| :--- | :---: | :--- | :---: |
| **STDIN** | 0 | Entrada Estándar | `<` |
| **STDOUT** | 1 | Salida Estándar | `>` |
| **STDERR** | 2 | Salida de Errores | `2>` |


> [!TIP] Truco
> `comando 2>/dev/null` envía los errores a la basura (limpia tu pantalla).


---


## 🛠️ 2. Diccionario de Comandos (Detallado)


### 2.1. Redes (Conexión y Datos) 🌐


#### `ssh` (Secure Shell)


El estándar para conexiones remotas cifradas.
**Sintaxis:** `ssh <flags> usuario@host`


| Flag | Descripción                                                               | Ejemplo                  |
| :--- | :------------------------------------------------------------------------ | :----------------------- |
| `-p` | **Port**. Especifica el puerto (Bandit usa 2220).                         | `ssh -p 2220 ...`        |
| `-i` | **Identity**. Usa un archivo de llave privada (Key) en vez de contraseña. | `ssh -i key.private ...` |
| `-l` | **Login**. Especifica el usuario (alternativa a user@host).               | `ssh -l bandit14 ...`    |
| `-v` | **Verbose**. Muestra detalles técnicos de la conexión (debug).            | `ssh -v bandit0@...`     |


#### `nc` (Netcat)


Lectura y escritura en conexiones de red (TCP/UDP).


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-l` | **Listen**. Escucha en un puerto (modo servidor). | `nc -l 1234` |
| `-p` | **Port**. Define el puerto de origen. | `nc -p 3000` |
| `-v` | **Verbose**. Muestra si la conexión fue exitosa. | `nc -v localhost 3000` |
| `-z` | **Zero-IO**. Escaneo de puertos (sin enviar datos). | `nc -zv localhost 1-100` |


#### `openssl` (Módulo s_client)


Para conexiones que requieren cifrado SSL/TLS (como HTTPS o servicios seguros).


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-connect             `| Define el host y puerto destino. | `-connect localhost:3001` |
| `-quiet             `| Oculta la información técnica del certificado (limpia la salida). | `openssl ... -quiet` |
| `-ign_eof             `| Ignora el fin de archivo (mantiene la conexión abierta). | `openssl ... -ign_eof` |


---


### 2.2. Exploración del Sistema 📂


#### `ls` (List)


Listar contenidos de directorios.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-a` | **All**. Muestra todo, incluidos ocultos (inician con `.`). | `ls -a` |
| `-l` | **Long**. Formato lista con detalles (permisos, dueño, tamaño). | `ls -l` |
| `-h` | **Human**. Tamaños en KB, MB, GB (legible). | `ls -lh` |
| `-t` | **Time**. Ordena por fecha de modificación (más nuevo arriba). | `ls -lt` |
| `-R` | **Recursive**. Lista también las subcarpetas. | `ls -R` |


#### `du` (Disk Usage)


Estima el espacio usado por archivos.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-h` | **Human**. Tamaños legibles (K, M, G). | `du -h` |
| `-a` | **All**. Muestra archivos individuales, no solo carpetas. | `du -a` |
| `-b` | **Bytes**. Muestra el tamaño exacto en bytes (Vital Nivel 5). | `du -b file.txt` |


#### `file`


Determina el tipo de archivo (ASCII, Data, Gzip, ELF) leyendo su cabecera.

- Usualmente se usa sin flags: `file data.txt`
- `-b`: **Brief**. No muestra el nombre del archivo, solo el tipo.

---


### 2.3. Búsqueda (`find`) 🔎


El comando gigante para filtrar inodos.


**Sintaxis:** `find <ruta> <condiciones>`


| Flag | Filtro por... | Ejemplo |
| :--- | :--- | :--- |
| `-name` | Nombre del archivo. | `-name "pass*"` |
| `-size` | Tamaño (`c`=bytes, `k`=KB, `M`=MB). | `-size 1033c` |
| `-user` | Propietario del archivo. | `-user bandit7` |
| `-group` | Grupo del archivo. | `-group bandit6` |
| `-perm` | Permisos (código octal). | `-perm 400` |
| `!` | **NOT** (Invierte la condición). | `! -executable` |
| `-exec` | Ejecuta un comando sobre lo encontrado. | `-exec file {} \;` |


---


### 2.4. Manipulación de Texto 📝


#### `grep`


Busca líneas que contengan un PATRÓN de texto.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-i` | **Ignore Case**. No distingue mayúsculas/minúsculas. | `grep -i "Pass"` |
| `-v` | **Invert**. Muestra las líneas que **NO** coinciden. | `grep -v "error"` |
| `-r` | **Recursive**. Busca dentro de carpetas. | `grep -r "pass" /etc/` |
| `-n` | **Number**. Muestra el número de línea. | `grep -n "user" file` |
| `-A <n>`| **After**. Muestra `n` líneas después de la coincidencia. | `grep -A 2 "título"` |


#### `sort`


Ordena las líneas de un archivo.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-n` | **Numeric**. Orden matemático (1, 2, 10) vs alfabético (1, 10, 2). | `sort -n` |
| `-r` | **Reverse**. Orden descendente (Z-A o 9-0). | `sort -r` |
| `-u` | **Unique**. Elimina duplicados (versión simple de uniq). | `sort -u` |


#### `uniq`


Filtra líneas repetidas. **Requiere estar ordenado (`sort`) antes.**


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-u` | **Unique**. Muestra SOLO las líneas que aparecen una vez. | `uniq -u` |
| `-d` | **Duplicate**. Muestra SOLO las líneas que se repiten. | `uniq -d` |
| `-c` | **Count**. Cuenta cuántas veces aparece cada línea. | `uniq -c` |


#### `base64`


Codificación de datos.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-d` | **Decode**. Decodifica de Base64 a texto plano. | `base64 -d file` |


---


### 2.5. Compresión y Hex (Nivel 12) 📦


#### `tar` (Tape Archive)


Empaquetar múltiples archivos.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-x` | **Extract**. Extraer contenido. | `tar -x ...` |
| `-v` | **Verbose**. Ver qué archivos salen. | `tar -xv ...` |
| `-f` | **File**. Especifica el nombre del archivo (siempre al final). | `tar -xvf data.tar` |


#### `xxd` (Hexdump)


Ver o manipular binarios en hexadecimal.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-r` | **Revert**. Convierte un volcado Hex de vuelta a Binario. | `xxd -r dump > bin` |


#### `gzip` / `bzip2`


Compresores estándar.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-d` | **Decompress**. Descomprimir archivo. | `gzip -d file.gz` |


---


### 2.6. Gestión de Archivos ⚙️


#### `rm` (Remove)


Borrar archivos.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-r` | **Recursive**. Borra carpetas y su contenido. | `rm -r carpeta/` |
| `-f` | **Force**. No pide confirmación (peligroso). | `rm -rf carpeta/` |


#### `mkdir` (Make Directory)


Crear carpetas.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-p` | **Parents**. Crea carpetas anidadas si no existen. | `mkdir -p a/b/c` |


#### `cp` (Copy)


Copiar archivos.


| Flag | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `-r` | **Recursive**. Necesario para copiar carpetas enteras. | `cp -r origen destino` |


---


## 🔐 3. Referencia de Permisos (`chmod`)


Para entender la columna de permisos en `ls -l`.


| Modo | Valor Octal | Significado |
| :---: | :---: | :--- |
| **r** | 4 | Read (Leer) |
| **w** | 2 | Write (Escribir) |
| **x** | 1 | Execute (Ejecutar) |


**Ejemplo:** `chmod 700 archivo`

- Dueño: 7 (4+2+1) -> Todo.
- Grupo: 0 -> Nada.
- Otros: 0 -> Nada.

---


## 🚀 4. Snippet: Flujo del Nivel 12


Copiar y pegar en la terminal cuando no tengas permisos en Home.


```bash
# 1. Crear entorno
mkdir /tmp/mi_espacio && cd /tmp/mi_espacio
# 2. Copiar
cp ~/data.txt .
# 3. Revertir Hex
xxd -r data.txt > data.bin
# 4. Loop de descompresión
file data.bin
mv data.bin data.gz  # (Renombrar según lo que diga file)
gzip -d data.gz

