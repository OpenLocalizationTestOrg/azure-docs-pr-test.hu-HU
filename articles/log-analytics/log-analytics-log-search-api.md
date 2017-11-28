---
title: "aaaLog Analytics napló search REST API |} Microsoft Docs"
description: "Ez az útmutató egy alapszintű hello használatát ismertető oktatóanyag Naplóelemzési keresni REST API hello Operations Management Suite (OMS), és példák, amelyek bemutatják, hogyan biztosít toouse hello parancsok."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: dafe5eeb8cc11a339f2cbf78cec657e344d87cac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-log-search-rest-api"></a><span data-ttu-id="e0144-103">A Naplóelemzési jelentkezzen search REST API-n</span><span class="sxs-lookup"><span data-stu-id="e0144-103">Log Analytics log search REST API</span></span>
<span data-ttu-id="e0144-104">Ez az útmutató egy alapszintű oktatóanyag, beleértve a példákat a hello napló Analytics Search REST API használatát.</span><span class="sxs-lookup"><span data-stu-id="e0144-104">This guide provides a basic tutorial, including examples, of how you can use hello Log Analytics Search REST API.</span></span> <span data-ttu-id="e0144-105">A Naplóelemzési hello Operations Management Suite (OMS) részét képezi.</span><span class="sxs-lookup"><span data-stu-id="e0144-105">Log Analytics is part of hello Operations Management Suite (OMS).</span></span>

> [!NOTE]
> <span data-ttu-id="e0144-106">Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd folytatja kell toouse hello örökölt lekérdezési nyelv hello napló keresése API az ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="e0144-106">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should continue toouse hello legacy query language with hello log search API as described in this article.</span></span>  <span data-ttu-id="e0144-107">Egy olyan új API megjelenik frissített munkaterületek, valamint hello dokumentáció frissülni fog ekkor.</span><span class="sxs-lookup"><span data-stu-id="e0144-107">A new API will be released for upgraded workspaces, and hello documentation will be updated at that time.</span></span> 

> [!NOTE]
> <span data-ttu-id="e0144-108">Naplóelemzési korábban meghívták az Operational Insights, ezért az erőforrás-szolgáltatón hello használt hello név.</span><span class="sxs-lookup"><span data-stu-id="e0144-108">Log Analytics was previously called Operational Insights, which is why it is hello name used in hello resource provider.</span></span>
>
>

