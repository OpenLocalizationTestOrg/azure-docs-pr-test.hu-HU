---
title: "aaaUse PowerShell tooget Azure Data Lake Store használatába |} Microsoft Docs"
description: "Használja az Azure PowerShell toocreate Data Lake Store-fiók és alapszintű műveletek végrehajtása"
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
ms.openlocfilehash: 9c958bfd63e412ec0b0a4113a149f61aee026bc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a><span data-ttu-id="98694-103">Az Azure Data Lake Store használatának első lépései az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="98694-103">Get started with Azure Data Lake Store using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="98694-104">Portál</span><span class="sxs-lookup"><span data-stu-id="98694-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="98694-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="98694-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="98694-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="98694-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="98694-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="98694-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="98694-108">REST API</span><span class="sxs-lookup"><span data-stu-id="98694-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="98694-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="98694-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="98694-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="98694-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="98694-111">Python</span><span class="sxs-lookup"><span data-stu-id="98694-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="98694-112">Ismerje meg, hogyan toouse Azure PowerShell toocreate egy Azure Data Lake tárolásához fiók és alapvető műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).</span><span class="sxs-lookup"><span data-stu-id="98694-112">Learn how toouse Azure PowerShell toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98694-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="98694-113">Prerequisites</span></span>
<span data-ttu-id="98694-114">Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="98694-114">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="98694-115">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="98694-115">**An Azure subscription**.</span></span> <span data-ttu-id="98694-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="98694-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="98694-117">Az **Azure PowerShell 1.0-s vagy újabb verziója**.</span><span class="sxs-lookup"><span data-stu-id="98694-117">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="98694-118">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="98694-118">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="authentication"></a><span data-ttu-id="98694-119">Authentication</span><span class="sxs-lookup"><span data-stu-id="98694-119">Authentication</span></span>
<span data-ttu-id="98694-120">Ez a cikk egy egyszerűbb hitelesítési módszert alkalmaz a Data Lake Store felszólító tooenter az Azure-fiók hitelesítő adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="98694-120">This article uses a simpler authentication approach with Data Lake Store where you are prompted tooenter your Azure account credentials.</span></span> <span data-ttu-id="98694-121">hello hozzáférési szint tooData Lake Store fiók és a fájl rendszer majd hello hozzáférési szint a bejelentkezett felhasználó hello szabályozza.</span><span class="sxs-lookup"><span data-stu-id="98694-121">hello access level tooData Lake Store account and file system is then governed by hello access level of hello logged in user.</span></span> <span data-ttu-id="98694-122">Van azonban más megoldások jól tooauthenticate a Data Lake Store, mint amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="98694-122">However, there are other approaches as well tooauthenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="98694-123">További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="98694-123">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="98694-124">Azure Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="98694-124">Create an Azure Data Lake Store account</span></span>
1. <span data-ttu-id="98694-125">Az asztalon nyisson meg egy új Windows PowerShell ablakot, és adja meg a következő kódrészletet toolog az Azure-fiók tooyour hello hello előfizetés beállítása és hello Data Lake Store-szolgáltató regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="98694-125">From your desktop, open a new Windows PowerShell window, and enter hello following snippet toolog in tooyour Azure account, set hello subscription, and register hello Data Lake Store provider.</span></span> <span data-ttu-id="98694-126">Amikor felszólító toolog, győződjön meg arról, hogy jelentkezik be a rendelkezésre álló hello előfizetés rendszergazdájaként/tulajdonosaként:</span><span class="sxs-lookup"><span data-stu-id="98694-126">When prompted toolog in, make sure you log in as one of hello subscription admininistrators/owner:</span></span>

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. <span data-ttu-id="98694-127">Az Azure Data Lake Store-fiókok egy Azure-erőforráscsoporthoz vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="98694-127">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="98694-128">Először hozzon létre egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="98694-128">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="98694-129">![Azure-erőforráscsoport létrehozása](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure-erőforráscsoport létrehozása")</span><span class="sxs-lookup"><span data-stu-id="98694-129">![Create an Azure Resource Group](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")</span></span>
3. <span data-ttu-id="98694-130">Hozzon létre egy Azure Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="98694-130">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="98694-131">megadott hello neve csak kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="98694-131">hello name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="98694-132">![Azure Data Lake Store-fiók létrehozása](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Azure Data Lake Store-fiók létrehozása")</span><span class="sxs-lookup"><span data-stu-id="98694-132">![Create an Azure Data Lake Store account](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")</span></span>
4. <span data-ttu-id="98694-133">Győződjön meg arról, hogy hello fiók sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="98694-133">Verify that hello account is successfully created.</span></span>

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    <span data-ttu-id="98694-134">a Ez lehet kimeneti hello **igaz**.</span><span class="sxs-lookup"><span data-stu-id="98694-134">hello output for this should be **True**.</span></span>

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a><span data-ttu-id="98694-135">Könyvtárstruktúrák létrehozása az Azure Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="98694-135">Create directory structures in your Azure Data Lake Store</span></span>
<span data-ttu-id="98694-136">Könyvtárak létrehozása az Azure Data Lake Store-fiók toomanage alatt, és adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="98694-136">You can create directories under your Azure Data Lake Store account toomanage and store data.</span></span>

