---
title: "teljesítmény és méretezés, a VMware-replikáció tooAzure az Azure Site Recovery aaaPlan |} Microsoft Docs"
description: "Ez a cikk tooplan kapacitás és a skála használja, az Azure Site Recovery VMware virtuális gépek tooAzure replikálása esetén"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: rayne
ms.openlocfilehash: 551533ab7090d85c216be242ea92781deb8287ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-vmware-tooazure-replication"></a>3. lépés: A kapacitás és a VMware tooAzure replikációs csoportok skálázási módjának tervezése

Ez a cikk toofigure jelent a kapacitástervezés és a méretezés, a helyszíni VMware virtuális gépek és fizikai kiszolgálók tooAzure való replikálása esetén használjon [Azure Site Recovery](site-recovery-overview.md).

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="how-do-i-start-capacity-planning"></a>Hogyan kezdhetem meg, hogy a kapacitástervezés?

A replikálási környezetre vonatkozó információkat gyűjt, és majd a kapacitástervezés ezeket az adatokat, a cikkben a kijelölt hello szempontok használatával.


## <a name="gather-information"></a>Adatainak összegyűjtése

1. Töltse le a hello [telepítési Planner eszköz](https://aka.ms/asr-deployment-planner) a VMware-replikáció.
2. [Ebben a cikkben](site-recovery-deployment-planner.md) toounderstand hogyan toorun hello eszköz.
3. Hello eszközzel kompatibilis és nem kompatibilis virtuális gépekhez, virtuális gépenként lemezek információt gyűjteni, és lemezenként kavarog adatokat. hello eszköz is magában foglalja az hálózat sávszélességigényét és hello Azure-infrastruktúra szükséges sikeres replikáció és a teszt feladatátvételhez.

## <a name="replication-considerations"></a>Adatreplikálási megfontolások

Mielőtt elkezdené a telepítést, jegyezze fel ezeket a szempontokat:

**Összetevő** | **Részletek** |
--- | --- | ---
**Replikáció** | **Legfeljebb napi adatváltozási sebesség:** A védett gép csak egy folyamat-kiszolgálót is használhat, és a folyamat egyetlen kiszolgáló kezelni tud a napi adatváltozási sebesség too2 TB fel. Így 2 TB nem hello maximális napi módosulása arány, amely a védett gépek esetén támogatott.<br/><br/> **Maximális átviteli sebesség:** A replikált gép tooone Azure storage-fiókot is tartozhatnak. Standard szintű tárfiók kezelni tud a legfeljebb 20 000 kérelmek / másodperc, és azt javasoljuk, hogy a forrás gép too20 000 keresztül tartsa hello bemeneti/kimeneti műveletek (IOPS) másodpercenkénti száma. Például ha a forrásgép 5 lemezzel rendelkezik, és egyes lemezek hello forrásgépen 120 IOPS (8 KB-os méret) hoz létre, majd lesz hello Azure lemez IOPS-korlát 500 belül. (a storage-fiókok szükséges hello száma egyenlő toohello teljes forrásgép IOPS, 20 000 osztva.)

## <a name="configuration-server-capacity"></a>Konfigurációs kiszolgáló kapacitásának

hello konfigurációs kiszolgáló összes védett gépeken futó munkaterhelések képes toohandle hello napi módosítási gyakorisága kapacitást kell lenniük, és elegendő sávszélesség toocontinuously replikálni kell adatok tooAzure tároló.

Ajánlott eljárásként, megtalálhatja a konfigurációs kiszolgáló hello hello ugyanazon a hálózaton és a LAN-szegmens, hello tooprotect kívánt gépek. Egy másik hálózati, de azt szeretné, hogy a tooprotect rendelkeznie kell a réteg 3 hálózati látható tooit gépek is található.

## <a name="sizing-recommendations"></a>Méretezési javaslatok

hello táblázat CPU alapuló méretezési javaslatokat foglalja össze.

**PROCESSZOR** | **Memória** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek**
--- | --- | --- | --- | ---
8 Vcpu (2 sockets * @ 2,5 gigahertz [GHz] 4 mag) | 16 GB | 300 GB | 500 GB vagy kevesebb | 100-nál kevesebb gépek replikálása.
12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) | 18 GB | 600 GB | 500 GB too1 TB | 100-150 gépek közti replikálásához.
16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag) | 32 GB | 1 TB | 1 TB-os too2 TB | 150-200 gépek közti replikálásához.
Egy másik folyamat-kiszolgáló központi telepítése | | | > 2 TB | Ha több mint 200 gépeket replikál, vagy ha hello napi módosulása aránya meghaladja a 2 TB további folyamat kiszolgálók telepítése

