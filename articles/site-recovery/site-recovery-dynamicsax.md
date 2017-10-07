---
title: "egy Azure Site Recovery segítségével többrétegű Dynamics AX telepítési aaaReplicate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate és az Azure Site Recovery segítségével Dynamics AX védelme"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.openlocfilehash: b974315ec50ab2ec43846b3d3f95c7de88b72fc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a>Egy Azure Site Recovery segítségével többrétegű Dynamics AX-alkalmazás replikálása

## <a name="overview"></a>Áttekintés


A Microsoft Dynamics AX hello legnépszerűbb ERP-megoldások között vállalatok toostandardized folyamat egyik helyszínen, kezelheti az erőforrásokat és a megfelelőségi leegyszerűsítve. Considering hello alkalmazás üzleti kritikus tooan szervezet nagyon fontos, hogy toobe, ha bármely katasztrófa alkalmazás kell működik, és minimális időtartam.

A Microsoft Dynamics AX jelenleg nem biztosít bármely out-of-az-box vész-helyreállítási funkciók. A Microsoft Dynamics AX sok kiszolgáló összetevőből áll például objektum alkalmazáskiszolgáló, Active Directory (AD), az SQL adatbázis-kiszolgáló, a SharePoint Server, a Reporting Server stb toomanage hello vész-helyreállítási az egyes összetevők manuálisan nem csak költséges de is hibákhoz vezethet.

Ez a cikk ismerteti részletesen hogyan hozhat létre egy vész-helyreállítási megoldást a Dynamics AX alkalmazás a [Azure Site Recovery](site-recovery-overview.md). Tervezett/nem tervezett/feladatátvételi tesztet egy kattintással helyreállítási tervet, a támogatott konfigurációk és az Előfeltételek is magában foglalja.
Az Azure Site Recovery-alapú vész-helyreállítási megoldást teljesen tesztelni, hitelesített, és a Microsoft Dynamics AX által ajánlott.



## <a name="prerequisites"></a>Előfeltételek

Hello Előfeltételek befejeződött a következő végrehajtási vész-helyreállítási Azure Site Recovery segítségével Dynamics AX-alkalmazáshoz szükséges.

• Egy helyszíni Dynamics AX-telepítés beállítása

• Az azure Site Recovery Services-tároló létrehozása a Microsoft Azure-előfizetés

• Ha Azure a helyreállítási helyen futtatja hello Azure virtuális gép Readiness Assessment eszközt a virtuális gépek tooensure, amelyek kompatibilisek a Azure virtuális gépek és az Azure Site Recovery Services


## <a name="site-recovery-support"></a>Webhely-helyreállítási támogatás

Hello célból történő létrehozásának ebben a cikkben VMware virtuális gépek, a Windows Server 2012 R2 Enterprise a Dynamics AX 2012R3 használták. Mivel helyreállítási helyreplikálásának alkalmazás független, hello javaslatok itt megadott várható toohold a következő helyzetekben is.

### <a name="source-and-target"></a>Forrása és célja

**A forgatókönyv** | **másodlagos hely tooa** | **tooAzure**
--- | --- | ---
**Hyper-V** | Igen | Igen
**VMware** | Igen | Igen
**Fizikai kiszolgáló** | Igen | Igen

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a>Azure Site Recovery segítségével DR Dynamics AX-alkalmazás engedélyezése
### <a name="protect-your-dynamics-ax-application"></a>A Dynamics AX alkalmazás védelme
Minden összetevője hello Dynamics AX igényeinek toobe védett tooenable hello teljes alkalmazás replikáció és a helyreállítás. Ez a szakasz a következőkkel foglalkozik:

**1. Az Active Directory védelme**

**2. SQL-réteg védelme**

**3. Alkalmazás-és webes réteg**

**4. Hálózati konfigurációja**

**5. Helyreállítási terv**

### <a name="1-setup-ad-and-dns-replication"></a>1. A telepítő AD és a DNS-replikáció

Az Active Directory szükséges a Dynamics AX alkalmazás toofunction hello vész-Helyreállítási helyen. Nincsenek ajánlott és két választási lehetőség, hello az ügyfél helyszíni környezetben hello összetettsége alapján.

