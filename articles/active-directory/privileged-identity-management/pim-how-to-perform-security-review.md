---
title: Realización de una revisión de acceso para los roles de directorio de Azure AD en PIM | Microsoft Docs
description: Aprenda a realizar una revisión de acceso de los roles de directorio de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: b4cffbd1ce240e4792fba84581dafb1933c71a62
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188558"
---
# <a name="perform-an-access-review-of-my-azure-ad-directory-roles-in-pim"></a>Realización de una revisión de acceso para los roles de directorio de Azure AD en PIM
Privileged Identity Management de Azure Active Directory (AD) simplifica el modo en que las empresas administra el acceso con privilegios a los recursos de Azure AD y en otros servicios en línea de Microsoft como Office 365 o Microsoft Intune.  

Si se le ha asignado un rol administrativo, el administrador de roles con privilegios de su organización le puede pedir que revise y confirme periódicamente que aún necesita ese rol para su trabajo. Puede que reciba un correo electrónico con un vínculo o que vaya directamente al [Portal de Azure](https://portal.azure.com). Siga los pasos de este artículo para realizar una autorrevisión de los roles asignados.

Si es un administrador de roles con privilegios o un administrador global y está interesado en las revisiones de acceso, puede obtener más información en [How to start an access review](pim-how-to-start-security-review.md) (Cómo iniciar una revisión de acceso).

## <a name="add-the-privileged-identity-management-application"></a>Incorporación de la aplicación Privileged Identity Management
Puede usar la aplicación Privileged Identity Management (PIM) de Azure AD en el [Portal de Azure](https://portal.azure.com/) para realizar la revisión.  Si no tiene la aplicación en el portal, siga estos pasos para comenzar:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. Seleccione su nombre de usuario en la esquina superior derecha del Portal de Azure y elija el directorio donde va a trabajar.
3. Seleccione **Todos los servicios** y use el cuadro de texto Filtro para buscar **Azure AD Privileged Identity Management**.
4. Active **Anclar al panel** y haga clic en **Crear**. Se abrirá la aplicación Privileged Identity Management.

## <a name="approve-or-deny-access"></a>Aprobación o denegación de acceso
Al aprobar o denegar el acceso, lo que hace es indicar al revisor si aún usa o no este rol. Elija **Aprobar** si desea permanecer en el rol o **Denegar** si no va a volver a necesitar el acceso. Su estado no cambiará inmediatamente, hay que esperar a que el revisor aplique los resultados.
Para buscar y completar la revisión de acceso, siga estos pasos:

1. En la aplicación PIM, seleccione **Revisar acceso con privilegios**. Si tienes revisiones de acceso pendientes, aparecerán en la hoja de revisiones de acceso de Azure AD.
2. Seleccione la revisión que quiere completar.
3. A no ser que haya creado la revisión, aparecerá como el único usuario en ella. Seleccione la marca de verificación junto a su nombre.
4. Elija **Aprobar** o **Denegar**. Puede que necesite incluir un motivo de su decisión en el cuadro de texto **Proporcionar una razón** .  
5. Cierre la hoja **Revisar roles de Azure AD** .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Pasos siguientes

- [Realización de una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-perform-access-review.md)