---
title: "Be annak az Azure ExpressRoute Microsoft társviszony-létesítés: PowerShell |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure útvonal szűrése a Microsoft Peering PowerShell használatával"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 395bcf7d6ad43c643ff041b154f8e4b797083e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a>Microsoft társviszony-létesítés útvonalszűrőinek konfigurálása

Útvonal-szűrők olyan módon tooconsume a Microsoft társviszony-létesítés keresztül támogatott szolgáltatások egy részhalmaza. Ez a cikk a Súgó konfigurálását és felügyeletét útvonal szűrők az ExpressRoute-Kapcsolatcsoportok hello szükséges lépések.

Dynamics 365 szolgáltatások és a vállalati, például az Exchange Online, SharePoint Online és Skype Office 365-szolgáltatásokhoz hello Microsoft társviszony-létesítés keresztül érhetők el. Ha a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot van konfigurálva, az összes előtagok kapcsolódó toothese szolgáltatás van-e hirdetve keresztül létesített hello BGP-munkamenetek. A BGP közösségi értéke csatolt tooevery előtag tooidentify hello ajánlott szolgáltatás hello előtag keresztül. Hello BGP logikai értékeket és rendeli hello szolgáltatások listáját lásd: [BGP Közösségek](expressroute-routing.md#bgp).

Ha a kapcsolat tooall szolgáltatások van szüksége, nagyszámú előtagok van-e hirdetve BGP keresztül. Ez jelentősen növeli a belül a hálózati útválasztók által fenntartott hello útvonaltáblák hello méretét. Ha azt tervezi, hogy csak a szolgáltatások egy részhalmaza kínált Microsoft társviszony-létesítés tooconsume, csökkentheti az útvonaltáblák kétféleképpen hello méretét. A következőket teheti:

- Nem kívánt előtagok szűrheti a BGP Közösségek útvonal szűrők alkalmazásával. Ez egy szabványos hálózatkezelési eljárás, és sok hálózatok általában arra használják.

- Útvonal-szűrők, és alkalmazni azokat tooyour ExpressRoute-kapcsolatcsoportot. Útvonal szűrő egy új erőforrást kiválaszthatja hello listája szolgáltatások, tervezze meg a Microsoft társviszony-létesítés tooconsume. Csak az ExpressRoute útválasztók továbbítják hello útvonal szűrő toohello szolgáltatás tartozó előtagok hello listája.

### <a name="about"></a>Útvonal-szűrők

Ha a Microsoft társviszony-létesítés konfigurálva van az ExpressRoute-kapcsolatcsoportot, hello Microsoft peremhálózati útválasztók létesíteni két BGP-munkameneteket indíthat más hello peremhálózati útválasztók (saját vagy a kapcsolat szolgáltatóját). Nincs útvonal hirdetett tooyour hálózati. tooenable útvonal hirdetmények tooyour hálózati, társítania kell egy útvonal-szűrőt.

Útvonal szűrő lehetővé teszi azt szeretné, hogy az ExpressRoute-kapcsolatcsoportot Microsoft társviszony-létesítés keresztül tooconsume szolgáltatás azonosítására. Lényegében egy fehér lista hello BGP közösségi értékek is. Amikor egy útvonal szűrő erőforrás van definiálva, és tooan ExpressRoute-kapcsolatcsoportot csatlakoztatva, összes toohello BGP közösségi értékek megfeleltetni előtagok hirdetett tooyour hálózati.

toobe képes tooattach útvonal szűrők az Office 365 szolgáltatásaival rajtuk, rendelkeznie kell engedélyezési tooconsume Office 365-szolgáltatásokhoz ExpressRoute keresztül. Ha nem engedélyezett tooconsume Office 365-szolgáltatások díjairól ExpressRoute, hello művelet tooattach útvonal szűrők sikertelen lesz. Hello engedélyezési folyamattal kapcsolatos további információkért lásd: [Azure ExpressRoute az Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). Kapcsolat tooDynamics 365 használatához nem szükséges az előzetes engedélyek.

> [!IMPORTANT]
> Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni a Microsoft társviszony-létesítést, meghirdetett összes szolgáltatás előtagot, akkor is, ha nincsenek megadva útvonal szűrők. A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön.
> 
> 

### <a name="workflow"></a>Munkafolyamat

toobe képes toosuccessfully tooservices csatlakozhat a Microsoft társviszony-létesítést, végre kell hajtania a következő konfigurációs lépések hello:

- Rendelkeznie kell egy aktív van a Microsoft társviszony kiosztott ExpressRoute-kapcsolatcsoportot. A következő utasításokat tooaccomplish hello használhatja ezeket a feladatokat:
  - [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.
  - [Hozzon létre a Microsoft társviszony-létesítés](expressroute-circuit-peerings.md) hello BGP-közvetlenül munkamenet kezelése. Vagy a kapcsolat szolgáltatójánál rendelkezik a kör társviszony Microsoft kiépítéséhez.

-  Hozzon létre és útvonal-szűrő konfigurálnia kell.
    - Azonosítsa hello szolgáltatásokhoz, a Microsoft társviszony-létesítés keresztül tooconsume
    - Azonosítsa a BGP-Közösség értékek hello szolgáltatásokkal társított hello listája
    - Hozzon létre egy szabály tooallow hello előtag lista egyező hello BGP logikai értékek

-  Hello útvonal szűrő toohello ExpressRoute-kapcsolatcsoportot kell csatolnia.

## <a name="before-you-begin"></a>Előkészületek

A konfigurálás elkezdése előtt ellenőrizze a következő feltételek hello:

 - Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez. További információkért lásd: [telepítése és konfigurálása az Azure PowerShelll](/powershell/azure/install-azurerm-ps).

  > [!NOTE]
  > Hello legújabb verzió letöltéséhez hello PowerShell-galériában, hanem hello Installer használatával. Telepítő hello jelenleg nem támogatja a szükséges hello parancsmagok.
  > 

 - Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.

 - Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.

 - Rendelkeznie kell egy aktív Microsoft társviszony-létesítés. Kövesse az utasításokat, [létrehozása, és társviszony-létesítési konfigurációjának módosítása](expressroute-circuit-peerings.md)

### <a name="log-in-tooyour-azure-account"></a>Jelentkezzen be tooyour Azure-fiók

Ez a konfiguráció megkezdése előtt be kell jelentkeznie tooyour Azure-fiók. hello parancsmag kéri hello bejelentkezési hitelesítő adatait az Azure-fiókjával. A bejelentkezés után az tölti le a fiók beállításait,-e elérhető tooAzure PowerShell.

Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon tooyour fiók. A következő példa toohelp csatlakozás hello használata:

```powershell
Login-AzureRmAccount
```

Ha több Azure-előfizetéssel rendelkezik, ellenőrizze a hello fiókhoz hello előfizetések.

```powershell
Get-AzureRmSubscription
```

Adja meg, hogy szeretné-e toouse hello előfizetés.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="prefixes"></a>1. lépés. Előtagok és BGP közösségi értékek listájának beolvasása

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP-Közösség értékek listájának beolvasása

A következő parancsmag tooget hello elérhető a Microsoft társviszony-létesítés szolgáltatásokkal társított BGP közösségi értékek listája hello használja, és előtaglistát kezel a hozzájuk társított runbookokat hello:

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a>2. Ellenőrizze a hello értékből álló lista, amelyet az toouse

A BGP kívánt logikai értékek listáját toouse teszi hello útvonal szűrő. Tegyük fel hello BGP közösségi érték Dynamics 365 szolgáltatások 12076:5040.

## <a name="filter"></a>2. lépés. Útvonal-szűrő, ezért a szűrési szabály létrehozása

Útvonal szűrő lehet csak egy szabályt, és hello szabály a "Engedélyezés" típusúnak kell lennie. Ez a szabály társítva BGP közösségi értékből álló lista lehet.

### <a name="1-create-a-route-filter"></a>1. Útvonal szűrő létrehozása

Először hozzon létre hello útvonal szűrő. "New-AzureRmRouteFilter" Hello parancs csak egy útvonal-szűrő erőforrás hoz létre. Hello erőforrás létrehozása után meg kell majd hozzon létre egy szabályt és mellékelje toohello útvonal szűrő objektum. Futtassa a következő parancs toocreate hello útvonal szűrő erőforrás:

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Állapotszűrő szabály létrehozása

Megadhat egy készletében BGP hajtsa végre egy vesszővel tagolt lista formájában, hello példában látható módon. Futtassa a következő parancs toocreate hello egy új szabályt:
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-hello-rule-toohello-route-filter"></a>3. Hello szabály toohello útvonal-szűrő hozzáadása

Futtassa a következő parancs tooadd hello szűrési szabály toohello útvonal szűrő hello:
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="attach"></a>3. lépés. Hello útvonal szűrő tooan ExpressRoute-kapcsolatcsoportot csatolása

Futtassa a következő parancs tooattach hello útvonal szűrő toohello ExpressRoute-kapcsolatcsoportot, feltéve, hogy csak a Microsoft társviszony-létesítés rendelkezik hello:

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="getproperties"></a>egy útvonal szűrő tooget hello tulajdonságait

egy útvonal szűrő tooget hello tulajdonságait a lépéseket követve hello használata:

1. Futtassa a következő parancs tooget hello útvonal szűrő erőforrás hello:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. Hello útvonal Állapotszűrő szabályok lekérése hello útvonal-szűrő erőforrás hello a következő parancs futtatásával:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <a name="updateproperties"></a>egy útvonal szűrő tooupdate hello tulajdonságait

Ha hello útvonal szűrő már hozzá van rendelve tooa áramkör, frissítések toohello BGP közösségi lista automatikusan tölti ki a megfelelő előtaggal hirdetmény módosítások létrehozott hello BGP-munkamenetek keresztül. Hello BGP közösségi listája az útvonal szűrő hello a következő parancs használatával frissítheti:

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="detach"></a>az ExpressRoute-kapcsolatcsoportot útvonal szűrő toodetach

Miután egy útvonal szűrő le van választva, az ExpressRoute-kapcsolatcsoportot hello, előtagok nélkül van-e hirdetve hello BGP-kapcsolaton keresztül. Útvonal szűrő használatával a következő parancs hello ExpressRoute-kapcsolatcsoportot az választhatják le:
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="delete"></a>toodelete útvonal szűrő

Csak a törlés egy útvonal-szűrő, ha nem csatlakoztatott tooany kör is. Győződjön meg arról, hogy hello útvonal szűrő nem csatlakoztatott tooany áramkör toodelete megkísérlése előtt azt. A következő parancs hello útvonal szűrő törlése:

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Következő lépések

ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
