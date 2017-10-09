---
title: "Azure Cloud Services és a virtuális gépek diagnosztika aaaConfiguring |} Microsoft Docs"
description: "Ismerteti, hogyan Azure cloude szolgáltatások és virtuális gépek (VM) a Visual Studio hibakeresési tooconfigure diagnosztikai információkat."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: e70cd7b4-6298-43aa-adea-6fd618414c26
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 74cdf49413d6d89a84195070dd9d817da2463373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Diagnosztika konfigurálása az Azure Felhőszolgáltatások és virtuális gépek
Ha tootroubleshoot egy Azure-felhőszolgáltatásban vagy Azure virtuális gép van szükség, konfigurálhatja az Azure diagnostics könnyebben Visual Studio használatával. Az Azure diagnostics rendszeradatok és naplózási adatok hello virtuális gépek és a felhőalapú szolgáltatás futtatásához virtuálisgép-példányok rögzíti, és átviszi az adatok az Ön által választott tárolási figyelembe. Lásd: [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](app-service-web/web-sites-enable-diagnostic-log.md) bejelentkezés az Azure diagnostics további információt.

Ez a témakör bemutatja, hogyan tooenable és konfigurálása az Azure diagnostics Visual Studio, mindkettő előtt és után központi telepítését, valamint az Azure virtuális gépeken. Azt is bemutatja, hogyan tooselect hello típusú diagnosztikai információk toocollect és hogyan tooview hello után az szolgáltatás által gyűjtött információk.

A következő módokon hello Azure Diagnostics konfigurálhatja:

* Módosíthatja a diagnosztikai beállításokat hello keresztül **diagnosztikai konfigurációja** párbeszédpanel a Visual Studióban. hello-beállítások mentése diagnostics.wadcfgx (az Azure SDK 2.4 vagy korábbi diagnostics.wadcfg) nevű fájlban. Másik megoldásként közvetlenül módosíthatja hello konfigurációs fájlt. Hello fájl manuális frissítésekor hello konfigurációs módosítások érvénybe lépéséhez hello hello cloud service tooAzure vagy hello emulátorban futtatási hello szolgáltatást telepít, amikor legközelebb.
* Használjon **Cloud Explorer** vagy **Server Explorer** a Visual Studio toochange hello diagnosztikai beállítások egy futó felhőalapú szolgáltatás, vagy a virtuális géphez.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostics változások
Azure SDK 2.6 projekteket a Visual Studio a következő módosításokat hello került sor. (Ezek is a módosítások Azure SDK toolater verziói.)

* hello helyi emulátor mostantól támogatja a diagnosztika. Ez azt jelenti, hogy diagnosztikai adatokat gyűjteni, és győződjön meg arról, az alkalmazás hoz létre a hello jobb nyomkövetési adatokat, amíg Ön fejlesztése és tesztelése a Visual Studio. kapcsolati karakterlánc hello `UseDevelopmentStorage=true` diagnosztikai adatok gyűjtésének lehetővé teszi a futtatása közben a felhőszolgáltatási projektet a Visual Studio hello az Azure storage emulator használatával. Minden diagnosztikai adatgyűjtés hello (fejlesztési tároló) tárfiókban.
* hello diagnosztika tárolási fiók kapcsolati karakterlánc (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) ismét hello szolgáltatás konfigurációs (.cscfg) fájljában tárolódik. Az Azure SDK 2.5 hello diagnosztikai tárfiók hello diagnostics.wadcfgx fájl van megadva.

Bizonyos különbségek vannak a figyelmet a jelentősebb hogyan hello kapcsolati karakterláncot az Azure SDK 2.4 és a korábbi munkaidő-e, és hogyan működik az Azure SDK 2.6-os és újabb verziók között.

* Azure SDK 2.4 és korábbi hello kapcsolati karakterlánc volt megadva a futtatókörnyezet által hello diagnosztika beépülő modul tooget hello tárfiókadatok átvitelére a diagnosztikai naplókat.
* Azure SDK 2.6 vagy újabb rendszerben hello diagnosztika kapcsolati karakterláncot a Visual Studio tooconfigure hello diagnosztika bővítmény hello megfelelő tárfiókadatok közzététele során használja. hello kapcsolati karakterláncot adja meg a Visual Studio fogja használni, amikor a közzétételi különböző szolgáltatáskonfiguráció eltérő tárfiókokból teszi lehetővé. Azonban hello diagnosztika beépülő modul (az Azure SDK 2.5) után már nem érhető el, mert hello .cscfg fájl önmagában hello diagnosztika bővítmény nem engedélyezhető. Hogy tooenable hello bővítmény külön eszközöket, például a Visual Studio vagy a PowerShell használatával.
* toosimplify hello folyamatán hello diagnosztika bővítmény a PowerShell-lel, hello csomag kimenetét a Visual Studio hello nyilvános konfigurációs XML-t az egyes szerepkörökhöz hello diagnosztika bővítmény is tartalmaz. A Visual Studio hello diagnosztika kapcsolati karakterlánc toopopulate hello tárfiókadatok hello nyilvános konfigurációs szerepel használja. hello nyilvános konfigurációs fájljainak hello bővítmények mappában jönnek létre, és kövesse a hello mintát PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. A PowerShell-alapú telepítések használhatja ezt a mintát toomap minden konfigurációs tooa szerepkör.
* hello kapcsolati karakterlánc hello .cscfg fájlban is használják hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) tooaccess hello diagnosztikai adatokat, így a hello szerepelhet **figyelés** külön-külön hello kapcsolati karakterlánc szükséges a részletes nyomon követési adatok hello portálon tooconfigure hello szolgáltatás tooshow.

