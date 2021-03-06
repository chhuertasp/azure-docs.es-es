---
title: "Tutorial: Integración de Azure Active Directory con Tidemark | Microsoft Docs"
description: "Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Tidemark."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 5cf80d4e-6e8b-48ec-81c8-27872af5e5d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 7cc133d6289bffbc3b7fc591104bc51ebfc67ddd
ms.openlocfilehash: 9c3927b64009c51fb8236e42fa9e6137a9909a4a
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-tidemark"></a>Tutorial: integración de Azure Active Directory con Tidemark
El objetivo de este tutorial es mostrar cómo integrar Tidemark con Azure Active Directory (Azure AD).

Integrar Tidemark con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Tidemark.
* Puede permitir que los usuarios inicien sesión automáticamente en Tidemark (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: el Portal de Azure clásico.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Requisitos previos
Para configurar la integración de Azure AD con Tidemark, necesita los siguientes elementos:

* Una suscripción de Azure AD
* Una suscripción habilitada para el inicio de sesión único (SSO) en Tidemark

>[!NOTE]
>Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción. 
> 

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

* No debe usar el entorno de producción, a menos que sea necesario.
* Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
El objetivo de este tutorial es permitirle probar el inicio de sesión único de Azure AD en un entorno de prueba. 

La situación descrita en este tutorial consta de dos bloques de creación principales:

* Adición de Tidemark desde la galería
* Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="add-tidemark-from-the-gallery"></a>Adición de Tidemark desde la galería
Para configurar la integración de Tidemark en Azure AD, será preciso que agregue Tidemark desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Tidemark desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo del **Portal de Azure clásico**, haga clic en **Active Directory**. 
   
    ![Active Directory][1]
2. En la lista **Directory** , seleccione el directorio cuya integración desee habilitar.
3. Para abrir la vista de aplicaciones, haga clic en **Applications** , en el menú superior de la vista de directorios.
   
    ![Applications][2]
4. Haga clic en **Agregar** en la parte inferior de la página.
   
    ![Aplicaciones][3]
5. En el cuadro de diálogo **¿Qué desea hacer?**, haga clic en **Agregar una aplicación de la galería**.
   
    ![Aplicaciones][4]
6. En el cuadro de búsqueda, escriba **Tidemark**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_01.png)
7. En el panel de resultados, seleccione **Tidemark** y haga clic en **Completar** para agregar la aplicación.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_02.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD
El objetivo de esta sección es mostrar cómo configurar y probar el inicio de sesión único de Azure AD con Tidemark con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Tidemark para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Tidemark.

Esta relación de vínculo se establece mediante la asignación del valor del **nombre de usuario** en Azure AD como valor del **nombre de usuario** en Tidemark.

Para configurar y probar el inicio de sesión único de Azure AD con Tidemark, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-single-sign-on)**: para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Tidemark](#creating-a-tidemark-test-user)** : para tener un homólogo de Britta Simon en Tidemark vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD
El objetivo de esta sección es habilitar el inicio de sesión único de Azure AD en el Portal de Azure clásico y configurar el inicio de sesión único en la aplicación Tidemark.

**Para configurar el inicio de sesión único de Azure AD con Tidemark, realice los pasos siguientes:**

1. En el Portal de Azure clásico, en la página de integración de la aplicación **Tidemark**, haga clic en **Configurar inicio de sesión único** para abrir el cuadro de diálogo **Configurar inicio de sesión único**.
   
    ![Configurar inicio de sesión único][6] 
2. En la página **¿Cómo desea que los usuarios inicien sesión en Tidemark?**, seleccione **Inicio de sesión único de Azure AD** y haga clic en **Siguiente**.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_03.png) 
3. En la página de diálogo **Configurar las opciones de la aplicación** , realice los pasos siguientes:
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_04.png)
  * En el cuadro de texto URL de inicio de sesión, escriba la dirección URL de inicio de sesión utilizada por los usuarios para iniciar sesión en la aplicación Tidemark con los siguientes patrones: **“https://contoso.tidemark.com/login”** o **"https://contoso.tidemark.net/login"**.
