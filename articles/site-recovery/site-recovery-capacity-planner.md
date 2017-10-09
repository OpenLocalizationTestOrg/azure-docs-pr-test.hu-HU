---
title: "az Azure-ban aaaEstimate replikációs kapacitás |} Microsoft Docs"
description: "Ez a cikk tooestimate kapacitás használja, ha az Azure Site Recovery replikálása"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: 54d10e50dd4fc1b875273c7fc0f38f0e85dadddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Virtuális gépek és fizikai kiszolgálók az Azure Site Recovery védelmének kapacitásának megtervezése

hello Azure Site Recovery Capacity Planner eszköz segítségével, a kapacitásigények toofigure Hyper-V virtuális gépek, a VMware virtuális gépek és a windowsos/Linuxos fizikai kiszolgálók, az Azure Site Recovery replikálása esetén.

Hello Site Recovery Capacity Planner tooanalyze használja a forráskörnyezetét és a munkaterhelések, a sávszélesség kell és a kiszolgáló erőforrásainak hello forráshely lesz szüksége, és a hello (virtuális gép és tárterület stb), szükséges erőforrások hello cél hely.

Hello eszköz módok néhány futtathatja:

* **Gyors tervezési**: a hálózat- és átlagos száma virtuális gépeket, a lemezek, a tároló és a változási sebessége alapján leképezések tooget futtassa hello eszközt ebben a módban.
* **Részletes tervezési**: hello futtathatja ezt a módot, és adja meg a virtuális gép szinten minden munkaterhelés részleteit. Virtuális gép kompatibilitási elemzéséhez, és a hálózat- és leképezések beolvasása.

## <a name="before-you-start"></a>Előkészületek


