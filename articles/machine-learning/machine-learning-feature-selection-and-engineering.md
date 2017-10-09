---
title: "aaaFeature termékgondozó csoportja és az Azure Machine Learning kiválasztása |} Microsoft Docs"
description: "Szolgáltatás kiválasztása és a szolgáltatás műszaki osztály hello célját ismerteti, és hello adatok javító folyamat során a machine learning szerepkörük példákat."
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
redirect_document_id: True
ms.openlocfilehash: e3e59329bf46f334396f5975b4e656137362d7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Jellemzőkiválasztás és -kiemelés az Azure Machine Learningben
Ez a témakör ismerteti a szolgáltatás termékgondozó csoportja és a gépi tanulás hello adatok javító folyamat szolgáltatásválasztást hello alkalmazásában. Azt mutatja be, hogy mi ezeket a folyamatokat tartalmaz, amely Azure Machine Learning Studio által biztosított példák felhasználásával.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

gépi tanulás használt hello betanítási adatok gyakran fejleszthető hello kijelölés vagy hello nyers adatokat gyűjt a szolgáltatások kibontásával. Egy visszafejtett szolgáltatás hello környezetében tanulási tooclassify hello képek kézzel karaktereket Mitől például bit sűrűségű térképre összeállított hello nyers bit terjesztési adatokból. Ez a térkép segítségével keresse meg a hello karakterek hello széleit hello nyers terjesztési hatékonyabban.

Visszafejtett és a kiválasztott szolgáltatások hello hatékonyabbá teheti hello képzési folyamat, amely tooextract hello kulcsadatokat hello adatok tartalmazzák. Akkor is tovább fejlesztheti a modellek tooclassify hello bemeneti adatokat hello power pontosan és toopredict eredményeit megkeresheti az Önt érdeklő több abroncsnyomásmérők erős. A szolgáltatás termékgondozó csoportja és a kijelölés kombinálhatja is toomake hello tanulási több számításilag tractable. Szerint továbbfejlesztésének kezeli, és majd a szolgáltatások hello számának csökkentése szükséges toocalibrate vagy tanítási modell. Matematikailag beszéd hello szolgáltatások kijelölt tootrain hello modell olyan minimális hello adatok hello minták ismertetik, és majd sikeresen az eredmények előrejelzése független változók.

hello mérnöki és funkciók kiválasztása egy egy nagyobb folyamat része, amely általában négy lépésből áll:

* Adatgyűjtés
* Adatok továbbfejlesztése
* Modell létrehozása
* Utófeldolgozási

Mérnöki csapathoz és kiválasztása jött létre hello adatok a fejlesztés gépi tanulás lépését. Ez a folyamat három aspektusainak a célokra különböztethetők meg:

* **Előtti adatfeldolgozási**: Ez a folyamat, amely az összegyűjtött adatok hello záma tooensure tiszta és konzisztens legyen. Ez magában foglalja a feladatokat, mint a integrálása több adatkészletek, hiányzó adatok kezelésére, kezelési tranzakciós szempontból inkonzisztens adatokat, és a adattípusokat.
* **Jellemzőkiemelés**: Ez a folyamat megkísérli toocreate vonatkozó további funkciók hello adatok és tooincrease prediktív power toohello tanulási algoritmus a nyers funkciókat meglévő hello.
* **Szolgáltatás kiválasztása**: Ez a folyamat kiválasztja az eredeti adatok szolgáltatások tooreduce hello dimenzióinak, hello képzési probléma hello kulcs részét.

Ez a témakör csak a hello szolgáltatás termékgondozó csoportja és a szolgáltatás kiválasztása aspektusainak hello fejlesztés folyamata tartalmazza. Hello adatok előre feldolgozásra lépésben további információkért lásd: [előzetesen feldolgozni az adatokat az Azure Machine Learning Studióban](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).

## <a name="creating-features-from-your-data--feature-engineering"></a>Szolgáltatások létrehozása az adatok--jellemzőkiemelés
hello betanítási adatok, amelyek mindegyike rendelkezik (változók vagy az oszlopokban tárolt mezők) funkciókat (rekordok vagy megfigyelések egy sor tárolt), például álló mátrix áll. hello kísérleti tervezési hello funkciók várt toocharacterize hello minták hello adatokat. Bár a kijelölt funkcióhoz használt tootrain modell hello hello nyers adatok mezők közvetlenül tartalmazhat számos, további visszafejtett szolgáltatások gyakran kell hello nyers adatok toogenerate egy továbbfejlesztett tanítási adathalmazt hello szolgáltatásai értékekből összeállított toobe.