## <a name="migrating-projects-tooazure-sdk-26-and-later"></a>Áttelepítése projektek tooAzure SDK 2.6-os és újabb verziók
Amikor áttelepítése az Azure SDK 2.5 tooAzure SDK 2.6 vagy újabb, ha egy diagnosztikai tárfiók hello .wadcfgx fájlban megadott majd marad van. eltérőek a tárolási konfigurációk fiókok tootake előnyeit hello rugalmasan különböző tárolót használ, a toomanually hello kapcsolati karakterlánc tooyour projekt hozzáadása. Ha az Azure SDK 2.4 vagy korábbi tooAzure SDK 2.6, amelybe migrálna egy projektet, majd hello diagnosztika kapcsolati karakterláncok megmaradnak. Vegye azonban figyelembe hello változásait hogyan kapcsolati karakterláncok számít az Azure SDK 2.6 megadott hello előző szakaszban.

### <a name="how-visual-studio-determines-hello-diagnostics-storage-account"></a>Hogyan határozza meg a Visual Studio hello diagnosztikai tárfiók
* Ha az hello .cscfg fájlban megadott diagnosztika kapcsolati karakterláncot, a Visual Studio használja tooconfigure hello diagnosztika bővítmény közzétételekor, valamint hogy mikor hello nyilvános konfigurációs XML-fájlok generálása csomagolás során.
* Nincs diagnosztika kapcsolódási karakterlánc esetén hello .cscfg fájlban, majd a Visual Studio visszavált toousing hello hello .wadcfgx tooconfigure hello diagnosztika kiterjesztés közzététele, és hello nyilvános generálása a megadott tárfiók konfigurációs XML-fájlokat, ha a csomagban.
* hello diagnosztika kapcsolati karakterlánc hello .cscfg fájlban élvez hello tárfiók hello .wadcfgx fájlban. Ha a diagnosztika a kapcsolati karakterlánc hello .cscfg fájlban megadott majd Visual Studio használ, és figyelmen kívül hagyja .wadcfgx hello storage-fiókot.

### <a name="what-does-hello-update-development-storage-connection-strings-checkbox-do"></a>What does hello "Fejlesztési tárolási kapcsolati karakterláncok frissítése..." jelölőnégyzet elvégezni?
hello jelölőnégyzetét **fejlesztési tárolási kapcsolati karakterláncok frissítése a diagnosztika és gyorsítótárazását a Microsoft Azure storage-fiók hitelesítő adataival tooMicrosoft Azure közzétételekor** lehetővé teszi egy kényelmes módszert arra tooupdate minden fejlesztési tárfiók kapcsolati karakterláncainak közzététele során megadott hello Azure storage-fiók.

Tegyük fel például, a bejelöli ezt a jelölőnégyzetet, és hello diagnosztika kapcsolati karakterlánc határoz meg `UseDevelopmentStorage=true`. Hello projekt tooAzure közzétételekor a Visual Studio automatikusan frissíti hello diagnosztika kapcsolati karakterlánc hello közzététel varázslóban megadott hello tárfiók. Azonban ha egy valódi tárfiók hello diagnosztika kapcsolati karakterláncként megadva, majd, hogy a fiókot használja.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnosztikai funkciók eltérései Azure SDK 2.4 és a korábbi és az Azure SDK 2.5-ös és újabb verziók
Ha a frissítés során a projekthez az Azure SDK 2.4 tooAzure SDK 2.5-ös vagy újabb kell szerepelnie a következő diagnosztikai funkciót különbségek szem előtt tartva hello.

