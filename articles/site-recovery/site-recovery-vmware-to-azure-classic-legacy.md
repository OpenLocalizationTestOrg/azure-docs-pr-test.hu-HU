---
title: "aaaReplicate VMware virtuális gépek és fizikai kiszolgálók tooAzure (klasszikus örökölt) |} Microsoft Docs"
description: "Ismerteti, hogyan tooreplicate a helyszíni virtuális gépek és a windowsos/Linuxos fizikai kiszolgálók tooAzure Azure Site Recovery használatát a korábbi központi telepítés hello a klasszikus portálon."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: f980fb52-8ec2-4dd9-85e2-8e52e449f1ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: raynew
ms.openlocfilehash: 64c6d906d54426fdd2b884350542a4562bb12528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-tooazure-with-azure-site-recovery-using-hello-classic-portal-legacy"></a>VMware virtuális gépek és fizikai kiszolgálók tooAzure replikálása az Azure Site Recovery a klasszikus portál hello (örökölt) használatával
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Klasszikus portál](site-recovery-vmware-to-azure-classic.md)
> * [Klasszikus portál (örökölt)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Üdvözli a Site Recovery tooAzure! Ez a cikk ismerteti a korábbi központi telepítés replikálásához a helyszíni VMware virtuális gépeket vagy windowsos/Linuxos fizikai kiszolgálók tooAzure hello klasszikus portál Azure Site Recovery segítségével.

## <a name="overview"></a>Áttekintés
A szervezetek kell, amely meghatározza, hogyan alkalmazások, munkaterhelések és az adatok üzemben maradni a rendelkezésre álló tervezett és nem tervezett leállások során (BCDR) stratégiára, és toonormal működésre minél hamarabb helyreállítani. Fontos, hogy a BCDR stratégia gondoskodjon a vállalati adatok biztonságáról és helyreállíthatóságáról, és lehetővé tegye, hogy a számítási feladatok még vészhelyzet esetén se álljanak le.

Webhely-helyreállítás egy Azure szolgáltatása tooyour BCDR stratégia megvalósításában replikáció a helyszíni fizikai kiszolgálóknak és virtuális gépek toohello Azure-felhőbe vagy tooa másodlagos adatközpontba. Az elsődleges helyen valamilyen okból kimaradás lép fel, ha feladatátvételt toohello másodlagos helyre tookeep alkalmazások és számítási feladatok nem állnak. Ön nem hátsó tooyour elsődleges hely toonormal műveleteket a rendszer visszaadja. További információk: [What is Azure Site Recovery?](site-recovery-overview.md) (Mire használható az Azure Site Recovery szolgáltatás?)

> [!WARNING]
> Ez a cikk **örökölt utasításokat**. Ne használjon új központi telepítéséhez. Ehelyett [kövesse ezeket az utasításokat](site-recovery-vmware-to-azure.md) hello Azure-portálon, a Site Recovery toodeploy vagy [kövesse ezeket az utasításokat](site-recovery-vmware-to-azure-classic.md) tooconfigure hello továbbfejlesztett üzembe helyezési modellt hello a klasszikus portálon. Ha már telepítette az ebben a cikkben leírt hello metódussal programot, azt javasoljuk, hogy az áttelepített fokozott toohello telepítési hello a klasszikus portálon.
>
>

## <a name="migrate-toohello-enhanced-deployment"></a>Továbbfejlesztett toohello telepítési áttelepítése
Ebben a szakaszban csak akkor szükséges, ha korábban már telepített hello utasításokat követve ebben a cikkben a Site Recovery.

toomigrate a jelenlegi telepítés lesz szüksége:

1. A helyszíni hely új Site Recovery-összetevők telepítéséhez.
2. Adja meg hitelesítő adatokat a VMware virtuális gépek automatikus felderítés hello új konfigurációs kiszolgálón.
3. Fedezze fel hello VMware Server hello új konfigurációs kiszolgálóval.
4. Hozzon létre egy új védelmi csoport hello új konfigurációs kiszolgáló.

Előkészületek:

* Azt javasoljuk, hogy beállította a karbantartási időszak megadása az áttelepítési.
* Hello **gépek áttelepítése** lehetőség áll rendelkezésre, csak akkor, ha van olyan védelmi csoportnál, amely a korábbi központi telepítés során lettek létrehozva.
* Hello áttelepítési lépések elvégzése után is igénybe vehet 15 perc vagy hosszabb toorefresh hello hitelesítő adatokat, és toodiscover, és frissítse a virtuális gépeket, hogy később is hozzáadhatja tooa védelmi csoportot. Frissítheti manuálisan várakozási helyett.

Telepítse át a következőképpen:

1. További információ a [továbbfejlesztett üzembe helyezési modellt a klasszikus portálon hello](site-recovery-vmware-to-azure-classic.md). Felülvizsgálati hello fokozott [architektúra](site-recovery-vmware-to-azure-classic.md), és [Előfeltételek](site-recovery-vmware-to-azure-classic.md).
2. Távolítsa el a hello mobilitási szolgáltatás jelenleg replikál gépekről. Hello szolgáltatás egy új verziója települ hello gépeken, amikor új védelmi csoport toohello hozzáadja őket.
3. Töltse le a [tárolóbeli regisztrációs kulcs](site-recovery-vmware-to-azure-classic.md) és [hello egyesített telepítése varázsló futtatása](site-recovery-vmware-to-azure-classic.md) tooinstall hello konfigurációs kiszolgáló, a folyamatkiszolgáló és fő cél a kiszolgáló-összetevőit. Tudjon meg többet az [kapacitástervezés](site-recovery-vmware-to-azure-classic.md).
4. [Hitelesítő adatok beállítása](site-recovery-vmware-to-azure-classic.md) a Site Recovery segítségével tooaccess VMware server tooautomatically VMware virtuális gépek felderítése. További tudnivalók [szükséges engedélyek](site-recovery-vmware-to-azure-classic.md).
5. Adja hozzá [vCenter kiszolgáló vagy vSphere-gazdagép](site-recovery-vmware-to-azure-classic.md). Kiszolgálók tooappear további 15 perc hello Site Recovery portálon is igénybe vehet.
6. Hozzon létre egy [új védelmi csoport](site-recovery-vmware-to-azure-classic.md). Hello portál toorefresh too15 percet, hogy a virtuális gépek hello felderített és jelennek meg is eltarthat. Ha nem szeretné, hogy toowait kiemelheti hello felügyeleti kiszolgáló neve (ne kattintson rá) > **frissítése**.
7. Az új védelmi csoport hello kattintson **gépek áttelepítése**.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)
8. A **válasszon gépek** szeretné, hogy a toomigrate, és azt szeretné, hogy toomigrate gépek hello válassza hello védelmi csoportot.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)
9. A **cél beállításainak konfigurálása** adja meg, hogy toouse hello ugyanazokat a beállításokat az összes gépek és hello válassza a folyamatkiszolgáló és az Azure storage-fiók. Ha nem rendelkezik különálló folyamatkiszolgálót ez lesz hello hello IP-cím hello konfigurációs kiszolgáló.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [A tárfiókok áttelepítési](../resource-group-move-resources.md) erőforrás csoportok belül hello ugyanahhoz az előfizetéshez vagy előfizetések között nem támogatott a Site Recovery telepítéséhez használt tárfiókok.

1. A **fiókokat adja meg**, válassza ki a hello folyamat server tooaccess hello gép toopush hello új szolgáltatásverzió hello létrehozott hello fiókot.

   ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)
2. A Site Recovery a replikált adatok toohello Azure storage-fiók, amely a megadott kell áttelepítenie. Opcionálisan hello örökölt környezetben használt hello tárfiók is felhasználhatja.
3. Hello feladat után befejeződik a virtuális gépek automatikusan szinkronizálja. Szinkronizálás befejeződése után törölheti a virtuális gépek hello hello örökölt védelmi csoportból.
4. Minden gépek áttelepítése után törölheti a hello örökölt védelmi csoportot.
5. Ne feledje toospecify hello feladatátvételi tulajdonságok gépekhez, és hello Azure-hálózat beállításait a szinkronizálás befejeződése után.
6. Ha rendelkezik meglévő helyreállítási tervek, telepíthet át őket fokozott toohello-telepítéshez olyan hello **helyreállítási terv áttelepítése** lehetőséget. Csak akkor tegye ezt, miután áttelepítette az összes védett gépek.

   ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

> [!NOTE]
> Áttelepítés befejezése után folytassa a hello [továbbfejlesztett cikk](site-recovery-vmware-to-azure-classic.md). örökölt cikkben hello többi nem lesz megfelelő, és minden toofollow nem kell további ismertetett informatikai ** hello lépésből áll.
>
>

## <a name="what-do-i-need"></a>Mit kell?
Hello telepítési összetevők látható.

