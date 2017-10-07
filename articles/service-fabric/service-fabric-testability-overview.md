---
title: "aaaFault az Analysis Services áttekintése |} Microsoft Docs"
description: "Ez a cikk ismerteti a hello tartalék elemzési szolgáltatás a Service Fabric hibák hogy és tesztelési forgatókönyvek futtatott a szolgáltatásokhoz."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: timlt
editor: vturecek
ms.assetid: 1f064276-293a-4989-a513-e0d0b9fdf703
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: deac16ec830aa10d4e488e60691faa9ef2b6cd33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-fault-analysis-service"></a>Bevezetés toohello hiba az Analysis Services
hello hiba az Analysis Services szolgáltatásokról, amelyek a Microsoft Azure Service Fabric épülnek tesztelési tervezték. Hiba az Analysis Services hello idéz elő a jelentéssel bíró hibák, és teljes Tesztelési forgatókönyvek az alkalmazások futtatásához. Ezeket a hibákat és forgatókönyvek gyakorolja és érvényesítése hello számos állapotok és átmenetek, amelyeknek a szolgáltatás teljes élettartamuk, minden olyan módon ellenőrzött, biztonságos és konzisztens.

A műveletek olyan hello célzó vizsgálja, hogy a szolgáltatás egyes hibák. A szolgáltatás fejlesztők építőelemeket bonyolult toowrite forgatókönyvek ezek segítségével. Példa:

* Indítsa újra a csomópont toosimulate tetszőleges számú helyzetekben, ahol egy számítógép vagy virtuális gép újraindítása után.
* Helyezze át az állapotalapú szolgáltatási toosimulate terheléselosztás, feladatátvétel vagy az alkalmazásfrissítés replikáját.
* Egy állapotalapú szolgáltatási toocreate olyan helyzet, ahol nem folytatható az írási műveletek, mert nincs elég "biztonsági másolat" vagy "másodlagos" replikák tooaccept új adatokat a kvórum elvesztése meghívni.
* Egy állapotalapú szolgáltatási toocreate olyan helyzet, ahol minden a memóriában teljesen tárolt adatok törléséig kimenő adatvesztéshez meghívni.

Forgatókönyvek a következők egy vagy több művelet álló összetett műveletek. Hiba az Analysis Services hello két beépített teljes eseteit tartalmazza:

* Chaos forgatókönyv
* Feladatátvételi forgatókönyv

## <a name="testing-as-a-service"></a>Szolgáltatásként tesztelése
Hiba az Analysis Services hello a Service Fabric-szolgáltatás, amely a Service Fabric-fürt automatikusan elindul. Ez a szolgáltatás gazdaként hello van, a vizsgálat a forgatókönyv végrehajtási és állapotelemzésre. 

![Hiba az Analysis Services][0]

A tartalék művelet vagy teszt forgatókönyv elindításakor egy parancs el lett küldve toohello hiba az Analysis Services toorun hello tartalék művelet vagy teszt forgatókönyv. hello hiba az Analysis Services rendszer állapotalapú, így azt megbízhatóan hibák és a forgatókönyvek futtatása, és ellenőrizze az eredményeket. Például egy hosszú futású tesztkörnyezet megbízhatóan által futtatható hello hiba az Analysis Services. És tesztek vannak végrehajtott hello fürtben található, mert hello szolgáltatás ellenőrizheti hello fürt hello állapotát és a szolgáltatások tooprovide hibákkal kapcsolatos további információt.

## <a name="testing-distributed-systems"></a>Elosztott rendszerek tesztelése
A Service Fabric miatt hello írást, és jelentősen egyszerűbb elosztott méretezhető alkalmazások kezelése. Hiba az Analysis Services hello teszi Hasonlóképpen könnyebb egy elosztott alkalmazás tesztelése. Számos három fő során felmerülő kérdések toobe tesztelése során lehet megoldani.