* **Konfigurációs API-k elavultak** – programozható konfigurációja diagnosztikai Azure SDK 2.4 vagy korábbi verzió érhető el, de az Azure SDK 2.5-ös vagy újabb elavult. Ha a diagnosztika jelenleg definiált kódban, szüksége lesz tooreconfigure ezeket a beállításokat a hello áttelepített projektben ahhoz, hogy diagnosztika tookeep működő teljesen. hello diagnosztika konfigurációs fájl az Azure SDK 2.4 diagnostics.wadcfg, de diagnostics.wadcfgx az Azure SDK 2.5 és újabb verzióihoz.
* **Diagnosztika szolgáltatás felhőalkalmazásokhoz csak szinten hello szerepkör nem hello példány szintjén konfigurálható.**
* **Minden alkalommal, amikor az alkalmazás telepítéséhez hello diagnosztikai konfigurációja frissül** – Ha a Server Explorer a diagnosztika konfigurációjának módosítása, és ezután telepítse újra az alkalmazást paritású problémák léphetnek.
* **Azure SDK 2.5-ös vagy újabb rendszerben összeomlási memóriaképek hello diagnosztika konfigurációs fájlban, nem a kódban vannak-e konfigurálva** – Ha összeomlási memóriaképek kódban konfigurálva van, akkor kell toomanually átviteli hello konfigurációs kódot toohello konfigurációs fájlból, mivel hello összeomlási memóriaképek hello áttelepítési tooAzure SDK 2.6 során nem kerülnek.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>A cloud service projektek diagnosztika engedélyezése központi telepítés előtt
Visual Studio toocollect diagnosztikai adatainak futtatásakor hello szolgáltatás hello emulátorban üzembe helyezése előtt, az Azure-ban futó szerepkörök választhatja ki. Minden toodiagnostics beállításainak módosítása a Visual Studio program hello diagnostics.wadcfgx konfigurációs fájlba menti. Ezeket a konfigurációs beállításokat adja meg, ahol diagnosztikai adatok mentése a felhőalapú szolgáltatás telepítésekor hello tárfiók.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="tooenable-diagnostics-in-visual-studio-before-deployment"></a>központi telepítés előtt a Visual Studio tooenable diagnosztika
1. Az hello a helyi menü az Önt érdeklő hello szerepkörhöz, majd válassza **tulajdonságok**, majd válassza a hello **konfigurációs** hello szerepkör lapján **tulajdonságok** ablak.
2. A hello **diagnosztika** területen győződjön meg arról, hogy hello **engedélyezése diagnosztikai** jelölőnégyzet be van jelölve.
   
    ![Hello diagnosztika engedélyezése beállítás elérése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Tárfiókot hello három ponttal (…) gombra toospecify hello hello diagnosztikai adatok toobe tárolni kívánt. hello tárfiók választja diagnosztikai adatokat tároló hello helye lesz.
   
    ![Adja meg a hello tárolási fiók toouse](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. A hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanelen adja meg, hogy tooconnect használatával hello Azure Storage Emulator, Azure-előfizetésre, vagy szeretné manuálisan kell megadni a hitelesítő adatokat.
   
    ![Tárolási fiók párbeszédpanel](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Ha hello Microsoft Azure Storage Emulator lehetőséget választja, hello kapcsolati karakterlánc beállítása tooUseDevelopmentStorage = true.
   * A lehetőséghez hello az előfizetés, dönthet úgy hello toouse és hello fióknév kívánt Azure-előfizetés. Kiválaszthatja a hello fiókok kezelése gomb toomanage Azure-előfizetését.
   * Ha hello manuálisan a megadott hitelesítő adatok lehetőséget választja, Ön felszólító tooenter hello nevét és hello toouse kívánt Azure-fiók kulcsát.
5. Válassza ki a hello **konfigurálása** gomb tooview hello **diagnosztikai konfigurációja** párbeszédpanel megnyitásához. Minden lap (kivéve a **általános** és **napló könyvtárak**) gyűjtheti diagnosztikai adatforrás jelöli. hello alapértelmezett lapon **általános**, kínál, akkor a következő diagnosztikai adatgyűjtési beállítások hello: **csak a hibák**, **kapcsolatos összes információ**, és **egyéni terv** . alapértelmezett beállítás, hello **csak a hibák**, vesz hello tárolási legkisebb, mert az nem továbbítja figyelmeztetés vagy a nyomkövetési adatokat. hello valamennyi információ beállítás átvitelt hello legtöbb információt, és van, ezért hello legköltségesebb beállítás tárhely tekintetében.
   
    ![Az Azure diagnosztikai és a konfiguráció engedélyezése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. Ehhez a példához válassza ki a hello **egyéni terv** beállítás testreszabásához, hello adatokat gyűjteni.
7. Hello **lemezkvóta MB-ban** mező határozza meg, hogy mekkora területet, a kívánt tooallocate tárhelyét a fiókot a diagnosztikai adatok. Hello alapértelmezett érték módosítható, ha azt szeretné.
8. Az egyes lapokon diagnosztikai adatok kívánt toocollect, jelölje be a **átvitelének engedélyezése <log type>**  jelölőnégyzetet. Például, ha azt szeretné, hogy toocollect alkalmazásnaplók, válassza hello **alkalmazásnaplók adatátvitel engedélyezése** hello jelölőnégyzet **alkalmazásnaplók** fülre. Is adja meg az egyes diagnosztika adattípus által előírt bármely más információ. Című rész hello **diagnosztikai adatok forrásainak konfigurálása** a témakör későbbi részében a konfigurációs adatokat az egyes lapokon.
9. Miután engedélyezte a kívánt összes hello diagnosztikai adatok gyűjtésére, válassza ki azt a hello **OK** gombra.
10. Futtassa az Azure-felhőszolgáltatás-projekt a Visual Studio, a szokásos módon. Az alkalmazás használata során, engedélyezte hello naplóadatok kerül toohello Azure storage-fiók a megadott.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Az Azure virtuális gépeken diagnosztika engedélyezése
Visual Studio toocollect diagnosztikai adatokat az Azure virtuális gépek választhatja ki.

### <a name="tooenable-diagnostics-in-azure-virtual-machines"></a>az Azure virtuális gépeken tooenable diagnosztika
1. A **Server Explorer**, válassza ki a hello Azure csomópont, és csatlakozzon az Azure-előfizetésre, tooyour, ha még nem csatlakozott.
2. Bontsa ki a hello **virtuális gépek** csomópont. Hozzon létre egy új virtuális gépet, vagy adja meg, hogy már telepítve van.
3. Az hello a helyi menü az Önt érdeklő hello virtuális géphez, majd válassza **konfigurálása**. Ez azt jelenti, hello virtuális gép konfigurálása párbeszédpanel.
   
    ![Egy Azure virtuális gép konfigurálása](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Ha még nincs telepítve, adja hozzá az hello Microsoft Monitoring Agent diagnosztika bővítményt. A bővítmény lehetővé teszi a hello Azure virtuális gép diagnosztikai adatainak összegyűjtése. A hello telepített bővítmények listájában hello válasszon egy elérhető bővítmény legördülő menüben válassza ki, és válassza a Microsoft Monitoring Agent diagnosztika.
   
    ![Egy Azure virtuálisgép-bővítmény telepítése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
   > [!NOTE]
   > Más diagnosztikai bővítmények érhetők el a virtuális gépek számára. További információkért lásd: Azure Virtuálisgép-bővítmények és a szolgáltatásokat.
   > 
   > 
5. Válassza ki a hello **Hozzáadás** tooadd hello bővítményt, és megtekintése gombra a **diagnosztikai konfigurációja** párbeszédpanel.
6. Válassza ki a hello **konfigurálása** toospecify tárfiók gombra, majd válassza a hello **OK** gombra.
   
    Minden lap (kivéve a **általános** és **napló könyvtárak**) gyűjtheti diagnosztikai adatforrás jelöli.
   
    ![Az Azure diagnosztikai és a konfiguráció engedélyezése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    hello alapértelmezett lapon **általános**, kínál, akkor a következő diagnosztikai adatgyűjtési beállítások hello: **csak a hibák**, **kapcsolatos összes információ**, és **egyéni terv** . alapértelmezett beállítás, hello **csak a hibák**, vesz hello tárolási legkisebb, mert az nem továbbítja figyelmeztetés vagy a nyomkövetési adatokat. Hello **kapcsolatos összes információ** beállítás átvitelek hello legtöbb információt, ezért hello legköltségesebb lehetősége, tárhely tekintetében.
7. Ehhez a példához válassza ki a hello **egyéni terv** beállítás testreszabásához, hello adatokat gyűjteni.
8. Hello **lemezkvóta MB-ban** mező határozza meg, hogy mekkora területet, a kívánt tooallocate tárhelyét a fiókot a diagnosztikai adatok. Hello alapértelmezett érték módosítható, ha azt szeretné.
9. Az egyes lapokon diagnosztikai adatok kívánt toocollect, jelölje be a **átvitelének engedélyezése <log type>**  jelölőnégyzetet.
   
    Például, ha azt szeretné, hogy toocollect alkalmazásnaplók, válassza hello **alkalmazásnaplók adatátvitel engedélyezése** hello jelölőnégyzet **alkalmazásnaplók** fülre. Is adja meg az egyes diagnosztika adattípus által előírt bármely más információ. Című rész hello **diagnosztikai adatok forrásainak konfigurálása** a témakör későbbi részében a konfigurációs adatokat az egyes lapokon.
10. Miután engedélyezte a kívánt összes hello diagnosztikai adatok gyűjtésére, válassza ki azt a hello **OK** gombra.
11. Mentse a frissített hello projektet.
    
     Megjelenik egy üzenet hello **Microsoft Azure tevékenységnapló** , amelyen a virtuális gép hello frissítve lett.

## <a name="configure-diagnostics-data-sources"></a>Diagnosztikai adatok forrásainak konfigurálása
Diagnosztikai adatok gyűjtésének engedélyezése után dönthet úgy, hogy pontosan milyen toocollect és gyűjtött információk köréről kívánt adatforrásokat ki. hello az alábbiakban olvashat egy listát hello lapfülek **diagnosztikai konfigurációja** párbeszédpanel megnyitásához, és azt jelenti, hogy minden egyes konfigurációs beállítást.

### <a name="application-logs"></a>Alkalmazás-naplók
**Alkalmazásnaplók** egy webes alkalmazás által létrehozott diagnosztikai információkat tartalmaznak. Ha toocapture alkalmazásnaplók, jelölje be a hello **alkalmazásnaplók adatátvitel engedélyezése** jelölőnégyzetet. Akkor növelheti vagy csökkentheti a hello hány perc hello alkalmazásnaplók meghatározásakor tooyour tárfiók hello módosításával **átviteli időtartam (perc)** érték. Hello naplóban rögzített érték hello naplózási szint hello adatmennyiség is módosíthatja. Például kiválaszthatja **részletes** tooget további információt, vagy válasszon **kritikus** toocapture csak kritikus hibák. Ha egy adott diagnosztika szolgáltatót, amelyet alkalmazásnaplók bocsát ki, rögzítheti őket hello szolgáltató GUID toohello hozzáadásával **szolgáltató GUID** mezőbe.

  ![Alkalmazás-naplók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Lásd: [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](app-service-web/web-sites-enable-diagnostic-log.md) alkalmazásnaplók további információt.

### <a name="windows-event-logs"></a>Windows-Eseménynapló
Ha azt szeretné, hogy a Windows-Eseménynapló toocapture, válassza a hello **engedélyezése a Windows-Eseménynapló átviteli** jelölőnégyzetet. Akkor növelheti vagy csökkentheti a hello hány perc hello eseménynaplók meghatározásakor tooyour tárfiók hello módosításával **átviteli időtartam (perc)** érték. Válassza ki a megjeleníteni kívánt tootrack hello eseménytípusokról hello jelölőnégyzetét.

  ![Eseménynaplók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Ha az Azure SDK 2.6-os vagy újabb verzióját használja, és szeretné, hogy toospecify egyéni adatforrás, akkor adja meg a hello  **<Data source name>**  szöveg mezőbe, majd válassza a hello **hozzáadása** gomb következő tooit. hello adatforrás fel van véve toohello diagnostics.cfcfg fájlt.

Ha az Azure SDK 2.5 használja, és szeretné, hogy egy egyéni adatforrás toospecify, ezt hozzáadhatja toohello `WindowsEventLog` hello diagnostics.wadcfgx szakasza fájlba, például a következő példa hello.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Teljesítményszámlálók
Teljesítményszámláló adatai segítségével keresse meg a rendszer szűk keresztmetszetei és finomhangolhatja, rendszer és az alkalmazás teljesítményét. Lásd: [létrehozása és a teljesítményszámlálók használata az Azure alkalmazásban](https://msdn.microsoft.com/library/azure/hh411542.aspx) további információt. Ha toocapture teljesítményszámlálókat, jelölje be a hello **teljesítményszámlálók átvitel engedélyezése** jelölőnégyzetet. Akkor növelheti vagy csökkentheti a hello hány perc hello eseménynaplók meghatározásakor tooyour tárfiók hello módosításával **átviteli időtartam (perc)** érték. Hello jelölőnégyzetek a hello teljesítményszámlálóinak, amelyet az tootrack.

  ![Teljesítményszámlálók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

tootrack teljesítményszámláló nem szerepel a listában, adja meg a javasolt szintaxis hello, és válassza a hello **Hozzáadás** gombra. hello virtuális gépen lévő hello operációs rendszer meghatározza, hogy mely teljesítményszámlálókat, követheti nyomon. Szintaxissal kapcsolatos további információkért lásd: [a számláló elérési útjának](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Infrastruktúra-naplók
Ha azt szeretné, hogy toocapture infrastruktúra naplófájlokban, hello Azure diagnosztikai infrastruktúra, hello RemoteAccess modul és hello RemoteForwarder modult információkat tartalmaznak, válassza ki a hello **az infrastruktúra-naplókátvitelengedélyezése**jelölőnégyzetet. Akkor növelheti vagy csökkentheti a hello hány perc hello naplók meghatározásakor tooyour tárfiók hello átviteli időtartam (perc) értékét.

  ![Diagnosztika infrastruktúra naplók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Lásd: [naplózási adatok gyűjtése Azure Diagnostics használatával](https://msdn.microsoft.com/library/azure/gg433048.aspx) további információt.

### <a name="log-directories"></a>Napló könyvtárak
Ha azt szeretné, hogy toocapture napló könyvtárak, az Internet Information Services (IIS) kérelmek napló könyvtárak gyűjtött adatokat tartalmaznak, a sikertelen kérelmek vagy mappákba, jelölje be az hello **napló könyvtárakátvitelengedélyezése**jelölőnégyzetet. Akkor növelheti vagy csökkentheti a hello hány perc hello naplók meghatározásakor tooyour tárfiók hello módosításával **átviteli időtartam (perc)** érték.

Kiválaszthatja a kívánt toocollect, többek között a hello naplók hello mezőkbe **IIS-napló** és **sikertelen kérelem** naplókat. Alapértelmezett tároló tároló neveket, de hello nevét módosíthatja, ha azt szeretné.

Is bármely mappában származó naplók rögzítése. Csak hello hello elérési útvonalát adja meg **naplózási könyvtár abszolút a** szakaszt, és válassza a hello **könyvtár hozzáadása** gombra. hello naplókat fog rögzíthetők toohello megadott tárolók.

  ![Napló könyvtárak](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW-naplók
Ha [Windows esemény-nyomkövetése](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) és szeretné, hogy toocapture ETW naplókat, jelölje be hello **az ETW-naplók átvitel engedélyezése** jelölőnégyzetet. Akkor növelheti vagy csökkentheti a hello hány perc hello naplók meghatározásakor tooyour tárfiók hello módosításával **átviteli időtartam (perc)** érték.

hello események kerülnek rögzítésre eseményforrások és eseményjegyzékfájlok megadott. egy eseményforrás toospecify adjon meg egy nevet a hello **eseményforrások** szakaszt, és válassza a hello **eseményforrás hozzáadása** gombra. Hasonlóképpen, megadhat egy esemény jegyzékfájl hello **esemény jelentkezik** szakaszt, és válassza a hello **esemény Manifest hozzáadása** gombra.

  ![ETW-naplók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  hello ETW keretrendszer támogatott ASP.NET keresztül az osztályokat hello [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) névtér. hello Microsoft.WindowsAzure.Diagnostics névtér, amely örökli és standard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) osztályok, () [System.Diagnostics.aspx] hello használata lehetővé teszi, hogy kiterjeszti https://msdn.microsoft.com/library/System.Diagnostics (v=vs.110), egy naplózási keretrendszert a hello Azure környezetben. További információkért lásd: [érvénybe vezérlő a naplózás és nyomkövetés a Microsoft Azure-ban](https://msdn.microsoft.com/magazine/ff714589.aspx) és [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Összeomlási memóriaképek
Ha azt szeretné, ha a szerepkör példánya összeomlik toocapture információt, jelölje be a hello **összeomlási memóriaképek adatátvitel engedélyezése** jelölőnégyzetet. (Az ASP.NET kezeli a legtöbb kivételeket, mert azt csak a feldolgozói szerepkörök számára általában hasznos.) Akkor növelheti vagy csökkentheti a hello százalékos tárolási terület toohello összeomlási memóriaképek fordított hello módosításával **kvóta (%)** érték. Ha hello összeomlási memóriaképek tárolják, és kiválaszthatja, hogy valóban toocapture hello tároló módosíthatja egy **teljes** vagy **Mini** biztonsági másolat.

jelenleg nyomon követett hello folyamatok vannak felsorolva. Válassza ki a megjeleníteni kívánt toocapture hello folyamatok hello jelölőnégyzetét. tooadd egy másik folyamat toohello lista hello folyamat nevét adja meg, és válassza a hello **hozzáadása folyamat** gombra.

  ![Összeomlási memóriaképek](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Lásd: [érvénybe vezérlő a naplózás és nyomkövetés a Microsoft Azure-ban](https://msdn.microsoft.com/magazine/ff714589.aspx) és [Microsoft Azure Diagnostics rész 4: egyéni naplózási összetevők és az Azure Diagnostics 1.3 módosítások](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) további információt.

## <a name="view-hello-diagnostics-data"></a>Hello diagnosztikai adatok megtekintése
Egy felhőalapú szolgáltatás, vagy egy virtuális gép hello diagnosztikai adatok begyűjtését követően megtekintheti.

### <a name="tooview-cloud-service-diagnostics-data"></a>tooview cloud service diagnosztikai adatok
1. A felhőalapú szolgáltatás, mint a szokásos telepítése, és futtassa azt.
2. A tárfiókban lévő hello diagnosztikai adatokat egy jelentést, amely a Visual Studio állít elő, vagy a táblákban tekintheti meg. Nyissa meg a jelentés, tooview hello adatainak **Cloud Explorer** vagy **Server Explorer**, nyissa meg az Önt érdeklő hello szerepkör hello csomópont hello helyi menüt, és válassza **diagnosztikai adatok megtekintése** .
   
    ![Diagnosztikai adatok megtekintése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    Hello elérhető adatokat tartalmazó jelentés jelenik meg.
   
    ![A Microsoft Azure diagnosztikai jelentés a Visual Studióban](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    Hello legfrissebb adatok nem jelennek meg, ha lehetséges, hogy a hello átviteli időszak tooelapse toowait.
   
    Válassza ki a hello **frissítése** tooimmediately frissítés hello adatok hivatkozásra, vagy válasszon időközönkénti hello **automatikus frissítésének** legördülő lista mezőben toohave hello adatok frissítése automatikusan. tooexport hello hiba adatokat, válassza ki a hello **tooCSV exportálása** gomb toocreate egy vesszővel tagolt fájlt egy táblázatot is megnyithatja.
   
    A **Cloud Explorer** vagy **Server Explorer**, nyissa meg a hello telepítési társított hello tárfiók.
3. Nyissa meg a hello diagnosztika táblák hello tábla megjelenítőben, és tekintse át az összegyűjtött adatok hello. Az IIS-naplókba és egyéni naplókat nyissa meg a blob-tároló. A következő táblázat hello megtekintésével található hello tábla vagy a blob tároló, amely az Önt érdeklő hello adatokat tartalmaz. Továbbá a toohello adatait, amely naplófájlt, hello táblabejegyzéseket EventTickCount, DeploymentId, szerepkör tartalmazza, és milyen virtuális gép és a szerepkör azonosítása RoleInstance toohelp generált hello adatokat és mikor. 
   
   | Diagnosztikai adatok | Leírás | Hely |
   | --- | --- | --- |
   | Alkalmazás-naplók |Az naplófájlokat, amelyek a kódot állít elő, hello System.Diagnostics.Trace osztály metódusai meghívásával. |WADLogsTable |
   | Eseménynaplók |Ezek az adatok van a Windows Eseménynapló hello hello virtuális gépeken. Windows ezek a naplók információkat tárol, de az alkalmazásokhoz és szolgáltatásokhoz is használhatja őket tooreport hibák, vagy naplózza az adatokat. |WADWindowsEventLogsTable |
   | Teljesítményszámlálók |Adatok hozhatja létre az összes teljesítményszámláló hello virtuális gépen elérhető. operációs rendszer hello szolgáltatásban olyan teljesítményszámlálók, többek között számos statisztikákról, például a memória kihasználtsága és a processzoridő tekintetében. |WADPerformanceCountersTable |
   | Infrastruktúra-naplók |Ezek a naplók hello diagnosztika infrastruktúrától maga jönnek létre. |WADDiagnosticInfrastructureLogsTable |
   | IIS-napló |Ezek a naplók rögzítik a webes kérelmek. A felhőalapú szolgáltatás lekérdezi a jelentős mennyiségű forgalom, ha ezek a naplók lehet túl hosszú, így össze kell gyűjtenie és adatainak tárolásához, csak akkor, ha esetleg szükség lenne rá. |Sikertelen kérelem bejelentkezik hello blob tárolóhoz tartozó üvegvatta-az iis-failedreqlogs egy adott központi telepítés, a szerepkör és a példány elérési úton található. Teljes naplók üvegvatta-az iis-naplófájlok alatt található. A fájl tételek hello WADDirectories táblában. |
   | Összeomlási memóriaképek |Ezt az információt nyújt a felhőalapú szolgáltatás folyamat (általában a feldolgozói szerepkör) bináris képek. |üvegvatta-crush-memóriaképek blob tároló |
   | Egyéni naplófájlok |Naplók meg az előre megadott adatok. |A tárfiókban lévő kód hello egyéni naplófájlok helyét adhat meg. Például egy egyéni blob tároló is megadhat. |
4. Ha bármilyen típusú adatok csonkolva van, próbálja meg az adott adattípus vagy az adatok továbbítása hello virtuális gép tooyour tárfiókból időközétől idő lerövidítése között hello növekvő hello puffer.
5. (választható) Kiürítési adatait hello tárolási fiók alkalmanként tooreduce általános tárolási költségek.
6. Ha így tesz, teljes körű telepítésére, hello diagnostics.cscfg fájl (az Azure SDK 2.5 .wadcfgx) frissítése az Azure-ban, és a felhőalapú szolgáltatás szerzi be a módosítások tooyour diagnosztika beállításra. Ha, ehelyett frissít egy meglévő központi telepítési, hello .cscfg fájl az Azure-ban nem frissíti. Diagnosztikai beállítások azonban továbbra is módosítani hello hello következő szakaszának lépéseit követve. Teljes körű telepítésére végrehajtása és a meglévő telepítés frissítését kapcsolatos további információkért lásd: [Azure alkalmazás közzététele varázsló](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="tooview-virtual-machine-diagnostics-data"></a>Virtuálisgép-diagnosztika adatokat tooview
1. A virtuális gép hello hello helyi menüben válasszon **diagnosztikai adatok megtekintéséhez**.
   
    ![Diagnosztikai adatok megtekintése az Azure virtuális gépben](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    Ekkor megnyílik a hello **diagnosztika összegzés** ablak.
   
    ![Virtuális gép az Azure diagnostics összefoglaló](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    Hello legfrissebb adatok nem jelennek meg, ha lehetséges, hogy a hello átviteli időszak tooelapse toowait.
   
    Válassza ki a hello **frissítése** tooimmediately frissítés hello adatok hivatkozásra, vagy válasszon időközönkénti hello **automatikus frissítésének** legördülő lista mezőben toohave hello adatok frissítése automatikusan. tooexport hello hiba adatokat, válassza ki a hello **tooCSV exportálása** gomb toocreate egy vesszővel tagolt fájlt egy táblázatot is megnyithatja.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>A telepítés utáni cloud service diagnosztika konfigurálása
Ha Ön most vizsgálja egy felhőalapú szolgáltatás probléma, hogy már fut, érdemes toocollect adatok, amelyek eredetileg telepített hello szerepkör előtt nem adott meg. Ebben az esetben is elindítható toocollect adatok a Server Explorer hello-beállítások használatával. Szerepkör, attól függően, hogy hello diagnosztika konfigurálása párbeszédpanel megnyitása hello helyi menüje hello példány vagy hello szerepkör egyetlen példányt vagy példányainak hello diagnosztika konfigurálhatja. Hello szerepkör csomópont állítja be, ha a módosítások alkalmazása tooall példányok. Hello példány csomópont állítja be, ha a módosítások alkalmazása toothat példány csak.

### <a name="tooconfigure-diagnostics-for-a-running-cloud-service"></a>egy futó felhőszolgáltatás tooconfigure diagnosztika
1. A Server Explorer eszközben bontsa ki a hello **Felhőszolgáltatások** csomópontot, majd bontsa ki a csomópontok toolocate hello szerepkör vagy a példány, amelyet tooinvestigate vagy mindkettőt.
   
    ![Diagnosztika konfigurálása](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. A hello helyi menüje egy példányt vagy egy szerepkör csomópontot, válassza **frissítés diagnosztikai beállítások**, és válassza a hello megjeleníteni kívánt toocollect diagnosztikai beállítások.
   
    Hello konfigurációs beállításaival kapcsolatos további információkért lásd: **diagnosztikai adatok forrásainak konfigurálása** ebben a témakörben. További információ a hogyan tooview hello diagnosztikai adatok: **hello diagnosztikai adatainak megtekintése** ebben a témakörben.
   
    Ha módosítja az adatok gyűjtését **Server Explorer**, ezeket a változásokat érvényben maradnak, amíg a teljes újratelepíti a felhőalapú szolgáltatás. Ha hello alapértelmezett közzétételi beállítások, hello módosítások nem lesznek felülírva, a hello alapértelmezett közzététele óta beállítás tooupdate hello meglévő telepített példányát, nem pedig tegye a teljes újbóli üzembe helyezése. toomake meg arról, hogy hello beállítások törölje a központi telepítéskor lépjen toohello **speciális beállítások** lap hello közzététele varázsló, és törölje a jelet hello **üzemelő példány frissítése** jelölőnégyzetet. Újratelepítésekor az tartozó jelölőnégyzet nincs bejelölve, a hello beállításai visszaállnak toothose hello .wadcfgx (vagy .wadcfg) fájlban beállított hello tulajdonságok szerkesztő hello szerepkör keresztül. Ha a telepítés frissítéséhez Azure tartja a régi hello-beállítások.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Azure cloud service hibák elhárítása
Tapasztal problémákat a felhőszolgáltatás-projektekre nézve, például egy szerepkör, amely lekérdezi a "elfoglalt" állapot Beragadt ismételten újraindul, vagy egy belső kiszolgálóhiba jelez, eszközöket és technikákat toodiagnose használja, és a problémák elhárításának van. Adott példák a gyakori problémák és megoldások, valamint áttekintést hello fogalmakat és eszközök toodiagnose használja, és hárítsa el az ilyen hibák, a következő témakörben: [Azure PaaS számítási diagnosztikai adatainak](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Kérdések és válaszok
**Mi hello puffer méretét, és hogyan nagy legyen?**

Összes virtuálisgép-példányt kvóták korlátozzák, mennyi diagnosztikai adatok tárolhatók a helyi fájlrendszer hello. Továbbá adja meg a puffer mérete az egyes elérhető diagnosztikai adatokat. A puffer mérete úgy viselkedik, mint egy egyéni kvóta az adott adatok. Hello párbeszédpanel alsó részén hello ellenőrzésével meghatározhatja általános kvóta és a fennmaradó memória mennyisége hello hello. Nagyobb puffert, vagy további típusú adatokat ad meg, ha lesz közelítse hello általános kvótát. Módosíthatja a hello általános kvóta hello diagnostics.wadcfg/.wadcfgx konfigurációs fájl módosításával. hello diagnosztikai adatok tárolása hello azonos fájlrendszer az alkalmazás adatként, ha az alkalmazás nagy mennyiségű lemezterületet, hello nem növeli általános diagnosztika kvótát.

**Mi hello átviteli időszak, és mennyi ideig legyen?**

hello átviteli időszak: hello, hogy mennyi idő eltelte közötti adatokat rögzíti. Minden adatátviteli idő után van áthelyezni az adatokat hello helyi fájlrendszer a egy virtuális gép tootables tárfiókba. Hello gyűjtött adatok mennyisége meghaladja a hello kvótát hello átviteli időszak vége előtt, ha a rendszer régebbi adatokat törli. Ha most adatok elvesztése, mert az adatok mennyisége meghaladja a pufferméretet hello vagy általános kvóta hello érdemes lehet toodecrease hello átviteli időszak.

**Milyen időzóna vannak hello az időbélyegző?**

hello időbélyegeket hello helyi időzóna szerint hello adatközpont, amelyen a felhőalapú szolgáltatás szerepelnek. hello következő három Timestamp típusú oszlop hello napló táblák szolgálnak.

* **PreciseTimeStamp** hello ETW időbélyegzője hello esemény. Ez azt jelenti, hogy hello idő hello naplóz hello ügyfélről.
* **Timestamp típusú** PreciseTimeStamp toohello feltöltési gyakoriságának határ lefelé kerekíteni. Ezért ha a feltöltési gyakoriságának 5 perc és hello esemény ideje 00:17:12, Timestamp típusú lesz 00:15:00.
* **Timestamp típusú** hello időbélyegzője, mely hello entitás hello Azure-tábla jött létre.

**Hogyan kezelhetők a költségek diagnosztikai adatainak összegyűjtése?**

alapértelmezett beállítások hello (**naplózási szintjének** túl beállítása**hiba** és **átviteli időszak** túl beállítása**1 perc**) kialakított toominimize költsége. A számítási költségek növekszik, ha további diagnosztikai adatok gyűjtésére, vagy csökkentse hello átviteli időtartamot. Nem gyűjt több adatot kell, és ne feledkezzen toodisable adatok gyűjtését, ha már nincs szüksége. Mindig engedélyezheti azt újra, még akkor is futásidőben, ahogy az előző szakaszban hello.

**Hogyan gyűjthetem be a sikertelen kérelmek naplóit IIS-ből?**

Alapértelmezés szerint az IIS nem naplógyűjtéshez sikertelen kérelem. Konfigurálhatja az IIS toocollect őket, ha szerkeszti a web.config fájlt a webes szerepkör hello.

**Nyomkövetési adatok a RoleEntryPoint módszerek, például a OnStart nem érkezik. mi a baj?**

hello módszerek RoleEntryPoint WAIISHost.exe, nem az IIS hello környezetében nevezzük. Ezért hello konfigurációs adatokat a Web.config fájlban, amely általában lehetővé teszi, hogy nyomkövetés nem vonatkozik. tooresolve probléma .config fájl tooyour webes szerepkör projekt hozzáadása, és nevezze hello fájl toomatch hello kimeneti tartalmazó szerelvény hello RoleEntryPoint kódot. Hello alapértelmezett webes szerepkör projekt hello .config fájl neve hello WAIISHost.exe.config lenne. Majd adja hozzá a következő sorokat toothis fájl hello:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Most, a hello **tulajdonságok** ablakban, a set hello **tooOutput Directory másolása** tulajdonság túl**mindig másolása**.

## <a name="next-steps"></a>Következő lépések
toolearn Azure, a bejelentkezés diagnosztika kapcsolatos további információkért lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services/cloud-services-dotnet-diagnostics.md) és [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](app-service-web/web-sites-enable-diagnostic-log.md).

