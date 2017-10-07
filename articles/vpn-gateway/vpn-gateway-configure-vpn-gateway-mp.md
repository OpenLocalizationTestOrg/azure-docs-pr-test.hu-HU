---
title: "VPN-átjáró konfigurálása: a klasszikus portálon: Azure |} Microsoft Docs"
description: "Ez a cikk útmutatást nyújt a virtuális hálózat VPN-átjáró konfigurálása és az átjáró VPN-útválasztási típus módosítása. Ezeket a lépéseket toohello klasszikus telepítési modell és hello klasszikus portál alkalmazni."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fbe59ba8-b11f-4d21-9bb1-225ec6c6d351
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
ms.openlocfilehash: 00d2fa18bab6eb24b33ddb18113f2a557db638d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vpn-gateway-in-hello-classic-portal"></a>Konfigurálja a VPN-átjáró hello klasszikus portál 
Ha azt szeretné, hogy toocreate Azure és a helyszíni hely közötti biztonságos létesítmények közötti kapcsolatot, a virtuális hálózati átjáró toocreate kell. VPN-átjáró egy olyan virtuális hálózati átjáró adott típusú. Hello klasszikus üzembe helyezési modellel, VPN-átjáró lehet két útválasztási VPN-típusok egyikét: statikus vagy dinamikus. hello VPN-típus úgy dönt, a hálózati tervet, és függ hello a helyszíni VPN-eszköz toouse szeretné. VPN-eszközökkel kapcsolatos további információkért lásd: [VPN-eszközök](vpn-gateway-about-vpn-devices.md).

**Tudnivalók az Azure üzembe helyezési modelljeiről**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-overview"></a>Konfigurálása – áttekintés
hello következő lépések végigvezetik a VPN-átjáró konfigurálása hello a klasszikus portálon. Ezeket a lépéseket hello klasszikus telepítési modell használatával létrehozott virtuális hálózatok toogateways alkalmazni. Jelenleg nem minden hello konfigurációs beállítások átjárók érhetők hello Azure-portálon. Ha vannak, létre fogunk hozni egy új készletét toohello Azure-portálon érvényes utasításokat.

### <a name="before-you-begin"></a>Előkészületek
Az átjáró konfigurálása előtt először toocreate a virtuális hálózat. Lépéseket toocreate közötti kapcsolatot nyújthassanak egy virtuális hálózatot, lásd: [virtuális hálózat konfigurálása a pont-pont VPN-kapcsolat](vpn-gateway-site-to-site-create.md), vagy [virtuális hálózat konfigurálása a VPN-kapcsolatpont-pont](vpn-gateway-point-to-site-create.md). Ezután használja a következő lépéseket tooconfigure hello VPN-átjáró hello és tooconfigure a VPN-eszköz szükség hello adatainak összegyűjtése. 

