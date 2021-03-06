---
title: Preparación del equipo host del Kit de desarrollo de Azure Stack (ASDK) | Microsoft Docs
description: Describe cómo preparar el equipo host del Kit de desarrollo de Azure Stack (ASDK) para la instalación de ASDK.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: dd02dfe37a1566a3121b3571f72d964621330cd0
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647313"
---
# <a name="prepare-the-asdk-host-computer"></a>Preparación del equipo host del Kit de desarrollo de Azure Stack
Para poder instalar ASDK en el equipo host, el entorno de ASDK se debe preparar para su instalación. Cuando el equipo host del kit de desarrollo esté preparado, se iniciará desde el disco duro de la máquina virtual CloudBuilder.vhdx para comenzar la implementación del Kit de desarrollo de Azure Stack.

## <a name="prepare-the-development-kit-host-computer"></a>Preparar el equipo host del kit de desarrollo
Para poder instalar el ASDK en el equipo host, se debe preparar el entorno de este.
1. Inicie sesión como administrador local en el equipo host del kit de desarrollo.
2. Asegúrese de que el archivo CloudBuilder.vhdx se ha movido a la raíz de la unidad C:\ (C:\CloudBuilder.vhdx).
3. Ejecute el script siguiente para descargar el archivo instalador del kit de desarrollo (asdk-installer.ps1) del [repositorios de herramientas de GitHub para Azure Stack](https://github.com/Azure/AzureStack-Tools) en la carpeta **C:\AzureStack_Installer** del equipo host del kit de desarrollo:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Enforce usage of TLSv1.2 to download from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. En una consola de PowerShell con privilegios elevados, inicie el script **C:\AzureStack_Installer\asdk-installer.ps1** y, a continuación, haga clic en **Preparar entorno**.

    ![](media/asdk-prepare-host/1.PNG) 

5. En la página **Select Cloudbuilder vhdx** (Seleccionar Cloudbuilder vhdx) del instalador, busque y seleccione el archivo **cloudbuilder.vhdx** que ha descargado y extraído [en los pasos anteriores](asdk-download.md). Si lo desea, en esta página también puede activar la casilla **Agregar controladores** si necesita agregar más controladores al equipo host del kit de desarrollo. Haga clic en **Next**.  

    ![](media/asdk-prepare-host/2.PNG)

6. En la página **Configuración opcional**, especifique la información de la cuenta de administrador local para el equipo host del kit de desarrollo y haga clic en **Siguiente**. También puede especificar los valores de los siguientes ajustes opcionales:
  - **Nombre del equipo**: esta opción permite establecer el nombre para el host del kit de desarrollo. El nombre debe cumplir los requisitos de FQDN y debe tener una longitud máxima de 15 caracteres. El valor predeterminado es un nombre de equipo aleatorio generado por Windows.
  - **Configuración de IP estática**: hace que la implementación use una dirección IP estática. En caso contrario, cuando se reinicia el instalador en cloudbuilder.vhx, las interfaces de red se configuran con DHCP.

    ![](media/asdk-prepare-host/3.PNG)

  > [!IMPORTANT]
  > Si no se especifican las credenciales de administrador local en este paso, se necesitará acceso directo o de KVM al host después de que el equipo se reinicie como parte de la configuración del kit de desarrollo.

7. Si ha elegido una configuración de IP estática en el paso anterior, ahora debe:
    - Seleccionar un adaptador de red. Asegúrese de que puede conectarse al adaptador antes de hacer clic en **Siguiente**.
    - Comprobar que los valores de **dirección IP**, **puerta de enlace**, y **DNS** valores son correctos y, a continuación, hacer clic en **Siguiente**.
13. Haga clic en **Siguiente** para iniciar el proceso de preparación.
14. Cuando la preparación indique **Completado**, haga clic en **Siguiente**.
15. Haga clic en **Reiniciar ahora** para iniciar el equipo host del kit de desarrollo en cloudbuilder.vhdx y [continuar el proceso de implementación](asdk-install.md).

    ![](media/asdk-prepare-host/4.PNG)


## <a name="next-steps"></a>Pasos siguientes
[Instalación del Kit de desarrollo de Azure Stack](asdk-install.md)
