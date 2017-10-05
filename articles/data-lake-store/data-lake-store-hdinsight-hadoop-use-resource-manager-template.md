---
title: "Azure-sablonok segítségével hozza létre a HDInsight és a Data Lake Store |} Microsoft Docs"
description: "Használja az Azure Resource Manager-sablonok létrehozása és használata a HDInsight-fürtök az Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: nitinme
ms.openlocfilehash: 6f43423096f0e74f41afea275e4ec9801dc2cea5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a><span data-ttu-id="c7cc3-103">HDInsight-fürtök létrehozása a Data Lake Store Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="c7cc3-103">Create an HDInsight cluster with Data Lake Store using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7cc3-104">A Portal használata</span><span class="sxs-lookup"><span data-stu-id="c7cc3-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="c7cc3-105">PowerShell használatával (az alapértelmezett tároló)</span><span class="sxs-lookup"><span data-stu-id="c7cc3-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="c7cc3-106">(Tárhely) a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c7cc3-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="c7cc3-107">Erőforrás-kezelő használatával</span><span class="sxs-lookup"><span data-stu-id="c7cc3-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="c7cc3-108">Azure PowerShell használata a HDInsight-fürtök konfigurálása az Azure Data Lake Store **további tárolóként**.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-108">Learn how to use Azure PowerShell to configure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span>

