---
title: "aaaCreate vagy Azure Alkalmazásátjáró frissítése a webalkalmazási tűzfal |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Alkalmazásátjáró webalkalmazási tűzfal használatával hello portál"
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
ms.openlocfilehash: 68d140fef14499da654ea251d1208e6a800f55a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-web-application-firewall-by-using-hello-portal"></a>Hozzon létre egy alkalmazás webalkalmazási tűzfal hello portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Ismerje meg, hogyan toocreate webalkalmazási tűzfal engedélyezve van az Alkalmazásátjáró.

hello webalkalmazási tűzfal (waf-ot) az Azure alkalmazás átjáró webalkalmazások védje a közös web-alapú támadások, például az SQL-injektálás, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának. Webes alkalmazás számos hello OWASP felső 10 közös webes biztonsági rések elleni védelmet nyújt.

## <a name="scenarios"></a>Forgatókönyvek

Ebben a cikkben van két olyan eset:

Hello első forgatókönyv, elsajátíthatja túl[webalkalmazási tűzfal Alkalmazásátjáró létrehozása](#create-an-application-gateway-with-web-application-firewall)

A második forgatókönyvben hello megismerheti túl[hozzáadása a webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró](#add-web-application-firewall-to-an-existing-application-gateway).

![Példaforgatókönyv][scenario]

> [!NOTE]
> További konfigurációs hello Alkalmazásátjáró, beleértve az egyéni rendszerállapot-vizsgálat, háttér címkészletet címek és a további szabályok hello Alkalmazásátjáró konfigurálása után, és nem a kezdeti telepítése során konfigurált.

## <a name="before-you-begin"></a>Előkészületek

Az Azure Application Gateway saját alhálózatba van szükség. Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja elég cím terület toohave több alhálózattal. Miután telepít egy alkalmazást tooa átjáróalhálózatot, csak további alkalmazásátjárót-e fel tudja toobe toohello alhálózat.

##<a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Webes alkalmazás tűzfal tooan meglévő Alkalmazásátjáró hozzáadása

Ez a példa egy meglévő alkalmazás átjáró toosupport webalkalmazási tűzfal megelőzési módban frissíti.

1. Az Azure-portálon hello **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello meglévő Alkalmazásátjáró hello **összes erőforrás** panelen. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja hello neve hello **Szűrés név alapján...** mezőbe tooeasily hozzáférés hello DNS-zónát.

   ![Alkalmazásátjáró létrehozása][1]

1. Kattintson a **webalkalmazási tűzfal** és hello Alkalmazásátjáró beállításainak frissítése. Ha végzett a kattintson **mentése**

    hello beállítások tooupdate egy meglévő alkalmazás átjáró toosupport webalkalmazási tűzfal a következők:

   | **Beállítás** | **Érték** | **Részletek**
   |---|---|---|
   |**Frissítési tooWAF réteg**| Bejelölve | Ez a beállítás hello réteg hello alkalmazás átjáró toohello WAF réteg.|
   |**Tűzfal állapota**| Engedélyezve | Ez a beállítás lehetővé teszi, hogy a tűzfala hello hello WAF.|
   |**Tűzfal módban** | Megelőzés | Ez azért, hogy a webalkalmazási tűzfal hogyan kezelje a forgalmat. **Észlelési** mód csak naplók hello eseményeket, ahol **megelőzési** mód hello eseményeket naplózza, és leállítja hello rosszindulatú forgalmat.|
   |**Szabálykészlete**|3.0|Ez a beállítás meghatározza, hogy hello [szabálykészlet alapvető](application-gateway-web-application-firewall-overview.md#core-rule-sets) , amely a használt tooprotect hello háttér a készlet tagjainak.|
   |**Letiltott szabályok konfigurálása**|Változó|tooprevent lehetséges téves, ez a beállítás lehetővé teszi toodisable bizonyos [szabályok és a csoportok](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Egy meglévő alkalmazás átjáró toohello WAF SKU frissítésekor, hello SKU túl méretezés-e módosítások**Közepes**. Ez is újra kell konfigurálni konfiguráció befejezése után.

    ![megjelenítő alapvető beállítások panel][2-1]

    > [!NOTE]
    > tooview webes alkalmazás tűzfal naplókat, diagnosztika engedélyezve kell lennie, és ApplicationGatewayFirewallLog kiválasztva. Tesztelési célokra példányszámának 1 választható ki. Fontos, hogy bármely példányok száma a két példányt tooknow hello SLA nem vonatkozik, és ezért nem ajánlott. Kis átjárók webalkalmazási tűzfal használata esetén nem érhetők el.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Webalkalmazási tűzfal Alkalmazásátjáró létrehozása

Ez a forgatókönyv tartalma:

* Hozzon létre egy közepes méretű webes alkalmazás tűzfal Alkalmazásátjáró két példányt.
* Nevű AdatumAppGatewayVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.
* Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.
* Kiszervezési SSL tanúsítvány konfigurálása.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). Ha már nincs fiókja, regisztrálhat az egy [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/free)
1. Hello portal hello Kedvencek ablaktábláján kattintson **új**
1. A hello **új** panelen kattintson a **hálózati**. A hello **hálózati** panelen kattintson a **Alkalmazásátjáró**, ahogy az a következő kép hello:
1. Keresse meg az Azure-portálon toohello, kattintson a **új** > **hálózati** > **Alkalmazásátjáró**

    ![Alkalmazásátjáró létrehozása][1]

1. A hello **alapjai** panel, amelyen megjelenik, írja be a következő értékek hello, majd kattintson **OK**:

   | **Beállítás** | **Érték** | **Részletek**
   |---|---|---|
   |**Name (Név)**|AdatumAppGateway|hello Alkalmazásátjáró hello neve|
   |**Réteg**|WAF|Lehetséges értékek a következők: Standard és a WAF. Látogasson el [webalkalmazási tűzfal](application-gateway-web-application-firewall-overview.md) toolearn WAF többet.|
   |**Termékváltozat-méretét**|Közepes|Standard csomag kiválasztásakor használhatók kis, közepes és nagy. WAF réteg kiválasztásakor beállítások: Közepes és nagy csak.|
   |**A példányok száma**|2|A magas rendelkezésre állású hello Alkalmazásátjáró példányainak száma. Példányok számát 1 csak tesztelési célokra használható.|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon egy előfizetés toocreate hello Alkalmazásátjáró a.|
   |**Erőforráscsoport**|**Új:** AdatumAppGatewayRG|Hozzon létre egy erőforráscsoportot. az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie. További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) a cikk áttekintése.|
   |**Hely**|USA nyugati régiója||

   ![megjelenítő alapvető beállítások panel][2-2]

1. A hello **beállítások** panel alatt megjelenő **virtuális hálózati**, kattintson a **virtuális hálózatot választ**. Ez a lépés nyílik meg hello **válasszon virtuális hálózati** panelen.  Kattintson a **hozzon létre új** tooopen hello **virtuális hálózat létrehozása** panelen.

   ![Válasszon egy virtuális hálózatot][2]

1. A hello **hozzon létre virtuális hálózat panel** írja be a következő értékek hello, majd kattintson a **OK**. Ez a lépés bezárja hello **virtuális hálózat létrehozása** és **válasszon virtuális hálózati** paneleken. Ez kitölti hello **alhálózati** található hello **beállítások** panelen kiválasztott hello alhálózattal.

   |**Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|AdatumAppGatewayVNET|Hello Alkalmazásátjáró neve|
   |**Címterület**|10.0.0.0/16| Ez az érték hello címterület hello virtuális hálózat|
   |**Alhálózat neve**|AppGatewaySubnet|Az Alkalmazásátjáró hello hello alhálózat neve|
   |**Alhálózati címtartomány**|10.0.0.0/28 | Ez az alhálózat lehetővé teszi több további alhálózatokat hello virtuális hálózat háttér a készlet tagjainak|

1. A hello **beállítások** részen **előtérbeli IP-konfiguráció**, válassza a **nyilvános** hello, **IP-cím típusa**

1. A hello **beállítások** részen **nyilvános IP-cím**, kattintson a **egy nyilvános IP-cím kiválasztása**, ez a lépés megnyitja hello **nyilvános IP-címkiválasztása**panelen kattintson a **hozzon létre új**.

   ![Válassza ki a nyilvános IP-cím][3]

1. A hello **nyilvános IP-cím létrehozása** panelen fogadja el alapértékként hello, és kattintson a **OK**. Ebben a lépésben bezárja hello **nyilvános IP-cím kiválasztása** panelen, hello **nyilvános IP-cím létrehozása** panelt, és tölthet **nyilvános IP-cím** hello kiválasztott nyilvános IP-címmel.

1. A hello **beállítások** részen **figyelő konfigurációs**, kattintson a **HTTP** alatt **protokoll**. toouse **https**, egy tanúsítványra szükség. hello hello tanúsítvány titkos kulcsának van szükség, így egy .pfx hello tanúsítvány exportálása a megadott toobe kell, és hello hello fájlhoz tartozó jelszót.

1. Hello konfigurálása **WAF** vonatkozó beállításokat.

   |**Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Tűzfal állapota**| Engedélyezve| Ez a beállítás be- vagy kikapcsolja a WAF.|
   |**Tűzfal módban** | Megelőzés| Ez a beállítás meghatározza, hogy WAF időt vesz igénybe, a rosszindulatú forgalom hello műveletek. Ha **észlelési** választása esetén csak kerül a forgalmat.  Ha **megelőzési** van kiválasztva, forgalmat a rendszer naplózza, és leállt a 403-as jogosulatlan választ.|


1. Hello összefoglalás lapon tekintse át, és kattintson a **OK**.  Most hello Alkalmazásátjáró sorba és létre.

1. Hello Alkalmazásátjáró létrehozása után keresse meg az Alkalmazásátjáró hello hello portál toocontinue konfigurációban tooit.

    ![Alkalmazás átjáró erőforrás-kihasználása nézetét][10]

Ezeket a lépéseket egy alapszintű application gateway hello figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok az alapértelmezett beállításokkal hozza létre. Módosíthatja a beállítások toosuit a telepítés után hello kiépítés sikeres

> [!NOTE]
> Hello alapvető webalkalmazás tűzfal konfigurációjának létre alkalmazásátjárót védelmet CRS 3.0-val van állítva.

## <a name="next-steps"></a>Következő lépések

Ezt követően megtanulhatja hogyan tooconfigure egy egyéni tartomány alias hello [nyilvános IP-cím](../dns/dns-custom-domain.md#public-ip-address) használata az Azure DNS- vagy egy másik DNS-szolgáltatónál.

Megtudhatja, hogyan tooconfigure diagnosztikai naplózás, vagy látogasson el a webalkalmazási tűzfal megakadályozta toolog hello események [Alkalmazásdiagnosztika átjáró](application-gateway-diagnostics.md)

Ismerje meg, hogyan toocreate egyéni állapot-mintavételi csomagjai ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)

Ismerje meg, hogyan SSL-Feladatkiszervezést tooconfigure és hajtsa végre a megfelelő hello költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
