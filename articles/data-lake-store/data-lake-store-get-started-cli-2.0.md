---
title: "Az Azure Data Lake Store használatának első lépései az Azure 2.0-s verziójú parancssori felületével | Microsoft Docs"
description: "Data Lake Store-fiók létrehozása és alapszintű műveletek végrehajtása az Azure 2.0-s verziójú, platformfüggetlen parancssorával"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 4ffa0f4a-1cca-46ac-803d-1fc8538c685b
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: ed78d25f2bac0a9996f1796ee503f31a36940977
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a><span data-ttu-id="f2273-103">Az Azure Data Lake Store használatának első lépései az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="f2273-103">Get started with Azure Data Lake Store using Azure CLI 2.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f2273-104">Portal</span><span class="sxs-lookup"><span data-stu-id="f2273-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="f2273-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f2273-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="f2273-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="f2273-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="f2273-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="f2273-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="f2273-108">REST API</span><span class="sxs-lookup"><span data-stu-id="f2273-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="f2273-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f2273-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="f2273-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="f2273-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="f2273-111">Python</span><span class="sxs-lookup"><span data-stu-id="f2273-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="f2273-112">Ismerje meg, hogyan hozhat létre Azure Data Lake Store-fiókot az Azure CLI 2.0 használatával, illetve hogyan végezhet el olyan alapvető műveleteket, mint például mappák létrehozása, adatfájlok le- és feltöltése, fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).</span><span class="sxs-lookup"><span data-stu-id="f2273-112">Learn how to use Azure CLI 2.0 to create an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="f2273-113">Az Azure CLI 2.0 az Azure új parancssori felülete, amely Azure-erőforrások felügyeletére szolgál.</span><span class="sxs-lookup"><span data-stu-id="f2273-113">The Azure CLI 2.0 is Azure's new command-line experience for managing Azure resources.</span></span> <span data-ttu-id="f2273-114">A szolgáltatás macOS, Linux és Windows rendszereken használható.</span><span class="sxs-lookup"><span data-stu-id="f2273-114">It can be used on macOS, Linux, and Windows.</span></span> <span data-ttu-id="f2273-115">További információért lásd: [Az Azure CLI 2.0 áttekintése](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f2273-115">For more information, see [Overview of Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).</span></span> <span data-ttu-id="f2273-116">A parancsok és a szintaxis teljes listája az [Azure Data Lake Store CLI 2.0 dokumentációjában](https://docs.microsoft.com/cli/azure/dls) található.</span><span class="sxs-lookup"><span data-stu-id="f2273-116">You can also look at the [Azure Data Lake Store CLI 2.0 reference](https://docs.microsoft.com/cli/azure/dls) for a complete list of commands and syntax.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f2273-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f2273-117">Prerequisites</span></span>
<span data-ttu-id="f2273-118">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="f2273-118">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="f2273-119">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="f2273-119">**An Azure subscription**.</span></span> <span data-ttu-id="f2273-120">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f2273-120">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="f2273-121">**Azure CLI 2.0** – az utasításokért lásd: [Az Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f2273-121">**Azure CLI 2.0** - See [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) for instructions.</span></span>

## <a name="authentication"></a><span data-ttu-id="f2273-122">Authentication</span><span class="sxs-lookup"><span data-stu-id="f2273-122">Authentication</span></span>

<span data-ttu-id="f2273-123">Ez a cikk egy egyszerűbb hitelesítési módszert használ a Data Lake Store-ral, ahol Ön végfelhasználóként jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="f2273-123">This article uses a simpler authentication approach with Data Lake Store where you log in as an end-user user.</span></span> <span data-ttu-id="f2273-124">Ezután a Data Lake Store-fiókhoz és a fájlrendszerhez való hozzáférés szintje a bejelentkezett felhasználó hozzáférési szintjétől függ.</span><span class="sxs-lookup"><span data-stu-id="f2273-124">The access level to Data Lake Store account and file system is then governed by the access level of the logged in user.</span></span> <span data-ttu-id="f2273-125">Azonban a Data Lake Store-ral más módokon is lehet hitelesíteni. Ezek a következők: **végfelhasználói hitelesítés** vagy **szolgáltatások közötti hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="f2273-125">However, there are other approaches as well to authenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="f2273-126">Útmutatás a hitelesítéshez és további tudnivalók a [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md) vagy a [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md) című témakörben.</span><span class="sxs-lookup"><span data-stu-id="f2273-126">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>


