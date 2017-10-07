---
title: "aaaLoad az alkalmazás tesztelése a Visual Studio Team Services használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toostress tesztelése a Visual Studio Team Services segítségével az Azure Service Fabric-alkalmazások."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: fc743585-0d1b-483f-981d-493f4552ac07
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 663cf8db5e8f0a4d0d7f27b585645d7f776392f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Betöltési az alkalmazás tesztelése a Visual Studio Team Services használatával
Ez a cikk bemutatja, hogyan toouse Microsoft Visual Studio terhelés teszt szolgáltatások toostress egy alkalmazás teszteléséhez. Az Azure Service Fabric állapotalapú szolgáltatási háttér és egy állapotmentes szolgáltatások előtér-webkiszolgáló használ. hello itt használt mintaalkalmazás egy repülőgép hely szimulátor. Megadja, hogy egy repülőgép Azonosítót, az indulási idő és a cél. hello alkalmazás háttérben dolgozza fel a hello kérelmeket, és hello előtér jeleníti meg a térkép hello repülőgép hello feltételeknek megfelelő.

hello következő diagram azt ábrázolja, hogy tesztelni kell hello Service Fabric-alkalmazás.

![Hello repülőgép hely mintaalkalmazás ábrája][0]

## <a name="prerequisites"></a>Előfeltételek
Az első lépések előtt toodo hello következőkre lesz szüksége:

