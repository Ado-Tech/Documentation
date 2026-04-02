---
sidebar_label: 'Sitio de Origen'
sidebar_position: 1
---
# 🚀 Guía de Paso a Producción – Sitio de Origen

Este documento explica cómo subir a producción el **Sitio de Origen**. Este es el sistema principal donde llegan y se consultan todas las transacciones.

## 0. 📋 Requisitos Previos (Antes de empezar)

Antes de mover cualquier archivo o abrir programas, usted debe tener a la mano la siguiente información. **No inicie el proceso si le falta alguno de estos datos.**

### 0.1 Información para solicitar
Contacte al encargado (Ej: Juan Manuel Naranjo) y solicite:
* **Configuración de S3:** Nombre del bucket, usuario y llaves de acceso (AccessKey y SecretKey).
* **Datos de Base de Datos:** Nombre exacto de la base de datos del cliente, usuario (ID) y contraseña de producción.

### 0.2 Herramientas necesarias
Asegúrese de tener instalados y funcionando:
* **Visual Studio** (con el proyecto ADOVCS cargado).
* **Git** (Sourcetree o similar).
* **Acceso a los servidores** por Escritorio Remoto.

---

## 1. 🧩 Preparación del código (Antes de empezar)

Para que el despliegue salga bien, debemos asegurarnos de tener la versión más reciente del sistema en nuestro computador.

### 1.1 ¿Dónde está el código? (Repositorio Git)

El proyecto se encuentra en un lugar llamado **ADOVCS**. 

1. **Ubicación:** Entre a su herramienta de Git (donde ve los proyectos) y asegúrese de estar en el proyecto **ADOVCS**.
2. **Validación de Ramas (Rutas de trabajo):**
   - Normalmente siempre trabajamos con la rama llamada `main` o `master`.
:::warning ¡ALTO! REVISE ESTO
   Hay clientes que tienen "Ramas Especiales" (por ejemplo, una versión solo para Sura). **Antes de seguir, pregunte al Líder Técnico si debe usar la rama `main` o una especial para este cliente.**
:::

### 1.2 Actualizar y Limpiar el Código (Sincronización)

Antes de crear los archivos para el servidor, debe asegurarse de que su computador tiene la "última palabra" de lo que se ha programado. Puede usar **Sourcetree** o **Visual Studio**. 

Siga este orden riguroso:

1. **Hacer un "Fetch" (Mirar afuera):** Es como asomarse a la ventana para ver si el cartero trajo cartas nuevas. Al darle clic, el programa revisará si algún compañero subió cambios.
2. **Revisar las flechas (Barra inferior):** - **Flecha hacia abajo (↓):** Significa que hay cambios nuevos por bajar.
   - **Flecha hacia arriba (↑):** Significa que hay cambios en su equipo que no se han subido.
3. **Hacer un "Pull" (Descargar):** Esto descarga los cambios y actualiza su proyecto.

:::danger REGLA DE ORO: EL CONTADOR EN CERO
**Nunca le dé al botón "Publicar" si ve números en las flechas.** Todo debe estar limpio y en **0**. 
- Si publica con números en (↓), estará subiendo una versión vieja.
- Si publica con números en (↑), estará subiendo cambios que no han sido aprobados en el servidor.
:::

### 1.3 Abrir el programa (Visual Studio)
1. Abra **Visual Studio** (preferiblemente como Administrador).
2. Abra el proyecto (Solución) del Sitio de Origen.
3. Confirme una última vez que en la esquina inferior derecha las flechas indiquen **0**. Ahora está listo para publicar.

---

## 2. 🛠️ Cómo generar los archivos del sitio (Publicar)

Una vez que el código está al día, el siguiente paso es "empaquetarlo" para poder llevarlo al servidor. Siga estos pasos en Visual Studio:

1. **Seleccionar el proyecto:** En la parte derecha, busque el que se llama `ADOValidation.Web`. Haga **clic derecho** sobre él y elija la opción **Publicar**.

2. **Elegir la ruta:** Se abrirá una ventana con un "Perfil de publicación". Aquí es donde elegimos dónde se guardarán los archivos en su computador. 
   * Busque la carpeta llamada **Publicaciones**.
   * Dentro de esa carpeta, cree una nueva con el nombre del cliente (Ejemplo: `Sura`).
   

3. **Configurar el perfil (Paso Crítico):** Haga clic en el enlace que dice **"Mostrar todas las configuraciones"**. Se abrirá una pequeña ventana donde debe revisar dos cosas muy importantes:

   * **Configuración:** Debe decir siempre **`Release`**.
   * **Eliminar archivos existentes:** Si aparece en `false`, cámbielo a **`true`**. (Esto limpia la carpeta antes de poner los archivos nuevos).

