---
title: hdinsight Hadoop-feladatokat aaaUpload adatainak |} Microsoft Docs
description: "Ismerje meg, hogyan tooupload és a hozzáférési adatok a Hadoop-feladatokat a Hdinsightban az Azure CLI-t, Azure Tártallózó, Azure PowerShell, hello Hadoop parancssori vagy Sqoop hello."
keywords: "etl hadoop adatok beérkezése hadoop, hadoop adatok betöltése"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: jgao
ms.openlocfilehash: 15da602085d41c19789e34800f3d9e238d7d1de8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Hadoop-feladatok adatainak feltöltése a HDInsightba
Az Azure HDInsight egy teljes körű Hadoop elosztott fájlrendszer (HDFS) biztosítanak Azure Blob Storage tárolóban. Arra tervezték, mint HDFS bővítmény tooprovide zökkenőmentes toocustomers tapasztalhat. Ez lehetővé teszi a hello hello Hadoop ökoszisztémájának toooperate közvetlenül adatokon hello kezeli az összetevők teljes készlete. Az Azure Blob storage és HDFS különböző fájlrendszerek, adatokat és a számítások az adatok tárolására vannak optimalizálva. Hello előnyeit az Azure Blob storage használatával információ: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].

**Előfeltételek**

Vegye figyelembe a következő követelmény, mielőtt elkezdené hello:

* Egy Azure HDInsight-fürtre. Útmutatásért lásd: [Ismerkedés az Azure HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision].

## <a name="why-blob-storage"></a>Miért blob storage?
Az Azure HDInsight fürtök jellemzően toorun MapReduce-feladatok telepítve, és ezek a feladatok befejezése után hello fürtök eldobott. Gondoskodik a hello adatok hello HDFS fürtök számítások befejezését követően ezek az adatok lenne egy drága módon toostore. Az Azure Blob storage egy magas rendelkezésre állású, nagymértékben méretezhető, nagy kapacitás, a HDInsight eszközzel feldolgozott toobe adatok alacsony költségű, és megosztható tárolási lehetőség. Adattárolás egy blobba lehetővé teszi, hogy a hello HDInsight fürtöket a számítási toobe biztonságosan kiadott adatok elvesztése nélkül használt.

### <a name="directories"></a>Könyvtárak
Az Azure Blob storage tárolók kulcs/érték párként adatok tárolásához, és nincs könyvtár-hierarchia. Azonban hello "/" karaktert használhatja hello kulcsnév toomake akkor jelenik meg, mintha a fájl könyvtárszerkezetben tárolódik. HDInsight ezek látja, mintha tényleges könyvtárak is.

Egy blob kulcsa lehet például az *input/log1.txt*. Nem tényleges "bemeneti" könyvtár létezik, de hello toohello jelenléte miatt "/" karakter hello kulcsnév van hello megjelenésének egy fájl elérési útját.

Ebből kifolyólag Azure Explorer eszközök használatakor előfordulhat néhány 0 bájt fájlt. Ezeket a fájlokat két célokra szolgál:

* Ha üres mappák, azokat megjelölni hello létezés hello mappa. Az Azure Blob storage, amely egy blob PEL/sáv nevű létezik, nincs-e egy nevű mappát elég intelligens tooknow **PEL**. Azonban csak úgy toosignify egy üres mappa neve a hello **PEL** azzal, hogy ez a különleges 0 bájt fájl érvényben van.
* Tartanak fenn speciális metaadatok szükséges hello Hadoop által fájlrendszer, nevezetesen hello engedélyek és hello mappák tulajdonosait.

## <a name="command-line-utilities"></a>Parancssori segédprogramok
A Microsoft hello segédprogramok toowork az Azure Blob storage a következő biztosít:

| Eszköz | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure parancssori felület][azurecli] |✔ |✔ |✔ |
| [Az Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] | | |✔ |
| [Hadoop parancs](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Külső Azure-ban Hadoop parancs csak érhető el hello HDInsight-fürtre, és csak az adatok betöltése a helyi fájlrendszer hello Azure Blob Storage tárolóban történő lehetővé hello közben hello Azure CLI-t, az Azure PowerShell és az AzCopy összes is használható.
>
>

### <a id="xplatcli"></a>Az Azure parancssori felület
hello Azure CLI az lehetővé teszi a toomanage Azure platformfüggetlen eszközzel szolgáltatások. A következő lépéseket tooupload adatok tooAzure Blob-tároló hello használata:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Telepítse és konfigurálja a hello Azure parancssori felület Mac, Linux és Windows](../cli-install-nodejs.md).
2. Nyisson meg egy parancssort, bash vagy más rendszerhéj, és használja a következő tooauthenticate tooyour Azure-előfizetés hello.

        azure login

    Amikor a rendszer kéri, adja meg hello felhasználónevet és jelszót az előfizetéséhez.
3. Adja meg a következő parancs toolist hello tárfiókok az előfizetéshez tartozó hello:

        azure storage account list
4. Válassza ki, amely tartalmazza azt szeretné, hogy a toowork hello blob hello tárfiókot, majd a következő parancs tooretrieve hello kulcs ehhez a fiókhoz hello használata:

        azure storage account keys list <storage-account-name>

    Ez visszaadja **elsődleges** és **másodlagos** kulcsok. Másolás hello **elsődleges** kulcs értékét, mert fogja használni a következő lépésekben hello.
5. A következő parancs tooretrieve hello tárfiókon belül blob tárolók listája hello használata:

        azure storage container list -a <storage-account-name> -k <primary-key>
6. A következő parancsok tooupload hello használja, és töltse le a fájlokat toohello blob:

   * a fájl tooupload:

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * a fájl toodownload:

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> Ha lesz mindig kell dolgozik hello azonos tárfiókot, beállíthatja a következő környezeti változók hello fiók megadása helyett hello és billentyűt minden parancs:
>
> * **AZURE\_tárolási\_fiók**: hello tárfiók neve
> * **AZURE\_tárolási\_hozzáférés\_kulcs**: hello tárfiók kulcsa
>
>

### <a id="powershell"></a>Az Azure PowerShell
Az Azure PowerShell egy parancsfájl-kezelési környezet, hogy toocontrol használ, és hello telepítése és kezelése az Azure-ban a feladatok automatizálásához. A munkaállomás toorun Azure PowerShell konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**egy helyi fájl tooAzure Blob-tároló tooupload**

1. Megnyitás hello Azure PowerShell-konzolon útmutatását [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).
2. Hello hello értékének első öt változók beállítása a következő parancsfájl hello:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get hello storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create hello storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy hello file from local workstation toohello Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. Beillesztés hello parancsfájlt a hello Azure PowerShell konzol toorun azt toocopy hello.

