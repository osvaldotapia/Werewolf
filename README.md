# Hombre lobo para Telegram

Este es el principal repositorio de Werewolf para Telegram.

Para actualizaciones de archivos de idioma, envíe el archivo xml en Telegram al [chat de soporte] (http://telegram.me/werewolfsupport) y solicite ayuda.

### Integración continua de Visual Studio Team Services	
![build status](https://parabola949.visualstudio.com/_apis/public/build/definitions/c0505bb4-b972-452b-88be-acdc00501797/2/badge)

## Requisitos
* .NET Framework 4.5.2
* SQL Server (I am using 2014) / SQL Server 2016
* Windows Server

## Preparar

Para configurar el hombre lobo en un servidor privado, siga estos pasos:

1. Vaya a [BotFather] (https://telegram.me/BotFather) y cree un nuevo bot. Responda todas las preguntas que haga y recibirá un token API.
    * En su servidor, abra regedit y vaya a `HKLM \ SOFTWARE \`, cree una nueva clave llamada `Werewolf` (HKLM - HKEY_LOCAL_MACHINE)
    * En la nueva clave, cree un nuevo valor de cadena denominado `ProductionAPI`.
    * Pegue su token API aquí.
    
2. Toma el archivo [werewolf.sql](https://github.com/osvaldotapia/Werewolf/blob/master/werewolf.sql) de este repositorio
392/5000
    * Abra el archivo en el bloc de notas, notepad ++, lo que sea que use
    * Verifique la ruta en la parte superior del archivo: actualícela si está utilizando una versión SQL diferente
    * Ejecute el script sql. Esto creará la base de datos `werewolf` y todas las tablas / vistas / procesos almacenados para ir con ella.
    * Si ya tiene algunos administradores (incluido usted), agregue sus TelegramID a la tabla `dbo.Admin`		
    * Para obtener su ID, acceda a su bot en Telegram y / Start. Después de eso, envíale un texto al azar. Ingrese esta URL en su navegador (https://api.telegram.org/botYOURTELEGRAMBOTAPIKEY/getUpdates).
    
3. Ahora es tiempo de compilar el código fuente
    * En su servidor, abra regedit
    * En la clave `Werewolf`, cree un nuevo valor de cadena llamado` BotConnectionString`.
    * Pegue la cadena de conexión aquí.
    * La cadena de conexión debería ser esto (cambiar los valores)
`metadata=res://*/WerewolfModel.csdl|res://*/WerewolfModel.ssdl|res://*/WerewolfModel.msl;provider=System.Data.SqlClient;provider connection string="data source=SERVERADDRESS;initial catalog=werewolf;user id=USERNAME;password=PASSWORD;MultipleActiveResultSets=True;App=EntityFramework"`
      * Si está utilizando la autenticación de Windows para su servidor MSSQL, tenga en cuenta que la propiedad de contraseña ya NO será necesaria. Debe reemplazarlo (tanto el ID de usuario como la contraseña) con "Trusted_Connection = True;" en lugar.
      * .gitignore ha marcado este archivo, por lo que no se confirmará. ** Sin embargo, cuando cree la configuración, VS la copiará en la app.config. Asegúrese de eliminarla si planea volver a su tenedor **
   * Cree otro nuevo valor de cadena llamado BotanReleaseAPI. Puede dejar esto en blanco si no desea realizar un seguimiento de su uso con BotanIO.
   * Si planea ejecutar otra instancia del bot como beta, agregue otros dos nuevos valores de cadena llamados BotanBetaAPI y BetaAPI. Nuevamente, puede dejar BotanBetaAPI vacío si lo desea. Establezca BetaAPI en el token de su beta bot.
   * En Visual Studio, abra la solución. Asegúrate de estar configurado en la versión 'RELEASE'. Es posible que desee entrar en
   `Werewolf_Control.Helpers.UpdateHelper.cs` and add your id to `internal static int[] Devs = { ... }`. Además, verifique dos veces el
 `Settings.cs` files in both `Werewolf Control/Helpers` and `Werewolf Node/Helpers`.
   * Construye la solución
   
4. Directorios del servidor
   * Elija cualquier directorio para su directorio raíz

   | Directory | Contents |
   |-----------|---------:|
   |`root\Instance Name\Control`|Control de construcción|
   |`root\Instance Name\Node 1`|Construcción del nodo|
   |`root\Instance Name\Node <#>`|Las actualizaciones de nodo se pueden agregar a una nueva carpeta de nodo. La ejecución /replacenodesen Telegram le indicará al bot que busque automáticamente el nodo más nuevo (por tiempo de compilación) y lo ejecute|
   |`root\Instance Name\Logs`|Directorio de registro|
   |`root\Languages`|Archivos xml de idioma: todos los casos de Werewolf comparten estos archivos|

   * Nota: Una vez que todos los nodos estén ejecutando la versión más reciente (directorio del Nodo 2), la próxima vez que actualice los nodos, puede colocar los nuevos archivos en el Nodo 1 y /replacenodes. Nuevamente, el bot siempre tomará el nodo que encuentre que sea el más nuevo, siempre que el directorio tenga Nodeel nombre. no nombrar cualquier otro directorio en la carpeta raíz con nada Nodeen ella.
5. ¡Enciende el bot!
6. Si intentas iniciar un juego ahora, notarás que el bot solo responderá con un error. Esto se debe a que aún no actualizaste los identificadores de gif. Consulte la siguiente sección para obtener instrucciones sobre cómo hacer esto.

## SOPORTE GIF
Para utilizar GIF con el bot, deberá "enseñar" al bot las nuevas ID de GIF. Desde Telegram, ejecute /learngif, el bot responderá con GIF learning = true. Ahora envíele un GIF y el bot responderá con una identificación. Envía al bot todos los GIF que necesites. En el proyecto Node, vaya a Helpers> Settings.cs y busque las listas de GIF. Deberá eliminar todas las ID existentes e ingresar las ID que acaba de obtener del bot.

Puede probarlos ejecutando /dumpgifs(¡preferiblemente en Mensaje privado!). Asegúrese de revisar DevCommands.cs y mire el DumpGifs()método, la mayoría está comentado. Descomenta lo que necesitas.
