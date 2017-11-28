---
title: "az aszinkron műveletek aaaAzure |} Microsoft Docs"
description: "Ismerteti, hogyan tootrack aszinkron műveletek az Azure-ban."
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
ms.openlocfilehash: b81254196013adf87998eff11a50993efa52d40d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="track-asynchronous-azure-operations"></a><span data-ttu-id="8224b-103">Az Azure aszinkron műveletek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="8224b-103">Track asynchronous Azure operations</span></span>
<span data-ttu-id="8224b-104">Néhány Azure REST művelet aszinkron módon futtatható, mert hello művelet nem fejezhető be gyorsan.</span><span class="sxs-lookup"><span data-stu-id="8224b-104">Some Azure REST operations run asynchronously because hello operation cannot be completed quickly.</span></span> <span data-ttu-id="8224b-105">Ez a témakör ismerteti, hogyan hello válaszul vissza tootrack hello állapotát, a értékek aszinkron műveletek.</span><span class="sxs-lookup"><span data-stu-id="8224b-105">This topic describes how tootrack hello status of asynchronous operations through values returned in hello response.</span></span>  

## <a name="status-codes-for-asynchronous-operations"></a><span data-ttu-id="8224b-106">Az aszinkron műveletek állapotkódjai</span><span class="sxs-lookup"><span data-stu-id="8224b-106">Status codes for asynchronous operations</span></span>
<span data-ttu-id="8224b-107">Egy aszinkron művelet kezdetben adja vissza egy HTTP-állapotkód: a következők:</span><span class="sxs-lookup"><span data-stu-id="8224b-107">An asynchronous operation initially returns an HTTP status code of either:</span></span>

* <span data-ttu-id="8224b-108">201-es (létrehozva)</span><span class="sxs-lookup"><span data-stu-id="8224b-108">201 (Created)</span></span>
* <span data-ttu-id="8224b-109">202 (elfogadható)</span><span class="sxs-lookup"><span data-stu-id="8224b-109">202 (Accepted)</span></span> 

<span data-ttu-id="8224b-110">Amikor hello művelet sikeresen befejeződött, adja vissza, vagy:</span><span class="sxs-lookup"><span data-stu-id="8224b-110">When hello operation successfully completes, it returns either:</span></span>

* <span data-ttu-id="8224b-111">200-AS (OK)</span><span class="sxs-lookup"><span data-stu-id="8224b-111">200 (OK)</span></span>
* <span data-ttu-id="8224b-112">204 (üres)</span><span class="sxs-lookup"><span data-stu-id="8224b-112">204 (No Content)</span></span> 

<span data-ttu-id="8224b-113">Tekintse meg a toohello [REST API-dokumentáció](/rest/api/) toosee hello válaszok állnak végrehajtás alatt hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="8224b-113">Refer toohello [REST API documentation](/rest/api/) toosee hello responses for hello operation you are executing.</span></span> 

## <a name="monitor-status-of-operation"></a><span data-ttu-id="8224b-114">Művelet állapotának figyelése</span><span class="sxs-lookup"><span data-stu-id="8224b-114">Monitor status of operation</span></span>
<span data-ttu-id="8224b-115">hello aszinkron REST műveletek visszatérési fejléc értékei, amely toodetermine hello állapotának hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="8224b-115">hello asynchronous REST operations return header values, which you use toodetermine hello status of hello operation.</span></span> <span data-ttu-id="8224b-116">Három fejléc értékek tooexamine potenciálisan vannak:</span><span class="sxs-lookup"><span data-stu-id="8224b-116">There are potentially three header values tooexamine:</span></span>

