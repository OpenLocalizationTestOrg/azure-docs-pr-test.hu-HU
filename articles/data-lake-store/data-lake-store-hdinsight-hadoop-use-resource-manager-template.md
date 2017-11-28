---
title: "Azure-sablonok toocreate aaaUse HDInsight és a Data Lake Store |} Microsoft Docs"
description: "Azure Resource Manager sablonok toocreate igénybe, valamint a HDInsight-fürtök az Azure Data Lake Store"
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
ms.openlocfilehash: eb88a626f2837dcc29295f3f73a91757059c3bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a><span data-ttu-id="9dfe7-103">HDInsight-fürtök létrehozása a Data Lake Store Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="9dfe7-103">Create an HDInsight cluster with Data Lake Store using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9dfe7-104">A Portal használata</span><span class="sxs-lookup"><span data-stu-id="9dfe7-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="9dfe7-105">PowerShell használatával (az alapértelmezett tároló)</span><span class="sxs-lookup"><span data-stu-id="9dfe7-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="9dfe7-106">(Tárhely) a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9dfe7-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="9dfe7-107">Erőforrás-kezelő használatával</span><span class="sxs-lookup"><span data-stu-id="9dfe7-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="9dfe7-108">Ismerje meg, hogyan toouse Azure PowerShell tooconfigure egy HDInsight-fürtöt az Azure Data Lake Store **további tárolóként**.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-108">Learn how toouse Azure PowerShell tooconfigure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span>

