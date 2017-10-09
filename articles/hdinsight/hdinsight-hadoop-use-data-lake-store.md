---
title: Data Lake Store az Azure HDInsight Hadoop aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan tooquery adatokat az Azure Data Lake Store és toostore eredmények elemzéshez."
keywords: "blob storage,hdfs,strukturált adatok,strukturálatlan adatok, data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.openlocfilehash: 89633218a37a2fe05043e05d61199dcc0252d7f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a>A Data Lake Store és az Azure HDInsight-fürtök együttes használata

HDInsight-fürt tooanalyze adatokat, hello adatait tárolhatja vagy a [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), vagy mindkettőt. Mindkét tárolási lehetőségek lehetővé teszik a HDInsight-fürtök toosafely törlése, felhasználói adatok elvesztése nélkül számításhoz használt.

Ebből a cikkből megtudhatja, hogyan használható a Data Lake Store a HDInsight-fürtökkel. toolearn Azure Storage HDInsight-fürtökkel működése lásd [használata Azure Storage Azure hdinsight-fürtök](hdinsight-hadoop-use-blob-storage.md). További információ a HDInsight-fürtök létrehozásáról: [Hadoop-fürtök létrehozása a HDInsightban](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]
> A Data Lake Store elérése mindig biztonságos csatornán keresztül történik, így nincs `adls` fájlrendszer-sémanév. Mindig `adl` használandó.
> 
> 

## <a name="availabilities-for-hdinsight-clusters"></a>Lehetőségek HDInsight-fürtök esetén

A Hadoop hello alapértelmezett fájlrendszer támogatja. hello alapértelmezett fájlrendszer egy alapértelmezett sémát és szolgáltatót jelenti. Használt tooresolve relatív elérési utakat is lehet. Hello HDInsight fürt létrehozási folyamata során az Azure Storage hello alapértelmezett fájlrendszer egy blob-tároló adhat meg, vagy a HDInsight 3.5-ös és újabb verziók, válassza vagy az Azure Storage, vagy az Azure Data Lake Store rendszerként hello alapértelmezett fájlok és a Néhány kivételtől eltekintve. 

A HDInsight-fürtök kétféleképpen használhatják a Data Lake Store-t:

* Hello alapértelmezett tárolóként
* Kiegészítő tárolóként, ahol az Azure Storage Blob az alapértelmezett tároló.

Mostantól típusok/verziók támogatás használatával a Data Lake Store alapértelmezett tároló és a további tárfiókok csak néhány hello HDInsight fürt:

| A HDInsight-fürt típusa | A Data Lake Store az alapértelmezett tároló | A Data Lake Store kiegészítő tároló| Megjegyzések |
|------------------------|------------------------------------|---------------------------------------|------|
| A HDInsight 3.6-os verziója | Igen | Igen | |
| A HDInsight 3.5-ös verziója | Igen | Igen | A HBase hello kivétellel|
| A HDInsight 3.4-es verziója | Nem | Igen | |
| A HDInsight 3.3-as verziója | Nem | Nem | |
| A HDInsight 3.2-es verziója | Nem | Igen | |
| Prémium (szintű) HDInsight| Nem | Nem | |
| Storm | | |A Storm-topológia a Data Lake Store toowrite adatokat is használhatja. A Data Lake Store-ban tárolhatók referenciaadatok is, amelyek olvashatók lesznek egy Storm-topológiából.|

További tárhely fiókként használatával a Data Lake Store nem befolyásolhatja a teljesítményt vagy hello képességét tooread vagy írási tooAzure tárolási hello fürtből.


## <a name="use-data-lake-store-as-default-storage"></a>A Data Lake Store használata az alapértelmezett tárolóként

HDInsight a Data Lake Store alapértelmezett tárolóként telepítésekor hello fürt kapcsolatos fájlok találhatók a Data Lake Store hello a következő helyen:

    adl://mydatalakestore/<cluster_root_path>/

Ha `<cluster_root_path>` egy mappát hoz létre a Data Lake Store hello neve. Egy elérési útjának gyökeréhez használatos egyes fürtök megadásával használhatja ugyanazt a Data Lake Store fiókot egynél több fürt hello. Így olyan beállítással rendelkezhet, ahol:

* Cluster1 használható hello elérési útja`adl://mydatalakestore/cluster1storage`
* Cluster2 használható hello elérési útja`adl://mydatalakestore/cluster2storage`

Figyelje meg, hogy mindkét hello fürtök használata hello azonos Data Lake Store-fiók **mydatalakestore**. Az egyes fürtökön saját Data Lake Store a legfelső szintű fájlrendszer hozzáférés tooits rendelkezik. hello Azure portál üzembe helyezését különösen kéri toouse mappanevet például **/clusters/\<clustername >** hello elérési útjának gyökeréhez tartozó.

toobe képes toouse egy Data Lake Store alapértelmezett tárolóként, engedélyt kell adni a következő elérési utak hello szolgáltatás egyszerű hozzáférés toohello:

