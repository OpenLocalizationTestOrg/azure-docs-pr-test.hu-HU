---
title: "Mérnöki csapathoz és kiválasztása az Azure Machine Learning szolgáltatás |} Microsoft Docs"
description: "Szolgáltatás kiválasztása és a szolgáltatás műszaki osztály ismerteti, és a gépi tanulás adatok javító folyamatának szerepkörük példákat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ceb524d-842e-4f77-9eae-a18e599442d6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: zhangya;bradsev
ROBOTS: NOINDEX
redirect_url: machine-learning-data-science-create-features
redirect_document_id: TRUE
ms.openlocfilehash: 51a5d8fed492cb9301e048c2b6a721e4573a47d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Jellemzőkiválasztás és -kiemelés az Azure Machine Learningben
Ez a témakör ismerteti a szolgáltatás termékgondozó csoportja és a gépi tanulás az adatok javító folyamatban szolgáltatásválasztást alkalmazásában. Azt mutatja be, hogy mi ezeket a folyamatokat tartalmaz, amely Azure Machine Learning Studio által biztosított példák felhasználásával.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

A gépi tanulás használt betanítási adatok gyakran fejleszthető a kijelölés vagy a nyers adatokat gyűjt a szolgáltatások kibontásával. Példa egy visszafejtett szolgáltatás megtanulása besorolni a képek kézzel karaktereket környezetében: a nyers bit terjesztési adataiból összeállított bit sűrűségű térképre. Ez a térkép segítségével keresse meg a nyers terjesztési hatékonyabban széleit, a karaktereket.

Tervezett és a kiválasztott szolgáltatásokat a hatékonyabbá teheti a képzési folyamat, amely az adatok tartalmazzák a főbb információkat. Is javítják ezek a modellek power pontosan a bemeneti adatok osztályozására szolgáló, és több abroncsnyomásmérők erős előre jelezni az érdeklődési eredményeit. A szolgáltatás termékgondozó csoportja és a kijelölés kombinálhatja is annak a tanulási több számításilag tractable. Igen, javítja, és majd a szolgáltatások szükséges továbbá bármikor kalibrálhatja vagy a modell betanításához számának csökkentése. Matematikailag nyelven, a modell betanításához kijelölt szolgáltatások olyan minimális független változó, amely az adatok kombinációját ismertetik, és majd sikeresen az eredmények előrejelzése.

A mérnöki és funkciók kiválasztása egy egy nagyobb folyamat része, amely általában négy lépésből áll:

* Adatgyűjtés
* Adatok továbbfejlesztése
* Modell létrehozása
* Utófeldolgozási

Mérnöki és kijelölés jött létre az adatok a fejlesztés lépés gépi tanulás. Ez a folyamat három aspektusainak a célokra különböztethetők meg:

* **Az előzetes adatfeldolgozás**: Ez a folyamat megpróbálja győződjön meg arról, hogy az összegyűjtött adatok tiszta és konzisztens legyen. Ez magában foglalja a feladatokat, mint a integrálása több adatkészletek, hiányzó adatok kezelésére, kezelési tranzakciós szempontból inkonzisztens adatokat, és a adattípusokat.
* **Jellemzőkiemelés**: Ez a folyamat megkísérli a meglévő nyers funkciókat az adatok további kapcsolódó szolgáltatások létrehozásához és a tanulási algoritmust előrejelzési teljesítményének növelése érdekében.
* **Szolgáltatás kiválasztása**: Ez a folyamat kiválasztása a kulcs az eredeti adatok funkciók részéhez dimenzióinak, a képzési probléma csökkentése érdekében.

Ez a témakör csak a a szolgáltatás termékgondozó csoportja és a szolgáltatás kiválasztása szempontok az továbbfejlesztése folyamat tartalmazza. Az adatok előzetes feldolgozás lépésben további információkért lásd: [előzetesen feldolgozni az adatokat az Azure Machine Learning Studióban](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).

## <a name="creating-features-from-your-data--feature-engineering"></a>Szolgáltatások létrehozása az adatok--jellemzőkiemelés
A betanítási adatok, amelyek mindegyike rendelkezik (változók vagy az oszlopokban tárolt mezők) funkciókat (rekordok vagy megfigyelések egy sor tárolt), például álló mátrix áll. A funkciók a kísérleti tervezési várhatóan írhatók le az adatok kombinációját. Bár a nyers adatok mezők számos közvetlenül szerepelhet a modell betanításához használandó kijelölt szolgáltatáskészlete, további visszafejtett szolgáltatások gyakran kell alakítható ki a nyers adatokat egy továbbfejlesztett tanítási adathalmazt létrehozása céljából a funkciókat.