## <a name="log-in-to-your-azure-subscription"></a><span data-ttu-id="f2273-127">Bejelentkezés az Azure-előfizetésbe</span><span class="sxs-lookup"><span data-stu-id="f2273-127">Log in to your Azure subscription</span></span>

1. <span data-ttu-id="f2273-128">Jelentkezzen be az Azure-előfizetésébe.</span><span class="sxs-lookup"><span data-stu-id="f2273-128">Log into your Azure subscription.</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="f2273-129">Kap egy kódot a következő lépésben való használatra.</span><span class="sxs-lookup"><span data-stu-id="f2273-129">You get a code to use in the next step.</span></span> <span data-ttu-id="f2273-130">A webböngészővel nyissa meg a https://aka.ms/devicelogin oldalt, és a hitelesítéshez adja meg a kódot.</span><span class="sxs-lookup"><span data-stu-id="f2273-130">Use a web browser to open the page https://aka.ms/devicelogin and enter the code to authenticate.</span></span> <span data-ttu-id="f2273-131">A rendszer kéri a hitelesítési adatokkal való bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f2273-131">You are prompted to log in using your credentials.</span></span>

2. <span data-ttu-id="f2273-132">Bejelentkezés után az ablakban megjelenő listában találhatók a fiókhoz társított Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="f2273-132">Once you log in, the window lists all the Azure subscriptions that are associated with your account.</span></span> <span data-ttu-id="f2273-133">Az alábbi paranccsal használhat egy adott előfizetést.</span><span class="sxs-lookup"><span data-stu-id="f2273-133">Use the following command to use a specific subscription.</span></span>
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="f2273-134">Azure Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2273-134">Create an Azure Data Lake Store account</span></span>

1. <span data-ttu-id="f2273-135">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="f2273-135">Create a new resource group.</span></span> <span data-ttu-id="f2273-136">Az alábbi parancsban adja meg a használni kívánt paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="f2273-136">In the following command, provide the parameter values you want to use.</span></span> <span data-ttu-id="f2273-137">Ha a hely neve tartalmaz szóközöket, használjon idézőjeleket.</span><span class="sxs-lookup"><span data-stu-id="f2273-137">If the location name contains spaces, put it in quotes.</span></span> <span data-ttu-id="f2273-138">Például: „USA 2. keleti régiója”.</span><span class="sxs-lookup"><span data-stu-id="f2273-138">For example "East US 2".</span></span> 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. <span data-ttu-id="f2273-139">Hozza létre a Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="f2273-139">Create the Data Lake Store account.</span></span>
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="f2273-140">Mappák létrehozása Data Lake Store-fiókban</span><span class="sxs-lookup"><span data-stu-id="f2273-140">Create folders in a Data Lake Store account</span></span>

<span data-ttu-id="f2273-141">Mappákat hozhat létre az Azure Data Lake Store-fiókjában az adatok kezelése és tárolása céljából.</span><span class="sxs-lookup"><span data-stu-id="f2273-141">You can create folders under your Azure Data Lake Store account to manage and store data.</span></span> <span data-ttu-id="f2273-142">Az alábbi parancs segítségével hozzon létre egy **mynewfolder** nevű mappát a Data Lake Store gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="f2273-142">Use the following command to create a folder called **mynewfolder** at the root of the Data Lake Store.</span></span>

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> <span data-ttu-id="f2273-143">A `--folder` paraméter gondoskodik arról, hogy a parancs egy mappát hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="f2273-143">The `--folder` parameter ensures that the command creates a folder.</span></span> <span data-ttu-id="f2273-144">Ha a paraméter hiányzik, a parancs egy mynewfolder nevű üres fájlt hoz létre a Data Lake Store-fiók gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="f2273-144">If this parameter is not present, the command creates an empty file called mynewfolder at the root of the Data Lake Store account.</span></span>
> 
>

