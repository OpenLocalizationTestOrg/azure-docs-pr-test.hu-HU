---
title: "aaaCreate Alkalmazásátjáró - Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate használatával Alkalmazásátjáró hello portál"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 24c1d5701eae372cd233162ceb58dea36a3b6a39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-hello-portal"></a>Alkalmazásátjáró létrehozása hello portállal

[Alkalmazásátjáró](application-gateway-introduction.md) egy dedikált virtuális készülék biztosító alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatásként, kínáló lehetőségeket biztosít az alkalmazás terheléselosztási különböző réteg 7. Ez a cikk végigvezeti hello lépéseket toocreate Alkalmazásátjáró hello Azure-portál használatával, és a háttérkiszolgáló tagként egy meglévő kiszolgálóról hozzáadása.

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be toohello: az Azure portál [http://portal.azure.com](http://portal.azure.com)

## <a name="create-application-gateway"></a>Alkalmazásátjáró létrehozása

toocreate Alkalmazásátjáró, teljes hello szükséges lépésekről. Alkalmazásátjáró saját alhálózatba van szükség. Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja elég cím terület toohave több alhálózattal. Miután hello Alkalmazásátjáró telepített tooa alhálózati, csak más alkalmazásátjárót tooit lehet hozzáadni.

1. Hello portal hello Kedvencek ablaktábláján kattintson **új**
1. A hello **új** panelen kattintson a **hálózati**. A hello **hálózati** panelen kattintson a **Alkalmazásátjáró**, ahogy az a következő kép hello:

    ![Alkalmazásátjáró létrehozása][1]

1. A hello **alapjai** panel, amelyen megjelenik, írja be a következő értékek hello, majd kattintson **OK**:

   | **Beállítás** | **Érték** | **Részletek**|
   |---|---|---|
   |**Name (Név)**|AdatumAppGateway|hello Alkalmazásátjáró hello neve|
   |**Réteg**|Standard|Lehetséges értékek a következők: Standard és a WAF. Látogasson el [webalkalmazási tűzfal](application-gateway-web-application-firewall-overview.md) toolearn WAF többet.|
   |**Termékváltozat-méretét**|Közepes|Standard csomag kiválasztásakor használhatók kis, közepes és nagy. WAF réteg kiválasztásakor beállítások: Közepes és nagy csak.|
   |**A példányok száma**|2|A magas rendelkezésre állású hello Alkalmazásátjáró példányainak száma. Példányok számát 1 csak tesztelési célokra használható.|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon egy előfizetés toocreate hello Alkalmazásátjáró a.|
   |**Erőforráscsoport**|**Új:** AdatumAppGatewayRG|Hozzon létre egy erőforráscsoportot. az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie. További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) a cikk áttekintése.|
   |**Hely**|USA nyugati régiója||

1. A hello **beállítások** panel alatt megjelenő **virtuális hálózati**, kattintson a **virtuális hálózatot választ**. Hello **válasszon virtuális hálózati** panel nyílik meg.  Kattintson a **hozzon létre új** tooopen hello **virtuális hálózat létrehozása** panelen.

   ![Válasszon egy virtuális hálózatot][2]

1. A hello **hozzon létre virtuális hálózat panel** írja be a következő értékek hello, majd kattintson a **OK**. Hello **virtuális hálózat létrehozása** és **válasszon virtuális hálózati** paneleken bezárásához. Ez a lépés feltölti hello **alhálózati** található hello **beállítások** panelen kiválasztott hello alhálózattal.

   | **Beállítás** | **Érték** | **Részletek**|
   |---|---|---|
   |**Name (Név)**|AdatumAppGatewayVNET|Hello Alkalmazásátjáró neve|
   |**Címterület**|10.0.0.0/16|Ez az hello címterület hello virtuális hálózat|
   |**Alhálózat neve**|AppGatewaySubnet|Az Alkalmazásátjáró hello hello alhálózat neve|
   |**Alhálózati címtartomány**|10.0.0.0/28|Ez az alhálózat lehetővé teszi több további alhálózatokat hello virtuális hálózat háttér a készlet tagjainak|

1. A hello **beállítások** részen **előtérbeli IP-konfiguráció**, válassza a **nyilvános** hello, **IP-cím típusa**

1. A hello **beállítások** részen **nyilvános IP-cím** kattintson **egy nyilvános IP-cím kiválasztása**, hello **nyilvános IP-cím kiválasztása** panel megnyitása , kattintson a **hozzon létre új**.

   ![Válassza ki a nyilvános IP-cím][3]

