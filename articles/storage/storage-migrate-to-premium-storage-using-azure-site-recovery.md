---
title: "Prémium szintű Azure Storage Azure Site Recovery segítségével történő |} Microsoft Docs"
description: "Prémium szintű Azure Storage a Site Recovery segítségével telepíthet át a meglévő virtuális gépeken. Prémium szintű Storage nagy teljesítményű, alacsony késésű támogatása az Azure virtuális gépeken futó I/O-igényes munkaterhelések kínál."
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: luywang
ms.openlocfilehash: cc364bdae49068a50ec86c537c3b878670b8b8b7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="migrating-to-premium-storage-using-azure-site-recovery"></a>Áttelepítés a Premium Storage-ba az Azure Site Recoveryvel

[Prémium szintű Storage](storage-premium-storage.md) nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek (VM) támogatását biztosítja. Ez az útmutató célja segítség a felhasználóknak át a virtuális gépek lemezei egy standard szintű tárfiók egy prémium szintű storage-fiók használatával [Azure Site Recovery](../site-recovery/site-recovery-overview.md).

Webhely-helyreállítás az Azure-szolgáltatások, amely elősegíti az üzleti folytonossági és vészhelyreállítási helyreállítási stratégia megvalósításában a helyszíni fizikai kiszolgálóknak és virtuális gépek replikálása az Azure-felhőbe vagy egy másodlagos adatközpontba. Ha az elsődleges helyen valamilyen okból kimaradás lép fel, hogy átadja a másodlagos helynek, így az alkalmazások és számítási feladatok nem állnak. Ön nem az elsődleges helyre, amikor visszatér a normál működés. A Site Recovery vészhelyreállítások részletezésének támogatásához anélkül, hogy befolyásolná a termelési környezetben feladatátvételi teszteket biztosít. A feladatátvételeket a váratlan vészhelyzetek esetére (replikáció gyakoriságától függően) minimális adatvesztéssel is futtathatja. Prémium szintű Storage áttelepítésének esetben használhatja a [a Site Recovery feladatátvételi](../site-recovery/site-recovery-failover.md) az Azure Site Recovery céllemezek át a prémium szintű storage-fiók.

Ajánlott, hogy az áttelepítés prémium szintű Storage, a Site Recovery használatával, mert ezt a lehetőséget biztosít a minimális állásidővel, és ezzel elkerülheti a manuális végrehajtási lemásolná a lemezeket, és új virtuális gépek létrehozása. A Site Recovery rendszeresen a lemezek másolása, és hozzon létre új virtuális gépek a feladatátvétel során. A Site Recovery támogatja a feladatátvételt a minimális típusú számos vagy állásidő nélkül. Tervezze meg az állásidőt, és adatvesztés becsléséhez, tekintse meg a [feladatátvételi típusú](../site-recovery/site-recovery-failover.md) tábla a Site Recovery szolgáltatásban. Ha Ön [Felkészülés az Azure virtuális gépek a feladatátvételt követően csatlakozhatnak](../site-recovery/site-recovery-vmware-to-azure.md), kell kapcsolódnia kell az Azure virtuális gép feladatátvételt követően RDP segítségével.

![][1]

## <a name="azure-site-recovery-components"></a>Az Azure Site Recovery-összetevők

Ezek azok a Site Recovery-összetevők, amelyek az áttelepítési forgatókönyvhöz kapcsolódó.

* **Konfigurációs kiszolgáló** van egy Azure virtuális Gépen, amely kommunikáció koordinálását, és az adatok replikációs és helyreállítási folyamatok kezeli. A virtuális gép telepítéséhez, a konfigurációs kiszolgáló és az új összetevőt, a folyamat kiszolgáló replikációs átjáróként egyetlen telepítőfájl kell futtatnia. További információ a [konfigurációs kiszolgáló Előfeltételek](../site-recovery/site-recovery-vmware-to-azure.md). Konfigurációs kiszolgáló csak egyszer kell beállítani, és nem használható ugyanabban a régióban összes áttelepítéshez.

