---
title: "Azure Site Recovery Deployment Planner a Hyper-V – Azure-hoz| Microsoft Docs"
description: "Ez a cikk az Azure Site Recovery Deployment Planner Hyper-V – Azure forgatókönyvére vonatkozó jelentést ismerteti."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: 9340fe48c1da874d6c0cf02c026e5dec6ddabbe7
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/15/2017
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report"></a>Az Azure Site Recovery Deployment Planner jelentés elemzése
Az elkészített Microsoft Excel-jelentés a következő táblázatokat tartalmazza:

## <a name="on-premises-summary"></a>Helyszíni összefoglalás
A Helyszíni összefoglalás munkalap áttekintést nyújt a vizsgált Hyper-V környezet ![Helyszíni összefoglalásáról](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-summary-h2a.png)

**Start Date** és **End Date** (Kezdő dátum és záró dátum): Azon profilkészítési adatok kezdő és záró dátuma, amelyekről jelentést kíván készíteni. Alapértelmezés szerint a kezdő dátum az a dátum, amikor a profilkészítés megkezdődik, a záró dátum pedig az a dátum, amikor a profilkészítés leáll. Ezek lehetnek a StartDate és EndDate értékek, ha a jelentést ezekkel a paraméterekkel hozza létre.

**Total number of profiling days** (Profilkészítés napjainak teljes száma): A jelentéskészítés kezdő és záró dátuma között eltelt profilkészítési napok teljes száma.

**Number of compatible virtual machines** (Kompatibilis virtuális gépek száma): Az olyan kompatibilis virtuális gépek teljes száma, amelyekhez a rendszer kiszámolta szükséges hálózati sávszélességet, valamint a szükséges tárfiókok és Microsoft Azure-magok számát.

**Total number of disks across all compatible virtual machines** (Lemezek teljes száma az összes kompatibilis virtuális gépen): Az összes kompatibilis virtuális gépen található összes lemez.

**Average number of disks per compatible virtual machine** (Lemezek átlagos száma kompatibilis virtuális gépenként): Az összes kompatibilis virtuális gép alapján számított átlagos lemezszám.

**Average disk size (GB)** (Átlagos lemezméret (GB)): Az összes kompatibilis virtuális gép alapján számított átlagos lemezméret.

**Desired RPO (minutes)** (Kívánt RPO (perc)): A helyreállításipont-célkitűzés alapértelmezett értéke vagy a jelentéskészítéskor a „DesiredRPO” paraméterhez szükséges sávszélesség becslése érdekében megadott érték.

**Desired bandwidth (Mbps)** (Kívánt sávszélesség (Mbps)): A jelentéskészítéskor az elérhető RPO becslése érdekében a „Bandwidth” paraméterhez megadott érték.

**Observed typical data churn per day (GB)** (Megfigyelt átlagos napi adatváltozás (GB)): Az összes profilkészítési napon megfigyelt átlagos adatváltozás.

## <a name="recommendations"></a>Javaslatok  
A Hyper-V – Azure jelentés javaslati táblázatában a következő részletek találhatók (a kiválasztott RPO szerint):

![Hyper-V – Azure jelentéshez kapcsolódó javaslatok](media/site-recovery-hyper-v-deployment-planner-analyze-report/Recommendations-h2a.png)

### <a name="profile-data"></a>Profiladatok
![Profiladatok](media/site-recovery-hyper-v-deployment-planner-analyze-report/profile-data-h2a.png)

**Profiled data period** (Profilkészítési adatok létrehozásának időtartama): A profilkészítés futtatásának időtartama. Az eszköz alapértelmezés szerint a profilkészítés során létrehozott minden adatot számításba vesz. Ha használta a StartDate és az EndDate lehetőséget a jelentés létrehozásakor, a rendszer a meghatározott időszakra vonatkozóan hozza azt létre. 

**Number of Hyper-V servers profiled** (Vizsgált Hyper-V kiszolgálók száma): Azon Hyper-V kiszolgálók száma, amelyek virtuális gépeiről jelentés készül. Kattintson a számra a Hyper-V kiszolgálók nevének megjelenítéséhez. Ekkor megnyílik a helyszíni tárolóra vonatkozó követelmények táblázata, amelyen minden kiszolgáló fel van sorolva a hozzá kapcsolódó tárolási követelménnyel.    

**Desired RPO** (Kívánt RPO): Az üzembe helyezés kívánt helyreállítási időkorlátja. Alapértelmezés szerint a szükséges hálózati sávszélesség kiszámítása 15, 30 és 60 perces RPO értékkel történik. A kiválasztástól függően az eszköz frissíti az érintett értékeket a munkalapon. Ha használta a DesiredRPOinMin paramétert a jelentés elkészítése közben, akkor az az érték jelenik meg a kívánt helyreállítási időkorlát eredményei között.

### <a name="profiling-overview"></a>Profilkészítés áttekintése
![Profilkészítés áttekintése](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-h2a.png)

**Total Profiled Virtual Machines** (Minden virtuális gép, amelyről profil készült): Azon virtuális gépek teljes száma, amelyekről profil készült. Ha a VMListFile olyan virtuális gépek nevét is tartalmazza, amelyekről nem készült profil, a rendszer ezeket figyelmen kívül hagyja a jelentéskészítés során, és nem számítja bele őket a profilkészítésben részt vevő virtuális gépek teljes számába.

**Compatible Virtual Machines** (Kompatibilis virtuális gépek): Azon virtuális gépek száma, amelyek az Azure-ban védhetők az Azure Site Recovery használatával. Az olyan kompatibilis virtuális gépek teljes száma, amelyekhez a rendszer kiszámolta szükséges hálózati sávszélességet, valamint a szükséges tárfiókok és Azure-magok számát. Az egyes kompatibilis virtuális gépek részletei elérhetők a „Compatible VMs” (Kompatibilis virtuális gépek) szakaszban.