<span data-ttu-id="9dfe7-109">Támogatott fürttípusok Data Lake Store alapértelmezett tároló vagy további tárfiókot kell használni.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-109">For supported cluster types, Data Lake Store be used as an default storage or additional storage account.</span></span> <span data-ttu-id="9dfe7-110">Ha a Data Lake Store további tárterületet, hello alapértelmezett tárfiók hello fürtök továbbra is Azure Storage Blobs (WASB), és hello fürt kapcsolatos fájlokhoz (például naplói, stb.) továbbra is írt toohello alapértelmezett tárolási hello adatainak közben, amikor szeretné, hogy tooprocess tárolható Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-110">When Data Lake Store is used as additional storage, hello default storage account for hello clusters will still be Azure Storage Blobs (WASB) and hello cluster-related files (such as logs, etc.) are still written toohello default storage, while hello data that you want tooprocess can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="9dfe7-111">További tárhely fiókként használatával a Data Lake Store nem befolyásolja a teljesítményt és a hello képességét tooread/írás toohello tárolást hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-111">Using Data Lake Store as an additional storage account does not impact performance or hello ability tooread/write toohello storage from hello cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="9dfe7-112">Data Lake Store használata a HDInsight-fürt tárolására</span><span class="sxs-lookup"><span data-stu-id="9dfe7-112">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="9dfe7-113">Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:</span><span class="sxs-lookup"><span data-stu-id="9dfe7-113">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="9dfe7-114">Beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store alapértelmezett tárolóként esetén HDInsight 3.5-ös és 3.6 verzió érhető el.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-114">Option toocreate HDInsight clusters with access tooData Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="9dfe7-115">Beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store további tárolóként verzióihoz áll rendelkezésre HDInsight 3.2-es, 3.4, 3.5-ös és 3.6.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-115">Option toocreate HDInsight clusters with access tooData Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="9dfe7-116">Ebben a cikkben azt a Data Lake Store a Hadoop fürtök a további tárolóként kell kiépíteni.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-116">In this article, we provision a Hadoop cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="9dfe7-117">Hogyan toocreate egy Hadoop-fürtöt Data Lake Store alapértelmezett tárolóként, lásd: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-117">For instructions on how toocreate a Hadoop cluster with Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9dfe7-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9dfe7-118">Prerequisites</span></span>
<span data-ttu-id="9dfe7-119">Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="9dfe7-119">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="9dfe7-120">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-120">**An Azure subscription**.</span></span> <span data-ttu-id="9dfe7-121">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-121">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9dfe7-122">Az **Azure PowerShell 1.0-s vagy újabb verziója**.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-122">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="9dfe7-123">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="9dfe7-124">**Az Azure Active Directory szolgáltatás egyszerű**.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-124">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="9dfe7-125">Ez az oktatóanyag lépéseit ad útmutatást toocreate egy egyszerű Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-125">Steps in this tutorial provide instructions on how toocreate a service principal in Azure AD.</span></span> <span data-ttu-id="9dfe7-126">Azonban az Azure AD rendszergazdai toobe képes toocreate szolgáltatásnevet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-126">However, you must be an Azure AD administrator toobe able toocreate a service principal.</span></span> <span data-ttu-id="9dfe7-127">Ha az Azure AD-rendszergazdaként, hagyja ki ezt az előfeltételt, és hello oktatóanyag folytatásához.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-127">If you are an Azure AD administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    <span data-ttu-id="9dfe7-128">**Ha nem az Azure AD-rendszergazda**, nem fogja tudni tooperform hello lépéseket szükséges toocreate egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-128">**If you are not an Azure AD administrator**, you will not be able tooperform hello steps required toocreate a service principal.</span></span> <span data-ttu-id="9dfe7-129">Ebben az esetben az Azure AD-rendszergazda először létre kell hoznia egy egyszerű szolgáltatást a Data Lake Store egy HDInsight-fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-129">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="9dfe7-130">Emellett hello szolgáltatás egyszerű segítségével kell létrehozni egy tanúsítványt, részben ismertetett módon [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-130">Also, hello service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a><span data-ttu-id="9dfe7-131">HDInsight-fürtök létrehozása az Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="9dfe7-131">Create an HDInsight cluster with Azure Data Lake Store</span></span>
<span data-ttu-id="9dfe7-132">hello Resource Manager-sablon, és hello előfeltételei a hello sablon legyenek elérhetők a Githubon: [az új Data Lake Store HDInsight Linux fürt központi telepítése](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-132">hello Resource Manager template, and hello prerequisites for using hello template, are available on GitHub at [Deploy a HDInsight Linux cluster with new Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span></span> <span data-ttu-id="9dfe7-133">Kövesse hello megjelenő utasításokat, ez a hivatkozás toocreate HDInsight-fürtök az Azure Data Lake Store hello további tárolóként.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-133">Follow hello instructions provided at this link toocreate an HDInsight cluster with Azure Data Lake Store as hello additional storage.</span></span>

<span data-ttu-id="9dfe7-134">hello található utasítások segítségével: hello hivatkozásra a fenti PowerShell igényelnek.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-134">hello instructions at hello link mentioned above require PowerShell.</span></span> <span data-ttu-id="9dfe7-135">Ezeket az utasításokat a Kezdés előtt győződjön meg arról tooyour Azure-fiók a bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-135">Before you start with those instructions, make sure you log in tooyour Azure account.</span></span> <span data-ttu-id="9dfe7-136">Az asztalon nyisson meg egy új Azure PowerShell-ablakot, és adja meg az alábbi kódtöredékek hello.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-136">From your desktop, open a new Azure PowerShell window, and enter hello following snippets.</span></span> <span data-ttu-id="9dfe7-137">Amikor felszólító toolog, győződjön meg arról, hogy jelentkezik be a rendelkezésre álló hello előfizetés rendszergazdájaként/tulajdonosaként:</span><span class="sxs-lookup"><span data-stu-id="9dfe7-137">When prompted toolog in, make sure you log in as one of hello subscription admininistrators/owner:</span></span>

```
# Log in tooyour Azure account
Login-AzureRmAccount

# List all hello subscriptions associated tooyour account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-toohello-azure-data-lake-store"></a><span data-ttu-id="9dfe7-138">A minta adatok toohello Azure Data Lake Store feltöltése</span><span class="sxs-lookup"><span data-stu-id="9dfe7-138">Upload sample data toohello Azure Data Lake Store</span></span>
<span data-ttu-id="9dfe7-139">hello Resource Manager-sablon létrehoz egy új Data Lake Store-fiókot, és társítja azt hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-139">hello Resource Manager template creates a new Data Lake Store account and associates it with hello HDInsight cluster.</span></span> <span data-ttu-id="9dfe7-140">Néhány példa adatok toohello Data Lake Store most kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-140">You must now upload some sample data toohello Data Lake Store.</span></span> <span data-ttu-id="9dfe7-141">Ezeket az adatokat később a hello oktatóanyag toorun feladatok a HDInsight-fürtöt, amely a Data Lake Store hello adatelérés lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-141">You'll need this data later in hello tutorial toorun jobs from an HDInsight cluster that access data in hello Data Lake Store.</span></span> <span data-ttu-id="9dfe7-142">Útmutatást tooupload adatokat, lásd: [feltöltése egy Data Lake Store fájl tooyour](data-lake-store-get-started-portal.md#uploaddata).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-142">For instructions on how tooupload data, see [Upload a file tooyour Data Lake Store](data-lake-store-get-started-portal.md#uploaddata).</span></span> <span data-ttu-id="9dfe7-143">Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-143">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

## <a name="set-relevant-acls-on-hello-sample-data"></a><span data-ttu-id="9dfe7-144">Hello mintaadatok vonatkozó hozzáférés-vezérlési listák beállítása</span><span class="sxs-lookup"><span data-stu-id="9dfe7-144">Set relevant ACLs on hello sample data</span></span>
<span data-ttu-id="9dfe7-145">toomake, hogy a HDInsight-fürt hello elérhető-e a feltöltött hello mintaadatok, gondoskodnia kell arról, hogy rendelkezik-e hozzáférési toohello fájl vagy mappa Ön hello Azure AD-alkalmazást, amely hello HDInsight-fürt és a Data Lake Store között használt tooestablish identitás Kísérlet történt tooaccess.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-145">toomake sure hello sample data you upload is accessible from hello HDInsight cluster, you must ensure that hello Azure AD application that is used tooestablish identity between hello HDInsight cluster and Data Lake Store has access toohello file/folder you are trying tooaccess.</span></span> <span data-ttu-id="9dfe7-146">toodo, hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-146">toodo this, perform hello following steps.</span></span>

1. <span data-ttu-id="9dfe7-147">Hello nevét hello HDInsight-fürthöz társított Azure AD-alkalmazást és a Data Lake Store hello található.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-147">Find hello name of hello Azure AD application that is associated with HDInsight cluster and hello Data Lake Store.</span></span> <span data-ttu-id="9dfe7-148">Egyirányú toolook hello neve tooopen hello HDInsight fürt paneljén hello Resource Manager sablonnal létrehozott, kattintson a hello **fürt AAD-identitása** lapra, és keressen a hello értékének **egyszerű szolgáltatásnév Megjelenítendő név**.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-148">One way toolook for hello name is tooopen hello HDInsight cluster blade that you created using hello Resource Manager template, click hello **Cluster AAD Identity** tab, and look for hello value of **Service Principal Display Name**.</span></span>
2. <span data-ttu-id="9dfe7-149">Most biztosítanak hozzáférést toothis az Azure AD-alkalmazást hello mappára vagy fájlra, amelyet a HDInsight-fürt hello tooaccess.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-149">Now, provide access toothis Azure AD application on hello file/folder that you want tooaccess from hello HDInsight cluster.</span></span> <span data-ttu-id="9dfe7-150">tooset hello jobb hozzáférés-vezérlési listák hello fájlt vagy mappát a Data Lake Store, lásd: [adatok védelme az Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-150">tooset hello right ACLs on hello file/folder in Data Lake Store, see [Securing data in Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span></span>

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a><span data-ttu-id="9dfe7-151">Hello HDInsight fürt toouse hello Data Lake Store a teszt feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="9dfe7-151">Run test jobs on hello HDInsight cluster toouse hello Data Lake Store</span></span>
<span data-ttu-id="9dfe7-152">Miután konfigurálta a HDInsight-fürtöt, futtathatja Tesztfeladatok hello fürt tootest adott hello HDInsight fürt Data Lake Store férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-152">After you have configured an HDInsight cluster, you can run test jobs on hello cluster tootest that hello HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="9dfe7-153">toodo úgy, hogy fog futni egy minta Hive-feladatot, amely táblát hoz létre, hogy a korábbi tooyour Data Lake Store feltöltött hello mintaadatok használatával.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-153">toodo so, we will run a sample Hive job that creates a table using hello sample data that you uploaded earlier tooyour Data Lake Store.</span></span>

<span data-ttu-id="9dfe7-154">Ebben a szakaszban fogja SSH egy HDInsight Linux-fürt és a futási hello minta Hive-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-154">In this section you will SSH into an HDInsight Linux cluster and run hello a sample Hive query.</span></span> <span data-ttu-id="9dfe7-155">Ha egy Windows ügyfél használ, azt javasoljuk, **PuTTY**, amely letölthető [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-155">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="9dfe7-156">A PuTTY használatával kapcsolatos további információkért lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-156">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

1. <span data-ttu-id="9dfe7-157">A csatlakozás után indítsa el a hello Hive CLI hello a következő parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="9dfe7-157">Once connected, start hello Hive CLI by using hello following command:</span></span>

   ```
   hive
   ```
2. <span data-ttu-id="9dfe7-158">Hello CLI, adja meg a következő utasítások toocreate nevű új tábla hello **járművekről gyűjtött** hello mintaadatok használatával a Data Lake Store hello:</span><span class="sxs-lookup"><span data-stu-id="9dfe7-158">Using hello CLI, enter hello following statements toocreate a new table named **vehicles** by using hello sample data in hello Data Lake Store:</span></span>

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   <span data-ttu-id="9dfe7-159">Egy kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="9dfe7-159">You should see an output similar toohello following:</span></span>

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


## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="9dfe7-160">Hozzáférés Data Lake Store HDFS parancs használatával</span><span class="sxs-lookup"><span data-stu-id="9dfe7-160">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="9dfe7-161">Miután konfigurálta a hello HDInsight fürt toouse Data Lake Store, hello HDFS rendszerhéj parancsok tooaccess hello tároló is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-161">Once you have configured hello HDInsight cluster toouse Data Lake Store, you can use hello HDFS shell commands tooaccess hello store.</span></span>

<span data-ttu-id="9dfe7-162">Ebben a szakaszban fogja SSH egy HDInsight Linux-fürt és a futási hello HDFS parancs.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-162">In this section you will SSH into an HDInsight Linux cluster and run hello HDFS commands.</span></span> <span data-ttu-id="9dfe7-163">Ha egy Windows ügyfél használ, azt javasoljuk, **PuTTY**, amely letölthető [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-163">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="9dfe7-164">A PuTTY használatával kapcsolatos további információkért lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="9dfe7-164">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

<span data-ttu-id="9dfe7-165">Miután csatlakozott, használja a következő HDFS hello filesystem parancs toolist fájlok hello Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-165">Once connected, use hello following HDFS filesystem command toolist hello files in hello Data Lake Store.</span></span>

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

<span data-ttu-id="9dfe7-166">Hello fájlt, hogy a korábbi toohello Data Lake Store feltöltött megjelenik.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-166">This should list hello file that you uploaded earlier toohello Data Lake Store.</span></span>

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

<span data-ttu-id="9dfe7-167">Is használhatja a hello `hdfs dfs -put` tooupload bizonyos fájlok toohello Data Lake Store parancsot, és ezután `hdfs dfs -ls` tooverify e hello fájlok sikeresen feltöltve.</span><span class="sxs-lookup"><span data-stu-id="9dfe7-167">You can also use hello `hdfs dfs -put` command tooupload some files toohello Data Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9dfe7-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9dfe7-168">Next steps</span></span>
* [<span data-ttu-id="9dfe7-169">Adatok másolása az Azure Storage Blobs tooData Lake Store-ból</span><span class="sxs-lookup"><span data-stu-id="9dfe7-169">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-wasb-distcp.md)