Ha már rendelkezik egy VPN-átjáró, és azt szeretné, hogy toochange hello VPN-útválasztási típusa, lásd: [hogyan toochange hello VPN-útválasztási típus az átjáró](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>VPN-átjáró létrehozása
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello **hálózatok** lapon, a virtuális hálózat van-e, hogy hello állapot oszlop **Created**.
2. A hello **neve** oszlop, kattintson a virtuális hálózat hello nevét.
3. A hello **irányítópult** lapon, láthatja, hogy ez a virtuális hálózat nem rendelkezik egy átjáró konfigurálva. Ez az állapot láthatja végrehajtása közben hello lépéseket tooconfigure az átjárót.

![Nem jött létre átjáró](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)

A következő hello a hello lap alján, kattintson **átjáró létrehozása**. Ki lehet *statikus útválasztás* vagy *dinamikus útválasztási*. VPN-útválasztási típus választja hello tényezőtől függ. Például a VPN-eszköz támogatja, és hogy mikor szükséges toosupport pont – hely kapcsolatok. Ellenőrizze [kapcsolatos VPN-eszközök a virtuális hálózati kapcsolatra](vpn-gateway-about-vpn-devices.md) tooverify hello VPN-útválasztási típus szükséges. Hello átjáró létrehozása után nem módosítható átjáró VPN útválasztási típus nélkül törli, majd újra létrehozza a hello átjáró között. Ha hello rendszer felajánlja a hello létrehozott átjárót, kattintson a kívánt tooconfirm **Igen**.

![Átjáró VPN-útválasztási típus](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Az átjáró létrehozásakor figyelje meg hello átjáró grafikus hello oldalon tooyellow változik, és a szerint *átjáró létrehozása*. Hello átjáró toocreate too45 percig is tarthat. Várjon, amíg hello átjáró befejeződött, a továbblépés előtt előre egyéb konfigurációs beállításokkal.

![Átjáró létrehozása](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Ha túl az Alkalmazásátjáró módosításai hello*csatlakozás*, szüksége lesz a VPN-eszköz hello adatokat gyűjthet.

![Átjáró csatlakozás](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="site-to-site-connections"></a>Helyek közötti kapcsolatok

### <a name="step-1-gather-information-for-your-vpn-device-configuration"></a>1. lépés A VPN-eszköz konfigurációjában adatainak összegyűjtése
A pont-pont kapcsolat létrehozása hello átjáró létrehozása után, a VPN-eszköz konfigurációjában adatainak összegyűjtése. Ez az információ hello található **irányítópult** a virtuális hálózat lap:

1. **Átjáró IP-cím -** hello IP-cím található hello **irányítópult** lap. Nem fog tudni toosee, amíg az átjáró létrehozása befejeződött.
2. **Megosztott kulcs -** kattintson **kezelése kulcs** üdvözlő képernyőt hello alján. Hello ikon következő toohello kulcs toocopy kattintson azt tooyour vágólapra, majd illessze be és mentse hello kulcsot. Ez a gomb csak akkor működik, ha egyetlen S2S VPN-alagúton. Ha több S2S VPN-alagutat, használja a hello *első virtuális hálózati átjáró megosztott kulcsos* API-t vagy a PowerShell-parancsmagot.

![Kulcs kezelése](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)

### <a name="step-2--configure-your-vpn-device"></a>2. lépés  VPN-eszköz konfigurálása
Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség. Az összes VPN-eszköz nem nyújtunk konfigurációs lépések, míg előfordulhat hello információkat a hasznos hivatkozásokat követve hello:

- A kompatibilis VPN-eszközökkel kapcsolatos információért tekintse meg a [VPN-eszközökkel foglalkozó szakaszt](vpn-gateway-about-vpn-devices.md). 
- Hivatkozások toodevice konfigurációs beállításokat, lásd: [érvényesítve VPN-eszközök](vpn-gateway-about-vpn-devices.md#devicetable). Ezeket a hivatkozásokat tájékoztató jelleggel biztosítjuk. Célszerű mindig a legfrissebb konfigurációs adatok hello eszköz gyártójához legjobb toocheck.
- Az eszközök konfigurációs mintáinak szerkesztésével kapcsolatos információkért tekintse meg a [minták szerkesztésével](vpn-gateway-about-vpn-devices.md#editing) kapcsolatos részt.
- Az IPsec/IKE-paraméterekkel kapcsolatos információkért tekintse meg a [paraméterekkel](vpn-gateway-about-vpn-devices.md#ipsec) kapcsolatos részt.
- Mielőtt konfigurálná a VPN-eszköz, ellenőrizze az összes [ismert eszköz kompatibilitási problémák](vpn-gateway-about-vpn-devices.md#known) hello VPN-eszköz, amelyet az toouse.

A VPN-eszköz konfigurálásakor a következő elemek hello lesz szüksége:

- hello a virtuális hálózati átjáró nyilvános IP-címét. Ön ez fog toohello keresheti **áttekintése** a virtuális hálózati panelén.
- Megosztott kulcs. Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs. A példákban igen alapvető megosztott kulcsot használunk. Létre kell hoznia egy összetett kulcs toouse.

Hello VPN-eszköz konfigurálása után a virtuális hálózat hello irányítópult-oldalon megtekintheti a frissített kapcsolódási adatok.

### <a name="step-3-verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>3. lépés Ellenőrizze a helyi hálózati tartományok és a VPN-átjáró IP-címe
#### <a name="verify-your-vpn-gateway-ip-address"></a>A VPN-átjáró IP-cím ellenőrzése
Az átjáró tooconnect megfelelően, a VPN-eszköz IP-címet hello megfelelően kell konfigurálni a helyi hálózat a létesítmények közötti konfigurációs megadott hello. Általában ez úgy van konfigurálva hello helyek a konfigurációs folyamat során. Ha korábban már használt a helyi hálózat egy másik eszköz, vagy hello IP-címe a helyi hálózat változott, szerkesztése hello beállítások toospecify hello megfelelő átjáró IP-címet.

1. tooverify az átjáró IP-cím, kattintson a **hálózatok** a hello a portál bal oldali ablaktáblán, és válassza ki **helyi hálózatok** hello oldal hello tetején. Látni fogja, VPN-átjáró címét hello minden Ön által létrehozott helyi hálózathoz. tooedit hello IP-cím, jelölje ki a hello VNet, majd kattintson **szerkesztése** hello lap hello alján.
2. A hello **adja meg a helyi hálózati részletes** lapon hello IP-cím szerkesztése, és kattintson a Tovább nyílra hello hello hello lap alján.
3. A hello **adja meg a hello Címterület** lapján kattintson az alsó jobb toosave hello hello pipa a beállításokat.

#### <a name="verify-hello-address-ranges-for-your-local-networks"></a>Ellenőrizze a helyi hálózat címtartományát hello
Hello megfelelő forgalmi tooflow hello átjáró tooyour helyszíni helyeken keresztül, a kell tooverify, hogy minden IP-címtartomány van megadva. Minden tartományban szerepelnie kell az Azure **helyi hálózatok** konfigurációs. Attól függően, hogy a helyszíni hely hello a hálózati konfiguráció feladat némileg nagy is lehet. Az IP-címet, amely felsorolt hello tartományokon belül található kötött forgalom küldi hello virtuális hálózat VPN-átjárón keresztül. hello címtartományok, hogy listájának toobe privát címtartományok nem rendelkezik, érdemes Bár a helyi konfigurációs fogadhassanak tooverify hello bejövő forgalom.

egy helyi hálózati tooadd vagy Szerkesztés hello tartományok lépések hello használata:

1. tooedit hello IP-címtartományok egy helyi hálózat kattintson **hálózatok** a hello a portál bal oldali ablaktáblán, és válassza ki **helyi hálózatok** hello oldal hello tetején. Hello portálon hello legegyszerűbb módja tooview hello tartományokat, amely már szerepel a listában megtalálható hello **szerkesztése** lap. toosee hello VNet tartományt, válassza ki, majd kattintson **szerkesztése** hello lap hello alján.
2. A hello **adja meg a helyi hálózati részletes** lapon, a beállítások módosításait nem. Kattintson a Tovább nyílra hello hello hello lap alján.
3. A hello **adja meg a hello Címterület** lapon, hogy a hálózat címtartományát érintő módosítások. Kattintson a pipa toosave hello a konfigurálása.

## <a name="how-tooview-gateway-traffic"></a>Hogyan tooview átjáró forgalom
A virtuális hálózat megtekintheti az átjáró és az átjáró forgalom **irányítópult** lap.

A hello **irányítópult** lapon megtekintheti a következő hello:

* hello adatmennyiséget áramlik át az átjáró, az adatok és a kimenő adatforgalmat.
* hello hello is meg van adva a virtuális hálózat DNS-kiszolgálók nevét.
* az átjáró és a VPN-eszköz kapcsolatot hello.
* hello megosztott kulcsot, amely használt tooconfigure a átjáró kapcsolat tooyour VPN-eszköz.

## <a name="how-toochange-hello-vpn-routing-type-for-your-gateway"></a>Hogyan toochange hello az átjáró a VPN-útválasztási típus
Mert átjáró útválasztási bizonyos néhány kapcsolat konfigurációk csak érhetők el, előfordulhat, hogy kell-e toochange hello átjáró VPN-útválasztási típus egy meglévő VPN-átjáró. Például érdemes lehet tooadd pont – hely kapcsolat tooan már meglévő helyek kapcsolatot egy statikus átjáró. Pont – hely kapcsolatok dinamikus átjáró szükséges. Az átjáró VPN-útválasztási típus a statikus toodynamic telepítve toochange, ez azt jelenti, hogy tooconfigure P2S kapcsolatot.

Ha egy átjáró VPN-útválasztási típus toochange van szüksége, lesz töröl hello meglévő átjáró, és majd hozzon létre egy új hello új útválasztás típusa. Nincs szükség a toodelete hello teljes virtuális hálózat toochange hello átjáró útválasztási típusa.

Az átjáró VPN-útválasztási típus módosítása, előtt kell arról, hogy a VPN-eszköz támogatja-e a hello útválasztás típusa, amelyet az toouse tooverify. Új útválasztási konfigurációs minták toodownload és ellenőrzés VPN követelmények, lásd: [kapcsolatos VPN-eszközök a virtuális hálózati kapcsolatra](vpn-gateway-about-vpn-devices.md).

> [!IMPORTANT]
> Ha töröl egy virtuális hálózat VPN-átjáró, hello toohello átjáró hozzárendelt VIP szabadul fel. Ismételt hello átjáró, egy új VIP tooit van hozzárendelve.
> 
> 

1. **Hello meglévő VPN-átjáró törlése.**
   
    A hello **irányítópult** a virtuális hálózatok lapon keresse meg a toohello a hello lap alján, majd kattintson **átjáró törlése**. Várjon, amíg az átjáró hello hello értesítést törölve lett. Miután megkapta hello értesítés az üdvözlő képernyőt, hogy törölték az átjárót, egy új átjárót is létrehozhat.
2. **Hozzon létre egy új VPN-átjáró.**
   
    Hello eljárással hello lap toocreate új átjáró hello tetején: [hozzon létre egy VPN-átjáró](#create-a-vpn-gateway).

## <a name="next-steps"></a>Következő lépések
Virtuális gépek tooyour virtuális hálózat is hozzáadhat. Lásd: [hogyan toocreate egyéni virtuális gép](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Ha azt szeretné, hogy a pont-pont tooconfigure VPN-kapcsolatot, lásd: [egy pont – hely VPN-kapcsolat konfigurálása](vpn-gateway-point-to-site-create.md).

