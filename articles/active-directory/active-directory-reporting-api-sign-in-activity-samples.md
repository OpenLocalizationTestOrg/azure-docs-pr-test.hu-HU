---
title: "az Active Directory-bejelentkezés aaaAzure tevékenység jelentés API minták |} Microsoft Docs"
description: "Hogyan a tooget hello Azure Active Directory Reporting API használatába"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d4fbbea95fe0b52828673b997681ae37481e21bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory bejelentkezési tevékenység jelentés API-minták
Ez a témakör hello Azure Active Directoryval kapcsolatos témakörök gyűjteményét része reporting API-val.  
Az Azure AD jelentéskészítési lehetőséget biztosít, amely lehetővé teszi az API-k tooaccess bejelentkezési tevékenység adatok használata a kódban, illetve a kapcsolódó eszközök.  
hello ebben a témakörben hatóköre példával kódaláírással a hello tooprovide **bejelentkezés tevékenység API**.

Lásd:

* [A naplók](active-directory-reporting-azure-portal.md#activity-reports) további információ
* [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) hello reporting API-val kapcsolatos további információk.


## <a name="prerequisites"></a>Előfeltételek
Hello minták ebben a témakörben használata előtt kell toocomplete hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>PowerShell-szkript
    # This script will require hello Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.microsoftonline.com/"
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
        Write-Output "Save hello output tooa file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-hello-script"></a>Hello-parancsprogram végrehajtása
Egyszer hello parancsfájl szerkesztése, fut azt, és ellenőrizze, hogy hello várt hello naplózási naplók jelentés adatokat ad vissza.

hello parancsfájl kimeneti hello bejelentkezési jelentés JSON formátumban ad vissza. Emellett létrehoz egy `SigninActivities.json` hello fájlt azonos kimeneti. Hello parancsfájl tooreturn adatok más jelentések, és nem kell hello formátumokban megjegyzésbe módosításával kísérletezhet.

## <a name="next-steps"></a>Következő lépések
* Ebben a témakörben toocustomize hello minták szeretné használni? Tekintse meg a hello [Azure Active Directory bejelentkezési tevékenység API-referencia](active-directory-reporting-api-sign-in-activity-reference.md). 
* Ha azt szeretné, hogy a teljes áttekintése, használatával toosee hello Azure Active Directory reporting API-val, lásd: [Ismerkedés az Azure Active Directory reporting API-val hello](active-directory-reporting-api-getting-started.md).
* Ha szeretne további információk az Azure Active Directory reporting toofind, tekintse meg a hello [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).  

