---
title: "Data Lake Store érintő aaaData forgatókönyvek |} Microsoft Docs"
description: "Megérteni a különböző alkalmazási helyzetek hello és eszközök használatával mely adatokra is okozhatnak, feldolgozása, letöltése és ábrázolt egy Data Lake Store-ban"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 37409a71-a563-4bb7-bc46-2cbd426a2ece
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: caaa3979b8a2532089778c3e3db3c711714d3c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Azure Data Lake Store használatát a big Data típusú adatok követelmények
Nagy adatfeldolgozási négy fő szakaszból áll:

* Választásával dolgozhat fel nagy mennyiségű adat adatok tárolóba, valós idejű vagy kötegek
* Hello adatainak feldolgozása
* Hello adatok letöltése
* Hello adatok megjelenítése

Ebben a cikkben úgy tekintünk, ezek a szakaszok a tekintetben tooAzure Data Lake Store toounderstand hello beállításokat és eszközei elérhető toomeet a big Data típusú adatok igényeinek.

## <a name="ingest-data-into-data-lake-store"></a>A Data Lake Store betöltik az adatokat
Ez a szakasz hello különböző forrásokból, amelyben el az adatok egy Data Lake Store figyelembe meghatározták adatok és hello különböző módokon mutatja be.

![A Data Lake Store betöltik az adatokat](./media/data-lake-store-data-scenarios/ingest-data.png "betöltik az adatokat a Data Lake Store")

### <a name="ad-hoc-data"></a>Ad hoc adatok
Ez jelöli, amelyek kisebb adatkészletek prototípusának a big Data típusú adatok alkalmazás használja. Többféleképpen attól függően, hogy hello adatforrás hello alkalmi adatok választásával dolgozhat fel.