## <a name="upload-data-to-a-data-lake-store-account"></a><span data-ttu-id="f2273-145">Adatok feltöltése a Data Lake Store-fiókba</span><span class="sxs-lookup"><span data-stu-id="f2273-145">Upload data to a Data Lake Store account</span></span>

<span data-ttu-id="f2273-146">Az adatokat közvetlenül a Data Lake Store gyökérmappájába vagy a fiókban létrehozott egyik mappába töltheti fel.</span><span class="sxs-lookup"><span data-stu-id="f2273-146">You can upload data to Data Lake Store directly at the root level or to a folder that you created within the account.</span></span> <span data-ttu-id="f2273-147">Az alábbi kódtöredékek bemutatják, hogyan tölthet fel néhány adatot az előző szakaszban létrehozott mappába (**mynewfolder**).</span><span class="sxs-lookup"><span data-stu-id="f2273-147">The snippets below demonstrate how to upload some sample data to the folder (**mynewfolder**) you created in the previous section.</span></span>

<span data-ttu-id="f2273-148">Ha feltölthető mintaadatokra van szüksége, használhatja az [Azure Data Lake Git-tárában](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) található **Ambulance Data** mappát.</span><span class="sxs-lookup"><span data-stu-id="f2273-148">If you are looking for some sample data to upload, you can get the **Ambulance Data** folder from the [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="f2273-149">Töltse le a fájlt, és tárolja a számítógépén egy helyi könyvtárban (pl. C:\sampledata).</span><span class="sxs-lookup"><span data-stu-id="f2273-149">Download the file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> <span data-ttu-id="f2273-150">Célként adja meg a teljes elérési utat, beleértve a fájlnevet is.</span><span class="sxs-lookup"><span data-stu-id="f2273-150">For the destination, you must specify the complete path including the file name.</span></span>
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a><span data-ttu-id="f2273-151">A Data Lake Store-fiók fájljainak listázása</span><span class="sxs-lookup"><span data-stu-id="f2273-151">List files in a Data Lake Store account</span></span>

<span data-ttu-id="f2273-152">Az alábbi parancs segítségével kilistázhatja a Data Lake Store-fiók fájljait.</span><span class="sxs-lookup"><span data-stu-id="f2273-152">Use the following command to list the files in a Data Lake Store account.</span></span>

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

<span data-ttu-id="f2273-153">A kimenet az alábbihoz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="f2273-153">The output of this should be similar to the following:</span></span>

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a><span data-ttu-id="f2273-154">A Data Lake Store-fiókban lévő adatok átnevezése, letöltése és törlése</span><span class="sxs-lookup"><span data-stu-id="f2273-154">Rename, download, and delete data from a Data Lake Store account</span></span> 

* <span data-ttu-id="f2273-155">**Fájlok átnevezéséhez** használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="f2273-155">**To rename a file**, use the following command:</span></span>
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* <span data-ttu-id="f2273-156">**Fájlok letöltéséhez** használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="f2273-156">**To download a file**, use the following command.</span></span> <span data-ttu-id="f2273-157">Ügyeljen arra, hogy a megadott cél elérési útja egy létező hely legyen.</span><span class="sxs-lookup"><span data-stu-id="f2273-157">Make sure the destination path you specify already exists.</span></span>
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > <span data-ttu-id="f2273-158">A parancs létrehozza a célmappát, ha az nem létezik.</span><span class="sxs-lookup"><span data-stu-id="f2273-158">The command creates the destination folder if it does not exist.</span></span>
    > 
    >

* <span data-ttu-id="f2273-159">**Fájlok törléséhez** használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="f2273-159">**To delete a file**, use the following command:</span></span>
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    <span data-ttu-id="f2273-160">Ha egyetlen paranccsal szeretné törölni a **mynewfolder** nevű mappát és a **vehicle1_09142014_copy.csv** nevű fájlt, használja a --recurse paramétert</span><span class="sxs-lookup"><span data-stu-id="f2273-160">If you want to delete the folder **mynewfolder** and the file **vehicle1_09142014_copy.csv** together in one command, use the --recurse parameter</span></span>

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a><span data-ttu-id="f2273-161">A Data Lake Store-fiókhoz tartozó engedélyek és a hozzáférés-vezérlési listák használata</span><span class="sxs-lookup"><span data-stu-id="f2273-161">Work with permissions and ACLs for a Data Lake Store account</span></span>

<span data-ttu-id="f2273-162">Ebben a szakaszban a hozzáférés-vezérlési listák és az engedélyek Azure CLI 2.0-beli felügyeletét ismerheti meg.</span><span class="sxs-lookup"><span data-stu-id="f2273-162">In this section you learn about how to manage ACLs and permissions using Azure CLI 2.0.</span></span> <span data-ttu-id="f2273-163">A hozzáférés-vezérlési listák Azure Data Lake Store-beli használatának részletes leírásáért lásd: [Az Azure Data Lake Store szolgáltatásban található hozzáférés-vezérlés](data-lake-store-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="f2273-163">For a detailed discussion on how ACLs are implemented in Azure Data Lake Store, see [Access control in Azure Data Lake Store](data-lake-store-access-control.md).</span></span>

* <span data-ttu-id="f2273-164">**Egy fájl/mappa tulajdonosának frissítését** az alábbi paranccsal végezheti el:</span><span class="sxs-lookup"><span data-stu-id="f2273-164">**To update the owner of a file/folder**, use the following command:</span></span>

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="f2273-165">**Egy fájl/mappa engedélyeinek frissítését** az alábbi paranccsal végezheti el:</span><span class="sxs-lookup"><span data-stu-id="f2273-165">**To update the permissions for a file/folder**, use the following command:</span></span>

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* <span data-ttu-id="f2273-166">**Adott elérési úthoz tartozó hozzáférés-vezérlési listák beolvasását** az alábbi paranccsal végezheti el:</span><span class="sxs-lookup"><span data-stu-id="f2273-166">**To get the ACLs for a given path**, use the following command:</span></span>

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    <span data-ttu-id="f2273-167">A kimenet az alábbihoz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="f2273-167">The output should be similar to the following:</span></span>

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* <span data-ttu-id="f2273-168">**Egy hozzáférés-vezérlési listához tartozó bejegyzés beállítását** az alábbi paranccsal végezheti el:</span><span class="sxs-lookup"><span data-stu-id="f2273-168">**To set an entry for an ACL**, use the following command:</span></span>

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* <span data-ttu-id="f2273-169">**Egy hozzáférés-vezérlési listához tartozó bejegyzés eltávolítását** az alábbi paranccsal végezheti el:</span><span class="sxs-lookup"><span data-stu-id="f2273-169">**To remove an entry for an ACL**, use the following command:</span></span>

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="f2273-170">**Egy alapértelmezett teljes hozzáférés-vezérlési lista eltávolítását** az alábbi paranccsal végezheti el:</span><span class="sxs-lookup"><span data-stu-id="f2273-170">**To remove an entire default ACL**, use the following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* <span data-ttu-id="f2273-171">**Egy nem alapértelmezett teljes hozzáférés-vezérlési lista eltávolítását** az alábbi paranccsal végezheti el:</span><span class="sxs-lookup"><span data-stu-id="f2273-171">**To remove an entire non-default ACL**, use the following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="f2273-172">Data Lake Store-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="f2273-172">Delete a Data Lake Store account</span></span>
<span data-ttu-id="f2273-173">Az alábbi parancs segítségével törölheti a Data Lake Store-fiókját.</span><span class="sxs-lookup"><span data-stu-id="f2273-173">Use the following command to delete a Data Lake Store account.</span></span>

```azurecli
az dls account delete --account mydatalakestore
```

<span data-ttu-id="f2273-174">Ha a rendszer rákérdez, írja be az **Y** karaktert a fiók törléséhez.</span><span class="sxs-lookup"><span data-stu-id="f2273-174">When prompted, enter **Y** to delete the account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2273-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2273-175">Next steps</span></span>

* [<span data-ttu-id="f2273-176">Az Azure Data Lake Store CLI 2.0 dokumentációja</span><span class="sxs-lookup"><span data-stu-id="f2273-176">Azure Data Lake Store CLI 2.0 reference</span></span>](https://docs.microsoft.com/cli/azure/dls)
* [<span data-ttu-id="f2273-177">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="f2273-177">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="f2273-178">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="f2273-178">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="f2273-179">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="f2273-179">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md
