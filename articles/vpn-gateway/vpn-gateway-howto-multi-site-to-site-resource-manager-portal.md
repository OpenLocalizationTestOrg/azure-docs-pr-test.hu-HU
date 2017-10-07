---
title: "Több VPN gateway pont-pont kapcsolatok tooa virtuális hálózat hozzáadása: Azure-portálon: erőforrás-kezelő |} Microsoft Docs"
description: "Többhelyes S2S kapcsolatok tooa VPN-átjáró, amely rendelkezik egy létező kapcsolat hozzáadása"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: b8c9ff454967f509dcef725f8bcec8564fad9b00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection"></a>A pont-pont kapcsolat tooa virtuális hálózatot egy meglévő VPN-átjáró kapcsolattal hozzáadása

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasszikus)](vpn-gateway-multi-site.md)
>
> 

Ez a cikk bemutatja, hogyan hello Azure portál tooadd webhelyek (közötti S2S) kapcsolatok tooa VPN-átjáró, amely rendelkezik egy létező kapcsolat használatával. Ez a kapcsolat típus gyakran hivatkozott tooas egy "több" Helykonfiguráció. Hozzáadhat egy S2S kapcsolat tooa virtuális hálózatot, amely már rendelkezik egy S2S kapcsolatot, a pont – hely típusú kapcsolatot vagy a VNet – VNet-kapcsolatot. Bizonyos korlátozások is kapcsolatok hozzáadása során. Ellenőrizze a hello [megkezdése előtt](#before) Ez a cikk tooverify konfigurációtól elindítása előtt című szakasza. 

Ez a cikk vonatkozik tooVNets hello erőforrás-kezelő telepítési modellel készült, amelyek RouteBased VPN-átjáró. Ezeket a lépéseket ne alkalmazza a tooExpressRoute/pont-pont vizsgálatát a kísérő kapcsolat konfigurációkat. Lásd: [vizsgálatát a kísérő ExpressRoute és az S2S-kapcsolatok](../expressroute/expressroute-howto-coexist-resource-manager.md) vizsgálatát a kísérő kapcsolatok kapcsolatos információkat.

### <a name="deployment-models-and-methods"></a>Üzemi modellek és módszerek
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Ebben a táblázatban, új cikket frissítjük, és további eszközök ehhez a konfigurációhoz elérhetővé válnak. Egy cikk nem érhető el, hogy hivatkozás közvetlenül tooit ebből a táblázatból.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Előkészületek
Ellenőrizze a következő elemek hello:

* Nem hoz létre egy vizsgálatát a kísérő ExpressRoute és az S2S-kapcsolatot.
* A virtuális hálózati létrehozott hello Resource Manager üzembe helyezési modellben már meglévő kapcsolattal rendelkezik.
* hello virtuális hálózati átjáró a Vnethez tartozó RouteBased. Ha PolicyBased VPN-átjáró, akkor törölje a hello virtuális hálózati átjáró, majd hozzon létre egy új VPN-átjáró mint RouteBased.
* Nincs hello címtartományai átfedésben vannak, bármely hello Vnetek, hogy ez a virtuális hálózat csatlakozik.
* VPN-kompatibilis eszköz és a rendszer képes tooconfigure azt. Lásd: [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Tudnivalók a VPN-eszközökről). Ha nem ismeri a konfigurálása a VPN-eszköz, vagy nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfigurációtól, toocoordinate kell valakitől meg ezeket az információkat is tartalmazza.
* A VPN-eszköz van külsőleg irányuló nyilvános IP-címet. Ez az IP-cím nem lehet NAT mögötti.

## <a name="part1"></a>1. rész - kapcsolat beállítása
1. Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és szükség esetén jelentkezzen be az Azure-fiókjával.
2. Kattintson a **összes erőforrás** , és keresse meg a **virtuális hálózati átjáró** erőforrásokat hello lista, és kattintson rá.
3. A hello **virtuális hálózati átjáró** panelen kattintson a **kapcsolatok**.
   
    ![Kapcsolatpanel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Kapcsolatpanel")<br>
4. A hello **kapcsolatok** panelen kattintson a **+ Hozzáadás**.
   
    ![Hozzáadás kapcsolat gombra](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Hozzáadás kapcsolat gombra")<br>
5. A hello **kapcsolat hozzáadása a** panelen kitöltése során a következő mezők hello:
   
   * **Name:** hello neve toogive toohello webhely hello kapcsolatot hoz létre.
   * **Kapcsolat típusa:** válasszon **pont-pont (IPsec)**.
     
     ![Hozzáadás kapcsolat panelen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Hozzáadás kapcsolat panelen")<br>

## <a name="part2"></a>2. rész – a helyi hálózati átjáró hozzáadása
1. Kattintson a **helyi hálózati átjáró** ***helyi hálózati átjáró kiválasztása***. Ekkor megnyílik hello **válassza a helyi hálózati átjáró** panelen.
   
    ![Válassza a helyi hálózati átjáró](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "helyi hálózati átjáró kiválasztása")<br>
2. Kattintson a **hozzon létre új** tooopen hello **helyi hálózati átjáró létrehozása** panelen.
   
    ![Létrehozás helyi hálózati átjáró panel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "helyi hálózati átjáró létrehozása")<br>
3. A hello **helyi hálózati átjáró létrehozása** panelen kitöltése során a következő mezők hello:
   
   * **Name:** hello nevet toogive toohello helyi hálózati átjáró-erőforráshoz.
   * **IP-cím:** hello hello VPN-eszköz, amelyet a tooconnect hello helyen nyilvános IP-címét.
   * **Címtér:** , amelyet az toobe hello címterület toohello új helyi hálózati telephely átirányítva.
4. Kattintson a **OK** a hello **helyi hálózati átjáró létrehozása** panel toosave hello módosításokat.

## <a name="part3"></a>3. rész – hello megosztott kulcs hozzáadása és hello kapcsolat létrehozása
1. A hello **kapcsolat hozzáadása a** panelen hello megosztott kulcsot, amelyet toouse toocreate a kapcsolatot hozzáadni. Is megadhat vagy hello megosztott kulcs beszerzése a VPN-eszköz vagy egy itt, majd konfigurálja a VPN-eszköz toouse hello azonos megosztott kulcs. fontos dolog, hogy hello kulcs pontosan van-e hello hello azonos.
   
    ![Megosztott kulcs](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Megosztott kulcs")<br>
2. Hello hello panel alsó részén, kattintson a **OK** toocreate hello kapcsolat.

## <a name="part4"></a>Rész 4 – hello VPN-kapcsolat ellenőrzése


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Következő lépések

Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. Tekintse meg a virtuális gépek hello [képzési](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) további információt.