* Hozzon létre egy Visual Studio Team Services-fiókot. Kaphat egy ingyenes [Visual Studio Team Services](https://www.visualstudio.com).
* Szerezze be, és telepítse a Visual Studio 2013 vagy Visual Studio 2015-öt. Ez a cikk a Visual Studio 2015 Enterprise edition használ, de a Visual Studio 2013 és a többi kiadás hasonlóan működnek.
* Az alkalmazás tooa átmeneti környezet telepítése. Lásd: [hogyan toodeploy alkalmazások tooa távoli fürtön, a Visual Studio használatával](service-fabric-publish-app-remote-cluster.md) információkat arról.
* Az alkalmazás használati szokások megismerése. Ezekkel az információkkal már használt toosimulate hello terhelés mintát.
* Ismerje meg, a betöltés tesztelés hello célja. Ezzel a megoldással értelmezése és elemzése hello – teszteredmények betöltése.

## <a name="create-and-run-hello-web-performance-and-load-test-project"></a>Hozzon létre és futtasson hello webalkalmazás teljesítmény- és terheléstesztet projekt
### <a name="create-a-web-performance-and-load-test-project"></a>Webalkalmazás teljesítmény- és terheléstesztet-projekt létrehozása
1. Nyissa meg a Visual Studio 2015-öt. Válasszon **fájl** > **új** > **projekt** tooopen hello hello menüsoron **új projekt** párbeszédpanel megnyitásához.
2. Bontsa ki a hello **Visual C#** csomópont válassza **teszt** > **webalkalmazás teljesítmény- és terheléstesztet projekt**. Nevezze el hello projektet, és válassza a hello **OK** gombra.

    ![Képernyőfelvétel a hello új projekt párbeszédpanel][1]

    Új webalkalmazás teljesítmény- és terheléstesztet projektben a Megoldáskezelőre kell megjelennie.

    ![Képernyőfelvétel a Megoldáskezelőben hello új projekt][2]

### <a name="record-a-web-performance-test"></a>Jegyezze fel a webtesztet teljesítmény
1. Nyissa meg hello .webtest projekt.
2. Válassza ki a hello **hozzáadása rögzítése** ikon toostart a rögzítési munkamenet a böngészőben.

    ![Képernyőfelvétel a hello adja hozzá a rögzítés ikonjára a böngészőben][3]

    ![Képernyőfelvétel a hello rekord gombra a böngészőben][4]
3. Keresse meg a Service Fabric-alkalmazás toohello. hello rögzítése panelen megjelennek az hello webes kérelmek.

    ![Képernyőfelvétel a hello panel rögzítése a webes kérések][5]
4. Hajtsa végre egy műveletsorozatot hello felhasználók tooperform várható. Ezek a műveletek egy minta toogenerate hello terheléselosztási használatosak.
5. Amikor elkészült, válassza ki a hello **leállítása** gomb toostop rögzítése.

    ![Képernyőfelvétel a hello Leállítás gombra.][6]

    hello .webtest projektre a Visual Studio több kérelem kell rögzített. Dinamikus paraméterek automatikusan lecseréli a rendszer. Ezen a ponton a felesleges, ismétlődő függőségi kéréseit, amelyek nem szerepelnek a tesztkörnyezet törlése.
6. Mentse hello projektet, és válassza a hello **teszteléséhez futtassa** parancs toorun hello webes teljesítmény helyi tesztelése, és győződjön meg arról, hogy minden megfelelően működik-e.

    ![Képernyőfelvétel a hello teszt futtatása parancsra][7]

### <a name="parameterize-hello-web-performance-test"></a>Hello teljesítmény webteszt parametrizálja
Hello teljesítmény webteszt is parametrizálja konvertálás kódolt tooa webalkalmazás teljesítmény-teszt, és majd a hello kód szerkesztésével. Alternatív megoldásként köthető hello webes teljesítmény teszt tooa adatok listája, hogy hello teszt telepítéseket hello adatokat. Lásd: [létrehozás és Futtatás kódolt teljesítmény webteszt](https://msdn.microsoft.com/library/ms182552.aspx) hogyan tooconvert hello webes teljesítmény teszt tooa kódolt teszt vonatkozó további információért. Lásd: [hozzáadása egy adatforrás tooa webes teljesítmény tesztje](https://msdn.microsoft.com/library/ms243142.aspx) hogyan toobind adatok tooa webalkalmazás teljesítmény információt tesztelése.

Ehhez a példához nem lesz átalakítás hello teljesítmény teszt kódolt tooa webteszt, így hello repülőgép Azonosítót cserélje le a előállított GUID Azonosítóhoz, és vegye fel a további kérelmeket toosend járatok toodifferent helyeket.

### <a name="create-a-load-test-project"></a>Betöltési teszt projekt létrehozása
Egy teszt a projekt betöltésekor hello webteszt teljesítmény és egységteszt mellett további megadott betöltési vizsgálati beállítások szerint egy vagy több forgatókönyv áll. lépések hello bemutatják, hogyan toocreate egy terheléselosztási tesztelése projekt:

1. A webalkalmazás teljesítmény- és terheléstesztet projekt hello helyi menüjében válassza **Hozzáadás** > **terheléstesztet**. A hello **terhelés tesztelése** varázsló, válassza ki a hello **következő** tooconfigure hello beállításainak tesztelése gombra.
2. A hello **betöltése mintát** területen válassza ki, hogy állandó felhasználói terhelést vagy egy lépés terhelés, néhány felhasználó kezdődő, és hogy növekszik adott idő alatt hello a felhasználók.

    Ha a helyes becsült hello összeg felhasználói terhelés, és szeretné, hogy hogyan hello aktuális rendszer hajt végre toosee, válassza ki a **állandó betöltése**. Ha a cél toolearn e hello rendszer következetesen hajt végre a különböző terhelések, válasszon **lépés terhelés**.
3. A hello **tesztelése vegyes** területen válassza ki a hello **Hozzáadás** gombra, majd jelölje be hello tesztelje, hogy szeretné-e a hello terheléstesztet tooinclude. Használhatja a hello **terjesztési** oszlop toospecify hello százalékos teljes vizsgálatok futtatása az egyes teszteket.
4. A hello **futtatása beállítások** területén adja meg a hello terhelés vizsgálat időtartama.

   > [!NOTE]
   > Hello **tesztelése ismétlési** beállítás érhető el csak egy terheléstesztet helyileg a Visual Studio használatával futtatásakor.
   >
   >
5. Hello a **hely** szakasza **futtatása beállítások**, adjon meg hello helyet, ahol teszt terhelésigényét jönnek létre. hello varázsló kérheti toolog a tooyour Team Services-fiók. Jelentkezzen be, és válassza a földrajzi helyet. Amikor elkészült, válassza ki a hello **Befejezés** gombra.
6. Hello terheléstesztet a létrehozása után nyissa meg a hello .loadtest projekt, és válassza a beállítást, például futtassa hello aktuális **futtatása beállítások** > **Settings1 futtatása [aktív]**. Megnyílik a Futtatás hello-beállítások a hello **tulajdonságok** ablak.
7. A hello **eredmények** hello szakasza **futtatása beállítások** tulajdonságai ablakban, hello **időzítés részleteit tárolási** beállítást kell **nincs** , az alapértelmezett értékre. Módosítsa ezt az értéket túl**minden egyes részletek** tooget hello – teszteredmények betöltése további tájékoztatást. Lásd: [betöltése tesztelés](https://www.visualstudio.com/load-testing.aspx) hogyan tooconnect tooVisual Studio Team Services, és futtassa a terhelés tesztelése olvashat.

### <a name="run-hello-load-test-by-using-visual-studio-team-services"></a>Hello terheléstesztet a Visual Studio Team Services használatával futtassa
Válassza ki a hello **terhelés teszteléséhez futtassa** parancs toostart hello teszt futtatásakor.

![Képernyőfelvétel a hello terheléstesztet futtatása parancs][8]

## <a name="view-and-analyze-hello-load-test-results"></a>Megtekintése és elemzése hello – teszteredmények betöltése
Mint hello betöltése teszt történik, és hello teljesítményadatok van grafikon. A következő diagram valami hasonló toohello kell megjelennie.

![Képernyőfelvétel a teljesítmény ábra a teszteredmények betöltése][9]

1. Válassza ki a hello **töltse le a jelentés** hivatkozás hello lap hello tetején. Miután hello jelentés tölti le, válassza ki azt a hello **jelentés megtekintéséhez** gombra.

    A hello **Graph** lapon megtekintheti a különböző teljesítményszámlálókkal diagramjait. A hello **összegzés** lap hello a teljes teszt eredményei jelennek meg. Hello **táblák** lapon láthatók hello átadott és sikertelen terhelés tesztek teljes száma.
2. Válassza ki a hello számú mutató hivatkozások hello **teszt** > **sikertelen** és hello **hibák** > **száma** oszlopok toosee hiba részletes adatait.

    Hello **részletes** lap információkat jeleníti meg virtuális felhasználói és tesztelési forgatókönyvhöz a sikertelen kérelmek. Ezek az adatok akkor lehet hasznos, ha hello terheléstesztet több forgatókönyv tartalmazza.

Lásd: [elemzése betöltése tesztelése eredményez hello hello tesztelése Analyzer betöltése diagramjait ábrázolása](https://www.visualstudio.com/load-testing.aspx) terhelés megjelenítéséről további információ a vizsgálati eredmények.

## <a name="automate-your-load-test"></a>A terheléstesztet automatizálása
A Visual Studio Team Services betöltése teszt API-k toohelp terhelés tesztek kezelése és Team Services-fiók eredményeket elemezni biztosít. Lásd: [felhő tesztelés betöltése az Rest API-k](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) további információt.

## <a name="next-steps"></a>Következő lépések
* [Figyelése és diagnosztizálása szolgáltatásai a helyi számítógép fejlesztési beállítása](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
