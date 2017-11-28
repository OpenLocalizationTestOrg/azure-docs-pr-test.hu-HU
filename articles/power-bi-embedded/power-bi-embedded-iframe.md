---
title: "a Power BI Embedded többi aaaHow toouse |} Microsoft Docs"
description: "Ismerje meg, hogy a Power BI Embedded többi toouse "
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 8bcef780-cca0-4f30-9a9b-9daa1a7ae865
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
ms.openlocfilehash: 98057724e60ba868f9c93de8c50383569eb8852d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-power-bi-embedded-with-rest"></a><span data-ttu-id="15b9f-103">Hogyan Power BI Embedded többi toouse</span><span class="sxs-lookup"><span data-stu-id="15b9f-103">How toouse Power BI Embedded with REST</span></span>

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a><span data-ttu-id="15b9f-104">A Power BI Embedded: Mi az, és mit</span><span class="sxs-lookup"><span data-stu-id="15b9f-104">Power BI Embedded: What it is and what it's for</span></span>

<span data-ttu-id="15b9f-105">A Power BI Embedded áttekintését hello hivatalos ismertetett [Power BI Embedded hely](https://azure.microsoft.com/services/power-bi-embedded/), de Ismerkedjen meg az gyors, mielőtt azt bejutni hello adatait használja azt a többi.</span><span class="sxs-lookup"><span data-stu-id="15b9f-105">An overview of Power BI Embedded is described in hello official [Power BI Embedded site](https://azure.microsoft.com/services/power-bi-embedded/), but let's take a quick look before we get into hello details about using it with REST.</span></span>

<span data-ttu-id="15b9f-106">A használata rendkívül egyszerű, valóban.</span><span class="sxs-lookup"><span data-stu-id="15b9f-106">It's quite simple, really.</span></span> <span data-ttu-id="15b9f-107">érdemes lehet toouse hello dinamikus adatok vizuális [Power BI](https://powerbi.microsoft.com) a saját alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="15b9f-107">you may want toouse hello dynamic data visualizations of [Power BI](https://powerbi.microsoft.com) in your own application.</span></span>

<span data-ttu-id="15b9f-108">A legtöbb egyéni alkalmazások számára, a saját, nem feltétlenül a saját szervezetében lévő felhasználók toodeliver hello adatokra van szükségük.</span><span class="sxs-lookup"><span data-stu-id="15b9f-108">Most custom applications need toodeliver hello data for their own customers, not necessarily users in their own organization.</span></span> <span data-ttu-id="15b9f-109">Például ha a vállalat és a B vállalat néhány szolgáltatás rendelkezik, A vállalatnál lévő felhasználóknál kell csak adatait jeleníti meg a saját vállalati azonosítójához. Ez azt jelenti, hogy több-bérlős hello hello kézbesítési van szükség.</span><span class="sxs-lookup"><span data-stu-id="15b9f-109">For example, if you're delivering some service for both company A and company B, users in company A should only see data for their own company A. That is, hello multi-tenancy is needed for hello delivery.</span></span>

<span data-ttu-id="15b9f-110">hello egyéni alkalmazás is lehet, hogy ajánlat saját hitelesítési módszerek, például űrlapalapú hitelesítés, alapszintű hitelesítés, stb.</span><span class="sxs-lookup"><span data-stu-id="15b9f-110">hello custom application might also be offering its own authentication methods such as forms auth, basic auth, etc..</span></span> <span data-ttu-id="15b9f-111">Ezt követően megoldás beágyazás hello kell vállalatokkal működnek együtt a meglévő hitelesítési módszerek biztonságosan.</span><span class="sxs-lookup"><span data-stu-id="15b9f-111">Then, hello embedding solution must collaborate with this existing authentication methods safely.</span></span> <span data-ttu-id="15b9f-112">A felhasználók toobe képes toouse szükséges nélkül ISV alkalmazásokon hello extra beszerzési vagy a Power BI előfizetés licencelésének.</span><span class="sxs-lookup"><span data-stu-id="15b9f-112">It's also necessary for users toobe able toouse those ISV applications without hello extra purchase or licensing of a Power BI subscription.</span></span>

 <span data-ttu-id="15b9f-113">**A Power BI Embedded** pontosan ilyen típusú forgatókönyvek tervezték.</span><span class="sxs-lookup"><span data-stu-id="15b9f-113">**Power BI Embedded** is designed for precisely these kinds of scenarios.</span></span> <span data-ttu-id="15b9f-114">Igen most, hogy a gyors bevezetés hello útból, folytassuk az egyes részletei</span><span class="sxs-lookup"><span data-stu-id="15b9f-114">So, now that we have that quick introduction out of hello way, let's get into some details</span></span>

<span data-ttu-id="15b9f-115">Használhatja a hello .NET \(C#) vagy a Node.js SDK, tooeasily építenie az alkalmazást a Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="15b9f-115">You can use hello .NET \(C#) or Node.js SDK, tooeasily build your application with Power BI Embedded.</span></span> <span data-ttu-id="15b9f-116">Azonban ez a cikk azt ismertetjük, HTTP-folyamat kapcsolatos \(AuthN együtt) a Power BI SDK-k nélkül.</span><span class="sxs-lookup"><span data-stu-id="15b9f-116">But, in this article, we'll explain about HTTP flow \(incl. AuthN) of Power BI without SDKs.</span></span> <span data-ttu-id="15b9f-117">Ez a folyamat ismertetése, az alkalmazás hozhat létre **bármely programozási nyelv**, és értelmezni lehet mélyen Power BI Embedded hello lényege.</span><span class="sxs-lookup"><span data-stu-id="15b9f-117">Understanding this flow, you can build your application **with any programming language**, and you can understand deeply hello essence of Power BI Embedded.</span></span>

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a><span data-ttu-id="15b9f-118">Munkaterület-csoport létrehozása a Power bi-ban, és a get elérési kulcsot \(kiépítés)</span><span class="sxs-lookup"><span data-stu-id="15b9f-118">Create Power BI workspace collection, and get access key \(Provisioning)</span></span>

<span data-ttu-id="15b9f-119">A Power BI Embedded egyike hello Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="15b9f-119">Power BI Embedded is one of hello Azure services.</span></span> <span data-ttu-id="15b9f-120">Csak hello Azure Portal használó ISV van szó, a használati díjak \(óránkénti felhasználói munkamenetenként), és a nézetek hello jelentés nem megterhelni vagy akár hello felhasználó Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="15b9f-120">Only hello ISV who uses Azure Portal is charged for usage fees \(per hourly user session), and hello user who views hello report isn't charged or even require an Azure subscription.</span></span>
<span data-ttu-id="15b9f-121">Az alkalmazásfejlesztés megkezdése előtt létre kell hoznia azt hello **Power BI-munkaterületcsoport** Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="15b9f-121">Before starting our application development, we must create hello **Power BI workspace collection** by using Azure Portal.</span></span>

<span data-ttu-id="15b9f-122">A Power BI Embedded egyes munkaterületeken hello munkaterület minden ügyfél (bérlői), és sok munkaterületek azt adhat hozzá minden egyes munkaterület-csoportot.</span><span class="sxs-lookup"><span data-stu-id="15b9f-122">Each workspace of Power BI Embedded is hello workspace for each customer (tenant), and we can add many workspaces in each workspace collection.</span></span> <span data-ttu-id="15b9f-123">hello azonos elérési kulcsot minden munkaterület-csoport szerepel.</span><span class="sxs-lookup"><span data-stu-id="15b9f-123">hello same access key is used in each workspace collection.</span></span> <span data-ttu-id="15b9f-124">A hatás, hello munkaterület-csoport a Power BI Embedded hello biztonsági határokat.</span><span class="sxs-lookup"><span data-stu-id="15b9f-124">In-effect, hello workspace collection is hello security boundary for Power BI Embedded.</span></span>

![](media/power-bi-embedded-iframe/create-workspace.png)

<span data-ttu-id="15b9f-125">Hello munkaterület-csoport létrehozása befejeződik, ha hello hozzáférési kulcs másolása az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="15b9f-125">When we finish creating hello workspace collection, copy hello access key from Azure Portal.</span></span>

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> <span data-ttu-id="15b9f-126">Azt is kiépítése hello munkaterület-csoportot, és a REST API-n keresztül hozzáférést kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="15b9f-126">We can also provision hello workspace collection and get access key via REST API.</span></span> <span data-ttu-id="15b9f-127">több, lásd: toolearn [Power BI erőforrás szolgáltató API-k](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="15b9f-127">toolearn more, see [Power BI Resource Provider APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span></span>

## <a name="create-pbix-file-with-power-bi-desktop"></a><span data-ttu-id="15b9f-128">A Power BI Desktop .pbix-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="15b9f-128">Create .pbix file with Power BI Desktop</span></span>

<span data-ttu-id="15b9f-129">Ezután azt kell létrehoznia hello adatkapcsolat és a beágyazott jelentések toobe.</span><span class="sxs-lookup"><span data-stu-id="15b9f-129">Next, we must create hello data connection and reports toobe embedded.</span></span>
<span data-ttu-id="15b9f-130">Ez a feladat nincs programozási vagy kódot.</span><span class="sxs-lookup"><span data-stu-id="15b9f-130">For this task, there’s no programming or code.</span></span> <span data-ttu-id="15b9f-131">A Microsoft Power BI Desktop használja.</span><span class="sxs-lookup"><span data-stu-id="15b9f-131">We just use Power BI Desktop.</span></span>
<span data-ttu-id="15b9f-132">Ez a cikk azt nem kerül a hello részletes tájékoztatást a Power BI Desktop toouse.</span><span class="sxs-lookup"><span data-stu-id="15b9f-132">In this article, we won't go through hello details about how toouse Power BI Desktop.</span></span> <span data-ttu-id="15b9f-133">Néhány segítséget itt, lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="15b9f-133">If you need some help here, see [Getting started with Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span></span> <span data-ttu-id="15b9f-134">A példa kedvéért most használjuk hello [kiskereskedelmi elemzési minta](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span><span class="sxs-lookup"><span data-stu-id="15b9f-134">For our example, we'll just use hello [Retail Analysis Sample](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span></span>

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a><span data-ttu-id="15b9f-135">A Power BI-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="15b9f-135">Create a Power BI workspace</span></span>

<span data-ttu-id="15b9f-136">Most, hogy hello kiépítés összes történik, lássunk neki egy ügyfél-munkaterület létrehozása a munkaterület-csoportok hello REST API-kon keresztül.</span><span class="sxs-lookup"><span data-stu-id="15b9f-136">Now that hello provisioning is all done, let’s get started creating a customer’s workspace in hello workspace collection via REST APIs.</span></span> <span data-ttu-id="15b9f-137">hello alábbi HTTP POST kérelem (REST) van hello új munkaterület létrehozása a meglévő munkaterület gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="15b9f-137">hello following HTTP POST Request (REST) is creating hello new workspace in our existing workspace collection.</span></span> <span data-ttu-id="15b9f-138">Ez a hello [POST munkaterület API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="15b9f-138">This is hello [POST Workspace API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span> <span data-ttu-id="15b9f-139">A példa kedvéért hello munkaterület gyűjtemény neve: **mypbiapp**.</span><span class="sxs-lookup"><span data-stu-id="15b9f-139">In our example, hello workspace collection name is **mypbiapp**.</span></span> <span data-ttu-id="15b9f-140">Azt az imént beállított hello elérési kulcsot, amely azt korábban másolta, mint a **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="15b9f-140">We just set hello access key, which we previously copied, as **AppKey**.</span></span> <span data-ttu-id="15b9f-141">Nagyon egyszerű hitelesítést is!</span><span class="sxs-lookup"><span data-stu-id="15b9f-141">It’s very simple authentication!</span></span>

<span data-ttu-id="15b9f-142">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="15b9f-142">**HTTP Request**</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="15b9f-143">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="15b9f-143">**HTTP Response**</span></span>

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

<span data-ttu-id="15b9f-144">visszaadott hello **workspaceId** hello a következő további API-hívásokban használt.</span><span class="sxs-lookup"><span data-stu-id="15b9f-144">hello returned **workspaceId** is used for hello following subsequent API calls.</span></span> <span data-ttu-id="15b9f-145">Az alkalmazás őrizze meg ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="15b9f-145">Our application must retain this value.</span></span>

## <a name="import-pbix-file-into-hello-workspace"></a><span data-ttu-id="15b9f-146">A .pbix fájlok importálása hello munkaterület</span><span class="sxs-lookup"><span data-stu-id="15b9f-146">Import .pbix file into hello workspace</span></span>

<span data-ttu-id="15b9f-147">Az egyes jelentések a munkaterületen felel meg egyetlen Power BI Desktop fájl tooa dataset \(datasource beállításokat is beleértve).</span><span class="sxs-lookup"><span data-stu-id="15b9f-147">Each report in a workspace corresponds tooa single Power BI Desktop file with a dataset \(including datasource settings).</span></span> <span data-ttu-id="15b9f-148">A .pbix fájl toohello munkaterület, ahogy az alábbi kód hello importálhatja azt.</span><span class="sxs-lookup"><span data-stu-id="15b9f-148">We can import our .pbix file toohello workspace as shown in hello code below.</span></span> <span data-ttu-id="15b9f-149">Ahogy látja, azt feltöltheti hello bináris .pbix fájl MIME multipart http használatával.</span><span class="sxs-lookup"><span data-stu-id="15b9f-149">As you can see, we can upload hello binary of .pbix file using MIME multipart in http.</span></span>

<span data-ttu-id="15b9f-150">hello uri-töredéket **32960a09-6366-4208-a8bb-9e0678cdbb9d** hello workspaceId, és a lekérdezési paraméter **datasetDisplayName** hello dataset neve toocreate van.</span><span class="sxs-lookup"><span data-stu-id="15b9f-150">hello uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** is hello workspaceId, and query parameter **datasetDisplayName** is hello dataset name toocreate.</span></span> <span data-ttu-id="15b9f-151">hello létrehozott adatkészlet alkalmazáshoz kapcsolódó összes adatot tartalmazza az importált adatok, például a .pbix fájlok összetevők hello mutató toohello adatforrás stb...</span><span class="sxs-lookup"><span data-stu-id="15b9f-151">hello created dataset holds all data related artifacts in .pbix file such as imported data, hello pointer toohello data source, etc..</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{hello content (binary) of .pbix file}
--A300testx--
```

<span data-ttu-id="15b9f-152">A importálási feladat lehet, hogy futtatni egy ideig.</span><span class="sxs-lookup"><span data-stu-id="15b9f-152">This import task might run for a while.</span></span> <span data-ttu-id="15b9f-153">Amikor végzett, az alkalmazás importálása azonosítójával hello feladatállapot teheti fel. A jelen példában hello importálási azonosító: **4eec64dd-533b-47c3-a72c-6508ad854659**.</span><span class="sxs-lookup"><span data-stu-id="15b9f-153">When complete, our application can ask hello task status using import id. In our example, hello import id is **4eec64dd-533b-47c3-a72c-6508ad854659**.</span></span>

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

<span data-ttu-id="15b9f-154">hello következő kért állapota az importálás azonosítójával:</span><span class="sxs-lookup"><span data-stu-id="15b9f-154">hello following is asking status using this import id:</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="15b9f-155">Ha hello feladat nem teljes, HTTP-válasz hello ehhez hasonló lehet:</span><span class="sxs-lookup"><span data-stu-id="15b9f-155">If hello task isn't complete, hello HTTP response could be like this:</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

<span data-ttu-id="15b9f-156">Hello feladat befejeződött, ha HTTP-válasz hello további ehhez hasonló lehet:</span><span class="sxs-lookup"><span data-stu-id="15b9f-156">If hello task is complete, hello HTTP response could be more like this:</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a><span data-ttu-id="15b9f-157">Az adatforrás-kapcsolat \(és az adatok több-bérlős)</span><span class="sxs-lookup"><span data-stu-id="15b9f-157">Data source connectivity \(and multi-tenancy of data)</span></span>

<span data-ttu-id="15b9f-158">A munkaterületre importált, szinte teljes egészében a .pbix fájlok hello összetevők hello adatforrások hitelesítő adatait nem is.</span><span class="sxs-lookup"><span data-stu-id="15b9f-158">While almost all of hello artifacts in .pbix file are imported into our workspace, hello  credentials for data sources are not.</span></span> <span data-ttu-id="15b9f-159">Ennek eredményeképpen használatakor **DirectQuery módban**, hello beágyazott jelentés nem jeleníthető meg helyesen.</span><span class="sxs-lookup"><span data-stu-id="15b9f-159">As a result, when using **DirectQuery mode**, hello embedded report cannot be shown correctly.</span></span> <span data-ttu-id="15b9f-160">De használata esetén **importálás módra**, azt megtekintheti hello jelentést hello meglévő importált adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="15b9f-160">But, when using **Import mode**, we can view hello report using hello existing imported data.</span></span> <span data-ttu-id="15b9f-161">Ebben az esetben a Microsoft hello hitelesítő adatot használja az alábbi lépések segítségével REST-hívások hello kell beállítania.</span><span class="sxs-lookup"><span data-stu-id="15b9f-161">In such a case, we must set hello credential using hello following steps via REST calls.</span></span>

<span data-ttu-id="15b9f-162">Először azt kell szereznie hello gateway datasource.</span><span class="sxs-lookup"><span data-stu-id="15b9f-162">First, we must get hello gateway datasource.</span></span> <span data-ttu-id="15b9f-163">Tudjuk, hello dataset **azonosító** hello korábban visszaadott azonosítója.</span><span class="sxs-lookup"><span data-stu-id="15b9f-163">We know hello dataset **id** is hello previously returned id.</span></span>

<span data-ttu-id="15b9f-164">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="15b9f-164">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="15b9f-165">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="15b9f-165">**HTTP Response**</span></span>

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

<span data-ttu-id="15b9f-166">Átjáró és adatforrás-azonosító segítségével hello visszaadott \(tekintse meg az előző hello **gatewayId** és **azonosító** eredményt adott vissza a hello), az alábbiak szerint módosíthatja azt hello Ez az adatforrás hitelesítő adatai:</span><span class="sxs-lookup"><span data-stu-id="15b9f-166">Using hello returned gateway id and datasource id \(see hello previous **gatewayId** and **id** in hello returned result), we can change hello credential of this datasource as follows:</span></span>

<span data-ttu-id="15b9f-167">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="15b9f-167">**HTTP Request**</span></span>

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

<span data-ttu-id="15b9f-168">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="15b9f-168">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

<span data-ttu-id="15b9f-169">Éles azt is beállíthatja az egyes munkaterületeken REST API használatával hello különböző kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="15b9f-169">In production, we can also set hello different connection string for each workspace using REST API.</span></span> <span data-ttu-id="15b9f-170">\(Egytényezős, azt is külön hello adatbázis egyes ügyfelek esetén.)</span><span class="sxs-lookup"><span data-stu-id="15b9f-170">\(i.e, we can separate hello database for each customers.)</span></span>

<span data-ttu-id="15b9f-171">hello következő hello kapcsolati karakterlánca datasource REST-en keresztül módosítja.</span><span class="sxs-lookup"><span data-stu-id="15b9f-171">hello following is changing hello connection string of datasource via REST.</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

<span data-ttu-id="15b9f-172">Vagy a Power BI Embedded is használhatók. a sorszintű biztonság, és azt is külön hello minden felhasználója egy jelentést.</span><span class="sxs-lookup"><span data-stu-id="15b9f-172">Or, we can use Row Level Security in Power BI Embedded and we can separate hello data for each users in one report.</span></span> <span data-ttu-id="15b9f-173">As a Result, azt hozhat létre minden ügyfél azonos .pbix jelentés \(felhasználói felületén, stb.) és a különböző adatforrásokból.</span><span class="sxs-lookup"><span data-stu-id="15b9f-173">As a result, we can provision each customer report with same .pbix \(UI, etc.) and different data sources.</span></span>

> [!NOTE]
> <span data-ttu-id="15b9f-174">Használata **importálás módra** helyett **DirectQuery módban**, nincs módja toorefresh modellek API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="15b9f-174">If you’re using **Import mode** instead of **DirectQuery mode**, there’s no way toorefresh models via API.</span></span> <span data-ttu-id="15b9f-175">És a helyszíni adatforrások keresztül Power BI-átjáró a Power BI Embedded még nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="15b9f-175">And, on-premises datasources through Power BI gateway isn't yet supported in Power BI Embedded.</span></span> <span data-ttu-id="15b9f-176">Azonban valóban érdemes tookeep követheti a hello [Power BI blog](https://powerbi.microsoft.com/blog/) újdonságok, és mi várható későbbi kiadásai.</span><span class="sxs-lookup"><span data-stu-id="15b9f-176">However, you'll really want tookeep an eye on hello [Power BI blog](https://powerbi.microsoft.com/blog/) for what's new and what's coming in future releases.</span></span>

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a><span data-ttu-id="15b9f-177">Hitelesítés és a weblap (beágyazási) jelentéseket tartalmazó</span><span class="sxs-lookup"><span data-stu-id="15b9f-177">Authentication and hosting (embedding) reports in our web page</span></span>

<span data-ttu-id="15b9f-178">A hello előző REST API-t, használhatjuk hello hozzáférési kulcs **AppKey** hello engedélyezési fejléc saját magát.</span><span class="sxs-lookup"><span data-stu-id="15b9f-178">In hello previous REST API, we can use hello access key **AppKey** itself as hello authorization header.</span></span> <span data-ttu-id="15b9f-179">Az adott hívások kezelhető hello háttér kiszolgáló oldalán, mert az biztonságos.</span><span class="sxs-lookup"><span data-stu-id="15b9f-179">Because these calls can be handled on hello backend server side, it's safe.</span></span>

<span data-ttu-id="15b9f-180">De azt hello jelentés beágyazása a weblapot, ha az ilyen biztonsági információk szeretné kezelni JavaScript használatával \(előtér).</span><span class="sxs-lookup"><span data-stu-id="15b9f-180">But, when we embed hello report in our web page, this kind of security information would be handled using JavaScript \(frontend).</span></span> <span data-ttu-id="15b9f-181">Majd hello engedélyezési fejléc értéke védetté kell tennie.</span><span class="sxs-lookup"><span data-stu-id="15b9f-181">Then hello authorization header value must be secured.</span></span> <span data-ttu-id="15b9f-182">A hozzáférési kulcsot a rosszindulatú felhasználók vagy a kártevő kód észlel, ha azokat a műveleteket, ezt a kulcsot használva tudják hívni.</span><span class="sxs-lookup"><span data-stu-id="15b9f-182">If our access key is discovered by a malicious user or malicious code, they can call any operations using this key.</span></span>

<span data-ttu-id="15b9f-183">Ha azt hello jelentés beágyazása a weblap, azt kell használnia hello számított token helyett a hozzáférési kulcsot **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="15b9f-183">When we embed hello report in our web page, we must use hello computed token instead of access key **AppKey**.</span></span> <span data-ttu-id="15b9f-184">Az alkalmazás létre kell hoznia az OAuth Json Web Token hello \(JWT) amely: hello jogcímek és hello számított digitális aláírás.</span><span class="sxs-lookup"><span data-stu-id="15b9f-184">Our application must create hello OAuth Json Web Token \(JWT) which consists of hello claims and hello computed digital signature.</span></span> <span data-ttu-id="15b9f-185">Alább bemutatott, az OAuth JWT kódolású karakterlánc ponttal elválasztott jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="15b9f-185">As illustrated below, this OAuth JWT is dot-delimited encoded string tokens.</span></span>

![](media/power-bi-embedded-iframe/oauth-jwt.png)

<span data-ttu-id="15b9f-186">Először azt kell előkészítenie hello bemeneti érték, amely később alá van írva.</span><span class="sxs-lookup"><span data-stu-id="15b9f-186">First, we must prepare hello input value, which is signed later.</span></span> <span data-ttu-id="15b9f-187">Ez az érték hello base64 kódolású url (rfc4648) karakterlánc a következő json hello, és ezek határolja hello pont \(.) karaktert.</span><span class="sxs-lookup"><span data-stu-id="15b9f-187">This value is hello base64 url encoded (rfc4648) string of hello following json, and these are delimited by hello dot \(.) character.</span></span> <span data-ttu-id="15b9f-188">Később azt ismertetjük, hogyan tooget hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="15b9f-188">Later, we'll explain how tooget hello report id.</span></span>

> [!NOTE]
> <span data-ttu-id="15b9f-189">Ha azt szeretné, hogy toouse sor sorszintű biztonságot (RLS) Power BI Embedded, azt is meg kell adnia **felhasználónév** és **szerepkörök** hello jogcímekben.</span><span class="sxs-lookup"><span data-stu-id="15b9f-189">If we want toouse Row Level Security (RLS) with Power BI Embedded, we must also specify **username** and **roles** in hello claims.</span></span>

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

<span data-ttu-id="15b9f-190">Ezután azt kell létrehoznia hello base64 kódolású karakterlánc HMAC \(hello aláírás) SHA-256 algoritmussal.</span><span class="sxs-lookup"><span data-stu-id="15b9f-190">Next, we must create hello base64 encoded string of HMAC \(hello signature) with SHA256 algorithm.</span></span> <span data-ttu-id="15b9f-191">Az aláírt bemeneti értéke hello előző karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="15b9f-191">This signed input value is hello previous string.</span></span>

<span data-ttu-id="15b9f-192">Utolsó, igazolnia kell alakítania hello bemeneti érték és időszak aláírás karakterláncot \(.) karaktert.</span><span class="sxs-lookup"><span data-stu-id="15b9f-192">Last, we must combine hello input value and signature string using period \(.) character.</span></span> <span data-ttu-id="15b9f-193">befejeződött hello karakterlánca hello app hello jelentés beágyazása lexikális eleme.</span><span class="sxs-lookup"><span data-stu-id="15b9f-193">hello completed string is hello app token for hello report embedding.</span></span> <span data-ttu-id="15b9f-194">Akkor is, ha egy rosszindulatú felhasználó hello alkalmazási jogkivonatának észlel, akkor nem olvasható be a hello eredeti elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="15b9f-194">Even if hello app token is discovered by a malicious user, they cannot get hello original access key.</span></span> <span data-ttu-id="15b9f-195">Az alkalmazás jogkivonatában gyorsan lejár.</span><span class="sxs-lookup"><span data-stu-id="15b9f-195">This app token will expire quickly.</span></span>

<span data-ttu-id="15b9f-196">Íme egy PHP-példa az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15b9f-196">Here's a PHP example for these steps:</span></span>

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is hello apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-hello-report-into-hello-web-page"></a><span data-ttu-id="15b9f-197">Végezetül hello jelentés beágyazása hello weblap</span><span class="sxs-lookup"><span data-stu-id="15b9f-197">Finally, embed hello report into hello web page</span></span>

<span data-ttu-id="15b9f-198">A jelentés beágyazása, igazolnia kell kapnak hello URL-cím és a jelentés beágyazása **azonosító** hello következő REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="15b9f-198">For embedding our report, we must get hello embed url and report **id** using hello following REST API.</span></span>

<span data-ttu-id="15b9f-199">**HTTP-kérelem**</span><span class="sxs-lookup"><span data-stu-id="15b9f-199">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="15b9f-200">**HTTP-válasz**</span><span class="sxs-lookup"><span data-stu-id="15b9f-200">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

<span data-ttu-id="15b9f-201">Azt is hello jelentés beágyazása a webalkalmazást a hello előző app token használatával.</span><span class="sxs-lookup"><span data-stu-id="15b9f-201">We can embed hello report in our web app using hello previous app token.</span></span>
<span data-ttu-id="15b9f-202">Ha úgy tekintünk hello következő mintakód, hello korábbi részében van ugyanaz, mint a korábbi példa hello hello.</span><span class="sxs-lookup"><span data-stu-id="15b9f-202">If we look at hello next sample code, hello former part is hello same as hello previous example.</span></span> <span data-ttu-id="15b9f-203">Hello utóbbi részében, az alábbi példa hello **embedUrl** \(hello előző eredményt) hello iframe, és van könyvelési hello alkalmazási jogkivonatának hello iframe be.</span><span class="sxs-lookup"><span data-stu-id="15b9f-203">In hello latter part, this sample shows hello **embedUrl** \(see hello previous result) in hello iframe, and is posting hello app token into hello iframe.</span></span>

> [!NOTE]
> <span data-ttu-id="15b9f-204">Toochange hello jelentés azonosító érték tooone saját lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="15b9f-204">You'll need toochange hello report id value tooone of your own.</span></span> <span data-ttu-id="15b9f-205">Emellett a Tartalomkezelés rendszerben tooa hiba miatt hello iframe-címke hello kódminta olvasható szó.</span><span class="sxs-lookup"><span data-stu-id="15b9f-205">Also, due tooa bug in our content management system, hello iframe tag in hello code sample is read literally.</span></span> <span data-ttu-id="15b9f-206">Ha másolja és illessze be a mintakód hello címke eltávolítása tárfiókonként hello szöveg.</span><span class="sxs-lookup"><span data-stu-id="15b9f-206">Remove hello capped text from hello tag if you copy and paste this sample code.</span></span>

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

<span data-ttu-id="15b9f-207">És az eredmény itt található:</span><span class="sxs-lookup"><span data-stu-id="15b9f-207">And here's our result:</span></span>

![](media/power-bi-embedded-iframe/view-report.png)

<span data-ttu-id="15b9f-208">Jelenleg a Power BI Embedded csak mutatja be hello hello iframe.</span><span class="sxs-lookup"><span data-stu-id="15b9f-208">At this time, Power BI Embedded only shows hello report in hello iframe.</span></span> <span data-ttu-id="15b9f-209">De, nyomon követheti a hello [Power BI Blog](https://powerbi.microsoft.com/blog/).</span><span class="sxs-lookup"><span data-stu-id="15b9f-209">But, keep an eye on hello [Power BI Blog](https://powerbi.microsoft.com/blog/).</span></span> <span data-ttu-id="15b9f-210">Jövőbeli fejlesztések használhatja új ügyféloldali API-t fog ossza meg velünk hello iframe adatokat küldeni, valamint információ. Izgalmas dolgai!</span><span class="sxs-lookup"><span data-stu-id="15b9f-210">Future improvements could use new client side APIs that will let us send information into hello iframe as well as get information out. Exciting stuff!</span></span>

## <a name="see-also"></a><span data-ttu-id="15b9f-211">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="15b9f-211">See also</span></span>
* [<span data-ttu-id="15b9f-212">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="15b9f-212">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)

<span data-ttu-id="15b9f-213">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="15b9f-213">More questions?</span></span> [<span data-ttu-id="15b9f-214">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="15b9f-214">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