Milyen funkciókat javítása érdekében az adathalmaz egy modell betanításakor létre kell hozni? Visszafejtett funkciókat, amelyek javítják a képzés információkkal jobban megkülönbözteti az adatok kombinációját. Az új szolgáltatások további információkkal egyértelműen rögzített nem várt vagy könnyen látható, az eredeti vagy meglévő készlet, de ez a folyamat történt egy kép. Megfelelő és hatékony döntések gyakran megkövetelik a néhány tartomány szakértői.

Azure Machine Learning segítségével indításakor a legegyszerűbb bonyolultnak Ez a folyamat konkrétan a Machine Learning Studióban biztosított minták használatával. Két példa a itt jelenik meg:

* Egy regressziós példa ([kerékpárt bérlését száma előrejelzését](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) egy felügyelt kísérletben, ahol a cél értékek ismertek
* A szöveg-adatbányászati besorolás példa használatával [Szolgáltatáskivonatolás][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>1. példa: Egy regressziós modell historikus szolgáltatások hozzáadására
Bemutatják, hogyan lehet szolgáltatásokat regressziós tevékenység tervezését, most használja a kísérlet "igény szerinti előrejelzéséhez kerékpárt" az Azure Machine Learning Studióban. Ehhez a kísérlethez célja előre jelezni a kerékpárt, ez azt jelenti, hogy annak bérlését belül egy adott hónap, nap, illetve órával száma terhelését. Az adatkészlet **kerékpárt bérleti UCI adatkészlet** szolgál a nyers bemeneti adatként.

Adatkészlet fenntartja az Amerikai Egyesült Államokban Washingtoni kerékpárt bérleti hálózat beruházási Bikeshare vállalattól valós adatokon alapul. Az adathalmaz a számát jelöli az kerékpárt bérlését belül egy adott óra, nap, 2011 2012-re, és 17379 sorok és oszlopok 17 tartalmaz. A nyers szolgáltatáskészlete időjárási feltételek (hőmérséklet, páratartalom, szél sebesség) és a típus a nap (szünnap vagy hét napja) tartalmazza. A mező előre jelezni **cnt**, a szám, amely a kerékpárt bérlését jelöli egy adott órán belül, és, hogy címtartományok 1 977.

Hatékony szolgáltatásai a betanítási adatok összeállításához, négy regressziós modell épülnek, az azonos algoritmussal, de négy különböző képzési adatkészletek. A négy adatkészletek nyers ugyanazon bemeneti adatait, de a szolgáltatások egyre több beállítása. Ezek a funkciók négy kategóriákba vannak csoportosítva:

1. A időjárási + szünnap + hétköznap = + hétvégi szolgáltatások előre jelezhető napi
2. B = kerékpárt, amely minden az előző 12 óra volt bérelt száma
3. C = kerékpárt, amely minden, az azonos órához az elmúlt 12 nap volt bérelt száma
4. D = kerékpárt, amely minden az előző 12 hetes, az azonos órához és ugyanarra a napra esnek volt bérelt száma

A szolgáltatás egy, már szerepel az eredeti nyers adatok, amelyek mellett a másik három különböző szolgáltatások a műszaki osztály folyamat funkción keresztül jönnek létre. A szolgáltatás beállítva B rögzíti a kerékpárt legutóbbi terhelését. A szolgáltatás kerékpárt iránti igény beállítva C rögzíti, egy adott óra. A szolgáltatás adott óra és a hét napját adott D rögzítésekre iránti igények kerékpárt beállítva. Egyes képzési négy adatkészletek szolgáltatáskészletek tartalmaz egy, A + B, A + B + C és A + B + C + D rendre.

Az Azure Machine Learning kísérletben betanítási adatok négy halmazok keresztül adatkészletből Előfeldolgozott bemeneti négy ágak jöttek létre. A bal szélső fiókirodai kivételével minden ilyen ág tartalmaz egy [R-parancsfájl végrehajtása] [ execute-r-script] , amelyben egy származtatott (szolgáltatás B, C és beállítása D) szolgáltatások modul rendre össze, és hozzáfűzi a importált adatkészlet. A következő ábra bemutatja az R-parancsfájl, a második bal fiókiroda B készlet létrehozásához használt.

![Szolgáltatás létrehozása](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

A következő táblázat összefoglalja a teljesítménymérési eredményeket négy modellek összehasonlítása. A legjobb eredmények elérése érdekében A + B + C látható funkcióihoz. Vegye figyelembe, hogy a Hibaarány csökken, amikor további szolgáltatáskészletek a betanítási adatok szerepelnek. Ez ellenőrzi a feltételezhető, hogy a B és C szolgáltatáskészletek biztosítson a regressziós feladat kapcsolatos további információt. A D szolgáltatáskészlete hozzáadása nem tűnik, hogy adja meg a Hibaarány további csökkentését.

![A teljesítményeredmények összehasonlítása](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>2. példa: Szöveg adatbányászati szolgáltatások létrehozása
Szöveg adatbányászati, például a besorolás és a céggel kapcsolatos véleményeket dokumentumelemzés kapcsolatos feladatok széles körben alkalmazása szolgáltatás mérnöki csapathoz. Például szeretné besorolni a dokumentumok számos kategóriába sorolhatók, amikor a tipikus feltételezi, a szavakat vagy kifejezéseket egy dokumentumot a kategóriába tartozik kevésbé valószínű egy másik dokumentum kategóriába. Más szóval a szó vagy kifejezés terjesztési gyakoriságát el tudja írhatók le másik dokumentum kategóriák. A szöveg adatbányászati alkalmazások a szolgáltatás műszaki osztály folyamat a szolgáltatásokat érintő szót vagy kifejezést gyakoriságot, mert egyes adatra szöveges-tartalom általában módon ellenőrizhető, hogy a bemeneti adatok létrehozásához szükséges.

Ez a feladat eléréséhez technika nevű *szolgáltatáskivonatolás* tetszőleges szöveg szolgáltatások hatékonyan ikonná indexek vonatkozik. Ahelyett, hogy egy adott index, a módszer funkciók úgy, hogy alkalmazza a kivonatoló függvényt a szolgáltatásokhoz, és segítségével a kivonati értékek indexek közvetlenül közötti társítás minden szöveges funkciót (szavakat vagy kifejezéseket).

Az Azure Machine Learning van egy [Szolgáltatáskivonatolás] [ feature-hashing] modul által létrehozott szót vagy kifejezést felhasználásokhoz. Az alábbi ábrán ez a modul használatával példáját mutatja be. A bemeneti adatkészlet két oszlopokat tartalmazza: a beállításnak 1 közötti könyv besorolása 5 és a tényleges tartalom áttekintése. Ez a cél [Szolgáltatáskivonatolás] [ feature-hashing] modul az új funkciók, amelyek megjelenítik a megfelelő szavakat vagy kifejezéseket a külön felülvizsgálati belül előfordulási gyakoriságát beolvasása. Ez a modul használatához kövesse az alábbi lépéseket kell:

1. Jelölje ki az oszlopot, amely tartalmazza a beírt szöveg (**Col2** ebben a példában).
2. Állítsa be *bitsize kivonatoláshoz* 8, ami azt jelenti, 2 ^ 8 = 256 szolgáltatások jönnek létre. A szöveg elkezdésekor majd kivonatolja 256 indexet. A paraméter *bitsize kivonatoláshoz* 1 és 31 tartományt. Ha a paraméter értéke nagyobb számot, a szavakat vagy kifejezéseket valószínűleg kevesebb az azonos indexbe tárolni.
3. A paraméternek *N-g* 2. A bemeneti szöveg ez lekéri előfordulási gyakoriságát unigrams (egy mindegyik egyszavas szolgáltatás) és bigrams (egy szolgáltatás minden pár szomszédos szavak). A paraméter *N-g* 10, amely megadja, hogy a szolgáltatás szereplő szekvenciális szavak maximális száma 0-tól tartományait.  

![A szolgáltatás kivonatoló modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Az alábbi ábra bemutatja, hogyan meg az új szolgáltatásokat.

![A szolgáltatás kivonatoló – példa](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Az adatok--szolgáltatás kiválasztása a szűrési funkciók
*Szolgáltatás kiválasztása* képzési adatkészleteket prediktív modellezési feladatokhoz, mint az osztályozási vagy regressziós feladatok létrehozása gyakran alkalmazott folyamat. A cél, hogy az eredeti készlet, amely csökkenti a dimenziók a legszükségesebb funkciókat az eltérés alapján, az adatok maximális száma ábrázolásához használatával válassza ki a funkciók egy részét. A funkciók részhalmazát hozzá lesznek adva a modell betanításához csak szolgáltatásokat tartalmazza. Szolgáltatás kiválasztása a két fő célokra szolgál:

* Szolgáltatás kiválasztása gyakran növeli a besorolás pontossága nem számít, redundáns kiküszöbölése révén, vagy a szolgáltatások szorosan összefügg.
* Szolgáltatás kiválasztása a számos funkciót tartalmaz, így hatékonyabb a modell képzési folyamat csökken. Ez különösen fontos a tanulókkal, amelyek például a támogatási vektoros gépek betanítása drága.

Szolgáltatás kiválasztása a modell betanításához használandó adatkészlet szolgáltatásainak számának csökkentése érdekében kér, de azt nem általában hivatkozik kifejezés *granularitása csökkentése érdekében.* A szolgáltatás kiválasztási módszerek őket módosítása nélkül bontsa ki az adatokat az eredeti funkciók egy részéhez.  Granularitása csökkentési módszereket tervezni a visszafejtett funkciókra átalakíthatja az eredeti funkciók és így módosíthatja azokat. Granularitása csökkentési módszerek például egyszerű összetevő elemzés, kanonikus korrelációs elemzés és egyes érték felbontás ellen.

Szolgáltatás kiválasztása módszerek olyan felügyelt környezetben egy széles körben alkalmazott kategória szűrő-alapú szolgáltatás kiválasztása. Értékelése az egyes szolgáltatások és a target attribútummal közötti összefüggést, ezek a módszerek pontszámot hozzárendelése az egyes szolgáltatások statisztikai mérték vonatkoznak. A szolgáltatások által a pontszám, amelyek megőrzésével, vagy egy adott funkcióhoz megszüntetésével küszöbértéke beállításához használhatja majd sorrendje. Ezek a módszerek használt statisztikai intézkedések például Pearson korrelációs kölcsönös információkat és a khi-négyzet vizsgálat.

Az Azure Machine Learning Studio modulok biztosít szolgáltatás kiválasztása. A következő ábrán látható, tartalmaz-e ezek a modulok [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] és [Fisher lineáris Discriminant elemzés] [ fisher-linear-discriminant-analysis].

![Szolgáltatás kiválasztása – példa](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)

Tegyük fel például, a [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] modulját és a korábban leírt szöveg adatbányászati példában. Tegyük fel, hogy szeretné-e egy regressziós modell létrehozása 256 funkciókat keresztül létrehozása után a [Szolgáltatáskivonatolás] [ feature-hashing] modul, és hogy a válasz változó **Col1** és jelöli egy könyv felülvizsgálati minősítés beállításnak 1 és 5. Állítsa be **pontozási metódus funkció** való **Pearson korrelációs**, **céloszlop** való **Col1**, és **száma szükséges szolgáltatások** való **50**. A modul [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] majd létrejön egy adatokat tartalmazó együtt a target attribútummal 50 szolgáltatások **Col1**. Az alábbi ábra szemlélteti a ehhez a kísérlethez és a bemeneti paramétereket.

![Szolgáltatás kiválasztása – példa](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Az alábbi ábrán az eredményül kapott adatkészletek. Egyes szolgáltatások program pontozza a mennyiségeket alapján a Pearson korrelációs és a target attribútummal között a **Col1**. A felső pontszámok funkcióinak tartanak.

![Szűrő-alapú szolgáltatás kijelölés adatkészletek](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Az alábbi ábrán a megfelelő eredmények a kiválasztott funkciók.

![Kiválasztott összetevő pontszámok](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Ez alkalmazásával [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] modul, 50 kívüli 256 szolgáltatások van kiválasztva, mert a legtöbb funkciót tartozzanak a célváltozó **Col1**pontozási módszernek **Pearson korrelációs**.

## <a name="conclusion"></a>Összegzés
A szolgáltatás termékgondozó csoportja és a szolgáltatás kiválasztása készíti elő a betanítási adatok, a gépi tanulási modell fejlesztéskor gyakran végre két lépésben történik. Általában a szolgáltatás műszaki osztály további szolgáltatások létrehozásához először alkalmazzák, és majd a szolgáltatás kiválasztása a lépést nem számít, redundáns vagy magas kapcsolódó szolgáltatásokat nyújthatnak.

Nincs mindig feltétlenül mérnöki vagy szolgáltatás szolgáltatásválasztást végrehajtásához. Hogy szükség van, vagy gyűjteni az adatokat, az algoritmus választja, és a cél a kísérlet függ.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
