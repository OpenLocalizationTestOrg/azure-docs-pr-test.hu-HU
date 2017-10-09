---
title: "Gépi tanulás gyakran ismételt kérdések (GYIK) aaaAzure |} Microsoft Docs"
description: "Azure Machine Learning bevezetés: a zökkenőmentes prediktív modellezést támogató felhőalapú szolgáltatással kapcsolatos számlázásra, képességekre és korlátozásokra vonatkozó GYIK."
keywords: "bevezetés a gépi tanulásba, prediktív modellezés, mi az a gépi tanulás"
services: machine-learning
documentationcenter: 
author: garyericson
manager: paulettm
editor: cgronlun
ms.assetid: a4a32a06-dbed-4727-a857-c10da774ce66
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/02/2017
ms.author: garye
ms.openlocfilehash: 3af84451dde064c3c9520ee520b541128b1eef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-frequently-asked-questions-billing-capabilities-limitations-and-support"></a>Az Azure Machine Learning szolgáltatásra vonatkozó gyakori kérdések (GYIK): Számlázás, képességek, korlátozások és támogatás
Az alábbiakban néhány gyakori kérdést (GYIK) és azok válaszait olvashatja az Azure Machine Learning szolgáltatással kapcsolatban, amely egy, a webszolgáltatásokon keresztül végrehajtott prediktív modellezést és a megoldások üzembe helyezését célzó felhőalapú szolgáltatás. E gyakran ismételt kérdések adja meg, hogyan toouse hello szolgáltatást, amely magában foglalja a számlázási modell, képességek, korlátozások és támogatás hello kapcsolatos kérdésekre.

**Kérdése van, és nem találja meg itt?**

