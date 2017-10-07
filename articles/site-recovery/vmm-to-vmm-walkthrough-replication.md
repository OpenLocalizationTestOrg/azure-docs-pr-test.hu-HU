---
title: "Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery egy replikációs házirendnek aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan tooset be olyan házirendeket, a Hyper-V virtuális gép replikációs tooa másodlagos VMM helyet az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5d9b79cf-89f2-4af9-ac8e-3a32ad8c6c4d
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6d008e3bb3fa0b666e91678cf6de3693dd712ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy"></a>8. lépés: A replikációs házirend beállítása

Konfigurálása után [hálózatleképezés](vmm-to-vmm-walkthrough-network-mapping.md), ez a cikk tooset fel egy replikációs házirendet használja a Hyper-V virtuális gép (VM) replikációs tooa másodlagos helyhez használata [Azure Site Recovery](site-recovery-overview.md).

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Előkészületek

- Replikációs házirend létrehozásakor kell rendelkeznie hello ugyanazon operációs rendszer minden gazdagép hello házirenddel. VMM-felhő hello csak Windows Server különböző verzióit futtató Hyper-V-gazdagépek, de ebben az esetben szüksége több replikációs házirend.
- Hello kezdeti replikálás offline végezheti el.

## <a name="configure-replication-settings"></a>Replikációs beállítások konfigurálása

1. Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.

    ![Network (Hálózat)](./media/vmm-to-vmm-walkthrough-replication/gs-replication.png)
2. A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét. hello forrás és cél-típusnak kell lennie **Hyper-V**.
3. A **Hyper-V gazdagép verziószámánál**, válassza ki, melyik operációs rendszer fut hello gazdagépen.
4. A **hitelesítési típus** és **hitelesítési portot**, adja meg, hogyan hello elsődleges és a helyreállítási Hyper-V gazdakiszolgálók közötti forgalom hitelesítése. Válassza ki **tanúsítvány** kivéve, ha Kerberos-környezetben működő rendelkezik. Az Azure Site Recovery automatikusan HTTPS-hitelesítési tanúsítványok konfigurálja. Nincs szükség a toodo semmit manuálisan. Alapértelmezés szerint a Windows tűzfal hello a Hyper-V gazdakiszolgálókra hello port 8083 és 8084 (a tanúsítványok) fognak megnyílni. Ha **Kerberos**, a Kerberos jegy hello kiszolgálók a kölcsönös hitelesítéshez használható. Vegye figyelembe, hogy ez a beállítás csak a Windows Server 2012 R2 rendszeren futó Hyper-V gazdakiszolgálók szükséges.
5. A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.
6. A **helyreállításipont-megőrzést**, adja meg, órákban mennyi ideig hello adatmegőrzési időtartam fogja az egyes helyreállítási pontok lehet. Védett gépek lehet helyreállítani egy időszakban tooany pont.
7. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, hogy milyen gyakran (1 – 12 órába) helyreállítási pontokat tartalmazó alkalmazáskonzisztens pillanatképeket jönnek létre. Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül. Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában. Ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére. Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.
8. A **adatátvitel tömörítésének**, adja meg, hogy van-e a replikált adatok tömörített.
9. Válassza ki **replika virtuális gép törlése**, toospecify, amely a replika virtuális gép hello hello forrás virtuális gép védelmének letiltásakor törölni kell. Hello forrás hello Site Recovery konzolján lekerül virtuális gép védelmének letiltásakor engedélyezi ezt a beállítást, ha a Site Recovery beállításait a VMM hello el lesznek távolítva hello VMM-konzol, és hello replika törlődik.
10. A **kezdeti replikációs módszer**, ha replikál hello hálózaton, adja meg, hogy toostart hello a kezdeti replikálás, vagy átütemezheti azt. toosave hálózati sávszélesség, érdemes lehet tooschedule azt a foglalt munkaidőn kívül. Ezután kattintson az **OK** gombra.

     ![Replikációs szabályzat](./media/vmm-to-vmm-walkthrough-replication/gs-replication2.png)
11. Ha egy új házirendet hoz létre, automatikusan rendelkezik hello VMM-felhő társítva. A **replikációs házirend**, kattintson a **OK**. További VMM-Felhőkben (és a bennük foglalt virtuális gépek hello) társíthatja a replikációs házirendet a **replikációs** > szabályzat neve > **VMM-felhő társítása**.

     ![Replikációs szabályzat](./media/vmm-to-vmm-walkthrough-replication/policy-associate.png)



## <a name="prepare-for-offline-initial-replication"></a>Offline kezdeti replikálás előkészítése

Az első adatmásolás hello offline replikációt teheti meg. Előkészítheti a következőképpen:

* Hello forráskiszolgálón adjon meg egy elérési helyet, mely hello származó adatok exportálása akkor kerül sor. Rendeljen teljes hozzáférés NTFS és megosztási engedélyek toohello VMM szolgáltatás hello exportálási elérési úton. Hello célkiszolgálón akkor adja meg egy elérési útja, amelyből hello adatok importálása megtörténik. Rendelje hozzá a hello ugyanazokkal az engedélyekkel az importálási útvonalat.
* Ha hello importálása vagy exportálása elérési út megosztása, rendeljen hozzájuk csoporttagságot rendszergazda, a kiemelt felhasználó, a Nyomtatófelelősök vagy a kiszolgáló operátor hello VMM-szolgáltatásfiók hello távoli számítógépen mely megosztott hello található.
* Futtató fiókok tooadd állomások használata, a hello importálása és exportálási útvonalakra, rendelje hozzá az olvasási és írási engedélyek toohello futtató fiókokat a VMM-ben.
* Importálás hello és megosztások exportálása nem kell elhelyezni minden olyan számítógépen, használja a Hyper-V gazdagép-kiszolgálón, mert visszacsatolási konfiguráció nem támogatott a Hyper-v.
* Az Active Directory minden Hyper-V gazdakiszolgálón futó virtuális gépeket tartalmazó szeretné, hogy tooprotect, engedélyezheti és konfigurálhatja a korlátozott delegálás tootrust hello távoli számítógépek mely hello importálási és exportálási útvonalak találhatók, az alábbiak szerint:
  1. Hello tartományvezérlőn nyissa meg a **Active Directory – felhasználók és számítógépek**.
  2. Hello konzol konzolfáján kattintson **tartománynév** > **számítógépek**.
  3. Kattintson a jobb gombbal a Hyper-V hello futtató kiszolgáló neve > **tulajdonságok**.
  4. A hello **delegálás** lapra, majd **a számítógépen csak a delegálás toospecified szolgáltatások**.
  5. Kattintson a **bármely hitelesítési protokoll**.
  6. Kattintson a **hozzáadása** > **felhasználók és számítógépek**.
  7. Hello típusnév hello számítógép, amelyen hello exportálási elérési útja > **OK**. Elérhető szolgáltatások hello listáról hello CTRL billentyűt lenyomva, majd kattintson a **cifs** > **OK**. Ismételje meg a hello hello számítógép nevét, hogy állomások hello importálási elérési útja. Ismételje meg a Hyper-V-gazdagép további kiszolgálókat a megfelelő.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[9. lépés: engedélyezze a replikálást](vmm-to-vmm-walkthrough-enable-replication.md).
