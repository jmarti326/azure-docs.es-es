---
title: Integración y desarrollo continuos con las herramientas de Stream Analytics
description: En este artículo se describe cómo usar las herramientas de Visual Studio para Azure Stream Analytics para configurar un proceso de integración e implementación continuas.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/27/2017
ms.openlocfilehash: 567e2f850e2c51a6103dc24b91d139042d58acb3
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986839"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-tools"></a>Integración y desarrollo continuos con las herramientas de Stream Analytics
En este artículo se describe cómo usar las herramientas de Azure Stream Analytics para Visual Studio para configurar un proceso de integración e implementación continuas.

Use la versión 2.3.0000.0 o superior de las [herramientas de Stream Analytics para Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio) para obtener compatibilidad para MSBuild.

También hay un paquete NuGet disponible: [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). Proporciona las herramientas de implementación, ejecución local y MSBuild que admite la integración continua y el proceso de implementación de los proyectos Stream Analytics para Visual Studio. 
> [!NOTE] 
El paquete NuGet solo se puede usar con la versión 2.3.0000.0 o superior de las herramientas de Stream Analytics para Visual Studio. Si tiene proyectos creados en versiones anteriores de las herramientas de Visual Studio, ábralos con la versión 2.3.0000.0 o superior y guárdelos. A continuación, se habilitarán las nuevas funcionalidades. 

Para más información, consulte [Uso de herramientas de Azure Stream Analytics para Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="msbuild"></a>MSBuild
Al igual que con la experiencia estándar de Visual Studio MSBuild, para compilar un proyecto tiene dos opciones. Puede hacer clic con el botón derecho en el proyecto y, después, elegir **Compilar**. También puede usar **MSBuild** en el paquete de NuGet desde la línea de comandos.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Cuando un proyecto de Stream Analytics para Visual Studio se compila correctamente, genera los dos siguientes archivos de plantilla de Azure Resource Manager en la carpeta **bin/[Debug/Retail]/Deploy**: 

*  Archivo de plantilla de Resource Manager

       [ProjectName].JobTemplate.json 

*  Archivo de parámetros de Resource Manager

       [ProjectName].JobTemplate.parameters.json   

Los parámetros predeterminados en el archivo parameters.json provienen de la configuración del proyecto de Visual Studio. Si quiere implementar en otro entorno, reemplace los parámetros según corresponda.

> [!NOTE] 
En todas las credenciales, los valores predeterminados están establecidos en null. Es *imperativo* establecer los valores antes de realizar la implementación en la nube.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Obtenga más información acerca de cómo [implementar con un archivo de plantilla de Resource Manager y Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Obtenga más información acerca de cómo [utilizar un objeto como un parámetro en una plantilla de Resource Manager](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).


## <a name="command-line-tool"></a>Herramienta de línea de comandos

### <a name="build-the-project"></a>Compilación del proyecto
El paquete NuGet tiene una herramienta de línea de comandos denominada **SA.exe**. Admite la compilación del proyecto y las pruebas locales en una máquina arbitraria, que puede usar en el proceso de integración y entrega continuas. 

De manera predeterminada, los archivos de implementación se colocan en el directorio actual. Puede especificar la ruta de acceso de salida mediante el uso del parámetro -OutputPath.

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Prueba local del script

Si el proyecto tiene archivos de entrada locales especificados en Visual Studio, puede ejecutar una prueba de script automatizada con el comando *localrun*. El resultado de salida se coloca en el directorio actual.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-a-job-definition-file-to-use-with-the-stream-analytics-powershell-api"></a>Genere un archivo de definición del trabajo para usarlo con la API de Stream Analytics PowerShell

El comando *arm* toma la plantilla de trabajo y los archivos de parámetro de la plantilla de trabajo que se generaron como entrada en la compilación. Luego los combina en un archivo JSON de definición de trabajo que se puede usar con la API de PowerShell de Stream Analytics.

```
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```
Ejemplo:
```
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```


