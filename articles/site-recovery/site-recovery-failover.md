---
title: "a helyreállítás aaaFailover |} Microsoft Docs"
description: "Az Azure Site Recovery koordinálja hello replikációjának, feladatátvételének és helyreállításának virtuális gépek és fizikai kiszolgálók. További információk a feladatátvételi tooAzure vagy egy másodlagos adatközpontba."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: pratshar
ms.openlocfilehash: 7cacea829d78bb7de2b2d67402291b472b10f023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="failover-in-site-recovery"></a>Feladatátvétel a Site Recoveryben
Ez a cikk ismerteti, hogyan toofailover virtuális gépek és fizikai kiszolgálók Site Recovery által védett.

## <a name="prerequisites"></a>Előfeltételek
1. Előtt végezzen feladatátvételi, tegye a [feladatátvételi teszt](site-recovery-test-failover-to-azure.md) tooensure, amely minden a várt módon működik.
1. [Készítse elő a hello hálózati](site-recovery-network-design.md) előtt végezzen feladatátvételi célként megadott helyen.  


## <a name="run-a-failover"></a>Feladatátvétel futtatása
Ez az eljárás ismerteti, hogyan toorun a feladatátvételt egy [helyreállítási terv](site-recovery-create-recovery-plans.md). Másik megoldásként futtathatja egyetlen virtuális gép vagy fizikai kiszolgálók hello feladatátvételt hello **replikált elemek** lap


![Feladatátvétel](./media/site-recovery-failover/Failover.png)

1. Válassza ki **helyreállítási tervek** > *recoveryplan_name*. Kattintson a **feladatátvételi**
2. A hello **feladatátvételi** képernyőn válassza ki a **helyreállítási pont** toofailover számára. Hello alábbi beállítások egyikét használhatja:
    1.  **Legújabb** (alapértelmezett): Ez a beállítás először dolgozza fel a minden hello adat, amelyet elküldött tooSite helyreállítási szolgáltatás toocreate minden egyes virtuális géphez a helyreállítási pont előtt tooit feladatátadás őket. Ezt a lehetőséget biztosít a hello legalacsonyabb rpo-val Rendelkeznek (Helyreállításipont-célkitűzés) hello virtuális gép létrehozása után feladatátvételi rendelkezik minden hello adat, amelyet replikált tooSite helyreállítási szolgáltatás hello feladatátvételi indítását.
    1.  **Legújabb feldolgozott**: Ez a beállítás átadja a feladatokat az összes virtuális gép hello helyreállítási terv toohello legújabb helyreállítási pont, amely a Site Recovery szolgáltatás feldolgozása már megtörtént. A virtuális gép feladatátvételi tesztje során, időbélyegzőjét hello legutóbbi feldolgozott helyreállítási pontot is látható. Akkor használatos, ha a helyreállítási terv feladatátvételi, nyissa meg tooindividual virtuális gépet, és nézze meg **legújabb helyreállítási pontok** csempe tooget ezt az információt. Nincs időt vesz igénybe tooprocess hello feldolgozatlan adatokat, ez a beállítás egy alacsony RTO (helyreállítási idő célkitűzése) feladatátvételi lehetőséget biztosít.
    1.  **Legutóbbi alkalmazáskonzisztens**: Ez a beállítás átadja a feladatokat a hello helyreállítási terv toohello legújabb alkalmazáskonzisztens helyreállítási pontnak, amely a Site Recovery szolgáltatás által már feldolgozott összes virtuális gépet. A virtuális gép feladatátvételi tesztje során, időbélyegzőjét hello legutóbbi alkalmazáskonzisztens helyreállítási pontnak is látható. Akkor használatos, ha a helyreállítási terv feladatátvételi, nyissa meg tooindividual virtuális gépet, és nézze meg **legújabb helyreállítási pontok** csempe tooget ezt az információt.
    1.  **Legújabb virtuális Gépre kiterjedő feldolgozott**: Ez a beállítás csak érhető el helyreállítási tervek, amelyeken legalább egy virtuális gép több virtuális Gépre kiterjedő konzisztencia. Virtuális gépek, amelyek egy replikációs csoport feladatátvételi toohello legújabb közös virtuális Gépre kiterjedő konzisztens helyreállítási pontot. Más virtuális gépek feladatátvételi tootheir legutóbbi feldolgozott helyreállítási pontot.  
    1.  **Legújabb virtuális Gépre kiterjedő alkalmazáskonzisztens**: Ez a beállítás csak érhető el, amelyek rendelkeznek legalább egy virtuális gép több virtuális Gépre kiterjedő konzisztencia ON helyreállítási tervek. Virtuális gépek részei egy replikációs csoport feladatátvételi toohello legújabb közös virtuális Gépre kiterjedő alkalmazáskonzisztens helyreállítási pontnak. Más virtuális gépek feladatátvételi tootheir utolsó alkalmazáskonzisztens helyreállítási pont.
    1.  **Egyéni**: Ha egy virtuális gép feladatátvételi tesztje során, akkor használhatja ezt a beállítást toofailover tooa adott helyreállítási pontot.

    > [!NOTE]
    > hello beállítás toochoose egy helyreállítási pontot csak akkor használható, ha Ön tooAzure alatt nem működnek.
    >
    >


