---
title: "a Machine Learning lineáris regressziós aaaUsing |} Microsoft Docs"
description: "Az Excel programban, és az Azure Machine Learning Studióban lineáris regressziós modell összehasonlítása"
metakeywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 417ae6ab-de4f-4bdd-957a-d96133234656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: kbaroni;garye
ms.openlocfilehash: 8716040ad296053a72fb06c7c9660a186123ac15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-linear-regression-in-azure-machine-learning"></a>Lineáris regresszió használata az Azure Machine Learning termékben
> *Kate Baroni* és *Ben Boatman* vállalati megoldás fejlesztők a Microsoft Data elemzések kiváló Center vannak. Ebben a cikkben leírt tapasztalataikat áttelepítését egy meglévő regressziós elemzés suite tooa felhőalapú megoldás Azure Machine Learning segítségével. 
> 
> 

&nbsp; 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="goal"></a>Cél
A projekt két célok szem előtt tartásával használatába: 

1. A prediktív elemzés tooimprove hello pontosságát a szervezet havi bevétel leképezések használata 
2. Használja az Azure Machine Learning tooconfirm, optimalizálása, sebesség növelése és a skála az eredmények. 

Számos vállalat, például a szervezet végig kell vinnie a havi előrejelzési folyamat bevétel. Az üzleti elemzők kis csoport lett Azure Machine Learning toosupport hello folyamat használata biztosítja, és növelve az előrejelzés pontosságát. hello team töltött több hónappal több forrásból származó adatokat gyűjt, és hello adatok attribútumok fut azonosító kulcsattribútum vonatkozó tooservices értékesítési előrejelzések statisztikai elemzése révén. hello tovább lett toobegin prototípusának statisztikai regressziós modell hello adatok az Excelben. Az Excel regressziós modell, amely lett outperforming hello aktuális mezőt és az előrejelzés folyamatok pénzügyi volt néhány héten belül. Ez vált hello alapterv előrejelzés eredménye. 

A Microsoft majd átvette hello tovább lépés toomoving a prediktív elemzés tooAzure Machine Learning toofind hogyan Machine Learning a prediktív teljesítményre javíthatja ki.

## <a name="achieving-predictive-performance-parity"></a>Paritás prediktív teljesítmény elérése érdekében
Az első elsőbbséget gépi tanulási és Excel regressziós modell tooachieve paritása volt. Megadott hello ugyanazokat az adatokat, és ugyanazt a modell betanítására és tesztelésére adatok felosztása hello, akartunk tooachieve prediktív teljesítmény paritása Excel és a gépi tanulás. Eredetileg nem sikerült. hello Excel outperformed modell hello gépi tanulási modell. hello sikertelenség hello alap eszköz beállítása a Machine Learning megértése tooa hiánya miatt. Hello Machine Learning termékért felelős csoport szinkronban után azt szeretné jobban megismerni hello alap beállítása szükséges az adatkészletek szerzett, és hello két modell paritása érhető el. 

### <a name="create-regression-model-in-excel"></a>Regressziós modell létrehozása az Excel programban
Az Excel regressziós hello Excel Analysis ToolPak található hello szabványos lineáris regressziós modellt használja. 

Azt a számított *átlagos abszolút % hiba* és hello modell hello teljesítmény mértékként használni. 3 hónapos tooarrive, egy működő modell Excelben vett igénybe. Jelenleg nincsenek a nagy részét a Machine Learning Studio kísérlet, amely végső soron a hasznos követelményeinek ismertetése a volt hello hello tanulás.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Hasonló kísérlet létrehozása az Azure gépi tanulás
A kísérletben a Machine Learning Studióban azt követte ezeket a lépéseket toocreate: 

1. A csv-fájl tooMachine Learning Studio (nagyon kis fájl), a feltöltött hello adatkészlet
2. Új kísérlet létrehozott és használt hello [Select Columns in Dataset] [ select-columns] modul tooselect hello használható Excel ugyanazt az adatokkal kapcsolatos funkciókkal 
3. Használt hello [Split Data] [ split] modul (a *relatív kifejezés* mód) toodivide hello adatok hello azonos tanítási adathalmazt, mivel az Excel programban 
4. A hello kikísérletezett [lineáris regressziós] [ linear-regression] modulban (alapértelmezett beállításai csak), dokumentált és hello eredmények tooour Excel regressziós modell képest

