---
title: Creación de una instancia de Function App desde Azure Portal | Microsoft Docs
description: Cree una nueva instancia de Function App en Azure App Service desde el portal.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 1e27fb26b00502b63d6023b41b6a26b179b453be
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094049"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Creación de una instancia de Function App desde Azure Portal

Azure Function Apps usa la infraestructura de Azure App Service. En este tema se muestra cómo crear una instancia de Function App en Azure Portal. Una instancia de Function App es el contenedor que hospeda la ejecución de funciones individuales. Cuando crea una instancia de Function App en el plan de hospedaje de App Service, esa instancia puede usar todas las características de App Service.

## <a name="create-a-function-app"></a>Creación de una aplicación de función

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Cuando cree una instancia de Function App, brinde un **nombre de aplicación** válido, que solo puede incluir letras, números y guiones. El carácter de subrayado (**_**) no se permite.

Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. El nombre de la cuenta de almacenamiento debe ser único dentro de Azure. 

Después de crear la instancia de Function App, puede crear funciones individuales en uno o más lenguajes distintos. Cree funciones [a través del portal](functions-create-first-azure-function.md#create-function), la [implementación continua](functions-continuous-deployment.md) o mediante la [carga con FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Planes de servicio

Azure Functions tiene dos planes de servicio diferentes: plan de consumo y plan de App Service. El plan de consumo asigna automáticamente potencia de proceso cuando se ejecuta código, se escala horizontalmente cuando es necesario para gestionar la carga y se reduce horizontalmente cuando no se ejecuta código. El plan de App Service da a su instancia de Function App acceso a todas las funciones de App Service. Debe elegir el plan de servicio cuando crea la instancia de Function App y actualmente no se puede cambiar. Para más información, consulte [Elección de un plan de hospedaje de Azure Functions](functions-scale.md).

Si va a ejecutar funciones de JavaScript en un plan de App Service, debería elegir un plan con menos núcleos. Para obtener más información, consulte la [referencia de JavaScript relativa a las funciones](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Requisitos de la cuenta de almacenamiento

Cuando crea una instancia de Function App en App Service, debe crear o vincular una cuenta de Azure Storage de uso general compatible con Blob Storage, Queue Storage y Table Storage. A nivel interno, Functions usa Storage para operaciones como la administración de desencadenadores y las ejecuciones de la función de registro. Algunas cuentas de almacenamiento no son compatibles con colas ni tablas, como las cuentas de almacenamiento solo para blob, Azure Premium Storage y cuentas de almacenamiento de uso general con replicación ZRS. Estas cuentas se filtran en la hoja Cuenta de almacenamiento cuando se crea una instancia de Function App.

>[!NOTE]
>Cuando usa el plan de hospedaje de consumo, los archivos de configuración de enlace y el código de la función se almacenan en Azure File Storage en la cuenta de almacenamiento principal. Si elimina la cuenta de almacenamiento principal, este contenido se suprimirá y no se podrá recuperar.

Para más información sobre los tipos de cuenta de almacenamiento, consulte [Introducción de los servicios Azure Storage](../storage/common/storage-introduction.md#azure-storage-services). 

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



