---
title: "Felügyeleti csomag (OMS) – áttekintés aaaOperations |} Microsoft Docs"
description: "A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében.  Ez a cikk ismerteti az OMS hello értékének, hello különböző szolgáltatásokat és az OMS szereplő ajánlatok azonosítja, valamint hivatkozásokat tootheir részletes tartalmat."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: bwren
ms.openlocfilehash: ec3fe6d82aec46d1f715a4338f126e79e04a9147
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Mi az az Operations Management Suite (OMS)?
A cikkben egy bevezető tooOperations felügyeleti csomag (OMS) többek között a hello üzleti értéket biztosít, hello szolgáltatások és felügyeleti megoldásokat tartalmazza és a csomagot együtt a különböző szolgáltatások hello ajánlatok rövid áttekintést és megoldások.  Hivatkozások szerepelnek toohello részletes telepítésére és használatára, minden egyes szolgáltatást és a megoldás vonatkozó dokumentációt.

## <a name="from-on-premises-toohello-cloud"></a>A helyszíni toohello felhőből
A Microsoft már régóta kínál termékeket a vállalati környezetek felügyeletéhez.  Több termék is hello a System Center termékcsomagot felügyeleti összevonni 2007.  Ez tartalmazza, amely olyan szolgáltatások, mint a szoftverterjesztési és a készlet, amely biztosítja a proaktív figyeléshez rendszerek és alkalmazások, az Orchestrator runbookok tooautomate manuálisan végrehajtott folyamatokat tartalmazó Operations Manager biztosít a Configuration Manager , és a Data Protection Manager a biztonsági mentésről és fontos adatok helyreállítását.

Több számítási erőforrással toohello felhő áthelyezését a System Center termékek szerzett további felhőalapú szolgáltatások, például Operations Manager és az Orchestrator, az Azure-erőforrások kezelése.  Ezek azonban még mindig alapvetően helyszíni megoldásoknak készültek, és így jelentős befektetéseket igényel a helyszíni felügyeleti környezet üzembe helyezése és karbantartása.  toocompletely hello felhő használja, és támogatja a jövőbeni alkalmazásokat, egy új módszer toomanagement volt szükség.

## <a name="introducing-operations-management-suite"></a>Bemutatkozik az Operations Management Suite
Az Operations Management Suite (OMS) hello felhőben hello indítás tervezett szolgáltatások gyűjteménye.  Helyszíni erőforrások üzembe helyezése és kezelése helyett az OMS-összetevők teljes mértékben az Azure-ban futnak.  Minimális konfigurációt igényelnek, és akár percek alatt használatba vehetők.  

- **Minimális költségű és összetettségű üzembe helyezés.**  Mivel minden hello összetevők és az OMS-adatokat az Azure-ban, be lehet és rövid időn belül hello összetettségét, és a beruházás nélkül fut a helyszíni összetevők.
- **Toocloud szintjeinek.**  Nem rendelkezik a számítási erőforrásokat, amelyek nem kell fizető kapcsolatos tooworry, illetve kapcsolatos kevés a tárolási hely hello óta felhő lehetővé teszi toopay csak a mi ténylegesen használ, és könnyen méretezni tooany terhelés van szüksége.  Első lépésként lépések néhány erőforrások tooget kezelése és a tooyour teljes környezetet majd méret.
- **Hello legújabb funkciók előnyeit.**  Az OMS-szolgáltatások funkciói folyamatosan bővülnek és frissülnek.  Folyamatosan rendelkezik hozzáférési toohello legújabb funkciókat nélkül követelmény toodeploy frissítéseket.
- **Integrált szolgáltatások.**  Hello OMS szolgáltatások jelentős értékének megadása a saját, amíg azok hogyan tudnak együttműködni toosolve összetett felügyeleti forgatókönyveket.  Például egy Azure Automation forgatókönyv előfordulhat, hogy a feladatátvételi folyamat az Azure Site Recovery meghajtó, és jelentkezzen információk tooLog Analytics toogenerate riasztást.
- **Globális ismeretek.**  Felügyeleti megoldás az OMS folyamatosan rendelkezik toohello legújabb adatok eléréséhez.  hello biztonsági, naplózási megoldást például végezheti a fenyegetés elemzésre érzékelt körül hello world hello legújabb fenyegetésekről használatával.
- **Hozzáférés bárhonnan.**  A felügyeleti környezetet bárhonnan elérheti egy böngészővel.  Alkalmazástelepítési hello OMS okostelefonos hozzáférhet tooyour figyelési adatok.