**Incompatible Virtual Machines** (Nem kompatibilis virtuális gépek): Azon virtuális gépek száma, amelyekről profil készült, és amelyek nem védhetők az Azure Site Recoveryvel. Az inkompatibilitás okait a „Nem kompatibilis virtuális gépek” című szakaszban olvashatja. Ha a VMListFile olyan virtuális gépek nevét is tartalmazza, amelyekről nem készült profil, a rendszer nem számítja bele őket a nem kompatibilis virtuális gépek számába. Az ilyen virtuális gépek az „Incompatible VMs” (Nem kompatibilis virtuális gépek) szakasz végén, „Data not found” (Nem található adat) megjelöléssel láthatók.

**Desired RPO**(Kívánt RPO): A kívánt helyreállítási időkorlát, percben megadva. Az eszköz három helyreállítási időkorlát értékről készít jelentést: 15 (alapértelmezett érték), 30 és 60 perc. A jelentésben szereplő ajánlott sávszélesség attól függően változik, hogy melyik lehetőséget választja a munkalap jobb felső sarkában található Desired RPO (Kívánt RPO) legördülő listából. Ha egyéni értékű kívánt RPO paraméterrel készítette el a jelentést, ez az egyéni érték jelenik meg alapértelmezett értékként a Desired RPO (Kívánt RPO) legördülő listában.

### <a name="required-network-bandwidth-mbps"></a>Szükséges hálózati sávszélesség (Mbps)
![Szükséges hálózati sávszélesség](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-network-bandwidth-h2a.png)

**To meet RPO 100 percent of the time** (Az RPO betartása az idő 100 százalékában): Az ajánlott sávszélesség (Mbps), amelyet ki kell osztania, ha mindig garantáltan el kívánja érni a kívánt helyreállítási időkorlátot. Ekkora sávszélességet kell kijelölnie az összes kompatibilis virtuális gép stabil állapotú változásreplikációjához, ha biztosan ki kívánja zárni a helyreállítási időkorlát megszegését.

**To meet RPO 90 percent of the time** (Az RPO betartása az idő 90 százalékában): Ha a szélessávú kapcsolat díjszabása miatt vagy egyéb okból kifolyólag nincs lehetősége arra, hogy akkora sávszélességet állítson be, amellyel a kívánt helyreállítási időkorlát mindig betartható, akkor választhat egy alacsonyabb sávszélesség-beállítást, amellyel az idő 90 százalékában betarthatja a kívánt RPO-t. Az alacsonyabb sávszélesség-beállítás következményeinek áttekintéséhez a jelentés lehetőségelemzést biztosít elemzést a helyreállítási időkorlát várható megszegéseinek számáról és időtartamáról.

**Achieved Throughput** (Elért átviteli sebesség): Az átviteli sebesség a GetThroughput parancs futtatásához használt kiszolgáló és a tárfióknak helyet adó Azure-régió között. Ez az átvitelisebesség-érték azt a becsült szintet jelöli, amely akkor érhető el, ha a kompatibilis virtuális gépeket az Azure Site Recoveryvel védi, feltéve, hogy a Hyper-V kiszolgáló tárolási és hálózati jellemzői megegyeznek azon kiszolgálóéval, amelyen az eszközt futtatta.