### <a name="review-initial-results"></a>Tekintse át a kezdeti eredmények
Először hello Excel modell egyértelműen outperformed hello Machine Learning Studio-modellek: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Teljesítmény | | |
| <ul style="list-style-type: none;"><li>R-négyzet igazítva</li></ul> |0.96 |N/A |
| <ul style="list-style-type: none;"><li>Variációs <br />Meghatározása</li></ul> |N/A |0.78<br />(kis pontosság) |
| Mean Absolute Error |$9. 5M |$ 19.4 M |
| Mean Absolute Error (%) |6.03% |12.2% |

Amikor miatt a folyamat, és az eredmények hello fejlesztők és adatszakértőkön hello Machine Learning csapat, néhány hasznos tipp gyorsan kapott. 

* Hello használata esetén [lineáris regressziós] [ linear-regression] a Machine Learning Studióban modul, a két módszer találhatók:
  * Online színátmenetes módszeren: Több alkalmasak lehetnek a nagyobb méretű problémák
  * A szokványos legkevésbé négyzetes: Esetében sokan úgy tekintenek a amikor lineáris regressziós nélkül hello módszert. Kisebb adatkészletek esetében a szokásos legkevésbé négyzetes lehet egy több optimális választás.
* Vegye figyelembe, hogy tökéletesítse hello L2 exportügyletek súly paraméter tooimprove teljesítményét. Alapértelmezés szerint too0.001 van állítva, de a kis adatkészletnél hivatott azt too0.005 tooimprove teljesítményét. 

### <a name="mystery-solved"></a>Orvosolhatók titokzatos!
Hello javaslatok alkalmazásakor azt érhető el, az Excel alkalmazással azonos alapteljesítményének a Machine Learning Studióban hello: 