### <a name="is-it-just-for-hello-cloud"></a>A folyamat azt csak hello felhő?
Mivel a OMS-szolgáltatások futnak hello felhő nem jelenti azt, hogy azokat ténylegesen nem tudja kezelni a helyszíni környezetben.  Ügynök helyezhető el a Windows vagy Linux rendszerű számítógép az adatközpont, és elküld elemzés, ahol elemzése egyéb adatokkal együtt a felhőbeli vagy helyszíni szolgáltatások során gyűjtött adatok tooLog.  Azure biztonsági mentési és az Azure Site Recovery tooleverage hello felhő használja a biztonsági mentési és magas rendelkezésre állás érdekében a helyszíni erőforrások.  
Runbookokat hello felhőben általában nem tud hozzáférni a helyszíni erőforrásokat, de egy vagy több számítógépet az ügynök telepíthető, amely túl runbookokat az Adatközpont fogja futtatni.  Amikor elindít egy runbookot, akkor egyszerűen adja meg, hogy azt toorun hello felhő, vagy a helyi munkavégző.

## <a name="hybrid-management-with-system-center"></a>Hibrid felügyelet a System Centerrel
Ha a System Center meglévő példányát, ezek az összetevők integrálása OMS szolgáltatások tooprovide hibrid megoldás mind a helyszíni, és a felhőalapú környezetben, ami relatív specialties hello az egyes termékek.  Kapcsolódás a meglévő Operations Manager felügyeleti csoport tooLog Analytics felügyelt tooanalyze ügynökök hello felhőben.  Használja a meglévő biztonsági mentési folyamat Data Protection Manager toobackup az adatok toohello felhő.  


## <a name="oms-services"></a>OMS-szolgáltatások
az Azure-ban futó szolgáltatások OMS hello alapvető funkcióit biztosítja.  Minden szolgáltatás egy adott felügyeleti funkciót biztosít, és kombinálhatja services tooachieve különböző felügyeleti lehetőségeket.

|| Szolgáltatás | Leírás |
|:--|:--|:--|
| ![Log Analytics](media/operations-management-suite-overview/icon-log-analytics.png) | Log Analytics | Figyelheti, és elemezheti a hello rendelkezésre állását és más erőforrások, például a fizikai és virtuális gépek teljesítményét. |
| ![Azure Automation](media/operations-management-suite-overview/icon-automation.png) | Automatizálás | Automatizálja a manuális folyamatokat, és érvényesíti a fizikai és virtuális gépekre vonatkozóan megadott konfigurációkat. |
| ![Azure Backup](media/operations-management-suite-overview/icon-backup.png) | Biztonsági mentés | A kritikus fontosságú adatok biztonsági mentését és visszaállítását végzi. |
| ![Azure Site Recovery](media/operations-management-suite-overview/icon-site-recovery.png) | Site Recovery | Biztosítja a kritikus fontosságú alkalmazások magas rendelkezésre állását. |

### <a name="log-analytics"></a>Log Analytics
A [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) figyelési szolgáltatásokat biztosít az OMS számára a felügyelt erőforrások adatainak egy központi tárházba gyűjtésével.  Ezek az adatok tartalmazhatnak eseményeket, teljesítményadatokat vagy egyéni hello API keresztül elérhető adatok. Amennyiben az összegyűjtött, riasztási, elemzés és exportálási hello adatok érhető el.  Ez a módszer lehetővé teszi a különböző forrásokból tooconsolidate adatait így kombinálhatja adatokat az Azure-szolgáltatások a a meglévő helyszíni környezetben.  Azt is egyértelműen elválasztja hello adatgyűjtés hello hello műveletet végre az adatok, így minden elérhető tooall típusú adatok.  

