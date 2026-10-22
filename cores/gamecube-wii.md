# Dolphin — Nintendo GameCube y Wii

## Descargar

```bash
curl -fL --retry 3 \
  "https://buildbot.libretro.com/nightly/linux/x86_64/latest/dolphin_libretro.so.zip" \
  -o ~/Downloads/dolphin_libretro.so.zip
```

## Instalar

```bash
mkdir -p "$RA/cores"

unzip -o ~/Downloads/dolphin_libretro.so.zip \
  -d "$RA/cores"
```

## Descargar información

```bash
mkdir -p "$RA/cores"

curl -fL \
  "https://raw.githubusercontent.com/libretro/libretro-core-info/master/dolphin_libretro.info" \
  -o "$RA/cores/dolphin_libretro.info"
```

## Instalar archivos de sistema

```bash
git clone --depth=1 \
  https://github.com/libretro/dolphin.git \
  /tmp/libretro-dolphin
```

```bash
mkdir -p "$RA/system/dolphin-emu"

cp -a /tmp/libretro-dolphin/Data/Sys \
  "$RA/system/dolphin-emu/"
```

## Configuración gráfica recomendada

| Opción                | Valor inicial    |
| --------------------- | ---------------- |
| Video Driver          | Vulkan o glcore  |
| Internal Resolution   | 3x               |
| Anisotropic Filtering | 16x              |
| Progressive Scan      | ON               |
| Scaled EFB Copy       | ON               |
| GPU Texture Decoding  | OFF inicialmente |
| Widescreen Hack       | OFF              |

Las opciones disponibles dependen de la versión del core.

**Nota:** aumentar la resolución interna mejora la definición geométrica, pero no aumenta la resolución original de las texturas.

## Formatos de juegos

Dolphin admite imágenes de disco como `.iso`, `.gcm` y `.rvz`.

Si un juego está comprimido en ZIP, revisar su contenido:

```bash
unzip -l "juego.zip"
```

Extraer la imagen:

```bash
unzip "juego.zip"
```

Cargar la imagen extraída en RetroArch.
