---
title: "egy felhőalapú szolgáltatás aaaTesting hello teljesítményének |} Microsoft Docs"
description: "Egy felhőalapú szolgáltatás hello Visual Studio profiler használatával hello teljesítményének tesztelése"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7a5501aa-f92c-457c-af9b-92ea50914e24
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 98bd775e6ffcf948e737c5ec26399c81f4770fe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="testing-hello-performance-of-a-cloud-service"></a>Egy felhőalapú szolgáltatás hello teljesítményének tesztelése
## <a name="overview"></a>Áttekintés
A következő módokon hello tesztelheti egy felhőalapú szolgáltatás teljesítményének hello:

* Információ használható az Azure Diagnostics toocollect kérések és kapcsolatok és tooreview hely statisztikai adatokat tartalmaz a megjelenítése, hogyan hello szolgáltatás hajtja végre, az ügyfél szempontjából. tooget használatába, lásd: [diagnosztika konfigurálása az Azure felhőalapú szolgáltatásairól és a virtuális gépek](http://go.microsoft.com/fwlink/p/?LinkId=623009).
* Hello Visual Studio Profilkészítő tooget egy részletes elemzéséhez hello számítási aspektusait hogyan hello szolgáltatás fut-e használni. Mivel ez a témakör ismerteti, használhatja hello Profilkészítő toomeasure teljesítmény szolgáltatás fut az Azure-ban. Hogyan toouse hello Profilkészítő toomeasure teljesítmény szolgáltatásként fut helyi a compute emulator kapcsolatos információkért lásd: [teszteli hello teljesítményét egy Azure Cloud Service helyileg a hello Compute Emulator használatával hello Visual Studio Profilkészítő ](http://go.microsoft.com/fwlink/p/?LinkId=262845).

## <a name="choosing-a-performance-testing-method"></a>Egy teljesítménytesztelési módszer kiválasztása
### <a name="use-azure-diagnostics-toocollect"></a>Azure Diagnostics toocollect használja:
* A weblapok vagy szolgáltatások, például a kérelmek és kapcsolatok statisztikája.
* A szerepköröket, például egy szerepkör újraindítása milyen gyakran statisztika.
* A teljes memória használata, például a hello töltött idő százalékos aránya szemétgyűjtő vesz hello vagy memória hello kapcsolatos információkat a futó szerepkörök beállítása.

### <a name="use-hello-visual-studio-profiler-to"></a>Visual Studio Profilkészítő hello használata:
* Határozza meg, mely függvények hello legtöbb hosszabb időt vehet igénybe.
* Mennyi számításilag intenzív program részét képező összes szükséges idő mérését.
* Hasonlítsa össze a két verziója szolgáltatás részletes teljesítmény a jelentésekre.
* Vizsgálja meg részletesebben, mint egyedi memória kiosztásokat hello szintjét a memóriafoglalás.
* Többszálas kódban párhuzamossági problémák elemzéséhez.

Hello Profilkészítő használatakor is összegyűjtötte az adatokat egy felhőalapú szolgáltatás futtatásakor helyileg vagy az Azure-ban.

### <a name="collect-profiling-data-locally-to"></a>Gyűjtse össze a profilkészítési adatokat helyileg:
* Egy felhőalapú szolgáltatás, például az adott feldolgozói szerepkör, nem igényel szimulált valósághű terhelést hello végrehajtása egy részének hello teljesítményének ellenőrzése.
* Egy felhőalapú szolgáltatás hello teljesítményének tesztelése elkülönítve, ellenőrzött feltételek.
* Hello teljesítményének tesztelése előtt felhőalapú szolgáltatások tooAzure telepítheti.
* Egy felhőalapú szolgáltatás hello teljesítményének tesztelése közvetlenül a Microsoftnak, zavaró hello telepítéseit nélkül.
* Hello szolgáltatás hello teljesítményének tesztelése az Azure-beli költségek nélkül.

### <a name="collect-profiling-data-in-azure-to"></a>Profilkészítési adatgyűjtést az Azure szolgáltatásban:
* Egy felhőalapú szolgáltatás szimulált vagy valós terhelésnek hello teljesítményének tesztelése.
* Ez a témakör később használja hello instrumentation metódusában a profilkészítési adatokat gyűjt.
* Teljesítménytesztelési hello hello szolgáltatást a hello ugyanabban a környezetben, mikor fusson a hello szolgáltatás éles környezetben.

Általában szimulálása szokásos terhelés tootest felhőszolgáltatások vagy emelje ki a feltételeket.

## <a name="profiling-a-cloud-service-in-azure"></a>Profilkészítési egy felhőalapú szolgáltatás, az Azure-ban
A Visual Studio felhőszolgáltatás közzétételekor profil hello szolgáltatást, és adja meg a beállításokat, hogy adja meg a kívánt információkat hello profilkészítési hello. A profilkészítési munkamenetét szerepkör minden egyes példányánál. További információ a hogyan toopublish a szolgáltatás a Visual Studio eszközből: [tooan Azure Cloud Service, a Visual Studio közzététel](https://msdn.microsoft.com/library/azure/ee460772.aspx).

toounderstand teljesítmény profilkészítési a Visual Studio kapcsolatos további információkért lásd: [kezdők útmutató tooPerformance profilkészítési](https://msdn.microsoft.com/library/azure/ms182372.aspx) és [alkalmazásteljesítmény elemzése profilkészítési eszközök használatával](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

> [!NOTE]
> IntelliTrace vagy a felhőszolgáltatás közzétételekor profilkészítési engedélyezheti. Nem engedélyezhető a is.
> 
> 

### <a name="profiler-collection-methods"></a>Profilkészítő adatgyűjtési módszerek
Különböző adatgyűjtési módszerek használható profilkészítési, a teljesítménnyel kapcsolatos problémákat alapján:

* **CPU mintavételi** -ezt a módszert, amelyek hasznosak a CPU-kihasználtság problémák kezdeti elemzésének alkalmazásstatisztika gyűjti. CPU mintavételi van hello javasolt módja a legtöbb teljesítmény vizsgálatok elindítása. Alacsonyabb hatással nincs hello alkalmazás CPU mintavételi adatok összegyűjtésében adatainak összegyűjtése.
* **Instrumentation** – Ez a módszer részletes adatait gyűjti, amelyeket akkor hasznos, a részletesebb elemzéshez és a bemeneti/kimeneti teljesítménnyel kapcsolatos problémák elemzéséhez. hello instrumentation metódus rögzít minden egyes belépési, kilépési és függvény hívásához szükséges hello funkciók modulban a profilkészítési futtatása során. Ez a módszer akkor hasznos, a kód egy szakaszban részletes információkat gyűjt és hello hatása az alkalmazások teljesítményének bemeneti és kimeneti műveletek ismertetése. Ez a módszer nem végezhető el egy 32 bites operációs rendszert futtató számítógépre. Ez a beállítás érhető el csak futtatásakor hello felhőalapú szolgáltatás az Azure-ban, nem helyi a hello compute emulator.
* **.NET-memóriafoglalás** – Ez a módszer .NET-keretrendszer memória foglalási adatokat gyűjt a hello mintavételi profilkészítési módszer használatával. hello gyűjtött adatok közé tartoznak hello száma és mérete lefoglalt objektumok.
* **Párhuzamossági** – Ez a módszer összegyűjti az erőforrás versengés adatait és a folyamat és a szál végrehajtási adatok, amelyek hasznos többszálas és több folyamatból alkalmazások elemzése. hello egyidejűségi metódus számára gyűjti az adatokat minden esemény blokkok végrehajtási, a kód, például ha a szál megvárja-e a zárolt hozzáférési tooan alkalmazás erőforrás toobe felszabadult. Ez a módszer akkor hasznos, többszálas alkalmazások elemzéséhez.
* Is engedélyezheti **réteg interakció profilkészítési**, szinkron ADO.NET hello végrehajtásának lassúságát további információt a többrétegű alkalmazások, amelyek egy vagy több kommunikálnak funkciók hívások adatbázisok. Réteg interakció adatok hello profilkészítési módszerek bármelyikével hozhatja létre. Réteg interakció profilkészítési kapcsolatos további információkért lásd: [réteg kapcsolati nézet](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profilkészítési beállításainak konfigurálása
a következő ábra bemutatja hogyan hello tooconfigure hello Azure-alkalmazás közzététele párbeszédpanelen a profilkészítési beállításait.

![Profilkészítési beállításainak konfigurálása](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

> [!NOTE]
> tooenable hello **adatgyűjtés engedélyezése** jelölőnégyzetet, rendelkeznie kell hello Profilkészítő használt toopublish a felhőalapú szolgáltatás hello helyi számítógépen telepítve. Alapértelmezés szerint hello Profilkészítő Visual Studio telepítése során települ.
> 
> 

### <a name="tooconfigure-profiling-settings"></a>profilkészítési beállítások tooconfigure
1. A Solution Explorerben nyissa meg az Azure-projekt helyi menüjének hello, és válassza **közzététel**. A részletes lépéseket toopublish egy felhőalapú szolgáltatás, lásd: [felhő közzétételi szolgáltatás használatával hello Azure eszközök](http://go.microsoft.com/fwlink/p?LinkId=623012).
2. A hello **Azure-alkalmazás közzététele** párbeszédpanelen, a kiválasztott hello **speciális beállítások** fülre.
3. profilkészítési, tooenable válasszon hello **adatgyűjtés engedélyezése** jelölőnégyzetet.
4. tooconfigure a profilkészítési beállítások kiválasztása hello **beállítások** hivatkozás. hello profilkészítési beállítások párbeszédpanel jelenik meg.
5. A hello **milyen profilkészítési metódusában szeretné toouse** választókapcsolók, válassza ki, telepíteni kell, amely profilkészítési hello típusú.
6. profilkészítési adatokat, válassza hello toocollect hello réteg interakció **engedélyezése réteg interakció profilkészítési** jelölőnégyzetet.
7. toosave hello-beállítások, válassza ki a hello **OK** gombra.
   
    Ha az alkalmazás közzétételéhez, ezek a beállítások esetén az egyes szerepkörökhöz munkamenet profilkészítési használt toocreate hello.

## <a name="viewing-profiling-reports"></a>Profilkészítési jelentések megtekintése
A profilkészítési munkamenet létrehozása a felhőszolgáltatásban található egy szerepkör minden egyes példányánál. tooview minden munkamenet a Visual Studio eszközből a profilkészítési jelent, hello Server Explorer ablak tekintheti meg és válassza a hello Azure számítási csomópont tooselect szerepkör példánya. Profilkészítési jelentés, ahogy az ábra a következő hello hello tekinthető meg.

![Az Azure-ból profilkészítési jelentés megtekintése](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="tooview-profiling-reports"></a>profilkészítési jelentések tooview
1. tooview hello Server Explorer ablak a Visual Studio hello menü sáv válasszon nézetet, Server Explorer.
2. Hello Azure számítási csomópont kiválasztása, és válassza az Azure-telepítés csomópont hello hello felhőszolgáltatás kijelölt tooprofile közzé a Visual Studio.
3. hello szolgáltatást, egy adott művelethez nyitott hello helyi menüben válassza a hello szerepkör, és válassza ki egy példány tooview profilkészítési jelentések **profilkészítési jelentés megtekintése**.
   
    hello jelentés, egy .vsp fájlt most le az Azure-ból, és hello letöltési hello állapotának hello Azure tevékenységnapló jelenik meg. Hello letöltés végeztével jelentés profilkészítési hello hello szerkesztőben nevű Visual Studio egy lapján jelenik meg <Role name>  *<Instance Number>*  <identifier>.vsp. Hello jelentés összefoglaló adatai jelennek meg.
4. különböző nézeteket toodisplay hello jelentés hello aktuális nézet listában válassza ki a kívánt nézetet hello típusú. További információkért lásd: [profilkészítési eszközök jelentés nézetek](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Következő lépések
[Hibakeresési Cloud Services csomag](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Közzétételi tooan Azure Cloud Service, a Visual Studio eszközből](https://msdn.microsoft.com/library/azure/ee460772.aspx)

