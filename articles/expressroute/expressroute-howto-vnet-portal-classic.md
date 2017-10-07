---
title: "aaaConfigure egy virtuális hálózatot és az ExpressRoute-átjáró hello klasszikus portál |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan ExpressRoute hello klasszikus telepítési modell és hello klasszikus portál használatával egy virtuális hálózat beállítása."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: dd1f6c5e85dbb3ad0a53ecd81c13b4d3f5c06e66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-hello-classic-portal"></a>Virtuális hálózat létrehozása az ExpressRoute hello klasszikus portál
a cikkben ismertetett hello végigvezeti egy virtuális hálózatot és egy virtuális hálózati átjáró konfigurálása az ExpressRoute hello klasszikus telepítési modell és hello klasszikus portál használatával.

Ha hello Resource Manager üzembe helyezési modellben vonatkozó utasításokat, használhatja a következő cikkek hello: [virtuális hálózat létrehozása a PowerShell használatával](../virtual-network/virtual-networks-create-vnet-arm-ps.md) és [egy VPN-átjáró tooa erőforrás-kezelő virtuális hálózat hozzáadása a ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Tudnivalók az Azure üzembe helyezési modelljeiről**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Klasszikus virtuális hálózatot és átjáró létrehozása
hello lépések hozzon létre egy klasszikus virtuális hálózat és a virtuális hálózati átjáró. Ha már rendelkezik egy klasszikus virtuális hálózaton, lásd: hello [konfigurálása egy létező klasszikus virtuális hálózatot](#config) című részben.

1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com).
2. Hello bal alsó sarkában üdvözlő képernyőt, kattintson **új**. Hello navigációs ablaktábláján kattintson **hálózati szolgáltatások**, és kattintson a **virtuális hálózati**. Kattintson a **egyéni létrehozás** toobegin hello konfigurációs varázsló.
3. A hello **virtuális hálózati adatok** lapján írja be a következő hello:
   
   * **Név** – nevezze el a virtuális hálózatot. A virtuális hálózati név fogja használni, a virtuális gépek és PaaS-példányok, ezért nem célszerű toomake hello neve túl bonyolult központi telepítésekor.
   * **Hely** – hello helye közvetlenül kapcsolódó toohello fizikai helyének (régió) az erőforrások (VM) tooreside. Ha azt szeretné, hogy a hello toothis virtuális hálózati toobe USA keleti régiója fizikailag található központilag telepített virtuális gépek, például válassza az adott helyen. Létrehozása után a virtuális hálózathoz tartozó hello régió nem módosítható.
4. A hello **DNS-kiszolgálók és a VPN-kapcsolat** lapon adja meg a következő információ hello, és kattintson a Tovább nyílra hello hello jobb alsó sarkában. 
   
   * **DNS-kiszolgálók** – hello DNS-kiszolgáló neve és IP-címet adja meg, vagy válasszon egy korábban már regisztrált DNS-kiszolgáló hello helyi menüjéből. A beállítás nem hoz létre DNS-kiszolgálót. Ez lehetővé teszi toospecify hello DNS-kiszolgálók, amelyet az toouse névfeloldás ehhez a virtuális hálózathoz.
   * **Hely-hely kapcsolatot** – Itt adhatja meg jelölőnégyzetét hello **a telephelyek közötti VPN konfigurálása**.
   * **ExpressRoute** – jelölje be jelölőnégyzetet hello **használjon ExpressRoute**. Ez a beállítás csak akkor jelenik meg, ha a kiválasztott **a telephelyek közötti VPN konfigurálása**.
   * **Helyi hálózati** -szükséges toohave ExpressRoute a helyi hálózati telephely áll. Azonban ExpressRoute kapcsolat hello esetben hello címelőtagokat megadott hello helyi hálózati telephely figyelmen kívül hagyja. Ehelyett hello címelőtagokat meghirdetett hello ExpressRoute-kapcsolatcsoportot keresztül tooMicrosoft útválasztási célokra használható.<BR>Ha már rendelkezik egy helyi hálózati az ExpressRoute-kapcsolatot létrehozni, válassza ki azt hello legördülő menüből. Ha nem, válassza ki a **adjon meg egy új helyi hálózati**.
5. Hello **hely-hely kapcsolatot** lap jelenik meg, ha egy új helyi hálózati toospecify hello előző lépésben kiválasztott. tooconfigure a helyi hálózaton, írja be a következő információ hello, és kattintson a hello tovább nyílra. 
   
   * **Név** -hello nevet toocall a helyi (helyszíni) hálózati telephelyhez.
   * **Címtér** – beleértve a kezdő IP- és CIDR (cím száma). A címtartomány adhatja meg, amíg nem az hello a virtuális hálózat címtartománya átfedésben. Általában ez a helyszíni hálózatokhoz hello címtartományát lehet megadni, de ExpressRoute hello esetben ezek a beállítások nem használhatók. Azonban ez a beállítás kötelező rendelés toocreate hello helyi hálózati hello klasszikus portál használata esetén.
   * **Címtartomány felvétele** – Ez a beállítás nem fontos ExpressRoute.
6. A hello **virtuális hálózati címtereket** lapon, adja meg a következő információ hello majd hello pipa hello alacsonyabb jobb tooconfigure a a hálózaton. 
   
   * **Címtér** – beleértve a kezdő IP, és oldja meg a száma. Győződjön meg arról, hogy megadott hello címterek nem fedhetnek át olyan hello címterek, hogy rendelkezik a helyi hálózaton.
   * **Vegye fel az alhálózati** – beleértve a kezdő IP- és a cím száma. További alhálózatokat esetén nincs szükség.
   * **Átjáró alhálózatának hozzáadása** -tooadd hello átjáró alhálózatának kattintson. hello átjáróalhálózatot csak hello virtuális hálózati átjáró szolgál, és ehhez a konfigurációhoz szükséges.<BR>átjáró-alhálózatot CIDR (cím száma) hello ExpressRoute /28 kell lennie, vagy nagyobb (/ 27- / 26 stb.). Ez lehetővé teszi, hogy alhálózati tooallow hello konfigurációs toowork elég IP-cím. A klasszikus portálon hello Ha be van jelölve hello jelölőnégyzet toouse ExpressRoute, hello portal megadja egy átjáró-alhálózatot /28.  Hello CIDR cím száma hello a klasszikus portálon nem lehet beállítani. hello átjáróalhálózatot frissítésként jelenik meg **átjáró** hello klasszikus portálon, bár hello hello átjáróalhálózatot létrehozott valódi neve valójában **GatewaySubnet**. Ez a név megtekintheti a PowerShell használatával vagy a hello Azure-portálon.
7. Kattintson a pipa ikonra az hello lap alján hello hello, és a virtuális hálózati toocreate megkezdődik. Amikor elkészült, látni fogja **Created** tartozó **állapot** a hello **hálózatok** hello klasszikus portál oldalán.

## <a name="gw"></a>Hello átjáró létrehozása
1. A hello **hálózatok** lapon, kattintson a virtuális hálózati hello imént létrehozott, majd kattintson **irányítópult** hello oldal hello tetején.
2. A hello alsó részén hello **irányítópult** lapján kattintson **átjáró létrehozása** válassza **dinamikus útválasztási**. Kattintson a **Igen** , amelyet az átjáró toocreate tooconfirm.
3. Hello átjáró hoz létre, megjelenik egy üzenet azzal kapcsolatban, tudni, hogy hello átjáró el lett indítva. Hello átjáró toocreate too45 percig is tarthat.
4. A hálózati tooa kapcsolatcsoport hivatkozásra. Hello hello cikk utasításait követve [hogyan toolink Vnetek tooExpressRoute Kapcsolatcsoportok](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Egy létező klasszikus virtuális hálózatot ExpressRoute konfigurálása
Ha már rendelkezik egy klasszikus virtuális hálózaton, konfigurálhatja azt tooconnect tooExpressRoute hello a klasszikus portálon. hello-beállítások vannak hello azonos hello szakaszok újabb, mint, olvassa el ezen ismeri a hello szakaszok toobecome keresztül kötelező beállítások. Ha azt szeretné, hogy egy ExpressRoute/pont-pont vizsgálatát a kísérő kapcsolat toocreate, [Ez a cikk](expressroute-howto-coexist-classic.md) hello lépéseket. Más, mint a hello ebben a cikkben ismertetett visszaállítási lépésekkel.

1. A VNet beállításait hello részeinek frissítése előtt kell toocreate hello helyi hálózaton. toocreate egy új helyi hálózati, amely pedig szükséges a klasszikus portálon hello ExpressRoute konfigurálásakor, kattintson a **új**  **>**  **hálózati szolgáltatások**  **>**  **Virtuális hálózati**  **>**  **hozzáadása helyi hálózati**. Hajtsa végre a hello varázsló lépéseit toocreate hello helyi hálózaton.
2. Használjon **konfigurálása** tooupdate hello rest hello beállítása a virtuális hálózat és tooassociate hello VNet toohello helyi hálózat lapon.
3. Hello beállításainak konfigurálása után nyissa meg toohello [hello átjáró létrehozása](#gw) Ez a cikk toocreate hello átjáró szakasza.

## <a name="next-steps"></a>Következő lépések
* Ha azt szeretné, hogy tooadd virtuális gépek tooyour virtuális hálózat, lásd: [képzési terveket, a virtuális gépek](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* Ha azt szeretné, hogy további kapcsolatos az ExpressRoute toolearn, tekintse meg a hello [ExpressRoute – áttekintés](expressroute-introduction.md).