Milyen funkciókat létre kell hozni tooenhance hello adatkészlet egy modell betanításakor? Továbbfejlesztett hello képzési visszafejtett szolgáltatások jobban különbséget tesz a hello adatok hello minták információkat tartalmaznak. Hello új szolgáltatások tooprovide további információkra, amely nem egyértelműen rögzített vagy könnyen látható, az eredeti hello vagy meglévő funkció, de ez a folyamat történt egy kép. Megfelelő és hatékony döntések gyakran megkövetelik a néhány tartomány szakértői.

Az Azure Machine Learning-től kezdődően esetén ez a folyamat konkrétan minták segítségével a Machine Learning Studióban megadott legegyszerűbb toograsp. Két példa a itt jelenik meg:

* Egy regressziós példa ([kerékpárt bérlését hello száma előrejelzését](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) egy felügyelt kísérletben, ahol hello célértékek ismertek
* A szöveg-adatbányászati besorolás példa használatával [Szolgáltatáskivonatolás][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>1. példa: Egy regressziós modell historikus szolgáltatások hozzáadására
toodemonstrate tooengineer regressziós feladat funkciói hogyan most használja hello kísérlet "igény szerinti előrejelzéséhez kerékpárt" az Azure Machine Learning Studióban. hello ehhez a kísérlethez célja toopredict hello iránti igények hello kerékpárt, kerékpárt bérlését belül egy adott hónap, nap, illetve órával ez azt jelenti, hogy hello száma. hello adatkészlet **kerékpárt bérleti UCI adatkészlet** hello nyers bemeneti adatként szolgál.

Adatkészlet hello beruházási Bikeshare vállalati hello az Amerikai Egyesült Államok a Washingtoni bérleti kerékpárt hálózat által a valódi adatokon alapul. hello adatkészlet hello számát jelöli, kerékpárt bérlését belül egy adott óra, nap, 2011 too2012, és 17379 sorok és oszlopok 17 tartalmaz. hello nyers szolgáltatáskészlet időjárási feltételek (hőmérséklet, páratartalom, szél sebesség) és hello típusú hello nap (szünnap vagy hét napja) tartalmazza. hello mező toopredict van **cnt**, egy adott órán belül hello kerékpárt bérlését képviselő és 1 too977 találhatók, amely egy száma.

tooconstruct hatékony funkciók hello betanítási adatok, négy regressziós modell beépített hello segítségével ugyanazt az algoritmust, de négy különböző betanítási adatok állítja be. hello négy adatkészletek jelentik hello azonos nyers bemeneti adatokat, de a szolgáltatások egyre több beállítása. Ezek a funkciók négy kategóriákba vannak csoportosítva:

1. A időjárási + szünnap + hétköznap = + hétvégi szolgáltatások hello előre jelzett nap
2. B = kerékpárt, amely az egyes hello volt bérelt előző 12 óra száma
3. C = kerékpárt, amely volt bérelt minden hello hello előző 12 napjait azonos száma óránként
4. D = kerékpárt, amely volt bérelt minden hello előző 12 hetes, hello azonos száma óra és hello ugyanarra a napra esnek

Hello eredeti nyers adat már létezik, a szolgáltatás beállítása A mellett hello más három különböző szolgáltatások jönnek létre folyamat mérnöki hello funkción keresztül. A szolgáltatás B rögzítésekre hello legutóbbi igény szerint a hello kerékpárt beállítva. A szolgáltatás hello iránti igények kerékpárt beállítva C rögzíti, egy adott óra. A szolgáltatás adott óra és az adott hét napja, hello D rögzítésekre iránti igények kerékpárt beállítva. Egyes hello négy képzési adatkészletek szolgáltatáskészletek tartalmaz egy, A + B, A + B + C és A + B + C + D rendre.

Hello Azure Machine Learning kísérlet, a képzési adatok négy halmazok keresztül hello Előfeldolgozott bemeneti adatkészlet négy ágát jöttek létre. Minden ilyen ág tartalmaz hello bal szélső fiókirodai, kivéve egy [R-parancsfájl végrehajtása] [ execute-r-script] , amelyben egy származtatott (szolgáltatás B, C és beállítása D) szolgáltatások modul rendre össze, és hozzáfűzi toohello importált adatkészlet. hello a következő ábra azt mutatja be, hello R-parancsfájl toocreate szolgáltatáskészlet B használt hello második bal ágában.

![Szolgáltatás létrehozása](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

hello következő táblázat összefoglalja hello hello teljesítménymérési eredményeket hello négy modellek összehasonlítása. hello legjobb eredmények elérése érdekében A + B + C látható funkcióihoz. Vegye figyelembe, hogy hello Hibaarány csökken, amikor további szolgáltatáskészletek hello betanítási adatok szerepelnek. Ez ellenőrzi a, B és C további információkkal vonatkozó hello regressziós feladat szolgáltatáskészletek hello feltételezés. Hello D szolgáltatáskészlet hozzáadása nem tűnik tooprovide hello Hibaarány további csökkentését.

![A teljesítményeredmények összehasonlítása](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>2. példa: Szöveg adatbányászati szolgáltatások létrehozása
A szolgáltatás műszaki osztály széles körben alkalmazása feladatok kapcsolódó tootext adatbányászati, például a dokumentum besorolás és a céggel kapcsolatos véleményeket analysis. Például, ha azt szeretné, tooclassify dokumentumok számos kategóriába sorolhatók, egy tipikus feltételezi, hogy hello szavakat vagy kifejezéseket egy dokumentum kategória-e egy másik dokumentum kategóriában kevésbé valószínű toooccur. Más szóval hello szót vagy kifejezést terjesztési hello gyakorisága képes toocharacterize másik dokumentum kategóriák. A szöveg adatbányászati alkalmazások hello folyamat mérnöki szükséges toocreate hello szolgáltatásokat érintő szót vagy kifejezést gyakoriságot, mert egyes adatra szöveges-tartalom általában szolgál, bemeneti adatokat hello.

tooachieve ennek a feladatnak, elnevezésű technikát *szolgáltatáskivonatolás* van alkalmazott tooefficiently kapcsolja tetszőleges szöveg szolgáltatások az indexet. Helyett minden szöveges funkció (szavakat vagy kifejezéseket) tooa adott index, a módszer funkciók úgy, hogy alkalmazza a kivonatoló függvényt toohello szolgáltatásokat, és segítségével a kivonati értékek indexek közvetlenül társítása.

Az Azure Machine Learning van egy [Szolgáltatáskivonatolás] [ feature-hashing] modul által létrehozott szót vagy kifejezést felhasználásokhoz. hello alábbi ábrán ez a modul használatának példája. hello bemeneti adatkészlet két oszlopokat tartalmazza: hello könyv minősítés közötti 1 too5 és hello tényleges tekintse át a tartalom. hello célja [Szolgáltatáskivonatolás] [ feature-hashing] tooretrieve új funkciók, amelyek megjelenítik a hello előfordulási gyakoriságát hello megfelelő szavakat vagy kifejezéseket hello külön felülvizsgálati belül esik. toouse Ez a modul kell toocomplete hello a következő lépéseket:

1. Jelölje be hello oszlop hello bemeneti szöveget tartalmazó (**Col2** ebben a példában).
2. Állítsa be *bitsize kivonatoláshoz* too8, ami azt jelenti, 2 ^ 8 = 256 szolgáltatások jönnek létre. hello szó vagy kifejezés hello szöveg van majd kivonatolt too256 indexet. hello paraméter *bitsize kivonatoláshoz* 1 too31 tartományát. Ha hello paraméter értéke tooa nagyobb számú, hello szavak vagy kifejezések kevésbé valószínű toobe kivonatolja a hello azonos index.
3. Hello paraméter *N-g* too2. Hello előfordulási gyakoriságát unigrams (egy szolgáltatás minden egyetlen szó) és bigrams (egy szolgáltatás minden pár szomszédos szó) kikeresi hello bemeneti szöveg. hello paraméter *N-g* tartományokat a 0 too10, amely megadja, hogy a szolgáltatás szerepel szekvenciális szavak toobe hello maximális számát.  

![A szolgáltatás kivonatoló modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

hello. a következő ábra bemutatja, hogyan meg az új szolgáltatásokat.

![A szolgáltatás kivonatoló – példa](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Az adatok--szolgáltatás kiválasztása a szűrési funkciók
*Szolgáltatás kiválasztása* egy folyamat, amely általában alkalmazott képzési adatkészleteket prediktív modellezési feladatokhoz, mint az osztályozási vagy regressziós feladatok toohello építése. hello célja tooselect hello szolgáltatás hello eredeti készlet, amely csökkenti a dimenziók használatával a legszükségesebb funkciókat toorepresent hello maximális mennyisége variancia hello adatok egy részét. A funkciók részhalmazát hello csak szolgáltatások része toobe tootrain hello modell tartalmaz. Szolgáltatás kiválasztása a két fő célokra szolgál:

* Szolgáltatás kiválasztása gyakran növeli a besorolás pontossága nem számít, redundáns kiküszöbölése révén, vagy a szolgáltatások szorosan összefügg.
* Szolgáltatás kiválasztása csökken hello néhány szolgáltatás, így hatékonyabb hello modell képzési folyamat. Ez különösen fontos, amelyek például a támogatási vektoros gépek költséges tootrain tanulókkal számára.

Szolgáltatás kiválasztása tooreduce hello néhány szolgáltatás hello használt adatkészlet tootrain hello modellben kér, bár nincs általában említett tooby hello kifejezés *granularitása csökkentése érdekében.* A szolgáltatás kiválasztási módszerek őket módosítása nélkül bontsa ki az hello adatokat az eredeti funkciók egy részéhez.  Granularitása csökkentési módszereket tervezni a visszafejtett funkciókra átalakíthatja hello eredeti funkciók és így módosíthatja azokat. Granularitása csökkentési módszerek például egyszerű összetevő elemzés, kanonikus korrelációs elemzés és egyes érték felbontás ellen.

Szolgáltatás kiválasztása módszerek olyan felügyelt környezetben egy széles körben alkalmazott kategória szűrő-alapú szolgáltatás kiválasztása. Kiértékelésével hello korrelációs közötti minden egyes szolgáltatást és hello target attribútummal, ezek a módszerek a statisztikai mérték tooassign pontszám tooeach szolgáltatása alkalmazni. hello szolgáltatások majd szerinti sorrendben hello pontszám, amelynek segítségével tooset hello küszöbértéke tartása, vagy egy adott funkcióhoz megszüntetésével. Ezek a módszerek használt hello statisztikai intézkedések például Pearson korrelációs, a kölcsönös adatokat és a khi-négyzet teszt hello.

Az Azure Machine Learning Studio modulok biztosít szolgáltatás kiválasztása. Hello a következő ábrán az látható, ezek a modulok tartalmaznia kell [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] és [Fisher lineáris Discriminant elemzés] [ fisher-linear-discriminant-analysis].

![Szolgáltatás kiválasztása – példa](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)

Például, használja a hello [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] hello szöveg adatbányászati példában korábban leírt modul. Tegyük fel, amelyet egy regressziós modell toobuild 256 funkciókat keresztül hello létrehozása után [Szolgáltatáskivonatolás] [ feature-hashing] modul, és adott hello válasz változó **Col1**könyv jelölő tekintse át a minősítési 1 too5 kezdve. Állítsa be **pontozási metódus funkció** túl**Pearson korrelációs**, **céloszlop** túl**Col1**, és **száma szükséges szolgáltatások** túl**50**. hello modul [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] majd létrejön egy adatokat tartalmazó hello target attribútummal együtt 50 szolgáltatások **Col1**. hello alábbi ábra azt mutatja be hello folyamata a kísérlet, és hello bemeneti paraméterek.

![Szolgáltatás kiválasztása – példa](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

hello alábbi ábrán látható hello eredményül kapott adatkészletek. Egyes szolgáltatások program pontozza a mennyiségeket alapján hello Pearson korrelációs és hello között a target attribútummal **Col1**. a felső pontszámok hello funkcióinak használatát tartanak.

![Szűrő-alapú szolgáltatás kijelölés adatkészletek](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

a következő ábra azt mutatja be hello hello megfelelő pontszámok kijelölt hello szolgáltatást.

![Kiválasztott összetevő pontszámok](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Ez alkalmazásával [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] modul, 50 kívüli 256 szolgáltatások van kiválasztva, mert a legtöbb szolgáltatások szorosan összefügg az hello célváltozó hello **Oszlop1** hello pontozási módszer alapján **Pearson korrelációs**.

## <a name="conclusion"></a>Összegzés
A szolgáltatás termékgondozó csoportja és a szolgáltatás kiválasztása két lépést általában végre tooprepare hello betanítási adatok, a gépi tanulási modell felépítése közben. Általában szolgáltatás mérnöki alkalmazott első toogenerate további szolgáltatásokat, pedig majd hello szolgáltatás kiválasztási lépésében elvégezni tooeliminate irreleváns, redundáns vagy magas kapcsolódó funkciókat.

Nincs mindig feltétlenül tooperform szolgáltatás műszaki osztály vagy a szolgáltatás kiválasztása. E szükséges attól függ, hogy rendelkezik, vagy gyűjtése, hello algoritmust választja, és az hello kísérlet cél hello hello adatokat.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
