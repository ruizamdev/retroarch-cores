# RetroArch en CachyOS — Guía de instalación y configuración

**Plataforma:** Linux / CachyOS  
**Instalación:** RetroArch mediante Steam  
**Hardware:** Lenovo Legion 5 — AMD Radeon integrada + NVIDIA GTX 1650  
**Cores:** Dolphin, LRPS2 y Mupen64Plus-Next

---

## 1. Introducción

La versión de RetroArch distribuida mediante Steam normalmente no incluye el actualizador en línea de cores.

Podemos instalar manualmente los emuladores descargando sus bibliotecas `.so` y archivos de información `.info`, sin perder la integración con Steam.

### Directorios principales

La ruta predeterminada de RetroArch en Steam nativo es:

```bash
~/.local/share/Steam/steamapps/common/RetroArch/
```

Para facilitar los comandos, podemos almacenar la ruta en una variable de entorno en bash:

```bash
RA="$HOME/.local/share/Steam/steamapps/common/RetroArch"
```

Pero si se utiliza la shell por defecto en CachyOS, fish, la forma de setear variables es diferente que en bash y el fichero .bashrc. En bash solo se escribe el nombre de la variable seguido de signo igual (=) y su contenido. En fish se utiliza el comando set:

```bash
set -gx VAR $HOME/.local/share/Steam/steamapps/common/RetroArch
```

Y el fichero de configuración de fish se encuentra en la siguiente ruta:

```bash
$HOME/.config/fish/config.fish
```

Solo se agrega la declaración de la variable dentro para tenerla disponible en la shell, los flags `-g` y `-x` son para otorgarle un scope global y capacidad de exportar la variable, respectivamente.

Estructura habitual:

```text
RetroArch/
├── cores/
│   ├── dolphin_libretro.info
│   ├── dolphin_libretro.so
│   ├── pcsx2_libretro.info
│   ├── pcsx2_libretro.so
│   ├── mupen64plus_next_libretro.info
│   └── mupen64plus_next_libretro.so
└── system/
    ├── dolphin-emu/
    │   └── Sys/
    └── pcsx2/
        └── bios/
```

**Importante:** las rutas reales pueden ser diferentes, especialmente si Steam utiliza otra biblioteca.

Comprobarlas en RetroArch:

`Settings → Directory`

Configurar `Cores` y `Core Info` para que ambos apunten a `$RA/cores`: los archivos `.so` y `.info` van juntos en esa carpeta. Revisar también `System/BIOS`.

---

## 2. Dependencias

Instalar herramientas necesarias:

```bash
sudo pacman -S --needed curl unzip git
```

### Repositorio oficial de cores

https://buildbot.libretro.com/nightly/linux/x86_64/latest/

Los archivos disponibles pueden cambiar con el tiempo. Verificar su existencia si alguna descarga devuelve un error 404.

---

---

---

## 5. Mupen64Plus-Next — Nintendo 64

### Descargar

```bash
curl -fL --retry 3 \
  "https://buildbot.libretro.com/nightly/linux/x86_64/latest/mupen64plus_next_libretro.so.zip" \
  -o ~/Downloads/mupen64plus_next_libretro.so.zip
```

### Instalar

```bash
unzip -o ~/Downloads/mupen64plus_next_libretro.so.zip \
  -d "$RA/cores"
```

### Archivo de información

```bash
curl -fL \
  "https://raw.githubusercontent.com/libretro/libretro-core-info/master/mupen64plus_next_libretro.info" \
  -o "$RA/cores/mupen64plus_next_libretro.info"
```

### Configuración gráfica mejorada

Dentro de un juego:

`Quick Menu → Core Options`

| Opción                | Valor recomendado  |
| --------------------- | ------------------ |
| RDP Plugin            | GLideN64           |
| RSP Plugin            | HLE                |
| CPU Core              | Dynamic Recompiler |
| Resolución            | 1920 × 1440        |
| MSAA                  | 4x, probar 8x      |
| Anisotropic Filtering | 16x                |
| Framebuffer Emulation | ON                 |
| Widescreen            | OFF inicialmente   |

GLideN64 está orientado a ofrecer mejoras visuales, mientras que ParaLLEl-RDP prioriza la fidelidad al hardware original.

### Mejoras adicionales

Según la compilación, se pueden encontrar opciones como:

- Texture Enhancement: xBRZ.
- High-Resolution Textures.
- Texture Cache.
- Bilinear Filtering.
- Custom Texture Packs.

Los paquetes de texturas HD requieren archivos adicionales y compatibilidad con el juego.

**FPS:** muchos títulos de Nintendo 64 utilizan 20 o 30 FPS originales. Aumentarlos arbitrariamente puede afectar la lógica del juego.

### Formatos

- `.z64`
- `.n64`
- `.v64`

Nintendo 64 normalmente no requiere una BIOS externa.

