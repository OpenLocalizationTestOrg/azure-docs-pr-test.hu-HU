---
title: "Machine Learning Anomáliadetektálási észlelési API aaaAzure |} Microsoft Docs"
description: "Az anomáliadetektálási észlelési API látható egy példa adatsorozat időadatok egységesen elosztásban időben numerikus értéket tartalmazó rendellenességeket észleli a Microsoft Azure Machine Learning-val készült."
services: machine-learning
documentationcenter: 
author: alokkirpal
manager: jhubbard
editor: cgronlun
ms.assetid: 52fafe1f-e93d-47df-a8ac-9a9a53b60824
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/05/2017
ms.author: alok;rotimpe
ms.openlocfilehash: ce153689b8ddb36b67a2ad3607d846ea83ebcf61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-anomaly-detection-api"></a>Számítógép-tanulási Anomáliadetektálás API
## <a name="overview"></a>Áttekintés
[Az anomáliadetektálási észlelési API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) adatsorozat időadatok egységesen elosztásban időben numerikus értéket tartalmazó rendellenességeket észlelő Azure Machine Learning-val készült példa.

Ez az API a következő típusú adatsorozat időadatok rendellenes minták hello képes észlelni:

* **Pozitív és negatív trendek**: például, ha egy növekvő tendenciát számítástechnikai memóriahasználat figyelése lehet egyik fontos lehet, a memóriavesztés,
* **Hello dinamikus értékek tartományán változásai**: például egy felhőalapú szolgáltatás hello kivételek figyelésekor hello dinamikus értékek tartományán módosításai utalhat instabillá válásának hello állapotának hello szolgáltatást, és
* **Napra és esik**: például figyelésekor hello szolgáltatás a sikertelen bejelentkezések száma vagy egy elektronikus kereskedelmi webhely, a kivételek száma igényeiben jelentkező vagy immerzióban utalhat rendellenes viselkedés.

A machine learning érzékelők ilyen értékek változásainak követése keresztül idő és a jelentés folyamatban lévő változásai szűkebb értékeik anomáliadetektálási pontszámait mint. Ad hoc küszöbérték hangolása nincs szükségük, és eredményeiket használt toocontrol téves pozitív sebessége. hello anomáliadetektálás API akkor hasznos, több forgatókönyv szerint, például a szolgáltatás a figyelés nyomon követése a KPI-k idővel használati metrikák például keresések, számok kattintással, a figyelt teljesítményfigyelés például a memória, Processzor, fájl számlálóin keresztül olvassa be, idővel stb.

hello Anomáliadetektálás ajánlat tartalmaz hasznos eszközök tooget-t elindította.

