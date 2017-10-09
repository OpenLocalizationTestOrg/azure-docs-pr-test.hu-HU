---
title: "aaaArchived kibocsátási megjegyzések - on Azure HDInsight Hadoop-összetevők |} Microsoft Docs"
description: "Archivált kibocsátási megjegyzések az Azure hdinsight Hadoop-összetevők régebbi verzióira."
services: hdinsight
documentationcenter: 
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 7a99c77f4557ca8c1dabe924cc67b2e0a134f8c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-archive-for-hadoop-components-on-azure-hdinsight"></a>Az Azure HDInsight Hadoop-összetevők (archív) kibocsátási megjegyzései

Ez a cikk ismerteti a hello **régebbi** Azure HDInsight kiadás frissítéseket. Az újabb kiadásokban információkért lásd: [HDInsight kibocsátási megjegyzések](hdinsight-release-notes.md).

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight versioning cikk](hdinsight-component-versioning.md).



## <a name="notes-for-08302016-release-of-hdinsight"></a>A HDInsight 08/30/2016 kibocsátási megjegyzései
Ebben a kiadásban teljes verziószámai hello Linux-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build | Ambari Build |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8268980 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8268980 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8269383 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Ebben a kiadásban teljes verziószámai hello Windows-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1033.2559206 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1033.2559206 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1033.2559206 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1033.2559206 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1033.2559206 |2.3 |2.3.3.1-25 |


## <a name="08172016---release-of-r-server-on-hdinsight"></a>08/17/2016 - R Server on HDInsight kiadása
* R Server 8.0.5 - főként a hibajavítás kiadása. Lásd: hello [R Server kibocsátási megjegyzések](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) vonatkozó információ.
* Az AzureML-csomagot a hello élcsomópont - [R csomag](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) lehetővé teszi, hogy R modellek toobe közzétenni, és az Azure gépi tanulás webszolgáltatás felhasználni.  Lásd: hello ["Azok a modell"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) szakasza a ["Áttekintése az R Server a HDInsight"](hdinsight-hadoop-r-server-overview.md) további információt a cikk.
* Linux-függőségeinek hello [top 100 legnépszerűbb R csomagok](https://github.com/metacran/cranlogs) -e Linux csomagfüggőségek előre telepítve vannak.
* Beállítás toouse hello CRAN tárház R hozzáadásakor toohello adatcsomópontokat csomagok. További információkért lásd: ["Az első lépéseiben R Server on HDInsight"](hdinsight-hadoop-r-server-get-started.md).
* R Server-fürtök létrehozásakor kiépítés továbbfejlesztett hello megbízhatóságát.

## <a name="notes-for-08012016-release-of-hdinsight"></a>A HDInsight 08/01/2016 kibocsátási megjegyzései
Ebben a kiadásban teljes verziószámai hello Linux-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build | Ambari Build |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8028416 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8028416 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8053402 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Ebben a kiadásban teljes verziószámai hello Windows-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1005.2488842 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1005.2488842 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1005.2488842 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1005.2488842 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1005.2488842 |2.3 |2.3.3.1-25 |

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például a Spark, Hadoop, HBase vagy Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| Módosítások tooHDInsight 3.4 fürtök |hello alapértelmezett értékeket a következő hive konfigurációk megváltoztak a jobb teljesítmény <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul> |Szolgáltatás |Összes |N/A |
| Ebben a kiadásban szereplő következő javítások |HIVE-13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933 |Szolgáltatás |Összes |N/A |

## <a name="notes-for-07142016-release-of-hdinsight"></a>A HDInsight 07/14/2016 kibocsátási megjegyzései
Ebben a kiadásban teljes verziószámai hello Linux-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build | Ambari Build |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7932505 |2.2 |2.2.9.1-11 |2.2.1.12-2 |
| 3.3 |3.3.1000.0.7932505 |2.3 |2.3.3.1-18 |2.2.1.12-2 |
| 3.4 |3.4.1000.0.7933003 |2.4 |2.4.2.0 |2.2.1.12-2 |

Ebben a kiadásban teljes verziószámai hello Windows-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build |
| --- | --- | --- | --- |
| 2.1 |2.1.10.989.2441725 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.989.2441725 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.989.2441725 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.989.2441725 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.989.2441725 |2.3 |2.3.3.1-21 |

## <a name="notes-for-07072016-release-of-hdinsight"></a>A HDInsight 07/07/2016 kibocsátási megjegyzései
Ebben a kiadásban teljes verziószámai hello Linux-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build |
| --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7864996 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.1000.0.7864996 |2.3 |2.3.3.1-18 |
| 3.4 |3.4.1000.0.7861906 |2.4 |2.4.2.0 |

Ebben a kiadásban teljes verziószámai hello Windows-alapú HDInsight-fürtök telepítve:

| HDI | Fürt HDI-verziója | HDP | HDP Build |
| --- | --- | --- | --- |
| 2.1 |2.1.10.977.2413853 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.977.2413853 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.977.2413853 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.977.2413853 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.977.2413853 |2.3 |2.3.3.1-21 |

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például a Spark, Hadoop, HBase vagy Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| [A HDInsight Tools for IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) |IntelliJ IDEA HDInsight Spark-fürtök beépülő modulja most integrálva van Azure-eszközkészlet az intellij-t. Azure SDK v2.9.1, legújabb Java SDK-k, támogatja, és az IntelliJ HDInsight beépülő modul hello önálló összes hello szolgáltatást tartalmaz. |Eszközök |Spark |N/A |
| [A HDInsight Tools for eclipse-ben](hdinsight-apache-spark-eclipse-tool-plugin.md) |Azure eszköztára eclipse-ben mostantól támogatja a HDInsight Spark-fürtjei. A következő funkciók hello lehetővé teszi: <ul><li>Hozzon létre és Spark-alkalmazások írásával könnyen Scala és Java első osztállyal szerzői támogatja az IntelliSense, automatikus formázás hibaellenőrzés, stb.</li><li>Hello Spark alkalmazás helyi teszteléséhez.</li><li>Küldje el a feladatok tooHDInsight Spark-fürt, majd hello eredmények beolvasásához.</li><li>Jelentkezzen be tooAzure, és az Azure-előfizetésekkel társított összes hello Spark fürt eléréséhez.</li><li>Keresse meg a HDInsight Spark-fürt összes hello tartozó tárolási erőforrások.</li></ul> |Eszközök |Spark |N/A |

Jelen kiadásával kezdődően módosítottuk hello vendég operációs rendszer javítási házirend a Linux-alapú HDInsight-fürtökön. hello hello új házirend célja toosignificantly hello újraindítások számának csökkentése miatt toopatching. hello új házirend javítások virtuális gépek (VM) Linux clusters minden hétfőn vagy csütörtök 12 óra UTC lépcsőzetes módon kezdődő bármely adott fürt csomópontjai között. A megadott virtuális gép csak legfeljebb 30 naponta egyszer tooguest OS javítását miatt újraindul. Ezenkívül hello első újraindítás, újonnan létrehozott fürt nem fordulhat elő, gyakoribb 30 napon hello fürt létrehozását.

> [!NOTE]
> A módosítások csak toonewly fürtöket létrehozni, egyenlő vagy nagyobb, mint a verzió érvényesek.
>
>

## <a name="notes-for-06062016-release-of-hdinsight"></a>A HDInsight 06/06/2016 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

| HDP | HDI-verziója | A Spark-verzió | Ambari buildszám | HDP buildszám |
| --- | --- | --- | --- | --- |
| 2.3 |3.3.1000.0.7702215 |1.5.2 |2.2.1.8-2 |2.3.3.1-18 |
| 2.4 |3.4.1000.0.7702224 |1.6.1 |2.2.1.8-2 |2.4.2.0 |

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például a Spark, Hadoop, HBase vagy Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A Spark on HDInsight az általánosan elérhető |Ebben a kiadásban a rendelkezésre állási, a méretezhetőség és a termelékenység tooopen forrás Apache Spark on HDInsight fejlesztései elérését. <ul><li>Iparágvezető rendelkezésre állási szolgáltatásiszint-szerződésben garantált 99,9 %-os épp ezért kiválóan alkalmas a vállalati munkaterhelések igényelnek.</li><li>Méretezhető tárolás réteg Azure Data Lake Store használatára.</li><li>Az adatok feltárása és fejlesztési minden fázisában termelékenységi eszközöket. Testre szabott Spark kernel használt Jupyter notebookokban adatok interaktív áttekintését, integráció BI-irányítópult például a Power bi-ban, a Tableau és Qlik kiválóan alkalmas adatok gyors megosztása és folyamatos reporting engedélyezéséhez az IntelliJ beépülő modul a beállítás a megbízható a hosszú távú kódot összetevő fejlesztési és hibakeresés folyamatát.</li></ul> |Szolgáltatás |Spark |N/A |
| A HDInsight Tools for IntelliJ |Ez az az IntelliJ IDEA HDInsight Spark-fürtök beépülő modul. A következő funkciók hello lehetővé teszi:<ul><li>Hozzon létre és Spark-alkalmazások írásával könnyen Scala és Java első osztállyal szerzői támogatja az IntelliSense, automatikus formázás hibaellenőrzés, stb.</li><li>Hello Spark alkalmazás helyi teszteléséhez.</li><li>Küldje el a feladatok tooHDInsight Spark-fürt, majd hello eredmények beolvasásához.</li><li>Jelentkezzen be tooAzure, és az Azure-előfizetésekkel társított összes hello Spark fürt eléréséhez.</li><li>Keresse meg a HDInsight Spark-fürt összes hello tartozó tárolási erőforrások.</li><li>Keresse meg az összes hello feladatok előzményekhez és a feladat információi a HDInsight Spark-fürt.</li><li>A hibakeresési Spark feladatok távoli asztali számítógépről.</li></ul> |Eszközök |Spark |N/A |

## <a name="notes-for-05132016-release-of-hdinsight"></a>A HDInsight 05/13/2016 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például a Spark, Hadoop, HBase vagy Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| Spark verziófrissítés és más hibajavítások |Ebben a kiadásban hello Spark HDInsight-fürt too1.6.1 verzióját frissíti, és kijavítja a más hibák |Szolgáltatás |Spark |N/A |

## <a name="notes-for-04112016-release-of-hdinsight"></a>A HDInsight 04/11/2016 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-változatlan)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| Egyéni metaadattárhoz frissítést ad ki a HDI 3.4 |A fürt létrehozása sikertelen lesz, ha egy egyéni metaadattárhoz, amely egy másik HDInsight-fürt régebbi verziója a korábban használt. Ez volt a tooan frissítési parancsfájlhiba most már javított határideje |A fürt létrehozása |Összes |N/A |
| Livy összeomlás utáni helyreállítást |Feladat állapota rugalmasságot biztosít a Livy keresztül feladathoz szükséges |Megbízhatóság |A Spark on Linux |N/A |
| Jupyter tartalom magas rendelkezésre ÁLLÁSÚ |Itt hello képességét toosave és a Jupyter notebook tartalma tooand hello-fürthöz tartozó hello tárfiókból. További információkért lásd: [Jupyter notebookok elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Notebookok |A Spark on Linux |N/A |
| A Jupyter notebookok hiveContext eltávolítása |Használjon `%%sql` magic helyett `%%hive` magic. Az SqlContext egyenértékű toohiveContext. További információkért lásd: [Jupyter notebookok elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md) |Notebookok |A Spark-fürtök Linux rendszeren |N/A |
| A Spark régebbi verziók érvénytelenítése |Régebbi verziójú Spark 1.3.1 el lett távolítva a hello szolgáltatásból, 5/31 |Szolgáltatás |A Spark-fürtök Windows rendszeren |N/A |

## <a name="notes-for-03292016-release-of-hdinsight"></a>A HDInsight 03/29/2016 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - változatlan)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - változatlan)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - változatlan)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A hozzáadott HDInsight 3.4 frissített HDP verziójához és a HDInsight-fürtök esetében |Ezzel a kiadással azt hozzáadta a HDInsight v3.4 (HDP 2.4 alapján) és más HDP verzióit is frissítése befejeződött. Kibocsátási megjegyzések HDP 2.4 érhetők el [Itt](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) és a HDInsight-verziókról további információt talál [Itt](hdinsight-component-versioning.md). |Szolgáltatás |Az összes Linux-fürtök |N/A |
| HDInsight prémium |A HDInsight - Standard és Premium két kategóriába most érhető el. HDInsight prémium jelenleg előzetes verzióban érhetők, és csak a Hadoop és Spark-fürtök Linux rendszeren. További információkért lásd: [Itt](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium). |Szolgáltatás |Hadoop- és Spark Linux rendszeren |N/A |
| Microsoft R Server |HDInsight prémium Microsoft R Server, a Hadoop és a Spark-fürtök a Linuxon a ágyazhatóak biztosít. További információkért lásd: [áttekintése az R Server, a HDInsight](hdinsight-hadoop-r-server-overview.md). |Szolgáltatás |Hadoop- és Spark Linux rendszeren |N/A |
| Spark 1.6.0 |HDInsight 3.4 fürtök között már elérhető a Spark 1.6.0 |Szolgáltatás |A Spark-fürtök Linux rendszeren |N/A |
| Jupyter notebook fejlesztései |Jupyter notebookok érhető el a Spark-fürtök mostantól elérhető egy további Spark mag. Is, például használatát fejlesztések %% magic, automatikus a képi megjelenítés és Python képi megjelenítés függvénytárak (például matplotlib) integrációját. További információkért lásd: [Jupyter notebookok elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Szolgáltatás |A Spark-fürtök Linux rendszeren |N/A |

## <a name="notes-for-03222016-release-of-hdinsight"></a>2016. 03/22-es kiadása a HDInsight megjegyzések
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - változatlan)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - változatlan)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - változatlan)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A HDInsight-fürtök esetében a frissített HDInsight-verziókról |Ezzel a kiadással frissítettük a HDInsight-fürtök esetében a HDInsight-verziókról |Szolgáltatás |Összes |N/A |

