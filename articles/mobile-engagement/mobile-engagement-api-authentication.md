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
# <a name="authenticate-with-mobile-engagement-rest-apis"></a><span data-ttu-id="da4b3-103">Azokkal a Mobilmarketing REST API-k</span><span class="sxs-lookup"><span data-stu-id="da4b3-103">Authenticate with Mobile Engagement REST APIs</span></span>
## <a name="overview"></a><span data-ttu-id="da4b3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="da4b3-104">Overview</span></span>
<span data-ttu-id="da4b3-105">Ez a dokumentum azt ismerteti, hogyan tooget egy érvényes AAD Oauth jogkivonatot a Mobile Engagement REST API-k hello tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="da4b3-105">This document describes how tooget a valid AAD Oauth token tooauthenticate with hello Mobile Engagement REST APIs.</span></span> 

<span data-ttu-id="da4b3-106">Feltételezzük, hogy egy érvényes Azure-előfizetéssel rendelkezik, és a Mobile Engagement-alkalmazáshoz egyikének használatával hozott létre a [fejlesztői oktatóanyagok](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="da4b3-106">It is assumed that you have a valid Azure subscription and you have created a Mobile Engagement app using one of our [Developer Tutorials](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>

## <a name="authentication"></a><span data-ttu-id="da4b3-107">Authentication</span><span class="sxs-lookup"><span data-stu-id="da4b3-107">Authentication</span></span>
<span data-ttu-id="da4b3-108">A Microsoft Azure Active Directory jogkivonat-hitelesítéshez használt OAuth-alapú.</span><span class="sxs-lookup"><span data-stu-id="da4b3-108">A Microsoft Azure Active Directory based OAuth token is used for authentication.</span></span> 

<span data-ttu-id="da4b3-109">Rendelés az API-k tooauthentication kérelemben egy engedélyezési fejléc hozzá kell adni tooevery kérelmet, amely a következő űrlap hello:</span><span class="sxs-lookup"><span data-stu-id="da4b3-109">In order tooauthentication an API request, an authorization header must be added tooevery request which is of hello following form:</span></span>

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> <span data-ttu-id="da4b3-110">Az Azure Active Directory-jogkivonatokat 1 óra múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="da4b3-110">Azure Active Directory tokens expire in 1 hour.</span></span>
> 
> 

<span data-ttu-id="da4b3-111">Nincsenek számos módon tooget jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="da4b3-111">There are several ways tooget a token.</span></span> <span data-ttu-id="da4b3-112">Hello API-k általában az egy felhőszolgáltatás nevezik, mivel azt szeretné, toouse API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="da4b3-112">Since hello APIs are generally called from a cloud service, you want toouse an API key.</span></span> <span data-ttu-id="da4b3-113">API-kulcs Azure terminológia a szolgáltatás egyszerű jelszó nevezik.</span><span class="sxs-lookup"><span data-stu-id="da4b3-113">An API key in Azure terminology is called a Service principal password.</span></span> <span data-ttu-id="da4b3-114">hello alábbi eljárás ismerteti egyirányú toosetting, akár manuálisan.</span><span class="sxs-lookup"><span data-stu-id="da4b3-114">hello following procedure describes one way toosetting it up manually.</span></span>

### <a name="one-time-setup-using-script"></a><span data-ttu-id="da4b3-115">Egyszeri telepítő (parancsfájl használatával)</span><span class="sxs-lookup"><span data-stu-id="da4b3-115">One-time setup (using script)</span></span>
<span data-ttu-id="da4b3-116">Hello készlete alábbi tooperform hello beállítása hello a telepítés minimális időt vesz igénybe, de hello legfeljebb megengedett alapértékeit használja a PowerShell parancsfájl használatával utasításokat kell követnie.</span><span class="sxs-lookup"><span data-stu-id="da4b3-116">You should follow hello set of instructions below tooperform hello setup using a PowerShell script which takes hello minimum time for setup but uses hello most permissible defaults.</span></span> <span data-ttu-id="da4b3-117">Szükség esetén is követheti hello hello utasításait [manuális telepítési módra](mobile-engagement-api-authentication-manual.md) mindezt hello közvetlenül az Azure portálon, és tegye egyeztetését.</span><span class="sxs-lookup"><span data-stu-id="da4b3-117">Optionally, you can also follow hello instructions in hello [manual setup](mobile-engagement-api-authentication-manual.md) for doing this from hello Azure portal directly and do finer configuration.</span></span> 

1. <span data-ttu-id="da4b3-118">Az Azure PowerShell legújabb verziójának hello [Itt](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="da4b3-118">Get hello latest version of Azure PowerShell from [here](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="da4b3-119">Hello letöltési utasításokat olvashat, erre úgy tekinthet [hivatkozás](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="da4b3-119">For more information on hello download instructions, you can see this [link](/powershell/azure/overview).</span></span>  
2. <span data-ttu-id="da4b3-120">Azure PowerShell telepítése után a használja hello következő parancsokat, hogy rendelkezik-e hello tooensure **Azure modul** telepítve:</span><span class="sxs-lookup"><span data-stu-id="da4b3-120">Once Azure PowerShell is installed, use hello following commands tooensure that you have hello **Azure module** installed:</span></span>
   
    <span data-ttu-id="da4b3-121">a.</span><span class="sxs-lookup"><span data-stu-id="da4b3-121">a.</span></span> <span data-ttu-id="da4b3-122">Ellenőrizze, hogy hello Azure PowerShell modul érhető el a rendelkezésre álló hello listájához.</span><span class="sxs-lookup"><span data-stu-id="da4b3-122">Make sure hello Azure PowerShell module is available in hello list of available modules.</span></span> 
   
        Get-Module –ListAvailable 
   
    ![Elérhető az Azure-modulok][1]
   
    <span data-ttu-id="da4b3-124">b.</span><span class="sxs-lookup"><span data-stu-id="da4b3-124">b.</span></span> <span data-ttu-id="da4b3-125">Ha a lista fölött hello hello Azure PowerShell modul nem találja majd kell toorun hello következő:</span><span class="sxs-lookup"><span data-stu-id="da4b3-125">If you do not find hello Azure PowerShell module in hello above list then you need toorun hello following:</span></span>
   
        Import-Module Azure 
3. <span data-ttu-id="da4b3-126">Bejelentkezési toohello Azure Resource Manager powershellből való futtatásával a következő parancsot, és a felhasználónév és jelszó megadása az Azure-fiókjával hello:</span><span class="sxs-lookup"><span data-stu-id="da4b3-126">Login toohello Azure Resource Manager from PowerShell by running hello following command and providing your user name and password for your Azure account:</span></span> 
   
        Login-AzureRmAccount
4. <span data-ttu-id="da4b3-127">Ha több előfizetéssel rendelkezik futtassuk hello következő:</span><span class="sxs-lookup"><span data-stu-id="da4b3-127">If you have multiple subscriptions then you should run hello following:</span></span>
   
    <span data-ttu-id="da4b3-128">a.</span><span class="sxs-lookup"><span data-stu-id="da4b3-128">a.</span></span> <span data-ttu-id="da4b3-129">Az előfizetések és a példány hello SubscriptionId kívánt hello előfizetés listájának lekérése toouse.</span><span class="sxs-lookup"><span data-stu-id="da4b3-129">Get a list of all your subscriptions and copy hello SubscriptionId of hello subscription you want toouse.</span></span> <span data-ttu-id="da4b3-130">Ellenőrizze, hogy ehhez az előfizetéshez ugyanarra a számítógépre, amelyen a Mobile Engagement-alkalmazáshoz, amely fog hello hello használata toointeract hello API-k.</span><span class="sxs-lookup"><span data-stu-id="da4b3-130">Make sure this subscription is hello same one which has hello Mobile Engagement App which you are going toointeract with using hello APIs.</span></span> 
   
        Get-AzureRmSubscription
   
    <span data-ttu-id="da4b3-131">b.</span><span class="sxs-lookup"><span data-stu-id="da4b3-131">b.</span></span> <span data-ttu-id="da4b3-132">Futtatási hello következő olyan hello SubscriptionId tooconfigure hello előfizetés toobe használt parancsot.</span><span class="sxs-lookup"><span data-stu-id="da4b3-132">Run hello following command providing hello SubscriptionId tooconfigure hello subscription toobe used.</span></span>
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. <span data-ttu-id="da4b3-133">Hello hello másolatot [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) tooyour helyi számítógép parancsfájlt, és mentse egy PowerShell-parancsmag (pl. `APIAuth.ps1`), majd hajtsa végre az `.\APIAuth.ps1`.</span><span class="sxs-lookup"><span data-stu-id="da4b3-133">Copy hello text for hello [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script tooyour local machine and save it as a PowerShell cmdlet (e.g. `APIAuth.ps1`) and execute it `.\APIAuth.ps1`.</span></span> 
6. <span data-ttu-id="da4b3-134">hello parancsfájl ekkor megkérdezi, hogy a megadott számot tooprovide **egyszerű név**.</span><span class="sxs-lookup"><span data-stu-id="da4b3-134">hello script will ask you tooprovide an input for **principalName**.</span></span> <span data-ttu-id="da4b3-135">Adjon meg egy megfelelő nevet, amelyet toouse toocreate az Active Directory-alkalmazás (pl. APIAuth).</span><span class="sxs-lookup"><span data-stu-id="da4b3-135">Provide a suitable name here that you want toouse toocreate your Active Directory application (e.g. APIAuth).</span></span> 
7. <span data-ttu-id="da4b3-136">Hello parancsfájl befejezése után megjelenik-e a következő négy érték, amelyre szüksége lesz hello tooauthenticate AD-val programozott módon, ezért győződjön meg arról, hogy toocopy őket.</span><span class="sxs-lookup"><span data-stu-id="da4b3-136">After hello script completes, it will display hello following four values that you will need tooauthenticate programmatically with AD so make sure toocopy them.</span></span> 
   
    <span data-ttu-id="da4b3-137">**A TenantId**, **SubscriptionId**, **ApplicationId**, és **titkos**.</span><span class="sxs-lookup"><span data-stu-id="da4b3-137">**TenantId**, **SubscriptionId**, **ApplicationId**, and **Secret**.</span></span>
   
    <span data-ttu-id="da4b3-138">Fogja használni, a TenantId `{TENANT_ID}`, mint ApplicationId `{CLIENT_ID}` és titkos, `{CLIENT_SECRET}`.</span><span class="sxs-lookup"><span data-stu-id="da4b3-138">You will use TenantId as `{TENANT_ID}`, ApplicationId as `{CLIENT_ID}` and Secret as `{CLIENT_SECRET}`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="da4b3-139">Az alapértelmezett biztonsági házirend letiltása a PowerShell-parancsfájlok futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="da4b3-139">Your default security policy may block you from running a PowerShell scripts.</span></span> <span data-ttu-id="da4b3-140">Ha igen, ideiglenesen adja meg a végrehajtási házirend tooallow parancsfájl végrehajtása a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="da4b3-140">If so, you temporarily configure your execution policy tooallow script execution using hello following command:</span></span>
   > 
   > <span data-ttu-id="da4b3-141">Set-ExecutionPolicy RemoteSigned</span><span class="sxs-lookup"><span data-stu-id="da4b3-141">Set-ExecutionPolicy RemoteSigned</span></span>
   > 
   > 
8. <span data-ttu-id="da4b3-142">Ez hogyan PS parancsmagok készlete hello jelenne meg.</span><span class="sxs-lookup"><span data-stu-id="da4b3-142">Here is how hello set of PS cmdlets would look like.</span></span> 
   
    ![][3]
9. <span data-ttu-id="da4b3-143">Ellenőrizze, hogy egy új AD-alkalmazást létrejött hello névvel rendelkező, megadott toohello parancsfájl nevű hello Azure felügyeleti portálon **egyszerű név** alatt **megjelenítése, a vállalat tulajdonában lévő alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="da4b3-143">Check in hello Azure Management portal that a new AD application was created with hello name you provided toohello script called **principalName** under **Show Applications my company owns**.</span></span>
   
    ![][4]

#### <a name="steps-tooget-a-valid-token"></a><span data-ttu-id="da4b3-144">Egy érvényes tokent lépéseket tooget</span><span class="sxs-lookup"><span data-stu-id="da4b3-144">Steps tooget a valid token</span></span>
1. <span data-ttu-id="da4b3-145">Hívja meg a következő paraméterek hello hello API, és győződjön meg arról, hogy tooreplace hello BÉRLŐI\_azonosító, az ügyfél\_azonosító és az ügyfél\_titkos kulcs:</span><span class="sxs-lookup"><span data-stu-id="da4b3-145">Call hello API with hello following parameters and make sure tooreplace hello TENANT\_ID, CLIENT\_ID and CLIENT\_SECRET:</span></span>
   
   * <span data-ttu-id="da4b3-146">**Kérelem URL-CÍMÉT** , *https://login.microsoftonline.com/ {BÉRLŐI\_azonosító} / oauth2/token*</span><span class="sxs-lookup"><span data-stu-id="da4b3-146">**Request URL** as *https://login.microsoftonline.com/{TENANT\_ID}/oauth2/token*</span></span>
   * <span data-ttu-id="da4b3-147">**HTTP Content-Type fejléc** , *alkalmazás/x-www-form-urlencoded*</span><span class="sxs-lookup"><span data-stu-id="da4b3-147">**HTTP Content-Type header** as *application/x-www-form-urlencoded*</span></span>
   * <span data-ttu-id="da4b3-148">**HTTP-kérelem törzse** , *biztosítani\_típusú ügyfél =\_hitelesítő adatok & client_id = {ügyfél\_azonosító} & client_secret = {ügyfél\_titkos} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span><span class="sxs-lookup"><span data-stu-id="da4b3-148">**HTTP Request Body** as *grant\_type=client\_credentials&client_id={CLIENT\_ID}&client_secret={CLIENT\_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span></span>
     
     <span data-ttu-id="da4b3-149">hello az alábbiakban látható egy példa egy kérelem:</span><span class="sxs-lookup"><span data-stu-id="da4b3-149">hello following is an example request:</span></span>
     
       <span data-ttu-id="da4b3-150">POST / {TENANT_ID} / oauth2/token HTTP/1.1 állomás: login.microsoftonline.com Content-Type: alkalmazás/x-www-form-urlencoded grant_type client_credentials & client_id = {CLIENT_ID} = & client_secret = {CLIENT_SECRET} & fel forrás https 3A % = 2f%2Fmanagement.Core.Windows.NET%2f</span><span class="sxs-lookup"><span data-stu-id="da4b3-150">POST /{TENANT_ID}/oauth2/token HTTP/1.1   Host: login.microsoftonline.com   Content-Type: application/x-www-form-urlencoded   grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso   urce=https%3A%2F%2Fmanagement.core.windows.net%2F</span></span>
     
     <span data-ttu-id="da4b3-151">Íme egy példa egy válasz:</span><span class="sxs-lookup"><span data-stu-id="da4b3-151">Here is an example response:</span></span>
     
       <span data-ttu-id="da4b3-152">A HTTP/1.1 200 OK Content-Type: az application/json; charset = utf-8 Content-Length: 1234</span><span class="sxs-lookup"><span data-stu-id="da4b3-152">HTTP/1.1 200 OK   Content-Type: application/json; charset=utf-8   Content-Length: 1234</span></span>
     
       <span data-ttu-id="da4b3-153">{"token_type": "Tulajdonos", "expires_in": "3599", "expires_on": "1445395811", "not_before": "144 5391911","erőforrás": "https://management.core.windows.net/", "access_token": {ACCESS_TOKEN}}</span><span class="sxs-lookup"><span data-stu-id="da4b3-153">{"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144   5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}</span></span>
     
     <span data-ttu-id="da4b3-154">Ebben a példában szereplő URL-kódolást hello a FELADÁS egy vagy több paraméterek `resource` érték `https://management.core.windows.net/`.</span><span class="sxs-lookup"><span data-stu-id="da4b3-154">This example included URL encoding of hello POST parameters, `resource` value is actually `https://management.core.windows.net/`.</span></span> <span data-ttu-id="da4b3-155">Legyen óvatos tooalso URL-cím kódolása `{CLIENT_SECRET}` , különleges karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="da4b3-155">Be careful tooalso URL encode `{CLIENT_SECRET}` as it may contain special characters.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="da4b3-156">Tesztelési, használhatja egy HTTP-ügyfél eszköz, például az [Fiddler](http://www.telerik.com/fiddler) vagy [Chrome Postman bővítmény](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span><span class="sxs-lookup"><span data-stu-id="da4b3-156">For testing, you can use an HTTP client tool like [Fiddler](http://www.telerik.com/fiddler) or [Chrome Postman extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span></span> 
     > 
     > 
2. <span data-ttu-id="da4b3-157">Most már minden API-hívásban hello engedélyezési kérelem fejléce tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="da4b3-157">Now in every API call, include hello authorization request header:</span></span>
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    <span data-ttu-id="da4b3-158">Ha egy 401-es állapotkód lett visszaadva, akkor ellenőrizze hello adott válasz törzsének, azt előfordulhat, hogy meghatározzák a hello-token érvényessége lejárt.</span><span class="sxs-lookup"><span data-stu-id="da4b3-158">If you get a 401 status code returned, check hello response body, it might tell you hello token is expired.</span></span> <span data-ttu-id="da4b3-159">Ebben az esetben be egy új token.</span><span class="sxs-lookup"><span data-stu-id="da4b3-159">In that case, get a new token.</span></span>

## <a name="using-hello-apis"></a><span data-ttu-id="da4b3-160">Hello API-k használatával</span><span class="sxs-lookup"><span data-stu-id="da4b3-160">Using hello APIs</span></span>
<span data-ttu-id="da4b3-161">Most, hogy egy érvényes tokent, készen áll a toomake hello API-hívások áll.</span><span class="sxs-lookup"><span data-stu-id="da4b3-161">Now that you have a valid token, you are ready toomake hello API calls.</span></span>

1. <span data-ttu-id="da4b3-162">Az egyes API-kérések szüksége lesz a toopass hello előző szakaszban beszerzett érvényes, nem lejárt token.</span><span class="sxs-lookup"><span data-stu-id="da4b3-162">In each API request, you will need toopass a valid, unexpired token which you obtained in hello previous section.</span></span>
2. <span data-ttu-id="da4b3-163">Szüksége lesz az egyes paraméterek tooplug hello kérelem URI, amely azonosítja az alkalmazást a.</span><span class="sxs-lookup"><span data-stu-id="da4b3-163">You will need tooplug in some parameters into hello request URI which identifies your application.</span></span> <span data-ttu-id="da4b3-164">hello kérelem URI-azonosítója a következőhöz hasonló, hello</span><span class="sxs-lookup"><span data-stu-id="da4b3-164">hello request URI looks like hello following</span></span>
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    <span data-ttu-id="da4b3-165">tooget hello paraméterek, kattintson a az alkalmazás nevét, majd kattintson az irányítópult és az oldal akkor jelenik meg, például az összes hello következő hello 3 paraméter.</span><span class="sxs-lookup"><span data-stu-id="da4b3-165">tooget hello parameters, click on your application name and click Dashboard and you will see a page like hello following with all hello 3 parameters.</span></span>
   
   * <span data-ttu-id="da4b3-166">**1** `{subscription-id}`</span><span class="sxs-lookup"><span data-stu-id="da4b3-166">**1** `{subscription-id}`</span></span>
   * <span data-ttu-id="da4b3-167">**2** `{app-collection}`</span><span class="sxs-lookup"><span data-stu-id="da4b3-167">**2** `{app-collection}`</span></span>
   * <span data-ttu-id="da4b3-168">**3** `{app-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="da4b3-168">**3** `{app-resource-name}`</span></span>
   * <span data-ttu-id="da4b3-169">**4** az erőforráscsoport neve lesz toobe **MobileEngagement** kivéve, ha létrehozott egy új.</span><span class="sxs-lookup"><span data-stu-id="da4b3-169">**4** Your Resource Group name is going toobe **MobileEngagement** unless you created a new one.</span></span> 
     
     ![Mobile Engagement API URI-paraméterek][2]

> [!NOTE]
> <br/>
> 
> 1. <span data-ttu-id="da4b3-171">Ez volt a hello figyelmen kívül a hello API legfelső szintű cím előző API-k.</span><span class="sxs-lookup"><span data-stu-id="da4b3-171">Ignore hello API Root Address as this was for hello previous APIs.</span></span><br/>
> 2. <span data-ttu-id="da4b3-172">Ha hello alkalmazást a klasszikus Azure portál használatával hozott létre majd szüksége toouse hello alkalmazás erőforrás nevét, amely eltér attól az hello alkalmazásnév magát.</span><span class="sxs-lookup"><span data-stu-id="da4b3-172">If you created hello app using Azure Classic portal then you need toouse hello Application Resource name which is different than hello Application name itself.</span></span> <span data-ttu-id="da4b3-173">Hello app hello Azure-portálon létrehozott majd használjon hello alkalmazásnév magát (nincs alkalmazás-Erőfforás neve és alkalmazásnév hello új portálon létrehozott alkalmazások közötti különbséget tenni).</span><span class="sxs-lookup"><span data-stu-id="da4b3-173">If you created hello app in hello Azure Portal then you should use hello App Name itself (there is no differentiation between Application Resource Name and App Name for apps created in hello new portal).</span></span>  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