Az elemek magyarázata:

* Minden forrásgép 100 GB, 3 lemezzel van konfigurálva.
* Összehasonlítási tárolása 8 SAS-meghajtókkal 10 k fordulat/PERCES, RAID 10-es, gyorsítótár lemez mérések használtuk.

## <a name="process-server-capacity"></a>Feldolgozása a kiszolgáló kapacitásának


hello folyamatkiszolgáló védett gépek kap replikációs adatokat, és gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket. Majd küldi hello adatok tooAzure.

- hello folyamat server gépnek rendelkeznie kell elegendő erőforrást tooperform ezeket a feladatokat.
- hello első folyamatkiszolgáló hello konfigurációs kiszolgálón alapértelmezés szerint telepítve van. További folyamat kiszolgálók tooscale telepítheti a környezetében.
- hello folyamat kiszolgáló használ a lemezalapú gyorsítótárban. Használjon külön gyorsítótár lemezt 600 GB vagy több toohandle adatok változásait a hálózati szűk vagy leállás hello esemény tárolja.
- Ha több mint 200 gépek kell tooprotect, vagy hello napi változási sebessége nagyobb, mint 2 TB, folyamat kiszolgálók toohandle hello replikációs terhelés is hozzáadhat. kimenő tooscale, a következőket teheti:
    - A konfigurációs kiszolgálók hello számának növeléséhez. Például védelmet biztosíthat a too400 gépek két konfigurációs kiszolgálóval.
    - Folyamat további kiszolgálók hozzáadásához, és ezek toohandle forgalom helyett (vagy kívül) hello konfigurációs kiszolgálót.


### <a name="example-process-server-scaling"></a>Példa folyamat kiszolgálóinak méretezéséről

a következő táblázat hello egy olyan forgatókönyvet, amelyben ismerteti:

* Toouse hello konfigurációs kiszolgáló nem készül, folyamat-kiszolgálóként.
* Egy további folyamatkiszolgáló beállítása.
* Védett virtuális gépek toouse hello további folyamatkiszolgáló konfigurálását.
* Minden védett forrásgép 100 GB három lemez van konfigurálva.

**Konfigurációs kiszolgáló** | **További folyamatkiszolgáló** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek**
--- | --- | --- | --- | ---
8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB memória | 4 Vcpu (2 sockets * @ 2,5 GHz-es 2 mag), 8 GB memória | 300 GB | 250 GB vagy kevesebb | 85 vagy kevesebb gépek replikálása.
8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB memória | 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 12 GB memória | 600 GB | 250 GB too1 TB | Replikálja a 85-150 gépek között.
12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag), 18 GB memória | 12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) 24 GB memória | 1 TB | 1 TB-os too2 TB | 150-225 gépek közti replikálásához.

amelyben a kiszolgálók méretezése hello módja méretezett és kibővített modell beállításoktól függ.  Vertikális felskálázás néhány csúcskategóriás konfigurációs és folyamat-kiszolgálók üzembe helyezésével, vagy kevesebb erőforrást további kiszolgálók üzembe helyezésével kiterjesztése. Például ha tooprotect 220 gépek van szüksége, módszerekkel hello alábbi:

* 12 vCPU, 18 GB memória, és egy további folyamatkiszolgáló 12 vCPU, 24 GB memóriát a konfigurációs kiszolgáló hello beállítása. Védett gépek toouse hello további folyamatkiszolgáló csak konfigurálása.
* (2 darab 8 vCPU, 16 GB RAM) két konfigurációs kiszolgáló és két további folyamat-kiszolgáló (1 x 8 vCPU és 4 vCPU x 1 toohandle 135-ös + 85 [220] gépek) beállítása. Védett gépek toouse hello további folyamat csak kiszolgálók konfigurálása.

## <a name="deploy-additional-process-servers"></a>További folyamat-kiszolgálók központi telepítése

Hajtsa végre az ezen utasításokat tooset fel egy további folyamatkiszolgáló. Hello kiszolgáló telepítése után telepít át forrás gépek toouse azt.

1. A **Site Recovery kiszolgálók**, kattintson a hello konfigurációs kiszolgáló > **+ Folyamatkiszolgáló**.
2. A **kiszolgálótípus**, kattintson a **folyamat server (helyszíni)**.

    ![Folyamatkiszolgáló](./media/vmware-walkthrough-capacity/migrate-ps2.png)
