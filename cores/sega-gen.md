# Genesis Plus GX - Sega Genesis

## Descargar

```bash
curl -fl --retry 3 \
  "https://buildbot.libretro.com/nightly/linux/x86_64/latest/genesis_plus_gx_libretro.so.zip" \
  -o ~/Downloads/genesis_plus_gx_libretro.so.zip
```

## Instalar

```bash
unzip -o ~/Downloads/genesis_plus_gx_libretro.so.zip \
  -d "$RA/cores"
```

## Archivo de información

```bash
curl -fL \
  "https://raw.githubusercontent.com/libretro/libretro-core-info/master/genesis_plus_gx_libretro.so.info" \
  -o "$RA/cores/genesis_plus_gx_libretro.so.info"
```

## BIOS

Genesis Plus GX no necesita archivos de sistema para emular juegos normales de Sega Genesis / Mega Drive, pero sí requiere BIOS para algunas funciones y consolas adicionales.
| Sistema | ¿Necesita BIOS? |
| --- | --- |
| Sega Genesis / Mega Drive | No |
| Sega Master System | No, generalmente |
| Sega Game Gear | No |
| Sega SG-1000 | No |
| Sega CD / Mega-CD | Sí, obligatoria |

Se colocan los ficheros bin directamente en el directorio de system
