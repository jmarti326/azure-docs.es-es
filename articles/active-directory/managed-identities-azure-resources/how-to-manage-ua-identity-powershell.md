---
title: En este artículo, aprenderá a crear, enumerar y eliminar una identidad administrada que haya asignado el usuario mediante Azure PowerShell.
description: Instrucciones paso a paso para crear, enumerar y eliminar una instancia de identidad administrada que haya asignado el usuario mediante Azure PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: c7191f60b8780e8ccee9b330aa21d8174f0f0148
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106538"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-azure-powershell"></a>Crear, enumerar y eliminar una identidad administrada que haya asignado el usuario mediante Azure PowerShell

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Las identidades administradas de los recursos de Azure proporcionan a los servicios de Azure una identidad administrada en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a crear, enumerar y eliminar una identidad administrada que haya asignado el usuario mediante Azure PowerShell.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad administrada asignada por el sistema y una identidad administrada asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instale [la versión más reciente de Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) si aún no lo ha hecho.
- Si decide instalar y usar PowerShell de forma local, en este tutorial necesitará la versión 5.7.0 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute ` Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps). 
- Si ejecuta PowerShell localmente, también necesita realizar las siguientes operaciones: 
    - Ejecute `Login-AzureRmAccount` para crear una conexión con Azure.
    - Instalar la [versión más reciente de PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Ejecutar `Install-Module -Name PowerShellGet -AllowPrerelease` para obtener la versión preliminar del módulo `PowerShellGet` (es posible que deba usar `Exit` para salir de la sesión actual de PowerShell después de ejecutar este comando para instalar el módulo `AzureRM.ManagedServiceIdentity`).
    - Ejecute `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` para instalar la versión preliminar del módulo `AzureRM.ManagedServiceIdentity` con el fin de realizar las operaciones de identidad administrada que haya asignado el usuario, y que se indican en este artículo.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear, leer (enumerar), actualizar y eliminar una identidad administrada que haya asignado el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para leer (enumerar) las propiedades de una identidad administrada que haya asignado el usuario.

## <a name="create-a-user-assigned-managed-identity"></a>Crear una identidad administrada asignada por el usuario

Para crear una identidad administrada asignada por el usuario, use el comando [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity). El parámetro `ResourceGroupName` especifica el grupo de recursos donde se creará la identidad administrada asignada por el usuario, mientras que el parámetro `-Name` especifica su nombre. Reemplace los valores de los parámetros `<RESOURCE GROUP>` y `<USER ASSIGNED IDENTITY NAME>` con sus propios valores:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Enumerar identidades administradas asignadas por el usuario

Para enumerar las identidades administradas asignadas por el usuario, use el comando [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity).  El parámetro `-ResourceGroupName` especifica el grupo de recursos donde se creó la identidad administrada asignada por el usuario. Reemplace `<RESOURCE GROUP>` por su propio valor:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
En la respuesta, las identidades administradas asignadas por el usuario reciben el valor `"Microsoft.ManagedIdentity/userAssignedIdentities"` para la clave `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Eliminar una identidad administrada asignada por el usuario

Para eliminar una identidad administrada asignada por el usuario, use el comando [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity).  El parámetro `-ResourceGroupName` especifica el grupo de recursos donde se creó la identidad asignada por el usuario, mientras que el parámetro `-Name` especifica su nombre. Reemplace los valores de los parámetros `<RESOURCE GROUP>` y `<USER ASSIGNED IDENTITY NAME>` con sus propios valores:

 ```azurepowershell-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> El proceso de eliminación de una identidad administrada asignada por el usuario no quitará la referencia de ningún recurso al que se haya asignado. Las asignaciones de identidad deben quitarse por separado.

## <a name="next-steps"></a>Pasos siguientes

Para obtener una lista completa y más detalles sobre las identidades administradas de Azure PowerShell para los comandos de los recursos de Azure, consulte [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).
