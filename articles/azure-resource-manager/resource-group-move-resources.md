---
title: "Azure-erőforrások áthelyezése új előfizetés vagy az erőforrás csoport |} Microsoft Docs"
description: "Azure Resource Manager segítségével az erőforrások áthelyezése egy új erőforráscsoportba vagy előfizetésbe."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: tomfitz
ms.openlocfilehash: e138f80e808968ab4bf5c11cfd5fd46fe4a1bcce
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe
Ez a témakör bemutatja, hogyan kell helyeznie az erőforrásokat az új előfizetés vagy egy új erőforráscsoportot ugyanahhoz az előfizetéshez. A portál, PowerShell, az Azure parancssori felület vagy a REST API használatával erőforrás áthelyezése. Ez a témakör az áthelyezési műveletek rendelkezésére álljanak a segítség nélkül az Azure-támogatás.

Ha az erőforrások áthelyezése, mind a forrás-csoport és a célcsoport zárolva van a művelet során. Írási és törlési műveletek blokkolják az erőforráscsoportok az áthelyezés befejezéséig. A lakat azt jelenti, nem adja hozzá, frissítenie vagy törölnie az erőforráscsoportok erőforrásokat, de nem jelent az erőforrások sincs rögzítve. Például ha egy SQL Server és a kapcsolódó adatbázis áthelyezése egy új erőforráscsoportot, az adatbázist használó alkalmazások teljesen állásidő nélkül. Továbbra is olvasni és írni az adatbázisba.

Az erőforrás helye nem módosítható. Egy erőforrás áthelyezése csak áthelyezi egy új erőforráscsoportot. Az új erőforráscsoportot egy másik helyre azonban lehet, hogy az erőforrás helye nem változik.