1. Olyan valós forgatókönyv előforduló hibák szimulálva/generálása: hello fontos szempontja a Service Fabric egyik, hogy lehetővé teszi, hogy a különféle hibák elosztott alkalmazások toorecover. Azonban, hogy az alkalmazás hello tootest képes toorecover felderítenie, igazolnia kell a mechanizmus toosimulate/készítése ezek valós hibák ellenőrzött tesztelési környezetben.
2. hello képességét toogenerate korrelált hibák: hello rendszerén, például hálózati hibák és a gép hibák, az alapszintű hibák könnyen tooproduce külön-külön. Nem triviális létrehozása, amely akkor fordulhat elő, a valós életben hello hello kapcsolati ezeket az egyéni hibák miatt forgatókönyvek jelentős számú.
3. Egységes felhasználói élmény fejlesztésére és üzembe különböző szintjei között: fault injektálási sok rendszert, amelyek képesek különböző típusú hibák. Azonban mindegyik hello élményt esetén gyenge tér át egy beépített fejlesztői forgatókönyvek, azonos tesztek nagy tesztkörülmények között, toousing azokat toorunning hello teszteli, éles környezetben.

Amíg a problémák, hello azonos kötelező garanciák – összes hello módja egy beépített fejlesztői környezetből, a rendszer sok mechanizmusok toosolve vannak éles fürt--tootest hiányzik. Hiba az Analysis Services hello a fejlesztőket hello alkalmazás tesztelése az üzleti logika összpontosíthat. Hiba az Analysis Services hello biztosít minden hello képességet szükséges tootest hello hello szolgáltatás rendszerrel folytatott kommunikációjuktól hello alapul szolgáló elosztott.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Valós hibát jelentő forgatókönyvek szimulálva/létrehozása
tootest hello szolgáltatás megbízhatóságára egy elosztott rendszer-hibákkal szemben, igazolnia kell a mechanizmus toogenerate hibák. A elméletileg generálásához. a hiba, például egy csomópont úgy tűnik, könnyen, szerezze meg a hello kezdődik azonos készlet konzisztencia problémákat, hogy a Service Fabric toosolve tett kísérlet. Tegyük fel Ha azt szeretné, hogy le a csomópont tooshut hello szükséges munkafolyamat hello következő:

1. Hello ügyféltől adjon ki egy leállítási csomópont kérelmet.
2. Hello kérelem toohello helyes csomópont küldése.
   
    a. Ha a hello csomópont nem található, a kell sikertelen.
   
    b. Ha hello csomópont található, az kell visszaadnia csak ha hello csomópont le van állítva.

tooverify hello sikertelen teszt szempontjából, meg kell, hogy ez a hiba előidézett, hello hiba ténylegesen történnek tooknow hello teszt. Service Fabric biztosító hello garantált, hogy a csomópont vagy hello halad, vagy már korábban le volt hello parancs elérésekor hello csomópont. Mindkét esetben hello a teszt kell kell tudni toocorrectly OK hello állapotával kapcsolatos információkat és lesz sikeres vagy sikertelen megfelelően az érvényesítési. A rendszer a Service Fabric toodo hello azonos beállítása sikertelen volt számos hálózati találat, a hardver és szoftverekkel kapcsolatos problémák, amely megakadályozná a biztosító hello előző garanciák kívül megvalósítva. Hello jelenlétében is hangsúlyoztuk, mielőtt a Service Fabric hello fürt állapot toowork hello kapcsolatos hibák elhárítása konfigurálja újra, és ezért hello hiba az Analysis Services hello problémák továbbra is képes toogive hello jobb hoznak garanciák.

### <a name="generating-required-events-and-scenarios"></a>Szükséges események és forgatókönyvek létrehozása
Míg a valós meghibásodásának következetesen szimulálása a bonyolult toostart, hello képességét korrelált toogenerate hibák is szigorúbb. Például adatvesztés történik a megőrzött állapot-nyilvántartó szolgáltatás dolgok következő hello fordulhat elő:

1. Csak egy írási kvórum hello replikák észlelt a replikáció. Összes hello másodlagos replika elsődleges hello verziót használják.
2. hello írási kvórum (megfelelő tooa kódcsomag vagy fog csomópont) lesz hello replikák miatt leáll.
3. hello írási kvórum nem helyreállnak mert hello hello adatkészleteként (megfelelő toodisk sérülése vagy a gép különösen) elvész.

A kapcsolódó hibák fordulhat elő, a valós életben hello, hanem nem bontható egyes hibák. hello képességét tootest ahhoz, azok fordulhat elő, éles környezetben forgatókönyvek esetén fontos. Még fontosabb van hello képességét toosimulate forgatókönyvekben a termelési számítási feladatokhoz ellenőrzött körülmények között (hello közepén meg kell oldani az összes mérnökök hello nap). Ez sokkal nagyobb mint azt történik meg a hello először 2:00 órakor éles környezetben

### <a name="unified-experience-across-different-environments"></a>Egységes felhasználói élmény különböző környezetek között
hello gyakorlat-ra volt toocreate három különböző felhasználócsoportokhoz lép, hello fejlesztési környezet egy, az egyik tesztek az és egy üzemi. hello modell volt:

1. Hello fejlesztői környezetben, amelyek lehetővé teszik az egyes módszerek egység tesztek Állapotváltások eredményez.
2. Hello tesztkörnyezetben a hibák tooallow végpont vizsgálatok különböző hibát jelentő forgatókönyvek, amelyek eredményez.
3. Tartsa hello éles környezet eredeti tooprevent bármely nem természetes hibákat és azokat, hogy van-e rendkívül gyors emberi válasz toofailure tooensure.

A Service Fabric keresztül hello tartalék elemzési szolgáltatás, azt is javaslat, amely tooturn ez körül, és használjon hello ugyanaz a fejlesztői környezet tooproduction módszert. Nincsenek két módon tooachieve ezt:

1. szabályozott tooinduce hibák, használjon hello tartalék Analysis Service API-k egy beépített környezetből összes hello módon tooproduction fürtök.
2. a fürt egy elleni, a hibákat, használjon hello hiba az Analysis Services toogenerate automatikus indukciós automatikus hibák fordulnak toogive hello. Konfigurációs hibák mértéke ellenőrző hello lehetővé teszi, hogy a hello szolgáltatás ugyanazon toobe másképp tesztelt különböző környezetekben.

A Service Fabric bár hello méretezési hibák hello különböző környezetekben, eltérő lenne a hello tényleges mechanizmusok lenne azonos. Ez lehetővé teszi egy sokkal gyorsabb kód központi telepítési feldolgozási sorban lévő és hello képességét tootest hello szolgáltatások valós terhelés alatt.

## <a name="using-hello-fault-analysis-service"></a>Hiba az Analysis Services hello használata
**C#**

Hiba az Analysis Services szolgáltatások akkor hello System.Fabric névtérben hello Microsoft.ServiceFabric NuGet-csomagot. toouse hello hiba az Analysis Services szolgáltatások, hello nuget-csomagot a projekt hivatkozásként közé tartozik.

**PowerShell**

toouse PowerShell, telepítenie kell a Service Fabric SDK hello. Hello hello ServiceFabric PowerShell SDK telepítése után a modul az Ön toouse betöltött automatikus.

## <a name="next-steps"></a>Következő lépések
toocreate valóban felhőméretű szolgáltatások kritikus tooensure előtt és a központi telepítést követően, hogy a szolgáltatások is képes elviselni a valós életben hibák. Az szolgáltatások hello world ma gyorsan hello képességét tooinnovate, és helyezze a kód tooproduction gyorsan nagyon fontos. Hiba az Analysis Services hello segít a szolgáltatás fejlesztők toodo, amely pontosan.

Az alkalmazások és szolgáltatások segítségével hello beépített tesztelés megkezdése [forgatókönyvek tesztelése](service-fabric-testability-scenarios.md), vagy a saját Tesztelési forgatókönyvek hello használatával szerzői [műveletek fault](service-fabric-testability-actions.md) hello hiba az Analysis Services által biztosított.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
