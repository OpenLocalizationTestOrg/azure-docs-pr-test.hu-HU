---
title: "Ügyfél Kavarog egyidejűleg Machine Learning segítségével aaaAnalyzing |} Microsoft Docs"
description: "Egy integrált modell elemzésére és a felhasználói forgalom pontozási fejlődő bemutató esettanulmány"
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: 1333ffe2-59b8-4f40-9be7-3bf1173fc38d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeannt
ms.openlocfilehash: 070e6a2ebe4f2fe439a42ffe1a3fa9d6d3788d62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Az ügyfél változásainak elemzése az Azure Machine Learning segítségével
## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja a felhasználói forgalom elemzése projekt, Azure Machine Learning segítségével részét képező egy hivatkozás végrehajtása. Ez a cikk arról lesz szó kapcsolódó általános modellek holistically megoldásához hello problémáját ipari ügyfél forgalmának kezeléséhez. Mérjük is modellt a Machine Learning segítségével beépített hello pontosságát, és azt értékeléséhez további fejlesztési szakasz utasításait.  

### <a name="acknowledgements"></a>A nyugtázás
Ehhez a kísérlethez fejlesztette ki és tesztelt Serge Berger, egyszerű adatok tudósok Microsoft és Roger Barga, a Microsoft Azure Machine Learning korábban termék Manager. hello Azure dokumentációs csapattól gratefully elfogadja a saját ismereteit, és Köszönjük őket, hogy ez a dokumentum megosztása.