4. **Guardar y Publicar:** Haga clic en **Siguiente** para conectar y luego en **Guardar**. Finalmente, presione el botón azul que dice **Publicar**.

5. **Verificar el éxito:** Espere a que el proceso termine. En la parte de abajo de Visual Studio aparecerá un mensaje confirmando que terminó exitosamente.

:::tip ¿CÓMO SABER SI QUEDÓ BIEN?
Al terminar, vaya a la carpeta que creó (Ej: `Publicaciones/Sura`). Debería ver muchos archivos y carpetas nuevos allí. Esos son los que llevaremos al servidor.
:::

---

## 3. 📦 Preparación de Archivos para el Servidor

Una vez que Visual Studio termina de publicar (Paso 2), usted tendrá una carpeta llena de archivos en su computador. Ahora debemos prepararlos para llevarlos a los servidores de producción.

### 3.1 Comprimir el sitio
1. Abra la carpeta donde se generó la publicación (Ejemplo: `C:\Publicaciones\Sura`).
2. Seleccione todos los archivos de esa carpeta, haga clic derecho y elija **Comprimir** (en formato `.zip` o `.rar`).
3. Póngale un nombre claro, por ejemplo: `Publicacion_Sura_26_03_2026.zip`.

### 3.2 Cómo pasar el archivo al servidor
Existen dos formas de llevar este archivo comprimido a las máquinas de producción:

* **Opción A (Recomendada - Más Rápida):** Suba el archivo a **WeTransfer**. Copie el enlace que le da la página. Este enlace lo usaremos más adelante cuando entremos al servidor para descargar el archivo directamente allá.
* **Opción B (Más Lenta):** Copiar el archivo en su computador y pegarlo directamente en la ventana de la "Conexión a Escritorio Remoto" del servidor. 

:::tip CONSEJO DE VELOCIDAD
Usar el enlace de **WeTransfer** es mucho más rápido porque el servidor descarga el archivo a máxima velocidad de internet, mientras que el "Copiar y Pegar" depende de su conexión local y puede fallar si el archivo es muy pesado.
:::

---

## 4. 🖥️ Despliegue en los Servidores de Producción

El sitio funciona en dos servidores al mismo tiempo (ambiente balanceado). Para que el cambio sea efectivo, debe realizar estos pasos en ambas máquinas. Lo ideal es hacerlo **en paralelo** (abrir las dos ventanas de servidor a la vez).

### 4.1 Identificación de los Servidores
Usted debe conectarse a:

| Nombre del Servidor | Dirección IP |
| :--- | :--- |
| **Aplicaciones Nuevo** | `172.29.6.206` |
| **Aplicaciones 3** | `172.29.6.89` |

### 4.2 Descarga del paquete en el Servidor
Siga estas instrucciones dentro de **cada servidor**:

1. **Conexión:** Entre al servidor usando "Conexión a Escritorio Remoto".
2. **Navegador:** Abra el navegador (Edge o Chrome) dentro del servidor.
3. **Descarga:** Pegue el enlace de **WeTransfer** que generó en el paso anterior.
4. **Guardar:** Descargue el archivo `.zip` o `.rar` en una carpeta temporal o en el Escritorio del servidor.

:::info DATO CLAVE
Hacerlo por WeTransfer directamente en el servidor asegura que el archivo no se corrompa y que la descarga sea inmediata, ya que usamos el internet del centro de datos y no el de su casa u oficina.
:::

---

## 5. ⚙️ Configuración del Servidor (IIS)

:::warning ¿ES UN SITIO NUEVO O UNA ACTUALIZACIÓN?
Antes de proceder, identifique el escenario de despliegue:

* **SITIO NUEVO:** Siga la guía paso a paso. Puede crear las carpetas y el Pool desde cero.
* **ACTUALIZACIÓN:** **¡TENGA CUIDADO!** No sobrescriba ni borre toda la carpeta actual sin respaldar. El archivo `web.config` existente en producción ya tiene las llaves de Coralogix, Redis y Base de Datos correctas. 
  * **Recomendación:** Suba solo los archivos de la aplicación y **no reemplace el `web.config`** a menos que sea estrictamente necesario por un cambio en la estructura del código.
:::

Ahora que tenemos el archivo en el servidor, debemos configurar el **IIS (Internet Information Services)**, que es el programa que permite que el sitio web sea visible.