* <span data-ttu-id="8224b-117">`Azure-AsyncOperation`-Hello folyamatban lévő hello művelet állapotának ellenőrzése a következő URL-címe</span><span class="sxs-lookup"><span data-stu-id="8224b-117">`Azure-AsyncOperation` - URL for checking hello ongoing status of hello operation.</span></span> <span data-ttu-id="8224b-118">A művelet ezt az értéket adja vissza, ha mindig használjon (helyett helye) informatikai tootrack hello hello művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="8224b-118">If your operation returns this value, always use it (instead of Location) tootrack hello status of hello operation.</span></span>
* <span data-ttu-id="8224b-119">`Location`-URL-cím meghatározásához, ha egy művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8224b-119">`Location` - URL for determining when an operation has completed.</span></span> <span data-ttu-id="8224b-120">Használja ezt az értéket csak akkor, ha Azure-aszinkron műveletek nem ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8224b-120">Use this value only when Azure-AsyncOperation is not returned.</span></span>
* <span data-ttu-id="8224b-121">`Retry-After`-hello másodperc toowait száma hello aszinkron művelet hello állapotának ellenőrzése előtt.</span><span class="sxs-lookup"><span data-stu-id="8224b-121">`Retry-After` - hello number of seconds toowait before checking hello status of hello asynchronous operation.</span></span>

<span data-ttu-id="8224b-122">Nem minden aszinkron művelethez azonban ezeket az értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8224b-122">However, not every asynchronous operation returns all these values.</span></span> <span data-ttu-id="8224b-123">Például szükség lehet tooevaluate hello Azure-aszinkron műveletek Fejlécérték egy műveletet, és hello hely állomásfejléc-érték egy másik művelet.</span><span class="sxs-lookup"><span data-stu-id="8224b-123">For example, you may need tooevaluate hello Azure-AsyncOperation header value for one operation, and hello Location header value for another operation.</span></span> 

<span data-ttu-id="8224b-124">Hello térközkaraktert Ön olvashatók be, mert a szeretné beolvasni a bármely állomásfejléc-érték, egy kérelem.</span><span class="sxs-lookup"><span data-stu-id="8224b-124">You retrieve hello header values as you would retrieve any header value for a request.</span></span> <span data-ttu-id="8224b-125">Például a C#, visszaállíthatja az hello Fejlécérték egy `HttpWebResponse` nevű objektum `response` a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="8224b-125">For example, in C#, you retrieve hello header value from an `HttpWebResponse` object named `response` with hello following code:</span></span>

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a><span data-ttu-id="8224b-126">Azure-aszinkron műveletek kérelem-válasz</span><span class="sxs-lookup"><span data-stu-id="8224b-126">Azure-AsyncOperation request and response</span></span>

<span data-ttu-id="8224b-127">hello aszinkron művelet, tooget hello állapotát GET kérelem toohello URL-cím küldése az Azure-aszinkron műveletek állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="8224b-127">tooget hello status of hello asynchronous operation, send a GET request toohello URL in Azure-AsyncOperation header value.</span></span>

<span data-ttu-id="8224b-128">hello művelet hello válasz törzsében hello műveletekre vonatkozó információk.</span><span class="sxs-lookup"><span data-stu-id="8224b-128">hello body of hello response from this operation contains information about hello operation.</span></span> <span data-ttu-id="8224b-129">hello alábbi példa bemutatja hello hello művelet által visszaadott a lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="8224b-129">hello following example shows hello possible values returned from hello operation:</span></span>

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

<span data-ttu-id="8224b-130">Csak `status` az összes választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8224b-130">Only `status` is returned for all responses.</span></span> <span data-ttu-id="8224b-131">hello hiba objektum ad vissza, ha hello állapota sikertelen vagy megszakítva.</span><span class="sxs-lookup"><span data-stu-id="8224b-131">hello error object is returned when hello status is Failed or Canceled.</span></span> <span data-ttu-id="8224b-132">Minden más értékek a következők kötelező megadni. ezért hello választ kap megjelenése eltérő lehet mint hello példa.</span><span class="sxs-lookup"><span data-stu-id="8224b-132">All other values are optional; therefore, hello response you receive may look different than hello example.</span></span>

## <a name="provisioningstate-values"></a><span data-ttu-id="8224b-133">provisioningState értékek</span><span class="sxs-lookup"><span data-stu-id="8224b-133">provisioningState values</span></span>