> [!NOTE]
> A cikkből megtudhatja, hogyan kívánja áthelyezni az erőforrásokat egy meglévő Azure fiók ajánlja. Ha ténylegesen módosítani szeretné az Azure-fiókjával (például a frissítés a használatalapú fizetésre előre kifizetni) nyújtó miközben továbbra is a meglévő erőforrásokkal folytatott munka című [az Azure-előfizetéshez Váltás másik ajánlatra](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Erőforrások áthelyezése előtt ellenőrzőlista
Néhány fontos lépést végre kell hajtani az erőforrások áthelyezése előtt. Ezen feltételek ellenőrzésével a hibák elkerülhetőek.

1. A forrás és cél előfizetések léteznie kell a belül azonos [Azure Active Directory-bérlő](../active-directory/active-directory-howto-tenant.md). Ellenőrizze, hogy mindkét előfizetéshez tartozik-e az azonos Bérlőazonosító, használja az Azure PowerShell vagy az Azure parancssori felület.

  Azure PowerShell használata:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  Az Azure CLI 2.0 használja:

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  Ha a bérlő azonosítók a forrás és cél előfizetésekhez nem egyeznek, esetleg módosítsa a könyvtárat, az előfizetés. Ez a beállítás, szolgáltatás-rendszergazdák, akik van bejelentkezve, akkor a Microsoft-fiók (nem lehet szervezeti fiókkal) csak érhető el. Sikertelen bejelentkezési kísérletet könyvtárváltás, jelentkezzen be a [klasszikus portál](https://manage.windowsazure.com/), és válassza ki **beállítások**, és válassza ki az előfizetést. Ha a **könyvtár szerkesztése** ikon érhető el, válassza ki azt a társított Azure Active Directory módosítása.

  ![címtár szerkesztése](./media/resource-group-move-resources/edit-directory.png)

  Erre az ikonra nem érhető el, ha az erőforrások áthelyezése új bérlőt kell ügyfélszolgálatához.

2. A szolgáltatásnak lehetővé kell tennie az erőforrások áthelyezését. Ez a témakör felsorolja a szolgáltatások lehetővé teszik az erőforrások áthelyezése, és a szolgáltatások nem engedélyezi az erőforrások áthelyezése.
3. A cél előfizetést regisztrálni kell az áthelyezett erőforrás erőforrás-szolgáltatóján. Ha nem, hibaüzenet arról, hogy a **az előfizetés nincs regisztrálva az erőforrástípus**. Ez a probléma erőforrások új előfizetésre történő áthelyezésekor fordulhat elő, ha az előfizetést még nem használták az adott erőforrástípushoz. A regisztrációs állapot ellenőrzésével és erőforrás-szolgáltatók regisztrálásával kapcsolatos további információ:[Erőforrás-szolgáltatók és erőforrástípusok](resource-manager-supported-services.md).

## <a name="when-to-call-support"></a>Mikor érdemes az ügyfélszolgálat
Áthelyezheti a legtöbb erőforrást ebben a témakörben látható önkiszolgáló műveletek révén. Használja az önkiszolgáló műveletek:

* Resource Manager-erőforrások áthelyezése.
* Hagyományos erőforrások áthelyezéséhez a [klasszikus üzembe helyezési korlátozások](#classic-deployment-limitations).

Hívja a támogatást, ha szeretné:

* Az erőforrások áthelyezése egy új Azure-fiók (és az Azure Active Directory-bérlő).
* Hagyományos erőforrások áthelyezéséhez, de problémát tapasztal a korlátozásokkal.

## <a name="services-that-enable-move"></a>Szolgáltatások, amelyek lehetővé teszik a áthelyezése
A lépést a szolgáltatások, amelyek lehetővé teszik egy új erőforráscsoportot és az előfizetés áthelyezését a következők:

* API Management
* App Service apps (webalkalmazások) – lásd: [App Service korlátozásai](#app-service-limitations)
* Application Insights
* Automatizálás
* Batch
* A Bing Maps
* Tartalomkézbesítési hálózat (CDN)
* A felhőalapú szolgáltatások – lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* Cognitive Services
* Tartalommoderátor
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Azure Cosmos DB
* Event Hubs
* A HDInsight-fürtök - Lásd [HDInsight korlátozásai](#hdinsight-limitations)
* IoT-központok
* Key Vault
* Terheléselosztók
* Logic Apps
* Machine Learning
* Media Services
* Mobile Engagement
* Notification Hubs
* Operational Insights
* Operations Management
* Power BI
* Redis Cache
* Scheduler
* Keresés
* Kiszolgálófelügyelet
* Service Bus
* Service Fabric
* Storage
* Tekintse meg a tároló (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* A Stream Analytics - feladatok nem helyezhető át, ha a Stream Analytics állapotban.
* SQL adatbázis-kiszolgáló – az adatbázis és a kiszolgáló ugyanabban az erőforráscsoportban kell lennie. Ha egy SQL server helyezi át, az adatbázisokat is kerülnek.
* Traffic Manager
* Virtuális gépek
* A Key Vault tárolt virtuális gépek, a tanúsítvány - áthelyezése új erőforrás ugyanahhoz az előfizetéshez csoport engedélyezve van, de több előfizetés áthelyezése nem engedélyezett.
* Virtuális gépek (klasszikus) - lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* Virtual Machine Scale Sets
* Virtuális hálózatok - jelenleg, peered virtuális hálózat nem helyezhető át, amíg a Vnetben társviszony-létesítés le van tiltva. Ha le van tiltva, a virtuális hálózat sikeresen áthelyezhető, és a Vnetben társviszony-létesítést is engedélyezhető. Emellett a virtuális hálózati nem helyezhető át egy másik előfizetést, ha a virtuális hálózat egyetlen alhálózatának sem az erőforrás-navigációs hivatkozásokkal tartalmazza. Például a virtuális hálózati alhálózat egy erőforrás-navigációs hivatkozás rendelkezik Microsoft.Cache redis erőforrás az alhálózaton történő telepítésekor.
* VPN Gateway


## <a name="services-that-do-not-enable-move"></a>Ne engedélyezze a move szolgáltatások
A szolgáltatások, amelyek jelenleg nem engedélyezi az erőforrás áthelyezése a következők:

* Az AD tartományi szolgáltatások
* AD hibrid Állapotfigyelő szolgáltatás
* Application Gateway
* A felügyelt lemezzel rendelkező virtuális gépek rendelkezésre állási készletek
* BizTalk Services
* Container Service
* Express Route
* DevTest Labs - helyezze át az új erőforráscsoport ugyanahhoz az előfizetéshez engedélyezve van, de több előfizetés áthelyezése nem engedélyezett.
* Dynamics LCS
* Felügyelt lemezekből lemezképeit
* Felügyelt lemezek
* Felügyelt alkalmazások
* Recovery Services-tároló - is do helyezi át a számítási, hálózati és tárolási erőforrásokat, a Recovery Services-tároló társított lásd [helyreállítási szolgáltatások korlátozásai](#recovery-services-limitations).
* Biztonság
* Felügyelt lemezekből létrehozott pillanatfelvételek
* StorSimple Device Manager
* Felügyelt lemezzel rendelkező virtuális gépek
* Tekintse meg a virtuális hálózatok (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* Piactér-erőforrások - alapján létrehozott virtuális gépeken nem helyezhető át, előfizetések között. Az aktuális előfizetésben platformelőfizetés és az új előfizetés újra üzembe kell erőforrás

## <a name="app-service-limitations"></a>App Service korlátozásai
Az App Service apps használatakor csak egy App Service-csomag nem helyezhető át. App Service apps áthelyezéséhez a lehetőségek a következők:

* Helyezze át az App Service-csomag és egyéb App Service-erőforrások az erőforráscsoport egy új erőforráscsoportot, amely még nincs az App Service-erőforrások. Ez a követelmény azt jelenti, hogy akkor is, amelyek nem kapcsolódnak az App Service-csomag az App Service erőforrások kell áthelyeznie.
* Az alkalmazások áthelyezése egy másik erőforráscsoportban található, de az App Service-csomagokról ne az eredeti erőforráscsoport.

Az App Service-csomag nem kell lennie, ugyanabban az erőforráscsoportban, az alkalmazás az alkalmazás megfelelő működéséhez.

Ha például az erőforráscsoport tartalmazza:

* **webalkalmazás-a** amely társítva van **terv-a**
* **webalkalmazás-b** amely társítva van **terv-b**

A lehetőségek a következők:

* Helyezze át **web-a**, **terv a**, **web-b**, és **terv-b**
* Helyezze át **web-a** és **web-b**
* Helyezze át **web-a**
* Helyezze át **web-b**

Minden más kombináció tartalmaz, amely így maradnak, amely nem hagyható mögött, amikor az App Service-csomag (bármilyen típusú App Service erőforrás) erőforrástípus.

Ha a webalkalmazás helyezkedik el, mint az App Service-csomag egy másik erőforráscsoportban található, de egyaránt egy új erőforráscsoportot át szeretné helyezni, el kell végeznie az áthelyezés két lépésben. Példa:

* **webalkalmazás-a** található **webalkalmazás-csoport**
* **terv a** található **terv-csoport**
* Kívánt **web-a** és **terv a** lenniük, hogy **kombinált csoport**

Az áthelyezés végrehajtásához két külön áthelyezési műveletet végrehajtani az alábbi sorrendben:

1. Helyezze át a **web-a** való **terv-csoport**
2. Helyezze át **web-a** és **terv a** való **kombinált csoport**.

Egy új erőforráscsoportot, vagy probléma nélkül előfizetési áthelyezheti egy App Service-tanúsítványt. Azonban ha a webalkalmazás SSL-tanúsítványt adott beszerzett kívülről, és az alkalmazásba feltöltött tartalmaz, törölnie kell a tanúsítvány előtt a webalkalmazást. Például végezheti el az alábbi lépéseket:

1. A feltöltött tanúsítvány törlése a webalkalmazásról
2. Helyezze át a webes alkalmazás
3. A webes alkalmazás a tanúsítvány feltöltése

## <a name="recovery-services-limitations"></a>Helyreállítási szolgáltatások korlátozásai
Helyezze át a tárolási, hálózati, nincs engedélyezve, vagy számítási erőforrásokat az Azure Site Recovery vész-helyreállítási telepítéséhez használt.

Tegyük fel például, hogy állította be a tárfiók (Storage1) helyszíni gépek replikációja, és szeretné, hogy a védett gép elérni a feladatátvételt követően az Azure-ba (Network1) virtuális hálózathoz csatlakozó virtuális gépként (VM1). Nem helyezhető át - Storage1 VM1 és Network1 - e az Azure erőforrások bármelyike erőforráscsoportok egyazon előfizetésen belül vagy az előfizetések.

## <a name="hdinsight-limitations"></a>A HDInsight-korlátozások

A HDInsight-fürtök áthelyezése egy új előfizetéshez vagy erőforráscsoporthoz. Azonban nem helyezhető át a hálózati erőforrások (például a virtuális hálózat, a hálózati adapter vagy a terheléselosztó) a HDInsight-fürthöz kapcsolódó előfizetések között. Ezenkívül nem helyezhető át egy új erőforráscsoportot egy hálózati Adaptert, amely a fürt virtuális gép csatlakozik.

Amikor egy új előfizetés helyezi át a HDInsight-fürtöt, először helyezze át az egyéb erőforrások (például a tárfiók). Ezután helyezze át a HDInsight-fürt önmagában.

## <a name="classic-deployment-limitations"></a>Klasszikus üzembe helyezési korlátozásai
A klasszikus modellben telepített erőforrások áthelyezésére szolgáló beállítások attól függően változnak, hogy helyez át az erőforrásokat egy előfizetésen belül vagy egy új előfizetést.

### <a name="same-subscription"></a>Ugyanahhoz az előfizetéshez
Erőforrások erőforráscsoportok közötti áthelyezése egy másik erőforráscsoportban egyazon előfizetésen belül, ha a következő korlátozások vonatkoznak:

* Nem lehet áthelyezni a virtuális hálózatok (klasszikus).
* Virtuális gépek (klasszikus) át szeretné helyezni a felhőalapú szolgáltatással.
* A felhőalapú szolgáltatás csak akkor helyezhető, ha az áthelyezés tartalmazza az összes virtuális gép.
* Egyszerre csak egy felhőalapú szolgáltatás helyezheti át.
* Egyszerre csak egy (klasszikus) tárfiókot helyezheti át.
* (Klasszikus) tárfiókot nem lehet áthelyezni a virtuális gép vagy egy felhőalapú szolgáltatás ugyanazt a műveletet.

Hagyományos erőforrás áthelyezése egy új erőforráscsoportot egyazon előfizetésen belül, a standard áthelyezési műveletek keresztül használja a [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), vagy [REST API-t](#use-rest-api). Használhatja ugyanazokat a műveleteket, mint a Resource Manager erőforrások áthelyezése.

### <a name="new-subscription"></a>Új előfizetés
Ha az erőforrások áthelyezése új előfizetés, a következő korlátozások vonatkoznak:

* Az előfizetés az összes hagyományos erőforrások át szeretné helyezni a ugyanazt a műveletet.
* A célként megadott előfizetés nem tartalmazhat más hagyományos erőforrások.
* Az Áthelyezés egy külön REST API-n keresztül klasszikus helyezi át a csak meg kell adniuk. A szabványos erőforrás-kezelő áthelyezés parancsok nem működnek a hagyományos erőforrás áthelyezése egy új előfizetést.

Hagyományos erőforrás áthelyezése egy új előfizetés, a jellemző hagyományos erőforrások REST műveleteinek használja. A többi használatához hajtsa végre az alábbi lépéseket:

1. Ellenőrizze, hogy ha a forrás-előfizetés részt vehetnek-e egy előfizetések közötti áthelyezés. Használja a következő műveletet:

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     A kérelem törzsében szereplő a következők:

  ```json
  {
    "role": "source"
  }
  ```

     A válasz az ellenőrzési művelet a következő formátumban kell megadni:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Ellenőrizze, hogy ha a célelőfizetés részt vehetnek-e egy előfizetések közötti áthelyezés. Használja a következő műveletet:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     A kérelem törzsében szereplő a következők:

  ```json
  {
    "role": "target"
  }
  ```

     A válasz van ugyanabban a formában, mint a forrás-előfizetés ellenőrzése.
3. Ha mindkét előfizetéshez teljesíti az ellenőrző, minden hagyományos erőforrás áthelyezése egy előfizetés másik előfizetéshez a következő műveletet:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    A kérelem törzsében szereplő a következők:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

A művelet több percig is futhat.

## <a name="use-portal"></a>A portál használatával
Erőforrások áthelyezéséhez jelölje ki ezeket az erőforrásokat tartalmazó erőforráscsoportot, majd a **áthelyezése** gombra.

![Erőforrások áthelyezése](./media/resource-group-move-resources/select-move.png)

Adja meg, hogy egy új erőforráscsoportot, vagy egy új előfizetés áthelyezni az erőforrásokat.

Válassza ki az áthelyezni kívánt erőforrásokat és a célként megadott erőforráscsoport. Megerősíti, hogy szeretné-e frissíteni a parancsfájl-ezeket az erőforrásokat, és válassza ki **OK**. Ha az előző lépésben kiválasztott előfizetés Szerkesztés ikonra, a célelőfizetés is kell választania.

![Válassza ki a cél](./media/resource-group-move-resources/select-destination.png)

A **értesítések**, láthatja, hogy fut-e a műveletet.

![Áthelyezési állapotának megjelenítése](./media/resource-group-move-resources/show-status.png)

Amikor befejeződött, az eredmény értesítést kap.

![Áthelyezési eredmény megjelenítése](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>A PowerShell használata
Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a `Move-AzureRmResource` parancsot.

Az első példában egy erőforrás áthelyezése egy új erőforráscsoportot.

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

A második példában több erőforrás áthelyezése egy új erőforráscsoportot.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Helyezze át az új előfizetés, tartalmazza a értéket a `DestinationSubscriptionId` paraméter.

A rendszer felkéri győződjön meg arról, hogy szeretné-e a megadott erőforrások áthelyezése.

```powershell
Confirm
Are you sure you want to move these resources to the resource group
'/subscriptions/{guid}/resourceGroups/newRG' the resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a>Azure parancssori felület használatával 2.0
Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a `az resource move` parancsot. Adja meg az erőforrás-azonosítók az erőforrások áthelyezése. Erőforrás-azonosítók a következő paranccsal szerezheti be:

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

A következő példa bemutatja, hogyan a storage-fiók áthelyezése egy új erőforráscsoportot. Az a `--ids` paramétert, az erőforrás-azonosítók áthelyezése szóközökkel elválasztott listáját tartalmazzák.

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

Helyezze át az új előfizetés, adja meg a `--destination-subscription-id` paraméter.

## <a name="use-azure-cli-10"></a>Azure parancssori felület használatával 1.0
Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a `azure resource move` parancsot. Adja meg az erőforrás-azonosítók az erőforrások áthelyezése. Erőforrás-azonosítók a következő paranccsal szerezheti be:

```azurecli
azure resource list -g sourceGroup --json
```

Amely adja vissza a következő formátumban:

```azurecli
[
  {
    "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
    "name": "storagedemo",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "southcentralus",
    "tags": {},
    "kind": "Storage",
    "sku": {
      "name": "Standard_RAGRS",
      "tier": "Standard"
    }
  }
]
```

A következő példa bemutatja, hogyan a storage-fiók áthelyezése egy új erőforráscsoportot. Az a `-i` paraméter, adja meg az erőforrás-azonosítók áthelyezése vesszővel tagolt listája.

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

Győződjön meg arról, hogy szeretné-e a megadott erőforrás áthelyezése kérni.

## <a name="use-rest-api"></a>A REST API használata
Meglévő erőforrások áthelyezése egy másik erőforráscsoportba vagy előfizetésbe, futtassa:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

A kérelem törzsében meg a célként megadott erőforráscsoportja és az erőforrások áthelyezése. Az áthelyezési REST művelet kapcsolatos további információkért lásd: [erőforrások áthelyezése](https://msdn.microsoft.com/library/azure/mt218710.aspx).

## <a name="next-steps"></a>Következő lépések
* Az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal kapcsolatban lásd: [Azure PowerShell használata a Resource Manager](powershell-azure-resource-manager.md).
* Az előfizetés kezelésének Azure parancssori felület parancsait kapcsolatos további tudnivalókért lásd: [az Azure parancssori felület használatával a Resource Manager](xplat-cli-azure-resource-manager.md).
* Az előfizetés kezelésének portál funkciókkal kapcsolatos további tudnivalókért lásd: [-erőforrások kezeléséhez Azure portál használatával](resource-group-portal.md).
* Az erőforrások logikus alkalmazásával kapcsolatos további tudnivalókért lásd: [az erőforrások rendszerezése címkék használatával](resource-group-using-tags.md).
