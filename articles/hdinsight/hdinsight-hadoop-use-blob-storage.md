---
title: HDFS-kompatibilis aaaQuery adatait az Azure storage - Azure HDInsight |} Microsoft Docs
description: "Ismerje meg, hogyan tooquery adatokat az Azure storage és az Azure Data Lake Store toostore eredmények elemzéshez."
keywords: "blob storage,hdfs,strukturált adatok,strukturálatlan adatok, data lake store,Hadoop-bemenet,Hadoop-kimenet, hadoop-tárolás, hdfs-bemenet,hdfs-kimenet,hdfs-tárolás,wasb azure"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: jgao
ms.openlocfilehash: 1032d60424b65e3c0c54a25c7c15970b017a788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Az Azure Storage és az Azure HDInsight-fürtök együttes használata

HDInsight-fürt tooanalyze adatokat, tárolhatja az adatokat hello, vagy az Azure Storage, az Azure Data Lake Store vagy mindkettőt. Mindkét tárolási lehetőségek lehetővé teszik a HDInsight-fürtök toosafely törlése, felhasználói adatok elvesztése nélkül számításhoz használt.

A Hadoop hello alapértelmezett fájlrendszer támogatja. hello alapértelmezett fájlrendszer egy alapértelmezett sémát és szolgáltatót jelenti. Használt tooresolve relatív elérési utakat is lehet. Során hello HDInsight fürt létrehozásának folyamatát az Azure Storage hello alapértelmezett fájlrendszer egy blob-tároló is megadhat, vagy a HDInsight 3.5 választhat vagy az Azure Storage, vagy az Azure Data Lake Store hello alapértelmezett fájlok rendszert néhány kivételtől eltekintve. Hello támogatásának Data Lake Store használatának hello alapértelmezett és a csatolt tároló is, lásd: [rendelkezésre álló termékek HDInsight-fürthöz](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).

Ebből a cikkből megtudhatja, hogyan használható az Azure Storage a HDInsight-fürtökkel. toolearn Data Lake Store működése HDInsight-fürtökkel, lásd: [használata Azure Data Lake Store az Azure HDInsight-fürtök](hdinsight-hadoop-use-data-lake-store.md). További információ a HDInsight-fürtök létrehozásáról: [Hadoop-fürtök létrehozása a HDInsightban](hdinsight-hadoop-provision-linux-clusters.md).

Az Azure Blob Storage egy robusztus, általános célú tárolómegoldás, amely zökkenőmentesen integrálható a HDInsight eszközzel. HDInsight használható egy blob-tároló az Azure Storage hello alapértelmezett fájlrendszer hello fürtökhöz. Egy Hadoop elosztott fájlrendszer (HDFS) rendszer felületen hello hdinsight összetevők teljes készlete működhet közvetlenül a strukturált vagy strukturálatlan adatok blobként tárolja.

> [!WARNING]
> Az Azure Storage-fiók létrehozása során számos beállítás áll rendelkezésre. a következő táblázat hello információt nyújt a hdinsight eszközzel milyen lehetőségeket támogat:
> 
> | Tárfiók típusa | Tárolási réteg | Támogatott a HDInsightban |
> | ------- | ------- | ------- |
> | Általános célú tárfiókok | Standard | __Igen__ |
> | &nbsp; | Prémium | Nem |
> | Blob Storage-fiók | Gyakori | Nem |
> | &nbsp; | Ritka | Nem |

Nem javasoljuk, hogy hello alapértelmezett blob tárolókat használ üzleti adatok tárolására. Hello alapértelmezett blob tároló törlése után minden használata tooreduce tárolási költségű érdemes. Megjegyzés: hello alapértelmezett tároló tartalmazza az alkalmazás- és naplókat. Győződjön meg arról, hogy tooretrieve hello naplók hello tároló törlése előtt.

Egy blobtároló több fürt közötti megosztása nem támogatott.

## <a name="hdinsight-storage-architecture"></a>HDInsight tároló-architektúra
a következő diagram hello hello Azure Storage használata a HDInsight Tárló-architektúra absztrakt nézetét nyújtja:

