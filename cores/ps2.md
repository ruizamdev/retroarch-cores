# LRPS2 — PlayStation 2

El core LRPS2 puede distribuirse bajo el nombre histórico `pcsx2_libretro.so`.

## Descargar

```bash
curl -fL --retry 3 \
  "https://buildbot.libretro.com/nightly/linux/x86_64/latest/pcsx2_libretro.so.zip" \
  -o ~/Downloads/pcsx2_libretro.so.zip
```

## Instalar

```bash
unzip -o ~/Downloads/pcsx2_libretro.so.zip \
  -d "$RA/cores"
```

## Archivo de información

```bash
curl -fL \
  "https://raw.githubusercontent.com/libretro/libretro-core-info/master/pcsx2_libretro.info" \
  -o "$RA/cores/pcsx2_libretro.info"
```

## BIOS

Crear el directorio:

```bash
mkdir -p "$RA/system/pcsx2/bios"
```

_"Colocar aquí los archivos BIOS extraídos de una consola propia"_ o lo puedes descargar desde [aqui](https://mega.nz/file/xp03XL6S#wZcupcLCV4ywMhTeDn5Ud_mtfJ4tU1KHkdMvJ6jpem8)

Verificar los requisitos de BIOS y directorios de la versión concreta del core, ya que pueden variar.

## Configuración gráfica inicial

| Opción                | Valor                               |
| --------------------- | ----------------------------------- |
| Renderer              | Vulkan Hardware, si está disponible |
| Internal Resolution   | 2x o 3x                             |
| Anisotropic Filtering | 16x                                 |
| GPU                   | NVIDIA GTX 1650                     |

Formatos habituales: `.iso` y `.chd`, según compatibilidad del core.
