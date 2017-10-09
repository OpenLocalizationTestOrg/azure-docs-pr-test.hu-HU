---
title: Data Lake Store az Azure Storage Blobs aaaCopy adatait |} Microsoft Docs
description: "Azure Storage Blobs tooData Lake Store-ból AdlCopy eszköz toocopy adatok használata"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: a3d4172eaefe7395cdef2fff72691bd70f642b78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-from-azure-storage-blobs-toodata-lake-store"></a>Adatok másolása az Azure Storage Blobs tooData Lake Store-ból
> [!div class="op_single_selector"]
> * [A DistCp használata](data-lake-store-copy-data-wasb-distcp.md)
> * [Az AdlCopy használata](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake Store biztosítja a parancssori eszköz [AdlCopy](http://aka.ms/downloadadlcopy), a következő források hello toocopy adatait:

* Tárolóblobokból az Azure Data Lake-tárolóba. Data Lake Store tooAzure tárolóblobokból AdlCopy toocopy adatok nem használhatók.
* Között két Azure Data Lake Store-fiókot.

Emellett eszközzel hello AdlCopy két különböző módban:

* **Önálló**, ahol a hello eszköz használja a Data Lake Store erőforrások tooperform hello feladat.
* **A Data Lake Analytics-fiókkal**, tooyour Data Lake Analytics-fiókhoz hozzárendelt hello egységek esetén használt tooperform hello másolási művelet. Érdemes lehet toouse ezt a beállítást, ha a keresett tooperform hello másolási feladat egy előre jelezhető módon.

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Az Azure Storage Blobs** néhány adat-tárolóban.
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Az Azure Data Lake Analytics fiók (nem kötelező)** -lásd [Ismerkedés az Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) kapcsolatban, hogyan toocreate egy Data Lake tárolásához fiók.
* **AdlCopy eszköz**. Hello AdlCopy eszköz telepítése [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-hello-adlcopy-tool"></a>Hello AdlCopy eszköz szintaxisa
A következő szintaxist toowork hello AdlCopy eszközzel hello használata

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

hello paraméterek hello szintaxisban leírása a következő:

| Beállítás | Leírás |
| --- | --- |
| Forrás |Az Azure storage-blob hello hello forrásadatok hello helyét adja meg. hello forrás lehet egy blob-tároló, egy blobba vagy egy másik Data Lake Store-fiók. |
| Cél |Megadja a hello Data Lake Store cél toocopy számára. |
| SourceKey |Megadja a tárelérési kulcs hello hello az Azure storage blob-forrás. Ez azért szükséges, csak ha hello forrás egy blob-tároló vagy egy blobot. |
| Fiók |**Választható**. Akkor használja, ha azt szeretné, hogy toouse Azure Data Lake Analytics fiók toorun hello másolási feladat. Ha hello /Account beállítást hello szintaxist használják, de nem adja meg a Data Lake Analytics-fiók, AdlCopy használ egy alapértelmezett fiókot toorun hello feladat. Is ha ezt a beállítást használja, hozzá kell adnia hello forrás (Azure Storage-Blobba) és a cél (az Azure Data Lake Store) adatforrásaként a Data Lake Analytics-fiókhoz. |
| egység |A Data Lake Analytics egység, amely jelzi a hello másolási feladat hello számát adja meg. Ez a beállítás nem kötelező, ha hello **/fiók** toospecify hello Data Lake Analytics-fiók lehetőséget. |
| Minta |Adja meg a reguláris kifejezéssel mintát, amely azt jelzi, mely blobokkal vagy a fájlok toocopy. AdlCopy használja a megfelelő kis-és nagybetűket. hello alapértelmezett mintát használható, ha nincs minta toocopy minden elem van megadva. Több fájl minták megadása nem támogatott. |

## <a name="use-adlcopy-as-standalone-toocopy-data-from-an-azure-storage-blob"></a>Az Azure Storage-blobból (önálló) a AdlCopy toocopy adatok használata
1. Nyisson meg egy parancssort, és keresse meg a toohello rendszert tartalmazó könyvtár AdlCopy, általában `%HOMEPATH%\Documents\adlcopy`.
2. Futtassa a következő parancs toocopy hello egy adott blob hello forrás tároló tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Példa:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] a fenti hello szintaxis hello fájlmappa toobe másolt tooa hello Data Lake Store-fiókot határozza meg. AdlCopy eszköz egy mappát hoz létre, ha hello megadott mappa neve nem létezik.

    Fogja felszólító tooenter hello hitelesítő adatai hello, amely alatt a Data Lake Store-fiók rendelkezik Azure-előfizetés. Egy kimeneti hasonló toohello következő jelenik meg:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. Egy tároló Data Lake Store-fiókból toohello hello a következő parancs használatával is másolhatja az összes hello BLOB:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Példa:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>A teljesítménnyel kapcsolatos megfontolások

Az Azure Blob Storage-fiók másolása, a lehetséges, hogy szabályozva, hello blob storage oldalon másolása során. Ez csökkenti a másolási feladat hello teljesítményét. További információ az Azure Blob Storage hello korlátai által megszabott toolearn tekintse meg az Azure Storage-korlátok, [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md).

## <a name="use-adlcopy-as-standalone-toocopy-data-from-another-data-lake-store-account"></a>(A különálló) AdlCopy toocopy adatait egy másik Data Lake Store-fiók használata
AdlCopy toocopy adatok között két Data Lake Store-fiók is használható.

1. Nyisson meg egy parancssort, és keresse meg a toohello rendszert tartalmazó könyvtár AdlCopy, általában `%HOMEPATH%\Documents\adlcopy`.
2. Futtassa a következő parancs toocopy hello egy adott fájlt egy Data Lake Store-fiók tooanother.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Példa:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > a fenti hello szintaxis hello fájlmappa toobe másolt tooa hello cél Data Lake Store-fiók határozza meg. AdlCopy eszköz egy mappát hoz létre, ha hello megadott mappa neve nem létezik.
   >
   >

    Fogja felszólító tooenter hello hitelesítő adatai hello, amely alatt a Data Lake Store-fiók rendelkezik Azure-előfizetés. Egy kimeneti hasonló toohello következő jelenik meg:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. hello következő parancs összes fájlt másolja hello Data Lake Store fiók tooa forrásmappában hello cél Data Lake Store-fiók egy adott mappából.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>A teljesítménnyel kapcsolatos megfontolások

AdlCopy önálló eszközként használatakor hello másolási futtatja megosztott, az Azure által felügyelt erőforrások. Ebben a környezetben programkódjában hello teljesítmény rendszerterhelés és a rendelkezésre álló erőforrások függ. Ebben a módban leginkább kis átvitelek eseti alapon szolgál. Paraméterek nélkül kell toobe beállított AdlCopy önálló eszközként használatakor.

## <a name="use-adlcopy-with-data-lake-analytics-account-toocopy-data"></a>(A Data Lake Analytics-fiók) AdlCopy toocopy adatok használata
Használhatja a Data Lake Analytics toorun hello AdlCopy feladat toocopy adatait az Azure storage-blobok tooData Lake Store fiók. Ha hello adatok toobe áthelyezése gigabájt és terabájt hello körét, és azt szeretné, hogy jobb és kiszámítható teljesítmény átviteli általában használja ezt a beállítást.

az az Azure Storage-Blobból, hello forrás (Azure Storage-Blobba) AdlCopy toocopy a Data Lake Analytics fiókot hozzá kell adni a Data Lake Analytics-fiók adatforrásként toouse. További adatok források tooyour Data Lake Analytics-fiók hozzáadására vonatkozó utasításokért lásd: [Data Lake Analytics kezelése az adatforrások fiók](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).

> [!NOTE]
> A Data Lake Analytics-fiókkal hello forrásaként másolása az Azure Data Lake Store-fiókból, nem kell tooassociate hello Data Lake Store-fiókot a hello Data Lake Analytics-fiók. hello követelmény tooassociate hello forrás tároló hello Data Lake Analytics-fiók csak hello forrás esetén egy Azure Storage-fiókot.
>
>

Futtassa a parancsot toocopy követően az Azure Storage blob tooa Data Lake Store-fiók használatával a Data Lake Analytics-fiók hello:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Példa:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Hasonlóképpen futtassa a hello parancs toocopy az Azure Storage blob tooa Data Lake Store-fiók Data Lake Analytics-fiókkal a következő:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>A teljesítménnyel kapcsolatos megfontolások

Hello közé terabájt adatok másolásakor hatékonyabb és sokkal kiszámíthatóbbá teljesítményt AdlCopy segítségével a saját Azure Data Lake Analytics-fiókkal biztosít. hello paraméter, amely kell kell hangolni Azure Data Lake Analytics egységek toouse hello másolási feladat hello számát jelenti. Hello egységek számának növelésével növeli a másolási feladat hello teljesítményét. Összes másolt fájl toobe használható maximális egységárát. Fájlok másolásának hello számnál több egységek megadása nem növeli a teljesítményt.

## <a name="use-adlcopy-toocopy-data-using-pattern-matching"></a>Használ a mintamegfeleltetés AdlCopy toocopy adatok használata
Ebben a szakaszban megismerheti, hogyan toouse AdlCopy toocopy adatok forrásból (az alábbi példában használjuk Azure Storage-Blobba) tooa Data Lake Store-fiók cél használ a mintamegfeleltetés. Például lépésekkel hello toocopy alatti összes fájl hello forrás blob toohello cél a .csv-bővítménnyel.

1. Nyisson meg egy parancssort, és keresse meg a toohello rendszert tartalmazó könyvtár AdlCopy, általában `%HOMEPATH%\Documents\adlcopy`.
2. Futtassa a következő parancs toocopy hello minden fájl *.csv kiterjesztésű hello forrás tároló tooa Data Lake Store az egy adott blob:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Példa:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Számlázás
* Ha eszközzel hello AdlCopy számlázott önálló kilépő költségek áthelyezésére az adatokat, ha nincs hello forrás Azure Storage-fiók hello ugyanabban a régióban, Data Lake Store hello.
* Ha hello AdlCopy eszköz használata a Data Lake Analytics fiók, szabványos [Data Lake Analytics díjszabás számlázási](https://azure.microsoft.com/pricing/details/data-lake-analytics/) alkalmazza.

## <a name="considerations-for-using-adlcopy"></a>AdlCopy használatának szempontjai
* (Verziójához 1.0.5), AdlCopy támogatja, amelyek együttesen több ezer fájlok és mappák forrásokból származó adatok másolását. Azonban ha problémák nagy dataset másolása, terjesztése hello fájlok és mappák különböző alárendelt mappákba szabadon hello elérési toothose almappák hello forrásként inkább.

## <a name="performance-considerations-for-using-adlcopy"></a>AdlCopy a teljesítménnyel kapcsolatos szempontok

AdlCopy támogatja az adatok másolását a fájlok és mappák ezer tartalmazó. Azonban ha problémák nagy dataset másolása, terjesztheti hello fájlok és mappák kisebb alárendelt mappákba. AdlCopy alkalmi példány lett létrehozva. Ha toocopy adatok ismétlődően, érdemes [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) , amely körül hello másolási műveletek teljes felügyeletet biztosít.

## <a name="release-notes"></a>Kibocsátási megjegyzések
* 1.0.13 - adatok toohello másolása több adlcopy között ugyanazt az Azure Data Lake Store-fiókot a parancsokat, nem kell többé futtatásához a hitelesítő adatokat az egyes tooreenter. Adlcopy között több futtatása most gyorsítótárazhatják az adatokat.

## <a name="next-steps"></a>Következő lépések
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
