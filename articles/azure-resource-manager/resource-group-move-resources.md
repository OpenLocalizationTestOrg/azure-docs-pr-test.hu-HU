---
title: "Azure-erőforrások toonew előfizetés vagy az erőforrás csoport aaaMove |} Microsoft Docs"
description: "Használja az Azure Resource Manager toomove erőforrások tooa új erőforráscsoportba vagy előfizetésbe."
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
ms.openlocfilehash: 09d35f0afbbcdc0c66779f98a982d878f0807497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-resources-toonew-resource-group-or-subscription"></a>Helyezze át az erőforrásokat toonew erőforráscsoportba vagy előfizetésbe
Ez a témakör bemutatja, hogyan toomove erőforrások tooeither egy új előfizetés vagy új erőforrás csoport hello ugyanahhoz az előfizetéshez. Hello portál, a PowerShell, a Azure CLI vagy a REST API toomove erőforrás hello is használhatja. hello áthelyezési műveletek ebben a témakörben elérhető tooyou bármely Azure támogatási segítsége nélkül.

Erőforrások áthelyezésekor hello forrás csoport és a célcsoport hello zároltak hello művelet során. Írási és törlési műveletek blokkolják hello erőforráscsoportok hello áthelyezés befejezéséig. A lakat azt jelenti, nem lehet hozzáadni, frissítenie vagy törölnie hello erőforráscsoportok erőforrásokat, de nem jelent hello erőforrások sincs rögzítve. Például ha egy SQL Server és az adatbázis tooa új erőforráscsoportot, a hello adatbázis használó alkalmazások teljesen állásidő nélkül. Azt is olvashat és írhat toohello adatbázis.

Hello hello erőforrás helye nem módosítható. Egy erőforrás áthelyezése csak áthelyezi azt tooa új erőforráscsoportot. Új erőforráscsoport hello rendelkezhet egy másik helyre, de nem változtatja meg, amely hello hello erőforrás helye.