## <a name="notes-for-03102016-release-of-hdinsight"></a>A HDInsight 03/10/2016 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 - változatlan)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A HDInsight-fürtök esetében a frissített HDInsight-verziókról |Ezzel a kiadással frissítettük a HDInsight-fürtök esetében a HDInsight-verziókról |Szolgáltatás |Összes |N/A |

## <a name="notes-for-01272016-release-of-hdinsight"></a>A HDInsight 01/27/2016 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 - változatlan)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A HDInsight-fürtök esetében a frissített HDInsight-verziókról |Ezzel a kiadással frissítettük a HDInsight-fürtök esetében a HDInsight-verziókról |Szolgáltatás |Összes |N/A |

## <a name="notes-for-12022015-release-of-hdinsight"></a>A HDInsight a 12/02/2015 kiadási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - változatlan)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - változatlan)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A hozzáadott HDInsight 3.3 frissített HDP verziójához és a HDInsight-fürtök esetében |Ezzel a kiadással azt hozzáadta a HDInsight v3.3 (HDP 2.3 alapján) és más HDP verzióit is frissítése befejeződött. Kibocsátási megjegyzések HDP 2.3 érhetők el [Itt](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) és a HDInsight-verziókról további információt talál [Itt](hdinsight-component-versioning.md). |Szolgáltatás |Összes |N/A |

## <a name="notes-for-11302015-release-of-hdinsight"></a>Megjegyzések a HDInsight 11/30/2015 kiadása
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A HDInsight-fürtök és a HDInsight 3.2 fürtök (a Windows és Linux) verzióinak HDP frissített HDInsight-verziókról |Ezzel a kiadással HDInsight és HDP verziók frissítése megtörtént |Szolgáltatás |Összes |N/A |

## <a name="notes-for-10272015-release-of-hdinsight"></a>A HDInsight 10/27/2015 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - változatlan)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| Frissített HDInsight-verziókról az összes HDInsight-fürtök (a Windows és Linux) |Ezzel a kiadással HDInsight és HDP verziók frissítése megtörtént |Szolgáltatás |Összes |N/A |
| Nagybetűvel fürtökkel rögzített Jupyter a Windows a Spark-fürtök |Fürtökben, amelyek a DNS-nevek megadott nagybetűvel kellett volna problémái miatt tooan eredeti kérelem ellenőrzése Jupyter notebookok. hello javítás toochange hello DNS-név Jupyter tartozó konfigurációs toolower esethez lett. |Szolgáltatás |A HDInsight Spark (Windows) |N/A |

## <a name="notes-for-10202015-release-of-hdinsight"></a>A HDInsight 10/20/2015 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| Alapértelmezett HDP verziója megváltozott tooHDP 2.2 |hello alapértelmezett HDInsight Windows-fürtök verziója megváltozott tooHDP 2.2. A HDInsight 3.2-es (HDP 2.2) verziója már általában 2015. február óta. Ez a módosítás csak tükrözi hello alapértelmezett fürt verziószáma, amikor egy explicit kiválasztás nem került sor a hello Azure-portálon, a PowerShell-parancsmagokkal vagy a hello SDK hello fürt kiépítése során. |Szolgáltatás |Összes |N/A |
| A virtuális gép nevének formátuma több HDInsight Linux fürtökön egyetlen virtuális hálózatban telepítéséhez módosítások |Ebben a kiadásban hozzáadott egyetlen virtuális hálózatban több HDInsight Linux-fürtök telepítésének támogatása. Frissítés részeként hello fürtben lévő virtuális gépek neveit hello formátuma megváltozott headnode\*, workernode\* és zookeepernode\* toohn\*, lefelé\*, és zk\* kulcsattribútumokkal. Nincs olyan ajánlott gyakorlat tootake közvetlen függőség hello formátumának a virtuális gép nevét, mivel ez tulajdonos toochange. "Állomásnév -f" hello helyi számítógép vagy az Ambari API-k toodetermine hello listáját gazdagépek és az összetevők toohosts hello leképezése használja. További információ: található [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) és [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). |Szolgáltatás |A HDInsight-fürtök Linux rendszeren |N/A |
| Konfigurációs változások |HDInsight 3.1-fürtök esetén a következő konfigurációk hello már támogatják: <ul><li>tez.yarn.ATS.Enabled és yarn.log.server.url. Ez lehetővé teszi a hello ütemterv alkalmazáskiszolgáló és hello napló server toobe képes tooserve el naplók.</li></ul>A HDInsight 3.2 fürtök esetén a következő konfigurációk hello módosítva lett: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version too2 van beállítva. Ez lehetővé teszi a hello FileOutputCommitter hello V2 verzió használatát.</li></ul> |Szolgáltatás |Összes |N/A |