Az Azure Site Recovery minden vállalati üzemelő példánya esetében az [ExpressRoute](https://aka.ms/expressroute) használata javasolt.

### <a name="required-storage-accounts"></a>Szükséges tárfiókok
A következő diagram az összes kompatibilis virtuális gép védelméhez szükséges (standard és prémium szintű) tárfiókok teljes számát mutatja. A „VM-storage placement” (Virtuálisgép-tároló elhelyezése) szakaszból megtudhatja, hogy melyik tárfiókot melyik virtuális géphez használhatja.

![Szükséges tárfiókok](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-storage-accounts-h2a.png)

### <a name="required-number-of-azure-cores"></a>Szükséges Azure-magok száma
Ez az eredmény az összes kompatibilis virtuális gép feladatátvétele vagy feladatátvételi tesztje előtt beállítandó magok teljes száma. Ha az előfizetés túl kevés maggal rendelkezik, az Azure Site Recovery nem tudja létrehozni a virtuális gépeket a feladatátvételi teszt vagy a tényleges feladatátvétel alatt.
![Szükséges Azure-magok száma](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-number-of-azure-cores-h2a.png)


### <a name="additional-on-premises-storage-requirement"></a>A helyszíni tárolásra vonatkozó további követelmények

A Hyper-V kiszolgálók sikeres replikációjához és a változásreplikációhoz szükséges teljes szabad tárterület, amelynek megléte esetén a virtuális gépek replikációja semmiképpen nem okozza a termelési alkalmazások nem kívánt leállását. Az egyes kötetkövetelmények részletei elérhetők a [helyszíni tárterület-követelmény](#on-premises-storage-requirement) alatt. 

További információt arról, hogy a replikációhoz miért van szükség szabad helyre, a [Helyszíni tárterület-követelmény](#why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication) című részben találhat.

![helyszíni tárolásra vonatkozó követelmény és másolási gyakoriság](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-and-copy-frequency-h2a.png)

### <a name="maximum-copy-frequency"></a>Maximális másolási gyakoriság
Az ajánlott maximális másolási gyakoriságot be kell állítani ahhoz, hogy a replikáció elérje a kívánt RPO-t. Az alapértelmezett érték 5 perc. A jobb RPO elérése érdekében beállíthat 30 másodperces másolási gyakoriságot.

### <a name="what-if-analysis"></a>Lehetőségelemzés
![Lehetőségelemzés](media/site-recovery-hyper-v-deployment-planner-analyze-report/what-if-analysis-h2a.png) Ez az elemzés kiemeli, hogy hányszor szegné meg a helyreállítási időkorlátot a profilkészítés időtartama alatt, ha alacsonyabb sávszélességet állít be, amely az idő 90 százalékában biztosítaná a kívánt RPO betartását. A helyreállítási időkorlát túllépése bármelyik nap egy vagy több alkalommal is előfordulhat. Az ábra a napi RPO-csúcsértéket mutatja. Az elemzés alapján eldöntheti, hogy az RPO-túllépések teljes száma az összes nap alatt, valamint a napi RPO-csúcsérték elfogadható-e a megadott alacsonyabb sávszélesség esetén. Ha elfogadható, akkor lefoglalhatja az alacsonyabb sávszélességet a replikációhoz. Ha nem, akkor foglalja le a javasolt magasabb sávszélességet ahhoz, hogy az idő 100 százalékában betarthassa az RPO-t. 

### <a name="recommendation-for-successful-initial-replication"></a>Javaslatok a sikeres kezdeti replikációhoz
Ebben a szakaszban a virtuális gépeket védő kötegek mennyiségére, valamint a kezdeti replikáció (IR) sikeres teljesítéséhez szükséges minimális sávszélességre vonatkozó javaslatokat olvashat. 

![IR-kötegelés](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-h2a.png)

A virtuális gépet a megadott kötegelési sorrendben kell levédeni. Minden egyes köteg meghatározott virtuális gépek listáját tartalmazza. Az 1. kötegbe tartozó virtuális gépeket a 2. kötegbe tartozók előtt kell levédeni. A 2. kötegbe tartozó virtuális gépeket a 3. kötegbe tartozók előtt kell levédeni, és így tovább. Ha az 1. kötegbe tartozó virtuális gépek replikációja befejeződött, engedélyezheti a 2. kötegbe tartozó virtuális gépekét. Ha a 2. kötegbe tartozó virtuális gépek replikációja befejeződött, ugyanígy engedélyezheti a 3. kötegbe tartozó virtuális gépekét. Ha nem követi a kötegelési sorrendet, előfordulhat, hogy nem lesz elérhető elegendő sávszélesség, amely a később levédendő virtuális gépek replikációjához szükséges. Ennek eredményeképpen a virtuális gépek vagy nem tudják majd befejezni a kezdeti replikációt, vagy előfordulhat, hogy több virtuális gép újraszinkronizálási üzemmódba kerül. A kiválasztott RPO-táblázatra vonatkozó IR-kötegelés részletes információkat tartalmaz arról, hogy mely virtuális gépeket kell belevenni a kötegekbe.

Az itt látható diagramon a kezdeti és változásreplikáció sávszélesség-elosztása látható kötegekre lebontva, a megadott kötegelési sorrend szerint. Az első kötegbe tartozó virtuális gépek levédésekor a kezdeti replikáció során a teljes sávszélesség elérhető. Ha az első köteg kezdeti replikációja befejeződött, a sávszélesség egy részére szükség lesz a változásreplikációhoz. A fennmaradó sávszélesség elérhető lesz a második köteg virtuális gépeinek replikációjához. A 2. köteg oszlopában az 1. köteg virtuális gépeinek változásreplikációjához szükséges sávszélesség, valamint a 2. köteg virtuális gépeinek kezdeti replikációjához elérhető sávszélesség látható. Ehhez hasonló módon a 3. köteg oszlopa a korábbi kötegek (1. és 2.) virtuális gépeinek változásreplikációjához szükséges sávszélességet, valamint a 3. köteg virtuális gépeinek kezdeti replikációjához elérhető sávszélességet tartalmazza, és így tovább.  Ha az összes köteg kezdeti replikációja befejeződött, az utolsó oszlopban az összes virtuális gép változásreplikációjához szükséges sávszélesség látható. 

**Miért van szükség a kezdeti replikáció kötegelésére?**
A kezdeti replikáció befejezési ideje a virtuális gépek lemezeinek méretétől, a felhasznált lemezterülettől és az elérhető hálózati átviteli sebességtől függ. A részletek a kiválasztott RPO-táblázat IR-kötegelésében érhetők el.

### <a name="cost-estimation"></a>Költségbecslés
Az ábra a választott célrégió és a jelentéskészítéshez megadott pénznem vészhelyreállításának (DR) becsült, teljes költségeinek összesített nézetét mutatja be az Azure-hoz.
![Költségbecslés összefoglalása](media/site-recovery-hyper-v-deployment-planner-analyze-report/cost-estimation-summary-h2a.png) Az összefoglalás segít megismerni a tárolás, számítás, hálózat és licenc költségeit, amelyet akkor kell fizetnie, ha az Azure-ban lévő minden kompatibilis virtuális gépet az Azure Site Recovery használatával védi. A költségek kiszámítása a kompatibilis virtuális gépek, nem pedig a profilkészítésben részt vevő virtuális gépek alapján történik.  
 
A költségeket havi vagy éves bontásban tekintheti meg. További információkat olvashat a [támogatott célrégiókról](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) és a [támogatott pénznemekről](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies).

**Összetevők szerinti költségek**: A vészhelyreállítás teljes költsége négy összetevőből áll: Számítás, Tárterület, Hálózat és az Azure Site Recovery licencköltségei. A költségeket a használat alapján számoljuk ki, amely költségek a replikáció során és a számítás, tárterület (prémium és standard), a helyszíni hely és az Azure között konfigurált ExpressRoute/VPN, valamint az Azure Site Recovery licence DR működési ideje során lépnek fel.

**Állapot szerinti költségek**: A vészhelyreállítás (DR) teljes költségeinek kategóriái két különböző állapoton alapulnak – Replikáció és DR működése. 

**Replikáció költségei**: A replikáció során felmerülő költségek. Ez fedezi a tárolás, a hálózat és az Azure Site Recovery-licenc költségeit. 

**DR működés költségei**: A feladatátvételi tesztek során felmerülő költségek. Az Azure Site Recovery virtuális gépeket indít el a feladatátvételi tesztek során. A DR működési költségei a futó virtuális gépek számítási és tárolási költségeit fedezik. 

**Az Azure Storage tárterület éves/havi költségei** A tárterület teljes költségeit mutatja, amelyek a replikációhoz és a DR működésekor használt prémium és standard tárterület esetén merülnek fel.
A virtuális gépenkénti részletes költségelemzést a [Költségbecslés](site-recovery-hyper-v-deployment-planner-cost-estimation.md) táblázatban tekintheti meg.

### <a name="growth-factor-and-percentile-values-used"></a>A használt növekedési tényező és százalékértékek
A munkalap alján található szakasz a profilkészítésben részt vevő virtuális gépek összes teljesítményszámlálójához használt százalékértéket (az alapértelmezett érték 95 százalék), valamint az összes számításban használt növekedési tényezőt (az alapértelmezett érték 30 százalék) mutatja.

![A használt növekedési tényező és százalékértékek](media/site-recovery-hyper-v-deployment-planner-run/growth-factor-max-percentile-value.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Javaslatok az elérhető sávszélesség bemenetként való megadásával
![Profilkészítés áttekintése sávszélesség-bevitellel](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-bandwidth-input-h2a.png)

Előfordulhat olyan helyzet, hogy legfeljebb x Mbps sávszélességet tud beállítani az Azure Site Recovery replikációjára. Az eszközzel megadhatja a rendelkezésre álló sávszélességet (a jelentéskészítés közben, a -Bandwidth paraméterrel), és megkapja az elérhető RPO-t percben megadva. Az elérhető RPO-értéknek ismeretében eldöntheti, hogy szükséges-e további sávszélességet kiépítenie, vagy elfogadja a vészhelyreállítási megoldást ezzel az RPO-val.

![Elérhető RPO](media/site-recovery-hyper-v-deployment-planner-analyze-report/achivable-rpo-h2a.PNG)

## <a name="vm-storage-placement-recommendation"></a>Javaslat a virtuálisgép-tároló elhelyezésére 

![Virtuálisgép-tároló elhelyezése](media/site-recovery-hyper-v-deployment-planner-analyze-report/vm-storage-placement-h2a.png)

**Disk Storage Type** (Lemezes tárolás típusa): A **VMs to Place**(Elhelyezendő virtuális gépek) oszlopban említett összes virtuális gép replikációjához használt standard vagy prémium szintű tárfiók.

**Suggested Prefix** (Javasolt előtag): A tárfiók elnevezéséhez javasolt háromkarakteres előtag. Saját előtagot is használhat, de az eszköz által javasolt nevek követik a [tárfiókok partíció-elnevezési szabályait](https://aka.ms/storage-performance-checklist).

**Suggested Account Name** (Javasolt fióknév): A Storage-fiók neve, a javasolt előtag használata után. Cserélje le a csúcsos zárójelben (< és >) lévő nevet a kívánt értékre.

**Log Storage Account**(Naplótárolási fiók): Minden replikációs naplót egy standard szintű tárfiók tárol. A prémium szintű tárfiókokba replikált virtuális gépek esetében be kell állítani egy további standard szintű tárfiókot a naplók tárolására. Egyetlen standard szintű naplótárolási fiókot több prémium szintű replikációs tárfiók is használhat. A standard szintű tárfiókokba replikált virtuális gépek ugyanazt a tárfiókot használják a naplókhoz is.

**Suggested Log Account Name** (Javasolt naplótárolási fióknév): A naplókat tároló tárfiók neve, a javasolt előtag használata után. Cserélje le a csúcsos zárójelben (< és >) lévő nevet a kívánt értékre.

**Placement Summary** (Elhelyezés összegzése): A tárfiókon található virtuális gépeknek a replikáció és feladatátvételi teszt vagy tényleges feladatátvétel során mért teljes terhelésének összegzése. Tartalmazza a tárfiókhoz rendelt virtuális gépek teljes számát, az Azure Storage-fiókba elhelyezni kívánt összes virtuális gép összesített olvasási/írási IOPS-értékét, az összesített írási (replikációs) IOPS-értéket, az összes lemez teljes beállítási méretét, valamint a lemezek teljes számát.

**Virtual Machines to Place** (Elhelyezendő virtuális gépek): Az összes olyan virtuális gép listája, amelyet az optimális teljesítmény és használat érdekében az adott tárfiókon ajánlott elhelyezni.

## <a name="compatible-vms"></a>Kompatibilis virtuális gépek
Az Azure Site Recovery Deployment Planner által létrehozott Microsoft Excel jelentésben minden kompatibilis virtuális gépre vonatkozó részletet megtalál a „Compatible VMs” (Kompatibilis virtuális gépek) táblázatban.

![Kompatibilis virtuális gépek](media/site-recovery-hyper-v-deployment-planner-analyze-report/compatible-vms-h2a.png)

**VM Name** (Virtuális gép neve): Jelentés létrehozásakor, a VMListFile-ban használt virtuálisgép-név. Ez az oszlop a virtuális gépekhez csatolt lemezek (VHD-k) listáját is megjeleníti. A nevek tartalmazzák a Hyper-V kiszolgálóneveket, amelyeken az eszköz felfedezte a virtuális gépeket a profilkészítési időszakban.

**VM Compatibility** (Virtuálisgép-kompatibilitás): Az érték **Yes** (Igen) és **Yes**\* (Igen) lehet. A **Yes**\* (Igen) azon példányokra vonatkozik, ahol a virtuális gép megfelel az [Azure Premium Storage-nak](https://aka.ms/premium-storage-workload). A profilkészítés során megállapított magas adatváltozású vagy IOPS-értékű lemez megfelel a lemezhez leképezett méretet meghaladó prémium lemezméretnek. A tárfiók a lemez mérete alapján dönti el, hogy melyik prémium szintű lemeztípushoz rendelje hozzá a lemezt. 
* 128 GB alatt P10.
* 128 GB és 256 GB között P15
* 256 GB és 512 GB között P20.
* 512 GB és 1024 GB között P30.
* 1025 GB és 2048 GB között P40.
* 2049 GB és 4095 GB között P50.

Ha például a számítási feladatok jellemzői alapján egy lemez a P20-as vagy P30-as kategóriába tartozik, de a mérete alapján egy alacsonyabb prémium szintű tárolólemez-típusba kerül, az eszköz **Yes**\* (Igen) megjelöléssel látja el ezeket a virtuális gépeket. Az eszköz azt is javasolja, hogy módosítsa a forráslemez méretét, hogy a lemez megfeleljen az ajánlott prémium szintű tárolólemez-típusnak, vagy hogy módosítsa a céllemez típusát a feladatátvétel után.

**Storage Type** (Tároló típusa): Standard vagy Premium.

**Suggested Prefix** (Javasolt előtag): A tárfiók három karakterből álló előtagja.

**Storage Account** (Tárfiók): A javasolt tárfiókelőtagot használó név.

**Peak R/W IOPS (with Growth Factor)** (Maximális írási/olvasási IOPS (növekedési tényezővel)): A lemez írási/olvasási IOPS-csúcsértéke (alapértelmezés szerint a 95. percentilis), beleértve a későbbi növekedési faktort is (az alapértelmezett érték: 30%). Vegye figyelembe, hogy egy virtuális gép teljes írási/olvasási IOPS-értéke nem mindig egyezik meg az egyes virtuálisgép-lemezek írási/olvasási IOPS-értékeinek összegével. Ez azért van, mert a virtuális gép írási/olvasási IOPS-csúcsértéke az egyes lemezeken a profilkészítési időszak alatt percenként mért írási/olvasási IOPS-értékek legmagasabb összege.

**Peak Data Churn in MB/s (with Growth Factor)** (Maximális adatváltozás (MB/s) (növekedési tényezővel)): A lemez adatváltozásának csúcsértéke (alapértelmezés szerint a 95. percentilis), beleértve a későbbi növekedési faktort is (alapértelmezés szerint: 30%). Vegye figyelembe, hogy a virtuális gép teljes adatváltozása nem mindig egyezik meg az egyes lemezek adatváltozásának összegével. Ez azért van, mert a virtuális gép adatváltozásának csúcsértéke az egyes lemezeken a profilkészítési időszak alatt percenként mért adatváltozás-értékek legmagasabb összege.

**Azure VM Size** (Azure-beli virtuális gép mérete): Az Azure Cloud Services-beli hozzárendelt virtuális gép ideális mérete az adott helyszíni virtuális gép esetén. A hozzárendelés a helyszíni virtuális gép memóriáján, a lemezek/magok/hálózati adapterek számán és az olvasási/írási IOPS-értéken alapul. Mindig a legalacsonyabb, a helyszíni virtuális gép összes jellemzőjének megfelelő Azure-beli virtuálisgép-méretet ajánlott használni.

**Number of Disks** (Lemezek száma): A virtuális gép lemezeinek (VHD-inek) teljes száma.

**Disk size (GB)** (Lemez mérete (GB)): A virtuális gép összes lemezének teljes mérete. Az eszköz a virtuális gép egyes lemezeinek méretét is megjeleníti.

**Cores** (Magok): A virtuális gép processzormagjainak száma.

**Memory (MB)** (Memória (MB)): A virtuális gép fizikai memóriája (RAM).

**NICs** (Hálózati adapterek): A virtuális gép hálózati adaptereinek száma.

**Boot Type** (Rendszerindítás típusa): A virtuális gép rendszerindítási típusa. Ez BIOS vagy EFI lehet.

## <a name="incompatible-vms"></a>Nem kompatibilis virtuális gépek
Az Azure Site Recovery Deployment Planner által létrehozott Microsoft Excel jelentésben az összes, nem kompatibilis virtuális gépre vonatkozó részletet megtalálja az „Incompatible VMs” (Nem kompatibilis virtuális gépek) táblázatban.

![Nem kompatibilis virtuális gépek](media/site-recovery-hyper-v-deployment-planner-analyze-report/incompatible-vms-h2a.png)

**VM Name** (Virtuális gép neve): Jelentés létrehozásakor, a VMListFile-ban használt virtuálisgép-név. Ez az oszlop a virtuális gépekhez csatolt lemezek (VHD-k) listáját is megjeleníti. A nevek tartalmazzák a Hyper-V kiszolgálóneveket, amelyeken az eszköz felfedezte a virtuális gépeket a profilkészítési időszakban.

**VM Compatibility** (Virtuális gép kompatibilitása): Azt jelzi, hogy miért nem kompatibilis az adott virtuális gép az Azure Site Recoveryvel való használattal. Az indokok a virtuális gép minden nem kompatibilis lemezénél vannak megadva, és a közzétett [tárhelykorlátok](https://aka.ms/azure-storage-scalbility-performance) alapján a következők lehetnek:

* A lemez mérete nagyobb 4095 GB-nál. Az Azure Storage jelenleg nem támogatja a 4095 GB-nál nagyobb adatlemez-méretet.

* Az 1. generációs virtuális gép (BIOS rendszerindítás-típus) operációsrendszer-lemezének mérete nagyobb 2047 GB-nál.  Az Azure Site Recovery jelenleg nem támogatja a 2047 GB-nál nagyobb méretű operációsrendszer-lemezeket 1. generációs virtuális gépek esetében.

* A 2. generációs virtuális gép (EFI rendszerindítás-típus) operációsrendszer-lemezének mérete nagyobb 300 GB-nál. Az Azure Site Recovery jelenleg nem támogatja a 300 GB-nál nagyobb méretű operációsrendszer-lemezeket 2. generációs virtuális gépek esetében.

* A virtuális gép neve tartalmazza a következő, nem támogatott karaktereket: “” [] `.  Az eszköz nem tudja lekérni azon virtuális gépek profilkészítési adatait, amelyek tartalmazzák nevükben a fenti karaktereket. 

* A VHD-t kettő vagy több virtuális gép osztja meg. Az Azure nem támogatja a közös VHD-jű virtuális gépeket.

* A Virtuális Fiber Channel vezérlőt használó virtuális gépek nem támogatottak. Az Azure Site Recovery nem támogatja a Virtuális Fiber Channel vezérlőt használó virtuális gépeket.

* A Hyper-V fürt nem tartalmaz replikációs közvetítőt. Az Azure Site Recovery nem támogatja az adott fürthöz beállított replikációs közvetítővel nem rendelkező Hyper-V fürtön lévő virtuális gépeket.

* A virtuális gép nem magas rendelkezésre állású. Az Azure Site Recovery nem támogatja az olyan Hyper-V fürtcsomóponton elhelyezett virtuális gépeket, amelynek VHD-i a helyi lemezen vannak tárolva, nem a fürtlemezen. 

* A virtuális gépek teljes mérete (replikáció + TFO) meghaladja a támogatott prémium tárfiók méretkorlátozását (35 TB). Ez a fajta inkompatibilitás általában akkor fordul elő, ha a virtuális gép egy lemezének egyik teljesítményjellemzője meghaladja a támogatott standard szintű tárolóra vonatkozó Azure- vagy Azure Site Recovery-korlátozásokat. Egy ilyen példány a prémium szintű tárolózónába kényszeríti a virtuális gépet. A prémium szintű tárfiókok maximális támogatott mérete azonban 35 TB, és egyetlen védett virtuális gép nem védhető több tárfiókon keresztül. Ügyeljen arra is, hogy amikor a feladatátvételi tesztet védett virtuális gépen hajtja végre, és ha a felügyelet nélküli lemez be van állítva feladatátviteli tesztre, akkor a teszt ugyanazon a tárfiókon fut, ahol a replikáció folyik. Ebben az esetben további, ugyanakkora mennyiségű tárterületre van szükség, mint a replikáció esetében. Ez biztosítja a replikáció lefolyását, valamint ezzel párhuzamosan a feladatátvételi teszt sikerét. Ha a felügyelt lemez be van állítva feladatátviteli tesztre, nincs szükség további terület kijelölésére a virtuális gép esetében, amelyen a feladatátviteli teszt zajlik.

* A forrás IOPS-érték meghaladja a tároló lemezenkénti 7500-as IOPS-korlátját.

* A forrás IOPS-érték meghaladja a tároló virtuális gépenkénti 80 000-es IOPS-korlátját.

* A forrás virtuális gép átlagos adatváltozása meghaladja az Azure Site Recovery átlagos I/O-mérethez megadott 10 MB/s értékű adatváltozási korlátját.

* A forrás virtuális gép átlagos tényleges írási IOPS-értéke meghaladja az Azure Site Recovery lemezenkénti 840-es támogatott IOPS-korlátját.

* A kiszámított pillanatkép-tároló mérete meghaladja a 10 TB-os támogatott méretet.

**Peak R/W IOPS (with Growth Factor)** (Maximális írási/olvasási IOPS (növekedési tényezővel)): A lemez IOPS-csúcsértéke (alapértelmezés szerint a 95. percentilis), beleértve a későbbi növekedési faktort is (az alapértelmezett érték: 30%). Vegye figyelembe, hogy a virtuális gép teljes írási/olvasási IOPS-értéke nem mindig egyezik meg az egyes virtuálisgép-lemezek írási/olvasási IOPS-értékeinek összegével. Ez azért van, mert a virtuális gép írási/olvasási IOPS-csúcsértéke az egyes lemezeken a profilkészítési időszak alatt percenként mért írási/olvasási IOPS-értékek legmagasabb összege.

**Peak Data Churn in MB/s (with Growth Factor)** (Maximális adatváltozás (MB/s) (növekedési tényezővel)): A lemez adatváltozásának csúcsértéke (alapértelmezés szerint a 95. percentilis), beleértve a későbbi növekedési faktort is (alapértelmezés szerint: 30%). Vegye figyelembe, hogy a virtuális gép teljes adatváltozása nem mindig egyezik meg az egyes lemezek adatváltozásának összegével. Ez azért van, mert a virtuális gép adatváltozásának csúcsértéke az egyes lemezeken a profilkészítési időszak alatt percenként mért adatváltozás-értékek legmagasabb összege.

**Number of Disks** (Lemezek száma): A virtuális gép VHD-inek teljes száma.

**Disk size (GB)** (Lemez mérete (GB)): A virtuális gép összes lemezének teljes beállítási mérete. Az eszköz a virtuális gép egyes lemezeinek méretét is megjeleníti.

**Cores** (Magok): A virtuális gép processzormagjainak száma.

**Memory (MB)** (Memória (MB)): A virtuális gép fizikai memóriájának (RAM) mennyisége.

**NICs** (Hálózati adapterek): A virtuális gép hálózati adaptereinek száma.

**Boot Type** (Rendszerindítás típusa): A virtuális gép rendszerindítási típusa. Ez BIOS vagy EFI lehet.

## <a name="azure-site-recovery-limits"></a>Az Azure Site Recovery korlátai
Az alábbi táblázat az Azure Site Recovery korlátait tartalmazza. Ezek a korlátok a saját tesztjeinken alapulnak, de nem fedhetik le az alkalmazások minden lehetséges I/O-kombinációját. A tényleges eredmények a saját alkalmazásának I/O-műveletei alapján változhatnak. A legjobb eredmények érdekében még az üzembe helyezés megtervezése után is ajánlott az alkalmazás alapos tesztelése feladatátvételi tesztek használatával, így valós képet kaphat az alkalmazás teljesítményéről.
 
**Replikáció tárolási célja** | **Forrás virtuális gép átlagos I/O-mérete** |**Forrás virtuális gép átlagos adatváltozása** | **Forrás virtuális gép teljes napi adatváltozása**
---|---|---|---
Standard szintű Storage | 8 KB | Virtuális gépenként 2 MB/s | Virtuális gépenként 168 GB
Prémium szintű Storage | 8 KB  | Virtuális gépenként 5 MB/s | Virtuális gépenként 421 GB
Prémium szintű Storage | 16 KB vagy több| Virtuális gépenként 10 MB/s | Virtuális gépenként 842 GB

Ezek átlagos határértékek, amelyek 30 százalékos I/O-átfedést feltételeznek. Az Azure Site Recovery képes magasabb átviteli sebesség kezelésére az átfedési arány, a nagyobb írási méretek és a számítási feladatok tényleges I/O-viselkedése alapján. Az előbbi számok egy általános, körülbelül ötperces várólistát feltételeznek. Ez azt jelenti, hogy a feltöltést követő öt percben megtörténik az adat feldolgozása, és létrejön egy helyreállítási pont.

## <a name="on-premises-storage-requirement"></a>A helyszíni tárolásra vonatkozó követelmények

A munkalapon szerepelnek a Hyper-V kiszolgálók (amelyeken a VHD-k megtalálhatók) köteteinek a sikeres kezdeti és változásreplikációhoz szükséges teljes szabad tárterületre vonatkozó követelményei. A replikáció engedélyezése előtt adja hozzá a szükséges tárterületet a kötetekhez, így biztosíthatja, hogy a replikáció során nem következik be nem kívánt leállás a termelési alkalmazásaiban. 

Az Azure Site Recovery Deployment Planner azonosítja az optimális tárterület-követelményeket a VHD-k mérete és a replikációhoz felhasznált hálózati sávszélesség alapján.

![helyszíni tárolásra vonatkozó követelmények](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-requirement-h2a.png)

### <a name="why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication"></a>Miért van szükség szabad területre a Hyper-V kiszolgálón a replikációhoz?
* Amikor engedélyezi egy virtuális gép replikációját, az Azure Site Recovery pillanatképet készít a virtuális gép egyes VHD-iről a kezdeti replikációhoz (IR). A kezdeti replikáció során az alkalmazás új módosításokat ír a lemezekre. Az Azure Site Recovery a naplófájlokban követi nyomon ezeket a változásokat, ehhez pedig további tárterületre van szükség.  A kezdeti replikáció befejezéséig a naplófájlokat a rendszer helyben tárolja. Ha nincs elegendő hely a naplófájlok és a pillanatkép (AVHDX) számára, a replikáció átlép újraszinkronizálási üzemmódba, és soha nem fejeződik be. A legrosszabb esetben a VHD-méret 100%-ának megfelelő szabad tárterületre van szükség a kezdeti replikációhoz.
* Ha a kezdeti replikáció befejeződik, elkezdődik a változásreplikáció. Az Azure Site Recovery a naplófájlokban követi nyomon ezeket a változásokat. Ezek ugyanazon a köteten találhatók, amelyen a virtuális gép VHD-i vannak. A rendszer replikálja ezeket a naplófájlokat az Azure-ba a konfigurált másolási gyakoriságnak megfelelő időközönként. Az elérhető hálózati sávszélességtől függően a naplófájlok Azure-ba történő replikációja némi időt vesz igénybe. Ha nincs elegendő szabad terület a naplófájlok tárolásához, a replikáció szünetel, a virtuális gép replikációja pedig „újraszinkronizálás szükséges” állapotúra vált.
* Ha a hálózati sávszélesség nem elég a naplófájlok Azure-ba történő áthelyezéséhez, akkor azok a köteten halmozódnak fel. A legrosszabb esetben – amikor a naplófájlok mérete eléri a VHD méretének 50%-át – a virtuális gép replikációja „újraszinkronizálás szükséges” állapotúra módosul. A legrosszabb esetben a VHD mérete 50%-ának megfelelő szabad tárterületre van szükség a változásreplikációhoz.

**Hyper-V host** (Hyper-V gazdagép): A vizsgált Hyper-V kiszolgálók listája. Ha egy kiszolgáló egy Hyper-V fürt részét képezi, az összes fürtcsomópont csoportosítva lesz.

**Volume (VHD path)** (Kötet (VHD-útvonal)): A Hyper-V gazdagép minden olyan kötete, ahol VHD-k vagy VHDX-ek találhatók. 

**Free space available (GB)** (Elérhető szabad tárterület (GB)): A köteten lévő szabad tárterület.

**Total storage space required on the volume (GB)** (Teljes szükséges tárterület a köteten (GB)): A sikeres kezdeti és változásreplikációhoz szükséges, teljes szabad tárterület a köteten. 

**Total additional storage to be provisioned on the volume for successful replication** (A sikeres replikációhoz szükséges teljes kiépítendő tárterület a köteten): Javaslatot tesz a teljes további tárterületre, amelyet ki kell építeni a köteten a sikeres kezdeti és változásreplikációhoz.

## <a name="initial-replication-batching"></a>Kezdeti replikáció kötegelése 

### <a name="why-do-i-need-initial-replication-ir-batching"></a>Miért van szükség a kezdeti replikáció (IR) kötegelésére?
Ha minden virtuális gép védve van ugyanabban az időben, a szabad tárterületre vonatkozó követelmény sokkal magasabb. Ha nem áll rendelkezésre elég tárhely, a virtuális gépek replikációja újraszinkronizálási üzemmódra vált. Emellett az összes virtuális gép együttes kezdeti replikációjának befejezéséhez szükséges hálózati sávszélesség is sokkal magasabb. 

### <a name="initial-replication-batching-for-a-selected-rpo"></a>Kezdeti replikáció kötegelése a kiválasztott RPO esetében
Ezen a munkalapon az egyes kötegek részletes nézete található a kezdeti replikációhoz (IR). A rendszer minden egyes RPO esetében külön IR-kötegelési táblázatot hoz létre. 

Ha minden kötet esetében megfelelt a javasolt helyszíni tárterület-követelményeknek, a replikálandó fő információ a párhuzamosan védhető virtuális gépek listája. Ezek a virtuális gépek egy kötegbe vannak csoportosítva. Több köteg is lehet. A virtuális gépeket a megadott kötegelési sorrendben kell levédenie. Először a 1. kötegbe tartozó virtuális gépeket védje le, majd ha befejeződött a kezdeti replikáció, védje le a 2. kötegbe tartozókat, és így tovább. A kötegek és a hozzájuk tartozó virtuális gépek listáját ebben a táblázatban találja. 

![IR-kötegelés részletei1](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo-h2a.png)
![IR-kötegelés részletei2](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo2-h2a.png)

### <a name="each-batch-provides-the-following-information"></a>Minden kötegben az alábbi információk találhatók meg 
**Hyper-V host** (Hyper-V gazdagép): A levédendő virtuális gép Hyper-V gazdagépe.
Virtual machine (Virtuális gép): A levédendő virtuális gép. 

**Comments** (Megjegyzések): Ha egy virtuális gép egy meghatározott kötete esetében bármilyen műveletre van szükség, itt látható a vonatkozó megjegyzés.  Ha például nem érhető el elegendő szabad terület egy köteten, akkor megjelenik az „Add additional storage to protect this VM” (Adjon hozzá további tárterületet a virtuális gép levédéséhez) megjegyzés.

**Volume (VHD path)** (Kötet (VHD útvonala)): A kötet neve, amelyen a virtuális gép VHD-i megtalálhatók. Free space available on the volume (GB) (Elérhető szabad tárterület a köteten (GB)): A virtuális géphez tartozó köteten lévő szabad lemezterület. A köteteken lévő elérhető szabad terület kiszámításakor a rendszer figyelembe veszi az előző kötegek virtuális gépei – amelyeknek a VHD-i egyazon köteten találhatók – változásreplikációja által felhasznált lemezterületet is.  

Például tegyük fel, hogy a VM1, VM2 és VM3 virtuális gép egy köteten található (E:\VHDpath). A replikáció előtt a köteten 500 GB szabad terület van. A VM1 gép az 1. köteg része, a VM2 a 2. kötegé, a VM3 pedig a 3. kötegé.  A VM1 által 500 GB szabad terület érhető el. A VM2 esetében a szabad terület annyival kevesebb 500 GB-nál, amennyire a VM1 változásreplikációjához van szükség.  Tehát ha a VM1-nek például 300 GB területre van szüksége a változásreplikációhoz, akkor a VM2 számára 500 GB - 300 GB = 200 GB szabad terület lesz elérhető.  A VM2-nek hasonló módon 300 GB területre van szüksége a változásreplikációhoz. A VM3 számára 200 GB - 300 GB = -100 GB szabad terület lesz elérhető.

**Storage required on the volume for initial replication (GB)** (A kezdeti replikációhoz szükséges tárterület a köteten (GB)): A virtuális gép kezdeti replikációjához szükséges szabad tárterület a köteten.

**Storage required on the volume for delta replication (GB)** (A változásreplikációhoz szükséges tárterület a köteten (GB)): A virtuális gép változásreplikációjához szükséges szabad tárterület a köteten.

**Additional storage required based on deficit to avoid replication failure (GB)** A deficiten alapuló, további szükséges tárterület a köteten, amellyel elkerülhető a replikáció meghiúsulása (GB): A kötetre vonatkozó, a virtuális gép számára szükséges további tárterület-követelmény.  Ez a maximális kezdeti replikációra és változásreplikációra vonatkozó tárterületi követelmények maximuma – a köteten elérhető szabad terület.

**Minimum bandwidth required for initial replication (Mbps)** (A kezdeti replikációhoz szükséges minimális sávszélesség (Mbps)): A virtuális gép kezdeti replikációjához szükséges minimális sávszélesség.

**Minimum bandwidth required for delta replication (Mbps)** (A változásreplikációhoz szükséges minimális sávszélesség (Mbps)): A virtuális gép változásreplikációjához szükséges minimális sávszélesség

### <a name="network-utilization-details-for-each-batch"></a>Az egyes kötegek hálózatfelhasználásának részletei 
Minden kötegtáblázathoz tartozik egy összefoglalás az adott köteg hálózatfelhasználásáról.

**Bandwidth available for Batch** (A köteg számára elérhető sávszélesség): A köteg számára elérhető sávszélesség az előző köteg változásreplikációjához szükséges sávszélesség figyelembe vételével.

**Approximate bandwidth available for initial replication of batch** (Hozzávetőleges elérhető sávszélesség a köteg kezdeti replikációjához): A köteg virtuális gépeinek kezdeti replikációja számára elérhető sávszélesség. 

**Approximate bandwidth consumed for delta replication of batch** (A köteg változásreplikációja által felhasznált hozzávetőleges sávszélesség): A köteg virtuális gépeinek változásreplikációjához szükséges sávszélesség. 

**Estimated Initial Replication time for Batch (HH:MM)** (A köteg kezdeti replikációjának becsült ideje (ÓÓ:PP)): A kezdeti replikáció becsült ideje óra:perc formátumban.

## <a name="cost-estimation"></a>Költségbecslés
További információk a [költségbecslésről](site-recovery-hyper-v-deployment-planner-cost-estimation.md). 

## <a name="next-steps"></a>Következő lépések
További információk a [költségbecslésről](site-recovery-hyper-v-deployment-planner-cost-estimation.md).