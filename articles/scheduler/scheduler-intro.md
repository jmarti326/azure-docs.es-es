---
title: ¿Qué es Azure Scheduler? | Microsoft Docs
description: Aprenda a crear, programar y ejecutar trabajos automatizados que llamen a servicios dentro y fuera de Azure
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.topic: hero-article
ms.date: 09/17/2018
ms.openlocfilehash: 4d4f7bf9c77dad21f9e66ab0fa023a4898163f1f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989196"
---
# <a name="what-is-azure-scheduler"></a>¿Qué es Azure Scheduler?

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se va a retirar. Para programar trabajos, [pruebe Azure Logic Apps en su lugar](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

[Azure Scheduler](https://azure.microsoft.com/services/scheduler/) le ayuda a crear [trabajos](../scheduler/scheduler-concepts-terms.md) que se ejecutan en la nube mediante la descripción declarativa de acciones. Posteriormente, el servicio programa y ejecuta esas acciones de forma automática. Por ejemplo, puede llamar a servicios dentro y fuera de Azure, como llamar a puntos de conexión HTTP o HTTPS, y también publicar mensajes en colas de Azure Storage y colas o temas de Azure Service Bus. Puede ejecutar trabajos de inmediato o en un momento posterior. Scheduler admite [programaciones complejas y periodicidad avanzada](../scheduler/scheduler-advanced-complexity.md) con facilidad. Scheduler especifica cuándo se debe ejecutar los trabajos, mantiene un historial con los resultados del trabajo que puede revisar y, a continuación, programa de forma predecible y confiable cargas de trabajo para que se ejecuten.

Aunque puede usar Scheduler para crear, mantener y ejecutar cargas de trabajo programadas, Scheduler no hospeda las cargas de trabajo ni ejecuta código. El servicio solo *invoca* los servicios o el código hospedado en cualquier otro lugar, por ejemplo, en Azure, localmente o con otro proveedor. Scheduler puede realizar la invocación mediante HTTP, HTTPS, una cola de Storage, una cola de Service Bus o un tema de Service Bus. Para crear, administrar y programar trabajos, puede usar [Azure Portal](../scheduler/scheduler-get-started-portal.md), código, la [API REST de Scheduler](https://docs.microsoft.com/rest/api/scheduler/) o la [referencia de cmdlets de PowerShell de Azure Scheduler](scheduler-powershell-reference.md). Por ejemplo, se puede mediante programación crear, ver, actualizar, administrar o eliminar trabajos y [colecciones de trabajos](../scheduler/scheduler-concepts-terms.md) mediante scripts y en Azure Portal.

Otras funcionalidades de Azure también usan Scheduler en segundo plano, por ejemplo, [Azure WebJobs](../app-service/web-sites-create-web-jobs.md), que es una característica de [Web Apps](https://azure.microsoft.com/services/app-service/web/) en Azure App Service. Puede administrar las comunicaciones de estas acciones mediante la [API REST de Scheduler](https://docs.microsoft.com/rest/api/scheduler/). Le ayuda a administrar la comunicación para estas acciones.

Estos son algunos escenarios en los que Scheduler puede ayudarle:

* **Ejecución de acciones de aplicación periódicas**: por ejemplo, recopilar periódicamente datos de Twitter en una fuente.

* **Realización del mantenimiento diario**: eliminación diaria de registros, realización de copias de seguridad y otras tareas de mantenimiento. 

  Por ejemplo, como administrador, es posible que desee realizar una copia de seguridad de la base de datos a la 1:00 a.m. cada día durante los próximos 9 meses.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Scheduler en Azure Portal](scheduler-get-started-portal.md)
* Obtenga información acerca de los [planes y la facturación de Azure Scheduler](scheduler-plans-billing.md)
* Aprenda a [crear programaciones complejas y periodicidad avanzada con Azure Scheduler](scheduler-advanced-complexity.md)