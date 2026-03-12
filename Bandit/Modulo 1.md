
Aquí tienes exactamente el contenido del **Módulo 1** (Teoría de Shell y Sistema de Archivos) que generé en esa respuesta, listo para copiar y pegar en tu nota de Obsidian `Bandit_Teoria_Mod1`.

Markdown

````
---
tags:
  - teoria
  - linux
  - modulo_1
  - shell
status: estudiando
---

# 🐚 Módulo 1: La Shell y el Sistema de Archivos (Bandit 0-4)

En estos primeros niveles, el desafío no es "hackear", es entender cómo Linux interpreta lo que escribes.

## 1. Anatomía del Prompt
Cuando te conectas por SSH, lo primero que ves es el **Prompt**. No es solo decoración; te da información vital de situación.

```bash
bandit0@bandit:~$
````

- **`bandit0`**: El **Usuario** con el que estás logueado.
    
- **`@bandit`**: El **Host** (la máquina) a la que estás conectado.
    
- **`~`**: El **Directorio Actual**. La tilde (`~`) es una abreviatura de "Mi carpeta personal" (`/home/bandit0`).
    
- **`$`**: El **Nivel de Privilegio**.
    
    - `$`: Usuario normal (mortal).
        
    - `#`: Usuario Root (dios/administrador). **¡Cuidado aquí!**
        

> [!TIP] Experimento Mental
> 
> Si cambias de carpeta con cd /tmp, el prompt cambiará a bandit0@bandit:/tmp$. ¡Siempre mira el prompt antes de borrar algo!

---

## 2. Rutas: Absolutas vs. Relativas

Linux necesita saber _exactamente_ dónde están los archivos. Hay dos formas de dárselas:

### A. Ruta Absoluta (El mapa completo)

Siempre empieza desde la raíz (`/`). Es la dirección exacta e inequívoca.

- _Ejemplo:_ `/home/bandit0/archivo.txt`
    
- _Ventaja:_ Funciona sin importar dónde estés parado.
    

### B. Ruta Relativa (El atajo)

Empieza desde donde estás parado **ahora mismo**.

- _Ejemplo:_ Si ya estoy en `/home`, solo escribo `bandit0/archivo.txt`.
    
- **Símbolos clave:**
    
    - `.` (Punto): "Aquí". El directorio actual.
        
    - `..` (Doble punto): "Atrás". El directorio padre (un nivel arriba).
        

> [!WARNING] El problema del Nivel 1 (El guion -)
> 
> Hay un archivo llamado -. Si haces cat -, la Shell se confunde.
> 
> - **¿Por qué?** La Shell piensa que `-` significa "leé la entrada estándar" (STDIN) o que inicia un parámetro (flag).
>     
> - **Solución Teórica:** Debes forzar a la Shell a entender que es una **Ruta**, no un comando.
>     
> - **Comando:** `cat ./-` (Léase: "En este directorio actual `.`, busca el archivo `-`").
>     

---

## 3. El Espacio como Separador (Nivel 2)

En la Shell, el **espacio** es sagrado. Se usa para separar el comando de sus argumentos.

- Comando: `cat archivo con espacios`
    
- Interpretación de la Shell:
    
    1. Ejecutar `cat`.
        
    2. Buscar archivo `archivo`.
        
    3. Buscar archivo `con`.
        
    4. Buscar archivo `espacios`.
        

Teoría del Escapado:

Para decirle a la Shell "este espacio es parte del nombre, no un separador", tienes dos opciones:

1. **Comillas:** `cat "archivo con espacios"` (Agrupa todo en un solo argumento).
    
2. **Backslash (Escapado):** `cat archivo\ con\ espacios` (El símbolo `\` anula el poder especial del siguiente caracter).
    

---

## 4. Archivos Ocultos y Tipos (Nivel 3 y 4)

### Archivos Ocultos (Dotfiles)

En Linux, cualquier archivo que empiece con un punto (`.bashrc`, `.profile`, `.hidden`) está oculto por defecto para no ensuciar la vista.

- No es seguridad, es **orden**.
    
- `ls` no los muestra. Necesitas `ls -a` (All).
    

### "Todo es un archivo" pero no todos son iguales

En Windows, `.exe` es programa y `.txt` es texto. **En Linux, la extensión no importa**. Puedes llamar `foto.jpg` a un virus ejecutable.

- **Números Mágicos (Magic Numbers):** Los primeros bits de un archivo le dicen al sistema qué es realmente.
    
- **Comando `file`:** Lee esos bits y te dice la verdad: "ASCII text", "ELF executable", "Gzip compressed data".
    
- _En el Nivel 4:_ Te obligan a usar esto porque todos los archivos se llaman igual, pero solo uno es texto legible.