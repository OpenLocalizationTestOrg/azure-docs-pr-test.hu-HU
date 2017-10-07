---
title: "Az Azure Active Directory B2C: Használati jelentéskészítési API-mintákat és meghatározások |} Microsoft Docs"
description: "Útmutató és a felhasználók, hitelesítés és többtényezős hitelesítés az Azure AD B2C bérlő jelentések témáját minták"
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: f01f0d1d12464e38f892f1fdd5c7408a3b4cd1aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-hello-reporting-api"></a><span data-ttu-id="264cf-103">Használati jelentések az Azure AD B2C segítségével hello reporting API elérése</span><span class="sxs-lookup"><span data-stu-id="264cf-103">Accessing usage reports in Azure AD B2C via hello reporting API</span></span>

<span data-ttu-id="264cf-104">Az Azure Active Directory B2C (az Azure AD B2C) alapján a felhasználói bejelentkezés és az Azure multi-factor Authentication hitelesítést nyújt.</span><span class="sxs-lookup"><span data-stu-id="264cf-104">Azure Active Directory B2C (Azure AD B2C) provides authentication based on user sign-in and Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="264cf-105">Hitelesítését a végfelhasználók számára az alkalmazás termékcsalád identitás-szolgáltatóktól között.</span><span class="sxs-lookup"><span data-stu-id="264cf-105">Authentication is provided for end users of your application family across identity providers.</span></span> <span data-ttu-id="264cf-106">Amikor tudja, hogy hello száma típusonként, hello bérlői, általa használt tooregister és hello számát hello szolgáltatók regisztrált felhasználók a válasz hasonló kérdések:</span><span class="sxs-lookup"><span data-stu-id="264cf-106">When you know hello number of users registered in hello tenant, hello providers they used tooregister, and hello number of authentications by type, you can answer questions like:</span></span>
* <span data-ttu-id="264cf-107">Az identitásszolgáltató (például a Microsoft vagy LinkedIn fiókok) különböző típusú hány felhasználó regisztrált a hello 10 napja?</span><span class="sxs-lookup"><span data-stu-id="264cf-107">How many users from each type of identity provider (for example, a Microsoft or LinkedIn account) have registered in hello last 10 days?</span></span>
* <span data-ttu-id="264cf-108">Hány hitelesítések multi-factor Authentication használatának sikeresen befejeződött az utolsó hónapban hello?</span><span class="sxs-lookup"><span data-stu-id="264cf-108">How many authentications using Multi-Factor Authentication have completed successfully in hello last month?</span></span>
* <span data-ttu-id="264cf-109">Hány bejelentkezési-a-alapú hitelesítés befejeződtek ebben a hónapban?</span><span class="sxs-lookup"><span data-stu-id="264cf-109">How many sign-in-based authentications were completed this month?</span></span> <span data-ttu-id="264cf-110">Napi?</span><span class="sxs-lookup"><span data-stu-id="264cf-110">Per day?</span></span> <span data-ttu-id="264cf-111">Alkalmazásonként?</span><span class="sxs-lookup"><span data-stu-id="264cf-111">Per application?</span></span>
* <span data-ttu-id="264cf-112">Hogyan tudom megbecsülheti hello várható havi költségét saját Azure AD B2C bérlő tevékenység?</span><span class="sxs-lookup"><span data-stu-id="264cf-112">How can I estimate hello expected monthly cost of my Azure AD B2C tenant activity?</span></span>