## <a name="overview-of-hello-log-search-rest-api"></a><span data-ttu-id="e0144-109">Hello napló Search REST API – áttekintés</span><span class="sxs-lookup"><span data-stu-id="e0144-109">Overview of hello Log Search REST API</span></span>
<span data-ttu-id="e0144-110">hello napló Analytics Search REST API RESTful és hello Azure Resource Manager API keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="e0144-110">hello Log Analytics Search REST API is RESTful and can be accessed via hello Azure Resource Manager API.</span></span> <span data-ttu-id="e0144-111">Ez a cikk példákat hello API használatával történő eléréséhez [ARMClient](https://github.com/projectkudu/ARMClient), egy nyílt forráskódú parancssori eszköz, amely egyszerűbbé teszi a meghívása hello Azure Resource Manager API-val.</span><span class="sxs-lookup"><span data-stu-id="e0144-111">This article provides examples of accessing hello API through [ARMClient](https://github.com/projectkudu/ARMClient), an open source command-line tool that simplifies invoking hello Azure Resource Manager API.</span></span> <span data-ttu-id="e0144-112">hello ARMClient használata sok beállítások tooaccess hello napló Analytics Search API közül.</span><span class="sxs-lookup"><span data-stu-id="e0144-112">hello use of ARMClient is one of many options tooaccess hello Log Analytics Search API.</span></span> <span data-ttu-id="e0144-113">Másik lehetőség is toouse hello Azure PowerShell-modul OperationalInsights, amely keresési eléréséhez parancsmagokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e0144-113">Another option is toouse hello Azure PowerShell module for OperationalInsights, which includes cmdlets for accessing search.</span></span> <span data-ttu-id="e0144-114">Ezekkel az eszközökkel hello Azure Resource Manager API toomake hívások tooOMS munkaterületek használják, és hajtsa végre a keresési parancsok rajtuk.</span><span class="sxs-lookup"><span data-stu-id="e0144-114">With these tools, you can utilize hello Azure Resource Manager API toomake calls tooOMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="e0144-115">hello API kiírja a keresési eredmények JSON formátumban, így toouse hello keresési eredmények között számos különböző módon programozott módon.</span><span class="sxs-lookup"><span data-stu-id="e0144-115">hello API outputs search results in JSON format, allowing you toouse hello search results in many different ways programmatically.</span></span>

<span data-ttu-id="e0144-116">hello Azure Resource Manager használható keresztül egy [.NET-keretrendszerhez készült](https://msdn.microsoft.com/library/azure/dn910477.aspx) és hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0144-116">hello Azure Resource Manager can be used via a [Library for .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) and hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span></span> <span data-ttu-id="e0144-117">toolearn több, tekintse át a kapcsolódó hello weblapok.</span><span class="sxs-lookup"><span data-stu-id="e0144-117">toolearn more, review hello linked web pages.</span></span>

> [!NOTE]
> <span data-ttu-id="e0144-118">Ha egy összesítési paranccsal például `|measure count()` vagy `distinct`, minden hívás toosearch visszatérhessen legfeljebb 500 000 rekordot.</span><span class="sxs-lookup"><span data-stu-id="e0144-118">If you use an aggregation command such as `|measure count()` or `distinct`, each call toosearch can return upto 500,000 records.</span></span> <span data-ttu-id="e0144-119">Keresések, amelyek nem tartalmaznak egy összesítési parancs legfeljebb 5 000 rekordot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="e0144-119">Searches that do not include an aggregation command return upto 5,000 records.</span></span>
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a><span data-ttu-id="e0144-120">Egyszerű napló Analytics Search REST API-oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="e0144-120">Basic Log Analytics Search REST API tutorial</span></span>
### <a name="toouse-armclient"></a><span data-ttu-id="e0144-121">toouse ARMClient</span><span class="sxs-lookup"><span data-stu-id="e0144-121">toouse ARMClient</span></span>
1. <span data-ttu-id="e0144-122">Telepítés [Chocolatey](https://chocolatey.org/), amelyen egy megnyitott forrás Package Manager Windows.</span><span class="sxs-lookup"><span data-stu-id="e0144-122">Install [Chocolatey](https://chocolatey.org/), which is an Open Source Package Manager for Windows.</span></span> <span data-ttu-id="e0144-123">Nyisson meg egy parancssort rendszergazdaként, és futtassa a parancsot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="e0144-123">Open a command prompt window as administrator and then run hello following command:</span></span>

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. <span data-ttu-id="e0144-124">Telepítse a ARMClient hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e0144-124">Install ARMClient by running hello following command:</span></span>

    ```
    choco install armclient
    ```

### <a name="tooperform-a-search-using-armclient"></a><span data-ttu-id="e0144-125">a kívánt ARMClient használatával tooperform</span><span class="sxs-lookup"><span data-stu-id="e0144-125">tooperform a search using ARMClient</span></span>
1. <span data-ttu-id="e0144-126">Jelentkezzen be Microsoft-fiókjával vagy a munkahelyi vagy iskolai fiók használatával:</span><span class="sxs-lookup"><span data-stu-id="e0144-126">Log in using your Microsoft account or your work or school account:</span></span>

    ```
    armclient login
    ```

    <span data-ttu-id="e0144-127">Sikeres bejelentkezés megadott fiók összes kötött előfizetések toohello sorolja fel:</span><span class="sxs-lookup"><span data-stu-id="e0144-127">A successful login lists all subscriptions tied toohello given account:</span></span>

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. <span data-ttu-id="e0144-128">Hello Operations Management Suite-munkaterület érhető el:</span><span class="sxs-lookup"><span data-stu-id="e0144-128">Get hello Operations Management Suite workspaces:</span></span>

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    <span data-ttu-id="e0144-129">Sikeres Get hívást volna kimeneti összes munkaterületek toohello előfizetés társítva:</span><span class="sxs-lookup"><span data-stu-id="e0144-129">A successful Get call would output all workspaces tied toohello subscription:</span></span>

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. <span data-ttu-id="e0144-130">A keresési változó létrehozása:</span><span class="sxs-lookup"><span data-stu-id="e0144-130">Create your search variable:</span></span>

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. <span data-ttu-id="e0144-131">A Keresés az új keresés változó használatával:</span><span class="sxs-lookup"><span data-stu-id="e0144-131">Search using your new search variable:</span></span>

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a><span data-ttu-id="e0144-132">Naplófájl Analytics Search REST API referencia példák</span><span class="sxs-lookup"><span data-stu-id="e0144-132">Log Analytics Search REST API reference examples</span></span>
<span data-ttu-id="e0144-133">hello a következő példák bemutatják, hogyan használhatja a keresés API hello.</span><span class="sxs-lookup"><span data-stu-id="e0144-133">hello following examples show you how you can use hello Search API.</span></span>

### <a name="search---actionread"></a><span data-ttu-id="e0144-134">Keresési - művelet olvasása</span><span class="sxs-lookup"><span data-stu-id="e0144-134">Search - Action/Read</span></span>
<span data-ttu-id="e0144-135">**A minta URL-címe:**</span><span class="sxs-lookup"><span data-stu-id="e0144-135">**Sample Url:**</span></span>

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

<span data-ttu-id="e0144-136">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e0144-136">**Request:**</span></span>

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
<span data-ttu-id="e0144-137">a következő táblázat hello hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e0144-137">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="e0144-138">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="e0144-138">**Property**</span></span> | <span data-ttu-id="e0144-139">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="e0144-139">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="e0144-140">Felső</span><span class="sxs-lookup"><span data-stu-id="e0144-140">top</span></span> |<span data-ttu-id="e0144-141">az eredmények tooreturn hello maximális számát.</span><span class="sxs-lookup"><span data-stu-id="e0144-141">hello maximum number of results tooreturn.</span></span> |
| <span data-ttu-id="e0144-142">Jelöljön ki</span><span class="sxs-lookup"><span data-stu-id="e0144-142">highlight</span></span> |<span data-ttu-id="e0144-143">Megfelelő mezőkben konzolban használt általában előtti és utáni paramétereket tartalmaz</span><span class="sxs-lookup"><span data-stu-id="e0144-143">Contains pre and post parameters, used usually for highlighting matching fields</span></span> |
| <span data-ttu-id="e0144-144">előre</span><span class="sxs-lookup"><span data-stu-id="e0144-144">pre</span></span> |<span data-ttu-id="e0144-145">Előtagok hello megadott karakterlánc megfelel tooyour mezőket.</span><span class="sxs-lookup"><span data-stu-id="e0144-145">Prefixes hello given string tooyour matched fields.</span></span> |
| <span data-ttu-id="e0144-146">Post</span><span class="sxs-lookup"><span data-stu-id="e0144-146">post</span></span> |<span data-ttu-id="e0144-147">Hozzáfűzi a megadott karakterlánc megfelel tooyour mezők hello.</span><span class="sxs-lookup"><span data-stu-id="e0144-147">Appends hello given string tooyour matched fields.</span></span> |
| <span data-ttu-id="e0144-148">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e0144-148">query</span></span> |<span data-ttu-id="e0144-149">hello keresési lekérdezés toocollect használja, és adja vissza az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="e0144-149">hello search query used toocollect and return results.</span></span> |
| <span data-ttu-id="e0144-150">start</span><span class="sxs-lookup"><span data-stu-id="e0144-150">start</span></span> |<span data-ttu-id="e0144-151">azt szeretné, hogy a található eredmények toobe hello időkerete hello elejére.</span><span class="sxs-lookup"><span data-stu-id="e0144-151">hello beginning of hello time window you want results toobe found from.</span></span> |
| <span data-ttu-id="e0144-152">Vége</span><span class="sxs-lookup"><span data-stu-id="e0144-152">end</span></span> |<span data-ttu-id="e0144-153">azt szeretné, hogy a található eredmények toobe hello időkerete hello végét.</span><span class="sxs-lookup"><span data-stu-id="e0144-153">hello end of hello time window you want results toobe found from.</span></span> |

<span data-ttu-id="e0144-154">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="e0144-154">**Response:**</span></span>

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a><span data-ttu-id="e0144-155">Keresés / {azonosító} - művelet olvasása</span><span class="sxs-lookup"><span data-stu-id="e0144-155">Search/{ID} - Action/Read</span></span>
<span data-ttu-id="e0144-156">**A kérelem egy mentett keresés hello tartalma:**</span><span class="sxs-lookup"><span data-stu-id="e0144-156">**Request hello contents of a Saved Search:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> <span data-ttu-id="e0144-157">Ha hello keresési "Folyamatban" állapotú ad vissza, majd lekérdezési frissítése hello eredmények végezhető el ez az API.</span><span class="sxs-lookup"><span data-stu-id="e0144-157">If hello search returns with a ‘Pending’ status, then polling hello updated results can be done through this API.</span></span> <span data-ttu-id="e0144-158">6 perc után hello hello keresés eredménye így a rendszer eldobja hello gyorsítótárból, és HTTP állapotba adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e0144-158">After 6 min, hello result of hello search will be dropped from hello cache and HTTP Gone will be returned.</span></span> <span data-ttu-id="e0144-159">Hello kezdeti keresési kérelem azonnal visszaadja a "Sikeres" állapotba, ha hello eredmények nem lettek hozzáadva a HTTP már meg API tooreturn okoz, ha a lekérdezés toohello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="e0144-159">If hello initial search request returns a ‘Successful’ status immediately, hello results are not added toohello cache causing this API tooreturn HTTP Gone if queried.</span></span> <span data-ttu-id="e0144-160">hello egy 200-as HTTP-eredmény tartalma a hello hello kezdeti keresési kérelem csak a frissített értékekkel formátuma azonos.</span><span class="sxs-lookup"><span data-stu-id="e0144-160">hello contents of an HTTP 200 result are in hello same format as hello initial search request just with updated values.</span></span>
>
>

### <a name="saved-searches"></a><span data-ttu-id="e0144-161">Mentett keresések</span><span class="sxs-lookup"><span data-stu-id="e0144-161">Saved searches</span></span>
<span data-ttu-id="e0144-162">**A kérelem mentett keresések listája:**</span><span class="sxs-lookup"><span data-stu-id="e0144-162">**Request List of Saved Searches:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

<span data-ttu-id="e0144-163">Támogatott módszerek: GET PUT törlése</span><span class="sxs-lookup"><span data-stu-id="e0144-163">Supported methods: GET PUT DELETE</span></span>

<span data-ttu-id="e0144-164">Gyűjtemény módszereket támogatja: beolvasása</span><span class="sxs-lookup"><span data-stu-id="e0144-164">Supported collection methods: GET</span></span>

<span data-ttu-id="e0144-165">a következő táblázat hello hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e0144-165">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="e0144-166">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e0144-166">Property</span></span> | <span data-ttu-id="e0144-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="e0144-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e0144-168">Azonosító</span><span class="sxs-lookup"><span data-stu-id="e0144-168">Id</span></span> |<span data-ttu-id="e0144-169">hello egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e0144-169">hello unique identifier.</span></span> |
| <span data-ttu-id="e0144-170">ETag</span><span class="sxs-lookup"><span data-stu-id="e0144-170">Etag</span></span> |<span data-ttu-id="e0144-171">**Szükséges javítás**.</span><span class="sxs-lookup"><span data-stu-id="e0144-171">**Required for Patch**.</span></span> <span data-ttu-id="e0144-172">Minden egyes kiszolgáló frissítve.</span><span class="sxs-lookup"><span data-stu-id="e0144-172">Updated by server on each write.</span></span> <span data-ttu-id="e0144-173">Érték egyenlő toohello aktuális tárolt értéknek kell lennie, vagy "*" tooupdate.</span><span class="sxs-lookup"><span data-stu-id="e0144-173">Value must be equal toohello current stored value or ‘*’ tooupdate.</span></span> <span data-ttu-id="e0144-174">409 régi vagy érvénytelen értéket adott vissza.</span><span class="sxs-lookup"><span data-stu-id="e0144-174">409 returned for old/invalid values.</span></span> |
| <span data-ttu-id="e0144-175">Properties.Query</span><span class="sxs-lookup"><span data-stu-id="e0144-175">properties.query</span></span> |<span data-ttu-id="e0144-176">**Szükséges**.</span><span class="sxs-lookup"><span data-stu-id="e0144-176">**Required**.</span></span> <span data-ttu-id="e0144-177">hello keresési lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="e0144-177">hello search query.</span></span> |
| <span data-ttu-id="e0144-178">properties.displayName</span><span class="sxs-lookup"><span data-stu-id="e0144-178">properties.displayName</span></span> |<span data-ttu-id="e0144-179">**Szükséges**.</span><span class="sxs-lookup"><span data-stu-id="e0144-179">**Required**.</span></span> <span data-ttu-id="e0144-180">hello lekérdezés hello felhasználó által definiált megjelenítési neve.</span><span class="sxs-lookup"><span data-stu-id="e0144-180">hello user-defined display name of hello query.</span></span> |
| <span data-ttu-id="e0144-181">Properties.Category</span><span class="sxs-lookup"><span data-stu-id="e0144-181">properties.category</span></span> |<span data-ttu-id="e0144-182">**Szükséges**.</span><span class="sxs-lookup"><span data-stu-id="e0144-182">**Required**.</span></span> <span data-ttu-id="e0144-183">hello felhasználói kategória hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="e0144-183">hello user-defined category of hello query.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0144-184">hello Naplóelemzési keresési API-JÁNAK jelenleg ad vissza, felhasználó által létrehozott mentett keresések során kérdezi le azt a mentett kereséseket a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="e0144-184">hello Log Analytics search API currently returns user-created saved searches when polled for saved searches in a workspace.</span></span> <span data-ttu-id="e0144-185">hello API nem ad vissza a mentett keresések megoldások által biztosított.</span><span class="sxs-lookup"><span data-stu-id="e0144-185">hello API does not return saved searches provided by solutions.</span></span>
>
>

### <a name="create-saved-searches"></a><span data-ttu-id="e0144-186">Mentett keresések létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0144-186">Create saved searches</span></span>
<span data-ttu-id="e0144-187">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e0144-187">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> <span data-ttu-id="e0144-188">minden mentett keresések, ütemezéseihez és hello napló Analytics API létre műveletek hello nevét kisbetűs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e0144-188">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

### <a name="delete-saved-searches"></a><span data-ttu-id="e0144-189">Törölje a mentett kereséseket</span><span class="sxs-lookup"><span data-stu-id="e0144-189">Delete saved searches</span></span>
<span data-ttu-id="e0144-190">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e0144-190">**Request:**</span></span>

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a><span data-ttu-id="e0144-191">Mentett keresések frissítése</span><span class="sxs-lookup"><span data-stu-id="e0144-191">Update saved searches</span></span>
 <span data-ttu-id="e0144-192">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e0144-192">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a><span data-ttu-id="e0144-193">Metaadat - JSON csak</span><span class="sxs-lookup"><span data-stu-id="e0144-193">Metadata - JSON only</span></span>
<span data-ttu-id="e0144-194">Íme egy módon toosee hello mezők a munkaterület hello adatgyűjtés az összes napló típusához.</span><span class="sxs-lookup"><span data-stu-id="e0144-194">Here’s a way toosee hello fields for all log types for hello data collected in your workspace.</span></span> <span data-ttu-id="e0144-195">Például ha szeretné tudni, hogy ha hello eseménytípus van-e a számítógép neve mező, akkor ez a lekérdezés értéke egyirányú toocheck.</span><span class="sxs-lookup"><span data-stu-id="e0144-195">For example, if you want you know if hello Event type has a field named Computer, then this query is one way toocheck.</span></span>

<span data-ttu-id="e0144-196">**Kérelem mezők:**</span><span class="sxs-lookup"><span data-stu-id="e0144-196">**Request for Fields:**</span></span>

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

<span data-ttu-id="e0144-197">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="e0144-197">**Response:**</span></span>

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

<span data-ttu-id="e0144-198">a következő táblázat hello hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e0144-198">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="e0144-199">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="e0144-199">**Property**</span></span> | <span data-ttu-id="e0144-200">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="e0144-200">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="e0144-201">név</span><span class="sxs-lookup"><span data-stu-id="e0144-201">name</span></span> |<span data-ttu-id="e0144-202">Mező neve.</span><span class="sxs-lookup"><span data-stu-id="e0144-202">Field name.</span></span> |
| <span data-ttu-id="e0144-203">displayName</span><span class="sxs-lookup"><span data-stu-id="e0144-203">displayName</span></span> |<span data-ttu-id="e0144-204">hello hello mező nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e0144-204">hello display name of hello field.</span></span> |
| <span data-ttu-id="e0144-205">type</span><span class="sxs-lookup"><span data-stu-id="e0144-205">type</span></span> |<span data-ttu-id="e0144-206">hello hello mező értékének típusa.</span><span class="sxs-lookup"><span data-stu-id="e0144-206">hello Type of hello field value.</span></span> |
| <span data-ttu-id="e0144-207">kategorizálható</span><span class="sxs-lookup"><span data-stu-id="e0144-207">facetable</span></span> |<span data-ttu-id="e0144-208">"Indexed", aktuális kombinációja "tárolt" és "dimenzió" tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="e0144-208">Combination of current ‘indexed’, ‘stored ‘and ‘facet’ properties.</span></span> |
| <span data-ttu-id="e0144-209">Megjelenítése</span><span class="sxs-lookup"><span data-stu-id="e0144-209">display</span></span> |<span data-ttu-id="e0144-210">Aktuális "üzenet megjelenítése" tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e0144-210">Current ‘display’ property.</span></span> <span data-ttu-id="e0144-211">Értéke TRUE, ha mező a keresési látható.</span><span class="sxs-lookup"><span data-stu-id="e0144-211">True if field is visible in search.</span></span> |
| <span data-ttu-id="e0144-212">ownerType</span><span class="sxs-lookup"><span data-stu-id="e0144-212">ownerType</span></span> |<span data-ttu-id="e0144-213">Csökkentett tooonly típusok tooonboarded IP-címekhez tartozó.</span><span class="sxs-lookup"><span data-stu-id="e0144-213">Reduced tooonly Types belonging tooonboarded IP’s.</span></span> |

## <a name="optional-parameters"></a><span data-ttu-id="e0144-214">Nem kötelező paraméterek</span><span class="sxs-lookup"><span data-stu-id="e0144-214">Optional parameters</span></span>
<span data-ttu-id="e0144-215">a következő információk hello rendelkezésre álló választható paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e0144-215">hello following information describes optional parameters available.</span></span>

### <a name="highlighting"></a><span data-ttu-id="e0144-216">Kiemelve</span><span class="sxs-lookup"><span data-stu-id="e0144-216">Highlighting</span></span>
<span data-ttu-id="e0144-217">hello "Highlight" paraméter egy nem kötelező paraméter, használhatja a toorequest hello keresési alrendszer tartalmaznak jelölők válaszában.</span><span class="sxs-lookup"><span data-stu-id="e0144-217">hello “Highlight” parameter is an optional parameter you may use toorequest hello search subsystem include a set of markers in its response.</span></span>

<span data-ttu-id="e0144-218">Ezek a jelölők hello start jelzik, és a záró kijelölt szöveg, amely megfelel a keresési lekérdezés megadott hello feltételeket.</span><span class="sxs-lookup"><span data-stu-id="e0144-218">These markers indicate hello start and end highlighted text that matches hello terms provided in your search query.</span></span>
<span data-ttu-id="e0144-219">Előfordulhat, hogy adja meg a hello kezdő és záró jelölők toowrap kiemelt hello keresőkifejezéssel által használt.</span><span class="sxs-lookup"><span data-stu-id="e0144-219">You may specify hello start and end markers that are used by search toowrap hello highlighted term.</span></span>

<span data-ttu-id="e0144-220">**Példa keresési lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="e0144-220">**Example search query**</span></span>

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

<span data-ttu-id="e0144-221">**A minta eredmény:**</span><span class="sxs-lookup"><span data-stu-id="e0144-221">**Sample result:**</span></span>

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

<span data-ttu-id="e0144-222">Figyelje meg, hogy az előző eredmény hello olyan hibaüzenetet, amely a következő előtaggal, és hozzáfűzi tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e0144-222">Notice that hello preceding result contains an error message that has been prefixed and appended.</span></span>

## <a name="computer-groups"></a><span data-ttu-id="e0144-223">Számítógépcsoportok</span><span class="sxs-lookup"><span data-stu-id="e0144-223">Computer groups</span></span>
<span data-ttu-id="e0144-224">Számítógépcsoportok különleges mentett keresések, amely a számítógépek készlettel tért vissza.</span><span class="sxs-lookup"><span data-stu-id="e0144-224">Computer groups are special saved searches that return a set of computers.</span></span>  <span data-ttu-id="e0144-225">Más lekérdezések toolimit hello eredmények toohello csoportban lévő számítógépek hello is használhatja a számítógép (csoport).</span><span class="sxs-lookup"><span data-stu-id="e0144-225">You can use a computer group in other queries toolimit hello results toohello computers in hello group.</span></span>  <span data-ttu-id="e0144-226">A számítógép (csoport), a mentett kereséseket értékkel rendelkező számítógép csoport címkével ellátott lett megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="e0144-226">A computer group is implemented as a saved search with a Group tag with a value of Computer.</span></span>

<span data-ttu-id="e0144-227">Az alábbiakban látható egy számítógépcsoport mintát választ.</span><span class="sxs-lookup"><span data-stu-id="e0144-227">Following is a sample response for a computer group.</span></span>

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a><span data-ttu-id="e0144-228">Számítógépcsoportok beolvasása</span><span class="sxs-lookup"><span data-stu-id="e0144-228">Retrieving computer groups</span></span>
<span data-ttu-id="e0144-229">tooretrieve egy számítógépcsoport használata hello Get metódus hello csoporttal azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="e0144-229">tooretrieve a computer group, use hello Get method with hello group ID.</span></span>

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a><span data-ttu-id="e0144-230">Létrehozásakor vagy frissítésekor. a számítógép (csoport)</span><span class="sxs-lookup"><span data-stu-id="e0144-230">Creating or updating a computer group</span></span>
<span data-ttu-id="e0144-231">toocreate egy számítógépcsoport hello Put metódust használja a mentett keresés egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="e0144-231">toocreate a computer group, use hello Put method with a unique saved search ID.</span></span> <span data-ttu-id="e0144-232">Használjon egy meglévő számítógép-csoport azonosítója, ha egy módosul.</span><span class="sxs-lookup"><span data-stu-id="e0144-232">If you use an existing computer group ID, then that one is modified.</span></span> <span data-ttu-id="e0144-233">Ha hello Log Analytics-portálról egy számítógép (csoport) hoz létre, majd hello azonosító hello csoportból, neve jön létre.</span><span class="sxs-lookup"><span data-stu-id="e0144-233">When you create a computer group in hello Log Analytics portal, then hello ID is created from hello group and name.</span></span>

<span data-ttu-id="e0144-234">hello lekérdezés hello csoport definíciójának megfelelően a hello csoport toofunction számítógépcsoportot kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="e0144-234">hello query used for hello group definition must return a set of computers for hello group toofunction properly.</span></span>  <span data-ttu-id="e0144-235">Javasoljuk, hogy a lekérdezés leállítása `| Distinct Computer` tooensure hello megfelelő adat.</span><span class="sxs-lookup"><span data-stu-id="e0144-235">It's recommended that you end your query with `| Distinct Computer` tooensure hello correct data is returned.</span></span>

<span data-ttu-id="e0144-236">mentett keresés hello hello meghatározása tartalmaznia kell egy csoport nevű számítógép érték hello keresési toobe fontosként megjelölt számítógépcsoport címkét.</span><span class="sxs-lookup"><span data-stu-id="e0144-236">hello definition of hello saved search must include a tag named Group with a value of Computer for hello search toobe classified as a computer group.</span></span>

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a><span data-ttu-id="e0144-237">Számítógép-csoportok törlése</span><span class="sxs-lookup"><span data-stu-id="e0144-237">Deleting computer groups</span></span>
<span data-ttu-id="e0144-238">toodelete egy számítógépcsoport használata hello Delete metódust hello csoporttal azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="e0144-238">toodelete a computer group, use hello Delete method with hello group ID.</span></span>

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a><span data-ttu-id="e0144-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0144-239">Next steps</span></span>
* <span data-ttu-id="e0144-240">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) toobuild lekérdezések egyéni mezők feltétel használatával.</span><span class="sxs-lookup"><span data-stu-id="e0144-240">Learn about [log searches](log-analytics-log-searches.md) toobuild queries using custom fields for criteria.</span></span>