4. En la página **Configure single sign-on at Tidemark** (Configurar inicio de sesión único en Tidemark), siga estos pasos:
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_05.png)   
  1. Haga clic en **Descargar certificado**y después guarde el archivo en el equipo.
  2. Haga clic en **Siguiente**.
5. Para configurar SSO para la aplicación, póngase en contacto con el equipo de soporte técnico de Tidemark y adjunte el archivo de certificado descargado al correo electrónico. Además, proporcione la dirección URL de inicio de sesión único de SAML, la dirección URL de cierre de sesión y la dirección URL del emisor para que se puedan configurar para la integración de SSO.
6. En el Portal de Azure clásico, seleccione la confirmación de la configuración de inicio de sesión único y haga clic en **Siguiente**.
   
    ![Inicio de sesión único de Azure AD ][10]
7. En la página **Confirmación del inicio de sesión único**, haga clic en **Completar**.  
   
    ![Inicio de sesión único de Azure AD ][11]

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en el Portal de Azure clásico llamado Britta Simon.

![Creación de un usuario de Azure AD][20]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo del **Portal de Azure clásico**, haga clic en **Active Directory**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_09.png) 
2. En la lista **Directory** , seleccione el directorio cuya integración desee habilitar.
3. Para mostrar la lista de usuarios, en el menú de la parte superior, haga clic en **Usuarios**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_03.png) 
4. Para abrir el cuadro de diálogo **Agregar usuario**, en la barra de herramientas de la parte inferior, haga clic en **Agregar usuario**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_04.png) 
5. En la página de diálogo **Proporcione información sobre este usuario** , realice los pasos siguientes:
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_05.png) 
  1. En Tipo de usuario, seleccione Nuevo usuario de la organización.
  2. En el cuadro de texto **Nombre de usuario**, escriba**BrittaSimon**.
  3. Haga clic en **Siguiente**.
6. En la página de diálogo **Perfil de usuario** , realice los pasos siguientes:
   
   ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_06.png) 
  1. En el cuadro de texto **Nombre**, escriba **Britta**. 
  2. En el cuadro de texto **Apellidos**, escriba **Simon**.
  3. En el cuadro de texto **Nombre para mostrar**, escriba **Britta Simon**.   
  4. En la lista **Rol**, seleccione **Usuario**.
  5. Haga clic en **Siguiente**.
7. En el cuadro de diálogo **Obtener contraseña temporal**, haga clic en **Crear**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_07.png) 
8. En la página de diálogo **Obtener contraseña temporal** , realice los pasos siguientes:
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_08.png) 
  1. Anote el valor del campo **Nueva contraseña**.
  2. Haga clic en **Complete**.   

### <a name="create-a-tidemark-test-user"></a>Creación de un usuario de prueba de Tidemark
El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Tidemark. Trabaje con el equipo de soporte técnico de Tidemark para agregar los usuarios en la cuenta de Tidemark. 

>[!NOTE]
>Si necesita crear manualmente un usuario, es preciso que se ponga contacto con el equipo de soporte técnico de Tidemark. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD
El objetivo de esta sección es permitir que Britta Simon use el inicio de sesión único de Azure, para lo cual se le concederá acceso a Tidemark.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Tidemark, realice los pasos siguientes:**

1. En el Portal de Azure clásico, para abrir la vista de aplicaciones, en la vista del directorio, haga clic en **Aplicaciones** en el menú superior.
   
    ![Asignar usuario][201] 
2. En la lista de aplicaciones, seleccione **Tidemark**.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_50.png) 
3. En el menú de la parte superior, haga clic en **Usuarios**.
   
    ![Asignar usuario][203] 
4. En la lista Usuarios, seleccione **Britta Simon**.
5. En la barra de herramientas de la parte inferior, haga clic en **Asignar**.
   
    ![Asignar usuario][205]

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único
El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso.

Al hacer clic en el icono de Tidemark en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Tidemark.

## <a name="additional-resources"></a>Recursos adicionales
* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_205.png

