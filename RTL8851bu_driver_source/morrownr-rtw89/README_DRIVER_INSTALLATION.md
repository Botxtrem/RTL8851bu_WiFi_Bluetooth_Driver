# Instalación del controlador Realtek RTL8851bu (rtw89)

Este documento resume los pasos realizados para instalar el controlador del adaptador WiFi Realtek RTL8851bu y los próximos pasos a seguir.

## Historial de acciones:

1.  **Identificación del dispositivo y kernel:**
    *   Adaptador WiFi USB: `ID 0bda:b851 Realtek Semiconductor Corp. 802.11ax WLAN Adapter` (RTL8851bu).
    *   Versión del kernel de Linux: `6.16.8+kali-amd64`.
    *   Se determinó que el controlador nativo para este chip se incluye a partir del kernel 6.17, por lo que se requiere un controlador externo.

2.  **Preparación del sistema:**
    *   Se actualizaron los paquetes del sistema y se instalaron las herramientas necesarias (`git`, `dkms`, `build-essential`, `bc`, `linux-headers-$(uname -r)`, `usb-modeswitch`).

3.  **Clonación del repositorio del controlador:**
    *   Se clonó el repositorio `morrownr/rtw89` (que proporciona controladores para adaptadores Wi-Fi 6/6E y Wi-Fi 7 de Realtek) en `/home/botxtrem/Escritorio/adactadorwifi/RTL8851bu/morrownr-rtw89`.
    *   El comando utilizado fue: `git clone https://github.com/morrownr/rtw89.git /home/botxtrem/Escritorio/adactadorwifi/RTL8851bu/morrownr-rtw89`

4.  **Compilación e instalación del controlador:**
    *   Se compilaron los módulos del kernel con `make`.
    *   Se instalaron los módulos del kernel con `sudo make install`.
    *   Se instaló el firmware necesario con `sudo make install_fw`.

## Próximos pasos (después del reinicio):

1.  **Reiniciar el sistema:**
    *   El sistema necesita reiniciarse para que el nuevo controlador `rtw89` se cargue y el adaptador `RTL8851bu` sea reconocido.
    *   Comando: `sudo reboot`

2.  **Verificación post-reinicio:**
    *   Después de reiniciar, verifica si el adaptador WiFi funciona.
    *   Puedes usar los siguientes comandos para verificar el estado:
        *   `lsmod | grep rtw89`: Debería mostrar que el módulo `rtw89` está cargado.
        *   `ip a`: Debería mostrar una nueva interfaz inalámbrica (por ejemplo, `wlan1` o `wlan0` si reemplazó a la anterior) o la interfaz existente `wlan0` ahora asociada con el controlador `rtw89`.
        *   `ethtool -i <nombre_interfaz_wifi>` (ej. `ethtool -i wlan0`): Debería mostrar `driver: rtw89`.

## Si el adaptador USB no funciona después del reinicio:

Si después de reiniciar el sistema el adaptador USB WiFi aún no funciona o no se detecta correctamente, los siguientes pasos serán:

1.  **Revisar logs del sistema:**
    *   `dmesg | grep rtw89`: Buscar mensajes relacionados con el controlador `rtw89` durante el arranque.
    *   `journalctl -xe`: Revisar los logs del sistema para posibles errores.

2.  **Verificar la lista negra de módulos:**
    *   A veces, los controladores nativos en conflicto pueden estar en la lista negra. Podríamos necesitar poner en la lista negra el controlador `rtw88_8822ce` si está causando conflicto.

3.  **Cargar el módulo manualmente:**
    *   Intentar cargar el módulo `rtw89` manualmente con `sudo modprobe rtw89`.

4.  **Buscar soluciones específicas:**
    *   Si los problemas persisten, buscaremos soluciones más específicas en foros de Linux o en la documentación del repositorio `morrownr/rtw89`.

Este documento servirá como guía para continuar con la resolución de problemas si es necesario.
