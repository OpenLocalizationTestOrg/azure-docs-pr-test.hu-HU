---
title: "aaaIntroduction bemutató Server on Azure HDInsight |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse R Server big data elemzés HDInsight-toocreate alkalmazásokra."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: daf7b70a15748d66510a04da370f39c5f26eb4ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#<a name="introduction-toor-server-and-open-source-r-capabilities-on-hdinsight"></a>Bevezetés bemutató kiszolgáló és nyílt forráskódú R képességek a HDInsight-on

Microsoft R Server a HDInsight-fürtök létrehozása az Azure-ban esetén érhető el, mint a központi telepítési lehetőség. Ez az új képesség adatszakértőkön, statisztikusok és R programozók az igény szerinti hozzáférés tooscalable elemzés hdinsight platformon történő elosztott módszereket biztosít.

Fürtök lehet méretének toohello projektek és az elvégzendő feladatok, és ha azok már nincs szükség majd bontva. Mivel azok Azure HDInsight részei, ezeken a fürtökön a vállalati szintű 24/7 támogatása szolgáltatásiszint-szerződésben garantált 99,9 %-os üzemidőt, térjen, és más összetevőket hello Azure ökoszisztéma képességét toointegrate hello.

Az R Server on HDInsight hello legújabb funkciókat az R-alapú használatával szinte bármilyen méretű betöltött tooeither Azure Blob vagy a Data Lake adatkészletek biztosít. R Server nyílt forráskódú R épül, mivel a hello R-alapú alkalmazások készít kihasználhatják a hello 8000 + nyílt forráskódú R csomagok bármelyikét. hello feladatok ScaleR, a Microsoft big data elemzés csomag részét képező R Server, az is elérhetők.

hello élcsomópont fürt kényelmesen tooconnect toohello fürt és biztosít toorun az R parancsfájlok. Egy élcsomópontot az informatikai részleg hello lehetőség a párhuzamos működésű futó hello elosztott feladatai ScaleR hello mag hello peremhálózati csomópont kiszolgáló között. Is futtathatja őket hello hello fürt csomópontjai között ScaleR a Hadoop térkép csökkentheti vagy Spark számítási környezeteket.

