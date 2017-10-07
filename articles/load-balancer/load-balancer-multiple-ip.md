---
title: "több IP-konfigurációk az Azure-ban a terheléselosztás aaaLoad |} Microsoft Docs"
description: "Terheléselosztás elsődleges és másodlagos IP-konfiguráció között."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 8619493b8102e9d158d428fe6c59ecf3f32edc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-hello-azure-portal"></a>Terheléselosztás több IP-konfigurációk hello Azure-portál használatával

> [!div class="op_single_selector"]
> * [Portál](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [Parancssori felület](load-balancer-multiple-ip-cli.md)

Ez a cikk ismerteti, hogyan toouse Azure Load Balancer több IP-címek egy másodlagos hálózati adapteren (NIC). Ebben a forgatókönyvben két virtuális gépeken futó Windows, az elsődleges és másodlagos hálózati tudunk Egyes hello másodlagos hálózati adapterei két IP-konfigurációk. Minden virtuális gép webhelyeket a contoso.com és fabrikam.com üzemelteti. Minden webhelyre kötött tooone az IP-konfigurációkhoz hello hello másodlagos hálózati adaptert. Azure Load Balancer tooexpose két előtérbeli IP-cím, egy a minden webhelyre, toodistribute forgalom toohello megfelelő IP-konfiguráció hello webhely használjuk. Ebben a forgatókönyvben használt hello ugyanazt a portszámot is frontends, valamint mindkét háttér címkészletet IP-címek között.

![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

##<a name="prerequisites"></a>Előfeltételek
Ez a példa feltételezi, hogy rendelkezik-e nevű erőforráscsoport *contosofabrikam* a következő konfigurációs hello:
 -  magában foglalja a virtuális hálózati nevű *myVNet*, nevű két virtuális gépek *VM1* és *vm2 virtuális gépnek* rendre belül hello azonos rendelkezésre állási csoport elnevezett *myAvailset*. 
 - minden virtuális gép rendelkezik egy elsődleges hálózati adapter és a másodlagos hálózati adaptert. hello elsődleges hálózati adapter neve *VM1NIC1* és *VM2NIC1* és hello másodlagos hálózati adapterek megnevezett *VM1NIC2* és *VM2NIC2*. Több hálózati adapterrel rendelkező virtuális gépek létrehozásával kapcsolatos további információkért lásd: [PowerShell segítségével több hálózati adapterrel rendelkező virtuális gép létrehozása](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a>Több IP-konfigurációk lépéseket tooload egyenlege

Kövesse az alábbi tooachieve hello forgatókönyv ebben a cikkben leírt lépéseket:

### <a name="step-1-configure-hello-secondary-nics-for-each-vm"></a>1. lépés: Konfigurálja az egyes virtuális gépek hello a másodlagos hálózati adapterrel

A virtuális hálózaton lévő összes virtuális Géphez beállított IP-konfigurációjának megadása fel hello a másodlagos hálózati adapter az alábbiak szerint:  

1. Egy böngészőből keresse meg a toohello Azure-portálon: http://portal.azure.com és az Azure-fiókkal történő bejelentkezés.
2. Hello felső bal oldalán található üdvözlő képernyőt, hello erőforráscsoport ikonra, és kattintson a virtuális gépek találhatók hello erőforrás csoport hello (például *contosofabrikam*). Hello **erőforráscsoportok** panel, amelyen a virtuális gépek hello hello hálózati adapterrel együtt hello erőforrásainak listája jelenik meg.
3. toohello másodlagos hálózati Adapterének virtuális gépek hozzáadása egy IP-konfiguráció használata a következőképpen történik:
    1. Válassza ki a kívánt tooadd hello IP-konfiguráció hello hálózati illesztőt.
    2. Hello paneljén megjelenő hello kiválasztott hálózati kattintson **IP-konfigurációk**. Kattintson a **Hozzáadás** hello felső részén megjelenő hello panel felé.
    3. A hello **hozzáadása IP-konfigurációk** panelen az alábbiak szerint adja hozzá a második IP-konfiguráció toohello NIC: 
        1. Adja meg a másodlagos IP-konfiguráció nevét (például a VM1 és vm2 virtuális gépnek neve hello IP-konfigurációk, *VM1NIC2-ipconfig2* és *VM2NIC2-ipconfig2* rendre).
        2. A **magánhálózati IP-cím**, a **foglalási**, jelölje be **statikus**.
        3. Kattintson az **OK** gombra.
        4. Ha második IP-konfiguráció hello hello másodlagos hálózati adapter befejeződött, az megjelenik hello **IP-konfigurációk** hello megadott hálózati adaptert. a beállítások panel

### <a name="step-2-create-a-load-balancer"></a>2. lépés:, Hozzon létre egy adott terheléselosztóhoz

Hozzon létre egy terhelés-kiegyenlítő az alábbiak szerint:

1. Egy böngészőből keresse meg a toohello Azure-portálon: http://portal.azure.com és az Azure-fiókkal történő bejelentkezés.
2. Hello felső bal oldalán található üdvözlő képernyőt, kattintson a **új** > **hálózati** > **terheléselosztó**. Ezután kattintson **létrehozása**.
3. A hello **létrehozás terheléselosztó** panelen írja be a terheléselosztó nevét. Itt azt nevezzük *mylb*.
4. A nyilvános IP-cím, hozzon létre egy új nyilvános IP-cím nevű **PublicIP1**.
5. Az erőforráscsoport, válassza ki a virtuális gépek meglévő erőforráscsoportot hello (például *contosofabrikam*). Válasszon egy megfelelő helyen, majd kattintson a **OK**. hello terheléselosztó toodeploy majd elindul, és toosuccessfully teljes telepítési eltarthat néhány percig.
6. Amennyiben a telepített, hello terheléselosztó az erőforráscsoportban erőforrásként jelenik meg.

### <a name="step-3-configure-hello-frontend-ip-pool"></a>3. lépés: Hello előtérbeli IP-készlet konfigurálása

Az alábbiak szerint konfigurálhatja az előtérbeli IP-címkészlet (a Contoso és Fabrikam) minden webhelyre vonatkozóan:

1. Hello portálon kattintson **további szolgáltatások** > típus **nyilvános IP-cím** hello a Szűrő mezőbe, és kattintson a **nyilvános IP-címek**. Kattintson a **Hozzáadás** hello felső részén megjelenő hello panel felé.
2. Két nyilvános IP-címek konfigurálása (*PublicIP1* és *PublicIP2*) mindkét webhelyekhez (contoso és fabrikam) az alábbiak szerint:
    1. Adjon meg egy nevet az előtérbeli IP-címe.
    2. A **erőforráscsoport**, válassza ki a virtuális gépek hello meglévő erőforráscsoportot hello (például *contosofabrikam*).
    3. A **hely**, válassza ki hello ugyanazon a helyen, mivel a virtuális gépek hello.
    4. Kattintson az **OK** gombra.
    5. Hello két nyilvános IP-címek létrehozása után is látható hello **nyilvános IP-cím** címek panelen.
3. Hello portálon kattintson **további szolgáltatások** > típus **terheléselosztó** hello a Szűrő mezőbe, és kattintson a **terheléselosztó**.  
4. Válassza ki a terheléselosztó hello (*mylb*), amelyet az tooadd hello előtérbeli IP-készlet.
5. A **beállítások**, jelölje be **előtér-készletek**. Kattintson a **Hozzáadás** hello felső részén megjelenő hello panel felé.
6. Adjon meg egy nevet az előtérbeli IP-cím (*farbikamfe* vagy **contosofe*).
7. Kattintson a **IP-cím** a hello **válassza a nyilvános IP-cím** panelen, jelölje be hello IP-címet az előtér (*PublicIP1* vagy *PublicIP2*).
8. Ismételje meg a lépéseket 3 too7 Ez a szakasz toocreate hello második előtérbeli IP-címen belül.
9. Ha hello előtérbeli IP-címkészlet konfigurációja befejeződött, mindkét előtérbeli IP-címek jelennek meg hello **előtérbeli IP-készlet** panel a terheléselosztó. 
    
### <a name="step-4-configure-hello-backend-pool"></a>4. lépés: Hello háttérkészlet konfigurálása   
Az alábbiak szerint konfigurálja hello háttércímkészletek a terheléselosztó minden webhelyhez (Contoso és Fabrikam):
        
1. Hello portálon kattintson **további szolgáltatások** > írja be a terheléselosztó hello szűrő mezőbe, és kattintson **terheléselosztó**.  
2. Válassza ki a terheléselosztó hello (*mylb*), amelyet az tooadd hello háttérkészletek menüpontot a.
3. A **beállítások**, jelölje be **Háttérkészletek**. Adjon meg egy nevet a háttérkészlet (például *contosopool* vagy *fabrikampool*). Kattintson a hello **Hozzáadás** gomb hello felső részén megjelenő hello panel felé. 
4. A **társított**, jelölje be **rendelkezésre állási csoport**.
5. A **rendelkezésre állási csoport**, jelölje be **myAvailset**.
6. Hozzáadása az alábbiak szerint cél hálózati IP-konfigurációk, mindkét virtuális gépek (lásd a 2. ábra):  
    1. A **cél virtuális gép**, válassza ki a virtuális gép, amelyet az tooadd toohello háttérkészlet (például VM1 vagy vm2 virtuális gépnek) hello.
    2. A **hálózati IP-konfiguráció**, jelölje be ezt a virtuális Gépet (például VM1NIC2-ipconfig2 vagy VM2NIC2-ipconfig2) hello másodlagos hálózati adapter IP-konfigurációval.
    ![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
            
        **2. ábra**: hello terheléselosztó háttérkészletek konfigurálása  
7. Kattintson az **OK** gombra.
8. Hello készlet háttérkonfiguráció befejeződése után mindkét háttércímkészletek jelennek-e hello **háttér címkészletet panel** a terheléselosztó.

### <a name="step-5-configure-a-health-probe-for-your-load-balancer"></a>5. lépés: A terheléselosztóhoz egy állapotmintáihoz konfigurálása
Az alábbiak szerint adja meg a terheléselosztóhoz egy állapotmintáihoz:
    1. Hello portálon, kattintson a további szolgáltatások > írja be a terheléselosztó hello szűrő mezőbe, és kattintson **terheléselosztó**.  
    2. Válassza ki a tooadd hello háttérkészletek menüpontot a használni kívánt terheléselosztó hello.
    3. A **beállítások**, jelölje be **állapotmintáihoz**. Kattintson a **Hozzáadás** hello felső részén megjelenő hello panel felé.
    4. Adjon meg egy nevet hello állapotmintáihoz (például HTTP), és kattintson a **OK**.

### <a name="step-6-configure-load-balancing-rules"></a>6. lépés: A terheléselosztási szabályok konfigurálása
A terheléselosztási szabályok konfigurálása (*HTTPc* és *HTTPf*) minden webhely, az alábbiak szerint:
    
1. A **beállítások**, jelölje be **állapotmintáihoz**. Kattintson a **Hozzáadás** hello felső részén megjelenő hello panel felé.
2. A **neve**, írja be az hello terheléselosztási szabály nevét (például *HTTPc* a Contoso, vagy *HTTPf* a Fabrikam)
3. Az előtér-IP-címet, válassza ki a hello hello front-end IP-címet (például *Contosofe* vagy *Fabrikamfe*)
4. A **Port** és **háttérportot**, tartsa hello alapértelmezett értéket **80**.
5. A **fix IP-Címek (közvetlen kiszolgálói válasz)**, kattintson a **engedélyezve**.
6. Kattintson az **OK** gombra.
7. Ismételje meg a lépéseket 1 too6 Ez a szakasz toocreate hello második terheléselosztó szabály belül.
8. Ha hello terheléselosztási szabályok konfigurálása nem fejeződött be, a szabályokat is ((*HTTPc* és *HTTPf*) jelennek meg hello **terheléselosztási szabályok** panelen található a terheléselosztón.

### <a name="step-7-configure-dns-records"></a>7. lépés: A DNS-rekordok konfigurálása
Végül konfigurálnia kell DNS erőforrás rekordok toopoint toohello megfelelő előtérbeli IP-cím hello terheléselosztó. A tartományok Azure DNS-ben is tartalmazhat. Az Azure DNS-sel terheléselosztással kapcsolatos további információkért lásd: [Azure DNS használata más Azure-szolgáltatásokkal](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Következő lépések
- További tudnivalók az Azure-ban hogyan toocombine terheléselosztás szolgáltatások [terheléselosztás szolgáltatások használata az Azure-ban](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Megtudhatja, hogyan naplók különböző típusait használják az Azure toomanage és hibaelhárítása a terheléselosztó [analytics keresse meg a Azure terheléselosztó](../load-balancer/load-balancer-monitor-log.md).
