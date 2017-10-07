---
title: Azure Data Lake Store aaaOverview |} Microsoft Docs
description: "Mi az Azure Data Lake Store és hello értékeket nyújt az egyéb adattárakhoz képest ismertetése"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b3475057-9427-4492-a3af-25a802a23a79
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 5a60a6b86a51c44647cf4ee168fb333d1c37b1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-data-lake-store"></a>Az Azure Data Lake Store áttekintése
Az Azure Data Lake Store egy vállalati szintű, nagy kapacitású adattár a big data koncepción alapuló adatelemzési célokra. Azure Data Lake lehetővé teszi egy helyen történő műveleti és felderítési jellegű bármilyen méretű, típusú és feldolgozási sebességű toocapture adatait.

> [!TIP]
> Használjon hello [Data Lake Store képzési terv](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) toostart felfedezése hello Azure Data Lake Store szolgáltatást.
> 
> 

Azure Data Lake Store a Hadoop elérhető (HDInsight-fürthöz érhető el) használatával hello WebHDFS-kompatibilis REST API-k. Hello tárolt adatok kifejezetten tervezett tooenable elemzés és a teljesítmény adatok analytics forgatókönyvek van beállítva. Hello mezőbe kívül minden hello vállalati szintű képességet tartalmaz – biztonsági, kezelhetőség, méretezhetőség, megbízhatóság és rendelkezésre állási – alapvető valós vállalati használatából adódó.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Hello hello Azure Data Lake főbb képességei közé hello következő.

### <a name="built-for-hadoop"></a>Hadoop-kompatibilis
hello Azure Data Lake store a Hadoop elosztott fájlrendszerrel (HDFS) kompatibilis Apache Hadoop fájl rendszer, és együttműködik a Hadoop ökoszisztémájának hello.  A meglévő HDInsight-alkalmazások vagy szolgáltatások hello WebHDFS API-t használó könnyen integrálható a Data Lake Store. A Data Lake Store továbbá elérhetővé tesz egy WebHDFS-kompatibilis REST-felületet is az alkalmazásokhoz

A Data Lake Store-ban tárolt adatok könnyen elemezhetők a Hadoop elemzési keretrendszerek, például a MapReduce vagy a Hive segítségével. A Microsoft Azure HDInsight-fürtök kiépítése, és konfigurálva a Data Lake Store-ban tárolt toodirectly hozzáférési adatok.

### <a name="unlimited-storage-petabyte-files"></a>Korlátlan tárterület, petabájtnyi fájlok
Az Azure Data Lake Store korlátlan tárterületet biztosít, és különböző, elemzési célú adatok tárolására alkalmas. Azt nem megkötések a fiókok méretének, fájlméret vagy hello data lake-ben tárolt adatok mennyisége. Egyes fájlok között lehet egy kiváló választás toostore így mérete a kilobájtoktól toopetabytes bármilyen típusú adatot. Adatait tárolja másolatnak köszönhetően, és a hello időtartam, mely hello az adatokat tárolhatja hello data Lake korlátozva van.

### <a name="performance-tuned-for-big-data-analytics"></a>A teljesítmény a big data koncepción alapuló adatelemzéshez lett igazítva
Azure Data Lake Store nagy méretű elemzési rendszerek, amelyek jelentős átviteli sebességet tooquery igényelnek, és nagy mennyiségű adatok elemzése futtatására lett tervezve. hello a data lake több egyéni tárolókiszolgáló egy fájl részeit terjed. Ez növeli a hello átviteli olvasási adatelemzés céljából történő párhuzamos hello fájl olvasásakor.

### <a name="enterprise-ready-highly-available-and-secure"></a>Felkészült a nagyvállalatok igényeire: Magas rendelkezésre állású és biztonságos
Az Azure Data Lake Store az iparági szabványnak megfelelő rendelkezésre állást és megbízhatóságot biztosít. Az adatok adatvagyonának tartós tárolását azáltal, hogy a redundáns másolatok tooguard váratlan hibák ellen. A vállalatok meglévő adatplatformjuk fontos részeként alkalmazhatják az Azure Data Lake-et megoldásaikban.