:::info NOTA SOBRE NOMBRES
En los ejemplos de esta guía usaremos el nombre **"Sura"**, pero usted debe reemplazarlo por el nombre del cliente o proyecto que esté desplegando en ese momento.
:::

### 5.1 Crear el "Application Pool" (El Motor del Sitio)
Antes de poner los archivos, debemos crear un "espacio" exclusivo para que el sitio corra sin interferir con otros.

1. **Abrir el Administrador de IIS:** Busque el programa "IIS Manager" en el servidor.
2. **Verificar si ya existe:** En la columna izquierda, haga clic en **Application Pools**. 
   * Use el cuadro de **Filter** para buscar el nombre de su cliente/proyecto (Ej: `Sura`). 
   * *Si ya aparece en la lista, no necesita crear uno nuevo.*
3. **Crear uno nuevo (Si no existe):** Haga clic en **Add Application Pool...**
   * **Name (Nombre):** Escriba el nombre del proyecto (Ej: `Sura`).
   * **Configuración:** No mueva nada más, deje las opciones que vienen por defecto.
   * Haga clic en **OK**.
4. **Detener el motor:** Busque el Pool que acaba de crear en la lista, haga clic derecho sobre él y elija **Stop** (Detener). Es importante tenerlo apagado mientras terminamos de organizar los archivos.

### 5.2 Preparar los archivos del sitio
1. Busque el archivo comprimido (`.zip` o `.rar`) que descargó de WeTransfer.
2. Haga clic derecho y elija **Extraer aquí** (asegúrese de que los archivos queden en la carpeta destinada al sitio del cliente).
3. **¡Atención con el Web.config!:** 

:::danger AVISO DE SEGURIDAD
   Como es la primera vez que se sube el sitio, el archivo llamado `web.config` viene con configuraciones de prueba. 
   **Antes de arrancar el sitio, debe "comentar" (desactivar) las cadenas de conexión a la base de datos** para evitar bloqueos de cuenta o errores mientras terminamos de parametrizar.
:::

---

## 6. 🔐 Configuración Crítica y Migraciones

:::danger ¡ALTO! LEA ESTO ANTES DE ENCENDER EL SITIO
El sistema tiene un proceso automático llamado **Migraciones**. Cuando usted activa el sitio, el programa lee el archivo `web.config` y ejecuta cambios en la base de datos de forma automática.

**El peligro:** Si el `web.config` apunta por error a la base de datos de otro cliente, las migraciones podrían **borrar o dañar información ajena**.
:::

### 6.1 Paso de Seguridad: Comentar la Conexión
1. Abra el archivo `web.config` ubicado en la carpeta del sitio que acaba de extraer.
2. Busque la sección `<connectionStrings>`.
3. **Comente** (desactive) las líneas de conexión. Esto "apaga" la comunicación con la base de datos para que podamos configurar el resto de parámetros con total seguridad.

### 6.2 Checklist de Personalización (Web.config)

Una vez que las conexiones a la base de datos estén comentadas, recorra el archivo y ajuste los siguientes valores según el cliente que esté desplegando:

1. **Nombre del Proyecto (Coralogix):**
   Busque la línea `ProjectNameCoralogix`. Cambie el valor por el nombre de su proyecto.
   * *Ejemplo:* `<add key="ProjectNameCoralogix" value="Sura" />`

2. **Conexión a Memoria (Redis):**
   * **RedisConnection:** No invente este valor. Lo más seguro es copiar la línea completa de un sitio que ya esté funcionando en producción y pegarla aquí.
   * `<add key="RedisConnection" value="VALOR_TOMADO_DE_SITIO_EXISTENTE" />`
   * **RedisKeyPrefix:** Escriba el nombre del proyecto (Ej: `Sura`). Esto evita que los datos de un cliente se mezclen con los de otro en la memoria rápida.
   * `<add key="RedisKeyPrefix" value="Sura" />`

3. **Correo de Archivos:**
   Verifique la llave `EmailSignedFilesTo` y asegúrese de que tenga la configuración de correo correcta para este cliente.

4. **Seguridad de Sesión (MachineKey):**
   Este paso es **obligatorio** para que el balanceo entre los servidores funcione. Como actualmente todas las máquinas están balanceadas, si no se configura una llave única, el sistema expulsará al usuario cada vez que el balanceador lo mueva de un servidor a otro.
  
  **¿Cómo generarla?**
Ejecute el siguiente script en **PowerShell** desde su computador local:

