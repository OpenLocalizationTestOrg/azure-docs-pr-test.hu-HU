---
title: "Az Azure Data Lake Store használatának első lépései PowerShell használatával | Microsoft Docs"
description: "Data Lake Store-fiók létrehozása és alapszintű műveletek végrehajtása az Azure PowerShell használatával"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 23c9aaa089251bff5132652475f4daadc2c128fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a><span data-ttu-id="420f4-103">Az Azure Data Lake Store használatának első lépései az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="420f4-103">Get started with Azure Data Lake Store using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="420f4-104">Portál</span><span class="sxs-lookup"><span data-stu-id="420f4-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="420f4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="420f4-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="420f4-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="420f4-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="420f4-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="420f4-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="420f4-108">REST API</span><span class="sxs-lookup"><span data-stu-id="420f4-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="420f4-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="420f4-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="420f4-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="420f4-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="420f4-111">Python</span><span class="sxs-lookup"><span data-stu-id="420f4-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="420f4-112">Ismerje meg, hogyan hozhat létre Azure Data Lake Store-fiókot az Azure PowerShell használatával, illetve hogyan végezhet el olyan alapvető műveleteket, mint például a mappák létrehozása, adatfájlok le- és feltöltése, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).</span><span class="sxs-lookup"><span data-stu-id="420f4-112">Learn how to use Azure PowerShell to create an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="420f4-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="420f4-113">Prerequisites</span></span>
<span data-ttu-id="420f4-114">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="420f4-114">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="420f4-115">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="420f4-115">**An Azure subscription**.</span></span> <span data-ttu-id="420f4-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="420f4-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="420f4-117">Az **Azure PowerShell 1.0-s vagy újabb verziója**.</span><span class="sxs-lookup"><span data-stu-id="420f4-117">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="420f4-118">Lásd: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="420f4-118">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="authentication"></a><span data-ttu-id="420f4-119">Authentication</span><span class="sxs-lookup"><span data-stu-id="420f4-119">Authentication</span></span>
<span data-ttu-id="420f4-120">Ez a cikk egy egyszerűbb, a Data Lake Store-ral történő hitelesítési módszert használ, ahol meg kell adnia az Azure-fiókjához tartozó hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="420f4-120">This article uses a simpler authentication approach with Data Lake Store where you are prompted to enter your Azure account credentials.</span></span> <span data-ttu-id="420f4-121">Ezután a Data Lake Store-fiókhoz és a fájlrendszerhez való hozzáférés szintje a bejelentkezett felhasználó hozzáférési szintjétől függ.</span><span class="sxs-lookup"><span data-stu-id="420f4-121">The access level to Data Lake Store account and file system is then governed by the access level of the logged in user.</span></span> <span data-ttu-id="420f4-122">Azonban a Data Lake Store-ral más módokon is lehet hitelesíteni. Ezek a következők: **végfelhasználói hitelesítés** vagy **szolgáltatások közötti hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="420f4-122">However, there are other approaches as well to authenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="420f4-123">Útmutatás a hitelesítéshez és további tudnivalók a [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md) vagy a [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md) című témakörben.</span><span class="sxs-lookup"><span data-stu-id="420f4-123">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="420f4-124">Azure Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="420f4-124">Create an Azure Data Lake Store account</span></span>
1. <span data-ttu-id="420f4-125">Nyisson meg egy új Windows PowerShell ablakot, és adja meg az alábbi kódrészletet az Azure-fiókba való bejelentkezéshez, az előfizetés beállításához és a Data Lake Store-szolgáltató regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="420f4-125">From your desktop, open a new Windows PowerShell window, and enter the following snippet to log in to your Azure account, set the subscription, and register the Data Lake Store provider.</span></span> <span data-ttu-id="420f4-126">Győződjön meg arról, hogy az előfizetés rendszergazdájaként/tulajdonosaként jelentkezik be, amikor a rendszer erre kéri:</span><span class="sxs-lookup"><span data-stu-id="420f4-126">When prompted to log in, make sure you log in as one of the subscription admininistrators/owner:</span></span>

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. <span data-ttu-id="420f4-127">Az Azure Data Lake Store-fiókok egy Azure-erőforráscsoporthoz vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="420f4-127">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="420f4-128">Először hozzon létre egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="420f4-128">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="420f4-129">![Azure-erőforráscsoport létrehozása](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure-erőforráscsoport létrehozása")</span><span class="sxs-lookup"><span data-stu-id="420f4-129">![Create an Azure Resource Group](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")</span></span>
3. <span data-ttu-id="420f4-130">Hozzon létre egy Azure Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="420f4-130">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="420f4-131">A megadott név csak kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="420f4-131">The name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="420f4-132">![Azure Data Lake Store-fiók létrehozása](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Azure Data Lake Store-fiók létrehozása")</span><span class="sxs-lookup"><span data-stu-id="420f4-132">![Create an Azure Data Lake Store account](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")</span></span>
4. <span data-ttu-id="420f4-133">Ellenőrizze, hogy a fiók létrehozása sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="420f4-133">Verify that the account is successfully created.</span></span>

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    <span data-ttu-id="420f4-134">A kimenet értéke **True** (Igaz) kell, hogy legyen.</span><span class="sxs-lookup"><span data-stu-id="420f4-134">The output for this should be **True**.</span></span>

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a><span data-ttu-id="420f4-135">Könyvtárstruktúrák létrehozása az Azure Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="420f4-135">Create directory structures in your Azure Data Lake Store</span></span>
<span data-ttu-id="420f4-136">Az Azure Data Lake Store-fiókjában könyvtárakat hozhat létre az adatok kezelésére és tárolására.</span><span class="sxs-lookup"><span data-stu-id="420f4-136">You can create directories under your Azure Data Lake Store account to manage and store data.</span></span>

