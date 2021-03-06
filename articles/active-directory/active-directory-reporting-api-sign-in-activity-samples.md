---
title: "Ejemplos de la API de informes de actividad de inicio de sesión de Azure Active Directory | Microsoft Docs"
description: "Introducción a la API de informes de Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e6b1137c8ca33774ef9852b9441b541cf7723ebd
ms.contentlocale: es-es
ms.lasthandoff: 12/28/2016


---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Ejemplos de la API de informe de actividad de inicio de sesión de Azure Active Directory
Este tema forma parte de una serie de temas sobre la API de informes de Azure Active Directory.  
La característica de generación de informes de Azure AD proporciona una API que permite acceder a los datos de actividades de inicio de sesión mediante el uso de código o herramientas relacionadas.  
Este tema se centra en proporcionar el código de ejemplo para la **API de actividad de inicio de sesión**.

Consulte:

* [Registros de auditoría](active-directory-reporting-azure-portal.md#audit-logs) para más información conceptual
* [Introducción a la API de generación de informes de Azure Active Directory](active-directory-reporting-api-getting-started.md) para obtener más información sobre esta API

Para ver preguntas, problemas o comentarios, póngase en contacto con el equipo de [ayuda de informes de AAD](mailto:aadreportinghelp@microsoft.com).

## <a name="prerequisites"></a>Requisitos previos
Para poder usar los ejemplos de este tema, debe completar la [requisitos previos para tener acceso a la API de generación de informes de Azure AD](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>Script de PowerShell
    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"

    $i=0

    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Ejecución del script
Una vez que termine de editar el script, ejecútelo y compruebe que el informe de registros de auditoría devuelve los datos esperados.

El script devuelve la salida del informe de inicio de sesión en formato JSON. También se crea un archivo `SigninActivities.json` con la misma salida. Puede experimentar modificando el script para que devuelva datos de otros informes y convertir en comentario los formatos de salida que no necesite.

## <a name="next-steps"></a>Pasos siguientes
* ¿Quiere personalizar los ejemplos de este tema? Consulte [Azure Active Directory sign-in activity report API samples](active-directory-reporting-api-sign-in-activity-reference.md)(Referencia de la API de la actividad de inicio de sesión de Azure Active Directory). 
* Si quiere obtener una descripción completa del uso de la API de generación de informes de Azure Active Directory, consulte el artículo de [introducción a la API de generación de informes de Azure Active Directory](active-directory-reporting-api-getting-started.md).
* Si quiere obtener más información sobre informes de Azure Active Directory, consulte la [guía de generación de informes de Azure Active Directory](active-directory-reporting-guide.md).  


