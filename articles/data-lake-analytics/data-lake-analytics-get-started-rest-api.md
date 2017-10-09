---
title: "aaaGet REST API használatával a Data Lake Analytics használatába |} Microsoft Docs"
description: "A Data Lake Analytics WebHDFS REST API-k tooperform műveletek használata"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
ms.openlocfilehash: a0b13d521821fd2d74716cc52485585feb7c51b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a><span data-ttu-id="3c959-103">Az Azure Data Lake Analytics használatának első lépései REST API-k használatával</span><span class="sxs-lookup"><span data-stu-id="3c959-103">Get started with Azure Data Lake Analytics using REST APIs</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="3c959-104">Ismerje meg, hogyan toouse WebHDFS REST API-k és a Data Lake Analytics REST API-k toomanage Data Lake Analytics fiókok, feladatok és a katalógus.</span><span class="sxs-lookup"><span data-stu-id="3c959-104">Learn how toouse WebHDFS REST APIs and Data Lake Analytics REST APIs toomanage Data Lake Analytics accounts, jobs, and catalog.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3c959-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3c959-105">Prerequisites</span></span>
* <span data-ttu-id="3c959-106">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="3c959-106">**An Azure subscription**.</span></span> <span data-ttu-id="3c959-107">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3c959-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3c959-108">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3c959-108">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="3c959-109">Hello Azure AD alkalmazás tooauthenticate hello Data Lake Analytics-alkalmazást az Azure ad-val használhatja.</span><span class="sxs-lookup"><span data-stu-id="3c959-109">You use hello Azure AD application tooauthenticate hello Data Lake Analytics application with Azure AD.</span></span> <span data-ttu-id="3c959-110">Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="3c959-110">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="3c959-111">További információt és útmutatást tooauthenticate, lásd: [hitelesítés az Azure Active Directory használatával a Data Lake Analytics](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="3c959-111">For instructions and more information on how tooauthenticate, see [Authenticate with Data Lake Analytics using Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="3c959-112">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="3c959-112">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="3c959-113">Ez a cikk a cURL toodemonstrate hogyan toomake REST API meghívja a Data Lake Analytics-fiók ellen használja.</span><span class="sxs-lookup"><span data-stu-id="3c959-113">This article uses cURL toodemonstrate how toomake REST API calls against a Data Lake Analytics account.</span></span>

## <a name="authenticate-with-azure-active-directory"></a><span data-ttu-id="3c959-114">Hitelesítés az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="3c959-114">Authenticate with Azure Active Directory</span></span>
<span data-ttu-id="3c959-115">Az Azure Active Directoryval kétféle módon lehet hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="3c959-115">There are two methods for authenticating with Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="3c959-116">Végfelhasználó hitelesítése (interaktív)</span><span class="sxs-lookup"><span data-stu-id="3c959-116">End-user authentication (interactive)</span></span>
<span data-ttu-id="3c959-117">Ezzel a módszerrel alkalmazás kérni fogja a hello felhasználói toolog, és minden hello műveleteket hello hello felhasználó környezetében.</span><span class="sxs-lookup"><span data-stu-id="3c959-117">Using this method, application prompts hello user toolog in and all hello operations are performed in hello context of hello user.</span></span> 

<span data-ttu-id="3c959-118">Az interaktív hitelesítéshez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3c959-118">Follow these steps for interactive authentication:</span></span>

1. <span data-ttu-id="3c959-119">Az alkalmazáson keresztül átirányítási URL-cím a következő hello felhasználói toohello:</span><span class="sxs-lookup"><span data-stu-id="3c959-119">Through your application, redirect hello user toohello following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="3c959-120">\<REDIRECT-URI > használható URL-kódolású toobe kell.</span><span class="sxs-lookup"><span data-stu-id="3c959-120">\<REDIRECT-URI> needs toobe encoded for use in a URL.</span></span> <span data-ttu-id="3c959-121">A https://localhost esetében tehát használja a következőt: `https%3A%2F%2Flocalhost`)</span><span class="sxs-lookup"><span data-stu-id="3c959-121">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="3c959-122">Az oktatóanyagban szereplő hello célból cserélje le a hello helyőrző értékeket a fenti hello URL-ben, és beillesztheti egy webböngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="3c959-122">For hello purpose of this tutorial, you can replace hello placeholder values in hello URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="3c959-123">Az Azure bejelentkezési azonosítójával átirányított tooauthenticate lesz.</span><span class="sxs-lookup"><span data-stu-id="3c959-123">You will be redirected tooauthenticate using your Azure login.</span></span> <span data-ttu-id="3c959-124">Miután sikeresen bejelentkezett, hello válasz hello böngésző címsorában megjelenik.</span><span class="sxs-lookup"><span data-stu-id="3c959-124">Once you succesfully log in, hello response is displayed in hello browser's address bar.</span></span> <span data-ttu-id="3c959-125">hello válasz hello formátuma a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="3c959-125">hello response will be in hello following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="3c959-126">Hello engedélyezési kód a hello válaszban rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c959-126">Capture hello authorization code from hello response.</span></span> <span data-ttu-id="3c959-127">Ebben az oktatóanyagban hello webböngésző címsorába hello hello engedélyezési kód másolását, és adja át hello POST kérelem toohello jogkivonat végpontjához az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="3c959-127">For this tutorial, you can copy hello authorization code from hello address bar of hello web browser and pass it in hello POST request toohello token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="3c959-128">Ebben az esetben hello \<REDIRECT-URI > kódolása nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3c959-128">In this case, hello \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="3c959-129">a rendszer hello választ, egy JSON-objektum, amely egy hozzáférési jogkivonatot tartalmaz (például `"access_token": "<ACCESS_TOKEN>"`) és egy frissítési (pl. `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="3c959-129">hello response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="3c959-130">Az alkalmazás használja hello hozzáférési jogkivonatot az Azure Data Lake Store és hello frissítési jogkivonat tooget való hozzáférés során egy új hozzáférési jogkivonat mikor jár le a hozzáférési tokent.</span><span class="sxs-lookup"><span data-stu-id="3c959-130">Your application uses hello access token when accessing Azure Data Lake Store and hello refresh token tooget another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="3c959-131">Amikor hello hozzáférési jogkivonat lejár, kérhet egy új hozzáférési jogkivonat hello frissítési jogkivonat használatával alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="3c959-131">When hello access token expires, you can request a new access token using hello refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="3c959-132">További információk az interaktív felhasználói hitelesítéssel kapcsolatban: [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Az engedélyezési kód engedélyezési folyamata).</span><span class="sxs-lookup"><span data-stu-id="3c959-132">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="3c959-133">Szolgáltatások közötti hitelesítés (nem interaktív)</span><span class="sxs-lookup"><span data-stu-id="3c959-133">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="3c959-134">Ezzel a módszerrel alkalmazás maga biztosítja saját hitelesítő adatait tooperform hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="3c959-134">Using this method, application provides its own credentials tooperform hello operations.</span></span> <span data-ttu-id="3c959-135">Ehhez hello az alábbihoz hasonló POST-kérelmet kell kiadnia:</span><span class="sxs-lookup"><span data-stu-id="3c959-135">For this, you must issue a POST request like hello one shown below:</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="3c959-136">a kérelem hello kimenete egy engedélyezési jogkivonatot tartalmazza (kimaradásával `access-token` hello kimenetben alább), amely ezt követően továbbítják a REST API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="3c959-136">hello output of this request will include an authorization token (denoted by `access-token` in hello output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="3c959-137">Mentse ezt az engedélyezési jogkivonatot egy szövegfájlba, mivel később még szüksége lesz rá a cikkben.</span><span class="sxs-lookup"><span data-stu-id="3c959-137">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="3c959-138">Ebben a cikkben az hello **nem interaktív** megközelítést.</span><span class="sxs-lookup"><span data-stu-id="3c959-138">This article uses hello **non-interactive** approach.</span></span> <span data-ttu-id="3c959-139">A nem interaktív (szolgáltatások közötti hívások) további információkért lásd: [tooservice hívások hitelesítő szolgáltatás](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c959-139">For more information on non-interactive (service-to-service calls), see [Service tooservice calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="3c959-140">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c959-140">Create a Data Lake Analytics account</span></span>
<span data-ttu-id="3c959-141">Data Lake Analytics-fiók létrehozása előtt létre kell hoznia egy Azure-erőforráscsoportot és egy Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="3c959-141">You must create an Azure Resource group, and a Data Lake Store account before you can create a Data Lake Analytics account.</span></span>  <span data-ttu-id="3c959-142">Lásd: [Data Lake Store-fiók létrehozása](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="3c959-142">See [Create a Data Lake Store account](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span></span>

<span data-ttu-id="3c959-143">a következő Curl-parancs bemutatja hogyan hello toocreate fiók:</span><span class="sxs-lookup"><span data-stu-id="3c959-143">hello following Curl command shows how toocreate an account:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

<span data-ttu-id="3c959-144">Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-Azonosítóval rendelkező \< `AzureResourceGroupName` \> egy meglévő Azure-erőforrás Csoport neve, és \< `NewAzureDataLakeAnalyticsAccountName` \> új Data Lake Analytics-fiók névvel.</span><span class="sxs-lookup"><span data-stu-id="3c959-144">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`NewAzureDataLakeAnalyticsAccountName`\> with a new Data Lake Analytics Account name.</span></span> <span data-ttu-id="3c959-145">hello kérelem hasznos adatai ezen parancs hello lévő **CreateDatalakeAnalyticsAccountRequest.json** hello a megadott fájl `-d` fenti paraméter.</span><span class="sxs-lookup"><span data-stu-id="3c959-145">hello request payload for this command is contained in hello **CreateDatalakeAnalyticsAccountRequest.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="3c959-146">hello input.json fájl tartalmának hello hello alábbihoz:</span><span class="sxs-lookup"><span data-stu-id="3c959-146">hello contents of hello input.json file resemble hello following:</span></span>

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a><span data-ttu-id="3c959-147">A Data Lake Analytics-fiókok felsorolása egy előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3c959-147">List Data Lake Analytics accounts in a subscription</span></span>
<span data-ttu-id="3c959-148">a következő Curl-parancsot hello bemutatja, hogyan toolist fiókok egy előfizetésben:</span><span class="sxs-lookup"><span data-stu-id="3c959-148">hello following Curl command shows how toolist accounts in a subscription:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

<span data-ttu-id="3c959-149">Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="3c959-149">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID.</span></span> <span data-ttu-id="3c959-150">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3c959-150">hello output is similar to:</span></span>

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="3c959-151">Data Lake Analytics-fiókkal kapcsolatos információk beszerzése</span><span class="sxs-lookup"><span data-stu-id="3c959-151">Get information about a Data Lake Analytics account</span></span>
<span data-ttu-id="3c959-152">a következő Curl-parancs bemutatja hogyan hello tooget egy fiók adatait:</span><span class="sxs-lookup"><span data-stu-id="3c959-152">hello following Curl command shows how tooget an account information:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

<span data-ttu-id="3c959-153">Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-Azonosítóval rendelkező \< `AzureResourceGroupName` \> egy meglévő Azure-erőforrás Csoport neve, és \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3c959-153">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="3c959-154">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3c959-154">hello output is similar to:</span></span>

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a><span data-ttu-id="3c959-155">Data Lake Analytics-fiókhoz tartozó Data Lake-tárolók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3c959-155">List Data Lake Stores of a Data Lake Analytics account</span></span>
<span data-ttu-id="3c959-156">a következő Curl-parancsot hello bemutatja, hogyan toolist Data Lake tárolja egy olyan fiók:</span><span class="sxs-lookup"><span data-stu-id="3c959-156">hello following Curl command shows how toolist Data Lake Stores of an account:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

<span data-ttu-id="3c959-157">Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `AzureSubscriptionID` \> az előfizetés-Azonosítóval rendelkező \< `AzureResourceGroupName` \> egy meglévő Azure-erőforrás Csoport neve, és \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3c959-157">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="3c959-158">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3c959-158">hello output is similar to:</span></span>

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="3c959-159">U-SQL-feladatok küldése</span><span class="sxs-lookup"><span data-stu-id="3c959-159">Submit U-SQL jobs</span></span>
<span data-ttu-id="3c959-160">a következő Curl-parancs bemutatja hogyan hello toosubmit egy U-SQL-feladatot:</span><span class="sxs-lookup"><span data-stu-id="3c959-160">hello following Curl command shows how toosubmit a U-SQL job:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

<span data-ttu-id="3c959-161">Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3c959-161">Replace \<`REDACTED`\> with hello authorization token, \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="3c959-162">hello kérelem hasznos adatai ezen parancs hello lévő **SubmitADLAJob.json** hello a megadott fájl `-d` fenti paraméter.</span><span class="sxs-lookup"><span data-stu-id="3c959-162">hello request payload for this command is contained in hello **SubmitADLAJob.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="3c959-163">hello input.json fájl tartalmának hello hello alábbihoz:</span><span class="sxs-lookup"><span data-stu-id="3c959-163">hello contents of hello input.json file resemble hello following:</span></span>

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    too\"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

<span data-ttu-id="3c959-164">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3c959-164">hello output is similar to:</span></span>

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a><span data-ttu-id="3c959-165">U-SQL-feladatok felsorolása</span><span class="sxs-lookup"><span data-stu-id="3c959-165">List U-SQL jobs</span></span>
<span data-ttu-id="3c959-166">a következő Curl-parancs bemutatja hogyan hello toolist U-SQL feladatok:</span><span class="sxs-lookup"><span data-stu-id="3c959-166">hello following Curl command shows how toolist U-SQL jobs:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

<span data-ttu-id="3c959-167">Cserélje le \< `REDACTED` \> az hello engedélyezési jogkivonatot, és \< `DataLakeAnalyticsAccountName` \> hello nevet, egy meglévő Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3c959-167">Replace \<`REDACTED`\> with hello authorization token, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> 

<span data-ttu-id="3c959-168">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3c959-168">hello output is similar to:</span></span>

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a><span data-ttu-id="3c959-169">Katalóguselemek beolvasása</span><span class="sxs-lookup"><span data-stu-id="3c959-169">Get catalog items</span></span>
<span data-ttu-id="3c959-170">a következő Curl-parancsot hello bemutatja, hogyan tooget hello adatbázisok hello katalógus:</span><span class="sxs-lookup"><span data-stu-id="3c959-170">hello following Curl command shows how tooget hello databases from hello catalog:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

<span data-ttu-id="3c959-171">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3c959-171">hello output is similar to:</span></span>

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a><span data-ttu-id="3c959-172">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3c959-172">See also</span></span>
* <span data-ttu-id="3c959-173">egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="3c959-173">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="3c959-174">megkezdődött a U-SQL-alkalmazások fejlesztésével tooget lásd [Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3c959-174">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="3c959-175">toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3c959-175">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="3c959-176">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="3c959-176">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="3c959-177">a Data Lake Analytics áttekintésének tooget lásd [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3c959-177">tooget an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
* <span data-ttu-id="3c959-178">toosee hello ugyanaz az oktatóanyagot más eszközök használatával hello szeretné a hello hello lap tetején kattintson.</span><span class="sxs-lookup"><span data-stu-id="3c959-178">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>