## <a name="notes-for-09092015-release-of-hdinsight"></a>A HDInsight 09/09/2015 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - változatlan)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - változatlan)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A HDInsight-fürtök esetében a frissített HDInsight-verziókról |Ezzel a kiadással a HDInsight-verziókról frissítve lett |Szolgáltatás |Összes |N/A |

## <a name="notes-for-07312015-release-of-hdinsight"></a>A HDInsight 07/31/2015 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - változatlan)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - változatlan)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| Javítsa ki a Spark fürt csomópont különösen munkafolyamat |Egy hiba okozta Spark toonot helyreállítása után a lemezkép-visszaállítási fürtcsomópontok, rögzített |Szolgáltatás |Spark |N/A |

## <a name="notes-for-07312015-release-of-hdinsight"></a>A HDInsight 07/31/2015 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - változatlan)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - változatlan)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A HDInsight-fürtök esetében a frissített HDInsight-verziókról |Ezzel a kiadással a HDInsight-verziókról frissítve lett |Szolgáltatás |Összes |N/A |

## <a name="notes-for-07072015-release-of-hdinsight"></a>Megjegyzések a HDInsight 07/07/2015 kiadása
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - változatlan)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

| Cím | Leírás | Érintett területen (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase, vagy a Storm) | JIRA (ha van ilyen) |
| --- | --- | --- | --- | --- |
| A HDInsight 3.2 fürtök frissített HDP verziók |Ebben a kiadásban a HDInsight 3.2-es verzióját telepíti a HDP 2.2.6.1-0012 |Szolgáltatás |Összes |N/A |

## <a name="notes-for-06262015-release-of-hdinsight"></a>Megjegyzések a HDInsight 2015-06/26 kiadása
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - változatlan)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>A HDInsight 3.2 fürtök frissített HDP verziók</td>
<td>Ebben a kiadásban a HDInsight 3.2-es verzióját telepíti a HDP 2.2.6.1</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Megjegyzések a HDInsight 2015-06/18 kiadása
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>További HTTPS-portok megnyitása</td>
<td>hello felhőalapú szolgáltatás most 5 portok 8001 too8005 hello fürtön például megnyitása a https://<clustername>.azurehdinsight.net:8001/. Kérelmek toothese URL-címek hitelesítése az hello ugyanazt az egyszerű hitelesítés jelszó mechanizmusnak a 443-as portot. Ezeket a portokat azonos portjához hello aktív headnode toohello kötni. A Parancsfájlműveletek lehet használt toomake ügyfél szolgáltatások figyelés hello headnode és útvonal toooutside hello fürt ezeket a portokat.</td>
<td>A felhőalapú szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>A HDInsight 3.2 időszakos MapReduce sorrendű probléma</td>
<td>Hárítsa el a ritka, időszakos versenyhelyzet térkép csökkentése sorrendű alkalmi feladat hibáihoz eredményezve nagy fürtjein. Lásd: <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a> további információt.</td>
<td>Hadoop-Core</td>
<td>Összes</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>
<tr>
<td>A HDInsight 3.2 tooLatest Azure Java SDK 2.2 áthelyezése</td>
<td>Áthelyezett toolatest hello Azure SDK for Java hello WASB illesztőprogram által használt verzióját. hello SDK legújabb rendelkezik néhány javításokat, és hello kibocsátási megjegyzések a hello éppen https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt érhető el.</td>
<td>Hadoop-Core</td>
<td>Összes</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP-11959</a></td>
</tr>
<tr>
<td>Helyezze át a tooHDP 2.1.15 HDInsight 3.1-fürtök</td>
<td>Hello kiadás kibocsátási megjegyzései Hortonworks érhetők el <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">Itt</a>.</td>
<td>HDP</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>A HDInsight 06/04/2015 kiadási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - változatlan)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Javítsa ki a Storm-fürtök 502 Hibás átjáró hiba</td>
<td>Ebben a kiadásban hello feladat elküldése API okozó hello webhely toobe le a rendszer újraindítása után érintő programhiba javítja.</td>
<td>Szolgáltatás</td>
<td>Storm</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Megjegyzések a HDInsight 2015-06/01 kiadása
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - változatlan))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - változatlan)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Különböző hibajavítások</td>
<td>Ebben a kiadásban hibákat javít toocluster kiépítés kapcsolatos.</td>
<td>Szolgáltatás</td>
<td>Összes fürt típusa</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>A HDInsight 2015-05/27 kiadási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Más fürt verziók és az SDK nem telepített ezen kiadásában.

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>HDP 2.2-es frissítés</td>
<td>Ebben a kiadásban a HDInsight 3.2 HDP 2.2.6 tartalmaz, és számos fontos hibajavítások tooHDInsight elérését. hello teljes kibocsátási megjegyzések érhetők el <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 kibocsátási megjegyzéseit</a>.</td>
<td>HDP</td>
<td>Összes fürt típusa</td>
<td>N/A</td>
</tr>
<tr>
<td>TooDefault Yarn tároló memória konfigurációjának módosítása</td>
<td>Ez a frissítés hello alapértelmezett rendelkezésre álló memória tooYARN tárolók (yarn.nodemanager.resource.memory MB-os és yarn.scheduler.maximum-foglalási-mb), csomópont-kezelő által elindított, nagyobb too5632MB. Csökkentett too4608MB volt korábban, de alapján különböző feladat fut, új érték hello kell nyújtania jobb megbízhatóságot és teljesítményt toomost feladatok, ezért a jobb alapértelmezett. A szokásos módon ha kritikus függőségi viszonyban van ez a memória-konfiguráció, állíthatja ezt be explicit módon hello fürt létrehozása során.</td>
<td>HDP</td>
<td>Összes fürt típusa</td>
<td>N/A</td>
</tr>
<tr>
<td>Alapértelmezett konfiguráció paritásos HBase és a Storm-fürtök</td>
<td>A frissítés visszaállítja a Hbase és a Storm-fürtök toouse hello Hadoop-fürtök a YARN configs azonos értékeket. Ez történik, a paritásos összes fürt típusa.</td>
<td>HDP</td>
<td>HBase, Storm</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>A HDInsight 2015-05/20 kiadási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>SCP.NET EventHub-támogatás</td>
<td>frissített hello fürt csomagok HDInsight alatt futó Storm kapcsolja a új szolgáltatások tooSCP.NET. Most már rendelkezik hozzáférési toonew API-k a topológia builderben, amelyekkel könnyebben toouse EventHubSpout vagy Java Spoutok. Frissítenie kell a SCP.NET ügyfél SDK toowork rendelkező új fürtök, hello szerződések frissítve lett. Új API-k, a használat és a kibocsátási megjegyzéseket (beleértve a hibajavításokat tartalmaz) hello kapcsolatos részletekért tekintse meg a toohello információs hello SCP.NET NuGet-csomagot tartalmazza.</td>
<td>VS tooling eszköz</td>
<td>A HDInsight 3.2 Storm-fürtök</td>
<td>N/A</td>
</tr>
<tr>
<td>JDBC illesztőprogram frissítése</td>
<td>Frissített hello illesztőprogram toohello SQL Server támogatott verziója a sqljdbc_4.1.5605.100.</td>
<td>Metaadattárhoz</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>A HDInsight 2015-04/27 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - változatlan)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Javítsa ki a kódtárat</td>
<td>Eltávolítja a HDInsight függőséget egység teszt keretrendszerre.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>A versenyhelyzet hibajavítás</td>
<td>Egy fürt most vár a PUT kérelmek toobe előtt a hello állapot lekérdezési kérelem létrehozása</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>A HDInsight 2015-04/14 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - változatlan)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Tez hibajavítások</td>
<td>Az Apache TEZ 2214 és TEZ 1923 javítások ezen kiadásával a HDI 3.2 szerepelnek. Ezek azonban bizonyos Hive-lekérdezéseket a Tez tooshuffle jelentős mennyiségű adat igénylő szükségesek.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>A HDInsight 2015-04/06 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - változatlan)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - változatlan)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - változatlan)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>A HDInsight .NET SDK 1.5.6</td>
<td>Néhány belső osztályok az tooremove frissíti a HDInsight Linux rendszeren.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>Az Avro könyvtár 1.5.6</td>
<td>Hozzáadott <b>KnownTypeAttribute</b> metódus <b>GetAllKnownTypes</b>. Rögzített NullReferenceException GetAllKnownTypes metódus null értékű típus esetén.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>Hibajavítások</td>
<td>Különböző hibajavítások toohello szolgáltatás</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04012015-release-of-hdinsight"></a>A HDInsight 2015-04/01 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Képes tooenable vagy letiltását távoli asztali hitelesítő adatok a Windows-fürtök .NET SDK használatával</td>
<td>Programozott támogatás engedélyezése vagy letiltása a Windows-fürtök RDP-felhasználó hitelesítő adatai.</td>
<td>SDK</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>Képes tooenable távoli asztali hitelesítő adatok fürtökön közben kiépítésüket folyamatban</td>
<td>Programozott támogatás engedélyezéséhez a távoli asztali hitelesítő adatok hello fürt létrehozását. Ezzel eltávolítja a hello kétlépéses folyamat első kiosztás hello fürt-vagy majd engedélyezni kívánja a távoli asztalt.</td>
<td>SDK</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>Frissített Python too2.7.8</td>
<td>A HDInsight-fürtök tooPython 2.7.8, a frissített Python, amely tartalmaz néhány fontos biztonsági javítások 2.1, 3.0-s, 3.1 és 3.2 HDInsight-verziókról</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>YARN konfiguráció módosítása</td>
<td>Módosított YARN konfigurációs yarn.resourcemanager.max befejeződött alkalmazások too1000 minden fürt esetében a HDInsight-verziókról 3.1 és 3.2-es verzióját. Ez az érték csak szabályozza hello a kész alkalmazások listájában hello YARN felhasználói felületen. tooget, melyeket alkalmazásokkal kapcsolatos információkat előzetes toohello közvetlenül elvégezheti a toohello előzmények kiszolgáló látható hello UI, az alkalmazások listáját.</td>
<td>YARN</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>A HBase-fürtöt a csomópontok átméretezése</td>
<td>HBase-fürtökkel most engedélyezése a HDInsight-verziókról 3.1 és 3.2 (felfelé és lefelé) csomópontok átméretezése</td>
<td>Szolgáltatás</td>
<td>HBase</td>
<td>N/A</td>
</tr>
<tr>
<td>JDBC frissítése</td>
<td>SQL JDBC-illesztőt frissített tooversion sqljdbc_4.0.2206.100 a HDInsight 3.2-es verziója. Ezen verziója továbbfejlesztett fontos biztonsági.</td>
<td>HDP</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>JVM konfiguráció frissítése</td>
<td>Frissített JVM konfigurációs networkaddress.cache.ttl too300 másodpercig hello alapértelmezett -1 érték a HDInsight-verziókról 3.1 és 3.2-es verzióját. Ez a konfigurációs érték hello gyorsítótárazási sikeres neve keresések hello szolgáltatást a házirend szabályozza. Ez a hiba megoldása kapcsolódó toogrowing és zsugorítását HBase-fürtökkel.</td>
<td>Szolgáltatás</td>
<td>HBase</td>
<td>N/A</td>
</tr>
<tr>
<td>Frissítési tooAzure Storage SDK for Java 2.0</td>
<td>A HDInsight 3.2-es verziójú frissített toouse hello legújabb verziójára hello Azure Storage SDK for Java. Ez számos fontos hibajavítást tartalmaz hello aktuális 0.6.0 verzióra.</td>
<td>HDP</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>Frissített tooApache WASB forráskód</td>
<td>HDInsight most használ hello hello WASB fájlrendszer illesztőprogram az Apache Hadoop legújabb kódját 3.2-es verziója. Ez a változás a hello WASB illesztőprogram most csomagolt, egy külön jar. Ez kizárólag csomagban módosítva lett, és nem tartalmaz semmilyen módosítások toobehavior hello WASB illesztőprogram. hello neve a JAR-fájlra a hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>A HDInsight 3.2 fájlnév frissítések JAR</td>
<td>A frissítés tooHDInsight 3.2-es verziójú számos hibajavítást tartalmaz, és néhány belső JAR-fájlok kivételével HDP részeként csomagolt frissítettek. Ezek JAR filesare belső toohello HDP csomag közvetlenül is felhasználható vevő alkalmazások esetében nem. Alkalmazások kell Csomagverzió saját hello JAR-fájlok kivételével az, hogy egy frissítési toohello belső HDP JARs törik alkalmazások vevői.</td>
<td>HDP</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-03032015-release-of-hdinsight"></a>A HDInsight 2015-03/03 kiadási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - változatlan)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - változatlan)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - változatlan)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - változatlan)
* SDK 1.5.0 (változatlan)

