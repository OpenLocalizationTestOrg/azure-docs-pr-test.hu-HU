---
title: "aaaBatch és HPC megoldások felhőben hello - Azure |} Microsoft Docs"
description: "Megismerheti a Batch- és nagy teljesítményű feldolgozási (HPC- és Big Compute-) forgatókönyveket és megoldási lehetőségeket az Azure rendszerben"
services: batch, virtual-machines, cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: aab5401d-2baf-4cf2-bf20-ad224de33888
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c5a3c8859d1f95040bcdad15942a815d71eb4486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="batch-and-hpc-solutions-for-large-scale-computing-workloads"></a>Batch- és HPC-megoldások nagyméretű számítási feladatokhoz

Az Azure hatékony, méretezhető felhőbeli megoldásokat nyújt a Batch- és nagy teljesítményű feldolgozási (HPC), más néven *Big Compute-forgatókönyvekhez*. Tudnivalók az itt nagy számítási feladatok és Azure-szolgáltatások toosupport őket, vagy helyettesítő közvetlenül túl[megoldási forgatókönyvek](#scenarios) című cikkben. Ez a cikk műszaki döntéshozók, az informatikai vezetők és a független szoftvergyártók, de más informatikai szakemberek és fejlesztők használható informatikai toofamiliarize maguk ezek a megoldások.

A szervezeteknek nagy méretű számítási feladatokat kell megoldaniuk: műszaki tervezés és elemzés, képrenderelés, összetett modellezés, Monte Carlo-szimulációk, pénzügyi kockázatszámítások és egyebek. Azure segítségével a szervezetek hello erőforrások, a méretezési és a szükséges ütemezés problémák megoldásához. Az Azure-ral a szervezetek a következőket tehetik:

* Hibrid megoldások, egy helyszíni HPC fürt toooffload csúcs munkaterhelések toohello felhőben kiterjesztése létrehozása
* HPC-fürteszközök és számítási feladatok futtatása kizárólag az Azure-ban
* Méretezhető és felügyelt Azure-szolgáltatásokat használja, mint [kötegelt](https://azure.microsoft.com/documentation/services/batch/) toorun számítási-igényes munkaterhelések anélkül, hogy toodeploy és számítási infrastruktúrájának kezelése.

Bár terjed hello ebben a cikkben, Azure is biztosít a fejlesztők és a partnerek képességek, architektúra lehetőségeket, és a fejlesztői eszközök toobuild nagyméretű, egyéni nagy számítási munkafolyamatok teljes készletét. És egy egyre bővülő partner ökoszisztéma készen toohelp hatékonyan dolgozhatnak a hello Azure felhőben ellenőrizze a nagy számítási munkaterhelések.

## <a name="batch-and-hpc-applications"></a>Batch- és HPC-alkalmazások
A webalkalmazásokkal és számos üzletági alkalmazással ellentétben a Batch- és HPC-alkalmazásoknak meghatározott elejük és végük van, és ütemezve vagy igény szerint is futtathatók, akár órákig vagy még tovább. A legtöbb két fő kategóriába sorolhatók: *belsőleg párhuzamos* (más néven "embarrassingly párhuzamos", mert azok megoldására hello problémák alkalmasak toorunning párhuzamosan több számítógépen vagy processzorok) és *szorosan összekapcsolt*. Tekintse meg a következő táblázat további információk az alábbi alkalmazástípusokat hello. Bizonyos Azure megoldás megközelíti egy típus vagy más hello jobb munkát.

> [!NOTE]
> A Batch- és HPC-megoldásokban az alkalmazások futó példányát általában *feladatnak* hívják, és minden feladat *tevékenységekre* osztható. Hello fürtözött számítási erőforrásokat hello alkalmazás gyakran nevezik, és *számítási csomópontok*.
> 
> 

| Típus | Jellemzők | Példák |
| --- | --- | --- |
| **Belsőleg párhuzamos**<br/><br/>![Intrinsically parallel][parallel] |• Az egyes számítógépek egymástól függetlenül futtatják az alkalmazáslogikát<br/><br/> • Hozzáadását számítógépek lehetővé teszi, hogy a hello alkalmazás tooscale és csökkentse számítási idő<br/><br/>• Az alkalmazás különálló végrehajtható fájlokból áll, vagy ügyfél által meghívott szolgáltatásokra oszlik (szolgáltatásorientált architektúrájú vagy SOA-alkalmazás) |• Pénzügyi kockázat modellezése<br/><br/>• Képrenderelés és -feldolgozás<br/><br/>• Médiatartalmak kódolása és átkódolása<br/><br/>• Monte Carlo-szimulációk<br/><br/>• Szoftvertesztelés |
| **Szorosan összekapcsolt**<br/><br/>![Tightly coupled][coupled] |• Alkalmazás megköveteli a számítási csomópontok toointeract vagy exchange köztes eredmények<br/><br/>• A számítási csomópontok használatával lehet kommunikálni a Message Passing Interface (MPI), egy általános kommunikációs protokollt párhuzamos számítástechnikai hello<br/><br/>• a hello alkalmazása a bizalmas toonetwork késleltetés és a sávszélesség<br/><br/>• Az alkalmazás teljesítménye nagy sebességű hálózatkezelési technológiákkal, például az InfiniBand technológiával és távoli közvetlen memória-hozzáféréssel (RDMA) javítható |• Olaj- és gáztározók modellezése<br/><br/>• Műszaki tervezés és elemzés, például folyadékdinamikai számítások<br/><br/>• Fizikai szimulációk, például autóbalesetek és nukleáris reakciók<br/><br/>• Időjárás-előrejelzés |

### <a name="considerations-for-running-batch-and-hpc-applications-in-hello-cloud"></a>Kötegelt és a HPC-alkalmazásokhoz hello felhőben futó szempontjai
Sok alkalmazásokat, amelyek a helyszíni HPC-fürt tooAzure vagy tooa (létesítmények közötti) hibrid környezetben tervezett toorun könnyen telepíthet át. Lehetséges azonban, hogy figyelembe kell vennie néhány korlátozást vagy szempontot, például a következőket:

* **Felhőalapú erőforrások rendelkezésre állási** -felhő számítási erőforrásokat használ hello típusától függően nem feltétlenül tudja toorely a folyamatos gép rendelkezésre állására egy feladat futtatása közben. Állapot kezelése és a ellenőrizze mutató közös technikák toohandle lehetséges átmeneti hibák, és a további szükséges felhőalapú erőforrások használata esetén.
* **Adat-hozzáférési** -adatok hozzáférési technikák általánosan elérhető vállalati fürtökben, például az NFS-SZEL szükség lehet a hello felhőben speciális beállítást. Vagy, meg kell tooadopt különböző data access eljárásairól és mintáiról hello felhő.
* **Adatátvitel** - alkalmazásokhoz, hogy a folyamat nagy mennyiségű adat, stratégiák szükséges toomove hello adatok felhőalapú tárolása és toocompute erőforrások. Ehhez nagy sebességű, létesítmények közötti hálózatkezelés lehet szükséges, amely például az [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)-tal biztosítható. Az adatok tárolásával vagy elérésével kapcsolatos jogi, szabályozási és házirendi korlátozásokat is vegye figyelembe.
* **Licencelési** – ellenőrizze a kereskedelmi alkalmazás licencelési hello gyártójánál, vagy egyéb korlátozások futó hello felhő. Nem minden szállító kínál használatalapú licencet. Lehet, hogy a tooplan kell hello felhőben licencelési kiszolgáló megoldást, vagy csatlakozzon tooan helyszíni licenckiszolgáló.

### <a name="big-compute-or-big-data"></a>Big Compute vagy Big Data?
elválasztó vonal nagy számítási és a Big Data típusú adatok alkalmazások közötti hello nem minden esetben törölje, és előfordulhat, hogy egyes alkalmazások jellemzőit is. Mindkettőben nagy méretű számítások futnak, általában számítógépfürtökön. De hello megoldás szempontok és a támogató eszközök eltérőek lehetnek.

• **Nagy számítási** tooinvolve alkalmazások processzorteljesítményre és a memória, például a mérnöki szimulációja modellezési, pénzügyi kockázat és digitális megjelenítési általában. hello infrastruktúra egy nagy számítási megoldás tooperform nyers számítási speciális multicore processzorokkal rendelkező számítógépek és a speciális, nagy sebességű hálózati hardver tooconnect hello számítógépek vehetők igénybe.

• A **Big Data** olyan adatelemzési problémákat old meg, amelyek adatmennyisége nem kezelhető egyetlen számítógéppel vagy adatbázis-kezelő rendszerrel. Ilyenek például a nagy mennyiségű webnapló- vagy egyéb üzletiintelligencia-adatok. Big Data típusú adatok általában toorely további lemezkapacitást és processzorteljesítményre a mint i/o-teljesítmény. Speciális Big Data típusú adatok eszközök, például az Apache Hadoop toomanage hello fürt és a partíció hello adatok is vannak. (Az Azure HDInsighttal és más Azure Hadoop-megoldásokkal kapcsolatban további információért lásd: [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## <a name="compute-management-and-job-scheduling"></a>Számításkezelés és feladatütemezés
Kötegelt és a HPC-alkalmazások gyakran futtatása magában foglalja a *kezelő* és egy *Feladatütemező* toohelp fürtözött számítási erőforrások kezelése, és hello feladatok futtatása toohello alkalmazások rendelnie azokat. Ezek a funkciók különálló eszközökkel, illetve integrált eszközökkel vagy szolgáltatásokkal is elvégezhetők.

* **Fürtkezelő** – Számítási erőforrások (vagy számítási csomópontok) kiépítésére, kibocsátására és felügyeletére szolgál. Egy kezelő előfordulhat, hogy automatizálják a telepítést az operációsrendszer-lemezképek, és a számítási csomópontok alkalmazások válik a számítási erőforrások toodemands szerint, és hello csomópontok hello teljesítményének figyeléséhez.
* **Feladatütemező** -hello erőforrások határozza meg (például processzor és memória) egy alkalmazást kell, és hello feltételek futtatásakor. A Feladatütemező feladatok várólista kezeli, és erőforrások toothem egy hozzárendelt prioritás vagy más jellemzők alapján foglal le.

Fürtszolgáltatás és a feladatütemezés a Windows- és Linux-alapú fürtök eszközök jól tooAzure telepíthet át. A [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), a Microsoft windowsos és linuxos HPC számítási feladatokhoz használható ingyenes számítási fürtkezelő megoldása például több lehetőséget nyújt az Azure-on való futtatáshoz. Linux-fürtök toorun nyílt forráskódú eszközök például nyomaték és SLURM is készíthetnek. Is helyezheti kereskedelmi rács megoldások tooAzure, például a [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/), [IBM pontszámot Symphony és Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/), és [Univa rács motor](http://www.univa.com/products/grid-engine).

Ahogy az a következő szakaszok hello, is kihasználhatja az Azure-szolgáltatások toomanage számítási erőforrásokat és ütemezett feladatok nélkül (vagy kívül) hagyományos fürt felügyeleti eszközei.

## <a name="scenarios"></a>Forgatókönyvek
Az alábbiakban három gyakori helyzetek toorun nagy számítási munkaterhelések az Azure-ban meglévő HPC-fürt megoldások, Azure-szolgáltatások vagy hello kettő kombinációja. Az egyes forgatókönyvek kiválasztásának fő szempontjait is láthatja, de ezek nem tekinthetők teljesnek. Több hello elérhető az Azure szolgáltatások használhatja a megoldás későbbre esik, hello cikkben.

| Forgatókönyv | Kiválasztás szempontjai |
| --- | --- | --- |
| **-Kapacitásnövelés a HPC-fürt tooAzure**<br/><br/>[![Cluster burst][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> További információ:<br/>• [Kapacitásnövelés a HPC Pack tooAzure worker-példányok](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Set up a hybrid compute cluster with HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) (Hibrid számítási fürt beállítása HPC Packkel)<br/><br/>• [Kapacitásnövelés a HPC Pack kötegelt tooAzure](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/> |• A [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) vagy más helyszíni fürtök további Azure-erőforrásokkal kombinálhatók egy hibrid megoldásban.<br/><br/>• A nagy számítási munkaterhelések toorun Platform (PaaS) szolgáltatás virtuálisgép-példányok (jelenleg csak Windows Server), terjed ki.<br/><br/>• Helyszíni licenckiszolgáló vagy adattár elérése választható Azure virtuális hálózattal |
| **HPC-fürt létrehozása kizárólag az Azure-ban**<br/><br/>[![Cluster in IaaS][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>További információ:<br/>• [HPC-fürtmegoldások az Azure-ban](big-compute-resources.md)<br/><br/> |• Gyorsan és egységesen telepítheti az alkalmazásait és fürteszközeit szolgáltatott infrastruktúrára (IaaS) épülő, szabványos vagy egyéni Windows- és Linux- alapú virtuális gépekre.<br/><br/>• Különböző nagy számítási alkalmazásokat és szolgáltatásokat futtathatnak hello a feladatütemezés az Ön által választott megoldás segítségével.<br/><br/>• További Azure szolgáltatásokat, például a hálózati és tárolási toocreate teljes felhőalapú megoldásokat használja. |
| **Egy párhuzamos kérelem tooAzure kiterjesztése**<br/><br/>[![Azure Batch][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>További információ:<br/>• [Az Azure Batch alapjai](batch-technical-overview.md)<br/><br/>• [Hello Azure Batch könyvtár az első lépései a .NET-hez](batch-dotnet-get-started.md) |• Fejlesztést [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) tooscale különböző nagy számítási munkaterhelések toorun a Windows vagy Linux rendszerű virtuális gépek készleteinek ki.<br/><br/>• Azure platform szolgáltatás toomanage központi telepítése és virtuális gépek, feladatütemezés, vész-helyreállítási, adatmozgás, függőségi felügyeleti és alkalmazás központi telepítése automatikus skálázás használjon. |

## <a name="azure-services-for-big-compute"></a>Azure-szolgáltatások a Big Compute-feladatokhoz
Itt található további információk a hello számítási, az adatokat, a hálózat és a kapcsolódó szolgáltatások kombinálhatja a megoldások nagy számítási és munkafolyamatok. További Azure-szolgáltatások, a részletes útmutató: Azure-szolgáltatások hello [dokumentáció](https://azure.microsoft.com/documentation/). Hello [forgatókönyvek](#scenarios) cikkben korábban megjelenítése néhány módokat használja ezeket a szolgáltatásokat.

> [!NOTE]
> Az Azure rendszeresen vezet be új szolgáltatásokat, amelyek hasznosak lehetnek az Ön forgatókönyvéhez. Ha kérdése van, lépjen kapcsolatba egy [Azure-partnerrel](https://pinpoint.microsoft.com/en-US/search?keyword=azure), vagy küldjön e-mailt *bigcompute@microsoft.com* címre.
> 
> 

### <a name="compute-services"></a>Számítási szolgáltatások
Az Azure compute szolgáltatásainak használata hello core nagy számítási megoldást, és hello különböző számítási szolgáltatások ajánlat előnyeit különböző helyzetek kezelésére. Alapszintű szinten ezeket a szolgáltatásokat kínál a virtuálisgép-alapú számítási példányokért Azure által biztosított Windows Server Hyper-V technológiát használó alkalmazások toorun mód választható. Ezek a példányok szabványos és egyéni Linux és Windows operációs rendszereket és eszközöket futtathatnak. Az Azure különböző [példányméreteket](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tesz elérhetővé a processzormagok, memória, lemezkapacitás és más jellemzők különböző konfigurációival. Igényeitől függően hello példányok toothousands magszámra vonatkozó követelmény méretezhető, vagy ha kevesebb erőforrást kell majd csökkentheti.

> [!NOTE]
> Hello Azure előnyeit [számítási igényű példányok például H-sorozat hello](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooimprove hello teljesítményét és méretezhetőséget biztosít a HPC-munkaterhelések. Ezek a példányok támogatják a párhuzamos MPI-alkalmazásokat is, amelyekhez alacsony késésű és nagy átviteli kapacitású alkalmazáshálózat szükséges. Is elérhető biztosan [N-sorozat](../virtual-machines/windows/sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) NVIDIA Feldolgozóegységekkel tooexpand hello tartománnyal számítási és a képi megjelenítés forgatókönyvek az Azure virtuális gépeken.  
> 
> 

| Szolgáltatás | Leírás |
| --- | --- |
| **[Virtual machines (Virtuális gépek)](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Szolgáltatott infrastruktúra (IaaS) biztosítása Microsoft Hyper-V technológiával<br/><br/>• Lehetővé teszik a tooflexibly kiépítése és állandó felhő számítógépek kezelése a szabványos Windows Server vagy Linux lemezképeinek a hello [Azure piactér](https://azure.microsoft.com/marketplace/), vagy képeket és adatlemezek megadnia<br/><br/>• Telepítése és kezelése [Virtuálisgép-méretezési készlet](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) nagyméretű toobuild szolgáltatásokhoz azonos virtuális gépek automatikus skálázás tooincrease vagy csökken kapacitással rendelkező automatikusan<br/><br/>• A helyi számítási fürt eszközök és alkalmazások teljesen hello felhőben<br/><br/> |
| **[Felhőszolgáltatások](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• A Big Compute-alkalmazásokat feldolgozói szerepkörpéldányokban futtathatja, amelyek a Windows Server valamelyik verzióját futtató, teljes mértékben az Azure által kezelt virtuális gépek<br/><br/>• Méretezhető, megbízható alkalmazásokat engedélyezhet alacsony adminisztrációs terheléssel, szolgáltatott platform (PaaS) modellben<br/><br/>• További eszközök vagy a helyszíni HPC-fürt megoldás fejlesztési toointegrate lehet szükség. |
| **[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Nagy méretű párhuzamos és kötegelt számítási feladatokat futtat egy teljes mértékben felügyelt szolgáltatásban<br/><br/>• Biztosítja a virtuális gépek felügyelt készletének feladatütemezését és automatikus méretezését<br/><br/>• Lehetővé teszi a fejlesztők toobuild és futtatható alkalmazások szolgáltatásként vagy a meglévő alkalmazások felhő engedélyezése<br/> |

### <a name="storage-services"></a>Tárolási szolgáltatások
A Big Compute-megoldások általában bemeneti adatok készleteit dolgozzák fel, adatokat hoznak létre az eredményként. A megoldások nagy számítási használt hello Azure storage szolgáltatások többek között:

* [Blob, Table és Queue Storage](https://azure.microsoft.com/documentation/services/storage/) – Nagy mennyiségű strukturálatlan adat, NoSQL-adat, valamint munkafolyamat- és kommunikációs üzenetek kezelésére szolgál. Például használhatja a blob-tároló műszaki nagy adatkészletek vagy hello bemeneti képek vagy médiafájlok az alkalmazás folyamatokat. A megoldásokban aszinkron kommunikációt is biztosíthat üzenetsorok használatával. Lásd: [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md).
* [Az Azure File storage](https://azure.microsoft.com/services/storage/files/) -megosztások közös fájlokat és adatokat az Azure használatával hello szabványos SMB-protokollt, amelyre szükség van néhány HPC-fürt megoldás.
* [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) -hello felhő hasznosak lehetnek a kötegelt, valós idejű, és az interaktív elemzések egy hyperscale Apache Hadoop elosztott fájlrendszer biztosít.

### <a name="data-and-analysis-services"></a>Adat- és elemzési szolgáltatások
Egyes Big Compute-forgatókönyvekben nagy méretű adatfolyamok szerepelnek, vagy olyan adatokat hoznak létre, amelyek további feldolgozást vagy elemzést igényelnek. Az Azure több adat- és elemzési szolgáltatást nyújt, például a következőket:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) – Adatvezérelt munkafolyamatokat (folyamatokat) hoz létre, amelyek adatokat egyesítenek, összesítenek és alakítanak át helyszíni, felhőalapú és internetes adattárakból.
* [SQL-adatbázis](https://azure.microsoft.com/documentation/services/sql-database/) -hello egy Microsoft SQL Server relációs adatbázis-kezelő rendszer egy felügyelt szolgáltatás a kulcsfontosságú szolgáltatásokat biztosít.
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) – üzembe és tesz elérhetővé a Windows Server vagy Linux-alapú Apache Hadoop-fürtöket helyez hello toomanage felhőalapú, elemzése és a big data jelentést.
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) – A segítségével teljes mértékben felügyelt szolgáltatásban hozhatja létre, tesztelheti, működtetheti és felügyelheti a prediktív elemzési megoldásokat.

### <a name="additional-services"></a>További szolgáltatások
A nagy számítási megoldás módosítania kell a más Azure-szolgáltatások tooconnect tooresources helyszíni vagy más környezetekben. Példák erre vonatkozóan:

* [Virtuális hálózati](https://azure.microsoft.com/documentation/services/virtual-network/) -logikailag elkülönített szakasz Azure tooconnect Azure-erőforrások tooeach más vagy tooyour a helyszíni adatközpont hoz létre. A létesítmények közötti virtuális hálózattal a Big Compute-alkalmazások elérhetik a helyszíni adatokat, az Active Directory-szolgáltatásokat és a licenckiszolgálókat.
* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) – Magánkapcsolatot hoz létre a Microsoft-adatközpontok és olyan infrastruktúrák között, amelyek helyszíniek vagy közös környezetben találhatók. ExpressRoute magasabb biztonsági szint, további megbízhatóságot biztosít, gyorsabb sebességet, és kisebb késések fordulnak elő, mint a szokásos kapcsolatok hello interneten keresztül.
* [A Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) – az alkalmazások több mechanizmus toocommunicate vagy exchange adatokat biztosít, hogy másik felhőalapú platform, vagy egy adott adatközpont Azure-ban találhatók.

## <a name="next-steps"></a>Következő lépések
* Lásd: [kötegelt és HPC technikai erőforrások](big-compute-resources.md) toofind műszaki útmutatót toobuild a megoldás.
* Beszélje meg az Azure-lehetőségeket az olyan partnerekkel is, mint a Cycle Computing, a Rescale és az UberCloud.
* Olvasson a [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/) és [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088) által készített Azure Big Compute-megoldásokról.
* Hello legújabb közlemények, lásd: hello [Microsoft HPC és kötegelt csapat blogja](http://blogs.technet.com/b/windowshpc/) és hello [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
