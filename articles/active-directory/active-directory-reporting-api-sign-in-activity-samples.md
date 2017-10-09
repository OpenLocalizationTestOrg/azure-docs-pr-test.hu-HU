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
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a><span data-ttu-id="662ee-103">Azure Active Directory bejelentkezési tevékenység jelentés API-minták</span><span class="sxs-lookup"><span data-stu-id="662ee-103">Azure Active Directory sign-in activity report API samples</span></span>
<span data-ttu-id="662ee-104">Ez a témakör hello Azure Active Directoryval kapcsolatos témakörök gyűjteményét része reporting API-val.</span><span class="sxs-lookup"><span data-stu-id="662ee-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="662ee-105">Az Azure AD jelentéskészítési lehetőséget biztosít, amely lehetővé teszi az API-k tooaccess bejelentkezési tevékenység adatok használata a kódban, illetve a kapcsolódó eszközök.</span><span class="sxs-lookup"><span data-stu-id="662ee-105">Azure AD reporting provides you with an API that enables you tooaccess sign-in activity data using code or related tools.</span></span>  
<span data-ttu-id="662ee-106">hello ebben a témakörben hatóköre példával kódaláírással a hello tooprovide **bejelentkezés tevékenység API**.</span><span class="sxs-lookup"><span data-stu-id="662ee-106">hello scope of this topic is tooprovide you with sample code for hello **sign-in activity API**.</span></span>

<span data-ttu-id="662ee-107">Lásd:</span><span class="sxs-lookup"><span data-stu-id="662ee-107">See:</span></span>

* <span data-ttu-id="662ee-108">[A naplók](active-directory-reporting-azure-portal.md#activity-reports) további információ</span><span class="sxs-lookup"><span data-stu-id="662ee-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>
* <span data-ttu-id="662ee-109">[Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) hello reporting API-val kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="662ee-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="662ee-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="662ee-110">Prerequisites</span></span>
<span data-ttu-id="662ee-111">Hello minták ebben a témakörben használata előtt kell toocomplete hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="662ee-111">Before you can use hello samples in this topic, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>  

## <a name="powershell-script"></a><span data-ttu-id="662ee-112">PowerShell-szkript</span><span class="sxs-lookup"><span data-stu-id="662ee-112">PowerShell script</span></span>
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




## <a name="executing-hello-script"></a><span data-ttu-id="662ee-113">Hello-parancsprogram végrehajtása</span><span class="sxs-lookup"><span data-stu-id="662ee-113">Executing hello script</span></span>
<span data-ttu-id="662ee-114">Egyszer hello parancsfájl szerkesztése, fut azt, és ellenőrizze, hogy hello várt hello naplózási naplók jelentés adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="662ee-114">Once you finish editing hello script, run it and verify that hello expected data from hello Audit logs report is returned.</span></span>

<span data-ttu-id="662ee-115">hello parancsfájl kimeneti hello bejelentkezési jelentés JSON formátumban ad vissza.</span><span class="sxs-lookup"><span data-stu-id="662ee-115">hello script returns output from hello sign-in report in JSON format.</span></span> <span data-ttu-id="662ee-116">Emellett létrehoz egy `SigninActivities.json` hello fájlt azonos kimeneti.</span><span class="sxs-lookup"><span data-stu-id="662ee-116">It also creates an `SigninActivities.json` file with hello same output.</span></span> <span data-ttu-id="662ee-117">Hello parancsfájl tooreturn adatok más jelentések, és nem kell hello formátumokban megjegyzésbe módosításával kísérletezhet.</span><span class="sxs-lookup"><span data-stu-id="662ee-117">You can experiment by modifying hello script tooreturn data from other reports, and comment out hello output formats that you do not need.</span></span>

## <a name="next-steps"></a><span data-ttu-id="662ee-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="662ee-118">Next Steps</span></span>
* <span data-ttu-id="662ee-119">Ebben a témakörben toocustomize hello minták szeretné használni?</span><span class="sxs-lookup"><span data-stu-id="662ee-119">Would you like toocustomize hello samples in this topic?</span></span> <span data-ttu-id="662ee-120">Tekintse meg a hello [Azure Active Directory bejelentkezési tevékenység API-referencia](active-directory-reporting-api-sign-in-activity-reference.md).</span><span class="sxs-lookup"><span data-stu-id="662ee-120">Check out hello [Azure Active Directory sign-in activity API reference](active-directory-reporting-api-sign-in-activity-reference.md).</span></span> 
* <span data-ttu-id="662ee-121">Ha azt szeretné, hogy a teljes áttekintése, használatával toosee hello Azure Active Directory reporting API-val, lásd: [Ismerkedés az Azure Active Directory reporting API-val hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="662ee-121">If you want toosee a complete overview of using hello Azure Active Directory reporting API, see [Getting started with hello Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="662ee-122">Ha szeretne további információk az Azure Active Directory reporting toofind, tekintse meg a hello [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="662ee-122">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

