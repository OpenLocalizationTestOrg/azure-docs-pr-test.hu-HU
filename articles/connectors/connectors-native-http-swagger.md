---
title: "kiegészítve HTTP Swagger aaaCall REST-végpontok Azure Logic Apps-összekötője |} Microsoft Docs"
description: "Csatlakozás tooREST végpontok a logic Apps alkalmazásokból keresztül Swagger kiegészítve hello HTTP Swagger-összekötő"
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
ms.openlocfilehash: baaa57689ff41fcd052f9d86086e36619ddec46e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http--swagger-action"></a><span data-ttu-id="69228-103">Ismerkedés a hello HTTP + Swagger művelet</span><span class="sxs-lookup"><span data-stu-id="69228-103">Get started with hello HTTP + Swagger action</span></span>

<span data-ttu-id="69228-104">Első osztályú összekötő tooany REST-végpont használatával hozhat létre egy [Swagger-dokumentum](https://swagger.io) használatakor hello HTTP + Swagger műveleti a logic app munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="69228-104">You can create a first-class connector tooany REST endpoint through a [Swagger document](https://swagger.io) when you use hello HTTP + Swagger action in your logic app workflow.</span></span> <span data-ttu-id="69228-105">Logic apps toocall bármely REST-végpont első osztályú Logic App Designer nyújthassunk is kiterjeszthető.</span><span class="sxs-lookup"><span data-stu-id="69228-105">You can also extend logic apps toocall any REST endpoint with a first-class Logic App Designer experience.</span></span>

<span data-ttu-id="69228-106">Hogyan toocreate a logic apps, összekötőkkel: toolearn [új logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="69228-106">toolearn how toocreate logic apps with connectors, see [Create a new logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a><span data-ttu-id="69228-107">A HTTP + Swagger egy eseményindító vagy egy műveletet</span><span class="sxs-lookup"><span data-stu-id="69228-107">Use HTTP + Swagger as a trigger or an action</span></span>

<span data-ttu-id="69228-108">hello HTTP + Swagger eseményindítója és tevékenysége fel munkahelyi hello ugyanaz, mint hello [HTTP-művelet](connectors-native-http.md) jelentkezik, mintha hello API struktúra és kimeneteinek hello Logic App Designer jobb környezetet biztosítson, de [Swagger-metaadatok](https://swagger.io) .</span><span class="sxs-lookup"><span data-stu-id="69228-108">hello HTTP + Swagger trigger and action work hello same as hello [HTTP action](connectors-native-http.md) but provide a better experience in Logic App Designer by exposing hello API structure and outputs from hello [Swagger metadata](https://swagger.io).</span></span> <span data-ttu-id="69228-109">Hello HTTP + Swagger-összekötőt használhatja kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="69228-109">You can also use hello HTTP + Swagger connector as a trigger.</span></span> <span data-ttu-id="69228-110">Tooimplement lekérdezési eseményindítót, hajtsa végre hello lekérdezési mintát ismertetett [más API-k, a szolgáltatások és a rendszer hozzon létre egyéni API-k toocall a logic Apps alkalmazásokból](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span><span class="sxs-lookup"><span data-stu-id="69228-110">If you want tooimplement a polling trigger, follow hello polling pattern that's described in [Create custom APIs toocall other APIs, services, and systems from logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span></span>

<span data-ttu-id="69228-111">További információ [logic app eseményindítók és műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="69228-111">Learn more about [logic app triggers and actions](connectors-overview.md).</span></span>

<span data-ttu-id="69228-112">Íme egy példa a hogyan toouse hello HTTP + Swagger műveletet, mert a logikai alkalmazás munkafolyamata művelet.</span><span class="sxs-lookup"><span data-stu-id="69228-112">Here's an example of how toouse hello HTTP + Swagger operation as an action in a workflow in a logic app.</span></span>

1. <span data-ttu-id="69228-113">Jelölje be hello **új lépés** gombra.</span><span class="sxs-lookup"><span data-stu-id="69228-113">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="69228-114">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="69228-114">Select **Add an action**.</span></span>
3. <span data-ttu-id="69228-115">Hello művelet keresési mezőbe, írja be a **swagger** toolist hello HTTP + Swagger művelet.</span><span class="sxs-lookup"><span data-stu-id="69228-115">In hello action search box, type **swagger** toolist hello HTTP + Swagger action.</span></span>
   
    ![Válassza ki a HTTP + Swagger művelet](./media/connectors-native-http-swagger/using-action-1.png)
4. <span data-ttu-id="69228-117">Írja be a Swagger-dokumentum hello URL-címe:</span><span class="sxs-lookup"><span data-stu-id="69228-117">Type hello URL for a Swagger document:</span></span>
   
   * <span data-ttu-id="69228-118">toowork hello Logic App Designer hello URL-címet a HTTPS-végpontnak kell lennie, és a CORS engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="69228-118">toowork from hello Logic App Designer, hello URL must be an HTTPS endpoint and have CORS enabled.</span></span>
   * <span data-ttu-id="69228-119">Hello Swagger-dokumentum nem felel meg ennek a követelménynek, ha [Azure Storage a CORS engedélyezése mellett](#hosting-swagger-from-storage) toostore hello dokumentum.</span><span class="sxs-lookup"><span data-stu-id="69228-119">If hello Swagger document doesn't meet this requirement, you can use [Azure Storage with CORS enabled](#hosting-swagger-from-storage) toostore hello document.</span></span>
5. <span data-ttu-id="69228-120">Kattintson a **következő** tooread és a leképezési hello Swagger-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="69228-120">Click **Next** tooread and render from hello Swagger document.</span></span>
6. <span data-ttu-id="69228-121">Adja hozzá a hello HTTP hívás a szükséges paramétereket.</span><span class="sxs-lookup"><span data-stu-id="69228-121">Add in any parameters that are required for hello HTTP call.</span></span>
   
    ![Teljes HTTP-művelet](./media/connectors-native-http-swagger/using-action-2.png)
7. <span data-ttu-id="69228-123">toosave és a logikai alkalmazás közzétételéhez kattintson **mentése** designer eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="69228-123">toosave and publish your logic app, click **Save** on designer toolbar.</span></span>

### <a name="host-swagger-from-azure-storage"></a><span data-ttu-id="69228-124">Az Azure Storage állomás Swagger</span><span class="sxs-lookup"><span data-stu-id="69228-124">Host Swagger from Azure Storage</span></span>
<span data-ttu-id="69228-125">Érdemes lehet tooreference, amely nem található, vagy, amely nem felel meg a hello biztonsági és eltérő eredetű követelményei hello designer Swagger dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="69228-125">You might want tooreference a Swagger document that's not hosted, or that doesn't meet hello security and cross-origin requirements for hello designer.</span></span> <span data-ttu-id="69228-126">tooresolve a probléma hello Swagger-dokumentum tárolására az Azure Storage és a CORS tooreference hello dokumentum engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="69228-126">tooresolve this issue, you can store hello Swagger document in Azure Storage and enable CORS tooreference hello document.</span></span>  

<span data-ttu-id="69228-127">Az alábbiakban hello lépéseket toocreate, konfigurálása és Swagger dokumentumok tárolására az Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="69228-127">Here are hello steps toocreate, configure, and store Swagger documents in Azure Storage:</span></span>

1. <span data-ttu-id="69228-128">[Az Azure storage-fiók létrehozása az Azure Blob storage szolgáltatással](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="69228-128">[Create an Azure storage account with Azure Blob storage](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="69228-129">tooperform ez. lépés:, engedélyek beállítása túl**nyilvános hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="69228-129">tooperform this step, set permissions too**Public Access**.</span></span>

2. <span data-ttu-id="69228-130">A CORS engedélyezése hello blob.</span><span class="sxs-lookup"><span data-stu-id="69228-130">Enable CORS on hello blob.</span></span> 

   <span data-ttu-id="69228-131">tooautomatically konfigurálja ezt a beállítást, használja [a PowerShell parancsfájl](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span><span class="sxs-lookup"><span data-stu-id="69228-131">tooautomatically configure this setting, you can use [this PowerShell script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span></span>

3. <span data-ttu-id="69228-132">Hello Swagger-fájl toohello blob feltöltése.</span><span class="sxs-lookup"><span data-stu-id="69228-132">Upload hello Swagger file toohello blob.</span></span> 

   <span data-ttu-id="69228-133">Ebben a lépésben végezhető el hello [Azure-portálon](https://portal.azure.com) vagy egy eszköz, például a [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="69228-133">You can perform this step from hello [Azure portal](https://portal.azure.com) or from a tool like [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

4. <span data-ttu-id="69228-134">Egy HTTPS kapcsolat toohello dokumentum az Azure Blob storage hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="69228-134">Reference an HTTPS link toohello document in Azure Blob storage.</span></span> 

   <span data-ttu-id="69228-135">hello hivatkozás ezt a formátumot használja:</span><span class="sxs-lookup"><span data-stu-id="69228-135">hello link uses this format:</span></span>

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a><span data-ttu-id="69228-136">Technikai részletek</span><span class="sxs-lookup"><span data-stu-id="69228-136">Technical details</span></span>
<span data-ttu-id="69228-137">Az alábbiakban részletesen hello hello eseményindítók és műveletek, amelyhez a HTTP + Swagger összekötő támogatja.</span><span class="sxs-lookup"><span data-stu-id="69228-137">Following are hello details for hello triggers and actions that this HTTP + Swagger connector supports.</span></span>

## <a name="http--swagger-triggers"></a><span data-ttu-id="69228-138">HTTP + Swagger eseményindítók</span><span class="sxs-lookup"><span data-stu-id="69228-138">HTTP + Swagger triggers</span></span>
<span data-ttu-id="69228-139">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="69228-139">A trigger is an event that can be used toostart hello workflow that's defined in a logic app.</span></span> [<span data-ttu-id="69228-140">További tudnivalók az eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="69228-140">Learn more about triggers.</span></span>](connectors-overview.md) <span data-ttu-id="69228-141">HTTP + Swagger hello összekötő egy eseményindító tartozik.</span><span class="sxs-lookup"><span data-stu-id="69228-141">hello HTTP + Swagger connector has one trigger.</span></span>

| <span data-ttu-id="69228-142">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="69228-142">Trigger</span></span> | <span data-ttu-id="69228-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="69228-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="69228-144">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="69228-144">HTTP + Swagger</span></span> |<span data-ttu-id="69228-145">Egy HTTP-hívást, és térjen vissza a hello válasz tartalom</span><span class="sxs-lookup"><span data-stu-id="69228-145">Make an HTTP call and return hello response content</span></span> |

## <a name="http--swagger-actions"></a><span data-ttu-id="69228-146">HTTP + Swagger műveletek</span><span class="sxs-lookup"><span data-stu-id="69228-146">HTTP + Swagger actions</span></span>
<span data-ttu-id="69228-147">Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="69228-147">An action is an operation that's carried out by hello workflow that's defined in a logic app.</span></span> [<span data-ttu-id="69228-148">Ismerje meg a műveletet.</span><span class="sxs-lookup"><span data-stu-id="69228-148">Learn more about actions.</span></span>](connectors-overview.md) <span data-ttu-id="69228-149">HTTP + Swagger hello összekötő tartozik egy lehetséges művelet.</span><span class="sxs-lookup"><span data-stu-id="69228-149">hello HTTP + Swagger connector has one possible action.</span></span>

| <span data-ttu-id="69228-150">Műveletek</span><span class="sxs-lookup"><span data-stu-id="69228-150">Action</span></span> | <span data-ttu-id="69228-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="69228-151">Description</span></span> |
| --- | --- |
| <span data-ttu-id="69228-152">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="69228-152">HTTP + Swagger</span></span> |<span data-ttu-id="69228-153">Egy HTTP-hívást, és térjen vissza a hello válasz tartalom</span><span class="sxs-lookup"><span data-stu-id="69228-153">Make an HTTP call and return hello response content</span></span> |

### <a name="action-details"></a><span data-ttu-id="69228-154">A művelet részletei</span><span class="sxs-lookup"><span data-stu-id="69228-154">Action details</span></span>
<span data-ttu-id="69228-155">HTTP + Swagger hello összekötő egy lehetséges műveletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="69228-155">hello HTTP + Swagger connector comes with one possible action.</span></span> <span data-ttu-id="69228-156">Az alábbiakban az egyes hello műveletek, a szükséges és választható beviteli mezőt, és a megfelelő kimeneti részletek használatát társított hello kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="69228-156">Following is information about each of hello actions, their required and optional input fields, and hello corresponding output details that are associated with their usage.</span></span>

#### <a name="http--swagger"></a><span data-ttu-id="69228-157">HTTP + Swagger</span><span class="sxs-lookup"><span data-stu-id="69228-157">HTTP + Swagger</span></span>
<span data-ttu-id="69228-158">Ellenőrizze a kimenő HTTP-kérelem a Swagger-metaadatok segítséget.</span><span class="sxs-lookup"><span data-stu-id="69228-158">Make an HTTP outbound request with assistance of Swagger metadata.</span></span>
<span data-ttu-id="69228-159">Egy csillag (*) azt jelenti, hogy a mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="69228-159">An asterisk (*) means a required field.</span></span>

| <span data-ttu-id="69228-160">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="69228-160">Display name</span></span> | <span data-ttu-id="69228-161">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="69228-161">Property name</span></span> | <span data-ttu-id="69228-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="69228-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="69228-163">Módszer *</span><span class="sxs-lookup"><span data-stu-id="69228-163">Method*</span></span> |<span data-ttu-id="69228-164">Módszer</span><span class="sxs-lookup"><span data-stu-id="69228-164">method</span></span> |<span data-ttu-id="69228-165">HTTP-művelet toouse.</span><span class="sxs-lookup"><span data-stu-id="69228-165">HTTP verb toouse.</span></span> |
| <span data-ttu-id="69228-166">URI *</span><span class="sxs-lookup"><span data-stu-id="69228-166">URI*</span></span> |<span data-ttu-id="69228-167">URI</span><span class="sxs-lookup"><span data-stu-id="69228-167">uri</span></span> |<span data-ttu-id="69228-168">Hello HTTP-kérelem URI-Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="69228-168">URI for hello HTTP request.</span></span> |
| <span data-ttu-id="69228-169">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="69228-169">Headers</span></span> |<span data-ttu-id="69228-170">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="69228-170">headers</span></span> |<span data-ttu-id="69228-171">A HTTP-fejlécek tooinclude JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="69228-171">A JSON object of HTTP headers tooinclude.</span></span> |
| <span data-ttu-id="69228-172">Törzs</span><span class="sxs-lookup"><span data-stu-id="69228-172">Body</span></span> |<span data-ttu-id="69228-173">Törzs</span><span class="sxs-lookup"><span data-stu-id="69228-173">body</span></span> |<span data-ttu-id="69228-174">hello HTTP-kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="69228-174">hello HTTP request body.</span></span> |
| <span data-ttu-id="69228-175">Authentication</span><span class="sxs-lookup"><span data-stu-id="69228-175">Authentication</span></span> |<span data-ttu-id="69228-176">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="69228-176">authentication</span></span> |<span data-ttu-id="69228-177">Hitelesítési toouse kérelem.</span><span class="sxs-lookup"><span data-stu-id="69228-177">Authentication toouse for request.</span></span> <span data-ttu-id="69228-178">További információkért lásd: hello [HTTP összekötő](connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="69228-178">For more information, see hello [HTTP connector](connectors-native-http.md#authentication).</span></span> |

<span data-ttu-id="69228-179">**Kimeneti részletei**</span><span class="sxs-lookup"><span data-stu-id="69228-179">**Output details**</span></span>

<span data-ttu-id="69228-180">HTTP-válasz</span><span class="sxs-lookup"><span data-stu-id="69228-180">HTTP response</span></span>

| <span data-ttu-id="69228-181">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="69228-181">Property Name</span></span> | <span data-ttu-id="69228-182">Adattípus</span><span class="sxs-lookup"><span data-stu-id="69228-182">Data type</span></span> | <span data-ttu-id="69228-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="69228-183">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="69228-184">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="69228-184">Headers</span></span> |<span data-ttu-id="69228-185">Objektum</span><span class="sxs-lookup"><span data-stu-id="69228-185">object</span></span> |<span data-ttu-id="69228-186">Válaszfejlécek</span><span class="sxs-lookup"><span data-stu-id="69228-186">Response headers</span></span> |
| <span data-ttu-id="69228-187">Törzs</span><span class="sxs-lookup"><span data-stu-id="69228-187">Body</span></span> |<span data-ttu-id="69228-188">Objektum</span><span class="sxs-lookup"><span data-stu-id="69228-188">object</span></span> |<span data-ttu-id="69228-189">Válasz objektum</span><span class="sxs-lookup"><span data-stu-id="69228-189">Response object</span></span> |
| <span data-ttu-id="69228-190">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="69228-190">Status Code</span></span> |<span data-ttu-id="69228-191">int</span><span class="sxs-lookup"><span data-stu-id="69228-191">int</span></span> |<span data-ttu-id="69228-192">HTTP-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="69228-192">HTTP status code</span></span> |

### <a name="http-responses"></a><span data-ttu-id="69228-193">HTTP-válaszok</span><span class="sxs-lookup"><span data-stu-id="69228-193">HTTP responses</span></span>
<span data-ttu-id="69228-194">Hívások toovarious műveletek meghozásakor bizonyos válaszokat kaphat.</span><span class="sxs-lookup"><span data-stu-id="69228-194">When making calls toovarious actions, you might get certain responses.</span></span> <span data-ttu-id="69228-195">Az alábbiakban látható egy táblázat a megfelelő válaszok és leírásokat.</span><span class="sxs-lookup"><span data-stu-id="69228-195">Following is a table that outlines corresponding responses and descriptions.</span></span>

| <span data-ttu-id="69228-196">Név</span><span class="sxs-lookup"><span data-stu-id="69228-196">Name</span></span> | <span data-ttu-id="69228-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="69228-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="69228-198">200</span><span class="sxs-lookup"><span data-stu-id="69228-198">200</span></span> |<span data-ttu-id="69228-199">OKÉ</span><span class="sxs-lookup"><span data-stu-id="69228-199">OK</span></span> |
| <span data-ttu-id="69228-200">202</span><span class="sxs-lookup"><span data-stu-id="69228-200">202</span></span> |<span data-ttu-id="69228-201">Elfogadva</span><span class="sxs-lookup"><span data-stu-id="69228-201">Accepted</span></span> |
| <span data-ttu-id="69228-202">400</span><span class="sxs-lookup"><span data-stu-id="69228-202">400</span></span> |<span data-ttu-id="69228-203">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="69228-203">Bad request</span></span> |
| <span data-ttu-id="69228-204">401</span><span class="sxs-lookup"><span data-stu-id="69228-204">401</span></span> |<span data-ttu-id="69228-205">Nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="69228-205">Unauthorized</span></span> |
| <span data-ttu-id="69228-206">403</span><span class="sxs-lookup"><span data-stu-id="69228-206">403</span></span> |<span data-ttu-id="69228-207">Tiltott</span><span class="sxs-lookup"><span data-stu-id="69228-207">Forbidden</span></span> |
| <span data-ttu-id="69228-208">404</span><span class="sxs-lookup"><span data-stu-id="69228-208">404</span></span> |<span data-ttu-id="69228-209">Nem található</span><span class="sxs-lookup"><span data-stu-id="69228-209">Not Found</span></span> |
| <span data-ttu-id="69228-210">500</span><span class="sxs-lookup"><span data-stu-id="69228-210">500</span></span> |<span data-ttu-id="69228-211">Belső kiszolgálóhiba.</span><span class="sxs-lookup"><span data-stu-id="69228-211">Internal server error.</span></span> <span data-ttu-id="69228-212">Ismeretlen hiba történt.</span><span class="sxs-lookup"><span data-stu-id="69228-212">Unknown error occurred.</span></span> |

- - -
## <a name="next-steps"></a><span data-ttu-id="69228-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69228-213">Next steps</span></span>

* [<span data-ttu-id="69228-214">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="69228-214">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="69228-215">Más összekötők keresése</span><span class="sxs-lookup"><span data-stu-id="69228-215">Find other connectors</span></span>](apis-list.md)