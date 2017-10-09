---
title: az Azure Security Center aaaRespond toosecurity incidensek |} Microsoft Docs
description: "Ez a dokumentum azt ismerteti, hogyan toouse az Azure Security Center az incidensekre adott reakciók forgatókönyvhöz."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 8af12f1c-4dce-4212-8ac4-170d4313492d
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: aaf50c0c7e774d03d517c3fd11686dbae48dd29b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-security-center-for-an-incident-response"></a>Az Azure Security Center használata incidensmegoldásra
Számos szervezet megtudhatja, hogyan toorespond toosecurity incidensek után csak a támadás érte. tooreduce költségeket, és károsíthatják, akkor fontos toohave az incidensekre adott reakciók helyen megtervezéséről, mielőtt a támadás akkor történik meg. Az Azure Security Center az incidensmegoldás több szakaszában is alkalmazható.

## <a name="incident-response-planning"></a>Incidensmegoldás-tervezés
Egy hatékony tervnek három alapvető képességek függ: képes tooprotect alatt, észlelésében és kezelésében toothreats. Adatvédelem feladata megakadályozza az incidensek, észlelési fenyegetések korai azonosításával kapcsolatos, pedig válasz kapcsolatos hello támadó kizárásának, valamint a rendszerek toomitigate hello hatások megsértették visszaállítása.

Ez a cikk fogja használni a hello biztonsági incidensekre adott reakciók szakaszaiban a hello [Microsoft Azure biztonsági válasz hello felhő](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) cikk, ahogy az ábra a következő hello:

![Az incidensmegoldás életciklusa](./media/security-center-incident-response/security-center-incident-response-fig1.png)

A Security Center hello hibakeresés, felmérési és diagnosztizálás szakaszában is használhatja. Az alábbiakban példákat hogyan Security Center hasznos lehet szakaszában hello három kezdeti incidensválasz:

* **Észleli**: tekintse át a hello első arra utal, hogy az esemény a vizsgálat.
  * Példa: felülvizsgálati hello ellenőrzés, hogy a magas prioritású biztonsági riasztás előfordult-e a Security Center irányítópultjának hello.
* **Értékeléséhez**: hajtsa végre a hello kezdeti assessment tooobtain hello gyanús tevékenységek további információt.
  * Példa: hello biztonsági riasztás további információhoz juthat.
* **Diagnosztika**: technikai vizsgálat lefolytatása, és az elkülönítési, elhárítási és áthidaló stratégiák azonosítása.
  * Példa: hajtsa végre a Security Center által ismertetett biztonsági riasztás hello javítási lépéseket.

hello a következő forgatókönyv bemutatja, hogyan tooleverage Security Center során hello hibakeresés, felmérési és diagnosztizálás/válaszoljon lépésben egy biztonsági incidens. A Security Centerben egy [biztonsági incidens](security-center-incident.md) az adott erőforráshoz tartozó összes olyan riasztás együttese, amelyek egy [támadási folyamatba](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) illeszthetők. Incidensek megjelennek a hello [biztonsági riasztások](security-center-managing-and-responding-alerts.md) csempe és panelen. Incidens felfedi hello kapcsolódó riasztások listája, amely lehetővé teszi, hogy tooobtain további információ a minden egyes előfordulásakor. A Security Center biztonsági riasztások, amely akkor is le egy gyanús tevékenységet használt tootrack is megjelenít önálló.

## <a name="scenario"></a>Forgatókönyv
Contoso át a helyszíni erőforrások tooAzure, beleértve az egyes virtuálisgép-alapú üzleti munkaterhelések és az SQL-adatbázisok némelyike. A Contoso fő számítógépes biztonsági incidensmegoldó csapata, a Core Computer Security Incident Response Team (CSIRT) jelenleg nem tudja kivizsgálni a biztonsági problémákat, mivel a biztonsági intelligencia nem képezi jelenlegi incidensmegoldási eszközparkjuk szerves részét. Integráció hiánya probléma vezet be, hello hibakeresés fázis (túl sok téves) során, és a hello felmérési és diagnosztika szakaszban. Ez az áttelepítés részeként tooopt lezárását a Security Center toohelp az azokat a probléma megoldása.

előkészítve hello első fázisa az áttelepítés befejezése után azok összes erőforrást és összes hello biztonsági Security Center javaslatait. Contoso CSIRT a számítógép biztonsági incidensek kezeléséhez hello központi eleme. hello team áll minden biztonsági incidensek kezeléséhez felelő másokkal. hello csoport tagjainak egyértelműen definiált feladatokat tooensure, amely nincs válasz terület marad nem sikerült felfedezni.

Ebben a forgatókönyvben a hello célra fogjuk toofocus hello szerepkörökhöz, a következő személyeknek Contoso CSIRT részét képező hello:

