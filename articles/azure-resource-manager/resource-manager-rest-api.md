---
title: aaaResource Manager REST API-k |} Microsoft Docs
description: "Hello Resource Manager REST API-k hitelesítés és használati példák áttekintése"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 3ccc3575c5e06c41f2fdc5317711980fc6a2f649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-manager-rest-apis"></a><span data-ttu-id="f6903-103">Erőforrás-kezelői REST API-k</span><span class="sxs-lookup"><span data-stu-id="f6903-103">Resource Manager REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f6903-104">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6903-104">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="f6903-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f6903-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="f6903-106">Portál</span><span class="sxs-lookup"><span data-stu-id="f6903-106">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="f6903-107">REST API</span><span class="sxs-lookup"><span data-stu-id="f6903-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="f6903-108">Minden hívás tooAzure erőforrás-kezelő mögött minden telepített sablon mögött minden konfigurált tárfiók mögött van egy vagy több hívások toohello Azure Resource Manager RESTful API.</span><span class="sxs-lookup"><span data-stu-id="f6903-108">Behind every call tooAzure Resource Manager, behind every deployed template, behind every configured storage account there are one or more calls toohello Azure Resource Manager's RESTful API.</span></span> <span data-ttu-id="f6903-109">Ez a témakör az fordított toothose API-t, és hogyan hívása őket bármely SDK minden használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="f6903-109">This topic is devoted toothose APIs and how you can call them without using any SDK at all.</span></span> <span data-ttu-id="f6903-110">Ezt a módszert akkor hasznos, ha azt szeretné, hogy teljes körű hozzáférést engedélyezzenek kérelmek tooAzure, vagy ha az elsődleges nyelvhez SDK hello nem érhető el, vagy nem támogatja a szükséges hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="f6903-110">This approach is useful if you want full control of requests tooAzure, or if hello SDK for your preferred language is not available or doesn't support hello operations you need.</span></span>

