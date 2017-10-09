---
title: "aaaGet használatába a HDInsight - Azure HBase például |} Microsoft Docs"
description: "Hajtsa végre az Apache HBase példa toostart a HDInsight hadoop használatával. A HBase rendszerhéjjal hello táblák létrehozása, és lekérdezheti Hive eszközzel."
keywords: "hbasecommand,hbase példa"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 43419780142b320b16180a2b1f25020dee2f7a11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a><span data-ttu-id="1f2f7-105">Bevezetés a HDInsight egy Apache HBase-példájába</span><span class="sxs-lookup"><span data-stu-id="1f2f7-105">Get started with an Apache HBase example in HDInsight</span></span>

<span data-ttu-id="1f2f7-106">Ismerje meg, hogyan toocreate a HDInsight HBase-fürtöt létre HBase táblákat és lekérdezéstáblákat a Hive.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-106">Learn how toocreate an HBase cluster in HDInsight, create HBase tables, and query tables by using Hive.</span></span> <span data-ttu-id="1f2f7-107">A HBase-re vonatkozó általános információért lásd: [HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-107">For general HBase information, see [HDInsight HBase overview][hdinsight-hbase-overview].</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a><span data-ttu-id="1f2f7-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1f2f7-108">Prerequisites</span></span>
<span data-ttu-id="1f2f7-109">A HBase példa megkísérlése előtt a következő elemek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-109">Before you begin trying this HBase example, you must have hello following items:</span></span>

