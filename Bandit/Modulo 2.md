
---

Markdown

````
---
tags:
  - teoria
  - linux
  - modulo_2
  - find
  - pipes
status: estudiando
---

# 🕵️ Módulo 2: Búsqueda, Filtros y Flujos (Bandit 5-10)

En estos niveles, el archivo que buscas está escondido entre miles de otros, o el contenido está sucio. Aprenderás a pedirle al sistema que busque por ti.

## 1. Metadatos e Inodos (Nivel 5 y 6)
Cuando ejecutas `ls -l`, no estás viendo el archivo en sí, estás viendo su **ficha técnica** (Metadatos). En Linux, esta ficha se guarda en una estructura llamada **Inodo**.

* **El Concepto:** `find` no lee el contenido de los archivos (sería lentísimo). `find` lee los **Inodos**.
* **Por qué importa:**
    * En el **Nivel 5**, te dicen: "El archivo pesa 1033 bytes".
    * Tú no abres 1000 archivos para pesarlos. Le dices al sistema: *"Traeme el inodo que tenga en su campo 'size' el valor 1033"*.
    * **Comando:** `find . -size 1033c` (`c` = bytes).

> [!NOTE] Human vs Machine
> `ls -lh` te dice "1K". Eso es para humanos. La máquina necesita precisión: `du -b archivo` te dirá "1033".

---

## 2. Los Flujos Estándar y la Basura (Nivel 6)
Aquí aprendes una de las lecciones más importantes de administración: **El Silencio**.

Cuando buscas en todo el sistema (`find / ...`), Linux intentará entrar a carpetas donde no tienes permiso. El Kernel te gritará: *"Permission Denied"*.
* Si hay 1 acierto y 10,000 errores, no verás el acierto.

### La Solución: Redirección de STDERR
Linux separa lo "bueno" de lo "malo" en dos canales distintos.
1.  **STDOUT (1):** El resultado que quieres.
2.  **STDERR (2):** Los errores.

Para ver solo la respuesta, mandamos el canal 2 a un agujero negro digital:
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
````

- `2>`: Agarra el canal de error.
    
- `/dev/null`: El dispositivo "nulo" (todo lo que entra ahí desaparece).
    

---

## 3. Tuberías (Pipes): La Cadena de Montaje (Niveles 7-10)

La filosofía Unix dice: _"Haz una cosa y hazla bien"_.

- `cat` solo lee.
    
- `grep` solo busca texto.
    
- `sort` solo ordena.
    

El símbolo **`|` (Pipe)** conecta la salida de uno con la entrada del otro.

### Anatomía de un Pipe

Fragmento de código

```
graph LR
    A[data.txt] -->|cat| B(Salida desordenada)
    B -->|PIPE| C(Comando grep 'millon')
    C -->|Salida filtrada| D[Pantalla]
```

- **En Nivel 7:** `grep "millionth" data.txt`
    
    - `grep` actúa como un colador. Tiras todo el archivo, y solo pasan las líneas que contienen la palabra clave.
        
- **En Nivel 8:** `sort data.txt | uniq -u`
    
    - `uniq` es "tonto"; solo compara líneas adyacentes.
        
    - Si tienes: `A, B, A`, `uniq` no ve duplicados.
        
    - Por eso **SIEMPRE** usas `sort` antes: `A, A, B`. Ahora `uniq` ve las dos A juntas y las elimina.
        

---

## 4. Codificación vs. Encriptación (Niveles 9 y 10)

Es vital distinguir esto en ciberseguridad.

1. **Encriptación:** Necesitas una clave secreta para leerlo. (Es seguro).
    
2. **Codificación (Encoding):** Solo necesitas saber el algoritmo para leerlo. (No es seguridad, es transporte).
    

- **Strings (Nivel 9):**
    
    - Un archivo binario ("data") tiene mezcla de texto humano y código de máquina.
        
    - Si haces `cat`, rompes tu terminal (suenan pitidos y salen símbolos raros).
        
    - `strings` filtra y solo muestra caracteres imprimibles (humanos).
        
- **Base64 (Nivel 10):**
    
    - Es una forma de codificar binarios en texto ASCII (letras y números).
        
    - Se reconoce porque termina en `==`.
        
    - **No es cifrado.** Cualquiera con `base64 -d` puede leerlo.
        

```

### Tu misión para este tramo:
1.  Entra a **Bandit Nivel 5**.
2.  Usa `find` pensando en **Inodos** (tamaño, usuario, permisos).
3.  En el **Nivel 6**, oblígate a escribir `2>/dev/null` y entiende qué pasaría si no lo pusieras (pruébalo sin eso para ver el caos).
4.  Llega hasta el **Nivel 10** usando Tuberías (`|`).

¿Listo para ensuciarte las manos? ¡Dale caña!
```