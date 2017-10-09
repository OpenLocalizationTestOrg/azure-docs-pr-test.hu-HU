---
title: "a HDInsight a Data Lake Tools for Visual Studio használatával aaaConnect tooAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall és -felhasználási Data Lake Tools for Visual Studio tooconnect tooHadoop fürtök Azure HDInsight és Hive-lekérdezések futtatása."
keywords: "hadoop-eszközök,hive-lekérdezés,visual studio,visual studio hadoop"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: ff5819a64bebe5f4ab3cf763ce6c45c81aa34b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-hdinsight-and-run-hive-queries-using-data-lake-tools-for-visual-studio"></a>TooAzure HDInsight csatlakozzon és Hive-lekérdezéseket a Data Lake Tools for Visual Studio használatával futtassa

Ismerje meg, hogyan toouse Data Lake Tools a Visual Studio tooconnect tooHadoop a fürtök [Azure HDInsight](hdinsight-hadoop-introduction.md) , és küldje el a Hive-lekérdezéseket. HDInsight használatával kapcsolatos további információkért lásd: [bemutatása tooHDInsight](hdinsight-hadoop-introduction.md) és [első lépései a hdinsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md). Csatlakozás tooa Storm-fürt kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák futó Apache stormra a Visual Studio használatával HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

A Data Lake Tools for Visual Studio használt tooaccess lehet Data Lake Analytics és a HDInsight.  A Data Lake Tools hello tudnivalókat lásd: [oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Előfeltételek**

Ez az oktatóanyag és -felhasználási hello Data Lake Visual Studio eszközök toocomplete, hello alábbi lesz szüksége:

* Egy Azure HDInsight fürt: toocreate egy, lásd: [Ismerkedés a Linux-alapú HDInsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md)
* A munkaállomás, amelyen a következő szoftver hello:
  
  * Windows 10, Windows 8.1, Windows 8 vagy Windows 7.
  * Visual Studio 2013/2015/2017.
    
    > [!NOTE]
    > Jelenleg hello a Data Lake Tools for Visual Studio csak kapható hello angol nyelvű.
    > 
    > 

## <a name="install-data-lake-tools-for-visual-studio"></a>A Data Lake Tools for Visual Studio telepítése

A Data Lake Tools alapértelmezés szerint a Visual Studio 2017-tel együtt települ. Régebbi verzióira telepítheti ezt hello [Webplatform-telepítő](https://www.microsoft.com/web/downloads/). Hello egyet választhat, amely megfelel a Visual Studio verziójának. Ha nincs telepítve a Visual Studio, telepítheti legújabb Visual Studio Community és Azure SDK-t használó hello hello [Webplatform-telepítő](https://www.microsoft.com/web/downloads/):

![A Data Lake Tools for Visual Studio Webplatform-telepítő. ] (./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "A Data Lake Tools for Visual Studio Webplatform-telepítő használata tooinstall")

## <a name="connect-tooazure-subscriptions"></a>Csatlakozás tooAzure előfizetések
A Data Lake Tools a Visual Studio lehetővé teszi a tooconnect tooyour HDInsight fürtökhöz, néhány alapvető felügyeleti műveletet végrehajtani, és futtathat Hive-lekérdezéseket.

> [!NOTE]
> Információk a kapcsolódó tooa általános Hadoop fürthöz: [írása, és küldje el a Hive-lekérdezések Visual Studio használatával](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

**tooconnect tooyour Azure-előfizetés**

1. Nyissa meg a Visual Studiót.
2. A hello **nézet** menüben kattintson a **Server Explorer** tooopen hello Server Explorer ablak.
3. Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.
   
   > [!NOTE]
   > Értesítés hello **HDInsight feladatlista** ablaknak nyitva kell lennie. Ha nem látja, kattintson a **más Windows** a hello **nézet** menüben, majd kattintson **HDInsight feladat lista ablakban**.  
   > 
   > 
4. Írja be az Azure-előfizetés hitelesítő adatait, majd kattintson a **Sign In** (Bejelentkezés) gombra. Erre csak akkor van szükség, ha soha nem csatlakozott-e be a toohello Azure-előfizetéshez a Visual Studio eszközből ezen a munkaállomáson.
5. A Server Explorer eszközben meglévő HDInsight-fürtök listáját láthatja. Ha nincsenek fürtjei, létrehozhat egy hello Azure-portálon, az Azure PowerShell vagy a hello HDInsight SDK használatával. További információ: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).
   
   ![Fürtök listája a Data Lake Tools for Visual Studio Kiszolgálókezelőjében](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Data Lake Tools for Visual Studio Kiszolgálókezelő")
6. Bontson ki egy HDInsight-fürtöt. Látja a **Hive adatbázisokat**, egy alapértelmezett tárfiókot, a társított tárfiókokat és a **Hadoop szolgáltatásnaplót**. Ennél jobban is kibonthatja hello entitásokat.

Azure-előfizetés tooyour csatlakoztatása után tudja toodo hello következő lesz:

**tooconnect toohello a Visual Studio Azure-portálon**

* A kiszolgálóböngészőből bontsa ki az **Azure** > **HDInsight** elemet, kattintson a jobb gombbal egy HDInsight-fürtre, majd kattintson a **Manage Cluster in Azure Portal** (Fürt kezelése az Azure Portalon) parancsra.

**tooask kérdések és visszajelzés küldése a Visual Studio eszközből**

* A hello **eszközök** menü kattintson **HDInsight**, és kattintson a **MSDN fórum** tooask kérdéseket, vagy kattintson a **visszajelzés**.

## <a name="navigate-hello-linked-resources"></a>Keresse meg a hello kapcsolódó erőforrások
A Server Explorer eszközből láthatja hello alapértelmezett tárfiókot és az összes kapcsolt tárfiókot. Ha kibontja hello alapértelmezett tárfiókot, láthatja a hello tárolók hello tárfiók. hello alapértelmezett tárfiók és hello alapértelmezett tároló meg van jelölve. Akkor is a jobb gombbal bármelyik hello tárolók tooview hello tartalma.

![Kapcsolt erőforrások listázása a Data Lake Tools for Visual Studio kiszolgálókezelőjével](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "kapcsolt erőforrások listázása")

A tároló megnyitása, után hello gombok tooupload, törlés és letöltési blobot a következő használható:

![Blobműveletek a Data Lake Tools for Visual Studio kiszolgálókezelőjével](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "blobok feltöltése, törlése és letöltése")

## <a name="run-a-hive-query"></a>Hive-lekérdezések futtatása
Az [Apache Hive](http://hive.apache.org) egy Hadoop-alapú adattárház-infrastruktúra, amely adatösszegzéseket, lekérdezéseket és elemzéseket biztosít. A Data Lake Tools for Visual Studio támogatja a Hive-lekérdezések Visual Studióból végzett futtatását. További információ a Hive-ról: [A Hive használata a HDInsightban](hdinsight-use-hive.md).

HDInsight fürtökhöz képest időigényes tootest Hive parancsfájlokat is. Percekbe vagy akár több időbe is telhet. A Data Lake Tools for Visual Studio által képes érvényesíteni a Hive parancsfájlokat helyileg nélkül csatlakozó tooa élő fürthöz.

A Data Lake Tools for Visual Studio is lehetővé teszi a felhasználók toosee belül hello Hive feladat Ha begyűjti és felszínre hozhatják bizonyos Hive-feladatok YARN naplóit hello újdonságai.

### <a name="view-hello-hivesampletable"></a>Nézet hello **hivesampletable**
Az összes HDInsight-fürt a *hivesampletable* nevű minta Hive táblát tartalmazza. Fogjuk használni a tábla tooshow, hogy hogyan toolist Hive táblák, hello táblasémákat megtekintése, és hello sorok listája hello Hive tábla.

**toolist Hive táblákat, és a Hive táblaséma megtekintése**

1. A **Server Explorer**, bontsa ki a **Azure** > **HDInsight** > hello választott fürt > **Hive adatbázisokat**  >  **Alapértelmezett** > **hivesampletable** toosee hello tábla sémáját.
2. Kattintson a jobb gombbal **hivesampletable**, és kattintson a **View Top 100 sor** toolist hello sorokat. A következő Hive-lekérdezés Hive ODBC-illesztőprogram használatával egyenértékű toorunning hello:
   
     SELECT * FROM hivesampletable LIMIT 100
   
   Testre szabhatja a hello sorok számát.
   
   ![Data Lake Tools: HDInsight Hive Visual Studio sémalekérdezés](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Hive-lekérdezés eredményei")

### <a name="create-hive-tables"></a>Hive táblák létrehozása
Hello grafikus felhasználói Felülettel toocreate Hive tábla használja, vagy Hive-lekérdezéseket használhat. A Hive-lekérdezésekkel kapcsolatban információért lásd: [Run Hive queries](#run.queries) (Hive-lekérdezések futtatása).

**Hive tábla toocreate**

1. A **Server Explorer** eszközből bontsa ki az **Azure** > **HDInsight-fürtök** egy HDInsight-fürt > **Hive Databases** (Hive-adatbázisok) elemet, majd kattintson a jobb gombbal az **alapértelmezett** elemre, és kattintson a **Create Table** (Tábla létrehozása) parancsra.
2. Konfigurálja a hello táblázatot.
3. Kattintson a **Create Table** toosubmit hello feladat toocreate hello új Hive tábla.
   
    ![Data Lake Tools: HDInsight Visual Studio-eszközök Hive-tábla létrehozásához](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Hive-tábla létrehozása")

### <a name="run.queries"></a>Hive-lekérdezések érvényesítése és futtatása
Nincsenek kétféleképpen toocreate és Hive-lekérdezések futtatása:

* Alkalmi lekérdezések létrehozása
* Hive alkalmazás létrehozása

**toocreate, érvényesítése és alkalmi lekérdezések futtatása**

1. A **Server Explorer** eszközből bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight Clusters** (HDInsight-fürtök) elemet.
2. Kattintson a jobb gombbal hello fürt, ahol szeretné toorun hello lekérdezés, és kattintson a **Hive lekérdezés írása**.
3. Adja meg a hello Hive-lekérdezéseket. Értesítés hello Hive szerkesztő támogatja az IntelliSense eszközt. A Data Lake Tools for Visual Studio támogatja hello távoli metaadatok, a Hive parancsfájl szerkesztésekor. Például amikor "SELECT * FROM", hello IntelliSense listázza az összes hello javasolt táblanevet. Ha meg van adva egy Táblanév, hello oszlopnevek hello IntelliSense alapján vannak listázva. hello eszközök szinte minden Hive DML-utasítások, segédlekérdezésekben, támogatja, és beépített felhasználó által megadott függvények hello.
   
    ![Data Lake Tools: HDInsight Visual Studio Tools IntelliSense](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Data Lake Tools: HDInsight Visual Studio Tools IntelliSense](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > Csak hello metaadatait hello fürtöket a HDInsight eszköztáron kijelölt lesz a javasolt.
   > 
   > 
4. (Nem kötelező): kattintson a **parancsfájl érvényesítése** toocheck hello parancsfájl szintaxishibáinak.
   
    ![Data Lake Tools: Data Lake Tools for Visual Studio helyi érvényesítés](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "parancsfájl érvényesítése")
5. Kattintson a **Submit** (Küldés) vagy a **Submit (Advanced)** (Küldés (Speciális)) elemre. Konfigurálhatja a speciális küldés lehetőséggel hello, **feladatnév**, **argumentumok**, **további konfigurációs**, és **állapot Directory**hello parancsfájlhoz:
   
    ![HDInsight Hadoop Hive-lekérdezés](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Lekérdezések beküldése")
   
    Hello feladat elküldése után megjelenik egy **Hive feladat összegzése** ablak.
   
    ![HDInsight Hadoop Hive-lekérdezés összegzése](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Hive-feladat összegzése")
6. Használjon hello **frissítése** tooupdate hello állapot gombra addig hello feladat állapotának megváltozásakor túl**befejezve**.
7. Kattintson hello alsó toosee hello következő hello hivatkozásait: **feladat lekérdezés**, **Job Output**, **Job log**, vagy **Yarn naplók**.

**toocreate és run Hive megoldás**

1. A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.
2. Válassza ki **HDInsight** hello bal oldali ablaktáblában jelölje ki a **Hive alkalmazás** hello középső ablaktábláján, adja meg a hello tulajdonságait, és kattintson **OK**.
   
    ![A Data Lake Tools: HDInsight Visual Studio-eszközök Új Hive-projekt](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Hive-alkalmazások létrehozása a Visual Studióból")
3. A **Megoldáskezelőben**, kattintson duplán a **Script.hql** tooopen azt.
4. toovalidate hello Hive parancsfájl hello kattinthat **parancsfájl érvényesítése** gombra, vagy kattintson a jobb gombbal a hello parancsfájl hello Hive szerkesztőben, majd **parancsfájl érvényesítése** hello helyi menüből.

### <a name="view-hive-jobs"></a>Hive-feladatok megtekintése
Megtekintheti a Hive-feladatok feladatlekérdezéseit, feladatkiemenetét, feladatnaplóit és Yarn naplóit. További információkért tekintse meg a hello előző képernyőképet.

hello hello eszközök legújabb kiadására lehetővé teszi a toosee Mi az a Hive-feladatokban belül által begyűjti és felszínre hozza a YARN naplóit. A YARN naplók segíthetnek a teljesítménnyel kapcsolatos problémák vizsgálatában. További információ arról, hogy a HDInsight hogyan gyűjti be a YARN-naplókat: [HDInsight alkalmazásnaplók szoftveres elérése](hdinsight-hadoop-access-yarn-app-logs.md).

**tooview Hive-feladatok**

1. A **Server Explorer** eszközből bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.
2. Kattintson a jobb gombbal egyyyy HDInsight-fürtre, majd kattintson a **View Jobs** (Feladatok megyytekintése) parancsra. Hive hello fürtön futtatott feladatok hello listáját láthatja.
3. Kattintson egy feladatra hello feladat lista tooselect, azt, majd használja hello **Hive feladat összegzése** ablak tooopen **feladat lekérdezés**, **Job Output**, **Job Log**, vagy **Yarn naplók**.
   
    ![Data Lake Tools: HDInsight Visual Studio-eszközök – Hive-feladatok megtekintése](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Hive-feladatok megtekintése")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Gyorsabb elérési úttal rendelkező Hive végrehajtás HiveServer2-n keresztül
> [!NOTE]
> Ez a szolgáltatás csak a HDInsight fürt 3.2-es és újabb verzióin működik.
> 
> 

hello Data Lake Tools használt toosubmit Hive-feladatok keresztül [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (más néven Templeton). Feladat részleteinek tartott egy hosszú ideig tooreturn és a hiba adatait.
A sorrend toosolve ezen teljesítményprobléma, végrehajtja a Data Lake Tools hello, kihagyva az RDP/SSH Hive feladatok közvetlenül hello fürtben hiveserver2-n keresztül.
Továbbá toobetter teljesítmény, a felhasználók is Hive Tez ábrákat megtekintheti, és hello feladat részletei.

A HDInsight-fürt 3.2-es vagy újabb verziói esetén egy **Execute via HiveServer2** (Végrehajtás HiveServer2-n keresztül) gomb látható:

![Data Lake Visual Studio Tools: Végrehajtás HiveServer2-n keresztül](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Hive-lekérdezések futtatása a HiveServer2-n keresztül")

És hello streamelt naplók valós idejű talál, és ha hello Hive-lekérdezést a tezben futtatja, lásd: hello feladatok grafikonjai.

![Data Lake Visual Studio Tools: Hive-végrehajtás gyors elérési úton](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "Feladatgrafikonok megjelenítése")

**A lekérdezések HiveServer2-n végzett végrehajtása és a lekérdezések WebHCaten keresztüli elküldése közötti különbségek**

Bár a lekérdezések HiveServer2-n keresztül végzett végrehajtásának számos előnnyel rendelkezik a teljesítmény szempontjából, vannak korlátozásai is. Hello korlátozások némelyike nem megfelelő az éles használathoz. hello következő táblázatban láthatók a hello különbségek:

|  | Végrehajtás HiveServer2-n keresztül | Elküldés WebHCaten keresztül |
| --- | --- | --- |
| Lekérdezések végrehajtása |Megszünteti a webhcat ("amely elindítja a MapReduce feladatot TempletonControllerJob" nevű) hello terhelést. |Amikor a WebHCaten keresztül hajtja végre a lekérdezést, a WebHCat elindít MapReduce feladatot, amely további késleltetést okoz. |
| Stream naplók vissza |Majdnem valós időben. |hello feladatvégrehajtási naplók csak akkor, ha hello feladat elvégzése után érhetők el. |
| Feladatelőzmények megtekintése |Ha egy lekérdezést a HiveServer2-n keresztül futtat, a feladatelőzményei (feladatnapló, feladatkimenet) nem maradnak meg. hello alkalmazás a YARN felhasználói felületen tekinthető korlátozott információkkal. |Ha egy lekérdezést a WebHCaten keresztül futtat, a feladatelőzményei (feladatnapló, feladatkimenet) megmaradnak és megtekinthetők a Visual Studio/HDInsight SDK/PowerShell használatával. |
| Ablak bezárása |Végrehajtás hiveserver2-n keresztül egy "szinkronos" módot, így mindig hello nyissa meg a windows; Ha be van zárva, hello windows hello lekérdezés-végrehajtás lehet megszakítani. |Elküldés Webhcaten keresztül módja a "aszinkron" hello elküldés Webhcaten keresztül, és zárja be a Visual Studio. Bármikor visszatérhet, és bármikor hello eredmények megtekintéséhez. |

### <a name="tez-hive-job-performance-graph"></a>Tez Hive feladat teljesítménye ábra
hello Data Lake Tools támogatja teljesítményi ábráinak hello Hive-feladatok hello Tez végrehajtómotor által végrehajtott. További információ a Tez engedélyezéséről: [A Hive használata a HDInsightban](hdinsight-use-hive.md). Elküldése után a Visual Studio, a Visual Studio projekt elsajátíthatja, hogy a struktúra hello graph hello feladat elvégzésekor.  Előfordulhat, hogy tooclick hello **frissítése** tooget hello legfrissebb feladatállapot lekérdezéséhez gombra.

> [!NOTE]
> Ez a szolgáltatás csak a HDInsight-fürt 3.2.4.593-as vagy újabb verzióihoz érhető el, és csak a befejezett feladatokhoz működik (ha a WebHCaten keresztül küldte el a feladatot; ez az ábra akkor jelenik meg, ha a lekérdezést a HiveServer2-n keresztül futtatja). Ez Windows- és Linux-alapú fürtök esetén is működik.
> 
> 

![hadoop hive tez teljesítmény ábra](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Feladat állapota")

a Hive tisztában toohelp jobb lekérdezéséhez hello eszközök hello Hive operátor nézet hozzáadása ebben a kiadásban. Egyszerűen toodouble kattintással a hello feladatgrafikon hello csúcspont, és láthatja a hello csúcspont összes hello operátort. Akkor is is vigye a mutatót a egy adott operátor tooview részletesebben ezzel a művelettel.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Feladatvégrehajtási nézet a Hive on Tez feladatokhoz
hello feladatvégrehajtási nézet a Hive on Tez feladatokhoz használható tooget strukturált és ábrázolt információkat a Hive-feladatok, és további részletek a feladatot. Ha nincs teljesítményproblémákat, hello nézet tooget további részleteket is használhatja. Például hogyan minden feladat működik, és hello részletes információt kaphat mindegyik feladatról (adatok olvasási/írási, ütemezés/kezdő/záró idő stb.), így észlelheti a feladat konfigurációja vagy a rendszer-architektúra hello ábrázolt adatok alapján.

![Data Lake Visual Studio Tools: feladat-végrehajtási nézet](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "feladat-végrehajtási nézet")

## <a name="run-pig-scripts"></a>Pig-parancsfájlok futtatása
A Data Lake Tools for Visual Studio létrehozását támogatja, és küldje el a Pig-parancsfájlok tooHDInsight fürtök. A felhasználók Pig-projektet létrehozása sablonból, és küldje el a hello parancsfájl tooHDInsight fürtök.

## <a name="feedbacks--known-issues"></a>Visszajelzések és ismert problémák
* Jelenleg a HiveServer2 eredmények tiszta szöveggel jelennek meg, ami nem ideális. Dolgozunk ennek megoldásán.
* Hello eredmények NULL értékekkel indulnak el, ha jelenleg hello eredmények nem jelennek meg. A Microsoft megoldottuk ezt a problémát, és ha elakad ennél a hibánál érzi, hogy szabad toodrop nekünk e-mailek, vagy forduljon a támogatási csoport.
* Visual Studio által létrehozott hello HQL parancsfájl attól függően, hogy a felhasználó helyi régió beállításának van kódolva. Akkor lehet, hogy nem fut megfelelően, ha felhasználó feltölt hello parancsfájl toocluster bináris formában.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan tooconnect tooHDInsight a Visual Studio eszközből hello Data Lake (HDInsight) használó fürtök eszközcsomag, és hogyan toorun Hive-lekérdezések. További információkért lásd:

* [A Hadoop Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Hadoop első lépései a HDInsightban](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Hadoop-feladatok elküldése a HDInsightban](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Twitter-adatok elemzése a Hadooppal a HDInsightban](hdinsight-analyze-twitter-data.md)