* <span data-ttu-id="1f2f7-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-110">**An Azure subscription**.</span></span> <span data-ttu-id="1f2f7-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="1f2f7-112">[Biztonságos rendszerhéj (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> 
* <span data-ttu-id="1f2f7-113">[curl](http://curl.haxx.se/download.html).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-113">[curl](http://curl.haxx.se/download.html).</span></span>

## <a name="create-hbase-cluster"></a><span data-ttu-id="1f2f7-114">HBase-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f2f7-114">Create HBase cluster</span></span>
<span data-ttu-id="1f2f7-115">hello alábbi eljárás egy Azure Resource Manager sablon toocreate 3.4-es verziójú Linux-alapú HBase fürt és hello függő alapértelmezett Azure Storage-fiókot használ.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-115">hello following procedure uses an Azure Resource Manager template toocreate a version 3.4 Linux-based HBase cluster and hello dependent default Azure Storage account.</span></span> <span data-ttu-id="1f2f7-116">hello eljárás és egyéb Fürtlétrehozási módszerek használt toounderstand hello paraméterek lásd [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-116">toounderstand hello parameters used in hello procedure and other cluster creation methods, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="1f2f7-117">Kattintson a következő kép tooopen hello sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-117">Click hello following image tooopen hello template in hello Azure portal.</span></span> <span data-ttu-id="1f2f7-118">hello sablon a következő nyilvános blobtárolóban található.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-118">hello template is located in a public blob container.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="1f2f7-119">A hello **egyéni telepítési** panelen adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-119">From hello **Custom deployment** blade, enter hello following values:</span></span>
   
   * <span data-ttu-id="1f2f7-120">**Előfizetés**: válassza ki az Azure-előfizetéssel, amely használt toocreate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-120">**Subscription**: Select your Azure subscription that is used toocreate hello cluster.</span></span>
   * <span data-ttu-id="1f2f7-121">**Erőforráscsoport**: Hozzon létre egy Azure Resource Management-csoportot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-121">**Resource group**: Create an Azure Resource Management group or use an existing one.</span></span>
   * <span data-ttu-id="1f2f7-122">**Hely**: hello erőforráscsoport hello helyének megadása.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-122">**Location**: Specify hello location of hello resource group.</span></span> 
   * <span data-ttu-id="1f2f7-123">**ClusterName**: Adjon meg egy nevet hello HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-123">**ClusterName**: Enter a name for hello HBase cluster.</span></span>
   * <span data-ttu-id="1f2f7-124">**A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az **admin**.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-124">**Cluster login name and password**: hello default login name is **admin**.</span></span>
   * <span data-ttu-id="1f2f7-125">**SSH-felhasználónév és jelszó**: hello alapértelmezett felhasználónév az **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-125">**SSH username and password**: hello default username is **sshuser**.</span></span>  <span data-ttu-id="1f2f7-126">Ezt át lehet nevezni.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-126">You can rename it.</span></span>
     
     <span data-ttu-id="1f2f7-127">Más paraméterek opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-127">Other parameters are optional.</span></span>  
     
     <span data-ttu-id="1f2f7-128">Minden egyes fürt az Azure Storage-fióktól függ.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-128">Each cluster has an Azure Storage account dependency.</span></span> <span data-ttu-id="1f2f7-129">Ha töröl egy fürtöt, hello adatok megmaradnak, hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-129">After you delete a cluster, hello data retains in hello storage account.</span></span> <span data-ttu-id="1f2f7-130">hello fürt alapértelmezett tárfiókneve hello fürt neve a "store" kifejezéssel kiegészítve.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-130">hello cluster default storage account name is hello cluster name with "store" appended.</span></span> <span data-ttu-id="1f2f7-131">Szoftveresen kötött a sablonváltozók szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-131">It is hardcoded in hello template variables section.</span></span>
3. <span data-ttu-id="1f2f7-132">Válassza ki **toohello feltételek és kikötések fenti elfogadom**, és kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-132">Select **I agree toohello terms and conditions stated above**, and then click **Purchase**.</span></span> <span data-ttu-id="1f2f7-133">Körülbelül 20 percet toocreate fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-133">It takes about 20 minutes toocreate a cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="1f2f7-134">HBase-fürtök törlése után egy másik HBase-fürtöt hello használatával létrehozhat alapértelmezett blob tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-134">After an HBase cluster is deleted, you can create another HBase cluster by using hello same default blob container.</span></span> <span data-ttu-id="1f2f7-135">Új fürt hello szerzi be hello eredeti fürtben létrehozott hello HBase táblákat.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-135">hello new cluster picks up hello HBase tables you created in hello original cluster.</span></span> <span data-ttu-id="1f2f7-136">tooavoid inkonzisztenciákat, ajánlott letiltani hello HBase táblákat hello fürt törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-136">tooavoid inconsistencies, we recommend that you disable hello HBase tables before you delete hello cluster.</span></span>
> 
> 

## <a name="create-tables-and-insert-data"></a><span data-ttu-id="1f2f7-137">Táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="1f2f7-137">Create tables and insert data</span></span>
<span data-ttu-id="1f2f7-138">SSH tooconnect tooHBase fürtök használata, és használhatja a HBase rendszerhéjjal toocreate HBase táblákat, adatok, illetve adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-138">You can use SSH tooconnect tooHBase clusters and then use HBase Shell toocreate HBase tables, insert data, and query data.</span></span> <span data-ttu-id="1f2f7-139">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-139">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="1f2f7-140">A legtöbbek számára adatokat hello táblázatos formátumban jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-140">For most people, data appears in hello tabular format:</span></span>

![HDInsight HBase táblázatos adatok][img-hbase-sample-data-tabular]

<span data-ttu-id="1f2f7-142">A HBase (BigTable megvalósítása), hello azonos adatok láthatóhoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-142">In HBase (an implementation of BigTable), hello same data looks like:</span></span>

![HDInsight HBase BigTable-adatok][img-hbase-sample-data-bigtable]


<span data-ttu-id="1f2f7-144">**toouse hello HBase rendszerhéj**</span><span class="sxs-lookup"><span data-stu-id="1f2f7-144">**toouse hello HBase shell**</span></span>

1. <span data-ttu-id="1f2f7-145">Az SSH-ból futtassa a következő HBase parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-145">From SSH, run hello following HBase command:</span></span>
   
    ```bash
    hbase shell
    ```

2. <span data-ttu-id="1f2f7-146">Hozzon létre egy HBase-t kétoszlopos családokkal:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-146">Create an HBase with two-column families:</span></span>

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. <span data-ttu-id="1f2f7-147">Adatok beszúrása:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-147">Insert some data:</span></span>
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase-rendszerhéj][img-hbase-shell]
4. <span data-ttu-id="1f2f7-149">Egyetlen sor lekérése</span><span class="sxs-lookup"><span data-stu-id="1f2f7-149">Get a single row</span></span>
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    <span data-ttu-id="1f2f7-150">Azonos eredmények hello látnak kell hello vizsgálat parancs használatával, mert csak egy sorból áll.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-150">You shall see hello same results as using hello scan command because there is only one row.</span></span>
   
    <span data-ttu-id="1f2f7-151">Hello HBase táblasémát kapcsolatos további információkért lásd: [bemutatása tooHBase Schema Design][hbase-schema].</span><span class="sxs-lookup"><span data-stu-id="1f2f7-151">For more information about hello HBase table schema, see [Introduction tooHBase Schema Design][hbase-schema].</span></span> <span data-ttu-id="1f2f7-152">További Hbase-parancsokért lásd: [Apache HBase reference guide][hbase-quick-start] (Apache HBase referencia-útmutató).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-152">For more HBase commands, see [Apache HBase reference guide][hbase-quick-start].</span></span>