Ebben a kiadásban a következő frissítés hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA</th>
</tr>
<tr>
<td>Megbízhatóság fejlesztései</td>
<td>A Microsoft végrehajtott javítások, amelyek lehetővé teszik a megnövekedett terhelés hello tekintetben toocluster létrehozásával jobb hello szolgáltatás tooscale.</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-02182015-release-of-hdinsight"></a>A HDInsight 2015-02/18 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - változatlan)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - változatlan)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - változatlan)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* SDK 1.5.0

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>A HDInsight 3.2 fürtök</td>
<td>Hadoop 2.6/HDP2.2 HDInsight 3.2 fürtökkel érhető el. Frissítése tooall hello nyílt forráskódú összetevőt tartalmaz. További információkért lásd: What's new in HDInsight és <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 kibocsátási megjegyzések</a>.</td>
<td>Nyílt forráskódú szoftvereket</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>HDInsight Linux (előzetes verzió)</td>
<td>Ubuntu Linux rendszerű fürtök helyezhető üzembe. További információkért lásd: Get Started with HDInsight Linux rendszeren.</td>
<td>Szolgáltatás</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>A Storm általános rendelkezésre állása</td>
<td>Apache Storm-fürtök általában elérhetők. További információkért lásd: első lépései a HDInsight alatt futó Storm használatával.</td>
<td>Szolgáltatás</td>
<td>Storm</td>
<td>N/A</td>
</tr>
<tr>
<td>Virtuálisgép-méretek</td>
<td>Az Azure HDInsight áll rendelkezésre több virtuális gép típusát és méretét. HDInsight használhatja az általános célra; készült A2 tooA7 mérete Amely a beállítást, SSD-meghajtót (SSD) és 60 százalékkal gyorsabb processzorokkal ellátott D sorozatú csomópontokat és támogatási hálózatkezelést InfiniBand A8 és A9 méreteket.</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>A fürtméretezés</td>
<td>Módosítsa egy futtatott HDInsight-fürt csomópontjainak adatok hello számát toodelete nélkül, vagy hozza létre újból. Jelenleg csak a Hadoop lekérdezés és Apache Storm fürttípusok, hogy a számítógép, de az Apache HBase fürt típusa hamarosan toofollow támogatás. További információkért lásd: kezelése a HDInsight-fürtök.</td>
<td>Szolgáltatás</td>
<td>Hadoop-, Storm</td>
<td>N/A</td>
</tr>
<tr>
<td>A Visual Studio eszközt használunk erre</td>
<td>Apache Storm, Apache Hive Visual Studio szükséges hello szükséges toocomplete lett továbbá frissített tooinclude utasítások kiegészítése, helyi érvényesítés és hibakeresési továbbfejlesztett támogatást. További információkért lásd: Ismerkedés a HDInsight Hadoop-eszközök Visual Studio.</td>
<td>Tooling eszköz</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<td>Az Azure Cosmos DB Hadoop-összekötő</td>
<td>Hadoop-összekötővel Azure Cosmos DB végezheti el összetett összesítéseket, elemzés és feldolgozás alatt a séma nélküli JSON-dokumentumok, tárolt Azure Cosmos DB gyűjtemények vagy az adatbázis-fiókokat. További információt és oktatóanyag: Azure Cosmos DB és HDInsight használatával futtassa Hadoop-feladatokat.</td>
<td>Szolgáltatás</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>Hibajavítások</td>
<td>A HDInsight-szolgáltatások különböző kisebb hibák javításával hajtottunk. Módosítások nélküli ügyfélkapcsolati viselkedés várható.</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-02062015-release-of-hdinsight"></a>A HDInsight 2015-02/06 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - változatlan)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - változatlan)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK N/A

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Hibajavítások</td>
<td>A HDInsight-szolgáltatások különböző kisebb hibák javításával hajtottunk. Módosítások nélküli ügyfélkapcsolati viselkedés várható.</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>HDP 2.1-es karbantartási frissítés</td>
<td>HDInsight 3.1 frissített toodeploy HDP 2.1.10.0. További információkért lásd: <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">kibocsátási megjegyzések HDP-2.1.10</a>. </td>
<td>Nyílt forráskódú szoftvereket</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>A bináris frissítések HDP</td>
<td>Néhány JAR-fájlok nincsenek HBase, mely fájl nevének frissültek. A JAR-fájlok vannak belsőleg HBase, így nem valószínű, hogy az ügyfelek rendelkezzen függőség hello nevek a JAR-fájlok. Ezek a következők:
<ul>
<li>./lib/jetty-6.1.26.hwx.JAR</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.JAR</li>
<li>./lib/jetty-Util-6.1.26.hwx.JAR</li>
</ul>
</td>
<td>Nyílt forráskódú szoftvereket</td>
<td>HBase</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-1292015-release-of-hdinsight"></a>A HDInsight 1/29/2015 kibocsátási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - változatlan)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - változatlan)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - változatlan)
* SDK N/A

Ebben a kiadásban a következő frissítés hello tartalmazza:

<table border="1">

