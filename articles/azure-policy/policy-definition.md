---
title: "Házirend-definíció Azure szerkezetet |} Microsoft Docs"
description: "Ismerteti, hogyan erőforrás házirend-definíció Azure házirend létrehozásához használt erőforrásokra vonatkozó konvenciók a szervezet ismertetésével, ha a házirend érvényesítve van-e, és a végrehajtandó műveletet."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/17/2018
ms.topic: article
ms.service: azure-policy
ms.custom: 
ms.openlocfilehash: af373e2770ad020b3a3eb669424c001670ec9204
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/18/2018
---
# <a name="azure-policy-definition-structure"></a>Azure szabályzatdefiníciók struktúrája

Erőforrás házirend-definíció Azure házirend által használt lehetővé teszi a szervezet erőforrások egyezmények létrehozása: Ha a házirend érvényesítve van-e, és a végrehajtandó műveletet. Egyezmények meghatározásával szabályozhatja költségeit, és több könnyen kezelheti az erőforrásokat. Megadhatja például, hogy engedélyezve legyenek-e a virtuális gépek csak bizonyos típusú. Vagy megkövetelheti, hogy minden erőforrás egy bizonyos címkével rendelkezik. Összes gyermek-erőforrás által örökölt házirendek. Igen a házirend vonatkozik egy erőforráscsoport, esetén alkalmazandó az erőforráscsoport összes erőforrást.

JSON házirend-definíció létrehozására használhatja. A házirend-definíció a elemeket tartalmazza:

* mód
* paraméterek
* Megjelenített név
* leírás
* Házirend szabályai
  * logikai kiértékelése
  * hatása

Például a következő JSON mutatja egy házirendet, amely korlátozza, amelyekre központilag telepítették erőforrások:

```json
{
  "properties": {
    "mode": "all",
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

Minden Azure házirend sablon minták erővel [sablonok Azure házirend](json-samples.md).

## <a name="mode"></a>Mód

Azt javasoljuk, hogy állítsa `mode` való `all` házirenddel rendelkeznie hozzárendelés értékelje ki, az összes erőforrás-csoportok és típusát. Láthatja, hogy egy házirend-definíció, amely képes kikényszeríteni a címke van megadva egy erőforráscsoport, például [engedélyezése egyéni Virtuálisgép-lemezkép egy erőforráscsoportból](scripts/allow-custom-vm-image.md).

Ha beállította azt **összes**, csoportok és az összes erőforrástípus kiértékeli a házirendet. A portál alkalmaz **összes** az összes házirendhez. Ha a PowerShell vagy Azure CLI-t használ, meg kell adnia a `mode` paramétert, majd állítsa be **összes**.

A portál használatával létrehozott összes házirend-definíciók használja egy `all` mód, azonban ha azt szeretné, a PowerShell vagy az Azure parancssori felület, meg kell adnia a `mode` paramétert, majd állítsa be `all`.

Ha a mód beállítása legyen `indexed`, a házirend-hozzárendelés erőforrástípusok esetében, amelyek támogatják a címkék és a hely csak a kiértékelendő táblakifejezés.


## <a name="parameters"></a>Paraméterek

Paraméterek leegyszerűsíti a Csoportházirend kezelése a házirend-definíciók számának csökkentésével. A mezőket az űrlap – például paraméterek gondoljon `name`, `address`, `city`, `state`. Ezek a paraméterek ugyanaz mindig maradjon, azonban azok az értékek módosítása az űrlap egyes kitöltése alapján. Paraméterek ugyanúgy működnek, szabályzatok létrehozásakor. Paraméterek belefoglalja házirend-definíció, felhasználhatja különböző helyzetek kezelésére házirend különböző értékek használatával.

Például egy erőforrás-tulajdonságok korlátozza a hely, ahol erőforrásokat is telepíthető a házirendet sikerült határozza meg. Ebben az esetben a következő paraméterek volna deklarál, a házirend létrehozásakor:


```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations",
      "strongType": "location"
    }
  }
}
```

A paraméter típusa karakterlánc vagy tömb lehet. A metaadat-tulajdonságnak szolgál például az Azure-portálon felhasználóbarát információk megjelenítéséhez.

A metaadat-tulajdonságnak belül használható **strongType** arra, hogy az Azure-portálon belül beállítások többszörös kiválasztási listája.  Engedélyezett értékek a **strongType** jelenleg tartalmaz:

* `"location"`
* `"resourceTypes"`
* `"storageSkus"`
* `"vmSKUs"`

A házirend szabályban hivatkozási paraméter a következő szintaxissal:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Megjelenített név és leírás

Használhat **displayName** és **leírás** azonosíthatja a házirend-definíció, és adja meg a környezetben használat esetén.

## <a name="policy-rule"></a>Szabályzatbeli szabály

A házirendszabály áll **Ha** és **majd** blokkolja. Az a **Ha** blokk, megadhatja, hogy egy vagy több feltételt, adja meg, ha a házirend érvényesítve van-e. Logikai operátorok ezek a feltételek pontosan meghatározni a forgatókönyvhöz, amelyben egy házirendet alkalmazhat.

Az a **majd** blokk, megadhatja a hatást, amelyet akkor fordul elő, amikor a **Ha** feltételek teljesülnek.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Logikai operátorok

Támogatott logikai operátorok a következők:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

A **nem** szintaxis invertálja a feltétel eredménye. A **allOf** szintaxis (a logikai hasonló **és** művelet) igényel minden feltétel igaz értékű lesz. A **anyOf** szintaxis (a logikai hasonló **vagy** művelet) szükséges egy vagy több feltétel igaz értékű lesz.

Logikai operátorok ágyazhatja be. Az alábbi példa mutatja egy **nem** művelet, amelynek van beágyazva egy **allOf** műveletet.

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Feltételek

A feltétel e egy **mező** meghatározott feltételeknek eleget. A támogatott feltételek esetén:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Használatakor a **például** feltétel, megadhatja a helyettesítő karakter (*) értékét.

Használatakor a **megfelelő** feltétel, adja meg `#` képviselő számjegy, `?` betűvel, és bármely más karaktert adott tényleges karakter helyettesítéséhez. Tekintse meg a [jóváhagyott Virtuálisgép-rendszerképek](scripts/allowed-custom-images.md).