<span data-ttu-id="f6903-111">Ez a cikk nem lép az Azure-ban van közzétéve, de ahelyett, hogy használja az egyes műveletek példa arra, hogyan kapcsolódnak a toothem minden API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="f6903-111">This article does not go through every API that is exposed in Azure, but rather uses some operations as examples of how you connect toothem.</span></span> <span data-ttu-id="f6903-112">Hello alapok elsajátítása után el tudja olvasni hello [Azure Resource Manager REST API-referencia](https://docs.microsoft.com/rest/api/resources/) toofind részletes információk hogyan toouse hello részeinek hello API-k.</span><span class="sxs-lookup"><span data-stu-id="f6903-112">After you understand hello basics, you can read hello [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) toofind detailed information on how toouse hello rest of hello APIs.</span></span>

## <a name="authentication"></a><span data-ttu-id="f6903-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="f6903-113">Authentication</span></span>
<span data-ttu-id="f6903-114">Hitelesítés az erőforrás-kezelő által Azure Active Directory (AD) kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="f6903-114">Authentication for Resource Manager is handled by Azure Active Directory (AD).</span></span> <span data-ttu-id="f6903-115">tooconnect tooany API, először az Azure AD tooreceive egy hitelesítési jogkivonatot, amelyek átadhatók tooevery kérésre tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="f6903-115">tooconnect tooany API, you first need tooauthenticate with Azure AD tooreceive an authentication token that you can pass on tooevery request.</span></span> <span data-ttu-id="f6903-116">Mivel azt egy tiszta hívás olvashat részletesen, közvetlenül a REST API-k toohello, feltételezzük, hogy nem szeretné, hogy tooauthenticate által meg kell adni egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="f6903-116">As we are describing a pure call directly toohello REST APIs, we assume that you don't want tooauthenticate by being prompted for a username and password.</span></span> <span data-ttu-id="f6903-117">Azt is feltételezzük, hogy nem használ két többtényezős hitelesítési mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="f6903-117">We also assume you are not using two factor authentication mechanisms.</span></span> <span data-ttu-id="f6903-118">Ezért létrehozhatunk egy Azure AD-alkalmazást és egy egyszerű, amelyek a használt toolog úgynevezett.</span><span class="sxs-lookup"><span data-stu-id="f6903-118">Therefore, we create what is called an Azure AD Application and a service principal that are used toolog in.</span></span> <span data-ttu-id="f6903-119">Ne feledje, hogy az Azure AD támogatja a több hitelesítési eljárás, és az összes, de lehet használt tooretrieve, hogy további kérelmeknél API szükséges hitelesítési tokent.</span><span class="sxs-lookup"><span data-stu-id="f6903-119">But remember that Azure AD support several authentication procedures and all of them could be used tooretrieve that authentication token that we need for subsequent API requests.</span></span>
<span data-ttu-id="f6903-120">Hajtsa végre a [létrehozása Azure AD-alkalmazást és egy egyszerű szolgáltatást](resource-group-create-service-principal-portal.md) részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f6903-120">Follow [Create Azure AD Application and Service Principal](resource-group-create-service-principal-portal.md) for step by step instructions.</span></span>

### <a name="generating-an-access-token"></a><span data-ttu-id="f6903-121">Egy hozzáférési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6903-121">Generating an Access Token</span></span>
<span data-ttu-id="f6903-122">Hitelesítés engedélyezésébe az Azure AD létesítő tooAzure AD, login.microsoftonline.com helyen történik. tooauthenticate, kell toohave hello a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="f6903-122">Authentication against Azure AD is done by calling out tooAzure AD, located at login.microsoftonline.com. tooauthenticate, you need toohave hello following information:</span></span>

* <span data-ttu-id="f6903-123">Az Azure AD bérlő azonosítója (hello nevét, hogy az Azure AD a toolog használ, gyakran hello ugyanaz, mint a vállalat, de nem szükséges)</span><span class="sxs-lookup"><span data-stu-id="f6903-123">Azure AD Tenant ID (hello name of that Azure AD you are using toolog in, often hello same as your company but not necessary)</span></span>
* <span data-ttu-id="f6903-124">Alkalmazásazonosító (hello Azure AD-alkalmazás létrehozása lépés során végrehajtott)</span><span class="sxs-lookup"><span data-stu-id="f6903-124">Application ID (taken during hello Azure AD application creation step)</span></span>
* <span data-ttu-id="f6903-125">Jelszó (hello Azure AD-alkalmazás létrehozása során kiválasztott)</span><span class="sxs-lookup"><span data-stu-id="f6903-125">Password (that you selected while creating hello Azure AD Application)</span></span>

<span data-ttu-id="f6903-126">A következő HTTP-kérelem hello győződjön meg arról, hogy tooreplace "Azure AD bérlő azonosítója", "Alkalmazásazonosító" és a "Password" hello megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="f6903-126">In hello following HTTP request, make sure tooreplace "Azure AD Tenant ID", "Application ID" and "Password" with hello correct values.</span></span>

<span data-ttu-id="f6903-127">**Általános HTTP-kérelem:**</span><span class="sxs-lookup"><span data-stu-id="f6903-127">**Generic HTTP Request:**</span></span>

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

<span data-ttu-id="f6903-128">... (Ha a hitelesítés sikeres) lesz a válasz a következő válasz hasonló toohello eredményezi:</span><span class="sxs-lookup"><span data-stu-id="f6903-128">... will (if authentication succeeds) result in a response similar toohello following response:</span></span>

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
<span data-ttu-id="f6903-129">(a válasz megelőző hello hello access_token rövidített tooincrease olvashatóság volt)</span><span class="sxs-lookup"><span data-stu-id="f6903-129">(hello access_token in hello preceding response have been shortened tooincrease readability)</span></span>

<span data-ttu-id="f6903-130">**Létrehozása a Bash használatával:**</span><span class="sxs-lookup"><span data-stu-id="f6903-130">**Generating access token using Bash:**</span></span>

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

<span data-ttu-id="f6903-131">**Létrehozása a PowerShell használatával:**</span><span class="sxs-lookup"><span data-stu-id="f6903-131">**Generating access token using PowerShell:**</span></span>

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

<span data-ttu-id="f6903-132">hello válasz egy hozzáférési jogkivonatot, információ arról, hogy mennyi ideig, hogy a jogkivonat érvényes, és milyen erőforrás is használhatja, hogy a jogkivonat kapcsolatos információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f6903-132">hello response contains an access token, information about how long that token is valid, and information about what resource you can use that token for.</span></span>
<span data-ttu-id="f6903-133">az összes kérelem toohello Resource Manager API hello kapott hozzáférési jogkivonat segítségével hello előző HTTP hívásban kell átadni.</span><span class="sxs-lookup"><span data-stu-id="f6903-133">hello access token you received in hello previous HTTP call must be passed in for all request toohello Resource Manager API.</span></span> <span data-ttu-id="f6903-134">"Engedélyezés" hello "Tulajdonosi YOUR_ACCESS_TOKEN" értékű nevű fejléc értékeként adja át.</span><span class="sxs-lookup"><span data-stu-id="f6903-134">You pass it as a header value named "Authorization" with hello value "Bearer YOUR_ACCESS_TOKEN".</span></span> <span data-ttu-id="f6903-135">Figyelje meg a "Tulajdonos" és a hozzáférési token hello térköze.</span><span class="sxs-lookup"><span data-stu-id="f6903-135">Notice hello space between "Bearer" and your access token.</span></span>

<span data-ttu-id="f6903-136">Ahogy fent HTTP hello látja, hello lexikális elem érvénytelen a meghatározott időn időintervalluma, gyorsítótárba, és használja fel, hogy ugyanezt a tokent.</span><span class="sxs-lookup"><span data-stu-id="f6903-136">As you can see from hello above HTTP Result, hello token is valid for a specific period of time during which you should cache and reuse that same token.</span></span> <span data-ttu-id="f6903-137">Akkor is, ha lehetséges tooauthenticate szemben Azure ad-val minden API-hívás, nagyon hatékony lenne.</span><span class="sxs-lookup"><span data-stu-id="f6903-137">Even if it is possible tooauthenticate against Azure AD for each API call, it would be highly inefficient.</span></span>

## <a name="calling-resource-manager-rest-apis"></a><span data-ttu-id="f6903-138">Hívása Resource Manager REST API-k</span><span class="sxs-lookup"><span data-stu-id="f6903-138">Calling Resource Manager REST APIs</span></span>
<span data-ttu-id="f6903-139">Ez a témakör csak néhány API-k tooexplain hello használatának alapjaival hello REST műveleteinek használja.</span><span class="sxs-lookup"><span data-stu-id="f6903-139">This topic only uses a few APIs tooexplain hello basic usage of hello REST operations.</span></span> <span data-ttu-id="f6903-140">Minden hello műveletekkel kapcsolatos információkért lásd: [Azure Resource Manager REST API-k](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="f6903-140">For information about all hello operations, see [Azure Resource Manager REST APIs](https://docs.microsoft.com/rest/api/resources/).</span></span>

### <a name="list-all-subscriptions"></a><span data-ttu-id="f6903-141">Minden előfizetés felsorolása</span><span class="sxs-lookup"><span data-stu-id="f6903-141">List all subscriptions</span></span>
<span data-ttu-id="f6903-142">Hello teheti legegyszerűbb műveletek egyike toolist hello az elérhető előfizetések keresztül elérhető.</span><span class="sxs-lookup"><span data-stu-id="f6903-142">One of hello simplest operations you can do is toolist hello available subscriptions that you can access.</span></span> <span data-ttu-id="f6903-143">A kérelem a következő hello láthatja, hogyan hello hozzáférési jogkivonat érték az átadott fejléc:</span><span class="sxs-lookup"><span data-stu-id="f6903-143">In hello following request, you see how hello access token is passed in as a header:</span></span>

<span data-ttu-id="f6903-144">(Cserélje YOUR_ACCESS_TOKEN a tényleges hozzáférési jogkivonat.)</span><span class="sxs-lookup"><span data-stu-id="f6903-144">(Replace YOUR_ACCESS_TOKEN with your actual Access Token.)</span></span>

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="f6903-145">... és eredményeként kapott előfizetések listáját, hogy a szolgáltatás egyszerű tooaccess engedélyezett</span><span class="sxs-lookup"><span data-stu-id="f6903-145">... and as a result, you get a list of subscriptions that this service principal is allowed tooaccess</span></span>

<span data-ttu-id="f6903-146">(Előfizetés-azonosítók rendelkezik lettek rövidítve olvashatóság érdekében)</span><span class="sxs-lookup"><span data-stu-id="f6903-146">(Subscription IDs have been shortened for readability)</span></span>

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a><span data-ttu-id="f6903-147">Egy adott előfizetés összes erőforráscsoportok felsorolása</span><span class="sxs-lookup"><span data-stu-id="f6903-147">List all resource groups in a specific subscription</span></span>
<span data-ttu-id="f6903-148">Hello Resource Manager API-k elérhető összes erőforrás egy erőforráscsoportban van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="f6903-148">All resources available with hello Resource Manager APIs are nested inside a resource group.</span></span> <span data-ttu-id="f6903-149">Meglévő erőforráscsoport erőforrás-kezelő lekérheti az előfizetéshez a következő HTTP GET kérést hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="f6903-149">You can query Resource Manager for existing resource groups in your subscription using hello following HTTP GET request.</span></span> <span data-ttu-id="f6903-150">Figyelje meg, hogyan hello előfizetés-azonosító érték az átadott hello URL-cím része az időt.</span><span class="sxs-lookup"><span data-stu-id="f6903-150">Notice how hello subscription ID is passed in as part of hello URL this time.</span></span>

<span data-ttu-id="f6903-151">(YOUR_ACCESS_TOKEN és lecserélése ELŐFIZETÉS_AZONOSÍTÓJA a tényleges access token és előfizetés-azonosító)</span><span class="sxs-lookup"><span data-stu-id="f6903-151">(Replace YOUR_ACCESS_TOKEN and SUBSCRIPTION_ID with your actual access token and subscription ID)</span></span>

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="f6903-152">hello választ kapott attól függ, hogy van-e olyan meghatározott erőforráscsoportokat, és ha igen, hogy hány.</span><span class="sxs-lookup"><span data-stu-id="f6903-152">hello response you get depends on whether you have any resource groups defined and if so, how many.</span></span>

<span data-ttu-id="f6903-153">(Előfizetés-azonosítók rendelkezik lettek rövidítve olvashatóság érdekében)</span><span class="sxs-lookup"><span data-stu-id="f6903-153">(Subscription IDs have been shortened for readability)</span></span>

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a><span data-ttu-id="f6903-154">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="f6903-154">Create a resource group</span></span>
<span data-ttu-id="f6903-155">Az eddigi jelenleg már csak lett lekérdezése hello Resource Manager API-k információt.</span><span class="sxs-lookup"><span data-stu-id="f6903-155">So far, we've only been querying hello Resource Manager APIs for information.</span></span> <span data-ttu-id="f6903-156">Azt bizonyos erőforrások létrehozza, és azok minden, egy erőforráscsoport legegyszerűbb hello először.</span><span class="sxs-lookup"><span data-stu-id="f6903-156">It's time we create some resources, and let's start by hello simplest of them all, a resource group.</span></span> <span data-ttu-id="f6903-157">hello következő HTTP-kérelem hoz létre egy erőforráscsoportot a régió/hely az Ön által választott, és hozzáad egy címke tooit.</span><span class="sxs-lookup"><span data-stu-id="f6903-157">hello following HTTP request creates a resource group in a region/location of your choice, and adds a tag tooit.</span></span>

<span data-ttu-id="f6903-158">(Cserélje YOUR_ACCESS_TOKEN, ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME a tényleges hozzáférési jogkivonat, előfizetés-azonosító és hello toocreate kívánt erőforráscsoport neve)</span><span class="sxs-lookup"><span data-stu-id="f6903-158">(Replace YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME with your actual Access Token, Subscription ID and name of hello Resource Group you want toocreate)</span></span>

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

<span data-ttu-id="f6903-159">Sikeres ellenőrzés esetén töltse le a válaszhoz, amely hasonló toohello válasz a következő:</span><span class="sxs-lookup"><span data-stu-id="f6903-159">If successful, you get a response that is similar toohello following response:</span></span>

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

<span data-ttu-id="f6903-160">Sikeresen létrehozott egy erőforráscsoportot az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f6903-160">You've successfully created a resource group in Azure.</span></span> <span data-ttu-id="f6903-161">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="f6903-161">Congratulations!</span></span>

### <a name="deploy-resources-tooa-resource-group-using-a-resource-manager-template"></a><span data-ttu-id="f6903-162">A Resource Manager sablonnal erőforrások tooa erőforráscsoport telepítése</span><span class="sxs-lookup"><span data-stu-id="f6903-162">Deploy resources tooa resource group using a Resource Manager Template</span></span>
<span data-ttu-id="f6903-163">A Resource Manager telepítheti az erőforrásokat sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="f6903-163">With Resource Manager, you can deploy your resources using templates.</span></span> <span data-ttu-id="f6903-164">A sablon meghatározza a több erőforrás és függőségi viszonyaikat.</span><span class="sxs-lookup"><span data-stu-id="f6903-164">A template defines several resources and their dependencies.</span></span> <span data-ttu-id="f6903-165">Az ebben a szakaszban feltételezzük, hogy ismeri a Resource Manager-sablonok, és csak megmutatjuk, hogyan toomake hello API hívása toostart központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="f6903-165">For this section, we assume you are familiar with Resource Manager templates, and we just show you how toomake hello API call toostart deployment.</span></span> <span data-ttu-id="f6903-166">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f6903-166">For more information about constructing templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="f6903-167">Központi telepítési sablon sok más API-k hívása toohow nem különböznek.</span><span class="sxs-lookup"><span data-stu-id="f6903-167">Deployment of a template doesn't differ much toohow you call other APIs.</span></span> <span data-ttu-id="f6903-168">Egy fontos eleme a központi telepítési sablon elég hosszú időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="f6903-168">One important aspect is that deployment of a template can take quite a long time.</span></span> <span data-ttu-id="f6903-169">hello API-hívás csak adja vissza, és azt fel tooyou fejlesztői tooquery állapotához tartozó hello telepítési toofind ki, amikor hello a telepítés befejezése.</span><span class="sxs-lookup"><span data-stu-id="f6903-169">hello API call just returns, and it's up tooyou as developer tooquery for status of hello deployment toofind out when hello deployment is done.</span></span> <span data-ttu-id="f6903-170">További információkért lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="f6903-170">For more information, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>

<span data-ttu-id="f6903-171">A jelen példában használjuk elérhető egy nyilvánosan elérhetővé sablon [GitHub](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="f6903-171">For this example, we use a publicly exposed template available on [GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="f6903-172">hello sablon használjuk telepíti egy Linux virtuális gép toohello USA nyugati régiója területet.</span><span class="sxs-lookup"><span data-stu-id="f6903-172">hello template we use deploys a Linux VM toohello West US region.</span></span> <span data-ttu-id="f6903-173">Annak ellenére, hogy ebben a példában egy nyilvános tárházban, például a Githubon elérhető sablont használ, ehelyett átadhatók hello teljes sablon hello kérelem részeként.</span><span class="sxs-lookup"><span data-stu-id="f6903-173">Even though this example uses a template available in a public repository like GitHub, you can instead pass hello full template as part of hello request.</span></span> <span data-ttu-id="f6903-174">Vegye figyelembe, hogy az általunk paraméterértékek hello kérelem hello belül használt sablon üzembe.</span><span class="sxs-lookup"><span data-stu-id="f6903-174">Note that we provide parameter values in hello request that are used inside hello deployed template.</span></span>

<span data-ttu-id="f6903-175">(A név felülírandó ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD és DNS_NAME_FOR_PUBLIC_IP toovalues kérelmét megfelelő)</span><span class="sxs-lookup"><span data-stu-id="f6903-175">(Replace SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD and DNS_NAME_FOR_PUBLIC_IP toovalues appropriate for your request)</span></span>

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

<span data-ttu-id="f6903-176">hello hosszú JSON-válasz ehhez a kérelemhez nincs megadva tooimprove olvashatóság érdekében a jelen dokumentáció le lett.</span><span class="sxs-lookup"><span data-stu-id="f6903-176">hello long JSON response for this request has been omitted tooimprove readability of this documentation.</span></span> <span data-ttu-id="f6903-177">hello válasz hello sablonalapú telepítés létrehozott adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f6903-177">hello response contains information about hello templated deployment that you created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6903-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6903-178">Next steps</span></span>

- <span data-ttu-id="f6903-179">toolearn aszinkron REST műveleteinek, kezelésével kapcsolatban lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="f6903-179">toolearn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
