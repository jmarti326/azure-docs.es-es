---
title: Azure Container Instances y orquestación de contenedores
description: Descripción de cómo Azure Container Instances interactúa con los orquestadores de contenedores.
services: container-instances
author: seanmck
ms.service: container-instances
ms.topic: article
ms.date: 10/05/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c17bdb5a81640a7162ae735a4633a31cdfffbb1d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803518"
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Container Instances y orquestadores de contenedores

Los contenedores, debido a su tamaño pequeño y a la orientación de aplicaciones, se adaptan perfectamente a entornos de entrega ágil y arquitecturas basadas en microservicios. La tarea de automatizar y administrar una gran cantidad de contenedores y cómo interactúan se conoce como *orquestación*. Entre los orquestadores de contenedores más populares se encuentran Kubernetes, DC/OS y Docker Swarm.

Azure Container Instances proporciona algunas de las funcionalidades básicas de programación de plataformas de orquestación. Y mientras no se cubran los servicios de más valor que esas plataformas proporcionan, Azure Container Instances puede complementarlos. En este artículo se describe el ámbito de los aspectos que controla Azure Container Instances y cómo los orquestadores de contenedores completos pueden interactuar con él.

## <a name="traditional-orchestration"></a>Orquestación tradicional

La definición estándar de orquestación incluye las tareas siguientes:

- **Programación**: en una solicitud de recurso e imagen de contenedor, busque una máquina adecuada en la cual ejecutar el contenedor.
- **Afinidad/antiafinidad**: especifique que un conjunto de contenedores se deben ejecutar de manera cercana entre sí (para mejorar el rendimiento) o con la distancia suficiente como para mejorar la disponibilidad.
- **Seguimiento de estado**: inspeccione los contenedores por si existen errores y vuelva a programarlos.
- **Conmutación por error**: realice un seguimiento de qué se ejecuta en cada máquina y vuelva a programar los contenedores desde máquinas con error a nodos en estado correcto.
- **Escalado**: agregue o quite instancias de contenedores para coincidir con la demanda, ya sea manual o automáticamente.
- **Redes**: proporcione una red superpuesta para coordinar los contenedores a fin de que se comuniquen entre varios equipos host.
- **Detección de servicios**: permita que los contenedores se localicen entre sí de manera automática incluso cuando se mueven entre equipos host y cambian las direcciones IP.
- **Actualizaciones de aplicaciones coordinadas**: administre las actualizaciones de contenedores para evitar el tiempo de inactividad de las aplicaciones y permitir la reversión si algo sale mal.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orquestación con Azure Container Instances: un enfoque por niveles

Azure Container Instances permite un enfoque por niveles a la orquestación, lo que proporciona todas las funcionalidades de programación y administración que se requieren para ejecutar un solo contenedor mientras se permite que las plataformas de orquestadores administren tareas de varios contenedores sobre él.

Dado que Azure administra la infraestructura subyacente de Container Instances, una plataforma de orquestadores no necesita ocuparse de encontrar un equipo host adecuado en el cual ejecutar un solo contenedor. La elasticidad de la nube garantiza que siempre haya uno disponible. En lugar de eso, el orquestador se puede centrar en las tareas que simplifican la implementación de arquitecturas con varios contenedores, incluidas actualizaciones coordinadas y escalado.

## <a name="potential-scenarios"></a>Escenarios potenciales

Si bien la integración de los orquestadores con Azure Container Instances todavía es incipiente, podemos prever que surgirán algunos entornos distintos:

### <a name="orchestration-of-container-instances-exclusively"></a>Orquestación exclusiva de Container Instances

Dado que se inician rápidamente y se facturan por segundo, un entorno basado exclusivamente en Azure Container Instances ofrece la forma más rápida de comenzar usar y trabajar con cargas de trabajo muy variables.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Combinación de Container Instances y contenedores en Virtual Machines

En el caso de cargas de trabajo estables y de ejecución prolongada, orquestar contenedores en un clúster de máquinas virtuales dedicadas habitualmente es más barato que ejecutar los mismos contenedores con Azure Container Instances. Sin embargo, Container Instances ofrece una solución excelente para expandir y contraer rápidamente la capacidad total para enfrentar las subidas inesperadas o breves en el uso.

En lugar de escalar horizontalmente el número de máquinas virtuales en el clúster y luego implementar contenedores adicionales en esas máquinas, el orquestador puede simplemente programar los contenedores adicionales con Azure Container Instances y eliminarlos una vez que deje de necesitarlos.

## <a name="sample-implementation-virtual-kubelet-for-kubernetes"></a>Implementación de ejemplo: Virtual Kubelet para Kubernetes

El proyecto [Virtual Kubelet][aci-connector-k8s] muestra cómo las plataformas de orquestación de contenedores se pueden integrar con Azure Container Instances.

Virtual Kubelet imita el [kubelet][kubelet-doc] de Kubernetes al registrarse como un nodo con capacidad ilimitada y distribuir la creación de [pods][pod-doc] como grupos de contenedores en Azure Container Instances.

Es posible crear conectores para otros orquestadores que se integren simplemente con las primitivas de plataforma para combinar la eficacia de la API del orquestador con la velocidad y la simplicidad de la administración de contenedores en Azure Container Instances.

## <a name="next-steps"></a>Pasos siguientes

Use la [guía de inicio rápido](container-instances-quickstart.md) para crear el primer contenedor con Azure Container Instances.

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/