### <a name="fields"></a>Mezők
Feltételek mezőkkel jöttek létre. A mező képviseli tulajdonság az erőforrás-kérések forgalma, amellyel ismertetik az erőforrás állapotát.  

A következő mezők támogatottak:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*`
* tulajdonságának aliasokat - listájáért lásd: [aliasok](#aliases).

### <a name="effect"></a>Következmény
A házirend hatása a következő típusú támogatja:

* **Megtagadási**: a biztonsági naplóban generál egy eseményt, és a kérelem sikertelen
* **Naplózási**: naplót hoz létre a figyelmeztető üzenet, de nem tudja teljesíteni a kérést
* **Hozzáfűzendő**: a definiált mezők halmaza alapján ad a kéréshez
* **AuditIfNotExists**: lehetővé teszi a naplózást, ha egy erőforrás nem létezik.
* **DeployIfNotExists**: erőforrás telepíti, ha még nem létezik. Ebből a célból beépített házirendeken keresztül jelenleg csak támogatott.

A **hozzáfűzése**, meg kell adnia a következő adatokat:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

Az érték lehet egy karakterlánc vagy egy JSON-formátumú objektum.

A **AuditIfNotExists** és **DeployIfNotExists** értékelje ki a gyermek-erőforrás meglétét, és alkalmazza a szabályt, és a megfelelő hatása, ha adott erőforrás nem létezik. Például megkövetelheti, hogy egy hálózati figyelőt az összes virtuális hálózathoz van-e telepítve.
Példa a naplózást, ha egy virtuálisgép-bővítmény nincs telepítve, tekintse meg [kiterjesztés nem található naplózási](scripts/audit-ext-not-exist.md).


## <a name="aliases"></a>Aliasnevek

Egy erőforrás típusára vonatkozó tulajdonságokat eléréséhez használt tulajdonságának aliasokat. Aliasok engedélyezi, hogy milyen értékeket, vagy a feltételek engedélyezve van a erőforrás-tulajdonságok korlátozása. Mindegyik aliasnak elérési utak a különböző API-verziók egy adott erőforrástípusra van leképezve. Házirend kiértékelése közben a házirendmotor lekérdezi az adott API-verzió útvonal.

**Microsoft.Cache/Redis**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Állítsa e a nem ssl-Redis-kiszolgáló port (6379) engedélyezve van. |
| Microsoft.Cache/Redis/shardCount | Adjon meg a szilánkok prémium fürt gyorsítótárat létrehozni.  |
| Microsoft.Cache/Redis/sku.capacity | A telepítendő Redis gyorsítótár méretének beállítása.  |
| Microsoft.Cache/Redis/sku.family | Állítsa be az SKU-család használható. |
| Microsoft.Cache/Redis/sku.name | Állítsa be a Redis Cache telepítendő típusú. |

**Microsoft.Cdn/profiles**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Állítsa be az árképzési szint nevét. |

**Microsoft.Compute/disks**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imagePublisher | Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imageSku | Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imageVersion | Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját. |


**Microsoft.Compute/virtualMachines**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/imageId | Állítsa be a virtuális gép létrehozásához használt kép azonosítója. |
| Microsoft.Compute/imageOffer | Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imagePublisher | Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imageSku | Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imageVersion | Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját. |
| Microsoft.Compute/licenseType | Állítsa be, hogy a lemezkép vagy lemez-e a licencelt helyszíni. Ez az érték csak a Windows Server operációs rendszert tartalmazó lemezképek használatos.  |
| Microsoft.Compute/virtualMachines/imageOffer | Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/virtualMachines/imagePublisher | Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/virtualMachines/imageSku | Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/virtualMachines/imageVersion | Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Állítsa be a virtuális merevlemez URI azonosítója. |
| Microsoft.Compute/virtualMachines/sku.name | A virtuális gép méretének beállítása. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Állítsa be a kiadó nevét: a bővítményt. |
| Microsoft.Compute/virtualMachines/extensions/type | Állítsa be a kiterjesztés típusú. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | A verzió megadása |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/imageId | Állítsa be a virtuális gép létrehozásához használt kép azonosítója. |
| Microsoft.Compute/imageOffer | Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imagePublisher | Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imageSku | Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez. |
| Microsoft.Compute/imageVersion | Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját. |
| Microsoft.Compute/licenseType | Állítsa be, hogy a lemezkép vagy lemez-e a licencelt helyszíni. Ez az érték csak a Windows Server operációs rendszert tartalmazó lemezképek használatos. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | A méretezési csoportban lévő összes virtuális gépet a számítógép nevének előtagját beállítása |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Állítsa be a blob URI felhasználói lemezkép. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | A méretezési az operációs rendszer lemezek tárolására használt tároló URL-címeinek beállítása. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Méretezési csoportban lévő virtuális gépek méretének beállítása. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Méretezési csoportban lévő virtuális gépek szintjének beállítása |

**Microsoft.Network/applicationGateways**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Az átjáró méretének beállítása. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Állítsa be a virtuális hálózati átjáró típusát. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Állítsa be az átjáró-termékváltozat. |

**Microsoft.Sql/servers**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Állítsa be a kiszolgáló verziója. |

**Microsoft.Sql/databases**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Állítsa be az adatbázis-kiadás. |
| Microsoft.Sql/servers/databases/elasticPoolName | Állítsa be a rugalmas készlet az adatbázis nevét. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Állítsa be az adatbázis konfigurált szolgáltatási szint célkitűzésének azonosítója. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Állítsa be a konfigurált szolgáltatásiszint-célkitűzés az adatbázis nevét.  |

**Microsoft.Sql/elasticpools**

| Alias | Leírás |
| ----- | ----------- |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Állítsa be a rugalmas adatbáziskészlet az összes megosztott DTU. |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Állítsa be a rugalmas készlet kiadása. |

**Microsoft.Storage/storageAccounts**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Állítsa be a hozzáférési réteg számlázási használatos. |
| Microsoft.Storage/storageAccounts/accountType | Állítsa be a termékváltozat. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Állítsa be, hogy a szolgáltatás titkosítja az adatokat a blob storage szolgáltatásban tárolt. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Állítsa be, hogy a szolgáltatás titkosítja az adatokat, a fájl storage szolgáltatásban tárolt. |
| Microsoft.Storage/storageAccounts/sku.name | Állítsa be a termékváltozat. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Beállítása, hogy a csak https-forgalom tároló szolgáltatást. |

## <a name="initiatives"></a>Kezdeményezések

Csoportosítása több kezdeményezések engedélyezése vonatkozó házirend-definíciók egyszerűsítése érdekében hozzárendelések és a felügyeleti, mert a csoport az egyetlen elemet használata. Például egy egyetlen kezdeményezésére minden kapcsolódó címkézési házirend definíciói csoportosíthatja. Ahelyett, hogy minden egyes házirend hozzárendelése egyenként, a kezdeményező alkalmaz.

A következő példa bemutatja, hogyan létrehozása kezelési két címkék kezdeményezés: `costCenter` és `productName`. Azt a két beépített házirendek segítségével az alapértelmezett címke.


```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>További lépések

- Tekintse át az Azure házirend sablon minták [sablonok Azure házirend](json-samples.md).