1. Ha néhány hello virtuális gépek hello helyreállítási tervben feladatátvételt volt az előző és hello virtuális gépek aktívak a forrás- és a célként megadott helyen, most használhatja **irányának módosítása** toodecide hello irányban lehetőséget hello feladatátvételt megtörténik.
1. Ha a feladat-visszavétele tooAzure keresztül, és hello felhő (csak akkor, ha a védett Hyper-v virtuális gépek VMM-kiszolgálóról vonatkozik) engedélyezett az adattitkosítás, a **titkosítási kulcs** válassza hello korábban kiadott tanúsítványt mikor meg engedélyezve van az adattitkosítás hello VMM-kiszolgáló telepítése során.
1. Válassza ki **gép leállítása a feladatátvétel megkezdése előtt** Ha azt szeretné, hogy a Site Recovery tooattempt toodo ahhoz, hogy kiváltsa a forrás virtuális gépek leállítására hello feladatátvételi. Feladatátvételi továbbra is fennáll, akkor is, ha a leállítása sikertelen.  

    > [!NOTE]
    > Hyper-v virtuális gépek esetén ez a beállítás megpróbál toosynchronize hello a helyszíni adatokhoz, amely rendelkezik még el nem küldött toohello szolgáltatás hello feladatátvételi elindítása előtt.
    >
    >

1. Kövesse a hello hello feladatátvételi folyamat előrehaladásának **feladatok** lap. Akkor is, ha a nem tervezett feladatátvétel során fordult elő, hello helyreállítási terv fut, amíg nem fejeződik be.
1. Hello feladatátvétel után ellenőrzéséhez hello virtuális gép tooit van bejelentkezve. Ha szeretné toogo egy másik helyreállítási pont hello virtuális géphez, majd használhatja **helyreállítási pont módosítása** lehetőséget.
1. Miután elégedett hello virtuális gép a feladatátvételt, **véglegesítése** feladatátvételi hello. Ez törli az összes hello elérhető helyreállítási pontokkal hello szolgáltatásban és **helyreállítási pont módosítása** lehetőség már nem lesz elérhető.

## <a name="planned-failover"></a>Tervezett feladatátvétel
Ezenkívül is támogatja a Site Recovery használatával védett Hyper-V virtuális gépek a feladatátvétel **tervezett feladatátvétel**. Ez a beállítás nulla adatok elvesztését feladatátvételi. Egy tervezett feladatátvételt akkor váltódik ki, amikor először hello a forrás virtuális gépek állítsa le, hello adatok még szinkronizált toobe szinkronizálva, és a feladatátvétel aktiválódik, majd.

> [!NOTE]
> Ha Ön feladatátvételi Hyper-v virtuális gépek egy helyszíni hely tooanother helyszíni hely, toocome hátsó toohello helyszíni elsődleges hely rendelkezik toofirst **visszirányú replikálás** hello virtuális gép hátsó tooprimary hely és majd elindítani a feladatátvételt. Ha hello elsődleges virtuális gép nem áll rendelkezésre, majd elindítása előtt túl**visszirányú replikálás** toorestore hello virtuális gép egy biztonsági másolatból.   
>
>

