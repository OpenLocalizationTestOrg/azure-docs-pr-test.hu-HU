---
title: "aaaPredictive karbantartási megoldás előre konfigurált |} Microsoft Docs"
description: "Leírását hello Azure IoT Suite prediktív karbantartási megoldás előre konfigurált."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 2d09801467d33db6b7d6333fa071aea2bf573f20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>A prediktív karbantartási előre konfigurált megoldás áttekintése

Hello *prediktív karbantartási* [előre konfigurált megoldás] [ lnk_preconfigured_solutions] hello egyike [Microsoft Azure IoT Suite] [ lnk_iot_suite] előre konfigurált megoldások. Ez a megoldás a valós idejű eszköztelemetria-gyűjtést az [Azure Machine Learning][lnk-machine-learning] használatával létrehozott prediktív modellel integrálja.

Azure IoT Suite gyorsan és könnyen tooand figyelő eszközök csatlakozni, és valós idejű irányítópultokat és képi megjelenítéseket telemetriai adatok elemzéséhez. A prediktív karbantartási megoldás hello hello irányítópultok és a képi megjelenítések biztosít, amely a meghajtó hatékonyság és javítása érdekében a bevétel adatfolyamok új eszközintelligencia.

## <a name="hello-scenario"></a>hello forgatókönyv

A Fabrikam egy regionális légitársaság, amely a nagyszerű ügyfélélményre összpontosít versenyképes árakon. A járatok késésének egyik okai a karbantartási problémák, és a repülőmotorok karbantartása különösen nagy kihívást jelent. Fabrikam kerülni kell motor hiba felé továbbított folyamán minden áron, hogy annak rendszeresen megvizsgálja az ütemezi a karbantartási terv tooa szerint. Azonban motorok nem mindig viselniük repülőgép hello azonos. Időnként feleslegesen végeznek karbantartást a motorokon. Még fontosabb, hogy olyan problémák merülnek fel, amelyek miatt a repülő nem szállhat fel a karbantartásig. Ha repülőgép egy helyen ahol hello jobb technikusok vagy tartalék részei nem érhetők el, ezek a problémák különösen költséges lehet.

Fabrikam repülőgép hello motorok az érzékelők motor feltételek figyelő felé továbbított folyamán vannak tagolva. Fabrikam hello prediktív karbantartási megoldás toocollect hello érzékelő során gyűjtött adatok hello felhőszolgáltató közötti átviteléhez használja. Halmozódó év-kezelő motor működési és adatai, után Fabrikam adatszakértőkön egy módon toopredict hello fennmaradó élettartama (Szabályainak) repülőgép motor van modellezve. hello modellje négy hello motor érzékelők adatait és motor elhasználódását, amely visszavezet tooeventual hiba közötti kapcsolat. Fabrikam tooperform rendszeres vizsgálatokat tooensure biztonsági folytatódik, amíg azt már a hello modellek toocompute hello Szabályainak minden motor után minden felhőszolgáltató közötti átviteléhez. hello modellje hello motorok hello repülési során gyűjtött hello telemetriáját. A Fabrikam így előre jelezheti a jövőbeli meghibásodási pontokat, és megtervezheti a karbantartást és a javítást.

> [!NOTE]
> hello megoldás modellje motor tényleges elhasználódását adatokat.

Előrejelzésére hello pontot, ha karbantartási szükség, amelyet a Fabrikam optimalizálhatja műveletek tooreduce költségeit.

A karbantartási koordinátorok és a menetrendek készítői együttműködve elvégzik a következőket:

- Egy adott helyen leállítása repülőgép toocoincide karbantartási terv.
- Gondoskodjon arról, hogy elegendő idő áll rendelkezésre hello repülőgép toobe nem működik az ütemezés megszakítása nélkül.
- tooschedule technikusok tooensure, hogy repülőgép a terjesztéskezelő hatékonyan várakozási idő nélkül.

