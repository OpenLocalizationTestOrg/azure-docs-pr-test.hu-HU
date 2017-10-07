---
title: "aaaAzure erőforrás-kezelő kérelmekre vonatkozó korlátokat |} Microsoft Docs"
description: "Ismerteti, hogyan kérelmezi a toouse az Azure Resource Manager-szabályozás, ha a rendszer elérte az előfizetési korlátozásait."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: ea274f3145af36aac753730e1f280d8a8b59c3bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-resource-manager-requests"></a>Erőforrás-kezelő szabályozás
Minden egyes előfizetésekhez és bérlői erőforrás-kezelő korlátok kérelmek too15, óránként 000 és olvasási kérések too1, óránként 200. Ezek a korlátozások alkalmazásához tooeach Azure Resource Manager-példány; több példánya van minden Azure-régiót, és Azure Resource Manager üzembe helyezett tooall Azure régióban.  Így a, a gyakorlatban korlátok hatékonyan sokkal nagyobb, mint a fent felsorolt, felhasználóként kérelmek általában által kiszolgált számos különböző példányai.

Ha az alkalmazás vagy a parancsfájl eléri a működés felső korlátjának, akkor toothrottle a kérelmek. Ez a témakör bemutatja, hogyan fennmaradó toodetermine hello kérelmek hello korlát elérése előtt van, és hogyan toorespond, amikor elérte hello vonatkozó korlátozást.

Hello korlát elérésekor kapni hello HTTP-állapotkód: **429-es jelű túl sok kérelem**.

kérelem hello hatókörön belüli tooeither van, az előfizetés vagy a bérlőt. Ha több, egyidejű alkalmazások az előfizetéséhez, a kérelmet benyújtó hello kérelmeinek azon alkalmazások felvétele együtt fennmaradó kérelmek toodetermine hello száma.

Hatókörű előfizetési kérelmek azok hello tartalmaz, amely átadja az előfizetés-azonosítóval, például az előfizetés hello erőforráscsoportok lekérése során. Bérlői hatókörű kérelmek nem tartalmaznak az előfizetés-azonosítóval, mint érvényes Azure helyek lekérése.

## <a name="remaining-requests"></a>Fennmaradó kérelmek
Válaszfejlécek megvizsgálásával azt is meghatározhatja hello fennmaradó kérések száma. Minden egyes kérelem fennmaradó olvasási és írási kérések száma hello értékeket tartalmaz. hello következő táblázatban láthatja, hogy ezeket az értékeket a hello válaszfejlécek.

| Válaszfejléc | Leírás |
| --- | --- |
| x-MS-ratelimit-remaining-Subscription-reads |Előfizetés hatókörű beolvassa a fennmaradó |
| x-MS-ratelimit-remaining-Subscription-writes |Előfizetés hatókörű ír fennmaradó |
| x-MS-ratelimit-remaining-tenant-reads |Bérlői hatókörű beolvassa a fennmaradó |
| x-MS-ratelimit-remaining-tenant-writes |Bérlői hatókörű ír fennmaradó |
| x-MS-ratelimit-remaining-Subscription-Resource-Requests |Előfizetés fennmaradó erőforrás típusú kérések hatókörét.<br /><br />Ezt a fejlécértéket csak akkor ad vissza, ha a szolgáltatás felülírta hello alapértelmezett korlát. Erőforrás-kezelő hozzáadása hello előfizetés olvasások és írások helyett ezt az értéket. |
| x-MS-ratelimit-remaining-Subscription-Resource-entities-Read |Előfizetés az erőforrás típusa adatgyűjtési kérelmek fennmaradó hatókörét.<br /><br />Ezt a fejlécértéket csak akkor ad vissza, ha a szolgáltatás felülírta hello alapértelmezett korlát. Ez az érték számos hello fennmaradó adatgyűjtési kérelmek (lista erőforrások). |
| x-MS-ratelimit-remaining-tenant-Resource-Requests |Bérlői erőforrás típusú kérések fennmaradó hatókörét.<br /><br />A bérlői szintű kérelmek csak hozzáadja az ezt a fejlécet, és csak akkor, ha a szolgáltatás felülírta hello alapértelmezett korlát. Erőforrás-kezelő hozzáadása hello bérlői olvasások és írások helyett ezt az értéket. |
| x-MS-ratelimit-remaining-tenant-Resource-entities-Read |Bérlői erőforrás típusa adatgyűjtési kérelmek fennmaradó hatókörét.<br /><br />A bérlői szintű kérelmek csak hozzáadja az ezt a fejlécet, és csak akkor, ha a szolgáltatás felülírta hello alapértelmezett korlát. |

## <a name="retrieving-hello-header-values"></a>Hello térközkaraktert beolvasása
A fejléc értékei a kód vagy parancsfájl beolvasása ugyanolyan helyzetet teremt, mint bármely fejléc értékének beolvasása. 

Például a **C#**, visszaállíthatja az hello Fejlécérték egy **HttpWebResponse** nevű objektum **válasz** a következő kód hello:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

A **PowerShell**, hello állomásfejléc-érték lekérése az Invoke-WebRequest műveletet.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Vagy, ha szeretné, hogy toosee hello fennmaradó vonatkozó kérések hibakeresés, megadhatja a hello **-Debug** paraméter a **PowerShell** parancsmag.

```powershell
Get-AzureRmResourceGroup -Debug
```

Többek között a következő válasz érték hello sok értéket visszaadó:

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

A **Azure CLI**, visszaállíthatja az hello állomásfejléc-érték használatával hello részletesebb lehetőséget.

```azurecli
azure group list -vv --json
```

Több értékhez, beleértve a következő objektum hello visszaadó:

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a>Várakozás a következő kérelem elküldése előtt
Hello kérelmi korlátjának elérésekor az erőforrás-kezelő adja vissza hello **429** HTTP-állapotkód és egy **újrapróbálkozási után** hello fejléc értéke. Hello **újrapróbálkozási után** érték hello következő kérelem elküldése előtt megvárja-e az alkalmazás másodperc (vagy alvó) hello számát adja meg. Hello Retry paraméter értéke letelte előtt kérelmet küld, ha a kérelem nincs feldolgozva, és próbálkozzon újra új értéket ad vissza.

## <a name="next-steps"></a>Következő lépések

* Korlátozásai és a kvóták kapcsolatos további információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).
* toolearn aszinkron REST kérések kezelésével kapcsolatban lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).
