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
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-hello-reporting-api"></a>Használati jelentések az Azure AD B2C segítségével hello reporting API elérése

Az Azure Active Directory B2C (az Azure AD B2C) alapján a felhasználói bejelentkezés és az Azure multi-factor Authentication hitelesítést nyújt. Hitelesítését a végfelhasználók számára az alkalmazás termékcsalád identitás-szolgáltatóktól között. Amikor tudja, hogy hello száma típusonként, hello bérlői, általa használt tooregister és hello számát hello szolgáltatók regisztrált felhasználók a válasz hasonló kérdések:
* Az identitásszolgáltató (például a Microsoft vagy LinkedIn fiókok) különböző típusú hány felhasználó regisztrált a hello 10 napja?
* Hány hitelesítések multi-factor Authentication használatának sikeresen befejeződött az utolsó hónapban hello?
* Hány bejelentkezési-a-alapú hitelesítés befejeződtek ebben a hónapban? Napi? Alkalmazásonként?
* Hogyan tudom megbecsülheti hello várható havi költségét saját Azure AD B2C bérlő tevékenység?

Ez a cikk foglalkozik kötött jelentések toobilling tevékenység, felhasználók, számlázható bejelentkezési-a-alapú hitelesítés és többtényezős hitelesítés hello száma alapján.


## <a name="prerequisites"></a>Előfeltételek
Mielőtt hozzáfogna, toocomplete hello lépéseket kell [Előfeltételek tooaccess hello Azure AD jelentéskészítési API-k](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/). Hozzon létre egy alkalmazást, a titkos kulcs beszerzése és azt a hozzáférés biztosítása a rights tooyour az Azure AD B2C bérlő jelentéseket. *Parancsfájl bash* és *Python-parancsfájl* példákat itt is megadja. 

## <a name="powershell-script"></a>PowerShell-szkript
Ez a parancsfájl hello segítségével hello négy használati jelentések létrehozását mutatja be `TimeStamp` paraméter és hello `ApplicationId` szűrő.

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


## <a name="usage-report-definitions"></a>Használati jelentésdefiníciók
* **tenantUserCount**: hello identitásszolgáltató hello napi típusú hello bérlő felhasználók számának utolsó 30 nap. (Ha szükséges, egy `TimeStamp` szűrő biztosít felhasználószám egy megadott dátum toohello az aktuális dátum). hello a jelentés tartalmazza:
  * **TotalUserCount**: hello minden felhasználói objektumok száma.
  * **OtherUserCount**: hello Azure Active Directory-felhasználók (nem az Azure AD B2C felhasználók) száma.
  * **LocalUserCount**: hello és hitelesítő adatok helyi toohello az Azure AD B2C bérlő létrehozása az Azure AD B2C-felhasználói fiókok számát.

* **AlternateIdUserCount**: külső Identitásszolgáltatók regisztrálva az Azure AD B2C felhasználók hello száma (például Facebook, a Microsoft-fiók vagy egy másik Azure Active Directory-bérlőt, más néven tooas egy `OrgId`).

* **b2cAuthenticationCountSummary**: hello napi számát számlázható keresztül hello 30 napja, nap és a hitelesítési folyamat típusú összefoglalását.

* **b2cAuthenticationCount**: hello hitelesítési események száma egy időtartamon belül. hello alapértelmezett érték hello utolsó 30 nap.  (Nem kötelező: hello nyitó és záró `TimeStamp` paraméterek megadása egy adott időszakban.) hello kimenete `StartTimeStamp` (ennél a bérlőnél tevékenység legkorábbi dátum) és `EndTimeStamp` (legújabb frissítés).

* **b2cMfaRequestCountSummary**: hello napi száma a multi-factor Authentication hitelesítések nap és típusa (SMS vagy hangos) összefoglalását.


## <a name="limitations"></a>Korlátozások
Felhasználói számának adatai too48 24 óránként frissül. Hitelesítési események frissülnek naponta több alkalommal. Hello használatakor `ApplicationId` szűrő, egy üres jelentés válasz miatt lehet tooone az alábbi feltételek:
  * hello Alkalmazásazonosító hello bérlő nem létezik. Győződjön meg arról, hogy helyes-e.
  * hello Alkalmazásazonosító létezik, de a jelentési időszak hello adatot nem található. Ellenőrizze a dátum/idő paramétereket.


## <a name="next-steps"></a>Következő lépések
### <a name="monthly-bill-estimates-for-azure-ad"></a>Havi számlán az Azure AD becslése
Együtt [hello a legfrissebb Azure AD B2C díjszabási elérhető](https://azure.microsoft.com/pricing/details/active-directory-b2c/), megbecsülheti napi, heti és havi Azure-használatát.  Becsült különösen fontos, ha azt tervezi, hogy a bérlő működését, ami hatással lehet a teljes költség szempontjából. Tekintse át a tényleges költségek a [Azure-előfizetéssel társított](active-directory-b2c-how-to-enable-billing.md).

### <a name="options-for-other-output-formats"></a>Egyéb formátumokban beállításai
hello következő kód bemutatja, elküldi a kimeneti tooJSON, a név-érték listáját és az XML-példák:
```powershell
# toooutput tooJSON use following line in hello PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# toooutput hello content tooa name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# toooutput hello content in XML use hello following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