**1. lehetőséget**

Ha hello vevő kis számú alkalmazást és annak teljes egyetlen tartományvezérlővel rendelkezik helyszíni hely és fog kell feladatátadás hello teljes webhelyet együtt, akkor azt javasoljuk, az ASR-replikáció tooreplicate hello DC gép toosecondary hely ( a alkalmazható hely tooSite és a hely tooAzure).

**2. lehetőséget**

Ha hello felhasználói kérelmek nagy számú és fut-e az Active Directory-erdőt és a rendszer feladatátvételi néhány alkalmazás egyszerre, akkor azt javasoljuk, hogy egy további tartományvezérlőt hello vész-Helyreállítási helyen beállítása (másodlagos helyre vagy az Azure-ban).

Tekintse meg a túl[útmutatója a tartományvezérlő elérhetővé tétele a vész-Helyreállítási helyen](site-recovery-active-directory.md). Ez a dokumentum hátralévő indulunk ki vész-Helyreállítási helyen rendelkezésre áll a tartományvezérlő.

### <a name="2-setup-sql-server-replication"></a>2. SQL Server-replikáció beállítása
Tekintse meg a javasolt beállítást védelmének hello részletes műszaki útmutatást toocompanion útmutató [SQL réteg](site-recovery-sql.md).

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a>3. A Dynamics AX-ügyfél és AOS virtuális gépek védelmének engedélyezése
Hajtsa végre a megfelelő Azure Site Recovery konfigurációs alapján e hello virtuális gépek vannak telepítve [Hyper-V](site-recovery-hyper-v-site-to-azure.md) vagy a [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Ajánlott összeomlás-konzisztens gyakoriság tooconfigure érték 15 perc.
>

hello pillanatkép alatt hello védelmi állapot Dynamics összetevő virtuális gépek "VMware-hely tooAzure" védelmi forgatókönyvben jeleníti meg.
![Védett elemek](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. A hálózatkezelés konfigurálását
Konfigurálja a virtuális gép számítási és hálózati beállításai

Hello AX ügyfél és AOS virtuális gépek hálózati beállítások konfigurálása a Azure Site Recovery így hello Virtuálisgép-hálózatok csatolt toohello jobb vész-Helyreállítási hálózati feladatátvételt követően. Győződjön meg ezek a rétegek hello vész-Helyreállítási hálózatot irányítható toohello SQL réteg.

Kiválaszthatja a hello VM hello replikált elemek tooconfigure hello hálózati beállítások hello pillanatkép az alábbi ábrán.

* AOS kiszolgálók hello megfelelő rendelkezésre állási csoport kiválasztása.

* Ha statikus IP-címet használ, akkor adja meg a használni kívánt virtuális gép tootake a hello hello hello IP **cél IP-címet** mező ![hálózati beállításai](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)



### <a name="5-creating-a-recovery-plan"></a>5. Helyreállítási terv létrehozása

Az Azure Site Recovery tooautomate hello feladatátvételi folyamat egy helyreállítási tervet is létrehozhat. Adja hozzá a helyreállítási terv hello app réteget és web réteget. Rendezze őket a különböző csoporthoz, amely hello előtér leállítás előtt app réteget.

1)  Jelölje ki az előfizetés hello Azure Site Recovery tárolójából, majd kattintson a helyreállítási tervek csempe.

2)  Kattintson a "+ a helyreállítási terv, és adjon meg egy nevet.

3)  Válassza ki a hello "Forrás" és "Target". hello cél Azure-bA vagy másodlagos hely lehet. Abban az esetben, ha úgy dönt, hogy Azure, meg kell adnia hello telepítési modell

