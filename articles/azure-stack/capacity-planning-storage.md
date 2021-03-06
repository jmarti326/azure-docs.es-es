---
title: Planeamiento de la capacidad de almacenamiento de Azure Stack | Microsoft Docs
description: Obtenga más información sobre el planeamiento de la capacidad de almacenamiento para las implementaciones de Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeffgilb
ms.reviewer: prchint
ms.openlocfilehash: 9ea46860817d60c2ffbde68c0fc5ae6f6ca14877
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369763"
---
# <a name="azure-stack-storage-capacity-planning"></a>Planeamiento de la capacidad de almacenamiento de Azure Stack
En las secciones siguientes se proporciona información acerca del planeamiento de la capacidad de almacenamiento de Azure Stack para ayudarle a planificarse según las necesidades de almacenamiento de las soluciones.

## <a name="uses-and-organization-of-storage-capacity"></a>Usos y organización de la capacidad de almacenamiento
La configuración hiperconvergida de Azure Stack permite el uso compartido de dispositivos de almacenamiento físico. Las tres divisiones principales del almacenamiento disponible están comprendidas entre la infraestructura, el almacenamiento temporal de las máquinas virtuales de inquilino y el almacenamiento de respaldo de los blobs, las tablas y las colas de los servicios de Azure Consistent Storage (ACS).

## <a name="spaces-direct-cache-and-capacity-tiers"></a>Niveles de capacidad y memoria caché de Espacios de almacenamiento directo
La capacidad de almacenamiento se puede utilizar para el sistema operativo, el registro local, el volcado y otro tipo de necesidades de almacenamiento de infraestructura temporal. Esta capacidad de almacenamiento local es independiente (dispositivos y capacidad) de los dispositivos de almacenamiento que se encuentran bajo la administración de la configuración de Espacios de almacenamiento directo. El resto de dispositivos de almacenamiento se coloca en un único grupo de capacidad de almacenamiento, independientemente del número de servidores de la unidad de escalado. Estos dispositivos son de dos tipos: memoria caché y capacidad.  Los dispositivos de memoria caché son simplemente eso, memoria caché. Los Espacios de almacenamiento directo usarán estos dispositivos para el almacenamiento en caché con reescritura y lectura. Las capacidades de estos dispositivos de memoria caché, mientras estén en uso, no se dedican a la capacidad "visible" y con formato de los discos virtuales con formato. Los dispositivos de capacidad se usan con este propósito y proporcionan la "ubicación principal" de los datos administrados por Espacios de almacenamiento directo.

Toda la capacidad de almacenamiento se asigna y administra directamente según la infraestructura de Azure Stack. El operador debe tomar decisiones sobre la configuración, la asignación o el tratamiento de las opciones en lo que se refiere a la expansión de la capacidad. Estas decisiones de diseño se han adoptado para adaptarse a los requisitos de la solución y se automatizan durante cualquier implementación o instalación inicial o durante la expansión de la capacidad. Como parte del diseño, se han considerado, entre otros, detalles acerca de la resistencia y la capacidad reservada para las recopilaciones. 

Los operadores pueden elegir entre una configuración de almacenamiento híbrido o completamente de flash:

![Planeamiento de la capacidad de almacenamiento de Azure](media/azure-stack-capacity-planning/storage.png)

En la configuración completamente de flash, la memoria caché es NVMe con una opción de capacidad de SATA SSD o NVMe. En la configuración híbrida, la memoria caché ofrece la opción de elegir entre SATA SSD y NVMe, mientras que la capacidad es HDD.

A continuación, se incluye un breve resumen de la configuración de almacenamiento de Azure Stack y Espacios de almacenamiento directo:
- Un grupo de espacios de almacenamiento por unidad de escalado (todos los dispositivos de almacenamiento se configuran dentro de un único grupo).
- Los discos virtuales se crean como un reflejo de tres copias para optimizar el rendimiento y la resistencia.
- Cada disco virtual tiene el formato de un sistema de archivos ReFS.
- La capacidad del disco virtual se calcula y asigna de forma que deje sin asignar una cantidad de la capacidad de datos del dispositivo de capacidad en el grupo. Esto equivale a una unidad de capacidad por servidor.
- Cada sistema de archivos ReFS tendrá BitLocker habilitado para el cifrado de datos en reposo. 

Los discos virtuales se crean automáticamente y sus capacidades se indican a continuación:




|NOMBRE|Cálculo de capacidad|DESCRIPCIÓN|
|-----|-----|-----|
|Dispositivo local o de arranque|340 GB<sup>1</sup> como mínimo|Almacenamiento individual del servidor para imágenes del sistema operativo y máquinas virtuales de infraestructura "local".|
|Infraestructura|3,5 TB|Uso de toda la infraestructura de Azure Stack|
|VmTemp|Ver a continuación<sup>2</sup>|Las máquinas virtuales de inquilino tienen un disco temporal conectado y los datos se almacenan en estos discos virtuales.|
|ACS|Ver a continuación<sup>3</sup>|Capacidad de Azure Consistent Storage para el servicio de blobs, tablas y colas.|

<sup>1</sup>Capacidad de almacenamiento mínimo necesaria del asociado de soluciones de Azure Stack.

<sup>2</sup>El tamaño del disco virtual usado para los discos temporales de la máquina virtual del inquilino se calcula como una relación de la memoria física del servidor. Como se indica en las tablas siguientes para los tamaños de VM de IaaS de Azure, el disco temporal es una proporción de la memoria física asignada a la máquina virtual. La asignación efectuada para el almacenamiento del "disco temporal" en Azure Stack se realizará de modo que se capturen la mayoría de los casos de uso, pero puede que no se satisfagan todas las necesidades de almacenamiento del disco temporal. La proporción elegida es una solución intermedia para disponer de almacenamiento temporal sin consumir la mayor parte de la capacidad de almacenamiento de la solución solo para la capacidad del disco temporal. Se crea un disco de almacenamiento temporal por servidor en la unidad de escalado. La capacidad de almacenamiento temporal no superará el límite del 10 % de la capacidad de almacenamiento total disponible en el grupo de almacenamiento de la unidad de escalado. El cálculo se realiza de forma parecida al siguiente ejemplo:

```
  DesiredTempStoragePerServer = PhysicalMemory * 0.65 * 8
  TempStoragePerSolution = DesiredTempStoragePerServer * NumberOfServers
  PercentOfTotalCapacity = TempStoragePerSolution / TotalAvailableCapacity
  If (PercentOfTotalCapacity <= 0.1)
      TempVirtualDiskSize = DesiredTempStoragePerServer
  Else
      TempVirtualDiskSize = (TotalAvailableCapacity * 0.1) / NumberOfServers
```

<sup>3</sup>Los discos virtuales creados para el uso con ACS son una división simple de la capacidad restante. Como se indicó anteriormente, todos los discos virtuales son un reflejo a tres bandas y el valor de una unidad de capacidad de la capacidad de cada servidor está sin asignar. Los diversos discos virtuales enumerados anteriormente se asignan en primer lugar y, a continuación, se usa la capacidad restante para los discos virtuales de ACS.

## <a name="next-steps"></a>Pasos siguientes
[Más información acerca de la hoja de cálculo de planeamiento de capacidad de Azure Stack](capacity-planning-spreadsheet.md)
