---
title: a Mobile Engagement REST API-k aaaAuthenticate
description: Ismerteti, hogyan tooauthenticate Azure Mobile Engagement REST API-khoz
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 9b54aa5ec3da4bcf55ffe5b7e8d1759095d0c486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Azokkal a Mobilmarketing REST API-k
## <a name="overview"></a>Áttekintés
Ez a dokumentum azt ismerteti, hogyan tooget egy érvényes AAD Oauth jogkivonatot a Mobile Engagement REST API-k hello tooauthenticate. 

Feltételezzük, hogy egy érvényes Azure-előfizetéssel rendelkezik, és a Mobile Engagement-alkalmazáshoz egyikének használatával hozott létre a [fejlesztői oktatóanyagok](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Authentication
A Microsoft Azure Active Directory jogkivonat-hitelesítéshez használt OAuth-alapú. 

Rendelés az API-k tooauthentication kérelemben egy engedélyezési fejléc hozzá kell adni tooevery kérelmet, amely a következő űrlap hello:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Az Azure Active Directory-jogkivonatokat 1 óra múlva lejár.
> 
> 

Nincsenek számos módon tooget jogkivonatot. Hello API-k általában az egy felhőszolgáltatás nevezik, mivel azt szeretné, toouse API-kulcs. API-kulcs Azure terminológia a szolgáltatás egyszerű jelszó nevezik. hello alábbi eljárás ismerteti egyirányú toosetting, akár manuálisan.

### <a name="one-time-setup-using-script"></a>Egyszeri telepítő (parancsfájl használatával)
Hello készlete alábbi tooperform hello beállítása hello a telepítés minimális időt vesz igénybe, de hello legfeljebb megengedett alapértékeit használja a PowerShell parancsfájl használatával utasításokat kell követnie. Szükség esetén is követheti hello hello utasításait [manuális telepítési módra](mobile-engagement-api-authentication-manual.md) mindezt hello közvetlenül az Azure portálon, és tegye egyeztetését. 

1. Az Azure PowerShell legújabb verziójának hello [Itt](http://aka.ms/webpi-azps). Hello letöltési utasításokat olvashat, erre úgy tekinthet [hivatkozás](/powershell/azure/overview).  
2. Azure PowerShell telepítése után a használja hello következő parancsokat, hogy rendelkezik-e hello tooensure **Azure modul** telepítve:
   
    a. Ellenőrizze, hogy hello Azure PowerShell modul érhető el a rendelkezésre álló hello listájához. 
   
        Get-Module –ListAvailable 
   
    ![Elérhető az Azure-modulok][1]
   
    b. Ha a lista fölött hello hello Azure PowerShell modul nem találja majd kell toorun hello következő:
   
        Import-Module Azure 
3. Bejelentkezési toohello Azure Resource Manager powershellből való futtatásával a következő parancsot, és a felhasználónév és jelszó megadása az Azure-fiókjával hello: 
   
        Login-AzureRmAccount
4. Ha több előfizetéssel rendelkezik futtassuk hello következő:
   
    a. Az előfizetések és a példány hello SubscriptionId kívánt hello előfizetés listájának lekérése toouse. Ellenőrizze, hogy ehhez az előfizetéshez ugyanarra a számítógépre, amelyen a Mobile Engagement-alkalmazáshoz, amely fog hello hello használata toointeract hello API-k. 
   
        Get-AzureRmSubscription
   
    b. Futtatási hello következő olyan hello SubscriptionId tooconfigure hello előfizetés toobe használt parancsot.
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Hello hello másolatot [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) tooyour helyi számítógép parancsfájlt, és mentse egy PowerShell-parancsmag (pl. `APIAuth.ps1`), majd hajtsa végre az `.\APIAuth.ps1`. 
6. hello parancsfájl ekkor megkérdezi, hogy a megadott számot tooprovide **egyszerű név**. Adjon meg egy megfelelő nevet, amelyet toouse toocreate az Active Directory-alkalmazás (pl. APIAuth). 
7. Hello parancsfájl befejezése után megjelenik-e a következő négy érték, amelyre szüksége lesz hello tooauthenticate AD-val programozott módon, ezért győződjön meg arról, hogy toocopy őket. 
   
    **A TenantId**, **SubscriptionId**, **ApplicationId**, és **titkos**.
   
    Fogja használni, a TenantId `{TENANT_ID}`, mint ApplicationId `{CLIENT_ID}` és titkos, `{CLIENT_SECRET}`.
   
   > [!NOTE]
   > Az alapértelmezett biztonsági házirend letiltása a PowerShell-parancsfájlok futtatásakor. Ha igen, ideiglenesen adja meg a végrehajtási házirend tooallow parancsfájl végrehajtása a következő parancs hello használata:
   > 
   > Set-ExecutionPolicy RemoteSigned
   > 
   > 
8. Ez hogyan PS parancsmagok készlete hello jelenne meg. 
   
    ![][3]
9. Ellenőrizze, hogy egy új AD-alkalmazást létrejött hello névvel rendelkező, megadott toohello parancsfájl nevű hello Azure felügyeleti portálon **egyszerű név** alatt **megjelenítése, a vállalat tulajdonában lévő alkalmazások**.
   
    ![][4]

#### <a name="steps-tooget-a-valid-token"></a>Egy érvényes tokent lépéseket tooget
1. Hívja meg a következő paraméterek hello hello API, és győződjön meg arról, hogy tooreplace hello BÉRLŐI\_azonosító, az ügyfél\_azonosító és az ügyfél\_titkos kulcs:
   
   * **Kérelem URL-CÍMÉT** , *https://login.microsoftonline.com/ {BÉRLŐI\_azonosító} / oauth2/token*
   * **HTTP Content-Type fejléc** , *alkalmazás/x-www-form-urlencoded*
   * **HTTP-kérelem törzse** , *biztosítani\_típusú ügyfél =\_hitelesítő adatok & client_id = {ügyfél\_azonosító} & client_secret = {ügyfél\_titkos} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*
     
     hello az alábbiakban látható egy példa egy kérelem:
     
       POST / {TENANT_ID} / oauth2/token HTTP/1.1 állomás: login.microsoftonline.com Content-Type: alkalmazás/x-www-form-urlencoded grant_type client_credentials & client_id = {CLIENT_ID} = & client_secret = {CLIENT_SECRET} & fel forrás https 3A % = 2f%2Fmanagement.Core.Windows.NET%2f
     
     Íme egy példa egy válasz:
     
       A HTTP/1.1 200 OK Content-Type: az application/json; charset = utf-8 Content-Length: 1234
     
       {"token_type": "Tulajdonos", "expires_in": "3599", "expires_on": "1445395811", "not_before": "144 5391911","erőforrás": "https://management.core.windows.net/", "access_token": {ACCESS_TOKEN}}
     
     Ebben a példában szereplő URL-kódolást hello a FELADÁS egy vagy több paraméterek `resource` érték `https://management.core.windows.net/`. Legyen óvatos tooalso URL-cím kódolása `{CLIENT_SECRET}` , különleges karaktereket tartalmazhat.
     
     > [!NOTE]
     > Tesztelési, használhatja egy HTTP-ügyfél eszköz, például az [Fiddler](http://www.telerik.com/fiddler) vagy [Chrome Postman bővítmény](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. Most már minden API-hívásban hello engedélyezési kérelem fejléce tartalmazza:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Ha egy 401-es állapotkód lett visszaadva, akkor ellenőrizze hello adott válasz törzsének, azt előfordulhat, hogy meghatározzák a hello-token érvényessége lejárt. Ebben az esetben be egy új token.

## <a name="using-hello-apis"></a>Hello API-k használatával
Most, hogy egy érvényes tokent, készen áll a toomake hello API-hívások áll.

1. Az egyes API-kérések szüksége lesz a toopass hello előző szakaszban beszerzett érvényes, nem lejárt token.
2. Szüksége lesz az egyes paraméterek tooplug hello kérelem URI, amely azonosítja az alkalmazást a. hello kérelem URI-azonosítója a következőhöz hasonló, hello
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    tooget hello paraméterek, kattintson a az alkalmazás nevét, majd kattintson az irányítópult és az oldal akkor jelenik meg, például az összes hello következő hello 3 paraméter.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** az erőforráscsoport neve lesz toobe **MobileEngagement** kivéve, ha létrehozott egy új. 
     
     ![Mobile Engagement API URI-paraméterek][2]

> [!NOTE]
> <br/>
> 
> 1. Ez volt a hello figyelmen kívül a hello API legfelső szintű cím előző API-k.<br/>
> 2. Ha hello alkalmazást a klasszikus Azure portál használatával hozott létre majd szüksége toouse hello alkalmazás erőforrás nevét, amely eltér attól az hello alkalmazásnév magát. Hello app hello Azure-portálon létrehozott majd használjon hello alkalmazásnév magát (nincs alkalmazás-Erőfforás neve és alkalmazásnév hello új portálon létrehozott alkalmazások közötti különbséget tenni).  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