```powershell
function New-MachineKey {
    $validationKey = -join ((1..128) | ForEach-Object { '{0:X}' -f (Get-Random -Minimum 0 -Maximum 16) })
    $decryptionKey = -join ((1..64)  | ForEach-Object { '{0:X}' -f (Get-Random -Minimum 0 -Maximum 16) })
 
    @"
<machineKey
  validationKey="$validationKey"
  decryptionKey="$decryptionKey"
  validation="HMACSHA256"
  decryption="AES" />
"@
}
 
New-MachineKey
```

**📝 Instrucciones de uso (MachineKey)**

1. El script de PowerShell retornará un bloque de texto que empieza con `<machineKey...`.
2. **Cópielo tal cual** y péguelo dentro de la sección `<system.web>` del archivo `web.config` del sitio que va a publicar.

:::info ¿DÓNDE PEGARLO?
Busque la etiqueta de apertura `<system.web>` y pegue el código justo debajo de ella o antes de que se cierre la etiqueta `</system.web>`.
:::

5. **Activar el Estado de Sesión (SessionState)**
Este parámetro ya existe en el archivo, pero debe ser validado y activado correctamente

**Instrucciones de configuración:**
1. Busque la etiqueta `<sessionState>`.
2. **Validación de la IP:** Verifique si la IP configurada en `stateConnectionString` es la correcta para este servidor.
3. **Regla de Oro (Copiar y Pegar):** Al igual que con la `RedisConnection`, lo más seguro es abrir el `web.config` de **cualquier otro sitio que ya esté publicado y funcionando** en este servidor. 
4. Copie la línea completa del `stateserver` del sitio activo y péguela en su nuevo archivo para asegurar que la IP y el puerto sean los que están operativos.

Debería quedar con una estructura similar a esta:
```xml
<sessionState mode="StateServer" stateConnectionString="tcpip=172.29.6.44:42424" stateNetworkTimeout="30" compressionEnabled="true" />
```

### 6.3 Configuración de Bases de Datos (Finalización del Web.config)

Este es el último paso de edición. Aquí conectaremos el sistema con sus datos reales. Utilice la información técnica entregada por el encargado (Ej: Juan Manuel Naranjo).

**A. Estructura de las 3 Cadenas de Conexión**
El sistema utiliza tres conexiones independientes. **Es obligatorio que cada una apunte a su propia base de datos:**

* **Validation:** Base de datos principal de transacciones.
* **Historic:** Base de datos para el historial de registros.
* **Templates:** Base de datos para las plantillas y formatos.

:::danger REGLA CRÍTICA: NO DUPLICAR
**No puede dejar el mismo nombre de base de datos en las tres conexiones.** Si lo hace, el sistema fallará. Cada conexión debe tener el nombre específico asignado por el área de BD.
:::

**B. Valores a Reemplazar**
Lo más recomendable es copiar la cadena de conexión de un sitio que ya sepa que está funcionando y simplemente reemplazar los valores específicos:

1.  **Server:** Casi siempre es `os-db-01.ado-tech.local`. **Este valor normalmente no se cambia.**
2.  **Database:** Coloque el nombre exacto de la base de datos correspondiente.
3.  **User ID:** Coloque el **ID** que le generó el encargado de BD.
4.  **Password:** Coloque la contraseña oficial de producción.

**C. Validación de Credenciales (Prueba de Fuego)**
Antes de guardar el archivo, realice esta prueba en el servidor para asegurar que el usuario tiene permisos:

1.  Abra **SQL Server Management Studio** dentro del servidor.
2.  En el login, coloque el servidor (`os-db-01.ado-tech.local`), el **ID** y la **Contraseña**.
3.  Haga clic en **Conectar**. Si conecta, expanda la base de datos para verificar que tiene permisos de lectura y escritura.
4.  Si la prueba en SQL es exitosa, proceda a reemplazar los datos en las 3 conexiones del `web.config` y **descomente** las líneas.

:::tip FINALIZACIÓN
Una vez realizados los cambios y verificado que no falten caracteres en la contraseña, **Guarde el archivo**. No se debe reemplazar nada más. El `web.config` ha quedado totalmente parametrizado.
:::

---

## 7. 🌐 Configuración del Sitio en IIS

Una vez que los archivos están en el servidor, debemos transformar esa carpeta física en una **Aplicación Web** funcional dentro del servidor de aplicaciones.

### 7.1 Despliegue de Archivos
1. Diríjase a la ruta raíz del servidor: `C:\inetpub\wwwroot`.
2. Cree una carpeta con el nombre del cliente o proyecto (Ej: `Sura`).
3. Pegue dentro de esta carpeta todos los archivos que descomprimió anteriormente.