* Hello [webes alkalmazás](http://anomalydetection-aml.azurewebsites.net/) segítségével értékelje ki és az anomáliadetektálás API-k az adatokon hello eredményeinek képi megjelenítése.

> [!NOTE]
> Próbálja **informatikai Anomáliadetektálási Insights-megoldást** technológiával [az API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)
> 
> tooget a záró tooend megoldás telepített Azure-előfizetés tooyour <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **az első lépések >**</a>
> 
>

## <a name="api-deployment"></a>API-telepítés
Rendelés toouse hello API telepítenie kell azt tooyour Azure-előfizetés ahol tárolható egy Azure Machine Learning webszolgáltatásként.  Ehhez a hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2).  Ezzel telepít két AzureML-webszolgáltatások (és a kapcsolódó erőforrások) tooyour Azure-előfizetés - egy anomáliadetektálás a szezonalitás értékének észlelési és szezonalitás értékének észlelési nélkül.  Hello központi telepítés befejezése után fog tudni toomanage az API-kat hello [AzureML webszolgáltatások](https://services.azureml.net/webservices/) lap.  Ezen a lapon akkor lesz kell tudni toofind a végpontok helyére, API-kulcsokat, valamint példakód az hello API felület meghívásakor.  Részletes útmutatás érhető el [Itt](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice).

## <a name="scaling-hello-api"></a>Méretezési hello API
Alapértelmezés szerint a központi telepítés lesz egy ingyenes fejlesztési és tesztelési célú számlázási tervet, amely 1000 tranzakciók/és 2 számítási órák/hónap.  Tooanother terv frissítheti az igényeinek megfelelően.  Hello különböző tervek árakkal kapcsolatos információk [Itt](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) "Éles webes API-k árképzési" alatt.

## <a name="managing-aml-plans"></a>Tervek AML kezelése 
Kezelheti a számlázási csomag [Itt](https://services.azureml.net/plans/).  hello neve hello erőforrásnév hello API telepítésekor választott, valamint egy karakterláncot, amelyet egyedi tooyour előfizetés alapján.  Hogyan tooupgrade a terv érhetők el az utasításokat [Itt](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice) a hello "Számlázási tervek kezelése" című szakaszban.

## <a name="api-definition"></a>API-definíció
hello webszolgáltatás biztosít egy REST-alapú API-t, amely képes használni a különböző módokon, például a web- vagy mobilalkalmazás R, Python, Excel, HTTPS-KAPCSOLATON keresztül stb.  A sorozat adatok toothis időszolgáltatás REST API-híváson keresztül küldi el, és fusson kombinációjából hello anomáliadetektálási háromféle az alábbiakban.

## <a name="calling-hello-api"></a>Hello API felület meghívásakor
A sorrend toocall hello API szüksége lesz a tooknow hello végponthelyét és API-kulcsot.  Mindkét esetben, mintakód hello API hívásakor együtt érhetők el a hello [AzureML webszolgáltatások](https://services.azureml.net/webservices/) lap.  Keresse meg a szükséges toohello API, és kattintson a hello "Felhasználás" lapon toofind őket.  Vegye figyelembe, hogy a Swagger API, hello API meghívása (azaz paraméterrel hello URL-cím `format=swagger`) vagy mint egy nem - Swagger API-t (azaz nélkül hello `format` URL-paramétert).  hello mintakód hello Swagger formátumot használja.  Az alábbiakban egy példa egy kérelem-válasz a a Swagger formátumban van.  Ezek többek között az toohello szezonalitás értékének végpont.  hello nem szezonalitás értékének végpont hasonlít.

### <a name="sample-request-body"></a>A minta kérelem törzsében
hello kérelem tartalmazza a két objektum: `Inputs` és `GlobalParameters`.  Hello az alábbi példa kérelem, az egyes paraméterek küldött explicit módon nem vannak (görgessen lefelé a végpontok paraméterek teljes listáját).  Paraméterek hello kérelem nem küldött explicit módon az alábbi hello alapértelmezett értékeket fogja használni.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Mintaválasz
Vegye figyelembe a következőket, sorrendben toosee hello `ColumnNames` mezőben meg kell adni `details=true` a kérés URL-cím paraméterként.  Tekintse meg az alábbi táblázatokban hello hello jelentése egyes mezők mögött.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Pontszám API
hello pontszám API anomáliadetektálás futó nem határozza idő adatsor szolgál. hello API hello adatokon anomáliadetektálási érzékelők számos fut, és visszaadja az anomáliadetektálási eredményeiket. az alábbi ábra hello pontszám API képes észlelni, hogy hello rendellenességeket példáját mutatja be. Ez a time series 2 különböző megváltozik, és 3 igényeiben jelentkező rendelkezik. hello piros pont, mely hello szintű észlelt, miközben fekete hello pontokból megjelenítése észlelt hello igényeiben jelentkező hello idő megjelenítése.
![Pontszám API][1]

### <a name="detectors"></a>Érzékelők
hello anomáliadetektálás API támogatja az érzékelők 3 kategóriába sorolhatók. Meghatározott bemeneti paraméterek és minden egyes érzékelő kimeneti hello a következő táblázatban találhatók.

| Érzékelő kategória | Érzékelő | Leírás | A bemeneti paraméterek | kimenetek |
| --- | --- | --- | --- | --- |
| Csúcs érzékelők |TSpike érzékelő |Észleli a teljesítményt és immerzióban szélen hello első és harmadik quartiles értékei alapján |*tspikedetector.Sensitivity:* egész értéket hello tartomány 1 – 10., alapértelmezett vesz: 3; Így így kevésbé érzékeny további rendkívüli értékek fog dolgozza fel a magasabb értékkel |TSpike: bináris értékek – "1", ha a rendszer észlelt egy csúcs/dip, egyébként pedig "0" |
| Csúcs érzékelők | ZSpike érzékelő |Észleli a teljesítményt és milyen távolságban hello esetén alapján immerzióban tartoznak, amely a középérték |*zspikedetector.Sensitivity:* érvénybe hello közé egész számot 1 – 10., alapértelmezett: 3; Így kevésbé érzékeny további rendkívüli értékek fog dolgozza fel a magasabb értékkel |ZSpike: bináris értékek – "1", ha a rendszer észlelt egy csúcs/dip, egyébként pedig "0" | |
| Lassú Trend érzékelő |Lassú Trend érzékelő |Lassú pozitív trend hello beállítása érzékenységi szerinti észlelése |*trenddetector.Sensitivity:* érzékelő pontszám küszöbértékét (alapértelmezett: 3,25, 3,25 – 5 egy ésszerű tartomány tooselect ezt a; a kisebb érzékeny magasabb hello hello) |tscore: a trend anomáliadetektálási pontszám jelentő lebegőpontos szám |
| Szint módosításának érzékelők | Kétirányú szint módosítása érzékelő |Hello beállítása érzékenységi szerint felfelé és lefelé szint is módosítás észlelése |*bileveldetector.Sensitivity:* érzékelő pontszám küszöbértékét (alapértelmezett: 3,25, 3,25 – 5 egy ésszerű tartomány tooselect ezt a; a kisebb érzékeny magasabb hello hello) |rpscore: jelentő anomáliadetektálási pontszám felfelé és lefelé szint módosítása a lebegőpontos szám | |

### <a name="parameters"></a>Paraméterek
Részletesebb információ ezen bemeneti paraméterek hello az alábbi táblázatban szerepel:

| A bemeneti paraméterek | Leírás | Alapértelmezett beállítás | Típus | Érvényes értékek | Javasolt tartomány |
| --- | --- | --- | --- | --- | --- |
| detectors.historyWindow |Anomáliadetektálási pontszám számításhoz használt előzményeket (az adatpontok száma) |500 |egész szám |10-2000 |Idősorozat függő |
| detectors.spikesdips | E toodetect csak napra, csak immerzióban vagy mindkettő |Mindkét |számbavétele |Mindkét, teljesítményt, immerzióban |Mindkét |
| bileveldetector.Sensitivity |Érzékenységi kétirányú szint módosítása érzékelő. |3.25 |Dupla |None |(A kisebb értékek jelenti érzékenyebb) 3,25 5 |
| trenddetector.Sensitivity |Érzékenysége a pozitív trend érzékelő. |3.25 |Dupla |None |(A kisebb értékek jelenti érzékenyebb) 3,25 5 |
| tspikedetector.Sensitivity |Érzékenysége a TSpike érzékelő |3 |egész szám |1-10 |3-5 (a kisebb értékek jelenti érzékenyebb) |
| zspikedetector.Sensitivity |Érzékenysége a ZSpike érzékelő |3 |egész szám |1-10 |3-5 (a kisebb értékek jelenti érzékenyebb) |
| postprocess.tailRows |Hello legújabb adatmennyiség hello kimeneti eredmények tartott toobe mutat |0 |egész szám |a 0 (tartani minden adatpontok), vagy adja meg a pontok tookeep számát eredmények |N/A |

### <a name="output"></a>Kimenet
hello API az az idő adatsorok összes érzékelők fut, és anomáliadetektálási pontszámokat és az egyes bináris csúcs mutatók időt adja vissza. az alábbi táblázat hello hello API kimeneteinek sorolja fel. 

| kimenetek | Leírás |
| --- | --- |
| Time |A nyers adatokat, vagy összesített (és/vagy) imputált adatok időbélyegeket Ha összesítési (és/vagy) hiányzik az adatok imputálási vonatkozik |
| Adatok |Nyers vagy összesített (és/vagy) imputált adatok közötti értéket, ha összesítő (és/vagy) hiányzó adatok imputálási vonatkozik |
| TSpike |Bináris jelölő tooindicate e észlelhető egy csúcs TSpike érzékelő |
| ZSpike |Bináris jelölő tooindicate e észlelhető egy csúcs ZSpike érzékelő |
| rpscore |Egy lebegőpontos szám képviselő anomáliadetektálási pontozása a kétirányú szint módosítása |
| rpalert |1 vagy 0 érték azt jelzi, kétirányú szintű hello bemeneti érzékenysége alapján anomáliadetektálási módosítása |
| tscore |Egy lebegőpontos szám képviselő anomáliadetektálási pontozása a pozitív trend |
| talert |1 vagy 0 értéket, amely jelzi, hogy az a pozitív trend anomáliadetektálási hello bemeneti érzékenysége alapján |

## <a name="scorewithseasonality-api"></a>ScoreWithSeasonality API
hello ScoreWithSeasonality API anomáliadetektálás futó határozza minták rendelkező a time series szolgál. Ez az API határozza minták hasznos toodetect eltérések.  
hello alábbi ábrán például határozza idősor észlelhető rendellenességeket. hello a time series egy csúcs (hello 1-jétől fekete pont), (2. fekete pont hello és egy hello végén) két immerzióban és egy szint módosítása (piros pont) rendelkezik. Ne feledje, hogy mindkét hello hello középső hello idősorozat dip hello szint módosításának követően a rendszer csak discernable határozza összetevők hello adatsorozat el lesznek távolítva.
![Szezonalitás értékének API][2]

### <a name="detectors"></a>Érzékelők
hello érzékelők hello szezonalitás értékének végpont olyan hasonló toohello néhányat a meglévők közül, hello nem szezonalitás értékének végpont, de a (lenti) kis mértékben eltérő paraméternevei.

### <a name="parameters"></a>Paraméterek

Részletesebb információ ezen bemeneti paraméterek hello az alábbi táblázatban szerepel:

| A bemeneti paraméterek | Leírás | Alapértelmezett beállítás | Típus | Érvényes értékek | Javasolt tartomány |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Az aggregációs időköznek másodpercben összesítéséhez szükséges tartományt adjon meg a time series |0 (nincs összesítési történik) |egész szám |0: ellenkező esetben hagyja ki az összesítés, amely > 0 |5 perc too1 nap, idősorozat függő |
| preprocess.aggregationFunc |A megadott AggregationInterval hello az adatok összesítéséhez szükséges tartományt függvény |témakörök |számbavétele |átlagos, sum, hossza |N/A |
| preprocess.replaceMissing |Hiányzó adatok tooimpute értéket fogja használni |LKV (utolsó ismert érték) |számbavétele |nulla, lkv, középérték |N/A |
| detectors.historyWindow |Anomáliadetektálási pontszám számításhoz használt előzményeket (az adatpontok száma) |500 |egész szám |10-2000 |Idősorozat függő |
| detectors.spikesdips | E toodetect csak napra, csak immerzióban vagy mindkettő |Mindkét |számbavétele |Mindkét, teljesítményt, immerzióban |Mindkét |
| bileveldetector.Sensitivity |Érzékenységi kétirányú szint módosítása érzékelő. |3.25 |Dupla |None |(A kisebb értékek jelenti érzékenyebb) 3,25 5 |
| postrenddetector.Sensitivity |Érzékenysége a pozitív trend érzékelő. |3.25 |Dupla |None |(A kisebb értékek jelenti érzékenyebb) 3,25 5 |
| negtrenddetector.Sensitivity |A negatív trend érzékelő érzékenységi. |3.25 |Dupla |None |(A kisebb értékek jelenti érzékenyebb) 3,25 5 |
| tspikedetector.Sensitivity |Érzékenysége a TSpike érzékelő |3 |egész szám |1-10 |3-5 (a kisebb értékek jelenti érzékenyebb) |
| zspikedetector.Sensitivity |Érzékenysége a ZSpike érzékelő |3 |egész szám |1-10 |3-5 (a kisebb értékek jelenti érzékenyebb) |
| seasonality.enable |E szezonalitás értékének elemzés toobe történik |Igaz |Logikai érték |IGAZ, hamis |Idősorozat függő |
| seasonality.numSeasonality |Rendszeres ciklusok toobe észlelt maximális száma |1 |egész szám |1, 2 |1-2 |
| seasonality.Transform |E határozza (és) anomáliadetektálás alkalmazása előtt el kell távolítani a trend összetevők |deseason |számbavétele |nincs, deseason, deseasontrend |N/A |
| postprocess.tailRows |Hello legújabb adatmennyiség hello kimeneti eredmények tartott toobe mutat |0 |egész szám |a 0 (tartani minden adatpontok), vagy adja meg a pontok tookeep számát eredmények |N/A |

### <a name="output"></a>Kimenet
hello API az az idő adatsorok összes érzékelők fut, és anomáliadetektálási pontszámokat és az egyes bináris csúcs mutatók időt adja vissza. az alábbi táblázat hello hello API kimeneteinek sorolja fel. 

| kimenetek | Leírás |
| --- | --- |
| Time |A nyers adatokat, vagy összesített (és/vagy) imputált adatok időbélyegeket Ha összesítési (és/vagy) hiányzik az adatok imputálási vonatkozik |
| OriginalData |Nyers vagy összesített (és/vagy) imputált adatok közötti értéket, ha összesítő (és/vagy) hiányzó adatok imputálási vonatkozik |
| ProcessedData |Hello alábbiak valamelyikét: <ul><li>A time series szezonálisan módosul, ha jelentős szezonalitás értékének már telepítve van, és deseason beállításnak;</li><li>szezonálisan módosul, és ha jelentős szezonalitás értékének már telepítve van a time series és deseasontrend választógomb detrended</li><li>Ellenkező esetben a rendszer hello ugyanaz, mint a OriginalData</li> |
| TSpike |Bináris jelölő tooindicate e észlelhető egy csúcs TSpike érzékelő |
| ZSpike |Bináris jelölő tooindicate e észlelhető egy csúcs ZSpike érzékelő |
| BiLevelChangeScore |Egy lebegőpontos szám képviselő anomáliadetektálási pontozása a szint módosítása |
| BiLevelChangeAlert |1 vagy 0 értéket, amely jelzi, hogy egy szint módosításának anomáliadetektálási hello bemeneti érzékenysége alapján |
| PosTrendScore |Egy lebegőpontos szám képviselő anomáliadetektálási pontozása a pozitív trend |
| PosTrendAlert |1 vagy 0 értéket, amely jelzi, hogy az a pozitív trend anomáliadetektálási hello bemeneti érzékenysége alapján |
| NegTrendScore |Egy lebegőpontos szám képviselő anomáliadetektálási pontozása a negatív trend |
| NegTrendAlert |1 vagy 0 értéket, amely jelzi, hogy egy negatív trend anomáliadetektálási hello bemeneti érzékenysége alapján |

[1]: ./media/machine-learning-apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection-api/anomaly-detection-seasonal.png