![A Log Analytics áttekintése](media/operations-management-suite-overview/overview-log-analytics.png)

#### <a name="collecting-data"></a>Adatok gyűjtése
Számos, hogy az adatok tud bejutni Naplóelemzési tooanalyze hello tárháza.

- **Windows vagy Linux rendszerű számítógépek és virtuális gépek.**  Telepíti a Microsoft Monitoring Agent hello [Windows](../log-analytics/log-analytics-windows-agents.md) és [Linux](../log-analytics/log-analytics-linux-agents.md) számítógépén vagy virtuális gépén, amelyet toocollect adatait.  hello ügynök automatikusan Log Analytics-konfigurációs események és a teljesítmény adatok toocollect definiáló töltik le.  Könnyedén telepítheti hello ügynök hello Azure portál használata Azure-beli virtuális gépeken.  Ha egy meglévő Operations Manager-környezettel rendelkezik, hello felügyeleti csoport tooLog Analytics csatlakozhat, és indul el automatikusan az összes meglévő ügynökök adatokat gyűjt.
- **Azure-szolgáltatások.**  Naplóelemzési a telemetriai adatokat gyűjt [Azure Diagnostics és az Azure-figyelési](../log-analytics/log-analytics-azure-storage.md) történő hello tárházba, hogy a figyelheti az Azure-erőforrások.
- **Adatgyűjtő API.**  A Log Analytics [REST API-jával bármelyik ügyfélből feltölthet adatokat](../log-analytics/log-analytics-data-collector-api.md).  Ez lehetővé teszi a harmadik felek alkalmazásaiból toocollect adatok, vagy valósítja meg az egyéni felügyeleti forgatókönyveket.  Gyakori módja toouse egy Azure Automation toocollect adatokat a runbook, majd hello adatait gyűjtője API toowrite azt toohello tárházba.

#### <a name="reporting-and-analyzing-data"></a>Jelentéskészítés és az adatok elemzése
A Naplóelemzési hello tárház tárolt hatékony lekérdezési nyelv tooextract adatokat tartalmaz.  Mivel a forrásokból származó adatok tárolása rekordként történik, egyetlen lekérdezésben akár több forrásból származó adatot is elemezhet.
  
Továbbá tooad hoc elemzés, Naplóelemzési több módon tooreport biztosít, és lekérdezés adatok elemzését.

- **Nézetek és irányítópultok.**  [Nézetek](../log-analytics/log-analytics-view-designer.md) és [irányítópultok](../log-analytics/log-analytics-dashboards.md) hello portálon lekérdezés hello eredményeinek képi megjelenítése.  Megoldások rendszerint nézeteket, amelyek hello megoldás hello adatok elemzését tartalmazza.  Is tooanalyze adatokat saját egyéni nézetek létrehozása és az egyéni portál könnyen elérhető legyen.
- **Exportálás.**  Hello beállítás tooexport hello a lekérdezés eredményeit, hogy elemezheti Naplóelemzési kívül van.  Akkor is ütemezheti az egy rendszeres exportálás túl[Power BI](../log-analytics/log-analytics-powerbi.md) jelentős képi megjelenítés és elemzési lehetőségeket biztosító.
- **Naplókeresési API.**  A Log Analytics [REST API-jával bármelyik ügyfélből gyűjthet adatokat](../log-analytics/log-analytics-log-search-api.md).  Ez lehetővé teszi a hello tárházban gyűjtött adatok tooprogrammatically munkahelyi vagy férjen hozzá egy másik felügyeleti eszköz.

#### <a name="alerting"></a>Riasztások kezelése
A Log Analytics képes [proaktívan riasztani](../log-analytics/log-analytics-alerts.md) Önt, vagy helyesbítő műveleteket végrehajtani, ha problémát észlel.  Ahogy a Log Analytics összes többi elemzése, ez is naplókeresés segítségével hajtható végre.  Ez a keresés rendszeres ütemezés szerint fut, és riasztás jön létre, ha hello eredmények adott feltételeknek.