### 7.2 Conversión a Aplicación (IIS Manager)
1. Abra el **IIS Manager** y navegue hasta `Sites` > `Default Web Site`.
2. Localice la carpeta que acaba de crear (`Sura`). 

:::info IDENTIFICACIÓN DE ESTADO
Es fundamental reconocer el estado del nodo en el árbol del IIS:
* **Icono de Carpeta Amarilla 📁:** Es solo un directorio de archivos (No ejecutará código .NET).
* **Icono de Aplicación (Globo con Documento) 🌐:** Es una aplicación activa vinculada a un Application Pool.
:::

3. Haga clic derecho sobre la carpeta y seleccione **Convert to Application**.
4. En la ventana emergente, haga clic en el botón **Select...** para asignar el Application Pool correspondiente (ej. `Sura`).
5. Haga clic en **OK**. 

> El icono debería cambiar automáticamente al símbolo de aplicación/mundo, indicando que el sitio ya es capaz de procesar peticiones web.

---

## 8. 🔄 Sincronización del Segundo Nodo

:::danger ¡IMPORTANTE! AMBIENTE BALANCEADO
El sitio funciona en dos servidores al mismo tiempo. Para que el cambio sea efectivo y no cause errores a los usuarios, **usted debe repetir el proceso en la segunda máquina**.
:::

### 8.1 ¿Qué pasos se deben repetir?
Conéctese al servidor **172.29.6.89** y realice nuevamente:
1. **Punto 5:** Configuración del Pool en el IIS.
2. **Punto 6:** Configuración del archivo `web.config`.
3. **Punto 7:** Creación de la carpeta y Conversión a Aplicación.

### 8.2 Regla de Oro para el Segundo Nodo
Para que el balanceo funcione, el archivo `web.config` del segundo servidor **debe ser una copia exacta** del que configuró en el primer servidor (IP 206).

* **MachineKey:** Debe ser **idéntica** en ambos servidores. No genere una nueva; copie la que pegó en el servidor 206.
* **Bases de Datos:** Deben apuntar exactamente a las mismas cadenas de conexión.
* **SessionState:** Ambos deben apuntar al mismo StateServer.

---

## 9. ✅ Verificación Final del Despliegue

Una vez que el pool de aplicaciones está iniciado en ambos servidores, siga estos pasos para confirmar que el sitio está operando correctamente.

### 9.1 Prueba de Enlace Local (IIS)
1. En el **IIS Manager**, seleccione su aplicación (Ej: `Sura`).
2. En el panel derecho (**Actions**), busque la sección **Browse Application** y haga clic en `Browse *:443 (https)`.
3. Si se abre Internet Explorer:
   * Haga clic en "Ask me later" en la configuración inicial.
   * **Sugerencia:** Copie la URL y péguela en **Chrome**, ya que es más fácil visualizar errores allí.
4. Si aparece un aviso de certificado, haga clic en **Advanced** (Opciones avanzadas) y luego en **Proceed to localhost (unsafe)**.
5. **Resultado esperado:** Si carga la pantalla de inicio o el login, la aplicación ha levantado correctamente en ese servidor.

### 9.2 Verificación de Base de Datos (Migraciones)
Como mencionamos en el Punto 6, el sistema ejecuta migraciones automáticas al encenderse.
1. Abra **SQL Server Management Studio**.
2. Refresque la lista de bases de datos del servidor.
3. **Confirmación:** Verifique que existan las bases de datos creadas (Ej: `SuraValidation`, `SuraHistoric` y `SuraTemplates`).
   * *Si las bases de datos aparecen con sus tablas, significa que la conexión y los permisos en el `web.config` quedaron perfectos.*

### 9.3 Acceso Externo y Balanceo
Pruebe el acceso desde fuera del servidor usando la URL pública (Ej: `adocolumbia.ado-tech.com/Sura`).

:::warning ¿ERROR 503 SERVICE UNAVAILABLE?
Si al intentar entrar desde afuera recibe un **Error 503**, la causa más probable es una falla en el balanceo:
* **Causa:** El balanceador envió su petición a un servidor donde el Pool de aplicaciones todavía está detenido (Stop).
* **Solución:** Asegúrese de que el Pool esté en estado **Started** en **AMBOS** servidores (`.206` y `.89`). Una vez que ambos estén arriba, el sitio cargará sin problemas.
:::

---

## 10. ⚙️ Configuración Inicial del Sistema

Una vez que el sitio está desplegado y las bases de datos han sido creadas por las migraciones, debe realizar la parametrización técnica desde el panel administrativo.