<span data-ttu-id="c7cc3-109">Támogatott fürttípusok Data Lake Store alapértelmezett tároló vagy további tárfiókot kell használni.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-109">For supported cluster types, Data Lake Store be used as an default storage or additional storage account.</span></span> <span data-ttu-id="c7cc3-110">Ha a Data Lake Store további tárterületet, az alapértelmezett tárfiókot, a fürt továbbra is Azure Storage Blobs (WASB), és a fürt kapcsolatos (például naplói, stb.) továbbra is írja a fájlt, alapértelmezett tárolására, amíg a Data Lake Store-fiók tárolhatja az adatokat, hogy fel szeretné dolgoztatni.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-110">When Data Lake Store is used as additional storage, the default storage account for the clusters will still be Azure Storage Blobs (WASB) and the cluster-related files (such as logs, etc.) are still written to the default storage, while the data that you want to process can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="c7cc3-111">További tárhely fiókként használatával a Data Lake Store nem befolyásolja a teljesítményt vagy olvasására vagy írására a tárhelyet a fürtből történő alkalmazásának képességét.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-111">Using Data Lake Store as an additional storage account does not impact performance or the ability to read/write to the storage from the cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="c7cc3-112">Data Lake Store használata a HDInsight-fürt tárolására</span><span class="sxs-lookup"><span data-stu-id="c7cc3-112">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="c7cc3-113">Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:</span><span class="sxs-lookup"><span data-stu-id="c7cc3-113">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="c7cc3-114">A HDInsight-fürtök létrehozása a Data Lake Store-hozzáféréssel rendelkező áll rendelkezésre a HDInsight 3.5-ös és 3.6, alapértelmezett tárolási lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-114">Option to create HDInsight clusters with access to Data Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="c7cc3-115">A HDInsight-fürtök létrehozása a Data Lake Store elérését, további tárhely áll rendelkezésre a HDInsight-verziókról 3.2-es, 3.4, 3.5-ös és 3.6 lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-115">Option to create HDInsight clusters with access to Data Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="c7cc3-116">Ebben a cikkben azt a Data Lake Store a Hadoop fürtök a további tárolóként kell kiépíteni.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-116">In this article, we provision a Hadoop cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="c7cc3-117">A Hadoop-fürt létrehozása a Data Lake Store alapértelmezett tárolóként, lásd: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-117">For instructions on how to create a Hadoop cluster with Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7cc3-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c7cc3-118">Prerequisites</span></span>
<span data-ttu-id="c7cc3-119">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="c7cc3-119">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="c7cc3-120">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-120">**An Azure subscription**.</span></span> <span data-ttu-id="c7cc3-121">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-121">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c7cc3-122">Az **Azure PowerShell 1.0-s vagy újabb verziója**.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-122">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="c7cc3-123">Lásd: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-123">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="c7cc3-124">**Az Azure Active Directory szolgáltatás egyszerű**.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-124">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="c7cc3-125">Ez az oktatóanyag lépéseit ad útmutatást az egyszerű szolgáltatás létrehozása az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-125">Steps in this tutorial provide instructions on how to create a service principal in Azure AD.</span></span> <span data-ttu-id="c7cc3-126">Azonban az Azure AD a rendszergazda létrehozhat egy egyszerű szolgáltatást kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-126">However, you must be an Azure AD administrator to be able to create a service principal.</span></span> <span data-ttu-id="c7cc3-127">Ha az Azure AD-rendszergazdaként, hagyja ki ezt az előfeltételt, és az oktatóanyag folytatásához.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-127">If you are an Azure AD administrator, you can skip this prerequisite and proceed with the tutorial.</span></span>

    <span data-ttu-id="c7cc3-128">**Ha nem az Azure AD-rendszergazda**, nem fog tudni egy egyszerű szolgáltatásnév létrehozásához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-128">**If you are not an Azure AD administrator**, you will not be able to perform the steps required to create a service principal.</span></span> <span data-ttu-id="c7cc3-129">Ebben az esetben az Azure AD-rendszergazda először létre kell hoznia egy egyszerű szolgáltatást a Data Lake Store egy HDInsight-fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-129">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="c7cc3-130">Emellett az egyszerű szolgáltatás segítségével kell létrehozni egy tanúsítványt, részben ismertetett módon [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-130">Also, the service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a><span data-ttu-id="c7cc3-131">HDInsight-fürtök létrehozása az Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c7cc3-131">Create an HDInsight cluster with Azure Data Lake Store</span></span>
<span data-ttu-id="c7cc3-132">A Resource Manager-sablon, és a sablon használatára vonatkozó Előfeltételek legyenek elérhetők a Githubon: [az új Data Lake Store HDInsight Linux fürt központi telepítése](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-132">The Resource Manager template, and the prerequisites for using the template, are available on GitHub at [Deploy a HDInsight Linux cluster with new Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span></span> <span data-ttu-id="c7cc3-133">Kövesse az utasításokat, ez a hivatkozás elérhető HDInsight-fürtök létrehozása az Azure Data Lake Store további tárolóként.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-133">Follow the instructions provided at this link to create an HDInsight cluster with Azure Data Lake Store as the additional storage.</span></span>

<span data-ttu-id="c7cc3-134">A fenti hivatkozás található útmutatás PowerShell szükséges.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-134">The instructions at the link mentioned above require PowerShell.</span></span> <span data-ttu-id="c7cc3-135">Mielőtt elkezdené a ezeket az utasításokat, győződjön meg arról az Azure-fiókjába való bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-135">Before you start with those instructions, make sure you log in to your Azure account.</span></span> <span data-ttu-id="c7cc3-136">Az asztalon nyisson meg egy új Azure PowerShell-ablakot, és adja meg az alábbi kódtöredékek.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-136">From your desktop, open a new Azure PowerShell window, and enter the following snippets.</span></span> <span data-ttu-id="c7cc3-137">Győződjön meg arról, hogy az előfizetés rendszergazdájaként/tulajdonosaként jelentkezik be, amikor a rendszer erre kéri:</span><span class="sxs-lookup"><span data-stu-id="c7cc3-137">When prompted to log in, make sure you log in as one of the subscription admininistrators/owner:</span></span>

```
# Log in to your Azure account
Login-AzureRmAccount

# List all the subscriptions associated to your account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-to-the-azure-data-lake-store"></a><span data-ttu-id="c7cc3-138">Az Azure Data Lake Store mintaadatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="c7cc3-138">Upload sample data to the Azure Data Lake Store</span></span>
<span data-ttu-id="c7cc3-139">A Resource Manager-sablon létrehoz egy új Data Lake Store-fiókot, és a HDInsight-fürthöz társítja azt.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-139">The Resource Manager template creates a new Data Lake Store account and associates it with the HDInsight cluster.</span></span> <span data-ttu-id="c7cc3-140">A Data Lake Store most tölthet fel néhány adatot.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-140">You must now upload some sample data to the Data Lake Store.</span></span> <span data-ttu-id="c7cc3-141">Feladatok futtatása az adatok a Data Lake Store-ban elérhető HDInsight-fürtök az oktatóanyag későbbi részében szüksége lesz az adatokat.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-141">You'll need this data later in the tutorial to run jobs from an HDInsight cluster that access data in the Data Lake Store.</span></span> <span data-ttu-id="c7cc3-142">Hogyan tölthet fel adatokat, lásd: [-fájl feltöltése a Data Lake store](data-lake-store-get-started-portal.md#uploaddata).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-142">For instructions on how to upload data, see [Upload a file to your Data Lake Store](data-lake-store-get-started-portal.md#uploaddata).</span></span> <span data-ttu-id="c7cc3-143">Ha feltölthető mintaadatokra van szüksége, használhatja az [Azure Data Lake Git-tárában](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData) található **Ambulance Data** mappát.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-143">If you are looking for some sample data to upload, you can get the **Ambulance Data** folder from the [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

## <a name="set-relevant-acls-on-the-sample-data"></a><span data-ttu-id="c7cc3-144">A mintaadatok vonatkozó hozzáférés-vezérlési listák beállítása</span><span class="sxs-lookup"><span data-stu-id="c7cc3-144">Set relevant ACLs on the sample data</span></span>
<span data-ttu-id="c7cc3-145">Annak biztosításához, a feltöltött mintaadatok érhető el a HDInsight-fürt, akkor biztosítania kell, hogy az Azure AD-alkalmazás, amely a HDInsight-fürt és a Data Lake Store között identitás hozzáfér a fájl vagy mappa elérésére tett kísérlet létrehozására szolgál.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-145">To make sure the sample data you upload is accessible from the HDInsight cluster, you must ensure that the Azure AD application that is used to establish identity between the HDInsight cluster and Data Lake Store has access to the file/folder you are trying to access.</span></span> <span data-ttu-id="c7cc3-146">Ehhez hajtsa végre a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-146">To do this, perform the following steps.</span></span>

1. <span data-ttu-id="c7cc3-147">Keresse meg a HDInsight-fürt és a Data Lake Store társított Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-147">Find the name of the Azure AD application that is associated with HDInsight cluster and the Data Lake Store.</span></span> <span data-ttu-id="c7cc3-148">Egyirányú kikeresni a következő a neve, amely a Resource Manager sablonnal létrehozott HDInsight fürt panel megnyitásához kattintson a **fürt AAD-identitása** lapot, és keresse meg a értékének **egyszerű szolgáltatás megjelenített neve**.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-148">One way to look for the name is to open the HDInsight cluster blade that you created using the Resource Manager template, click the **Cluster AAD Identity** tab, and look for the value of **Service Principal Display Name**.</span></span>
2. <span data-ttu-id="c7cc3-149">Most biztosítanak az Azure AD-alkalmazáshoz való hozzáférés a mappára vagy fájlra, amelyet a HDInsight-fürt eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-149">Now, provide access to this Azure AD application on the file/folder that you want to access from the HDInsight cluster.</span></span> <span data-ttu-id="c7cc3-150">A megfelelő hozzáférés-vezérlési beállítani a fájl vagy mappa a Data Lake Store, lásd: [adatok védelme az Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-150">To set the right ACLs on the file/folder in Data Lake Store, see [Securing data in Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span></span>

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a><span data-ttu-id="c7cc3-151">A HDInsight-fürt használata a Data Lake Store tesztet-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="c7cc3-151">Run test jobs on the HDInsight cluster to use the Data Lake Store</span></span>
<span data-ttu-id="c7cc3-152">Miután konfigurálta a HDInsight-fürtöt, a teszt feladatok ellenőrzéséhez, hogy a HDInsight-fürt hozzáférhet-e a Data Lake Store a fürtön is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-152">After you have configured an HDInsight cluster, you can run test jobs on the cluster to test that the HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="c7cc3-153">Ehhez az szükséges, azt egy minta Hive táblát hoz létre a Data Lake Store korábban feltöltött megadott mintaadatokat használja feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-153">To do so, we will run a sample Hive job that creates a table using the sample data that you uploaded earlier to your Data Lake Store.</span></span>

<span data-ttu-id="c7cc3-154">Ebben a szakaszban fogja SSH egy HDInsight Linux-fürt, és futtassa a minta Hive-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-154">In this section you will SSH into an HDInsight Linux cluster and run the a sample Hive query.</span></span> <span data-ttu-id="c7cc3-155">Ha egy Windows ügyfél használ, azt javasoljuk, **PuTTY**, amely letölthető [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-155">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="c7cc3-156">A PuTTY használatával kapcsolatos további információkért lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-156">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

1. <span data-ttu-id="c7cc3-157">A csatlakozás után a következő parancs használatával indítsa el a Hive CLI:</span><span class="sxs-lookup"><span data-stu-id="c7cc3-157">Once connected, start the Hive CLI by using the following command:</span></span>

   ```
   hive
   ```
2. <span data-ttu-id="c7cc3-158">A parancssori felület használatával adja meg az alábbi állításokat annak nevű új tábla létrehozása **járművekről gyűjtött** által megadott mintaadatokat használja a Data Lake Store-ban:</span><span class="sxs-lookup"><span data-stu-id="c7cc3-158">Using the CLI, enter the following statements to create a new table named **vehicles** by using the sample data in the Data Lake Store:</span></span>

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   <span data-ttu-id="c7cc3-159">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="c7cc3-159">You should see an output similar to the following:</span></span>

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="c7cc3-160">Hozzáférés Data Lake Store HDFS parancs használatával</span><span class="sxs-lookup"><span data-stu-id="c7cc3-160">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="c7cc3-161">Miután konfigurálta a Data Lake Store használata a HDInsight-fürthöz, a HDFS felületparancsokat használhatja az áruház eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-161">Once you have configured the HDInsight cluster to use Data Lake Store, you can use the HDFS shell commands to access the store.</span></span>

<span data-ttu-id="c7cc3-162">Ebben a szakaszban fogja SSH egy HDInsight Linux a fürt és a HDFS parancsokat.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-162">In this section you will SSH into an HDInsight Linux cluster and run the HDFS commands.</span></span> <span data-ttu-id="c7cc3-163">Ha egy Windows ügyfél használ, azt javasoljuk, **PuTTY**, amely letölthető [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-163">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="c7cc3-164">A PuTTY használatával kapcsolatos további információkért lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="c7cc3-164">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

<span data-ttu-id="c7cc3-165">A csatlakozás után a következő HDFS filesystem parancs segítségével a Data Lake Store található fájlok listázása.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-165">Once connected, use the following HDFS filesystem command to list the files in the Data Lake Store.</span></span>

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

<span data-ttu-id="c7cc3-166">Megjelenik a korábban a Data Lake Store-bA feltöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-166">This should list the file that you uploaded earlier to the Data Lake Store.</span></span>

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

<span data-ttu-id="c7cc3-167">Használhatja a `hdfs dfs -put` parancs egyes fájlok feltöltése a Data Lake Store, és ezután `hdfs dfs -ls` ellenőrzése, hogy a fájlok sikeresen feltöltve.</span><span class="sxs-lookup"><span data-stu-id="c7cc3-167">You can also use the `hdfs dfs -put` command to upload some files to the Data Lake Store, and then use `hdfs dfs -ls` to verify whether the files were successfully uploaded.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c7cc3-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7cc3-168">Next steps</span></span>
* [<span data-ttu-id="c7cc3-169">Adatok másolása az Azure Storage Blobs Data Lake Store-bA</span><span class="sxs-lookup"><span data-stu-id="c7cc3-169">Copy data from Azure Storage Blobs to Data Lake Store</span></span>](data-lake-store-copy-data-wasb-distcp.md)
