---
title: "Azure Active Directory bejelentkezési tevékenység jelentés API-minták |} Microsoft Docs"
description: "Útmutató az Azure Active Directory Reporting API használatába"
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
ms.openlocfilehash: 7fc2b59fe37ed2ffe85925c457300ef8fd83c3c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a><span data-ttu-id="c9468-103">Azure Active Directory bejelentkezési tevékenység jelentés API-minták</span><span class="sxs-lookup"><span data-stu-id="c9468-103">Azure Active Directory sign-in activity report API samples</span></span>
<span data-ttu-id="c9468-104">Ez a témakör az Azure Active Directory reporting API-val kapcsolatos témakörök gyűjteményét részét képezi.</span><span class="sxs-lookup"><span data-stu-id="c9468-104">This topic is part of a collection of topics about the Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="c9468-105">Az Azure AD jelentéskészítési lehetőséget biztosít az API-k, amely lehetővé teszi a kód vagy a kapcsolódó eszközök bejelentkezési tevékenység adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c9468-105">Azure AD reporting provides you with an API that enables you to access sign-in activity data using code or related tools.</span></span>  
<span data-ttu-id="c9468-106">Ez a témakör a hatóköre használatával látják el a mintakódot az **bejelentkezés tevékenység API**.</span><span class="sxs-lookup"><span data-stu-id="c9468-106">The scope of this topic is to provide you with sample code for the **sign-in activity API**.</span></span>

<span data-ttu-id="c9468-107">Lásd:</span><span class="sxs-lookup"><span data-stu-id="c9468-107">See:</span></span>

* <span data-ttu-id="c9468-108">[A naplók](active-directory-reporting-azure-portal.md#activity-reports) további információ</span><span class="sxs-lookup"><span data-stu-id="c9468-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>
* <span data-ttu-id="c9468-109">[Bevezetés az Azure Active Directory Reporting API használatába](active-directory-reporting-api-getting-started.md) a reporting API-val kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="c9468-109">[Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about the reporting API.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c9468-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c9468-110">Prerequisites</span></span>
<span data-ttu-id="c9468-111">A minták ebben a témakörben használata előtt végre kell hajtania a [Előfeltételek az Azure AD reporting API eléréséhez](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="c9468-111">Before you can use the samples in this topic, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>  

## <a name="powershell-script"></a><span data-ttu-id="c9468-112">PowerShell-szkript</span><span class="sxs-lookup"><span data-stu-id="c9468-112">PowerShell script</span></span>
    # This script will require the Web Application and permissions setup in Azure Active Directory
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
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a><span data-ttu-id="c9468-113">A parancsfájl végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c9468-113">Executing the script</span></span>
<span data-ttu-id="c9468-114">Miután a parancsfájl szerkesztése, futtassa, és győződjön meg arról, hogy a naplózás a várt adatokat naplózza a jelentés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c9468-114">Once you finish editing the script, run it and verify that the expected data from the Audit logs report is returned.</span></span>

<span data-ttu-id="c9468-115">A parancsfájl kimenete a bejelentkezési jelentés JSON formátumban ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c9468-115">The script returns output from the sign-in report in JSON format.</span></span> <span data-ttu-id="c9468-116">Emellett létrehoz egy `SigninActivities.json` az azonos kimenethez fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9468-116">It also creates an `SigninActivities.json` file with the same output.</span></span> <span data-ttu-id="c9468-117">Ha adatokat más jelentések, és a kimeneti formátum, amely nem kell megjegyzésbe a parancsfájl módosításával kísérletezhet.</span><span class="sxs-lookup"><span data-stu-id="c9468-117">You can experiment by modifying the script to return data from other reports, and comment out the output formats that you do not need.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9468-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9468-118">Next Steps</span></span>
* <span data-ttu-id="c9468-119">Szeretné testre szabhatja a minták ebben a témakörben?</span><span class="sxs-lookup"><span data-stu-id="c9468-119">Would you like to customize the samples in this topic?</span></span> <span data-ttu-id="c9468-120">Tekintse meg a [Azure Active Directory bejelentkezési tevékenység API-referencia](active-directory-reporting-api-sign-in-activity-reference.md).</span><span class="sxs-lookup"><span data-stu-id="c9468-120">Check out the [Azure Active Directory sign-in activity API reference](active-directory-reporting-api-sign-in-activity-reference.md).</span></span> 
* <span data-ttu-id="c9468-121">Ha azt szeretné, hogy teljes áttekintése, az Azure Active Directory reporting API használatával [Bevezetés az Azure Active Directory reporting API használatába](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c9468-121">If you want to see a complete overview of using the Azure Active Directory reporting API, see [Getting started with the Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="c9468-122">Ha meg szeretne többet megtudni az Azure Active Directory reporting, tekintse meg a [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="c9468-122">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