hello modellek vagy elemzések kapott eredmény letölthető az előrejelzés használja a helyszíni. Akkor is is lehet operationalized máshol Azure, különösen keresztül [Azure Machine Learning Studio](http://studio.azureml.net) [webszolgáltatás](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>A HDInsight R első lépések
tooinclude R Server a HDInsight-fürtöt, hello Azure-portál használatával HDInsight-fürtök létrehozásakor ki kell választania hello R Server-fürt típusa. hello R Server-fürt típusa tartalmazza az R Server hello adatok hello fürt csomópontján, és egy peremhálózati csomóponton, az R Server-alapú analytics követően zónának szolgál. Lásd: [Ismerkedés az R Server on HDInsight az](hdinsight-hadoop-r-server-get-started.md) kapcsolatos általános bemutatót hogyan toocreate hello fürt.

## <a name="learn-about-data-storage-options"></a>További tudnivalók az adatok tárolási lehetőségek
Alapértelmezett tároló hello HDFS a HDInsight-fürtök a fájlrendszer vagy egy Azure Storage-fiókot, vagy egy Azure Data Lake store társítható. Ez a társítás biztosítja, hogy az adatokat tölt fel toohello fürttároló elemzési során állandó történik. Számos különböző eszköz kezelése hello adatok átvitel toohello tárolási lehetőség választ ki, beleértve a hello portálalapú feltöltés létesítmény hello tárfiók és hello [AzCopy](../storage/common/storage-use-azcopy.md) segédprogramot.

Lehetősége van hello hozzáférés tooadditional Blob és a Data lake tárolók hozzáadásának hello fürt létesítésének folyamatát kell használnia függetlenül hello elsődleges tárolási lehetőség használata során. Lásd: [Ismerkedés az R Server on HDInsight az](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) tooadditional fiókok hozzáadására vonatkozó információt. Tekintse meg a kiegészítő hello [Azure tárolási lehetőségek R Server on HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) cikk toolearn több storage-fiókok használatával kapcsolatos további.

Is [Azure fájlok](../storage/files/storage-how-to-use-files-linux.md) tárolási beállításként való használatra hello peremhálózati csomóponton. Az Azure Files lehetővé teszi toomount egy fájlmegosztást, amely az Azure Storage toohello Linux fájlrendszer jött létre. HDInsight-fürt az R Serverhez adatok tárolási lehetőségek kapcsolatos további információkért lásd: [Azure Storage vonatkozó az R Server on HDInsight-fürtök beállítások](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-hello-cluster"></a>Hozzáférés az R Server on hello fürt
Hello élcsomópontjához a böngészőbe, feltéve, hogy a kiszolgáló hello kiépítési folyamat során kiválasztott tooinclude Rstudióból Server bemutató is elérheti. Ha nem telepítette, akkor hello fürt létesítésekor, később adhat hozzá. További információ a Rstudióból kiszolgáló telepítése egy fürt létrehozása után: [Rstudióból kiszolgáló telepítése a HDInsight-fürtök](hdinsight-hadoop-r-server-install-r-studio.md). R Server toohello SSH/PuTTY tooaccess hello R konzol használatával is kapcsolódhatnak. 

## <a name="develop-and-run-r-scripts"></a>Fejlesztéséhez és az R parancsfájlok futtatása
hello R parancsfájlok létrehozása és futtatása hello 8000 + nyílt forráskódú R csomagok hozzáadása toohello párhuzamos működésű, és az elosztott hello ScaleR könyvtárban elérhető rutinok bármelyikét használhatja. R Server hello peremhálózati csomóponton futó parancsfájl általában hello R parancsértelmező belül fut, ezen a csomóponton. hello kivételek olyan toocall ScaleR függvény és a számítási környezet tooHadoop térkép csökkentése (RxHadoopMR) vagy a Spark (RxSpark) beállított kell témakör lépéseit. Hello függvény ebben az esetben egy elosztott módon fut, ezen adatok (feladat)-csomópontokon keresztüli hello fürt, amely társul hello adatokra hivatkozik. Hello különböző számítási környezet beállításokkal kapcsolatos további információkért lásd: [számítási környezet lehetőségek R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Azok a modell
Amikor befejeződött az adatok modellezési, hello modell toomake előrejelzéseket az új adatokat, vagy az Azure és a helyszíni üzembe. Ez a folyamat pontozási néven ismert. Pontozó teheti HDInsight, Azure Machine Learning vagy a helyszínen.

### <a name="score-in-hdinsight"></a>Pontszám a Hdinsightban
a Hdinsightban, tooscore írni egy R függvényt, amely meghívja a modell toomake előrejelzéseket új adatfájl, hogy be van töltve tooyour tárfiók. Mentse hello előrejelzéseket hátsó toohello tárfiók. Hello rutin igény hello élcsomópont a fürt vagy egy ütemezett feladat segítségével is futtathatja.  

### <a name="score-in-azure-machine-learning-aml"></a>Az Azure-ban pontszám számítógép-képzési (AML)
egy AML webes szolgáltatás, és a használata hello tooscore nyissa meg a forrás Azure Machine Learning R csomag néven [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) toopublish a modell Azure webszolgáltatásként. Kényelmi célokat szolgál a csomag nincs előre telepítve van a hello élcsomópont. A következő hello webszolgáltatáshoz hello létesítményekben a Machine Learning toocreate felhasználói felületet használja, és majd hello webszolgáltatás hívására, pontozó igény szerint.

Válassza ezt a beállítást, ha szüksége tooconvert ScaleR modell objektumok tooequivalent nyílt forráskódú modell objektumok hello webes szolgáltatás használatához. Használjon ScaleR kényszerítési funkciók, például `as.randomForest()` ensemble alapú modellek esetében az átalakításhoz.

### <a name="score-on-premises"></a>Helyszíni pontszám
tooscore a helyszíni a modell létrehozása után is szerializálható R, töltse le, deszerializálni, mert azt, és kövesse az új adatok pontozása hello modellt. Korábban a leírt hello módszer segítségével új adatok is pontozása [hdinsight pontozási](#scoring-in-hdinsight) vagy használatával [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-hello-cluster"></a>Hello fürt karbantartása
### <a name="install-and-maintain-r-packages"></a>Telepítheti és karbantarthatja R csomagok
Hello R csomagok, amelyekkel a legtöbb hello élcsomópont óta az R parancsfájlok nincs legtöbb lépések szükségesek. tooinstall további R csomagokat hello peremhálózati csomóponton, a szokásos hello használhatja `install.packages()` R. metódus

Ha rutinok hello ScaleR könyvtárból hello fürtön csak használ, nem általában akkor tooinstall további R csomagokat hello adatok csomópontján. Azonban szükség lehet további csomagokat toosupport hello használata **rxExec** vagy **RxDataStep** végrehajtási hello adatok csomópontján.

Ezekben az esetekben a hello fürt létrehozása után parancsfájl művelettel hello további csomagokat is telepítheti. További információkért lásd: [HDInsight fürtök létrehozásával R Server](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoop-leképezés csökkentése memória beállításainak módosítása
A fürt módosított toochange hello memóriamennyiség, amely elérhető bemutató kiszolgáló, amikor egy térképen csökkentse feladat fut is lehet. toomodify egy fürt használja hello Apache Ambari felhasználói felület, amely a fürt számára az Azure portál panel hello keresztül érhető el. Leírja, hogyan tooaccess hello Ambari felhasználói felület a fürtöt, tekintse meg [hello Ambari webes felhasználói felület használatával kezelheti a HDInsight-fürtök](hdinsight-hadoop-manage-ambari.md).

Az is lehetséges toochange hello memóriamennyiség, amely elérhető bemutató Server Hadoop kapcsolók alkalmazásával hello hívásában túl**RxHadoopMR** az alábbiak szerint:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Fürt méretezése
Egy meglévő fürthöz is méretezhető felfelé vagy lefelé hello portálon keresztül. Skálázással, hello további kapacitást, amelyek hasznosak lehetnek a nagyobb feldolgozási feladatok áttekintésével felmérheti, vagy méretezheti vissza egy fürt tétlen állapotában. Leírja, hogyan a tooscale egy fürtbe, lásd: [kezelése HDInsight-fürtök](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-hello-system"></a>Hello rendszer karbantartása
Az alapul szolgáló Linux virtuális gépek a HDInsight-fürtök során munkaidőn kívüli hello karbantartási tooapply operációsrendszer-javítások és más frissítések történik. Általában karbantartási történik, 3:30-kor (hello helyi időre hello VM alapján), minden hétfőjén és csütörtökén. Úgy, hogy több, mint a negyedév hello fürt egyszerre nem hatással lesznek a frissítések megtörténik.  

Mivel hello átjárócsomópontokkal redundáns, és nem minden adatcsomópontokat érintettek, bármely ez idő alatt futó feladatok lelassíthatják. Ezek továbbra is fusson toocompletion, azonban. Bármely egyéni szoftvert vagy a helyi adatok, amelyekhez őrzi meg karbantartási eseményekből keresztül, kivéve, ha végzetes hiba lép fel, amely egy fürt újjáépítése szükséges.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>További tudnivalók: IDE beállítások R Server on HDInsight-fürt
hello Linux élcsomópont egy HDInsight-fürt az R-alapú elemzése zóna üzenetsorokra hello. HDInsight újabb verzióiban adjon meg egy alapértelmezett beállítása az hello közösségi verziójának telepítése [Rstudióból Server](https://www.rstudio.com/products/rstudio-server/) , egy webböngésző-alapú IDE hello peremhálózati csomóponton. Egy IDE hello fejlesztési Rstudióból kiszolgáló használni és az R parancsfájlok végrehajtását lehet jelentősen hatékonyabb, mint csak a hello R konzol használatával. Ha úgy döntött, hogy nem tooadd Rstudióból Server hello a fürt létrehozása, de szeretné tooadd azt később, majd tekintse meg [R Studio kiszolgáló telepítése a HDInsight-fürtökön](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-install-r-studio). +

Egy másik teljes IDE lehetőség tooinstall asztali IDE, és tooaccess hello fürt használata egy távoli térkép csökkentse vagy Spark számítási környezetet használni. A Microsoft a választható lehetőségek [R Tools for Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) Rstudióból és Walware tartozó Eclipse-alapú [StatET](http://www.walware.de/goto/statet).

Végül hozzáférhessen hello R Server konzolhoz hello peremhálózati csomóponton beírásával **R** parancssorban hello Linux SSH vagy PuTY segítségével történő kapcsolódás után. Hello konzol felületét használatakor kényelmes toorun egy szövegszerkesztőben, egy másik ablakban R-parancsfájl fejlesztési és darabolást és illessze be a parancsfájl szakaszok hello R konzolba igény szerint.

## <a name="learn-about-pricing"></a>További tudnivalók díjszabása
hello egy HDInsight-fürt az R Server rendszer társított díjak szerkezete hasonlít toohello díjai hello szabványos HDInsight-fürtök. Az alapul szolgáló virtuális gépek között hello nevét, az adatok és a peremhálózati csomópontokat, a hello hozzáadása egy alapvető órás uplift hello hello méretezési alapulnak. HDInsight árak, és egy 30 napos ingyenes próbaverzióval hello rendelkezésre állását kapcsolatos további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Következő lépések
toolearn hogyan toouse R Server a HDInsight-fürtök, kapcsolatos további információkért tekintse meg a következő témakörök hello:

* [Bevezetés az R Server on HDInsight használatába](hdinsight-hadoop-r-server-get-started.md)
* [Adja hozzá a Rstudióból Server tooHDInsight (Ha nincs telepítve a fürt létrehozása során)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Számítási környezeti beállítások a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage lehetőségek a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-storage.md)
