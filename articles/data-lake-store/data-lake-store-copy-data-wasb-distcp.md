---
title: "aaaCopy adatok tooand a ból a Distcp használatával a Data Lake Store WASB |} Microsoft Docs"
description: "Ból a Distcp eszköz toocopy adatok tooand használja az Azure Storage Blobs tooData Lake Store-ból"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: ec23410bbab0f82449a475412bc3b097c4a8fccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-distcp-toocopy-data-between-azure-storage-blobs-and-data-lake-store"></a>Azure Storage Blobs és a Data Lake Store ból a Distcp toocopy adatok használata
> [!div class="op_single_selector"]
> * [A DistCp használata](data-lake-store-copy-data-wasb-distcp.md)
> * [Az AdlCopy használata](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Miután létrehozta a HDInsight-fürtöt, amely rendelkezik hozzáférési tooa Data Lake Store-fiók, például ból a Distcp toocopy adatokat Hadoop-ökoszisztémával eszközök használhatja **tooand a** egy HDInsight fürt storage (WASB) a Data Lake Store-fiók. Ez a cikk útmutatás tooachieve ez.

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa. Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ellenőrizze, hogy engedélyezte a távoli asztal hello fürt.

## <a name="do-you-learn-fast-with-videos"></a>Gyorsan tanul videók segítségével?
[A videó](https://mix.office.com/watch/1liuojvdx6sie) hogyan toocopy adatok Azure Storage Blobs és a Data Lake Store használatának ból a DistCp között.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>HDInsight Linux fürtök ból a Distcp használata

HDInsight-fürtök hello ból a Distcp segédprogram, amely lehet használt toocopy adatokat a HDInsight-fürtök különböző forrásokból származnak. Ha konfigurálta a hello HDInsight fürt toouse Data Lake Store további tárterületként, hello ból a Distcp segédprogram lehet használt out-of-az-box toocopy adatok tooand, valamint egy Data Lake Store-fiókból. Ebben a szakaszban úgy tekintünk, hogyan toouse hello ból a Distcp segédprogram.

1. Az asztalról SSH tooconnect toohello-fürt használatára. Lásd: [Connect tooa Linux-alapú HDInsight-fürt](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Hello parancsokat hello SSH parancssorból.

2. Győződjön meg arról, hogy van-e hozzáférési hello Azure Storage Blobs (WASB). Futtassa a következő parancs hello:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Így kell hello tárolási blob tartalmának listáját.
3. Ehhez hasonlóan ellenőrizze, hogy van-e hozzáférési hello Data Lake Store-fiók hello fürtből. Futtassa a következő parancs hello:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Ez biztosítania kell a fájlok és mappák a Data Lake Store-fiók hello listáját.
4. Data Lake Store-fiók WASB tooa ból a Distcp toocopy adatait használják.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Ezzel másolatot készít a hello hello tartalmát **/példa/data/gutenberg/** WASB mappájában túl**/myfolder** a hello Data Lake Store-fiók.
5. Hasonlóképpen használja a Data Lake Store-fiók tooWASB ból a Distcp toocopy adatait.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Ezzel másolatot készít a hello tartalmát **/myfolder** hello Data Lake Store a fiókja túl**/példa/data/gutenberg/** WASB mappájában.

## <a name="performance-considerations-while-using-distcp"></a>Teljesítménnyel kapcsolatos szempontok ból a DistCp használata során

Mivel ból a DistCp tartozó legalacsonyabb granularitási egyetlen fájl, a beállítás hello a példányok maximális száma párhuzamos hello legfontosabb paraméter toooptimize, Data Lake Store ellen. Ez úgy, hogy mappers hello számát szabályozza (a perceké 'M ') hello paraméter. Ez a paraméter, amely lesz használt toocopy adatok mappers hello maximális számát. Alapértelmezett érték 20.

**Példa**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-hello-number-of-mappers-toouse"></a>Hogyan állapítható meg mappers toouse hello száma?

Az alábbiakban olvashat némi útmutatást ezzel kapcsolatban.

* **1. lépés: A teljes YARN memória meghatározása** -hello első lépéseként toodetermine hello YARN memória rendelkezésre álló toohello fürt hello-ból a DistCp feladat futtató. Ez az információ hello Ambari portálon hello-fürthöz tartozó érhető el. Keresse meg a tooYARN és hello Configs lapon toosee hello YARN memória megtekintéséhez. tooget hello teljes YARN memória, többszörösének hello YARN memória mennyisége a csomópontok száma hello rendelkezik a fürtön.

* **2. lépés: Kiszámítása hello mappers** -értékének hello **m** egyenlő toohello hányadosa hello YARN tároló mérete elosztva teljes YARN memória van. hello YARN tároló információi hello Ambari portálon is érhető el. Keresse meg a tooYARN és nézet hello konfigurációk esetén külön-külön hello YARN tároló mérete ebben az ablakban jelenik meg. hello egyenlet tooarrive mappers hello számú (**m**) van

        m = (number of nodes * YARN memory for each node) / YARN container size

**Példa**

Tegyük fel, hogy egy 4 D14v2s csomópontok hello fürt rendelkezik, és 10 TB-os tootransfer próbált adatok 10 mappákból. Hello Mappánkénti változó mennyiségű adatot tartalmaz, és hello fájlméret egyes mappákban lévő eltérőek.

* Teljes YARN memória - hello határozhatja meg, hogy hello YARN memória Ambari portal 96GB D14 csomópont. Igen a teljes YARN memória 4 csomópontot tartalmazó fürtben van: 

        YARN memory = 4 * 96GB = 384GB

* Mappers - hello Ambari portal annak meghatározását, hogy hello YARN tároló mérete 3072 D14 fürtcsomópont esetén a száma. Igen van mappers száma:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Ha más alkalmazásokat memóriát használ, akkor választhatja tooonly használja a fürt YARN memória azon része a ból a DistCp.

### <a name="copying-large-datasets"></a>Nagy adatkészletek másolása

Nagyon nagy hello dataset toobe hello mérete áthelyezésekor (pl. > 1 TB-os), vagy ha több különböző mappa, érdemes lehet használni több ból a DistCp feladat. Valószínűleg nincs nincs jobb teljesítménye, de azt fogja felülbírálásokkal hello feladatok, hogy a feladat sikertelen lesz, ha csak akkor toorestart hello teljes feladatot ahelyett, hogy adott feladat.

### <a name="limitations"></a>Korlátozások

* Ból a DistCp megpróbál toocreate mappers hasonló mérete toooptimize teljesítményét. Mappers hello számának növelése nem mindig növelheti a teljesítményt.

* Ból a DistCp korlátozott tooonly egy leképező fájlonként. Emiatt nem kell további mappers, mint a fájlokat. Ból a DistCp hozzárendelhet egy leképező tooa fájlt, mivel ez használható használt toocopy nagy fájlok feldolgozási hello mennyisége korlátozza.

* Ha kis számú nagy fájlok, akkor kell 256MB fájl adattömbök toogive ossza meg több lehetséges egyidejűségi. 
 
* Az Azure Blob Storage-fiók másolása, a másolási feladat lehet, hogy szabályozva hello blob storage ügyféloldali. Ez csökkenti a másolási feladat hello teljesítményét. További információ az Azure Blob Storage hello korlátai által megszabott toolearn tekintse meg az Azure Storage-korlátok, [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md).

## <a name="see-also"></a>Lásd még:
* [Adatok másolása az Azure Storage Blobs tooData Lake Store-ból](data-lake-store-copy-data-azure-storage-blob.md)
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