## <a name="failover-job"></a>Feladatátvételi feladatban

![Feladatátvétel](./media/site-recovery-failover/FailoverJob.png)

Amikor elindul a feladatátvétel, magában foglalja a következő lépéseket:

1. Előfeltételek ellenőrzése: Ez a lépés biztosítja, hogy a feladatátvétel szükséges összes feltétel teljesül-e
1. Feladatátvétel: Ez a lépés hello adatokat dolgozza fel, így készen, hogy egy Azure virtuális gépek hozhatók létre belőle. Ha úgy döntött, **legújabb** helyreállítási pont ebben a lépésben létrehoz egy helyreállítási pontot, amely el lett küldve toohello szolgáltatás hello adatokból.
1. Kezdete: Ebben a lépésben egy Azure virtuális gép hello adatfeldolgozási hello előző lépésben használatával hoz létre.

> [!WARNING]
> **Ne szakítsa meg az egy folyamatban lévő feladatátvételi**: elindítani a feladatátvételt, mielőtt hello virtuális gép replikálása leállt. Ha Ön **Mégse** egy folyamatban lévő feladat, a feladatátvétel leállítja, de hello virtuális gép nem fog elindulni tooreplicate. Nem lehet újból elindítani a replikációt.
>
>

## <a name="time-taken-for-failover-tooazure"></a>Feladatátvételi tooAzure igénybe vett idő

Bizonyos esetekben virtuális gépeinek feladatátvételi egy extra köztes lépés, amely általában körülbelül 8 too10 perc toocomplete van szükség. Ezekben az esetekben vannak, mint a következő:

* VMware virtuális gépek használata a mobilitási szolgáltatás régebbi, mint 9.8 verzió
* Fizikai kiszolgálók 
* VMware Linux virtuális gépek
* Fizikai kiszolgálóként védett Hyper-V virtuális gépek
* VMware virtuális gépek nincsenek jelen, ahol következő illesztőprogramok rendszerindító illesztőprogramok 
    * storvsc 
    * VMBus 
    * storflt 
    * Intelide 
    * ATAPI
* VMware virtuális gépek, amelyek nem rendelkeznek a DHCP-szolgáltatás engedélyezve van, függetlenül attól, hogy használják DHCP vagy statikus IP-címek

Az összes hello más esetekben a köztes lépésre nincs szükség, és határozottan kevesebb hello feladatátvételi hello idő. 





## <a name="using-scripts-in-failover"></a>A feladatátvevő parancsfájlok használata
Érdemes lehet tooautomate bizonyos műveleteket a feladatátvétel során. Parancsfájlokat vagy [Azure automation-forgatókönyveket](site-recovery-runbook-automation.md) a [helyreállítási tervek](site-recovery-create-recovery-plans.md) toodo, amely.

## <a name="other-considerations"></a>Egyéb szempontok
* **Meghajtóbetűjel** – tooretain hello betűjelű meghajtóra telepítse a virtuális gépek feladatátvétel után is beállíthatja hello **TÁROLÓHÁLÓZATI szabályzatát** hello virtuális a gép túl**OnlineAll**. [További információk](https://support.microsoft.com/en-us/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).



## <a name="next-steps"></a>Következő lépések
Miután a virtuális gépek átvette, és a helyszíni adatközpont hello érhető el, akkor [ **védelmének újbóli beállításához** ](site-recovery-how-to-reprotect.md) VMware virtuális gépek biztonsági toohello helyszíni adatközpontban.

Használja [ **tervezett feladatátvétel** ](site-recovery-failback-from-azure-to-hyper-v.md) beállítás túl**feladat-visszavétel** Hyper-v virtuális gépek biztonsági tooon helyszíni az Azure-ból.

Ha nem sikerült a Hyper-v virtuális gép tooanother a helyszíni adatok felügyeli a VMM-kiszolgáló és a hello elsődleges adatközpont center érhető el, majd **a visszirányú replikálás** beállítás toostart hello replikációs hátsó toohello elsődleges adatközpont.