<span data-ttu-id="8224b-134">Műveletek létrehozása, frissítése vagy törlése (PUT, javítás, Törlés) erőforrás általában vissza egy `provisioningState` érték.</span><span class="sxs-lookup"><span data-stu-id="8224b-134">Operations that create, update, or delete (PUT, PATCH, DELETE) a resource typically return a `provisioningState` value.</span></span> <span data-ttu-id="8224b-135">Egy művelet befejezése után az alábbi értékek egyike vissza:</span><span class="sxs-lookup"><span data-stu-id="8224b-135">When an operation has completed, one of following three values is returned:</span></span> 

* <span data-ttu-id="8224b-136">Sikeres</span><span class="sxs-lookup"><span data-stu-id="8224b-136">Succeeded</span></span>
* <span data-ttu-id="8224b-137">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="8224b-137">Failed</span></span>
* <span data-ttu-id="8224b-138">Törölve</span><span class="sxs-lookup"><span data-stu-id="8224b-138">Canceled</span></span>

<span data-ttu-id="8224b-139">Minden más értékek azt jelzik, hogy továbbra is fut hello művelet.</span><span class="sxs-lookup"><span data-stu-id="8224b-139">All other values indicate hello operation is still running.</span></span> <span data-ttu-id="8224b-140">hello erőforrás-szolgáltató az állapotát jelző testreszabott értéket adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="8224b-140">hello resource provider can return a customized value that indicates its state.</span></span> <span data-ttu-id="8224b-141">Például jelenhet meg **elfogadott** fogadott és futó hello kérelem esetén.</span><span class="sxs-lookup"><span data-stu-id="8224b-141">For example, you may receive **Accepted** when hello request is received and running.</span></span>

## <a name="example-requests-and-responses"></a><span data-ttu-id="8224b-142">Példa kérések és válaszok</span><span class="sxs-lookup"><span data-stu-id="8224b-142">Example requests and responses</span></span>

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a><span data-ttu-id="8224b-143">Indítsa el a virtuális gép (az Azure-aszinkron műveletek 202)</span><span class="sxs-lookup"><span data-stu-id="8224b-143">Start virtual machine (202 with Azure-AsyncOperation)</span></span>
<span data-ttu-id="8224b-144">Ez a példa bemutatja, hogyan toodetermine hello állapotának **start** műveletet a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="8224b-144">This example shows how toodetermine hello status of **start** operation for virtual machines.</span></span> <span data-ttu-id="8224b-145">hello kezdeti kérés hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="8224b-145">hello initial request is in hello following format:</span></span>

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

<span data-ttu-id="8224b-146">Állapotkód: 202 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8224b-146">It returns status code 202.</span></span> <span data-ttu-id="8224b-147">Hello fejléc értékei, közötti jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8224b-147">Among hello header values, you see:</span></span>

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="8224b-148">hello aszinkron művelet, egy másik kérelem toothat URL-cím küldésekor toocheck hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="8224b-148">toocheck hello status of hello asynchronous operation, sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

<span data-ttu-id="8224b-149">hello adott válasz törzsének hello művelet hello állapotának tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8224b-149">hello response body contains hello status of hello operation:</span></span>

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a><span data-ttu-id="8224b-150">Erőforrások (az Azure-aszinkron műveletek 201) telepítése</span><span class="sxs-lookup"><span data-stu-id="8224b-150">Deploy resources (201 with Azure-AsyncOperation)</span></span>

<span data-ttu-id="8224b-151">Ez a példa bemutatja, hogyan toodetermine hello állapotának **központi telepítések** művelet erőforrások tooAzure telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8224b-151">This example shows how toodetermine hello status of **deployments** operation for deploying resources tooAzure.</span></span> <span data-ttu-id="8224b-152">hello kezdeti kérés hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="8224b-152">hello initial request is in hello following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

