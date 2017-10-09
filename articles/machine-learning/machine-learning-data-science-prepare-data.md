---
title: "aaaClean és készítse elő az adatokat az Azure Machine Learning |} Microsoft Docs"
description: "Előre folyamat, illetve a tiszta adatok tooprepare azt a machine Learning szolgáltatáshoz."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: bdf659ec-4881-4324-8b9c-747cbfa0c3cd
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 3e3c3e4b0cfb9187f5820d7165e6ee1ea013ba02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tasks-tooprepare-data-for-enhanced-machine-learning"></a>Továbbfejlesztett gépi tanulási feladatok tooprepare adatait
Előzetesen feldolgozni, és az adatok tisztítása, amelyek általában kell elvégzése előtt a DataSet adatkészlet nem használható hatékonyan gépi tanulás fontos feladatokat. Nyers adatok gyakran zajos vagy nem megbízható, és előfordulhat, hogy értékek hiányzik. Ilyen adatok használata a modellezési félrevezető eredményeket hozhat létre. Ezek a feladatok hello Team adatok tudományos folyamat (TDSP) részét képezik, és általában kövesse a használt adatkészlet toodiscover és a terv hello előzetes feldolgozás szükséges egy kezdeti feltárása. Részletes utasítások hello TDSP folyamathoz, lásd: hello leírt hello lépéseket [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Előzetesen feldolgozni, és a tisztítási feladatok, például hello adatok feltárása feladat, elvégzési környezetekben, például az SQL vagy a Hive vagy az Azure Machine Learning Studio és a különböző eszközök és nyelven, például az R vagy Python adatai függően számos tárolja, és hogyan van formázva. Mivel TDSP iteratív ideiglenesek, ezek a feladatok történhet: hello munkafolyamat hello folyamat több lépést.

Ez a cikk be különböző adatfeldolgozási fogalmakat és feladatokat, amelyek előtt vagy után az Azure Machine Learning adatok bevitele végezhető.

Az adatok feltárása és az Azure Machine Learning studio belül történik előzetes feldolgozás példáért lásd: hello [előzetesen feldolgozni az adatokat az Azure Machine Learning Studióban](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) videó.

## <a name="why-pre-process-and-clean-data"></a>Miért előre feldolgozzák, és adatait?
Valós adatokat különböző forrásokból gyűjtött, és folyamatokat, és tartalmazhat szabálytalanságok vagy veszélyeztetése hello minőségének hello adatkészlet adatai sérültek. hello jellemző adatok minőségi kérdések merülhetnek fel a következők:

* **Hiányos**: adatok attribútumot, vagy a hiányzó értékeket tartalmazó hiányzik.
* **Zajos**: adatok hibás rekordok vagy kiugró tartalmaz.
* **Inkonzisztens**: adatok ütköző rekordok vagy azok az eltérések tartalmaz.

Minőségi adatok minőségi prediktív modelleket előfeltétele. a kimenő szemétgyűjtési tooavoid "szemétgyűjtési" és az adatminőségi javítása és ezért a teljesítmény modell, a imperatív tooconduct egy adatok állapotfigyelő képernyő toospot, adatok korai állít ki, és adja meg a megfelelő adatok feldolgozása és lépéseket tisztítás hello.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Mik azok a néhány tipikus data állapotfigyelő képernyő alkalmazott?
Hello általános adatok minőségének azt is ellenőrizze, hogy ellenőrzése:

* hello száma **rekordok**.
* hello száma **attribútumok** (vagy **szolgáltatások**).
* hello attribútum **adattípusok** (névleges, sorszám vagy folyamatos).
* hello száma **hiányzó értékek**.
* **-Kód szabályosságának** hello adatok.
  * Ha hello adatok TSV vagy a fürt megosztott kötetei szolgáltatás, ellenőrizze, hogy hello oszlop elválasztóinak és a sor elválasztók megfelelően mindig önálló oszlopok és sorok.
  * Ha hello adatok HTML vagy XML formátumú, ellenőrizze, hogy hello adatok helyes formátumú-e alapján a megfelelő előírásokról.
  * Elemzés is szükség lehet a strukturált tooextract rendelésinformációkat félig strukturált vagy strukturálatlan adatok.
* **Inkonzisztens rekordok**. Ellenőrizze, hogy hello értéktartományhoz engedélyezettek. például ha hello adatai student GPA, ellenőrizze, hogy a kijelölt tartomány, hello van hello GPA tegyük fel például 0 ~ 4.

Ha megtalálta az adatokat, problémái **lépések feldolgozása** van szükség, amelyek gyakran magában foglalja a hiányzó értékeket, adatok normalizálási, diszkretizálási, szöveges feldolgozási tooremove tisztítás és/vagy beágyazott karakterek, ami hatással lehet a adatok igazításának vegyes adattípusok közös mezők és mások számára.

**Az Azure Machine Learning formátumú táblázatos adatokat felhasználva**.  Ha hello adatok már táblázatos formában, előtti adatfeldolgozási közvetlenül az Azure Machine Learning a Machine Learning Studio hello hajtható végre.  Ha adatok nem táblázatos formában, az XML-elemzés szóbeli szükség lehet ahhoz tooconvert hello adatok tootabular képernyőn.  

## <a name="what-are-some-of-hello-major-tasks-in-data-pre-processing"></a>Mik azok a hello fő feladatokat az adatok előzetes feldolgozás?
* **Adatok tisztítása**: Töltse ki vagy hiányzó értékek észlelése és zajos adatok és kiugró.
* **Adatok átalakítása**: az tooreduce dimenziókat és a zaj optimalizálására.
* **Adatok csökkentési**: rekordok vagy könnyebben adatkezeléssel attribútumait.
* **Adatok diszkretizálási**: Convert folyamatos attribútumok, a használat megkönnyítése érdekében toocategorical attribútumok bizonyos machine learning metódusával.
* **Szöveg tisztítás**: távolítsa el a beágyazott karaktereket, így előfordulhat, hogy az adatok hibás illesztés hibákat, például tabulátorral tagolt adatfájl, a beágyazott lapjaira Embedded új sort, amelyben a megszakadhat a rekordok stb.

hello az alábbi részek adatfeldolgozási lépés néhány.

## <a name="how-toodeal-with-missing-values"></a>Hogyan toodeal a hiányzó értékeket?
Hiányzó értékekkel toodeal, célszerű toofirst hello OK azonosítása, a hello hiányzó értékei toobetter leíró hello probléma. Hiányzó érték kezelése tipikus megoldások a következők:

* **Törlés**: hiányzó értéket tartalmazó rekordok eltávolítása
* **Üres helyettesítő**: hiányzó értékek cserélje le egy üres értéket: például azt, *ismeretlen* kategorikus vagy 0 numerikus értékeket.
* **Helyettesítés jelenti**: Ha hello hiányzó adatok numerikus, hello hiányzó értékek cseréje hello közepét.
* **Gyakran használják a helyettesítés**: Ha hello hiányzó adatok kategorikus, hello hiányzó értékek cseréje hello leggyakoribb elem
* **Regressziós helyettesítés**: hiányzó regressziós metódus tooreplace közleményében szerepelt értékekkel értékeket használja.  

## <a name="how-toonormalize-data"></a>Hogyan toonormalize adatokat?
A megadott tartomány adatok normalizálási újra méretezi számértékeket tooa. Népszerű adatok normalizálási módszerek a következők:

* **Minimális-maximális normalizálási**: lineárisan hello tooa adattartomány átalakító, például 0 és 1 közötti adott hello minimális értéke méretezett too0 és a maximális érték too1.
* **Z-pontszám normalizálási**: adatok és szórásnál alapuló méretezési: hello adatok és hello közepét hello különbségének nullával hello szórás.
* **Decimális skálázás**: bővítse a hello adatok áthelyezése hello tizedesvessző hello attribútum-érték.  

## <a name="how-toodiscretize-data"></a>Hogyan toodiscretize adatokat?
Adatok folyamatos értékek toonominal attribútumot, vagy az intervallumok átalakításával is diszkretizálható. Bizonyos értelemben az ezzel a következők:

* **A Dobozolás egyenlő szélességű**: hello értéktartományhoz minden lehetséges egy attribútum felosztani hello azonos méretezés és rendelhet hozzá hello eső értékeket egy van hello bin számú N csoportját.
* **A Dobozolás egyenlő magasságú**: hello tartomány osztani lehetséges értékek N csoportokba attribútum, minden egyes tartalmazó hello példányok száma azonos, majd hozzárendelése hello eső értékeket egy van a hello bin számát.  

## <a name="how-tooreduce-data"></a>Hogyan tooreduce adatokat?
Nincsenek könnyebb adatkezelési különböző módszerek tooreduce adatok mérete. Attól függően, hogy az adatok méretét és hello tartomány a következő módszerek hello alkalmazhatók:

* **Jegyezze fel a mintavétel**: hello rekordok Sample, és csak a hello adatok hello reprezentatív részhalmazát választhat.
* **Mintavételi attribútum**: hello legfontosabb attribútumok csak egy részét jelölje ki a hello adatokból.  
* **Összesítési**: hello adatok felosztani a csoportok és az egyes csoportok hello számok tárolja. Például hello egy étterem lánc keresztül hello elmúlt 20 évben számok lehetnek napi bevétel összesítve toomonthly bevétel tooreduce hello hello adatok méretét.  

## <a name="how-tooclean-text-data"></a>Hogyan tooclean szöveg adatokat?
**Táblázatos adatok szövegmezők** oszlopok igazítás és/vagy rekord határok befolyásoló karaktereket tartalmazhatnak. Például lapok ágyazva egy tabulátorral tagolt fájl OK oszlop hibás illesztés hibákat, és a beágyazott új sor karaktereket törés rekord sorokat. Nem megfelelő szövegkódolás szöveg írás/olvasás közben kezelési tooinformation adatvesztés, véletlen bevezetése olvashatatlan karaktereket, például null értékeket, és előfordulhat, hogy is befolyásolják szöveg elemzése vezet. Gondos elemzése és -szerkesztő rendelés tooclean szöveg mezőiben megfelelő igazítás és/vagy szöveges strukturálatlan és félig strukturált adatok tooextract strukturált adatok szükség lehet.

**Az adatok feltárása** egy korai képet kaphat hello adatokat biztosít. Ezzel a lépéssel adatproblémákat számos fedetlen lehet, és a megfelelő módszereket is lehet alkalmazott tooaddress ismertetünk.  Fontos tooask kaphat például mi hello hello probléma forrását, és hogyan hello probléma lehet, hogy be. Azt is lehetővé teszi eldöntheti, hogy a hello adatfeldolgozási lépéseket, hogy szükség toobe tooresolve venni őket. egy százalékát hello adatokból tooderive insights hello típusú is lehet használt tooprioritize hello adatfeldolgozási beavatkozást.

## <a name="references"></a>Referencia
> *Adatbányászat: Elvekről és technikákról*, harmadik Edition Morgan Kaufmann 2011 Jiawei Han, Micheline Kamber és Jian Pei
> 
> 

