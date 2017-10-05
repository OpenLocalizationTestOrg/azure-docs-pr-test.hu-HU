---
title: "Erőforrás-kezelő REST API-k |} Microsoft Docs"
description: "A Resource Manager REST API-k hitelesítés és használati példák áttekintése"
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
ms.openlocfilehash: 2f7ba23775545637de865f9ef63680ae22c62164
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-rest-apis"></a><span data-ttu-id="edc28-103">Erőforrás-kezelői REST API-k</span><span class="sxs-lookup"><span data-stu-id="edc28-103">Resource Manager REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="edc28-104">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="edc28-104">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="edc28-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="edc28-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="edc28-106">Portál</span><span class="sxs-lookup"><span data-stu-id="edc28-106">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="edc28-107">REST API</span><span class="sxs-lookup"><span data-stu-id="edc28-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="edc28-108">Minden hívás az Azure Resource Manager mögött minden telepített sablon mögött minden konfigurált tárfiók mögött van egy vagy több hívásokat az Azure Resource Manager RESTful API.</span><span class="sxs-lookup"><span data-stu-id="edc28-108">Behind every call to Azure Resource Manager, behind every deployed template, behind every configured storage account there are one or more calls to the Azure Resource Manager's RESTful API.</span></span> <span data-ttu-id="edc28-109">Ez a témakör ezen API-t, és hogyan hívása őket bármely SDK minden használata nélkül használják.</span><span class="sxs-lookup"><span data-stu-id="edc28-109">This topic is devoted to those APIs and how you can call them without using any SDK at all.</span></span> <span data-ttu-id="edc28-110">Ez a módszer akkor hasznos, ha azt szeretné, hogy teljes körű hozzáférést engedélyezzenek a kérelmeket az Azure-ba, vagy ha az SDK for a választott nyelv nem érhető el, vagy nem támogatja a műveleteket kell.</span><span class="sxs-lookup"><span data-stu-id="edc28-110">This approach is useful if you want full control of requests to Azure, or if the SDK for your preferred language is not available or doesn't support the operations you need.</span></span>