1. A hello **nyilvános IP-cím létrehozása** panelen fogadja el alapértékként hello, és kattintson a **OK**. hello panel bezárása után, és feltölti a hello **nyilvános IP-cím** hello kiválasztott nyilvános IP-címmel.

1. A hello **beállítások** részen **figyelő konfigurációs**, kattintson a **HTTP** alatt **protokoll**. Adja meg a hello port toouse hello **Port** mező.

2. Kattintson a **OK** a hello **beállítások** panel toocontinue.

1. Tekintse át a hello hello beállításokat **összegzés** panel megnyitásához, és kattintson **OK** hello Alkalmazásátjáró toostart létrehozását. Alkalmazásátjáró létrehozása egy hosszú ideig futó feladat és toocomplete időt vesz igénybe.

## <a name="add-servers-toobackend-pools"></a>Kiszolgálók toobackend készletek hozzáadása

Hello Alkalmazásátjáró létrehozása után a gazdagép hello alkalmazás toobe terhelésű hello rendszereket továbbra is kell toobe hozzáadott toohello Alkalmazásátjáró. hello IP-címek, teljesen minősített tartománynév vagy ezek a kiszolgálók hálózati adapterek toohello háttércímkészletek kerülnek.

### <a name="ip-address-or-fqdn"></a>IP-cím vagy teljes Tartományneve

1. A hello Azure-portálon létrehozott hello Alkalmazásátjáró **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **AdatumAppGateway** az Alkalmazásátjáró hello összes erőforrások panelen. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **AdatumAppGateway** a hello **Szűrés név alapján...** mezőbe tooeasily hozzáférés hello Alkalmazásátjáró.

1. az Alkalmazásátjáró hello létrehozott jelenik meg. Kattintson a **háttérkészletek**, és válassza ki az aktuális háttérkészlet hello **appGatewayBackendPool**, hello **appGatewayBackendPool** panel nyílik meg.

   ![Alkalmazáskészletek átjáró háttér][4]

1. Kattintson a **tároló hozzáadása** tooadd IP-címek a teljes tartománynevet. Válassza ki **IP címe vagy teljes Tartományneve** , hello **típus** mezőbe pedig írja be az IP-cím vagy FQDN hello. Ismételje meg ezt a lépést további háttér a készlet tagjainak. Miután befejezte az kattintson **mentése**.

### <a name="virtual-machine-and-nic"></a>Virtuális gép és a hálózati adapter

Virtuális gép hálózati adapterek háttér címkészletet tagként is hozzáadhat. Alkalmazásátjáró hello azonos virtuális hálózaton keresztül érhetők el hello csak virtuális gépeit hello legördülő menüből.

1. A hello Azure-portálon létrehozott hello Alkalmazásátjáró **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **AdatumAppGateway** az Alkalmazásátjáró hello összes erőforrások panelen. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **AdatumAppGateway** a hello **Szűrés név alapján...** mezőbe tooeasily hozzáférés hello Alkalmazásátjáró.

1. az Alkalmazásátjáró hello létrehozott jelenik meg. Kattintson a **háttérkészletek**, és válassza ki az aktuális háttérkészlet hello **appGatewayBackendPool**, hello **appGatewayBackendPool** panel nyílik meg.

   ![Alkalmazáskészletek átjáró háttér][5]

1. Kattintson a **tároló hozzáadása** tooadd IP-címek a teljes tartománynevet. Válassza ki **virtuális gép** , hello **típus** hello virtuális gép és a hálózati adapter toouse válassza ki. Miután befejezte az kattintson **mentése**

   > [!NOTE]
   > Virtuális gépek csak azonos virtuális hálózatban hello, vagy hello Alkalmazásátjáró sem érhető el az hello legördülő listából.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, hello erőforráscsoport, Alkalmazásátjáró és minden kapcsolódó erőforrás törléséhez. toodo tehát válasszon erőforráscsoportot hello hello alkalmazás átjáró panel megnyitásához, és kattintson a **törlése**.

## <a name="next-steps"></a>Következő lépések

Ebben a forgatókönyvben Alkalmazásátjáró telepített, és a kiszolgáló toohello háttér hozzáadni. hello lépések tooconfigure hello Alkalmazásátjáró beállítások módosítása, és az átjáró hello beállító szabályok. Ezeket a lépéseket a következő cikkek hello felkeresésével található:

Ismerje meg, hogyan toocreate egyéni állapot-mintavételi csomagjai ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)

Ismerje meg, hogyan SSL-Feladatkiszervezést tooconfigure és hajtsa végre a megfelelő hello költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-portal.md)

Megtudhatja, hogyan tooprotect az alkalmazások [webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md) Alkalmazásátjáró szolgáltatása.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
