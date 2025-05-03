# Cómo usar prototipo para generar archivo de output
## Setup
### Placa
Desde el MPU6050, es necesario conectar los pines al ESP32 de la siguiente manera:
- **VCC** con **3.3V**
- **GND** con **G**
- **SCL** con **20**
- **SDA** con **21**
No es necesario conectar los otros pines.

### Arduino IDE
#### Ajustes iniciales y paquetes:
- Descargar [Arduino IDE](https://www.arduino.cc/en/software/)
- Agregar en _Boards Manager_ (en la barra de la izquierda) el manager **esp32** por Espressif Systems.
- Agregar en _Library Manager_ la librería **Adafruit MPU6050** por Adafruit, junto a sus dependencias.

#### Al conectar la placa al computador:
- Revisar _Tools_ > _Port_. Selecciona el puerto con la placa. Si hay múltiples dispositivos conectados, puedes ver los puertos antes de conectar la placa y luego seleccionar el puerto nuevo que aparece al conectarla.
- Revisar _Tools_ > _Board_ y seleccionar **ESP32C3 Dev Module**.
- Una vez seleccionado esto, deberían aparecer nuevas opciones bajo _Tools_. Ir a _Tools_ > _USB CDC On Boot_ y seleccionar **Enabled**.
- **OPCIONAL**: Si se quiere cambiar la posición de los pines _SDA_ y _SCL_ en la placa ESP32, esto se puede hacer con las variables globales **sda** y **scl** al inicio del programa _read\_data.ino_.

### Putty
- Descargar [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

#### Configuración:
- Bajo _Session_ > _Logging_:
    - En _Session logging_, elige **All session output**
    - En _Log file name_, elige un nombre de archivo y la ubicación donde quieres que se guarde.
    - En _What to do if the log file already exists_, elige **Ask the user every time**.
- Bajo _Session_:
    - En _Connection type_, elige **Serial**.
    - En _Serial line_, escribe **COMx**, reemplazando **x** por el puerto a usar si ya tienes la placa conectada e identificaste el puerto usado con Arduino IDE.
    - En _Speed_, escribir **115200** (el mismo valor pasado al método _Serial.begin_ en el programa). 
    - En _Saved Sessions_, escribe un nombre para tus ajustes _(ejemplo: ESP32)_. Luego presiona **Save**. Esto guardará los ajustes para usar a futuro.
    - **OPCIONAL:** En el paso anterior, se puede elegir _"Default Settings"_ del recuadro de sesiones guardadas y guardar ahí nuestros ajustes. Esto hará que los ajustes elegidos sean los que se carguen cada vez que se abra Putty, en vez de tener que elegir cargarlos con **Load**.

## Ejecución
1. Abrir el programa _read\_data.ino_ en Arduino IDE.
2. En el IDE, presionar **Upload** (la flecha arriba a la izquierda).
3. Una vez que se haya subido a la placa el programa, se puede abrir **Serial Monitor** (lupa arriba a la derecha) para verificar que esté funcionando. Recordar cerrar Serial Monitor para que no interfiera con Putty.
4. Abrir **Putty**. Una vez cargada la sesión guardada, recordar revisar en _Serial line_ que esté el puerto elegido en Arduino IDE. También puede ser útil revisar el nombre del archivo que guardará los datos en _Session_ > _Logging_ > _Log file name_.
5. Hacer click a **Open** en Putty.
6. Una vez que quiera terminar o cortar la toma de datos, cerrar la terminal abierta por Putty. Los datos quedarán registrados en el archivo seleccionado.

Para separar tomas de datos en una sesión, se pueden repetir los pasos 4 a 6 todas las veces que sea necesario. Se debe cambiar el _Log file name_ para cada toma de dato para no sobreescribir tomas anteriores.

## Consideraciones
- Aún no existe funcionalidad Bluetooth o WiFi. Se debe mantener el ESP32 enchufado al computador para que este tenga energía y pueda enviar los datos.
- El acelerómetro registra la aceleración por la gravedad. Se puede intentar restar su efecto por software, pero la imprecisión del MPU6050 hace esto poco confiable después de unos segundos. Se recomienda realizar los tests intentando mantener la orientación vertical estable.

## Troubleshooting
**En Arduino IDE, ir a _File_ > _Preferences_ y checkear  _show verbose input_ para _compile_ o _upload_, según la sección que cause problemas.**

- **Puerto no disponible**: Revisar que el puerto no esté siendo utilizado por otro programa. En el caso de Arduino IDE, esto puede ser otra instancia del mismo IDE. En el caso de Putty, puede ser el Serial Monitor de Arduino IDE. Cerrar estos otros programas o monitores debería habilitar el acceso al puerto. Si los problemas persisten, se puede reiniciar el computador para cerrar todos los programas.