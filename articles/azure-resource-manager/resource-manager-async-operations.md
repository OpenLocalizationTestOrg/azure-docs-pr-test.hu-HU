---
title: "Az Azure aszinkron műveletek |} Microsoft Docs"
description: "Nyomon követheti az Azure-ban aszinkron műveleteket ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: 9fe3d98cd345aae45722295b6c1b7fc3e9036e95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="track-asynchronous-azure-operations"></a><span data-ttu-id="2a2cf-103">Az Azure aszinkron műveletek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="2a2cf-103">Track asynchronous Azure operations</span></span>
<span data-ttu-id="2a2cf-104">Néhány Azure REST művelet aszinkron módon futtatható, mert a művelet nem fejezhető be gyorsan.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-104">Some Azure REST operations run asynchronously because the operation cannot be completed quickly.</span></span> <span data-ttu-id="2a2cf-105">Ez a témakör ismerteti a válaszban visszaadott értékek keresztül aszinkron műveletek állapotának nyomon követését.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-105">This topic describes how to track the status of asynchronous operations through values returned in the response.</span></span>  

## <a name="status-codes-for-asynchronous-operations"></a><span data-ttu-id="2a2cf-106">Az aszinkron műveletek állapotkódjai</span><span class="sxs-lookup"><span data-stu-id="2a2cf-106">Status codes for asynchronous operations</span></span>
<span data-ttu-id="2a2cf-107">Egy aszinkron művelet kezdetben adja vissza egy HTTP-állapotkód: a következők:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-107">An asynchronous operation initially returns an HTTP status code of either:</span></span>

* <span data-ttu-id="2a2cf-108">201-es (létrehozva)</span><span class="sxs-lookup"><span data-stu-id="2a2cf-108">201 (Created)</span></span>
* <span data-ttu-id="2a2cf-109">202 (elfogadható)</span><span class="sxs-lookup"><span data-stu-id="2a2cf-109">202 (Accepted)</span></span> 

<span data-ttu-id="2a2cf-110">Ha a művelet sikeresen befejeződött, adja vissza, vagy:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-110">When the operation successfully completes, it returns either:</span></span>

* <span data-ttu-id="2a2cf-111">200-AS (OK)</span><span class="sxs-lookup"><span data-stu-id="2a2cf-111">200 (OK)</span></span>
* <span data-ttu-id="2a2cf-112">204 (üres)</span><span class="sxs-lookup"><span data-stu-id="2a2cf-112">204 (No Content)</span></span> 

<span data-ttu-id="2a2cf-113">Tekintse meg a [REST API-dokumentáció](/rest/api/) tekintheti meg a műveletet, amelyek végrehajtása a válaszokat.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-113">Refer to the [REST API documentation](/rest/api/) to see the responses for the operation you are executing.</span></span> 

## <a name="monitor-status-of-operation"></a><span data-ttu-id="2a2cf-114">Művelet állapotának figyelése</span><span class="sxs-lookup"><span data-stu-id="2a2cf-114">Monitor status of operation</span></span>
<span data-ttu-id="2a2cf-115">Az aszinkron REST műveleteinek térjen vissza a fejléc értékei, amelyek segítségével állapítja meg, a művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-115">The asynchronous REST operations return header values, which you use to determine the status of the operation.</span></span> <span data-ttu-id="2a2cf-116">Nincsenek potenciálisan három térközkaraktert vizsgálata:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-116">There are potentially three header values to examine:</span></span>

* <span data-ttu-id="2a2cf-117">`Azure-AsyncOperation`-A művelet folyamatban lévő állapotának ellenőrzése a következő URL-címe</span><span class="sxs-lookup"><span data-stu-id="2a2cf-117">`Azure-AsyncOperation` - URL for checking the ongoing status of the operation.</span></span> <span data-ttu-id="2a2cf-118">A művelet ezt az értéket adja vissza, ha mindig segítségével (hely) helyett a művelet állapotának nyomon követését.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-118">If your operation returns this value, always use it (instead of Location) to track the status of the operation.</span></span>
* <span data-ttu-id="2a2cf-119">`Location`-URL-cím meghatározásához, ha egy művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-119">`Location` - URL for determining when an operation has completed.</span></span> <span data-ttu-id="2a2cf-120">Használja ezt az értéket csak akkor, ha Azure-aszinkron műveletek nem ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-120">Use this value only when Azure-AsyncOperation is not returned.</span></span>
* <span data-ttu-id="2a2cf-121">`Retry-After`-Hány másodpercig várjon az aszinkron művelet állapotának ellenőrzésekor.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-121">`Retry-After` - The number of seconds to wait before checking the status of the asynchronous operation.</span></span>

<span data-ttu-id="2a2cf-122">Nem minden aszinkron művelethez azonban ezeket az értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-122">However, not every asynchronous operation returns all these values.</span></span> <span data-ttu-id="2a2cf-123">Például szükség lehet az Azure-aszinkron műveletek Fejlécérték egy művelet, és a hely fejléc értékének egy másik művelet.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-123">For example, you may need to evaluate the Azure-AsyncOperation header value for one operation, and the Location header value for another operation.</span></span> 