### 10.1 Acceso al Administrador (Reseteo de Credenciales)
El sistema crea un usuario `admin` por defecto, pero por seguridad debe activar su acceso y definir la contraseña directamente en la base de datos:

1.  **Identificar el usuario:** Vaya a la base de datos **Validation** y ejecute: 
    `SELECT * FROM dbo.Users` (El admin suele ser el `Id = 1`).
2.  **Generar el Hash de Contraseña:**
    * Entre a [sha1-online.com](https://sha1-online.com) y escriba la contraseña deseada.
    * Copie el resultado (Hash).
    * Entre a [mayusculasminusculas.com](https://mayusculasminusculas.com) y convierta ese Hash a **MAYÚSCULAS**.
3.  **Actualizar en SQL Server:** Ejecute el siguiente script con el hash generado:
    ```sql
    UPDATE dbo.Users 
    SET Password = 'HASH_GENERADO_EN_MAYUSCULAS', 
        ConfirmedEmail = 1 
    WHERE Id = 1;
    ```
4.  Ingrese al sitio web con el usuario `admin` y su nueva contraseña.

### 10.2 Configuración de Roles y Menús (Paso Obligatorio)
Para visualizar todos los módulos de configuración, debe habilitar los permisos del rol administrador:
1.  Vaya a **Administración > Administración de roles**.
2.  Seleccione la opción **Módulos**.
3.  **Active los módulos faltantes:** Administración de productos, Administración de calificaciones, Administración de guiones, Reportes, Reporte de transacciones (Operaciones), Reporte de facturación, Reporte de datos de georeferenciación, Reporte Transacciones Por Sucursales, Reporte Transacciones sesiones Webex, Transacciones del día, Administración de Enrolamiento, Administración Proyecto-Productos, Desbloqueo de clientes, Reasignar transacciones, Cambiar datos de transacción, Reporte Operaciones, Reporte de Transacción, Administración de observaciones, Administración de dominios permitidos, Buscar por rostro.
4.  **Cierre Sesión** y vuelva a ingresar para que los menús se actualicen.

### 10.3 Registro en Portal Biométrico (Generación de API Key)
Antes de configurar el sitio, debe registrar el proyecto en el servidor de biometría para obtener una llave de acceso:

1.  Ingrese al portal: [http://172.29.6.43/BiometricServices](http://172.29.6.43/BiometricServices)
    * **Usuario:** `admin` | **Password:** `Ado12345`
2.  Vaya al menú **Administración > Administración de proyectos**.
3.  **Crear Proyecto:** Si el proyecto no aparece en la lista, haga clic en **Agregar Proyecto**.
    * **Nombre del proyecto:** Escriba el nombre del cliente (Ej: `Sura`).
    * **API Key:** Haga clic en la opción **Generar**. 
:::warning NOTA TÉCNICA
Este portal suele presentar bloqueos visuales al generar la llave. Si el proceso parece quedarse "pegado", **recargue la página (F5)**; esto suele refrescar el estado y mostrar la llave generada.
:::
4.  **Activar:** Asegúrese de marcar el check de **Activo**, copie la **API Key** generada y haga clic en **Guardar**.

### 10.4 Parametrización Técnica (Módulo Configuración)
Regrese al sitio del cliente (Sitio de Origen) y vaya al menú **Configuración > Configuración de la aplicación**. 

#### A. Servicios Biométricos
* Localice el campo **Apikey servicios Biometricos** y pegue el valor que copió del portal anterior (Punto 10.3).
* **Bandeja Global:** En la sección "Bandeja Global", active el check **Implementar envío a bandeja global**. (Esto es vital para que las transacciones viajen correctamente).

#### B. Almacenamiento S3
Ingrese la información entregada por el encargado de infraestructura al inicio del proceso:
* **Llave para el acceso a S3:** Coloque el `AccessKeyId`.
* **Llave secreta para el acceso a S3:** Coloque el `SecretAccessKey`.
* **Nombre del Bucket en S3:** Coloque el `Destination_Bucket`.

#### C. Card Capture e Identidad
* **Tipo de Card Capture:** Seleccione **Card Capture V2**.
* **Intentos de captura:** Establezca en `3`.
* **URL Card Capture / URL Card Capture V2:** Copie los endpoints del sitio de referencia.
* **Scanovate:** Verifique la URL del servicio y la ruta de comparación de imágenes.

#### D. Rutas, Logs y Otros Parámetros
Replique los valores de un sitio funcional (Ej: EPM), ajustando siempre las rutas al cliente actual:
* **Rutas Temporales:** Ajuste las rutas para Validación y OCR (Ej: `C:\Temporales\Sura\...`).
* **URL del sitio de VCS:** URL del servicio de validación correspondiente.

### 10.5 Administración de IDs y Productos
1.  **Tipos de IDs:** Vaya a **Administración > Administración de tipos de ids**. Verifique que "Cédula de Ciudadanía" esté **Activo**. Active otros documentos si el requerimiento lo pide.
2.  **Configuración de Producto:** Vaya a **Administración > Administración de productos**.
    * Haga clic en **Modificar** sobre el producto (suele decir "Sin especificar").
    * **Nombre:** Coloque el nombre del proyecto (Ej: `Sura`).
    * **Calificación:** Establezca `Calificación verify: Automática`.
    * **Cotejo con QA:** Verifique que la configuración coincida con el ambiente de QA según el ticket.

### 10.6 Configuración de Liveness V3 (Producción)
Dentro del mismo formulario, configure los servicios de producción:
* **Tipo de liveness usado:** `Liveness V3`.
* **Url del servicio de liveness V3:** (Backend)`https://engine-prod.ado-tech.com`
* **Url del UI de liveness V3:** (UI) `https://liveness-ui.ado-tech.com/liveness/V8`
* **Intentos de recuperación archivos webhook:**`3`.
* **Segundos entre reintentos recuperación webhook:**`3`
* Haga clic en **Guardar producto**.

### 10.7 Configuración Regional (Ecuador)
Si el despliegue es para un cliente en **Ecuador**, debe realizar un paso adicional:
1.  Vaya a la pestaña **Configuración General**.
2.  Localice la sección **Configuración Registraduría (Ecuador)**.
3.  Parametrice los campos según los datos de conexión del servicio de identidad de ese país.

:::tip RECOMENDACIÓN PRÁCTICA
Para asegurar que el sitio funcione igual que los demás, abra en otra pestaña la configuración de un sitio activo (Ej: `adocolumbia.ado-tech.com/Epm/home`) y replique sus valores.
:::

:::info VALIDACIÓN CON QA
Para configuraciones específicas de **Liveness** o **CardCapture**, puede tomar como base los valores del ambiente de **QA**, pero asegúrese de cambiar los apuntamientos (URLs) a los servidores de **Producción**.
:::

:::danger REVISIÓN INTEGRAL
No se limite solo a los campos anteriores. **Debe recorrer y verificar todos los demás parámetros visibles en la pantalla.** Compare cada campo con el sitio de referencia (EPM o el sitio de QA) para asegurar que no falten tokens, llaves de API o configuraciones específicas de comportamiento del sistema.
:::

### 10.8 Finalización y Guardado
Una vez completados todos los campos:
1.  Haga clic en el botón **Guardar**.
2.  **Validación de seguridad:** Salga del módulo y vuelva a ingresar inmediatamente para verificar que todos los valores (API Keys, rutas y checks) persistan correctamente.

---

## 11. 👥 Gestión de Identidad y Usuarios Administrativos

Una vez configurada la aplicación, se debe formalizar la identidad del proyecto en el Sitio de Origen y habilitar los accesos definitivos para el equipo de Tecnología y Operaciones.

### 11.1 Sincronización del Identificador (Sitio de Origen)
Para que la comunicación con los servicios core sea exitosa, debe actualizar el nombre del proyecto dentro del aplicativo desplegado:

1.  En el Sitio de Origen, vaya al menú **Administración > Administración de proyectos**.
2.  Haga clic en **Modificar**.
3.  Cambie el nombre del proyecto para que tenga el identificador exacto del sitio (Ej: `Sura`).
4.  Haga clic en el botón **Generar** para crear una nueva llave. 
5.  Coloque el mismo Token KYC que se utilizó y validó en el ambiente de **QA**.
6.  Guarde los cambios.

### 11.2 Ajuste Técnico en IIS (Solución Error Creación de Usuarios)
:::warning PASO CRÍTICO DE CONFIGURACIÓN
Para poder registrar usuarios sin errores de sistema, debe realizar un ajuste manual en el archivo de configuración del servidor.
:::

1.  Vaya a la carpeta física donde se desplegó el sitio en el **IIS**.
2.  Abra el archivo `web.config`.
3.  Busque el tag `<dependentAssembly>` que contiene la referencia a `System.ValueTuple`.
4.  **Elimine o comente** completamente dicho tag. 
5.  Guarde el archivo. Esto libera la dependencia y permite el correcto funcionamiento del módulo de gestión de usuarios.

### 11.3 Creación de Usuario Administrador (Tecnología)
Siga estos pasos para crear su acceso y validar el flujo de notificaciones:

1.  En el sitio de origen, ingrese a **Administración > Gestión de usuarios**.
2.  Haga clic en **Agregar usuario** y complete:
    * **Nombre de inicio de sesión:** 
    * **Nombre:**  | **Apellidos:** 
    * **Correo:** 
    * **Sucursal:** `Principal`
    * **Requerir código para iniciar sesión:** `Habilitar`
    * **Roles:** `Administrador`
    * **Proyectos:** `Sura`
3.  **Validación:** Verifique la llegada del correo de confirmación, active la cuenta desde el link y cambie su contraseña al ingresar.

### 11.4 Entrega a Operaciones (Creación de Usuario Líder)
Para que el equipo de **Operaciones** inicie la parametrización funcional, cree el siguiente usuario:

* **Nombre de inicio de sesión:** `laura_vasquez`
* **Nombre:** `Laura` | **Apellidos:** `Vasquez`
* **Correo:** `laura.vasquez@ado-tech.com`
* **Sucursal:** `Principal`
* **Roles:** `Administrador`
* **Proyectos:** `Sura`

---

## 12. 📥 Integración con Global Inbox

Para que las transacciones generadas en el sitio de origen puedan ser gestionadas por el equipo operativo, debe registrar el proyecto en la bandeja central.

1.  **Acceso:** Ingrese a [https://adocolumbia.ado-tech.com/GlobalInbox](https://adocolumbia.ado-tech.com/GlobalInbox).
2.  **Registro:** Vaya al módulo de **Administración > Administración de proyectos** y haga clic en **Agregar un proyecto**.
3.  **Configuración:**
    * **Nombre del proyecto:** `Sura` (Debe coincidir con el identificador del sitio).
    * **Url del proyecto:** La URL pública configurada (Ej: `https://adocolumbia.ado-tech.com/Sura`).
    * **Llave de acceso para el API:** Pegue la APIKey generada en el Punto 11.1.
    * **Proyecto inactivo/activo:** Verifique que el check **Activo** esté marcado.
4.  **Guardar:** Haga clic en **Guardar proyecto**. Con esto, el flujo de datos entre el sitio y la bandeja queda habilitado.

---

## 13. 🎨 Configuración de Interfaz (GitHub)

El sistema de Liveness V3 requiere que los estilos y configuraciones visuales del cliente estén cargados en el repositorio central.

1.  **Verificación:** Ingrese al repositorio `github.com/Ado-tech/Liveness-V3-Clients-Configuration`.
2.  **Validación:** Busque si existe la carpeta con el nombre del proyecto (`Sura`) en la **rama de producción**.
3.  **Acción:** Si la carpeta no existe, informe al equipo de desarrollo o créela siguiendo la estructura de carpetas de los demás clientes para asegurar que la interfaz del Liveness cargue correctamente.

---

## 14. 🧪 Validación Final y Acta de Entrega

Antes de dar el despliegue por terminado, se debe realizar una Prueba (Smoke Test) para garantizar que todo el flujo (Captura -> Envío -> Recepción) funciona.

### 14.1 Prueba de Funcionamiento (URL de Validación)
Construya y ejecute una URL de prueba para simular una transacción real:

En key se coloca la Apikey del sitio 321FE4D20DDC01A
En projectName se coloca el nombre del proyecto en este caso Sura
En producto el número del producto al que corresponde en este caso 1

`https://adocolumbia.ado-tech.com/Sura/validar-persona?callback=https://www.google.com&key=321FE4D20DDC01A&projectName=Sura&product=1`

* **Verificación:** Complete el flujo de liveness y captura de documento.
* **Confirmación:** Entre al sitio de origen para validar que la transacción se creó y es visible.

### 14.2 Acta de Entrega a Operaciones
Una vez la prueba sea satisfactoria, proceda a la entrega formal a Operaciones **Laura Vásquez**. 
Complete el formato **SE-FT-001 (Acta de Entrega de Servicio)** con los datos específicos obtenidos durante este despliegue.

El correo o acta de entrega debe contener los siguientes datos críticos:

* **URL del Sitio:** `https://adocolumbia.ado-tech.com/Sura`
* **Nombre del Proyecto:** `Sura`
* **API Key del Proyecto:** `[Valor de la API Key]`
* **ID del Producto:** `1`
* **Usuario de Acceso:** `laura_vasquez` (creado en el punto 11.4).

:::success DESPLIEGUE FINALIZADO
Al enviar esta información, el equipo de Operaciones queda encargado de la creación de usuarios adicionales y la parametrización funcional de las campañas.
:::

---