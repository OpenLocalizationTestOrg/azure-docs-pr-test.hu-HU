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
ms.date: 10/05/2017
ms.author: tomfitz
ms.openlocfilehash: 7d500d20dcce3e472e3e1e15b9ce307874caf22a
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/22/2018
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe

Ez a cikk bemutatja, hogyan kell helyeznie az erőforrásokat az új előfizetés vagy egy új erőforráscsoportot ugyanahhoz az előfizetéshez. A portál, PowerShell, az Azure parancssori felület vagy a REST API használatával erőforrás áthelyezése. Ez a cikk az áthelyezési műveletek rendelkezésére álljanak a segítség nélkül az Azure-támogatás.

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
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Azure CLI esetén használja az alábbi parancsot:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Ha a bérlő azonosítók a forrás és cél előfizetésekhez nem egyeznek, vegye fel a kapcsolatot [támogatja](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) az erőforrások áthelyezése új bérlőhöz.

2. A szolgáltatásnak lehetővé kell tennie az erőforrások áthelyezését. Ez a cikk felsorolja a szolgáltatások lehetővé teszik az erőforrások áthelyezése, és a szolgáltatások nem engedélyezi az erőforrások áthelyezése.
3. A cél előfizetést regisztrálni kell az áthelyezett erőforrás erőforrás-szolgáltatóján. Ha nem, hibaüzenet arról, hogy a **az előfizetés nincs regisztrálva az erőforrástípus**. Ez a probléma erőforrások új előfizetésre történő áthelyezésekor fordulhat elő, ha az előfizetést még nem használták az adott erőforrástípushoz.

  A PowerShell környezethez használja az alábbi parancsokat a regisztrációs állapot lekérdezése céljából:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Egy erőforrás-szolgáltató regisztrálásához használhatja:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
  ```

  Azure CLI használata esetén használja a regisztrációs állapot lekérdezése céljából a következő parancsokat:

  ```azurecli-interactive
  az account set -s <destination-subscription-name-or-id>
  az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
  ```

  Egy erőforrás-szolgáltató regisztrálásához használhatja:

  ```azurecli-interactive
  az provider register --namespace Microsoft.Batch
  ```

## <a name="when-to-call-support"></a>Mikor érdemes az ügyfélszolgálat

Áthelyezheti a legtöbb erőforrást ebben a cikkben szereplő önkiszolgáló műveletek révén. Használja az önkiszolgáló műveletek:

* Resource Manager-erőforrások áthelyezése.
* Hagyományos erőforrások áthelyezéséhez a [klasszikus üzembe helyezési korlátozások](#classic-deployment-limitations).

Ügyfél [támogatja](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) kell tennie:

* Az erőforrások áthelyezése egy új Azure-fiók (és az Azure Active Directory-bérlő).
* Hagyományos erőforrások áthelyezéséhez, de problémát tapasztal a korlátozásokkal.

## <a name="services-that-enable-move"></a>Szolgáltatások, amelyek lehetővé teszik a áthelyezése

A szolgáltatások, amelyek lehetővé teszik egy új erőforráscsoportot és az előfizetés áthelyezését a következők:

* API Management
* App Service apps (webalkalmazások) – lásd: [App Service korlátozásai](#app-service-limitations)
* Application Insights
* Automatizálás
* Azure Cosmos DB
* Batch
* Bing Térképek
* Tartalomkézbesítési hálózat (CDN)
* A felhőalapú szolgáltatások – lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* Cognitive Services
* Tartalommoderátor
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Event Hubs
* A HDInsight-fürtök - Lásd [HDInsight korlátozásai](#hdinsight-limitations)
* IoT Hubok
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
* Tárolás
* Tekintse meg a tároló (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* A Stream Analytics - feladatok nem helyezhető át, ha a Stream Analytics állapotban.
* SQL-adatbáziskiszolgáló - adatbázis és a kiszolgáló ugyanabban az erőforráscsoportban kell lennie. Ha egy SQL server helyezi át, az adatbázisokat is kerülnek.
* Traffic Manager
* Virtuális gépek - felügyelt lemezzel rendelkező virtuális gép nem helyezhető át. Lásd: [virtuális gépek korlátozásai](#virtual-machines-limitations)
* Virtuális gépek (klasszikus) - lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)
* Tekintse meg a virtuálisgép-méretezési csoportok - [virtuális gépek korlátozásai](#virtual-machines-limitations)
* Tekintse meg a virtuális hálózatok - [virtuális hálózatok korlátozásai](#virtual-networks-limitations)
* VPN Gateway

## <a name="services-that-do-not-enable-move"></a>Ne engedélyezze a move szolgáltatások

A szolgáltatások, amelyek jelenleg nem engedélyezi az erőforrás áthelyezése a következők:

* AD Domain Services
* AD hibrid Állapotfigyelő szolgáltatás
* Application Gateway
* BizTalk Services
* Container Service
* Express Route
* DevTest Labs - helyezze át az új erőforráscsoport ugyanahhoz az előfizetéshez engedélyezve van, de több előfizetés áthelyezése nem engedélyezett.
* Dynamics LCS
* Felügyelt alkalmazások
* Tekintse meg a felügyelt lemezek - [virtuális gépek korlátozásai](#virtual-machines-limitations)
* Recovery Services-tároló - is do helyezi át a számítási, hálózati és tárolási erőforrásokat, a Recovery Services-tároló társított lásd [helyreállítási szolgáltatások korlátozásai](#recovery-services-limitations).
* Biztonság
* StorSimple-eszközkezelő
* Tekintse meg a virtuális hálózatok (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Virtuális gépek korlátozásai

Felügyelt lemezek nem támogatják az áthelyezés. Ez a korlátozás, az azt jelenti, hogy néhány kapcsolódó erőforrások túl nem helyezhető át. Nem helyezhető át:

* Felügyelt lemezek
* A felügyelt lemezzel rendelkező virtuális gépek
* Felügyelt lemezekből lemezképeit
* Felügyelt lemezek készült pillanatképek
* A felügyelt lemezzel rendelkező virtuális gépek rendelkezésre állási készletek

Piactér-erőforrások alapján létrehozott virtuális gépeken nem helyezhető át, előfizetések között. A virtuális gép az aktuális előfizetésben kiosztásának megszüntetése, majd telepítse újra az új előfizetés.

A virtuális gépek a Key Vault tárolt tanúsítvány helyezheti át egy új erőforráscsoportot ugyanazt az előfizetést, de nem előfizetések között.

## <a name="virtual-networks-limitations"></a>Virtuális hálózatok korlátozásai

Peered virtuális hálózat áthelyezni, előbb le kell tiltania a virtuális hálózati társviszony. Ha le van tiltva, áthelyezheti a virtuális hálózat. Az áthelyezés után újból engedélyezi, a virtuális hálózati társviszony.

Virtuális hálózat nem helyezhető át egy másik előfizetésben található, ha a virtuális hálózati alhálózat erőforrás-navigációs hivatkozásokkal tartalmaz. Például ha a Redis Cache erőforrás egy alhálózatba van telepítve, alhálózaton erőforrás navigációs hivatkozást tartalmaz.

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

## <a name="recovery-services-limitations"></a>Helyreállítási szolgáltatások korlátozásai

Helyezze át a tárolási, hálózati, nincs engedélyezve, vagy számítási erőforrásokat az Azure Site Recovery vész-helyreállítási telepítéséhez használt.

Tegyük fel például, hogy állította be a tárfiók (Storage1) helyszíni gépek replikációja, és szeretné, hogy a védett gép elérni a feladatátvételt követően az Azure-ba (Network1) virtuális hálózathoz csatlakozó virtuális gépként (VM1). Nem helyezhető át - Storage1 VM1 és Network1 - e az Azure erőforrások bármelyike erőforráscsoportok egyazon előfizetésen belül vagy az előfizetések.

A regisztrált virtuális gép áthelyezése **Azure biztonsági mentés** erőforráscsoportba:
 1. Ideiglenesen állítsa le a biztonsági mentés és a biztonsági mentési adatok megőrzése mellett
 2. A virtuális gép áthelyezése a célként megadott erőforráscsoportja
 3. Védelmének újbóli beállításához, a felhasználók állíthatja vissza az áthelyezés előtt létrehozott rendelkezésre visszaállítási pontok azonos/új tárolóban.
Ha a felhasználó a biztonsági másolat virtuális Gépet egy előfizetések között, 1 és 2. lépést változatlanok maradnak. 3. lépésben a felhasználónak kell egy új tárolóban található / a célként megadott előfizetés-ben jött létre a virtuális gép védelme. Helyreállítási szolgáltatások tárolóban nem tartalmazza támogatás előfizetés biztonsági mentések közötti.

## <a name="hdinsight-limitations"></a>A HDInsight-korlátozások

A HDInsight-fürtök áthelyezése egy új előfizetéshez vagy erőforráscsoporthoz. Azonban nem helyezhető át a hálózati erőforrások (például a virtuális hálózat, a hálózati adapter vagy a terheléselosztó) a HDInsight-fürthöz kapcsolódó előfizetések között. Ezenkívül nem helyezhető át egy új erőforráscsoportot egy hálózati Adaptert, amely a fürt virtuális gép csatlakozik.

Amikor egy új előfizetés helyezi át a HDInsight-fürtöt, először helyezze át az egyéb erőforrások (például a tárfiók). Ezután helyezze át a HDInsight-fürt önmagában.

## <a name="search-limitations"></a>Keresési korlátozásai

Helyezni különböző régiókban egyszerre több keresési erőforrások nem helyezhető át.
Ebben az esetben áthelyezése külön kell.

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

Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a [Move-AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) parancsot. A következő példa bemutatja, hogyan több erőforrás áthelyezése egy új erőforráscsoportot.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Helyezze át az új előfizetés, tartalmazza a értéket a `DestinationSubscriptionId` paraméter.

## <a name="use-azure-cli"></a>Az Azure parancssori felület használatával

Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a [az erőforrás-áthelyezés](/cli/azure/resource?view=azure-cli-latest#az_resource_move) parancsot. Adja meg az erőforrás-azonosítók az erőforrások áthelyezése. A következő példa bemutatja, hogyan több erőforrás áthelyezése egy új erőforráscsoportot. Az a `--ids` paramétert, az erőforrás-azonosítók áthelyezése szóközökkel elválasztott listáját tartalmazzák.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Helyezze át az új előfizetés, adja meg a `--destination-subscription-id` paraméter.

## <a name="use-rest-api"></a>A REST API használata

Meglévő erőforrások áthelyezése egy másik erőforráscsoportba vagy előfizetésbe, futtassa:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

A kérelem törzsében meg a célként megadott erőforráscsoportja és az erőforrások áthelyezése. Az áthelyezési REST művelet kapcsolatos további információkért lásd: [erőforrások áthelyezése](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>További lépések

* Az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal kapcsolatban lásd: [Azure PowerShell használata a Resource Manager](powershell-azure-resource-manager.md).
* Az előfizetés kezelésének Azure parancssori felület parancsait kapcsolatos további tudnivalókért lásd: [az Azure parancssori felület használatával a Resource Manager](xplat-cli-azure-resource-manager.md).
* Az előfizetés kezelésének portál funkciókkal kapcsolatos további tudnivalókért lásd: [-erőforrások kezeléséhez Azure portál használatával](resource-group-portal.md).
* Az erőforrások logikus alkalmazásával kapcsolatos további tudnivalókért lásd: [az erőforrások rendszerezése címkék használatával](resource-group-using-tags.md).