<span data-ttu-id="2a2cf-124">A fejléc értékei Ön olvashatók be, mert a szeretné beolvasni a bármely állomásfejléc-érték, egy kérelemre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-124">You retrieve the header values as you would retrieve any header value for a request.</span></span> <span data-ttu-id="2a2cf-125">Például a C#, visszaállíthatja a Fejlécérték egy `HttpWebResponse` nevű objektum `response` az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-125">For example, in C#, you retrieve the header value from an `HttpWebResponse` object named `response` with the following code:</span></span>

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a><span data-ttu-id="2a2cf-126">Azure-aszinkron műveletek kérelem-válasz</span><span class="sxs-lookup"><span data-stu-id="2a2cf-126">Azure-AsyncOperation request and response</span></span>

<span data-ttu-id="2a2cf-127">Ahhoz, hogy az aszinkron művelet állapotát, GET kérés küldése az Azure-aszinkron műveletek állomásfejléc-érték az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-127">To get the status of the asynchronous operation, send a GET request to the URL in Azure-AsyncOperation header value.</span></span>

<span data-ttu-id="2a2cf-128">Ez a művelet kapott válasz törzsében működésével kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-128">The body of the response from this operation contains information about the operation.</span></span> <span data-ttu-id="2a2cf-129">A következő példa bemutatja a művelet által visszaadott a lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-129">The following example shows the possible values returned from the operation:</span></span>

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}", 
    "status" : "Succeeded | Failed | Canceled | {resource provider values}", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : { 
        "code": "{error code}",  
        "message": "{error description}" 
    }
}
```

<span data-ttu-id="2a2cf-130">Csak `status` az összes választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-130">Only `status` is returned for all responses.</span></span> <span data-ttu-id="2a2cf-131">A hiba objektum ad vissza, ha az állapot sikertelen vagy megszakítva.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-131">The error object is returned when the status is Failed or Canceled.</span></span> <span data-ttu-id="2a2cf-132">Minden más értékek a következők kötelező megadni. ezért a választ kap láthatótól tűnhet.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-132">All other values are optional; therefore, the response you receive may look different than the example.</span></span>

## <a name="provisioningstate-values"></a><span data-ttu-id="2a2cf-133">provisioningState értékek</span><span class="sxs-lookup"><span data-stu-id="2a2cf-133">provisioningState values</span></span>

<span data-ttu-id="2a2cf-134">Műveletek létrehozása, frissítése vagy törlése (PUT, javítás, Törlés) erőforrás általában vissza egy `provisioningState` érték.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-134">Operations that create, update, or delete (PUT, PATCH, DELETE) a resource typically return a `provisioningState` value.</span></span> <span data-ttu-id="2a2cf-135">Egy művelet befejezése után az alábbi értékek egyike vissza:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-135">When an operation has completed, one of following three values is returned:</span></span> 

* <span data-ttu-id="2a2cf-136">Sikeres</span><span class="sxs-lookup"><span data-stu-id="2a2cf-136">Succeeded</span></span>
* <span data-ttu-id="2a2cf-137">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="2a2cf-137">Failed</span></span>
* <span data-ttu-id="2a2cf-138">Törölve</span><span class="sxs-lookup"><span data-stu-id="2a2cf-138">Canceled</span></span>

<span data-ttu-id="2a2cf-139">Minden egyéb értékek azt jelzik, hogy még mindig fut, a műveletet.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-139">All other values indicate the operation is still running.</span></span> <span data-ttu-id="2a2cf-140">Az erőforrás-szolgáltató az állapotát jelző testreszabott értéket adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-140">The resource provider can return a customized value that indicates its state.</span></span> <span data-ttu-id="2a2cf-141">Például jelenhet meg **elfogadott** fogadott és megfelelően fut a kérelem esetén.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-141">For example, you may receive **Accepted** when the request is received and running.</span></span>

## <a name="example-requests-and-responses"></a><span data-ttu-id="2a2cf-142">Példa kérések és válaszok</span><span class="sxs-lookup"><span data-stu-id="2a2cf-142">Example requests and responses</span></span>

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a><span data-ttu-id="2a2cf-143">Indítsa el a virtuális gép (az Azure-aszinkron műveletek 202)</span><span class="sxs-lookup"><span data-stu-id="2a2cf-143">Start virtual machine (202 with Azure-AsyncOperation)</span></span>
<span data-ttu-id="2a2cf-144">Ez a példa bemutatja, hogyan állapotának megállapítása **start** műveletet a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-144">This example shows how to determine the status of **start** operation for virtual machines.</span></span> <span data-ttu-id="2a2cf-145">A kezdeti kérelme, mert a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-145">The initial request is in the following format:</span></span>

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

<span data-ttu-id="2a2cf-146">Állapotkód: 202 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-146">It returns status code 202.</span></span> <span data-ttu-id="2a2cf-147">A fejléc értékei között jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-147">Among the header values, you see:</span></span>

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="2a2cf-148">Az aszinkron művelet, egy másik kérelem küldése adott URL-cím állapotának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-148">To check the status of the asynchronous operation, sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="2a2cf-149">Az adott válasz törzsének tartalmaz a művelet állapotát:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-149">The response body contains the status of the operation:</span></span>

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a><span data-ttu-id="2a2cf-150">Erőforrások (az Azure-aszinkron műveletek 201) telepítése</span><span class="sxs-lookup"><span data-stu-id="2a2cf-150">Deploy resources (201 with Azure-AsyncOperation)</span></span>

<span data-ttu-id="2a2cf-151">Ez a példa bemutatja, hogyan állapotának megállapítása **központi telepítések** művelet erőforrásokat üzembe helyezi az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-151">This example shows how to determine the status of **deployments** operation for deploying resources to Azure.</span></span> <span data-ttu-id="2a2cf-152">A kezdeti kérelme, mert a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-152">The initial request is in the following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

<span data-ttu-id="2a2cf-153">201-es állapotkód adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-153">It returns status code 201.</span></span> <span data-ttu-id="2a2cf-154">A választörzs tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-154">The body of the response includes:</span></span>

```json
"provisioningState":"Accepted",
```

<span data-ttu-id="2a2cf-155">A fejléc értékei között jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-155">Among the header values, you see:</span></span>

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="2a2cf-156">Az aszinkron művelet, egy másik kérelem küldése adott URL-cím állapotának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-156">To check the status of the asynchronous operation, sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="2a2cf-157">Az adott válasz törzsének tartalmaz a művelet állapotát:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-157">The response body contains the status of the operation:</span></span>

```json
{"status":"Running"}
```

<span data-ttu-id="2a2cf-158">Ha a telepítés befejeződött, a válasz tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-158">When the deployment is finished, the response contains:</span></span>

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a><span data-ttu-id="2a2cf-159">(A hely és újrapróbálkozási után 202) storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a2cf-159">Create storage account (202 with Location and Retry-After)</span></span>

<span data-ttu-id="2a2cf-160">Ez a példa bemutatja, hogyan állapotának megállapítása a **létrehozása** storage-fiókok műveletet.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-160">This example shows how to determine the status of the **create** operation for storage accounts.</span></span> <span data-ttu-id="2a2cf-161">A kezdeti kérelme, mert a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-161">The initial request is in the following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

<span data-ttu-id="2a2cf-162">És a kérés törzsében tartalmazza a tárfiók tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-162">And the request body contains properties for the storage account:</span></span>

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

<span data-ttu-id="2a2cf-163">Állapotkód: 202 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-163">It returns status code 202.</span></span> <span data-ttu-id="2a2cf-164">A fejléc értékei között a következő két érték jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2a2cf-164">Among the header values, you see the following two values:</span></span>

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

<span data-ttu-id="2a2cf-165">Miután Várakozás száma a másodpercben megadott újrapróbálkozási után ellenőrizze az aszinkron művelet állapotát úgy, hogy küld egy másik kérelem URL-címet.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-165">After waiting for number of seconds specified in Retry-After, check the status of the asynchronous operation by sending another request to that URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

<span data-ttu-id="2a2cf-166">Ha a kérés továbbra is fut, a állapotkód: 202 jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-166">If the request is still running, you receive a status code 202.</span></span> <span data-ttu-id="2a2cf-167">Ha a kérelem befejeződött, a 200-as állapotkód kap, és a választörzs a tárfiók már létrehozott tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2a2cf-167">If the request has completed, your receive a status code 200, and the body of the response contains the properties of the storage account that has been created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a2cf-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a2cf-168">Next steps</span></span>

* <span data-ttu-id="2a2cf-169">Az egyes REST műveletekre vonatkozó dokumentációjáért lásd: [REST API-dokumentáció](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="2a2cf-169">For documentation about each REST operation, see [REST API documentation](/rest/api/).</span></span>
* <span data-ttu-id="2a2cf-170">A Resource Manager REST API-n keresztül erőforrások kezelésével kapcsolatos információkért lásd: [a Resource Manager REST API használatával](resource-manager-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="2a2cf-170">For information about managing resources through the Resource Manager REST API, see [Using the Resource Manager REST API](resource-manager-rest-api.md).</span></span>
* <span data-ttu-id="2a2cf-171">a Resource Manager REST API-n keresztül sablonok telepítésével kapcsolatos információkért lásd: [központi telepítése a Resource Manager-sablonok és a Resource Manager REST API erőforrások](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="2a2cf-171">for information about deploying templates through the Resource Manager REST API, see [Deploy resources with Resource Manager templates and Resource Manager REST API](resource-group-template-deploy-rest.md).</span></span>