1. <span data-ttu-id="420f4-137">Adjon meg egy gyökérkönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="420f4-137">Specify a root directory.</span></span>

        $myrootdir = "/"
2. <span data-ttu-id="420f4-138">Hozzon létre egy új könyvtárat **mynewdirectory** néven a megadott gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="420f4-138">Create a new directory called **mynewdirectory** under the specified root.</span></span>

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. <span data-ttu-id="420f4-139">Ellenőrizze, hogy az új könyvtár létrehozása sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="420f4-139">Verify that the new directory is successfully created.</span></span>

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    <span data-ttu-id="420f4-140">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="420f4-140">It should show an output like the following:</span></span>

    <span data-ttu-id="420f4-141">![A könyvtár ellenőrzése](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "A könyvtár ellenőrzése")</span><span class="sxs-lookup"><span data-stu-id="420f4-141">![Verify Directory](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")</span></span>

## <a name="upload-data-to-your-azure-data-lake-store"></a><span data-ttu-id="420f4-142">Fájlok feltöltése az Azure Data Lake Store-ba</span><span class="sxs-lookup"><span data-stu-id="420f4-142">Upload data to your Azure Data Lake Store</span></span>
<span data-ttu-id="420f4-143">Az adatait feltöltheti közvetlenül a Data Lake Store-ba gyökérszinten, vagy a fiókon belül létrehozott könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="420f4-143">You can upload your data to Data Lake Store directly at the root level or to a directory that you created within the account.</span></span> <span data-ttu-id="420f4-144">Az alábbi részletek bemutatják, hogyan tölthet fel néhány adatot az előző szakaszban létrehozott könyvtárba (**mynewdirectory**).</span><span class="sxs-lookup"><span data-stu-id="420f4-144">The snippets below demonstrate how to upload some sample data to the directory (**mynewdirectory**) you created in the previous section.</span></span>

<span data-ttu-id="420f4-145">Ha feltölthető mintaadatokra van szüksége, használhatja az [Azure Data Lake Git-tárában](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) található **Ambulance Data** mappát.</span><span class="sxs-lookup"><span data-stu-id="420f4-145">If you are looking for some sample data to upload, you can get the **Ambulance Data** folder from the [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="420f4-146">Töltse le a fájlt, és tárolja a számítógépén egy helyi könyvtárban (pl. C:\sampledata).</span><span class="sxs-lookup"><span data-stu-id="420f4-146">Download the file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a><span data-ttu-id="420f4-147">A Data Lake Store-ban lévő adatok átnevezése, letöltése és törlése</span><span class="sxs-lookup"><span data-stu-id="420f4-147">Rename, download, and delete data from your Data Lake Store</span></span>
<span data-ttu-id="420f4-148">Fájlok átnevezéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="420f4-148">To rename a file, use the following command:</span></span>

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="420f4-149">Fájlok letöltéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="420f4-149">To download a file, use the following command:</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

<span data-ttu-id="420f4-150">Fájlok törléséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="420f4-150">To delete a file, use the following command:</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="420f4-151">Ha a rendszer rákérdez, írja be az **Y** karaktert az elem törléséhez.</span><span class="sxs-lookup"><span data-stu-id="420f4-151">When prompted, enter **Y** to delete the item.</span></span> <span data-ttu-id="420f4-152">Ha több fájlt kíván törölni, megadhatja az összes elérési utat, vesszővel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="420f4-152">If you have more than one file to delete, you can provide all the paths separated by comma.</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a><span data-ttu-id="420f4-153">Az Azure Data Lake Store-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="420f4-153">Delete your Azure Data Lake Store account</span></span>
<span data-ttu-id="420f4-154">Az alábbi parancs segítségével törölheti a Data Lake Store-fiókját.</span><span class="sxs-lookup"><span data-stu-id="420f4-154">Use the following command to delete your Data Lake Store account.</span></span>

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

<span data-ttu-id="420f4-155">Ha a rendszer rákérdez, írja be az **Y** karaktert a fiók törléséhez.</span><span class="sxs-lookup"><span data-stu-id="420f4-155">When prompted, enter **Y** to delete the account.</span></span>

## <a name="performance-guidance-while-using-powershell"></a><span data-ttu-id="420f4-156">Teljesítménnyel kapcsolatos útmutató a PowerShell használata során</span><span class="sxs-lookup"><span data-stu-id="420f4-156">Performance guidance while using PowerShell</span></span>

<span data-ttu-id="420f4-157">Alább azon legfontosabb beállítások láthatók, amelyek megfelelő hangolásával a legjobb teljesítmény érhető el, miközben a PowerShellt használja a Data Lake Store-ral való munkavégzés során:</span><span class="sxs-lookup"><span data-stu-id="420f4-157">Below are the most important settings that can be tuned to get the best performance while using PowerShell to work with Data Lake Store:</span></span>

| <span data-ttu-id="420f4-158">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="420f4-158">Property</span></span>            | <span data-ttu-id="420f4-159">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="420f4-159">Default</span></span> | <span data-ttu-id="420f4-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="420f4-160">Description</span></span> |
|---------------------|---------|-------------|
| <span data-ttu-id="420f4-161">PerFileThreadCount</span><span class="sxs-lookup"><span data-stu-id="420f4-161">PerFileThreadCount</span></span>  | <span data-ttu-id="420f4-162">10</span><span class="sxs-lookup"><span data-stu-id="420f4-162">10</span></span>      | <span data-ttu-id="420f4-163">Ez a paraméter lehetővé teszi a párhuzamos szálak számának megadását az egyes fájlok fel- vagy letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="420f4-163">This parameter enables you to choose the number of parallel threads for uploading or downloading each file.</span></span> <span data-ttu-id="420f4-164">Ez a szám a fájlonként maximálisan lefoglalható szálakat jelenti, azonban a forgatókönyvtől függően előfordulhat, hogy kevesebb szálat kap (ha például, egy 1 KB-os fájlt tölt fel, akkor is csak egy szálat kap, ha 20-at kér).</span><span class="sxs-lookup"><span data-stu-id="420f4-164">This number represents the max threads that can be allocated per file, but you may get less threads depending on your scenario (e.g. if you are uploading a 1KB file, you will get one thread even if you ask for 20 threads).</span></span>  |
| <span data-ttu-id="420f4-165">ConcurrentFileCount</span><span class="sxs-lookup"><span data-stu-id="420f4-165">ConcurrentFileCount</span></span> | <span data-ttu-id="420f4-166">10</span><span class="sxs-lookup"><span data-stu-id="420f4-166">10</span></span>      | <span data-ttu-id="420f4-167">Ez a paraméter kifejezetten a mappák fel- és letöltéséhez kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="420f4-167">This parameter is specifically for uploading or downloading folders.</span></span> <span data-ttu-id="420f4-168">Ez a paraméter határozza meg az egyidejűleg fel- vagy letölthető fájlok számát.</span><span class="sxs-lookup"><span data-stu-id="420f4-168">This parameter determines the number of concurrent files that can be uploaded or downloaded.</span></span> <span data-ttu-id="420f4-169">Ez a szám az adott időpontban egyidejűleg fel- vagy letölthető fájlok maximális számát jelenti, azonban a forgatókönyvtől függően előfordulhat, hogy kisebb egyidejűséget kap (ha például két fájlt tölt fel, akkor is csak két egyidejű fájlfeltöltést kap, ha 15-öt kér).</span><span class="sxs-lookup"><span data-stu-id="420f4-169">This number represents the maximum number of concurrent files that can be uploaded or downloaded at one time, but you may get less concurrency depending on your scenario (e.g. if you are uploading two files, you will get two concurrent file uploads even if you ask for 15).</span></span> |

<span data-ttu-id="420f4-170">**Példa**</span><span class="sxs-lookup"><span data-stu-id="420f4-170">**Example**</span></span>

<span data-ttu-id="420f4-171">Ezzel a paranccsal fájlokat tölthet le az Azure Data Lake Store-ból a felhasználó helyi lemezére fájlonként 20 szálat és 100 egyidejű fájlt használva.</span><span class="sxs-lookup"><span data-stu-id="420f4-171">This command downloads files from Azure Data Lake Store to the user's local drive using 20 threads per file and 100 concurrent files.</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-the-value-to-set-for-these-parameters"></a><span data-ttu-id="420f4-172">Hogyan határozhatom meg a paraméterek számára beállítandó értéket?</span><span class="sxs-lookup"><span data-stu-id="420f4-172">How do I determine the value to set for these parameters?</span></span>

<span data-ttu-id="420f4-173">Az alábbiakban olvashat némi útmutatást ezzel kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="420f4-173">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="420f4-174">**1. lépés: A teljes szálszám meghatározása** – Kezdje a használni kívánt szálak teljes számának kiszámításával.</span><span class="sxs-lookup"><span data-stu-id="420f4-174">**Step 1: Determine the total thread count** - You should start by calculating the total thread count to use.</span></span> <span data-ttu-id="420f4-175">Általánosságban 6 szálat használjon minden egyes fizikai maghoz.</span><span class="sxs-lookup"><span data-stu-id="420f4-175">As a general guideline, you should use 6 threads for each physical core.</span></span>

        Total thread count = total physical cores * 6

    <span data-ttu-id="420f4-176">**Példa**</span><span class="sxs-lookup"><span data-stu-id="420f4-176">**Example**</span></span>

    <span data-ttu-id="420f4-177">Tételezzük fel, hogy egy 16 maggal rendelkező D14 VM-en futtatja a PowerShell-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="420f4-177">Assuming you are running the PowerShell commands from a D14 VM that has 16 cores</span></span>

        Total thread count = 16 cores * 6 = 96 threads


* <span data-ttu-id="420f4-178">**2. lépés: A PerFileThreadCount kiszámítása** – A PerFileThreadCount számítását fájlok mérete alapján végezzük.</span><span class="sxs-lookup"><span data-stu-id="420f4-178">**Step 2: Calculate PerFileThreadCount**  - We calculate our PerFileThreadCount based on the size of the files.</span></span> <span data-ttu-id="420f4-179">A 2,5 GB-nál kisebb fájlok esetén ezt a paramétert nem kell módosítani, mert az alapértelmezett érték (10) elegendő.</span><span class="sxs-lookup"><span data-stu-id="420f4-179">For files smaller than 2.5GB, there is no need to change this parameter because the default of 10 is sufficient.</span></span> <span data-ttu-id="420f4-180">A 2,5 GB-nál nagyobb fájlok esetén az első 2,5 GB-nál 10 szálat kell használnia alapként, és a fájlméret minden további 256 MB-os növekedése után 1 szálat kell hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="420f4-180">For files larger than 2.5GB, you should use 10 threads as the base for the first 2.5GB and add 1 thread for each additional 256MB increase in file size.</span></span> <span data-ttu-id="420f4-181">Ha olyan mappát másol, amelyben nagyon eltérő méretű fájlok találhatók, érdemes hasonló fájlméret alapján csoportosítani a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="420f4-181">If you are copying a folder with a large range of file sizes, consider grouping them into similar file sizes.</span></span> <span data-ttu-id="420f4-182">Az eltérő fájlméretek az optimálisnál kisebb teljesítménnyel járhatnak.</span><span class="sxs-lookup"><span data-stu-id="420f4-182">Having dissimilar file sizes may cause non-optimal performance.</span></span> <span data-ttu-id="420f4-183">Ha nem lehetséges a hasonló fájlméret alapján történő csoportosítás, akkor a PerFileThreadCount értékét a legnagyobb fájlméret alapján kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="420f4-183">If that's not possible to group similar file sizes, you should set PerFileThreadCount based on the largest file size.</span></span>

        PerFileThreadCount = 10 threads for the first 2.5GB + 1 thread for each additional 256MB increase in file size

    <span data-ttu-id="420f4-184">**Példa**</span><span class="sxs-lookup"><span data-stu-id="420f4-184">**Example**</span></span>

    <span data-ttu-id="420f4-185">Ha feltételezzük, hogy 100 fájllal rendelkezik, amelyek mérete 1 és 10 GB között változik, akkor a 10 GB-os fájlt, mint legnagyobb méretű fájlt használjuk a számításban, ami az alábbiakban látható.</span><span class="sxs-lookup"><span data-stu-id="420f4-185">Assuming you have 100 files ranging from 1GB to 10GB, we use the 10GB as the largest file size for equation, which would read like the following.</span></span>

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* <span data-ttu-id="420f4-186">**3. lépés: A ConcurrentFilecount kiszámítása** – Használja a teljes szálszámot és a PerFileThreadCount értékét a ConcurrentFileCount kiszámításához az alábbi egyenlet alapján.</span><span class="sxs-lookup"><span data-stu-id="420f4-186">**Step 3: Calculate ConcurrentFilecount** - Use the total thread count and PerFileThreadCount to calculate ConcurrentFileCount based on the following equation.</span></span>

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    <span data-ttu-id="420f4-187">**Példa**</span><span class="sxs-lookup"><span data-stu-id="420f4-187">**Example**</span></span>

    <span data-ttu-id="420f4-188">Az eddig használt példaértékek alapján</span><span class="sxs-lookup"><span data-stu-id="420f4-188">Based on the example values we have been using</span></span>

        96 = 40 * ConcurrentFileCount

    <span data-ttu-id="420f4-189">Tehát a **ConcurrentFileCount** értéke **2,4**, amelyet kerekíthetünk **2**-re.</span><span class="sxs-lookup"><span data-stu-id="420f4-189">So, **ConcurrentFileCount** is **2.4**, which we can round off to **2**.</span></span>

### <a name="further-tuning"></a><span data-ttu-id="420f4-190">További hangolás</span><span class="sxs-lookup"><span data-stu-id="420f4-190">Further tuning</span></span>

<span data-ttu-id="420f4-191">Elképzelhető, hogy további hangolásra lesz szüksége, mert különböző fájlméretekkel kell dolgoznia.</span><span class="sxs-lookup"><span data-stu-id="420f4-191">You might require further tuning because there is a range of file sizes to work with.</span></span> <span data-ttu-id="420f4-192">A fenti számítás jól működik, ha az összes, vagy legalábbis a legtöbb fájl nagyobb méretű, és közelebb van a 10 GB-os mérethez.</span><span class="sxs-lookup"><span data-stu-id="420f4-192">The above calculation works well if all or most of the files are larger and closer to the 10GB range.</span></span> <span data-ttu-id="420f4-193">Ha ehelyett sok különböző, nagyon eltérő méretű fájllal rendelkezik, akkor csökkentheti a PerFileThreadCount értékét.</span><span class="sxs-lookup"><span data-stu-id="420f4-193">If instead, there are many different files sizes with many files being smaller, then you could reduce PerFileThreadCount.</span></span> <span data-ttu-id="420f4-194">A PerFileThreadCount értékének csökkentésével, növelhetjük a ConcurrentFileCount értékét.</span><span class="sxs-lookup"><span data-stu-id="420f4-194">By reducing the PerFileThreadCount, we can increase ConcurrentFileCount.</span></span> <span data-ttu-id="420f4-195">Ha tehát feltételezzük, hogy a legtöbb fájlunk kisebb, az 5 GB-os tartományba eső mérettel rendelkezik, akkor újra végrehajthatjuk a számítást:</span><span class="sxs-lookup"><span data-stu-id="420f4-195">So, if we assume that most of our files are smaller in the 5GB range, we can re-do our calculation:</span></span>

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

<span data-ttu-id="420f4-196">A **ConcurrentFileCount** értéke így 96/20, ami 4,8-del egyenlő, kerekítve pedig **4** lesz.</span><span class="sxs-lookup"><span data-stu-id="420f4-196">So, **ConcurrentFileCount** will now be 96/20, which is 4.8, rounded off to **4**.</span></span>

<span data-ttu-id="420f4-197">A beállítások hangolását a **PerFileThreadCount** értékének növelésével vagy csökkentésével folytathatja a fájlméretek eloszlásától függően.</span><span class="sxs-lookup"><span data-stu-id="420f4-197">You can continue to tune these settings by changing the **PerFileThreadCount** up and down depending on the distribution of your file sizes.</span></span>

### <a name="limitation"></a><span data-ttu-id="420f4-198">Korlátozás</span><span class="sxs-lookup"><span data-stu-id="420f4-198">Limitation</span></span>

* <span data-ttu-id="420f4-199">**A fájlok száma kisebb, mint a ConcurrentFileCount értéke**: Ha a feltölteni kívánt fájlok száma kisebb a **ConcurrentFileCount** kiszámított értékének, akkor csökkentse a **ConcurrentFileCount** értékét úgy, hogy az egyenlő legyen a fájlok számával.</span><span class="sxs-lookup"><span data-stu-id="420f4-199">**Number of files is less than ConcurrentFileCount**: If the number of files you are uploading is smaller than the **ConcurrentFileCount** that you calculated, then you should reduce **ConcurrentFileCount** to be equal to the number of files.</span></span> <span data-ttu-id="420f4-200">A fennmaradó szálakat a **PerFileThreadCount** értékének a növelésére használhatja.</span><span class="sxs-lookup"><span data-stu-id="420f4-200">You can use any remaining threads to increase **PerFileThreadCount**.</span></span>

* <span data-ttu-id="420f4-201">**Túl sok szál**: Ha túlságosan megnöveli a szálszámot a fürt méretének növelése nélkül, az rosszabb teljesítménnyel járhat.</span><span class="sxs-lookup"><span data-stu-id="420f4-201">**Too many threads**: If you increase thread count too much without increasing your cluster size, you run the risk of degraded performance.</span></span> <span data-ttu-id="420f4-202">Versengési problémák adódhatnak, ha kontextusváltás történik a CPU-n.</span><span class="sxs-lookup"><span data-stu-id="420f4-202">There can be contention issues when context-switching on the CPU.</span></span>

* <span data-ttu-id="420f4-203">**Elégtelen egyidejűség**: Ha az egyidejűség nem elégséges, lehet, hogy túl kicsi a fürt.</span><span class="sxs-lookup"><span data-stu-id="420f4-203">**Insufficient concurrency**: If the concurrency is not sufficient, then your cluster may be too small.</span></span> <span data-ttu-id="420f4-204">A fürtben növelheti a csomópontok számát, ami több egyidejűséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="420f4-204">You can increase the number of nodes in your cluster which will give you more concurrency.</span></span>

* <span data-ttu-id="420f4-205">**Szabályozási hibák**: Elképzelhető, hogy szabályozási hibákat tapasztal, ha az egyidejűség túl magas.</span><span class="sxs-lookup"><span data-stu-id="420f4-205">**Throttling errors**: You may see throttling errors if your concurrency is too high.</span></span> <span data-ttu-id="420f4-206">Ha szabályozás hibák merülnek fel, csökkentse az egyidejűséget vagy lépjen kapcsolatba velünk.</span><span class="sxs-lookup"><span data-stu-id="420f4-206">If you are seeing throttling errors, you should either reduce the concurrency or contact us.</span></span>

## <a name="next-steps"></a><span data-ttu-id="420f4-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="420f4-207">Next steps</span></span>
* [<span data-ttu-id="420f4-208">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="420f4-208">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="420f4-209">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="420f4-209">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="420f4-210">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="420f4-210">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

