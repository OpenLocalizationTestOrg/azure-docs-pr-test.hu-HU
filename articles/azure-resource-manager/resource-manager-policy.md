---
title: "erőforrás-házirendek aaaAzure |} Microsoft Docs"
description: "Ismerteti, üzembe helyezése során toouse Azure Resource Manager házirendek tooensure egységes erőforrás-tulajdonságok beállításának módját. Szabályzatok alkalmazhatók a hello előfizetés vagy az erőforrás-csoportok."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: tomfitz
ms.openlocfilehash: f1b0bbb5f838f6bb70721e1040ad3eac2d881cea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-policy-overview"></a>Erőforrás-házirendek – áttekintés
Erőforrás-házirendek lehetővé teszik a szervezet erőforrásaihoz tooestablish szabályai. Egyezmények meghatározásával szabályozhatja költségeit, és több könnyen kezelheti az erőforrásokat. Megadhatja például, hogy engedélyezve legyenek-e a virtuális gépek csak bizonyos típusú. Vagy megkövetelheti, hogy minden erőforrás egy bizonyos címkével rendelkezik. Összes gyermek-erőforrás által örökölt házirendek. Ezért, ha a házirend alkalmazott tooa erőforráscsoportban, ezt a megfelelő tooall hello erőforrások az erőforráscsoport.

Nincsenek két fogalmak toounderstand szabályzataival kapcsolatban:

* házirend-definíció - ismerteti hello szabályzat érvényes mikor és milyen művelet tootake
* házirend-hozzárendelés - alkalmazása hello házirend definition tooa hatókör (előfizetéshez vagy erőforráscsoporthoz)

Ez a témakör ismerteti a házirend-definíció. További információ a házirend-hozzárendelést: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md) vagy [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).

Házirendek létrehozása, és az erőforrások (PUT és műveletek javítás) frissítésekor kiértékelése.

> [!NOTE]
> Házirend jelenleg nem értékelhetők erőforrástípusok esetében, amelyek nem támogatják a címkék, típusa és helyre, például hello Microsoft.Resources/deployments erőforrástípus. Ez a támogatás a későbbiekben lesz hozzáadva. tooavoid visszafelé kompatibilitási problémák, explicit módon adjon meg típusú házirendek runboookok létrehozásakor. Például egy címke házirend, amely nem határoz meg típusok minden alkalmazástípus esetében érvényes. Ebben az esetben egy sablon telepítés sikertelen lehet, ha egy beágyazott erőforrást, amely nem támogatja a címkék, és hello telepítési erőforrástípus toopolicy értékelési hozzá lett adva. 
> 
> 

## <a name="how-is-it-different-from-rbac"></a>Miben különbözik az RBAC?
Házirend és a szerepköralapú hozzáférés-vezérlést (RBAC) közötti néhány fontos különbség van. Az RBAC-ra összpontosít **felhasználói** különböző hatóköröket műveletek. Például akkor kerülnek toohello közreműködő szerepkört az egy erőforráscsoport szükséges hello hatókörben, hogy módosításokat toothat erőforráscsoport. Házirend összpontosít **erőforrás** tulajdonságok üzembe helyezése során. Például szabályzatokkal, szabályozhatja hello típusú erőforrások, amelyek létesíthetők. Vagy létesíthetők hello erőforrások hello helyek korlátozhatja. Szerepalapú, eltérően házirend egy alapértelmezett engedélyezése és explicit megtagadja a rendszer. 

toouse házirendek, akkor RBAC lehet hitelesíteni. Pontosabban, a fiókot kell-e a:

* `Microsoft.Authorization/policydefinitions/write`engedély toodefine házirend
* `Microsoft.Authorization/policyassignments/write`engedély tooassign házirend 

Ezek az engedélyek nem szerepelnek a hello **közreműködő** szerepkör.

## <a name="built-in-policies"></a>Beépített házirendek

Azure biztosít néhány beépített házirend-definíciókat tartalmazzon, hello számát csökkentheti a házirendek toodefine rendelkezik. Házirend-definíciók a folytatás előtt érdemes lehet akár egy beépített házirend már használatával hello definíciója van szüksége. hello beépített házirend-definíciók száma a következők:

* Engedélyezett helyek
* Engedélyezett erőforrástípusok
* A tárfiók termékváltozatok engedélyezett
* Virtuális gép termékváltozatok engedélyezett
* Címke és az alapértelmezett érték alkalmazása
* Címke és érték
* Nem engedélyezett típusú erőforrások
* SQL Server verziója 12.0 megkövetelése
* Tárolási fiók titkosításának kötelezővé tétele

Említett házirendjeinek hello keresztül rendelhet [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), vagy [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).

## <a name="policy-definition-structure"></a>Házirend szerkezete
Házirend-definíció JSON toocreate használhatja. hello házirend-definíció a elemeket tartalmazza:

* paraméterek
* Megjelenített név
* leírás
* Házirend szabályai
  * logikai kiértékelése
  * hatása

