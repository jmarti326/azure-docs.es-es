---
title: Implementación de la máquina virtual de Azure con Chef | Microsoft Docs
description: Obtenga información acerca de cómo usar chef para realizar la implementación y configuración de máquinas virtuales automatizada en Microsoft Azure
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: jeconnoc
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: de89756a3f9ef1139e855da16c0343a9919b56cb
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585381"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatización de la implementación de la máquina virtual de Azure con Chef
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef es una fantástica herramienta para ofrecer automatización y las configuraciones de estado que desee.

Con la versión de API de nube más reciente, Chef proporciona una perfecta integración con Azure, lo que ofrece la posibilidad de aprovisionar e implementar los estados de configuración mediante un único comando.

En este artículo, configurará su entorno de Chef para aprovisionar máquinas virtuales de Azure y se le guiará en la creación de una directiva o "CookBook" y, a continuación, implementará esta guía en una máquina virtual de Azure.

Vamos a comenzar

## <a name="chef-basics"></a>Conceptos básicos de Chef
Antes de comenzar, [revise los conceptos básicos de Chef](http://www.chef.io/chef). 

El diagrama siguiente muestra la arquitectura Chef de alto nivel.

![][2]

Chef tiene tres componentes de arquitectura principales: servidor de Chef, cliente de Chef (nodo) y estación de trabajo de Chef.

El servidor Chef es el punto de administración y hay dos opciones para el servidor Chef: una solución hospedada o una solución local. En este tutorial vamos a usar una solución hospedada.

El cliente Chef (nodo) es el agente que se encuentra en los servidores que está administrando.

La estación de trabajo Chef es la estación de trabajo de administración donde se crean las directivas y se ejecutan los comandos de administración. Ejecutamos el comando **knife** desde la estación de trabajo Chef para administrar la infraestructura.

También está el concepto de "Guías" y "Recetas". Estas son efectivamente las directivas que definimos y aplicamos a los servidores.

## <a name="preparing-the-workstation"></a>Preparar la estación de trabajo
En primer lugar, vamos a preparar la estación de trabajo. Estoy usando una estación de trabajo de Windows estándar. Es necesario crear un directorio para almacenar las guías y archivos de configuración.

En primer lugar, cree un directorio denominado C:\chef.

A continuación, cree un segundo directorio denominado c:\chef\cookbooks.

Ahora es necesario descargar el archivo de configuración de Azure para que Chef pueda comunicarse con la suscripción de Azure.

Descargue la configuración de publicación con el comando [Get-AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) de Azure PowerShell. 

Guarde el archivo de configuración de publicación en C:\chef.

## <a name="creating-a-managed-chef-account"></a>Crear una cuenta de Chef administrada
Regístrese para una cuenta de Chef hospedada [aquí](https://manage.chef.io/signup).

Durante el proceso de registro, se le pedirá que cree una nueva organización.

![][3]

Cuando se cree la organización, descargue el starter kit.

![][4]

> [!NOTE]
> Si recibe un mensaje que le advierte que se restablecerán las claves, está bien continuar como si no tuviéramos ninguna infraestructura existente configurada todavía.
> 
> 

El archivo zip del starter kit contiene sus claves y archivos de configuración de la organización.

## <a name="configuring-the-chef-workstation"></a>Configurar la estación de trabajo de Chef
Extraiga el contenido de chef-starter.zip en C:\chef.

Copie todos los archivos de chef-starter\chef-repo\.chef en el directorio c:\chef.

El directorio debería tener ahora un aspecto similar al siguiente ejemplo.

![][5]

Debería tener ahora cuatro archivos, incluido el archivo de publicación de Azure en la raíz de c:\chef.

Los archivos PEM contienen sus claves privadas de organización y administración para la comunicación mientras que el archivo knife.rb archivo contiene la configuración de knife. Tendremos que editar el archivo knife.rb.

Abra el archivo en el editor que desee y quite /../ de la ruta de acceso para modificar "cookbook_path" de forma que aparezca como se muestra a continuación.

    cookbook_path  ["#{current_dir}/cookbooks"]

Agregue además la siguiente línea que refleja el nombre de su archivo de configuración de publicación de Azure.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

El archivo knife.rb ahora debería ser ahora similar al siguiente ejemplo.

![][6]

Estas líneas asegurarán las referencias de Knife en el directorio de cookbooks en c:\chef\cookbooks y también usa nuestro archivo de configuración de publicación de Azure durante las operaciones de Azure.

## <a name="installing-the-chef-development-kit"></a>Instalar el kit de desarrollo de Chef
A continuación, [descargue e instale](http://downloads.getchef.com/chef-dk/windows) ChefDK (Kit de desarrollo de Chef) para configurar su estación de trabajo de Chef.

![][7]

Instálelo en su ubicación predeterminada de c:\opscode. Esta instalación tardará unos 10 minutos.

Confirme que la variable PATH contiene entradas para C:\opscode\chefdk\bin;C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Si no están ahí, asegúrese de agregar estas rutas de acceso.

> [!NOTE]
> El orden de la ruta de acceso es importante. Si las rutas de acceso de opscode no están en el orden correcto, tendrá problemas. 
> 

Reinicie la estación de trabajo antes de continuar.

A continuación, instalaremos la extensión de Knife Azure. De esta se proporciona a Knife el "complemento de Azure".

Ejecute el siguiente comando.

    chef gem install knife-azure ––pre

> [!NOTE]
> El argumento –pre garantiza que está recibiendo la versión de RC más reciente del complemento Knife Azure que ofrece acceso al conjunto más reciente de API.
> 
> 

Es probable que también se instalen varias dependencias al mismo tiempo.

![][8]

Para garantizar que todo esté configurado correctamente, ejecute el siguiente comando.

    knife azure image list

Si todo está configurado correctamente, verá una lista de imágenes de Azure disponibles en desplazamiento.

¡Enhorabuena! Se ha configurado la estación de trabajo.

## <a name="creating-a-cookbook"></a>Crear una guía
Chef usa las guías para definir un conjunto de comandos que desea ejecutar en el cliente administrado. La creación de una guía es un proceso sencillo y usamos el comando **chef generate cookbook** para generar la plantilla de Cookbook. Llamaré a mi servidor web de Cookbook ya que me gustaría una directiva que implemente IIS automáticamente.

En el directorio C:\Chef, ejecute el siguiente comando.

    chef generate cookbook webserver

Esto generará un conjunto de archivos en el directorio C:\Chef\cookbooks\webserver. Ahora es necesario definir el conjunto de comandos que nos gustaría que el cliente de Chef ejecutara en la máquina virtual administrada.

Los comandos se almacenan en el archivo default.rb. En este archivo, definiré un conjunto de comandos que instala IIS, inicia IIS y copia un archivo de plantilla en la carpeta wwwroot.

Modifique el archivo C:\chef\cookbooks\webserver\recipes\default.rb y agregue las siguientes líneas.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Cuando haya terminado, guarde el archivo.

## <a name="creating-a-template"></a>Crear una plantilla
Como mencionamos anteriormente, tenemos que generar un archivo de plantilla que se usará como la página default.html.

Ejecute el siguiente comando para generar la plantilla.

    chef generate template webserver Default.htm

Ahora navegue al archivo C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Edite el archivo agregando el código HTML simple "Hello World" y guarde el archivo.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Cargar la guía en el servidor Chef
En este paso, tomamos una copia de la guía que creamos en la máquina local y la cargamos en el servidor hospedado de Chef. Cuando se cargue, la guía aparecerá en la pestaña **Directiva** .

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Implementar una máquina virtual con Knife Azure
Ahora implementaremos una máquina virtual de Azure y aplicaremos la guía "Webserver" que instalará el servicio web de IIS y la página web predeterminada.

Para ello, use el comando **knife azure server create** .

A continuación aparecerá un ejemplo del comando.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Los parámetros son totalmente explicativos. Sustituir sus variables concretas y ejecutar.

> [!NOTE]
> Mediante la línea de comandos, también estoy automatizando mis reglas de filtro de red de punto de conexión mediante el parámetro –tcp-endpoints. He abierto los puertos 80 y 3389 para proporcionar acceso a mi página web y la sesión de RDP.
> 
> 

Cuando haya ejecutado el comando, pase al Portal de Azure y verá que su máquina comienza a aprovisionar.

![][13]

A continuación aparecerá el símbolo del sistema:

![][10]

Cuando haya finalizado la implementación, deberíamos podremos conectarnos al servicio web en el puerto 80 como habíamos abierto el puerto cuando aprovisionamos la máquina virtual con el comando Knife Azure. Como esta máquina virtual es la única máquina virtual de mi servicio en la nube, la conectaré con la dirección url del servicio en la nube.

![][11]

Como puede observar, me ha entrado la creatividad con mi código HTML.

No olvide que también podemos conectarnos mediante una sesión RDP desde Azure Portal a través del puerto 3389.

Espero que esto les haya resultado útil. Empiece hoy su viaje de su infraestructura como código con Azure.

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->
