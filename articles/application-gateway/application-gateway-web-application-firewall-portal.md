---
title: "Hozható létre vagy frissíthető egy Azure Application Gateway a webalkalmazási tűzfal |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy alkalmazás webalkalmazási tűzfal a portál használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: 650f26d19615d27a94f3947aad7b7904b6c1fabc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Hozzon létre egy alkalmazás webalkalmazási tűzfal a portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Útmutató a webes alkalmazás engedélyezve van a tűzfal alkalmazás átjáró létrehozásához.

A webes alkalmazás tűzfalat (WAF) Azure Application Gateway webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának. Webes alkalmazás számos OWASP felső 10 közös webes biztonsági rések elleni védelmet nyújt.

## <a name="scenarios"></a>Forgatókönyvek

Ebben a cikkben van két olyan eset:

Az első forgatókönyv, elsajátíthatja, hogy [webalkalmazási tűzfal Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall)

A második forgatókönyv szerint, elsajátíthatja, hogy [webalkalmazási tűzfal hozzáadása egy meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway).

![Példaforgatókönyv][scenario]

> [!NOTE]
> Az Alkalmazásátjáró, beleértve az egyéni állapotfigyelő további konfigurációs vizsgálat, háttér címkészletet címeket, és további szabályok is konfigurálva van az Alkalmazásátjáró konfigurálása után, és nem a kezdeti üzembe helyezése során.

## <a name="before-you-begin"></a>Előkészületek

Az Azure Application Gateway saját alhálózatba van szükség. Amikor egy virtuális hálózat létrehozásával, győződjön meg arról, hogy hagyja-e elég hely a cím több alhálózattal rendelkezik. Miután telepít egy alhálózatra Alkalmazásátjáró, csak további alkalmazásátjárót képesek fel kell venni az alhálózat.

##<a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Webalkalmazási tűzfal hozzáadása egy meglévő Alkalmazásátjáró

Ez a példa frissíti a meglévő Alkalmazásátjáró megelőzési módban webalkalmazási tűzfal támogatásához.

1. Az Azure Portal **Kedvencek** panelén kattintson az **Összes erőforrás** elemre. Kattintson a meglévő alkalmazás-átjáró a **összes erőforrás** panelen. Ha már kijelölt előfizetésben több erőforrást tartalmaz, a beírása a **Szűrés név alapján...** mezőbe.

   ![Alkalmazásátjáró létrehozása][1]

