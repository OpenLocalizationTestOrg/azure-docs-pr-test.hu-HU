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
# <a name="track-asynchronous-azure-operations"></a>Az Azure aszinkron műveletek nyomon követése
Néhány Azure REST művelet aszinkron módon futtatható, mert hello művelet nem fejezhető be gyorsan. Ez a témakör ismerteti, hogyan hello válaszul vissza tootrack hello állapotát, a értékek aszinkron műveletek.  

## <a name="status-codes-for-asynchronous-operations"></a>Az aszinkron műveletek állapotkódjai
Egy aszinkron művelet kezdetben adja vissza egy HTTP-állapotkód: a következők:

* 201-es (létrehozva)
* 202 (elfogadható) 

Amikor hello művelet sikeresen befejeződött, adja vissza, vagy:

* 200-AS (OK)
* 204 (üres) 

Tekintse meg a toohello [REST API-dokumentáció](/rest/api/) toosee hello válaszok állnak végrehajtás alatt hello a művelethez. 

## <a name="monitor-status-of-operation"></a>Művelet állapotának figyelése
hello aszinkron REST műveletek visszatérési fejléc értékei, amely toodetermine hello állapotának hello a művelethez. Három fejléc értékek tooexamine potenciálisan vannak:

* `Azure-AsyncOperation`-Hello folyamatban lévő hello művelet állapotának ellenőrzése a következő URL-címe A művelet ezt az értéket adja vissza, ha mindig használjon (helyett helye) informatikai tootrack hello hello művelet állapotát.
* `Location`-URL-cím meghatározásához, ha egy művelet befejeződött. Használja ezt az értéket csak akkor, ha Azure-aszinkron műveletek nem ad vissza.
* `Retry-After`-hello másodperc toowait száma hello aszinkron művelet hello állapotának ellenőrzése előtt.

Nem minden aszinkron művelethez azonban ezeket az értékeket adja vissza. Például szükség lehet tooevaluate hello Azure-aszinkron műveletek Fejlécérték egy műveletet, és hello hely állomásfejléc-érték egy másik művelet. 

Hello térközkaraktert Ön olvashatók be, mert a szeretné beolvasni a bármely állomásfejléc-érték, egy kérelem. Például a C#, visszaállíthatja az hello Fejlécérték egy `HttpWebResponse` nevű objektum `response` a hello a következő kódot:

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a>Azure-aszinkron műveletek kérelem-válasz

hello aszinkron művelet, tooget hello állapotát GET kérelem toohello URL-cím küldése az Azure-aszinkron műveletek állomásfejléc-érték.

hello művelet hello válasz törzsében hello műveletekre vonatkozó információk. hello alábbi példa bemutatja hello hello művelet által visszaadott a lehetséges értékek:

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

Csak `status` az összes választ ad vissza. hello hiba objektum ad vissza, ha hello állapota sikertelen vagy megszakítva. Minden más értékek a következők kötelező megadni. ezért hello választ kap megjelenése eltérő lehet mint hello példa.

## <a name="provisioningstate-values"></a>provisioningState értékek

Műveletek létrehozása, frissítése vagy törlése (PUT, javítás, Törlés) erőforrás általában vissza egy `provisioningState` érték. Egy művelet befejezése után az alábbi értékek egyike vissza: 

* Sikeres
* Nem sikerült
* Törölve

Minden más értékek azt jelzik, hogy továbbra is fut hello művelet. hello erőforrás-szolgáltató az állapotát jelző testreszabott értéket adhat vissza. Például jelenhet meg **elfogadott** fogadott és futó hello kérelem esetén.

## <a name="example-requests-and-responses"></a>Példa kérések és válaszok

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Indítsa el a virtuális gép (az Azure-aszinkron műveletek 202)
Ez a példa bemutatja, hogyan toodetermine hello állapotának **start** műveletet a virtuális gépek. hello kezdeti kérés hello a következő formátumban kell megadni:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

Állapotkód: 202 adja vissza. Hello fejléc értékei, közötti jelenik meg:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

hello aszinkron művelet, egy másik kérelem toothat URL-cím küldésekor toocheck hello állapotát.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

hello adott válasz törzsének hello művelet hello állapotának tartalmazza:

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Erőforrások (az Azure-aszinkron műveletek 201) telepítése

Ez a példa bemutatja, hogyan toodetermine hello állapotának **központi telepítések** művelet erőforrások tooAzure telepítéséhez. hello kezdeti kérés hello a következő formátumban kell megadni:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

201-es állapotkód adja vissza. hello adott válasz törzsének hello tartalmazza:

```json
"provisioningState":"Accepted",
```

Hello fejléc értékei, közötti jelenik meg:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

hello aszinkron művelet, egy másik kérelem toothat URL-cím küldésekor toocheck hello állapotát.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

hello adott válasz törzsének hello művelet hello állapotának tartalmazza:

```json
{"status":"Running"}
```

Ha hello telepítés befejeződött, hello válasz tartalmazza:

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a>(A hely és újrapróbálkozási után 202) storage-fiók létrehozása

Ez a példa bemutatja, hogyan toodetermine hello hello állapotának **létrehozása** storage-fiókok műveletet. hello kezdeti kérés hello a következő formátumban kell megadni:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

És hello kérelemtörzset hello tárfiók tulajdonságait tartalmazza:

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

Állapotkód: 202 adja vissza. Között hello fejléc értékei tekintse meg a következő két érték hello:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

Száma várakozás után másodpercben megadott újrapróbálkozási után, állapotának hello hello aszinkron művelet úgy, hogy küld egy másik kérelem toothat URL-CÍMÉT.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

Ha hello kérés továbbra is fut, a állapotkód: 202 jelenik meg. Ha hello kérelem befejezése után a 200-as állapotkód kap, és hello hello válasz törzsében hello tulajdonságok hello tárfiók lett létrehozva.

## <a name="next-steps"></a>Következő lépések

* Az egyes REST műveletekre vonatkozó dokumentációjáért lásd: [REST API-dokumentáció](/rest/api/).
* Erőforrások hello Resource Manager REST API-n keresztül kezelésével kapcsolatos információkért lásd: [Resource Manager REST API használatával hello](resource-manager-rest-api.md).
* hello Resource Manager REST API-n keresztül sablonok telepítésével kapcsolatos információkért lásd: [központi telepítése a Resource Manager-sablonok és a Resource Manager REST API erőforrások](resource-group-template-deploy-rest.md).