![Log Analytics-riasztások](media/operations-management-suite-overview/overview-alerts.png)

Ezenkívül toocreating egy riasztási rekord hello Log Analytics-tárházban, riasztásokat is igénybe vehet a következő műveletek hello.

- **E-mail-cím.**  Az e-mailek küldése tooproactively értesíti az észlelt probléma.
- **Forgatókönyv.**  A Log Analytics riasztása elindíthat egy runbookot az Azure Automationben.  Ez általában tooattempt toocorrect hello észlelt probléma történik.  hello runbook indítható hello hello felhő esetet az Azure vagy egy másik felhőben, vagy probléma sikerült elindítani a helyi ügynök egy problémát a fizikai vagy virtuális gépen.
- **Webhook.**  Riasztást a webhook elindíthatja, és adja át adatokat a hello hello napló keresés eredményeit.  Ez lehetővé teszi, hogy a külső, például egy másik riasztási rendszer szolgáltatásokkal való integrációt, vagy azt megkísérelhet tootake kijavítására a lépéseket egy külső webhely.

### <a name="azure-automation"></a>Azure Automation
[Azure Automation szolgáltatásbeli](http://azure.microsoft.com/documentation/services/automation) automatizálás és a konfigurációs folyamat felügyeleti tooOMS biztosít.  Automatizálja manuálisan végrehajtott folyamatokat, és segít a fizikai és virtuális számítógépeket tooenforce konfigurációi.  

#### <a name="process-automation"></a>Folyamatautomatizálás
Az Azure Automation PowerShell-szkripteken vagy PowerShell-munkafolyamatokon alapuló [runbookok](../automation/automation-runbook-types.md) használatával automatizálja a manuális folyamatokat.  Például a változókat, amelyek több runbookok és a hitelesítő adatok és a kapcsolatok, amelyek lehetővé teszik egy runbook hitelesítéshez szükséges titkosított toostore információk között megosztható runbookok támogató eszközök is tartalmaz.
Runbookok ajánlatot folyamatok automatizálása a hello hello csomagban található más szolgáltatásokkal.  Egyes hello óta más szolgáltatások is elérhetők a PowerShell-lel vagy a REST API-n keresztül, hozhat létre runbookot tooperform bizonyos funkciókat, például a felügyeleti adatokat gyűjt a Naplóelemzési vagy a biztonsági mentéshez az Azure Backup szolgáltatás elindítása.

##### <a name="accessing-resources"></a>Erőforrások elérése
Mivel a runbookok a PowerShellen alapulnak, a PowerShell-parancsmagokkal elérhető erőforrások bármelyikét képesek kezelni.  Ha Ön [modul betöltéséhez](../automation/automation-integration-modules.md) az Automation-fiók a fiókhoz tartozó elérhető tooall runbookok válik. 
 
Amikor runbookokat hello felhőben futtatható, hello felhőből elérhető erőforrások eléréséhez.  Ezek lehetnek az Azure-előfizetésben vagy egy másik felhőben, például az Amazon Web Servicesben (AWS) található erőforrások, vagy egy REST API-n keresztül elérhető szolgáltatások.  Runbookokat hello felhő minden hitelesítő adat nem futhatnak, de Automation erőforrásokat, például a hitelesítő adatokat, a kapcsolatok és a tanúsítványok tooauthenticate tooresources hozzáféréskor is kihasználja.

Erőforrások az Adatközpont valószínűleg nem érhető el egy hello felhőben futó runbook.  Telepíthet egy vagy több [hibrid forgatókönyv-feldolgozók](../automation/automation-hybrid-runbook-worker.md) az adatok center azonban toorun runbookok toolocal erőforrások eléréséhez szükséges.  Amikor elindít egy runbookot, megadhatja, hogy lefuttassa hello felhőbe vagy egy adott munkavégző.

![Azure Automation-runbookok](media/operations-management-suite-overview/overview-runbooks.png)

##### <a name="starting-a-runbook"></a>Runbook indítása
A runbookok [számos módszerrel elindíthatók](../automation/automation-starting-a-runbook.md), így több különféle felügyeleti forgatókönyvben használhatók.  

- **Azure Portal.**  Más Azure-szolgáltatásokkal, például Azure Automation hello Azure portálon is kezelhető.  Továbbá toostarting runbookok, importálja a fájlokat, de saját hozhatnak létre.
- **Ütemezés.**  Rendszeres időközönként ütemezheti a runbookok toostart.  Ez lehetővé teszi olyan rendszeres kezelési folyamat tooautomatically ismétlés vagy adatokat gyűjthet a tooLog elemzés.
- **PowerShell és API.**  Indítsa el a runbookok és pass őket szükséges PowerShell parancsmag paraméter adatait, vagy hello Azure Automation REST API-t.  
- **Webhook.**  A webhook is létrehozható, amely lehetővé teszi runbookok esetén toobe indította el a külső alkalmazások vagy webhelyek.
- **Log Analytics-riasztás.**  A Naplóelemzési riasztást automatikusan elindíthatja egy runbook tooattempt toocorrect hello során felismert probléma hello riasztás.

#### <a name="configuration-management"></a>Konfigurációkezelés
[PowerShell kívánt állapot konfigurációs szolgáltatása (DSC)](../automation/automation-dsc-overview.md) olyan felügyeleti platform, a Windows PowerShellben, amely lehetővé teszi a toodeploy és hello konfigurációja a fizikai és virtuális gépek.  Azure Automation DSC-konfigurációk kezeli, és biztosít lekéréses hello felhőben, hogy a ügynökök hozzáférhet-e a szükséges tooretrieve konfigurációkat.

![Azure Automation DSC](media/operations-management-suite-overview/overview-dsc.png)

### <a name="azure-backup-and-azure-site-recovery"></a>Azure Backup és Azure Site Recovery
Az Azure biztonsági mentési és az Azure Site Recovery közre toobusiness folytonossági és vészhelyreállítási helyreállítási.  Minden egyes rendelkeznek, amelyek segítenek, hogy alkalmazások továbbra is elérhető Ha valamilyen okból kimaradás fordul elő, és toonormal műveletek vissza, ha a rendszer ismét online állapotú lesz tooensure szolgáltatásokat.  Mindkét szolgáltatás toohello helyreállításipont-céljai (Rpo) és helyreállítási időre vonatkozó célkitűzések (RTO) a szervezet meghatározott függ. A helyreállítási Időkorlát meghatározza hello elfogadható korlátot, amelyben adatok nem érhető el kimaradás során, és hello RTO korlátozza hello elfogadható időn, amelyben egy szolgáltatás vagy alkalmazás nem érhető el kimaradás során.

#### <a name="azure-backup"></a>Azure Backup
Az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) az adatok biztonsági mentését és helyreállítását végző szolgáltatásokat biztosít az OMS számára.  Védelmet biztosít az alkalmazásadatok számára, valamint évekig megőrzi őket minden tőkebefektetés nélkül és minimális működési költségek mellett.  Azt a fizikai és virtuális Windows kiszolgálók hozzáadása tooapplication munkaterhelések, például az SQL Server és a SharePoint adatainak biztonsági mentése.  Azt is használhatják a System Center Data Protection Manager (DPM) tooreplicate védett adatok tooAzure a redundancia és a hosszú távú tároláshoz.