A készletgazdálkodási vezetők karbantartási terveket kapnak, hogy optimalizálhassák a rendelési folyamatokat és a pótalkatrészek készletét.

Ezek a tevékenységek Fabrikam toominimize repülőgép ground idő engedélyezése, és a működési költségek csökkentése során a hello utasok és a személyzet biztonságát.

toounderstand hogyan [Azure IoT Suite] [ lnk_iot_suite] biztosít hello képességek kell toorealize hello lehetőségeket kínál a prediktív karbantartási, tekintse át a [infographic] [lnk_infographic].

## <a name="how-hello-predictive-maintenance-solution-is-built"></a>Hogyan hello prediktív karbantartási megoldás épül.

hello megoldást használja egy meglévő Azure Machine Learning modell egy sablon tooshow ezeket a képességeket IoT Suite szolgáltatás segítségével gyűjtött telemetriát dolgozik. A Microsoft létrehozta a [regressziós modell] [ lnk_regression_model] nyilvánosan elérhető adatok alapján repülőgép motor<sup>\[1\]</sup>, és lépésről lépésre Hogyan toouse hello modell útmutatást.

hello Azure IoT prediktív karbantartási megoldás a sablon alapján létrehozott hello regressziós modellt használja. hello modell központilag telepítik az Azure-előfizetéshez, és elérhetővé tett egy automatikusan létrehozott API-n keresztül. hello megoldás tesztelési 4 (a teljes 100) képviselő adatokat hello egy részét tartalmazza motorok és hello 4 (az összesen 21) érzékelő adatfolyamot. Ezek az adatok megfelelő tooprovide hello betanított modell egy pontos eredménye.

*\[1\] A. Saxena és K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set”, NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## <a name="get-started-with-predictive-maintenance"></a>Ismerkedés a prediktív karbantartással

Az oktatóanyag bemutatja, hogyan tooprovision hello prediktív karbantartási megoldás. Azt is bemutatja, hogyan hello prediktív karbantartási megoldás hello alapvető funkcióit. Ezek a szolgáltatások számos hello megoldás irányítópultja együtt előre konfigurált hello megoldás központi telepítését végző keresztül érheti el.

toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.

> [!NOTE]
> Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk_free_trial].

1. Jelentkezzen be túl[azureiotsuite.com] [ lnk-azureiotsuite] az Azure használatával fiók hitelesítő adatait, és kattintson a  **+**  toocreate megoldást.
1. Kattintson a **válasszon** hello **prediktív karbantartási** csempére.
1. Adja meg a **Megoldásnevet** az előre konfigurált prediktív karbantartási megoldáshoz.
1. Jelölje be hello **régió** és **előfizetés** kívánt toouse tooprovision hello megoldás.
1. Kattintson a **megoldás létrehozása** toobegin hello létesítésének folyamatát kell használnia. Ez a folyamat általában több percet toorun időt vesz igénybe.

### <a name="wait-for-hello-provisioning-process-toocomplete"></a>Várjon, amíg a kiépítési folyamat toocomplete hello

1. Kattintson a megoldás a hello csempe **kiépítési** állapotát.
1. Értesítés hello **állapotok kiépítés** , Azure-szolgáltatások vannak telepítve az Azure-előfizetéshez.
1. Miután kiépítése befejeződött, hello állapotmódosítások túl**készen**.
1. A megoldás hello jobb oldali ablaktáblában hello csempe toosee hello Részletek gombra. Ebben az ablaktáblában indítja el a hello megoldás-irányítópult és az access hello Machine Learning munkaterülettel.

> [!NOTE]
> Ha hibát tapasztal előre konfigurált hello megoldás telepítésének, tekintse át a [hello azureiotsuite.com hely engedélyeinek] [ lnk-permissions] és hello [gyakran ismételt kérdések] [ lnk-faq]. Ha hello problémák továbbra is fennáll, hozzon létre szolgáltatásjegyet a hello [portal][lnk-portal].

