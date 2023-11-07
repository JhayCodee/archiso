# archiso

Para crear una imagen ISO de tu distribución personalizada de Arch Linux que incluya todas las personalizaciones que has mencionado, sigue estos pasos detallados. Ten en cuenta que necesitarás tener privilegios de superusuario (sudo) para ejecutar la mayoría de estos comandos.

### Paso 1: Preparar el entorno de trabajo

Primero, debes instalar los paquetes necesarios para poder utilizar `archiso`:

```bash
sudo pacman -Sy archiso
```

Copia los archivos de configuración predeterminados de `archiso` a tu directorio de trabajo:

```bash
sudo cp -r /usr/share/archiso/configs/releng/ ~/archlive
```

### Paso 2: Copiar la configuración de mkinitcpio y el kernel

Copia el preset de mkinitcpio que has modificado:

```bash
sudo cp /etc/mkinitcpio.d/linuxnew.preset ~/archlive/airootfs/etc/mkinitcpio.d/
```

Copia el kernel personalizado y los módulos:

```bash
sudo cp /ruta/a/kernelbuild/arch/x86/boot/bzImage ~/archlive/airootfs/boot/vmlinuz-linuxnew
sudo cp -r /ruta/a/kernelbuild/lib/modules/* ~/archlive/airootfs/lib/modules/
```

### Paso 3: Añadir personalizaciones de GRUB

Copia el archivo de configuración de GRUB personalizado y el tema:

```bash
sudo cp /ruta/a/tu/grub.cfg ~/archlive/airootfs/etc/default/grub
sudo cp -r /ruta/a/tu/tema/grub ~/archlive/airootfs/boot/grub/themes/
```

### Paso 4: Personalizar el logo y el fondo de escritorio

```bash
sudo cp /ruta/a/tu/logo.png ~/archlive/airootfs/usr/share/plymouth/themes/spinfinity/header-image.png
sudo cp /ruta/a/tu/fondo.jpg ~/archlive/airootfs/usr/share/backgrounds/
```

### Paso 5: Añadir el driver USB

```bash
sudo cp /ruta/a/tu/usb_driver.ko ~/archlive/airootfs/lib/modules/$(uname -r)/kernel/drivers/usb/
```

### Paso 6: Crear la imagen initramfs

Aquí tendrás que trabajar dentro de un entorno chroot para ejecutar mkinitcpio. `archiso` se encarga de esto automáticamente al construir la ISO.

### Paso 7: Construir la ISO con Archiso

Este comando construirá la imagen ISO con todas las modificaciones incluidas:

```bash
cd ~/archlive
sudo mkarchiso -v -w ~/archlive/work -o ~/archlive/out .
```

### Paso 8: Probar la ISO

Una vez finalizado, tendrás un archivo ISO en `~/archlive/out/` que podrás quemar en un USB o usar en una máquina virtual para probar.

### Paso 9: Ajustes y Reconstrucción

Si algo no funciona, vuelve a tu entorno de trabajo, realiza los ajustes necesarios y reconstruye la ISO siguiendo los pasos anteriores.

Recuerda reemplazar `/ruta/a/` con la ruta actual a tus archivos personalizados. Esto es solo una guía y puede requerir ajustes según la configuración específica de tu sistema y las personalizaciones que hayas realizado.