Az Azure Backup védett adatainak tárolása egy meghatározott földrajzi régióban elhelyezkedő biztonságimásolat-tárolóban történik. hello adatait replikálja a rendszer belül hello ugyanabban a régióban, és, tároló hello típusától függően is lehet további rugalmasságot replikált tooanother régióját.

Az Azure Backup három alapvető alkalmazási helyzetben használható.

- **Windows rendszerű gép Azure Backup-ügynökkel.** Fájlok és mappák ügyfél vagy a Windows server biztonsági mentés közvetlenül tooyour Azure mentési tárolóval.<br><br>![Windows rendszerű gép Azure Backup-ügynökkel](media/operations-management-suite-overview/overview-backup-01.png)
- **System Center Data Protection Manager (DPM) vagy Microsoft Azure Backup Server.** Használja ki a dpm-et és a Microsoft Azure Backup Server toobackup fájlok és mappák hozzáadása tooapplication munkaterhelések, például az SQL és a SharePoint toolocal tárolási és majd replikálja az tooyour Azure mentési tárolóval. Windows és Linux rendszerű virtuális gépek támogatása Hyper-V-n vagy VMware-en.<br><br>![System Center Data Protection Manager (DPM) vagy Microsoft Azure Backup Server](media/operations-management-suite-overview/overview-backup-02.png)
- **Azure Virtual Machine Extensions.** Biztonsági mentés Windows vagy Linux virtuális gépek Azure tooyour Azure mentési tárolóval.<br><br>![Azure Virtual Machine Extensions](media/operations-management-suite-overview/overview-backup-03.png)



