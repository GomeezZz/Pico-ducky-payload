Pico Ducky: Payload de Hacking Ético 🦆

Este repositorio contiene un payload de DuckyScript diseñado para ser utilizado con una Raspberry Pi Pico configurada como un USB Rubber Ducky. El script automatiza una serie de acciones en un sistema Windows con fines de aprendizaje y demostración en un entorno de hacking ético controlado.

Este proyecto ha sido desarrollado como parte de una práctica académica sobre técnicas de hardware hacking.

⚠️ ¡Advertencia! Uso Ético y Educativo

Este script está diseñado exclusivamente para fines educativos y de investigación en entornos controlados y autorizados.

    El uso de este código en sistemas sin el consentimiento explícito y por escrito del propietario es ilegal y no ético.

    El autor no se hace responsable del mal uso que se le pueda dar a esta herramienta.

    Utilízalo bajo tu propia responsabilidad y siempre dentro de la legalidad.

⚙️ Configuración del Dispositivo

Para que este payload funcione, la Raspberry Pi Pico debe estar flasheada con el firmware de CircuitPython y configurada para actuar como un dispositivo de interfaz humana (HID), emulando un teclado.

Toda la configuración inicial del hardware se basa en el excelente trabajo de dbisu. Para preparar tu Pico, sigue las instrucciones detalladas en su repositorio:

➡️ dbisu/pico-ducky en GitHub

📜 Análisis del Payload (payload.dd)

El script está escrito en DuckyScript y ejecuta las siguientes acciones de forma secuencial al ser conectado a una máquina objetivo con Windows:

    Retraso Inicial: Espera 3 segundos para asegurar que el sistema operativo ha reconocido el dispositivo.

    Apertura de PowerShell:

        Abre el cuadro de diálogo "Ejecutar" (Win + R).

        Escribe powershell y lo ejecuta para iniciar una nueva sesión.

    Extracción de Contraseñas WiFi:

        Ejecuta un comando de netsh para listar todos los perfiles WiFi guardados en el equipo.

        Itera sobre cada perfil y extrae su contraseña en texto plano.

        Guarda toda la información en un archivo temporal llamado wifi_passwords.txt en el directorio $env:TEMP.

    Exfiltración de Datos:

        Utiliza Invoke-WebRequest para enviar el contenido del archivo wifi_passwords.txt mediante una petición POST a un servidor controlado por el atacante. En el script de ejemplo, se usa un endpoint de OAST (Out-of-band Application Security Testing).

    Descarga de Reverse Shell:

        Vuelve a usar Invoke-WebRequest para descargar un ejecutable (en este caso, nc64.exe) desde un servidor web local y lo guarda en el directorio temporal.

    Ejecución del Reverse Shell:

        Inicia el proceso nc64.exe de forma oculta (-WindowStyle Hidden).

        El proceso establece una conexión inversa (reverse shell) hacia la máquina del atacante, dándole acceso a una terminal cmd.exe del sistema víctima.

    Limpieza de Evidencias:

        Para no dejar rastro, elimina los archivos creados durante la ejecución: wifi_passwords.txt y nc64.exe.

    Cierre de Sesión:

        Escribe exit para cerrar la ventana de PowerShell y finalizar el script.

🚀 Uso

    Prepara tu Pico: Asegúrate de haber seguido todos los pasos del repositorio pico-ducky.

    Personaliza el Payload: Edita el archivo payload.dd con tus propias URLs, direcciones IP y puertos.

    Copia el Payload: Arrastra y suelta el archivo payload.dd modificado a la raíz de tu Raspberry Pi Pico (que aparecerá como una unidad USB).

    Ejecuta: Conecta la Pico a la máquina objetivo. El payload se ejecutará automáticamente.