> [!NOTE]
> Ehhez a kísérlethez használt hello adatok nincs nyilvánosan elérhető. Hogyan toobuild egy gépi tanulási modell a forgalom elemzése példát lásd: [kereskedelmi kavarog folyamatmodell-sablont](https://gallery.cortanaintelligence.com/Collection/Retail-Customer-Churn-Prediction-Template-1) a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)
> 
> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-problem-of-customer-churn"></a>a felhasználói forgalom hello probléma
Vállalatok hello fogyasztói piacon és az összes vállalati ágazatban a forgalom toodeal rendelkeznek. Egyes esetekben adatforgalom túlzott, és hogyan befolyásolja a szabályozási döntések. hello hagyományos megoldás toopredict magas-innovációkká alakuljon churners, és azok marketingkampányok segítõ szolgáltatáson keresztül vagy különleges felmentésekről alkalmazásával igények kielégítéséhez. Ezek a módszerek iparági tooindustry és még egy adott felhasználó fürt tooanother belül (például távközlési) egy iparági eltérőek lehetnek.

hello közös tényező, vállalatok kell toominimize ilyen speciális ügyfél megőrzési erőfeszítések. Ebből kifolyólag természetes módszer volna tooscore hello valószínűségértékének inverzét forgalom minden ügyfélnek kell, és kezelje a hello legfontosabb N néhányat a meglévők közül. Előfordulhat, hogy hello felső ügyfelek hello legnyereségesebb megfelelően; például kifinomultabb forgatókönyvekben nyereség függvény alkalmaznak a különleges felmentéssel jelöltek hello kiválasztása során. Ezeket a szempontokat azonban csak hello forgalom kapcsolatos körű stratégia részét képezik. Vállalatok is rendelkeznek fiók kockázat (és társított kockázattűrése) tootake hello szint és hello beavatkozás és egyértelmű felhasználói szegmentálást költsége.  

## <a name="industry-outlook-and-approaches"></a>Iparági outlook és módszerek
Kifinomult kezelésének forgalmának kezeléséhez egy összetett iparági bejelentkezési. hello klasszikus példája hello távközlési iparági előfizetők esetén az egyik szolgáltató tooanother ismert toofrequently kapcsoló. Ez önkéntes forgalom elsődleges szempont. Ezenkívül szolgáltatóktól rendelkezik összegzi a rendszer jelentős Tudásbázis kapcsolatos *kavarog egyidejűleg illesztőprogramok*, amely nem áll hello adott meghajtó ügyfelek tooswitch.

Például kézibeszélő vagy az eszköz választott nem jól ismert vezetőjeként hello mobiltelefon üzleti forgalmáról. Ennek eredményeképpen a népszerű házirend az új előfizetők és egy teljes ár tooexisting frissítéshez ügyfelek díjszabási kézibeszélő toosubsidize hello árát. Hagyományosan a ezt a házirendet az egyik szolgáltató tooanother tooget hopping új kedvezményes, ami viszont rendelkezik kéri szolgáltatók toorefine azok stratégiák toocustomers vezetett.

A kézibeszélő ajánlatok magas illékonyság módszere, amely nagyon gyorsan az aktuális kézibeszélő modellek alapuló forgalom modelljei érvénytelenné szempontja. Emellett mobiltelefonok nincsenek csak telekommunikációs eszközök; szerepelnek (vegye figyelembe a hello iPhone) módon utasításokat, és a közösségi előre rendszeres távközlési adatkészletek hello hatókörén kívül.

hello nettó modellezési az eredménye, hogy egy hang házirend meg nem megtervezi a egyszerűen forgalom ismert okait kiküszöbölése révén. Folyamatos modellezési stratégiát, beleértve a klasszikus modellt a számlálása kategorikus változók (például a döntési fák), valójában **kötelező**.

Big data készletek használ az ügyfelek, a szervezetek big data-elemzések (ebben az esetben, a forgalom észlelési big Data típusú adatok alapján), nem hatékony módszer toohello problémát végzik. További információk hello big Data típusú adatok megközelítés toohello problémáját hello ETL szakasz javaslatok forgalmának találja.  

## <a name="methodology-toomodel-customer-churn"></a>Módszer toomodel ügyfél os forgalom
A gyakori problémák megoldásához folyamat toosolve felhasználói forgalom írja le a számok 1-3:  

1. A kockázat modell lehetővé teszi a tooconsider milyen hatással vannak a műveletek a valószínűség és a kockázatot jelent.
2. Egy beavatkozás modell lehetővé teszi a tooconsider hogyan beavatkozás hello szintű adatforgalom és hello mennyiségű ügyfél élettartamértékénél (CLV) hello valószínűségét hatással lehet.
3. Az elemzés adatmodelljeinek tooa minőségi elemzés eszkalált tooa proaktív marketingkampányt, amelynek célpontja a felhasználói szegmensek toodeliver hello optimális ajánlat.  

![][1]

Ez a megközelítés előre kinézetű hello legjobb módja tootreat forgalmának kezeléséhez, de összetettségét és az ismét: toodevelop több modellre archetype és nyomkövetési függőségek hello modellek között van. hello interakció modellek között is encapsulated, ahogy az ábra a következő hello:  

![][2]

*4. ábra: Több modellre archetype egyesített*  

Hello modellek közötti interakció kulcs esetén vagy egy körű megközelítés toocustomer megőrzési toodeliver folyamatban. Egyes modellek feltétlenül csökkenti időbeli; hello architektúra ezért egy implicit hurok (hasonló toohello archetype hello SZÍNELOSZLÁS-DM adatok adatbányászati standard beállítása [***3***]).  

hello általános kockázati-döntési-marketing Szegmentálás/felbontás ellen ciklus még mindig egy általánosított struktúra, amely megfelelő toomany üzleti problémák. Forgalom elemzése nem egyszerűen csoportját, problémák erős képviselője, mert egy összetett üzleti probléma, amely nem engedélyezi a egyszerűsített prediktív megoldás összes hello jellemzők azonban. hello közösségi hello modern megközelítés toochurn aspektusainak nem különösen kijelölt hello megközelítést, de hello közösségi szempontok encapsulated a hello modellezési archetype, azok bármely modellben lenne.  

Itt érdekes kiegészítése big data-elemzések. A mai telekommunikációs és a kereskedelmi cégek az ügyfeleknek részletes adatainak gyűjtését, és azt könnyen várható, hogy hello kell a több modellre kapcsolat lesz közös a tendencia, megadott trendek például feltörekvő hello az eszközök internetes hálózatát és széles körben használt eszközök, amelyek lehetővé teszik az üzleti tooemploy intelligens megoldások a több rétegből.  

 

## <a name="implementing-hello-modeling-archetype-in-machine-learning-studio"></a>A Machine Learning Studióban archetype modellezési hello megvalósítása
Hello legjobb módja tooimplement egy integrált modellezési és pontozási megközelítést hello probléma csak adott, mekkora? Ebben a szakaszban a bemutatjuk, hogyan azt valósítható meg ezzel Azure Machine Learning Studio használatával.  

hello több modellre megközelítés az kell, a forgalom egy globális archetype tervezése során. Pontozási hello megközelítés (prediktív) része, még akkor is, hello több modellre kell lennie.  

hello alábbi ábrán látható hello prototípus létrehozott, a Machine Learning Studio toopredict forgalom négy pontozási algoritmus alkalmazó. hello több modellre megközelítéssel oka, hogy nem csak toocreate ensemble osztályozó tooincrease pontosság, de tooprotect túlzott méretezés és tooimprove előíró szolgáltatásválasztást ellen is.  

![][3]

*5. ábra: A forgalom modellezési megközelítés az Prototípuson*  

hello következő szakaszokban azt a Machine Learning Studio használatával végrehajtott modell pontozása hello prototípus további információt.  

### <a name="data-selection-and-preparation"></a>Kijelölt adatok és előkészítése
hello adatok használt toobuild hello modellek és pontszám ügyfelek hello rejtjelezett tooprotect felhasználói adatvédelem kapott egy CRM függőleges megoldás. hello adatok információkat tartalmaz az USA hello 8000 előfizetések, és három forrásának egyesít: kiépítési adatok (előfizetés metaadatai), a tevékenység adatokat (használat hello rendszer) és a támogatási ügyféladatok. hello adatok nem tartalmaz semmilyen üzleti kapcsolatos adatokat hello ügyfelek; például az tartalmazza hűség metaadatok vagy jóváírás pontszámait.  

Az egyszerűség kedvéért ETL és a folyamatok tisztításokat adatok hatókörén kívül mivel feltételezzük, hogy adatok előkészítése már rendelkezik-e már végzett máshol.   

Szolgáltatás kiválasztása a modellezési alapján előzetes többszörösére pontozási közül hello előre hello folyamata hello véletlenszerű erdő modul által tartalmazott. A Machine Learning Studióban hello végrehajtásához azt számított hello középérték, középérték és címtartományok reprezentatív szolgáltatások. Például hozzáadott összesítések hello minőségi adatok, például a felhasználói tevékenység minimális és maximális értékeket.    

Azt is rögzített historikus adatai hello legutóbbi hat hónapban. Elemeztük az adatok egy évig, és azt létrehozni, hogy akkor is, ha nincsenek statisztikailag jelentős trendeket, forgalom hello hatással van jelentős mértékben csökken hat hónap után.  

hello legfontosabb pont hello teljes folyamat, beleértve az ETL, szolgáltatás kiválasztása, és modellezési Machine Learning Studio, a Microsoft Azure-ban tárolt adatforrások használatával lett megvalósítva.   

hello alábbi ábrák bemutatják hello adatok használttal.  

![][4]

*6. ábra: Cikkből-adatforrás (rejtjelezett)*  

![][5]

*7. ábra: Szolgáltatások kinyert adatforrás*
 

> Vegye figyelembe, hogy ezek az adatok személyes, és ezért hello modell és az adatok nem osztható meg.
> Azonban egy hasonló modell nyilvánosan elérhető adatok használatával, lásd: a minta a hello kísérletezhet [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/): [Telco ügyfél Kavarog](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> További információk a forgalom elemzési modellek használata a Cortana Intelligence Suite megvalósításától toolearn, azt javasoljuk [Ez a videó](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) által kiemelt Programvezető hétköznapon Hyong Tok. 
> 
> 

### <a name="algorithms-used-in-hello-prototype"></a>Hello prototípus használt algoritmusok
A következő négy gépi tanulási algoritmusok hello használtuk toobuild hello prototípus (nincs testreszabása):  

1. Logisztikai regresszió (LR)
2. Súlyozott döntési fa (BT)
3. Átlagolt perceptron (AP)
4. Támogatja a vektoros machine (SVM)  

hello következő diagram azt ábrázolja, hello kísérlet a tervezési felülethez, amely megadja, hogy mely hello a modellek létrehozott hello sorozat része:  

![][6]  

*8. ábra: Modellek létrehozása a Machine Learning Studióban*  

### <a name="scoring-methods"></a>A pontozási módszerek
A Microsoft pontozza a mennyiségeket hello négy modellek címkézett képzési dataset használatával.  

Adatkészlet tooa összehasonlítható modell által hello asztali kiadással SAS vállalati bányászati 12 pontozási hello is elküldése megtörtént. Hello SAS-modell, és minden négy Machine Learning Studio-modellek hello pontosságát mérni azt.  

## <a name="results"></a>Results (Eredmények)
Ebben a részben azt jelent-e az eredményekről kapcsolatos hello pontosságát hello modellek hello pontozási adatkészlet alapján.  

### <a name="accuracy-and-precision-of-scoring"></a>És pontozási pontossága
Az Azure Machine Learning hello végrehajtása általában SAS mögött van, pontossággal körülbelül 10 – 15 % (terület a görbe vagy AUC).  

Hello legfontosabb mérőszám a forgalom azonban hello téves besorolás aránya: Ez azt jelenti, hogy hello legfontosabb N churners, előre jelzett hello osztályozó által, a melyikük ténylegesen volt **nem** kavarog egyidejűleg, és még érkezett különleges kezelést? hello következő ábra a téves besorolás aránya az összes hello modell hasonlítja össze:  

![][7]

*9. ábra: Passau prototípus területen görbévé*

### <a name="using-auc-toocompare-results"></a>AUC toocompare eredmények használatával
Terület a görbe (AUC) a globális mérték jelölő metrika *separability* vonatkozó eredmények pozitív és negatív feltöltések hello disztribúciók között. Hasonló toohello hagyományos fogadó operátor jellemző (ROC) diagram, de egy fontos különbséggel, hogy hello AUC metrika nem szükséges toochoose egy küszöbértéket. Ehelyett összefoglalja az hello eredményeit a keresztül **összes** lehetőségeket. Ezzel szemben hello hagyományos ROC grafikonon hello pozitív arány hello függőleges tengely és hello téves pozitív alapján hello vízszintes tengelyre, és hello besorolás küszöbérték függ.   

AUC szolgál érdemes intézkedésként különböző algoritmusokat (vagy különböző rendszerek esetén), mivel így modellek toobe összehasonlítjuk AUC értékükre. Ez az a népszerű megközelítés például meteorológia és biosciences iparágakban. Ebből kifolyólag AUC jelöli egy népszerű eszköz osztályozó teljesítmény értékeléséhez.  

### <a name="comparing-misclassification-rates"></a>Téves besorolás díjszabás összehasonlítása
Körülbelül 8000 előfizetések hello CRM-adatok használatával hello téves besorolás sebesség az adott hello dataset az azt képest.  

* hello SAS téves besorolás sebesség 10 – 15 % volt.
* hello Machine Learning Studio téves besorolás volt az első 200-300 churners hello 15-20 %-át.  

A hello távközlési iparági esetében fontos tooaddress csak ezek az ügyfelek, akik rendelkeznek legnagyobb kockázatú toochurn hello segítõ szolgáltatásként vagy más speciális kezelés felajánlásával őket. Ebben a tekintetben hello Machine Learning Studio megvalósítási hello SAS-modell hatékonysága eléri eredmények éri el.  

Hello által azonos token, pontosság több fontos, mint pontosság mert dolgozunk leginkább megfelelő zárolásának lehetséges churners iránt érdeklődik.  

diagram követően Wikipedia hello hello kapcsolat színes, könnyen érthető kép ábrázolja:  

![][8]

*10. ábra: Kompromisszumot pontosság és a pontosság között*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Súlyozott döntési fa modell pontosságát, és a pontosság eredmények
a következő diagram megjelenítése hello nyers hello annak az eredménye pontozási hello Machine Learning prototípus toobe hello legpontosabb hello négy modellek között történik hello súlyozott döntési fa modell használatával:  

![][9]

*11. ábra: Súlyozott döntési fa modell jellemzői*

## <a name="performance-comparison"></a>Teljesítmény összehasonlítása
A Microsoft képest hello sebesség, amellyel adatokat lett pontozza a mennyiségeket hello Machine Learning Studio-modellek és az SAS vállalati bányászati 12.1 hello asztali kiadásának használatával létrehozott összehasonlítható modell használatával.  

a következő táblázat hello hello teljesítmény hello algoritmusok foglalja össze:  

*1. táblázat. Általános teljesítménye (pontosság) hello algoritmusok*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Átlagos modell |hello legjobb modell |Underperforming |Átlagos modell |

a végrehajtási, de pontossága sebességének 15-25 %-kal outperformed SAS lett nagymértékben par a Machine Learning Studióban üzemeltetett hello modellek.  

## <a name="discussion-and-recommendations"></a>Az ismertető és javaslatok
Hello távközlési iparágban, több eljárások tooanalyze rendelkezik kiderült kavarog, beleértve:  

* Származtatni a használatmérés négy alapvető kategóriák:
  * **Entitás (például egy előfizetés)**. Alapszintű információkat tartalmaz a hello előfizetés és/vagy az ügyfél, amely a forgalom hello tárgya kiépítéséhez.
  * **Tevékenység**. Szerezze be az összes kapcsolódó toohello entitás, például hello bejelentkezések száma a lehetséges használati adatait.
  * **Ügyfél-támogatási**. Nem a felhasználói támogatási naplók tooindicate származó információkat kér be az adatokat, hogy hello előfizetése kellett problémák vagy a felhasználói interakció támogatja.
  * **Versenyképes és üzleti adatokat**. Minden lehetséges kapcsolatos információkhoz hello ügyfél (például lehet nem érhető el vagy rögzített tootrack).
* Fontos toodrive szolgáltatásválasztást használja. Ez azt jelenti, hogy hello súlyozott döntési fa modell mindig a ígéret megközelítést.  

hello használata négy kategóriába hoz létre hello érhet, amely egy egyszerű *determinisztikus* módszert használja, kategóriánként, nagy valószínűséggel tényezők alakult indexek alapján elegendőnek kell lennie a forgalom veszélyben tooidentify ügyfelek. Sajnos bár ez formátumban egyértelmű úgy tűnik, továbbra is egy hamis ismertetése. hello oka az, hogy forgalmának kezeléséhez egy ideiglenes hatást és hello tényezők toochurn általában átmeneti állapotban van. Mi eredménye egy ügyfél tooconsider elhagyása Ma Holnap lehet különböző, és biztosan lesz különböző hat hónap múlva. Ezért egy *probabilisztikus* modell áttelepítésére.  

Ez fontos megfigyelési gyakran kihagyott üzleti, amely általában inkább a business intelligence megközelítéssel tooanalytics, főleg, mivel már egy egyszerűbb eladásra és egyszerű automatizálási elismeri.  

Hello ígéret önkiszolgáló Analytics a Machine Learning Studio használatával azonban, hogy a hello négy található információs kategóriák között, részleg vagy osztály által osztályozott számítógép biztonságával kapcsolatos forgalom értékes forrássá válik.  

Egy másik izgalmas az Azure Machine Learning hamarosan hello képességét tooadd egy egyéni modul toohello tárház előre definiált modulokat, amelyek már rendelkezésre állnak. Ez a funkció tulajdonképpen létrehoz egy lehetőség tooselect szalagtárak és függőleges piacok sablonok létrehozása. Az Azure Machine Learning fontos különbséget hello piaci érvényben.  

Ez a témakör a hello jövőbeli, különösen akkor kapcsolódó toobig adatelemzés Reméljük, toocontinue.
  

## <a name="conclusion"></a>Összegzés
A dokumentum nem ésszerű megközelítés tootackling hello gyakori probléma a felhasználói forgalom ismerteti egy általános keretrendszer használatával. Azt a prototípus modellek pontozó minősül, és valósítják meg az Azure Machine Learning segítségével. Végül azt értékelni hello pontosságát és hello prototípus megoldás SAS a legutóbb toocomparable algoritmussal teljesítményét.  

**További információ:**  

Adott a dokumentum segítséget? Adja meg a visszajelzést. Mondja el, 1 (rossz) too5 (kiváló) terjedő skálán, hogyan minősítené a dokumentum, és miért, adott azt minősítése? Példa:  

* Vannak, minősítés azt magas toohaving jó példa, kiváló képernyőképek, törölje a jelet írása, vagy egy másik ok miatt?
* Vannak, értékelése az alacsony megfelelő toopoor példák, intelligens képernyőképek vagy nem egyértelmű írást?  

Ezzel a visszajelzéssel fog azt kiadási tanulmányok hello minőségének javítása érdekében.   

[Visszajelzés küldése](mailto:sqlfback@microsoft.com).
 

## <a name="references"></a>Referencia
[1] prediktív elemzési: hello előrejelzéseket. augusztusi július McKnight, kezelése, 2011, p.18-20 felett.  

[2] Wikipedia cikk: [pontosság és a pontosság](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [SZÍNELOSZLÁS-DM 1.0: részletes adatok adatbányászati útmutató](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [big Data típusú adatok Marketing: hatékonyabban végezhetnek az ügyfelek és a meghajtó érték](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco kavarog folyamatmodell-sablont](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) a [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) 
 

## <a name="appendix"></a>Függelék:
![][10]

*12. ábra: Pillanatképe forgalom prototípuson bemutató*

[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