Vannak-e részletek toosee teheti meg, amelyek nem jelennek meg a megoldáshoz? A [felhasználói visszajelzési webhelyen](https://feedback.azure.com/forums/321918-azure-iot) elküldheti a szolgáltatásokkal kapcsolatos javaslatait.

## <a name="view-hello-solution"></a>Hello megoldás megtekintése

Ez a szakasz végigvezeti hello megoldás felhasználói felületén.

### <a name="predictive-maintenance-dashboard"></a>Prediktív karbantartási irányítópult

Ez a lap hello webalkalmazásban használ a Power bi JavaScript vezérlők (lásd: hello [Power bi-látványelemek tárház][lnk-powerbi]) toovisualize:

* a blob Storage tárolóban hello Stream Analytics-feladatok hello kimeneti adatait.
* hello Szabályainak és ciklus (Event) számának repülőgép motor.

### <a name="observing-hello-behavior-of-hello-cloud-solution"></a>Hello viselkedését betartásával hello felhőalapú megoldás

A hello Azure-portálon, keresse meg a toohello erőforráscsoport hello megoldás nevű úgy döntött, hogy tooview a kiosztott erőforrásokat.

![][img-resource-group]

Ha előre konfigurált hello megoldás, kap e-mailben található hivatkozásra toohello Machine Learning munkaterülettel. Is megtalálhatja a hello toohello Machine Learning-munkaterület [azureiotsuite.com] [ lnk-azureiotsuite] lap kiosztott megoldást. Egy csempe esetén érhető el ezen a lapon hello megoldás a hello **készen** állapotát.

![][img-machine-learning]

Hello megoldás portálon láthatja, hogy hello minta ki van építve négy szimulált eszköz toorepresent két repülőgép / repülőgép, egyenként négy érzékelők két motorral. Amikor először toohello megoldás portal, hello szimuláció le van állítva.

![][img-simulation-stopped]

Kattintson a **indítsa el a szimuláció** toobegin hello szimulálása. hello érzékelő előzmények, Szabályainak, ciklusokat és Szabályainak előzmények hello irányítópult feltöltéséhez.

![][img-simulation-running]

Ha Szabályainak kisebb, mint 160 (egy tetszőleges bemutatásra szolgál a kiválasztott küszöbérték), a hello megoldás portál egy figyelmeztetés szimbólum következő toohello Szabályainak megjelenítése jeleníti meg. hello megoldás portálon is hello repülőgép motor sárga mutatja be. Figyelje meg, hogyan hello Szabályainak értékek rendelkezik teljes általános csökkenő tendenciát, de az egyes toobounce felfelé és lefelé. Ez a viselkedés hello különböző ciklus hosszak és hello modell pontosságát az eredménye.

![][img-simulation-warning]

hello teljes szimuláció körülbelül 35 perc toocomplete 148 ciklusok vesz igénybe. a hello hello 160 Szabályainak küszöbérték körülbelül 5 percig, először teljesül, és mindkét motorok hello küszöbérték találati körülbelül 8 perccel.

hello szimuláció keresztül hello teljes adatkészlet 148 ciklus fut, és a végső Szabályainak és ciklus értékek rendezi.

Hello szimuláció bármikor leállíthatja, pont, de a gombra kattintva **indítsa el a szimuláció** felhasználásait hello hello dataset hello indítás szimulálása.

## <a name="next-steps"></a>Következő lépések

További információk miként prediktív karbantartási forgatókönyvben olvassa el az Azure IoT toolearn [hello az eszközök internetes hálózatát értéket rögzítése][lnk_capture_value].

Igénybe vehet egy [forgatókönyv] [ lnk-predictive-walkthrough] hello prediktív karbantartási megoldás.

Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]
* [A hello IoT biztonsági szabad][lnk-security-groundup]

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/