Például a PowerShell parancsfájlok toowork a hdinsight eszközzel, lásd: [a HDInsight tools](https://github.com/blackmist/hdinsight-tools).

### <a id="azcopy"></a>AzCopy
AzCopy egy parancssori eszköz, amely tervezett toosimplify hello feladatát adatátvitel esetében bejövő és kimenő egy Azure Storage-fiókot. Önálló eszközként használható, vagy az eszköz már meglévő alkalmazás tartalmaznia. [Töltse le az AzCopy][azure-azcopy-download].

hello AzCopy szintaxisa a következő:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

További információkért lásd: [AzCopy - feltöltése/letöltése fájlok Azure Blobokra vonatkozó][azure-azcopy].

### <a id="commandline"></a>Hadoop parancssor
hello Hadoop parancssori csak akkor hasznos, ha hello adat már szerepel a hello átjárócsomóponthoz adatok tárolásához a blob-tárolóba.

A sorrend toouse hello Hadoop parancs először kapcsolódnia toohello headnode hello a következő módszerek egyikével:

* **Windows-alapú HDInsight**: [csatlakozzon a távoli asztali kapcsolattal](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **Linux-alapú HDInsight**: SSH protokoll használatával kapcsolódó levelezőprogramokkal ([SSH-parancstól hello](hdinsight-hadoop-linux-use-ssh-unix.md) vagy [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))

A csatlakozás után a következő szintaxist tooupload egy fájl toostorage hello is használhatja.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Például: `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Mivel hello alapértelmezett fájlrendszer a HDInsight az Azure Blob Storage tárolóban, /example/data.txt valójában az Azure Blob Storage tárolóban. Tekintse meg a toohello fájlt is:

    wasb:///example/data/data.txt

vagy

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Más Hadoop listáját, hogy a munkahelyi fájlok parancsok, lásd: [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> A HBase fürtök esetén hello alapértelmezett blokkméret használható, ha a adatainak írása 256KB. Ez a HBase API-k és a REST API-k használata remekül működik, amíg használatával hello `hadoop` vagy `hdfs dfs` parancsok toowrite adatok ~ 12 GB-nál nagyobb hibát eredményez. Lásd: hello [írás a blob storage-hibát](#storageexception) szakasz alatt további információt.
>
>

## <a name="graphical-clients"></a>Grafikus ügyfelek
Van még több alkalmazás, amely a grafikus felületet biztosít az Azure Storage. hello az alábbiakban néhány ezeket az alkalmazásokat a listája látható:

| Ügyfél | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [A Microsoft Visual Studio Tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |✔ |✔ |✔ |
| [Azure Storage Explorer](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Felhőalapú tárolás Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Az Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

### <a name="visual-studio-tools-for-hdinsight"></a>HDInsight Visual Studio eszközök
További információkért lásd: [Navigálás hello kapcsolódó erőforrások](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

### <a id="storageexplorer"></a>Az Azure Storage Explorer
*Az Azure Tártallózó* hasznos eszköz a Kérem, és módosítsa a blobok hello adatokat. Ingyenes, nyissa meg a forrás eszköz, amely letölthető [http://storageexplorer.com/](http://storageexplorer.com/). hello a forráskód nem érhető el ez a hivatkozás.

Hello eszköz használata előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs. Az alábbi információk eljussanak vonatkozó utasításokért lásd: hello "hogyan: megtekintése, másolása és újragenerálása tárolási hívóbetűk" szakasza [létrehozása, kezelése vagy törlése a tárfiók][azure-create-storage-account].

1. Az Azure Storage-kezelő futtatása. Ha hello első alkalommal hello Tártallózó futtatása, akkor a rendszer bekéri a hello **tá_rolási fióknév** és **Tárfiók kulcsa**. Futtatásakor az előtt, használja a hello **Hozzáadás** tooadd egy új tárolási fióknevet és kulcsot gombra.

    Adja meg a név és kulcs a HDInsight-fürt által használt hello tárfiók, és adja hello **MENTÉS és MEGNYITÁS**.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. Tárolók toohello balra hello felület hello listájában kattintson a HDInsight-fürthöz társított hello tároló hello neve. Alapértelmezés szerint ez hello hello HDInsight-fürt nevét, de eltérő, ha hello fürt létrehozásakor adja meg az adott név lehet.
3. Hello eszköztáron jelölje ki hello Feltöltés ikonra.

    ![A feltöltés ikonja kiemelve eszköztár](./media/hdinsight-upload-data/toolbar.png)
4. Adjon meg egy fájl tooupload, és kattintson a **nyitott**. Amikor a rendszer kéri, válassza ki a **feltöltése** tooupload hello fájl toohello gyökere hello tároló. Ha azt szeretné, hogy tooupload hello tooa megadott elérési útja, adja meg hello elérési hello **cél** mezőbe, majd válassza ki **feltöltése**.

    ![Fájl feltöltése párbeszédpanel](./media/hdinsight-upload-data/fileupload.png)

    Hello fájl befejezte a feltöltést, amint a HDInsight-fürt hello feladatokból használhatja.

## <a name="mount-azure-blob-storage-as-local-drive"></a>Helyi meghajtó csatlakoztatási Azure Blob Storage tárolót
Lásd: [csatlakoztatási Azure Blob Storage tárolót helyi meghajtó](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="services"></a>Szolgáltatások
### <a name="azure-data-factory"></a>Azure Data Factory
hello Azure Data Factory szolgáltatásnak egy teljes körűen felügyelt szolgáltatás zökkenőmentes, méretezhető és megbízható éles adatcsatornák a tárolás, az adatfeldolgozás és adatok adatátviteli szolgáltatások létrehozására.

Az Azure Data Factory lehet használt toomove adatok Azure Blob Storage tárolóban, vagy toocreate adatok folyamatok közvetlenül használni, mint a szolgáltatások HDInsight Hive és a sertésfelmérés.

További információkért lásd: hello [Azure Data Factory dokumentáció](https://azure.microsoft.com/documentation/services/data-factory/).

### <a id="sqoop"></a>Apache Sqoop
Sqoop egy eszköz tootransfer adatokat Hadoop és a relációs adatbázisok között. Használat tooimport adatait egy relációs adatbázis-kezelő rendszerének (RDBMS), például az SQL Server, MySQL, vagy hello Hadoop elosztott fájlrendszer (HDFS), az Oracle hello adatok a Hadoop MapReduce vagy a Hive, majd exportálja újra hello adatok egy RDBMS.

További információkért lásd: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop].

## <a name="development-sdks"></a>Fejlesztési SDK-k
Az Azure Blob storage használata az Azure SDK-t a következő programozási nyelvek hello is elérhető:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Hello Azure SDK-k telepítésével kapcsolatos további információkért lásd: [Azure tölti le.](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Hibaelhárítás
### <a id="storageexception"></a>Írás a blob Storage-hibát
**A jelenség**: hello használatakor `hadoop` vagy `hdfs dfs` toowrite fájlok parancsok is ~ 12 GB vagy nagyobb a HBase-fürtöt, felmerülhet a következő hiba hello:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: hello request body is too large and exceeds hello maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**OK**: a HBase a HDInsight-fürtök alapértelmezett tooa 256 KB-os blokkméret tooAzure tárolási írása közben. Amíg ez működik, a HBase API-k vagy a REST API-k, okoz hiba hello használatakor `hadoop` vagy `hdfs dfs` parancssori segédprogramok.

**Megoldási**: használata `fs.azure.write.request.size` toospecify a nagyobb blokkméret. Ehhez a használati alapon hello segítségével `-D` paraméter. hello az alábbiakban látható egy példa, ez a paraméter használata hello `hadoop` parancs:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Növelje a hello értékének `fs.azure.write.request.size` globálisan Ambari használatával. hello lépések lehet toochange hello érték szerepel hello Ambari webes felhasználói felületén:

1. A böngészőben nyissa meg a fürt Ambari webes felhasználói felületén toohello. Ez a https://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello a fürt neve van.

    Amikor a rendszer kéri, adja meg hello admin nevét és jelszavát hello fürt.
2. Hello bal oldalán található üdvözlő képernyőt, válassza ki **HDFS**, majd válassza ki a hello **Configs** fülre.
3. A hello **szűrő...**  mezőbe írja be `fs.azure.write.request.size`. Ez megjeleníti hello mező és az aktuális érték hello lap hello közepén.
4. Új érték 262144 (256KB) toohello hello érték módosítása Például 4194304 (4MB).

![Ambari webes felhasználói felületén keresztül hello érték módosítása képe](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

További információ az Ambari használatával, lásd: [hello Ambari webes felhasználói felület használatával kezelheti a HDInsight-fürtök](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerkedett hogyan tooget adatok HDInsight, olvassa el a következő cikkek toolearn hogyan hello tooperform analysis:

* [Azure HDInsight – első lépések][hdinsight-get-started]
* [Hadoop-feladatokat programozott módon küldhet][hdinsight-submit-jobs]
* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [A Pig használata a HDInsightban][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