Az Azure Machine Learning ahol hello adatok tudományos Közösség tagjai is kérdései vannak az Azure Machine Learning kapcsolatos MSDN fórumon rendelkezik. hello Azure Machine Learning team hello fórum figyeli. Nyissa meg toohello [Azure Machine Learning fórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toosearch válaszok vagy toopost saját egy másik kérdést.

## <a name="general-questions"></a>Általános kérdések
**Mi az Azure Machine Learning?**

Az Azure Machine Learning egy olyan teljes körűen felügyelt szolgáltatás, hogy lehet toocreate használja, teszteléséhez, fog működni, és kezelni hello felhőalapú prediktív elemzési megoldásokat. Csupán egy böngésző szükséges ahhoz, hogy bejelentkezzen, feltöltse adatait, és azonnal nekiláthasson a gépi tanulási kísérletekhez. Az áthúzással működtethető prediktív modellezés, a modulok széles skálájának és a kezdősablonok gyűjteményének segítségével egyszerűen és gyorsan elvégezhetők az általános gépi tanulási feladatok. További információkért lásd: hello [Azure Machine Learning szolgáltatás áttekintése](https://azure.microsoft.com/services/machine-learning/). Egy bevezető toomachine tanulási, amely ismerteti a kulcs fontos szakszavai és koncepciói, lásd: [Machine Learning bemutatása tooAzure](machine-learning-what-is-machine-learning.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Mi a Machine Learning Studio?**

A Machine Learning Studio egy böngészőn keresztül elérhető munkaterület-környezet. A Machine Learning Studio vizuális-összeállítási interfészekkel tartalmaz, amelyek megkönnyítik az-végpont létrehozása modulokat, adattudomány munkafolyamat egy kísérlet formájában hello sorát tartalmazza üzemelteti.

A Machine Learning Studióval kapcsolatos további információkért olvassa el a [Mi a Machine Learning Studio](machine-learning-what-is-ml-studio.md) című fejezetet.

**Mi az az hello Machine Learning API szolgáltatás?**

hello Machine Learning API szolgáltatás lehetővé teszi toodeploy prediktív modelleket, mint a Machine Learning Studióhoz, méretezhető, hibatűrő, webszolgáltatásként készült. hello Machine Learning API szolgáltatás által létrehozott hello webszolgáltatások olyan REST API-k, amely a felületet biztosít a külső alkalmazások és a prediktív elemzési modelljei közötti kommunikációt.

További információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

**Hol találom meg a klasszikus webszolgáltatások listáját? Hol találom az Azure Resource Manageren alapuló új webszolgáltatások listáját?**

Hello klasszikus telepítési modell és a webes szolgáltatások hello új Azure Resource Manager telepítési modellel készült használatával létrehozott webszolgáltatások hello szereplő [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/) portálon.

Klasszikus webszolgáltatásokat is szereplő [Machine Learning Studio](http://studio.azureml.net) a hello **webszolgáltatások** fülre.

## <a name="azure-machine-learning-questions"></a>Kérdések az Azure Machine Learning szolgáltatással kapcsolatban
**Mik azok a Microsoft Azure Machine Learning webszolgáltatások?**

A Machine Learning webszolgáltatások illesztőfelületet biztosítanak az alkalmazások és a Machine Learning munkafolyamatának pontozási modelljei között. Külső alkalmazás Azure Machine Learning toocommunicate használható a Machine Learning munkafolyamat pontozási modell valós időben. A Machine Learning webszolgáltatás hívás tooa előrejelzés eredmények tooan külső alkalmazás adja vissza. a hívás tooa webszolgáltatás toomake, át hello webszolgáltatás üzembe helyezésekor létrehozott API-kulcs. A Machine Learning webszolgáltatások a webprogramozási projektekben népszerű REST architektúrán alapulnak.

Az Azure Machine Learning két különböző típusú webszolgáltatást tud biztosítani:

* Kérés-válasz szolgáltatás (RR-EKET): A kis késleltetésű, amely egy felület toohello állapot nélküli modellekhez biztosít magas szinten méretezhető szolgáltatás által létrehozott és telepített Machine Learning Studio használatával.
* Kötegelt végrehajtási szolgáltatás (BES): aszinkron szolgáltatás, amely adatrekordok szerint pontozza a kötegeket.

Számos módon tooconsume hello REST API-t és hello webes szolgáltatás. Például írhat egy alkalmazást a C#, R vagy Python hello mintakódot az Ön hello webszolgáltatás üzembe helyezésekor létrehozott használatával.

hello mintakód érhető el:
- hello felhasználás lap hello webszolgáltatáshoz hello Azure Machine Learning webszolgáltatások portálon
- hello API Súgóoldalát hello webes szolgáltatás irányítópultján a Machine Learning Studióban

Hello minta Microsoft Excel-munkafüzet jön létre, és hello webes szolgáltatás irányítópultján a Machine Learning Studióban elérhető is használható.

**Mik azok a hello fő frissítések tooAzure Machine Learning?**

Hello legújabb frissítéseit, lásd: [What's new in Azure Machine Learning](machine-learning-whats-new.md).

## <a name="machine-learning-studio-questions"></a>A Machine Learning Studióra vonatkozó kérdések
### <a name="import-and-export-data-for-machine-learning"></a>Adatok importálása és exportálása a Machine Learninghez
**Milyen adatforrásokat támogat a Machine Learning?**

Adatok tooa Machine Learning Studio-kísérlet három módon tölthető le:

- Helyi fájl feltöltése adatkészletként
- A modul tooimport adatok felhőalapú adatszolgáltatásokból származó használata
- Egy másik kísérletből mentett adatkészlet importálása

További részletek toolearn támogatott fájlformátumok [betanítási adatok importálása a Machine Learning Studio](machine-learning-data-science-import-data.md).

#### <a id="ModuleLimit"></a>Hogyan nagy hello adatkészlet lehet a modulok?
A Machine Learning Studióban modulok támogatnak too10 GB-nyi számadatot tartalmazó adatkészletet be a gyakori alkalmazási esetekben. Ha egy modul egynél több bemenetből fogad adatokat, hello 10 GB érték van hello teljes az összes bemenet mérete. A nagyobb adatkészletekből Hive- vagy Azure SQL Database-lekérdezések, illetve az adatfeldolgozást megelőzően a Learning by Counts előzetes feldolgozás segítségével lehet mintát venni.  

hello adatokat toolarger adatkészletek bővítheti során és a következő korlátozott tooless 10 GB-nál:

* Ritka
* Kategorikus
* Karakterláncok
* Bináris adatok

hello a következő modulok 10 GB-nál kisebb korlátozott toodatasets:

* Ajánló modulok
* SMOTE (Synthetic Minority Oversampling Technique) modul
* Parancsfájlkezelési modulok: R, Python, SQL
* Modulok hello a kimeneti adatok mérete meghaladhatja a bemeneti adatok méretét; például az egyesítés vagy a Szolgáltatáskivonatolás
* Kereszt-ellenőrzési, hangolja modell-Hiperparamétereket, sorszám-regressziót és egyik-vs-összes Multiclass, ha nagyon nagy hello az ismétlések száma

#### <a id="UploadLimit"></a>Töltse fel az adatok hello korlátai?
A néhány GB-nál nagyobb adatkészletek esetében töltse fel az adatok tooAzure tároló vagy az Azure SQL Database, vagy használja az Azure HDInsight közvetlen feltöltés helyett egy helyi fájlból.

**Az Amazon S3-ból is lehetséges az adatbeolvasás?**

Ha kis mennyiségű adatokat, és tooexpose keresztül egy HTTP URL-CÍMÉT, majd használni tudja hello [és adatokat importálhat] [ import-data] modul. Nagyobb mennyiségű adat vigye tooAzure tárolási először, és a hello [és adatokat importálhat] [ import-data] modul toobring kísérletbe be azt.
<!--

<SEE CLOUD DS PROCESS>
-->

**Létezik beépített képbeviteli képesség?**

A hello képbeviteli képességről többet is megtudhat [képek importálása] [ image-reader] hivatkozás.

### <a name="modules"></a>Modulok
**hello algoritmus, adatforrás, adatformátum vagy adat-átalakítási művelet, amely a keresett nem az Azure Machine Learning Studióban. Milyen lehetőségeim vannak?**

Nyissa meg toohello [felhasználó-visszajelzési fórumon](http://go.microsoft.com/fwlink/?LinkId=404231) toosee szolgáltatás-kérelmek jelenleg követési. Ha egy olyan, amely a keresett képességet már kérelmezték, adja hozzá a tooa szavazáskérelem. Ha hello funkció, amely a keresett nem létezik, hozzon létre egy új kérelmet. Ezen a fórumon túl megtekintheti a kérés hello állapota. Azt szorosan nyomon követheti az ebben a listában, és a szolgáltatás rendelkezésre állásával hello állapotának gyakran frissíteni. Ezenkívül R és Python toocreate egyéni átalakítások szükség esetén hello beépített támogatást is használhatja.

**Hozzáadhatom a már létező kódomat a Machine Learning Studióhoz?**

Helyezheti Igen, a meglévő R vagy Python kódját a Machine Learning Studióhoz, hello azonos Azure Machine Learning tanulókkal kísérletezhet, és Azure Machine Learningen keresztül webszolgáltatásként hello megoldás üzembe helyezéséhez futtassa azt. További információk: [Kísérletének kiterjesztése az R nyelv használatával](machine-learning-extend-your-experiment-with-r.md), valamint [A Python gépi tanulási parancsfájlok végrehajtása az Azure Machine Learning Studióban](machine-learning-execute-python-scripts.md).

**Ennyi az alábbihoz hasonló lehetséges toouse [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) toodefine modell?**

Nem, a PMML (Predictive Model Markup Language) használata nem támogatott. Használhat egyéni R és Python code toodefine modul.

**Hány modult hajthatok végre párhuzamosan a kísérletemben?**  

Egy kísérletben párhuzamosan toofour modulok mentése hajthat végre.

### <a name="data-processing"></a>Adatfeldolgozás
**Van egy képességét toovisualize adatokat, (R megjelenítéseken kívül) hello kísérlet kísérleten belül?**

A modul toovisualize hello adatok hello kimeneti kattintson, és megtekintheti a statisztikákat.

**Eredményeit, illetve egy böngészőben adatok áttekintésekor a sorok és oszlopok száma hello korlátozva. Hogy miért?**

Nagy mennyiségű adat tooa böngésző lehet küldeni, mert adatok mérete a Machine Learning Studio lelassításának korlátozott tooprevent. toovisualize összes adat/eredmény hello, fennállt jobb toodownload hello adatokat, és az Excel vagy egy másik eszközzel.

### <a name="algorithms"></a>Algoritmusok
**Mely létező algoritmusokat támogatja a Machine Learning Studio?**

A Machine Learning Studio a legkorszerűbb algoritmusokat biztosítja, többek között például a továbbfejlesztett méretezhető döntési fákat, a Bayes ajánlási rendszereket, a neurális hálózatokat és a Microsoft Research által fejlesztett Decision Jungle algoritmust. Az olyan méretezhető, nyílt forráskódú gépi tanulási csomagokat, mint a Vowpal Wabbit, szintén támogatja a Machine Learning Studio. A Machine Learning Studio támogatja a multiclass és bináris osztályzásra, a regresszióra és a fürtszolgáltatásra használt gépi tanulási algoritmusokat. Hello teljes listájának [gépi tanulási modulok][machine-learning-modules].

**Automatikusan javasolja hello jobb gépi tanulási algoritmus toouse az adataimat?**

Nem, azonban a Machine Learning Studio különböző módokon toocompare hello eredményeit minden algoritmus toodetermine hello a problémát a megfelelőt.

**A kiadási algoritmus egy másikkal a megadott hello algoritmusok bármiféle irányelv van?**

Lásd: [hogyan toochoose algoritmus](machine-learning-algorithm-choice.md).

**A megadott hello algoritmusok R vagy Python nyelven íródtak?**

Ezek az algoritmusok nem, főleg a jobb teljesítmény érdekében lefordított nyelveken tooprovide írja.

**Azok a megadott hello algoritmusokkal kapcsolatban részletes tájékoztató?**

hello dokumentációja tartalmaz némi információt hello algoritmusok és a paraméterek a finomhangoláshoz ismertetett toooptimize hello algoritmus a használatra.  

**Létezik online tanulási támogatás?**

Nem, jelenleg csak a szoftveres átképezés támogatott.

**Neurális Net modell hello rétegek színtartományok hello beépített modul segítségével?**

Nem.

**Létrehozhatom a saját moduljaimat C# vagy más nyelvet használva?**

Jelenleg csak használható R toocreate új, egyéni modulokat.

### <a name="r-module"></a>R modul
**Milyen R csomagok érhetők el a Machine Learning Studióban?**

A Machine Learning Studio támogatja a több mint 400 CRAN R csomagokat ma, és itt hello [listába](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) az összes elérhető csomag. Lásd még [kiterjesztése az r kísérletbe](machine-learning-extend-your-experiment-with-r.md) toolearn hogyan tooretrieve a lista saját maga. Ha a hello csomagot, amelyet nem szerepel ebben a listában, adja meg a hello a hello csomag nevét a hello [felhasználó-visszajelzési fórumon](http://go.microsoft.com/fwlink/?LinkId=404231).

**Ennyi az egész lehetséges toobuild egy egyéni R modult?**

Igen, ehhez az [Egyéni R modul létrehozása az Azure Machine Learningben](machine-learning-custom-r-modules.md) című fejezetben talál további információt.

**Létezik REPL környezet az R nyelvhez?**

Nem, nincs olvasási-próbaverzió-nyomtató-hurok (REPL) környezet az R hello Studio.

### <a name="python-module"></a>Python modul
**Ennyi az egész lehetséges toobuild egy egyéni Python modult?**

Jelenleg nem de használhat egy vagy több [Python-parancsfájl végrehajtására] [ python] modulok tooget hello ugyanazt az eredményt.

**Létezik REPL környezet a Python nyelvhez?**

A Machine Learning Studióban hello Jupyter notebookok is használhatja. További információ: [Jupyter notebookok használatának bemutatása az Azure Machine Learning Studióban](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Webszolgáltatás
### <a name="retrain"></a>Újratanítás
**Hogyan működik az Azure Machine Learning modellek szoftveres átképezése?**

Hello átképezési API-k használata. További információ: [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md) (Machine Learning-modellek szoftveres átképezése). Mintakód is rendelkezésre áll, a hello [Microsoft Azure Machine Learning Átképezési bemutatóban](https://azuremlretrain.codeplex.com/).

### <a name="create"></a>Létrehozás
**Központi telepítését hello modell helyben vagy egy internetkapcsolattal nem rendelkező alkalmazás?**

Nem.

**Minden webszolgáltatás esetében számolni kell egy alapkésleltetéssel?**

Lásd: hello [Azure-előfizetésre vonatkozó korlátok](../azure-subscription-service-limits.md).

### <a name="use"></a>Használat
**Mikor érdemes toorun prediktív modellemet kötegelt végrehajtási szolgáltatásként kérelem-válasz szolgáltatás helyett?**

hello kérelem-válasz szolgáltatás (RR-EKET) értéke alacsony késleltetésű, jelentősen méretezhető webszolgáltatás, amely egy felület toostateless használt tooprovide modellek, amelyek létrehozása és hello kísérleti környezetben telepítve. hello kötegelt végrehajtási szolgáltatás (BES) olyan szolgáltatás, amely aszinkron módon pontszámaihoz a rekordok egy tranzakcióköteghez. a BES bemeneti hello például RR-EKET használó bemeneti adatokhoz. hello fő különbség az, hogy BES olvassa be az adatköteget különböző forrásokból, például az Azure Blob Storage tárolóban, az Azure Table storage, a Azure SQL Database, a HDInsight (hive lekérdezés) és a HTTP-források. További információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

**Hogyan frissíthetők a hello modell telepített hello webszolgáltatáshoz?**

tooupdate egy már telepített szolgáltatáshoz a prediktív modell módosítása, és futtassa újra a tooauthor használt hello kísérlet, és mentse a hello betanított modell. Miután hello betanított modell elérhető új verzióját, a Machine Learning Studio rákérdez, tooupdate a webszolgáltatás. Részletes információt tooupdate már üzembe helyezett webszolgáltatás, lásd: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).

Hello Retraining API-kat is használható.
További információ: [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md) (Machine Learning-modellek szoftveres átképezése). Mintakód is rendelkezésre áll, a hello [Microsoft Azure Machine Learning Átképezési bemutatóban](https://azuremlretrain.codeplex.com/).

**Hogyan követhetem figyelemmel az éles környezetben telepített webszolgáltatásaimat?**

Miután telepítette a prediktív modellt, keresztül figyelheti a hello klasszikus Azure portál (klasszikus csak a webszolgáltatások) vagy hello Azure Machine Learning webszolgáltatások portál. Minden telepített szolgáltatásnak van egy saját irányítópultja, ahol az adott szolgáltatásra vonatkozó információkat láthatja. További információ a hogyan toomanage az üzembe helyezett webszolgáltatások: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md) és [kezelése az Azure Machine Learning-munkaterület](machine-learning-manage-workspace.md).

**Van egy olyan helyre, ahol láthatom valahol az RRS/BES hello kimeneti?**

Az RRS hello webes esetében általában ahol hello eredménye látható. Akkor is kiírhatja azokat tooAzure Blob Storage tárolóban. A BES esetében hello kimeneti alapértelmezett tooa blob írja. Is írhat hello kimeneti tooa adatbázisokon vagy táblákon hello segítségével [adatok exportálása] [ export-data] modul.

**Lehetséges kizárólag a Machine Learning Studióban létrehozott modelleket használva webszolgáltatást készíteni?**

Nem lehetséges, azonban közvetlenül Jupyter-notebookok és az RStudio segítségével is létrehozhat webszolgáltatásokat.

**Hol találok információkat a hibakódokkal kapcsolatban?**

A hibakódok listáját és azok leírását a [Machine Learning modulok hibakódjai](https://msdn.microsoft.com/library/azure/dn905910.aspx) című fejezetben találja.

## <a name="scalability"></a>Méretezhetőség
**Mi az a hello webszolgáltatás hello méretezhetősége?**

Jelenleg hello alapértelmezett végpont 20 egyidejű RRS-kérelemmel végpontonként rendelkező lett kiépítve. Ezt végpontonként too200 egyidejű kérelmek méretezheti, és méretezheti minden webes szolgáltatás too10, 000 végpontra a [egy webszolgáltatás-bővítmény skálázás](machine-learning-scaling-webservice.md). A BES esetében minden végpont 40 kérelem feldolgozását tudja elvégezni egyidejűleg, illetve, amennyiben a kérelmek száma meghaladja a 40-et, az új kérelmek várólistára kerülnek. A várólistára került kérelmeket automatikusan futtató hello lista elkezd kiürülni.

**Az R-munkák csomópontokon vannak elosztva?**

Nem.  

**Mekkora mennyiségű adatot használhatok a betanításhoz?**

A Machine Learning Studióban modulok támogatnak too10 GB-nyi számadatot tartalmazó adatkészletet be a gyakori alkalmazási esetekben. Ha egy modul egynél több bemeneti, hello teljes fogad összes bemenet mérete 10 GB-os. A nagyobb adatkészletből a Hive-lekérdezések vagy az Azure SQL Database-lekérdezések, illetve az adatfeldolgozás előtt a [statisztikai tanulási (Learning with Counts)][counts] modulokkal végrehajtott előzetes feldolgozással lehet mintát venni.  

hello adatokat toolarger adatkészletek bővítheti során és a következő korlátozott tooless 10 GB-nál:

* Ritka
* Kategorikus
* Karakterláncok
* Bináris adatok

hello a következő modulok 10 GB-nál kisebb korlátozott toodatasets:

* Ajánló modulok
* SMOTE (Synthetic Minority Oversampling Technique) modul
* Parancsfájlkezelési modulok: R, Python, SQL
* Modulok hello a kimeneti adatok mérete meghaladhatja a bemeneti adatok méretét; például az egyesítés vagy a Szolgáltatáskivonatolás
* Nagyszámú ismétlődés esetében végezzen kereszt-ellenőrzést, állítsa be a modell-hiperparamétereket, továbbá végezzen sorszám-regressziót és multi-osztályú osztályozási műveletet

A néhány GB-nál nagyobb adatkészletek esetében töltse fel az adatok tooAzure tároló vagy az Azure SQL Database, vagy használja a HDInsight közvetlen feltöltés helyett egy helyi fájlból.

**Létezik bármilyen korlátozás a vektorok méretét illetően?**

Sorok és oszlopok az egyes korlátozott toohello maximális Int .NET korlátozás: 2 147 483 647.

**Módosíthatja a hello rendszert futtató virtuális gépet hello webszolgáltatás hello mérete?**

Nem.  

## <a name="security-and-availability"></a>Biztonság és elérhetőség
**Alapértelmezés szerint http-végpont hello hello webszolgáltatáshoz hozzáférő felhasználók? Hogyan korlátozhatja a hozzáférést toohello végpont?**

A webszolgáltatások telepítése után egy alapértelmezett végpont kerül létrehozásra az adott szolgáltatáshoz. hello alapértelmezett végpont az API-kulcs használatával hívható. Saját kulcsukat hello a klasszikus Azure portálon vagy programozottan használatával további végpontok hello Web Service Management API-kat is hozzáadhat. Kulcsok eléréséhez szükséges toomake hívások toohello webszolgáltatás vannak. További információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

**Mit kell tenni, ha nem találom az Azure Storage-fiókomat?**

A Machine Learning Studio támaszkodik az Azure storage felhasználó által megadott fiók toosave köztes adatokat hello munkafolyamat végrehajtásakor. Ez a tárfiók egy munkaterület létrehozása során tooMachine Learning Studio valósul meg. Hello után munkaterület jön létre, ha hello tárfiók törlődik, és többé nem lesz elérhető, hello munkaterület nem működik, és minden kísérleteket abban, hogy a munkaterület sikertelen lesz.

Ha véletlenül törölt hello tárfiókot, hozza létre újra a tárfiók hello azonos neve a hello hello hello és ugyanabban a régióban törölni a tárfiókot. Ezt követően újraszinkronizálásra hello a hozzáférési kulcsot.

**Mi történik, ha a tárfiókom hívóbetűje nincs szinkronizálva?**

A Machine Learning Studio támaszkodik az Azure storage felhasználó által megadott fiók toostore köztes adatokat hello munkafolyamat végrehajtásakor. Ez a tárfiók egy munkaterület létrehozása, és hello elérési kulcsok társítva, hogy a munkaterület tooMachine Learning Studio valósul meg. Ha hello hívóbetűket hello munkaterület létrehozását követően, hello munkaterület már nem tud hozzáférni a hello tárfiók. A munkamenet működése minden benne futó kísérlettel együtt leáll.

Ha módosította a fiók tárelérési kulcsok, újraszinkronizálásra hello hívóbetűk hello munkaterületen hello klasszikus Azure portál használatával.  

## <a name="support-and-training"></a>Támogatás és betanítás
**Hol kaphatok képzést az Azure Machine Learning használatáról?**

Hello [Azure Machine Learning dokumentációs központban](https://azure.microsoft.com/services/machine-learning/) videó oktatóanyagok és hogyan-tooguides tárolja. Ezek a lépésenkénti útmutatók hello szolgáltatások esetében, és azt ismertetik, hello adatok tudományos életciklusának adatimportálási, adattisztításon, prediktív modellek és éles környezetben őket Azure Machine Learning segítségével történő telepítéséhez.

Új jelentős toohello Machine Learning központját folyamatosan jelenleg felvenni. Hello a Machine Learning központba további oktatóanyagok anyagok esetében kérelmezheti [felhasználó-visszajelzési fórumon](https://windowsazure.uservoice.com/forums/257792-machine-learning).

A [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning) további oktatóanyagokat is kínál.

**Hol kaphatok támogatást az Azure Machine Learninghez?**

tooget technikai támogatás az Azure Machine Learning, nyissa meg túl[Azure támogatja](/support/options/), és válassza ki **Machine Learning**.

Az Azure Machine Learning egy közösségi fórummal is rendelkezik az MSDN-en, ahol az Azure Machine Learninggel kapcsolatos kérdéseit teheti fel. hello Azure Machine Learning team hello fórum figyeli. Nyissa meg túl[Azure fórumra](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## <a name="billing-questions"></a>Számlázással kapcsolatos kérdések
**Milyen díjszámítási módszer vonatkozik a Machine Learning szolgáltatásra?**

Az Azure Machine Learning két összetevőből áll: a Machine Learning Studióból és a Machine Learning-webszolgáltatásokból.

A Machine Learning Studio értékelése, amíg hello ingyenes számlázási csomagot is használhat. Ingyenes szint hello emellett lehetővé teszi a kapacitás korlátozott klasszikus webes szolgáltatás telepítése.

Ha úgy dönt, hogy az Azure Machine Learning megfelel az igényeinek, regisztrálhatnak az hello Standard csomagra. toosign be, rendelkeznie kell egy Microsoft Azure-előfizetést.

Hello normál rétegben kell fizetni havi munkaterületekhez adhat meg a Machine Learning Studióban. A kísérlet hello Studio futtatásakor díját is felszámítjuk a számítási erőforrásokat a kísérlet futtatása során. Ha a klasszikus webes szolgáltatás, a tranzakciók telepítése, és számítási órák számlázása hello Használatalapú alapon.

Az új (Resource Manager-alapú) webszolgáltatások esetében bevezettük a számlázási csomagokat, amelyek megkönnyítik a költségek tervezését. Kedvezményes díjszabás toocustomers, akik kapacitása nagy mennyiségű rétegzett árképzési kínál.

Terv létrehozása, amikor véglegesíti tooa fix egy keretbe foglalt API-számítási üzemóra és API-tranzakciószám mellékelt költség. Ha több belefoglalt mennyiségek van szüksége, példányok tooyour terv is hozzáadhat. Ha lényegesen nagyobb keretre van szüksége, érdemes magasabb szinthez tartozó csomagra áttérni, mivel így kedvezményesebb áron juthat hozzá a szükséges erőforrásokhoz.

Miután hello belefoglalt mennyiségek meglévő példányát is használta, további használati díj hello keretét hello számlázási csomag réteghez tartozó fel van töltve.

> [!NOTE]
Belefoglalt mennyiségek vannak teljesítményigény 30 nap, és nem használt belefoglalt mennyiségek nem váltása toohello következő időszak.

A számlázással és a díjszabással kapcsolatos további információkért olvassa el a [Machine Learning díjszabás](https://azure.microsoft.com/pricing/details/machine-learning/) című fejezetet.

**Létezik a Machine Learningnek ingyenes próbaverziója?**

 Az Azure Machine Learning ingyenes előfizetési lehetőséget is kínál, amelyről további információkat a [Machine Learning díjszabását](https://azure.microsoft.com/pricing/details/machine-learning/) ismertető cikkben talál. A Machine Learning Studio tartalmaz egy 8 órás gyors értékelési próbaverzió, amely akkor használható, ha túl jelentkezzen be[Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2).

 Emellett az Azure ingyenes próbaverziójára történő regisztrációt követően bármely Azure-szolgáltatást kipróbálhatja egy hónapig. toolearn hello Azure ingyenes próbaverzió kapcsolatos további információkért látogasson el [Azure ingyenes próbaverzió – GYIK](https://azure.microsoft.com/pricing/free-trial-faq/).

**Mit nevezünk tranzakciónak?**

A tranzakciók azok az API-hívások, amelyekre az Azure Machine Learning válaszol. A kérés-válasz szolgáltatás (RRS) és a kötegelt végrehajtási szolgáltatás (BES) által küldött hívások alapján létrejövő tranzakciókat a rendszer összesíti; ezekért a számlázási csomag szerint fizetnie kell.

**Használható szereplő hello tranzakció mennyiségek tervben RR-EKET és a BES tranzakciókhoz?**

Igen, a rendszer összesíti az RRS- és BES-tranzakciókat, és ezekért kell fizetnie a számlázási csomag szerint.

**Mik azok az API számítási órák?**

Egy API-számítási óra hello számlázási egység hello ideje, hogy az API-hívások hajtsa végre a megfelelő toorun a Machine Learning segítségével számítási erőforrásokat. A hívásokat a rendszer a számlázás céljából összesíti.

**Általában milyen hosszú időt vesz igénybe egy jellemző éles üzemi API-hívás?**

Éles üzemi API-hívás többször is jelentős eltérések lehetnek, általában kezdve akár több százszor is ezredmásodperc tooa néhány másodpercig. Néhány az API-hívásokban perc hello az adatfeldolgozás és -gépi tanulásra modell hello összetettségétől függően szükség lehet. hello legjobb módja tooestimate éles üzemi API hívása többször egy modellt a Machine Learning szolgáltatás hello toobenchmark.

**Mik a számítási órák a Studióban?**

A Studio számítási hello számlázási egység hello összesített ideje, hogy a kísérletek Studio számítási erőforrást használnak.

**Az új webes (Azure Resource Manager-alapú) szolgáltatásban mi hello fejlesztési és tesztelési célú réteg vállalatán?**

Resource Manager-alapú webszolgáltatásokból több tiers használható tooprovision a számlázási csomag. hello tarifacsomag fejlesztési és tesztelési célú biztosít, amelyek lehetővé teszik tootest korlátozott, belefoglalt mennyiségek kísérletbe webszolgáltatásként ezzel járó költségek nélkül. Hogyan működik a hello lehetőség toosee rendelkezik.

**Kell külön fizetnem a tárterületért?**

hello Machine Learning ingyenes szint nem kér és nem külön tároló engedélyezése. hello Machine Learning Standard csomagra felhasználók toohave Azure storage-fiók szükséges. Az Azure-tárterületért [külön kell fizetnie](https://azure.microsoft.com/pricing/details/storage/).

**Támogatja a Machine Learning a magas szintű rendelkezésre állást?**

Igen. További információkért lásd: [Machine Learning díjszabás](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) hello szolgáltatásiszint-szerződéssel (SLA) leírása.

**Konkrétan milyen számítási erőforrásokon fognak futni az éles üzemi API-hívásaim?**

hello Machine Learning szolgáltatás egy olyan több-bérlős szolgáltatás. Hello háttérben futó használt tényleges számítási erőforrások eltérők lehetnek, és a teljesítmény- és kiszámíthatóságot vannak optimalizálva.

### <a name="management-of-new-resource-manager-based-web-services"></a>Az új (Resource Manager-alapú) webszolgáltatások kezelése
**Mi történik, ha törlöm a csomagot?**

hello terv eltávolítják az előfizetés, és arányosított használati díját is felszámítjuk.

> [!NOTE]
Ha a csomagot használja egy webszolgáltatás, nem lehet törölni. toodelete hello terv, vagy jelöljön ki egy új toohello webszolgáltatás terv vagy hello webszolgáltatás törlése.

**Mik azok a csomagpéldányok?**

A terv példány egy egység belefoglalt mennyiségek számlázási csomag tooyour adhat hozzá. Amikor kiválasztja a számlázási szintet a számlázási csomaghoz, egy darab példányt kap. Ha több belefoglalt mennyiségek van szüksége, hello kijelölt számlázási réteg tooyour csomag példányai is hozzáadhat.

**Legfeljebb hány csomagpéldányt használhatok?**

Előfizetés hello fejlesztési és tesztelési célú tarifacsomag egy példánya lehet.

A Standard S1, a Standard S2 és a Standard S3 szinten annyi példányt vehet fel, amennyire csak szüksége van.

> [!NOTE]
Attól függően, hogy a várható használati, előfordulhat, hogy tooupgrade tooa költséghatékonyabb réteghez, amelynek több tartalmazott mennyiségek kell helyett adja hozzá a példányok toohello jelenlegi rétegtől.

**Mi történik, ha módosítom a csomagom szintjét (magasabbra vagy alacsonyabbra)?**

hello régi terv törlődik, és hello aktuális használati lesz számlázva van szükség. Egy új tervet hello belefoglalt teljes mennyiség hello frissítése/visszaminősített réteg hello rest hello időszak jön létre.

> [!NOTE]
A szolgáltatási keret az adott időszakra vonatkozik, a fel nem használt erőforrások nem vihetők át a következő időszakra.

**Mi történik, ha hello példányok tudom növelni a tervben?**

Mennyiségek részei van szükség, és 24 óra toobe hatékony eltarthat.

**Mi történik, ha törlök egy példányt a csomagból?**

hello példányt eltávolítják az előfizetés, és arányosított használati díját is felszámítjuk.

### <a name="sign-up-for-new-resource-manager-based-web-services-plans"></a>Regisztráció új (Resource Manager-alapú) webszolgáltatás-csomagokra
**Hogyan tudok regisztrálni a csomagokra?**

Két módon toocreate számlázási tervek rendelkezik.

A Resource Manager-alapú webszolgáltatás első üzembe helyezésekor választhat egy meglévő csomagot, vagy létrehozhat egy új csomagot.

Ez a fajta energiasémák alapértelmezett régiója, és a webszolgáltatás üzembe helyezett toothat terület lesz.

Ha nem az alapértelmezett régiójában toodeploy szolgáltatások tooregions, érdemes lehet toodefine a számlázási tervek a szolgáltatás telepítése előtt.

Ebben az esetben toohello Azure Machine Learning webszolgáltatások portálon bejelentkezhet, és válassza a toohello tervek lap. Itt hozzáadhat és törölhet csomagokat, valamint módosíthatja a meglévő csomagokat.

**Terv válasszam toostart ki együtt?**

Azt javasoljuk, hogy a kiindulási pont hello standard szintű, S1 réteg és a szolgáltatás használatának figyelése. Ha talál meg, hogy használja a belefoglalt mennyiségek gyorsan, adja hozzá a példányok vagy helyezze át a tooa magasabb szintű használható, és jobban kedvezményes díjszabás beolvasása. A számlázási csomagot bármikor tetszés szerint módosíthatja.

**Mely régiókat találhatók hello új terveket?**

hello új számlázási tervek hello amelyben új webszolgáltatások hello támogatott három éles régiókban érhetőek el:

* USA déli középső régiója
* Nyugat-Európa
* Délkelet-Ázsia

**Több régióban is működnek webszolgáltatásaim. Minden régióhoz külön csomagot kell létrehoznom?**

Igen. A csomagok ára régiónként változik. Amikor telepít egy webes szolgáltatás tooanother régióban, tooassign kell a tervet, amely az adott toothat régió informatikai. További információért lásd a [régiónként elérhető termékeket]( https://azure.microsoft.com/regions/services/).

### <a name="new-web-services-overages"></a>Új webszolgáltatások: Többletköltségek
**Hogyan tudom ellenőrizni, hogy túlléptem-e a webszolgáltatások használati keretét?**

Hello használati megtekintheti minden a tervek hello Azure Machine Learning webszolgáltatások portál hello csomagok oldalán. Jelentkezzen be toohello portálon, és kattintson a hello **tervek** menüjét.

A hello **tranzakciók** és **számítási** hello tábla oszlopainak, látható része hello mennyiségek hello terv és a használt hello százalékot.

**Mi történik, ha másolatot hello használata mennyiségek foglalandó hello fejlesztési és tesztelési célú árképzési szint?**

A fejlesztési és tesztelési célú árképzési szint hozzárendelt toothem rendelkező szolgáltatás le van állítva amíg hello következő időszak, vagy amíg fizetett tooa réteg helyezi őket.

**Hogyan számítják ki a kérés-válasz (RRS) és a kötegelt (BES) számítási feladatokért fizetendő összeget a klasszikus webszolgáltatásoknál és az új (Resource Manager-alapú) webszolgáltatások többletköltségeinél?**

Az RRS-munkaterhelés minden API-tranzakció hívás végrehajtott és hello számítási ideje, hogy ezeket a kérelmeket, amelyhez társítva van szó. Az RR-EKET éles API tranzakció költségszámítás, hello ára (egyedi tranzakció arányosítva) 1000 tranzakciók megszorozza hello végrehajtott API-hívások száma. Az RRS API éles üzemi API számítási óra költségszámítás hello összegként minden API hívása toorun, API-tranzakciószám, éles üzemi API-számítási óránként hello ár szorzata hello száma és a szükséges idő.

Például a standard szintű, S1 túlhasználati 0,72 másodpercre minden toorun 1 000 000 API-tranzakciószám eredményezne (1 000 000 * $0,50 / 1 KB-os API-tranzakciószám) a 500 (1 000 000 * 0.72 mp * $2 / hr) $400 az éles üzemi API-számítási és éles üzemi API tranzakciós költségek óra $900 összesen.

A BES munkaterhelés van szó, a hello azonos módon. Azonban hello API tranzakciós költségek jelentse hello elküldését kötegelt feladatok számát, hello számítási költségek pedig e kötegelt feladatok társított hello számítási idejét. A BES éles API tranzakciós költségek, és a hello küldött feladatok teljes száma (arányosítva egyedi tranzakció) 1000 tranzakciók hello napi ára kiszámítása. A BES API éles üzemi API számítási óra költségszámítás hello összegként a feladat toorun megszorozza hello sorok számát szorozva hello hello ára éles üzemi API és a feladatok teljes száma a feladat minden egyes sorára szükséges idő számítási óra. Gépi tanulás Számológép hello használata esetén a hello tranzakció mérő jelöli hello feladatok száma toosubmit tervezi, és hello idő-tranzakciónkénti mező képviseli hello együtt minden feladat toorun minden sorában szükséges idő.

Tételezzük fel, hogy a Standard S1 csomagban érvényes többletköltségekkel számolva napi 100 feladatot ad be, és ezek mindegyike 500, egyenként 0,72 másodpercen át futó sort tartalmaz. A havi többletköltség 1,55 USD (napi 100 feladat = 3100 feladat/hónap * 0,50 USD/1000 API-tranzakció) az éles API-tranzakciókért, és 620 USD (500 sor * 0,72 másodperc * 3100 feladat * 2 USD/óra) az éles üzemi API-k üzemórájáért, azaz összesen 621,55 USD.

### <a name="azure-machine-learning-classic-web-services"></a>Az Azure Machine Learning klasszikus webszolgáltatásai
**Elérhető még a használatalapú modell?**

Igen, az Azure Machine Learning továbbra is használható a klasszikus webszolgáltatásokkal is.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Az Azure Machine Learning Ingyenes és Standard szintje
**Hello Azure Machine Learning ingyenes szint tartalmát?**

hello Azure Machine Learning ingyenes szint tervezett tooprovide egy részletes bemutatása toohello Azure Machine Learning Studio. Legfeljebb egy Microsoft-fiók toosign szüksége lesz. hello ingyenes szint tartalmaz szabad hozzáférés tooone Azure Machine Learning Studio munkaterület / [Microsoft-fiók](https://www.microsoft.com/account/default.aspx). A réteg mentése too10 GB tárhelyet használ, és azok átmeneti API-k, modellek. Az Ingyenes szinthez tartozó számítási feladatokra nem vonatkozik SLA, ezek csak fejlesztési és személyes célokra használhatók. 

Ingyenes szint munkaterületek rendelkezik hello a következő korlátozások vonatkoznak:

* Munkaterhelések adatok csatlakozzon az SQL Server rendszert futtató tooan helyszíni kiszolgáló nem érhető el.
* Nem helyezhet üzembe új Resource Manager-alapú webszolgáltatást.


**Hello Azure Machine Learning Standard csomagban és a csomagok tartalmát?**

hello Azure Machine Learning Standard csomagra egy Azure Machine Learning Studio fizetős éles verziója telepítve. hello Azure Machine Learning Studio havi díj számlázzuk egy munkaterület / havi időközönként / és arányosított részleges hónapig. Az Azure Machine Learning Studióban eltöltött kísérletezési idő (óra) után az aktív kísérletezéssel töltött üzemóránként számítunk fel díjat. A nem teljes órákért időarányos módon kell fizetni.  

hello Azure Machine Learning API szolgáltatás lesz számlázva, attól függően, hogy az egy klasszikus webszolgáltatás-bővítmény vagy egy új, (Resource Manager-alapú) webszolgáltatáshoz.

a következő díjak hello / előfizetése munkaterület összesítése.

* Machine Learning munkaterület előfizetés: hello Machine Learning-munkaterület előfizetés hozzáférés tooa Machine Learning Studio munkaterület biztosító havi díjért. hello előfizetés hello studio és tooutilize hello üzemi API-k szükséges toorun benne.
* Stúdiókísérleti órák: A mérési összesíti, amely a Machine Learning Studióban kísérletek futtatásával vannak elhatárolt összes költségeket, és átmeneti környezet hello hívások futó éles API.
* Hozzáférés adatait csatlakozás tooan helyszíni kiszolgáló SQL Server rendszert futtató a modellekben, a képzési és pontozási.
* Klasszikus webszolgáltatásoknál:
  * Számítási üzemidő éles üzemi API-n (óra): ez az érték az éles üzemben futó webszolgáltatásokért fizetendő számítási díjakat adja meg.
  * Éles üzemi API-tranzakciók (1000 egység): A mérni tartalmaz / hívás tooyour éles webszolgáltatás vannak elhatárolt költségek.

Hello költségek, megelőző hello esetben Resource Manager-alapú webszolgáltatás, leszámítva költségek olyan összesített toohello kiválasztott terv:

* Standard S1/S2/S3 API megtervezése (egység): A mérni van kiválasztva a Resource Manager-alapú web services-példány hello típusát jelöli.
* Standard S1/S2/S3 keretét túllépő API-számítási üzemóra: A mérni tartalmaz, amely után is használta meglévő példányát tartalmazza hello mennyiségek éles környezetben futó Resource Manager-alapú webes szolgáltatások vannak elhatárolt költségeket. hello további használati díjakon van hello overate kiválasztása S1/S2/S3-csomaghoz társított sebessége.
* Standard S1/S2/S3 Túlhasználati API-tranzakciószám (1000 egység): A mérni magában foglalja a Resource Manager-alapú webszolgáltatás után hello mennyiségek szereplő meglévő példányok felhasználásának hívás tooyour gyártási vannak elhatárolt költségek. hello további használati van díjakon hello overate kiválasztása S1/S2/S3-csomaghoz társított sebessége.
* Mennyiség API-számítási üzemóra tartalmazza: Resource Manager-alapú webes szolgáltatások, a mérési jelenti API-számítási üzemóra szereplő hello mennyiségét.
* Szereplő mennyiség API-tranzakciószám (1000 egység): A Resource Manager-alapú webes szolgáltatások, a mérni API-tranzakciók mennyisége szereplő hello jelöli.

**Hogy tudok regisztrálni az Azure Machine Learning Ingyenes szintjére?**

Mindössze egy Microsoft-fiókra van szükség. Nyissa meg túl[Azure Machine Learning otthoni](https://azure.microsoft.com/services/machine-learning/), és kattintson a **Start most**. Jelentkezzen be Microsoft-fiókjával, és a rendszer létrehozza az Ön számára az Ingyenes szinthez tartozó munkaterületet. Indítsa el a tooexplore, és hozzon létre azonnal a Machine Learning kísérleteket.

**Hogy tudok regisztrálni az Azure Machine Learning Standard szintjére?**

Kell hozzáférést tooan Azure-előfizetés toocreate szabványos Machine Learning munkaterülettel. Egy 30 napos ingyenes próbaverzió Azure-előfizetéshez regisztrálhat és későbbi frissítési tooa fizetős Azure-előfizetés, vagy egy fizetős Azure-előfizetés végleges is vásárolhat. Ezután létrehozhat Machine Learning-munkaterület hello Microsoft Azure klasszikus portálon után hozzáférést toohello előfizetés ettől kezdve. Nézet hello [részletesen](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Azt is megteheti hogy meghívott egy szabványos Machine Learning munkaterület tulajdonos tooaccess hello tulajdonos munkaterületen.

**Megadhatok saját Azure Blob storage-fiók toouse hello ingyenes szint?**

Nem, hello Standard csomag Machine Learning szolgáltatás, de a rétegek bevezetett hello előtt hello egyenértékű toohello verziója telepítve.

**Telepítheti a gépi tanulási a modellek API-k, a hello ingyenes szint?**

Igen, üzembe machine learning-modellek toostaging API szolgáltatások hello ingyenes szint részeként. tooput hello átmeneti API szolgáltatás éles környezetben, és egy üzemi végpont lekérése hello operationalized szolgáltatást, hello Standard csomagot kell használnia.

**Mi az Azure ingyenes próbaverziója és az Azure Machine Learning ingyenes szint hello különbségének?**

Hello [Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/free/) kreditek tooany Azure is alkalmazható egy hónapig szolgáltatást kínál. hello Azure Machine Learning ingyenes szint ajánlatok folyamatos kifejezetten tooAzure Machine Learning nem üzemi terhelés esetén érhető el.

**Hogyan kísérlet át hello ingyenes szint toohello Standard csomagra?**

toocopy a kísérletek hello ingyenes szint toohello szabványos réteg alapján:

1. Jelentkezzen be a Machine Learning Studio tooAzure, és győződjön meg arról, hogy megjelenik hello szabad munkaterület és a szabványos munkaterületén hello hello munkaterület választó hello felső navigációs sávon.
2. TooFree munkaterület vált, ha hello szabványos munkaterületen.
3. Hello kísérlet lista nézetben jelölje ki a kísérlet, hogy kívánja toocopy, majd kattintson a hello **másolási** parancsgombra.
4. A megnyíló párbeszédpanelen hello hello szabványos munkaterület kiválasztása, és kattintson a hello **másolási** gombra.
   Az összes kapcsolódó adathalmazok hello, betanított modell stb. másolja hello kísérlet hello szabványos munkaterületre együtt.
5. Toorerun kell hello kísérletet, és a webszolgáltatás hello szabványos munkaterületen közzé.

### <a name="studio-workspace"></a>Studio-munkaterület
**A különböző munkaterületekhez különböző számlák tartoznak?**

A munkaterületek díjai az egyes vonatkozó értékek szerint vannak részletezve a számlán.

**Konkrétan milyen számítási erőforrásokon fognak futni a kísérleteim?**

hello Machine Learning szolgáltatás egy olyan több-bérlős szolgáltatás. Hello háttérben futó használt tényleges számítási erőforrások eltérők lehetnek, és a teljesítmény- és kiszámíthatóságot vannak optimalizálva.

### <a name="guest-access"></a>Vendéghozzáférés
**Mi az a vendég hozzáférés tooAzure Machine Learning Studio?**

A vendéghozzáférés a szolgáltatás korlátozott kipróbálására nyújt lehetőséget. Ingyenesen, hitelesítő adatok megadása nélkül hozhat létre és futtathat kísérleteket az Azure Machine Learning Studióban. Vendég munkameneteket nem állandó (nem lehet menteni) és a korlátozott tooeight óra. További korlátozások: nincs R- és Python-támogatás, nincs lehetőség átmeneti API-k használatára, illetve az adatkészletek mérete és a tárolókapacitás is korlátozott. Képest felhasználók, akik be egy Microsoft-fiókkal toosign választják rendelkezik teljes körű hozzáférési toohello ingyenes szint Machine Learning Studio korábban ismertetett ide tartozik az állandó munkaterület és átfogóbb képességeket. toochoose a szabad Machine Learning tapasztal, kattintson a **első lépések** a [https://studio.azureml.net](https://studio.azureml.net), majd válassza ki **kitalálni a hozzáférés** vagy jelentkezzen be a Microsoft fiók.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
