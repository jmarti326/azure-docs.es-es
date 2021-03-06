
1. Visite el [Azure Portal]. Haga clic en **Examinar todo** > **Aplicaciones móviles** > el back-end que acaba de crear. En la configuración de la aplicación móvil, haga clic en **Inicio rápido** > **Android**. En **Configurar la aplicación cliente**, haga clic en **Descargar**. Así se descarga un proyecto de Android completo para que una aplicación previamente configurada pueda conectarse al back-end. 
2. Abra el proyecto mediante **Android Studio**, con **Importar proyecto (Eclipse ADT, Gradle, etc.)**. Asegúrese de que realice esta selección de importación para evitar los errores del JDK.
3. Presione el botón **Ejecutar "aplicación"** para compilar el proyecto e iniciar la aplicación en el simulador de Android.
4. En la aplicación, escriba un texto significativo, como *Realizar el tutorial* y luego haga clic en el botón "Agregar". Esto envía una solicitud POST al back-end de Azure implementado anteriormente. El back-end inserta los datos de la solicitud en la tabla SQL TodoItem y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure Portal]: https://portal.azure.com/