Data Lake Store hello tárolt adatok nagyvállalati szintű védelmet is biztosít. További információ: [Az adatok védelme az Azure Data Lake Store-ban](#DataLakeStoreSecurity).

### <a name="all-data"></a>Minden adat
Az Azure Data Lake Store bármilyen adatot képes natív formátumában, módosítás vagy előzetes átalakítás nélkül tárolni. Data Lake Store toohello egyéni elemzési keretrendszer toointerpret hello adatokat hagyja a séma toobe hello adatok feltöltése előtt definiálni kell, és nem definiálhat egy sémát hello elemzési hello idő. Tetszőleges méretű és formátumú fájlok képes toostore alatt lehetővé teszi a Data Lake Store toohandle strukturált, félig strukturált és strukturálatlan adatokat.

Az Azure Data Lake Store adattárolói lényegében mappák és fájlok. Az SDK-k, az Azure portál és az Azure Powershell hello tárolt adatokat fog működni. Mindaddig, amíg hello tároló fenti felületeken keresztül és hello megfelelő tárolók használatával helyezi el adatait, bármilyen típusú adatot tárolhat. Data Lake Store nem kezeli különleges módon az adatok alapján hello tárolt adatokat.

## <a name="DataLakeStoreSecurity"></a>Az adatok védelme az Azure Data Lake Store-ban
Azure Data Lake Store az Azure Active Directory-hitelesítéshez, és hozzáférés-vezérlési lista (ACL) toomanage access tooyour adatokat.

| Szolgáltatás | Leírás |
| --- | --- |
| Authentication |Azure Data Lake Store integrálható az Azure Active Directory (AAD) az Azure Data Lake Store-ban tárolt összes hello adatok identitások és hozzáférések felügyeletéhez. Miatt hello integráció, az Azure Data Lake előnyeit az AAD összes funkcióját, többek között a többtényezős hitelesítést, a feltételes hozzáférés, a szerepköralapú hozzáférés-vezérlés, a alkalmazás-használat figyelését, biztonsági figyelést és riasztást stb. Azure Data Lake Store hello REST-felületen belüli hitelesítéshez hello OAuth 2.0 protokollt támogatja. |
| Hozzáférés-vezérlés |Azure Data Lake Store hello WebHDFS protokoll által elérhetővé tett POSIX-stílusú engedélyek támogatásával biztosítja a hozzáférés-vezérlés. Hello Data Lake Store nyilvános előzetes (hello aktuális kiadás), a hozzáférés-vezérlési listák engedélyezhető hello legfelső szintű mappa, almappák és a fájlokat. További információkat a hozzáférés-vezérlési listák Data Lake Store-környezetben való működéséről a következő témakörben talál: [Hozzáférés-vezérlés a Data Lake Store-ban](data-lake-store-access-control.md). |
| Titkosítás |Data Lake Store is biztosít a hello fiókban tárolt adatok titkosítását. Data Lake Store-fiók létrehozásakor megadhatja a hello titkosítási beállításait. Kiválasztott toohave képes a titkosított adatok, vagy választhat titkosítás nélkül. További információt a tooprovide titkosítással kapcsolatos konfigurációs, lásd: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md). |

További információk a Data Lake Store adatok védelme toolearn szeretné. Kövesse az alábbi hello hivatkozásokat.

