---
title: "Azure virtuális gépeken Server FCI - aaaSQL |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan toocreate SQL Server feladatátvevő fürt példány Azure virtuális gépeken."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bee3b27805c5f6cc02a43b25d480c129c254cb90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Azure virtuális gépeken futó SQL Server-példány feladatátvevő fürt konfigurálása

Ez a cikk azt ismerteti, hogyan toocreate SQL Server feladatátvevő fürt-példány (FCI) Resource Manager modellt az Azure virtuális gépeken. Ez a megoldás használ [közvetlen tárolóhelyek a Windows Server 2016 Datacenter edition \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) szoftveres virtuális TÁROLÓHÁLÓZATTAL, amely szinkronizálja hello storage (az adatlemezek) (Azure virtuális gépeken) csomópontjai hello közötti, egy Windows-fürt. S2D 's new in Windows Server 2016.

hello alábbi ábrán látható hello teljes megoldás az Azure virtuális gépeken:

![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

hello a fenti ábrán látható:

- Két Azure virtuális gépeken a Windows feladatátvevő fürtben. Ha egy virtuális gép feladatátvevő fürtben is nevezzük egy *fürtcsomópont*, vagy *csomópontok*.
- Minden virtuális gép két vagy több adatlemezek van.
- S2D hello adatlemez hello adatokat szinkronizálja, és megadja a szinkronizált hello adattároló mint egy tárolókészletet.
- hello tárolókészlet feladatátvevő fürt fürt megosztott kötet (CSV) toohello mutatja be.
- SQL Server FCI fürtszerepkör hello hello CSV hello adatmeghajtók használ.
- Azure load balancer toohold hello IP-címet az SQL Server FCI hello.
- Az Azure rendelkezésre állási csoport összes hello erőforrásokat tartalmazza.

   >[!NOTE]
   >Az összes Azure-erőforrások hello ábrán vannak a hello ugyanabban az erőforráscsoportban.

S2D kapcsolatos részletekért lásd: [közvetlen tárolóhelyek a Windows Server 2016 Datacenter edition \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

S2D kétféle típusú architektúrák - átszervezett és többszörösen összevont támogatja. Ez a dokumentum hello architektúrájáról többszörösen összevont. A többszörösen összevont infrastruktúra helyek hello tárolási hello ugyanazokat a kiszolgálókat, hogy fürtözött hello gazdaalkalmazást. Ebben az architektúrában hello tárhelyre van, minden egyes SQL Server FCI csomóponton.

### <a name="example-azure-template"></a>Példa Azure-sablon alapján

Az Azure-ban hello teljes megoldás létrehozhat egy sablont. A sablon példa, hogy elérhető a Githubon hello [Azure gyors üzembe helyezési sablonokat](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad). Ebben a példában nem tervezett, vagy bármely adott munkaterhelés tesztelve. Futtathatja hello sablon toocreate egy SQL Server FCI S2D tárolási csatlakoztatott tooyour tartománnyal. Hello sablon értékelje ki, és módosítsa a célokra.

## <a name="before-you-begin"></a>Előkészületek

Néhány dolgot kell tooknow és a különböző dolgok állíthatók, amelyekre szüksége van érvényben, a folytatás előtt.

### <a name="what-tooknow"></a>Milyen tooknow
A következő technológiák hello működési ismeretét kell rendelkezniük:

- [Windows-fürt technológiák](http://technet.microsoft.com/library/hh831579.aspx)
-  [SQL Server feladatátvevő fürt példányok](http://msdn.microsoft.com/library/ms189134.aspx).

Emellett rendelkeznie kell egy általános ismeretekkel rendelkezik a következő technológiák hello:

- [A Windows Server 2016 közvetlen tárolóhelyek használatával Hyper átszervezett megoldás](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Azure erőforráscsoport-sablonok](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-toohave"></a>Milyen toohave

Hello jelen cikkben lévő utasítások követése, előtt már rendelkeznie kell:

- A Microsoft Azure-előfizetés.
- Azure virtuális gépeken futó Windows-tartományhoz.
- Engedély toocreate objektumokkal hello Azure virtuális gép a fiók.
- Egy Azure-beli virtuális hálózat és alhálózat a következő összetevők hello megfelelő IP-címtér:
   - Mindkét virtuális gép.
   - hello feladatátvételi fürt IP-címet.
   - Minden egyes FCI IP-címet.
- A DNS-konfigurált hello Azure hálózati toohello tartományvezérlők mutat.

Az előfeltételek teljesülnek folytassa a a feladatátvevő fürt. első lépés hello toocreate hello virtuális gépek.

## <a name="step-1-create-virtual-machines"></a>1. lépés: Virtuális gépek létrehozása

1. Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com) az előfizetéshez.

1. [Egy Azure rendelkezésre állási csoport létrehozása](../tutorial-availability-sets.md).

   hello rendelkezésre állási csoportok virtuális gépek be tartalék tartományokban, és a tartományok frissítése. hello rendelkezésre állási csoport gondoskodik arról, hogy az alkalmazás egyetlen ponton felmerülő hibákat, például hello hálózati kapcsoló vagy hello power egysége kiszolgálók állvány nincs hatással.

   Ha még nem hozott hello erőforráscsoportot a virtuális gépek, elvégezhető az Azure rendelkezésre állási csoport létrehozásakor. Hello Azure portál toocreate hello rendelkezésre állási csoport használata, hello a következő lépéseket:

   - Hello Azure-portálon, kattintson  **+**  tooopen hello Azure piactéren. Keresse meg **rendelkezésre állási csoport**.
   - Kattintson a **rendelkezésre állási csoport**.
   - Kattintson a **Create** (Létrehozás) gombra.
   - A hello **rendelkezésre állási csoport létrehozása** panelen, a következő értékek set hello:
      - **Név**: hello rendelkezésre állási csoport nevét.
      - **Előfizetés**: az Azure-előfizetést.
      - **Erőforráscsoport**: toouse egy meglévő csoporthoz, kattintson a **meglévő** és hello legördülő listából válassza hello csoport. Máskülönben válassza **hozzon létre új** , és írja be a hello csoport nevét.
      - **Hely**: állítsa be a hello helyet, ahol toocreate tervezi a virtuális gépek.
      - **Tartományok fault**: hello alapértelmezett (3) használja.
      - **Tartományok frissítése**: hello alapértelmezett (5) használja.
   - Kattintson a **létrehozása** toocreate hello rendelkezésre állási csoportot.

1. Hozzon létre virtuális gépek hello hello rendelkezésre állási csoport.

   Hello Azure rendelkezésre állási készlet két SQL Server virtuális gép kiépítéséhez. Útmutatásért lásd: [egy SQL Server rendszerű virtuális gép az Azure-portálon hello](virtual-machines-windows-portal-sql-server-provision.md).

   Mindkét virtuális gép helye:

   - A hello azonos Azure erőforráscsoport, amely a rendelkezésre állási csoportban van.
   - Hello azonos hálózati a tartományvezérlővel.
   - Az alhálózat IP-címterület elegendő a virtuális gépek és az összes példányoktól végül használhat ezen a fürtön.
   - A hello Azure rendelkezésre állási csoportot.   

      >[!IMPORTANT]
      >Nem állíthatók be vagy rendelkezésre állási csoport egy virtuális gép létrehozása után módosítsa.

   Kép kiválasztása a hello Azure piactéren. Használhatja a Piactéri lemezkép, amely tartalmazza a Windows Server és SQL Server, vagy csak hello Windows Server. További információkért lásd: [áttekintése az SQL Server Azure virtuális gépeken](../../virtual-machines-windows-sql-server-iaas-overview.md)

   hello hivatalos SQL Server-rendszerképeit hello Azure-katalógus tartalmazhatnak a telepített SQL Server-példányt, és hello SQL Server telepítési szoftver és hello szükséges kulcs.

   Kattintson a hello jobb kép kívánt toohow az SQL Server licence hello toopay szerint:

   - **Használati licencelési kell fizetnie**: hello perc költségét, ezeket a lemezképeket tartalmaz hello SQL Server licencelési:
      - **A Windows Server Datacenter 2016 SQL Server 2016 Enterprise**
      - **Az SQL Server 2016 Standard a Windows Server Datacenter 2016**
      - **SQL Server 2016 fejlesztői a Windows Server Datacenter 2016**

   - **Bring your-saját-licencet (BYOL)**

      - **{BYOL} A Windows Server Datacenter 2016 SQL Server 2016 Enterprise**
      - **{BYOL} Az SQL Server 2016 Standard a Windows Server Datacenter 2016**

   >[!IMPORTANT]
   >Hello virtuális gép létrehozása után távolítsa el a hello előre telepített önálló SQL Server-példányt. Miután konfigurálta a hello feladatátvevő fürt és S2D hello előre telepített SQL Server media toocreate hello SQL Server FCI fogja használni.

   Másik lehetőségként használata Azure piactéren elérhető rendszerkép csak hello operációs rendszer. Válasszon egy **Windows Server 2016 Datacenter** rendszerképet, SQL Server FCI hello telepítése hello feladatátvevő fürt és S2D konfigurálása után. A kép nem tartalmaz az SQL Server telepítési adathordozóról. Hello telepítési adathordozón egy SQL Server-telepítés hello kiszolgálónként futtató helyen helyezze el.

1. Miután a virtuális gépeket az Azure létrehoz, csatlakoztassa tooeach virtuális gép RDP.

   Amikor első alkalommal csatlakoztatja tooa virtuális gép RDP-, hello számítógép megkérdezi tooallow a PC-toobe felderíthető hello hálózaton. Kattintson a **Yes** (Igen) gombra.

1. Ha egy hello SQL Server-alapú virtuális gépek lemezképet használ, távolítsa el a hello SQL Server-példány.

   - A **programok és szolgáltatások**, kattintson a jobb gombbal **Microsoft SQL Server 2016 (64 bites)** kattintson **Eltávolítás/módosítás**.
   - Kattintson a **eltávolítása**.
   - Válassza ki a hello alapértelmezett példány.
   - Távolítsa el az összes szolgáltatás **adatbázismotor-szolgáltatások**. Ne távolítsa el az **közös szolgáltatások**. Tekintse meg az alábbi képen hello:

      ![Szolgáltatások eltávolítása](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Kattintson a **következő**, és kattintson a **eltávolítása**.

1. <a name="ports"></a>Nyissa meg a hello tűzfal portjai.

   Minden egyes virtuális gépen nyissa meg a következő portokat a Windows tűzfal hello hello.

   | Cél | TCP-Port | Megjegyzések
   | ------ | ------ | ------
   | SQL Server | 1433 | Normál port az SQL Server alapértelmezett példánya esetében. Ha lemezkép hello gyűjteményből, ezt a portot automatikusan megnyílik.
   | Állapotmintáihoz | 59999 | Bármely nyitott TCP-portot. Egy későbbi lépésben konfigurálja a terheléselosztó hello [állapotmintáihoz](#probe) és hello fürt toouse ezt a portot.  

1. Adja hozzá a tárolási toohello virtuális gépet. Részletes információkért lásd: [tároló](../../../storage/common/storage-premium-storage.md).

   Mindkét virtuális gépek legalább két lemez szükséges.

   Csatlakoztassa a lemezeket nyers - nem NTFS formátumú lemezek.
      >[!NOTE]
      >Ha NTFS fájlrendszerű lemezek csatlakoztat, nem szabad jogosultsági ellenőrzéssel csak engedélyezheti a S2D.  

   Legalább két prémium szintű Storage (SSD lemezek) tooeach VM csatolni. Ajánlott legalább P30 (1 TB) lemezek.

   Set-állomás gyorsítótárazását túl**írásvédett**.

   éles környezetben használhat hello tárolókapacitás attól függ, hogy a számítási feladatok. hello cikkben leírt értékei demonstrációs és tesztelésére.

1. [Hello virtuális gépek tooyour már meglévő tartomány hozzáadása](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Után hello virtuális gépek létrehozása és konfigurálva, konfigurálhat hello feladatátvevő fürtöt.

## <a name="step-2-configure-hello-windows-failover-cluster-with-s2d"></a>2. lépés: Feladatátvevő fürt Windows hello konfigurálása S2D

hello tovább S2D tooconfigure hello feladatátvevő fürtön. Ebben a lépésben ennek következő részlépések hello:

1. Adja hozzá a Windows feladatátvételi fürtszolgáltatás
1. Hello fürt ellenőrzése
1. Hello feladatátvevő fürt létrehozása
1. Hello felhő tanúsító létrehozása
1. Tároló hozzáadása

### <a name="add-windows-failover-clustering-feature"></a>Adja hozzá a Windows feladatátvételi fürtszolgáltatás

1. toobegin, csatlakozás toohello első virtuális gép RDP-a helyi rendszergazdák tagja, amely rendelkezik engedélyekkel toocreate objektumok az Active Directory tartományi fiók használatával. Ezt a fiókot használják a hello részeinek hello konfigurációs.

1. [Adja hozzá a Feladatátvételi fürtszolgáltatás funkció tooeach virtuális gép](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   tooinstall Feladatátvételi fürtszolgáltatást a felhasználói felület, hello hello mindkét virtuális gépekre lépésekkel.
   - A **Kiszolgálókezelő**, kattintson a **kezelése**, és kattintson a **szerepkörök és szolgáltatások hozzáadása**.
   - A **hozzáadása szerepkörök és szolgáltatások varázsló**, kattintson a **következő** amíg elér túl**szolgáltatások kiválasztása**.
   - A **szolgáltatások kiválasztása**, kattintson a **feladatátvételi fürtszolgáltatás**. Az összes szükséges szolgáltatásokat és hello felügyeleti eszközök közé tartozik. Kattintson a **szolgáltatások hozzáadása**.
   - Kattintson a **következő** majd **Befejezés** tooinstall hello szolgáltatásokat.

   tooinstall hello Feladatátvételi fürtszolgáltatást a PowerShell-lel, futtassa a következő parancsfájl egy rendszergazda PowerShell-munkamenetben hello virtuális gépek egyik hello.

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Referenciaként hello lépések követésével hello a 3. lépés [közvetlen tárolóhelyek használata a Windows Server 2016 Hyper átszervezett megoldás](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-hello-cluster"></a>Hello fürt ellenőrzése

Ez az útmutató alapján tooinstructions hivatkozik [fürt ellenőrzése](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Hello felhasználói felületén vagy a PowerShell-lel hello fürt ellenőrzése.

hello felhasználói felületén toovalidate hello fürt hello lépések hello virtuális gépek egyikét.

1. A **Kiszolgálókezelő**, kattintson a **eszközök**, majd kattintson a **Feladatátvevőfürt-kezelő**.
1. A **Feladatátvevőfürt-kezelő**, kattintson a **művelet**, majd kattintson a **konfiguráció ellenőrzése...** .
1. Kattintson a **Tovább** gombra.
1. A **kiszolgálók kiválasztása vagy a fürt**, mindkét virtuális gép hello nevét.
1. A **tesztelési beállítások**, válassza a **csak a kijelölt tesztek futtatása**. Kattintson a **Tovább** gombra.
1. A **kijelölés tesztelése**, következők kivételével az összes teszt **tárolási**. Tekintse meg az alábbi képen hello:

   ![Tesztek ellenőrzése](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. Kattintson a **Tovább** gombra.
1. A **megerősítő**, kattintson a **következő**.

Hello **konfiguráció ellenőrzése varázsló** futtatása hello ellenőrző tesztek.

a PowerShell-lel, futtassa a következő parancsfájl egy rendszergazda PowerShell-munkamenetben hello virtuális gépek egyik hello toovalidate a hello fürt.

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Miután hello fürt ellenőrzéséhez hello feladatátvevő fürt létrehozása.

### <a name="create-hello-failover-cluster"></a>Hello feladatátvevő fürt létrehozása

Ez az útmutató hivatkozik túl[hello feladatátvevő fürt létrehozása](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

toocreate hello feladatátvevő fürt lesz szüksége:
- hello virtuális gépek fürtcsomópontok hello váló hello nevei.
- Hello feladatátvevő fürt nevét
- Hello feladatátvevő fürt IP-címet. Használhatja a hello nem használt IP-címet azonos Azure virtuális hálózat és alhálózat, a fürtcsomópontok hello.

a következő PowerShell hello egy feladatátvevő fürtöt hoz létre. Hello parancsfájl hello nevekkel hello csomópontok (hello a virtuális gép neve) és egy szabad IP-cím hello Azure virtuális hálózat frissítése:

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>A felhő tanú létrehozása

Felhő tanúsító is egy új fürt kvórum tanúsítójának tárolódnak az Azure Storage-Blobba. Ezzel eltávolítja a különálló virtuális gépek üzemeltetéséhez a tanúsító fájlmegosztás hello kell.

1. [Hozzon létre egy felhő hello feladatátvevő fürt tanúsító](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. A blob-tároló létrehozása.

1. Hello hívóbetűk és hello tároló URL-cím mentése.

1. Hello feladatátvevő fürt fürt kvórum tanúsító konfigurálása. Lásd például a [konfigurálása hello kvórumtanúsító hello felhasználói felületen]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) a felhasználói felület hello.

### <a name="add-storage"></a>Tároló hozzáadása

S2D hello lemezeket kell toobe, üres, partíciók vagy egyéb adatok nélkül. hajtsa végre a tooclean lemezek [hello lépések útmutatóban](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Engedélyezési tároló közvetlen tárolóhelyek \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   a következő PowerShell hello lehetővé teszi, hogy a tárolóhelyek – közvetlen.  

   ```PowerShell
   Enable-ClusterS2D
   ```

   A **Feladatátvevőfürt-kezelő**, most már megtekintheti hello a tárolókészletben.

1. [Hozzon létre egy kötetet](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   Hello funkcióit S2D egyike, hogy automatikusan létrehoz egy tárolókészletet engedélyezése. Most már áll készen toocreate egy köteten. PowerShell-parancsmag segítségével hello `New-Volume` automatizálja a hello kötet létrehozási folyamata, beleértve a formázás, toohello fürt hozzáadása, és a fürt megosztott kötetei (CSV) létrehozása. a következő példa hello egy 800 gigabájt (GB) CSV hoz létre.

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Ez a parancs után egy 800 GB-os kötet fürterőforrásként csatlakoztatva. hello kötet jelenleg `C:\ClusterStorage\Volume1\`.

   a következő diagram hello jeleníti meg a fürt megosztott kötetei S2D rendelkező:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>3. lépés: Feladatátvevő fürtön belüli feladatátvétel tesztelése

A Feladatátvevőfürt-kezelőben, győződjön meg arról, hogy átvihető hello tárolási erőforrás toohello másik fürtcsomópontra. Ha a kapcsolódás toohello feladatátvevő fürt **Feladatátvevőfürt-kezelő** és hello tárolási áthelyezését más egy csomópont toohello, készen áll a tooconfigure hello FCI áll.

## <a name="step-4-create-sql-server-fci"></a>4. lépés: Az SQL Server FCI létrehozása

Miután konfigurálta a hello feladatátvevő fürt és a fürt-összetevők, beleértve a tárolási, SQL Server FCI hello hozhat létre.

1. Csatlakozás RDP toohello első virtuális gépen.

1. A **Feladatátvevőfürt-kezelő**, győződjön meg arról, hogy minden fürt alapvető erőforrásai hello első virtuális gépen. Ha szükséges, helyezze az összes erőforrások toothis virtuális gépet.

1. Hello telepítési adathordozóján található. Virtuális gép hello hello Azure piactéren elérhető rendszerkép valamelyikét használja, ha hello media itt található: `C:\SQLServer_<version number>_Full`. Kattintson a **telepítő**.

1. A hello **SQL Server telepítési központjának**, kattintson a **telepítési**.

1. Kattintson a **új SQL Server feladatátvevő fürt telepítése**. Hello varázsló tooinstall hello SQL Server FCI hello utasításait kövesse.

   hello FCI adatkönyvtárak kell toobe fürtözött tárhelyen. A S2D már nem egy megosztott lemez, de a csatlakoztatási pont tooa kötet minden kiszolgálón. S2D szinkronizálja hello kötet mindkét csomópont között. hello kötet toohello fürt jeleníti meg egy megosztott fürtkötethez. Hello adatkönyvtárak hello CSV csatlakoztatási pont használatára.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Hello varázsló befejezése után telepíti egy SQL Server FCI hello első csomóponton.

1. A telepítő sikeres telepítése után hello FCI hello első csomóponton csatlakozás toohello második csomópont RDP.

1. Nyissa meg hello **SQL Server telepítési központjának**. Kattintson a **telepítési**.

1. Kattintson a **Hozzáadás csomópont tooa SQL Server feladatátvevő fürt**. Hello varázsló tooinstall SQL server hello utasításokat követve, majd adja meg a kiszolgáló toohello FCI.

   >[!NOTE]
   >Ha SQL Server Azure piactér gyűjtemény kép használt, SQL Server-eszközök szerepeltek hello lemezképpel. Ha nem használja ezt a képet, SQL Server-eszközök hello külön kell telepítenie. Lásd: [töltse le az SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>5. lépés: Az Azure terheléselosztó létrehozása

Az Azure virtuális gépeken fürtök egy terheléselosztási terheléselosztó toohold egyszerre egy csomóponton toobe igénylő IP-címet használja. Ebben a megoldásban hello terheléselosztó hello SQL Server FCI hello IP-címet tartalmazza.

[Hozza létre és konfigurálja az Azure terheléselosztó](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-hello-load-balancer-in-hello-azure-portal"></a>Hozzon létre hello terheléselosztó hello Azure-portálon

toocreate hello terheléselosztó:

1. A hello Azure-portálon válassza a toohello erőforráscsoport hello virtuális gépekkel.

1. Kattintson a **+ Hozzáadás**. Keresési hello piactér a **terheléselosztó**. Kattintson a **terheléselosztó**.

1. Kattintson a **Create** (Létrehozás) gombra.

1. Rendelkező terheléselosztó hello konfigurálása:

   - **Név**: hello terheléselosztó azonosító nevét.
   - **Típus**: hello terheléselosztó nyilvános vagy titkos lehet. Személyes terheléselosztó belül elérhető hello ugyanazt a virtuális Hálózatot. Az Azure alkalmazások használhatják a saját terheléselosztó. Ha az alkalmazásnak hozzáféréssel tooSQL Server hello interneten keresztül közvetlenül, használjon egy nyilvános terheléselosztó.
   - **Virtuális hálózati**: hello azonos hálózati hello virtuális gépként.
   - **Alhálózati**: hello hello virtuális gépek azonos alhálózaton.
   - **Magánhálózati IP-cím**: hello toohello SQL Server FCI fürt hálózati erőforráshoz rendelt IP-cím.
   - **előfizetés**: az Azure-előfizetést.
   - **Erőforráscsoport**: használata hello azonos erőforráscsoporthoz tartozik, mint a virtuális gépeket.
   - **Hely**: használata hello ugyanaz a virtuális gépek Azure-beli helyhez.
   Tekintse meg az alábbi képen hello:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-hello-load-balancer-backend-pool"></a>A terheléselosztó háttérkészletének hello konfigurálása

1. Térjen vissza az Azure erőforráscsoport toohello hello virtuális gépekkel, és keresse meg az új terheléselosztó hello. Előfordulhat, hogy toorefresh hello nézet a hello erőforráscsoportot. Kattintson a hello terheléselosztóhoz.

1. Hello load balancer paneljén kattintson **háttérkészletek**.

1. Kattintson a **+ Hozzáadás** tooadd háttérkészlet.

1. Írja be a hello háttérkészlet nevét.

1. Kattintson a **adja hozzá a virtuális gépek**.

1. A hello **válassza ki a virtuális gépek** panelen kattintson a **rendelkezésre állási csoport kiválasztása**.

1. Válassza ki a hello rendelkezésre állási csoportban, hogy a hello SQL Server virtuális gépeket helyez.

1. A hello **válassza ki a virtuális gépek** panelen kattintson a **válassza ki a virtuális gépek hello**.

   Az Azure-portálon kép a következő hello hasonlóan kell kinéznie:

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. Kattintson a **válasszon** a hello **válassza ki a virtuális gépek** panelen.

1. Kattintson a **OK** kétszer.

### <a name="configure-a-load-balancer-health-probe"></a>Terheléselosztói állapotfigyelő mintavétel konfigurálása

1. Hello load balancer paneljén kattintson **állapot-mintavételi csomagjai**.

1. Kattintson a **+ Hozzáadás**.

1. A hello **Hozzáadás állapotmintáihoz** panelen <a name="probe"> </a>hello állapotfigyelő mintavételi paraméterek beállítása:

   - **Név**: hello állapotmintáihoz nevét.
   - **Protokoll**: TCP.
   - **Port**: tooan elérhető TCP-port beállítása. Ez a port egy megnyitott tűzfal portra van szüksége. Használjon hello [ugyanazt a portot](#ports) hello tűzfalon hello állapotmintáihoz beállítása.
   - **Időköz**: 5 másodperc.
   - **Sérült küszöbérték**: 2 egymást követő hibák.

1. Kattintson az OK gombra.

### <a name="set-load-balancing-rules"></a>Terheléselosztási szabályok beállítása

1. Hello load balancer paneljén kattintson **terheléselosztási szabályok**.

1. Kattintson a **+ Hozzáadás**.

1. Hello terhelés terheléselosztási szabályok paraméterek beállítása:

   - **Név**: hello terheléselosztási szabályok nevét.
   - **Előtérbeli IP-cím**: SQL Server FCI fürt hálózati erőforrás hello hello IP-címet használni.
   - **Port**: hello SQL Server FCI TCP-port beállítása. hello alapértelmezett példány portja az 1433-as.
   - **A háttérportot**: Ez az érték hello ugyanazt a portot használja, mint a hello **Port** értéke, ha engedélyezi a **fix IP-Címek (közvetlen kiszolgálói válasz)**.
   - **Háttérkészlet**: használata hello háttér alkalmazáskészlet neve korábban megadott értékektől.
   - **Állapotmintáihoz**: használata hello állapotmintáihoz korábban megadott értékektől.
   - **Munkamenet megőrzését**: nincs.
   - **Üresjárat időkorlátja (perc)**: 4.
   - **Lebegőpontos IP (közvetlen kiszolgálói válasz)**: engedélyezve

1. Kattintson az **OK** gombra.

## <a name="step-6-configure-cluster-for-probe"></a>6. lépés: A fürt mintavétel konfigurálása

PowerShell hello fürt mintavételi portot paraméterrel állítható be.

tooset hello fürt mintavételi portot paraméter, a változók a parancsfájl a környezetben a következő hello frissíti.

  ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "IP Address Resource Name" # hello IP Address cluster resource name.
   $ILBIP = "<10.0.0.x>" # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <59999>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```


## <a name="step-7-test-fci-failover"></a>: 7. lépés az FCI-feladatátvétel

Teszt feladatátvétele hello FCI toovalidate fürt működését. Hello a következő lépéseket:

1. Csatlakozás RDP tooone hello SQL Server FCI-csomópont.

1. Nyissa meg **Feladatátvevőfürt-kezelő**. Kattintson a **szerepkörök**. Figyelje meg, melyik csomóponton hello SQL Server FCI szerepkör.

1. Kattintson a jobb gombbal hello SQL Server FCI szerepkör.

1. Kattintson a **áthelyezése** kattintson **lehető legalkalmasabb csomópontra**.

**Feladatátvevőfürt-kezelő** mutat be hello szerepkört és erőforrást kapcsolat nélküli módba. hello erőforrások áthelyezése, majd online állapotba a hello a másik csomóponton.

### <a name="test-connectivity"></a>Kapcsolat tesztelése

tootest kapcsolatot, hello tooanother virtuális gépre való bejelentkezéshez ugyanazt a virtuális hálózatot. Nyissa meg **SQL Server Management Studio** , és csatlakozzon a toohello SQL Server FCI nevét.

>[!NOTE]
>Ha szükséges, akkor [töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Korlátozások
Az Azure virtuális gépeken a Microsoft elosztott tranzakciók koordinátora (DTC) nem támogatott a példányoktól mert hello terheléselosztó által nem támogatott a hello RPC-portot.

## <a name="see-also"></a>Lásd még:

[A telepítő S2D a távoli asztal (Azure)](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Hyper-összevont megoldás a tárolóhelyek közvetlen](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Közvetlen tárolóhelyek áttekintése](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[S2D SQL Server támogatása](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