| Adatforrás | Betöltési használatával |
| --- | --- |
| Helyi számítógép |<ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Az Azure platformfüggetlen parancssori felület 2.0](data-lake-store-get-started-cli-2.0.md)</li> <li>[A Data Lake Tools for Visual Studio használatával](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Az Azure Storage-Blobba |<ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)</li> <li>[AdlCopy eszköz](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[HDInsight-fürtön futó ból a DistCp](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Adatfolyamként továbbított adatok
Adatok, például alkalmazások, eszközök, érzékelőket és stb különböző forrásokból létrehozható jelképez. Ezek az adatok különböző eszközök által történő egy Data Lake Store meghatározták. Ezek az eszközök általában fog rögzítése és az esemény által alapon hello adatok feldolgozása valós idejű, majd írja hello események kötegekben Data Lake Store az, hogy azok további dolgozhatók fel.

Az alábbiakban eszközök közül választhat:

* [Az Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -események az Event Hubsban okozhatnak csak írható tooAzure Data Lake az Azure Data Lake Store kimeneti használatával.
* [Az Azure HDInsight alatt futó Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) -adatokat írhatna közvetlen tooData Lake Store a hello Storm-fürt.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – események fogadásához az Event Hubs, és ezután írják tooData Lake Store hello segítségével [Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relációs adatok
A forrásadatok relációs adatbázisból. Egy meghatározott időtartam során rendelkezésre a relációs adatbázisok gyűjtése hatalmas mennyiségű adat, amely kulcs áttekintést adnak is, ha a big data-feldolgozási folyamaton keresztül. Is használhatja a következő eszközök toomove hello ilyen adatok Data Lake Store.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web server naplóadatokat (a feltöltési egyéni alkalmazások használata)
Ez a fajta adatkészlet kifejezetten feltüntettük mert web server napló adatok elemzése a big data-alkalmazások gyakori használati eset, és megköveteli a naplófájl fájlok toobe nagy mennyiségű feltöltött toohello Data Lake Store. Használhatja a következő eszközök toowrite hello bármelyikét saját parancsfájlok vagy alkalmazások tooupload ezeket az adatokat.

* [Az Azure platformfüggetlen parancssori felület 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Az Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Web server napló adatfeltöltési, valamint a más típusú adatok (például közösségi hangulati elemek adatok) feltöltése, már jó közelítse toowrite saját egyéni parancsfájlok alkalmazások mert biztosít rugalmasságot tooinclude hello az adatok feltöltése részeként összetevő a nagyobb big Data típusú adatok alkalmazás. Egyes esetekben ez a kód hello űrlap egy parancsfájl vagy egy egyszerű parancssori segédprogram is eltarthat. Más esetekben a hello kód használt toointegrate nagy az adatfeldolgozás fel üzleti alkalmazás vagy megoldás lehet.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Társított alkalmazások Azure HDInsight-fürtök
A legtöbb HDInsight-fürttípusok (Hadoop, HBase, Storm) Data Lake Store támogatják az adatok tárolási tára. A HDInsight-fürtök elérni az adatokat az Azure Storage Blobs (WASB). A jobb teljesítmény érdekében átmásolhatja hello adatok WASB be hello-fürthöz tartozó Data Lake Store-fiókba. A következő eszközök toocopy hello adatok hello is használhatja.

* [Apache ból a DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy szolgáltatás](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Tárolt adatokat a helyszíni vagy infrastruktúra-szolgáltatási Hadoop fürtök
Nagy mennyiségű adat tárolható meglévő Hadoop-fürtök, helyi HDFS használatával gépeken. hello Hadoop-fürtök egy helyi központi esetleg lehet, hogy az infrastruktúra-szolgáltatási fürtöt az Azure-on belül. Lehetnek követelmények toocopy ilyen adatok tooAzure Data Lake Store megközelítésre egyszeri vagy ismétlődő módon. Használható tooachieve ez különböző lehetőség áll rendelkezésre. Az alábbiakban olvashat egy listát alternatívák és hello tartozó kompromisszumot.

| Módszer | Részletek | Előnyei | Megfontolandó szempontok |
| --- | --- | --- | --- |
| Használja az Azure Data Factory (ADF) toocopy adatokat közvetlenül a Hadoop fürtök tooAzure Data Lake Store |[ADF HDFS adatforrásként támogatja.](../data-factory/data-factory-hdfs-connector.md) |ADF out-of-az-box támogatást biztosít az HDFS és első-végpontok kezelése és figyelése |Az adatkezelési átjáró toobe helyszíni telepítésére van szükség vagy hello IaaS fürtben |
| Adatok exportálása a Hadoop-fájlok formájában. Ezután másolja hello fájlok tooAzure Data Lake Store megfelelő mechanizmussal. |Másolhatja a fájlokat tooAzure Data Lake Store használata: <ul><li>[Az Azure PowerShell, a Windows operációs rendszer](data-lake-store-get-started-powershell.md)</li><li>[Az Azure platformfüggetlen parancssori felület 2.0 a nem - Windows operációs rendszer](data-lake-store-get-started-cli-2.0.md)</li><li>Minden Data Lake Store SDK használatával egyéni alkalmazás</li></ul> |Gyors tooget elindult. Testre szabott feltöltések teheti |Folyamat, amely magában foglalja a több technológiák. Kezelési és figyelési toobe kihívást nőhet hello eszközök testre szabott hello jellege megadott idő alatt |
| Hadoop tooAzure tárolási ból a Distcp toocopy adatait használják. Ezután másolja át az Azure Storage tooData Lake Store megfelelő mechanizmussal adatokat. |Adatok másolása az Azure Storage tooData Lake Store használata: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy eszköz](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[A HDInsight-fürtökön futó Apache ból a DistCp](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Nyílt forráskódú eszközök is használhatók. |Folyamat, amely magában foglalja a több technológiák |

### <a name="really-large-datasets"></a>Valóban nagy adatkészletek
Az adatkészleteket, amelyek között a több terabájt feltöltése, a fent leírt hello módszerekkel néha lassú és költséges lehet. Ilyen esetben az alábbi hello-beállítások is használhatja.

* **Az Azure ExpressRoute segítségével**. Az Azure ExpressRoute lehetővé teszi az Azure adatközpontjaiban és az infrastruktúra közötti magánhálózati kapcsolatok létrehozása a helyszínen. Ez lehetővé teszi nagy mennyiségű adat átvitele egy megbízható beállítása. További információkért lásd: [Azure ExpressRoute dokumentációja](../expressroute/expressroute-introduction.md).
* **Az adatok "Offline" feltöltése a**. Ha az Azure ExpressRoute használata nem megvalósítható a bármilyen okból, [Azure Import/Export szolgáltatás](../storage/common/storage-import-export-service.md) tooship merevlemez-meghajtók és az adatok tooan Azure-adatközpont. Az adatok első feltöltött tooAzure Storage Blobsba. Ezután [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) vagy [AdlCopy eszköz](data-lake-store-copy-data-azure-storage-blob.md) toocopy adatok Azure Storage Blobs tooData Lake Store-ból.

  > [!NOTE]
  > Amíg Import/Export szolgáltatás használata hello, hello fájlméret a lemezeket, hogy a tooAzure adatok szolgáltatástól center hello nem lehet nagyobb, mint 195 GB.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Data Lake Store-ban tárolt adatok feldolgozása
Hello adatok elérhetővé válik a Data Lake Store futtathatja elemzés, hogy az adatok hello támogatott big data-alkalmazások. Jelenleg a Data Lake Store hello adataihoz az Azure HDInsight és az Azure Data Lake Analytics toorun az elemzés feladatok is használhatja.

![Adatok elemzése az Data Lake Store](./media/data-lake-store-data-scenarios/analyze-data.png "elemezhetik a Data Lake Store-ban")

A következő példák hello is megtekinthetik.

* [HDInsight-fürtök létrehozása a Data Lake Store tárolóként](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Töltse le az adatok Data Lake Store-ból
Előfordulhat, hogy szeretné, hogy toodownload, vagy tárolt adatok mozgatása az Azure Data Lake Store forgatókönyvek például:

* Helyezze át az adatokat tooother adattárak toointerface a meglévő adatok feldolgozása kimenetátirányítási mechanizmusát használó műveletekről. Például akkor érdemes toomove Data Lake Store tooAzure SQL-adatbázis adatait vagy a helyszíni SQL Server.
* Töltse le az adatok tooyour helyi számítógép alkalmazás prototípusok felépítésekor IDE környezetben feldolgozásra.

![Data Lake Store-ból adatokat kilépési](./media/data-lake-store-data-scenarios/egress-data.png "kilépési adatok Data Lake Store-ból")

Ilyen esetekben hello a következő beállítások bármelyikét használhatja:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache ból a DistCp](data-lake-store-copy-data-wasb-distcp.md)

Használhatja a következő módszerek toowrite hello saját parancsprogram-alkalmazás toodownload adatok Data Lake Store-ból.

* [Az Azure platformfüggetlen parancssori felület 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Az Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>A Data Lake Store-adatok ábrázolása
Használhatja a szolgáltatások toocreate vizuális megjelenítésére Data Lake Store-ban tárolt adatok kombinációját.

![A Data Lake Store-adatok ábrázolása](./media/data-lake-store-data-scenarios/visualize-data.png "a Data Lake Store-adatok ábrázolása")

* Megkezdheti a [Data Lake Store tooAzure SQL Data Warehouse Azure Data Factory toomove adatait](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
* Ezt követően is [Power BI integrálása az Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) toocreate hello adatok vizuális ábrázolását.