![Az incidensmegoldás életciklusa](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judit biztonsági üzemeltetéssel foglalkozik. Feladatköre:

* Figyelés és toosecurity fenyegetések hello óra körül válaszol.
* Növekvő toohello felhőbeli számítási feladatok felelőse vagy biztonsági elemző igény szerint.

Sándor biztonsági elemző, és feladatai a következők:

* A támadások kivizsgálása.
* A riasztások javítása.
* A munkaterhelések tulajdonosai toodetermine dolgozik, és alkalmazza azok mérséklési.

Ahogy látja, kezdetét és Sam rendelkezik különböző feladatokat, és azok kell működnek együtt a tooshare Security Center információkat.

## <a name="recommended-solution"></a>Javasolt megoldás
Kis- és Sam különböző szerepkörökkel rendelkeznek, mivel azokat használni a Security Center tooobtain vonatkozó információkat a különböző területek a napi tevékenységek. Judit a **Biztonsági riasztásokat** használja majd napi figyelési tevékenysége során.

![Biztonsági riasztások](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Kis biztonsági riasztások közbeni hibakeresés hello és felmérési fázisból áll. Kezdetét hello kezdeti assessment befejezése után ugyanúgy lehet, hogy eszkalálása a hello probléma tooSam, ha további vizsgálat szükséges. Ezen a ponton Sam hello kapott információk Security Center által néha más adatforrások együtt toomove toohello diagnosztika szakaszban fogja használni.

## <a name="how-tooimplement-this-solution"></a>Hogyan tooimplement ehhez a megoldáshoz
toosee hogyan egy incidensválasz-forgatókönyvben használhatja az Azure Security Center, azt fogja meg a kis lépésekkel hello észlelése és felmérési fázisból áll, és olvassa el Sam funkciója toodiagnose hello probléma.

### <a name="detect-and-assess-incident-response-stages"></a>Az észlelési és a felmérési incidensmegoldási szakaszok
Kis toohello bejelentkezve az Azure portálon, és működik-e a hello Security Center konzolján. A napi tevékenységek megfigyelése részeként ő lépések elvégzésével riasztások hello lépések magas prioritású biztonsági áttekintése:

1. Kattintson a hello **biztonsági riasztások** csempe- és hozzáférés hello **biztonsági riasztások** panelen.
    ![Biztonsági riasztások panel](./media/security-center-incident-response/security-center-incident-response-fig4.png)

   > [!NOTE]
   > Ebben a forgatókönyvben hello célját kezdetét esetén folyamatos tooperform hello rosszindulatú SQL figyelmeztetés, értékelését hello megelőző. ábrán látható módon.
   >
   >
2. Hello kattintson **rosszindulatú SQL tevékenység** riasztást, és tekintse át a hello hello megtámadott erőforrások **rosszindulatú SQL tevékenység** panel: ![incidens részletei](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    Ezen a panelen kezdetét is készítsen feljegyzéseket vonatkozó hello megtámadott erőforrások, hogyan sokszor a támadás történt, és ha észlelhető.
3. Kattintson a hello **megtámadott erőforrások** tooobtain a támadás további információt.

Hello leírás elolvasása, után kezdetét meggyőződése, hogy ez nem vakriasztás és, hogy ő kell eszkalálása a nagybetűk tooSam.

### <a name="diagnose-incident-response-stage"></a>Az incidensmegoldás diagnosztizálási szakasza
SAM hello eset kap kezdetét, és elindítja a hello javítási lépéseket, amelyek a Security Center javasolt áttekintése.

![Az incidensmegoldás életciklusa](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>További források
hello incidensválasz-csapat is kihasználhatják a hello [Security Center Power BI](security-center-powerbi.md) funkció toosee különféle jelentéseket. Ezek a jelentések segítséget nyújthatnak a támogatási során további vizsgálat toovisualize, elemzése és javaslatok és biztonsági riasztások szűrése. A vállalatok számára, amelyek a biztonsági információk és az esemény (SIEM) megoldás hello vizsgálat során, ők is [Security Center integrálása a megoldással](security-center-integrating-alerts-with-log-integration.md). Integrálhatja Azure vizsgálati naplók és a virtuális gép (VM) biztonsági események hello segítségével [Azure naplóelemzés integrációs eszköz](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). tooinvestigate támadás, ezeket az információkat használhatja, amely a Security Center hello adatokkal együtt.

## <a name="conclusion"></a>Összegzés
Egy csoport összeállításakor esemény bekövetkezése előtt nagyon fontos tooyour szervezet, és pozitív befolyásolják, hogyan történik az incidensek kezelése. Hello megfelelő eszközökkel toomonitor erőforrásaira segíthet a csapat tootake pontos lépéseket tooremediate egy biztonsági incidens. A Security Center [az észlelési képességek](security-center-detection-capabilities.md) is segítséget nyújt az informatikai tooquickly válaszoljon toosecurity incidensek és a biztonsági problémák megoldásához.