1. A környezetben, beleértve a virtuális gépek, virtuális gép, lemezenkénti lemezeket információt gyűjteni.
2. Azonosítsa a replikált adatok napi adatváltozás (forgalom) sebessége. toodo ezt:

   * Ha a Hyper-V virtuális gépeket replikál, majd töltse le hello [Hyper-V kapacitástervezési eszköz](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello változási sebessége. [További](site-recovery-capacity-planning-for-hyper-v-replication.md) erről az eszközről. Azt javasoljuk, hogy futtatja az eszközt egy hét toocapture keresztül átlagok.
   * Ha VMware virtuális gépeket replikál, használja a hello [Azure Site Recovery telepítési Planner](./site-recovery-deployment-planner.md) kimenő hello toofigure kavarog sebessége.
   * Ha fizikai kiszolgálókat replikál, manuálisan kell tooestimate.

## <a name="run-hello-quick-planner"></a>Hello gyors Planner futtatása
1. Töltse le és nyissa meg a hello [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) eszköz. Szüksége toorun makrók, ezért select tooenable szerkesztése és engedélyezése tartalom, amikor a rendszer kéri.
2. A **planner-típus kiválasztása** válasszon **gyors Planner** hello legördülő listából.

   ![Bevezetés](./media/site-recovery-capacity-planner/getting-started.png)
3. A hello **Capacity Planner** munkalap, adja meg a hello szükséges adatokat. Meg kell adnia az alábbi képernyőképen hello piros körben összes hello mezőt.

   * A **válassza ki a forgatókönyv**, válassza a **Hyper-V tooAzure** vagy **VMware vagy fizikai tooAzure**.
   * A **átlagos napi módosulása aránya (%)**, hello adatokat gyűjtse össze a hello használata be [Hyper-V kapacitástervezési eszköz](site-recovery-capacity-planning-for-hyper-v-replication.md) vagy hello [Azure Site Recovery telepítési Planner](./site-recovery-deployment-planner.md).  
   * **Tömörítés** csak a VMware virtuális gépek vagy fizikai kiszolgálók tooAzure replikálása esetén felajánlott toocompression vonatkozik. Azt becsléséhez 30, de hello beállítás szükség szerint módosítható. A Hyper-V virtuális gépek tooAzure tömörítési replikálására, egy külső készülék például Riverbed is használhatja.
   * A **megőrzési bemenetek**, adja meg, hogy mennyi ideig kell megtartani replikák. Ha VMware vagy fizikai kiszolgálókat replikál, adjon meg hello érték nap múlva. Ha a Hyper-V-bA replikál, adja meg hello idő órában.
   * A **kell végeznie a kezdeti replikálást melyik hello köteg a virtuális gépek órák száma** és **számú virtuális gép kezdeti replikációs kötegenként**, beállítások, amelyek megadott toocompute kezdeti replikációs követelményeket.  A Site Recovery telepítésekor hello teljes kezdeti adatkészlet fel kell tölteni.

   ![Bemenetek](./media/site-recovery-capacity-planner/inputs.png)
4. Miután hello forráskörnyezettel hello értékeinek helyezett megjelenített kimeneti tartalmazza:

   * **Változásreplikálás szükséges sávszélesség** (MB/s). Hálózati sávszélesség a különbözeti replikáció hello átlagos napi adatváltozási sebesség számolja ki.
   * **A kezdeti replikáláshoz szükséges sávszélesség** (MB/s). Hálózati sávszélesség a kezdeti replikálás a helyezett hello kezdeti replikálás értékek kiszámítása.
   * **Minimális (GB) tárolási** hello teljes az Azure storage szükség van.
   * **Teljes IOPS szabványos tárfiókokban** van 8 K IOPS egység méretét a teljes standard tárfiókok hello alapján számítja ki.  A gyors Planner hello hello száma alapján van kiszámítva összes hello forrás virtuális gépek lemezét és napi adatváltozási sebesség. A hello részletes Planner, hello száma, amelyek csatlakoztatott toostandard Azure virtuális gépeken futó virtuális gépek számított alapján teljes száma, és adatok módosítása és a virtuális gépek.
   * **Standard szintű storage-fiókok száma** hello száma standard szintű storage-fiókok szükséges tooprotect hello virtuális gépeket biztosít. Egy standard szintű tárfiókot is tartsa be too20000 IOPS egy standard tárolási hello virtuális gépeinek között, és legfeljebb 500 iops-érték lemezenként támogatott.
   * **A blob szükséges lemezek számát** által biztosított hello az Azure storage a létrehozott lemezek számát.
   * **Prémium szintű storage-fiókok szükséges száma** biztosítja a prémium szintű storage szükséges fiókok tooprotect hello száma hello virtuális gépeket. A forrás virtuális gép magas iops-érték (több mint 20000) és a prémium szintű tárfiók van szüksége. A prémium szintű storage-fiók mentése too80000 IOPS tárolására képes.
   * **A prémium szintű storage IOPS teljes** van 256 KB-os IOPS egység méretét a hello teljes prémium szintű storage-fiókok alapján számítja ki.  A gyors Planner hello hello száma alapján van kiszámítva összes hello forrás virtuális gépek lemezét és napi adatváltozási sebesség. A hello részletes Planner, hello száma alapján számított hello adatgyűjtésre beállított virtuális gépeket, amelyek csatlakoztatott toopremium Azure virtuális gép (DS és GS-sorozat), és hello adatok módosítása a virtuális gépek és a.
   * **Szükséges konfigurációs kiszolgálók száma** mutat be, hány konfigurációs kiszolgáló hello telepítéséhez szükséges.
   * **További folyamat szükséges kiszolgálók száma** mutat be, hogy további folyamat kiszolgálók által megkövetelt, továbbá toohello folyamatot futtató kiszolgáló hello konfigurációs kiszolgálón alapértelmezés szerint.
   * **További tárhely 100 %-os hello forrás** jeleníti meg, hogy a további tárhely szükséges-e hello forráshelyeként.

   ![Kimenet](./media/site-recovery-capacity-planner/output.png)

## <a name="run-hello-detailed-planner"></a>Futtassa a részletes Planner hello

1. Töltse le és nyissa meg a hello [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) eszköz. Szüksége toorun makrók, ezért select tooenable szerkesztése és engedélyezése tartalom, amikor a rendszer kéri.
2. A **planner-típus kiválasztása**, jelölje be **részletes Planner** hello legördülő listából.

   ![Első lépések](./media/site-recovery-capacity-planner/getting-started-2.png)
3. A hello **munkaterhelés minősítési** munkalap, adja meg a hello szükséges adatokat. Meg kell adnia minden hello mezők megjelölve.

   * A **Processzormagok**, adja meg a hello magok teljes száma a forráskiszolgálón.
   * A **MB-ban a memóriafoglalás**, adja meg a forráskiszolgáló hello RAM méretét.
   * Hello **hálózati adapterek száma**, hello hálózati adapterek számát adja meg a forráskiszolgálón.
   * A **tárhely (GB) a teljes**, adja meg a teljes méretét hello hello Virtuálisgép-tároló. Például ha hello forráskiszolgálóra 500 GB 3 lemezt, majd teljes tárterület mérete 1500 GB.
   * A **csatlakoztatott lemezek számát**, adja meg a forráskiszolgáló lemezeinek hello teljes száma.
   * A **lemez a tárolókapacitás kihasználtságát**, adja meg a hello átlagos kihasználtság.
   * A **napi adatváltozási sebessége (%)**, adja meg a hello napi adatok módosítása a forráskiszolgálón aránya.
   * A **Azure leképezési méret**, adja meg, hogy szeretné-e toomap hello Azure Virtuálisgép-méretet. Nem toodo ebben manuálisan, kattintson a **számítási IaaS virtuális gépeket**. Adjon meg egy manuális beállítást, és kattintson a számítási IaaS virtuális gépeket, ha hello kézi beállítás lehet, hogy frissíthetők, mert hello számítási folyamat automatikusan azonosítja hello Azure Virtuálisgép-méretet a legmegfelelőbb.

   ![Munkaterhelés minősítés](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Ha **számítási IaaS virtuális gépeket** Itt a hatása van:

   * Hello kötelező bemeneti ellenőrzi.
   * Kiszámítja az IOPS, és javaslatot tesz hello legjobb Azure virtuális gép aize egyezés minden egyes virtuális gépekhez, amely jogosult a replikációs tooAzure. Ha a megfelelő méretű Azure virtuális gép nem észlelhető hiba jelenik meg. Például ha a lemezek hello száma 65 csatolt, hiba ki mert hello legmagasabb Azure virtuális gép mérete 64.
   * A storage-fiók, amely egy Azure virtuális gép nem használható javasol.
   * Standard szintű storage-fiókok és a prémium szintű storage-fiókok hello terheléshez szükséges teljes száma hello számítja ki. Görgessen lefelé tooview hello Azure tárolási típus, és hello tárfiókot, amely a forráskiszolgáló is használható.
   * Befejeződött, és hello rest hello tábla szükséges tárolás (standard vagy prémium) társított típusból egy VM-et és a csatlakoztatott lemezek hello száma alapján rendezi. Virtuális gép esetében, amelyek megfelelnek az Azure-hello követelményeknek, hello oszlop **van VM minősíteni?** látható **Igen**. Ha egy virtuális gép nem készíthető biztonsági másolat tooAzure, egy hibaüzenet jelenik meg.

Oszlopok AA tooAE kimenete, és adja meg az egyes virtuális gépek információkat.

![Munkaterhelés minősítés](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Példa
Tegyük fel, hat virtuális gépek hello táblázatban hello értékekkel a hello eszköz számítja ki, és hozzárendeli a hello Azure virtuális Géphez legmegfelelőbb és hello Azure tárolási követelményeinek.

![Munkaterhelés minősítés](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* A példa a kimenetre hello vegye figyelembe a hello következőket:

  * hello első oszlop hello virtuális gépeket, a lemezek és a forgalom érvényesítési oszlop.
  * Öt virtuális gépek két szabványos storage-fiókok és egy prémium szintű tárfiók van szükség.
  * VM3 védelmét, mert egy vagy több lemez 1 Terabájtnál nem jogosultak.
  * VM1 és vm2 virtuális gépnek is hello első normál tárfiókot használni
  * VM4 hello második szabványos tárolási fiókot használhatják.
  * VM5 és VM6 van szüksége a prémium szintű tárfiók, és használhatja is ugyanazt a fiókot.

    > [!NOTE]
    > A standard és prémium szintű storage IOPS kiszámítása hello VM szintjét, illetve nem szintet. Egy szabványos virtuális gép mentése too500 iops-érték lemezenként képes kezelni. Ha egy lemez IOPS még nagyobb, mint 500, prémium szintű storage kell. Azonban ha IOPS egy lemez legfeljebb 500, de IOPS hello teljes virtuális gépek lemezei: hello belül támogatja a szabványos Azure virtuális gép korlátok (Virtuálisgép-méretet, lemezek, adapterek, Processzor, memória száma száma), majd hello planner választja ki egy szabványos virtuális Gépet, és nem hello DS vagy a GS adatsorozat. Toomanually frissítés hello leképezési Azure mérete cella adatsorozattal megfelelő DS vagy a GS VM van szüksége.


Miután összes hello részletes érvényben vannak, kattintson **Submit toohello planner eszköz** tooopen hello **Capacity Planner** és szolgáltatások kiemelt, tooshow, hogy fontosságúak védelemre jogosult-e.

### <a name="submit-data-in-hello-capacity-planner"></a>A Capacity Planner hello adatok küldése
1. Amikor megnyitja hello **Capacity Planner** munkalap a telepítéskor megadott hello-beállítások alapján. hello megjelenik "Munkaterhelési" word hello **Infra bemeneti forrás** cella, tooshow, amely a bemeneti hello hello **munkaterhelés minősítési** munkalap.
2. Azt szeretné, hogy toomake módosításokat, ha szüksége van-e toomodify hello **munkaterhelés minősítési** munkalap, majd kattintson **Submit toohello planner eszköz** újra.  

   ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)