![Új tároló](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

A következőkre lesz szüksége:

| **Összetevő** | **Üzembe helyezés** | **Részletek** |
| --- | --- | --- |
| **Konfigurációs kiszolgáló** |Egy Azure standard A3 virtuális gép hello Site Recovery tárolóként ugyanazt az előfizetést. |hello konfigurációs kiszolgáló a védett gépek hello folyamatkiszolgáló és fő célkiszolgálóra, az Azure közötti kommunikáció koordinálását. Létrehozza replikáció és az Azure-ban koordináták helyreállítási feladatátvétel esetén. |
| **Fő célkiszolgáló** |Azure virtuális gépként – vagy egy Windows Serveren egy Windows Server 2012 R2 gyűjtemény rendszerképre (tooprotect Windows gépek) vagy a Linux-kiszolgálóként a OpenLogic CentOS 6.6 gyűjtemény lemezkép (tooprotect Linux-gépek) alapján.<br/><br/> Három méretezési lehetőségek érhetők el – szabványos A4, a Standard D14 és a szabványos DS4.<br/><br/> hello kiszolgáló csatlakoztatott toohello és hello konfigurációs kiszolgáló ugyanahhoz az Azure-hálózathoz.<br/><br/> A Site Recovery portálon hello beállítása |Fogadása és megőrzi a replikált adatokat az Azure-tárfiók a blob-tároló létre csatolt VHD-k használata a védett gépek.<br/><br/> Válassza ki a szabványos DS4 kifejezetten a konzisztens magas teljesítménye és kis késleltetése prémium szintű Storage-fiók használatával igénylő munkaterhelések védelmének beállítása. |
| **Folyamatkiszolgáló** |Egy Windows Server 2012 R2 rendszert futtató helyszíni virtuális vagy fizikai kiszolgáló<br/><br/> Javasoljuk, hogy azt helyezte el a hello azonos hálózati és a LAN-szegmens tooprotect kívánt, de mindaddig, amíg védett gépek 3. van egy másik hálózati futtatható hello gépekként hálózati látható tooit.<br/><br/> Állítsa be, és regisztrálhatja azt hello Site Recovery portálon toohello konfigurációs kiszolgáló. |Védett gépek elküldeni a replikációs adatok toohello helyszíni folyamatkiszolgáló. A lemezalapú gyorsítótárban toocache replikációs adatok fogadott rendelkezik. Az adatok több műveletet hajtja végre.<br/><br/> Optimalizálja a gyorsítótárazás, tömörítés és titkosítás toohello fő célkiszolgáló elküldés előtt adatait.<br/><br/> Kezelési hello mobilitási szolgáltatás leküldéses telepítéséhez.<br/><br/> A VMware virtuális gépek automatikus felderítés hajtja végre. |
| **A helyszíni gépeket** |Helyszíni VMware virtuális gépeket vagy windowsos vagy Linuxos fizikai kiszolgálók. |Egy vagy több különböző replikációs beállítások konfigurálása Akkor sikertelen lehet gyakrabban, vagy egy egyedi számítógép keresztül több számítógép, amely akkor összegyűjti a helyreállítási tervbe. |
| **Mobilitási szolgáltatás** |Minden virtuális gép vagy fizikai kiszolgálóra telepíteni szeretné a tooprotect<br/><br/> Manuálisan telepíteni vagy leküldött, és automatikusan telepíti hello folyamatkiszolgáló egyes gépek replikálásának engedélyezése esetén. |hello mobilitási szolgáltatás adatok toohello folyamatkiszolgáló elküldi a kezdeti replikálás (újraszinkronizálási) során. Miután hello gép védett állapotban van, (végeztével újraszinkronizálási) hello mobilitási szolgáltatás írások toodisk memórián belüli rögzíti és azokat elküldi őket toohello folyamatkiszolgáló. Windows-kiszolgálók Applicationconsistency érhető el, használja a VSS-t. |
| **Az Azure Site Recovery tárolójában.** |A Site Recovery-tároló létrehozása Azure-előfizetés, és regisztrálja a kiszolgálókat hello tárolóban lévő állapottal. |hello tároló koordinálja és koordinálja a replikálása, feladatátvételének és a helyszíni hely és az Azure közötti helyreállítás. |
| **Replikációs eljárás** |**Hello interneten keresztül**– védett helyszíni kiszolgálók tooAzure protokollt használó biztonságos SSL/TLS-csatorna kommunikál és követően replikálja az adatokat hello internet. Ez a lehetőség hello alapértelmezett.<br/><br/> **VPN/ExpressRoute**– egy VPN-kapcsolaton keresztül a helyszíni kiszolgálók és az Azure közötti kommunikál és a követően replikálja az adatokat. A telephelyek közötti VPN vagy a helyszíni hely és a hálózat az Azure között ExpressRoute kapcsolat tooset lesz szüksége.<br/><br/> Válasszon ki a Site Recovery üzembe helyezése során tooreplicate módját. Hello mechanizmus a meglévő gépek replikálását befolyásolása nélkül konfigurálását követően nem módosíthatja. |Sem a beállítás megköveteli az Ön tooopen minden bejövő hálózati portok a védett gépek. Minden hálózati kommunikációt kezdeményezni hello helyszíni hely. |

## <a name="capacity-planning"></a>Kapacitástervezés
hello fő területet tooconsider lesz szüksége:

* **Forráskörnyezettel**– hello VMware infrastruktúra, a gép adatforrás-beállítások és a követelmények.
* **Összetevők kiszolgálóin**– hello folyamatkiszolgáló, a konfigurációs kiszolgáló és a fő célkiszolgáló

### <a name="considerations-for-hello-source-environment"></a>Hello forráskörnyezettel kapcsolatos megfontolások
* **Lemez maximális mérete**– hello aktuális maximális, amely virtuális géphez csatolt tooa hello lemez mérete 1 TB. Így hello maximális replikálható forrás lemez mérete is korlátozott too1 TB.
* **Maximális méret forrásonként**– hello maximális egyetlen forrásgép mérete 31 TB (31 lemezzel) és fő célkiszolgáló hello kiépítve D14 példánnyal.
* **Egy fő kiszolgálón források száma**– több forrásgépek védelme a fő célkiszolgálón. Azonban egyetlen forrásgép nem lehet védetté tenni több fő kiszolgálón, mert a lemezek replikálása, mert egy virtuális merevlemez, amely tükrözi a hello lemez mérete hello Azure blob storage létrehozza, és csatlakoztatott lemez toohello fő kiszolgáló.  
* **Legfeljebb napi adatváltozási sebesség forrásonként**– nincsenek a három tényezőt kell figyelembe venni, amikor annak eldöntéséhez, hogy a hello ajánlott toobe adatváltozási sebessége forrásonként. Hello alapú cél szempontokról két IOPS hello céllemezt minden művelethez hello forrás van szükség. Ennek az az oka a régi adatok olvasási és írási hello új adatok hello céllemezt fog történni.
  * **Naponta módosítani hello folyamatkiszolgáló által támogatott**– A forrásgép nem terjedhetnek ki több folyamat kiszolgáló. A folyamat egyetlen kiszolgálón mentést napi adatváltozási sebesség too1 TB-os használható. Ezért az 1 TB-os hello maximális napi adatok módosítása a forrásgép támogatott.
  * **Hello céllemezt által támogatott maximális átviteli sebesség**– maximális forgalom forrás lemezenként nem lehet több mint 144 GB/nap (a 8 KB-os írási méret). Hello fő szakaszban hello átviteli sebesség és a különböző írási méretű hello cél IOPs hello táblázatban találja. Ez a szám minden forrás IOP hello cél lemezen 2 IOPS állít elő, mert két kell osztani. További információ a [Azure méretezhetőségi és Teljesítménycélok](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) hello célként prémium szintű storage-fiókok konfigurálása során.
  * **Hello tárfiók által támogatott maximális átviteli sebesség**– A forrás nem terjedhetnek ki több tárfiókot. Adott meg, hogy a tárfiók legfeljebb 20 000 kérelmek / másodperc vesz igénybe, és, hogy minden forrás IOP hoz létre a fő célkiszolgáló hello 2 IOPS, ajánlott mindig hello száma IOPS hello forrás too10 000 között. További információ a [Azure méretezhetőségi és Teljesítménycélok](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) hello forrás prémium szintű storage-fiókok konfigurálása során.

### <a name="considerations-for-component-servers"></a>Összetevők kiszolgálóin szempontjai
1. táblázat foglalja össze a virtuálisgép-méretek hello hello konfigurációs és fő célkiszolgálóra.

| **Összetevő** | **Telepített Azure-példányokon** | **Magok** | **Memória** | **Maximális lemezek** | **Lemez mérete** |
| --- | --- | --- | --- | --- | --- |
| Konfigurációs kiszolgáló |Standard a3 csomag |4 |7 GB |8 |1023 GB |
| Fő célkiszolgáló |Standard A4 |8 |14 GB |16 |1023 GB |
| Standard D14 |16 |112 GB |32 |1023 GB | |
| Standard DS4 |8 |28 GB |16 |1023 GB | |

**1. táblázat**

#### <a name="process-server-considerations"></a>Folyamatok server szempontjai
Általában folyamat méretezése függ hello napi adatváltozási sebesség minden védett munkaterhelések között.

* Elegendő számítási tooperform feladatokat, mint a beágyazott tömörítés és a titkosítás van szüksége.
* hello folyamatkiszolgáló lemezes gyorsítótárát használja. Győződjön meg arról, hogy hello gyorsítótár-terület ajánlott és a lemez adatátviteli sebessége elérhető toofacilitate hello adatváltozásokat tárolja a hálózati szűk vagy leállás hello esemény.
* Győződjön meg arról elegendő sávszélesség, így a hello folyamatkiszolgáló feltöltheti hello adatok toohello fő server tooprovide folyamatos védelmét.

2. táblázat összefoglalja a hello folyamat server irányelveket.

| **Adatváltozási sebesség** | **PROCESSZOR** | **Memória** | **Gyorsítótár-lemez mérete** | **Gyorsítótár lemez adatátviteli sebessége** | **Sávszélesség belépés és kilépés** |
| --- | --- | --- | --- | --- | --- |
| < 300 GB |4 Vcpu (2 sockets * @ 2.5 GHz-es 2 mag) |4 GB |600 GB |7 too10 MB / s |30 MB/s/21 MB/s |
| 300 too600 GB |8 Vcpu (2 sockets * @ 2.5 GHz, 4 mag) |6 GB |600 GB |11 too15 MB / s |60 MB/s/42 MB/s |
| 600 GB too1 TB |12 Vcpu (2 sockets * @ 2.5 GHz-es 6 mag) |8 GB |600 GB |16 too20 MB / s |100 MB/s/70 MB/s |
| > 1 TB-OS |Egy másik folyamat-kiszolgáló központi telepítése | | | | |

**2. táblázat**

Az elemek magyarázata:

* Érkező letöltési sávszélesség (intranetes hello forrás- és folyamat-kiszolgáló között).
* Kimenő forgalom feltöltés sávszélesség (internet hello folyamatkiszolgáló és fő célkiszolgáló közötti). Kimenő forgalom számok feltételezik, átlagos 30 % folyamat server tömörítést.
* Gyorsítótár lemez egy külön operációsrendszer-lemez minimális 128 GB ajánlott az összes folyamat.
* A gyorsítótár lemez átviteli hello a következő tárolási használt teljesítménymérésre: 10 KB-os RPM RAID 10 konfigurációval 8 SAS-meghajtókat.

#### <a name="configuration-server-considerations"></a>Konfigurációs kiszolgáló kapcsolatos szempontok
Minden egyes konfigurációs kiszolgáló 3 – 4 kötetekkel too100 forrás gépek is támogatja. Ha a központi telepítés nagyobb ajánlott telepít egy másik konfigurációs kiszolgálón. Tekintse meg az 1. táblázat a hello alapértelmezett virtuálisgép-tulajdonságokat hello konfigurációs kiszolgáló.

#### <a name="master-target-server-and-storage-account-considerations"></a>A fő célkiszolgáló kiszolgálóra és tárhelyre fiókokkal kapcsolatos megfontolások
hello tárolási fő kiszolgálónként egy operációsrendszer-lemez, egy adatmegőrzési kötet, és adatlemezek tartalmazza. hello adatmegőrzési meghajtó hello lapjának lemezen változások az hello megőrzési időszak hello Site Recovery portálon definiált hello időtartama kezeli.  Tekintse meg a tooTable 1 hello virtuális gép tulajdonságait a fő célkiszolgáló hello. 3. táblázat bemutatja a hello a4 méretű lemezek használatának módját.

| **Példány** | **Az operációsrendszer-lemez** | **Megőrzés** | **Adatlemezek** |
| --- | --- | --- | --- |
| **Megőrzés** |**Adatlemezek** | | |
| Standard A4 |1 lemez (1 * 1023 GB) |1 lemez (1 * 1023 GB) |15 lemezek (15 * 1023 GB) |
| Standard D14 |1 lemez (1 * 1023 GB) |1 lemez (1 * 1023 GB) |31 lemezek (15 * 1023 GB) |
| Standard DS4 |1 lemez (1 * 1023 GB) |1 lemez (1 * 1023 GB) |15 lemezek (15 * 1023 GB) |

**3. táblázat**

Fő célkiszolgáló hello kapacitástervezését függ:

* Az Azure storage teljesítmény- és korlátozások
  * hello maximális száma magas lemezek a Standard szint virtuális gépek kezeléséhez, körülbelül 40 (20 000 500 iops-érték lemezenként) egyetlen tárfiókokban. További információ a [méretezhetőségi célok szabványos tárfiókok](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) és [prémium szintű storage-fiókok](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
* Napi adatváltozási sebesség
* Adatmegőrzési kötet tároló.

Vegye figyelembe:

* Egy adatforrás nem terjedhetnek ki több tárfiókot. Nyissa meg a védelem konfigurálásakor kijelölt toohello tárfiókok toohello adatlemez vonatkozik. az operációs rendszer és hello hello megőrzési lemezek irányulna toohello automatikusan telepíti a tárfiók.
* hello adatmegőrzési tárolási kötet szükséges hello napi adatváltozási sebesség és a megőrzési napok száma hello függ. szükséges fő célkiszolgáló adatmegőrzési tárolási hello naponta forrásból teljes adatforgalom = * megőrzési napok száma.
* Mindegyik fő célkiszolgáló rendelkezik csak egy adatmegőrzési kötet. hello lemezek csatolt toohello fő célkiszolgáló által megosztott hello adatmegőrzési kötet. Példa:
  * 5 lemezek forrás virtuális gép, és egyes lemezek hello forrás 120 IOPS (8 KB-os méret) állít elő, ha ez az eszköz too240 iops-érték lemezenként (hello céllemezt IO forrásonként 2 műveletek). 240 IOPS-érték hello Azure lemez IOPS-korlát 500 belül.
  * Hello adatmegőrzési kötet, ez lesz 120 * 5 = 600 IOPS, és ez a bottle nyak válhat. Ebben a forgatókönyvben egy működő stratégiára volna tooadd további lemezek toohello adatmegőrzési kötet és span között, akkor a RAID paritásos beállításként. Ez javítja a teljesítményt, mert több meghajtó elosztott hello iops-érték. a hozzáadott meghajtók toobe hello száma toohello adatmegőrzési kötet a következők:
    * Teljes IOPS forrás környezetből / 500
    * Forráskörnyezettel (tömörítetlen) napi teljes adatforgalom / 287 GB. 287 GB hello maximális átviteli sebesség támogatott napi tároló lemez. Ez a metrika hello írási méret és 8 KB-os függően változik, mert ebben az esetben 8 KB-os ezekkel írási méret feltételezi. Például hello írási méret esetén 4 KB-os akkor átviteli lesz 287/2. És hello írási méret esetén 16 KB-os majd átviteli 287 * 2.
* hello szükséges storage-fiókok száma = teljes source IOPs/10000.

## <a name="before-you-start"></a>Előkészületek
| **Összetevő** | **Követelmények** | **Részletek** |
| --- | --- | --- |
| **Azure-fiók** |Szüksége lesz egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. | |
| **Azure Storage** |Szüksége lesz egy Azure storage fiók toostore replikált adatok<br/><br/> Vagy hello fióknak kell lennie egy [Standard georedundáns Tárfiókot](../storage/common/storage-redundancy.md#geo-redundant-storage) vagy [prémium szintű Tárfiók](../storage/common/storage-premium-storage.md).<br/><br/> Kell a hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és hello társítható ugyanahhoz az előfizetéshez. Nem támogatjuk a hello áthelyezés hello használatával létrehozott tárfiókok [új Azure-portálon](../storage/common/storage-create-storage-account.md) erőforráscsoportok között.<br/><br/> több olvasási toolearn [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md) | |
| **Azure-beli virtuális hálózat** |Szüksége lesz egy Azure virtuális hálózatot, mely hello a konfigurációs kiszolgáló és a fő célkiszolgálón telepítve lesz. Nem lehet hello azonos előfizetésével és régiójával, hello Azure Site Recovery-tárolóban. Ha tooreplicate adatok keresztül egy ExpressRoute- vagy VPN-kapcsolat hello Azure virtuális hálózati kell csatlakoztatott tooyour a helyi hálózaton keresztül egy ExpressRoute-kapcsolatot vagy a telephelyek közötti VPN. | |
| **Azure-erőforrások** |Ellenőrizze, hogy van elegendő Azure-erőforrások toodeploy valamennyi összetevőnél. További információ: [Azure-előfizetési korlátozásait](../azure-subscription-service-limits.md). | |
| **Azure-alapú virtuális gépek** |Virtuális gépek tooprotect kívánt meg kell felelnie az [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).<br/><br/> **Lemez száma**– legfeljebb 31 lemezre támogatható egy védett kiszolgálón<br/><br/> **A lemez mérete**– önálló lemezeinek kapacitása nem 1023 GB-nál nagyobb lehet.<br/><br/> **Fürtszolgáltatás**– fürtözött kiszolgálók nem támogatottak.<br/><br/> **Rendszerindító**– Unified Extensible Firmware Interface (UEFI) / EFI típusú rendszerindítás nem használható<br/><br/> **Kötetek**– a Bitlocker titkosított kötetek nem támogatottak.<br/><br/> **Kiszolgálók nevei**– nevének 1 és 63 karakter (betűket, számokat és kötőjeleket tartalmazhat) kell tartalmaznia. hello nevét kell kezdődnie, betűvel vagy számmal, és betűvel vagy számmal végződhet. A gép védelme után hello Azure nevét módosíthatja. | |
| **Konfigurációs kiszolgáló** |Standard A3 virtuális gép Azure Site Recovery Windows Server 2012 R2-gyűjtemény lemezkép alapján létrejön az előfizetés hello konfigurációs kiszolgáló. Az első példánynál az új felhőalapú szolgáltatás hello jön létre. Ha nyilvános Internet módon hello kapcsolattípus hello konfigurációs kiszolgáló hello felhőalapú szolgáltatás nyilvános fenntartott IP-címek jön létre.<br/><br/> csak az angol karakterek hello telepítési útvonalat kell megadni. | |
| **Fő célkiszolgáló** |Azure virtuális gép, A4 standard, D14 vagy DS4.<br/><br/> csak az angol karakterek hello telepítési útvonalat kell megadni. Például hello elérési út legyen **/usr/local/ASR** a linuxos fő célkiszolgáló számára. | |
| **Folyamatkiszolgáló** |Fizikai vagy virtuális gép hello legújabb frissítésekkel rendelkező Windows Server 2012 R2 rendszerű hello folyamatkiszolgáló telepítése. Telepítse a C: /.<br/><br/> Javasoljuk, hogy hello hello server helyez el ugyanazon a hálózaton és alhálózaton hello tooprotect kívánt gépek.<br/><br/> Telepítse a VMware vSphere parancssori felülete 5.5.0 hello folyamat kiszolgálón. hello VMware vSphere parancssori felülete összetevő szükséges hello folyamatkiszolgáló rendelés toodiscover virtuális gépeken a vCenter-kiszolgálót vagy ESXi-állomáson futó virtuális gépek kezeli.<br/><br/> csak az angol karakterek hello telepítési útvonalat kell megadni.<br/><br/> ReFS fájlrendszer nem támogatott. | |
| **VMware** |A VMware vCenter-kiszolgálót a VMware vSphere hipervizorok kezeléséhez. Akkor kell futtatnia 5.1 vagy 5.5 vCenter verziója hello legújabb frissítéseit.<br/><br/> VMware virtuális gépeket tartalmazó egy vagy több vSphere hipervizorok kívánt tooprotect. hello hipervizor ESX/ESXi 5.1 vagy 5.5 verziója fut hello legújabb frissítéseit.<br/><br/> VMware virtuális gépek telepítve és fut a VMware-eszközök kell rendelkeznie. | |
| **Windows-alapú gépek** |Védett fizikai kiszolgálókon vagy VMware virtuális gépek Windows rendszerű rendelkezik a követelmények számát.<br/><br/> A támogatott 64 bites operációs rendszer: **Windows Server 2012 R2**, **Windows Server 2012**, vagy **Windows Server 2008 R2: legalább SP1**.<br/><br/> hello állomásnév, a csatlakoztatási pontok, a eszköz nevét, a Windows rendszer elérési utat (például: C:\Windows) kell lennie csak angol nyelven.<br/><br/> hello operációs rendszer C:\ meghajtóra telepíthető.<br/><br/> Csak az alapszintű lemezek támogatottak. A dinamikus lemezek nem támogatottak.<br/><br/> A védett gépek tűzfalszabályokat kell lehetővé teszik tooreach hello konfigurációs és a fő cél-kiszolgálók a Azure.p ><p>Szüksége lesz a tooprovide rendszergazdai fiókkal (a Windows hello gépen helyi rendszergazdának kell lennie) toopush hello mobilitási szolgáltatás telepítése a Windows Server kiszolgálókon. Ha hello megadott fiók nem tartományi fiók toodisable távelérési felhasználói vezérlő hello helyi gépen lesz szüksége. toodo a Hozzáadás hello LocalAccountTokenFilterPolicy DWORD beállításjegyzék bejegyzés alatt HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System 1 értékű. tooadd hello beállításjegyzékbeli bejegyzést a CLI nyissa meg a cmd vagy a PowerShell használatával, és adja meg  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** . [További](https://msdn.microsoft.com/library/aa826699.aspx) hozzáférés-vezérléssel kapcsolatos.<br/><br/> Feladatátvétel Ha azt szeretné, hogy a tooWindows virtuális gépek Azure-ban csatlakozzon a távoli asztal győződjön meg arról, hogy a távoli asztal engedélyezve van-e a hello a helyi számítógépen. Ha nem VPN-kapcsolaton keresztül, tűzfalszabályok keresztül hello engedélyezése a távoli asztali kapcsolatok internet. | |
| **Linux-gépek** |Egy támogatott 64 bites operációs rendszeren: **Centos 6.4 6.5, 6.6**; **Oracle Enterprise Linux 6.4 hello Red Hat kompatibilis kernel vagy szoros vállalati Kernel Release 3 (UEK3) futtató 6.5**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> A védett gépek tűzfalszabályok engedélyezni kell őket tooreach hello konfigurációs és fő célkiszolgálóra az Azure-ban.<br/><br/> / etc/hosts fájlokat a védett gépek bejegyzéseit, amelyek hozzárendeléséről hello helyi állomás neve tooIP társított összes hálózati adaptert kell tartalmaznia. <br/><br/> Ha azt szeretné, hogy tooconnect tooan Azure virtuális gépen futó Linux után a Feladatátvevőfürt-ügyfélprogrammal egy Secure Shell (ssh), győződjön meg arról, hogy hello védett hello Secure Shell szolgáltatás gép toostart rendszerindításkor automatikusan az van beállítva, és hogy a tűzfalszabályok engedélyezése egy ssh kapcsolat tooit.<br/><br/> hello állomásnév, csatlakoztatási pontokat, eszköz, és Linux rendszer elérési utakat és fájlneveket (pl./etc /; / usr) kell angolul csak.<br/><br/> A következő tárolási hello a helyszíni gépeket is engedélyezni a védelmet:-<br>Fájlrendszer: EXT3, ETX4, ReiserFS, XFS<br>A többutas szoftver-eszköz leképező (többutas)<br>Kötetkezelő: LVM2<br>HP CCISS vezérlő tároló fizikai kiszolgálók nem támogatottak. | |
| **Külső** |Ebben a forgatókönyvben egyes telepítési összetevők megfelelően harmadik féltől származó szoftverek toofunction függ. Teljes listáját lásd: [külső szoftver értesítéseket és információk](#third-party) | |

### <a name="network-connectivity"></a>Hálózati kapcsolat
Két lehetőség közül választhat a helyszíni hely közötti hálózati kapcsolat konfigurálásakor és hello Azure virtuális hálózat mely hello infrastruktúra összetevőinek (konfigurációs kiszolgáló, a fő célkiszolgáló kiszolgálók) vannak telepítve. Mely hálózati kapcsolódási lehetőséget toouse előtt telepítheti a konfigurációs kiszolgáló toodecide lesz szüksége. Ezt a beállítást, a központi telepítés hello időpontjában kell toochoose. Később nem módosítható.

**Internet:** biztonságos SSL-en keresztül történik és hello a helyszíni kiszolgálók (a folyamatkiszolgáló, a védett gépek) és hello Azure-infrastruktúra összetevőkiszolgálókat (konfigurációs kiszolgáló, a fő célkiszolgáló) közötti replikálási / A helyszíni toohello nyilvános végpontok a hello konfigurációs és a fő cél a TLS-kapcsolatot. (hello egyetlen kivétel, hello kapcsolatot hello folyamatkiszolgáló és a hello fő célkiszolgáló TCP-porton 9080, amelyek nem titkosítottak. Csak vezérlő vonatkozó információkat ezen a kapcsolaton keresztül megosztott toohello replikációs protokoll replikációs beállítás.)

![Központi telepítési diagram internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: és hello a helyszíni kiszolgálók (a folyamatkiszolgáló, a védett gépek) és hello Azure-infrastruktúra összetevőkiszolgálókat (konfigurációs kiszolgáló, a fő célkiszolgáló) közötti replikálási történik egy VPN-kapcsolaton keresztül a helyszíni hálózat között az hello Azure virtuális hálózatot, mely hello konfigurációs kiszolgáló és a fő célkiszolgálóra van telepítve. Győződjön meg arról, hogy a helyszíni hálózat csatlakoztatott toohello Azure virtuális hálózatot, ExpressRoute kapcsolat vagy egy pont-pont VPN-kapcsolatot.

![Telepítési diagram VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)

## <a name="step-1-create-a-vault"></a>1. lépés: A tároló létrehozása
1. Jelentkezzen be toohello [kezelési portál](https://portal.azure.com).
2. Bontsa ki a **adatszolgáltatások** > **Recovery Services** kattintson **Site Recovery-tároló**.
3. Kattintson az **Új létrehozása** > **Gyorslétrehozás** elemre.
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.
5. A **régió**, válassza ki a hello hello tároló földrajzi régióját. támogatott toocheck régiók, tekintse meg a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Kattintson a **Tároló létrehozása** elemre.

    ![Új tároló](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Ellenőrizze, hogy hello állapot sáv tooconfirm hello tároló sikeresen létrejött. hello tároló tulajdonosaként **aktív** a fő hello **Recovery Services** lap.

## <a name="step-2-deploy-a-configuration-server"></a>2. lépés: A konfigurációs kiszolgáló központi telepítése
### <a name="configure-server-settings"></a>Kiszolgáló beállításainak konfigurálása
1. A hello **Recovery Services** hello tároló tooopen hello gyors kezdés lapon kattintson. Gyors üzembe helyezési hello ikon segítségével is megnyithatja.

    ![Első lépések ikon](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)
2. Hello legördülő listában válassza ki a **egy helyszíni hely VMware vagy fizikai kiszolgálók és az Azure közötti**.
3. A **előkészítése Target(Azure) erőforrások** kattintson **konfigurációs kiszolgáló telepítése**.

    ![Konfigurációs kiszolgáló központi telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)
4. A **új konfigurációs kiszolgáló adatait** adja meg:

   * Hello konfigurációs kiszolgáló és a hitelesítő adatok tooconnect tooit nevét.
   * Hello hálózati kapcsolat típusát a legördülő listán válassza ki **nyilvános Internet** vagy **VPN**. Vegye figyelembe, hogy nem módosíthatja ezt a beállítást, az alkalmazása után.
   * Válassza ki a hello Azure-hálózatot a mely hello kiszolgálónak kell lennie. Használata esetén a VPN-győződjön meg arról, hogy hello Azure-hálózat csatlakoztatott tooyour a helyszíni hálózat várt módon.
   * Adja meg a hello belső IP-címet és alhálózatot, amely toohello server hozzá lesz rendelve. Vegye figyelembe, hogy minden alhálózat első négy IP-címeivel hello belső Azure használati számára vannak fenntartva. Bármilyen elérhető IP-cím használatára.

     ![Konfigurációs kiszolgáló központi telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)
5. Amikor rákattint **OK** egy szabványos A3 egy Azure Site Recovery Windows Server 2012 R2 gyűjtemény lemezképen alapuló virtuális gép létrejön az előfizetés hello konfigurációs kiszolgáló. Az első példánynál az új felhőalapú szolgáltatás hello jön létre. Ha kiválasztott tooconnect keresztül hello internet hello felhőalapú szolgáltatás nyilvános fenntartott IP-címek jön létre. Is nyomon követheti a hello **feladatok** fülre.

    ![A figyelő folyamatban](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)
6. Ha keresztül hello internet, miután hello konfigurációs kiszolgálón telepített Megjegyzés hello nyilvános IP cím tooit hello a **virtuális gépek** hello Azure portálra a lap. Végül a hello **végpontok** lapon Megjegyzés hello nyilvános HTTPS-porthoz csatlakoztatott tooprivate 443-as portot. Ez az információ később lesz szüksége, regisztrálásakor hello fő célkiszolgáló és a folyamat kiszolgálók hello konfigurációs kiszolgálóval. hello konfigurációs kiszolgáló ezeket a végpontokat a következőkkel:

   * HTTPS: Egy nyilvános port használt toocoordinate Server adatbázisok közötti kommunikáció összetevő, és Azure keresztül hello internet. A magánhálózati port 443-as használt toocoordinate kommunikációs összetevők kiszolgálóin és az Azure közötti megadása a VPN-kapcsolaton keresztül.
   * Egyéni: A nyilvános port feladat-visszavétel eszköz kommunikációhoz használt keresztül hello internet. Magánhálózati port 9443 feladat-visszavétel eszköz kommunikációs szolgál a VPN-kapcsolaton keresztül.
   * PowerShell: Magánhálózati port 5986-os
   * Távoli asztali: titkos 3389-es port

   ![VM-végpontok](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

   > [!WARNING]
   > Nem törölheti, vagy módosíthatja a konfigurációs kiszolgáló telepítése során létrehozott végpontok hello nyilvános és magánhálózati port számát.
   >
   >

hello konfigurációs kiszolgálón van telepítve, az automatikusan létrehozott Azure-felhőszolgáltatás fenntartott IP-címmel. hello fenntartott címe szükséges, amely a konfigurációs kiszolgáló felhőalapú szolgáltatás IP-cím hello tooensure marad hello azonos újraindítások hello virtuális gépek (beleértve a hello konfigurációs kiszolgáló), hello felhőalapú szolgáltatás. hello fenntartott nyilvános IP-címet kell toobe manuálisan nem lefoglalt Ha hello konfigurációs kiszolgálón le van szerelve, vagy a fenntartott kell maradnia. 20 fenntartott nyilvános IP-címek előfizetésenként alapértelmezett korlátozva van. [További](../virtual-network/virtual-networks-reserved-private-ip.md) fenntartott IP-címek.

### <a name="register-hello-configuration-server-in-hello-vault"></a>Hello tároló hello konfigurációs kiszolgáló regisztrálása
1. A hello **gyors üzembe helyezés** kattintson **Célerőforrások előkészítése** > **a regisztrációs kulcs letöltése**. hello kulcsfájl jön létre automatikusan. Érvénytelen, így ezt követően 5 napig. Másolja a toohello konfigurációs kiszolgáló.
2. A **virtuális gépek** hello virtuális gépek listájából válassza hello konfigurációs kiszolgáló. Nyissa meg hello **irányítópult** fülre, és kattintson **Connect**. **Nyissa meg** hello letöltött RDP-fájl toolog távoli asztali kapcsolattal hello konfigurációs kiszolgálóra. VPN használata, hello belső IP-címét (hello hello konfigurációs kiszolgáló telepítésekor megadott) használata a távoli asztali kapcsolat hello a helyszíni helyről. hello Azure Site Recovery konfigurációs kiszolgáló telepítése varázsló automatikusan elindul, amikor bejelentkezik a hello az első alkalommal.

    ![Regisztráció](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)
3. A **külső Szoftvertelepítés** kattintson **elfogadom** toodownload és MySQL telepítése.

    ![MySQL-telepítés](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)
4. A **MySQL kiszolgálóadatok** hitelesítő adatok toolog alakzatot hello MySQL server-példány létrehozása.

    ![MySQL hitelesítő adatok](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)
5. A **Internetbeállítások** adja meg, hogyan fog csatlakozni hello konfigurációs kiszolgáló toohello internet. Vegye figyelembe:

   * Ha azt szeretné, hogy toouse egyéni proxyt érdemes beállítania, hello szolgáltató telepítése előtt.
   * Amikor rákattint **tovább** egy teszt lefuttatásával toocheck hello proxykapcsolatot.
   * Ha egyéni proxyt használ, vagy az alapértelmezett proxy hitelesítést igényel tooenter hello proxy adatait, beleértve a hello cím, port és a hitelesítő adatok lesz szüksége.
   * a következő URL-címek hello hello proxyn keresztül történő elérhetőnek kellene lennie:
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Ha IP-címeken alapuló tűzfalszabályok győződjön meg arról, hogy hello szabályok be vannak állítva tooallow kommunikációs hello konfigurációs kiszolgáló toohello IP-címekről ismertetett [Azure Datacenter IP-címtartományok](https://msdn.microsoft.com/library/azure/dn175718.aspx) és a HTTPS (443) protokollt. IP-címtartományok toowhite-lista hello Azure-régió, toouse, és az USA nyugati régiója megtervezése kellene lennie.

     ![Proxy regisztrálása](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)
6. A **szolgáltató hiba üzenetek honosítási beállításai** adja meg, milyen nyelven hiba üzenetek tooappear.

    ![Üzenet-regisztrációs hiba](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)
7. A **Azure Site Recovery regisztrációs** tallózással keresse meg és jelölje be hello kulcsfájl toohello server másolt.

    ![A regisztrációs kulcsot tartalmazó fájlt](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)
8. Hello varázsló hello Befejezés lapon válassza az ezeket a beállításokat:

   * Válassza ki **indítsa el a fiókhoz felügyeleti párbeszédpanel** , hogy a fiókok kezelése párbeszédpanel hello toospecify meg kell nyitnia a hello varázsló befejezése után.
   * Válassza ki **hozzon létre egy asztali ikon Cspsconfigtool** tooadd parancsikont hello konfigurációs kiszolgálón, hogy meg tudja nyitni a hello **fiókok kezelése** toorerun hello anélkül bármikor párbeszédpanel varázsló.

     ![Regisztráció elvégzésére](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)
9. Kattintson a **Befejezés** toocomplete hello varázsló. A hozzáférési kód jön létre. Tooa biztonságos helyre másolja. Szükség lenne rá tooauthenticate lesz, és regisztrálja hello folyamat és a fő célkiszolgálóra hello konfigurációs kiszolgáló. Azt is használta tooensure csatorna integritását a konfigurációs kiszolgáló közötti kommunikációt. Hello jelszót helyreállíthatók, de kell használnunk toore-nyilvántartás hello fő célkiszolgáló és a folyamat kiszolgálók hello új jelszót.

    ![Hozzáférési kód](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Regisztrálás után hello konfigurációs kiszolgáló megjelenik a hello **konfigurációs kiszolgálók** lap hello tárolóban lévő állapottal.

### <a name="set-up-and-manage-accounts"></a>Állítson be és fiókok kezelése
Központi telepítése során a Site Recovery kéri a hitelesítő adatokat a következő műveletek hello:

* Egy VMware-fiók, thatSite helyreállítási képes automatikusan felderítési futó virtuális gépek egy vCenter-kiszolgáló vagy vSphere-gazdagép.
* Ha hozzá ügyfélgépek, a Site Recovery hello mobilitásiszolgáltatás telepíthetik őket.

Hello konfigurációs kiszolgáló már regisztrálása után megnyithatja hello **fiókok kezelése** párbeszédpanel tooadd és fiókok kezeléséhez, amelyek használja a rendszer ezeket a műveleteket. Számos módon toodo néhány ez.

* Nyissa meg a hello helyi hello párbeszédpanel hello hello konfigurációs kiszolgáló (cspsconfigtool) telepítése és utolsó lapján toocreated választotta.
* Nyissa meg hello párbeszédpanel fejezze be a konfigurációs kiszolgáló telepítése.

1. A **fiókok kezelése** kattintson **fiók hozzáadása**. Módosítja, és törölje a meglévő fiókokat.

    ![Fiókok kezelése](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)
2. A **fiókadatok** adja meg a fiók neve toouse Azure és a hitelesítő adatok (tartomány\felhasználónév).

    ![Fiókok kezelése](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-toohello-configuration-server"></a>Csatlakoztassa toohello konfigurációs kiszolgálót
Két módon tooconnect toohello konfigurációs kiszolgálón van:

* A VPN-webhelyek vagy ExpressRoute-kapcsolat
* Keresztül hello internet

Vegye figyelembe:

* Az internetkapcsolat hello végpontok hello virtuális gép hello nyilvános virtuális IP-cím hello kiszolgáló együtt használja.
* VPN-kapcsolat együtt hello titkos végpontportokat hello kiszolgáló hello belső IP-címét használja.
* E egy egyszeri döntési toodecide tooconnect (vezérlő és a replikációs adatok) a helyszíni kiszolgálók toohello a különböző összetevőkiszolgálókat (konfigurációs kiszolgáló, a fő célkiszolgáló) Azure-beli egy VPN-kapcsolaton keresztül, vagy internetes hello. Ezt a beállítást, ezek után nem módosítható. Ha így tesz, akkor lesz kell tooredeploy hello forgatókönyv és a gépeket állítsa.  

## <a name="step-3-deploy-hello-master-target-server"></a>3. lépés: Hello fő célkiszolgáló telepítése
1. Kattintson a **előkészítése Target(Azure) erőforrások** > **telepítés fő célkiszolgáló**.
2. Adja meg a hello fő server adatokat és hitelesítő adatokat. hello kiszolgálót telepíteni fogja a hello és hello konfigurációs kiszolgáló ugyanahhoz az Azure-hálózathoz. Toocomplete kattintva Azure virtuális gép jön létre a Windows vagy Linux gyűjtemény lemezkép.

    ![Cél kiszolgáló beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Vegye figyelembe, hogy minden alhálózat első négy IP-címeivel hello belső Azure használati számára vannak fenntartva. Adja meg, hogy bármilyen elérhető IP-cím.

> [!NOTE]
> Válassza a szabványos DS4, ha egységes magas i/o-teljesítménye és kis késleltetése rendelés toohost i/o igényes munkaterhelések használatával a igénylő munkaterhelések védelmének beállítása [prémium szintű Tárfiók](../storage/common/storage-premium-storage.md).
>
>

1. A Windows fő célkiszolgáló virtuális gép jön létre a fenti végpontokkal. Vegye figyelembe, hogy a nyilvános végpontok csak akkor, ha a kapcsolaton keresztül csatlakozó hello internet jöttek létre.

   * Egyéni: A nyilvános port hello hello folyamat server toosend replikációs adatok által használt internetes. Magánhálózati port 9443 hello folyamat server toosend replikációs adatok toohello fő célkiszolgáló használják VPN-kapcsolaton keresztül.
   * Custom1: Nyilvános port hello hello folyamat server toosend metaadatok által használt internetes. Magánhálózati port 9080 hello folyamat server toosend metaadatok toohello fő célkiszolgáló használják VPN-kapcsolaton keresztül.
   * PowerShell: Magánhálózati port 5986-os
   * Távoli asztali: titkos 3389-es port
2. Linux fő célkiszolgáló virtuális gép ezeket a végpontokat hozza létre. Vegye figyelembe a nyilvános végpontok létrehozása, csak ha keresztül hello internet.

   * Egyéni: A nyilvános port hello folyamat server toosend replikációs adatok által használt internetes. Magánhálózati port 9443 hello folyamat server toosend replikációs adatok toohello fő célkiszolgáló használják VPN-kapcsolaton keresztül.
   * Custom1: Nyilvános portot használja hello folyamat toosend metaadatai keresztül hello internet. Magánhálózati port 9080 hello folyamat server toosend metaadatok toohello fő kiszolgáló által használt VPN-kapcsolaton keresztül
   * SSH: Titkos 22-es portot

     > [!WARNING]
     > Nem törölheti, vagy módosíthatja a hello nyilvános és magánhálózati port száma bármely hello fő kiszolgáló telepítése során létrehozott hello végpontok.
     >
     >
3. A **virtuális gépek** hello virtuális gép toostart várja.

   * Ha egy Windows server jegyezze fel hello távoli asztali részleteit.
   * Ha a Linux-kiszolgáló, és, keresztül VPN Megjegyzés hello belső IP-címet hello virtuális gép csatlakozik. Ha keresztül hello internet Megjegyzés hello nyilvános IP-cím.
4. Jelentkezzen be hello server toocomplete telepítése, és regisztrálhatja azt az hello konfigurációs kiszolgáló.
5. Ha a Windows fut:

   1. Indítsa el a távoli asztali kapcsolat toohello virtuális gépek. hello első alkalommal jelentkezik be egy parancsfájl fog futni egy PowerShell-ablakot. Ne zárja be azt. A Befejezés után hello állomás ügynök konfigurációs eszköz automatikusan megnyílik tooregister hello kiszolgáló.
   2. A **állomás ügynök konfigurációs** hello belső IP-cím hello konfigurációs kiszolgáló és a 443-as portot adja meg. Hello belső cím és a titkos 443-as portot is használhat a akkor is, ha nincs kapcsolat VPN-kapcsolaton keresztül mert hello virtuális gép csatlakoztatott toohello és hello konfigurációs kiszolgáló ugyanahhoz az Azure-hálózathoz. Hagyja **használja HTTPS** engedélyezve van. Adja meg hello az hello konfigurációs kiszolgáló, amelyet korábban feljegyzett. Kattintson a **OK** tooregister hello kiszolgáló. Hello NAT-beállításokat figyelmen kívül hagyhatja. Nem használja ezeket.
   3. Ha a becsült adatmegőrzési meghajtó követelmény több mint 1 TB-os hello adatmegőrzési kötet (r) egy virtuális lemez segítségével beállíthatja és [tárolóhelyek](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx)

   ![Fő célkiszolgáló Windows](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)
6. Ha a Linux fut:

   1. Győződjön meg arról, hogy telepítette hello legújabb Linux integrációs szolgáltatások (LIS) hello fő célkiszolgáló telepítése előtt telepítve. Hogyan hello legújabb verziójának LIS utasításokat mellett található tooinstall [Itt](https://www.microsoft.com/download/details.aspx?id=46842). Hello gép újraindítása hello LIS telepítése után.
   2. A **előkészítése (Azure) Célerőforrások** kattintson **töltse le és telepítse a további szoftverfrissítési (csak a Linuxos fő célkiszolgáló)**. Másolás hello letöltött bont fájl toohello virtuális gépet egy sftp-ügyfél használatával. Másik megoldásként jelentkezzen be toohello telepített linux fő célkiszolgáló, és használjon *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* toodownload hello hello fájlt.
   3. Jelentkezzen be toohello server Secure Shell-ügyfél használatával. Ha csatlakozott toohello Azure-hálózatot VPN-kapcsolaton keresztül a hello belső IP-címet használja. Ellenkező esetben használja a hello külső IP-cím és hello SSH nyilvános végpontot.
   4. Csomagolja ki hello fájlokat hello gzipped telepítő futtatásával: **bont – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
      ![Linux fő célkiszolgáló](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
   5. Ellenőrizze, hogy van-e hello directory toowhich kicsomagolta hello hello bont fájl tartalmát.
   6. Hello konfigurációs kiszolgáló jelszava tooa helyi fájl másolása hello paranccsal **echo  *`<passphrase>`*  > passphrase.txt**
   7. Hello parancs "**sudo. / install -t mindkét - egy gazdagép -R -d /usr/local/ASR MasterTarget -i  *`<Configuration server internal IP address>`*  -p 443 -s y - c https -P passphrase.txt**".

      ![Kiszolgáló regisztrálása](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)
7. Várjon néhány percet (10 – 15), és a hello lapon ellenőrizze, hogy hello fő célkiszolgáló szerepel a regisztrált **kiszolgálók** > **konfigurációs kiszolgálók** **kiszolgálóadatok** fülre. Ha a Linux futtatja, és nem regisztrált futtassa hello gazdagép konfigurációs eszköz újra /usr/local/ASR/Vx/bin/hostconfigcli. Legfelső szintű chmod futtatásával kell tooset hozzáférési engedélyeket.

    ![Ellenőrizze a célkiszolgálón](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

> [!NOTE]
> Percig is tarthat too15 be regisztrációs hello fő server toobe hello portal szerepel a befejezése után. tooupdate azonnal, kattintson a **frissítése** a hello **konfigurációs kiszolgálók** lap.
>
>

## <a name="step-4-deploy-hello-on-premises-process-server"></a>4. lépés: Hello helyszíni folyamatkiszolgáló telepítése
Megkezdése előtt ajánlott konfigurált statikus IP-címnek hello folyamatkiszolgáló, hogy biztosított állandó toobe keresztben újraindul.

1. Kattintson a gyors üzembe helyezés > **Folyamatkiszolgáló telepítése a helyszíni** > **töltse le és telepítse a hello folyamatkiszolgáló**.

    ![Folyamatkiszolgáló telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)
2. Másolás hello letöltött zip toohello fájlkiszolgáló amelyen tooinstall hello folyamatkiszolgáló fog. hello zip-fájl két telepítési fájlokat tartalmazza:

   * Microsoft-ASR_CX_TP_8.4.0.0_Windows*
   * Microsoft-ASR_CX_8.4.0.0_Windows*
3. Bontsa ki a hello archiválási és másolási hello telepítési tooa helye hello kiszolgálón.
4. Futtassa a hello **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** telepítési fájlt, és kövesse a hello utasításokat. Ezzel telepíti a hello telepítést külső összetevőket.
5. Ezután futtassa **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. A hello **kiszolgáló üzemmód** lapon jelölje be **Folyamatkiszolgáló**.
7. A hello **környezet részletei** lap hello a következő:

    - Ha azt szeretné, hogy tooprotect VMware virtuális gépek kattintson **Igen**
    - Ha csak szeretné tooprotect fizikai kiszolgálók, és így nem kell VMware vCLI hello folyamat-kiszolgálóra telepíthető. Kattintson a **nem** és a folytatáshoz.

1. Vegye figyelembe a következő hello VMware vCLI telepítésekor:

   * **Csak a VMware vSphere parancssori felülete 5.5.0 támogatott**. hello folyamatkiszolgálóra a vSphere parancssori felülete frissítéseit és más verzióit nem működik.
   * Töltse le a vSphere parancssori felülete 5.5.0 a [itt.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
   * Ha telepítette a vSphere parancssori felülete előtt használatba hello folyamatkiszolgáló telepítése, és a telepítő nem ismeri fel, várjon legalább toofive perc elteltével, próbálkozzon újra. Ez biztosítja, hogy minden hello környezeti változók szükséges a vSphere parancssori felülete észleléséhez lett inicializálva megfelelően.
2. A **NIC-kiválasztás használata a Folyamatkiszolgáló** hello folyamat kiszolgálón válassza hello hálózati adaptert kell használnia.

   ![Válassza ki az adapter](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)
3. A **konfigurációs kiszolgáló adatait**:

   * Hello IP-címet és portot Ha VPN-kapcsolaton keresztül adja meg hello konfigurációs kiszolgáló hello belső IP-címét és a 443-as hello port. Ellenkező esetben adja meg a hello nyilvános virtuális IP-cím és a csatlakoztatott nyilvános HTTP-végpont.
   * Írja be a konfigurációs kiszolgáló hello hello jelszót.
   * Törölje a jelet **ellenőrizze a mobilitási szolgáltatás szoftver aláírás** Ha toodisable ellenőrzési automatikus leküldéses tooinstall hello szolgáltatás használatakor. Aláírás-ellenőrzés hello folyamat-internetkapcsolattal kell.
   * Kattintson a **Tovább** gombra.

   ![Regisztráljon egy konfigurációs kiszolgálót](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)
4. A **válassza ki a telepítési meghajtóján** válassza ki a gyorsítótárazáshoz használt lemezen. hello folyamatkiszolgáló a gyorsítótárazáshoz használt lemezen legalább 600 GB szabad lemezterület szükséges. Ezt követően kattintson a **Telepítés** gombra.

   ![Regisztráljon egy konfigurációs kiszolgálót](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)
5. Vegye figyelembe, hogy szükség lehet a toorestart hello server toocomplete hello telepítése. A **konfigurációs kiszolgáló** > **kiszolgálóadatok** ellenőrizze, hogy hello folyamatkiszolgáló jelenik meg, és regisztrálása sikeres hello tárolóban.

> [!NOTE]
> Too15 perc regisztrációs a hello folyamat server tooappear hello konfigurációs kiszolgáló felsorolt befejezése után is eltarthat. hello konfigurációs kiszolgáló lap hello alján hello frissítés gombra kattintva azonnal, frissítse hello konfigurációs kiszolgáló tooupdate
>
>

![Folyamatkiszolgáló ellenőrzése](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Ha hello mobilitásiszolgáltatás aláírás-ellenőrzés nem letiltja a hello folyamatkiszolgáló regisztrálásakor is elvégezheti később, az alábbiak szerint:

1. Jelentkezzen be rendszergazdaként hello folyamatkiszolgálót, és nyissa meg szerkesztésre a C:\pushinstallsvc\pushinstaller.conf hello fájlt. Hello szakaszban **[PushInstaller.transport]** adja hozzá a sort: **SignatureVerificationChecks = "0"**. Mentse és zárja be hello fájlt.
2. Indítsa újra a hello InMage PushInstall szolgáltatás.

## <a name="step-5-update-site-recovery-components"></a>5. lépés: Frissítés a Site Recovery-összetevők
Site Recovery-összetevőkhöz idő tootime frissül. Ha új frissítések érhetők el telepítenie kell őket a sorrend hello:

1. Konfigurációs kiszolgáló
2. Folyamatkiszolgáló
3. Fő célkiszolgáló
4. Feladat-visszavétel eszköz (vContinuum)

### <a name="obtain-and-install-hello-updates"></a>Szerezzen be és hello frissítések telepítése
1. A Site Recovery hello beszerezhesse a frissítéseket a hello konfigurációs, folyamat és a fő célkiszolgálóra **irányítópult**. Linux rendszerhez – telepítés csomagolja ki hello fájlokat hello gzipped telepítő és hello parancs "sudo. / telepítése" tooinstall hello frissítés.
2. [Töltse le](http://go.microsoft.com/fwlink/?LinkID=533813) hello feladat-visszavétel tool(vContinuum) legújabb frissítésének hello.
3. Ha futtatja a virtuális gépek vagy fizikai kiszolgálók, amelyeket már hello mobilitási szolgáltatás telepítve van, akkor kaphat frissítéseket hello szolgáltatást az alábbiak szerint:

   * **1. lehetőség**: Töltse le a frissítéseket:
     * [Windows Server (csak a 64 bites)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
     * [CentOS 6.4,6.5,6.6 (csak a 64 bites)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
     * [Oracle Linux vállalati 6.4,6.5 (csak a 64 bites)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
     * [SUSE Linux Enterprise Server SP3 (csak a 64 bites)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
     * Hello folyamat kiszolgáló köszönésére frissítése után hello mobilitási szolgáltatás frissített verzióját hello C:\pushinstallsvc\repository mappa hello folyamatkiszolgáló lesz elérhető.
   * **2. lehetőség**: Ha a virtuális gép hello telepíteni a mobilitási szolgáltatás régebbi verziója van, automatikusan frissítheti hello mobilitási szolgáltatás hello gépen hello felügyeleti portálról.

     1. Győződjön meg arról, hogy hello folyamatkiszolgáló frissül.
     2. Győződjön meg arról, hogy hello védett gép megfelel-e hello [Előfeltételek](#install-the-mobility-service-automatically) hello a mobilitási szolgáltatást, automatikusan tárházat, így hello frissítés megfelelően működik-e.
     3. Jelölje be hello védelmi csoport, kiemelési hello védett gép, és kattintson **frissítés mobilitásiszolgáltatás**. Erre a gombra kattintva csak érhető el, ha nincs hello mobilitási szolgáltatás egy újabb verziója.

         ![Válassza ki a vCenter-kiszolgáló](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Válassza ki a fiókokat adja meg hello rendszergazdai fiók használt toobe tooupdate hello mobilitásiszolgáltatás hello védett kiszolgálón. Kattintson az OK gombra, majd várja meg a feladat toocomplete indított hello.

## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>6. lépés: Egy vCenter-kiszolgáló vagy vSphere-gazdagép hozzáadása
1. Kattintson a **kiszolgálók** > **konfigurációs kiszolgálók** > konfigurációs kiszolgáló >**vCenter Server hozzáadása** tooadd vCenter-kiszolgáló vagy vSphere-gazdagép.

    ![Válassza ki a vCenter-kiszolgáló](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)
2. Adja meg a részleteket hello server vagy a gazdagép és a select hello folyamat kiszolgálón használt toodiscover azt.

   * Ha hello vCenter-kiszolgáló nem fut a hello alapértelmezett 443 portot adja meg a mely hello vCenter-kiszolgáló fut. hello portot.
   * ugyanaz, mint a hello vCenter kiszolgáló vagy vSphere-gazdagép hálózati kell állnia, és VMware vSphere parancssori felülete telepítve 5.5.0 hello hello folyamatkiszolgáló kell lennie.

     ![vCenter-kiszolgáló beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)
3. Felderítés végeztével hello vCenter-kiszolgáló alatt helyezkednek el hello konfigurációs kiszolgáló adatait.

    ![vCenter-kiszolgáló beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)
4. Ha a nem rendszergazdai fiók tooadd hello kiszolgáló vagy a gazdagép használ, győződjön meg arról hello fióknak legyen jogosultsága a következő hello:

   * vCenter fiókok Datacenter, Datastore, mappa, Host, hálózati, erőforrás, tárolási nézetek, virtuális gép és kell vSphere elosztott kapcsoló jogosultságokkal engedélyezve van.
   * vSphere-gazdagép fiókok hello Datacenter, Datastore, mappa, állomás, hálózati, erőforrás, virtuális gép és engedélyezve van a vSphere elosztott kapcsoló jogosultságokkal kell rendelkeznie.

## <a name="step-7-create-a-protection-group"></a>7. lépés: A védelmi csoport létrehozása
1. Nyissa meg **védett elemek** > **védelmi csoport** > **védelmi csoport létrehozása**.

    ![Védelmi csoport létrehozása](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)
2. A hello **adja meg a védelmi csoport beállításainál** lapon adja meg a hello csoport, és válassza hello konfigurációs kiszolgálón, amelyen toocreate hello csoport nevét.

    ![Védelmi csoport beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)
3. A hello **adja meg a replikációs beállításokat** lap hello replikációs beállítások konfigurálásához, amelyeket hello csoportban lévő összes hello gépekhez használt portbesorolás.

    ![Védelmi csoport replikációs](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)
4. Beállítások:

   * **Több virtuális Gépre kiterjedő konzisztencia**: Ha bekapcsolja ezt a azt megosztott alkalmazáskonzisztens helyreállítási pontokat hoz létre hello gépek hello védelmi csoport között. A beállítás akkor a leghasznosabb, ha minden hello gépek hello védelmi csoportban lévő futnak hello ugyanaz az alkalmazás. Az összes gép lesz a helyreállított toohello azonos adatpont. Csak Windows-kiszolgálók esetén érhető el.
   * **Helyreállítási Időkorlát küszöbértéke**: hello folyamatos adatvédelemhez kapcsolódó replikáció helyreállítási Időkorlátja túllépi a beállított hello helyreállítási Időkorlát küszöbértékét hoz létre riasztásokat.
   * **Helyreállítási pontok megőrzésének ideje**: hello adatmegőrzési időtartam határozza meg. Védett gépek lehet ebben az ablakban tooany pont helyre.
   * **Alkalmazáskonzisztens pillanatkép gyakorisága**: határozza meg, milyen gyakran alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat hoz létre.

Figyelheti a hello védelmi csoport szerint jönnek létre a hello **védett elemek** lap.

## <a name="step-8-set-up-machines-you-want-tooprotect"></a>8. lépés: A gépek beállítása azt szeretné, tooprotect
Szüksége lesz a mobilitási szolgáltatást a virtuális gépek és fizikai kiszolgálók tooprotect tooinstall hello. Ezt két módon teheti meg:

* Automatikusan leküldéses, és minden számítógépen az hello folyamatkiszolgáló hello szolgáltatás telepítése.
* Manuálisan telepítse hello szolgáltatást.

### <a name="install-hello-mobility-service-automatically"></a>Hello mobilitási szolgáltatás automatikus telepítése
Hozzáadásakor gépek tooa védelmi csoport hello mobilitási szolgáltatás automatikusan leküldött és hello folyamat kiszolgáló minden számítógépen telepítve.

**Leküldéses automatikusan hello mobilitási szolgáltatás telepítése a Windows-kiszolgálók:**

1. Telepítse legújabb frissítéseit hello hello folyamatkiszolgáló [5. lépés: telepítse a legújabb frissítéseket](#step-5-install-latest-updates), és győződjön meg arról, hogy hello folyamatkiszolgáló érhető el.
2. Győződjön meg arról, nincs hálózati kapcsolat között hello forrásgép és hello folyamatkiszolgálót, és adott hello forrásgép elérhető hello folyamatkiszolgáló.  
3. Konfigurálja a hello Windows tűzfal tooallow **fájl- és nyomtatómegosztás** és **Windows Management Instrumentation**. A Windows tűzfal beállításai hello beállításnak a "Egy alkalmazás vagy szolgáltatás átengedése tűzfal", és válassza ki a hello alkalmazásokat, ahogy az alábbi képen hello. Hello tűzfal-házirendet egy csoportházirend-objektum konfigurálható tooa tartományhoz tartozó gépek.

    ![Tűzfalbeállítások](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)
4. hello használt fiók tooperform hello ügyfélleküldéses telepítés hello hello számítógép rendszergazdák csoportjába kell lennie, tooprotect szeretné. Hello mobilitási szolgáltatás leküldéses telepítéséhez csak szolgálnak, és fog nekik biztosított a gép tooa védelmi csoport hozzáadásakor.
5. Ha hello megadott fiók nem tartományi fiók toodisable távelérési felhasználói vezérlő hello helyi gépen lesz szüksége. toodo a Hozzáadás hello LocalAccountTokenFilterPolicy DWORD beállításjegyzék bejegyzés alatt HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System 1 értékű. tooadd hello beállításjegyzékbeli bejegyzést a CLI nyissa meg a cmd vagy a PowerShell használatával, és adja meg  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .

**Automatikusan leküldéses hello mobilitási szolgáltatás telepítése Linux-kiszolgálókon:**

1. Telepítse legújabb frissítéseit hello hello folyamatkiszolgáló [5. lépés: telepítse a legújabb frissítéseket](#step-5-install-latest-updates), és győződjön meg arról, hogy hello folyamatkiszolgáló érhető el.
2. Győződjön meg arról, nincs hálózati kapcsolat között hello forrásgép és hello folyamatkiszolgálót, és adott hello forrásgép elérhető hello folyamatkiszolgáló.  
3. Ellenőrizze, hogy hello fiók a forráskiszolgálón hello Linux gyökér szintű felhasználó.
4. Győződjön meg arról, hogy hello/etc/hosts fájl hello forrás Linux-kiszolgáló bejegyzéseit, amelyek hozzárendeléséről hello helyi állomás neve tooIP társított összes hálózati adaptert tartalmaz.
5. Telepítse a legújabb hello openssh, openssh-kiszolgáló openssl csomagok hello gépen tooprotect szeretné.
6. Ügyeljen arra, hogy az SSH engedélyezve legyen, és a 22-es porton fusson.
7. SFTP alrendszer és a jelszó-hitelesítés engedélyezése hello sshd_config fájlban az alábbiak szerint:

   * a) jelentkezzen be rendszergazdaként.
   * b) a hello fájl/etc/ssh/sshd_config fájl keresése hello. sor, amelyek kezdődik **PasswordAuthentication**.
   * c) állítsa vissza a hello sort, és módosítsa a "no" hello értéket túl "yes".

       ![Linux mobilitási](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)
   * d) keresés hello sor alrendszer kezdődik, és állítsa vissza a hello sor.

       ![Linux leküldéses mobilitási](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    
8. Győződjön meg arról, hello forrás gép Linux variant esetén támogatott.

### <a name="install-hello-mobility-service-manually"></a>Telepítse manuálisan a mobilitási szolgáltatás hello
hello szoftvercsomagok használt tooinstall hello a mobilitási szolgáltatás hello folyamatkiszolgáló C:\pushinstallsvc\repository a rendszer. Hello folyamatkiszolgáló és a példány hello megfelelő telepítési csomag toohello forrásgép hello az alábbi táblázat alapján bejelentkezik:-

| Forrás operációs rendszer | Folyamatkiszolgáló a mobilitási szolgáltatás csomag |
| --- | --- |
| Windows Server (csak 64 bites) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe` |
| CentOS 6.4, 6.5, 6.6 (csak 64 bites) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz` |
| SUSE Linux Enterprise Server 11 SP3 (csak 64 bites) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz` |
| Oracle Enterprise Linux 6.4, 6.5 (csak 64 bites) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz` |

**a Windows server manuálisan a mobilitási szolgáltatás tooinstall hello**, a következő hello:

1. Másolás hello **Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** toohello forrásgép fent hello táblázatban felsorolt csomag és program hello folyamat server könyvtár elérési útja.
2. Hello mobilitási szolgáltatás telepítése hello forrásgépen hello végrehajtható fájl futtatásával.
3. Kövesse a hello telepítési utasításokat.
4. Válassza ki **mobilitásiszolgáltatás** hello szerepkör, és kattintson **következő**.

    ![Telepítse a mobilitási szolgáltatás](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)
5. Hagyja hello telepítőkönyvtár hello alapértelmezett telepítési útvonalon, és kattintson a **telepítése**.
6. A **állomás ügynök konfigurációs** adja meg a hello IP-címe és HTTPS-portja hello konfigurációs kiszolgáló.

   * Ha internet meg hello keresztül hello nyilvános virtuális IP-cím és a HTTPS-végpont hello-portjaként működik.
   * VPN-kapcsolaton keresztüli kapcsolat adja meg a hello belső IP-cím és a 443-as hello port. Hagyja **használja HTTPS** be van jelölve.

     ![Telepítse a mobilitási szolgáltatás](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)
7. Adja meg a hello konfigurációs kiszolgáló jelszava, és kattintson a **OK** tooregister hello mobilitásiszolgáltatás hello konfigurációs kiszolgálóval.

**toorun hello parancssorból:**

1. Hello jelszó másolása hello CX toohello fájl "C:\connection.passphrase" hello kiszolgálón, és futtassa a parancsot. Ebben a példában CX i 104.40.75.37 és a HTTPS-portja hello 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Telepítse manuálisan a hello mobilitási szolgáltatás egy Linux-kiszolgálón**:

1. Másolja a hello megfelelő bont archív hello folyamat server toohello forrásgép hello a táblázatot a fenti alapján.
2. Nyissa meg a rendszerhéj programot, és bontsa ki a hello zip bont archív tooa helyi elérési útja a következő futtatásával`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Hozzon létre egy passphrase.txt fájlt hello helyi könyvtár toowhich kicsomagolta hello bont archívum hello tartalmának megadásával  *`echo <passphrase> >passphrase.txt`*  rendszerhéjból.
4. Hello mobilitási szolgáltatás telepítése megadásával  *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`* .
5. Adja meg a hello IP-cím és port:

   * Ha toohello konfigurációs kiszolgáló hello internet adja meg a hello konfigurációs kiszolgáló virtuális nyilvános IP-cím és a nyilvános HTTPS-végponton keresztül kapcsolódik `<IP address>` és `<port>`.
   * Ha a VPN-kapcsolaton keresztül adja meg a hello belső IP-cím és a 443-as.

**hello parancssorból toorun**:

1. Hello jelszó másolása hello CX toohello fájl "passphrase.txt" hello kiszolgálón, és futtassa a parancsokat. Ebben a példában CX i 104.40.75.37 és a HTTPS-portja hello 62519:

tooinstall egy üzemi kiszolgálón:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

tooinstall hello célkiszolgálón:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

> [!NOTE]
> Gépek tooa védelmi csoportot, amely már fut a mobilitási szolgáltatás hello megfelelő verzióját majd hello leküldéses telepítését a rendszer kihagyja hozzáadásakor.
>
>

## <a name="step-9-enable-protection"></a>9. lépés: A védelem engedélyezése
Adja hozzá a virtuális gépek és fizikai kiszolgálók tooa védelmi csoport tooenable védelme. Mielőtt elkezdené, vegye figyelembe, hogy:

* Virtuális gépek felderített 15 percenként, és azokat too15 percig is eltarthat, az Azure Site Recovery tooappear felderítése után.
* Környezet módosításokat (például a VMware-eszközök telepítése) hello virtuális gépen is too15 perc toobe frissítve a Site Recovery is eltarthat.
* Ellenőrizheti a hello a legutóbb felfedezett hello idő **utolsó lépjen KAPCSOLATBA a** mezőjét a hello hello vCenter kiszolgálón vagy ESXi állomások **konfigurációs kiszolgálók** lap.
* Ha a védelmi csoport már létrehozott és vCenter kiszolgálón vagy ESXi állomás hozzáadása után, amely, hello Azure Site Recovery portál toorefresh és a virtuális gépek toobe hello felsorolt 15 percet vesz igénybe **gépek tooa védelmi csoport hozzáadása**  párbeszédpanel.
* Ha azt szeretné, azonnal a gépek tooprotection csoport hozzáadása a hello ütemezett felderítési várakozás nélkül tooproceed, jelölje ki a hello konfigurációs kiszolgálót (ne kattintson rá) hello kattintson **frissítése** gombra.
* Amikor virtuális gépeket vagy fizikai gépek tooa védelmi csoport, a hello folyamatkiszolgáló automatikusan leküldéses értesítések, és telepíti hello mobilitási szolgáltatás a forráskiszolgálón hello hello még nincs telepítve.
* Hello automatikus leküldéses mechanizmus toowork ellenőrizze, hogy beállította a védett gépek hello előző lépésben leírt.

Hozzáadni az alábbiak szerint:

1. Kattintson a **védett elemek** > **védelmi csoport** > **gépek** > **gépeket** . Ajánlott eljárásként azt javasoljuk, hogy a védelmi csoportok tükrözve-e a munkaterhelések, hogy egy adott alkalmazás toohello rendszerű gépeket ugyanabban a csoportban.
2. A **virtuális gépek kijelölése** Ha fizikai kiszolgálókat, a hello védi **fizikai gépek felvétele** varázsló hello IP-cím és az egyszerű nevet meg. Ezután válassza ki a hello operációsrendszer-családot.

    ![Adja hozzá a V-központ kiszolgálón](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)
3. A **virtuális gépek kijelölése** VMware virtuális gépek számára kíván védelmet biztosítani, ha egy vCenter-kiszolgálót, amely kezeli a virtuális gépeken (vagy hello EXSi gazdagépet, amelyen futtatja), majd válassza ki és hello gépek.

    ![Adja hozzá a V-központ kiszolgálón](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png)    
4. A **adja meg tároló erőforrásait** válassza ki a hello fő célkiszolgálóra, és a tárolási toouse replikációhoz, és válassza ki, hogy a munkaterhelések használhatók hello-beállítások. Válassza ki [prémium szintű Tárfiók](../storage/common/storage-premium-storage.md) konzisztens magas I/O teljesítménye és kis késleltetése a rendelés toohost IO-igényes munkaterhelések igénylő munkaterhelések védelmének konfigurálása közben. Ha a munkaterhelések lemezen toouse prémium tárfiókot használni szeretne, toouse hello a fő célkiszolgálón a DS sorozatnak kell. Prémium szintű Storage-lemezeket a fő célkiszolgáló nem DS-sorozat nem használható.

   > [!NOTE]
   > Nem támogatjuk a hello áthelyezés hello használatával létrehozott tárfiókok [új Azure-portálon](../storage/common/storage-create-storage-account.md) erőforráscsoportok között.
   >
   >

    ![vCenter-kiszolgáló](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)
5. A **meg fiókok** toouse szánt hello a mobilitási szolgáltatás telepítése a védett gépek hello fiók kiválasztása. hello fiók hitelesítő adatait a mobilitási szolgáltatás hello automatikus telepítéséhez szükségesek. Ha nem választja ki egy fiók győződjön meg arról, hogy beállította egy 2. lépésben leírtak szerint. Vegye figyelembe, hogy ez a fiók nem érhető el az Azure-ban. A Windows hello szolgáltatásfiókjától hello forráskiszolgálón rendszergazdai jogosultságokkal kell rendelkeznie. Linux hello fióknak root kell lennie.

    ![Linux hitelesítő adatok](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)
6. Kattintson a hello pipa toofinish gépek toohello védelmi csoport és toostart gép kezdeti replikációja minden hozzáadása. Figyelheti a hello állapotát **feladatok** lap.

    ![Adja hozzá a V-központ kiszolgálón](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)
7. Továbbá figyeli a állapotát kattintva **védett elemek** > védelmi csoport neve > **virtuális gépek** . Kezdeti replikálás befejeződik, és hello gépek adatok szinkronizálása után azok megjelenítése **védett** állapotát.

    ![Virtuális gép feladatok](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)

### <a name="set-protected-machine-properties"></a>Állítsa be a védett számítógép tulajdonságait
1. Miután a számítógép rendelkezik egy **védett** állíthatja be a feladatátvételi tulajdonságait állapot. Hello védelmi csoport részletes adatait a select hello gépi és nyitott hello **konfigurálása** fülre.
2. Hello nevet kap toohello gép az Azure-feladatátvétel és hello Azure virtuálisgép-méret után módosíthatja. Igény szerint kiválaszthatja hello Azure hálózati toowhich hello gép a feladatátvételt követően fog csatlakozni.

   > [!NOTE]
   > [Áttelepítési hálózatok](../resource-group-move-resources.md) erőforrás csoportok belül ugyanahhoz az előfizetéshez hello vagy előfizetések között nem támogatott a Site Recovery üzembe helyezéséhez használt hálózatok.
   >
   >

    ![Virtuális gép beállításainak megadása](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Vegye figyelembe:

* az Azure machine hello hello nevét meg kell felelnie, Azure-követelményeknek.
* Alapértelmezés szerint az Azure-ban a replikált virtuális gépek nem csatlakoztatott tooan Azure-hálózatot. Ha azt szeretné, hogy a replikált virtuális gépek toocommunicate győződjön meg arról, hogy tooset hello azonos Azure-hálózat őket.
* Ha VMware virtuális gép vagy fizikai kiszolgálón levő kötet átméretezése kritikus állapotba kerül. Ha szüksége toomodify hello mérete, a hello, a következő:

  * a) hello mérete beállítást módosíthatja.
  * b) a hello **virtuális gépek** lapra, válassza ki a hello virtuális gépet, kattintson a **eltávolítása**.
  * c) a **távolítsa el a virtuális gép** hello beállítást **tiltsa le a védelmet (helyreállítási részletezéshez és kötetméretezéshez)**. Ez a beállítás a védelem letiltása, de megőrzi hello helyreállítási pontok az Azure-ban.

      ![Virtuális gép beállításainak megadása](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)
  * d) hello virtuális gép védelmét engedélyezi. Amikor védelmi hello adatainak átméretezték, ezért hello kötet újból engedélyezi az átvitt tooAzure lesz.

## <a name="step-10-run-a-failover"></a>10. lépés: A feladatátvétel futtatása
Jelenleg csak futtathatja a védett VMware virtuális gépek és fizikai kiszolgálók nem tervezett feladatátvételt. Vegye figyelembe a következőket hello:

* Kezdeményezzen feladatátvételt, mielőtt arról, hogy hello konfigurációs és fő célkiszolgálók fut, és kifogástalan. Ellenkező esetben feladatátvétel sikertelen lesz.
* Forrásgépek nem állítsa le a nem tervezett feladatátvétel részeként. Hello védett kiszolgálók replikálása egy nem tervezett feladatátvétel végrehajtása leáll. Lesz kell toodelete hello gépek hello védelmi csoportból, és adja hozzá újra a rendelés toostart gépek újra védelme hello nem tervezett feladatátvétel befejezése után.
* Ha azt szeretné, toofail keresztül adatvesztés nélkül, győződjön meg arról, hogy hello elsődleges hely virtuális gépek ki vannak kapcsolva hello feladatátvételi elindítása előtt.

1. A hello **helyreállítási tervek** lapon, majd adja hozzá a helyreállítási terv. Adja meg a hello terv adatait, és válassza ki **Azure** hello célként.

    ![Konfigurálja a helyreállítási terv](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)
2. A **válassza ki a virtuális gép** válasszon ki egy védelmi csoportot, és válassza ki a gépeket hello csoport tooadd toohello helyreállítási tervben. [További](site-recovery-create-recovery-plans.md) helyreállítási tervek.

    ![Virtuális gépek hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)
3. Ha a szükséges és személyre is szabhatja hello terv toocreate csoportok hello sorrendje mely gépeken a hello helyreállítási terv feladatátvétel történt. Manuális műveletek és a parancsfájlok kér is hozzáadhat. hello parancsfájlokat, ha a helyreállítás tooAzure vehetők [Azure Automation-forgatókönyveket](site-recovery-runbook-automation.md).
4. A hello **helyreállítási tervek** lapon jelölje be hello terv, és kattintson **nem tervezett feladatátvétel**.
5. A **megerősítéséhez feladatátvétel** hello feladatátvételi irányát (tooAzure) ellenőrizze, és válassza a hello helyreállítási pont toofail keresztül.
6. Hello feladatátvételi feladat toocomplete várja meg, és ellenőrizze, hogy hello feladatátvételi munkaidő a várt módon, és adott hello replikált virtuális gépek elindítása sikeresen megtörtént az Azure-ban.

## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>11. lépés: Sikertelen vissza átadja a feladatokat az Azure-ból gépek
[További](site-recovery-failback-azure-to-vmware-classic-legacy.md) kapcsolatos toobring a sikertelen gépeket Azure-beli biztonsági tooyour helyszíni környezetben.

## <a name="manage-your-process-servers"></a>A folyamat-kiszolgálók kezelése
hello folyamatkiszolgáló replikációs adatok toohello fő célkiszolgáló küld az Azure-ban, és felderíti az új VMware virtuális gépek hozzáadott tooa vCenter-kiszolgálót. A következő körülmények között hello a központi telepítésben toochange hello folyamatkiszolgáló lehet szükség:

* Ha az aktuális kiszolgáló hello a folyamat leáll
* Ha a helyreállítási pont az időkorlát (RPO) nő tooan elfogadhatatlan szint a szervezet számára.

Ha szükséges, áthelyezheti hello replikálását vagy azok egy részét a helyszíni VMware virtuális gépek és fizikai kiszolgálók tooa különböző server feldolgozni. Példa:

* **Hiba**– Ha a folyamatkiszolgáló nem sikerül, vagy nem érhető el a védett gép replikálás tooanother folyamatkiszolgáló áthelyezheti. Metaadatok hello forrásgép és a replika gépet áthelyezett toohello új folyamatkiszolgáló és adatok újra kell szinkronizálni. hello új folyamatkiszolgáló automatikusan fog csatlakozni toohello vCenter tooperform automatikus felderítése. Hello hello Site Recovery irányítópult folyamat kiszolgálók állapotának figyelése
* **Terheléselosztási tooadjust RPO**– javíthatja a terheléselosztást, válasszon egy másik folyamatkiszolgálóra hello Site Recovery portálon, és helyezze át a gépek egy vagy több tooit manuális terheléselosztás replikálását. Ebben az esetben hello kijelölt forrás- és a replika virtuális gépek metaadatai áthelyezett toohello új folyamatkiszolgáló. hello eredeti folyamatkiszolgáló csatlakoztatott toohello vCenter-kiszolgáló marad.

### <a name="monitor-hello-process-server"></a>A figyelő hello folyamatkiszolgáló
Ha a folyamatkiszolgáló egy állapot figyelmeztetés jelenik meg a Site Recovery irányítópult hello kritikus állapotban van. Kattintson a hello állapot tooopen hello események lapján, majd majd részletekbe menően tárhatják toospecific feladatok hello feladatok lapján.

### <a name="modify-hello-process-server-used-for-replication"></a>A replikáláshoz használt hello folyamatkiszolgáló módosítása
1. Nyissa meg **kiszolgálók** > **konfigurációs kiszolgálók** > konfigurációs kiszolgáló > **kiszolgálóadatok**.
2. Kattintson a **folyamat kiszolgálók** > **Folyamatkiszolgáló módosítása** következő toohello-kiszolgálónak toomodify.

    ![Folyamatkiszolgáló módosítása 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)
3. A **Folyamatkiszolgáló módosítása** > **folyamat-célkiszolgálót** válassza ki a kívánt toouse, és válassza ki, hogy szeretné-e tooreplicate toohello új kiszolgáló hello virtuális gépek új kiszolgáló hello. Hello információk ikon következő toohello kiszolgáló nevére kattint a szabad terület és a használt memória részleteit. hello átlagos területet, amely szükséges, hogy elvégezte a döntések betöltése megjelenített toohelp minden kijelölt virtuális gép toohello új folyamatkiszolgáló tooreplicate lesz.

    ![Folyamatkiszolgáló módosítása 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)
4. Kattintson a hello pipa toobegin toohello új folyamatkiszolgáló replikálása. Vegye figyelembe, hogy ha eltávolítja az összes virtuális gépet a folyamatkiszolgáló, de a kritikus azt már nem megjelenjen-e a kritikus figyelmeztetés hello irányítópulton.

## <a name="third-party-software-notices-and-information"></a>Harmadik felek szoftvereivel, megjegyzések és információk
Ne Localize vagy lefordítása

hello szoftver és a belső vezérlőprogram futó hello Microsoft termék vagy szolgáltatás alapul, vagy a szerződés magában foglalja a hello anyag projektek lenti (együttesen "harmadik féltől származó kód").  A Microsoft hello hello harmadik féltől származó kód nem eredeti szerzője.  hello eredeti szerzői és a licenc, amely alatt a Microsoft az ilyen harmadik féltől származó kód kapott alábbi van beállítva.

hello leírtak A harmadik féltől származó kód hello projektek-összetevőt az alább felsorolt kapcsolódik. Ilyen licencek és információkat csak tájékoztatási célokat szolgálnak.  A harmadik féltől származó kód folyamatban van a Microsoft szoftverlicenc-feltételek hello Microsoft termék vagy szolgáltatás a Microsoft által relicensed tooyou.  

b. hello információk kapcsolatos harmadik féltől származó kód összetevők végrehajtott elérhető tooyou Microsoft hello eredeti licencelési feltételei szerint.

hello teljes fájl is található a hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft kifejezetten át nem adott, az összes jogot fenntart által Ennek következménye, hogy belül vagy más módon.
