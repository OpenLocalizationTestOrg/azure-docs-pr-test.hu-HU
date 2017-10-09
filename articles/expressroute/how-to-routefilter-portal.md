---
title: "Be annak az Azure ExpressRoute Microsoft társviszony-létesítés: portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure útvonal szűrők Microsoft Peering használatára vonatkozó hello Azure-portálon"
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
ms.date: 08/25/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 2a47d465ec5f175d9510cef94606f70f036f0862
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
  - [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.
  - [Hozzon létre a Microsoft társviszony-létesítés](expressroute-howto-routing-portal-resource-manager.md) hello BGP-közvetlenül munkamenet kezelése. Vagy a kapcsolat szolgáltatójánál rendelkezik a kör társviszony Microsoft kiépítéséhez.

-  Hozzon létre és útvonal-szűrő konfigurálnia kell.
    - Azonosítsa hello szolgáltatásokhoz, a Microsoft társviszony-létesítés keresztül tooconsume
    - Azonosítsa a BGP-Közösség értékek hello szolgáltatásokkal társított hello listája
    - Hozzon létre egy szabály tooallow hello előtag lista egyező hello BGP logikai értékek

-  Hello útvonal szűrő toohello ExpressRoute-kapcsolatcsoportot kell csatolnia.

## <a name="before-you-begin"></a>Előkészületek

A konfigurálás elkezdése előtt ellenőrizze a következő feltételek hello:

 - Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.

 - Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot kiépített és engedélyezett állapotban kell lennie.

 - Rendelkeznie kell egy aktív Microsoft társviszony-létesítés. Kövesse az utasításokat, [létrehozása, és társviszony-létesítési konfigurációjának módosítása](expressroute-howto-routing-portal-resource-manager.md)


## <a name="prefixes"></a>1. lépés. Előtagok és BGP közösségi értékek listájának beolvasása

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP-Közösség értékek listájának beolvasása

Elérhető a Microsoft társviszony-létesítés szolgáltatásokkal társított BGP közösségi értékek érhető el hello [ExpressRoute útválasztási követelmények](expressroute-routing.md) lap.

### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a>2. Ellenőrizze a hello értékből álló lista, amelyet az toouse

A BGP kívánt logikai értékek listáját toouse teszi hello útvonal szűrő. Tegyük fel hello BGP közösségi érték Dynamics 365 szolgáltatások 12076:5040.

## <a name="filter"></a>2. lépés. Útvonal-szűrő, ezért a szűrési szabály létrehozása

Útvonal szűrő lehet csak egy szabályt, és hello szabály a "Engedélyezés" típusúnak kell lennie. Ez a szabály társítva BGP közösségi értékből álló lista lehet.

### <a name="1-create-a-route-filter"></a>1. Útvonal szűrő létrehozása
Létrehozhat egy útvonalat szűrőt hello beállítás toocreate kiválasztásával egy új erőforrást. Kattintson a **új** > **hálózati** > **RouteFilter**, ahogy az a következő kép hello:

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

Hello útvonal szűrő erőforráscsoportban kell elhelyezni. 

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. Állapotszűrő szabály létrehozása

Adhat hozzá, és frissítési szabályok hello kiválasztásával kezelheti a útvonal szűrési szabály lapján.

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


Kiválaszthatja a hello tooconnect toofrom hello kívánt szolgáltatásokban legördülő listából, és végzett hello szabály mentéséhez.

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <a name="attach"></a>3. lépés. Hello útvonal szűrő tooan ExpressRoute-kapcsolatcsoportot csatolása

Csatolhat hello útvonal szűrő tooa kör kiválasztásával hello "kör felvétele" gombra, majd válassza a hello ExpressRoute-kapcsolatcsoportot hello legördülő listából.

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

## <a name="getproperties"></a>egy útvonal szűrő tooget hello tulajdonságait

Hello erőforrás hello portál megnyitásakor egy útvonal szűrő tulajdonságait tekintheti meg.

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


## <a name="updateproperties"></a>egy útvonal szűrő tooupdate hello tulajdonságait

Hello "kezelése szabály" gombra kattintva frissítheti a BGP közösségi értékek csatolt tooa áramkör hello listája.


![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


## <a name="detach"></a>az ExpressRoute-kapcsolatcsoportot útvonal szűrő toodetach

hello útvonal szűrő, az expressroute-kapcsolatcsoporthoz toodetach hello körön kattintson a jobb gombbal, majd kattintson a "társítását".

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


## <a name="delete"></a>toodelete útvonal szűrő

Útvonal szűrő hello törlése gombra kattintva törölheti. 

![Útvonal szűrő létrehozása](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a>Következő lépések

ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