5. <span data-ttu-id="1f2f7-153">Kilépés hello rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="1f2f7-153">Exit hello shell</span></span>
   
    ```hbaseshell
    exit
    ```

<span data-ttu-id="1f2f7-154">**toobulk adatok betöltése hello névjegyek HBase táblájába**</span><span class="sxs-lookup"><span data-stu-id="1f2f7-154">**toobulk load data into hello contacts HBase table**</span></span>

<span data-ttu-id="1f2f7-155">A HBase több módszert tartalmaz az adatok táblába töltéséhez.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-155">HBase includes several methods of loading data into tables.</span></span>  <span data-ttu-id="1f2f7-156">További információ: [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load) (Kötegelt betöltés).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-156">For more information, see [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load).</span></span>

<span data-ttu-id="1f2f7-157">Egy minta adatfájl található a következő nyilvános blobtárolóban található: *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-157">A sample data file can be found in a public blob container, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span></span>  <span data-ttu-id="1f2f7-158">hello hello adatfájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-158">hello content of hello data file is:</span></span>

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

<span data-ttu-id="1f2f7-159">Lehetősége van hozzon létre egy szövegfájlt, és töltse fel a hello fájl tooyour saját tárfiók.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-159">You can optionally create a text file and upload hello file tooyour own storage account.</span></span> <span data-ttu-id="1f2f7-160">Hello útmutatásért lásd: [feltölteni az adatokat a HDInsight Hadoop-feladatok][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="1f2f7-160">For hello instructions, see [Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data].</span></span>

> [!NOTE]
> <span data-ttu-id="1f2f7-161">Ez az eljárás során létrehozott az utolsó eljárás hello hello Contacts HBase táblát használja.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-161">This procedure uses hello Contacts HBase table you have created in hello last procedure.</span></span>
> 

1. <span data-ttu-id="1f2f7-162">Az SSH-ból futtassa a következő parancs tootransform hello tooStoreFiles fájlt, és a Dimporttsv.bulk.output által meghatározott relatív elérési úton tárolja hello.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-162">From SSH, run hello following command tootransform hello data file tooStoreFiles and store at a relative path specified by Dimporttsv.bulk.output.</span></span>  <span data-ttu-id="1f2f7-163">Ha a HBase rendszerhéjban, használja a hello kilépési parancs tooexit.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-163">If you are in HBase Shell, use hello exit command tooexit.</span></span>

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. <span data-ttu-id="1f2f7-164">Futtassa a következő parancs tooupload hello adatok /example/data/storeDataFileOutput toohello HBase tábla hello:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-164">Run hello following command tooupload hello data from  /example/data/storeDataFileOutput toohello HBase table:</span></span>
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. <span data-ttu-id="1f2f7-165">Nyissa meg a HBase rendszerhéjjal hello és hello vizsgálat parancs toolist hello tábla tartalmát használja.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-165">You can open hello HBase shell, and use hello scan command toolist hello table content.</span></span>

## <a name="use-hive-tooquery-hbase"></a><span data-ttu-id="1f2f7-166">Hive tooquery HBase használata</span><span class="sxs-lookup"><span data-stu-id="1f2f7-166">Use Hive tooquery HBase</span></span>

<span data-ttu-id="1f2f7-167">A HBase táblákban lévő adatokat a Hive eszközzel kérdezheti le.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-167">You can query data in HBase tables by using Hive.</span></span> <span data-ttu-id="1f2f7-168">Ebben a szakaszban hoz létre olyan Hive táblát, amely leképezhető toohello HBase tábla és a HBase táblában tooquery hello adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-168">In this section, you create a Hive table that maps toohello HBase table and uses it tooquery hello data in your HBase table.</span></span>

1. <span data-ttu-id="1f2f7-169">Nyissa meg **PuTTY**, és csatlakoztassa toohello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-169">Open **PuTTY**, and connect toohello cluster.</span></span>  <span data-ttu-id="1f2f7-170">Lásd: hello hello az előző eljárás utasításait.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-170">See hello instructions in hello previous procedure.</span></span>
2. <span data-ttu-id="1f2f7-171">Hello SSH-munkamenetet használja a következő parancs toostart Beeline hello:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-171">From hello SSH session, use hello following command toostart Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="1f2f7-172">A Beeline-nal kapcsolatos további információkért lásd [a Hive és a Hadoop együttes, a Beeline-nal történő használatát a HDInsightban](hdinsight-hadoop-use-hive-beeline.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-172">For more information about Beeline, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
       