1. <span data-ttu-id="98694-137">Adjon meg egy gyökérkönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="98694-137">Specify a root directory.</span></span>

        $myrootdir = "/"
2. <span data-ttu-id="98694-138">Hozzon létre egy új könyvtárat **mynewdirectory** hello megadott legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="98694-138">Create a new directory called **mynewdirectory** under hello specified root.</span></span>

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. <span data-ttu-id="98694-139">Ellenőrizze, hogy új hello könyvtárba sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="98694-139">Verify that hello new directory is successfully created.</span></span>

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    <span data-ttu-id="98694-140">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="98694-140">It should show an output like hello following:</span></span>

    <span data-ttu-id="98694-141">![A könyvtár ellenőrzése](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "A könyvtár ellenőrzése")</span><span class="sxs-lookup"><span data-stu-id="98694-141">![Verify Directory](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")</span></span>

## <a name="upload-data-tooyour-azure-data-lake-store"></a><span data-ttu-id="98694-142">Adatok tooyour Azure Data Lake Store feltöltése</span><span class="sxs-lookup"><span data-stu-id="98694-142">Upload data tooyour Azure Data Lake Store</span></span>
<span data-ttu-id="98694-143">Az adatok tooData Lake Store közvetlenül gyökérkönyvtárában hello szint vagy tooa hello fiókon belül létrehozott feltölthet.</span><span class="sxs-lookup"><span data-stu-id="98694-143">You can upload your data tooData Lake Store directly at hello root level or tooa directory that you created within hello account.</span></span> <span data-ttu-id="98694-144">hello alábbi kódtöredékek bemutatják, hogyan tooupload néhány minta toohello adatkönyvtára (**mynewdirectory**) hello előző szakaszban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="98694-144">hello snippets below demonstrate how tooupload some sample data toohello directory (**mynewdirectory**) you created in hello previous section.</span></span>

<span data-ttu-id="98694-145">Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="98694-145">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="98694-146">Töltse le a hello fájlt, és a számítógépre, például C:\sampledata\ egy helyi könyvtárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="98694-146">Download hello file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a><span data-ttu-id="98694-147">A Data Lake Store-ban lévő adatok átnevezése, letöltése és törlése</span><span class="sxs-lookup"><span data-stu-id="98694-147">Rename, download, and delete data from your Data Lake Store</span></span>
<span data-ttu-id="98694-148">toorename egy fájl a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="98694-148">toorename a file, use hello following command:</span></span>

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="98694-149">toodownload egy fájl a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="98694-149">toodownload a file, use hello following command:</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

<span data-ttu-id="98694-150">toodelete egy fájl a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="98694-150">toodelete a file, use hello following command:</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="98694-151">Amikor a rendszer kéri, adja meg a **Y** toodelete hello elemet.</span><span class="sxs-lookup"><span data-stu-id="98694-151">When prompted, enter **Y** toodelete hello item.</span></span> <span data-ttu-id="98694-152">Ha több fájl toodelete, megadhatja az összes hello elérési utat, vesszővel elválasztva.</span><span class="sxs-lookup"><span data-stu-id="98694-152">If you have more than one file toodelete, you can provide all hello paths separated by comma.</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a><span data-ttu-id="98694-153">Az Azure Data Lake Store-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="98694-153">Delete your Azure Data Lake Store account</span></span>
<span data-ttu-id="98694-154">A következő parancs toodelete hello a Data Lake Store-fiókot használni.</span><span class="sxs-lookup"><span data-stu-id="98694-154">Use hello following command toodelete your Data Lake Store account.</span></span>

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

<span data-ttu-id="98694-155">Amikor a rendszer kéri, adja meg a **Y** toodelete hello fiók.</span><span class="sxs-lookup"><span data-stu-id="98694-155">When prompted, enter **Y** toodelete hello account.</span></span>

## <a name="performance-guidance-while-using-powershell"></a><span data-ttu-id="98694-156">Teljesítménnyel kapcsolatos útmutató a PowerShell használata során</span><span class="sxs-lookup"><span data-stu-id="98694-156">Performance guidance while using PowerShell</span></span>

<span data-ttu-id="98694-157">Az alábbiakban hello legfontosabb beállítások, amelyek lehetnek bennünket tooget hello lehető legjobb teljesítményt PowerShell toowork a Data Lake Store használata során:</span><span class="sxs-lookup"><span data-stu-id="98694-157">Below are hello most important settings that can be tuned tooget hello best performance while using PowerShell toowork with Data Lake Store:</span></span>

| <span data-ttu-id="98694-158">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="98694-158">Property</span></span>            | <span data-ttu-id="98694-159">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="98694-159">Default</span></span> | <span data-ttu-id="98694-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="98694-160">Description</span></span> |
|---------------------|---------|-------------|
| <span data-ttu-id="98694-161">PerFileThreadCount</span><span class="sxs-lookup"><span data-stu-id="98694-161">PerFileThreadCount</span></span>  | <span data-ttu-id="98694-162">10</span><span class="sxs-lookup"><span data-stu-id="98694-162">10</span></span>      | <span data-ttu-id="98694-163">Ez a paraméter lehetővé teszi toochoose hello száma párhuzamos szálainak vagy minden fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="98694-163">This parameter enables you toochoose hello number of parallel threads for uploading or downloading each file.</span></span> <span data-ttu-id="98694-164">Ez a szám hello maximális szálak / fájl felosztható jelöli, de kevesebb szálak kaphat a forgatókönyvtől függően (pl. egy 1 KB fájlt feltölteni, ha elérhetővé válik egy szálat még akkor is, ha 20 szálak fel).</span><span class="sxs-lookup"><span data-stu-id="98694-164">This number represents hello max threads that can be allocated per file, but you may get less threads depending on your scenario (e.g. if you are uploading a 1KB file, you will get one thread even if you ask for 20 threads).</span></span>  |
| <span data-ttu-id="98694-165">ConcurrentFileCount</span><span class="sxs-lookup"><span data-stu-id="98694-165">ConcurrentFileCount</span></span> | <span data-ttu-id="98694-166">10</span><span class="sxs-lookup"><span data-stu-id="98694-166">10</span></span>      | <span data-ttu-id="98694-167">Ez a paraméter kifejezetten a mappák fel- és letöltéséhez kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="98694-167">This parameter is specifically for uploading or downloading folders.</span></span> <span data-ttu-id="98694-168">Ez a paraméter meghatározza, hogy hello száma párhuzamos feltöltött, illetve letöltött fájlokat.</span><span class="sxs-lookup"><span data-stu-id="98694-168">This parameter determines hello number of concurrent files that can be uploaded or downloaded.</span></span> <span data-ttu-id="98694-169">Ez a szám hello feltöltött, illetve egy időben letöltött egyidejű fájlok maximális számát jelöli, de kevesebb párhuzamossági kaphat a forgatókönyvtől függően (pl. két fájlt feltölteni, ha két egyidejű fájlfeltöltéseket kapja, még akkor is, ha meg kell kérni. 15).</span><span class="sxs-lookup"><span data-stu-id="98694-169">This number represents hello maximum number of concurrent files that can be uploaded or downloaded at one time, but you may get less concurrency depending on your scenario (e.g. if you are uploading two files, you will get two concurrent file uploads even if you ask for 15).</span></span> |

<span data-ttu-id="98694-170">**Példa**</span><span class="sxs-lookup"><span data-stu-id="98694-170">**Example**</span></span>

<span data-ttu-id="98694-171">Ez a parancs letölti a fájlokat az Azure Data Lake Store toohello felhasználói helyi meghajtóról 20 szálak számát, és 100 egyidejű fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="98694-171">This command downloads files from Azure Data Lake Store toohello user's local drive using 20 threads per file and 100 concurrent files.</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-hello-value-tooset-for-these-parameters"></a><span data-ttu-id="98694-172">Hogyan állapítható meg e paraméterek hello érték tooset?</span><span class="sxs-lookup"><span data-stu-id="98694-172">How do I determine hello value tooset for these parameters?</span></span>

<span data-ttu-id="98694-173">Az alábbiakban olvashat némi útmutatást ezzel kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="98694-173">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="98694-174">**1. lépés: Hello teljes szálak számának meghatározása** -kiszámításának hello teljes szál száma toouse el.</span><span class="sxs-lookup"><span data-stu-id="98694-174">**Step 1: Determine hello total thread count** - You should start by calculating hello total thread count toouse.</span></span> <span data-ttu-id="98694-175">Általánosságban 6 szálat használjon minden egyes fizikai maghoz.</span><span class="sxs-lookup"><span data-stu-id="98694-175">As a general guideline, you should use 6 threads for each physical core.</span></span>

        Total thread count = total physical cores * 6

    <span data-ttu-id="98694-176">**Példa**</span><span class="sxs-lookup"><span data-stu-id="98694-176">**Example**</span></span>

    <span data-ttu-id="98694-177">Feltéve, hogy futtatja a hello PowerShell-parancsok D14 VM 16 maggal rendelkező</span><span class="sxs-lookup"><span data-stu-id="98694-177">Assuming you are running hello PowerShell commands from a D14 VM that has 16 cores</span></span>

        Total thread count = 16 cores * 6 = 96 threads


* <span data-ttu-id="98694-178">**2. lépés: Kiszámításához PerFileThreadCount** -azt a PerFileThreadCount hello fájlok hello méretének kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="98694-178">**Step 2: Calculate PerFileThreadCount**  - We calculate our PerFileThreadCount based on hello size of hello files.</span></span> <span data-ttu-id="98694-179">2.5 GB-nál kisebb fájlok esetében nincs nincs szükség toochange ennek a paraméternek, mert hello alapértelmezett 10 is elegendő.</span><span class="sxs-lookup"><span data-stu-id="98694-179">For files smaller than 2.5GB, there is no need toochange this parameter because hello default of 10 is sufficient.</span></span> <span data-ttu-id="98694-180">2.5 GB-nál nagyobb méretű fájlokhoz 10 szálat hello alapja hello első 2,5 GB, és a fájlméret 1 szál minden további 256 MB-os növekedést hozzáadása kell használnia.</span><span class="sxs-lookup"><span data-stu-id="98694-180">For files larger than 2.5GB, you should use 10 threads as hello base for hello first 2.5GB and add 1 thread for each additional 256MB increase in file size.</span></span> <span data-ttu-id="98694-181">Ha olyan mappát másol, amelyben nagyon eltérő méretű fájlok találhatók, érdemes hasonló fájlméret alapján csoportosítani a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="98694-181">If you are copying a folder with a large range of file sizes, consider grouping them into similar file sizes.</span></span> <span data-ttu-id="98694-182">Az eltérő fájlméretek az optimálisnál kisebb teljesítménnyel járhatnak.</span><span class="sxs-lookup"><span data-stu-id="98694-182">Having dissimilar file sizes may cause non-optimal performance.</span></span> <span data-ttu-id="98694-183">Ha ez nem lehetséges toogroup hasonló méretűek, célszerű PerFileThreadCount hello legnagyobb mérete alapján.</span><span class="sxs-lookup"><span data-stu-id="98694-183">If that's not possible toogroup similar file sizes, you should set PerFileThreadCount based on hello largest file size.</span></span>

        PerFileThreadCount = 10 threads for hello first 2.5GB + 1 thread for each additional 256MB increase in file size

    <span data-ttu-id="98694-184">**Példa**</span><span class="sxs-lookup"><span data-stu-id="98694-184">**Example**</span></span>

    <span data-ttu-id="98694-185">Ha pedig az 1 GB-os too10GB közötti 100 fájlokat, használjuk hello 10 GB-os, hello legnagyobb fájlméret egyenlet, amely hasonló hello olvashatók.</span><span class="sxs-lookup"><span data-stu-id="98694-185">Assuming you have 100 files ranging from 1GB too10GB, we use hello 10GB as hello largest file size for equation, which would read like hello following.</span></span>

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* <span data-ttu-id="98694-186">**3. lépés: Kiszámításához ConcurrentFilecount** -használata hello teljes szálak száma és PerFileThreadCount toocalculate ConcurrentFileCount alapján a következő egyenlet hello.</span><span class="sxs-lookup"><span data-stu-id="98694-186">**Step 3: Calculate ConcurrentFilecount** - Use hello total thread count and PerFileThreadCount toocalculate ConcurrentFileCount based on hello following equation.</span></span>

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    <span data-ttu-id="98694-187">**Példa**</span><span class="sxs-lookup"><span data-stu-id="98694-187">**Example**</span></span>

    <span data-ttu-id="98694-188">A jelenleg használt hello példa értékek alapján</span><span class="sxs-lookup"><span data-stu-id="98694-188">Based on hello example values we have been using</span></span>

        96 = 40 * ConcurrentFileCount

    <span data-ttu-id="98694-189">Igen **ConcurrentFileCount** van **2.4**, amely azt is túl kerekítése**2**.</span><span class="sxs-lookup"><span data-stu-id="98694-189">So, **ConcurrentFileCount** is **2.4**, which we can round off too**2**.</span></span>

### <a name="further-tuning"></a><span data-ttu-id="98694-190">További hangolás</span><span class="sxs-lookup"><span data-stu-id="98694-190">Further tuning</span></span>

<span data-ttu-id="98694-191">Lehet szükség, mert a fájl mérete toowork számos további hangolása.</span><span class="sxs-lookup"><span data-stu-id="98694-191">You might require further tuning because there is a range of file sizes toowork with.</span></span> <span data-ttu-id="98694-192">számítási fent hello jól vagy hello fájlok nagyobb és szorosabb toohello 10 GB-os tartomány működik.</span><span class="sxs-lookup"><span data-stu-id="98694-192">hello above calculation works well if all or most of hello files are larger and closer toohello 10GB range.</span></span> <span data-ttu-id="98694-193">Ha ehelyett sok különböző, nagyon eltérő méretű fájllal rendelkezik, akkor csökkentheti a PerFileThreadCount értékét.</span><span class="sxs-lookup"><span data-stu-id="98694-193">If instead, there are many different files sizes with many files being smaller, then you could reduce PerFileThreadCount.</span></span> <span data-ttu-id="98694-194">Hello PerFileThreadCount csökkentésével ConcurrentFileCount növelheti azt.</span><span class="sxs-lookup"><span data-stu-id="98694-194">By reducing hello PerFileThreadCount, we can increase ConcurrentFileCount.</span></span> <span data-ttu-id="98694-195">Ezért, ha feltételezzük, hogy a fájlok többsége kisebb hello 5GB közé, az alábbiakat tehetjük újra a számítás elvégzése:</span><span class="sxs-lookup"><span data-stu-id="98694-195">So, if we assume that most of our files are smaller in hello 5GB range, we can re-do our calculation:</span></span>

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

<span data-ttu-id="98694-196">Igen **ConcurrentFileCount** most 96/20, amely 4.8, kerekíti túl**4**.</span><span class="sxs-lookup"><span data-stu-id="98694-196">So, **ConcurrentFileCount** will now be 96/20, which is 4.8, rounded off too**4**.</span></span>

<span data-ttu-id="98694-197">A Folytatás tootune ezek a beállítások módosításával hello **PerFileThreadCount** felfelé és lefelé attól függően, a fájlméret hello terjesztése.</span><span class="sxs-lookup"><span data-stu-id="98694-197">You can continue tootune these settings by changing hello **PerFileThreadCount** up and down depending on hello distribution of your file sizes.</span></span>

### <a name="limitation"></a><span data-ttu-id="98694-198">Korlátozás</span><span class="sxs-lookup"><span data-stu-id="98694-198">Limitation</span></span>

* <span data-ttu-id="98694-199">**A fájlok száma nem éri el ConcurrentFileCount**: Ha a feltölteni kívánt fájlok hello száma értéke kisebb a hello **ConcurrentFileCount** számolt ki, majd csökkentse  **ConcurrentFileCount** toobe egyenlő toohello azon fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="98694-199">**Number of files is less than ConcurrentFileCount**: If hello number of files you are uploading is smaller than hello **ConcurrentFileCount** that you calculated, then you should reduce **ConcurrentFileCount** toobe equal toohello number of files.</span></span> <span data-ttu-id="98694-200">Használhat a fennmaradó szálak tooincrease **PerFileThreadCount**.</span><span class="sxs-lookup"><span data-stu-id="98694-200">You can use any remaining threads tooincrease **PerFileThreadCount**.</span></span>

* <span data-ttu-id="98694-201">**Túl sok szál**: Ha növeli a szál száma túl sok, a fürtméret növelése nélkül, a teljesítmény csökkenését hello kockázatát futtatja.</span><span class="sxs-lookup"><span data-stu-id="98694-201">**Too many threads**: If you increase thread count too much without increasing your cluster size, you run hello risk of degraded performance.</span></span> <span data-ttu-id="98694-202">Lehet versengés problémák váltáskor környezet – a hello CPU.</span><span class="sxs-lookup"><span data-stu-id="98694-202">There can be contention issues when context-switching on hello CPU.</span></span>

* <span data-ttu-id="98694-203">**Nincs elegendő feldolgozási**: Ha hello CONCURRENCY paraméterének értéke nem megfelelő, akkor előfordulhat, hogy a fürt túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="98694-203">**Insufficient concurrency**: If hello concurrency is not sufficient, then your cluster may be too small.</span></span> <span data-ttu-id="98694-204">A fürtben, ez több egyidejű hello csomópontok száma növelhető.</span><span class="sxs-lookup"><span data-stu-id="98694-204">You can increase hello number of nodes in your cluster which will give you more concurrency.</span></span>

* <span data-ttu-id="98694-205">**Szabályozási hibák**: Elképzelhető, hogy szabályozási hibákat tapasztal, ha az egyidejűség túl magas.</span><span class="sxs-lookup"><span data-stu-id="98694-205">**Throttling errors**: You may see throttling errors if your concurrency is too high.</span></span> <span data-ttu-id="98694-206">Ha Ön hibák szabályozás, meg kell csökkentse hello párhuzamossági vagy, lépjen velünk kapcsolatba.</span><span class="sxs-lookup"><span data-stu-id="98694-206">If you are seeing throttling errors, you should either reduce hello concurrency or contact us.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98694-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98694-207">Next steps</span></span>
* [<span data-ttu-id="98694-208">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="98694-208">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="98694-209">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="98694-209">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="98694-210">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="98694-210">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

