---
title: "Bevezetés a HDInsight egy HBase-példájába – Azure | Microsoft Docs"
description: "Kövesse ezt az Apache HBase-példát a HDInsight-alapú Hadoop használatának megkezdéséhez. Táblákat hozhat létre a HBase rendszehéjból, és lekérdezheti azokat a Hive eszközzel."
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
ms.openlocfilehash: bbd8a838062795ee03ae02dc5e3fd45d841a6e17
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a><span data-ttu-id="c2322-105">Bevezetés a HDInsight egy Apache HBase-példájába</span><span class="sxs-lookup"><span data-stu-id="c2322-105">Get started with an Apache HBase example in HDInsight</span></span>

<span data-ttu-id="c2322-106">Megtudhatja, hogyan hozhat létre HBase-fürtöket a HDInsight eszközben, illetve hogyan hozhat létre HBase táblákat és lekérdezéstáblákat a Hive eszközzel.</span><span class="sxs-lookup"><span data-stu-id="c2322-106">Learn how to create an HBase cluster in HDInsight, create HBase tables, and query tables by using Hive.</span></span> <span data-ttu-id="c2322-107">A HBase-re vonatkozó általános információért lásd: [HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése).</span><span class="sxs-lookup"><span data-stu-id="c2322-107">For general HBase information, see [HDInsight HBase overview][hdinsight-hbase-overview].</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a><span data-ttu-id="c2322-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2322-108">Prerequisites</span></span>
<span data-ttu-id="c2322-109">Az alábbi HBase-példa kipróbálásához a következőkkel kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="c2322-109">Before you begin trying this HBase example, you must have the following items:</span></span>