#### <a name="azure-site-recovery"></a>Azure Site Recovery
[Az Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) replikálását a helyszíni virtuális és fizikai gépek tooAzure vagy tooa másodlagos helyre replikálásával biztosítja az üzletmenet folytonosságát. Az elsődleges hely nem érhető el, ha rendszer átadja a toohello másodlagos helyet, hogy a felhasználók működő tartása, és sikertelen vissza, ha a rendszerek tooworking rendelés adja vissza. 

Az Azure Site Recovery magas rendelkezésre állást biztosít a kiszolgálók és alkalmazások számára.  Hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia megvalósításában a replikáció, feladatátvételével és helyreállításával helyszíni Hyper-V virtuális gépek, a VMware virtuális gépek és a fizikai Windows/Linux-kiszolgálókon. Gépek tooa másodlagos adatközpontba replikálja, vagy az Adatközpont kiterjesztésére tooAzure végez replikációt. A Site Recovery is egyszerű feladatátvételt és helyreállítási lehetőségeket biztosít a számítási feladatok számára. Integrálható az olyan vészhelyreállítási mechanizmusokkal, mint például az SQL Server AlwaysOn, valamint helyreállítási terveket kínál a több számítógépen rétegzett számítási feladatok egyszerű feladatátvételéhez.

Az Azure Site Recovery három alapvető replikációs helyzetben használható.

- **Hyper-V virtuális gépek replikálása.**  Ha a Hyper-V virtuális gépek VMM-felhőkben felügyelt, tooa másodlagos center vagy tooAzure adattárolás replikálhatja. Replikációs tooAzure van egy biztonságos internetkapcsolaton keresztül. Replikációs tooa másodlagos adatközpontba hello LAN felett van.  Hyper-V virtuális gépek nem a VMM felügyelete alatt, ha csak a tárolási tooAzure replikálhatja. Replikációs tooAzure van egy biztonságos internetkapcsolaton keresztül.<br><br>![Hyper-V virtuális gépek replikálása](media/operations-management-suite-overview/overview-siterecovery-hyperv.png)
- **VMware virtuális gépek replikálása.**  VMware virtuális gépek tooa másodlagos adatközpontba VMware vagy tooAzure tárolási futtató replikálhatja. Replikációs tooAzure akkor fordulhat elő, a pont-pont VPN vagy Azure ExpressRoute vagy biztonságos internetkapcsolaton keresztül. Replikációs tooa másodlagos adatközpontba hello InMage Scout adatcsatornán keresztül történik.<br><br>![VMware virtuális gépek replikálása](media/operations-management-suite-overview/overview-siterecovery-vmware.png)
- **A fizikai Windows- és Linux-kiszolgálók replikálása.**  Fizikai kiszolgálók tooa datacenter vagy tooAzure háttértár replikálhatja. Replikációs tooAzure akkor fordulhat elő, a pont-pont VPN vagy Azure ExpressRoute vagy biztonságos internetkapcsolaton keresztül. Replikációs tooa másodlagos adatközpontba hello InMage Scout adatcsatornán keresztül történik. Az Azure Site Recovery egy OMS megoldást, amely megjeleníti a statisztikai adatokat tartalmaz, de a műveleteket hello Azure-portálon kell használnia.<br><br>![A fizikai Windows- és Linux-kiszolgálók replikálása](media/operations-management-suite-overview/overview-siterecovery-physical.png)