<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Érintett területen (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase, vagy a Storm)</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Hibajavítások</td>
<td>Néhány fontos hibajavításokat tartalmaz, amelyek javítják a HDInsight-fürtök hello megbízhatóságának hello Azure frissítéskor hajtottunk.</td>
<td>Szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-152015-release-of-hdinsight"></a>A HDInsight 1/5/2015 kiadási megjegyzései
hello teljes verziószámai HDInsight-fürtök ebben a kiadásban:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - változatlan)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - változatlan)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - változatlan)

Ebben a kiadásban a következő frissítések hello tartalmazza:

<table border="1">

<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Twitter trendelemzés és Mahout-alapú movie javaslatok minták</td>
<td><p>Ebben a kiadásban hello HDInsight lekérdezés konzol két további minták rendelkezik:</p>

<p><b>Twitter trendelemzés</b><br>
Nyilvános API-k, például a Twitter helyek által biztosított az hasznos adatforrást ismertetése népszerű trendeket és elemzésére. Ebből az oktatóanyagból megtudhatja, hogyan toouse Hive tooget küldött hello legtöbb Twitter-üzeneteket tartalmazó egy adott word Twitter-felhasználók listáját. </p>

<p><b>Mahout Movie javaslat</b><br>
Apache Mahout egy Apache Hadoop machine learning-könyvtárral. Mahout algoritmusok (például a szűrést, besorolást és a fürtszolgáltatás) adatok feldolgozására tartalmazza. Ebben az oktatóanyagban egy javaslat motor toogenerate movie meg ajánlásainkat az ismerősök láthatta filmek használja.</p></td>
<td>Lekérdezés-konzol</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>Hive konfigurációs toodefault értékének módosításához: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>A méretre vonatkozó konfigurációja konvertálni tooautomatically térkép illesztések vonatkozik. hello érték azt jelenti, hogy olyan táblázatot, amely lehet, hogy a memóriában konvertált toohash maps hello méretek hello összege. Az előzetes kiadásban ez az érték hello alapértelmezett érték 10 MB too128 MB nőtt. Azonban az új érték hello 128 MB okozta feladatok toofail esedékes toolack memória. Ebben a kiadásban helyreállítja hello alapértelmezett érték biztonsági too10 MB. Az ügyfelek továbbra is választhat toooverride ennek az értéknek fürt létrehozásakor, a lekérdezések és a tábla méretét. Ezzel a beállítással kapcsolatban további információt, és hogyan toooverride, lásd: <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">automatikus csatlakozás átalakítás optimalizálása</a> Hortonworks dokumentációjában. </p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-12232014-release-of-hdinsight"></a>A HDInsight a 12/23/2014 kiadási megjegyzései
teljes verziószámai hello HDInsight-fürtök ebben a kiadásban a következők:

* HDInsight 2.1.10.420.1246783 (HDP változatlan verzió)
* HDInsight 3.0.6.420.1246783 (HDP változatlan verzió)
* HDInsight 3.1.1.420.1246783 (HDP változatlan verzió)

Ebben a kiadásban a következő frissítés hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Tooexcessive terhelés miatt hibák időszakos fürt létrehozása során</td>
<td><p>Továbbfejlesztett algoritmus HDP csomagok letöltéséhez a fürt létrehozása során lehetővé teszi a hibák tooexcessive terhelés miatt robusztusabb kezelését.</p></td>
<td>Szolgáltatás</td>
<td>Hadoop, Hbase, Storm</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-12182014-release-of-hdinsight"></a>A HDInsight a 12/18/2014 kiadási megjegyzései
Ebben a kiadásban a következő összetevő frissítése hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Fürt testreszabási általánosan rendelkezésre álló</a></td>
<td><p>Testreszabási segítségével hello meg az Azure HDInsight-fürtök hello Apache Hadoop ökoszisztémájának elérhető projektek toocustomize. Az új szolgáltatással kísérletezhet, és a Hadoop-projektek tooAzure HDInsight telepítése. Ez az hello keresztül engedélyezhető **parancsfájlművelet** szolgáltatás, amely a Hadoop-fürtök módosíthatja a tetszőleges módon egyéni parancsfájlok használatával. Ez a lehetőség érhető el, beleértve a Hadoop, HBase és a Storm a HDInsight-fürtök összes típusú. toodemonstrate hello power ezt a képességet, hogy rendelkezik dokumentált hello folyamat tooinstall hello népszerű <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>, és <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> modulok. Ebben a kiadásban is ügyfelek toospecify hello funkció hozzáadja az Egyéni parancsfájlművelet hello Azure-portálon keresztül, irányelvek és bevált gyakorlatok hogyan toobuild egyéni parancsfájl-műveletek segítő módszerekkel kapcsolatos biztosít, és kapcsolatos útmutatásokat tootest hello parancsfájlművelet. </p></td>
<td>A szolgáltatás általános rendelkezésre állása</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-12052014-release-of-hdinsight"></a>A HDInsight a 12/05/2014 kiadási megjegyzései
teljes verziószámai hello HDInsight-fürtök ebben a kiadásban a következők:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK N/A

Ebben a kiadásban a következő összetevő-frissítések hello tartalmazza:

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van ilyen)</th>
</tr>
<tr>
<td>Hibajavítás: átmeneti hiba történt a partíciók tooa tábla nagy számú felvételekor a Hive DDL </td>
<td><p>Ha hello Hive metaadattárhoz adatbázis időszakos kapcsolattal hiba történt partíciók tooa Hive tábla számos hozzáadásakor, hello Hive DDL sikertelen lehet. Ez a hiba akkor fordul elő, ha a következő utasítás isseen hello Hive hibanapló hello: </p><p>"[Fő]. hiba: ql. Illesztőprogram (SessionState.java:printError(547)) – nem sikerült: végrehajtási hiba, a visszatérési kód 1 org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (message:java.lang.RuntimeException: commitTransaction lett meghívva, de openTransactionCalls = 0. Ez valószínűleg azt jelzi, hogy vannak-e egyenetlen hívások tooopenTransaction/commitTransaction)"</p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>HIVE-482 (azt egy belső JIRA, így azt nem szerepelhetnek idézőjelek között kívülről. Az áttelepítés előtt feljegyzett itt referenciaként.)</td>
</tr>
<tr>
<td>Hibajavítás: a HDInsight lekérdezés konzol hello alkalmi lefagy</td>
<td>Ha ez történik, hello következő utasításra látható hello WebHCat napló hello WebHCat indító feladat: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper |} org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): nem sikerült betölteni a előzményfájlban {wasb URL-cím toohello előzményfájlban} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>HIVE-482 (azt egy belső JIRA, így azt nem szerepelhetnek idézőjelek között kívülről. Az áttelepítés előtt feljegyzett itt referenciaként.)</td>
</tr>
<tr>
<td>Hibajavítás: a Hbase lekérdezések késés alkalmi csúcs</td>
<td>Ez akkor fordul elő, ha felhasználók hello késés Hbase lekérdezések egy alkalmi csúcs 3 másodpercenként láthatja. </td>
<td>HDInsight-fürt átjáró</td>
<td>HBase</td>
<td>N/A</td>
</tr>
<tr>
<td>Fájl neve megváltozik HDP JAR</td>
<td>A HDI fürt 3.0-s, ott néhány módosítások toohello belső JAR fájlok HDP által telepített verziója. jetty-6.1.26.jar helyébe a jetty-6.1.26.hwx.jar. jetty-util-6.1.26.jar helyébe a jetty-util-6.1.26.hwx.jar. Ezek a változások tooHadoop, Mahout, WebHCat és az Oozie-projektek alkalmazni.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-11212014-release-of-hdinsight"></a>A HDInsight 11/21/2014 kiadási megjegyzései
teljes verziószámai hello HDInsight-fürtök ebben a kiadásban a következők:

* HDInsight 2.1.9.382.1169709 (nincs módosítás a 11/14/2014)
* HDInsight 3.0.5.382.1169709 (nincs módosítás a 11/14/2014)
* HDInsight 3.1.1.382.1169709 (nincs módosítás a 11/14/2014)
* HDInsight SDK 1.4.0

Ebben a kiadásban a következő összetevő-frissítések hello tartalmazza:

<table border="1">
<tr><th>Cím</th><th>Leírás</th><th>Összetevő</th><th>Fürt típusa</th><th>JIRA (ha van ilyen)</th></tr>
<tr>
<td>Naplók. alkalmazás elérése során.</td>
<td>Képes tooprogrammatically a fürtökön futó alkalmazások és toodownload megfelelő alkalmazás-specifikus vagy a tároló-specifikus naplók toohelp hibakeresési hibás alkalmazásokat számbavétele.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>Képes toospecify régió neve a IHdInsightClient.DeleteCluster </td>
<td>hello Azure HDInsight SDK elnevezi hello képességét toospecify régió használatakor **DeleteCluster**. Ez segít a felhasználóknak, akik erőforrásokkal két azonos nevű különböző régiókban, és már nem toodelete feloldása lehet őket.</td>
<td>SDK</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Hello **ClusterDetails** objektumot ad vissza egy **DeploymentID** hello fürt egyedi azonosítója képviselő mező. Válik biztossá keresztül a fürt létrehozása egyedi toobe kísérletek hello azonos nevek.</td>
<td>SDK</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-11142014-release-of-hdinsight"></a>A HDInsight 11/14/2014 kiadásban megjegyzései