3. <span data-ttu-id="1f2f7-173">Futtassa a következő HiveQL-parancsfájlt toocreate hello olyan Hive táblát, amely leképezhető toohello HBase tábla.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-173">Run hello following HiveQL script  toocreate a Hive table that maps toohello HBase table.</span></span> <span data-ttu-id="1f2f7-174">Győződjön meg arról, hogy létrehozta hello HBase rendszerhéj használatával, az utasítás futtatása előtt az oktatóanyag korábbi részében hivatkozott mintatáblát hello.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-174">Make sure that you have created hello sample table referenced earlier in this tutorial by using hello HBase shell before you run this statement.</span></span>

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. <span data-ttu-id="1f2f7-175">Futtassa a következő HiveQL parancsfájl tooquery hello adatok hello HBase tábla hello:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-175">Run hello following HiveQL script tooquery hello data in hello HBase table:</span></span>

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a><span data-ttu-id="1f2f7-176">HBase REST API-k használata Curl használatával</span><span class="sxs-lookup"><span data-stu-id="1f2f7-176">Use HBase REST APIs using Curl</span></span>

<span data-ttu-id="1f2f7-177">hello REST API védelméről [az egyszerű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-177">hello REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="1f2f7-178">Kérések mindig biztonságos HTTP (HTTPS) toohelp győződjön meg arról, hogy a hitelesítő adatait biztonságos módon küldje toohello server kell végezzen.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-178">You shall always make requests by using Secure HTTP (HTTPS) toohelp ensure that your credentials are securely sent toohello server.</span></span>

2. <span data-ttu-id="1f2f7-179">A következő parancs toolist hello meglévő HBase táblák hello használata:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-179">Use hello following command toolist hello existing HBase tables:</span></span>

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. <span data-ttu-id="1f2f7-180">A következő parancs toocreate egy új HBase tábla két oszlopcsaláddal hello használata:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-180">Use hello following command toocreate a new HBase table with two-column families:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    <span data-ttu-id="1f2f7-181">hello séma hello JSon formátumban van megadva.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-181">hello schema is provided in hello JSon format.</span></span>
4. <span data-ttu-id="1f2f7-182">Használja a következő parancs tooinsert hello néhány adat:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-182">Use hello following command tooinsert some data:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    <span data-ttu-id="1f2f7-183">Meg kell, hogy base64 kódolása hello -d kapcsolón megadott hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-183">You must base64 encode hello values specified in hello -d switch.</span></span> <span data-ttu-id="1f2f7-184">Hello példa:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-184">In hello example:</span></span>
   
   * <span data-ttu-id="1f2f7-185">MTAwMA==: 1000</span><span class="sxs-lookup"><span data-stu-id="1f2f7-185">MTAwMA==: 1000</span></span>
   * <span data-ttu-id="1f2f7-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span><span class="sxs-lookup"><span data-stu-id="1f2f7-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span></span>
   * <span data-ttu-id="1f2f7-187">Sm9obiBEb2xl: John Dole</span><span class="sxs-lookup"><span data-stu-id="1f2f7-187">Sm9obiBEb2xl: John Dole</span></span>
     
     <span data-ttu-id="1f2f7-188">[hamis sorkulcsa](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) tooinsert lehetővé teszi több (kötegelt) értéket.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) allows you tooinsert multiple (batched) values.</span></span>
5. <span data-ttu-id="1f2f7-189">A következő parancs tooget sor hello használata:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-189">Use hello following command tooget a row:</span></span>
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

<span data-ttu-id="1f2f7-190">További információ a HBase REST-ről: [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest) (Apache HBase referencia-útmutató).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-190">For more information about HBase Rest, see [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest).</span></span>

