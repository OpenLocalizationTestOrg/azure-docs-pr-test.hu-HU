---
title: "a feladatátvétel és az Azure Site Recovery helyreállítási aaaCreate helyreállítási tervek |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate és testreszabása az Azure Site Recovery szolgáltatásban toofail helyreállítási tervek keresztül, és a virtuális gépek és fizikai kiszolgálók helyreállításához"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 09ca7719e92460b283947fdbe752e8654e5b9cab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-recovery-plans"></a>A helyreállítási terv létrehozása


Ez a cikk tájékoztatást ad azokról a létrehozása és testreszabása a helyreállítási terv [Azure Site Recovery](site-recovery-overview.md).

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Hozzon létre helyreállítási tervek toodo hello a következő:

* Adja meg a csoportok a feladataikat együtt átadó, amely majd együtt elindításához gépek.
* Gépek, együtt csoportosításával őket egy helyreállítási modellre függőségeinek csoport megtervezése. Például keresztül toofail és elindítani az adott alkalmazást, a csoportosítás összes hello virtuális gépek az adott alkalmazáshoz történő hello ugyanabban a helyreállítási terv csoportban.
* A feladatátvételt. A helyreállítási terv futtathatja a teszteket, tervezett, vagy nem tervezett feladatátvételt.


## <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása

1. Kattintson a **helyreállítási tervek** > **helyreállítási terv létrehozása**.
   Adjon meg egy nevet hello helyreállítási tervet, és a forrás és cél. hello forráshely kell rendelkeznie, amelyek engedélyezve vannak a feladatátvétel és a helyreállítási virtuális gépek.

    - A VMM tooVMM replikáció, válassza ki a **forrástípus** > **VMM**, és a forrás és cél VMM-kiszolgálók hello. Kattintson a **Hyper-V** toosee felhők védett.
    - Válassza a VMM tooAzure **forrástípus** > **VMM**.  Jelölje be hello forrás VMM-kiszolgálón, és **Azure** hello célként.
    - A Hyper-V replikáció tooAzure (VMM nélkül), válassza **forrástípus** > **Hyper-V hely**. Jelölje be hello hely hello forrásként, és **Azure** hello célként.
    - A VMware virtuális gép vagy fizikai helyszíni kiszolgáló tooAzure, válassza ki a konfigurációs kiszolgáló hello forrásként, és **Azure** hello célként.
    - Azure tooAzure helyreállítási tervet válasszon egy Azure-régiót hello forrásként, és egy másodlagos Azure-régiót hello cél. hello másodlagos Azure régiók a következők csak azon toowhich virtuális gépek védelmét.
2. A **válassza ki a virtuális gépek**, válassza ki a virtuális gépek hello (vagy a replikációs csoport), amelyet az tooadd toohello alapértelmezett csoport (1. csoport) hello helyreállítási tervben.

## <a name="customize-and-extend-recovery-plans"></a>Testreszabására és kibővítésére helyreállítási tervek

Testre szabhatja és a helyreállítási terv kiterjesztése:

- **Új csoportok hozzáadása**– adja hozzá a további helyreállítási terv csoportok (felfelé tooseven) toohello alapértelmezett csoportot, és adja hozzá a további gépek vagy replikációs csoportok toothose helyreállítási terv csoportok. Csoportok számozása hello ahhoz, ahol hozzá őket. Egy virtuális géphez, vagy a replikációs csoport esetében is csak egy helyreállítási terv csoportba foglalandó.
- **Adja hozzá a manuális műveletet**– manuális műveletek előtt vagy a helyreállítási terv csoport után is hozzáadhat. Hello helyreállítási terv futtatásakor megáll hello pontot, amellyel beszúrni hello manuális műveletet. Egy párbeszédpanelen kéri, hogy befejeződött-e hello manuális műveletet toospecify.
- **A parancsprogramok hozzáadásához**– előtt vagy után egy helyreállítási terv csoport futtatott parancsfájlok, adhat hozzá. Ha egy új, műveletek hello csoport egy új csoportját ad hozzá. Például előtti ismertetett lépések az 1. csoport jön létre hello name: csoport 1: előtti lépéseket. Összes előtti lépés jelenik meg ebben a készletben található. Csak adhat hozzá parancsfájl hello elsődleges helyen, ha a VMM-kiszolgáló telepítve van.
- **Adja hozzá az Azure runbookok**– kiterjesztheti a helyreállítási terv Azure runbookok. Például tooautomate feladatokat, vagy toocreate egylépéses helyreállítási. [További információk](site-recovery-runbook-automation.md).

## <a name="add-scripts"></a>Parancsfájlok hozzáadása

Is használhatja a PowerShell-parancsfájlok a helyreállítási terv.

 - Győződjön meg arról, hogy parancsfájlok try-catch blokkok, hogy hello kivételek szabályosan kezeli.
    - Ha hello parancsfájl-kivételt, akkor leáll, és hello tevékenység sikertelenként mutatja.
    - Ha hiba lép fel, minden fennmaradó részét hello parancsprogram nem fut.
    - Ha hiba lép fel, ha futtatja a nem tervezett feladatátvételt, hello helyreállítási terv továbbra is.
    - Ha hiba történik, amikor a tervezett feladatátvételt, leállítja az hello helyreállítási terv. Szüksége toofix hello parancsfájl, ellenőrizze, hogy a várt módon fut, és futtassa újra a hello helyreállítási terv.