<span data-ttu-id="edc28-111">Ez a cikk nem lép az Azure-ban van közzétéve, de ahelyett, hogy használja az egyes műveletek példa arra, hogyan csatlakozhat őket minden API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="edc28-111">This article does not go through every API that is exposed in Azure, but rather uses some operations as examples of how you connect to them.</span></span> <span data-ttu-id="edc28-112">Az alapok elsajátítása után áttekintheti a [Azure Resource Manager REST API-referencia](https://docs.microsoft.com/rest/api/resources/) a rest API-k használatával kapcsolatos részletes információk.</span><span class="sxs-lookup"><span data-stu-id="edc28-112">After you understand the basics, you can read the [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) to find detailed information on how to use the rest of the APIs.</span></span>

## <a name="authentication"></a><span data-ttu-id="edc28-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="edc28-113">Authentication</span></span>
<span data-ttu-id="edc28-114">Hitelesítés az erőforrás-kezelő által Azure Active Directory (AD) kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="edc28-114">Authentication for Resource Manager is handled by Azure Active Directory (AD).</span></span> <span data-ttu-id="edc28-115">Ha csatlakozni szeretne API-k, először egy hitelesítési jogkivonatot, amely lehet átadni kérelmek fogadásához az Azure AD szolgáltatással való hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="edc28-115">To connect to any API, you first need to authenticate with Azure AD to receive an authentication token that you can pass on to every request.</span></span> <span data-ttu-id="edc28-116">Vannak leíró, azt egy tiszta hívása közvetlenül a REST API-k, mivel feltételezzük, hogy nem kívánja elvégezni a hitelesítést meg kell adni egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="edc28-116">As we are describing a pure call directly to the REST APIs, we assume that you don't want to authenticate by being prompted for a username and password.</span></span> <span data-ttu-id="edc28-117">Azt is feltételezzük, hogy nem használ két többtényezős hitelesítési mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="edc28-117">We also assume you are not using two factor authentication mechanisms.</span></span> <span data-ttu-id="edc28-118">Ezért létrehozhatunk úgynevezett az Azure AD-alkalmazások és a szolgáltatás egyszerű használt a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="edc28-118">Therefore, we create what is called an Azure AD Application and a service principal that are used to log in.</span></span> <span data-ttu-id="edc28-119">De ne feledje, hogy az Azure AD támogatja a több hitelesítési eljárás, és ezek a További kérelmeknél API szükséges hitelesítési jogkivonat beolvasása használható.</span><span class="sxs-lookup"><span data-stu-id="edc28-119">But remember that Azure AD support several authentication procedures and all of them could be used to retrieve that authentication token that we need for subsequent API requests.</span></span>
<span data-ttu-id="edc28-120">Hajtsa végre a [létrehozása Azure AD-alkalmazást és egy egyszerű szolgáltatást](resource-group-create-service-principal-portal.md) részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="edc28-120">Follow [Create Azure AD Application and Service Principal](resource-group-create-service-principal-portal.md) for step by step instructions.</span></span>

### <a name="generating-an-access-token"></a><span data-ttu-id="edc28-121">Egy hozzáférési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="edc28-121">Generating an Access Token</span></span>
<span data-ttu-id="edc28-122">Hitelesítés engedélyezésébe az Azure AD végezhető el az Azure AD létesítő login.microsoftonline.com helyen.</span><span class="sxs-lookup"><span data-stu-id="edc28-122">Authentication against Azure AD is done by calling out to Azure AD, located at login.microsoftonline.com.</span></span> <span data-ttu-id="edc28-123">Hitelesítést végezni, akkor rendelkeznie kell a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="edc28-123">To authenticate, you need to have the following information:</span></span>

* <span data-ttu-id="edc28-124">Az Azure AD bérlő azonosítója (használatát a bejelentkezéshez, gyakran ugyanaz, mint a vállalat, de nem szükséges, hogy az Azure AD neve)</span><span class="sxs-lookup"><span data-stu-id="edc28-124">Azure AD Tenant ID (the name of that Azure AD you are using to log in, often the same as your company but not necessary)</span></span>
* <span data-ttu-id="edc28-125">(Az Azure AD-alkalmazás létrehozása lépés során végrehajtott) alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="edc28-125">Application ID (taken during the Azure AD application creation step)</span></span>
* <span data-ttu-id="edc28-126">Jelszó (az Azure AD-alkalmazás létrehozása során kiválasztott)</span><span class="sxs-lookup"><span data-stu-id="edc28-126">Password (that you selected while creating the Azure AD Application)</span></span>

<span data-ttu-id="edc28-127">A következő HTTP-kérelmek végezze el az "Azure AD bérlő azonosítója", "Alkalmazásazonosító" és "Password" cserélje le a megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="edc28-127">In the following HTTP request, make sure to replace "Azure AD Tenant ID", "Application ID" and "Password" with the correct values.</span></span>

<span data-ttu-id="edc28-128">**Általános HTTP-kérelem:**</span><span class="sxs-lookup"><span data-stu-id="edc28-128">**Generic HTTP Request:**</span></span>

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

<span data-ttu-id="edc28-129">... (Ha a hitelesítés sikeres) lesz az eredmény a következő válasz hasonlít:</span><span class="sxs-lookup"><span data-stu-id="edc28-129">... will (if authentication succeeds) result in a response similar to the following response:</span></span>

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
<span data-ttu-id="edc28-130">(Az előző válaszban access_token rendelkezik lett csonkolva olvashatóság)</span><span class="sxs-lookup"><span data-stu-id="edc28-130">(The access_token in the preceding response have been shortened to increase readability)</span></span>

<span data-ttu-id="edc28-131">**Létrehozása a Bash használatával:**</span><span class="sxs-lookup"><span data-stu-id="edc28-131">**Generating access token using Bash:**</span></span>

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

<span data-ttu-id="edc28-132">**Létrehozása a PowerShell használatával:**</span><span class="sxs-lookup"><span data-stu-id="edc28-132">**Generating access token using PowerShell:**</span></span>

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

<span data-ttu-id="edc28-133">A válasz egy hozzáférési jogkivonatot, információ arról, hogy mennyi ideig, hogy a jogkivonat érvényes, és milyen erőforrás is használhatja, hogy a jogkivonat kapcsolatos információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="edc28-133">The response contains an access token, information about how long that token is valid, and information about what resource you can use that token for.</span></span>
<span data-ttu-id="edc28-134">A kapott hozzáférési jogkivonat segítségével az előző HTTP-hívásban kell átadnia, az összes kérelem a Resource Manager API-val.</span><span class="sxs-lookup"><span data-stu-id="edc28-134">The access token you received in the previous HTTP call must be passed in for all request to the Resource Manager API.</span></span> <span data-ttu-id="edc28-135">A fejléc értékeként "Tulajdonosi YOUR_ACCESS_TOKEN" értékű "Engedélyezés" nevű adja át.</span><span class="sxs-lookup"><span data-stu-id="edc28-135">You pass it as a header value named "Authorization" with the value "Bearer YOUR_ACCESS_TOKEN".</span></span> <span data-ttu-id="edc28-136">Figyelje meg a "Tulajdonos" és a hozzáférési token közötti távolságot.</span><span class="sxs-lookup"><span data-stu-id="edc28-136">Notice the space between "Bearer" and your access token.</span></span>

<span data-ttu-id="edc28-137">Ahogy a fenti HTTP eredményt látja, a lexikális elem érvénytelen egy adott időn időintervalluma, gyorsítótárba, és használja fel, hogy ugyanezt a tokent.</span><span class="sxs-lookup"><span data-stu-id="edc28-137">As you can see from the above HTTP Result, the token is valid for a specific period of time during which you should cache and reuse that same token.</span></span> <span data-ttu-id="edc28-138">Akkor is, ha az is lehet az Azure AD egyes API-hívások hitelesítése, nagyon hatékony lenne.</span><span class="sxs-lookup"><span data-stu-id="edc28-138">Even if it is possible to authenticate against Azure AD for each API call, it would be highly inefficient.</span></span>

## <a name="calling-resource-manager-rest-apis"></a><span data-ttu-id="edc28-139">Hívása Resource Manager REST API-k</span><span class="sxs-lookup"><span data-stu-id="edc28-139">Calling Resource Manager REST APIs</span></span>
<span data-ttu-id="edc28-140">Ez a témakör csak néhány API-kat használ a többi műveletek alapvető használati ismertetik.</span><span class="sxs-lookup"><span data-stu-id="edc28-140">This topic only uses a few APIs to explain the basic usage of the REST operations.</span></span> <span data-ttu-id="edc28-141">A műveletekkel kapcsolatos információkért lásd: [Azure Resource Manager REST API-k](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="edc28-141">For information about all the operations, see [Azure Resource Manager REST APIs](https://docs.microsoft.com/rest/api/resources/).</span></span>

### <a name="list-all-subscriptions"></a><span data-ttu-id="edc28-142">Minden előfizetés felsorolása</span><span class="sxs-lookup"><span data-stu-id="edc28-142">List all subscriptions</span></span>
<span data-ttu-id="edc28-143">A legegyszerűbb elvégezhető műveletek egyike az elérhető előfizetésekkel, hogy van-e hozzáférési listát.</span><span class="sxs-lookup"><span data-stu-id="edc28-143">One of the simplest operations you can do is to list the available subscriptions that you can access.</span></span> <span data-ttu-id="edc28-144">A következő kérés láthatja, hogyan a hozzáférési jogkivonat érték az átadott fejléc:</span><span class="sxs-lookup"><span data-stu-id="edc28-144">In the following request, you see how the access token is passed in as a header:</span></span>

<span data-ttu-id="edc28-145">(Cserélje YOUR_ACCESS_TOKEN a tényleges hozzáférési jogkivonat.)</span><span class="sxs-lookup"><span data-stu-id="edc28-145">(Replace YOUR_ACCESS_TOKEN with your actual Access Token.)</span></span>

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="edc28-146">... és emiatt kap, hogy a szolgáltatás egyszerű hozzáférés előfizetések listája</span><span class="sxs-lookup"><span data-stu-id="edc28-146">... and as a result, you get a list of subscriptions that this service principal is allowed to access</span></span>

<span data-ttu-id="edc28-147">(Előfizetés-azonosítók rendelkezik lettek rövidítve olvashatóság érdekében)</span><span class="sxs-lookup"><span data-stu-id="edc28-147">(Subscription IDs have been shortened for readability)</span></span>

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

### <a name="list-all-resource-groups-in-a-specific-subscription"></a><span data-ttu-id="edc28-148">Egy adott előfizetés összes erőforráscsoportok felsorolása</span><span class="sxs-lookup"><span data-stu-id="edc28-148">List all resource groups in a specific subscription</span></span>
<span data-ttu-id="edc28-149">A Resource Manager API-khoz elérhető összes erőforrás egy erőforráscsoportban beágyazott.</span><span class="sxs-lookup"><span data-stu-id="edc28-149">All resources available with the Resource Manager APIs are nested inside a resource group.</span></span> <span data-ttu-id="edc28-150">Meglévő erőforráscsoport erőforrás-kezelő lekérheti az előfizetéshez a következő HTTP GET kérelem használatával.</span><span class="sxs-lookup"><span data-stu-id="edc28-150">You can query Resource Manager for existing resource groups in your subscription using the following HTTP GET request.</span></span> <span data-ttu-id="edc28-151">Figyelje meg, hogyan az előfizetés-azonosító érték az átadott az URL-címben megadott idő.</span><span class="sxs-lookup"><span data-stu-id="edc28-151">Notice how the subscription ID is passed in as part of the URL this time.</span></span>

<span data-ttu-id="edc28-152">(YOUR_ACCESS_TOKEN és lecserélése ELŐFIZETÉS_AZONOSÍTÓJA a tényleges access token és előfizetés-azonosító)</span><span class="sxs-lookup"><span data-stu-id="edc28-152">(Replace YOUR_ACCESS_TOKEN and SUBSCRIPTION_ID with your actual access token and subscription ID)</span></span>

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="edc28-153">A kapott válasz attól függ, hogy van-e olyan meghatározott erőforráscsoportokat, és ha igen, hogy hány.</span><span class="sxs-lookup"><span data-stu-id="edc28-153">The response you get depends on whether you have any resource groups defined and if so, how many.</span></span>

<span data-ttu-id="edc28-154">(Előfizetés-azonosítók rendelkezik lettek rövidítve olvashatóság érdekében)</span><span class="sxs-lookup"><span data-stu-id="edc28-154">(Subscription IDs have been shortened for readability)</span></span>

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

### <a name="create-a-resource-group"></a><span data-ttu-id="edc28-155">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="edc28-155">Create a resource group</span></span>
<span data-ttu-id="edc28-156">Az eddigi jelenleg már csak lett lekérdezése a Resource Manager API-k információt.</span><span class="sxs-lookup"><span data-stu-id="edc28-156">So far, we've only been querying the Resource Manager APIs for information.</span></span> <span data-ttu-id="edc28-157">A Microsoft bizonyos erőforrások létrehozza, és először a legegyszerűbb azok összes, egy erőforráscsoportot is.</span><span class="sxs-lookup"><span data-stu-id="edc28-157">It's time we create some resources, and let's start by the simplest of them all, a resource group.</span></span> <span data-ttu-id="edc28-158">A következő HTTP-kérelem hoz létre egy erőforráscsoportot a régió/hely az Ön által választott, és egy címkét ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="edc28-158">The following HTTP request creates a resource group in a region/location of your choice, and adds a tag to it.</span></span>

<span data-ttu-id="edc28-159">(Cserélje YOUR_ACCESS_TOKEN, ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME a tényleges hozzáférési jogkivonat, előfizetés-azonosító és szeretne létrehozni az erőforráscsoport neve)</span><span class="sxs-lookup"><span data-stu-id="edc28-159">(Replace YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME with your actual Access Token, Subscription ID and name of the Resource Group you want to create)</span></span>

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

<span data-ttu-id="edc28-160">Ha sikeres, a következő válasz hasonló választ kap:</span><span class="sxs-lookup"><span data-stu-id="edc28-160">If successful, you get a response that is similar to the following response:</span></span>

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

<span data-ttu-id="edc28-161">Sikeresen létrehozott egy erőforráscsoportot az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="edc28-161">You've successfully created a resource group in Azure.</span></span> <span data-ttu-id="edc28-162">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="edc28-162">Congratulations!</span></span>

### <a name="deploy-resources-to-a-resource-group-using-a-resource-manager-template"></a><span data-ttu-id="edc28-163">A Resource Manager sablonnal erőforráscsoporthoz erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="edc28-163">Deploy resources to a resource group using a Resource Manager Template</span></span>
<span data-ttu-id="edc28-164">A Resource Manager telepítheti az erőforrásokat sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="edc28-164">With Resource Manager, you can deploy your resources using templates.</span></span> <span data-ttu-id="edc28-165">A sablon meghatározza a több erőforrás és függőségi viszonyaikat.</span><span class="sxs-lookup"><span data-stu-id="edc28-165">A template defines several resources and their dependencies.</span></span> <span data-ttu-id="edc28-166">Az ebben a szakaszban feltételezzük, hogy ismeri a Resource Manager-sablonok, és csak megmutatjuk, hogyan végezheti el az API-hívás központi telepítésének elindítása.</span><span class="sxs-lookup"><span data-stu-id="edc28-166">For this section, we assume you are familiar with Resource Manager templates, and we just show you how to make the API call to start deployment.</span></span> <span data-ttu-id="edc28-167">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="edc28-167">For more information about constructing templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="edc28-168">Központi telepítési sablon nem térnek el egymástól sokkal hogyan más API-k hívására-e.</span><span class="sxs-lookup"><span data-stu-id="edc28-168">Deployment of a template doesn't differ much to how you call other APIs.</span></span> <span data-ttu-id="edc28-169">Egy fontos eleme a központi telepítési sablon elég hosszú időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="edc28-169">One important aspect is that deployment of a template can take quite a long time.</span></span> <span data-ttu-id="edc28-170">Az API-hívással most adja vissza, és Önnek, fejlesztő lekérdezze a központi telepítés állapotának megállapítása az üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="edc28-170">The API call just returns, and it's up to you as developer to query for status of the deployment to find out when the deployment is done.</span></span> <span data-ttu-id="edc28-171">További információkért lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="edc28-171">For more information, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>

<span data-ttu-id="edc28-172">A jelen példában használjuk elérhető egy nyilvánosan elérhetővé sablon [GitHub](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="edc28-172">For this example, we use a publicly exposed template available on [GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="edc28-173">A sablon használjuk az USA nyugati régiója régió Linux virtuális gép telepítését.</span><span class="sxs-lookup"><span data-stu-id="edc28-173">The template we use deploys a Linux VM to the West US region.</span></span> <span data-ttu-id="edc28-174">Annak ellenére, hogy ebben a példában egy nyilvános tárházban, például a Githubon elérhető sablont használ, ehelyett átadhatók teljes sablonjával a kérelem részeként.</span><span class="sxs-lookup"><span data-stu-id="edc28-174">Even though this example uses a template available in a public repository like GitHub, you can instead pass the full template as part of the request.</span></span> <span data-ttu-id="edc28-175">Vegye figyelembe, hogy azt adja meg a paraméter a kérésben használt értékek a telepített sablon belül.</span><span class="sxs-lookup"><span data-stu-id="edc28-175">Note that we provide parameter values in the request that are used inside the deployed template.</span></span>

<span data-ttu-id="edc28-176">(Csere ELŐFIZETÉS_AZONOSÍTÓJA, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD és DNS_NAME_FOR_PUBLIC_IP a kérést a megfelelő értékeket)</span><span class="sxs-lookup"><span data-stu-id="edc28-176">(Replace SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD and DNS_NAME_FOR_PUBLIC_IP to values appropriate for your request)</span></span>

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

<span data-ttu-id="edc28-177">A hosszú JSON-választ a kérelemhez tartozó kimaradt a jelen dokumentáció olvashatóság javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="edc28-177">The long JSON response for this request has been omitted to improve readability of this documentation.</span></span> <span data-ttu-id="edc28-178">A válasz, amelyet hozott létre sablonalapú üzembe helyezésével kapcsolatos információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="edc28-178">The response contains information about the templated deployment that you created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edc28-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edc28-179">Next steps</span></span>

- <span data-ttu-id="edc28-180">Aszinkron REST műveleteinek kezelésére vonatkozó további tudnivalókért lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="edc28-180">To learn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
