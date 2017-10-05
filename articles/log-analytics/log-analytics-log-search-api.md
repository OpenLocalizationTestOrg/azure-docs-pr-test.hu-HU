---
title: "Naplóelemzési jelentkezzen search REST API |} Microsoft Docs"
description: "Ez az útmutató egy alapszintű oktatóanyag: hogyan használhatja a Naplóelemzési search REST API-t az Operations Management Suite (OMS) és példák a parancsok használata biztosít."
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
ms.openlocfilehash: 78afb2f065dde4a3e7a3ab787c939b3c52b72cc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="log-analytics-log-search-rest-api"></a><span data-ttu-id="e95e7-103">A Naplóelemzési jelentkezzen search REST API-n</span><span class="sxs-lookup"><span data-stu-id="e95e7-103">Log Analytics log search REST API</span></span>
<span data-ttu-id="e95e7-104">Ez az útmutató egy alapszintű oktatóanyag, valamint az, a napló Analytics Search REST API használatát.</span><span class="sxs-lookup"><span data-stu-id="e95e7-104">This guide provides a basic tutorial, including examples, of how you can use the Log Analytics Search REST API.</span></span> <span data-ttu-id="e95e7-105">A Naplóelemzési része az Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="e95e7-105">Log Analytics is part of the Operations Management Suite (OMS).</span></span>