<span data-ttu-id="8224b-153">201-es állapotkód adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8224b-153">It returns status code 201.</span></span> <span data-ttu-id="8224b-154">hello adott válasz törzsének hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8224b-154">hello body of hello response includes:</span></span>

```json
"provisioningState":"Accepted",
```

<span data-ttu-id="8224b-155">Hello fejléc értékei, közötti jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8224b-155">Among hello header values, you see:</span></span>

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="8224b-156">hello aszinkron művelet, egy másik kérelem toothat URL-cím küldésekor toocheck hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="8224b-156">toocheck hello status of hello asynchronous operation, sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

<span data-ttu-id="8224b-157">hello adott válasz törzsének hello művelet hello állapotának tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8224b-157">hello response body contains hello status of hello operation:</span></span>

```json
{"status":"Running"}
```

<span data-ttu-id="8224b-158">Ha hello telepítés befejeződött, hello válasz tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8224b-158">When hello deployment is finished, hello response contains:</span></span>

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a><span data-ttu-id="8224b-159">(A hely és újrapróbálkozási után 202) storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8224b-159">Create storage account (202 with Location and Retry-After)</span></span>

<span data-ttu-id="8224b-160">Ez a példa bemutatja, hogyan toodetermine hello hello állapotának **létrehozása** storage-fiókok műveletet.</span><span class="sxs-lookup"><span data-stu-id="8224b-160">This example shows how toodetermine hello status of hello **create** operation for storage accounts.</span></span> <span data-ttu-id="8224b-161">hello kezdeti kérés hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="8224b-161">hello initial request is in hello following format:</span></span>

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

<span data-ttu-id="8224b-162">És hello kérelemtörzset hello tárfiók tulajdonságait tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8224b-162">And hello request body contains properties for hello storage account:</span></span>

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

<span data-ttu-id="8224b-163">Állapotkód: 202 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8224b-163">It returns status code 202.</span></span> <span data-ttu-id="8224b-164">Között hello fejléc értékei tekintse meg a következő két érték hello:</span><span class="sxs-lookup"><span data-stu-id="8224b-164">Among hello header values, you see hello following two values:</span></span>

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

<span data-ttu-id="8224b-165">Száma várakozás után másodpercben megadott újrapróbálkozási után, állapotának hello hello aszinkron művelet úgy, hogy küld egy másik kérelem toothat URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="8224b-165">After waiting for number of seconds specified in Retry-After, check hello status of hello asynchronous operation by sending another request toothat URL.</span></span>

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

<span data-ttu-id="8224b-166">Ha hello kérés továbbra is fut, a állapotkód: 202 jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8224b-166">If hello request is still running, you receive a status code 202.</span></span> <span data-ttu-id="8224b-167">Ha hello kérelem befejezése után a 200-as állapotkód kap, és hello hello válasz törzsében hello tulajdonságok hello tárfiók lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="8224b-167">If hello request has completed, your receive a status code 200, and hello body of hello response contains hello properties of hello storage account that has been created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8224b-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8224b-168">Next steps</span></span>

* <span data-ttu-id="8224b-169">Az egyes REST műveletekre vonatkozó dokumentációjáért lásd: [REST API-dokumentáció](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="8224b-169">For documentation about each REST operation, see [REST API documentation](/rest/api/).</span></span>
* <span data-ttu-id="8224b-170">Erőforrások hello Resource Manager REST API-n keresztül kezelésével kapcsolatos információkért lásd: [Resource Manager REST API használatával hello](resource-manager-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="8224b-170">For information about managing resources through hello Resource Manager REST API, see [Using hello Resource Manager REST API](resource-manager-rest-api.md).</span></span>
* <span data-ttu-id="8224b-171">hello Resource Manager REST API-n keresztül sablonok telepítésével kapcsolatos információkért lásd: [központi telepítése a Resource Manager-sablonok és a Resource Manager REST API erőforrások](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="8224b-171">for information about deploying templates through hello Resource Manager REST API, see [Deploy resources with Resource Manager templates and Resource Manager REST API](resource-group-template-deploy-rest.md).</span></span>