|  | Excel | Studio (kezdeti) | Legkevesebb négyzetes keresztüli rendelkező Studio |
| --- |:---:|:---:|:---:|
| Címkézett érték |Tényleges (numerikus) |Azonos |Azonos |
| Tanuló |Excel -> adatok elemzési regressziós -> |Lineáris regressziós. |Lineáris regressziós |
| Tanuló beállítások |N/A |Alapértelmezés szerint használt érték |a szokványos legkevésbé négyzetes<br />2. SZINTŰ 0,005 = |
| Adatkészlet |26 sorok, 3 funkciókat, 1 címke. Az összes numerikus. |Azonos |Azonos |
| Vegyes: vonat |Excel a hello betanítása először 18 sorok, utolsó 8 sorok hello vizsgálni. |Azonos |Azonos |
| Vegyes: teszt |Excel-alkalmazott regressziós képlet toohello utolsó 8 sorok |Azonos |Azonos |
| **Teljesítmény** | | | |
| R-négyzet igazítva |0.96 |N/A | |
| Együttható |N/A |0.78 |0.952049 |
| Mean Absolute Error |$9. 5M |$ 19.4 M |$9. 5M |
| Mean Absolute Error (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

Ezenkívül hello Excel együttható képest jól toohello szolgáltatás súlyok a hello Azure betanított modell:

|  | Excel együttható | Az Azure-funkciót súlyozás |
| --- |:---:|:---:|
| INTERCEPT/eltérés |19470209.88 |19328500 |
| A szolgáltatása |0.832653063 |0.834156 |
| B szolgáltatás |11071967.08 |11007300 |
| A szolgáltatás C |25383318.09 |25140800 |

## <a name="next-steps"></a>Következő lépések
Akartunk tooconsume hello Machine Learning webszolgáltatásba Excel belül. Az üzleti elemzők támaszkodhat a Microsoft Excel és szükséges egy módon toocall hello Machine Learning webszolgáltatás az Excel adatok egy sort, és annak vissza hello érték tooExcel előre jelezni. 

Is meg akartunk toooptimize tekinthetők, hello-beállítások és a Machine Learning Studióban elérhető algoritmussal.

### <a name="integration-with-excel"></a>Integráció az Excel használatával
A megoldás a Machine Learning regressziós modell hozzon létre egy webszolgáltatás-bővítmény a betanított modell hello toooperationalize volt. Néhány percen belül hello webszolgáltatás hozta létre, és ezért sikertelen nevezik közvetlenül az Excel tooreturn bevétel előre jelzett érték. 

Hello *Web Services irányítópult* rész tartalmaz egy letölthető Excel-munkafüzet. hello munkafüzet hello webszolgáltatások API és a séma adatait a beágyazott előre formázott tartalmaz. Amikor rákattint *töltse le az Excel-munkafüzet*, hello munkafüzet nyílik meg, és mentse azt tooyour helyi számítógépen. 

![][1]

Nyissa meg a hello munkafüzeten másolja az előre definiált paraméterek kék hello paraméter szakasz alább látható módon. Miután hello paraméterek kerülnek, Excel meghívja a Machine Learning webszolgáltatásba toohello ki, és hello előre jelzett pontozott címkék megjelenik a zöld hello előre jelzett értékek szakasz. hello munkafüzet továbbra is az összes sort a megadott paraméterek alapján a betanított modell alapján paraméterek toocreate előrejelzéseket. Hogyan toouse ezt a beállítást, a további információkért lásd: [fel az Azure Machine Learning webszolgáltatásba az Excelből](machine-learning-consuming-from-excel.md). 

![][2]

### <a name="optimization-and-further-experiments"></a>Optimalizálás, és további kísérletek
Most, hogy az alapterv volt az Excel-modellt, helyeztük előre toooptimize a gépi tanulási lineáris regressziós modellt. Hello modul használtuk [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] tooimprove a kezdeti adatok elemeket, és azt a kiválasztását, a teljesítmény fokozása 4.6 % elérése segített Mean Absolute Error. A későbbi projektek a szolgáltatás, amely tudta menteni velünk héttel az adatok attribútumok toofind hello megfelelő halmazát szolgáltatások toouse modellezési iteráció használjuk. 

Ezután tervezzük tooinclude további algoritmusok, például a [bayes-féle] [ bayesian-linear-regression] vagy [súlyozott döntési fák] [ boosted-decision-tree-regression] az a kísérlet toocompare teljesítmény. 

Ha azt szeretné, hogy a regressziós tooexperiment, a megfelelő dataset tootry numerikus attribútumok sok hello energia hatékonyságát regressziós minta adatkészlet. a Machine Learning Studióban hello mintaként használható adathalmazt részeként hello dataset valósul meg. Használhatja a tanulási modulok toopredict számos melegítés betöltése vagy hűtési betöltése. az alábbi hello diagram, különböző regressziós teljesítmény összehasonlítása megtanulja elleni hello energiahatékonyság dataset becslése a hello cél változó hűtési betöltése: 

| Modell | Mean Absolute Error | Legfelső szintű közepét négyzet hiba | Relative Absolute Error | Relatív négyzet hiba | Együttható |
| --- | --- | --- | --- | --- | --- |
| Súlyozott döntési fája |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Lineáris regressziós (átmenetes módszeren) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Neurális hálózat regressziós |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Lineáris regressziós (szokásos legkevésbé négyzetes) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Kulcs Takeaways
Azt tanult sokkal által Excel fut regresszióhoz, és az Azure Machine Learning kísérleteket párhuzamosan. Hello létrehozása modell Excel és a Machine Learning segítségével toomodels össze [lineáris regressziós] [ linear-regression] segített, ismerje meg az Azure Machine Learning, és azt felderített lehetőségek tooimprove adatok Kijelölés és modell teljesítményét. 

Azt is, hogy a rendszer ajánlott toouse található [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] tooaccelerate jövőbeli előrejelzés projektek. Szolgáltatás kiválasztása tooyour adatok alkalmazásával létrehozhat egy továbbfejlesztett modell Machine Learning jobb összesített teljesítményt. 

hello képességét tootransfer hello prediktív elemzési a Machine Learning tooExcel elviselhető előrejelzés lehetővé teszi, hogy jelentősen növelheti hello képességét toosuccessfully találatokat adjon vissza tooa széleskörű üzleti felhasználói célközönség. 

## <a name="resources"></a>Erőforrások
Íme néhány forrás, a regressziós együttműködve segít: 

* Az Excel programban regressziós. Ha soha nem próbálta regressziós az Excel programban, ez az oktatóanyag segítségével egyszerűen: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Az előrejelzés regressziós vs. Tyler Chessman megírt a blog cikke elmagyarázza, hogyan toodo time series előrejelzési az Excel programban, amely lineáris regressziós helyes kezdő leírása tartalmazza. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts) 
* A szokványos legkevésbé négyzetes lineáris regressziós: Hibái, problémák és nehézségek. Bevezetés és regressziós értékelése: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

