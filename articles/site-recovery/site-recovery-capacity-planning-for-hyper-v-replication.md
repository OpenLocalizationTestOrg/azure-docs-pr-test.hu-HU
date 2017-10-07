---
title: "a Site Recovery aaaRun hello Hyper-V kapacitás planner eszköz |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toorun hello az Azure Site Recovery Hyper-V kapacitás planner eszköz"
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: b853598e5cd290c48b59794ba48eefc72ac8ded6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-hyper-v-capacity-planner-tool-for-site-recovery"></a>A Site Recovery hello Hyper-V kapacitás planner eszköz futtatása

Az Azure Site Recovery üzembe helyezése részeként kell toofigure a replikáció és a sávszélesség-követelményekkel. Hyper-V hello kapacitás planner a Site Recovery eszközével, toodo, a Hyper-V Virtuálisgép-replikációt.

Ez a cikk ismerteti, hogyan toorun hello Hyper-V kapacitás planner eszköz. Ez az eszköz a hello adatokkal együtt használandó [a Site Recovery kapacitástervezési](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Előkészületek
Az elsődleges hely Hyper-V kiszolgáló vagy fürt csomópontja hello eszközt futtatja. toorun hello eszköz hello Hyper-V-kiszolgálóknak kell:

* Operációs rendszer: Windows Server 2012 vagy 2012 R2
* Memória: 20 MB (minimum)
* CPU: 5 százalékkal terhelés (minimum)
* Szabad lemezterület: 5 MB (minimum)

Hello eszköz futtatása előtt kell tooprepare hello elsődleges hely. Ha közötti replikál két helyszíni hely és meg toocheck sávszélesség, a replikakiszolgáló tooprepare van szüksége.

## <a name="step-1-prepare-hello-primary-site"></a>1. lépés: Felkészülés hello elsődleges hely

1. Hello elsődleges hely győződjön meg az összes Hyper-V virtuális gépek kívánt hello tooreplicate és állomás/fürt Hyper-V hello amelyen fontosságúak található. hello eszköz futtatható több önálló gazdagépen vagy egy fürtön, de nem mindkettőt együtt. Is kell toorun külön-külön az egyes operációs rendszerek, a Hyper-V kiszolgálók információt az alábbiak szerint kell gyűjteni:

   * Windows Server 2012 önálló kiszolgálók
   * Windows Server 2012-fürtök
   * Windows Server 2012 R2 önálló kiszolgálók
   * Windows Server 2012 R2-fürt
2. Engedélyezze a távoli elérés tooWMI összes hello Hyper-V gazdagépeken és fürtökben. A parancs futtatásához minden kiszolgálófürtön toomake meg arról, hogy a tűzfalszabályok és felhasználói engedélyek vannak beállítva:

        netsh firewall set service RemoteAdmin enable
3. Engedélyezze a teljesítmény figyelése kiszolgálói és fürtjei a következőképpen:

   * Nyissa meg a Windows tűzfal a hello hello **biztonságú** beépülő modul, és ezután engedélyezése hello következő bejövő szabályok: **COM + hálózati hozzáférés (DCOM-IN)** és az összes szabályai hello **Távoli Eseménynapló Felügyeleti csoport**.

## <a name="step-2-prepare-a-replica-server-on-premises-tooon-premises-replication"></a>2. lépés: Készítse elő a kiszolgálót (a helyszíni tooon replikáció)
Ez nem szükséges toodo Ha tooAzure replikál.

Javasoljuk a helyreállítási kiszolgálónak, egy Hyper-V állomás beállítása, hogy egy üres virtuális gép replikált tooit toocheck sávszélesség is lehet.  Kihagyhatja ezt, de nem fog tudni toomeasure sávszélesség kivéve, ha lehetővé teszi.

1. Ha azt szeretné, hogy toouse egy fürt csomópontja hello replikaként a Hyper-V replikaszervező konfigurálása:

   * A **Kiszolgálókezelő**, nyissa meg **Feladatátvevőfürt-kezelő**.
   * Toohello fürthöz csatlakozzon, jelölje ki a hello fürtnév, és kattintson **műveletek** > **szerepkör konfigurálása** tooopen hello magas rendelkezésre állás varázsló.
   * A **Szerepkörválasztás**, kattintson a **Hyper-V Replikaszervező**. Hello varázslóban adja meg a **NetBIOS-név** és hello **IP-cím** toobe hello kapcsolódási pont toohello fürt (ügyfél-hozzáférési pont) használja. Hello **Hyper-V Replikaszervező** lesz konfigurálva, ami azt eredményezi, hogy egy ügyfél-hozzáférési pont nevét, vegye figyelembe.
   * Győződjön meg arról, hogy hello Hyper-V Replikaszervező szerepkör sikeresen online módba kerül, és hello fürt minden csomópontjáról képes átvenni. toodo, hello szerepkör kattintson a jobb gombbal, majd túl**áthelyezése**, és kattintson a **csomópont kiválasztása**. Válasszon ki egy csomópontot > **OK**.
   * Ha Tanúsítványalapú hitelesítés használata esetén győződjön meg arról, hogy minden fürtcsomóponton, és hello ügyfél-hozzáférési pont összes telepítve hello tanúsítvány.
2. Engedélyezze a replikakiszolgálón:

   * Fürt nyissa meg a hiba-kezelőt, csatlakoztassa toohello fürtöt, és kattintson a **szerepkörök** > Válassza szerepkör > **replikációs beállítások** > **replikaként fürt engedélyezése kiszolgáló**. Ha hello replikaként-fürtöt használ, akkor toohave hello Hyper-V Replikaszervező szerepkör hello fürt hello elsődleges helyen is megtalálható.
   * Egy önálló kiszolgáló nyissa meg a Hyper-V kezelőjét. A hello **műveletek** ablaktáblában kattintson **Hyper-V beállítások** hello kiszolgáló kívánt tooenable, és a **replikációs konfiguráció** kattintson **ennek engedélyezéséhez a számítógépre, mint a replikakiszolgáló**.
3. Hitelesítés beállítása:

   * A **hitelesítés és portok**, válassza ki, hogyan tooauthenticate hello elsődleges kiszolgáló, és hello hitelesítési portok. Ha a tanúsítvány kattintson **tanúsítvány kiválasztása** tooselect egyet. Kerberos használja, ha hello elsődleges és a helyreállítási Hyper-V-gazdagépek hello ugyanabba a tartományba vagy megbízható tartományban. Tanúsítványokat használ a különböző tartományokhoz, vagy egy munkacsoport-telepítéshez.
   * A **engedélyezés és tárolás**, engedélyezése **bármely** hitelesített (elsődleges) kiszolgáló toosend replikációs adatok toothis replikakiszolgáló.

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Futtatás **netsh http show servicestate**, toocheck adott hello figyelő fut. hello protokoll/port megadott:  
4. Állítsa be a tűzfalon. Hyper-V telepítése során a tűzfalszabályok jönnek létre a tooallow forgalom hello alapértelmezett porton (HTTPS a 443-as, a 80-as Kerberos). Engedélyezze a ezeket a szabályokat az alábbiak szerint:
  - Tanúsítványhitelesítés fürtön (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Kerberos-hitelesítés (80-as) fürtön:``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Tanúsítványhitelesítés önálló kiszolgálón:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Kerberos-hitelesítés önálló kiszolgálón:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-hello-capacity-planner-tool"></a>3. lépés: Hello kapacitás planner eszköz futtatása
Miután elő az elsődleges helyet, és állítsa be a helyreállítási kiszolgálónak, hello eszköz is futtathatja.

1. [Töltse le](https://www.microsoft.com/download/details.aspx?id=39057) hello Microsoft Download Center hello eszközt.
2. Az egyik hello elsődleges kiszolgáló (vagy egy hello csomópontok hello elsődleges fürtből) hello eszköz futtatásához. Kattintson a jobb gombbal a hello .exe fájl, és válassza a **Futtatás rendszergazdaként**.
3. A **megkezdése előtt**, adja meg, mennyi ideig toocollect adatokat. Azt javasoljuk, hogy az adatok képviselője éles óra tooensure során hello eszközt futtatja. Csak próbált toovalidate hálózati kapcsolatot, ha csak egy percet hozhatja létre.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. A **elsődleges hely adatai**, hello kiszolgáló nevét vagy teljes Tartománynevét adja meg egy önálló gazdagép vagy fürt adhatja hello hello ügyfél teljes Tartományneve fogadja el a pont, a fürt nevét vagy a hello fürt bármely csomópontjára, és kattintson **tovább**. hello eszköz automatikusan észleli a futó hello kiszolgáló hello nevét. virtuális gépek, amelyek figyelhetők meg hello eszköz felveszi a megadott kiszolgálók hello.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. A **replika helyadatok**, ha tooAzure replikál, vagy ha másodlagos adatközpontba tooa replikál, és még nem állított be a replikakiszolgálón **hagyja ki a teszteket használata esetén a replika helyszínére**. Ha tooa másodlagos adatközpontba replikálja, és beállította a replika típusa, adja meg hello önálló kiszolgáló vagy hello ügyfél-hozzáférési pont teljes Tartományneve hello fürt **kiszolgáló nevét (vagy) Hyper-V replika Broker CAP**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. A **kiterjesztett replika részletek**, engedélyezése **Skip hello tesztek érintik a kiterjesztett replika hely**. Ezeket a teszteket a Site Recovery által nem támogatott.
7. A **válassza ki a virtuális gépek tooReplicate**, hello eszközök toohello kiszolgáló vagy fürt kapcsolódik, és megjeleníti a virtuális gépek és a lemezek hello elsődleges kiszolgálón futó, hello beállításai összhangban megadott hello **elsődleges hely adatai**  lap. Virtuális gépek, amelyek a replikáció már engedélyezve, vagy az, hogy nem fut, nem jelennek meg. Válassza ki a kívánt toocollect metrikák hello virtuális gépeket. Automatikus kiválasztása hello VHD-k számára gyűjti az adatokat virtuális gépek hello túl.
8. Ha már konfigurált egy adatbázisreplika-kiszolgáló vagy fürt, **hálózati adatok**, adja meg a használandó hello hozzávetőleges WAN sávszélességét, amennyire véleménye szerint, ha konfigurálta az hello elsődleges és replika helyek között, és jelölje be hello tanúsítványok tanúsítványhitelesítés.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. A **összegzés**, ellenőrizze a hello beállításait, és kattintson a **következő** toobegin metrikák gyűjtése. Eszköz folyamat- és állapot jelenik meg hello **kiszámításához kapacitás** lap. Hello eszköz futtatásának befejezése után kattintson **jelentés megtekintése** tooview hello kimeneti. Alapértelmezés szerint jelentések és a naplók vannak tárolva **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-hello-results"></a>4. lépés: Hello eredmények értelmezéséhez.

Az alábbiakban a fontos metrikák hello. Itt listán nem szereplő metrikák figyelmen kívül hagyhatja. Nem fontosságúak a Site Recovery szükséges.

### <a name="on-premises-tooon-premises-replication"></a>A helyszíni tooon replikáció

* A replikáció hello elsődleges állomás számítási, memória hatása
* A replikáció hello elsődleges, helyreállítási állomások tároló szabad lemezterület, IOPS hatása
* Teljes sávszélességre van szüksége változásreplikálás (MB/s)
* Megfigyelt hálózati sávszélesség közötti hello elsődleges állomás és hello helyreállítási gazdagépen (MB/s)
* Hello közötti átvitel hello ideális száma párhuzamos aktív javaslatot két gazdagép vagy fürt

### <a name="on-premises-tooazure-replication"></a>A helyszíni tooAzure replikáció

* A replikáció hello elsődleges állomás számítási, memória hatása
* A replikáció hello elsődleges állomás tárolási helyet a lemezen, IOPS hatása
* Teljes sávszélességre van szüksége változásreplikálás (MB/s)

## <a name="more-resources"></a>További erőforrások
* Hello eszközzel kapcsolatos részletes információkért olvassa el a hello dokumentum hello eszköz letöltése olvashatja el.
* Tekintse meg a Keith Mayer hello eszköz bemutatóért [TechNet blog](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Hello eredményt](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) a teljesítményt, a helyszíni tooon helyszíni Hyper-V replikáció tesztelése

## <a name="next-steps"></a>Következő lépések

A kapacitástervezés befejezése után megkezdheti a Site Recovery telepítését:

* [A VMM-felhők tooAzure Hyper-V virtuális gépek replikálása](site-recovery-vmm-to-azure.md)
* [Hyper-V virtuális gépek (VMM nélkül) tooAzure replikálása](site-recovery-hyper-v-site-to-azure.md)
* [A VMM-helyek közti replikálásához a Hyper-V virtuális gépek](site-recovery-vmm-to-vmm.md)
