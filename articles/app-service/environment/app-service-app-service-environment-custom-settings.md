---
title: Configuración personalizada de entornos de App Service
description: Opciones de configuración personalizada para Entornos de App Service
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/22/2016
ms.author: stefsch
ms.custom: mvc
ms.openlocfilehash: d60cdca78c143996fa5935726db0631321c9e2fe
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2017
ms.locfileid: "26129522"
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Opciones de configuración personalizada para Entornos de App Service
## <a name="overview"></a>Información general
Puesto que los entornos de App Service están aislados en un solo cliente, hay ciertas opciones de configuración que se pueden aplicar exclusivamente a ellos. En este artículo se documentan las distintas personalizaciones específicas disponibles para los entornos de App Service.

Si no cuenta con un entorno de App Service, consulte [Creación de un entorno de App Service](app-service-web-how-to-create-an-app-service-environment.md).

Puede almacenar las personalizaciones del entorno de App Service mediante el uso de una matriz en el nuevo atributo **clusterSettings** . Este atributo se encuentra en el diccionario de "Propiedades" de la entidad *hostingEnvironments* de Azure Resource Manager.

La siguiente plantilla abreviada de Resource Manager muestra el atributo **clusterSettings** :

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

El atributo **clusterSettings** se puede incluir en una plantilla de Resource Manager para actualizar el entorno de App Service.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Uso del Explorador de recursos de Azure para actualizar un entorno de App Service
Como alternativa, puede actualizar el entorno de App Service mediante el [Explorador de recursos de Azure](https://resources.azure.com).  

1. En el Explorador de recursos, vaya al nodo de App Service Environment (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**). A continuación, haga clic en el entorno de App Service específico que quiere actualizar.
2. En el panel derecho, haga clic en **Lectura/escritura** en la barra de herramientas superior para permitir la edición interactiva en el Explorador de recursos.  
3. Haga clic en el botón azul **Editar** para que la plantilla de Resource Manager se pueda editar.
4. Desplácese hasta la parte inferior del panel derecho. El atributo **clusterSettings** se encuentra abajo del todo, donde puede especificar o actualizar su valor.
5. Escriba (o copie y pegue) la matriz de valores de configuración que quiera en el atributo **clusterSettings** .  
6. Haga clic en el botón verde **PUT** situado en la parte superior del panel derecho para confirmar el cambio en el entorno de App Service.

Una vez enviado el cambio, tarda en aplicarse aproximadamente 30 minutos multiplicado por el número de front-ends en el entorno de App Service.
Por ejemplo, si un entorno de App Service tiene cuatro front-ends, tardará aproximadamente dos horas en finalizar la actualización de la configuración. Mientras se implementa el cambio de configuración, no será posible realizar otras operaciones de escalado o de cambio de configuración en el entorno de App Service.

## <a name="disable-tls-10"></a>Deshabilitación de TLS 1.0
Una pregunta recurrente de los clientes, especialmente de aquellos con auditorías de cumplimiento de PCI, es cómo deshabilitar explícitamente TLS 1.0 en sus aplicaciones.

TLS 1.0 se puede deshabilitar mediante la siguiente entrada de **clusterSettings** :

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Cambio del orden del conjunto de aplicaciones de cifrado TLS
Otra pregunta de los clientes es si pueden modificar la lista de cifrados negociados por su servidor, y si esto puede lograrse mediante la modificación del atributo **clusterSettings** , como se muestra a continuación. La lista de conjuntos de cifrado disponibles se puede recuperar de este [artículo de MSDN](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> Nota: Si se establecen valores incorrectos para el conjunto de aplicaciones de cifrado que SChannel no entiende, toda la comunicación TLS en el servidor podría dejar de funcionar. En tal caso, debe quitar la entrada *FrontEndSSLCipherSuiteOrder* de **clusterSettings** y enviar la plantilla de Resource Manager actualizada para volver a la configuración de conjunto de cifrado predeterminada.  Utilice esta funcionalidad con precaución.
> 
> 

## <a name="get-started"></a>Primeros pasos
El sitio de inicio rápido de plantillas de Azure Resource Manager incluye una plantilla con la definición base para [crear un entorno de App Service](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
