---
title: "Több IP-konfigurációk az Azure-ban a terheléselosztási |} Microsoft Docs"
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
ms.openlocfilehash: cf1e68c7b37b2506de007bdf24eea63a27187a33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-the-azure-portal"></a>Az Azure portál használatával több IP-konfigurációk terheléselosztás

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [Parancssori felület](load-balancer-multiple-ip-cli.md)

Ez a cikk ismerteti az Azure Load Balancer használata a másodlagos hálózati adapteren (NIC) több IP-címmel. Ebben a forgatókönyvben két virtuális gépeken futó Windows, az elsődleges és másodlagos hálózati tudunk A másodlagos hálózati adapterrel rendelkezhetnek két IP-konfigurációk. Minden virtuális gép webhelyeket a contoso.com és fabrikam.com üzemelteti. Minden webhelyre van kötve egy IP-konfigurációk másodlagos hálózati adapteren Azure Load Balancer használatával teszi közzé a két előtérbeli IP-cím, egy, a megfelelő IP-konfiguráció a webhelyre irányuló forgalom terjeszteni minden webhelyre vonatkozóan. Ebben a példában ugyanazt a portszámot is frontends, valamint mindkét háttér címkészletet IP-címek között.

![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

##<a name="prerequisites"></a>Előfeltételek
Ez a példa feltételezi, hogy rendelkezik-e nevű erőforráscsoport *contosofabrikam* a következő beállításokkal:
 -  magában foglalja a virtuális hálózati nevű *myVNet*, két virtuális gépek nevű *VM1* és *vm2 virtuális gépnek* belül az azonos rendelkezésre állási beállítása a nevesített *myAvailset*. 
 - minden virtuális gép rendelkezik egy elsődleges hálózati adapter és a másodlagos hálózati adaptert. Az elsődleges hálózati adapter neve *VM1NIC1* és *VM2NIC1* és a másodlagos hálózati adapterek megnevezett *VM1NIC2* és *VM2NIC2*. Több hálózati adapterrel rendelkező virtuális gépek létrehozásával kapcsolatos további információkért lásd: [PowerShell segítségével több hálózati adapterrel rendelkező virtuális gép létrehozása](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>A több IP-konfigurációk terheléselosztásához lépései

Kövesse az alábbi cikkben leírt forgatókönyv eléréséhez az alábbi lépéseket:

### <a name="step-1-configure-the-secondary-nics-for-each-vm"></a>1. lépés: A másodlagos hálózati adapterek konfigurálása az egyes virtuális gépek

Az egyes virtuális gépek a virtuális hálózat az alábbiak szerint adja hozzá IP-konfiguráció beállítása a másodlagos hálózati adapter:  

1. Egy böngészőből keresse meg az Azure-portálon: http://portal.azure.com és az Azure-fiókkal történő bejelentkezés.
2. A képernyő felső bal oldalán kattintson az erőforráscsoport ikonra, és kattintson a az erőforráscsoportot, a virtuális gépek találhatók (például *contosofabrikam*). A **erőforráscsoportok** panel, amelyen a virtuális gépek jeleníti meg a hálózati adapterrel együtt tárolt összes erőforrás megjelenik.
3. Minden virtuális gép másodlagos hálózati adapter vegye fel IP-konfigurációt az alábbiak szerint:
    1. Jelölje be a hálózati adaptert, az IP-konfiguráció hozzáadásához.
    2. A kiválasztott hálózati adapter megjelenő panelen kattintson **IP-konfigurációk**. Kattintson a **Hozzáadás** felé be a panel tetején.
    3. Az a **hozzáadása IP-konfigurációk** panelen, a második IP-konfiguráció hozzáadása a hálózati adapter az alábbiak szerint: 
        1. Adja meg a másodlagos IP-konfiguráció nevét (például a VM1 és vm2 virtuális gépnek neve, IP-konfigurációk *VM1NIC2-ipconfig2* és *VM2NIC2-ipconfig2* rendre).
        2. A **magánhálózati IP-cím**, a **foglalási**, jelölje be **statikus**.
        3. Kattintson az **OK** gombra.
        4. Ha a második, a másodlagos hálózati adapter IP-konfiguráció befejeződött, megjelenik a **IP-konfigurációk** beállítások panelen a megadott hálózati adaptert.

### <a name="step-2-create-a-load-balancer"></a>2. lépés:, Hozzon létre egy adott terheléselosztóhoz

Hozzon létre egy terhelés-kiegyenlítő az alábbiak szerint:

1. Egy böngészőből keresse meg az Azure-portálon: http://portal.azure.com és az Azure-fiókkal történő bejelentkezés.
2. A képernyő felső bal oldalán kattintson **új** > **hálózati** > **terheléselosztó**. Ezután kattintson **létrehozása**.
3. A **Terheléselosztó létrehozása** panelen írja be a terheléselosztó nevét. Itt azt nevezzük *mylb*.
4. A nyilvános IP-cím, hozzon létre egy új nyilvános IP-cím nevű **PublicIP1**.
5. Az erőforráscsoport, válassza ki a meglévő erőforráscsoportot a virtuális gépek (például *contosofabrikam*). Válasszon egy megfelelő helyen, majd kattintson a **OK**. Ekkor elindul a terheléselosztó üzembe helyezése, ami néhány perc alatt sikeresen befejeződik.
6. Amennyiben a telepített, a terheléselosztó az erőforráscsoportban erőforrásként jelenik meg.

### <a name="step-3-configure-the-frontend-ip-pool"></a>3. lépés: Az előtérbeli IP-címkészlet konfigurálása

Az alábbiak szerint konfigurálhatja az előtérbeli IP-címkészlet (a Contoso és Fabrikam) minden webhelyre vonatkozóan:

1. Kattintson a portál **további szolgáltatások** > típus **nyilvános IP-cím** a Szűrő mezőbe, majd **nyilvános IP-címek**. Kattintson a **Hozzáadás** felé be a panel tetején.
2. Két nyilvános IP-címek konfigurálása (*PublicIP1* és *PublicIP2*) mindkét webhelyekhez (contoso és fabrikam) az alábbiak szerint:
    1. Adjon meg egy nevet az előtérbeli IP-címe.
    2. A **erőforráscsoport**, válassza ki a meglévő erőforráscsoportot a virtuális gépek (például *contosofabrikam*).
    3. A **hely**, jelölje ki a virtuális gépek ugyanazon a helyen.
    4. Kattintson az **OK** gombra.
    5. A két nyilvános IP-címek létrehozása után is megjelennek a **nyilvános IP-cím** címek panelen.
3. A portálon kattintson **további szolgáltatások** > típus **terheléselosztó** a Szűrő mezőbe, majd **terheléselosztó**.  
4. Válassza ki a terheléselosztó (*mylb*), hogy szeretné-e az előtérbeli IP-címkészlet való hozzáadása.
5. A **beállítások**, jelölje be **előtér-készletek**. Kattintson a **Hozzáadás** felé be a panel tetején.
6. Adjon meg egy nevet az előtérbeli IP-cím (*farbikamfe* vagy **contosofe*).
7. Kattintson a **IP-cím** és az a **válassza a nyilvános IP-cím** panelen, jelölje be az IP-címek az előtér (*PublicIP1* vagy *PublicIP2*).
8. Ismételje meg a 3-7 ebben a szakaszban a második előtérbeli IP-cím létrehozásához.
9. Ha az előtérbeli IP-címkészlet konfigurációja befejeződött, mindkét előtérbeli IP-címek jelennek meg a **előtérbeli IP-készlet** panel a terheléselosztó. 
    
### <a name="step-4-configure-the-backend-pool"></a>4. lépés: A háttérkészlet konfigurálása   
Az alábbiak szerint konfigurálja a háttércímkészletek a terheléselosztó minden webhelyhez (Contoso és Fabrikam):
        
1. Kattintson a portál **további szolgáltatások** > írja be a terheléselosztó szót a Szűrő mezőbe, és kattintson **terheléselosztó**.  
2. Válassza ki a terheléselosztó (*mylb*), hogy szeretné-e a háttérkészlet, a hozzá.
3. A **beállítások**, jelölje be **Háttérkészletek**. Adjon meg egy nevet a háttérkészlet (például *contosopool* vagy *fabrikampool*). Kattintson a **Hozzáadás** a panelen megjelenő tetejénél gombra. 
4. A **társított**, jelölje be **rendelkezésre állási csoport**.
5. A **rendelkezésre állási csoport**, jelölje be **myAvailset**.
6. Hozzáadása az alábbiak szerint cél hálózati IP-konfigurációk, mindkét virtuális gépek (lásd a 2. ábra):  
    1. A **cél virtuális gép**, válassza ki a virtuális Gépet, amely hozzá szeretne adni a háttérkészletbe (például VM1 vagy vm2 virtuális gépnek).
    2. A **hálózati IP-konfiguráció**, jelölje be ezt a virtuális gépet (például VM1NIC2-ipconfig2 vagy VM2NIC2-ipconfig2) másodlagos hálózati adapter IP-konfigurációja.
    ![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
            
        **2. ábra**: konfigurálja a terheléselosztó háttérkészlet  
7. Kattintson az **OK** gombra.
8. Amikor készletbe háttérkonfiguráció befejeződött, mindkét háttércímkészletek jelennek meg a **háttér címkészletet panel** a terheléselosztó.

### <a name="step-5-configure-a-health-probe-for-your-load-balancer"></a>5. lépés: A terheléselosztóhoz egy állapotmintáihoz konfigurálása
Az alábbiak szerint adja meg a terheléselosztóhoz egy állapotmintáihoz:
    1. A portálon, kattintson a további szolgáltatások > írja be a terheléselosztó szót a Szűrő mezőbe, és kattintson **terheléselosztó**.  
    2. Válassza ki a terheléselosztóhoz, amely a háttérkészlet való hozzáadásához.
    3. A **beállítások**, jelölje be **állapotmintáihoz**. Kattintson a **Hozzáadás** felé be a panel tetején.
    4. Adjon meg egy nevet a állapotmintáihoz (például HTTP), és kattintson a **OK**.

### <a name="step-6-configure-load-balancing-rules"></a>6. lépés: A terheléselosztási szabályok konfigurálása
A terheléselosztási szabályok konfigurálása (*HTTPc* és *HTTPf*) minden webhely, az alábbiak szerint:
    
1. A **beállítások**, jelölje be **állapotmintáihoz**. Kattintson a **Hozzáadás** felé be a panel tetején.
2. A **neve**, írja be a terheléselosztási szabály nevét (például *HTTPc* a Contoso, vagy *HTTPf* a Fabrikam)
3. Előtérbeli IP-címet, válassza ki az előtérbeli IP-címét (például *Contosofe* vagy *Fabrikamfe*)
4. A **Port** és **háttérportot**, tartsa meg az alapértelmezett értéket **80**.
5. A **fix IP-Címek (közvetlen kiszolgálói válasz)**, kattintson a **engedélyezve**.
6. Kattintson az **OK** gombra.
7. Ismételje meg a 1-6 Ebben a szakaszban a második terheléselosztó szabály létrehozásához.
8. Ha a terheléselosztási szabályok konfigurálása nem fejeződött be, a szabályokat is ((*HTTPc* és *HTTPf*) jelennek meg a **terheléselosztási szabályok** panelen található a terheléselosztón.

### <a name="step-7-configure-dns-records"></a>7. lépés: A DNS-rekordok konfigurálása
Végül konfigurálnia kell DNS-erőforrásrekordok a terheléselosztó megfelelő előtérbeli IP-címére mutasson. A tartományok Azure DNS-ben is tartalmazhat. Az Azure DNS-sel terheléselosztással kapcsolatos további információkért lásd: [Azure DNS használata más Azure-szolgáltatásokkal](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Következő lépések
- További tudnivalók az Azure a terheléselosztási egyesítése [terheléselosztás szolgáltatások használata az Azure-ban](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Ismerje meg, hogyan használhatja különféle naplók az Azure-ban kezelésére és hibaelhárítására terheléselosztó [analytics keresse meg a Azure terheléselosztó](../load-balancer/load-balancer-monitor-log.md).