* Útmutatást toosecure adatok a Data Lake Store, lásd: [adatok védelme az Azure Data Lake Store](data-lake-store-secure-data.md).
* Szeretne inkább videókat megtekinteni? [A videó](https://mix.office.com/watch/1q2mgzh9nn5lx) a toosecure adatok Data Lake Store-ban történő tárolásának módját.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Az Azure Data Lake Store-ral kompatibilis alkalmazások
Azure Data Lake Store található hello Hadoop-ökoszisztéma legtöbb nyílt forráskódú összetevőjével kompatibilis. Emellett egyéb Azure-szolgáltatásokkal is jól integrálható. Mindez azt jelenti, hogy a Data Lake Store tökéletes megoldás az adattárolási igények kielégítésére. Hello hivatkozásokból hogyan használható Data Lake Store, mind a nyílt forráskódú összetevőkkel, valamint a más Azure-szolgáltatásokkal kapcsolatos további toolearn alatt.

* A Data Lake Store-ral kompatibilis nyílt forráskódú alkalmazások listáját: [Az Azure Data Lake Store-ral kompatibilis alkalmazások és szolgáltatások](data-lake-store-compatible-oss-other-applications.md).
* Lásd: [integrálása más Azure-szolgáltatásokkal](data-lake-store-integrate-with-other-services.md) toounderstand, hogyan használható Data Lake Store más Azure-szolgáltatások tooenable a lehetséges forgatókönyvek bővítése.
* Lásd: [Data Lake Store használatára vonatkozó forgatókönyvek](data-lake-store-data-scenarios.md) hogyan toouse Data Lake eseményközpontokból, hasonló forgatókönyvek esetén tárolja toolearn adatok feldolgozása, letöltése és adatok megjelenítése.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Mi az Azure Data Lake Store-fájlrendszer (adl://)?
Data Lake Store hello új fájlrendszer keresztül is elérhetők, hello AzureDataLakeFilesystem (adl: / /), a Hadoop-környezetekben (HDInsight-fürthöz érhető el). Alkalmazások és szolgáltatások, amelyek használják az adl: / / további, amelyek nem érhető el a webhdfs-ben teljesítmény optimalizálása képes tootake előnyeit. Ennek eredményeképpen Data Lake Store biztosítja azt a rugalmasságot tooeither hello igénybe-hello legjobb teljesítmény érdekében ajánlott adl használatának lehetőségét hello: / / vagy a meglévő kód által folyamatos toouse hello WebHDFS API közvetlen kezelése. Az Azure HDInsight teljes mértékben kihasználja hello AzureDataLakeFilesystem tooprovide hello legjobb teljesítmény érdekében a Data Lake Store.

Az adatok hello Data Lake Store használatával végezheti el `adl://<data_lake_store_name>.azuredatalakestore.net`. Hogyan tooaccess hello hello Data Lake Store az adatok további információkért lásd: [hello tulajdonságainak megtekintése tárolt adatok](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Hogyan kezdhetem meg az Azure Data Lake Store használatát?
Lásd: [Azure portál használatával a Data Lake Store hello](data-lake-store-get-started-portal.md), a hogyan using Data Lake Store tooprovision hello Azure portálon. Azure Data Lake kiépítése után megtudhatja hogyan toouse big data ajánlatokat az Azure Data Lake Analytics vagy az Azure HDInsight a Data Lake Store. A .NET-alkalmazás toocreate Azure Data Lake Store-fiók létrehozása is, és hajtsa végre a műveletek, például a feltöltési adatok, az adatok letöltése, stb.

* [Ismerkedés az Azure Data Lake Analytics szolgáltatással](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Az Azure Data Lake Store használatának első lépései a .NET SDK-val](data-lake-store-get-started-net-sdk.md)

## <a name="data-lake-store-videos"></a>Data Lake Store-videók
Ha Ön tanul videók toolearn, Data Lake Store számos szolgáltatáshoz biztosít videókat.

* [Azure Data Lake Store-fiók létrehozása](https://mix.office.com/watch/1k1cycy4l4gen)
* [Az Azure Data Lake Store hello adatkezelő tooManage adatok használata](https://mix.office.com/watch/icletrxrh6pc)
* [Csatlakozás az Azure Data Lake Analytics tooAzure Data Lake Store](https://mix.office.com/watch/qwji0dc9rx9k)
* [Az Azure Data Lake Store elérése a Data Lake Analytics használatával](https://mix.office.com/watch/1n0s45up381a8)
* [Csatlakozás az Azure HDInsight tooAzure Data Lake Store](https://mix.office.com/watch/l93xri2yhtp2)
* [Az Azure Data Lake Store elérése a Hive és a Pig használatával](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Az Azure Data Lake Store ból a DistCp (Hadoop Distributed Copy) toocopy adatok tooand használata](https://mix.office.com/watch/1liuojvdx6sie)
* [Használja az Apache Sqoop toomove adatok relációs források és az Azure Data Lake Store között](https://mix.office.com/watch/1butcdjxmu114)
* [Adatok előkészítése az Azure Data Lake Store-hoz készült Azure Data Factory használatával](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Adatok védelme az Azure Data Lake Store hello](https://mix.office.com/watch/1q2mgzh9nn5lx)

