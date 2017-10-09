---
title: "aaaPredictive karbantartási forgatókönyv |} Microsoft Docs"
description: "A forgatókönyv hello Azure IoT – prediktív karbantartási megoldás előre konfigurált."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 900d6351019489a8e2f4b98908364e3bd14975c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>A prediktív karbantartási előre konfigurált megoldás bemutatója

hello előre konfigurált prediktív karbantartási egy olyan végpont megoldás, amely képes hello pont, amely a hiba valószínűleg toooccur üzleti forgatókönyv esetén. Ezt az előre konfigurált megoldást proaktív módon használhatja olyan tevékenységekhez, mint a karbantartás optimalizálása. hello megoldás egyesíti fontos Azure IoT Suite-szolgáltatások, például az IoT-központ, a Stream analytics, és egy [Azure Machine Learning] [ lnk-machine-learning] munkaterületen. A munkaterület modellben, egy nyilvános minta adatkészlet toopredict hello fennmaradó élettartama (Szabályainak) repülőgép motor áll. hello megoldás teljes hello IoT üzleti helyzethez megvalósítja az Ön tooplan kiindulási pontként, megvalósítani a saját speciális üzleti igényeinek megfelelő megoldást.

## <a name="logical-architecture"></a>Logikai architektúra

a következő diagram hello hello logikai összetevőinek előre konfigurált hello megoldás ismerteti:

![][img-architecture]

kék hello elemek már az Azure-szolgáltatásokkal létesített hello régióban, amelybe telepítette előre konfigurált hello megoldás. hello előre konfigurált hello megoldás telepíthető régiók listáját jeleníti meg a hello [üzembe helyezési oldal][lnk-azureiotsuite].

zöld hello elem a szimulált eszköz, amely egy repülőgép alrendszeren jelöli. Többet tudhat meg ezeket a következő szakasz hello szimulált eszközökkel kapcsolatos.

hello szürke elemek-összetevőket képviselő megvalósító *Eszközkezelés* képességeit. hello előre konfigurált prediktív karbantartási megoldás jelenlegi kiadásában hello kiépítése ezeket az erőforrásokat. További információ az eszközkezelés toolearn tekintse meg a toohello [távoli felügyeleti előkonfigurált megoldás][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Szimulált eszközök

Előre konfigurált hello a megoldásban a szimulált eszköz repülőgép motor jelöli. hello megoldás tooa egyetlen repülőgép hozzárendelését két motorral lett kiépítve. Minden egyes motor bocsát ki a négy típusú telemetriai: érzékelő 9, érzékelő 11, érzékelő 14 és érzékelő 15 adatokkal hello hello gépi tanulási modell toocalculate hello Szabályainak hello motor szükséges. Minden egyes szimulált eszköz küld hello telemetriai üzenetek tooIoT központ a következő:

*Ciklusszám*. A ciklusok két és tíz óra közti időtartamú befejezett repülőutakat jelölnek. Hello repülési során telemetriai adatokat minden fél óra lesz rögzítve.

*Telemetria*. Négy érzékelő jelzi a motorattribútumokat. hello érzékelők általános tartalma érzékelő 9, érzékelő 11, érzékelő 14 és érzékelő 15. Ezek a négy érzékelők telemetriai elegendő tooobtain eredményeit hasznosítani hello Szabályainak modellből jelölik. előre konfigurált hello megoldásban használt hello modell, amely tartalmazza az érzékelőadatok valós motor nyilvános adatkészlet készült. Hogyan hello modell hello eredeti adatkészlet alapján készült-e további információkért lásd: hello [Cortana Intelligence Gallery prediktív karbantartási sablon][lnk-cortana-analytics].

Szimulált hello eszközöket képes kezelni hello hello megoldásban hello IoT-központ által küldött parancsokat a következő:

| Parancs | Leírás |
| --- | --- |
| StartTelemetry |Vezérlők hello hello szimuláció állapotát.<br/>Telemetriai adatok küldését indítása hello eszköz |
| StopTelemetry |Vezérlők hello hello szimuláció állapotát.<br/>Telemetriai adatok küldése leáll hello eszköz |

Az IoT Hub nyugtázza az eszközparancsokat.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-feladat

**Feladat: Telemetriai** olyan hello bejövő eszköz telemetriai adatfolyam két utasítások segítségével:

* hello először kiválasztása összes telemetriai adat hello eszközökről és az adattárolás tooblob küld. Itt azt formájában jelenik meg hello web app alkalmazásban.
* hello második kiszámítja az átlagos érzékelő keresztül egy kétperces csúszóablak értéket, majd elküldi keresztül hello Event hub tooan **eseményfeldolgozóhoz**.

## <a name="event-processor"></a>Eseményfeldolgozó
Hello **esemény processzor állomás** egy Azure webes feladatot futtat. Hello **eseményfeldolgozóhoz** hello átlagos érzékelő értékek befejezett ciklus szükséges időt. Ezután ezen értékek tooan API, amely elérhetővé teszi a betanított modell toocalculate hello Szabályainak egy adja át. Machine Learning-munkaterület hello megoldás részeként ki van építve hello API tesz elérhetővé.

## <a name="machine-learning"></a>Machine Learning
Gépi tanulás összetevő hello valós repülőgép motorok gyűjtött adatokat származó modellt használ. Hello hello csempe a Machine Learning-munkaterület toohello navigálhat [azureiotsuite.com] [ lnk-azureiotsuite] lap kiosztott megoldást. hello csempe esetén érhető el hello megoldás a hello **készen** állapotát.


## <a name="next-steps"></a>Következő lépések
Most már megtekintett hello legfontosabb összetevők hello előre konfigurált prediktív karbantartási megoldás, érdemes lehet a toocustomize azt. Lásd: [Útmutatás az előre konfigurált megoldások testreszabásáról][lnk-customize].

Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]
* [A hello IoT biztonsági szabad][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/