a következő példa hello egy házirendet, amely korlátozza, amelyekre központilag telepítették erőforrások láthatók:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
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

## <a name="parameters"></a>Paraméterek
Paraméterek használatával egyszerűbbé teszi a Csoportházirend kezelése házirend-definíciók hello számának csökkentésével. Adja meg a házirendet (például korlátozza a hello helyeken, ahol erőforrásokat is telepíthető) erőforrás-tulajdonságot, és paraméterek közé tartoznak a hello definícióban. Ezt követően többször használ, hogy a házirend-definíció különböző helyzetek kezelésére különböző értékeket (például megadják az előfizetéshez tartozó helyek egy set) történő amikor hello házirend hozzárendelése.

Ha a házirend-meghatározások létrehozása deklarálhatja paraméterek.

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "hello list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

egy paraméter hello típusú karakterlánc vagy tömb lehet. például az Azure portál toodisplay felhasználóbarát információk hello metaadat-tulajdonságnak szolgál. 

Hello házirendszabály, a paraméterek a hello szintaxisa a következő hivatkozik: 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Megjelenített név és leírás

Hello használata **displayName** és **leírás** tooidentify hello házirend-definíció, és adja meg a környezetben használat esetén.

## <a name="policy-rule"></a>Házirend szabályai

hello házirendszabály áll **Ha** és **majd** blokkolja. A hello **Ha** blokk, megadhatja, hogy egy vagy több feltételt, adja meg, amikor a hello házirend van érvényben. Logikai operátorok toothese feltételek alkalmazása tooprecisely hello forgatókönyv egy házirend határozza meg. A hello **majd** blokk, megadhatja, hogy történik, ha hello hello hatással **Ha** feltételek teljesülnek.

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
hello támogatott logikai operátorok a következők:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

Hello **nem** szintaxis invertálja hello feltétel hello eredményét. Hello **allOf** szintaxis (logikai hasonló toohello **és** művelet) igényel minden feltételek toobe teljesül. Hello **anyOf** szintaxis (logikai hasonló toohello **vagy** művelet) egy vagy több feltételek toobe true igényel.

Logikai operátorok ágyazhatja be. a következő példa azt mutatja meg hello egy **nem** művelet, amelynek van beágyazva egy **allOf** műveletet. 

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
hello feltétel e egy **mező** meghatározott feltételeknek eleget. hello támogatott feltételek esetén:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Hello használatakor **például** feltétel, megadhatja a helyettesítő karakter (*) hello érték.

Hello használata esetén **megfelelő** feltétel, adja meg `#` toorepresent számjegy, `?` a betű, és bármilyen más karakter toorepresent Ez a tényleges karakter. Tekintse meg a [nevét és az erőforrás-szabályzatok alkalmazása](resource-manager-policy-naming-convention.md).

### <a name="fields"></a>Mezők
Feltételek mezőkkel jöttek létre. A mező képviseli tulajdonságokat az erőforrás-kérések forgalma hello hello erőforrás, amely használt toodescribe hello állapotát.  