* <span data-ttu-id="c2322-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="c2322-110">**An Azure subscription**.</span></span> <span data-ttu-id="c2322-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="c2322-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="c2322-112">[Biztonságos rendszerhéj (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c2322-112">[Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> 
* <span data-ttu-id="c2322-113">[curl](http://curl.haxx.se/download.html).</span><span class="sxs-lookup"><span data-stu-id="c2322-113">[curl](http://curl.haxx.se/download.html).</span></span>

## <a name="create-hbase-cluster"></a><span data-ttu-id="c2322-114">HBase-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2322-114">Create HBase cluster</span></span>
<span data-ttu-id="c2322-115">Az alábbi eljárás egy Azure Resource Manager-sablont használ egy 3.4 verziójú Linux-alapú HBase-fürt és a függő Azure Storage-fiók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c2322-115">The following procedure uses an Azure Resource Manager template to create a version 3.4 Linux-based HBase cluster and the dependent default Azure Storage account.</span></span> <span data-ttu-id="c2322-116">Az eljárásban és egyéb fürtlétrehozási módszerekben használt paraméterek megértéséhez lásd: [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Linux-alapú Hadoop-fürtök létrehozása a HDInsightban).</span><span class="sxs-lookup"><span data-stu-id="c2322-116">To understand the parameters used in the procedure and other cluster creation methods, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="c2322-117">Az alábbi képre kattintva megnyithatja a sablont az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="c2322-117">Click the following image to open the template in the Azure portal.</span></span> <span data-ttu-id="c2322-118">A sablon egy nyilvános blobtárolóban található.</span><span class="sxs-lookup"><span data-stu-id="c2322-118">The template is located in a public blob container.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. <span data-ttu-id="c2322-119">Az **Egyéni üzembe helyezés** panelen adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="c2322-119">From the **Custom deployment** blade, enter the following values:</span></span>
   
   * <span data-ttu-id="c2322-120">**Előfizetés**: Válassza ki a fürt létrehozásához használt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c2322-120">**Subscription**: Select your Azure subscription that is used to create the cluster.</span></span>
   * <span data-ttu-id="c2322-121">**Erőforráscsoport**: Hozzon létre egy Azure Resource Management-csoportot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="c2322-121">**Resource group**: Create an Azure Resource Management group or use an existing one.</span></span>
   * <span data-ttu-id="c2322-122">**Location** (Hely): Adja meg az erőforráscsoport helyét.</span><span class="sxs-lookup"><span data-stu-id="c2322-122">**Location**: Specify the location of the resource group.</span></span> 
   * <span data-ttu-id="c2322-123">**Fürt neve**: Adjon nevet a HBase-fürtnek.</span><span class="sxs-lookup"><span data-stu-id="c2322-123">**ClusterName**: Enter a name for the HBase cluster.</span></span>
   * <span data-ttu-id="c2322-124">**A fürt bejelentkezési neve és jelszava**: Az alapértelmezett bejelentkezési név az **admin**.</span><span class="sxs-lookup"><span data-stu-id="c2322-124">**Cluster login name and password**: The default login name is **admin**.</span></span>
   * <span data-ttu-id="c2322-125">**SSH-felhasználónév és jelszó**: Az alapértelmezett felhasználónév az **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="c2322-125">**SSH username and password**: The default username is **sshuser**.</span></span>  <span data-ttu-id="c2322-126">Ezt át lehet nevezni.</span><span class="sxs-lookup"><span data-stu-id="c2322-126">You can rename it.</span></span>
     
     <span data-ttu-id="c2322-127">Más paraméterek opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="c2322-127">Other parameters are optional.</span></span>  
     
     <span data-ttu-id="c2322-128">Minden egyes fürt az Azure Storage-fióktól függ.</span><span class="sxs-lookup"><span data-stu-id="c2322-128">Each cluster has an Azure Storage account dependency.</span></span> <span data-ttu-id="c2322-129">A fürtök törlése után az adatok megmaradnak a tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="c2322-129">After you delete a cluster, the data retains in the storage account.</span></span> <span data-ttu-id="c2322-130">A fürt alapértelmezett tárfiókneve a fürt neve a „store” kifejezéssel kiegészítve.</span><span class="sxs-lookup"><span data-stu-id="c2322-130">The cluster default storage account name is the cluster name with "store" appended.</span></span> <span data-ttu-id="c2322-131">A név szoftveresen kötött a sablonváltozók szakaszban.</span><span class="sxs-lookup"><span data-stu-id="c2322-131">It is hardcoded in the template variables section.</span></span>
3. <span data-ttu-id="c2322-132">Válassza az **I agree to the terms and conditions stated above** (Elfogadom a fenti feltételeket) lehetőséget majd kattintson a **Purchase** (Vásárlás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c2322-132">Select **I agree to the terms and conditions stated above**, and then click **Purchase**.</span></span> <span data-ttu-id="c2322-133">Egy fürt létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c2322-133">It takes about 20 minutes to create a cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="c2322-134">A HBase-fürtök törlése után egy másik HBase-fürtöt hozhat létre ugyanazon alapértelmezett blobtárolóval.</span><span class="sxs-lookup"><span data-stu-id="c2322-134">After an HBase cluster is deleted, you can create another HBase cluster by using the same default blob container.</span></span> <span data-ttu-id="c2322-135">Az új fürt felveszi az eredeti fürtben létrehozott HBase-táblákat.</span><span class="sxs-lookup"><span data-stu-id="c2322-135">The new cluster picks up the HBase tables you created in the original cluster.</span></span> <span data-ttu-id="c2322-136">Az inkonzisztenciák elkerülése érdekében javasoljuk, hogy a fürt törlése előtt tiltsa le a HBase-táblákat.</span><span class="sxs-lookup"><span data-stu-id="c2322-136">To avoid inconsistencies, we recommend that you disable the HBase tables before you delete the cluster.</span></span>
> 
> 

## <a name="create-tables-and-insert-data"></a><span data-ttu-id="c2322-137">Táblák létrehozása és adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="c2322-137">Create tables and insert data</span></span>
<span data-ttu-id="c2322-138">Az SSH-val HBase-fürtökhöz csatlakozhat, majd a HBase-rendszerhéjjal HBase-táblákat hozhat létre, adatokat szúrhat be, és adatokat kérdezhet le.</span><span class="sxs-lookup"><span data-stu-id="c2322-138">You can use SSH to connect to HBase clusters and then use HBase Shell to create HBase tables, insert data, and query data.</span></span> <span data-ttu-id="c2322-139">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c2322-139">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="c2322-140">A legtöbbek számára az adatok táblázatos formátumban jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="c2322-140">For most people, data appears in the tabular format:</span></span>

![HDInsight HBase táblázatos adatok][img-hbase-sample-data-tabular]

<span data-ttu-id="c2322-142">A HBase-ben, amely a BigTable implementációja, ugyanezen adatok a következőképpen néznek ki:</span><span class="sxs-lookup"><span data-stu-id="c2322-142">In HBase (an implementation of BigTable), the same data looks like:</span></span>

![HDInsight HBase BigTable-adatok][img-hbase-sample-data-bigtable]


<span data-ttu-id="c2322-144">**A Hbase-rendszerhéj használata**</span><span class="sxs-lookup"><span data-stu-id="c2322-144">**To use the HBase shell**</span></span>

1. <span data-ttu-id="c2322-145">Az SSH-ból futtassa az alábbi HBase-parancsot:</span><span class="sxs-lookup"><span data-stu-id="c2322-145">From SSH, run the following HBase command:</span></span>
   
    ```bash
    hbase shell
    ```

2. <span data-ttu-id="c2322-146">Hozzon létre egy HBase-t kétoszlopos családokkal:</span><span class="sxs-lookup"><span data-stu-id="c2322-146">Create an HBase with two-column families:</span></span>

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. <span data-ttu-id="c2322-147">Adatok beszúrása:</span><span class="sxs-lookup"><span data-stu-id="c2322-147">Insert some data:</span></span>
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase-rendszerhéj][img-hbase-shell]
4. <span data-ttu-id="c2322-149">Egyetlen sor lekérése</span><span class="sxs-lookup"><span data-stu-id="c2322-149">Get a single row</span></span>
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    <span data-ttu-id="c2322-150">Ugyanazokat az eredményeket látja, mint a vizsgálat parancs használatakor, mert csak egy sor van.</span><span class="sxs-lookup"><span data-stu-id="c2322-150">You shall see the same results as using the scan command because there is only one row.</span></span>
   
    <span data-ttu-id="c2322-151">A Hbase-táblasémáról további információért lásd: [Introduction to HBase Schema Design][hbase-schema] (Bevezetés a Hbase-sématervezésbe).</span><span class="sxs-lookup"><span data-stu-id="c2322-151">For more information about the HBase table schema, see [Introduction to HBase Schema Design][hbase-schema].</span></span> <span data-ttu-id="c2322-152">További Hbase-parancsokért lásd: [Apache HBase reference guide][hbase-quick-start] (Apache HBase referencia-útmutató).</span><span class="sxs-lookup"><span data-stu-id="c2322-152">For more HBase commands, see [Apache HBase reference guide][hbase-quick-start].</span></span>
5. <span data-ttu-id="c2322-153">Kilépés a rendszerhéjból</span><span class="sxs-lookup"><span data-stu-id="c2322-153">Exit the shell</span></span>
   
    ```hbaseshell
    exit
    ```

<span data-ttu-id="c2322-154">**Adatok kötegelt betöltése a névjegyek HBase-táblába**</span><span class="sxs-lookup"><span data-stu-id="c2322-154">**To bulk load data into the contacts HBase table**</span></span>

<span data-ttu-id="c2322-155">A HBase több módszert tartalmaz az adatok táblába töltéséhez.</span><span class="sxs-lookup"><span data-stu-id="c2322-155">HBase includes several methods of loading data into tables.</span></span>  <span data-ttu-id="c2322-156">További információ: [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load) (Kötegelt betöltés).</span><span class="sxs-lookup"><span data-stu-id="c2322-156">For more information, see [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load).</span></span>

<span data-ttu-id="c2322-157">Egy minta adatfájl található a következő nyilvános blobtárolóban található: *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span><span class="sxs-lookup"><span data-stu-id="c2322-157">A sample data file can be found in a public blob container, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.</span></span>  <span data-ttu-id="c2322-158">Az adatfájl tartalma a következő:</span><span class="sxs-lookup"><span data-stu-id="c2322-158">The content of the data file is:</span></span>

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

<span data-ttu-id="c2322-159">Igény szerint létrehozhat egy szövegfájlt, és feltöltheti a fájlt a saját tárfiókjába.</span><span class="sxs-lookup"><span data-stu-id="c2322-159">You can optionally create a text file and upload the file to your own storage account.</span></span> <span data-ttu-id="c2322-160">Az utasításokért lásd: [Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data] (Adatok feltöltése Hadoop-feladatokhoz a HDInsightban).</span><span class="sxs-lookup"><span data-stu-id="c2322-160">For the instructions, see [Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data].</span></span>

> [!NOTE]
> <span data-ttu-id="c2322-161">Ez az eljárás az utolsó eljárás során létrehozott Contacts HBase táblát használja.</span><span class="sxs-lookup"><span data-stu-id="c2322-161">This procedure uses the Contacts HBase table you have created in the last procedure.</span></span>
> 

1. <span data-ttu-id="c2322-162">Az SSH-ból futtassa az alábbi parancsot, hogy az adatfájlt StoreFiles-fájllá alakítsa, és a Dimporttsv.bulk.output által meghatározott relatív elérési úton tárolja.</span><span class="sxs-lookup"><span data-stu-id="c2322-162">From SSH, run the following command to transform the data file to StoreFiles and store at a relative path specified by Dimporttsv.bulk.output.</span></span>  <span data-ttu-id="c2322-163">Ha a HBase rendszerhéjban van, a kilépés paranccsal lépjen ki.</span><span class="sxs-lookup"><span data-stu-id="c2322-163">If you are in HBase Shell, use the exit command to exit.</span></span>

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. <span data-ttu-id="c2322-164">Futtassa a következő parancsot, hogy adatokat töltsön fel az /example/data/storeDataFileOutput mappából a HBase táblába:</span><span class="sxs-lookup"><span data-stu-id="c2322-164">Run the following command to upload the data from  /example/data/storeDataFileOutput to the HBase table:</span></span>
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. <span data-ttu-id="c2322-165">Megnyithatja a HBase rendszerhéjat, és a vizsgálat paranccsal listázhatja a tábla tartalmát.</span><span class="sxs-lookup"><span data-stu-id="c2322-165">You can open the HBase shell, and use the scan command to list the table content.</span></span>

## <a name="use-hive-to-query-hbase"></a><span data-ttu-id="c2322-166">A Hive használata a HBase lekérdezéséhez</span><span class="sxs-lookup"><span data-stu-id="c2322-166">Use Hive to query HBase</span></span>

<span data-ttu-id="c2322-167">A HBase táblákban lévő adatokat a Hive eszközzel kérdezheti le.</span><span class="sxs-lookup"><span data-stu-id="c2322-167">You can query data in HBase tables by using Hive.</span></span> <span data-ttu-id="c2322-168">Ebben a szakaszban egy, a HBase-táblára leképezést biztosító Hive-táblát hoz létre, amellyel lekérdezheti a HBase-táblában lévő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c2322-168">In this section, you create a Hive table that maps to the HBase table and uses it to query the data in your HBase table.</span></span>

1. <span data-ttu-id="c2322-169">Nyissa meg a **PuTTY** eszközt, és csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="c2322-169">Open **PuTTY**, and connect to the cluster.</span></span>  <span data-ttu-id="c2322-170">Lásd az előző eljárás utasításait.</span><span class="sxs-lookup"><span data-stu-id="c2322-170">See the instructions in the previous procedure.</span></span>
2. <span data-ttu-id="c2322-171">Az SSH-munkamenetből a következő paranccsal indíthatja el a Beeline-t:</span><span class="sxs-lookup"><span data-stu-id="c2322-171">From the SSH session, use the following command to start Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="c2322-172">A Beeline-nal kapcsolatos további információkért lásd [a Hive és a Hadoop együttes, a Beeline-nal történő használatát a HDInsightban](hdinsight-hadoop-use-hive-beeline.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="c2322-172">For more information about Beeline, see [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md).</span></span>
       
3. <span data-ttu-id="c2322-173">Futtassa a következő HiveQL-szkriptet, hogy egy, a HBase-táblára leképező Hive-táblát hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="c2322-173">Run the following HiveQL script  to create a Hive table that maps to the HBase table.</span></span> <span data-ttu-id="c2322-174">Ellenőrizze, hogy létrehozta-e az oktatóanyag korábbi részében hivatkozott mintatáblát az utasítás futtatása előtt a HBase rendszerhéjjal.</span><span class="sxs-lookup"><span data-stu-id="c2322-174">Make sure that you have created the sample table referenced earlier in this tutorial by using the HBase shell before you run this statement.</span></span>

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. <span data-ttu-id="c2322-175">Futtassa a következő HiveQL-parancsfájlt a HBase-tábla adatainak lekérdezéséhez:</span><span class="sxs-lookup"><span data-stu-id="c2322-175">Run the following HiveQL script to query the data in the HBase table:</span></span>

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a><span data-ttu-id="c2322-176">HBase REST API-k használata Curl használatával</span><span class="sxs-lookup"><span data-stu-id="c2322-176">Use HBase REST APIs using Curl</span></span>

<span data-ttu-id="c2322-177">A REST API védelméről [alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication) gondoskodik.</span><span class="sxs-lookup"><span data-stu-id="c2322-177">The REST API is secured via [basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication).</span></span> <span data-ttu-id="c2322-178">Mindig biztonságos HTTP-n (HTTPS-en) keresztül kell kéréseket végeznie, hogy a hitelesítő adatait biztonságos módon küldje el a kiszolgálónak.</span><span class="sxs-lookup"><span data-stu-id="c2322-178">You shall always make requests by using Secure HTTP (HTTPS) to help ensure that your credentials are securely sent to the server.</span></span>

2. <span data-ttu-id="c2322-179">Használja az alábbi parancsot a meglévő HBase-táblák listázásához:</span><span class="sxs-lookup"><span data-stu-id="c2322-179">Use the following command to list the existing HBase tables:</span></span>

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. <span data-ttu-id="c2322-180">Használja az alábbi parancsot egy új, kétoszlopos családokkal rendelkező HBase-tábla létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="c2322-180">Use the following command to create a new HBase table with two-column families:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    <span data-ttu-id="c2322-181">A séma a JSon formátumban van megadva.</span><span class="sxs-lookup"><span data-stu-id="c2322-181">The schema is provided in the JSon format.</span></span>
4. <span data-ttu-id="c2322-182">Használja az alábbi parancsot néhány adat beviteléhez:</span><span class="sxs-lookup"><span data-stu-id="c2322-182">Use the following command to insert some data:</span></span>

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    <span data-ttu-id="c2322-183">A -d kapcsolóban megadott értékeket a base64 használatával kell kódolnia.</span><span class="sxs-lookup"><span data-stu-id="c2322-183">You must base64 encode the values specified in the -d switch.</span></span> <span data-ttu-id="c2322-184">A példában:</span><span class="sxs-lookup"><span data-stu-id="c2322-184">In the example:</span></span>
   
   * <span data-ttu-id="c2322-185">MTAwMA==: 1000</span><span class="sxs-lookup"><span data-stu-id="c2322-185">MTAwMA==: 1000</span></span>
   * <span data-ttu-id="c2322-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span><span class="sxs-lookup"><span data-stu-id="c2322-186">UGVyc29uYWw6TmFtZQ==: Personal:Name</span></span>
   * <span data-ttu-id="c2322-187">Sm9obiBEb2xl: John Dole</span><span class="sxs-lookup"><span data-stu-id="c2322-187">Sm9obiBEb2xl: John Dole</span></span>
     
     <span data-ttu-id="c2322-188">A [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) lehetővé teszi több (kötegelt) érték beszúrását.</span><span class="sxs-lookup"><span data-stu-id="c2322-188">[false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) allows you to insert multiple (batched) values.</span></span>
5. <span data-ttu-id="c2322-189">Használja az alábbi parancsot egy sor lekéréséhez:</span><span class="sxs-lookup"><span data-stu-id="c2322-189">Use the following command to get a row:</span></span>
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

<span data-ttu-id="c2322-190">További információ a HBase REST-ről: [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest) (Apache HBase referencia-útmutató).</span><span class="sxs-lookup"><span data-stu-id="c2322-190">For more information about HBase Rest, see [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest).</span></span>

> [!NOTE]
> <span data-ttu-id="c2322-191">A HBase nem támogatja a Thriftet a HDInsightban.</span><span class="sxs-lookup"><span data-stu-id="c2322-191">Thrift is not supported by HBase in HDInsight.</span></span>
>
> <span data-ttu-id="c2322-192">Amikor a Curl vagy más REST kommunikációt használ a WebHCattel, hitelesítenie kell a kéréseket a HDInsight fürt rendszergazdája felhasználónevének és jelszavának megadásával.</span><span class="sxs-lookup"><span data-stu-id="c2322-192">When using Curl or any other REST communication with WebHCat, you must authenticate the requests by providing the user name and password for the HDInsight cluster administrator.</span></span> <span data-ttu-id="c2322-193">A fürtnevet a kérések a kiszolgálóhoz küldéséhez használt egységes erőforrás-azonosító (URI) részeként is használnia kell.</span><span class="sxs-lookup"><span data-stu-id="c2322-193">You must also use the cluster name as part of the Uniform Resource Identifier (URI) used to send the requests to the server:</span></span>
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    <span data-ttu-id="c2322-194">A következőhöz hasonló választ kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="c2322-194">You should receive a response similar to the following response:</span></span>
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a><span data-ttu-id="c2322-195">A fürt állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c2322-195">Check cluster status</span></span>
<span data-ttu-id="c2322-196">A HBase a HDInsightban a fürtök megfigyelésére szolgáló webes felhasználói felülettel kapható.</span><span class="sxs-lookup"><span data-stu-id="c2322-196">HBase in HDInsight ships with a Web UI for monitoring clusters.</span></span> <span data-ttu-id="c2322-197">A webes felhasználói felülettel a régiók statisztikáit vagy információit kérheti le.</span><span class="sxs-lookup"><span data-stu-id="c2322-197">Using the Web UI, you can request statistics or information about regions.</span></span>

<span data-ttu-id="c2322-198">**A HBase mesterfelületének elérése**</span><span class="sxs-lookup"><span data-stu-id="c2322-198">**To access the HBase Master UI**</span></span>

1. <span data-ttu-id="c2322-199">Jelentkezzen be az Ambari webes felületre a https://&lt;Fürtnév>.azurehdinsight.net címen.</span><span class="sxs-lookup"><span data-stu-id="c2322-199">Sign into the the Ambari Web UI at https://&lt;Clustername>.azurehdinsight.net.</span></span>
2. <span data-ttu-id="c2322-200">Kattintson a **HBase** elemre a bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="c2322-200">Click **HBase** from the left menu.</span></span>
3. <span data-ttu-id="c2322-201">Kattintson a **Gyorshivatkozások** elemre a lap tetején, mutasson az aktív Zookeeper-csomópont hivatkozására, majd kattintson a **HBase-mesterfelület** elemre.</span><span class="sxs-lookup"><span data-stu-id="c2322-201">Click **Quick links** on the top of the page, point to the active Zookeeper node link, and then click **HBase Master UI**.</span></span>  <span data-ttu-id="c2322-202">A felület egy új böngészőlapon nyílik meg:</span><span class="sxs-lookup"><span data-stu-id="c2322-202">The UI is opened in another browser tab:</span></span>

  ![HDInsight HBase HMaster felhasználói felülete](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  <span data-ttu-id="c2322-204">A HBase-mesterfelület az alábbi részeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="c2322-204">The HBase Master UI contains the following sections:</span></span>

  - <span data-ttu-id="c2322-205">régiós kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="c2322-205">region servers</span></span>
  - <span data-ttu-id="c2322-206">biztonsági mentési főkiszolgálók</span><span class="sxs-lookup"><span data-stu-id="c2322-206">backup masters</span></span>
  - <span data-ttu-id="c2322-207">táblák</span><span class="sxs-lookup"><span data-stu-id="c2322-207">tables</span></span>
  - <span data-ttu-id="c2322-208">feladatok</span><span class="sxs-lookup"><span data-stu-id="c2322-208">tasks</span></span>
  - <span data-ttu-id="c2322-209">szoftverattribútumok</span><span class="sxs-lookup"><span data-stu-id="c2322-209">software attributes</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="c2322-210">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="c2322-210">Delete the cluster</span></span>
<span data-ttu-id="c2322-211">Az inkonzisztenciák elkerülése érdekében javasoljuk, hogy a fürt törlése előtt tiltsa le a HBase-táblákat.</span><span class="sxs-lookup"><span data-stu-id="c2322-211">To avoid inconsistencies, we recommend that you disable the HBase tables before you delete the cluster.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="c2322-212">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="c2322-212">Troubleshoot</span></span>

<span data-ttu-id="c2322-213">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="c2322-213">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2322-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2322-214">Next steps</span></span>
<span data-ttu-id="c2322-215">Ebből a cikkből megismerhette, hogyan hozhat létre HBase-fürtöt, hogyan hozhat létre táblákat, és hogyan tekintheti meg ezen táblák adatait a HBase-rendszerhéjból.</span><span class="sxs-lookup"><span data-stu-id="c2322-215">In this article, you learned how to create an HBase cluster and how to create tables and view the data in those tables from the HBase shell.</span></span> <span data-ttu-id="c2322-216">Azt is megtanulta, hogyan használhat Hive-lekérdezést a HBase-táblákban lévő adatokon, és hogyan használhatja a HBase C# REST API-kat egy HBase-tábla létrehozásához és adatok lekérdezéséhez a táblából.</span><span class="sxs-lookup"><span data-stu-id="c2322-216">You also learned how to use a Hive query on data in HBase tables and how to use the HBase C# REST APIs to create an HBase table and retrieve data from the table.</span></span>

<span data-ttu-id="c2322-217">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="c2322-217">To learn more, see:</span></span>

* <span data-ttu-id="c2322-218">[HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="c2322-218">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>

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