1. Kattintson a **webalkalmazási tűzfal** és az alkalmazás-átjáró beállításainak frissítése. Ha végzett a kattintson **mentése**

    Webalkalmazási tűzfal támogatásához meglévő Alkalmazásátjáró frissítése a beállítások a következők:

   | **Beállítás** | **Érték** | **Részletek**
   |---|---|---|
   |**Frissítési WAF réteghez**| Bejelölve | Az Alkalmazásátjáró WAF csomagra szintjének állít be.|
   |**Tűzfal állapota**| Engedélyezve | Ez a beállítás lehetővé teszi, hogy a WAF a tűzfalat.|
   |**Tűzfal módban** | Megelőzés | Ez azért, hogy a webalkalmazási tűzfal hogyan kezelje a forgalmat. **Észlelési** mód csak naplózza az eseményeket, ahol **megelőzési** mód az eseményeket naplózza, és leállítja a forgalmat.|
   |**Szabálykészlete**|3.0|Ez a beállítás meghatározza, hogy a [szabálykészlet alapvető](application-gateway-web-application-firewall-overview.md#core-rule-sets) , hogy a háttér a készlet tagjainak védelmére szolgál.|
   |**Letiltott szabályok konfigurálása**|Változó|Lehetséges téves elkerülése érdekében ez a beállítás lehetővé teszi, hogy, hogy tiltsa le az egyes [szabályok és a csoportok](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Meglévő Alkalmazásátjáró a WAF-re való frissítéskor a Termékváltozat-méretét vált **Közepes**. Ez is újra kell konfigurálni konfiguráció befejezése után.

    ![megjelenítő alapvető beállítások panel][2-1]

    > [!NOTE]
    > Webes alkalmazás tűzfal naplók megtekintéséhez a diagnosztika engedélyeznie kell, és ApplicationGatewayFirewallLog kiválasztva. Tesztelési célokra példányszámának 1 választható ki. Fontos tudni, hogy bármely példányok száma alapján két példányt nem mutatja be az SLA-t, és ezért nem ajánlott. Kis átjárók webalkalmazási tűzfal használata esetén nem érhetők el.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Webalkalmazási tűzfal Alkalmazásátjáró létrehozása

Ez a forgatókönyv tartalma:

* Hozzon létre egy közepes méretű webes alkalmazás tűzfal Alkalmazásátjáró két példányt.
* Nevű AdatumAppGatewayVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.
* Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.
* Kiszervezési SSL tanúsítvány konfigurálása.

1. Jelentkezzen be az [Azure portálra](https://portal.azure.com). Ha már nincs fiókja, regisztrálhat az egy [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/free)
1. Kattintson a Kedvencek ablaktáblában, a portál, **új**
1. Az **Új** panelen kattintson a **Hálózat** elemre. Az a **hálózati** panelen kattintson a **Alkalmazásátjáró**, a következő ábrán látható módon:
1. Lépjen az Azure-portálon, kattintson **új** > **hálózati** > **Alkalmazásátjáró**

    ![Alkalmazásátjáró létrehozása][1]

1. Az a **alapjai** panelen megjelenő, adja meg a következő értékeket, majd kattintson a **OK**:

   | **Beállítás** | **Érték** | **Részletek**
   |---|---|---|
   |**Name (Név)**|AdatumAppGateway|Az Alkalmazásátjáró neve|
   |**Réteg**|WAF|Lehetséges értékek a következők: Standard és a WAF. Látogasson el [webalkalmazási tűzfal](application-gateway-web-application-firewall-overview.md) WAF tájékozódhat.|
   |**Termékváltozat-méretét**|Közepes|Standard csomag kiválasztásakor használhatók kis, közepes és nagy. WAF réteg kiválasztásakor beállítások: Közepes és nagy csak.|
   |**A példányok száma**|2|A magas rendelkezésre álláshoz az Alkalmazásátjáró példányainak száma. Példányok számát 1 csak tesztelési célokra használható.|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon ki egy előfizetést, amelyben létrehozza az alkalmazásátjárót.|
   |**Erőforráscsoport**|**Új:** AdatumAppGatewayRG|Hozzon létre egy erőforráscsoportot. Az erőforráscsoport nevének egyedinek kell lennie a kiválasztott előfizetésen belül. Az erőforráscsoportokkal kapcsolatos további információkért olvassa el [A Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) áttekintése című cikket.|
   |**Hely**|USA nyugati régiója||

   ![megjelenítő alapvető beállítások panel][2-2]

1. Az a **beállítások** panel alatt megjelenő **virtuális hálózati**, kattintson a **virtuális hálózatot választ**. Ez a lépés nyílik meg a **válasszon virtuális hálózati** panelen.  Kattintson a **hozzon létre új** megnyitásához a **virtuális hálózat létrehozása** panelen.

   ![Válasszon egy virtuális hálózatot][2]

1. Az a **hozzon létre virtuális hálózat panel** adja meg a következő értékeket, majd kattintson a **OK**. Ez a lépés bezárja a **virtuális hálózat létrehozása** és **válasszon virtuális hálózati** paneleken. Ez kitölti a **alhálózati** található a **beállítások** a kiválasztott alhálózat panelről.

   |**Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|AdatumAppGatewayVNET|Az Alkalmazásátjáró neve|
   |**Címterület**|10.0.0.0/16| Ez az érték a virtuális hálózat a címtér|
   |**Alhálózat neve**|AppGatewaySubnet|Az Alkalmazásátjáró az alhálózat nevét|
   |**Alhálózati címtartomány**|10.0.0.0/28 | Ez az alhálózat lehetővé teszi több további alhálózatokat a virtuális hálózat háttér a készlet tagjainak|

1. Az a **beállítások** részen **előtérbeli IP-konfiguráció**, válassza a **nyilvános** , a **IP-cím típusa**

1. A a **beállítások** részen **nyilvános IP-cím**, kattintson a **egy nyilvános IP-cím kiválasztása**, ebben a lépésben megnyitja a **nyilvános IP-cím kiválasztása** panelen kattintson a **hozzon létre új**.

   ![Válassza ki a nyilvános IP-cím][3]

1. Az a **nyilvános IP-cím létrehozása** panelen fogadja el az alapértelmezett értéket, és kattintson a **OK**. Ebben a lépésben bezárja a **nyilvános IP-cím kiválasztása** panelen a **nyilvános IP-cím létrehozása** panelt, és tölthet **nyilvános IP-cím** kiválasztott nyilvános IP-címmel.

1. Az a **beállítások** részen **figyelő konfigurációs**, kattintson a **HTTP** alatt **protokoll**. Használandó **https**, egy tanúsítványra szükség. A tanúsítvány titkos kulcsát egy .pfx exportálja a tanúsítványt kell megadni, és a fájlhoz tartozó jelszót, van szükség.

1. Konfigurálja a **WAF** vonatkozó beállításokat.

   |**Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Tűzfal állapota**| Engedélyezve| Ez a beállítás be- vagy kikapcsolja a WAF.|
   |**Tűzfal módban** | Megelőzés| Ez a beállítás meghatározza, hogy a műveletek WAF időt vesz igénybe, a rosszindulatú forgalom. Ha **észlelési** választása esetén csak kerül a forgalmat.  Ha **megelőzési** van kiválasztva, forgalmat a rendszer naplózza, és leállt a 403-as jogosulatlan választ.|


1. Az összefoglalás lapon tekintse át, és kattintson a **OK**.  Az Alkalmazásátjáró most sorba és létre.

1. Az Alkalmazásátjáró létrehozása után, nyissa meg azt az alkalmazás-átjáró konfigurációs folytatja a portálon.

    ![Alkalmazás átjáró erőforrás-kihasználása nézetét][10]

Ezeket a lépéseket egy alapszintű application gateway a figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok az alapértelmezett beállításokkal hozza létre. Módosíthatja a beállításokat a központi telepítés megfelelően, ha a kiépítés sikeres

> [!NOTE]
> Az alapszintű webes alkalmazás tűzfal konfigurációját létre alkalmazásátjárót védelmet CRS 3.0-val van állítva.

## <a name="next-steps"></a>Következő lépések

A következő megismerheti a tartozó egyéni tartomány alias beállítása a [nyilvános IP-cím](../dns/dns-custom-domain.md#public-ip-address) használata az Azure DNS- vagy egy másik DNS-szolgáltatónál.

Diagnosztikai naplózás ellátogatva jelentkezzen az eseményeket, és megakadályozta webalkalmazási tűzfal konfigurálásával [Alkalmazásdiagnosztika átjáró](application-gateway-diagnostics.md)

Megtudhatja, hogyan hozzon létre egyéni állapotteljesítmény ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)

Megtudhatja, hogyan konfigurálja az SSL-Feladatkiszervezést, és tegye meg a költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
