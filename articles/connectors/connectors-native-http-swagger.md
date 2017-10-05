---
title: "Hívja a Swagger kiegészítve HTTP REST-végpontok Azure Logic Apps-összekötője |} Microsoft Docs"
description: "REST-végpontok csatlakozni a logic Apps alkalmazásokból, kiegészítve a HTTP Swagger Swagger keresztül összekötő"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 3e9229d94e96aad7b769d0e55d208d856e3b80bc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-http--swagger-action"></a><span data-ttu-id="63c74-103">Ismerkedjen meg a HTTP és a Swagger művelet</span><span class="sxs-lookup"><span data-stu-id="63c74-103">Get started with the HTTP + Swagger action</span></span>

<span data-ttu-id="63c74-104">A REST-végpont keresztül az első osztályú összekötő hozhat létre egy [Swagger-dokumentum](https://swagger.io) a HTTP és a Swagger használata esetén a logic app munkafolyamat művelet.</span><span class="sxs-lookup"><span data-stu-id="63c74-104">You can create a first-class connector to any REST endpoint through a [Swagger document](https://swagger.io) when you use the HTTP + Swagger action in your logic app workflow.</span></span> <span data-ttu-id="63c74-105">A logic apps hívni minden REST-végpont első osztályú Logic App Designer nyújthassunk is kiterjeszthető.</span><span class="sxs-lookup"><span data-stu-id="63c74-105">You can also extend logic apps to call any REST endpoint with a first-class Logic App Designer experience.</span></span>

<span data-ttu-id="63c74-106">Az összekötők logic Apps alkalmazások létrehozásához, lásd: [új logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="63c74-106">To learn how to create logic apps with connectors, see [Create a new logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a><span data-ttu-id="63c74-107">A HTTP + Swagger egy eseményindító vagy egy műveletet</span><span class="sxs-lookup"><span data-stu-id="63c74-107">Use HTTP + Swagger as a trigger or an action</span></span>

<span data-ttu-id="63c74-108">A HTTP + Swagger indul el, és azonos módon működik, a művelet a [HTTP-művelet](connectors-native-http.md) jelentkezik, mintha az API-struktúra és kimeneteinek Logic App Designer jobb környezetet biztosítson, de a [Swagger-metaadatok](https://swagger.io).</span><span class="sxs-lookup"><span data-stu-id="63c74-108">The HTTP + Swagger trigger and action work the same as the [HTTP action](connectors-native-http.md) but provide a better experience in Logic App Designer by exposing the API structure and outputs from the [Swagger metadata](https://swagger.io).</span></span> <span data-ttu-id="63c74-109">Használhatja a HTTP + Swagger összekötő kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="63c74-109">You can also use the HTTP + Swagger connector as a trigger.</span></span> <span data-ttu-id="63c74-110">Lekérdezési eseményindító végrehajtásához, hajtsa végre a lekérdezés minta ismertetett [hozzon létre egyéni API-k hívására más API-k, a szolgáltatások és a rendszer a logic Apps alkalmazásokból](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span><span class="sxs-lookup"><span data-stu-id="63c74-110">If you want to implement a polling trigger, follow the polling pattern that's described in [Create custom APIs to call other APIs, services, and systems from logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span></span>

<span data-ttu-id="63c74-111">További információ [logic app eseményindítók és műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="63c74-111">Learn more about [logic app triggers and actions](connectors-overview.md).</span></span>

<span data-ttu-id="63c74-112">Itt található példa bemutatja, hogyan használja a HTTP és a Swagger-műveleti logikai alkalmazás munkafolyamata a műveletet.</span><span class="sxs-lookup"><span data-stu-id="63c74-112">Here's an example of how to use the HTTP + Swagger operation as an action in a workflow in a logic app.</span></span>

1. <span data-ttu-id="63c74-113">Válassza ki a **új lépés** gombra.</span><span class="sxs-lookup"><span data-stu-id="63c74-113">Select the **New Step** button.</span></span>
2. <span data-ttu-id="63c74-114">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="63c74-114">Select **Add an action**.</span></span>
3. <span data-ttu-id="63c74-115">A művelet a keresőmezőbe írja be **swagger** lista a HTTP + Swagger művelet.</span><span class="sxs-lookup"><span data-stu-id="63c74-115">In the action search box, type **swagger** to list the HTTP + Swagger action.</span></span>
   
    ![Válassza ki a HTTP + Swagger művelet](./media/connectors-native-http-swagger/using-action-1.png)
4. <span data-ttu-id="63c74-117">Írja be a Swagger-dokumentum URL-címe:</span><span class="sxs-lookup"><span data-stu-id="63c74-117">Type the URL for a Swagger document:</span></span>
   
   * <span data-ttu-id="63c74-118">A Logic App Designer használatához az URL-cím HTTPS-végpontnak kell lennie, és a CORS engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="63c74-118">To work from the Logic App Designer, the URL must be an HTTPS endpoint and have CORS enabled.</span></span>
   * <span data-ttu-id="63c74-119">A Swagger-dokumentum nem felel meg ennek a követelménynek, ha [Azure Storage a CORS engedélyezése mellett](#hosting-swagger-from-storage) a fájlt kívánja tárolni.</span><span class="sxs-lookup"><span data-stu-id="63c74-119">If the Swagger document doesn't meet this requirement, you can use [Azure Storage with CORS enabled](#hosting-swagger-from-storage) to store the document.</span></span>
5. <span data-ttu-id="63c74-120">Kattintson a **következő** olvassa el és a Swagger-dokumentumból.</span><span class="sxs-lookup"><span data-stu-id="63c74-120">Click **Next** to read and render from the Swagger document.</span></span>
6. <span data-ttu-id="63c74-121">Adja hozzá a HTTP-hívás a szükséges paramétereket.</span><span class="sxs-lookup"><span data-stu-id="63c74-121">Add in any parameters that are required for the HTTP call.</span></span>
   
    ![Teljes HTTP-művelet](./media/connectors-native-http-swagger/using-action-2.png)
7. <span data-ttu-id="63c74-123">Mentse, és tegye közzé a Logic Apps alkalmazást, kattintson a **mentése** designer eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="63c74-123">To save and publish your logic app, click **Save** on designer toolbar.</span></span>

### <a name="host-swagger-from-azure-storage"></a><span data-ttu-id="63c74-124">Az Azure Storage állomás Swagger</span><span class="sxs-lookup"><span data-stu-id="63c74-124">Host Swagger from Azure Storage</span></span>
<span data-ttu-id="63c74-125">Előfordulhat, hogy szeretne hivatkozni, amely nem található, vagy, amely nem felel meg a biztonsági és a Tervező eltérő eredetű követelményei Swagger dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="63c74-125">You might want to reference a Swagger document that's not hosted, or that doesn't meet the security and cross-origin requirements for the designer.</span></span> <span data-ttu-id="63c74-126">A probléma megoldásához, a Swagger-dokumentum az Azure Storage tárolja, és hivatkozni, a dokumentum a CORS engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="63c74-126">To resolve this issue, you can store the Swagger document in Azure Storage and enable CORS to reference the document.</span></span>  

<span data-ttu-id="63c74-127">Létrehozása, konfigurálása és a Swagger-dokumentumok tárolására az Azure Storage lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="63c74-127">Here are the steps to create, configure, and store Swagger documents in Azure Storage:</span></span>

1. <span data-ttu-id="63c74-128">[Az Azure storage-fiók létrehozása az Azure Blob storage szolgáltatással](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="63c74-128">[Create an Azure storage account with Azure Blob storage](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="63c74-129">Ez a lépés végrehajtásához engedélyek beállítása **nyilvános hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="63c74-129">To perform this step, set permissions to **Public Access**.</span></span>

2. <span data-ttu-id="63c74-130">A CORS engedélyezése blobot.</span><span class="sxs-lookup"><span data-stu-id="63c74-130">Enable CORS on the blob.</span></span> 

   <span data-ttu-id="63c74-131">Ezzel a beállítással automatikusan konfigurálásához használható [a PowerShell parancsfájl](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span><span class="sxs-lookup"><span data-stu-id="63c74-131">To automatically configure this setting, you can use [this PowerShell script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span></span>

3. <span data-ttu-id="63c74-132">A Swagger-fájl feltöltése a blob.</span><span class="sxs-lookup"><span data-stu-id="63c74-132">Upload the Swagger file to the blob.</span></span> 

   <span data-ttu-id="63c74-133">Az ebben a lépésben végezheti el a [Azure-portálon](https://portal.azure.com) vagy egy eszköz, például a [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="63c74-133">You can perform this step from the [Azure portal](https://portal.azure.com) or from a tool like [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

4. <span data-ttu-id="63c74-134">A dokumentum az Azure Blob storage HTTPS-kapcsolat hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="63c74-134">Reference an HTTPS link to the document in Azure Blob storage.</span></span> 

   <span data-ttu-id="63c74-135">A hivatkozás ezt a formátumot használja:</span><span class="sxs-lookup"><span data-stu-id="63c74-135">The link uses this format:</span></span>

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a><span data-ttu-id="63c74-136">Technikai részletek</span><span class="sxs-lookup"><span data-stu-id="63c74-136">Technical details</span></span>
<span data-ttu-id="63c74-137">Az alábbiakban az eseményindítók és műveletek részleteit, amely a HTTP + Swagger összekötő támogatja.</span><span class="sxs-lookup"><span data-stu-id="63c74-137">Following are the details for the triggers and actions that this HTTP + Swagger connector supports.</span></span>

## <a name="http--swagger-triggers"></a><span data-ttu-id="63c74-138">HTTP + Swagger eseményindítók</span><span class="sxs-lookup"><span data-stu-id="63c74-138">HTTP + Swagger triggers</span></span>
<span data-ttu-id="63c74-139">Egy eseményindító egy eseményt, amely segítségével indítsa el a munkafolyamatot, amely a logikai alkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="63c74-139">A trigger is an event that can be used to start the workflow that's defined in a logic app.</span></span> [<span data-ttu-id="63c74-140">További tudnivalók az eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="63c74-140">Learn more about triggers.</span></span>](connectors-overview.md) <span data-ttu-id="63c74-141">A HTTP és a Swagger összekötő egy eseményindító tartozik.</span><span class="sxs-lookup"><span data-stu-id="63c74-141">The HTTP + Swagger connector has one trigger.</span></span>

| <span data-ttu-id="63c74-142">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="63c74-142">Trigger</span></span> | <span data-ttu-id="63c74-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="63c74-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="63c74-144">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="63c74-144">HTTP + Swagger</span></span> |<span data-ttu-id="63c74-145">Egy HTTP-hívást, és vissza válasz tartalma</span><span class="sxs-lookup"><span data-stu-id="63c74-145">Make an HTTP call and return the response content</span></span> |

## <a name="http--swagger-actions"></a><span data-ttu-id="63c74-146">HTTP + Swagger műveletek</span><span class="sxs-lookup"><span data-stu-id="63c74-146">HTTP + Swagger actions</span></span>
<span data-ttu-id="63c74-147">Egy művelet során, amely a logikai alkalmazás definiált munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="63c74-147">An action is an operation that's carried out by the workflow that's defined in a logic app.</span></span> [<span data-ttu-id="63c74-148">Ismerje meg a műveletet.</span><span class="sxs-lookup"><span data-stu-id="63c74-148">Learn more about actions.</span></span>](connectors-overview.md) <span data-ttu-id="63c74-149">A HTTP és a Swagger összekötő tartozik egy lehetséges művelet.</span><span class="sxs-lookup"><span data-stu-id="63c74-149">The HTTP + Swagger connector has one possible action.</span></span>

| <span data-ttu-id="63c74-150">Műveletek</span><span class="sxs-lookup"><span data-stu-id="63c74-150">Action</span></span> | <span data-ttu-id="63c74-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="63c74-151">Description</span></span> |
| --- | --- |
| <span data-ttu-id="63c74-152">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="63c74-152">HTTP + Swagger</span></span> |<span data-ttu-id="63c74-153">Egy HTTP-hívást, és vissza válasz tartalma</span><span class="sxs-lookup"><span data-stu-id="63c74-153">Make an HTTP call and return the response content</span></span> |

### <a name="action-details"></a><span data-ttu-id="63c74-154">A művelet részletei</span><span class="sxs-lookup"><span data-stu-id="63c74-154">Action details</span></span>
<span data-ttu-id="63c74-155">A HTTP és a Swagger összekötő egy lehetséges műveletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="63c74-155">The HTTP + Swagger connector comes with one possible action.</span></span> <span data-ttu-id="63c74-156">Az alábbiakban az egyes műveletek, a szükséges és választható beviteli mezők és a megfelelő kimeneti részletek használatát társított információkat.</span><span class="sxs-lookup"><span data-stu-id="63c74-156">Following is information about each of the actions, their required and optional input fields, and the corresponding output details that are associated with their usage.</span></span>

#### <a name="http--swagger"></a><span data-ttu-id="63c74-157">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="63c74-157">HTTP + Swagger</span></span>
<span data-ttu-id="63c74-158">Ellenőrizze a kimenő HTTP-kérelem a Swagger-metaadatok segítséget.</span><span class="sxs-lookup"><span data-stu-id="63c74-158">Make an HTTP outbound request with assistance of Swagger metadata.</span></span>
<span data-ttu-id="63c74-159">Egy csillag (*) azt jelenti, hogy a mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="63c74-159">An asterisk (*) means a required field.</span></span>

| <span data-ttu-id="63c74-160">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="63c74-160">Display name</span></span> | <span data-ttu-id="63c74-161">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="63c74-161">Property name</span></span> | <span data-ttu-id="63c74-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="63c74-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63c74-163">Módszer *</span><span class="sxs-lookup"><span data-stu-id="63c74-163">Method*</span></span> |<span data-ttu-id="63c74-164">Módszer</span><span class="sxs-lookup"><span data-stu-id="63c74-164">method</span></span> |<span data-ttu-id="63c74-165">Használja a HTTP-műveletet.</span><span class="sxs-lookup"><span data-stu-id="63c74-165">HTTP verb to use.</span></span> |
| <span data-ttu-id="63c74-166">URI *</span><span class="sxs-lookup"><span data-stu-id="63c74-166">URI*</span></span> |<span data-ttu-id="63c74-167">URI</span><span class="sxs-lookup"><span data-stu-id="63c74-167">uri</span></span> |<span data-ttu-id="63c74-168">A HTTP-kérelem URI-Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="63c74-168">URI for the HTTP request.</span></span> |
| <span data-ttu-id="63c74-169">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="63c74-169">Headers</span></span> |<span data-ttu-id="63c74-170">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="63c74-170">headers</span></span> |<span data-ttu-id="63c74-171">A HTTP-fejlécek tartalmazza JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="63c74-171">A JSON object of HTTP headers to include.</span></span> |
| <span data-ttu-id="63c74-172">Törzs</span><span class="sxs-lookup"><span data-stu-id="63c74-172">Body</span></span> |<span data-ttu-id="63c74-173">Törzs</span><span class="sxs-lookup"><span data-stu-id="63c74-173">body</span></span> |<span data-ttu-id="63c74-174">A HTTP-kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="63c74-174">The HTTP request body.</span></span> |
| <span data-ttu-id="63c74-175">Authentication</span><span class="sxs-lookup"><span data-stu-id="63c74-175">Authentication</span></span> |<span data-ttu-id="63c74-176">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="63c74-176">authentication</span></span> |<span data-ttu-id="63c74-177">Hitelesítési kérelem használatára.</span><span class="sxs-lookup"><span data-stu-id="63c74-177">Authentication to use for request.</span></span> <span data-ttu-id="63c74-178">További információkért lásd: a [HTTP összekötő](connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="63c74-178">For more information, see the [HTTP connector](connectors-native-http.md#authentication).</span></span> |

<span data-ttu-id="63c74-179">**Kimeneti részletei**</span><span class="sxs-lookup"><span data-stu-id="63c74-179">**Output details**</span></span>

<span data-ttu-id="63c74-180">HTTP-válasz</span><span class="sxs-lookup"><span data-stu-id="63c74-180">HTTP response</span></span>

| <span data-ttu-id="63c74-181">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="63c74-181">Property Name</span></span> | <span data-ttu-id="63c74-182">Adattípus</span><span class="sxs-lookup"><span data-stu-id="63c74-182">Data type</span></span> | <span data-ttu-id="63c74-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="63c74-183">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63c74-184">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="63c74-184">Headers</span></span> |<span data-ttu-id="63c74-185">Objektum</span><span class="sxs-lookup"><span data-stu-id="63c74-185">object</span></span> |<span data-ttu-id="63c74-186">Válaszfejlécek</span><span class="sxs-lookup"><span data-stu-id="63c74-186">Response headers</span></span> |
| <span data-ttu-id="63c74-187">Törzs</span><span class="sxs-lookup"><span data-stu-id="63c74-187">Body</span></span> |<span data-ttu-id="63c74-188">Objektum</span><span class="sxs-lookup"><span data-stu-id="63c74-188">object</span></span> |<span data-ttu-id="63c74-189">Válasz objektum</span><span class="sxs-lookup"><span data-stu-id="63c74-189">Response object</span></span> |
| <span data-ttu-id="63c74-190">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="63c74-190">Status Code</span></span> |<span data-ttu-id="63c74-191">int</span><span class="sxs-lookup"><span data-stu-id="63c74-191">int</span></span> |<span data-ttu-id="63c74-192">HTTP-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="63c74-192">HTTP status code</span></span> |

### <a name="http-responses"></a><span data-ttu-id="63c74-193">HTTP-válaszok</span><span class="sxs-lookup"><span data-stu-id="63c74-193">HTTP responses</span></span>
<span data-ttu-id="63c74-194">Amikor különböző műveletekkel, bizonyos válaszokat kaphat.</span><span class="sxs-lookup"><span data-stu-id="63c74-194">When making calls to various actions, you might get certain responses.</span></span> <span data-ttu-id="63c74-195">Az alábbiakban látható egy táblázat a megfelelő válaszok és leírásokat.</span><span class="sxs-lookup"><span data-stu-id="63c74-195">Following is a table that outlines corresponding responses and descriptions.</span></span>

| <span data-ttu-id="63c74-196">Név</span><span class="sxs-lookup"><span data-stu-id="63c74-196">Name</span></span> | <span data-ttu-id="63c74-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="63c74-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="63c74-198">200</span><span class="sxs-lookup"><span data-stu-id="63c74-198">200</span></span> |<span data-ttu-id="63c74-199">OKÉ</span><span class="sxs-lookup"><span data-stu-id="63c74-199">OK</span></span> |
| <span data-ttu-id="63c74-200">202</span><span class="sxs-lookup"><span data-stu-id="63c74-200">202</span></span> |<span data-ttu-id="63c74-201">Elfogadva</span><span class="sxs-lookup"><span data-stu-id="63c74-201">Accepted</span></span> |
| <span data-ttu-id="63c74-202">400</span><span class="sxs-lookup"><span data-stu-id="63c74-202">400</span></span> |<span data-ttu-id="63c74-203">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="63c74-203">Bad request</span></span> |
| <span data-ttu-id="63c74-204">401</span><span class="sxs-lookup"><span data-stu-id="63c74-204">401</span></span> |<span data-ttu-id="63c74-205">Nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="63c74-205">Unauthorized</span></span> |
| <span data-ttu-id="63c74-206">403</span><span class="sxs-lookup"><span data-stu-id="63c74-206">403</span></span> |<span data-ttu-id="63c74-207">Tiltott</span><span class="sxs-lookup"><span data-stu-id="63c74-207">Forbidden</span></span> |
| <span data-ttu-id="63c74-208">404</span><span class="sxs-lookup"><span data-stu-id="63c74-208">404</span></span> |<span data-ttu-id="63c74-209">Nem található</span><span class="sxs-lookup"><span data-stu-id="63c74-209">Not Found</span></span> |
| <span data-ttu-id="63c74-210">500</span><span class="sxs-lookup"><span data-stu-id="63c74-210">500</span></span> |<span data-ttu-id="63c74-211">Belső kiszolgálóhiba.</span><span class="sxs-lookup"><span data-stu-id="63c74-211">Internal server error.</span></span> <span data-ttu-id="63c74-212">Ismeretlen hiba történt.</span><span class="sxs-lookup"><span data-stu-id="63c74-212">Unknown error occurred.</span></span> |

- - -
## <a name="next-steps"></a><span data-ttu-id="63c74-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="63c74-213">Next steps</span></span>

* [<span data-ttu-id="63c74-214">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="63c74-214">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="63c74-215">Más összekötők keresése</span><span class="sxs-lookup"><span data-stu-id="63c74-215">Find other connectors</span></span>](apis-list.md)