![Hadoop-fürtök hello HDFS API-val tooaccess és strukturált és strukturálatlan adatokat a Blob Storage tárolóban tárolja. ] (./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight tároló-architektúra")

HDInsight biztosít hozzáférést toohello elosztott fájlrendszer, amely a helyileg csatlakoztatott toohello számítási csomópontok. Ez a fájlrendszer használatával elérhető például csak a teljesen minősített URI, hello:

    hdfs://<namenodehost>/<path>

Ezenkívül HDInsight lehetővé teszi az Azure Storage tárolt tooaccess adatokat. hello szintaxisa:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Az alábbiakban néhány szempont olvasható Azure Storage-fiók és HDInsight-fürtök együttes használatával kapcsolatban.

* **A tárolók hello storage-fiókok, amelyek csatlakoztatott tooa fürt:** hello fióknevet és kulcsot társított hello fürt létrehozása során, mert rendelkezik teljes körű hozzáférési toohello tárolókban lévő összes blobhoz.

* **Nyilvános tárolók vagy nyilvános blobok a tárfiókokban, amelyek nincsenek csatlakoztatva a tooa fürt:** hello tárolók toohello blobokat olvasási engedéllyel rendelkezik.
  
  > [!NOTE]
  > Nyilvános tárolók lehetővé teszik a tooget, amelyek a tárolóban elérhető, és a tároló metaadatot beszerezni összes BLOB listáját. A nyilvános blobok meg tooaccess hello blobok csak akkor, ha tudja hello pontos URL-cím. További információkért lásd: <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">korlátozzák a hozzáférést toocontainers és blobok</a>.
  > 
  > 
* **Személyes tárolók a tárfiókokban, amelyek nincsenek csatlakoztatva tooa fürt:** hello tárolókban lévő hello blobok nem érhetők el, kivéve, ha hello tárfiók adhat meg hello WebHCat-feladatok elküldésekor. Ennek a magyarázatát a cikk későbbi részében találja.

hello létrehozási folyamata és azok kulcsait meghatározott tárfiókok hello hello fürtcsomópontokon %HADOOP_HOME%/conf/core-site.xml fájlban tárolódnak. HDInsight alapértelmezett viselkedése hello toouse hello hello core-site.xml fájlban meghatározott tárfiókok. A beállítást az [Ambari](./hdinsight-hadoop-manage-ambari.md) használatával módosíthatja

Több WebHCat-feladat (beleértve a Hive, MapReduce, Hadoop-stream és Pig-feladatokat) is tartalmazhatja a tárfiókok és a metaadatok leírását. (Ez jelenleg a tárfiókokkal működik a Pig-feladatokkal, a metaadatokkal nem.) További információkért lásd: [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (HDInsight-fürtök használata alternatív tárfiókokkal és metaadattárakkal).

A blobok a strukturált és strukturálatlan adatokhoz használhatók. A blobtárolók kulcs/érték párokként tárolnak adatokat, és nincs könyvtár-hierarchia. Azonban hello perjelet (/) használhatja hello kulcsnév toomake akkor jelenik meg, mintha a fájl könyvtárszerkezetben tárolódik. Egy blob kulcsa lehet például az *input/log1.txt*. Nem tényleges *bemeneti* könyvtár létezik, de miatt hello perjel karakter hello kulcsnév toohello jelenlétét, egy fájl elérési útját hello megjelenésének rendelkezik.

## <a id="benefits"></a>Az Azure Storage előnyei
hello hallgatólagos teljesítményarányos költsége közös elhelyezéskor számítási fürtök és tárolási erőforrásokat elhárítására hello módon hello számítási fürtök Bezárás toohello tárfiók erőforrásainak belül hello Azure-régió, ahol nagy sebességű hálózati hello teszi jönnek létre. a számítási csomópontok hello hatékonyan tooaccess hello az Azure storage található adatok közül.

Hello adatok tárolása a HDFS helyett az Azure storage társított több előnye is van:

* **Adatok újbóli használata és megosztása:** hello adatokat HDFS-ben hello számítási fürtön belül található. Csak az hello alkalmazásokat, amelyek hozzáférést toohello számítási fürt használhassa a hello adatokat HDFS API-k használatával. hello HDFS API-k vagy hello keresztül elérhető hello adatokat az Azure storage [Blob Storage REST API-k][blob-storage-restAPI]. Emiatt az alkalmazások (beleértve más HDInsight fürtöket) és eszközök nagyobb készlete használt tooproduce kell és hello adatokat.
* **Adatarchiválás:** adatok Azure Storage tárolóban végzett tárolása lehetővé teszi, hogy biztonságosan törölje a felhasználói adatok elvesztése nélkül számítási toobe használt hello a HDInsight-fürtök.
* **Adattárolási költség:** adattárolás DFS-ben, a hosszú távú hello költségesebb, mint az Azure storage hello adatok tárolására, mert a számítási fürt hello költség magasabb, mint az Azure storage hello költségét. Ezenkívül hello adatokat nem kell újra minden számítási fürt létrehozásakor toobe, mert is megtakaríthatja Adatbetöltési költségeket.
* **Rugalmas kibővítés:** Bár a HDFS kibővített fájlrendszert biztosít, hello méretezési hello a fürthöz létrehozott csomópontok száma határozza meg. Hello skála módosítása hello elérhető rugalmas léptékezési képességekre, az Azure storage automatikusan kapott egy bonyolultabb folyamattá válhat.
* **Georeplikáció:** Az Azure Storage tárolókról georeplikálás készíthető. Ez lehetővé teszi földrajzi helyreállítást és adatredundanciát, bár egy feladatátvételi toohello georeplikált helyre súlyos hatással van a teljesítményre, és további költségekkel járhat. Ezért azt javasoljuk, toochoose hello georeplikáció georeplikációt, és csak akkor, ha hello hello adatok értéke érdemes hello további költség nélkül.

Bizonyos MapReduce-feladatok és csomagok akkor is, hogy valóban szeretné toostore az Azure storage köztes eredményeket hozhatnak létre. Ebben az esetben választhatja, amelyeket toostore hello adatok hello helyi HDFS. Valójában a HDInsight a DFS-t használja több ilyen köztes eredményhez a Hive-feladatokban és egyéb folyamatokban.

> [!NOTE]
> A legtöbb HDFS parancs (például az <b>ls</b>, a <b>copyFromLocal</b> és a <b>mkdir</b>) továbbra is a várt módon működik. Csak az adott toohello natív HDFS implementációra (amely hivatkozott tooas DFS), parancsok, mint hello <b>fschk</b> és <b>dfsadmin</b>, az Azure storage másképp viselkednek megjelenítése.
> 
> 

## <a name="create-blob-containers"></a>Blob tárolók létrehozása
toouse blobokat, először létre kell hoznia egy [Azure Storage-fiók][azure-storage-create]. Ennek keretében adja meg az Azure-régió, ahol hello tárfiók létrehozása. hello fürt és hello tárfiókot kell futnia hello ugyanabban a régióban. hello Hive-metaadattár SQL Server adatbázisának és az Oozie metaadattárhoz SQL Server adatbázis is található a hello ugyanabban a régióban.

Akárhol él, mindegyik létrehozott blob tartozik tooa tároló Azure Storage-fiókjában. Ez a tároló egy már létező, a HDInsight eszközön kívül létrejövő blob vagy egy HDInsight-fürthöz létrehozott tároló lehet.

hello alapértelmezett Blob tároló például a feladatelőzményeket és a naplók fürt-specifikus adatait tárolja. Ne osszon meg alapértelmezett Blob tárolókat több HDInsight-fürttel. Ez károsíthatja a feladatelőzményeket. Ajánlott különböző tárolót minden fürt és a megosztott adatok elhelyezése hello alapértelmezett tárfiókot, hanem az összes kapcsolódó fürt üzemelő példányában meghatározott kapcsolt tárfiókra toouse. A kapcsolt tárfiókok konfigurálásáról további információért lásd: [HDInsight-fürtök létrehozása][hdinsight-creation]. Alapértelmezett tárolókat azonban hello eredeti HDInsight fürt törlése után is felhasználhatja. A HBase fürtök esetén megőrizheti hello HBase táblasémát és adatokat hozzon létre egy új HBase fürtöt a törölt HBase fürt által használt hello alapértelmezett blob tárolókat.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-hello-azure-portal"></a>Hello Azure portál használata
Ha a HDInsight-fürtök létrehozása a portál hello, akkor hello lehetőségek (lásd alább), tooprovide hello tárfiókadatok. Megadhatja, hogy kívánja-e egy további tárfiók hello-fürthöz tartozó, és ha igen, a Data Lake Store vagy egy másik Azure Storage-blobba hello további tárolóként választhat.

![HDInsight hadoop létrehozási adatforrás](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> Egy további storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.


### <a name="use-azure-powershell"></a>Azure PowerShell használatával
Ha Ön [telepítette és konfigurálta az Azure PowerShell][powershell-install], használhatja a tárfiók és tároló hello Azure PowerShell Rákérdezés toocreate követően hello:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-cli"></a>Az Azure parancssori felület használatával

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

Ha rendelkezik [telepítette és konfigurálta az Azure parancssori felület hello](../cli-install-nodejs.md), hello következő parancsot a használt tooa tárfiók és tároló lehet.

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> Hello `--type` paraméter azt jelzi, hogyan hello tárfiók replikálódik. További információ: [Azure Storage replication](../storage/storage-redundancy.md) (Az Azure Storage replikációja). Na használjon ZRS-t, mivel a ZRS nem támogatja a lapblobokat, a fájlokat, a táblákat és az üzenetsorokat.
> 
> 

Biztosan felszólító toospecify hello földrajzi régiót, amelyben hello storage-fiók jön létre. A hello készítsen hello storage-fiók ugyanabban a régióban, amely a HDInsight-fürt létrehozását tervezi.

Hello storage-fiók létrehozása után használja a következő parancs tooretrieve hello tárfiókkulcsok hello:

    azure storage account keys list <storageaccountname>

egy tároló toocreate hello a következő parancsot használja:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a>Az Azure Storage tárolóban található címfájlok
hello URI-sémája a HDInsight-ból az Azure storage fájlokhoz való hozzáférést a következő:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

hello URI séma titkosítatlan hozzáférést biztosít (az hello *wasb:* előtag) és SSL titkosított hozzáférést (a *wasbs*). Azt javasoljuk, *wasbs* lehetőség, akkor is, ha elérése során adatokat hello Azure-ban ugyanabban a régióban.

Hello &lt;BlobStorageContainerName&gt; hello blob tároló az Azure storage hello neve azonosítja.
Hello &lt;StorageAccountName&gt; hello Azure Storage-fiók neve azonosítja. Szükség van a teljes tartománynévre (FQDN-re).

Ha sem &lt;BlobStorageContainerName&gt; sem &lt;StorageAccountName&gt; van megadva, hello alapértelmezett fájlrendszert használja. Hello alapértelmezett fájlrendszeren hello fájlok esetén relatív elérési utat vagy abszolút elérési utat is használhatja. Például hello *hadoop-mapreduce-examples.jar* fájl, a HDInsight-fürtök lehet hivatkozott tooby hello következő használatával:

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> hello Fájlnév <i>hadoop-examples.jar</i> HDInsight 2.1-es és 1.6-os fürtökben.
> 
> 

Hello &lt;elérési&gt; hello fájl vagy könyvtár HDFS elérési út. Mivel az Azure Blob Storage tárolóban lévő tárolók egyszerűen kulcs-érték tárolók, nincs valós hierarchikus fájlrendszer. A blob kulcson belüli perjel karaktert ( / ) a rendszer könyvtárelválasztóként értelmezi. Például hello blob neve *hadoop-mapreduce-examples.jar* van:

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> HDInsight eszközön kívüli blobokkal dolgozik, amikor legtöbb segédprogram nem ismeri fel a WASB formátumot hello és helyette, mint az egy alapvető elérési út formátumot várhatóan `example/jars/hadoop-mapreduce-examples.jar`.
> 
> 

## <a name="access-blobs"></a>Blobok elérése 


### <a name="access-blobs-using-azure-powershell"></a> Az Azure PowerShell használata
> [!NOTE]
> Ezen szakasz parancsaiban hello adjon meg egy alapszintű példa blobok tárolt PowerShell tooaccess adatok használatára. A HDInsight használatához testreszabott további teljes körű példa, lásd: hello [a HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).
> 
> 

A következő parancs toolist hello blobbal kapcsolatos parancsmagok hello használata:

    Get-Command *blob*

![A blobbal kapcsolatos PowerShell parancsmagok listája.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Fájlok feltöltése
Lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].

#### <a name="download-files"></a>Fájlok letöltése
hello következő parancsfájl letölti egy blokk blob toohello aktuális mappába. Hello a parancsfájl futtatását, mielőtt módosítani hello tooa könyvtármappát ahol írási jogosultsággal rendelkeznek-e.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # hello storage account used for hello default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # hello default file system container has hello same name as hello cluster.
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    # Use Add-AzureAccount if you haven't connected tooyour Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List hello downloaded file ..." -ForegroundColor Green
    cat "./$blob"

Hello erőforráscsoport-név és hello fürt nevének megadásával, hello a következő kódot használhatja:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a>Fájlok törlése
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a>Fájlok listázása
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Hive-lekérdezések futtatása nem meghatározott tárfiókkal
Ez a példa bemutatja, hogyan toolist során nem meghatározott tárfiókból mappa hello létrehozási folyamat.
$clusterName = "<HDInsightClusterName>"

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a>Az Azure parancssori felület használatával
A következő parancs toolist hello blobbal kapcsolatos parancsok hello használata:

    azure storage blob

**Az Azure parancssori felület tooupload fájl használatának példája**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Az Azure parancssori felület toodownload fájl használatának példája**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Az Azure parancssori felület toodelete fájl használatának példája**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Toolist fájlok az Azure parancssori felület használatának példája**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a>További tárfiókok használata

HDInsight-fürtök létrehozásakor adja meg azt szeretné, hogy azt tooassociate hello Azure Storage-fiók. Ezenkívül toothis tárfiókot, hozzáadhat további tárfiókok a hello azonos Azure-előfizetés vagy különböző Azure-előfizetések hello létrehozási folyamata során, vagy a fürt létrehozása után. Útmutatás további tárfiókok hozzáadásához: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> Egy további storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan meg toouse HDFS-kompatibilis az Azure storage a hdinsight eszközzel. Ez lehetővé teszi, hogy toobuild méretezhető, hosszú távú, archiválás beszerzési megoldások kiépítését és -felhasználási HDInsight toounlock hello lévő információkat hello tárolt strukturált és strukturálatlan adatokat.

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
