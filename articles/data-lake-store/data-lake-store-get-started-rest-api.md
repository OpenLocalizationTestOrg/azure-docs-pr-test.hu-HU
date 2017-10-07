---
title: "aaaUse hello REST API tooget Data Lake Store használatába |} Microsoft Docs"
description: "A Data Lake Store a WebHDFS REST API-k tooperform műveletek használata"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: 62fce8293dfee730a61f2a3d37fc138ce7c3afdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a><span data-ttu-id="20fdc-103">Az Azure Data Lake Store használatának első lépései a REST API használatával</span><span class="sxs-lookup"><span data-stu-id="20fdc-103">Get started with Azure Data Lake Store using REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="20fdc-104">Portál</span><span class="sxs-lookup"><span data-stu-id="20fdc-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="20fdc-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="20fdc-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="20fdc-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="20fdc-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="20fdc-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="20fdc-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="20fdc-108">REST API</span><span class="sxs-lookup"><span data-stu-id="20fdc-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="20fdc-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="20fdc-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="20fdc-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="20fdc-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="20fdc-111">Python</span><span class="sxs-lookup"><span data-stu-id="20fdc-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="20fdc-112">Ebből a cikkből megtudhatja, hogyan toouse WebHDFS REST API-k és a Data Lake Store REST API-k tooperform fiók kezelése, valamint az Azure Data Lake Store a fájlrendszer-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="20fdc-112">In this article, you will learn how toouse WebHDFS REST APIs and Data Lake Store REST APIs tooperform account management as well as filesystem operations on Azure Data Lake Store.</span></span> <span data-ttu-id="20fdc-113">Az Azure Data Lake Store saját REST API-kkal rendelkezik a fiókkezelési műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="20fdc-113">Azure Data Lake Store exposes its own REST APIs for account management operations.</span></span> <span data-ttu-id="20fdc-114">Mivel azonban a Data Lake Store kompatibilis a HDFS- és a Hadoop-ökoszisztémával, támogatja a WebHDFS REST API-k használatát a fájlrendszer-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="20fdc-114">However, because Data Lake Store is compatible with HDFS and Hadoop ecosystem, it supports using WebHDFS REST APIs for filesystem operations.</span></span>