---

## 6. NVIDIA PRIME — Gráficos híbridos

La laptop dispone de GPU AMD integrada y NVIDIA GTX 1650 dedicada.

Para solicitar el uso de NVIDIA, abrir:

`Steam → RetroArch → Propiedades → Opciones de lanzamiento`

Agregar:

```bash
prime-run %command%
```

Comprobar si existe:

```bash
which prime-run
```

Si no está disponible, utilizar:

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only %command%
```

### Verificar la GPU

Con RetroArch abierto:

```bash
nvidia-smi
```

Para consultar los dispositivos Vulkan:

```bash
sudo pacman -S vulkan-tools
vulkaninfo --summary
```

La presencia de ambas GPU en Vulkan no garantiza cuál está renderizando RetroArch.

---

## 7. Drivers de video

En RetroArch:

`Settings → Drivers → Video`

Opciones habituales:

| Driver | Descripción    |
| ------ | -------------- |
| gl     | OpenGL clásico |
| glcore | OpenGL moderno |
| vulkan | API Vulkan     |

El driver del frontend y el plugin gráfico del core no son exactamente lo mismo.

Por ejemplo, Mupen64Plus-Next puede utilizar GLideN64 para renderizar mediante OpenGL.

Al cambiar de driver, reiniciar RetroArch.

---

## 8. Medición de rendimiento

Activar FPS:

`Settings → On-Screen Display → On-Screen Notifications → Notification Visibility → Display Framerate`

También se puede utilizar MangoHud.

Si está instalado y disponible:

```bash
mangohud prime-run %command%
```

Introducirlo en las opciones de lanzamiento de Steam.

MangoHud permite consultar métricas como FPS, carga gráfica, temperaturas y memoria de video, según el sistema y su configuración.

---

## 9. Diagnóstico de errores

### RetroArch se cierra al cargar una ROM

Activar los registros:

`Settings → Logging`

Configurar:

- Logging Verbosity: ON.
- Log to File: ON.
- Frontend Logging Level: Debug.
- Core Logging Level: Debug.

Guardar la configuración, reproducir el error y buscar registros recientes:

```bash
find ~/.config/retroarch ~/.local/share/Steam \
  -type f -name "*.log" -mmin -10 2>/dev/null
```

### Error: Failed to load content

Verificar primero que el archivo del juego sea compatible con el core.

En Dolphin se presentó este error al intentar cargar directamente un ZIP de Wind Waker:

```text
Could not recognize file
Could not boot
Failed to load content
```

La solución fue extraer la imagen del juego antes de cargarla.

### Error: libCg.so not found

Al ejecutar directamente el binario de RetroArch de Steam:

```bash
./retroarch --verbose
```

puede aparecer:

```text
error while loading shared libraries:
libCg.so: cannot open shared object file
```

Esto puede ocurrir porque el ejecutable necesita bibliotecas proporcionadas por el entorno de ejecución de Steam.

No demuestra por sí mismo que Dolphin o NVIDIA estén fallando.

Preferir la ejecución desde Steam para reproducir el entorno habitual.

### Core no aparece en RetroArch

Verificar:

1. Que el `.so` esté instalado.
2. Que el `.info` correspondiente exista.
3. Que las rutas configuradas coincidan con las carpetas utilizadas.
4. Que el core sea compatible con Linux x86_64.
5. Que no existan bibliotecas compartidas faltantes.

### Diagnóstico de dependencias

```bash
ldd "$RA/cores/dolphin_libretro.so" | grep "not found"
```

Este comando ayuda a detectar dependencias ausentes, aunque no identifica todos los problemas de carga.

---

## 10. Comprobación final

Verificar los cores instalados:

```bash
ls -lh "$RA/cores/"*.so
```

Verificar archivos de información:

```bash
ls -lh "$RA/cores/"*.info
```

Dentro de RetroArch:

`Main Menu → Load Core`

Comprobar que aparezcan:

- Nintendo — GameCube / Wii (Dolphin).
- Sony — PlayStation 2 (LRPS2).
- Nintendo — Nintendo 64 (Mupen64Plus-Next).

## Referencias

- [Libretro Buildbot](https://buildbot.libretro.com/)
- [Libretro Core Info](https://github.com/libretro/libretro-core-info)
- [Libretro Dolphin](https://github.com/libretro/dolphin)
- [Documentación de Libretro](https://docs.libretro.com/)
- [RetroArch en Steam](https://store.steampowered.com/app/1118310/RetroArch/)

---

**Notas:** Esta guía documenta la instalación manual y las configuraciones utilizadas o recomendadas durante el proceso. No todos los ajustes han sido verificados en cada core; los nombres, rutas y funciones pueden cambiar entre versiones. Antes de actualizar un core manualmente, conviene respaldar su biblioteca `.so` y la configuración correspondiente.