> [!NOTE]
> <span data-ttu-id="1f2f7-191">A HBase nem támogatja a Thriftet a HDInsightban.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-191">Thrift is not supported by HBase in HDInsight.</span></span>
>
> <span data-ttu-id="1f2f7-192">Használata Curl vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello felhasználónevet és jelszót hello HDInsight fürt rendszergazdája által.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-192">When using Curl or any other REST communication with WebHCat, you must authenticate hello requests by providing hello user name and password for hello HDInsight cluster administrator.</span></span> <span data-ttu-id="1f2f7-193">Hello egységes erőforrás-azonosító (URI) részeként használt toosend hello kérelmek toohello server hello fürtnév is kell használnia:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-193">You must also use hello cluster name as part of hello Uniform Resource Identifier (URI) used toosend hello requests toohello server:</span></span>
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    <span data-ttu-id="1f2f7-194">Egy hasonló toohello választ, a következő választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-194">You should receive a response similar toohello following response:</span></span>
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a><span data-ttu-id="1f2f7-195">A fürt állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1f2f7-195">Check cluster status</span></span>
<span data-ttu-id="1f2f7-196">A HBase a HDInsightban a fürtök megfigyelésére szolgáló webes felhasználói felülettel kapható.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-196">HBase in HDInsight ships with a Web UI for monitoring clusters.</span></span> <span data-ttu-id="1f2f7-197">Hello webes felhasználói felület használatával, kérhet statisztikáit vagy információit régiók.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-197">Using hello Web UI, you can request statistics or information about regions.</span></span>

<span data-ttu-id="1f2f7-198">**tooaccess hello HBase fő felhasználói felület**</span><span class="sxs-lookup"><span data-stu-id="1f2f7-198">**tooaccess hello HBase Master UI**</span></span>

1. <span data-ttu-id="1f2f7-199">Jelentkezzen be a hello hello Ambari webes felhasználói felületén a https://&lt;Clustername >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-199">Sign into hello hello Ambari Web UI at https://&lt;Clustername>.azurehdinsight.net.</span></span>
2. <span data-ttu-id="1f2f7-200">Kattintson a **HBase** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-200">Click **HBase** from hello left menu.</span></span>
3. <span data-ttu-id="1f2f7-201">Kattintson a **Gyorshivatkozások** hello hello oldal, pont toohello a Zookeeper csomópont hivatkozás aktív, és kattintson a **HBase fő felhasználói felület**.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-201">Click **Quick links** on hello top of hello page, point toohello active Zookeeper node link, and then click **HBase Master UI**.</span></span>  <span data-ttu-id="1f2f7-202">felhasználói felület hello meg van nyitva egy másik böngészőben lapon:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-202">hello UI is opened in another browser tab:</span></span>

  ![HDInsight HBase HMaster felhasználói felülete](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  <span data-ttu-id="1f2f7-204">hello HBase fő felhasználói felülete a következő szakaszok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-204">hello HBase Master UI contains hello following sections:</span></span>

  - <span data-ttu-id="1f2f7-205">régiós kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="1f2f7-205">region servers</span></span>
  - <span data-ttu-id="1f2f7-206">biztonsági mentési főkiszolgálók</span><span class="sxs-lookup"><span data-stu-id="1f2f7-206">backup masters</span></span>
  - <span data-ttu-id="1f2f7-207">táblák</span><span class="sxs-lookup"><span data-stu-id="1f2f7-207">tables</span></span>
  - <span data-ttu-id="1f2f7-208">feladatok</span><span class="sxs-lookup"><span data-stu-id="1f2f7-208">tasks</span></span>
  - <span data-ttu-id="1f2f7-209">szoftverattribútumok</span><span class="sxs-lookup"><span data-stu-id="1f2f7-209">software attributes</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="1f2f7-210">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="1f2f7-210">Delete hello cluster</span></span>
<span data-ttu-id="1f2f7-211">tooavoid inkonzisztenciákat, ajánlott letiltani hello HBase táblákat hello fürt törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-211">tooavoid inconsistencies, we recommend that you disable hello HBase tables before you delete hello cluster.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="1f2f7-212">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="1f2f7-212">Troubleshoot</span></span>

<span data-ttu-id="1f2f7-213">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="1f2f7-213">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f2f7-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f2f7-214">Next steps</span></span>
<span data-ttu-id="1f2f7-215">Ebben a cikkben megtanulta, hogyan toocreate HBase-fürtöt, és hogyan toocreate tábla és nézet hello ezen táblák adatait hello HBase rendszerhéjból.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-215">In this article, you learned how toocreate an HBase cluster and how toocreate tables and view hello data in those tables from hello HBase shell.</span></span> <span data-ttu-id="1f2f7-216">Is megtanulta, hogyan toouse a struktúra a HBase táblákat, és hogyan toouse hello HBase C# REST API-k toocreate lévő adatok lekérdezése egy HBase tábla és hello tábla adatainak lekérése.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-216">You also learned how toouse a Hive query on data in HBase tables and how toouse hello HBase C# REST APIs toocreate an HBase table and retrieve data from hello table.</span></span>

<span data-ttu-id="1f2f7-217">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="1f2f7-217">toolearn more, see:</span></span>

* <span data-ttu-id="1f2f7-218">[HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="1f2f7-218">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