teljes verziószámai hello HDInsight-fürtök ebben a kiadásban a következők:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Ebben a kiadásban a következő új szolgáltatások összetevőinek frissítései és hibajavítások hello tartalmazza.

<table border="1">
<tr><th>Cím</th><th>Leírás</th><th>Összetevő</th><th>Fürt típusa</th><th>JIRA (ha van ilyen)</th></tr>
<tr>
<td>Parancsfájlművelet (előzetes verzió)</td>
<td>Előzetes verziója hello fürt testreszabási szolgáltatás, amely lehetővé teszi, hogy a módosítás hadoop-fürtök tetszőleges módon egyéni parancsfájlok segítségével. Ezzel a szolgáltatással a felhasználók kísérletezhet és hello Apache Hadoop ökoszisztémájának tooAzure HDInsight-fürtök rendelkezésre álló projektek telepítése. A testreszabási szolgáltatás a HDInsight-fürtök, beleértve a Hadoop, HBase és a Storm minden típusú érhető el.</td>
<td>Új szolgáltatás</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>Az Azure websites és a tárolási naplóelemzés előre elkészített feladataihoz</td>
<td>hello HDInsight lekérdezés konzol első lépések gyűjteményt, amely támogatja a megoldások, amelyek működnek, az adatok vagy a mintaadatok rendelkezik.
<p>**Az adatok működő megoldások**:<br>
Létrehoztunk Önnek feladatok néhány hello leggyakrabban használt adatokat elemzés forgatókönyvek tooprovide saját megoldások kiindulási pontot. Az adatok hogyan működik a hello feladat toosee használható. Ha készen áll, használni, mi megtanulhatta toocreate olyan megoldás, amely hello előre elkészített feladat után van modellezve.</p>
<p>**Megoldások, amelyek működnek a mintaadatok**:<br>
Megtudhatja, hogyan toowork hdinsight érdekében néhány alapvető forgatókönyv (például a webes naplókat, valamint érzékelőadatok elemzése) keresztül. Megismerheti, hogyan toouse HDInsight tooanalyze ilyen adatok és egyéb alkalmazások és szolgáltatások toothis adatok csatlakoztatásának módja. Adatok megjelenítése tooMicrosoft Excel csatlakozzon egy példát hello hatványa ezt a módszert használja.</p></td>
<td>Lekérdezés-konzol</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
<tr>
<td>A lépni a Templeton memória memóriavesztés javítás</td>
<td>A hosszú ideig futó fürtök, vagy a 100-as egység feladat volt elküldése felhasználókat érintő lépni a Templeton memóriavesztés kérelmek egy második megoldottuk. hello lemezegység lépni a Templeton 5xx hibák és hello megkerülő megoldás, jelenik meg a probléma toorestart hello szolgáltatás volt. hello megoldás már nem szükséges.</td>
<td>Lépni a Templeton</td>
<td>Összes</td>
<td>https://issues.Apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>

> [!NOTE]
> toodemonstrate hello új képességeket által elérhetővé tett fürt testreszabási parancsfájlművelet tooinstall Spark és R modul használatával olyan fürtön hello eljárások jegyezték. További információkért lásd:

* [Telepítése és használata Spark 1.0 a HDInsight-fürtökön](hdinsight-hadoop-spark-install.md)
* [Telepítheti és használhatja R HDInsight Hadoop-fürtök](hdinsight-hadoop-r-scripts.md)

## <a name="notes-for-11072014-release-of-hdinsight"></a>A HDInsight 11/07/2014 kiadási megjegyzései

teljes verziószámai hello telepített ebben a kiadásban a HDInsight-fürtök a következők:

* A HDInsight 2.1 2.1.9.374.1153876
* A HDInsight 3.0-s 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Ebben a kiadásban a következő összetevő-frissítések hello tartalmazza:

<table border="1">
<tr><th>Cím</th><th>Leírás</th><th>Összetevő</th><th>Fürt típusa</th><th>JIRA (ha van ilyen)</th></tr>
<tr>
<td>HDP 2.1.7</td>
<td>Ebben a kiadásban a Hortonworks Data Platform (HDP) 2.1.7 alapul. További információkért lásd: <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">HDP 2.1.7 kibocsátási megjegyzései</a>.</td>
<td>HDP</td>
<td>Összes</td>
<td>N/A</td>
</tr>
<tr>
<td>YARN ütemterv kiszolgáló</td>
<td>hello YARN ütemterv Server (más néven hello általános alkalmazáskiszolgáló előzmények) alapértelmezés szerint engedélyezve van. hello ütemterv Server (például az Alkalmazásazonosító, alkalmazás neve, az alkalmazás állapota, alkalmazás küldésének ideje és kérelem végrehajtási időt) befejeződött alkalmazásokkal kapcsolatos általános információkat nyújt.

Az alkalmazás adatait hello átjárócsomópont lekérhetők, hello URI http://headnodehost:8188 elérésével vagy futó hello YARN paranccsal: yarn alkalmazás – - appStates lista összes.

Ez az információ is beolvashatók távolról, ha egy REST API https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

További részletekért lásd: <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN ütemterv Server</a>.</td>
<td>Szolgáltatás, YARN</td>
<td>Hadoop, HBase</td>
<td>N/A</td>
</tr>
<tr>
<td>Fürt üzembehelyezés-azonosító</td>
<td>SDK verzió 1.3.3.1.5426.29232 verziótól kezdődően felhasználók férhetnek hozzá az egyes fürtökön HDInsight kiadott egyedi Azonosítóját. Ez lehetővé teszi, hogy az ügyfelek toofigure egyedi példánya fürtök, ha a DNS-név ismételten van folyamatban, hozzon létre, vagy dobja el a forgatókönyveket.</td>
<td>SDK</td>
<td>Összes</td>
<td>N/A</td>
</tr>
</table>

> [!NOTE]
> Ebben a kiadásban, amely megakadályozta a hello teljes verziószáma jelenik meg a hello portál vagy a Windows PowerShell vagy hello SDK által visszaadott hello hiba elhárítása.

## <a name="notes-for-10152014-release"></a>10/15/2014 kibocsátási megjegyzései

Ez a gyorsjavítás kiadás memóriavesztés lépni a Templeton hello gyakori felhasználók lépni a Templeton az érintett rögzített. Bizonyos esetekben felhasználók, akik lépni a Templeton fokozottan gyakorolni mint 500 hibakódok lemezegység jelenik meg, mert hello kérelmek nem rendelkezik elég memóriával toorun hibák jelenik meg. a probléma megoldása hello toorestart hello lépni a Templeton-szolgáltatás volt. Ez a hiba kijavítása.

## <a name="notes-for-1072014-release"></a>10/7/2014 kibocsátási megjegyzései

* Ambari használatakor végpont, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" hello *gazdaszámítógép_neve* mezőben értéket ad vissza hello teljesen minősített tartománynevét (FQDN) hello csomópont csak a hello állomásnév helyett. Például helyett visszaadó "**headnode0**", hello FQDN kapott"**headnode0. { ClusterDNS} .azurehdinsight .net**". Ez a változás szükséges toofacilitate forgatókönyvek, ahol több fürt típust (például a HBase és a Hadoop) is telepíthető egy virtuális hálózati volt. Ez történik, például a HBase a háttér-platformként a Hadoop használatakor.

* Hello alapértelmezett telepítési hello HDInsight-fürt új memóriabeállításait adtunk. Előző alapértelmezett memória beállításainak megfelelően adta derül hello bemutató hello telepített Processzormagok száma. Ezek új memória a beállítások jobb alapértelmezett (visszamenőleges Hortonworks javaslatok) kell biztosítania. toochange ezeket, tekintse meg a toohello SDK referenciadokumentációt fürtkonfiguráció módosításával. a következő táblázat hello hello új memóriabeállításait hello alapértelmezett 4 Processzor mag (8 tároló) HDInsight-fürt által használt van felsorolva. (hello használt értékek toothis előzetes kiadás is rendelkezésre állnak segítségével történik.)