> [!NOTE]
> <span data-ttu-id="20fdc-115">A Data Lake Store REST API-támogatása hello részletes információkért lásd: [Azure Data Lake Store REST API-referencia](https://msdn.microsoft.com/library/mt693424.aspx).</span><span class="sxs-lookup"><span data-stu-id="20fdc-115">For detailed information on hello REST API support for Data Lake Store, see [Azure Data Lake Store REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="20fdc-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="20fdc-116">Prerequisites</span></span>
* <span data-ttu-id="20fdc-117">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="20fdc-117">**An Azure subscription**.</span></span> <span data-ttu-id="20fdc-118">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="20fdc-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="20fdc-119">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="20fdc-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="20fdc-120">Hello Azure AD alkalmazás tooauthenticate hello Data Lake Store-alkalmazás használhatja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="20fdc-120">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="20fdc-121">Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="20fdc-121">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="20fdc-122">További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="20fdc-122">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="20fdc-123">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="20fdc-123">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="20fdc-124">Ez a cikk a cURL toodemonstrate hogyan toomake REST API-t hívja, egy Data Lake Store-fiók használ.</span><span class="sxs-lookup"><span data-stu-id="20fdc-124">This article uses cURL toodemonstrate how toomake REST API calls against a Data Lake Store account.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="20fdc-125">Hogyan végezhető el a hitelesítés az Azure Active Directory használatával?</span><span class="sxs-lookup"><span data-stu-id="20fdc-125">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="20fdc-126">Az Azure Active Directory használatával két megközelítés tooauthenticate is használhatja.</span><span class="sxs-lookup"><span data-stu-id="20fdc-126">You can use two approaches tooauthenticate using Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="20fdc-127">Végfelhasználó hitelesítése (interaktív)</span><span class="sxs-lookup"><span data-stu-id="20fdc-127">End-user authentication (interactive)</span></span>
<span data-ttu-id="20fdc-128">Ebben a forgatókönyvben hello alkalmazás kérni fogja a hello felhasználói toolog, és minden hello műveleteket hello hello felhasználó környezetében.</span><span class="sxs-lookup"><span data-stu-id="20fdc-128">In this scenario, hello application prompts hello user toolog in and all hello operations are performed in hello context of hello user.</span></span> <span data-ttu-id="20fdc-129">Hajtsa végre a következő lépéseket az interaktív hitelesítéshez hello.</span><span class="sxs-lookup"><span data-stu-id="20fdc-129">Perform hello following steps for interactive authentication.</span></span>

1. <span data-ttu-id="20fdc-130">Az alkalmazáson keresztül átirányítási URL-cím a következő hello felhasználói toohello:</span><span class="sxs-lookup"><span data-stu-id="20fdc-130">Through your application, redirect hello user toohello following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="20fdc-131">\<REDIRECT-URI > használható URL-kódolású toobe kell.</span><span class="sxs-lookup"><span data-stu-id="20fdc-131">\<REDIRECT-URI> needs toobe encoded for use in a URL.</span></span> <span data-ttu-id="20fdc-132">A https://localhost esetében tehát használja a következőt: `https%3A%2F%2Flocalhost`)</span><span class="sxs-lookup"><span data-stu-id="20fdc-132">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="20fdc-133">Az oktatóanyagban szereplő hello célból cserélje le a hello helyőrző értékeket a fenti hello URL-ben, és beillesztheti egy webböngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="20fdc-133">For hello purpose of this tutorial, you can replace hello placeholder values in hello URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="20fdc-134">Az Azure bejelentkezési azonosítójával átirányított tooauthenticate lesz.</span><span class="sxs-lookup"><span data-stu-id="20fdc-134">You will be redirected tooauthenticate using your Azure login.</span></span> <span data-ttu-id="20fdc-135">Miután sikeresen jelentkezik be, hello válasz hello böngésző címsorában megjelenik.</span><span class="sxs-lookup"><span data-stu-id="20fdc-135">Once you successfully log in, hello response is displayed in hello browser's address bar.</span></span> <span data-ttu-id="20fdc-136">hello válasz hello formátuma a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="20fdc-136">hello response will be in hello following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="20fdc-137">Hello engedélyezési kód a hello válaszban rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="20fdc-137">Capture hello authorization code from hello response.</span></span> <span data-ttu-id="20fdc-138">Ebben az oktatóanyagban hello webböngésző címsorába hello hello engedélyezési kód másolását, és adja át hello POST kérelem toohello jogkivonat végpontjához az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="20fdc-138">For this tutorial, you can copy hello authorization code from hello address bar of hello web browser and pass it in hello POST request toohello token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="20fdc-139">Ebben az esetben hello \<REDIRECT-URI > kódolása nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="20fdc-139">In this case, hello \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="20fdc-140">a rendszer hello választ, egy JSON-objektum, amely egy hozzáférési jogkivonatot tartalmaz (például `"access_token": "<ACCESS_TOKEN>"`) és egy frissítési (pl. `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="20fdc-140">hello response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="20fdc-141">Az alkalmazás használja hello hozzáférési jogkivonatot az Azure Data Lake Store és hello frissítési jogkivonat tooget való hozzáférés során egy új hozzáférési jogkivonat mikor jár le a hozzáférési tokent.</span><span class="sxs-lookup"><span data-stu-id="20fdc-141">Your application uses hello access token when accessing Azure Data Lake Store and hello refresh token tooget another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="20fdc-142">Amikor hello hozzáférési jogkivonat lejár, kérhet egy új hozzáférési jogkivonat hello frissítési jogkivonat használatával alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="20fdc-142">When hello access token expires, you can request a new access token using hello refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="20fdc-143">További információk az interaktív felhasználói hitelesítéssel kapcsolatban: [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Az engedélyezési kód engedélyezési folyamata).</span><span class="sxs-lookup"><span data-stu-id="20fdc-143">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="20fdc-144">Szolgáltatások közötti hitelesítés (nem interaktív)</span><span class="sxs-lookup"><span data-stu-id="20fdc-144">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="20fdc-145">Ebben a forgatókönyvben hello hello alkalmazás maga biztosítja saját hitelesítő adatait tooperform hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="20fdc-145">In this scenario, hello hello application provides its own credentials tooperform hello operations.</span></span> <span data-ttu-id="20fdc-146">Ehhez hello az alábbihoz hasonló POST-kérelmet kell kiadnia.</span><span class="sxs-lookup"><span data-stu-id="20fdc-146">For this, you must issue a POST request like hello one shown below.</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="20fdc-147">a kérelem hello kimenete egy engedélyezési jogkivonatot tartalmazza (kimaradásával `access-token` hello kimenetben alább), amely ezt követően továbbítják a REST API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="20fdc-147">hello output of this request will include an authorization token (denoted by `access-token` in hello output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="20fdc-148">Mentse ezt az engedélyezési jogkivonatot egy szövegfájlba, mivel később még szüksége lesz rá a cikkben.</span><span class="sxs-lookup"><span data-stu-id="20fdc-148">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="20fdc-149">Ebben a cikkben az hello **nem interaktív** megközelítést.</span><span class="sxs-lookup"><span data-stu-id="20fdc-149">This article uses hello **non-interactive** approach.</span></span> <span data-ttu-id="20fdc-150">A nem interaktív (szolgáltatások közötti hívások) további információkért lásd: [tooservice hívások hitelesítő szolgáltatás](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="20fdc-150">For more information on non-interactive (service-to-service calls), see [Service tooservice calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="20fdc-151">Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="20fdc-151">Create a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-152">Ez a művelet alapján definiált hello REST API-hívás [Itt](https://msdn.microsoft.com/library/mt694078.aspx).</span><span class="sxs-lookup"><span data-stu-id="20fdc-152">This operation is based on hello REST API call defined [here](https://msdn.microsoft.com/library/mt694078.aspx).</span></span>

<span data-ttu-id="20fdc-153">A következő cURL-parancsot hello használata.</span><span class="sxs-lookup"><span data-stu-id="20fdc-153">Use hello following cURL command.</span></span> <span data-ttu-id="20fdc-154">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-154">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

<span data-ttu-id="20fdc-155">A fenti parancs hello, cserélje le a \< `REDACTED` \> hello engedélyezési jogkivonat a korábban kapott.</span><span class="sxs-lookup"><span data-stu-id="20fdc-155">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span> <span data-ttu-id="20fdc-156">hello kérelem hasznos adatai ezen parancs hello lévő **input.json** hello a megadott fájl `-d` fenti paraméter.</span><span class="sxs-lookup"><span data-stu-id="20fdc-156">hello request payload for this command is contained in hello **input.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="20fdc-157">hello input.json fájl tartalmának hello hello alábbihoz:</span><span class="sxs-lookup"><span data-stu-id="20fdc-157">hello contents of hello input.json file resemble hello following:</span></span>

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="20fdc-158">Mappák létrehozása Data Lake Store-fiókban</span><span class="sxs-lookup"><span data-stu-id="20fdc-158">Create folders in a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-159">Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="20fdc-159">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span></span>

<span data-ttu-id="20fdc-160">A következő cURL-parancsot hello használata.</span><span class="sxs-lookup"><span data-stu-id="20fdc-160">Use hello following cURL command.</span></span> <span data-ttu-id="20fdc-161">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-161">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

<span data-ttu-id="20fdc-162">A fenti parancs hello, cserélje le a \< `REDACTED` \> hello engedélyezési jogkivonat a korábban kapott.</span><span class="sxs-lookup"><span data-stu-id="20fdc-162">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span> <span data-ttu-id="20fdc-163">Ezzel a paranccsal létrejön egy nevű könyvtárat **mytempdir** hello gyökérmappájában a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="20fdc-163">This command creates a directory called **mytempdir** under hello root folder of your Data Lake Store account.</span></span>

<span data-ttu-id="20fdc-164">Ha hello művelet sikeresen befejeződik a következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="20fdc-164">You should see a response like this if hello operation completes successfully:</span></span>

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a><span data-ttu-id="20fdc-165">Data Lake Store-fiók mappáinak listázása</span><span class="sxs-lookup"><span data-stu-id="20fdc-165">List folders in a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-166">Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="20fdc-166">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span></span>

<span data-ttu-id="20fdc-167">A következő cURL-parancsot hello használata.</span><span class="sxs-lookup"><span data-stu-id="20fdc-167">Use hello following cURL command.</span></span> <span data-ttu-id="20fdc-168">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-168">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

<span data-ttu-id="20fdc-169">A fenti parancs hello, cserélje le a \< `REDACTED` \> hello engedélyezési jogkivonat a korábban kapott.</span><span class="sxs-lookup"><span data-stu-id="20fdc-169">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span>

<span data-ttu-id="20fdc-170">Ha hello művelet sikeresen befejeződik a következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="20fdc-170">You should see a response like this if hello operation completes successfully:</span></span>

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a><span data-ttu-id="20fdc-171">Adatok feltöltése egy Data Lake Store-fiókba</span><span class="sxs-lookup"><span data-stu-id="20fdc-171">Upload data into a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-172">Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span><span class="sxs-lookup"><span data-stu-id="20fdc-172">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span></span>

<span data-ttu-id="20fdc-173">A következő cURL-parancsot hello használata.</span><span class="sxs-lookup"><span data-stu-id="20fdc-173">Use hello following cURL command.</span></span> <span data-ttu-id="20fdc-174">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-174">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

<span data-ttu-id="20fdc-175">A fenti szintaxis hello **-T** paraméter hello fájl helyét, hello meg feltölteni.</span><span class="sxs-lookup"><span data-stu-id="20fdc-175">In hello above syntax **-T** parameter is hello location of hello file you are uploading.</span></span>

<span data-ttu-id="20fdc-176">hello hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="20fdc-176">hello output is similar toohello following:</span></span>
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a><span data-ttu-id="20fdc-177">Adatok beolvasása a Data Lake Store-fiókból</span><span class="sxs-lookup"><span data-stu-id="20fdc-177">Read data from a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-178">Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span><span class="sxs-lookup"><span data-stu-id="20fdc-178">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span></span>

<span data-ttu-id="20fdc-179">Az adatok beolvasása a Data Lake Store-fiókból egy kétlépéses folyamat.</span><span class="sxs-lookup"><span data-stu-id="20fdc-179">Reading data from a Data Lake Store account is a two-step process.</span></span>

* <span data-ttu-id="20fdc-180">Először küldenie kell egy GET kérelmet hello végpont `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span><span class="sxs-lookup"><span data-stu-id="20fdc-180">You first submit a GET request against hello endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span></span> <span data-ttu-id="20fdc-181">Ezzel visszatér a hely toosubmit hello következő GET kérelmet.</span><span class="sxs-lookup"><span data-stu-id="20fdc-181">This will return a location toosubmit hello next GET request to.</span></span>
* <span data-ttu-id="20fdc-182">Ezután küldenie kell hello GET kérelmet hello végpont `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span><span class="sxs-lookup"><span data-stu-id="20fdc-182">You then submit hello GET request against hello endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span></span> <span data-ttu-id="20fdc-183">Ez megjeleníti hello hello fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="20fdc-183">This will display hello contents of hello file.</span></span>

<span data-ttu-id="20fdc-184">Azonban mivel nincs különbség hello a bemeneti paraméterek között hello először és hello második lépésben, használhatja a hello `-L` paraméter toosubmit hello első kérésre.</span><span class="sxs-lookup"><span data-stu-id="20fdc-184">However, because there is no difference in hello input parameters between hello first and hello second step, you can use hello `-L` parameter toosubmit hello first request.</span></span> <span data-ttu-id="20fdc-185">`-L`a beállítás lényegében egyesít két kérelmet egy, és biztosítják a cURL hello kérelem hello új helyen.</span><span class="sxs-lookup"><span data-stu-id="20fdc-185">`-L` option essentially combines two requests into one and will make cURL redo hello request on hello new location.</span></span> <span data-ttu-id="20fdc-186">Végül minden hello kérelem hívások hello kimenet jelenik meg, például alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="20fdc-186">Finally, hello output from all hello request calls is displayed, like shown below.</span></span> <span data-ttu-id="20fdc-187">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-187">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

<span data-ttu-id="20fdc-188">Egy kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="20fdc-188">You should see an output similar toohello following:</span></span>

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="20fdc-189">Fájl átnevezése a Data Lake Store-fiókban</span><span class="sxs-lookup"><span data-stu-id="20fdc-189">Rename a file in a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-190">Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="20fdc-190">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span></span>

<span data-ttu-id="20fdc-191">Használjon hello alábbi cURL parancs toorename egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="20fdc-191">Use hello following cURL command toorename a file.</span></span> <span data-ttu-id="20fdc-192">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-192">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

<span data-ttu-id="20fdc-193">Egy kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="20fdc-193">You should see an output similar toohello following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a><span data-ttu-id="20fdc-194">Fájl törlése a Data Lake Store-fiókból</span><span class="sxs-lookup"><span data-stu-id="20fdc-194">Delete a file from a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-195">Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="20fdc-195">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span></span>

<span data-ttu-id="20fdc-196">Használjon hello alábbi cURL parancs toodelete egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="20fdc-196">Use hello following cURL command toodelete a file.</span></span> <span data-ttu-id="20fdc-197">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-197">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

<span data-ttu-id="20fdc-198">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="20fdc-198">You should see an output like hello following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="20fdc-199">Data Lake Store-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="20fdc-199">Delete a Data Lake Store account</span></span>
<span data-ttu-id="20fdc-200">Ez a művelet alapján definiált hello REST API-hívás [Itt](https://msdn.microsoft.com/library/mt694075.aspx).</span><span class="sxs-lookup"><span data-stu-id="20fdc-200">This operation is based on hello REST API call defined [here](https://msdn.microsoft.com/library/mt694075.aspx).</span></span>

<span data-ttu-id="20fdc-201">Használjon hello alábbi cURL parancs toodelete Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="20fdc-201">Use hello following cURL command toodelete a Data Lake Store account.</span></span> <span data-ttu-id="20fdc-202">Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.</span><span class="sxs-lookup"><span data-stu-id="20fdc-202">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

<span data-ttu-id="20fdc-203">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="20fdc-203">You should see an output like hello following:</span></span>

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a><span data-ttu-id="20fdc-204">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="20fdc-204">See also</span></span>
* [<span data-ttu-id="20fdc-205">Az Azure Data Lake Store-ral kompatibilis nyílt forráskódú big data-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="20fdc-205">Open Source Big Data applications compatible with Azure Data Lake Store</span></span>](data-lake-store-compatible-oss-other-applications.md)