> [!NOTE]
> Ez a cikk ismerteti, hogyan toomove erőforrásokat egy meglévő Azure fiók frissítést. Az Azure-fiókjával (például frissítése használatalapú fizetéses toopre-fizetési) során a meglévő erőforrásokkal folytatott toowork Folytatás ajánlat lásd ténylegesen kívánja-e toochange [az Azure-előfizetés tooanother ajánlatot váltani](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Erőforrások áthelyezése előtt ellenőrzőlista
Nincsenek néhány fontos lépések az tooperform erőforrás áthelyezése előtt. Ezen feltételek ellenőrzésével a hibák elkerülhetőek.

1. hello forrás és cél előfizetések léteznie kell hello belül azonos [Azure Active Directory-bérlő](../active-directory/active-directory-howto-tenant.md). hogy mindkét előfizetéshez rendelkezik hello azonos toocheck bérlői azonosító, az Azure PowerShell vagy Azure CLI használata.

  Azure PowerShell használata:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  Az Azure CLI 2.0 használja:

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  Ha hello bérlői azonosítóit hello forrás és cél előfizetések nincsenek hello azonos, megkísérelheti toochange hello directory hello az előfizetéshez. Ez a beállítás azonban csak a rendelkezésre álló tooService rendszergazdák számára van bejelentkezve, akkor a Microsoft-fiók (nem lehet szervezeti fiókkal). hello directory, a toohello napló módosítása tooattempt [klasszikus portál](https://manage.windowsazure.com/), válassza ki **beállítások**, és válassza ki a hello előfizetés. Ha hello **könyvtár szerkesztése** ikon érhető el, válassza ki azt a toochange hello társított Azure Active Directoryban.

  ![címtár szerkesztése](./media/resource-group-move-resources/edit-directory.png)

  Ha erre az ikonra nem érhető el, forduljon támogatási toomove hello erőforrások tooa új bérlőt.

2. hello szolgáltatást engedélyezni kell a hello képességét toomove erőforrásokat. Ez a témakör felsorolja a szolgáltatások lehetővé teszik az erőforrások áthelyezése, és a szolgáltatások nem engedélyezi az erőforrások áthelyezése.
3. hello célelőfizetés mozgatásának hello erőforrás hello erőforrás-szolgáltató regisztrálva kell lennie. Ha nem, hibaüzenetet kap, egy adott hello feltüntetve **az előfizetés nincs regisztrálva az erőforrástípus**. Előforduló probléma erőforrás tooa új előfizetés áthelyezése, de az adott előfizetéshez még nem használta az adott erőforrástípus. toolearn hogyan toocheck hello regisztrációs állapotát, és regisztrálni az erőforrás-szolgáltató, lásd: [erőforrás-szolgáltatók és típusok](resource-manager-supported-services.md).

## <a name="when-toocall-support"></a>Ha toocall támogatja
Áthelyezheti a legtöbb erőforrást hello önkiszolgáló műveletek ebben a témakörben látható révén. Hello önkiszolgáló műveletek használata:

* Resource Manager-erőforrások áthelyezése.
* Toohello szerint hagyományos erőforrás áthelyezése [klasszikus üzembe helyezési korlátozások](#classic-deployment-limitations).

Hívja a támogatást, ha szeretné:

* Helyezze át a erőforrások tooa új Azure-fiók (és az Azure Active Directory-bérlő).
* Hagyományos erőforrások áthelyezéséhez, de tapasztal hello korlátozásokkal.

## <a name="services-that-enable-move"></a>Szolgáltatások, amelyek lehetővé teszik a áthelyezése
Most, amelyek lehetővé teszik a mozgóátlag tooboth egy új erőforráscsoportot és az előfizetés hello szolgáltatások a következők:

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
* SQL-adatbáziskiszolgáló - hello adatbázis és a kiszolgáló kell lennie, hello ugyanabban az erőforráscsoportban. Ha egy SQL server helyezi át, az adatbázisokat is kerülnek.
* Traffic Manager
* Virtuális gépek
* A virtuális gépek a Key Vault - áthelyezési toonew erőforráscsoport ugyanahhoz az előfizetéshez tárolt tanúsítvány engedélyezve van, de több előfizetés áthelyezése nem engedélyezett.
* Virtuális gépek (klasszikus) - lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* Virtual Machine Scale Sets
* Virtuális hálózatok - jelenleg, peered virtuális hálózat nem helyezhető át, amíg a Vnetben társviszony-létesítés le van tiltva. Ha le van tiltva, virtuális hálózati hello sikeresen áthelyezhető és hello Vnetben társviszony-létesítés engedélyezhető. Emellett egy virtuális hálózatot nem lehet áthelyezett tooa másik előfizetést, ha hello virtuális hálózat egyetlen alhálózatának sem az erőforrás-navigációs hivatkozásokkal tartalmazza. Például a virtuális hálózati alhálózat egy erőforrás-navigációs hivatkozás rendelkezik Microsoft.Cache redis erőforrás az alhálózaton történő telepítésekor.
* VPN Gateway


## <a name="services-that-do-not-enable-move"></a>Ne engedélyezze a move szolgáltatások
jelenleg nem engedélyezi az erőforrás áthelyezése hello szolgáltatások a következők:

* Az AD tartományi szolgáltatások
* AD hibrid Állapotfigyelő szolgáltatás
* Application Gateway
* A felügyelt lemezzel rendelkező virtuális gépek rendelkezésre állási készletek
* BizTalk Services
* Container Service
* Express Route
* DevTest Labs - áthelyezési toonew erőforráscsoport ugyanahhoz az előfizetéshez engedélyezve van, de közötti előfizetés áthelyezése nem engedélyezett.
* Dynamics LCS
* Felügyelt lemezekből lemezképeit
* Felügyelt lemezek
* Felügyelt alkalmazások
* Recovery Services-tároló - is tegye nem hello számítási, hálózati és tárolási erőforrások áthelyezése társított hello Recovery Services tároló című [helyreállítási szolgáltatások korlátozásai](#recovery-services-limitations).
* Biztonság
* Felügyelt lemezekből létrehozott pillanatfelvételek
* StorSimple Device Manager
* Felügyelt lemezzel rendelkező virtuális gépek
* Tekintse meg a virtuális hálózatok (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* Piactér-erőforrások - alapján létrehozott virtuális gépeken nem helyezhető át, előfizetések között. Meg kell toobe ebben az előfizetésben hello platformelőfizetés és hello új előfizetés újra üzembe helyezett erőforrás

## <a name="app-service-limitations"></a>App Service korlátozásai
Az App Service apps használatakor csak egy App Service-csomag nem helyezhető át. App Service apps toomove, a lehetőségek a következők:

* Az erőforrás csoport tooa új erőforráscsoport, amely még nincs az App Service-erőforrások helyezi át a hello App Service-csomag és egyéb App Service-erőforrásokat. Ez a követelmény azt jelenti, hogy akkor is kell áthelyeznie hello App Service-erőforrások, amelyek nincsenek társítva hello App Service-csomag.
* Helyezze át a hello alkalmazások tooa másik erőforráscsoportban található, de megőrizni annak összes App Service-csomagokról hello eredeti erőforráscsoportban.

hello App Service-csomag nem kell a tooreside hello ugyanazt az erőforráscsoportot a hello app toofunction hello alkalmazásként megfelelően.

Ha például az erőforráscsoport tartalmazza:

* **webalkalmazás-a** amely társítva van **terv-a**
* **webalkalmazás-b** amely társítva van **terv-b**

A lehetőségek a következők:

* Helyezze át **web-a**, **terv a**, **web-b**, és **terv-b**
* Helyezze át **web-a** és **web-b**
* Helyezze át **web-a**
* Helyezze át **web-b**

Minden más kombináció tartalmaz, amely így maradnak, amely nem hagyható mögött, amikor az App Service-csomag (bármilyen típusú App Service erőforrás) erőforrástípus.

Ha a webalkalmazás helyezkedik el, mint az App Service-csomag egy másik erőforráscsoportban található, de nem szeretnének toomove mindkét tooa új erőforráscsoportot, két lépésben hello áthelyezés kell elvégeznie. Példa:

* **webalkalmazás-a** található **webalkalmazás-csoport**
* **terv a** található **terv-csoport**
* Kívánt **web-a** és **terv a** a tooreside **kombinált csoport**

Ez a áthelyezi, két külön áthelyezés műveleteinek elvégzéséhez a következő feladatütemezési hello tooaccomplish:

1. Helyezze át a hello **web-a** túl**terv-csoport**
2. Helyezze át **web-a** és **terv a** túl**kombinált csoport**.

Áthelyezheti egy App Service tanúsítvány tooa új erőforráscsoportba vagy előfizetés probléma nélkül. Azonban ha a webalkalmazás beszerzett kívülről, és a feltöltött alkalmazás toohello SSL-tanúsítványt tartalmaz, törölnie kell a mozgóátlag hello webalkalmazás előtt hello tanúsítvány is. Például végezheti el a lépéseket követve hello:

1. Hello webalkalmazásból hello feltöltött tanúsítvány törlése
2. Helyezze át a hello webalkalmazás
3. Hello tanúsítvány toohello webalkalmazás feltöltése

## <a name="recovery-services-limitations"></a>Helyreállítási szolgáltatások korlátozásai
Helyezze át a tárolási, hálózati, nincs engedélyezve, vagy számítási erőforrások használt vész-helyreállítási tooset az Azure Site Recovery szolgáltatással.

Például tegyük fel, hogy a helyszíni gépeket tooa tárfiókja (Storage1) replikációs beállítását, és szeretné, hogy hello védett gép toocome mentése feladatátvételi tooAzure után a virtuális gép (VM1) hozzá van kapcsolva tooa virtuális hálózat (Network1). Nem helyezhető át, ezek az Azure erőforrásokat - Storage1, VM1, és Network1 - erőforrás között csoportok hello belül azonos előfizetés vagy előfizetések között.

## <a name="hdinsight-limitations"></a>A HDInsight-korlátozások

Áthelyezheti a HDInsight-fürtök tooa új előfizetéshez vagy erőforráscsoporthoz. Azonban hogy nem helyezhetők át előfizetések hello hálózati erőforrások csatolt toohello HDInsight-fürt (például hello virtuális hálózat, a hálózati adapter vagy a terheléselosztó). Ezenkívül tooa új erőforráscsoportot egy hálózati Adaptert, amely hello fürt csatolt tooa virtuális gép nem helyezhető át.

Amikor egy HDInsight fürt tooa új előfizetés helyezi át, először helyezze át más erőforrások (például hello tárfiók). Ezután helyezze át a hello HDInsight-fürt önmagában.

## <a name="classic-deployment-limitations"></a>Klasszikus üzembe helyezési korlátozásai
hello hello klasszikus modellben telepített erőforrások áthelyezésére szolgáló beállítások attól függően változnak, hogy áthelyez egy előfizetés vagy tooa új előfizetés hello erőforrásokat.

### <a name="same-subscription"></a>Ugyanahhoz az előfizetéshez
Ha erőforrások áthelyezése erőforráscsoportból egy csoport tooanother erőforrás belül hello ugyanahhoz az előfizetéshez, hello a következő korlátozások vonatkoznak:

* Nem lehet áthelyezni a virtuális hálózatok (klasszikus).
* Virtuális gépek (klasszikus) kell áthelyezni hello felhőalapú szolgáltatással.
* A felhőalapú szolgáltatás csak akkor helyezhető hello áthelyezés adattípust a virtuális gépek esetén.
* Egyszerre csak egy felhőalapú szolgáltatás helyezheti át.
* Egyszerre csak egy (klasszikus) tárfiókot helyezheti át.
* (Klasszikus) tárfiókot nem lehet áthelyezni a hello egy virtuális gép vagy egy felhőalapú szolgáltatás ugyanazt a műveletet.

toomove hagyományos erőforrások tooa új erőforráscsoport belül hello ugyanahhoz az előfizetéshez, használja a hello szabványos áthelyezési műveletek keresztül hello [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), vagy [REST API-t](#use-rest-api). Használhat hello ugyanazokat a műveleteket, mint a Resource Manager erőforrások áthelyezése.

### <a name="new-subscription"></a>Új előfizetés
Erőforrások tooa új előfizetés áthelyezésekor hello a következő korlátozások vonatkoznak:

* Minden hagyományos erőforrás hello előfizetésben át szeretné helyezni a hello ugyanazt a műveletet.
* hello célként megadott előfizetés nem tartalmazhat más hagyományos erőforrások.
* hello áthelyezése egy külön REST API-n keresztül klasszikus helyezi át a csak meg kell adniuk. hello szabványos erőforrás-kezelő áthelyezés parancsok tooa új előfizetés hagyományos erőforrás áthelyezése nem működnek.

toomove hagyományos erőforrások tooa új előfizetés, használjon hello REST műveleteinek, amelyek adott tooclassic erőforrásokat. toouse REST, hajtsa végre a következő lépéseket hello:

1. Ellenőrizze a forrás-előfizetés hello részt vehetnek-kereszt-előfizetés helyezze át. Használja a következő művelet hello:

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     A kérelem törzsében hello a következők:

  ```json
  {
    "role": "source"
  }
  ```

     hello válasz hello ellenőrzési művelet hello a következő formátumban kell megadni:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Jelölőnégyzet Ha hello célelőfizetés részt vehetnek-a-előfizetések közötti áthelyezése. Használja a következő művelet hello:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     A kérelem törzsében hello a következők:

  ```json
  {
    "role": "target"
  }
  ```

     az előfizetés érvényesítése hello formátuma azonos hello rendszer hello választ.
3. Ha mindkét előfizetéshez teljesíti az ellenőrző, minden hagyományos erőforrás áthelyezése egy előfizetés tooanother előfizetés a következő művelet hello:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    A kérelem törzsében hello a következők:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

hello művelet több percig is futhat.

## <a name="use-portal"></a>A portál használatával
toomove erőforrások hello ezen erőforrásokat tartalmazó erőforráscsoport, majd válassza ki és hello **áthelyezése** gombra.

![Erőforrások áthelyezése](./media/resource-group-move-resources/select-move.png)

Adja meg, hogy hello erőforrások tooa új erőforráscsoport vagy egy új előfizetés áthelyezni.

Válassza ki a hello erőforrások toomove és hello célként megadott erőforráscsoport. Megerősíti, hogy kell-e tooupdate parancsfájlok ezen erőforrásokat, és válassza ki **OK**. Ha hello előző lépésben kiválasztott hello Szerkesztés előfizetés ikonra, hello célelőfizetés is kell választania.

![Válassza ki a cél](./media/resource-group-move-resources/select-destination.png)

A **értesítések**, láthatja, hogy helyezze át a művelet fut. hello.

![Áthelyezési állapotának megjelenítése](./media/resource-group-move-resources/show-status.png)

Amikor befejeződött, hello eredmény értesítést kap.

![Áthelyezési eredmény megjelenítése](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>A PowerShell használata
toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello `Move-AzureRmResource` parancsot.

hello az első példában látható szövegrészt hogyan toomove egy erőforrás tooa új erőforráscsoportot.

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

hello hogyan példa azt mutatja meg a második toomove több erőforrások tooa új erőforráscsoportot.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

toomove tooa új előfizetés, hello értéket tartalmazza `DestinationSubscriptionId` paraméter.

A rendszer felkéri, hogy szeretné-e toomove hello tooconfirm megadott erőforrások.

```powershell
Confirm
Are you sure you want toomove these resources toohello resource group
'/subscriptions/{guid}/resourceGroups/newRG' hello resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a>Azure parancssori felület használatával 2.0
toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello `az resource move` parancsot. Adjon meg hello erőforrást hello erőforrások toomove azonosítói. Erőforrás-azonosítók kaphat a hello a következő parancsot:

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

hello a következő példa bemutatja, hogyan toomove tárolási fiók tooa új erőforráscsoportot. A hello `--ids` paraméter, erőforrás-azonosítók toomove hello szóközökkel elválasztott listáját tartalmazzák.

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

toomove tooa új előfizetés, adja meg a hello `--destination-subscription-id` paraméter.

## <a name="use-azure-cli-10"></a>Azure parancssori felület használatával 1.0
toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello `azure resource move` parancsot. Adjon meg hello erőforrást hello erőforrások toomove azonosítói. Erőforrás-azonosítók kaphat a hello a következő parancsot:

```azurecli
azure resource list -g sourceGroup --json
```

A következő formátumban hello visszaadó:

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

hello a következő példa bemutatja, hogyan toomove tárolási fiók tooa új erőforráscsoportot. A hello `-i` paraméter, adja meg az erőforrás-azonosítók toomove hello vesszővel tagolt listája.

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

A rendszer felkéri, hogy szeretné-e toomove hello tooconfirm megadott erőforrás.

## <a name="use-rest-api"></a>A REST API használata
toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, futtassa:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

Hello kérelemtörzset célként megadott erőforráscsoportja hello és hello erőforrások toomove kell megadni. Hello áthelyezési REST művelet kapcsolatos további információkért lásd: [erőforrások áthelyezése](https://msdn.microsoft.com/library/azure/mt218710.aspx).

## <a name="next-steps"></a>Következő lépések
* az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal toolearn lásd [Azure PowerShell használata a Resource Manager](powershell-azure-resource-manager.md).
* az előfizetés kezelésének Azure parancssori felület parancsait kapcsolatos toolearn lásd [Using hello Azure parancssori felület a Resource Manager](xplat-cli-azure-resource-manager.md).
* az előfizetés kezelésének portál funkciókkal kapcsolatos toolearn lásd [hello Azure portál toomanage erőforrásokat használó](resource-group-portal.md).
* a szervezet logikai tooyour erőforrások alkalmazásával kapcsolatos toolearn lásd [Using címkéket tooorganize az erőforrások](resource-group-using-tags.md).