> [!NOTE]
> <span data-ttu-id="e95e7-106">Ha a munkaterületet lett frissítve a [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor kell továbbra is a hagyományos lekérdezési nyelv használata a naplófájl-keresési API, ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="e95e7-106">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should continue to use the legacy query language with the log search API as described in this article.</span></span>  <span data-ttu-id="e95e7-107">Egy olyan új API megjelenik frissített munkaterületek, valamint a dokumentáció frissülni fog ekkor.</span><span class="sxs-lookup"><span data-stu-id="e95e7-107">A new API will be released for upgraded workspaces, and the documentation will be updated at that time.</span></span> 

> [!NOTE]
> <span data-ttu-id="e95e7-108">Naplóelemzési korábban meghívták az Operational Insights, ezért az erőforrás-szolgáltató használt a név.</span><span class="sxs-lookup"><span data-stu-id="e95e7-108">Log Analytics was previously called Operational Insights, which is why it is the name used in the resource provider.</span></span>
>
>

## <a name="overview-of-the-log-search-rest-api"></a><span data-ttu-id="e95e7-109">A naplófájl-keresési REST API – áttekintés</span><span class="sxs-lookup"><span data-stu-id="e95e7-109">Overview of the Log Search REST API</span></span>
<span data-ttu-id="e95e7-110">A napló Analytics Search REST API RESTful, és az Azure Resource Manager API-n keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="e95e7-110">The Log Analytics Search REST API is RESTful and can be accessed via the Azure Resource Manager API.</span></span> <span data-ttu-id="e95e7-111">Ez a cikk ismerteti az API használatával történő eléréséhez példák [ARMClient](https://github.com/projectkudu/ARMClient), egy nyílt forráskódú parancssori eszköz, amely leegyszerűsíti az Azure Resource Manager API meghívása.</span><span class="sxs-lookup"><span data-stu-id="e95e7-111">This article provides examples of accessing the API through [ARMClient](https://github.com/projectkudu/ARMClient), an open source command-line tool that simplifies invoking the Azure Resource Manager API.</span></span> <span data-ttu-id="e95e7-112">A ARMClient használata számos lehetőség a napló Analytics Search API eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e95e7-112">The use of ARMClient is one of many options to access the Log Analytics Search API.</span></span> <span data-ttu-id="e95e7-113">Lehetősége az Azure PowerShell modul használandó OperationalInsights, amely keresési eléréséhez parancsmagokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e95e7-113">Another option is to use the Azure PowerShell module for OperationalInsights, which includes cmdlets for accessing search.</span></span> <span data-ttu-id="e95e7-114">Ezekkel az eszközökkel használhatja az Azure Resource Manager API-hívások indítása az OMS-munkaterület, és rajtuk keresési parancsok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="e95e7-114">With these tools, you can utilize the Azure Resource Manager API to make calls to OMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="e95e7-115">Az API-t kiírja a keresési eredmények JSON formátumban, hogy lehetővé teszi a programozott módon használja a keresési eredmények között számos különböző módja.</span><span class="sxs-lookup"><span data-stu-id="e95e7-115">The API outputs search results in JSON format, allowing you to use the search results in many different ways programmatically.</span></span>

<span data-ttu-id="e95e7-116">Az Azure Resource Managerrel használható keresztül egy [.NET-keretrendszerhez készült](https://msdn.microsoft.com/library/azure/dn910477.aspx) és a [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span><span class="sxs-lookup"><span data-stu-id="e95e7-116">The Azure Resource Manager can be used via a [Library for .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) and the [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span></span> <span data-ttu-id="e95e7-117">További információkért tekintse át a csatolt weblapokat.</span><span class="sxs-lookup"><span data-stu-id="e95e7-117">To learn more, review the linked web pages.</span></span>

> [!NOTE]
> <span data-ttu-id="e95e7-118">Ha egy összesítési paranccsal például `|measure count()` vagy `distinct`, minden hívás kereséséhez lépjen vissza legfeljebb 500 000 rekordot.</span><span class="sxs-lookup"><span data-stu-id="e95e7-118">If you use an aggregation command such as `|measure count()` or `distinct`, each call to search can return upto 500,000 records.</span></span> <span data-ttu-id="e95e7-119">Keresések, amelyek nem tartalmaznak egy összesítési parancs legfeljebb 5 000 rekordot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="e95e7-119">Searches that do not include an aggregation command return upto 5,000 records.</span></span>
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a><span data-ttu-id="e95e7-120">Egyszerű napló Analytics Search REST API-oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="e95e7-120">Basic Log Analytics Search REST API tutorial</span></span>
### <a name="to-use-armclient"></a><span data-ttu-id="e95e7-121">ARMClient használata</span><span class="sxs-lookup"><span data-stu-id="e95e7-121">To use ARMClient</span></span>
1. <span data-ttu-id="e95e7-122">Telepítés [Chocolatey](https://chocolatey.org/), amelyen egy megnyitott forrás Package Manager Windows.</span><span class="sxs-lookup"><span data-stu-id="e95e7-122">Install [Chocolatey](https://chocolatey.org/), which is an Open Source Package Manager for Windows.</span></span> <span data-ttu-id="e95e7-123">Nyisson meg egy parancssort rendszergazdaként, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e95e7-123">Open a command prompt window as administrator and then run the following command:</span></span>

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. <span data-ttu-id="e95e7-124">ARMClient telepítse a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e95e7-124">Install ARMClient by running the following command:</span></span>

    ```
    choco install armclient
    ```

### <a name="to-perform-a-search-using-armclient"></a><span data-ttu-id="e95e7-125">A keresések ARMClient végrehajtásához</span><span class="sxs-lookup"><span data-stu-id="e95e7-125">To perform a search using ARMClient</span></span>
1. <span data-ttu-id="e95e7-126">Jelentkezzen be Microsoft-fiókjával vagy a munkahelyi vagy iskolai fiók használatával:</span><span class="sxs-lookup"><span data-stu-id="e95e7-126">Log in using your Microsoft account or your work or school account:</span></span>

    ```
    armclient login
    ```

    <span data-ttu-id="e95e7-127">Sikeres bejelentkezés kötve az adott fiókhoz az összes előfizetés listája:</span><span class="sxs-lookup"><span data-stu-id="e95e7-127">A successful login lists all subscriptions tied to the given account:</span></span>

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. <span data-ttu-id="e95e7-128">Az Operations Management Suite-munkaterület érhető el:</span><span class="sxs-lookup"><span data-stu-id="e95e7-128">Get the Operations Management Suite workspaces:</span></span>

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    <span data-ttu-id="e95e7-129">Sikeres Get hívást volna kimeneti összes munkaterületek előfizetéshez társítva:</span><span class="sxs-lookup"><span data-stu-id="e95e7-129">A successful Get call would output all workspaces tied to the subscription:</span></span>

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
3. <span data-ttu-id="e95e7-130">A keresési változó létrehozása:</span><span class="sxs-lookup"><span data-stu-id="e95e7-130">Create your search variable:</span></span>

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. <span data-ttu-id="e95e7-131">A Keresés az új keresés változó használatával:</span><span class="sxs-lookup"><span data-stu-id="e95e7-131">Search using your new search variable:</span></span>

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a><span data-ttu-id="e95e7-132">Naplófájl Analytics Search REST API referencia példák</span><span class="sxs-lookup"><span data-stu-id="e95e7-132">Log Analytics Search REST API reference examples</span></span>
<span data-ttu-id="e95e7-133">A következő példák azt szemléltetik, hogyan használhatja a keresési API-t.</span><span class="sxs-lookup"><span data-stu-id="e95e7-133">The following examples show you how you can use the Search API.</span></span>

### <a name="search---actionread"></a><span data-ttu-id="e95e7-134">Keresési - művelet olvasása</span><span class="sxs-lookup"><span data-stu-id="e95e7-134">Search - Action/Read</span></span>
<span data-ttu-id="e95e7-135">**A minta URL-címe:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-135">**Sample Url:**</span></span>

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

<span data-ttu-id="e95e7-136">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-136">**Request:**</span></span>

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
<span data-ttu-id="e95e7-137">A következő táblázat ismerteti a rendelkezésre álló tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e95e7-137">The following table describes the properties that are available.</span></span>

| <span data-ttu-id="e95e7-138">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="e95e7-138">**Property**</span></span> | <span data-ttu-id="e95e7-139">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="e95e7-139">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="e95e7-140">Felső</span><span class="sxs-lookup"><span data-stu-id="e95e7-140">top</span></span> |<span data-ttu-id="e95e7-141">A visszaadandó eredmény maximális száma.</span><span class="sxs-lookup"><span data-stu-id="e95e7-141">The maximum number of results to return.</span></span> |
| <span data-ttu-id="e95e7-142">Jelöljön ki</span><span class="sxs-lookup"><span data-stu-id="e95e7-142">highlight</span></span> |<span data-ttu-id="e95e7-143">Megfelelő mezőkben konzolban használt általában előtti és utáni paramétereket tartalmaz</span><span class="sxs-lookup"><span data-stu-id="e95e7-143">Contains pre and post parameters, used usually for highlighting matching fields</span></span> |
| <span data-ttu-id="e95e7-144">előre</span><span class="sxs-lookup"><span data-stu-id="e95e7-144">pre</span></span> |<span data-ttu-id="e95e7-145">Előtagok az adott karakterlánc, a megfelelő mezőket.</span><span class="sxs-lookup"><span data-stu-id="e95e7-145">Prefixes the given string to your matched fields.</span></span> |
| <span data-ttu-id="e95e7-146">Post</span><span class="sxs-lookup"><span data-stu-id="e95e7-146">post</span></span> |<span data-ttu-id="e95e7-147">A megadott karakterlánc hozzáfűzi a megfelelő mezőket.</span><span class="sxs-lookup"><span data-stu-id="e95e7-147">Appends the given string to your matched fields.</span></span> |
| <span data-ttu-id="e95e7-148">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e95e7-148">query</span></span> |<span data-ttu-id="e95e7-149">A keresési lekérdezést, és adja vissza az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="e95e7-149">The search query used to collect and return results.</span></span> |
| <span data-ttu-id="e95e7-150">start</span><span class="sxs-lookup"><span data-stu-id="e95e7-150">start</span></span> |<span data-ttu-id="e95e7-151">A kívánt eredmény található időszak kezdete.</span><span class="sxs-lookup"><span data-stu-id="e95e7-151">The beginning of the time window you want results to be found from.</span></span> |
| <span data-ttu-id="e95e7-152">Vége</span><span class="sxs-lookup"><span data-stu-id="e95e7-152">end</span></span> |<span data-ttu-id="e95e7-153">A kívánt eredmény található időszak végén.</span><span class="sxs-lookup"><span data-stu-id="e95e7-153">The end of the time window you want results to be found from.</span></span> |

<span data-ttu-id="e95e7-154">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-154">**Response:**</span></span>

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

### <a name="searchid---actionread"></a><span data-ttu-id="e95e7-155">Keresés / {azonosító} - művelet olvasása</span><span class="sxs-lookup"><span data-stu-id="e95e7-155">Search/{ID} - Action/Read</span></span>
<span data-ttu-id="e95e7-156">**A kérelem egy mentett keresés tartalma:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-156">**Request the contents of a Saved Search:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> <span data-ttu-id="e95e7-157">A search függvény "Folyamatban" állapotú, majd a frissített eredményeit lekérdezési végezhető el ez az API.</span><span class="sxs-lookup"><span data-stu-id="e95e7-157">If the search returns with a ‘Pending’ status, then polling the updated results can be done through this API.</span></span> <span data-ttu-id="e95e7-158">6 perc után a a Keresés eredménye így a rendszer eldobja a gyorsítótárból, és HTTP állapotba adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e95e7-158">After 6 min, the result of the search will be dropped from the cache and HTTP Gone will be returned.</span></span> <span data-ttu-id="e95e7-159">A kezdeti keresési kérelem azonnal visszaadja a "Sikeres" állapotba, ha az eredmények nem kerülnek a gyorsítótárba, amely az API vissza HTTP állapotba, ha a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="e95e7-159">If the initial search request returns a ‘Successful’ status immediately, the results are not added to the cache causing this API to return HTTP Gone if queried.</span></span> <span data-ttu-id="e95e7-160">Egy 200-as HTTP-eredmény tartalma ugyanabban a formában, a kezdeti keresési kérelem csak a frissített értékekkel.</span><span class="sxs-lookup"><span data-stu-id="e95e7-160">The contents of an HTTP 200 result are in the same format as the initial search request just with updated values.</span></span>
>
>

### <a name="saved-searches"></a><span data-ttu-id="e95e7-161">Mentett keresések</span><span class="sxs-lookup"><span data-stu-id="e95e7-161">Saved searches</span></span>
<span data-ttu-id="e95e7-162">**A kérelem mentett keresések listája:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-162">**Request List of Saved Searches:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

<span data-ttu-id="e95e7-163">Támogatott módszerek: GET PUT törlése</span><span class="sxs-lookup"><span data-stu-id="e95e7-163">Supported methods: GET PUT DELETE</span></span>

<span data-ttu-id="e95e7-164">Gyűjtemény módszereket támogatja: beolvasása</span><span class="sxs-lookup"><span data-stu-id="e95e7-164">Supported collection methods: GET</span></span>

<span data-ttu-id="e95e7-165">A következő táblázat ismerteti a rendelkezésre álló tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e95e7-165">The following table describes the properties that are available.</span></span>

| <span data-ttu-id="e95e7-166">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e95e7-166">Property</span></span> | <span data-ttu-id="e95e7-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="e95e7-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e95e7-168">Azonosító</span><span class="sxs-lookup"><span data-stu-id="e95e7-168">Id</span></span> |<span data-ttu-id="e95e7-169">Az egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e95e7-169">The unique identifier.</span></span> |
| <span data-ttu-id="e95e7-170">ETag</span><span class="sxs-lookup"><span data-stu-id="e95e7-170">Etag</span></span> |<span data-ttu-id="e95e7-171">**Szükséges javítás**.</span><span class="sxs-lookup"><span data-stu-id="e95e7-171">**Required for Patch**.</span></span> <span data-ttu-id="e95e7-172">Minden egyes kiszolgáló frissítve.</span><span class="sxs-lookup"><span data-stu-id="e95e7-172">Updated by server on each write.</span></span> <span data-ttu-id="e95e7-173">Értéket meg kell egyeznie az aktuális tárolt érték vagy "*" frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e95e7-173">Value must be equal to the current stored value or ‘*’ to update.</span></span> <span data-ttu-id="e95e7-174">409 régi vagy érvénytelen értéket adott vissza.</span><span class="sxs-lookup"><span data-stu-id="e95e7-174">409 returned for old/invalid values.</span></span> |
| <span data-ttu-id="e95e7-175">Properties.Query</span><span class="sxs-lookup"><span data-stu-id="e95e7-175">properties.query</span></span> |<span data-ttu-id="e95e7-176">**Szükséges**.</span><span class="sxs-lookup"><span data-stu-id="e95e7-176">**Required**.</span></span> <span data-ttu-id="e95e7-177">A keresési lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="e95e7-177">The search query.</span></span> |
| <span data-ttu-id="e95e7-178">properties.displayName</span><span class="sxs-lookup"><span data-stu-id="e95e7-178">properties.displayName</span></span> |<span data-ttu-id="e95e7-179">**Szükséges**.</span><span class="sxs-lookup"><span data-stu-id="e95e7-179">**Required**.</span></span> <span data-ttu-id="e95e7-180">A lekérdezés felhasználói megjelenítendő nevét.</span><span class="sxs-lookup"><span data-stu-id="e95e7-180">The user-defined display name of the query.</span></span> |
| <span data-ttu-id="e95e7-181">Properties.Category</span><span class="sxs-lookup"><span data-stu-id="e95e7-181">properties.category</span></span> |<span data-ttu-id="e95e7-182">**Szükséges**.</span><span class="sxs-lookup"><span data-stu-id="e95e7-182">**Required**.</span></span> <span data-ttu-id="e95e7-183">A lekérdezés felhasználói kategóriát.</span><span class="sxs-lookup"><span data-stu-id="e95e7-183">The user-defined category of the query.</span></span> |

> [!NOTE]
> <span data-ttu-id="e95e7-184">A Naplóelemzési keresési API jelenleg adja vissza a felhasználó által létrehozott mentett keresések során kérdezi le azt a mentett kereséseket a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="e95e7-184">The Log Analytics search API currently returns user-created saved searches when polled for saved searches in a workspace.</span></span> <span data-ttu-id="e95e7-185">Az API nem ad vissza a mentett keresések megoldások által biztosított.</span><span class="sxs-lookup"><span data-stu-id="e95e7-185">The API does not return saved searches provided by solutions.</span></span>
>
>

### <a name="create-saved-searches"></a><span data-ttu-id="e95e7-186">Mentett keresések létrehozása</span><span class="sxs-lookup"><span data-stu-id="e95e7-186">Create saved searches</span></span>
<span data-ttu-id="e95e7-187">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-187">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> <span data-ttu-id="e95e7-188">A nevet minden mentett keresések, ütemezéseihez és napló Analytics API-val létrehozott kisbetűs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e95e7-188">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

### <a name="delete-saved-searches"></a><span data-ttu-id="e95e7-189">Törölje a mentett kereséseket</span><span class="sxs-lookup"><span data-stu-id="e95e7-189">Delete saved searches</span></span>
<span data-ttu-id="e95e7-190">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-190">**Request:**</span></span>

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a><span data-ttu-id="e95e7-191">Mentett keresések frissítése</span><span class="sxs-lookup"><span data-stu-id="e95e7-191">Update saved searches</span></span>
 <span data-ttu-id="e95e7-192">**A kérelem:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-192">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a><span data-ttu-id="e95e7-193">Metaadat - JSON csak</span><span class="sxs-lookup"><span data-stu-id="e95e7-193">Metadata - JSON only</span></span>
<span data-ttu-id="e95e7-194">Itt módja a mezőket az adatok gyűjtése a munkaterületen az összes napló megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e95e7-194">Here’s a way to see the fields for all log types for the data collected in your workspace.</span></span> <span data-ttu-id="e95e7-195">Például ha szeretné tudni, hogy ha az esemény típusa van-e a számítógép neve mező, majd a lekérdezés módja egy ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="e95e7-195">For example, if you want you know if the Event type has a field named Computer, then this query is one way to check.</span></span>

<span data-ttu-id="e95e7-196">**Kérelem mezők:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-196">**Request for Fields:**</span></span>

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

<span data-ttu-id="e95e7-197">**Válasz:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-197">**Response:**</span></span>

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

<span data-ttu-id="e95e7-198">A következő táblázat ismerteti a rendelkezésre álló tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e95e7-198">The following table describes the properties that are available.</span></span>

| <span data-ttu-id="e95e7-199">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="e95e7-199">**Property**</span></span> | <span data-ttu-id="e95e7-200">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="e95e7-200">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="e95e7-201">név</span><span class="sxs-lookup"><span data-stu-id="e95e7-201">name</span></span> |<span data-ttu-id="e95e7-202">Mező neve.</span><span class="sxs-lookup"><span data-stu-id="e95e7-202">Field name.</span></span> |
| <span data-ttu-id="e95e7-203">displayName</span><span class="sxs-lookup"><span data-stu-id="e95e7-203">displayName</span></span> |<span data-ttu-id="e95e7-204">A mezőben megjelenítendő nevét.</span><span class="sxs-lookup"><span data-stu-id="e95e7-204">The display name of the field.</span></span> |
| <span data-ttu-id="e95e7-205">type</span><span class="sxs-lookup"><span data-stu-id="e95e7-205">type</span></span> |<span data-ttu-id="e95e7-206">A mező értékének típusa.</span><span class="sxs-lookup"><span data-stu-id="e95e7-206">The Type of the field value.</span></span> |
| <span data-ttu-id="e95e7-207">kategorizálható</span><span class="sxs-lookup"><span data-stu-id="e95e7-207">facetable</span></span> |<span data-ttu-id="e95e7-208">"Indexed", aktuális kombinációja "tárolt" és "dimenzió" tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="e95e7-208">Combination of current ‘indexed’, ‘stored ‘and ‘facet’ properties.</span></span> |
| <span data-ttu-id="e95e7-209">Megjelenítése</span><span class="sxs-lookup"><span data-stu-id="e95e7-209">display</span></span> |<span data-ttu-id="e95e7-210">Aktuális "üzenet megjelenítése" tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e95e7-210">Current ‘display’ property.</span></span> <span data-ttu-id="e95e7-211">Értéke TRUE, ha mező a keresési látható.</span><span class="sxs-lookup"><span data-stu-id="e95e7-211">True if field is visible in search.</span></span> |
| <span data-ttu-id="e95e7-212">ownerType</span><span class="sxs-lookup"><span data-stu-id="e95e7-212">ownerType</span></span> |<span data-ttu-id="e95e7-213">Csak a előkészítve IP-címekhez tartozó típusok csökken.</span><span class="sxs-lookup"><span data-stu-id="e95e7-213">Reduced to only Types belonging to onboarded IP’s.</span></span> |

## <a name="optional-parameters"></a><span data-ttu-id="e95e7-214">Nem kötelező paraméterek</span><span class="sxs-lookup"><span data-stu-id="e95e7-214">Optional parameters</span></span>
<span data-ttu-id="e95e7-215">A következő rendelkezésre álló választható paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e95e7-215">The following information describes optional parameters available.</span></span>

### <a name="highlighting"></a><span data-ttu-id="e95e7-216">Kiemelve</span><span class="sxs-lookup"><span data-stu-id="e95e7-216">Highlighting</span></span>
<span data-ttu-id="e95e7-217">A "Kiemelt" paraméter egy nem kötelező paraméter, használhatja a keresés alrendszer kéréséhez tartalmaznak jelölők válaszában.</span><span class="sxs-lookup"><span data-stu-id="e95e7-217">The “Highlight” parameter is an optional parameter you may use to request the search subsystem include a set of markers in its response.</span></span>

<span data-ttu-id="e95e7-218">Ezek a jelölők jelzik kezdetét, és a záró kijelölt szöveg, amely megfelel a keresési lekérdezés megadott feltételek.</span><span class="sxs-lookup"><span data-stu-id="e95e7-218">These markers indicate the start and end highlighted text that matches the terms provided in your search query.</span></span>
<span data-ttu-id="e95e7-219">Előfordulhat, hogy adja meg a kezdő és záró jelölők kiemelt kifejezés csomagolásához keresés által használt.</span><span class="sxs-lookup"><span data-stu-id="e95e7-219">You may specify the start and end markers that are used by search to wrap the highlighted term.</span></span>

<span data-ttu-id="e95e7-220">**Példa keresési lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="e95e7-220">**Example search query**</span></span>

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

<span data-ttu-id="e95e7-221">**A minta eredmény:**</span><span class="sxs-lookup"><span data-stu-id="e95e7-221">**Sample result:**</span></span>

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

<span data-ttu-id="e95e7-222">Figyelje meg, hogy az előző eredmény tartalmazza-e olyan hibaüzenetet, amely a következő előtaggal, és hozzáfűzi.</span><span class="sxs-lookup"><span data-stu-id="e95e7-222">Notice that the preceding result contains an error message that has been prefixed and appended.</span></span>

## <a name="computer-groups"></a><span data-ttu-id="e95e7-223">Számítógépcsoportok</span><span class="sxs-lookup"><span data-stu-id="e95e7-223">Computer groups</span></span>
<span data-ttu-id="e95e7-224">Számítógépcsoportok különleges mentett keresések, amely a számítógépek készlettel tért vissza.</span><span class="sxs-lookup"><span data-stu-id="e95e7-224">Computer groups are special saved searches that return a set of computers.</span></span>  <span data-ttu-id="e95e7-225">A számítógép (csoport) további lekérdezések segítségével az eredményeket a csoport számítógépeinek korlátozza.</span><span class="sxs-lookup"><span data-stu-id="e95e7-225">You can use a computer group in other queries to limit the results to the computers in the group.</span></span>  <span data-ttu-id="e95e7-226">A számítógép (csoport), a mentett kereséseket értékkel rendelkező számítógép csoport címkével ellátott lett megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="e95e7-226">A computer group is implemented as a saved search with a Group tag with a value of Computer.</span></span>

<span data-ttu-id="e95e7-227">Az alábbiakban látható egy számítógépcsoport mintát választ.</span><span class="sxs-lookup"><span data-stu-id="e95e7-227">Following is a sample response for a computer group.</span></span>

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

### <a name="retrieving-computer-groups"></a><span data-ttu-id="e95e7-228">Számítógépcsoportok beolvasása</span><span class="sxs-lookup"><span data-stu-id="e95e7-228">Retrieving computer groups</span></span>
<span data-ttu-id="e95e7-229">Egy számítógépcsoport lekéréséhez használja a Get metódust csoport azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="e95e7-229">To retrieve a computer group, use the Get method with the group ID.</span></span>

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a><span data-ttu-id="e95e7-230">Létrehozásakor vagy frissítésekor. a számítógép (csoport)</span><span class="sxs-lookup"><span data-stu-id="e95e7-230">Creating or updating a computer group</span></span>
<span data-ttu-id="e95e7-231">Hozzon létre egy számítógépcsoportot, használja a Put metódust a mentett keresés egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="e95e7-231">To create a computer group, use the Put method with a unique saved search ID.</span></span> <span data-ttu-id="e95e7-232">Használjon egy meglévő számítógép-csoport azonosítója, ha egy módosul.</span><span class="sxs-lookup"><span data-stu-id="e95e7-232">If you use an existing computer group ID, then that one is modified.</span></span> <span data-ttu-id="e95e7-233">Ha egy számítógép (csoport) hoz létre a Log Analytics-portálról, majd az azonosító a csoportból, a név jön létre.</span><span class="sxs-lookup"><span data-stu-id="e95e7-233">When you create a computer group in the Log Analytics portal, then the ID is created from the group and name.</span></span>

<span data-ttu-id="e95e7-234">A lekérdezés a meghatározása használt számítógépek csoportjára működnek majd megfelelően kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="e95e7-234">The query used for the group definition must return a set of computers for the group to function properly.</span></span>  <span data-ttu-id="e95e7-235">Javasoljuk, hogy a lekérdezés leállítása `| Distinct Computer` annak érdekében, hogy a megfelelő adat.</span><span class="sxs-lookup"><span data-stu-id="e95e7-235">It's recommended that you end your query with `| Distinct Computer` to ensure the correct data is returned.</span></span>

<span data-ttu-id="e95e7-236">A mentett keresés definition tartalmaznia kell egy címke nevű csoport a számítógép a következő keresésre számítógépcsoportként besorolását értékű.</span><span class="sxs-lookup"><span data-stu-id="e95e7-236">The definition of the saved search must include a tag named Group with a value of Computer for the search to be classified as a computer group.</span></span>

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a><span data-ttu-id="e95e7-237">Számítógép-csoportok törlése</span><span class="sxs-lookup"><span data-stu-id="e95e7-237">Deleting computer groups</span></span>
<span data-ttu-id="e95e7-238">Olyan számítógép-csoport törléséhez használja a Delete metódus csoport azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="e95e7-238">To delete a computer group, use the Delete method with the group ID.</span></span>

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a><span data-ttu-id="e95e7-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e95e7-239">Next steps</span></span>
* <span data-ttu-id="e95e7-240">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) egyéni mezőkkel feltétel lekérdezések összeállításához.</span><span class="sxs-lookup"><span data-stu-id="e95e7-240">Learn about [log searches](log-analytics-log-searches.md) to build queries using custom fields for criteria.</span></span>