A Site Recovery a metaadatokat meghatározott földrajzi Azure-régióban elhelyezkedő tárolókban tárolja. Nem replikált adatok hello Site Recovery szolgáltatásban tárolja.

## <a name="management-solutions"></a>Felügyeleti megoldások
A [felügyeleti megoldások](operations-management-suite-solutions.md) olyan előre összeállított logikakészletek, amelyek egy adott felügyeleti forgatókönyvet valósítanak meg az OMS egy vagy több szolgáltatásának használatával.  Más megoldások érhetők el, a Microsoft és a partnerek, hogy egyszerűen hozzáadhatja tooyour Azure-előfizetés tooincrease hello a befektetés értékét az OMS Szolgáltatáshoz.  Partnerként hozhat létre a saját megoldások toosupport, az alkalmazások és szolgáltatások és toousers hello Azure piactér vagy gyorsindítási sablonok révén azokat.

Egy olyan megoldás, amely kihasználja több szolgáltatások tooprovide további jó példa, hogy hello [frissítés felügyeleti megoldás](oms-solution-update-management.md).  Ez a megoldás használ hello Naplóelemzési ügynök Windows és Linux toocollect információt frissítéseit minden ügynök.  Ahol elemezhetjük egy befoglalt irányítópultot az adatok toohello Naplóelemzési tárházhoz ír.  A központi telepítés létrehozásakor az Azure Automation runbookjai használt tooinstall szükséges frissítések érhetők el.  A teljes folyamat hello portálon kezelheti, és nem kell tooworry hello alapul szolgáló részleteiről.

![Megoldás](media/operations-management-suite-overview/overview-solution.png)

A legtöbb megoldás legalább egy, a következő funkciók hello hajthat végre.

- További információ gyűjtése.  A Log Analytics különféle adatokat gyűjt az ügyfelekről és szolgáltatásokból, például eseményeket és teljesítményadatokat.  A felügyeleti megoldás a más adatforrásokból nem elérhető további információkat gyűjthet, gyakran Azure Automation-runbookok használatával.
- A gyűjtött adatok további elemzése.  A felügyeleti megoldások irányítópultokat és nézeteket tartalmaznak, amelyek az adatok elemzését és megjelenítését biztosítják.  Ezek hátsó toopredefined hivatkozás a napló, amelyek lehetővé teszik a hello toodrill keresések részletes adatokat.  Akkor is elvégezheti a kapcsolódó elemzés már lett összegyűjtött adatokon történő hello tárházba, például a biztonsági eseményeket fenyegetést jelző minták között keresése.
- Funkció hozzáadása.  Néhány Microsoft által nyújtott megoldás hello képességeit hello alapvető szolgáltatások tooprovide további funkciókat is épül.  Szolgáltatástérkép például a saját konzolján toodiscover biztosít, és leképezi a kiszolgáló és a valós idejű folyamat függőségek.
Megoldások rendszeresen közé éppen felvett tooOMS Microsoft, és így toocontinuously partnerek befektetéseit hello értékének növelése.  Keresse meg és telepítése Microsoft megoldások hello megoldások katalógus az OMS-portálon hello vagy keresse meg és hello Azure piactér a Microsoft és a partneri megoldások hello Azure-portál telepítése.  

![Megoldástár](media/operations-management-suite-overview/solution-gallery.png)


## <a name="next-steps"></a>Következő lépések
* További tudnivalók a [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) szolgáltatásról.
* További tudnivalók az [Azure Automation](../automation/automation-intro.md) szolgáltatásról.
* További tudnivalók az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) szolgáltatásról.
* További tudnivalók az [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) szolgáltatásról.
* Fedezze fel hello [megoldások, amelyek lehetővé](../log-analytics/log-analytics-add-solutions.md) a hello különböző OMS ajánlatokat. 