3. Töltse le a hello Site Recovery az egységes telepítő fájlját.
4. Futtassa a telepítő tooinstall hello folyamatkiszolgáló, és regisztrálja azt hello tárolóban.
5. A **megkezdése előtt**, jelölje be **további folyamat kiszolgálók tooscale ki a központi telepítés hozzáadása**.
6. A **konfigurációs kiszolgáló adatait**, hello hello konfigurációs kiszolgáló IP-címét adja meg, és hello jelszót. Ha még nem rendelkezik hello jelszót, töltse le innen futtatásával **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** hello konfigurációs kiszolgálón.

    ![Konfigurációs kiszolgáló](./media/vmware-walkthrough-capacity/add-ps2.png)
7. Fejezze be a telepítő hello hello részeinek pontosan ugyanúgy hello konfigurációs kiszolgáló beállításakor.

### <a name="migrate-machines-toouse-hello-process-server"></a>Telepítse át a gépek toouse hello folyamatkiszolgáló

1. A **beállítások** > **Site Recovery kiszolgálók**, hello konfigurációs kiszolgálón kattintson > **kiszolgálók feldolgozni**.
2. Kattintson a jobb gombbal hello folyamatkiszolgáló jelenleg > **kapcsoló**.

    ![Folyamatkiszolgáló kapcsoló](./media/vmware-walkthrough-capacity/migrate-ps3.png)
3. A **válassza folyamat-célkiszolgálót**, jelölje be hello folyamatkiszolgáló toouse szeretné, és válassza ki a hello virtuális gépeket, hogy hello kiszolgáló fogja kezelni.
4. Kattintson a hello ikonra. döntések betöltése, és szükséges tooreplicate jelenik meg minden kijelölt virtuális gép toohello új folyamatkiszolgáló átlagos lemezterület hello elvégezte toohelp.
5. Kattintson a hello pipa toostart replikációs toohello új folyamatkiszolgáló.

## <a name="control-network-bandwidth"></a>Hálózati sávszélesség szabályozására

Futtatása után [hello telepítési Planner eszköz](site-recovery-deployment-planner.md) toocalculate hello sávszélesség van szüksége a replikáció (hello kezdeti replikáció és majd különbözeti), megadhatja a beállítások több replikáláshoz használt sávszélességet hello mennyisége:

* **A sávszélesség szabályozását**: tooAzure replikáló VMware-forgalom halad át egy adott folyamat kiszolgáló. Beállíthatja a hello rendszereken futó folyamat kiszolgálóként sávszélesség szabályozását.
* **Sávszélesség befolyásolja**: hello sávszélesség-replikációhoz használt néhány beállításkulcsok használatával is szabályozhatja:
  * Hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** beállításazonosítót hello egy lemezen adatátvitelre (kezdeti vagy változásreplikálásra replikációs) használt szálak számát határozza meg. A nagyobb érték növeli a hello replikációhoz használt hálózati sávszélességet.
  * Hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** hello adatátvitel feladat-visszavétel során használt szálak számát adja meg.

### <a name="throttle-bandwidth"></a>Sávszélesség szabályozása

1. Nyissa meg a hello Azure Backup szolgáltatás MMC beépülő modul a hello gép működő hello folyamatkiszolgáló. Alapértelmezés szerint a helyi biztonsági mentés érhető hello asztalon, vagy a mappa a következő hello: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Kattintson a beépülő modul hello **tulajdonságainak módosítása**.
3. A hello **sávszélesség-szabályozási** lapon jelölje be **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**.
4. Állítsa be a hello munkaórákra és a munkaórákon kívüli időre. Érvényes tartomány: az 512 kbit/s too102 MB / s.

    ![Szabályozás](./media/vmware-walkthrough-capacity/throttle2.png)

Is használhatja a hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag tooset sávszélesség-szabályozás. Íme egy példa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

A **Set-OBMachineSetting -NoThrottle** beállítás azt jelenti, hogy nincs szükség szabályozásra.

### <a name="influence-network-bandwidth-for-a-vm"></a>A hálózati sávszélesség szabályozása a virtuális gépek

1. A virtuális gép hello beállításjegyzékében, váltson túl**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello sávszélesség forgalom replikációs lemezen hello értékének módosítása **UploadThreadsPerVM**, vagy hozzon létre hello kulcsot, ha még nem létezik.
   * tooinfluence hello sávszélesség feladatátvételi forgalomhoz, az Azure-ból hello értékének módosítása **DownloadThreadsPerVM**.
2. hello alapértelmezett értéke 4. Egy szükségesnél több erőforrással ellátott hálózatban ezek a kulcsok kell módosítani. hello maximális 32. Forgalom toooptimize hello érték figyelése.




## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[4. lépés: hálózati terv](vmware-walkthrough-network.md).
