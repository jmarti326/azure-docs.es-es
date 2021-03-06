---
title: 'Tutorial: Integración de Azure Active Directory con Bonusly | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Bonusly.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 175c00d36491fbf43149aef9a590219b330581c1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422984"
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Tutorial: Integración de Azure Active Directory con Bonusly

En este tutorial, obtendrá información sobre cómo integrar Bonusly con Azure Active Directory (Azure AD).

La integración de Bonusly con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Bonusly.
- Puede permitir que los usuarios inicien sesión automáticamente en Bonusly (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Bonusly, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Bonusly

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Bonusly desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-bonusly-from-the-gallery"></a>Adición de Bonusly desde la galería
Para configurar la integración de Bonusly en Azure AD, debe agregar Bonusly desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Bonusly desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Bonusly**, seleccione **Bonusly** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Bonusly en la lista de resultados](./media/bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD
En esta sección, puede configurar y probar el inicio de sesión único de Azure AD con Bonusly con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Bonusly para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Bonusly.

Para establecer la relación de vínculo, en Bonusly, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Bonusly, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Bonusly](#create-a-bonusly-test-user)**: para tener un homólogo de Britta Simon en Bonusly que esté vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Bonusly.

**Para configurar el inicio de sesión único de Azure AD con Bonusly, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Bonusly**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/bonus-tutorial/tutorial_bonusly_samlbase.png)

1. En la sección **Dominio y direcciones URL de Bonusly**, lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Bonusly](./media/bonus-tutorial/tutorial_bonusly_url.png)

    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://Bonus.ly/saml/<tenant-name>`.

    > [!NOTE] 
    > Este valor no es real. Actualícelo con la dirección URL de respuesta real. Póngase en contacto con el [equipo de soporte técnico al cliente de Bonusly](https://bonus.ly/contact) para obtener estos valores.
 
1. En la sección **Certificado de firma de SAML**, copie el valor de **HUELLA DIGITAL** del certificado.

    ![Vínculo de descarga del certificado](./media/bonus-tutorial/tutorial_bonusly_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/bonus-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Bonusly**, haga clic en **Configurar Bonusly** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **SAML Entity ID y SAML Single Sign-On Service URL** (Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML) de la sección **Referencia rápida**.

    ![Configuración de Bonusly](./media/bonus-tutorial/tutorial_bonusly_configure.png) 

1. En una ventana del explorador diferente, inicie sesión en su inquilino de **Bonusly**.

1. En la barra de herramientas de la parte superior, haga clic en **Configuración** y seleccione **Integraciones y aplicaciones**.
   
    ![Sección Bonusly Social](./media/bonus-tutorial/ic773686.png "Bonusly")
1. En **Inicio de sesión único**, seleccione **SAML**.

1. En la página de diálogo **SAML** , realice los pasos siguientes:
   
    ![Página de diálogo Saml Bonusly](./media/bonus-tutorial/ic773687.png "Bonusly")
   
    a. En el cuadro de texto **IdP SSO Target URL** (Dirección URL de destino de SSO de IdP), pegue el valor de la **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que ha copiado de Azure Portal.
   
    b. En el cuadro de texto **IdP Issuer** (Emisor de IdP), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal. 

    c. En el cuadro de texto **IdP Login URL** (Dirección URL de inicio de sesión de IdP), pegue el valor de la **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que ha copiado de Azure Portal.

    d. Pegue el valor de **Huella digital** de Azure Portal en el cuadro de texto **Cert Fingerprint** (Huella digital de certificado).
   
1. Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/bonus-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/bonus-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Botón Agregar](./media/bonus-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Cuadro de diálogo Usuario](./media/bonus-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-bonusly-test-user"></a>Creación de un usuario de prueba de Bonusly

Para permitir que los usuarios de Azure AD inicien sesión en Bonusly, tienen que aprovisionarse en Bonusly. En el caso de Bonusly, el aprovisionamiento es una tarea manual.

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Bonusly que proporcione Bonusly para aprovisionar cuentas de usuario de Azure Active Directory.
>  

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. En una ventana de explorador web diferente, inicie sesión en su inquilino de Bonusly.

1. Haga clic en **Configuración**.
 
    ![Configuración](./media/bonus-tutorial/ic781041.png "Configuración")

1. Haga clic en la pestaña **Usuarios y bonificaciones** .
   
    ![Usuarios y bonificaciones](./media/bonus-tutorial/ic781042.png "Usuarios y bonificaciones")

1. Haga clic en **Administrar usuarios**.
   
    ![Administración de usuarios](./media/bonus-tutorial/ic781043.png "Administración de usuarios")

1. Haga clic en **Agregar usuario**.
   
    ![Agregar usuario](./media/bonus-tutorial/ic781044.png "Agregar usuario")

1. En el cuadro de diálogo **Agregar usuario** , realice los pasos siguientes:
   
    ![Agregar usuario](./media/bonus-tutorial/ic781045.png "Agregar usuario")  

    a. En el cuadro de texto **Nombre**, escriba el nombre de usuario, en este caso **Britta**.

    b. En el cuadro de texto **Apellidos**, escriba el apellido del usuario, en este caso **Simon**.
 
    c. En el cuadro de texto **E-mail** (Correo electrónico), escriba el correo electrónico del usuario con el siguiente formato **brittasimon@contoso.com**.

    d. Haga clic en **Save**(Guardar).
   
     >[!NOTE]
     >El titular de la cuenta de Azure AD recibirá un mensaje de correo electrónico que incluye un vínculo para confirmar la cuenta antes de que se active.
     >  

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Bonusly.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Bonusly, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Bonusly**.

    ![Vínculo a Bonusly en la lista de aplicaciones](./media/bonus-tutorial/tutorial_bonusly_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso.

Al hacer clic en el icono de Bonusly en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Bonusly.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bonus-tutorial/tutorial_general_01.png
[2]: ./media/bonus-tutorial/tutorial_general_02.png
[3]: ./media/bonus-tutorial/tutorial_general_03.png
[4]: ./media/bonus-tutorial/tutorial_general_04.png

[100]: ./media/bonus-tutorial/tutorial_general_100.png

[200]: ./media/bonus-tutorial/tutorial_general_200.png
[201]: ./media/bonus-tutorial/tutorial_general_201.png
[202]: ./media/bonus-tutorial/tutorial_general_202.png
[203]: ./media/bonus-tutorial/tutorial_general_203.png