![Helyreállítási terv létrehozása](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  Jelölje ki a hello AOS és az ügyfél virtuális gépek toohello helyreállítási tervet, majd kattintson a ✓.
![Helyreállítási terv létrehozása](./media/site-recovery-dynamics-ax/selectvms.png)


![Helyreállítási terv](./media/site-recovery-dynamics-ax/recoveryplan.png)

Az alábbiakban leírtak szerint számos lépés hozzáadásával testre hello helyreállítási terv Dynamics AX-alkalmazás. pillanatkép fent hello hello teljes helyreállítási terv hello lépéseket hozzáadása után jeleníti meg.

*Lépéseket:*

*1. SQL Server feladatátvételt lépései*

Tekintse meg a túl["SQL Server vész-Helyreállítási megoldás"](site-recovery-sql.md) helyreállítási lépéseket adott tooSQL server részleteit útmutatója.

*2. Feladatátvételi csoport 1: Feladatok átadása hello AOS virtuális gépeken a*

Győződjön meg arról, hogy kiválasztott helyreállítási pont hello PIT lehetséges toohello adatbázisként zárja be, de nem előre.

*3. Parancsfájl: Hozzáadás terheléselosztó (csak az E-A)* (az Azure automation) keresztül parancsfájl hozzáadása után AOS Virtuálisgép-csoport, a load balancer tooit tooadd megjelenik. Ezt a feladatot használhatja egy parancsfájl toodo. Tekintse meg a cikk [hogyan tooadd terheléselosztóját többrétegű alkalmazást vész-Helyreállítási](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)

*4. Feladatátvételi csoport 2: Hello AX ügyfél virtuális gépek feladatátvételt.*
Feladatok átadása hello webes réteg virtuális gépek hello helyreállítási terv részeként.


### <a name="doing-a-test-failover"></a>A teszt feladatátvétel

Tekintse meg a vész-Helyreállítási megoldás too'AD "és"SQL Server vész-Helyreállítási megoldás"kiegészítő útmutatók adott tooAD szempontok és SQL server feladatátvételi teszt során.

1.  Nyissa meg tooAzure portálon, és válassza ki a Site Recovery-tárolóban.
2.  Kattintson a hello helyreállítási tervben készült Dynamics AX.
3.  Kattintson a "Test Failover".
4.  Válassza ki a hello virtuális hálózati toostart hello teszt feladatátvételi folyamatot.
5.  Hello másodlagos környezetben működik, ha az érvényesítést végezheti el.
6.  Ha hello ellenőrzés befejeződött, kiválaszthatja, hogy érvényesítést végrehajtani, és hello teszt feladatátvételi környezet törölve lesznek.

Hajtsa végre a [Ez az útmutató](site-recovery-test-failover-to-azure.md) toodo feladatátvételi tesztet.

### <a name="doing-a-failover"></a>A feladatátvétel

1.  Nyissa meg tooAzure portálon, és válassza ki a Site Recovery-tárolóban.
2.  Kattintson a hello helyreállítási tervben készült Dynamics AX.
3.  Kattintson a "Failover", és válassza a "Failover".
4.  Jelölje ki a hello célhálózat, majd kattintson a ✓ toostart hello feladatátvételi folyamat.

Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) Ha feladatátvételt végez.

### <a name="perform-a-failback"></a>A feladat-visszavételt végrehajtani

Tekintse meg a kiszolgáló vész-Helyreállítási megoldás too'SQL "útmutatója szempontok adott tooSQL kiszolgáló feladat-visszavétel során.

1.  Nyissa meg tooAzure portálon, és válassza ki a Site Recovery-tárolóban.
2.  Kattintson a hello helyreállítási tervben készült Dynamics AX.
3.  Kattintson a "Failover", és válassza ki a feladatátvételt.
4.  Kattintson a "Módosítás irány".
5.  Válassza ki hello megfelelő beállítást - adatok szinkronizálása és a virtuális gép létrehozásának beállításai
6.  Kattintson a ✓ toostart hello feladat-visszavétel"folyamat.


Hajtsa végre a [Ez az útmutató](site-recovery-failback-azure-to-vmware.md) Ha egy feladat-visszavétel során.

##<a name="summary"></a>Összefoglalás
Azure Site Recovery segítségével, a Dynamics AX-alkalmazás egy teljes automatizált vészhelyreállítási tervet hozhat létre. Hello feladatátvételi bárhonnan másodpercen belül is kezdeményezhető a hello eseményeket, a szüneteltetése és hello alkalmazás első lépéseivel perc múlva.

## <a name="next-steps"></a>Következő lépések
Olvasási [milyen számítási feladatokat tud védeni?](site-recovery-workload.md) toolearn további információk az Azure Site Recovery vállalati munkaterhelések védelme.