- hello Data Lake Store-fiók gyökere.  Példa: adl://mydatalakestore/.
- hello mappa a fürt összes mappa.  Példa: adl://mydatalakestore/clusters.
- hello mappa hello fürthöz.  Példa: adl://mydatalakestore/clusters/cluster1storage.

A szolgáltatásnevek létrehozásával és a hozzáférés biztosításával kapcsolatos további információkért lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](#configure-data-lake-store-access).


## <a name="use-data-lake-store-as-additional-storage"></a>A Data Lake Store használata kiegészítő tárolóként

Data Lake Store további tárolóként hello fürt is használja. Ilyen esetekben hello alapértelmezett fürttároló lehet egy Azure Storage-Blobba vagy egy Data Lake Store-fiók. Ha futtatja a HDInsight-feladatot a Data Lake Store további tárolóként hello adataihoz ellen, hello teljes elérési útja toohello fájlokat kell használnia. Példa:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Vegye figyelembe, hogy nincs nincs **cluster_root_path** most hello URL-címben. Amely az oka Data Lake Store nem alapértelmezett tárolási ebben az esetben, toodo szüksége hello elérési toohello fájlokkal.

toobe képes toouse egy Data Lake Store további tárolóként, csak akkor kell toogrant hello szolgáltatás egyszerű hozzáférés toohello elérési utak hol tárolja a fájlokat.  Példa:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

A szolgáltatásnevek létrehozásával és a hozzáférés biztosításával kapcsolatos további információkért lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](#configure-data-lake-store-access).


## <a name="use-more-than-one-data-lake-store-accounts"></a>Több Data Lake Store-fiók használata

További Data Lake Store-fiók hozzáadása, és több mint egy Data Lake Store hozzáadása fiókok kivitelezése hello HDInsight fürt engedélyt adjon egy vagy több Data Lake Store-fiók lévő adatokon. Lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>A Data Lake Store-hoz történő hozzáférés konfigurálása

tooconfigure Data Lake store hozzáférést a HDInsight-fürtjéhez, rendelkeznie kell egy Azure Active directory (Azure AD) szolgáltatás egyszerű. Kizárólag Azure AD-rendszergazdák hozhatnak létre szolgáltatásnevet. hello szolgáltatás egyszerű tanúsítvánnyal kell létrehozni. További információkért lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access), és [Szolgáltatásnév létrehozása önaláírt tanúsítvánnyal](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]
> Ha azt tervezi, hogy toouse Azure Data Lake Store további tárolóként HDInsight-fürthöz, javasoljuk, hogy ehhez hello fürt létrehozásakor, a cikkben leírtak szerint. Azure Data Lake Store további tárhely tooan hozzáadása meglévő HDInsight-fürt egy összetett folyamat és hibalehetőségeket rejt magában tooerrors.
>

## <a name="access-files-from-hello-cluster"></a>Fájlok elérése hello fürtből

Számos módon el tudja érni a Data Lake Store hello fájlokat a HDInsight-fürtöt.

* **Hello teljesen minősített nevet használó**. Hello teljes elérési útja toohello fájl megadta ezt a módszert, amelyet az tooaccess.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Hello rövidebb elérési utat formátumban**. Ezt a módszert használja, akkor cserélje le hello elérési út mentése toohello fürt legfelső szintű adl: / / /. Igen, a hello a fenti példában helyettesíthetők `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` rendelkező `adl:///`.

        adl:///<file path>

* **Hello relatív elérési út használatával**. Ezt a módszert csak olyan hello relatív elérési út toohello fájl, amelyet az tooaccess. Például ha hello toohello fájl teljes elérési útja a következő:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Van-e hozzáférési hello azonos sample.log fájl helyett a relatív elérési út használatával.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-toodata-lake-store"></a>A HDInsight-fürtök létrehozása a lépjen be az tooData Lake Store

Használja a következő hivatkozások részletes információkra van szüksége a hogyan férhetnek hozzá a HDInsight-fürtök az toocreate tooData Lake Store hello.

* [A Portal használata](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [A PowerShell használata (alapértelmezett tárolóként beállított Data Lake Store-ral)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [A PowerShell használata (kiegészítő tárolóként beállított Data Lake Store-ral)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure-sablonok használata](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan meg toouse HDFS-kompatibilis Azure Data Lake Store a hdinsight eszközzel. Ez lehetővé teszi, hogy toobuild méretezhető, hosszú távú, archiválás beszerzési megoldások kiépítését és -felhasználási HDInsight toounlock hello lévő információkat hello tárolt strukturált és strukturálatlan adatokat.

További információkért lásd:

* [Azure HDInsight – első lépések][hdinsight-get-started]
* [Az Azure Data Lake Store használatának első lépései](../data-lake-store/data-lake-store-get-started-portal.md)
* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]
* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [A Pig használata a HDInsightban][hdinsight-use-pig]
* [Azure Storage megosztott hozzáférési aláírásokkal toorestrict hozzáférés toodata használata a hdinsight eszközzel][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