<table border="1">
<tr><th>Összetevő</th><th>A memóriafoglalás</th></tr>
<tr><td> yarn.Scheduler.minimum-foglalás</td><td>768 MB (korábban 512 MB)</td></tr>
<tr><td> yarn.Scheduler.maximum-foglalás</td><td>6144 MB (változatlan)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 MB (változatlan)</td></tr>
<tr><td>Mapreduce.Map.Memory</td><td>768 MB (korábban 512 MB)</td></tr>
<tr><td>mapreduce.Map.Java.opts</td><td>azt a =-Xmx512m (korábban - Xmx410m)</td></tr>
<tr><td>Mapreduce.reduce.Memory</td><td>1536 MB (korábban 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>azt a =-Xmx1024m (korábban - Xmx819m)</td></tr>
<tr><td>yarn.App.mapreduce.am.Resource</td><td>768 MB (korábban 1024 MB)</td></tr>
<tr><td>yarn.App.mapreduce.am.Command</td><td>azt a =-Xmx512m (korábban - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.sort</td><td>256 MB (korábban 200 MB)</td></tr>
<tr><td>tez.am.Resource.Memory</td><td>1536 MB (változatlan)</td></tr>
</table>

A HDInsight által használt Hortonworks Data Platform hello YARN és a MapReduce által használt hello memória konfigurációs beállítások kapcsolatos további információkért lásd: [meghatározásához HDP memória konfigurációs beállítások](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks toocalculate megfelelő memória beállításait is megadja egy eszköz.

Hello Azure PowerShell és hello HDInsight SDK hibaüzenet: "*fürt nincs konfigurálva HTTP-szolgáltatások hozzáférést*":

* Ez a hiba nem egy ismert [kompatibilitási probléma](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) esetleg előforduló hello hello HDInsight SDK vagy az Azure PowerShell és hello verziója hello fürt tooa eltérése miatt. A 8/15-ös vagy újabb létrehozott fürtök van a virtuális hálózatok az új üzembe helyezési funkció támogatása. Azonban ez a funkció nem megfelelően értelmezi hello HDInsight SDK vagy az Azure PowerShell korábbi verziói által. hello eredménye néhány feladat elküldése művelet hibája miatt. HDInsight SDK API-k vagy az Azure PowerShell-parancsmagok használatakor (**használata-AzureRmHDInsightCluster** vagy **Invoke-AzureRmHDInsightHiveJob**) toosubmit feladatok, ezek a műveletek sikertelenek lehetnek, hello hibaüzenet " *Fürt <clustername> nincs konfigurálva HTTP-szolgáltatások hozzáférést*. " Vagy (attól függően, hogy hello művelet), akkor előfordulhat, hogy hibaüzenet jelenik meg más, például a "*nem lehet kapcsolódni a toocluster*".
* A kompatibilitási problémák hello HDInsight SDK és Azure PowerShell legújabb verziói hello megoldott. Javasoljuk, hogy frissítési HDInsight SDK tooversion 1.3.1.6 vagy későbbi és az Azure PowerShell eszközök tooversion 0.8.8 hello vagy újabb. Kaphat hozzáférést toohello HDInsight SDK legújabb [Nuget](http://nuget.codeplex.com/wikipage?title=Getting%20Started) és Azure PowerShell-eszközök: hello [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Megjegyzések a HDInsight 3.1 9, a 12/a 2014 kiadásban
* Ebben a kiadásban a Hortonworks Data Platform (HDP) 2.1.5 alapul. Ebben a kiadásban hello javított listájáért lásd: hello [ebben a kiadásban rögzített](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) lap hello Hortonworks helyen.
* Hello Pig szalagtárak mappában hello "az avro-mapred-1.7.4.jar" fájl módosult túl "az avro-mapred-1.7.4-hadoop2.jar." a fájl tartalmának hello tartalmaz, amely nem törhető kisebb hibajavítás. Javasoljuk, hogy az ügyfelek ne hajtsa végre a közvetlen függőség hello hello neve fájl tooavoid oldaltörések JAR fájlok átnevezéskor.

## <a name="notes-for-8212014-release"></a>8/21/2014 kibocsátási megjegyzései
* A következő WebHCat konfigurációs (HIVE-7155), amely hello alapértelmezett memóriakorlátja lépni a Templeton-vezérlő feladat too1 GB hello hozzáadni azt. (hello előző alapértelmezett érték volt 512 MB).

     templeton.mapper.Memory.MB (= 1024)

  * Ez a változás a következő hiba történt a következő bizonyos Hive-lekérdezések miatt toolower memóriakorlátokat hello címek: "Tárolóban fut túl fizikai memóriakorlátokat."
  * toorevert toohello a régi alapértelmezett értékeket, beállíthatja a konfigurációs érték too512 Azure PowerShell használatával fürt létrehozáskor hello a következő parancs használatával:

      Adja hozzá AzureRmHDInsightConfigValues-Core @{"templeton.mapper.memory.mb"="512";}
* hello zookeeper szerepkör hello állomás neve túl megváltozott*zookeeper*. Ez hatással van a névfeloldás hello fürtön belül, de ez nincs hatással a külső REST API-k. Ha összetevők adott használata hello *zookeepernode* állomásnevet, tooupdate kell őket toouse új nevet. hello hello három zookeeper csomópontok tartozó új nevek a következők:

  * zookeeper0
  * zookeeper1
  * zookeeper2
* A HBase verzió támogatási mátrix frissül. Csak a HDInsight 3.1-es (HBase 0,98 verzió) verzióját éles HBase munkaterhelések esetén támogatott. 3.0-s verziója (amely nem volt előzetes verzió) nem támogatott áthelyezése előre.

## <a name="notes-about-clusters-created-prior-too8152014"></a>Megjegyzések az fürtök too8/15/2014 előzetes létrehozása
Azure PowerShell vagy a HDInsight SDK hibaüzenetet "fürt <clustername> nincs konfigurálva HTTP-szolgáltatások hozzáférést" (vagy hello művelet függően más hibaüzenetek például: "Nem lehet kapcsolódni a toocluster") tooa verzió különbség miatt esetlegesen felmerülő Azure PowerShell vagy a HDInsight SDK hello és egy fürt. A 8/15-ös vagy újabb létrehozott fürtök van a virtuális hálózatok az új üzembe helyezési funkció támogatása. Ez a funkció nem megfelelően értelmezi hello Azure PowerShell vagy a HDInsight SDK-t, így ez a feladat elküldése műveletek hibák hello régebbi verzióját. A HDInsight SDK API-k vagy az Azure PowerShell-parancsmagok (például használja-AzureRmHDInsightCluster vagy Invoke-AzureRmHDInsightHiveJob) toosubmit feladatok, ha ezek a műveletek sikertelenek lehetnek hello ismertetett hibaüzenetek valamelyikével.

A kompatibilitási problémák hello HDInsight SDK és Azure PowerShell legújabb verziói hello megoldott. Javasoljuk, hogy frissítési HDInsight SDK tooversion 1.3.1.6 vagy későbbi és az Azure PowerShell eszközök tooversion 0.8.8 hello vagy újabb. Kaphat hozzáférést toohello HDInsight SDK legújabb [NuGet][nuget-link]. Hello Azure PowerShell eszközök használatával végezheti el [Microsoft Webplatform-telepítő][webpi-link].

## <a name="notes-for-7282014-release"></a>7/28/2014 kibocsátási megjegyzései
* **Új régiókban elérhető HDInsight**: HDInsight földrajzi jelenléte toothree régiók kibontva azt. HDInsight ügyfelei ezeken a területeken tárolófürtöket hozhat létre:
  * Kelet-Ázsia
  * USA északi középső régiója
  * USA déli középső régiója
* HDInsight (HDP 1.1-es és a Hadoop 1.0.3) 1.6-os verziójának és a HDInsight 2.1-es (HDP1.3 és Hadoop 1.2-es) verziójával van távolít el hello Azure-portálon. Folytatás toocreate Hadoop-fürtök az ezen verziói használatával hello Azure PowerShell-parancsmag [New-AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) vagy hello segítségével [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx). További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).
* Ebben a kiadásban módosítások Hortonworks Data Platform (HDP):

<table border="1">
<tr><th>HDP</th><th>Változások</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Nincs változás</td></tr>
<tr><td>HDP 2.0-S VAGY HDI 3.0</td><td>Nincs változás</td></tr>
<tr><td>HDP 2.1 / HDI 3.1.</td><td>zookeeper: [3.4.5.2.1.3.0-1948]-[3.4.5.2.1.3.2-0002] ></td></tr>
</table>

## <a name="notes-for-6242014-release"></a>6/24/2014 kibocsátási megjegyzései
Ebben a kiadásban fejlesztések toohello HDInsight-szolgáltatás tartalmazza:

* **HDP 2.1 rendelkezésre állási**: HDInsight 3.1 (HDP 2.1-es verzióját tartalmazó) általánosan elérhető, és új fürtök hello alapértelmezett verzió.
* **HBase - Azure portál fejlesztései**: hajtunk HBase-fürtökkel ebben a nézetben. Néhány kattintással hello portálról hozhat létre HBase-fürtökkel.

A hbase eszközzel hozhat létre a valós idejű munkaterhelések különböző hdinsight, az interaktív webhelyeket, amelyek használhatók a nagy adatkészletek tooservices végpontok több millió érzékelői és telemetriai adatainak tárolásához. hello tovább tooanalyze hello adatokat az ilyen terhelések Hadoop-feladatokkal, és ez a HDInsight az Azure PowerShell és hello Hive fürt irányítópulton keresztül lehetséges.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>HDInsight 3.1 előtelepítve Apache Mahout
 [Mahout](http://hortonworks.com/hadoop/mahout/) előre telepítve van a HDInsight 3.1 Hadoop-fürtök,, így a fürt további konfiguráció nélkül hello Mahout feladatok futtatása. Például akkor távoli Hadoop fürtbe Remote Desktop Protocol (RDP) használatával, és további lépések nélkül futtathatja a következő Hello World Mahout parancs hello:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Ezzel az eljárással teljesebb leírását a dokumentációban hello hello [Breiman példa](https://mahout.apache.org/users/classification/breiman-example.html) hello Apache Mahout webhelyen.

### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Hive-lekérdezéseket használhat Tez HDInsight 3.1
0,13 Hive HDInsight 3.1 érhető el, és képes futó lekérdezések Tez, amelyek jelentős teljesítménnyel kapcsolatos fejlesztések a használatával.
Tez Hive-lekérdezések alapértelmezés szerint nincs engedélyezve. toouse, bírálhatja felül a. Tez engedélyezheti a következő kódrészletet hello futtatásával:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

A szabványos referenciaalapok szállított Hortonworks közzétett részletes információkat a Tez Hive lekérdezés teljesítmény fejlesztések. További információkért lásd: [teljesítménymérésre Apache Hive 13 a vállalati Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

További információkért lásd: [a Tez Hive](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

### <a name="global-availability"></a>Globális rendelkezésre állása
A HDInsight Hadoop 2.2 a hello kiadással Microsoft HDInsight elérhetővé tett összes fő földrajzi, amennyiben rendelkezésre áll-e Azure a. Pontosabban hello nyugati Európa és Délkelet-ázsiai adatközpontokban léptették online. Ez lehetővé teszi az ügyfelek toolocate fürtök esetén egy adatközpontban, amely közel van és potenciálisan hasonló megfelelőségi követelményekkel zónában.

### <a name="dos--donts-between-cluster-versions"></a>DOS & dolog fürt verziói között
**HDInsight 3.1-fürthöz használt Oozie metastores nem kompatibilis a HDInsight 2.1 fürtök, és azokat az előző verzió nem használható**.

HDInsight 3.1-fürthöz telepített egyéni Oozie metaadattárhoz adatbázis nem használható fel újra egy HDInsight 2.1-fürthöz. Ez helyzet hello akkor is, ha hello metaadattárhoz egy HDInsight 2.1 fürttel származik. Ez a forgatókönyv nem támogatott, mert hello metaadattárhoz séma lekérdezi frissítve, így már nem kompatibilis a HDInsight 2.1 hello fürtök által igényelt hello metaadattárhoz egy HDInsight 3.1 fürt használata esetén. A kísérlet tooreuse egy Oozie metaadattárhoz HDInsight 3.1-fürthöz használt leképezési hello HDInsight 2.1 fürt használhatatlan.

**Oozie metastores nem osztható meg más fürtökre.**

Oozie metastores csatolt toospecific fürtöket, és nem oszthatók meg fürtök.

### <a name="breaking-changes"></a>Módosítások megszakítása
**Előtag-szintaxis**: csak hello "wasb: / /" HDInsight 3.1 és 3.0-s a szintaxis támogatott. régebbi hello "asv: / /" szintaxis HDInsight 2.1-es és 1.6 fürtök támogatott, de a HDInsight 3.1 vagy 3.0-s nem támogatott. Ez azt jelenti, hogy bármely verzióba küldött feladatok tooan HDInsight 3.1 vagy fürt, amely explicit módon 3.0 használ hello "asv: / /" szintaxis sikertelen lesz. hello "wasb: / /" szintaxist kell használni. Is, tooany HDInsight 3.1 küldött feladatok, vagy 3.0 fürtök használatával létrehozott egy meglévő metaadattárhoz tartalmazó explicit hivatkozik hello segítségével tooresources "asv: / /" szintaxis sikertelen lesz. Ezeket a metaadattárakat kell hello újra létrehozni toobe "wasb: / /" szintaxis tooaddress erőforrásokat.

**Portok**: hello HDInsight-szolgáltatás által használt hello portok megváltoztak. hello portszámok, amelyek volt használatban volt hello elmúló porttartományának hello Windows operációs rendszer belül. Portok automatikusan kiosztott egy előre meghatározott elmúló tartományból rövid élettartamú Internet protocol-alapú kommunikációra. engedélyezett Hortonworks Data Platform (HDP) szolgáltatás portszámok hello új készletét a tartomány tooavoid, amelyek hello központi csomóponton futó szolgáltatások által használt hello porttal felmerülhetnek ütközések észlelése kívül vannak. hello új portszámok nem okozhat a legfrissebb változtatásokat. hello számok a következők:

 **HDInsight 1.6 (HDP 1.1)**

<table border="1">
<tr><th>Név</th><th>Érték</th></tr>
<tr><td>DFS.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.Server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table>

 **HDInsight 3.1 és 3.0-s (HDP 2.1-es és 2.0-s)**

<table border="1">
<tr><th>Név</th><th>Érték</th></tr>
<tr><td>DFS.namenode.http-cím</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.https-cím</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.http-cím</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table>

### <a name="dependencies"></a>Függőségek
hello következő függőségek lettek hozzáadva a Hdinsightban 3.x (HDP2.x):

* guice-servlet
* optiq magos
* javax.inject
* az aktiválás
* jsr305
* geronimo-jaspic_1.0_spec
* július-slf4j
* Java-xmlbuilder
* telepítsenek
* Commons-fordító
* jdo-api
* Commons-math3
* paranamer
* jaxb-impl
* stringtemplate
* eigenbase-xom
* Jersey-servlet
* Commons-exec
* jaxb-api
* jetty-minden-kiszolgáló
* janino
* xercesImpl
* optiq-avatica
* jta
* eigenbase-tulajdonságok
* groovy-all
* hamcrest magos
* mail
* linq4j
* jpam
* Jersey-ügyfél
* aopalliance
* geronimo-annotation_1.0_spec
* telepítsenek-indítója
* Jersey-guice
* XML-API-k
* stax-api
* a címterület-kezelés-commons
* a címterület-kezelés-fa
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-all
* sebesség
* ellehetetlenülését okozta
* klassz kis java
* jetty-all
* Commons-dbcp

hello következő függőség már nem létezik a Hdinsightban 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* ivy
* aspectjrt
* JSON-ban
* mag
* jdo2-api
* az avro-mapred
* datanucleus-enhancer
* jsp
* Commons-naplózás-api
* matematikai Commons
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* a hbase
* klassz kis

### <a name="version-changes"></a>Megváltozik a verzió
HDInsight közötti végzett megváltozik a verzió a következő hello 2.x (HDP1.x) és a HDInsight 3.x (HDP2.x):

* metrikák-core: [2.1.2]-[3.0.0] >
* derbynet: [10.4.2.0]-[10.10.1.1] >
* datanucleus: ['rdbms-3.0.8'] -> ["rdbms-3.2.9']
* jasper-fordító: [5.5.12]-[5.5.23] >
* log4j: ["1.2.15. pontját", '1.2.16'] -> ["1.2.16", "1.2.17"]
* derbyclient: [10.4.2.0]-[10.10.1.1] >
* httpcore: [4.2.4]-[4.2.5] >
* hsqldb: [1.8.0.10]-[2.0.0] >
* jets3t: [0.6.1]-[0.9.0-s] >
* a java-protobuf: [2.4.1]-[2.5.0] >
* Derby: [10.4.2.0]-[10.10.1.1] >
* jasper: [' futásidejű-5.5.12'] -> ["futtatókörnyezet-5.5.23']
* Commons-démon: [1.0.1-es]-[1.0.13] >
* datanucleus-core: [3.0.9]-[3.2.10] >
* datanucleus-api-jdo: [3.0.7]-[3.2.6] >
* zookeeper: [3.4.5.1.3.9.0-01320]-[3.4.5.2.1.3.0-1948] >
* bonecp: [0.7.1.RELEASE] -> ["
* 0.8.0.RELEASE "]

### <a name="drivers"></a>Illesztőprogramok
SQL Server illesztőprogramját hello Java adatbázis kapcsolat (JDBC) belsőleg HDInsight, és nem használják-e külső műveletek. Ha tooconnect tooHDInsight megnyitott adatbázis-kapcsolat (ODBC) használatával, használja a Microsoft Hive ODBC-illesztőprogram hello. További információkért lásd: [kapcsolódás Excel tooHDInsight a Microsoft Hive ODBC-illesztő hello](hdinsight-connect-excel-hive-odbc-driver.md).

### <a name="bug-fixes"></a>Hibajavítások
Ebben a kiadásban a következő számos hibajavítást a HDInsight-verziókról hello frissítette azt:

* A HDInsight 2.1-es (HDP 1.3)
* A HDInsight 3.0-s (HDP 2.0-s)
* HDInsight 3.1 (HDP 2.1)

## <a name="hortonworks-release-notes"></a>Hortonworks kibocsátási megjegyzései
Kibocsátási megjegyzések a hello Hortonworks Data platform (HDPs) verziója a HDInsight-fürtök által használt hello helyek a következő webhelyen érhetők el:

* HDInsight 3.1-es verzióját használja egy Hadoop-terjesztést hello alapuló [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. Ez a hello alapértelmezett Hadoop-fürt létrehozása után 11/7/2014 hello Azure portál használata esetén. HDInsight 3.1 fürtöket létrehozni, mielőtt 11/7/2014 hello alapuló [Hortonworks Data Platform 2.1.1][hdp-2-1-1]
* HDInsight 3.0-s verzióját használja egy Hadoop-terjesztést hello alapuló [Hortonworks Data Platform 2.0][hdp-2-0-8].
* A HDInsight 2.1-es verziója hello alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight 1.6-os verziójának hello alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
