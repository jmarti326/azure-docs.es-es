---
title: Sincronización de contenido de una carpeta de nube a Azure App Service
description: Descubra cómo implementar su aplicación en Azure App Service mediante una sincronización de contenido desde una carpeta de la nube.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: 3781010c74daa51c92813db85ee03eaa4c02a4cf
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233594"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Sincronización de contenido de una carpeta de nube a Azure App Service
En este artículo se explica cómo sincronizar el contenido en [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) desde Dropbox y OneDrive. 

La implementación de la sincronización de contenido a petición funciona con el [motor de implementación Kudu](https://github.com/projectkudu/kudu/wiki) de App Service. Puede trabajar con el código y el contenido de la aplicación en una carpeta en la nube des y luego sincronizarlos en App Service con tan solo hacer clic en un botón. La sincronización de contenido usa el servidor de compilación Kudu. 

## <a name="enable-content-sync-deployment"></a>Habilitación de la implementación de la sincronización de contenido

Para habilitar la sincronización de contenido, vaya a la página de la aplicación de App Service en [Azure Portal](https://portal.azure.com).

En el menú de la izquierda, haga clic en **Centro de implementación** > **OneDrive** o **Dropbox** > **Autorizar**. Siga las indicaciones de autorización. 

![](media/app-service-deploy-content-sync/choose-source.png)

Solo debe autorizarse con OneDrive o Dropbox una vez. Si ya dispone de autorización, simplemente haga clic en **Continuar**. Puede cambiar la cuenta de OneDrive o Dropbox autorizada si hace clic en **Cambiar cuenta**.

![](media/app-service-deploy-content-sync/continue.png)

En la página **Configurar**, seleccione la carpeta con la que desea sincronizar. Esta carpeta se crea en la siguiente ruta de acceso de contenido designada en OneDrive o Dropbox. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

Cuando haya terminado, haga clic en **Continuar**.

En la página **Resumen**, verifique las opciones y haga clic en **Finalizar**.

## <a name="synchronize-content"></a>Sincronización de contenido

Si desea sincronizar contenido de la carpeta en la nube con App Service, vuelva a la página del **Centro de implementación** y haga clic en **Sincronizar**.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > Debido a diferencias subyacentes en las API, **OneDrive para la empresa** no se admite en este momento. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>Deshabilitación de la implementación de la sincronización de contenido

Para deshabilitar la sincronización de contenido, vaya a la página de la aplicación de App Service en [Azure Portal](https://portal.azure.com).

En el menú de la izquierda, haga clic en **Centro de implementación** > **OneDrive** o **Dropbox** > **Desconectar**.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementación desde el repositorio GIT local](app-service-deploy-local-git.md)