- hello Write-Host parancs egy helyreállítási tervhez tartozó parancsfájl nem működik, és hello parancsfájl futtatása sikertelen lesz. egy proxyparancsfájl pedig a fő parancsfájlt futtató toocreate kimenetét, hozzon létre. Győződjön meg arról, hogy az összes kimeneti adatcsatornán-e kimenő hello segítségével >> parancsot.
  * hello parancsfájlt időtúllépés történik, ha azt nem ad visszatérési 600 másodpercen belül.
  * Ha bármi tooSTDERR bekerül, hello parancsfájl besorolásának sikertelen. Ez az információ hello parancsfájl végrehajtásának részletes adatai jelennek meg.

A VMM a központi telepítésben használata:

* A helyreállítási terv parancsprogramokat a VMM szolgáltatás fiókja hello hello környezetében futnak. Győződjön meg arról, hogy ez a fiók rendelkezik olvasási engedéllyel a hello távoli megosztás melyik hello parancsfájl helyezkedik. A VMM szolgáltatás fiókja jogosultsági szint hello hello parancsfájl toorun tesztelése.
* VMM-parancsmagok a Windows PowerShell-moduljának érkeznek. hello modul telepítve van a VMM-konzol hello telepítésekor. A parancsfájl, a következő parancs a hello parancsfájl hello betölthető:
   - Import-Module-Name virtualmachinemanager. [További információk](https://technet.microsoft.com/library/hh875013.aspx).
* Gondoskodjon legalább egy erőforrástár-kiszolgáló a VMM-környezetben. Alapértelmezés szerint a VMM-kiszolgáló hello könyvtármegosztásának elérési helyezkedik helyileg a VMM-kiszolgáló, a hello mappanévre MSCVMMLibrary hello.
    * Ha a Könyvtármegosztás elérési útjára távoli (vagy helyi azonban nem megosztott a MSCVMMLibrary), állítsa be az hello megosztás következőképpen (használatával \\példaként libserver2.contoso.com\share\):
      * Nyissa meg a Beállításszerkesztőt hello, és keresse meg a túl**HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure hely Recovery\Registration**.
      * Hello érték szerkesztése **ScriptLibraryPath** és helyezze el, mint a \\libserver2.contoso.com\share\. Adja meg hello teljes Tartománynevet. Engedélyek toohello megosztásnak a helyét adja meg.
      * Győződjön meg arról, hogy tesztelje hello parancsfájl egy felhasználói fiókkal, amely rendelkezik hello azonos engedélyek, hello a VMM szolgáltatás fiókja. Ellenőrzi, hogy önálló tesztelt parancsfájlok futtatásához hello ugyanaz, mint ahogyan a helyreállítási terv fognak. Hello VMM-kiszolgálón az alábbiak szerint állíthatja hello végrehajtási házirend toobypass:
        * Nyissa meg a hello 64 bites Windows PowerShell-konzolt emelt szintű jogosultságokkal.
        * Típus: **Set-executionpolicy áthidaló**. [További információk](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="add-a-script-or-manual-action-tooa-plan"></a>Hozzáad egy parancsfájl vagy a manuális műveletet tooa terv

Egy parancsfájl toohello alapértelmezett helyreállítási terv csoportot hozzáadni a virtuális gépek vagy a replikációs csoportok tooit, és hello-csomag létrehozása után is hozzáadhat.

1. Nyissa meg hello helyreállítási terv.
2. Hello elemeire **lépés** listára, majd **parancsfájl** vagy **manuális művelet**.
3. Adja meg, hogy toowant tooadd hello parancsfájl vagy a művelet előtt vagy után hello kijelölt elem. Használjon hello **fel** és **le** gombok toomove hello pozíciója hello parancsfájl felfelé vagy lefelé.
4. Ha egy új VMM, válassza ki a **feladatátvételi tooVMM parancsfájl**. A **parancsfájl elérési útján**, típus hello relatív elérési út toohello megosztást. Hello VMM az alábbi példában a hello elérési utat adott meg: **\RPScripts\RPScript.PS1**.
5. Ha hozzáad egy Azure Automation szolgáltatásbeli könyv futtassa, adja meg, melyik hello runbook hello található, és válassza ki a megfelelő Azure-forgatókönyvek parancsfájl hello Azure Automation-fiók.
6. A helyreállítási tervben hello feladatátvétellel, toomake meg arról, hogy hello parancsfájl megfelelően működik-e.


### <a name="add-a-vmm-script"></a>A VMM-parancsfájl hozzáadása

Ha egy VMM forráshelyet, hozzon létre egy parancsfájlt hello VMM-kiszolgálón, és adja hozzá a helyreállítási tervben.

1. Hello könyvtármegosztáson hozzon létre egy új mappát. Például \<VMMServerName > \MSSCVMMLibrary\RPScripts. Helyezze el hello forrás és cél VMM-kiszolgálók.
2. Hello parancsfájlt (például RPScript), és ellenőrizze, hogy megfelelően működik-e.
3. Hello parancsfájl hello helyen helyezze el \<VMMServerName > \MSSCVMMLibrary hello forrás és cél VMM-kiszolgálókon.


## <a name="next-steps"></a>Következő lépések

[További](site-recovery-failover.md) feladatátvételek futtatásával kapcsolatos.