a következő mezők hello támogatottak:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* tulajdonságának aliasokat - listájáért lásd: [aliasok](#aliases).

### <a name="effect"></a>Következmény
Házirend érvénybe - három típusú támogat `deny`, `audit`, és `append`. 

* **Megtagadási** hello naplóban generál egy eseményt, és nem sikerül hello kérelem
* **Naplózási** naplót hoz létre a figyelmeztető üzenet, de nem sikertelen hello kérelem
* **Hozzáfűzendő** mezők toohello kérelem hello meghatározott készletét hozzáadása 

A **hozzáfűzése**, meg kell adnia a következő adatok hello:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of hello field"
  }
]
```

hello érték lehet egy karakterlánc vagy egy JSON-formátumú objektum. 

## <a name="aliases"></a>Aliasok

Az erőforrástípus tulajdonságának aliasokat tooaccess tulajdonságokat használhatja. Aliasok toorestrict milyen értékeket, vagy a feltételek engedélyezve van a erőforrás-tulajdonságok engedélyezése. Mindegyik aliasnak maps toopaths egy adott erőforrástípusra különböző API-verziókban. Házirend kiértékelése közben hello házirendmotor kap, az API-verzióhoz tartozó hello útvonal.

**Microsoft.Cache/Redis**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Állítsa be e hello Redis-kiszolgáló-ssl port (6379) engedélyezve van. |
| Microsoft.Cache/Redis/shardCount | Prémium szintű fürt gyorsítótárat létrehozni szilánkok toobe hello számának megadása.  |
| Microsoft.Cache/Redis/sku.capacity | Hello Redis gyorsítótár toodeploy hello méret megadása.  |
| Microsoft.Cache/Redis/sku.family | Hello SKU-család toouse beállítása. |
| Microsoft.Cache/Redis/sku.name | Állítsa be a Redis Cache toodeploy hello típusú. |

**Microsoft.Cdn/profiles**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | IP-címek hello hello nevének beállítása. |

**Microsoft.Compute/disks**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/imagePublisher | Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/imageSku | Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja. |
| Microsoft.Compute/imageVersion | Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |


**Microsoft.Compute/virtualMachines**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/imageId | Hello hello használt kép toocreate hello virtuális gép azonosítójának megadása |
| Microsoft.Compute/imageOffer | Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/imagePublisher | Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/imageSku | Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja. |
| Microsoft.Compute/imageVersion | Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/licenseType | Állítsa be, hogy hello lemezkép vagy lemez-e a licencelt helyszíni. Ez az érték csak a hello Windows Server operációs rendszert tartalmazó lemezképek használatos.  |
| Microsoft.Compute/virtualMachines/imageOffer | Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/virtualMachines/imagePublisher | Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/virtualMachines/imageSku | Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja. |
| Microsoft.Compute/virtualMachines/imageVersion | Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Állítsa be a hello vhd URI. |
| Microsoft.Compute/virtualMachines/sku.name | Virtuális gép hello hello méretének beállítása. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Hello bővítmény publisher hello nevének beállítása. |
| Microsoft.Compute/virtualMachines/extensions/type | Állítsa be a bővítmény hello típusát. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Állítsa be a hello bővítmény hello verziója. |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Compute/imageId | Hello hello használt kép toocreate hello virtuális gép azonosítójának megadása |
| Microsoft.Compute/imageOffer | Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/imagePublisher | Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/imageSku | Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja. |
| Microsoft.Compute/imageVersion | Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja. |
| Microsoft.Compute/licenseType | Állítsa be, hogy hello lemezkép vagy lemez-e a licencelt helyszíni. Ez az érték csak a hello Windows Server operációs rendszert tartalmazó lemezképek használatos. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Hello méretezési csoportban lévő hello gépek nevének előtagja hello virtuális gép beállítása |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Állítsa be a hello blob URI felhasználói lemezkép. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Hello tároló URL-címeinek beállítása, amelyek használt toostore operációs rendszer lemezek hello méretezési készlet. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Méretezési csoportban lévő virtuális gépek méretét hello beállítása |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Méretezési csoportban lévő virtuális gépek hello szintjének beállítása |
  
**Microsoft.Network/applicationGateways**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Hello átjáró hello méret megadása. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Állítsa be a virtuális hálózati átjáró hello típusú. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Átjáró-termékváltozat hello beállítása. |

**Microsoft.Sql/servers**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Állítsa be a hello hello server verziója. |

**Microsoft.Sql/databases**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Állítsa be a hello adatbázis hello kiadásában. |
| Microsoft.Sql/servers/databases/elasticPoolName | Hello rugalmas készlet hello adatbázis hello neve van. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Hello beállítása a szolgáltatási szint célkitűzésének azonosítója hello adatbázis konfigurálva. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Hello konfigurált szolgáltatási szint célkitűzésének hello adatbázis hello nevének beállítása.  |

**Microsoft.Sql/elasticpools**

| Alias | Leírás |
| ----- | ----------- |
| kiszolgálók/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Set hello összesen hello database rugalmas készlet DTU megosztott. |
| kiszolgálók/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Állítsa be a rugalmas készlet hello hello kiadása. |

**Microsoft.Storage/storageAccounts**

| Alias | Leírás |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Állítsa a számlázási használt hello hozzáférési szintet. |
| Microsoft.Storage/storageAccounts/accountType | Állítsa be a hello termékváltozat. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Állítsa be, hogy hello blob storage szolgáltatásban tárolt hello szolgáltatás titkosítja az hello adatokat. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Állítsa be, hogy hello storage szolgáltatásban tárolt hello szolgáltatás titkosítja az hello adatokat. |
| Microsoft.Storage/storageAccounts/sku.name | Állítsa be a hello termékváltozat. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Adja meg tooallow csak https-forgalom toostorage szolgáltatást. |


## <a name="policy-examples"></a>Házirend példák

a következő témakörök hello házirend példákat tartalmaz:

* Példák címke házirendek, lásd: [címkék erőforrás házirendek alkalmazását](resource-manager-policy-tags.md).
* Annak a kiosztási és a szöveg, tekintse meg a [nevét és az erőforrás-szabályzatok alkalmazása](resource-manager-policy-naming-convention.md).
* A tárolási házirendek, tekintse meg a [alkalmazni házirendeket toostorage erőforrásfiókok](resource-manager-policy-storage.md).
* A virtuális gép házirendek, tekintse meg a [alkalmazni az erőforrás-házirendek tooLinux virtuális gépek](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) és [alkalmazni az erőforrás-házirendek tooWindows virtuális gépek](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)


## <a name="next-steps"></a>Következő lépések
* Házirend szabály megadása után rendelje hozzá tooa hatókör. hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md). REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).
* hello házirend séma közzé van téve a [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

