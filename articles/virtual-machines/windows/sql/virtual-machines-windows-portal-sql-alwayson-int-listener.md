---
title: "egy SQL Server rendelkezésre állási csoport figyelőjének az Azure virtuális gépeken aaaCreate |} Microsoft Docs"
description: "Részletes útmutatást ad a figyelő egy Always On rendelkezésre állási csoport létrehozása az SQL Server Azure virtuális gépeken"
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/01/2017
ms.author: mikeray
ms.openlocfilehash: c6a44dc5c7c18b572c2bf5772b4651b7210aacbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a>A terheléselosztó egy Always On rendelkezésre állási csoport konfigurálása az Azure-ban
Ez a cikk azt ismerteti, hogyan toocreate egy terheléselosztót egy SQL Server Always On rendelkezésre állási csoport az Azure virtuális gépen is fut az Azure Resource Manager eszközzel. Rendelkezésre állási csoport egy adott terheléselosztóhoz szükséges, ha a hello SQL-kiszolgálópéldányok el vannak az Azure virtuális gépeken. hello terheléselosztó hello rendelkezésre állási csoport figyelőjének hello IP-címét tárolja. Rendelkezésre állási csoport több régióba is, ha minden egyes régió egy terhelés-kiegyenlítő van szüksége.

toocomplete ebben a feladatban van szüksége a Resource Manager rendszert futtató Azure virtuális gépeken telepített egy SQL Server rendelkezésre állási csoport toohave. Mindkét SQL Server virtuális gépek kell tartozniuk toohello azonos rendelkezésre állási csoportot. Használhatja a hello [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically hello rendelkezésre állási csoport létrehozására az erőforrás-kezelőben. Ez a sablon automatikusan létrehoz egy belső terheléselosztót. 

Ha kívánja, akkor [manuálisan konfigurálnia a rendelkezésre állási csoport](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Ez a cikk megköveteli, hogy a rendelkezésre állási csoportok már be van állítva.  

Kapcsolódó témakörök az alábbiak:

* [Always On rendelkezésre állási csoportok konfigurálása az Azure virtuális gép (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Egy VNet – VNet-kapcsolat beállítása az Azure Resource Manager és a PowerShell használatával](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Érdekében ez a cikk keresztül, hozzon létre, és konfigurálja a terheléselosztó hello Azure-portálon. Hello folyamat befejezése után konfigurálnia hello toouse hello IP-címet terheléselosztóról hello hello elérhetőségi csoport figyelője.

## <a name="create-and-configure-hello-load-balancer-in-hello-azure-portal"></a>Létrehozhat és konfigurálhat hello terheléselosztó hello Azure-portálon
Az hello ezen részén hello feladat a következő:

1. Hello terheléselosztó létrehozása hello Azure-portálon, és hello IP-címének beállítása.
2. Hello háttér-készlet beállítása.
3. Hello mintavétel létrehozása. 
4. Hello terheléselosztási szabályok beállítása.

> [!NOTE]
> Ha több erőforráscsoportok és régiókban hello SQL Server-példányokat, hajtsa végre az kétszer, egyszer minden erőforráscsoportban.
> 
> 

### <a name="step-1-create-hello-load-balancer-and-configure-hello-ip-address"></a>1. lépés: Hello terheléselosztó létrehozása, és hello IP-cím konfigurálása
Először hozzon létre hello terheléselosztóhoz. 

1. Hello Azure-portálon nyissa meg hello SQL Server virtuális gépeket tartalmazó erőforráscsoport hello. 

2. Hello erőforráscsoportban, kattintson a **Hozzáadás**.

3. Keresse meg **terheléselosztó** majd hello keresési eredmények között, jelölje ki **terheléselosztó**, által közzétett **Microsoft**.

4. A hello **terheléselosztó** panelen kattintson a **létrehozása**.

5. A hello **létrehozás terheléselosztó** párbeszédpanel hello terheléselosztó adja meg az alábbiak szerint:

   | Beállítás | Érték |
   | --- | --- |
   | **Name (Név)** |Hello terheléselosztó képviselő szöveges nevét. Például **sqlLB**. |
   | **Típus** |**Belső**: a legtöbb megvalósítások használja egy belső terheléselosztó, amely lehetővé teszi az alkalmazások hello belül azonos virtuális hálózati tooconnect toohello rendelkezésre állási csoport.  </br> **Külső**: lehetővé teszi az alkalmazások tooconnect toohello rendelkezésre állási csoport nyilvános internetkapcsolaton keresztül. |
   | **Virtuális hálózat** |Válassza ki, amelyek hello SQL Server intances hello virtuális hálózat. |
   | **Alhálózat** |Válassza ki, amelyek az SQL Server-példányokat hello hello alhálózat. |
   | **IP-cím hozzárendelése** |**Statikus** |
   | **Magánhálózati IP-cím** |Adja meg a hello alhálózat elérhető IP-címeit. Használja az IP-cím, egy figyelő hello fürt létrehozásakor. Egy PowerShell-parancsfájlt, a cikk későbbi részében használni ezt a címet hello `$ILBIP` változó. |
   | **Előfizetés** |Ha több előfizetéssel rendelkezik, ez a mező jelenhet meg. Válassza ki, hogy szeretné-e az ehhez az erőforráshoz tooassociate hello előfizetést. Azt az általában hello ugyanahhoz az előfizetéshez hello rendelkezésre állási csoporthoz tartozó összes hello erőforrás. |
   | **Erőforráscsoport** |Válassza ki, amelyek az SQL Server-példányokat hello hello erőforráscsoport. |
   | **Hely** |Válassza ki a hello Azure-beli hely, amelyek hello SQL Server-példányokat. |

6. Kattintson a **Create** (Létrehozás) gombra. 

Azure hello terheléselosztót hoz létre. hello terheléselosztóhoz tartozik, adott hálózati tooa, alhálózaton, erőforráscsoportot és helyet. Azure hello feladat befejezése után ellenőrizze a hello terheléselosztási beállításokat az Azure-ban. 

### <a name="step-2-configure-hello-back-end-pool"></a>2. lépés: Hello háttér-készlet konfigurálása
Az Azure hívások hello háttér címkészletet *háttérkészlet*. Ebben az esetben a hello háttér-készlet hello címek hello két SQL Server-példányok a rendelkezésre állási csoportban. 

1. Az erőforráscsoportban kattintson a létrehozott hello terheléselosztóhoz. 

2. A **beállítások**, kattintson a **háttérkészletek**.

3. A **háttérkészletek**, kattintson a **Hozzáadás** toocreate egy háttér címkészletet. 

4. A **háttérkészlet hozzáadása**a **neve**, adjon meg egy nevet hello háttér-készlet.

5. A **virtuális gépek**, kattintson a **adja hozzá a virtuális gépek**. 

6. A **válassza ki a virtuális gépek**, kattintson a **rendelkezésre állási csoport kiválasztása**, majd adja meg a hello rendelkezésre állási csoportban, hogy hello SQL Server virtuális gépek tartozik.

7. Hello rendelkezésre állási csoport kiválasztása után kattintson **válassza ki a virtuális gépek hello**, válasszon hello két olyan virtuális gépet, amely az SQL Server-példányokat hello hello rendelkezésre állási csoportban, és kattintson **Válasszon**. 

8. Kattintson a **OK** tooclose hello paneleket az **válassza ki a virtuális gépek**, és **háttérkészlet hozzáadása**. 

Azure frissíti hello háttér címkészletet hello beállításait. A rendelkezésre állási csoport már két SQL Server-példányokat készletét.

### <a name="step-3-create-a-probe"></a>3. lépés: A mintavétel létrehozása
hello mintavételi határozza meg, hogyan Azure ellenőrzi, amelyek hello SQL Server-példány éppen birtokolja hello rendelkezésre állási csoport figyelőjét. Azure-vizsgálat hello szolgáltatás hello mintavételi létrehozásakor meghatározó port hello IP-címe alapján.

1. Hello terheléselosztó **beállítások** panelen kattintson a **állapot-mintavételi csomagjai**. 

2. A hello **állapot-mintavételi csomagjai** panelen kattintson a **Hozzáadás**.

3. Hello mintavétel konfigurálása hello **Hozzáadás mintavételi** panelen. A következő értékek tooconfigure hello mintavételi használata hello:

   | Beállítás | Érték |
   | --- | --- |
   | **Name (Név)** |Hello mintavételi képviselő szöveges nevét. Például **SQLAlwaysOnEndPointProbe**. |
   | **Protocol (Protokoll)** |**TCP** |
   | **Port** |A rendelkezésre álló portot is használhat. Például *59999*. |
   | **Időköz** |*5* |
   | **Sérült küszöbérték** |*2* |

4.  Kattintson az **OK** gombra. 

> [!NOTE]
> Győződjön meg arról, hogy a megadott hello port a hello tűzfalat mindkét SQL Server-példányok nyitva-e. Mindkét esetben szükséges egy bejövő forgalomra vonatkozó szabály hello TCP-portot használja. További információkért lásd: [hozzáadása vagy szerkesztése tűzfalszabály](http://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure hello mintavételi hoz létre, és melyik SQL Server-példány hello figyelő hello rendelkezésre állási csoport rendelkezik tootest használja.

### <a name="step-4-set-hello-load-balancing-rules"></a>4. lépés: Hello terheléselosztási szabályok beállítása
hello terheléselosztási szabályok konfigurálása, hogyan hello terheléselosztó irányítja a forgalmat toohello SQL Server-példányokat. Ez a terheléselosztó engedélyeznie a közvetlen kiszolgálói válasz mert hello két SQL Server-példányok csak az egyik hello rendelkezésre állási csoport figyelőjének erőforrás tulajdonosa egyszerre.

1. Hello terheléselosztó **beállítások** panelen kattintson a **terheléselosztási szabályok**. 

2. A hello **terheléselosztási szabályok** panelen kattintson a **Hozzáadás**.

3. A hello **Hozzáadás terheléselosztási szabályok** panelen hello terheléselosztási szabály konfigurálása. A következő beállítások hello használata: 

   | Beállítás | Érték |
   | --- | --- |
   | **Name (Név)** |Hello terheléselosztási szabályok képviselő szöveges nevét. Például **SQLAlwaysOnEndPointListener**. |
   | **Protocol (Protokoll)** |**TCP** |
   | **Port** |*1433* |
   | **Háttér-Port** |*1433*. Rendszer figyelmen kívül hagyja ezt az értéket, mert ez a szabály **fix IP-Címek (közvetlen kiszolgálói válasz)**. |
   | **Hálózatfigyelő** |Hello mintavételi létrehozott hello nevét használni a terheléselosztóhoz. |
   | **Munkamenet megőrzését** |**Egyik sem** |
   | **Üresjárati időkorlátja (perc)** |*4* |
   | **Lebegőpontos IP (közvetlen kiszolgálói válasz)** |**Engedélyezve** |

   > [!NOTE]
   > Lehetséges, hogy le hello panel tooview tooscroll összes hello-beállítások.
   > 

4. Kattintson az **OK** gombra. 
5. Azure hello terheléselosztási szabály konfigurálása Hello terheléselosztó most konfigurált tooroute forgalom toohello SQL Server-példány hello figyelő hello rendelkezésre állási csoporthoz. 

Ezen a ponton hello erőforráscsoport rendelkezik olyan terheléselosztóhoz, amely az SQL Server-gépek tooboth csatlakozik. hello terheléselosztó hello SQL Server Always On rendelkezésre állási csoport figyelőjét, IP-címet is tartalmazza, hogy bármelyik számítógép válaszolhassanak toorequests hello rendelkezésre állási csoportok számára.

> [!NOTE]
> Ha az SQL Server-példány két külön régióban, ismételje meg a hello hello a más régióban. Minden egyes régió egy terheléselosztót igényel. 
> 
> 

## <a name="configure-hello-cluster-toouse-hello-load-balancer-ip-address"></a>Hello fürt toouse hello load balancer IP-cím konfigurálása
következő lépés hello tooconfigure hello figyelő hello fürtön, és hello figyelő online állapotba. A következő hello: 

1. Hozzon létre hello rendelkezésre állási csoport figyelőjének hello feladatátvevő fürtön. 

2. Hello figyelő online állapotba.

### <a name="step-5-create-hello-availability-group-listener-on-hello-failover-cluster"></a>5. lépés: Hello rendelkezésre állási csoport figyelőjének létrehozása hello feladatátvevő fürtön
Ebben a lépésben kézzel létrehozhat hello rendelkezésre állási csoport figyelőjének a Feladatátvevőfürt-kezelő és az SQL Server Management Studio.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-hello-configuration-of-hello-listener"></a>Hello figyelő hello konfigurációjának ellenőrzése

Ha hello fürterőforrások és a függőségek helyesen van konfigurálva, az SQL Server Management Studio képes tooview hello figyelő legyen. tooset hello figyelő portja, a következő hello:

1. Indítsa el az SQL Server Management Studio eszközt, és csatlakoztassa a toohello elsődleges másodpéldány.

2. Nyissa meg túl**AlwaysOn magas rendelkezésre állású** > **rendelkezésre állási csoportok** > **rendelkezésre állási csoport figyelői**.  
    Most látnia kell a Feladatátvevőfürt-kezelő létrehozta hello figyelő nevét. 

3. Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson **tulajdonságok**.

4. A hello **Port** hello rendelkezésre állási csoport figyelőjének hello portszáma hello segítségével adja meg a korábban használt $EndpointPort (1433 volt hello alapértelmezett), és kattintson a **OK**.

Most már rendelkezik egy rendelkezésre állási csoport erőforrás-kezelő módban futó Azure virtuális gépeken. 

## <a name="test-hello-connection-toohello-listener"></a>Teszt hello kapcsolat toohello figyelő
Hello kapcsolat tesztelése hello következő tevékenységek végrehajtásával:

1. RDP tooa SQL Server-példányt, amely hello azonos virtuális hálózat, de nem saját hello replika. A kiszolgáló más SQL Server-példány hello fürt hello is lehet.

2. Használjon **sqlcmd** segédprogram tootest hello kapcsolat. Például a következő parancsfájl hello hoz létre egy **sqlcmd** kapcsolat toohello elsődleges replika hello figyelő Windows-hitelesítés használatával:
   
        sqlcmd -S <listenerName> -E

hello SQLCMD kapcsolat automatikusan csatlakozik a hello elsődleges másodpéldányt futtató toohello SQL Server-példányt. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Egy IP-cím, egy további rendelkezésre állási csoport létrehozása

Egyes rendelkezésre állási csoport egy külön figyelő használja. Minden egyes figyelő saját IP-címmel rendelkezik. Használja az azonos hello további figyelők terheléselosztó toohold hello IP-cím betölteni. Miután létrehozott egy rendelkezésre állási csoportban, hello IP cím toohello terheléselosztó hozzáadása, és adja meg hello figyelő.

az IP cím tooa rendelkező terheléselosztó hello Azure-portálon tooadd hello a következő:

1. Hello Azure-portálon nyissa meg a terheléselosztó hello tartalmazó erőforráscsoportot hello, és kattintson a hello terheléselosztó. 

2. A **beállítások**, kattintson a **előtér-IP-címkészlet**, és kattintson a **Hozzáadás**. 

3. A **előtérbeli IP-cím hozzáadása**, rendelje hozzá a hello előtér nevét. 

4. Győződjön meg arról, hogy hello **virtuális hálózati** és hello **alhálózati** megegyezik az SQL Server-példányokat hello hello.

5. Hello figyelő hello IP-címének beállítása. 
   
   >[!TIP]
   >IP-cím toostatic hello beállítása, és adjon meg egy címet, amely jelenleg nincs használatban hello alhálózaton. Azt is megteheti IP-cím toodynamic hello beállítása és mentése hello új előtér-IP-készlet. Ha így tesz, hello Azure-portálon automatikusan hozzárendel egy elérhető IP-cím toohello készletből. Ezután nyissa meg újra a hello előtér-IP-készlet és hello hozzárendelés toostatic módosítása. 

6. Mentse a hello figyelő hello IP-címet. 

7. Adja hozzá a állapotmintáihoz hello-beállítások a következő használatával:

   |Beállítás |Érték
   |:-----|:----
   |**Name (Név)** |A név tooidentify hello mintavétel.
   |**Protocol (Protokoll)** |TCP
   |**Port** |Egy nem használt TCP-port, amelyen az összes virtuális gép rendelkezésre kell állnia. Semmilyen más célra nem használható. Nincs két figyelői hello használhatja ugyanazt a mintavételi portot. 
   |**Időköz** |hello időn közötti mintavételi kísérletek. Hello alapértelmezett (5) használja.
   |**Sérült küszöbérték** |egymást követő küszöbértékeket, amelyek a virtuális gép nem kell hello száma nem megfelelő állapotúnak számít.

8. Kattintson a **OK** toosave hello mintavétel. 

9. Terheléselosztási szabály létrehozása. Kattintson a **terheléselosztási szabályok**, és kattintson a **Hozzáadás**.

10. Hello új terheléselosztási szabály használatával a következő beállítások hello konfigurálása:

   |Beállítás |Érték
   |:-----|:----
   |**Name (Név)** |A név tooidentify hello terheléselosztási szabály betöltése. 
   |**Előtérbeli IP-cím** |Válassza ki a létrehozott hello IP-címet. 
   |**Protocol (Protokoll)** |TCP
   |**Port** |Hello SQL Server-példány által használt hello port használatára. Egy alapértelmezett példány 1433-as portot használja, kivéve, ha módosította az. 
   |**Háttér-port** |Ugyanaz, mint érték használata hello **Port**.
   |**Háttérkészlet** |virtuális gépek hello hello SQL Server-példányokat tartalmazó hello készlet. 
   |**Állapotmintáihoz** |Válassza ki a létrehozott hello mintavétel.
   |**Munkamenet megőrzését** |None
   |**Üresjárati időkorlátja (perc)** |Alapértelmezett (4)
   |**Lebegőpontos IP (közvetlen kiszolgálói válasz)** | Engedélyezve

### <a name="configure-hello-availability-group-toouse-hello-new-ip-address"></a>Hello rendelkezésre állási csoport toouse hello új IP-cím konfigurálása

toofinish hello fürt, ismétlődő hello lépéseket követte hello első rendelkezésre állási csoport elküldésekor konfigurálása. Ez azt jelenti, hogy a hello konfigurálása [toouse hello új IP-címe](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

Miután hozzáadta a hello figyelő IP-címet, a hello további rendelkezésre állási csoport konfigurálásához hello következő tevékenységek végrehajtásával: 

1. Győződjön meg arról, hogy mindkét SQL Server virtuális gépen nyissa meg hello mintavételi portot hello új IP-cím. 

2. [A kezelő hozzáadása hello ügyfél-hozzáférési pont](#addcap).

3. [Hello IP-erőforrás hello rendelkezésre állási csoport konfigurálása](#congroup).

   >[!IMPORTANT]
   >Hello IP-cím létrehozásakor használja, hogy hozzáadta a toohello terheléselosztó hello IP-cím.  

4. [Ügyfél-hozzáférési pont hello tegyen hello SQL Server rendelkezésre állási csoport erőforrása](#dependencyGroup).

5. [Ellenőrizze a hello ügyfél-hozzáférési pont erőforrás hello IP-címtől függő](#listname).
 
6. [A PowerShell hello fürt paraméterek beállítása](#setparam).

Miután konfigurálta a hello rendelkezésre állási csoport toouse hello új IP-címet, konfigurálja a hello kapcsolat toohello figyelő. 

## <a name="next-steps"></a>Következő lépések

- [Különböző régiókban Azure virtuális gépeken futó SQL Server Always On rendelkezésre állási csoport konfigurálása](virtual-machines-windows-portal-sql-availability-group-dr.md)
