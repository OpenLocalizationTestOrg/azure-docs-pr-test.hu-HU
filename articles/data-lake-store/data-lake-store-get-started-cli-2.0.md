---
title: "az Azure parancssori 2.0 aaaUse felület tooget Azure Data Lake Store használatába |} Microsoft Docs"
description: "Használja az Azure platformfüggetlen parancssori 2.0 toocreate Data Lake Store-fiók és alapszintű műveletek végrehajtása"
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
ms.openlocfilehash: 374dcd6cdbc13ad19f6c65502329986ecae60ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a><span data-ttu-id="0d2fd-103">Az Azure Data Lake Store használatának első lépései az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="0d2fd-103">Get started with Azure Data Lake Store using Azure CLI 2.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0d2fd-104">Portal</span><span class="sxs-lookup"><span data-stu-id="0d2fd-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="0d2fd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d2fd-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="0d2fd-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="0d2fd-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="0d2fd-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="0d2fd-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="0d2fd-108">REST API</span><span class="sxs-lookup"><span data-stu-id="0d2fd-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="0d2fd-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0d2fd-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="0d2fd-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="0d2fd-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="0d2fd-111">Python</span><span class="sxs-lookup"><span data-stu-id="0d2fd-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="0d2fd-112">Ismerje meg, hogyan toouse Azure CLI 2.0 toocreate egy Azure Data Lake tárolásához fiók és alapvető műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).</span><span class="sxs-lookup"><span data-stu-id="0d2fd-112">Learn how toouse Azure CLI 2.0 toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="0d2fd-113">hello Azure CLI 2.0 Azure új parancssori felületet Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-113">hello Azure CLI 2.0 is Azure's new command-line experience for managing Azure resources.</span></span> <span data-ttu-id="0d2fd-114">A szolgáltatás macOS, Linux és Windows rendszereken használható.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-114">It can be used on macOS, Linux, and Windows.</span></span> <span data-ttu-id="0d2fd-115">További információért lásd: [Az Azure CLI 2.0 áttekintése](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0d2fd-115">For more information, see [Overview of Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).</span></span> <span data-ttu-id="0d2fd-116">Is megtalálhatja hello [Azure Data Lake Store CLI 2.0 hivatkozás](https://docs.microsoft.com/cli/azure/dls) teljes listáját a parancsokat és szintaxist.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-116">You can also look at hello [Azure Data Lake Store CLI 2.0 reference](https://docs.microsoft.com/cli/azure/dls) for a complete list of commands and syntax.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0d2fd-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0d2fd-117">Prerequisites</span></span>
<span data-ttu-id="0d2fd-118">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-118">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="0d2fd-119">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-119">**An Azure subscription**.</span></span> <span data-ttu-id="0d2fd-120">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d2fd-120">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="0d2fd-121">**Azure CLI 2.0** – az utasításokért lásd: [Az Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0d2fd-121">**Azure CLI 2.0** - See [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) for instructions.</span></span>

## <a name="authentication"></a><span data-ttu-id="0d2fd-122">Authentication</span><span class="sxs-lookup"><span data-stu-id="0d2fd-122">Authentication</span></span>

<span data-ttu-id="0d2fd-123">Ez a cikk egy egyszerűbb hitelesítési módszert használ a Data Lake Store-ral, ahol Ön végfelhasználóként jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-123">This article uses a simpler authentication approach with Data Lake Store where you log in as an end-user user.</span></span> <span data-ttu-id="0d2fd-124">hello hozzáférési szint tooData Lake Store fiók és a fájl rendszer majd hello hozzáférési szint a bejelentkezett felhasználó hello szabályozza.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-124">hello access level tooData Lake Store account and file system is then governed by hello access level of hello logged in user.</span></span> <span data-ttu-id="0d2fd-125">Van azonban más megoldások jól tooauthenticate a Data Lake Store, mint amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-125">However, there are other approaches as well tooauthenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="0d2fd-126">További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="0d2fd-126">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>


## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="0d2fd-127">Jelentkezzen be tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="0d2fd-127">Log in tooyour Azure subscription</span></span>

1. <span data-ttu-id="0d2fd-128">Jelentkezzen be az Azure-előfizetésébe.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-128">Log into your Azure subscription.</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="0d2fd-129">A kód toouse hello következő lépésben kapott.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-129">You get a code toouse in hello next step.</span></span> <span data-ttu-id="0d2fd-130">Egy webes böngésző tooopen hello lap https://aka.ms/devicelogin használata, és írja be a hello kód tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-130">Use a web browser tooopen hello page https://aka.ms/devicelogin and enter hello code tooauthenticate.</span></span> <span data-ttu-id="0d2fd-131">A hitelesítő adataival felszólító toolog áll.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-131">You are prompted toolog in using your credentials.</span></span>

2. <span data-ttu-id="0d2fd-132">Jelentkezzen be, miután hello ablak felsorolja az összes hello Azure-fiókjához társított előfizetéseket.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-132">Once you log in, hello window lists all hello Azure subscriptions that are associated with your account.</span></span> <span data-ttu-id="0d2fd-133">A következő parancs toouse adott előfizetés hello használata.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-133">Use hello following command toouse a specific subscription.</span></span>
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="0d2fd-134">Azure Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d2fd-134">Create an Azure Data Lake Store account</span></span>

1. <span data-ttu-id="0d2fd-135">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-135">Create a new resource group.</span></span> <span data-ttu-id="0d2fd-136">A következő parancs hello adja meg hello toouse kívánt paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-136">In hello following command, provide hello parameter values you want toouse.</span></span> <span data-ttu-id="0d2fd-137">Ha hello a hely neve szóközt tartalmaz, tegye idézőjelek közé foglalt.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-137">If hello location name contains spaces, put it in quotes.</span></span> <span data-ttu-id="0d2fd-138">Például: „USA 2. keleti régiója”.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-138">For example "East US 2".</span></span> 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. <span data-ttu-id="0d2fd-139">Hello Data Lake Store-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-139">Create hello Data Lake Store account.</span></span>
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="0d2fd-140">Mappák létrehozása Data Lake Store-fiókban</span><span class="sxs-lookup"><span data-stu-id="0d2fd-140">Create folders in a Data Lake Store account</span></span>

<span data-ttu-id="0d2fd-141">Mappák létrehozása az Azure Data Lake Store-fiók toomanage alatt, és adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-141">You can create folders under your Azure Data Lake Store account toomanage and store data.</span></span> <span data-ttu-id="0d2fd-142">A következő parancs toocreate nevű egy mappát használja hello **mynewfolder** : hello hello Data Lake Store gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-142">Use hello following command toocreate a folder called **mynewfolder** at hello root of hello Data Lake Store.</span></span>

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> <span data-ttu-id="0d2fd-143">Hello `--folder` paraméter biztosítja, hogy hello parancs létrehoz egy mappát.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-143">hello `--folder` parameter ensures that hello command creates a folder.</span></span> <span data-ttu-id="0d2fd-144">Ha ez a paraméter nincs megadva, hello parancs létrehoz egy üres nevű mynewfolder: hello hello Data Lake Store-fiók gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-144">If this parameter is not present, hello command creates an empty file called mynewfolder at hello root of hello Data Lake Store account.</span></span>
> 
>

## <a name="upload-data-tooa-data-lake-store-account"></a><span data-ttu-id="0d2fd-145">Töltse fel az adatok tooa Data Lake Store-fiók</span><span class="sxs-lookup"><span data-stu-id="0d2fd-145">Upload data tooa Data Lake Store account</span></span>

<span data-ttu-id="0d2fd-146">Feltöltheti tooData Lake adattárban közvetlenül gyökérmappában hello szint vagy tooa hello fiókon belül létrehozott.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-146">You can upload data tooData Lake Store directly at hello root level or tooa folder that you created within hello account.</span></span> <span data-ttu-id="0d2fd-147">hello alábbi kódtöredékek bemutatják, hogyan tooupload néhány minta toohello Adatmappa (**mynewfolder**) hello előző szakaszban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-147">hello snippets below demonstrate how tooupload some sample data toohello folder (**mynewfolder**) you created in hello previous section.</span></span>

<span data-ttu-id="0d2fd-148">Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="0d2fd-148">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="0d2fd-149">Töltse le a hello fájlt, és a számítógépre, például C:\sampledata\ egy helyi könyvtárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-149">Download hello file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> <span data-ttu-id="0d2fd-150">Hello cél hello teljes elérési útját együtt hello fájlnevet kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-150">For hello destination, you must specify hello complete path including hello file name.</span></span>
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a><span data-ttu-id="0d2fd-151">A Data Lake Store-fiók fájljainak listázása</span><span class="sxs-lookup"><span data-stu-id="0d2fd-151">List files in a Data Lake Store account</span></span>

<span data-ttu-id="0d2fd-152">Használja a következő parancs toolist hello fájlok egy Data Lake Store-fiókban hello.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-152">Use hello following command toolist hello files in a Data Lake Store account.</span></span>

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

<span data-ttu-id="0d2fd-153">hello kimenet az hasonló toohello következő legyen:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-153">hello output of this should be similar toohello following:</span></span>

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

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a><span data-ttu-id="0d2fd-154">A Data Lake Store-fiókban lévő adatok átnevezése, letöltése és törlése</span><span class="sxs-lookup"><span data-stu-id="0d2fd-154">Rename, download, and delete data from a Data Lake Store account</span></span> 

* <span data-ttu-id="0d2fd-155">**a fájl toorename**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-155">**toorename a file**, use hello following command:</span></span>
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* <span data-ttu-id="0d2fd-156">**a fájl toodownload**, használja a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-156">**toodownload a file**, use hello following command.</span></span> <span data-ttu-id="0d2fd-157">Győződjön meg arról, hogy hello cél elérési út már létezik.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-157">Make sure hello destination path you specify already exists.</span></span>
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > <span data-ttu-id="0d2fd-158">hello parancs hello célmappa hoz létre, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-158">hello command creates hello destination folder if it does not exist.</span></span>
    > 
    >

* <span data-ttu-id="0d2fd-159">**a fájl toodelete**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-159">**toodelete a file**, use hello following command:</span></span>
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    <span data-ttu-id="0d2fd-160">Ha azt szeretné, hogy toodelete hello mappa **mynewfolder** és hello fájl **vehicle1_09142014_copy.csv** együtt egy parancsban, használjon hello – recurse paraméter</span><span class="sxs-lookup"><span data-stu-id="0d2fd-160">If you want toodelete hello folder **mynewfolder** and hello file **vehicle1_09142014_copy.csv** together in one command, use hello --recurse parameter</span></span>

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a><span data-ttu-id="0d2fd-161">A Data Lake Store-fiókhoz tartozó engedélyek és a hozzáférés-vezérlési listák használata</span><span class="sxs-lookup"><span data-stu-id="0d2fd-161">Work with permissions and ACLs for a Data Lake Store account</span></span>

<span data-ttu-id="0d2fd-162">Ebben a szakaszban, megtudhatja, hogyan toomanage hozzáférés-vezérlési listák és az engedélyek az Azure CLI 2.0 verziót használja.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-162">In this section you learn about how toomanage ACLs and permissions using Azure CLI 2.0.</span></span> <span data-ttu-id="0d2fd-163">A hozzáférés-vezérlési listák Azure Data Lake Store-beli használatának részletes leírásáért lásd: [Az Azure Data Lake Store szolgáltatásban található hozzáférés-vezérlés](data-lake-store-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="0d2fd-163">For a detailed discussion on how ACLs are implemented in Azure Data Lake Store, see [Access control in Azure Data Lake Store](data-lake-store-access-control.md).</span></span>

* <span data-ttu-id="0d2fd-164">**egy fájl vagy mappa tulajdonosának tooupdate hello**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-164">**tooupdate hello owner of a file/folder**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="0d2fd-165">**egy fájl vagy mappa engedélyeit tooupdate hello**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-165">**tooupdate hello permissions for a file/folder**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* <span data-ttu-id="0d2fd-166">**a megadott elérési út a hozzáférés-vezérlési listák tooget hello**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-166">**tooget hello ACLs for a given path**, use hello following command:</span></span>

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    <span data-ttu-id="0d2fd-167">hello kimeneti hasonló toohello következő legyen:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-167">hello output should be similar toohello following:</span></span>

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

* <span data-ttu-id="0d2fd-168">**egy bejegyzést a hozzáférés-vezérlési Listában tooset**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-168">**tooset an entry for an ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* <span data-ttu-id="0d2fd-169">**egy bejegyzést a hozzáférés-vezérlési Listában tooremove**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-169">**tooremove an entry for an ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="0d2fd-170">**egy teljes alapértelmezett hozzáférés-vezérlési lista tooremove**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-170">**tooremove an entire default ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* <span data-ttu-id="0d2fd-171">**egy teljes nem alapértelmezett ACL tooremove**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d2fd-171">**tooremove an entire non-default ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="0d2fd-172">Data Lake Store-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="0d2fd-172">Delete a Data Lake Store account</span></span>
<span data-ttu-id="0d2fd-173">A következő parancs toodelete Data Lake Store-fiók hello használata.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-173">Use hello following command toodelete a Data Lake Store account.</span></span>

```azurecli
az dls account delete --account mydatalakestore
```

<span data-ttu-id="0d2fd-174">Amikor a rendszer kéri, adja meg a **Y** toodelete hello fiók.</span><span class="sxs-lookup"><span data-stu-id="0d2fd-174">When prompted, enter **Y** toodelete hello account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d2fd-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d2fd-175">Next steps</span></span>

* [<span data-ttu-id="0d2fd-176">Az Azure Data Lake Store CLI 2.0 dokumentációja</span><span class="sxs-lookup"><span data-stu-id="0d2fd-176">Azure Data Lake Store CLI 2.0 reference</span></span>](https://docs.microsoft.com/cli/azure/dls)
* [<span data-ttu-id="0d2fd-177">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="0d2fd-177">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="0d2fd-178">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="0d2fd-178">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="0d2fd-179">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="0d2fd-179">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md
