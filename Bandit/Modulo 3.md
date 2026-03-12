
---

Markdown

```
---
tags:
  - teoria
  - linux
  - modulo_3
  - compresion
  - redes
status: estudiando
---

# 📦 Módulo 3: Transformación, Compresión y Redes (Bandit 11-15)

En este tramo, los datos no están listos para leer. Tienes que "cocinarlos" (descomprimir, decodificar, transferir) antes de obtener la contraseña.

## 1. Traducción de Caracteres (Nivel 11)
A veces la seguridad es solo oscuridad.
* **ROT13:** Es un cifrado antiguo (usado por Julio César) que simplemente rota el abecedario 13 posiciones. A -> N, B -> O.
* **Comando `tr` (Translate):** Linux tiene una herramienta nativa para sustituir caracteres uno a uno.
    * `tr 'A-Za-z' 'N-ZA-Mn-za-m'`
    * Le dice: *"Cambia la A por la N... y cuando llegues a la Z, vuelve a empezar"*.

---

## 2. El Reto de la Cebolla (Nivel 12) 🧅
Este es el **Jefe Final** de los niveles básicos. Tienes un archivo `data.txt` que es un Hexdump de un archivo comprimido repetidamente.

### A. Hexdumps (`xxd`)
Un archivo binario no se puede imprimir. Para verlo o compartirlo en texto, usamos Hexadecimal.
* **El problema:** Tienes el mapa (Hex), pero necesitas el territorio (Binario).
* **Solución:** `xxd -r` (Reverse). Convierte el texto Hex de vuelta a un archivo binario funcional.

### B. Compresión vs. Empaquetado
Linux distingue dos conceptos que Windows suele mezclar:
1.  **Empaquetar (`tar`):** Meter muchos archivos en una caja. (No reduce tamaño).
2.  **Comprimir (`gzip`, `bzip2`):** Sacar el aire a la caja. (Reduce tamaño).

> [!TIP] Estrategia Nivel 12
> 1.  **Extensiones:** A Linux no le importan, pero a `gzip` sí. Si `file` dice que es gzip, **renómbralo** a `.gz` antes de descomprimir (`mv archivo archivo.gz`).
> 2.  **Iteración:** Descomprimes -> miras qué salió con `file` -> renombras -> descomprimes de nuevo.

### C. Permisos de Escritura (`/tmp`)
En el Nivel 12, **no puedes escribir en tu carpeta Home**.
* Para descomprimir archivos, necesitas crear archivos nuevos.
* **Solución:** Crea tu propio taller en la carpeta temporal.
    * `mkdir /tmp/mi_taller`
    * `cd /tmp/mi_taller`
    * `cp ~/data.txt .` (Copia el reto a tu taller).

---

## 3. Identidad SSH (Nivel 13) 🔑
Hasta ahora usaste contraseñas. En el mundo profesional, usamos **Llaves**.
* **Concepto:** Es como una llave física. Si tienes el archivo `sshkey.private`, la puerta se abre. No necesitas memorizar nada.
* **Comando:** `ssh -i sshkey.private usuario@host`
* **Importante:** La llave privada es sagrada. Si alguien la copia, es tú.

---

## 4. Redes Básicas: Puertos y Localhost (Nivel 14 y 15)
Aquí dejas de manipular archivos y empiezas a manipular **conexiones**.

### A. Netcat (`nc`) - El "Telnet" moderno
En el Nivel 14, la contraseña no está en un archivo, está siendo servida por un programa en el **Puerto 30000**.
* Tienes que "tocar el timbre" en ese puerto para que te la den.
* **Comando:** `nc localhost 30000`
* **Inyección:** El nivel te pide que le des la contraseña actual.
    * `echo "contraseña_actual" | nc localhost 30000`

### B. Cifrado SSL (`openssl`)
En el Nivel 15, es lo mismo, pero el puerto 30001 habla un idioma seguro (SSL/TLS).
* Si usas `nc` (texto plano), el servidor te rechazará (es como intentar hablar español con alguien que solo habla chino).
* Necesitas un cliente que hable SSL.
* **Comando:** `openssl s_client -connect localhost:30001`

> [!NOTE] ¿Qué es Localhost?
> Es la dirección de "esta misma computadora" (127.0.0.1). Estás conectando el cliente de Bandit al servidor de Bandit, sin salir a internet.
```

### ⚠️ Advertencia antes de empezar el Nivel 12:

El Nivel 12 es donde más gente se frustra. Recuerda los pasos:

1. Ve a `/tmp`.
    
2. Crea carpeta.
    
3. Copia el archivo.
    
4. `xxd -r`.
    
5. `file` -> `mv` -> descomprimir -> REPETIR.
    

¡Suerte con la cebolla! Avísame cuando llegues al Nivel 15 y veremos qué sigue (Git y scripts).