<span data-ttu-id="264cf-113">Ez a cikk foglalkozik kötött jelentések toobilling tevékenység, felhasználók, számlázható bejelentkezési-a-alapú hitelesítés és többtényezős hitelesítés hello száma alapján.</span><span class="sxs-lookup"><span data-stu-id="264cf-113">This article focuses on reports tied toobilling activity, which is based on hello number of users, billable sign-in-based authentications, and multi-factor authentications.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="264cf-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="264cf-114">Prerequisites</span></span>
<span data-ttu-id="264cf-115">Mielőtt hozzáfogna, toocomplete hello lépéseket kell [Előfeltételek tooaccess hello Azure AD jelentéskészítési API-k](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="264cf-115">Before you get started, you need toocomplete hello steps in [Prerequisites tooaccess hello Azure AD reporting APIs](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span></span> <span data-ttu-id="264cf-116">Hozzon létre egy alkalmazást, a titkos kulcs beszerzése és azt a hozzáférés biztosítása a rights tooyour az Azure AD B2C bérlő jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="264cf-116">Create an application, obtain a secret for it, and grant it access rights tooyour Azure AD B2C tenant’s reports.</span></span> <span data-ttu-id="264cf-117">*Parancsfájl bash* és *Python-parancsfájl* példákat itt is megadja.</span><span class="sxs-lookup"><span data-stu-id="264cf-117">*Bash script* and *Python script* examples are also provided here.</span></span> 

## <a name="powershell-script"></a><span data-ttu-id="264cf-118">PowerShell-szkript</span><span class="sxs-lookup"><span data-stu-id="264cf-118">PowerShell script</span></span>
<span data-ttu-id="264cf-119">Ez a parancsfájl hello segítségével hello négy használati jelentések létrehozását mutatja be `TimeStamp` paraméter és hello `ApplicationId` szűrő.</span><span class="sxs-lookup"><span data-stu-id="264cf-119">This script demonstrates hello creation of four usage reports by using hello `TimeStamp` parameter and hello `ApplicationId` filter.</span></span>

```powershell
# This script will require hello Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"  
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.microsoftonline.com"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"  
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from hello tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for hello report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for hello " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a><span data-ttu-id="264cf-120">Használati jelentésdefiníciók</span><span class="sxs-lookup"><span data-stu-id="264cf-120">Usage report definitions</span></span>
* <span data-ttu-id="264cf-121">**tenantUserCount**: hello identitásszolgáltató hello napi típusú hello bérlő felhasználók számának utolsó 30 nap.</span><span class="sxs-lookup"><span data-stu-id="264cf-121">**tenantUserCount**: hello number of users in hello tenant by type of identity provider, per day in hello last 30 days.</span></span> <span data-ttu-id="264cf-122">(Ha szükséges, egy `TimeStamp` szűrő biztosít felhasználószám egy megadott dátum toohello az aktuális dátum).</span><span class="sxs-lookup"><span data-stu-id="264cf-122">(Optionally, a `TimeStamp` filter provides user counts from a specified date toohello current date).</span></span> <span data-ttu-id="264cf-123">hello a jelentés tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="264cf-123">hello report provides:</span></span>
  * <span data-ttu-id="264cf-124">**TotalUserCount**: hello minden felhasználói objektumok száma.</span><span class="sxs-lookup"><span data-stu-id="264cf-124">**TotalUserCount**: hello number of all user objects.</span></span>
  * <span data-ttu-id="264cf-125">**OtherUserCount**: hello Azure Active Directory-felhasználók (nem az Azure AD B2C felhasználók) száma.</span><span class="sxs-lookup"><span data-stu-id="264cf-125">**OtherUserCount**: hello number of Azure Active Directory users (not Azure AD B2C users).</span></span>
  * <span data-ttu-id="264cf-126">**LocalUserCount**: hello és hitelesítő adatok helyi toohello az Azure AD B2C bérlő létrehozása az Azure AD B2C-felhasználói fiókok számát.</span><span class="sxs-lookup"><span data-stu-id="264cf-126">**LocalUserCount**: hello number of Azure AD B2C user accounts created with credentials local toohello Azure AD B2C tenant.</span></span>

* <span data-ttu-id="264cf-127">**AlternateIdUserCount**: külső Identitásszolgáltatók regisztrálva az Azure AD B2C felhasználók hello száma (például Facebook, a Microsoft-fiók vagy egy másik Azure Active Directory-bérlőt, más néven tooas egy `OrgId`).</span><span class="sxs-lookup"><span data-stu-id="264cf-127">**AlternateIdUserCount**: hello number of Azure AD B2C users registered with external identity providers (for example, Facebook, a Microsoft account, or another Azure Active Directory tenant, also referred tooas an `OrgId`).</span></span>

* <span data-ttu-id="264cf-128">**b2cAuthenticationCountSummary**: hello napi számát számlázható keresztül hello 30 napja, nap és a hitelesítési folyamat típusú összefoglalását.</span><span class="sxs-lookup"><span data-stu-id="264cf-128">**b2cAuthenticationCountSummary**: Summary of hello daily number of billable authentications over hello last 30 days, by day and type of authentication flow.</span></span>

* <span data-ttu-id="264cf-129">**b2cAuthenticationCount**: hello hitelesítési események száma egy időtartamon belül.</span><span class="sxs-lookup"><span data-stu-id="264cf-129">**b2cAuthenticationCount**: hello number of authentications within a time period.</span></span> <span data-ttu-id="264cf-130">hello alapértelmezett érték hello utolsó 30 nap.</span><span class="sxs-lookup"><span data-stu-id="264cf-130">hello default is hello last 30 days.</span></span>  <span data-ttu-id="264cf-131">(Nem kötelező: hello nyitó és záró `TimeStamp` paraméterek megadása egy adott időszakban.) hello kimenete `StartTimeStamp` (ennél a bérlőnél tevékenység legkorábbi dátum) és `EndTimeStamp` (legújabb frissítés).</span><span class="sxs-lookup"><span data-stu-id="264cf-131">(Optional: hello beginning and ending `TimeStamp` parameters define a specific time period.) hello output includes `StartTimeStamp` (earliest date of activity for this tenant) and `EndTimeStamp` (latest update).</span></span>

* <span data-ttu-id="264cf-132">**b2cMfaRequestCountSummary**: hello napi száma a multi-factor Authentication hitelesítések nap és típusa (SMS vagy hangos) összefoglalását.</span><span class="sxs-lookup"><span data-stu-id="264cf-132">**b2cMfaRequestCountSummary**: Summary of hello daily number of multi-factor authentications, by day and type (SMS or voice).</span></span>


## <a name="limitations"></a><span data-ttu-id="264cf-133">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="264cf-133">Limitations</span></span>
<span data-ttu-id="264cf-134">Felhasználói számának adatai too48 24 óránként frissül.</span><span class="sxs-lookup"><span data-stu-id="264cf-134">User count data is refreshed every 24 too48 hours.</span></span> <span data-ttu-id="264cf-135">Hitelesítési események frissülnek naponta több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="264cf-135">Authentications are updated several times a day.</span></span> <span data-ttu-id="264cf-136">Hello használatakor `ApplicationId` szűrő, egy üres jelentés válasz miatt lehet tooone az alábbi feltételek:</span><span class="sxs-lookup"><span data-stu-id="264cf-136">When using hello `ApplicationId` filter, an empty report response can be due tooone of following conditions:</span></span>
  * <span data-ttu-id="264cf-137">hello Alkalmazásazonosító hello bérlő nem létezik.</span><span class="sxs-lookup"><span data-stu-id="264cf-137">hello application ID does not exist in hello tenant.</span></span> <span data-ttu-id="264cf-138">Győződjön meg arról, hogy helyes-e.</span><span class="sxs-lookup"><span data-stu-id="264cf-138">Make sure it is correct.</span></span>
  * <span data-ttu-id="264cf-139">hello Alkalmazásazonosító létezik, de a jelentési időszak hello adatot nem található.</span><span class="sxs-lookup"><span data-stu-id="264cf-139">hello application ID exists, but no data was found in hello reporting period.</span></span> <span data-ttu-id="264cf-140">Ellenőrizze a dátum/idő paramétereket.</span><span class="sxs-lookup"><span data-stu-id="264cf-140">Review your date/time parameters.</span></span>


## <a name="next-steps"></a><span data-ttu-id="264cf-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="264cf-141">Next steps</span></span>
### <a name="monthly-bill-estimates-for-azure-ad"></a><span data-ttu-id="264cf-142">Havi számlán az Azure AD becslése</span><span class="sxs-lookup"><span data-stu-id="264cf-142">Monthly bill estimates for Azure AD</span></span>
<span data-ttu-id="264cf-143">Együtt [hello a legfrissebb Azure AD B2C díjszabási elérhető](https://azure.microsoft.com/pricing/details/active-directory-b2c/), megbecsülheti napi, heti és havi Azure-használatát.</span><span class="sxs-lookup"><span data-stu-id="264cf-143">When combined with [hello most current Azure AD B2C pricing available](https://azure.microsoft.com/pricing/details/active-directory-b2c/), you can estimate daily, weekly, and monthly Azure consumption.</span></span>  <span data-ttu-id="264cf-144">Becsült különösen fontos, ha azt tervezi, hogy a bérlő működését, ami hatással lehet a teljes költség szempontjából.</span><span class="sxs-lookup"><span data-stu-id="264cf-144">An estimate is especially useful when you plan for changes in tenant behavior that might impact overall cost.</span></span> <span data-ttu-id="264cf-145">Tekintse át a tényleges költségek a [Azure-előfizetéssel társított](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="264cf-145">You can review actual costs in your [linked Azure subscription](active-directory-b2c-how-to-enable-billing.md).</span></span>

### <a name="options-for-other-output-formats"></a><span data-ttu-id="264cf-146">Egyéb formátumokban beállításai</span><span class="sxs-lookup"><span data-stu-id="264cf-146">Options for other output formats</span></span>
<span data-ttu-id="264cf-147">hello következő kód bemutatja, elküldi a kimeneti tooJSON, a név-érték listáját és az XML-példák:</span><span class="sxs-lookup"><span data-stu-id="264cf-147">hello following code shows examples of sending output tooJSON, a name value list, and XML:</span></span>
```powershell
# toooutput tooJSON use following line in hello PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# toooutput hello content tooa name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# toooutput hello content in XML use hello following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
