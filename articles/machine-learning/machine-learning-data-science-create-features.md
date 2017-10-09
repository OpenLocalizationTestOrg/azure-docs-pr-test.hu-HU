---
title: "adattudomány aaaFeature mérnöki |} Microsoft Docs"
description: "Bemutatja a szolgáltatás mérnöki hello alkalmazásában, valamint szerepét a gépi tanulás hello adatok a fejlesztés folyamata példái."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3fde69e8-5e7b-49ad-b3fb-ab8ef6503a4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: af40ea9cc9395bc87fe695eeaef26aa71e0ec9e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="feature-engineering-in-data-science"></a>Jellemzőkiemelés tudományos adatok
Ez a témakör ismerteti a szolgáltatás műszaki osztály hello alkalmazásában és példák a szerepét a gépi tanulás hello adatok a fejlesztés folyamata. hello példák ezt a folyamatot az Azure Machine Learning Studio állítják tooillustrate használt. 

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toocreate-adatok különböző környezetekben funkcióit tootopics. Ez a feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Ez a funkció mérnöki kísérletek tooincrease hello prediktív hatványa tanulási algoritmus a nyers adatok hello tanulási folyamat megkönnyítése érdekében a szolgáltatások létrehozásával. hello mérnöki és funkciók kiválasztása része egy TDSP leírt hello hello [hello Team adatok tudományos folyamat életciklus újdonságai?](data-science-process-overview.md) A szolgáltatás termékgondozó csoportja és a kijelölés hello részét képezik **szolgáltatások fejlesztéséhez** hello TDSP lépését. 

* **jellemzőkiemelés**: Ez a folyamat kísérletet tett toocreate vonatkozó további funkciók a meglévő nyers funkciók hello hello adatokat, és tooincrease hello prediktív power hello tanulási algoritmus.
* **szolgáltatás kiválasztása**: Ez a folyamat az eredeti adatokkal kapcsolatos funkciókkal hello kulcs részét egy kísérlet tooreduce hello dimenzióinak hello képzési probléma, választja ki.

Általában **jellemzőkiemelés** alkalmazott első toogenerate további szolgáltatásokat, valamint hello **kijelölés funkció** lépést elvégezte tooeliminate irreleváns, redundáns vagy magas korrelált szolgáltatások.

gépi tanulás használt hello betanítási adatok gyakran fejleszthető úgy hello nyers adatokat gyűjt a szolgáltatásokat. Példa egy visszafejtett szolgáltatás létrehozása egy kicsit hello nyers bit terjesztési adataiból összeállított sűrűség térkép tooclassify hello képek kézzel karaktereket Mitől tanulási hello környezetében. Ez a térkép segítségével keresse meg a hello széleit hello karaktereket, mint egyszerűen közvetlenül az hello nyers eloszlás használatával hatékonyabban.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="creating-features-from-your-data---feature-engineering"></a>Szolgáltatások létrehozása az adatok - Jellemzőkiemelés
hello betanítási adatok, amelyek mindegyike rendelkezik (változók vagy az oszlopokban tárolt mezők) funkciókat (rekordok vagy megfigyelések egy sor tárolt), például álló mátrix áll. hello kísérleti tervezési hello funkciók várt toocharacterize hello minták hello adatokat. Bár a kijelölt funkcióhoz használt tootrain modell hello hello nyers adatok mezők közvetlenül tartalmazhat számos, az helyzet gyakran áll hello igénylő további (visszafejtett) szolgáltatások hello nyers adatok toogenerate egy továbbfejlesztett hello szolgáltatásai értékekből összeállított toobe képzési adatkészlet.

Milyen funkciókat létre kell hozni tooenhance hello adatkészlet egy modell betanításakor? Továbbfejlesztett hello képzési visszafejtett szolgáltatások jobban különbséget tesz a hello adatok hello minták információkat tartalmaznak. A Microsoft hello új szolgáltatások tooprovide további információkra, amely nem egyértelműen rögzített vagy egyszerűen a nyilvánvaló hello eredeti vagy meglévő készlet. Azonban ez a folyamat történt egy kép. Megfelelő és hatékony döntések gyakran megkövetelik a néhány tartomány szakértői.

Az Azure Machine Learning-től kezdődően esetén a folyamat konkrétan minták megadott hello Studio legegyszerűbb toograsp. Két példa a itt jelenik meg:

* Egy regressziós példa [kerékpárt bérlését hello száma előrejelzését](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) egy felügyelt kísérletben, ahol hello célértékek ismertek
* A szöveg adatbányászati besorolás példa használatával [Szolgáltatáskivonatolás](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

## <a name="example-1-adding-temporal-features-for-regression-model"></a>1. példa: Historikus szolgáltatások hozzáadására regressziós modell
Most használja hello kísérlet "igény szerinti előrejelzés kerékpárt az" Azure Machine Learning Studio toodemonstrate tooengineer funkciói hogyan regressziós feladathoz. hello ehhez a kísérlethez célja toopredict hello iránti igények hello kerékpárt, egy adott hónap/nap/órán belül kerékpárt bérlését Ez azt jelenti, hogy hello száma. a dataset hello "bérlet kerékpárt UCI dataset" hello nyers bemeneti adatként használt. Ez az adatkészlet hello beruházási Bikeshare vállalati hello az Amerikai Egyesült Államok a Washingtoni bérleti kerékpárt hálózat által a valódi adatokon alapul. hello dataset kerékpárt bérlését hello számát jelenti. a nap hello éven belülre esik 2011 és 2012-ben meghatározott órán belül, és tartalmazza a 17379 sorok és oszlopok 17. hello nyers szolgáltatáskészlet időjárási feltételek (hőmérséklet/páratartalom/szél sebesség) és hello típusú hello nap (szünnap/hét napja) tartalmazza. hello mező toopredict "számláló", amely hello kerékpárt bérlését jelöli egy adott órán belül, és amely tartományok, 1 too977 a tartományok száma.

Hello célja hozhat létre, hatékony szolgáltatásai hello betanítási adatok, a négy regressziós modell használatával készített hello ugyanazt az algoritmust, de négy különböző képzési adatkészletekkel. hello négy adatkészletek jelentik hello azonos nyers bemeneti adatokat, de a szolgáltatások egyre több beállítása. Ezek a funkciók négy kategóriákba vannak csoportosítva:

1. A időjárási + szünnap + hétköznap = + hétvégi szolgáltatások hello előre jelzett nap
2. B = kerékpárt, amely az egyes hello volt bérelt előző 12 óra száma
3. C = kerékpárt, amely volt bérelt minden hello hello előző 12 napjait azonos száma óránként
4. D = kerékpárt, amely volt bérelt minden hello előző 12 hetes, hello azonos száma óra és hello ugyanarra a napra esnek

Hello eredeti nyers adat már létezik, a szolgáltatás beállítása A mellett hello más három különböző szolgáltatások jönnek létre folyamat mérnöki hello funkción keresztül. A szolgáltatás nagyon friss iránti igények hello kerékpárt beállítva a B rögzíti. A szolgáltatás hello iránti igények kerékpárt beállítva C rögzíti, egy adott óra. A szolgáltatás adott óra és az adott hét napja, hello D rögzítésekre iránti igények kerékpárt beállítva. hello négy tanítási adathalmazt minden tartalmazza a szolgáltatás egy, A + B, A + B + C, és A + B + C + D rendre.

Az Azure Machine Learning kísérlet hello ezen négy képzési adatkészletek hozhatók létre a Windowsban hello Előfeldolgozott bemeneti adatkészletből négy ágak keresztül. Hello balra legtöbb fiókirodai, kivéve a minden ilyen ág tartalmaz egy [R-parancsfájl végrehajtása](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modult, amelyben származtatott funkciókat, (B, C és D szolgáltatáskészleteket) importált felépített és hozzáfűzött értékre toohello adatkészlet. hello a következő ábra azt mutatja be, hello R-parancsfájl toocreate szolgáltatáskészlet B használt hello második bal ágában.

![szolgáltatások létrehozása](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

hello teljesítménymérési eredményeket hello négy modellek összehasonlítása hello hello a következő táblázat foglalja össze. hello legjobb eredmények elérése érdekében A + B + C látható funkcióihoz. Figyelje meg, hogy hello hiba gyakorisága csökken, amikor további szolgáltatáskészlet hello betanítási adatok szerepelnek. Ellenőrzi, hogy hello szolgáltatáskészlet B, C biztosítson hello regressziós feladat kapcsolatos további információt a feltételezés. De hozzáadását hello D szolgáltatás látszólag nem tooprovide hello Hibaarány további csökkentését.

![eredmények összehasonlítása](./media/machine-learning-data-science-create-features/result1.png)

## <a name="example2"></a>2. példa: Szöveg adatbányászati létrehozása szolgáltatásai
A szolgáltatás műszaki osztály széles körben alkalmazása feladatok kapcsolódó tootext adatbányászati, például a dokumentum besorolás és a céggel kapcsolatos véleményeket analysis. Például különböző kategóriákba tooclassify dokumentumok azt szeretnénk, ha egy tipikus feltételezi, hogy hello word/kifejezések egy doc kategória-e egy másik doc kategóriában kevésbé valószínű toooccur. Más szóval hello szavak vagy kifejezések terjesztési hello gyakorisága képes toocharacterize másik dokumentum kategóriák. A szöveg adatbányászati alkalmazások mivel egyes adatra szöveges-tartalom általában szolgál hello bemeneti adatként, hello folyamat mérnöki szó vagy kifejezés gyakoriságot érintő szükséges toocreate hello szolgáltatásokat.

tooachieve ennek a feladatnak, elnevezésű technikát **szolgáltatáskivonatolás** van alkalmazott tooefficiently kapcsolja tetszőleges szöveg szolgáltatások az indexet. Helyett minden szöveges (szavak vagy kifejezések) szolgáltatás tooa adott index, a kivonatoló függvényt toohello szolgáltatások alkalmazásával, és közvetlenül a kivonati értékek használata indexek metódus funkciók társítása.

Az Azure Machine Learning van egy [Szolgáltatáskivonatolás](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modul, amely hozza létre ezeket a szó vagy kifejezés szolgáltatások kényelmesen. Alábbi ábrán ez a modul használatának példája. hello bemeneti adatkészlet két oszlopokat tartalmazza: hello 1 too5 közötti könyv minősítés, és a tényleges felülvizsgálati tartalom hello. hello célja [Szolgáltatáskivonatolás](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modul tooretrieve álló, lemezcsoport típusú új funkciók, amelyek megjelenítik a hello előfordulási gyakoriságát szavak megfelelő hello / tájékoztatás(ok) hello külön felülvizsgálati belül. toouse Ez a modul igazolnia kell a következő lépéseket toocomplete hello:

* Első lépésként válassza ki a bemeneti szöveg hello hello oszlopa (ebben a példában "Col2" jelöli).
* Ezután állítsa be a hello "Hashing bitsize" too8, ami azt jelenti, 2 ^ 8 = 256 szolgáltatások jön létre. hello word/fázisában összes hello szövegként lesz kivonatolt too256 indexet. hello paraméter "Hashing bitsize" 1 too31 találhatók. hello szavak / tájékoztatás(ok) kevésbé valószínű toobe kivonatolt azonos index, ha a beállítás a korábbinál több toobe hello be.
* Harmadik állítsa be a hello paraméter "N-g" too2. Hello előfordulási gyakoriságát unigrams (egy szolgáltatás minden egyetlen szó) és bigrams (egy szolgáltatás minden pár szomszédos szó) lekérése hello bemeneti szöveg. hello paraméter "N-g" tartományok 0 too10, amely megadja, hogy a soros szavak toobe hello maximális száma a szolgáltatás szerepel.  

!["Funkció Hashing" modul](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

hello következő ábrán az látható, mi hello ezek új szolgáltatás megjelenését hasonlóan.

!["Funkció Hashing" – példa](./media/machine-learning-data-science-create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Összegzés
Visszafejtett és a kiválasztott szolgáltatások hello hatékonyabbá teheti hello képzési folyamat, amely tooextract hello kulcsadatokat hello adatok tartalmazzák. Akkor is tovább fejlesztheti a modellek tooclassify hello bemeneti adatokat hello power pontosan és toopredict eredményeit megkeresheti az Önt érdeklő több abroncsnyomásmérők erős. A szolgáltatás termékgondozó csoportja és a kijelölés kombinálhatja is toomake hello tanulási több számításilag tractable. Szerint továbbfejlesztésének kezeli, és majd a szolgáltatások hello számának csökkentése szükséges toocalibrate vagy tanítási modell. Matematikailag beszéd hello szolgáltatások kijelölt tootrain hello modell olyan minimális hello adatok hello minták ismertetik, és majd sikeresen az eredmények előrejelzése független változók.

Ne feledje, hogy nem mindig feltétlenül tooperform szolgáltatás műszaki osztály vagy a szolgáltatás kiválasztása. E rá szükség, vagy nem attól függ, hogy azt rendelkezik vagy gyűjtése, válassza ki, hogy hello algoritmus és cél hello kísérlet hello hello adatokat.

