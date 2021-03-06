---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de Facebook mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión en sus aplicaciones a los clientes con cuentas de Facebook mediante Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b1fbf77bfb771bd94ed9acdecb3c54e4c6894687
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182117"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Facebook mediante Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Creación de una aplicación de Facebook

Para usar una cuenta de Facebook como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de Facebook, puede obtenerla en [https://www.facebook.com/](https://www.facebook.com/).

1. Inicie sesión en [Facebook for developers](https://developers.facebook.com/) con las credenciales de su cuenta de Facebook.
2. Si aún no lo ha hecho, debe registrarse como desarrollador de Facebook. Para ello, seleccione **Register** (Registrarte) en la esquina superior derecha de la página, acepte las directivas de Facebook y complete los pasos de registro.
3. Seleccione **My Apps** (Mis aplicaciones) y haga clic en **Add a New App** (Agregar una nueva aplicación). 
4. Especifique el valor de **Display Name** (Nombre para mostrar) y un valor de **Contact Email** (Correo electrónico de contacto) válido.
5. Haga clic en **Create App ID** (Crear id. de aplicación). Es posible que deba aceptar las políticas de la plataforma Facebook y realizar una comprobación de seguridad en línea.
6. Seleccione **Settings** (Configuración)  > **Basic** (Básica).
7. Elija una **Categoría**, por ejemplo, `Business and Pages`. Este valor es obligatorio para Facebook, pero no se usa para Azure AD B2C.
8. En la parte inferior de la página, seleccione **Add Platform** (Agregar plataforma) y, después, seleccione **Website** (Sitio web).
9. En **URL del sitio**, escriba `https://your-tenant-name.b2clogin.com/`, pero sustituya `your-tenant-name` por el nombre del inquilino. Escriba una dirección URL en **Privacy Policy URL** (URL de directiva de privacidad), por ejemplo `http://www.contoso.com`. La dirección URL de directiva es una página que sirve para proporcionar información de privacidad de la aplicación.
10. Seleccione **Save changes** (Guardar los cambios).
11. En la parte superior de la página, copie el valor de **App ID** (Id. de la aplicación). 
12. Haga clic en **Show** (Mostrar) y copie el valor de **App Secret** (Secreto de la aplicación). Use ambos para configurar Facebook como proveedor de identidades de su inquilino. El **secreto de la aplicación** es una credencial de seguridad importante.
13. Seleccione **Products** (Productos) y, después, seleccione **Set up** (Configurar) en **Facebook Login** (Inicio de sesión de Facebook).
14. Seleccione **Settings** (configuración) en **Facebook Login** (Inicio de sesión de Facebook).
15. En **Valid OAuth redirect URIs** (URI de redireccionamiento OAuth válidos), escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Reemplace `your-tenant-name` por el nombre del inquilino. Haga clic en **Save Changes** (Guardar cambios) en la parte inferior de la página.
16. Para que la aplicación de Facebook esté disponible en Azure AD B2C, seleccione **App Review** (Revisión de aplicación), establezca **Make My Application public?** (¿Hacer que mi aplicación sea pública?) en **YES** (Sí) y haga clic en **Confirmar**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configuración de una cuenta de Facebook como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino. 
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Escriba un **nombre**. Por ejemplo, escriba *Facebook*.
6. Haga clic en **Tipo de proveedor de identidades**, seleccione **Facebook** y haga clic en **Aceptar**.
7. Seleccione **Configurar este proveedor de identidades** y escriba el identificador de aplicación que anotó anteriormente como **identificador de cliente** y el secreto de aplicación que registró como **secreto de cliente** de la aplicación de Facebook que creó anteriormente.
8. Haga clic en **Aceptar** y, después, en **Crear** para guardar la configuración de Facebook.