* **Folyamatkiszolgáló** replikációs adatokat fogadó forrás virtuális gép replikációs átjáró optimalizálja az adatokat a gyorsítótárazás, tömörítés és titkosítás, és elküldi a storage-fiók. Kezeli a forrás virtuális gépeknek a mobilitási szolgáltatás leküldéses telepítéséhez és a forrás virtuális gépek automatikus felderítést hajt végre. Az alapértelmezett folyamatkiszolgáló a konfigurációs kiszolgálón van telepítve. A telepítés méretezésére további önálló folyamatot kiszolgálók telepítése. További információ a [ajánlott eljárások a folyamatkiszolgáló telepítésének](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) és [további folyamat kiszolgálók üzembe helyezésével](../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Folyamatkiszolgáló csak egyszer kell beállítani, és nem használható ugyanabban a régióban összes áttelepítéshez.

* **Mobilitásiszolgáltatás** minden szabványos replikálni kívánt virtuális gépen központilag telepített összetevője. A normál virtuális Gépen végbemenő adatírásokat, és továbbítja őket a folyamatkiszolgálónak. További információ a [replikált gép Előfeltételek](../site-recovery/site-recovery-vmware-to-azure.md).

A kép bemutatja, hogyan működnek együtt ezeket az összetevőket.

![][15]

> [!NOTE]
> A Site Recovery nem támogatja a tárolóhelyek lemezek az áttelepítés.

A további összetevők az egyéb forgatókönyvek, tekintse meg [kialakítandó architektúra](../site-recovery/site-recovery-vmware-to-azure.md).

## <a name="azure-essentials"></a>Az Azure alapjai

Ezek azok az áttelepítési forgatókönyv szerint az Azure-követelményeknek.

* Azure-előfizetés
* Egy prémium szintű Azure storage-fiókot a replikált adatok tárolásához
* Egy Azure virtuális hálózatot (VNet), amelyhez virtuális gépek csatlakozni fognak Ha feladatátvétel jönnek létre. Az Azure virtuális hálózat ugyanabban a régióban, amelyben a Site Recovery fut azonosnak kell lennie.
* Egy Azure standard szintű tárfiók replikálási naplók tárolására. Ez lehet ugyanazt a tárfiókot, az áttelepítés alatt álló virtuális gépek lemezei

## <a name="prerequisites"></a>Előfeltételek

* Az előző szakaszban leírt megfelelő áttelepítési forgatókönyv-összetevők megismerése
* Tervezze meg a leállás megismerését által a [feladatátvételi a Site Recovery szolgáltatásban](../site-recovery/site-recovery-failover.md)

## <a name="setup-and-migration-steps"></a>Telepítés és áttelepítés lépései

A Site Recovery segítségével Azure IaaS virtuális gépeket áttelepíteni, régiók közötti vagy régión belül. Az alábbi utasításokat kell lett különböző áttelepítési forgatókönyvhöz a cikkből [replikálja a VMware virtuális gépek vagy fizikai kiszolgálók Azure-bA](../site-recovery/site-recovery-vmware-to-azure.md). Kövesse a hivatkozást, a részletes lépéseket további a jelen cikkben lévő utasítások.

1. **Recovery Services-tároló létrehozása**. Létrehozásához és kezeléséhez a Site Recovery-tároló keresztül a [Azure-portálon](https://portal.azure.com). Kattintson a **új** > **felügyeleti** > **biztonsági mentés** és **(OMS) helyreállítási hely**. Másik lehetőségként kattinthat **Tallózás** > **Recovery Services-tároló** > **Hozzáadás**. Virtuális gépek replikálása a régióban, akkor ebben a lépésben adja meg. Ugyanabban a régióban áttelepítéshez válassza ki a régiót, amelyben a forrás virtuális gépeket és a forrás storage-fiókok is. Vegye figyelembe, hogy prémium szintű tárfiókokba történő áttelepítés csak akkor támogatott a a [Azure-portálon](https://portal.azure.com), nem a [klasszikus portál](https://manage.windowsazure.com).

2. A következő lépések segítenek **védelmi célok megválasztása**.

    2a. Nyissa meg a virtuális gép, ahol a konfigurációs kiszolgáló telepíteni szeretné, a [Azure-portálon](https://portal.azure.com). Ugrás a **Recovery Services-tárolók** > **beállítások**. A **beállítások**, jelölje be **Site Recovery**. A **Site Recovery**, jelölje be **1. lépés: az infrastruktúra előkészítése**. A **infrastruktúra előkészítése**, jelölje be **védelmi cél**.

    ![][2]

    2b. A **védelmi cél**, az első legördülő listában válassza ki a **az Azure-bA**. A második legördülő listában válassza ki a **nem virtualizált vagy egyéb**, és kattintson a **OK**.

    ![][3]

3. A következő lépések segítenek **forrás környezetet (konfigurációs kiszolgáló)**.

    3a. Töltse le a **Azure Site Recovery az egységes telepítő** és a **tárolóbeli regisztrációs kulcs** nyissa meg a **infrastruktúra előkészítése** > **forrás előkészítése** > **kiszolgáló hozzáadása** panelen. Szüksége lesz a tárolóbeli regisztrációs kulcs az egységes telepítő futtatásához. A kulcs a generálásától számított öt napig érvényes.

    ![][4]

    3b. A konfigurációs kiszolgáló hozzáadása a **kiszolgáló hozzáadása** panelen.

    ![][5]

    3 c.. A virtuális gép, mint a konfigurációs kiszolgálót használ futtassa a konfigurációs kiszolgáló és a folyamatkiszolgáló telepítése az egységes telepítő. A pillanatképek révén végigvezetheti [Itt](../site-recovery/site-recovery-vmware-to-azure.md) a telepítés befejezéséhez. A megadott áttelepítési forgatókönyvhöz lépéseket a következő képernyőképeket hivatkozhat.

    Az **Előkészületek** területen válassza **A konfigurációs kiszolgáló és a folyamatkiszolgáló telepítése** lehetőséget.

    ![][6]

    3D. A **regisztrációs**, keresse meg és jelölje ki a regisztrációs kulcs, amelyikbe kibontotta a tárolóban.

    ![][7]

    3e. A **Környezet részletei** területen válassza ki, hogy kívánja-e replikálni a VMware virtuális gépeket. Válassza ki az áttelepítési forgatókönyv szerint **nem**.

    ![][8]

    3f. Miután a telepítés befejeződött, megjelenik a **Microsoft Azure Site Recovery konfigurációs kiszolgáló** ablak. Használja a **fiókok kezelése** lap használatával összeállíthatja a fiókot, hogy a Site Recovery automatikus felderítést is használhat. (A forgatókönyv védelme fizikai gépek, a fiók beállítása nem megfelelő, de az alábbi lépések egyikét ahhoz legalább egy fiók szükséges. Ebben az esetben az nevet adhat a fiók és jelszó, mint bármelyik.) Használja a **tároló regisztrációs** lapján töltse fel a tárolói hitelesítő adatok fájlját.

    ![][9]

4. **A célkörnyezet beállítása**. Kattintson a **infrastruktúra előkészítése** > **cél**, és adja meg a virtuális gépek a feladatátvételt követően használni kívánt telepítési modellt. Választhat **klasszikus** vagy **erőforrás-kezelő**, attól függően, a forgatókönyv.

    ![][10]

    A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal. Vegye figyelembe, hogy a replikált adatok használata a prémium szintű storage-fiók, akkor be kell állítania egy további standard szintű tárfiók replikálási naplók tárolásához.

5. **Replikációs beállítások megadása**. Kövesse az [replikációs beállítások megadása](../site-recovery/site-recovery-vmware-to-azure.md) annak ellenőrzéséhez, hogy a konfigurációs kiszolgáló sikeresen társítva a replikációs házirendet hoz létre.

6. **Kapacitástervezés**. Használja a [kapacitástervező](../site-recovery/site-recovery-capacity-planner.md) pontosan a hálózati sávszélesség, tárolási és egyéb követelmények teljesítéséhez a replikáció felbecsülni-nak szüksége van. Amikor elkészült, válassza ki a **Igen** a **elvégezte a kapacitástervezést?**.

    ![][11]

7. A következő lépések segítenek **telepíteni a mobilitási szolgáltatást, és engedélyezze a replikálást**.

    7a. Dönthet úgy, hogy [leküldéses telepítése](../site-recovery/site-recovery-vmware-to-azure.md) a forrás virtuális gépeket vagy [manuálisan telepíteni a mobilitási szolgáltatás](../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) a forrás virtuális gépeken. A követelmény a telepítési és a kézi telepítő elérési útját küldését a megadott hivatkozás található. Akkor használatos, ha a manuális telepítés, szükség lehet egy belső IP-címet használja a konfigurációs kiszolgáló található.

    ![][12]

    Virtuális gép feladatátadása két ideiglenes lemezek fog rendelkezni: egyet az elsődleges virtuális gép, a másik virtuális gép helyreállítási régióban kiépítés során létrehozott. Az ideiglenes a(z) replikáció előtt, telepítse a mobilitási szolgáltatást a replikáció engedélyezése előtt. Az ideiglenes lemez kihagyása kapcsolatos további tudnivalókért lásd [lemezek kizárása a replikációból](../site-recovery/site-recovery-vmware-to-azure.md).

    7b. Most már engedélyezheti a replikációt a következők szerint:
      * Kattintson a **alkalmazás replikálása** > **forrás**. Replikáció első alkalommal történő engedélyezését, kattintson a + replikálja a további gépek replikációjának engedélyezése a tárolóban lévő állapottal.
      * Az 1. lépésben forrásának beállítása a folyamat-kiszolgálóként.
      * A 2. lépésben adja meg a feladatátvételt követően üzembe helyezési modellel, a prémium szintű tárfiók át, egy standard szintű tárfiókot naplókat, és hogy áthelyezze a virtuális hálózat mentéséhez.
      * A 3. lépésben adja hozzá a védett virtuális gépek IP-címe (szükség lehet egy belső IP-cím kereséséhez őket).
      * A 4. lépésben a fiókok beállítása korábban a folyamatkiszolgáló kiválasztásával tulajdonságainak konfigurálása.
      * 5. lépésben válassza ki a replikációs házirendet, korábban létrehozott replikációs beállítások megadása.
      Kattintson a **OK** és engedélyezze a replikálást.

    > [!NOTE]
    > Ha egy Azure virtuális gép felszabadítása. lehetséges, és újra elindítani, akkor nem garantálja, hogy megkapja-e az azonos IP-cím. Ha megváltoztatja a konfigurációs kiszolgáló/folyamat kiszolgáló vagy a védett Azure virtuális gépek IP-címét, előfordulhat, hogy ebben a forgatókönyvben a replikálás nem működik megfelelően.

    ![][13]

    Az Azure Storage környezet tervezésekor azt javasoljuk, hogy használja-e külön storage-fiókok az egyes virtuális gépek rendelkezésre állási csoportba. Azt javasoljuk, hogy kövesse az ajánlott eljárás a tárolási rétegben [több tárfiókot használja minden egyes rendelkezésre állási csoport](../virtual-machines/windows/manage-availability.md). Virtuális gépek lemezei több tárfiókot fokozza lévő tárterület rendelkezésre állását és az i/o elosztja a Azure tároló-infrastruktúra. Ha a virtuális gépek rendelkezésre állási csoportba, ahelyett, hogy azok a virtuális gépek lemezek egy tárfiókot, a magas ajánlott az áttelepítés több virtuális gép többször, így az azonos rendelkezésre állási csoport a virtuális gépek nem ugyanazt a egyetlen storage-fiók. Használja a **replikáció engedélyezése** panelt, és az egyes virtuális gépek, egyenként, a cél tárfiókkal beállítása. Kiválaszthatja a feladatátvételt követően telepítési modell az igényeknek megfelelően. Ha úgy dönt, erőforrás-kezelő (RM), a feladatátvételt követően üzembe helyezési modellel, átveheti az erőforrás-kezelő virtuális gép egy erőforrás-kezelő virtuális Gépet, vagy átveheti a klasszikus virtuális gépek egy erőforrás-kezelő virtuális gépre.

8. **Feladatátvételi teszt futtatása**. Ellenőrizze, hogy a replikáció befejeződött, kattintson a helyreállítás, és kattintson a **beállítások** > **replikált elemek**. Az állapot és a replikáció során százalékos jelenik meg. Kezdeti replikáció befejeződése után ellenőrizze a replikációs stratégiájának feladatátvételi teszt futtatásához. A részletes lépéseket a feladatátvételi tesztet, tekintse meg [feladatátvételi teszt futtatása a Site Recovery](../site-recovery/site-recovery-vmware-to-azure.md). A feladatátvételi teszt állapota látható **beállítások** > **feladatok** > **YOUR_FAILOVER_PLAN_NAME**. A panelen megjelenik a lépéseket és a sikeres és sikertelen eredmények részletes információkat. A feladatátvételi tesztet bármely lépése meghiúsul, ha kattintson a lépést a tekintse meg a hibaüzenetet. Győződjön meg arról, hogy a virtuális gépek és a replikációs stratégiájának megfelelnek a feladatátvétel futtatása előtt. Olvasási [Azure Site Recovery a feladatátvételi teszt](../site-recovery/site-recovery-test-failover-to-azure.md) további információt és útmutatást a feladatátvételi tesztet.

9. **Feladatátvételt**. A vizsgálat után feladatátvétel befejeződött, a lemezek áttelepítése a prémium szintű Storage, és replikálja a Virtuálisgép-példányok feladatátvételt. Kövesse a részletes lépéseket a [feladatátvételt](../site-recovery/site-recovery-failover.md#run-a-failover). Válasszon **virtuális gépek leállítása és a legfrissebb adatok szinkronizálása** adhatja meg, hogy a Site Recovery végezzen el a védett virtuális gépek leállítása és szinkronizálja az adatokat, hogy a legújabb adatok átvétele fog megtörténni. Ha nem ezt a beállítást, vagy a kísérlet nem jár sikerrel a feladatátvétel nem a virtuális gép számára a legújabb elérhető helyreállítási pontból. A Site Recovery hoz létre egy Virtuálisgép-példány, amelynek típusa megegyezik, vagy hasonló egy prémium szintű Storage képes virtuális géphez. Ellenőrizheti a teljesítmény és a különböző Virtuálisgép-példányok árát a [Windows Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) vagy [Linux virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Az áttelepítést követő lépések

1. **A rendelkezésre állási csoportot, ha van ilyen a replikált virtuális gépek**. A Site Recovery nem támogatja a rendelkezésre állási csoport együtt áttelepítése virtuális gépek. Attól függően, hogy a replikált virtuális gép telepítése esetén tegye a következők egyikét:
  * A virtuális gépek, a klasszikus telepítési modellel készült: a virtuális gép hozzáadása a rendelkezésre állási csoportot az Azure portálon. A részletes lépéseket lásd a [hozzáadása egy meglévő virtuális gép rendelkezésre állási csoportok](../virtual-machines/windows/classic/configure-availability.md#addmachine).
  * A Resource Manager telepítési modell: a virtuális gép a konfiguráció mentéséhez, és törölje és hozza létre a virtuális gépek a rendelkezésre állási csoport. Ehhez használja a következő parancsfájl [beállítása Azure Resource Manager virtuális gép rendelkezésre állási csoport](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Ellenőrizze a parancsfájl a korlátozást, és állásidő tervezze meg a parancsfájl futtatása előtt.

2. **Törölje a régi virtuális gépek és lemezek**. Mielőtt törölné ezeket, ellenőrizze, hogy a Premium lemezek megegyeznek a forráslemezekhez és az új virtuális gépek a forrás virtuális gépeknek ugyanazon művelet végrehajtására szolgál(nak). Az erőforrás-kezelő (RM) üzembe helyezési modellel törölje a virtuális Gépet, és a lemezek törlése a forrás storage-fiókok az Azure portálon. A klasszikus üzembe helyezési modellel törölheti a virtuális gép és a lemezek a klasszikus portálon vagy az Azure-portálon. Ha a probléma, amelyben a lemez nem törlődik annak ellenére, hogy törli a virtuális gép van, olvassa el a [hibák elhárítása a virtuális merevlemezek törlésekor](storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Az Azure Site Recovery-infrastruktúra tiszta**. Ha már nincs szükség a Site Recovery, törölheti is az infrastruktúra replikált elemek, a konfigurációs kiszolgáló és a helyreállítási irányelv törlésével, és ezután törlése az Azure Site Recovery-tárolóban.

## <a name="troubleshooting"></a>Hibaelhárítás

* [Figyelése és hibaelhárítása szempontjából a virtuális gépek és fizikai kiszolgálók védelme](../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [A Microsoft Azure Site Recovery-fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Következő lépések

A virtuális gépek meghatározott forgatókönyvek a következő cikkekben találhat:

* [Az Azure virtuális gépek közötti Storage-fiókok áttelepítése](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Hozzon létre, és a Windows Server VHD feltöltése az Azure-bA.](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Létrehozás és a Linux operációs rendszert tartalmazó virtuális merevlemez feltöltése](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Virtuális gépek áttelepítési Amazon AWS a Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

A következő források további információt az Azure Storage és az Azure virtuális gépek lásd még:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Az Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz](storage-premium-storage.md)

[1]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png
