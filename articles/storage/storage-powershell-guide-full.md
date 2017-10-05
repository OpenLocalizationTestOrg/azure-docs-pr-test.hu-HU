---
title: "Az Azure Storage Azure PowerShell használatával |} Microsoft Docs"
description: "Az Azure Storage Azure PowerShell-parancsmagok használata létrehozásához és kezeléséhez a storage-fiókok; blobok, táblák, üzenetsorok és fájlok; használata konfigurálja és tárolási analitika lekérdezése, és a közös hozzáférésű jogosultságkód létrehozása."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 51e3e93ebedd31370857e61a00139294bcee9237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a><span data-ttu-id="70324-103">Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)</span><span class="sxs-lookup"><span data-stu-id="70324-103">Using Azure PowerShell with Azure Storage</span></span>
## <a name="overview"></a><span data-ttu-id="70324-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="70324-104">Overview</span></span>
<span data-ttu-id="70324-105">Az Azure PowerShell modul, amely kezelése a Windows PowerShell segítségével Azure-parancsmagokat kínál.</span><span class="sxs-lookup"><span data-stu-id="70324-105">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="70324-106">Ez egy feladatalapú parancshéj és parancsnyelv, amely kifejezetten rendszerfelügyeleti célra készült.</span><span class="sxs-lookup"><span data-stu-id="70324-106">It is a task-based command-line shell and scripting language designed especially for system administration.</span></span> <span data-ttu-id="70324-107">A PowerShell-lel könnyen szabályozhatja és az Azure-szolgáltatások és alkalmazások automatizálják.</span><span class="sxs-lookup"><span data-stu-id="70324-107">With PowerShell, you can easily control and automate the administration of your Azure services and applications.</span></span> <span data-ttu-id="70324-108">Például használhatja a parancsmagokat a azonos feladatok végrehajtását, amelyek segítségével végezheti el a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="70324-108">For example, you can use the cmdlets to perform the same tasks that you can perform through the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="70324-109">Az útmutató azt fogja felfedezés használata a [Azure tárolási parancsmagok](/powershell/module/azurerm.storage/#storage) különböző az Azure Storage fejlesztői és felügyeleti feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="70324-109">In this guide, we'll explore how to use the [Azure Storage Cmdlets](/powershell/module/azurerm.storage/#storage) to perform a variety of development and administration tasks with Azure Storage.</span></span>

<span data-ttu-id="70324-110">Ez az útmutató feltételezi, hogy rendelkezik tapasztalattal használatával [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) és [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-110">This guide assumes that you have prior experience using [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) and [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span></span> <span data-ttu-id="70324-111">Az útmutató számos bemutatásához az használatát, az Azure Storage PowerShell parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="70324-111">The guide provides a number of scripts to demonstrate the usage of PowerShell with Azure Storage.</span></span> <span data-ttu-id="70324-112">Minden parancsprogram futtatása előtt a konfiguráció alapján a parancsfájl-változókat frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="70324-112">You should update the script variables based on your configuration before running each script.</span></span>

<span data-ttu-id="70324-113">A jelen útmutató az első szakaszban Azure Storage és a PowerShell kiadványok biztosít.</span><span class="sxs-lookup"><span data-stu-id="70324-113">The first section in this guide provides a quick glance at Azure Storage and PowerShell.</span></span> <span data-ttu-id="70324-114">Részletes információk és utasítások, indítását a [az Azure Storage Azure PowerShell használatára vonatkozó Előfeltételek](#prerequisites-for-using-azure-powershell-with-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="70324-114">For detailed information and instructions, start from the [Prerequisites for using Azure PowerShell with Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span></span>

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a><span data-ttu-id="70324-115">Ismerkedés az Azure Storage és a PowerShell 5 perc</span><span class="sxs-lookup"><span data-stu-id="70324-115">Getting started with Azure Storage and PowerShell in 5 minutes</span></span>
<span data-ttu-id="70324-116">Ez a szakasz bemutatja, hogyan számára az Azure Storage PowerShell 5 perc.</span><span class="sxs-lookup"><span data-stu-id="70324-116">This section shows you how to access Azure Storage via PowerShell in 5 minutes.</span></span>

<span data-ttu-id="70324-117">**Új Azure-bA:** a Microsoft Azure-előfizetés és az adott előfizetéshez tartozó Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="70324-117">**New to Azure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="70324-118">Az Azure megvásárlási lehetőségeinek információkért lásd: [ingyenes](https://azure.microsoft.com/pricing/free-trial/), [beszerzési lehetőségek](https://azure.microsoft.com/pricing/purchase-options/), és [ajánlatok](https://azure.microsoft.com/pricing/member-offers/) (az MSDN, a Microsoft Partner Network, és a BizSpark és egyéb Microsoft programok tagjai).</span><span class="sxs-lookup"><span data-stu-id="70324-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="70324-119">Lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure-előfizetések további információt.</span><span class="sxs-lookup"><span data-stu-id="70324-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="70324-120">**Miután létrehozta a Microsoft Azure-előfizetésre és -fiókra:**</span><span class="sxs-lookup"><span data-stu-id="70324-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="70324-121">Töltse le és telepítse a legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span><span class="sxs-lookup"><span data-stu-id="70324-121">Download and install the latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span></span>
2. <span data-ttu-id="70324-122">Kezdő Windows PowerShell integrált parancsfájlkezelés környezet (ISE): A helyi számítógépen, válassza a **Start** menü.</span><span class="sxs-lookup"><span data-stu-id="70324-122">Start Windows PowerShell Integrated Scripting Environment (ISE): In your local computer, go to the **Start** menu.</span></span> <span data-ttu-id="70324-123">Típus **felügyeleti eszközök** kattintson futtatni.</span><span class="sxs-lookup"><span data-stu-id="70324-123">Type **Administrative Tools** and click to run it.</span></span> <span data-ttu-id="70324-124">Az a **felügyeleti eszközök** ablak, kattintson a jobb gombbal **Windows PowerShell ISE**, kattintson a **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="70324-124">In the **Administrative Tools** window, right-click **Windows PowerShell ISE**, click **Run as Administrator**.</span></span>
3. <span data-ttu-id="70324-125">A **Windows PowerShell ISE**, kattintson a **fájl** > **új** létrehozni egy új parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="70324-125">In **Windows PowerShell ISE**, click **File** > **New** to create a new script file.</span></span>
4. <span data-ttu-id="70324-126">Most lesz ad egy egyszerű parancsprogram, amely alapszintű PowerShell-parancsok Azure Storage eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="70324-126">Now, we'll give you a simple script that shows basic PowerShell commands to access Azure Storage.</span></span> <span data-ttu-id="70324-127">A parancsfájl először kérje meg az Azure fiók hitelesítő adatait az Azure hozzáadandó fiókot a helyi PowerShell környezetben.</span><span class="sxs-lookup"><span data-stu-id="70324-127">The script will first ask your Azure account credentials to add your Azure account to the local PowerShell environment.</span></span> <span data-ttu-id="70324-128">Ezt követően a parancsfájl az alapértelmezett Azure-előfizetés, és új tárfiók létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="70324-128">Then, the script will set the default Azure subscription and create a new storage account in Azure.</span></span> <span data-ttu-id="70324-129">Ezután a parancsfájl hozzon létre egy új tárolót az új tárfiókot, és töltse fel a meglévő képfájl (blob) a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="70324-129">Next, the script will create a new container in this new storage account and upload an existing image file (blob) to that container.</span></span> <span data-ttu-id="70324-130">Miután a parancsfájl megjeleníti az adott tároló összes BLOB, a helyi számítógépen egy új célkönyvtáron létrehozza, és töltse le a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="70324-130">After the script lists all blobs in that container, it will create a new destination directory in your local computer and download the image file.</span></span>
5. <span data-ttu-id="70324-131">A következő kódot a szakaszban válassza ki a parancsfájl a megjegyzések között **#begin** és **#end**.</span><span class="sxs-lookup"><span data-stu-id="70324-131">In the following code section, select the script between the remarks **#begin** and **#end**.</span></span> <span data-ttu-id="70324-132">Nyomja le a CTRL + C billentyűkombinációval másolja a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="70324-132">Press CTRL+C to copy it to the clipboard.</span></span>

    ```powershell
    # begin
    # Update with the name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name to your new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name to your new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account to the local PowerShell environment.
    Add-AzureAccount
       
    # Set a default Azure subscription.
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
       
    # Create a new storage account.
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location
       
    # Set a default storage account.
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
       
    # Create a new container.
    New-AzureStorageContainer -Name $ContainerName -Permission Off
       
    # Upload a blob into a container.
    Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload
       
    # List all blobs in a container.
    Get-AzureStorageBlob -Container $ContainerName
       
    # Download blobs from the container:
    # Get a reference to a list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create the destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into the local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. <span data-ttu-id="70324-133">A **Windows PowerShell ISE**, nyomja le a CTRL + V billentyűkombinációval másolja a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="70324-133">In **Windows PowerShell ISE**, press CTRL+V to copy the script.</span></span> <span data-ttu-id="70324-134">Kattintson a **fájl** > **mentése**.</span><span class="sxs-lookup"><span data-stu-id="70324-134">Click **File** > **Save**.</span></span> <span data-ttu-id="70324-135">Az a **Mentés másként** párbeszédpanelen írja be a nevét, a parancsfájl, például a "mystoragescript."</span><span class="sxs-lookup"><span data-stu-id="70324-135">In the **Save As** dialog window, type the name of the script file, such as "mystoragescript."</span></span> <span data-ttu-id="70324-136">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="70324-136">Click **Save**.</span></span>
7. <span data-ttu-id="70324-137">Most módosítania a parancsfájl-változókat a konfigurációs beállítások alapján.</span><span class="sxs-lookup"><span data-stu-id="70324-137">Now, you need to update the script variables based on your configuration settings.</span></span> <span data-ttu-id="70324-138">Frissítenie kell a **$SubscriptionName** változó a saját előfizetésével.</span><span class="sxs-lookup"><span data-stu-id="70324-138">You must update the **$SubscriptionName** variable with your own subscription.</span></span> <span data-ttu-id="70324-139">Tartsa a többi változó a parancsfájlban megadott, vagy igény szerint frissítse azokat.</span><span class="sxs-lookup"><span data-stu-id="70324-139">You can keep the other variables as specified in the script or update them as you wish.</span></span>
   
   * <span data-ttu-id="70324-140">**$SubscriptionName:** ezt a változót a saját előfizetés nevével kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="70324-140">**$SubscriptionName:** You must update this variable with your own subscription name.</span></span> <span data-ttu-id="70324-141">Kövesse a következő háromféleképpen keresse meg az előfizetés nevét:</span><span class="sxs-lookup"><span data-stu-id="70324-141">Follow one of the following three ways to locate the name of your subscription:</span></span>
     
    <span data-ttu-id="70324-142">a.</span><span class="sxs-lookup"><span data-stu-id="70324-142">a.</span></span> <span data-ttu-id="70324-143">A **Windows PowerShell ISE**, kattintson a **fájl** > **új** létrehozni egy új parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="70324-143">In **Windows PowerShell ISE**, click **File** > **New** to create a new script file.</span></span> <span data-ttu-id="70324-144">Másolja a következő parancsfájlt az új parancsfájlt, és kattintson a **Debug** > **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="70324-144">Copy the following script to the new script file and click **Debug** > **Run**.</span></span> <span data-ttu-id="70324-145">A következő parancsfájl először kérje meg az Azure fiók hitelesítő adatait az Azure hozzáadása a helyi PowerShell környezetre fiókot, és majd az összes olyan előfizetést, a helyi PowerShell-munkamenethez kapcsolódó megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="70324-145">The following script will first ask your Azure account credentials to add your Azure account to the local PowerShell environment and then show all the subscriptions that are connected to the local PowerShell session.</span></span> <span data-ttu-id="70324-146">Jegyezze fel az oktatóanyag során használni kívánt előfizetést nevét:</span><span class="sxs-lookup"><span data-stu-id="70324-146">Take a note of the name of the subscription that you want to use while following this tutorial:</span></span>
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    <span data-ttu-id="70324-147">b.</span><span class="sxs-lookup"><span data-stu-id="70324-147">b.</span></span> <span data-ttu-id="70324-148">Keresse meg és másolja a előfizetés nevét a [Azure-portálon](https://portal.azure.com), kattintson a bal oldali menüben **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="70324-148">To locate and copy your subscription name in the [Azure portal](https://portal.azure.com), in the Hub menu on the left, click **Subscriptions**.</span></span> <span data-ttu-id="70324-149">Másolja át az előfizetést, amelyet a jelen útmutató a parancsfájlok futtatása során használni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="70324-149">Copy the name of subscription that you want to use while running the scripts in this guide.</span></span>
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    <span data-ttu-id="70324-151">c.</span><span class="sxs-lookup"><span data-stu-id="70324-151">c.</span></span> <span data-ttu-id="70324-152">Keresse meg és másolja a előfizetés nevét a [klasszikus Azure portál](https://manage.windowsazure.com/), görgessen lefelé, és kattintson a **beállítások** a portál bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="70324-152">To locate and copy your subscription name in the [Azure Classic Portal](https://manage.windowsazure.com/), scroll down and click **Settings** on the left side of the portal.</span></span> <span data-ttu-id="70324-153">Kattintson a **előfizetések** az előfizetések listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="70324-153">Click **Subscriptions** to see a list of your subscriptions.</span></span> <span data-ttu-id="70324-154">Másolja át az előfizetést, amelyet a jelen útmutató a megadott parancsfájlok futtatása során használni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="70324-154">Copy the name of subscription that you want to use while running the scripts given in this guide.</span></span>
     
     ![klasszikus Azure portál](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * <span data-ttu-id="70324-156">**$StorageAccountName:** használja a parancsfájl a megadott névvel, vagy adjon meg egy új nevet a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="70324-156">**$StorageAccountName:** Use the given name in the script or enter a new name for your storage account.</span></span> <span data-ttu-id="70324-157">**Fontos:** a tárfiók nevét az Azure-ban egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="70324-157">**Important:** The name of the storage account must be unique in Azure.</span></span> <span data-ttu-id="70324-158">Az kisbetűnek kell lennie, túl!</span><span class="sxs-lookup"><span data-stu-id="70324-158">It must be lowercase, too!</span></span>
   * <span data-ttu-id="70324-159">**$Location:** a parancsfájl a megadott "USA nyugati régiója" használja, vagy válasszon más Azure helyeken, például az USA keleti régiója, Észak-Európa, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="70324-159">**$Location:** Use the given "West US" in the script or choose other Azure locations, such as East US, North Europe, and so on.</span></span>
   * <span data-ttu-id="70324-160">**$ContainerName:** használja a parancsfájl a megadott névvel, vagy adjon meg egy új nevet a tároló.</span><span class="sxs-lookup"><span data-stu-id="70324-160">**$ContainerName:** Use the given name in the script or enter a new name for your container.</span></span>
   * <span data-ttu-id="70324-161">**$ImageToUpload:** adja meg egy elérési utat kép a helyi számítógépen, például: "C:\Images\HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="70324-161">**$ImageToUpload:** Enter a path to a picture on your local computer, such as: "C:\Images\HelloWorld.png".</span></span>
   * <span data-ttu-id="70324-162">**$DestinationFolder:** meg elérési útját a helyi könyvtárat, többek között az Azure Storage-ból letöltött fájlok tárolására szolgáló: "C:\DownloadImages".</span><span class="sxs-lookup"><span data-stu-id="70324-162">**$DestinationFolder:** Enter a path to a local directory to store files downloaded from Azure Storage, such as: "C:\DownloadImages".</span></span>
8. <span data-ttu-id="70324-163">Miután frissítette a parancsfájl-változókat a "mystoragescript.ps1" fájlban, kattintson a **fájl** > **mentése**.</span><span class="sxs-lookup"><span data-stu-id="70324-163">After updating the script variables in the "mystoragescript.ps1" file, click **File** > **Save**.</span></span> <span data-ttu-id="70324-164">Kattintson a **Debug** > **futtatása** vagy nyomja le az ENTER **F5** a parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="70324-164">Then, click **Debug** > **Run** or press **F5** to run the script.</span></span>  

<span data-ttu-id="70324-165">A parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött lemezképfájlt.</span><span class="sxs-lookup"><span data-stu-id="70324-165">After the script runs, you should have a local destination folder that includes the downloaded image file.</span></span> <span data-ttu-id="70324-166">Az alábbi képernyőfelvételen látható egy példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="70324-166">The following screenshot shows an example output:</span></span>

![Blobok letöltése](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> <span data-ttu-id="70324-168">A "Bevezetés az Azure Storage és a PowerShell 5 percben" szakaszban megadott gyors Bevezetés az Azure Storage Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="70324-168">The "Getting started with Azure Storage and PowerShell in 5 minutes" section provided a quick introduction on how to use Azure PowerShell with Azure Storage.</span></span> <span data-ttu-id="70324-169">Részletes információk és utasítások javasoljuk, hogy olvassa el a következő szakaszokat.</span><span class="sxs-lookup"><span data-stu-id="70324-169">For detailed information and instructions, we encourage you to read the following sections.</span></span>
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a><span data-ttu-id="70324-170">Az Azure Storage Azure PowerShell használatának előfeltételei</span><span class="sxs-lookup"><span data-stu-id="70324-170">Prerequisites for using Azure PowerShell with Azure Storage</span></span>
<span data-ttu-id="70324-171">Egy Azure-előfizetés és a fiók ebben az útmutatóban megadott PowerShell-parancsmagok futtatásához a fent leírt módon van szükség.</span><span class="sxs-lookup"><span data-stu-id="70324-171">You need an Azure subscription and account to run the PowerShell cmdlets given in this guide, as described above.</span></span>

<span data-ttu-id="70324-172">Az Azure PowerShell modul, amely kezelése a Windows PowerShell segítségével Azure-parancsmagokat kínál.</span><span class="sxs-lookup"><span data-stu-id="70324-172">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="70324-173">A telepítése és beállítása az Azure PowerShell információkért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="70324-173">For information on installing and setting up Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="70324-174">Azt javasoljuk, hogy letöltése és telepítése vagy frissítése a legújabb Azure PowerShell modulra az útmutató használata előtt.</span><span class="sxs-lookup"><span data-stu-id="70324-174">We recommend that you download and install or upgrade to the latest Azure PowerShell module before using this guide.</span></span>

<span data-ttu-id="70324-175">A parancsmagokat futtathatja a szabványos Windows PowerShell-konzol vagy a Windows PowerShell integrált parancsfájlkezelési környezet (ISE).</span><span class="sxs-lookup"><span data-stu-id="70324-175">You can run the cmdlets in the standard Windows PowerShell console or the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="70324-176">Ahhoz például, hogy nyissa meg a **Windows PowerShell ISE**, válassza a Start menü, írja be a felügyeleti eszközök, és kattintson a futtatni.</span><span class="sxs-lookup"><span data-stu-id="70324-176">For example, to open **Windows PowerShell ISE**, go to the Start menu, type Administrative Tools and click to run it.</span></span> <span data-ttu-id="70324-177">A felügyeleti eszközök ablakban kattintson a jobb gombbal a Windows PowerShell ISE, kattintson a Futtatás rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="70324-177">In the Administrative Tools window, right-click Windows PowerShell ISE, click Run as Administrator.</span></span>

## <a name="how-to-manage-storage-accounts-in-azure"></a><span data-ttu-id="70324-178">Az Azure storage-fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-178">How to manage storage accounts in Azure</span></span>

<span data-ttu-id="70324-179">Vessen egy pillantást a PowerShell-lel Azure storage-fiók kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-179">Let's take a look at managing storage accounts in Azure with PowerShell</span></span>

### <a name="how-to-set-a-default-azure-subscription"></a><span data-ttu-id="70324-180">Az alapértelmezett Azure-előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="70324-180">How to set a default Azure subscription</span></span>
<span data-ttu-id="70324-181">Kezeli az Azure Storage Azure PowerShell használatával, az ügyfél-környezet Azure az Azure Active Directory-hitelesítés vagy Tanúsítványalapú hitelesítés használatával hitelesíteni kell.</span><span class="sxs-lookup"><span data-stu-id="70324-181">To manage Azure Storage using Azure PowerShell, you need to authenticate your client environment with Azure via Azure Active Directory authentication or certificate-based authentication.</span></span> <span data-ttu-id="70324-182">Részletes információkért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="70324-182">For detailed information, see [How to install and configure Azure PowerShell](/powershell/azure/overview) tutorial.</span></span> <span data-ttu-id="70324-183">Ez az útmutató az Azure Active Directory-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="70324-183">This guide uses the Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="70324-184">A Windows PowerShell ISE, írja be a következő parancs futtatásával adja hozzá az Azure fiók a helyi PowerShell környezetre:</span><span class="sxs-lookup"><span data-stu-id="70324-184">In Windows PowerShell ISE, type the following command to add your Azure account to the local PowerShell environment:</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="70324-185">A "Bejelentkezés a Microsoft Azure-beli" ablakban írja be az e-mail címet és a fiókhoz társított jelszót.</span><span class="sxs-lookup"><span data-stu-id="70324-185">In the "Sign in to Microsoft Azure" window, type the email address and password associated with your account.</span></span> <span data-ttu-id="70324-186">Az Azure hitelesíti és menti a hitelesítő adatokat, majd bezárja az ablakot.</span><span class="sxs-lookup"><span data-stu-id="70324-186">Azure authenticates and saves the credential information, and then closes the window.</span></span>

3. <span data-ttu-id="70324-187">Ezután futtassa a következő parancsot az Azure-fiókra megtekintheti a helyi PowerShell-környezetében, és győződjön meg arról, hogy a fiók szerepel:</span><span class="sxs-lookup"><span data-stu-id="70324-187">Next, run the following command to view the Azure accounts in your local PowerShell environment, and verify that your account is listed:</span></span>
   
    ```powershell
    Get-AzureAccount
    ```
4. <span data-ttu-id="70324-188">Ezután futtassa a következő parancsmagot a helyi PowerShell-munkamenethez kapcsolódó összes előfizetés megtekintéséhez, és ellenőrizze, hogy szerepel-e az előfizetés:</span><span class="sxs-lookup"><span data-stu-id="70324-188">Then, run the following cmdlet to view all the subscriptions that are connected to the local PowerShell session, and verify that your subscription is listed:</span></span>

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. <span data-ttu-id="70324-189">Az alapértelmezett Azure-előfizetés beállításához futtassa a Select-AzureSubscription parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="70324-189">To set a default Azure subscription, run the Select-AzureSubscription cmdlet:</span></span>

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. <span data-ttu-id="70324-190">Ellenőrizze a nevét, az alapértelmezett előfizetés a Get-AzureSubscription parancsmag futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70324-190">Verify the name of the default subscription by running the Get-AzureSubscription cmdlet:</span></span>

    ```powershell
    Get-AzureSubscription -Default
    ```

7. <span data-ttu-id="70324-191">Az Azure Storage összes rendelkezésre álló PowerShell-parancsmagok megtekintéséhez futtassa:</span><span class="sxs-lookup"><span data-stu-id="70324-191">To see all the available PowerShell cmdlets for Azure Storage, run:</span></span>
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-to-create-a-new-azure-storage-account"></a><span data-ttu-id="70324-192">Egy új Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-192">How to create a new Azure storage account</span></span>
<span data-ttu-id="70324-193">Az Azure storage használatához szüksége lesz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="70324-193">To use Azure storage, you will need a storage account.</span></span> <span data-ttu-id="70324-194">Egy új Azure-tárfiókot is létrehozhat, a számítógép csatlakozni az előfizetés konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="70324-194">You can create a new Azure storage account after you have configured your computer to connect to your subscription.</span></span>

1. <span data-ttu-id="70324-195">Futtassa a Get-AzureLocation parancsmagot a rendelkezésre álló adatközpont-helyeinek kereséséhez:</span><span class="sxs-lookup"><span data-stu-id="70324-195">Run the Get-AzureLocation cmdlet to find all the available datacenter locations:</span></span>

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. <span data-ttu-id="70324-196">Ezután futtassa a New-AzureStorageAccount parancsmaggal hozzon létre egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="70324-196">Next, run the New-AzureStorageAccount cmdlet to create a new storage account.</span></span> <span data-ttu-id="70324-197">Az alábbi példában az "USA nyugati régiója" adatközpontban hoz létre egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="70324-197">The following example creates a new storage account in the "West US" datacenter.</span></span>
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> <span data-ttu-id="70324-198">A tárfiók neve Azure belül egyedieknek kell lenniük, és kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="70324-198">The name of your storage account must be unique within Azure and must be lowercase.</span></span> <span data-ttu-id="70324-199">Az elnevezési konvenciókat és korlátozások, lásd: [kapcsolatos Azure Storage-fiókok](storage-create-storage-account.md) és [elnevezési és hivatkozó tárolók, Blobok és metaadatok](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-199">For naming conventions and restrictions, see [About Azure Storage Accounts](storage-create-storage-account.md) and [Naming and Referencing Containers, Blobs, and Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span></span>
> 
> 

### <a name="how-to-set-a-default-azure-storage-account"></a><span data-ttu-id="70324-200">Egy alapértelmezett Azure storage-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="70324-200">How to set a default Azure storage account</span></span>
<span data-ttu-id="70324-201">Az előfizetés több tárfiókot is lehet.</span><span class="sxs-lookup"><span data-stu-id="70324-201">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="70324-202">Válasszon egyet közülük, és állítsa be az alapértelmezett tárfiókot, a tárolási parancs összes ugyanabban a PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="70324-202">You can choose one of them and set it as the default storage account for all the storage commands in the same PowerShell session.</span></span> <span data-ttu-id="70324-203">Ez lehetővé teszi, hogy az Azure PowerShell tárolási parancsok az adattároló-környezet explicit megadása nélkül futtatja.</span><span class="sxs-lookup"><span data-stu-id="70324-203">This enables you to run the Azure PowerShell storage commands without specifying the storage context explicitly.</span></span>

1. <span data-ttu-id="70324-204">Állítsa be az előfizetés egy alapértelmezett tárfiókot, a Set-AzureSubscription parancsmag is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="70324-204">To set a default storage account for your subscription, you can run the Set-AzureSubscription cmdlet.</span></span>

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. <span data-ttu-id="70324-205">Ezután futtassa a Get-AzureSubscription parancsmagot, ellenőrizze, hogy a storage-fiók alapértelmezett előfizetés fiókhoz tartozó.</span><span class="sxs-lookup"><span data-stu-id="70324-205">Next, run the Get-AzureSubscription cmdlet to verify that the storage account is associated with your default subscription account.</span></span> <span data-ttu-id="70324-206">Ez a parancs az előfizetés tulajdonságai a jelenlegi előfizetés, beleértve az aktuális tárfiók adja vissza.</span><span class="sxs-lookup"><span data-stu-id="70324-206">This command returns the subscription properties on the current subscription including its current storage account.</span></span>

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a><span data-ttu-id="70324-207">Hogyan listázhat előfizetés minden Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="70324-207">How to list all Azure storage accounts in a subscription</span></span>
<span data-ttu-id="70324-208">Minden Azure-előfizetés lehet akár 100 storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="70324-208">Each Azure subscription can have up to 100 storage accounts.</span></span> <span data-ttu-id="70324-209">Korlátozások a legfrissebb információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="70324-209">For the most up-to-date information on limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="70324-210">Futtassa a nevét és a jelenlegi előfizetés tárfiókok állapotának megállapítása a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="70324-210">Run the following cmdlet to find out the name and status of the storage accounts in the current subscription:</span></span>

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-to-create-an-azure-storage-context"></a><span data-ttu-id="70324-211">Egy Azure storage-környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-211">How to create an Azure storage context</span></span>
<span data-ttu-id="70324-212">Az Azure storage-környezet egy objektum a PowerShell foglalják magukban a tároló hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="70324-212">Azure storage context is an object in PowerShell to encapsulate the storage credentials.</span></span> <span data-ttu-id="70324-213">Tárolási környezetet használ, bármely ezt követő parancsmag lehetővé teszi, hogy a kérés hitelesítéséhez a tárfiók és a hozzáférési kulcs explicit megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="70324-213">Using a storage context while running any subsequent cmdlet enables you to authenticate your request without specifying the storage account and its access key explicitly.</span></span> <span data-ttu-id="70324-214">Adattároló-környezet az sokféleképpen, például a tárolási fiók nevét és hívóbetűjét, a közös hozzáférésű jogosultságkód (SAS) jogkivonatot, a kapcsolati karakterlánc használatával hozhat létre vagy névtelen.</span><span class="sxs-lookup"><span data-stu-id="70324-214">You can create a storage context in many ways, such as using storage account name and access key, shared access signature (SAS) token, connection string, or anonymous.</span></span> <span data-ttu-id="70324-215">További információkért lásd: [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span><span class="sxs-lookup"><span data-stu-id="70324-215">For more information, see [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span></span>  

<span data-ttu-id="70324-216">Adattároló-környezet létrehozása a következő három módon egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="70324-216">Use one of the following three ways to create a storage context:</span></span>

* <span data-ttu-id="70324-217">Futtassa a [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) parancsmag tudja meg a elsődleges hozzáférési kulcsot az Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="70324-217">Run the [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet to find out the primary storage access key for your Azure storage account.</span></span> <span data-ttu-id="70324-218">Ezután hívja a [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) parancsmaggal hozzon létre egy tárolási környezetben:</span><span class="sxs-lookup"><span data-stu-id="70324-218">Next, call the [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet to create a storage context:</span></span>

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* <span data-ttu-id="70324-219">Az Azure storage-tároló megosztott hozzáférési aláírást jogkivonat előállítása és adattároló-környezet létrehozására használható:</span><span class="sxs-lookup"><span data-stu-id="70324-219">Generate a shared access signature token for an Azure storage container and use it to create a storage context:</span></span>

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    <span data-ttu-id="70324-220">További információkért lásd: [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) és [használatával megosztott hozzáférési aláírásokkal (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="70324-220">For more information, see [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) and [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span></span>

* <span data-ttu-id="70324-221">Bizonyos esetekben érdemes lehet a szolgáltatás végpontokat határoz meg az új adattároló-környezet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="70324-221">In some cases, you may want to specify the service endpoints when you create a new storage context.</span></span> <span data-ttu-id="70324-222">Erre akkor lehet szükség, ha egy egyéni tartománynevet a tárfiók a Blob szolgáltatásban regisztrált vagy a közös hozzáférésű jogosultságkód használandó tárolási erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="70324-222">This might be necessary when you have registered a custom domain name for your storage account with the Blob service or you want to use a shared access signature for accessing storage resources.</span></span> <span data-ttu-id="70324-223">Állítsa be a végpontok a kapcsolati karakterláncot, és segítségével hozzon létre egy új adattároló-környezet alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="70324-223">Set the service endpoints in a connection string and use it to create a new storage context as shown below:</span></span>

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

<span data-ttu-id="70324-224">A tárolási kapcsolati karakterlánc konfigurálásával kapcsolatos további információkért lásd: [kapcsolati karakterláncok konfigurálása](storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="70324-224">For more information on how to configure a storage connection string, see [Configuring Connection Strings](storage-configure-connection-string.md).</span></span>

<span data-ttu-id="70324-225">Most, hogy a számítógépet, és megtanulta, előfizetések és az Azure PowerShell storage-fiókok kezelése, ugorjon a következő szakaszban megtudhatja, hogyan kezelheti az Azure BLOB, és a blob-pillanatképek.</span><span class="sxs-lookup"><span data-stu-id="70324-225">Now that you have set up your computer and learned how to manage subscriptions and storage accounts using Azure PowerShell, go to the next section to learn how to manage Azure blobs and blob snapshots.</span></span>

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a><span data-ttu-id="70324-226">Hogyan kérhető le, és az Azure storage kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="70324-226">How to retrieve and regenerate Azure storage keys</span></span>
<span data-ttu-id="70324-227">Egy Azure Storage-fiók két kulcsait tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="70324-227">An Azure Storage account comes with two account keys.</span></span> <span data-ttu-id="70324-228">A következő parancsmag segítségével a kulcsok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="70324-228">You can use the following cmdlet to retrieve your keys.</span></span>

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

<span data-ttu-id="70324-229">A következő parancsmag segítségével egy adott kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="70324-229">Use the following cmdlet to retrieve a specific key.</span></span> <span data-ttu-id="70324-230">Érvényes értékek: az elsődleges és másodlagos.</span><span class="sxs-lookup"><span data-stu-id="70324-230">Valid values are Primary and Secondary.</span></span>  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

<span data-ttu-id="70324-231">Ha azt szeretné, újragenerálja a kulcsokat, használja a következő parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="70324-231">If you would like to regenerate your keys, use the following cmdlet.</span></span> <span data-ttu-id="70324-232">KeyType – az érvényes értékek: "Elsődleges" és "Másodlagos"</span><span class="sxs-lookup"><span data-stu-id="70324-232">Valid values for -KeyType are "Primary" and "Secondary"</span></span>

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-to-manage-azure-blobs"></a><span data-ttu-id="70324-233">Azure-blobokat kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-233">How to manage Azure blobs</span></span>
<span data-ttu-id="70324-234">Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a világon HTTP vagy HTTPS PROTOKOLLON keresztül tárolásához.</span><span class="sxs-lookup"><span data-stu-id="70324-234">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="70324-235">Ez a szakasz feltételezi, hogy Ön már ismeri az Azure Blob Storage szolgáltatással kapcsolatos fogalmak.</span><span class="sxs-lookup"><span data-stu-id="70324-235">This section assumes that you are already familiar with the Azure Blob Storage Service concepts.</span></span> <span data-ttu-id="70324-236">Részletes információkért lásd: [az Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-236">For detailed information, see [Get started with Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="how-to-create-a-container"></a><span data-ttu-id="70324-237">Egy tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-237">How to create a container</span></span>
<span data-ttu-id="70324-238">Az Azure storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="70324-238">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="70324-239">A személyes tárolók a New-AzureStorageContainer parancsmaggal hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="70324-239">You can create a private container using the New-AzureStorageContainer cmdlet:</span></span>

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> <span data-ttu-id="70324-240">A névtelen olvasási hozzáférés három szintje van: **ki**, **Blob**, és **tároló**.</span><span class="sxs-lookup"><span data-stu-id="70324-240">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="70324-241">Blobok névtelen hozzáférés érdekében a engedély paraméter értéke **ki**.</span><span class="sxs-lookup"><span data-stu-id="70324-241">To prevent anonymous access to blobs, set the Permission parameter to **Off**.</span></span> <span data-ttu-id="70324-242">Alapértelmezés szerint az új tároló privát, és csak a fiók tulajdonosa férhet.</span><span class="sxs-lookup"><span data-stu-id="70324-242">By default, the new container is private and can be accessed only by the account owner.</span></span> <span data-ttu-id="70324-243">A névtelen felhasználók engedélyezése nyilvános olvasási hozzáférés a blob-erőforrásokhoz, de nem csomagtároló metaadatai vagy a tárolóban lévő blobok listájának értékre az engedély paraméter **Blob**.</span><span class="sxs-lookup"><span data-stu-id="70324-243">To allow anonymous public read access to blob resources, but not to container metadata or to the list of blobs in the container, set the Permission parameter to **Blob**.</span></span> <span data-ttu-id="70324-244">A blob-erőforrások, tároló metaadatait és a tárolóban lévő blobok listájának teljes nyilvános olvasási hozzáférés engedélyezéséhez a engedély paraméter értéke **tároló**.</span><span class="sxs-lookup"><span data-stu-id="70324-244">To allow full public read access to blob resources, container metadata, and the list of blobs in the container, set the Permission parameter to **Container**.</span></span> <span data-ttu-id="70324-245">További információkért lásd: [tárolók és blobok névtelen olvasási hozzáférés kezelése](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="70324-245">For more information, see [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>
> 
> 

### <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="70324-246">Hogyan tölthetők fel blobok egy tárolóba</span><span class="sxs-lookup"><span data-stu-id="70324-246">How to upload a blob into a container</span></span>
<span data-ttu-id="70324-247">Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="70324-247">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="70324-248">További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-248">For more information, see [Understanding Block Blobs, Append BLobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="70324-249">A blobok feltöltése tárolót, használhatja a [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-249">To upload blobs in to a container, you can use the [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span></span> <span data-ttu-id="70324-250">Alapértelmezés szerint ez a parancs a helyi fájlok feltöltése blokkblobba.</span><span class="sxs-lookup"><span data-stu-id="70324-250">By default, this command uploads the local files to a block blob.</span></span> <span data-ttu-id="70324-251">Adja meg a BLOB, - BlobType paraméterét használhatja.</span><span class="sxs-lookup"><span data-stu-id="70324-251">To specify the type for the blob, you can use the -BlobType parameter.</span></span>

<span data-ttu-id="70324-252">A következő példában a [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) parancsmagot, hogy megkapja a megadott mappában lévő összes fájlt és majd továbbadja őket a következő parancsmag használatával a csővezeték-kezelőt.</span><span class="sxs-lookup"><span data-stu-id="70324-252">The following example runs the [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet to get all the files in the specified folder, and then passes them to the next cmdlet by using the pipeline operator.</span></span> <span data-ttu-id="70324-253">A [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag feltölti a helyi fájlok a tároló:</span><span class="sxs-lookup"><span data-stu-id="70324-253">The [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet uploads the local files to your container:</span></span>

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-to-download-blobs-from-a-container"></a><span data-ttu-id="70324-254">Blobok letöltése tárolója</span><span class="sxs-lookup"><span data-stu-id="70324-254">How to download blobs from a container</span></span>
<span data-ttu-id="70324-255">A következő példa bemutatja, hogyan töltheti le blobok egy tárolóba.</span><span class="sxs-lookup"><span data-stu-id="70324-255">The following example demonstrates how to download blobs from a container.</span></span> <span data-ttu-id="70324-256">A példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és az elsődleges elérési kulcsát kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="70324-256">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its primary access key.</span></span> <span data-ttu-id="70324-257">Majd, lekéri a példa egy blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-257">Then, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span></span> <span data-ttu-id="70324-258">Ezután a példában a [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) parancsmag használatával töltheti le a blobok a helyi cél mappába.</span><span class="sxs-lookup"><span data-stu-id="70324-258">Next, the example uses the [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet to download blobs into the local destination folder.</span></span>

```powershell
#Define the variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a><span data-ttu-id="70324-259">A tároló egy másik BLOB másolása</span><span class="sxs-lookup"><span data-stu-id="70324-259">How to copy blobs from one storage container to another</span></span>
<span data-ttu-id="70324-260">Átmásolhatja BLOB storage-fiókok és régiók közötti aszinkron módon.</span><span class="sxs-lookup"><span data-stu-id="70324-260">You can copy blobs across storage accounts and regions asynchronously.</span></span> <span data-ttu-id="70324-261">A következő példa bemutatja, hogyan BLOB másolása egy tároló között két különböző tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="70324-261">The following example demonstrates how to copy blobs from one storage container to another in two different storage accounts.</span></span> <span data-ttu-id="70324-262">A példa első beállítja a forrás és cél storage-fiókok változókat, és létrehoz egy adattároló-környezet az egyes fiókok számára.</span><span class="sxs-lookup"><span data-stu-id="70324-262">The example first sets variables for source and destination storage accounts, and then creates a storage context for each account.</span></span> <span data-ttu-id="70324-263">Ezután a példa másolja blobok a forrás-tárolójából. a cél tároló használata a [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-263">Next, the example copies blobs from the source container to the destination container using the [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span></span> <span data-ttu-id="70324-264">A példa feltételezi, hogy a forrás és cél tárfiók és tároló már létezik.</span><span class="sxs-lookup"><span data-stu-id="70324-264">The example assumes that the source and destination storage accounts and containers already exist.</span></span>

```powershell
#Define the source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define the destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference to blobs in the source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container to another.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

<span data-ttu-id="70324-265">Vegye figyelembe, hogy az ebben a példában egy aszinkron másolatot végez.</span><span class="sxs-lookup"><span data-stu-id="70324-265">Note that this example performs an asynchronous copy.</span></span> <span data-ttu-id="70324-266">Minden egyes példányra állapotának futtassa a figyelheti a [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-266">You can monitor the status of each copy by running the [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span></span>

### <a name="how-to-copy-blobs-from-a-secondary-location"></a><span data-ttu-id="70324-267">Egy másodlagos helyet BLOB másolása</span><span class="sxs-lookup"><span data-stu-id="70324-267">How to copy blobs from a secondary location</span></span>
<span data-ttu-id="70324-268">Blobok RA-GRS-engedélyezett fiók másodlagos helyre másolhatja.</span><span class="sxs-lookup"><span data-stu-id="70324-268">You can copy blobs from the secondary location of a RA-GRS-enabled account.</span></span>

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-to-delete-a-blob"></a><span data-ttu-id="70324-269">Egy blob törlése</span><span class="sxs-lookup"><span data-stu-id="70324-269">How to delete a blob</span></span>
<span data-ttu-id="70324-270">Blobok törléséhez először kérjen le egy blobhivatkozást, és majd hívja meg a Remove-AzureStorageBlob parancsmagot rajta.</span><span class="sxs-lookup"><span data-stu-id="70324-270">To delete a blob, first get a blob reference and then call the Remove-AzureStorageBlob cmdlet on it.</span></span> <span data-ttu-id="70324-271">A következő példa egy adott tároló összes blobjának törli.</span><span class="sxs-lookup"><span data-stu-id="70324-271">The following example deletes all the blobs in a given container.</span></span> <span data-ttu-id="70324-272">A példa első beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet.</span><span class="sxs-lookup"><span data-stu-id="70324-272">The example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="70324-273">A következő példában kéri le egy blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatja a [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) parancsmag segítségével távolítsa el a blobok az Azure storage-tárolójából.</span><span class="sxs-lookup"><span data-stu-id="70324-273">Next, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet to remove blobs from a container in Azure storage.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to all the blobs in the container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-to-manage-azure-blob-snapshots"></a><span data-ttu-id="70324-274">Az Azure blob pillanatfelvételek kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-274">How to manage Azure blob snapshots</span></span>
<span data-ttu-id="70324-275">Azure lehetővé teszi a pillanatkép létrehozása a blob.</span><span class="sxs-lookup"><span data-stu-id="70324-275">Azure lets you create a snapshot of a blob.</span></span> <span data-ttu-id="70324-276">Pillanatkép egy blobot, amely egy adott időben csak olvasható verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="70324-276">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="70324-277">Pillanatkép létrehozása, azt is kell olvasni, másolja, vagy törölni, de nem módosított.</span><span class="sxs-lookup"><span data-stu-id="70324-277">Once a snapshot has been created, it can be read, copied, or deleted, but not modified.</span></span> <span data-ttu-id="70324-278">A pillanatképek lehetőséget nyújtanak olyan biztonsági mentése blob, ahogyan megjelenik egy időben el.</span><span class="sxs-lookup"><span data-stu-id="70324-278">Snapshots provide a way to back up a blob as it appears at a moment in time.</span></span> <span data-ttu-id="70324-279">További információkért lásd: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-279">For more information, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span>

### <a name="how-to-create-a-blob-snapshot"></a><span data-ttu-id="70324-280">Egy blob pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-280">How to create a blob snapshot</span></span>
<span data-ttu-id="70324-281">Hozzon létre egy BLOB snaphot, először kérjen le egy blobhivatkozást, és majd hívja a `ICloudBlob.CreateSnapshot` metódust.</span><span class="sxs-lookup"><span data-stu-id="70324-281">To create a snaphot of a blob, first get a blob reference and then call the `ICloudBlob.CreateSnapshot` method on it.</span></span> <span data-ttu-id="70324-282">Az alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet.</span><span class="sxs-lookup"><span data-stu-id="70324-282">The following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="70324-283">A következő példában kéri le egy blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmagot, és futtatja a [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) módszer segítségével készít pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="70324-283">Next, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method to create a snapshot.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of the blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-to-list-a-blobs-snapshots"></a><span data-ttu-id="70324-284">Hogyan listázhat egy blob pillanatképek</span><span class="sxs-lookup"><span data-stu-id="70324-284">How to list a blob's snapshots</span></span>
<span data-ttu-id="70324-285">Egy BLOB kívánt annyi pillanatképeket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="70324-285">You can create as many snapshots as you want for a blob.</span></span> <span data-ttu-id="70324-286">A pillanatképek a blob nyomon követéséhez a jelenlegi pillanatképek társított felsorolása</span><span class="sxs-lookup"><span data-stu-id="70324-286">You can list the snapshots associated with your blob to track your current snapshots.</span></span> <span data-ttu-id="70324-287">Az alábbi példában egy előre meghatározott blob és a hívások a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) felsorolása található, hogy a blob-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="70324-287">The following example uses a predefined blob and calls the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet to list the snapshots of that blob.</span></span>  

```powershell
#Define the blob name.
$BlobName = "yourblobname"

#List the snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-to-copy-a-snapshot-of-a-blob"></a><span data-ttu-id="70324-288">Pillanatkép készítése a blob másolása</span><span class="sxs-lookup"><span data-stu-id="70324-288">How to copy a snapshot of a blob</span></span>
<span data-ttu-id="70324-289">Pillanatkép készítése a pillanatkép visszaállítása blob másolhatja.</span><span class="sxs-lookup"><span data-stu-id="70324-289">You can copy a snapshot of a blob to restore the snapshot.</span></span> <span data-ttu-id="70324-290">Részletes információk és korlátozások: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-290">For detailed information and restrictions, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span> <span data-ttu-id="70324-291">Az alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet.</span><span class="sxs-lookup"><span data-stu-id="70324-291">The following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="70324-292">A következő példában a tároló és a blob neve változók határozza meg.</span><span class="sxs-lookup"><span data-stu-id="70324-292">Next, the example defines the container and blob name variables.</span></span> <span data-ttu-id="70324-293">A példa lekéri a blob hivatkozás segítségével a [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmagot, és futtatja a [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) módszer segítségével készít pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="70324-293">The example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method to create a snapshot.</span></span> <span data-ttu-id="70324-294">A példa fut majd, a [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmagot, hogy a pillanatkép a ICloudBlob objektum használatával a forrás BLOB BLOB másolása.</span><span class="sxs-lookup"><span data-stu-id="70324-294">Then, the example runs the [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet to copy the snapshot of a blob using the ICloudBlob object for the source blob.</span></span> <span data-ttu-id="70324-295">Ne felejtse el frissíteni a változók a példa futtatása előtt a konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="70324-295">Be sure to update the variables based on your configuration before running the example.</span></span> <span data-ttu-id="70324-296">Vegye figyelembe, hogy az alábbi példa azt feltételezi, hogy a forrás és cél tárolókat, és a forrás blob már létezik.</span><span class="sxs-lookup"><span data-stu-id="70324-296">Note that the following example assumes that the source and destination containers, and the source blob already exist.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define the variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy the snapshot to another container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

<span data-ttu-id="70324-297">Most, hogy rendelkezik megtudta, hogyan kezelheti az Azure BLOB, és a blob-pillanatképeket az Azure PowerShell, ugorjon a következő szakaszban megtudhatja, hogyan kezelheti a táblák, üzenetsorok és fájlokat.</span><span class="sxs-lookup"><span data-stu-id="70324-297">Now that you have learned how to manage Azure blobs and blob snapshots with Azure PowerShell, go to the next section to learn how to manage tables, queues, and files.</span></span>

## <a name="how-to-manage-azure-tables-and-table-entities"></a><span data-ttu-id="70324-298">Azure-táblákban és táblaentitásokat kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-298">How to manage Azure tables and table entities</span></span>
<span data-ttu-id="70324-299">Az Azure Table storage szolgáltatás egy NoSQL-adattár, amely segítségével tárolja, és hatalmas strukturált, nem relációs adatok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="70324-299">Azure Table storage service is a NoSQL datastore, which you can use to store and query huge sets of structured, non-relational data.</span></span> <span data-ttu-id="70324-300">A szolgáltatás fő összetevőit táblák, entitásokat és tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="70324-300">The main components of the service are tables, entities, and properties.</span></span> <span data-ttu-id="70324-301">A tábla az entitások gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="70324-301">A table is a collection of entities.</span></span> <span data-ttu-id="70324-302">Egy entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="70324-302">An entity is a set of properties.</span></span> <span data-ttu-id="70324-303">Minden entitás legfeljebb 252 tulajdonságot, amely minden név-érték pár is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="70324-303">Each entity can have up to 252 properties, which are all name-value pairs.</span></span> <span data-ttu-id="70324-304">Ez a szakasz feltételezi, hogy Ön már ismeri az Azure Table Storage szolgáltatással kapcsolatos fogalmak.</span><span class="sxs-lookup"><span data-stu-id="70324-304">This section assumes that you are already familiar with the Azure Table Storage Service concepts.</span></span> <span data-ttu-id="70324-305">Részletes információkért lásd: [ismertetése a Table szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx) és [Ismerkedés az Azure Table storage használatának .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="70324-305">For detailed information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx) and [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="70324-306">A következő szakaszban akkor megtudhatja, hogyan kezelheti az Azure Table storage szolgáltatást Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="70324-306">In the following subsections, you'll learn how to manage Azure Table storage service using Azure PowerShell.</span></span> <span data-ttu-id="70324-307">Az ismertetett forgatókönyvek **létrehozása**, **törlése**, és **beolvasása** **táblák**, valamint **hozzáadása**, **lekérdezése**, és **tábla entitások törlése**.</span><span class="sxs-lookup"><span data-stu-id="70324-307">The scenarios covered include **creating**, **deleting**, and **retrieving** **tables**, as well as **adding**, **querying**, and **deleting table entities**.</span></span>

### <a name="how-to-create-a-table"></a><span data-ttu-id="70324-308">Táblázat létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-308">How to create a table</span></span>
<span data-ttu-id="70324-309">Minden táblának kell lennie az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="70324-309">Every table must reside in an Azure storage account.</span></span> <span data-ttu-id="70324-310">A következő példa bemutatja, hogyan hozzon létre egy táblát az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="70324-310">The following example demonstrates how to create a table in Azure Storage.</span></span> <span data-ttu-id="70324-311">A példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="70324-311">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="70324-312">Ezután használja a [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmaggal hozzon létre egy táblát az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="70324-312">Next, it uses the [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet to create a table in Azure Storage.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-retrieve-a-table"></a><span data-ttu-id="70324-313">Hogyan lehet lekérni egy tábla</span><span class="sxs-lookup"><span data-stu-id="70324-313">How to retrieve a table</span></span>
<span data-ttu-id="70324-314">Lekérdezi, és egy vagy az összes tábla tárfiókokban beolvasása.</span><span class="sxs-lookup"><span data-stu-id="70324-314">You can query and retrieve one or all tables in a Storage account.</span></span> <span data-ttu-id="70324-315">A következő példa bemutatja, hogyan lehet lekérni a megadott tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-315">The following example demonstrates how to retrieve a given table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span>

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

<span data-ttu-id="70324-316">Ha felhívja a Get-AzureStorageTable parancsmag paraméterek nélkül, egy tárfiók lekérdezi összes storage-táblákat.</span><span class="sxs-lookup"><span data-stu-id="70324-316">If you call the Get-AzureStorageTable cmdlet without any parameters, it gets all storage tables for a Storage account.</span></span>

### <a name="how-to-delete-a-table"></a><span data-ttu-id="70324-317">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="70324-317">How to delete a table</span></span>
<span data-ttu-id="70324-318">Törölheti a tábla a tárfiókból használatával a [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-318">You can delete a table from a storage account by using the [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span></span>  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-manage-table-entities"></a><span data-ttu-id="70324-319">Táblaentitásokat kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-319">How to manage table entities</span></span>
<span data-ttu-id="70324-320">Azure PowerShell jelenleg nem biztosít parancsmaggal táblaentitásokat közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="70324-320">Currently, Azure PowerShell does not provide cmdlets to manage table entities directly.</span></span> <span data-ttu-id="70324-321">A táblaentitásokat műveletek végrehajtásához használhatja a megadott osztályok a [Azure Storage ügyféloldali kódtára a .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-321">To perform operations on table entities, you can use the classes given in the [Azure Storage Client Library for .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span></span>

#### <a name="how-to-add-table-entities"></a><span data-ttu-id="70324-322">Táblaentitásokat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="70324-322">How to add table entities</span></span>
<span data-ttu-id="70324-323">Entitás hozzáadása egy táblát, először létre kell hoznia egy objektum, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="70324-323">To add an entity to a table, first create an object that defines your entity properties.</span></span> <span data-ttu-id="70324-324">Egy entitás legfeljebb 255 tulajdonságokat, beleértve a 3 rendszertulajdonsággal állhat: **PartitionKey**, **RowKey**, és **időbélyeg**.</span><span class="sxs-lookup"><span data-stu-id="70324-324">An entity can have up to 255 properties, including 3 system properties: **PartitionKey**, **RowKey**, and **Timestamp**.</span></span> <span data-ttu-id="70324-325">Ön felelőssége lesz beszúrva, és az értékek frissítése **PartitionKey** és **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="70324-325">You are responsible for inserting and updating the values of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="70324-326">A kiszolgáló kezeli értékének **időbélyeg**, amely nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="70324-326">The server manages the value of **Timestamp**, which cannot be modified.</span></span> <span data-ttu-id="70324-327">Együtt a **PartitionKey** és **RowKey** egyedi módon azonosítja az egy táblázaton belüli összes entitás.</span><span class="sxs-lookup"><span data-stu-id="70324-327">Together the **PartitionKey** and **RowKey** uniquely identify every entity within a table.</span></span>

* <span data-ttu-id="70324-328">**PartitionKey**: meghatározza a partíció az entitás tárolt.</span><span class="sxs-lookup"><span data-stu-id="70324-328">**PartitionKey**: Determines the partition that the entity is stored in.</span></span>
* <span data-ttu-id="70324-329">**RowKey**: egyedileg azonosítja az entitást a partíción belül.</span><span class="sxs-lookup"><span data-stu-id="70324-329">**RowKey**: Uniquely identifies the entity within the partition.</span></span>

<span data-ttu-id="70324-330">Egy entitás legfeljebb 252 egyéni tulajdonságok adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="70324-330">You may define up to 252 custom properties for an entity.</span></span> <span data-ttu-id="70324-331">További információkért lásd: [ismertetése a Table szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-331">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="70324-332">A következő példa bemutatja, hogyan entitások hozzáadása a táblához.</span><span class="sxs-lookup"><span data-stu-id="70324-332">The following example demonstrates how to add entities to a table.</span></span> <span data-ttu-id="70324-333">A példa bemutatja, hogyan kérhető le az alkalmazott tábla, és vegyen fel több entitás.</span><span class="sxs-lookup"><span data-stu-id="70324-333">The example shows how to retrieve the employee table and add several entities into it.</span></span> <span data-ttu-id="70324-334">Először az Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="70324-334">First, it establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="70324-335">Ezt követően lekérdezi a megadott tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-335">Next, it retrieves the given table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="70324-336">Ha a tábla nem létezik, a [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmag létrehoz egy táblát az Azure Storage szolgál.</span><span class="sxs-lookup"><span data-stu-id="70324-336">If the table does not exist, the [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet is used to create a table in Azure Storage.</span></span> <span data-ttu-id="70324-337">A példában a következő egyéni függvény Add-entitás entitások hozzáadása a tábla minden egyes entitás partíció- és sorkulcsa megadásával határozza meg.</span><span class="sxs-lookup"><span data-stu-id="70324-337">Next, the example defines a custom function Add-Entity to add entities to the table by specifying each entity's partition and row key.</span></span> <span data-ttu-id="70324-338">Az Add-entitás függvényhívásokat a [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) osztály entitásobjektumra létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="70324-338">The Add-Entity function calls the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on the [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) class to create an entity object.</span></span> <span data-ttu-id="70324-339">Később, a példában meghívja a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) metódust a forrásentitás-objektum hozzáadása a táblához.</span><span class="sxs-lookup"><span data-stu-id="70324-339">Later, the example calls the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) method on this entity object to add it to a table.</span></span>

```powershell
#Function Add-Entity: Adds an employee entity to a table.
function Add-Entity() {
    [CmdletBinding()]
    param(
       $table,
       [String]$partitionKey,
       [String]$rowKey,
       [String]$name,
       [Int]$id
    )

  $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
  $entity.Properties.Add("Name", $name)
  $entity.Properties.Add("ID", $id)

  $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
}

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve the table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities to a table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-to-query-table-entities"></a><span data-ttu-id="70324-340">Táblaentitásokat lekérdezése</span><span class="sxs-lookup"><span data-stu-id="70324-340">How to query table entities</span></span>
<span data-ttu-id="70324-341">Ha egy táblából, használja a [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="70324-341">To query a table, use the [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) class.</span></span> <span data-ttu-id="70324-342">Az alábbi példa azt feltételezi, hogy már futtatását a parancsfájl a megadott hozzáadása entitások című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="70324-342">The following example assumes that you've already run the script given in the How to add entities section of this guide.</span></span> <span data-ttu-id="70324-343">A példa első Azure Storage a tárolási adataival, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="70324-343">The example first establishes a connection to Azure Storage using the storage context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="70324-344">A következő megpróbálja beolvasni a korábban létrehozott "alkalmazottak" tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-344">Next, it tries to retrieve the previously created "Employees" table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="70324-345">Hívja a [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a Microsoft.WindowsAzure.Storage.Table.TableQuery osztály az új objektumot hoz létre lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="70324-345">Calling the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on the Microsoft.WindowsAzure.Storage.Table.TableQuery class creates a new query object.</span></span> <span data-ttu-id="70324-346">A példa egy "ID" oszlop, amelynek értéke 1 megadott egy karakterlánc-szűrővel rendelkező entitások keresi.</span><span class="sxs-lookup"><span data-stu-id="70324-346">The example looks for the entities that have an 'ID' column whose value is 1 as specified in a string filter.</span></span> <span data-ttu-id="70324-347">Részletes információkért lásd: [lekérdezése táblákat és entitásokat](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-347">For detailed information, see [Querying Tables and Entities](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span></span> <span data-ttu-id="70324-348">Ez a lekérdezés futtatásakor a szűrési feltételeknek megfelelő összes entitásokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="70324-348">When you run this query, it returns all entities that match the filter criteria.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference to a table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns to select.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute the query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with the table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-to-delete-table-entities"></a><span data-ttu-id="70324-349">Tábla entitás törlése</span><span class="sxs-lookup"><span data-stu-id="70324-349">How to delete table entities</span></span>
<span data-ttu-id="70324-350">A partíció- és sorfejlécek kulcsokkal entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="70324-350">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="70324-351">Az alábbi példa azt feltételezi, hogy már futtatását a parancsfájl a megadott hozzáadása entitások című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="70324-351">The following example assumes that you've already run the script given in the How to add entities section of this guide.</span></span> <span data-ttu-id="70324-352">A példa első Azure Storage a tárolási adataival, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="70324-352">The example first establishes a connection to Azure Storage using the storage context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="70324-353">A következő megpróbálja beolvasni a korábban létrehozott "alkalmazottak" tábla használatával a [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-353">Next, it tries to retrieve the previously created "Employees" table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="70324-354">Ha a tábla létezik, a példában meghívja a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metódusának segítéségével lekérheti az entitást a partíció- és sorfejlécek értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="70324-354">If the table exists, the example calls the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) method to retrieve an entity based on its partition and row key values.</span></span> <span data-ttu-id="70324-355">Így az az entitás továbbítsa a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metódus törlése.</span><span class="sxs-lookup"><span data-stu-id="70324-355">Then, pass the entity to the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) method to delete.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If the table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together the PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete the entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-to-manage-azure-queues-and-queue-messages"></a><span data-ttu-id="70324-356">Az Azure várólisták és üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-356">How to manage Azure queues and queue messages</span></span>
<span data-ttu-id="70324-357">Az Azure Queue Storage szolgáltatás üzenetek nagy számban történő tárolására szolgál, amelyek HTTP- vagy HTTPS-kapcsolattal, hitelesített hívásokon keresztül a világon bárhonnan elérhetők.</span><span class="sxs-lookup"><span data-stu-id="70324-357">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="70324-358">Ez a szakasz feltételezi, hogy Ön már ismeri az Azure Queue Storage szolgáltatás fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="70324-358">This section assumes that you are already familiar with the Azure Queue Storage Service concepts.</span></span> <span data-ttu-id="70324-359">Részletes információkért lásd: [Ismerkedés az Azure Queue storage használatának .NET](storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="70324-359">For detailed information, see [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md).</span></span>

<span data-ttu-id="70324-360">Ez a szakasz bemutatja, hogyan kezelheti az Azure Queue storage szolgáltatás Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="70324-360">This section will show you how to manage Azure Queue storage service using Azure PowerShell.</span></span> <span data-ttu-id="70324-361">Az ismertetett forgatókönyvek **beszúrása** és **törlése** üzenetek várólistára, valamint **létrehozása**, **törlése**, és **sorok beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="70324-361">The scenarios covered include **inserting** and **deleting** queue messages, as well as **creating**, **deleting**, and **retrieving queues**.</span></span>

### <a name="how-to-create-a-queue"></a><span data-ttu-id="70324-362">A várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-362">How to create a queue</span></span>
<span data-ttu-id="70324-363">Az alábbi példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="70324-363">The following example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="70324-364">Ezután meghívja [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) parancsmag segítségével hozzon létre egy "queuename" nevű várólistát.</span><span class="sxs-lookup"><span data-stu-id="70324-364">Next, it calls [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet to create a queue named 'queuename'.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

<span data-ttu-id="70324-365">Azure Queue szolgáltatás elnevezési konvencióira vonatkozó információkért lásd: [elnevezési üzenetsorok és metaadatok](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-365">For information on naming conventions for Azure Queue Service, see [Naming Queues and Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

### <a name="how-to-retrieve-a-queue"></a><span data-ttu-id="70324-366">Hogyan lehet lekérni egy várólistát</span><span class="sxs-lookup"><span data-stu-id="70324-366">How to retrieve a queue</span></span>
<span data-ttu-id="70324-367">Lekérdezi, és egy konkrét várólistába helyezi vagy a tárfiókban lévő összes várólistán listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="70324-367">You can query and retrieve a specific queue or a list of all the queues in a Storage account.</span></span> <span data-ttu-id="70324-368">A következő példa bemutatja, hogyan lehet lekérni a megadott várólista használatával a [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-368">The following example demonstrates how to retrieve a specified queue using the [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span></span>

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

<span data-ttu-id="70324-369">Ha meghívja a [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag-paraméterek nélkül, akkor a várólisták listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="70324-369">If you call the [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet without any parameters, it gets a list of all the queues.</span></span>

### <a name="how-to-delete-a-queue"></a><span data-ttu-id="70324-370">A várólista törlése</span><span class="sxs-lookup"><span data-stu-id="70324-370">How to delete a queue</span></span>
<span data-ttu-id="70324-371">Egy üzenetsor és a benne tárolt összes üzenet törléséhez hívja meg a Remove-AzureStorageQueue parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="70324-371">To delete a queue and all the messages contained in it, call the Remove-AzureStorageQueue cmdlet.</span></span> <span data-ttu-id="70324-372">A következő példa bemutatja, hogyan használja a Remove-AzureStorageQueue parancsmag egy megadott várólista törlése.</span><span class="sxs-lookup"><span data-stu-id="70324-372">The following example shows how to delete a specified queue using the Remove-AzureStorageQueue cmdlet.</span></span>

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="70324-373">Üzenet beszúrása egy várólistára</span><span class="sxs-lookup"><span data-stu-id="70324-373">How to insert a message into a queue</span></span>
<span data-ttu-id="70324-374">Üzenet beszúrása egy létező üzenetsorba, először létre kell hoznia egy új példányt a [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="70324-374">To insert a message into an existing queue, first create a new instance of the [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="70324-375">Ezután hívja meg az [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) módszert.</span><span class="sxs-lookup"><span data-stu-id="70324-375">Next, call the [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method.</span></span> <span data-ttu-id="70324-376">Egy CloudQueueMessage egy karakterláncból (UTF-8 formátumban), illetve egy bájttömböt hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="70324-376">A CloudQueueMessage can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="70324-377">A következő példa bemutatja, hogyan üzenet hozzáadása egy üzenetsort.</span><span class="sxs-lookup"><span data-stu-id="70324-377">The following example demonstrates how to add message to a queue.</span></span> <span data-ttu-id="70324-378">A példa első Azure Storage használata a tárfiók környezetét, amely tartalmazza a tárfiók nevét és a hozzáférési kulcsot kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="70324-378">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="70324-379">Ezt követően lekérdezi a megadott várólista használatával a [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70324-379">Next, it retrieves the specified queue using the [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span></span> <span data-ttu-id="70324-380">Ha a várólista létezik, a [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmag segítségével hozzon létre egy példányát a [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="70324-380">If the queue exists, the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet is used to create an instance of the [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="70324-381">Később, a példában meghívja a [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) az üzenet objektum egy várólista veheti fel a metódust.</span><span class="sxs-lookup"><span data-stu-id="70324-381">Later, the example calls the [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method on this message object to add it to a queue.</span></span> <span data-ttu-id="70324-382">Itt található kód, amely lekérdezi a várólistát, és szúrja be a "MessageInfo" üzenet:</span><span class="sxs-lookup"><span data-stu-id="70324-382">Here is code which retrieves a queue and inserts the message 'MessageInfo':</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If the queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of the CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message to the queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-to-de-queue-at-the-next-message"></a><span data-ttu-id="70324-383">Útmutató a következő üzenet kivétele az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="70324-383">How to de-queue at the next message</span></span>
<span data-ttu-id="70324-384">A kód két lépésben távolít el egy üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="70324-384">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="70324-385">A hívás esetén a [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metódus, a következő üzenetet kap a sorhoz.</span><span class="sxs-lookup"><span data-stu-id="70324-385">When you call the [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) method, you get the next message in a queue.</span></span> <span data-ttu-id="70324-386">A **GetMessage** módszerrel lekért üzenet láthatatlanná válik az adott üzenetsorban található üzeneteket olvasó többi kód számára.</span><span class="sxs-lookup"><span data-stu-id="70324-386">A message returned from **GetMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="70324-387">Szeretné távolítani az üzenetet az üzenetsorból, is hívja meg a [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="70324-387">To finish removing the message from the queue, you must also call the [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) method.</span></span> <span data-ttu-id="70324-388">Az üzenetek kétlépéses eltávolítása lehetővé teszi, hogy ha a kód hardver- vagy szoftverhiba miatt nem tud feldolgozni egy üzenetet, a kód egy másik példánya megkaphassa ugyanazt az üzenetet, és újra megpróbálkozhasson a feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="70324-388">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="70324-389">A kód a **DeleteMessage** módszert rögtön az üzenet feldolgozása után hívja meg.</span><span class="sxs-lookup"><span data-stu-id="70324-389">Your code calls **DeleteMessage** right after the message has been processed.</span></span>

```powershell
# Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get the message object from the queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete the message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-to-manage-azure-file-shares-and-files"></a><span data-ttu-id="70324-390">Az Azure fájlmegosztások és fájlok kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-390">How to manage Azure file shares and files</span></span>
<span data-ttu-id="70324-391">Az Azure File storage közös tárterületet biztosít számára a szabványos SMB protokollt használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="70324-391">Azure File storage offers shared storage for applications using the standard SMB protocol.</span></span> <span data-ttu-id="70324-392">Microsoft Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások hozzáférhetnek a fájladatok egy megosztáson található, a File storage API vagy az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="70324-392">Microsoft Azure virtual machines and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share via the File storage API or Azure PowerShell.</span></span>

<span data-ttu-id="70324-393">További információ az Azure File storage: [Ismerkedés az Azure File storage on Windows](storage-dotnet-how-to-use-files.md) és [File szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span><span class="sxs-lookup"><span data-stu-id="70324-393">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) and [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span></span>

## <a name="how-to-set-and-query-storage-analytics"></a><span data-ttu-id="70324-394">Beállítása és a lekérdezés tárolási analitika</span><span class="sxs-lookup"><span data-stu-id="70324-394">How to set and query storage analytics</span></span>
<span data-ttu-id="70324-395">Használhat [Azure Storage Analytics](storage-analytics.md) gyűjtéséhez az Azure storage-fiókok és a tárfiók küldött kérelmek naplózni.</span><span class="sxs-lookup"><span data-stu-id="70324-395">You can use [Azure Storage Analytics](storage-analytics.md) to collect metrics for your Azure storage accounts and log data about requests sent to your storage account.</span></span> <span data-ttu-id="70324-396">Storage mérőszámainak segítségével egy tárfiókot, és a tárolási naplózási diagnosztizálása és elhárítása a tárfiók állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="70324-396">You can use storage metrics to monitor the health of a storage account, and storage logging to diagnose and troubleshoot issues with your storage account.</span></span> <span data-ttu-id="70324-397">Az Azure portálon vagy a Windows PowerShell használatával, vagy programozott módon, a storage ügyféloldali kódtár figyelését be tudja állítani.</span><span class="sxs-lookup"><span data-stu-id="70324-397">You can configure monitoring using the Azure portal or Windows PowerShell, or programmatically using the storage client library.</span></span> <span data-ttu-id="70324-398">Tárolási naplózási kiszolgálóoldali történik, és lehetővé teszi a tárfiókban lévő sikeres és a sikertelen kérelmek adatainak rögzítését.</span><span class="sxs-lookup"><span data-stu-id="70324-398">Storage logging happens server-side and enables you to record details for both successful and failed requests in your storage account.</span></span> <span data-ttu-id="70324-399">Ezek a naplók lehetővé teszik a Részletek területen az olvasási, írási és törlési műveleteket a táblák, üzenetsorok, blobok, valamint a sikertelen kérelmek okait.</span><span class="sxs-lookup"><span data-stu-id="70324-399">These logs enable you to see details of read, write, and delete operations against your tables, queues, and blobs as well as the reasons for failed requests.</span></span>

<span data-ttu-id="70324-400">Engedélyezése és megtekintése a PowerShell használatával Storage Metrics data kapcsolatban [PowerShell-lel Storage mérőszámainak engedélyezése](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span><span class="sxs-lookup"><span data-stu-id="70324-400">To learn how to enable and view Storage Metrics data using PowerShell, see [How to enable Storage Metrics using PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span></span>

<span data-ttu-id="70324-401">Engedélyezése és a PowerShell használatával tároló-naplózás adatainak beolvasása további tudnivalókért lásd: [engedélyezése a tárolási-naplózás PowerShell-lel](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) és [keresése a naplózás tárolási naplóadatok](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span><span class="sxs-lookup"><span data-stu-id="70324-401">To learn how to enable and retrieve Storage Logging data using PowerShell, see [How to enable Storage Logging using PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) and [Finding your Storage Logging log data](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span></span>
<span data-ttu-id="70324-402">A Storage Metrics és a naplózás tárolási tárolási problémák elhárítása részletes információkért lásd: [figyelés felismerése és hibaelhárítása a Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="70324-402">For detailed information on using Storage Metrics and Storage Logging to troubleshoot storage issues, see [Monitoring, Diagnosing, and Troubleshooting Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span></span>

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a><span data-ttu-id="70324-403">Közös hozzáférésű Jogosultságkód (SAS) és a tárolt házirend kezelése</span><span class="sxs-lookup"><span data-stu-id="70324-403">How to manage Shared Access Signature (SAS) and Stored Access Policy</span></span>
<span data-ttu-id="70324-404">Közös hozzáférésű jogosultságkód bármely alkalmazás Azure-tárolót a biztonsági modell fontos részét képezik.</span><span class="sxs-lookup"><span data-stu-id="70324-404">Shared access signatures are an important part of the security model for any application using Azure Storage.</span></span> <span data-ttu-id="70324-405">A tárfiókhoz korlátozott engedélyekkel biztosítani az ügyfelek nem rendelkezhet a fiókkulcs hasznosak.</span><span class="sxs-lookup"><span data-stu-id="70324-405">They are useful for providing limited permissions to your storage account to clients that should not have the account key.</span></span> <span data-ttu-id="70324-406">Alapértelmezés szerint csak a tárfiók tulajdonosa elérhessék blobot, táblát és üzenetsort, hogy a fiókon belül.</span><span class="sxs-lookup"><span data-stu-id="70324-406">By default, only the owner of the storage account may access blobs, tables, and queues within that account.</span></span> <span data-ttu-id="70324-407">Ha a szolgáltatás vagy alkalmazás ezek erőforrásai elérhetővé más ügyfelek számára a hozzáférési kulcs megosztása nélkül, három lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="70324-407">If your service or application needs to make these resources available to other clients without sharing your access key, you have three options:</span></span>

* <span data-ttu-id="70324-408">Egy tároló engedélyeket a tároló és a blobok névtelen olvasási hozzáférés engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="70324-408">Set a container's permissions to permit anonymous read access to the container and its blobs.</span></span> <span data-ttu-id="70324-409">Ez nem engedélyezett táblák vagy olyan várólisták esetében.</span><span class="sxs-lookup"><span data-stu-id="70324-409">This is not allowed for tables or queues.</span></span>
* <span data-ttu-id="70324-410">Használjon egy közös hozzáférésű jogosultságkódot, hogy korlátozott hozzáférési jogosultsága ahhoz, tárolók, blobok, üzenetsorok és táblák biztosít egy adott időintervallumban.</span><span class="sxs-lookup"><span data-stu-id="70324-410">Use a shared access signature that grants restricted access rights to containers, blobs, queues, and tables for a specific time interval.</span></span>
* <span data-ttu-id="70324-411">A tárolt házirend segítségével szerezzen be egy közös hozzáférésű jogosultságkód egy tárolót vagy a benne található blobokat, egy sor vagy tábla szabályozhatják további szintet.</span><span class="sxs-lookup"><span data-stu-id="70324-411">Use a stored access policy to obtain an additional level of control over shared access signatures for a container or its blobs, for a queue, or for a table.</span></span> <span data-ttu-id="70324-412">A tárolt házirend lehetővé teszi a kezdő időpont, a lejárat időpontjának és az aláírás engedélyeinek módosítása, vagy visszavonni az azt követő ki van állítva.</span><span class="sxs-lookup"><span data-stu-id="70324-412">The stored access policy allows you to change the start time, expiry time, or permissions for a signature, or to revoke it after it has been issued.</span></span>

<span data-ttu-id="70324-413">A közös hozzáférésű jogosultságkód lehet két űrlap egyikében:</span><span class="sxs-lookup"><span data-stu-id="70324-413">A shared access signature can be in one of two forms:</span></span>

* <span data-ttu-id="70324-414">**Ad hoc biztonsági Társítások**: egy ad hoc SAS-a kezdési ideje, a lejárat időpontjának létrehozásakor, és a SAS engedélyeinek összes adhatók meg a SAS URI-t.</span><span class="sxs-lookup"><span data-stu-id="70324-414">**Ad hoc SAS**: When you create an ad hoc SAS, the start time, expiry time, and permissions for the SAS are all specified on the SAS URI.</span></span> <span data-ttu-id="70324-415">Az ilyen típusú SAS lehet létrehozni egy tárolót, a blob, tábla, vagy várólista, és nem visszavonható.</span><span class="sxs-lookup"><span data-stu-id="70324-415">This type of SAS may be created on a container, blob, table, or queue and it is non-revocable.</span></span>
* <span data-ttu-id="70324-416">**SAS tárolt hozzáférési házirenddel**: A tárolt házirend van definiálva az erőforrás-tárolónak egy blob-tároló, táblázat vagy várólista -, és legalább egy közös hozzáférésű jogosultságkód megkötéseit kezelésére használhatja.</span><span class="sxs-lookup"><span data-stu-id="70324-416">**SAS with stored access policy**: A stored access policy is defined on a resource container a blob container, table, or queue - and you can use it to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="70324-417">SAS-kód társítása a tárolt házirend, a SAS - a kezdési ideje, a lejárat időpontjának és a-vonatkozó engedélyeit a tárolt házirend korlátozásait örökölnek.</span><span class="sxs-lookup"><span data-stu-id="70324-417">When you associate a SAS with a stored access policy, the SAS inherits the constraints - the start time, expiry time, and permissions - defined for the stored access policy.</span></span> <span data-ttu-id="70324-418">Ez a biztonsági Társítások típus visszavonható.</span><span class="sxs-lookup"><span data-stu-id="70324-418">This type of SAS is revocable.</span></span>

<span data-ttu-id="70324-419">További információkért lásd: [használatával megosztott hozzáférési aláírásokkal (SAS)](storage-dotnet-shared-access-signature-part-1.md) és [tárolók és blobok névtelen olvasási hozzáférés kezelése](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="70324-419">For more information, see [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) and [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>

<span data-ttu-id="70324-420">A következő szakaszokban lévő, megtudhatja, hogyan Azure-táblákban egy megosztott hozzáférési aláírást token és tárolt hozzáférési házirend létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="70324-420">In the next sections, you will learn how to create a shared access signature token and stored access policy for Azure tables.</span></span> <span data-ttu-id="70324-421">Az Azure PowerShell hasonló parancsmagokat tárolók, blobok és várólisták is kínál.</span><span class="sxs-lookup"><span data-stu-id="70324-421">Azure PowerShell provides similar cmdlets for containers, blobs, and queues as well.</span></span> <span data-ttu-id="70324-422">Ez a szakasz a parancsfájlok futtatásához töltse le a [Azure PowerShell verziója 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="70324-422">To run the scripts in this section, download the [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) or later.</span></span>

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a><span data-ttu-id="70324-423">A csoportházirend-alapú közös hozzáférésű Jogosultságkód jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-423">How to create a policy-based Shared Access Signature token</span></span>
<span data-ttu-id="70324-424">A New-AzureStorageTableStoredAccessPolicy parancsmag segítségével hozzon létre egy új tárolt házirend.</span><span class="sxs-lookup"><span data-stu-id="70324-424">Use the New-AzureStorageTableStoredAccessPolicy cmdlet to create a new stored access policy.</span></span> <span data-ttu-id="70324-425">Majd, meghívják a [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmaggal hozzon létre egy új csoportházirend-alapú megosztott aláírás jogkivonatot egy Azure Storage tábla.</span><span class="sxs-lookup"><span data-stu-id="70324-425">Then, call the [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet to create a new policy-based shared access signature token for an Azure Storage table.</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-to-create-an-ad-hoc-non-revocable-shared-access-signature-token"></a><span data-ttu-id="70324-426">Az ad hoc (nem visszavonható) közös hozzáférésű Jogosultságkód-token létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-426">How to create an ad hoc (non-revocable) Shared Access Signature token</span></span>
<span data-ttu-id="70324-427">Használja a [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmag segítségével hozzon létre egy új alkalmi (nem visszavonható) közös hozzáférésű Jogosultságkód tokent egy Azure Storage-tábla a(z):</span><span class="sxs-lookup"><span data-stu-id="70324-427">Use the [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet to create a new ad hoc (non-revocable) Shared Access Signature token for an Azure Storage table:</span></span>

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-to-create-a-stored-access-policy"></a><span data-ttu-id="70324-428">A tárolt hozzáférési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="70324-428">How to create a stored access policy</span></span>
<span data-ttu-id="70324-429">A New-AzureStorageTableStoredAccessPolicy parancsmag segítségével hozzon létre egy új tárolt házirend egy Azure Storage táblához:</span><span class="sxs-lookup"><span data-stu-id="70324-429">Use the New-AzureStorageTableStoredAccessPolicy cmdlet to create a new stored access policy for an Azure Storage table:</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-to-update-a-stored-access-policy"></a><span data-ttu-id="70324-430">A tárolt házirend frissítése</span><span class="sxs-lookup"><span data-stu-id="70324-430">How to update a stored access policy</span></span>
<span data-ttu-id="70324-431">A Set-AzureStorageTableStoredAccessPolicy parancsmag segítségével egy meglévő tárolt hozzáférési házirendet egy Azure Storage-táblázat frissítése:</span><span class="sxs-lookup"><span data-stu-id="70324-431">Use the Set-AzureStorageTableStoredAccessPolicy cmdlet to update an existing stored access policy for an Azure Storage table:</span></span>

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-to-delete-a-stored-access-policy"></a><span data-ttu-id="70324-432">A tárolt házirend törlése</span><span class="sxs-lookup"><span data-stu-id="70324-432">How to delete a stored access policy</span></span>
<span data-ttu-id="70324-433">A Remove-AzureStorageTableStoredAccessPolicy parancsmag segítségével törölheti a tárolt házirend egy Azure Storage táblán:</span><span class="sxs-lookup"><span data-stu-id="70324-433">Use the Remove-AzureStorageTableStoredAccessPolicy cmdlet to delete a stored access policy on an Azure Storage table:</span></span>

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a><span data-ttu-id="70324-434">Azure Storage használata az Amerikai Egyesült Államok kormánya és Azure Kínában</span><span class="sxs-lookup"><span data-stu-id="70324-434">How to use Azure Storage for U.S. government and Azure China</span></span>
<span data-ttu-id="70324-435">Környezetet az Azure például egy Microsoft Azure-ban független telepítését [Azure Government az Amerikai Egyesült Államok kormánya](https://azure.microsoft.com/features/gov/), [globális Azure AzureCloud](https://portal.azure.com), és [AzureChinaCloud Azure Kínában a 21Vianet által működtetett](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="70324-435">An Azure environment is an independent deployment of Microsoft Azure, such as [Azure Government for U.S. government](https://azure.microsoft.com/features/gov/), [AzureCloud for global Azure](https://portal.azure.com), and [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span> <span data-ttu-id="70324-436">Új Azure-környezetek Egyesült kormányzati és Azure Kína telepítése.</span><span class="sxs-lookup"><span data-stu-id="70324-436">You can deploy new Azure environments for U.S government and Azure China.</span></span>

<span data-ttu-id="70324-437">AzureChinaCloud az Azure Storage használatához szüksége AzureChinaCloud társított adattároló-környezet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70324-437">To use Azure Storage with AzureChinaCloud, you need to create a storage context that is associated with AzureChinaCloud.</span></span> <span data-ttu-id="70324-438">Kövesse az alábbi lépéseket az első lépésekhez:</span><span class="sxs-lookup"><span data-stu-id="70324-438">Follow these steps to get you started:</span></span>

1. <span data-ttu-id="70324-439">Futtassa a [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag használatával ellenőrizheti az elérhető az Azure-környezetek:</span><span class="sxs-lookup"><span data-stu-id="70324-439">Run the [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet to see the available Azure environments:</span></span>
   
    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="70324-440">Egy Azure Kína fiók hozzáadása a Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="70324-440">Add an Azure China account to Windows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. <span data-ttu-id="70324-441">Adattároló-környezet AzureChinaCloud fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="70324-441">Create a storage context for an AzureChinaCloud account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

<span data-ttu-id="70324-442">Az Azure Storage használatához [USA Az Azure Government](https://azure.microsoft.com/features/gov/), érdemes egy új környezet, és ezután hozzon létre egy új adattároló-környezet ebben a környezetben:</span><span class="sxs-lookup"><span data-stu-id="70324-442">To use Azure Storage with [U.S. Azure Government](https://azure.microsoft.com/features/gov/), you should define a new environment and then create a new storage context with this environment:</span></span>

1. <span data-ttu-id="70324-443">Futtassa a [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag használatával ellenőrizheti az elérhető az Azure-környezetek:</span><span class="sxs-lookup"><span data-stu-id="70324-443">Run the [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet to see the available Azure environments:</span></span>

    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="70324-444">A Windows PowerShell Azure Amerikai Egyesült államokbeli kormányzati fiók hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="70324-444">Add an Azure US Government account to Windows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. <span data-ttu-id="70324-445">Adattároló-környezet AzureUSGovernment fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="70324-445">Create a storage context for an AzureUSGovernment account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
<span data-ttu-id="70324-446">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="70324-446">For more information, see:</span></span>

* <span data-ttu-id="70324-447">[A Microsoft Azure Government – útmutató fejlesztőknek](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="70324-447">[Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>
* [<span data-ttu-id="70324-448">Kínai szolgáltatás egy alkalmazás létrehozásakor különbségek áttekintése</span><span class="sxs-lookup"><span data-stu-id="70324-448">Overview of Differences When Creating an Application on China Service</span></span>](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a><span data-ttu-id="70324-449">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70324-449">Next Steps</span></span>
<span data-ttu-id="70324-450">Ez az útmutató az Azure PowerShell Azure Storage kezelése hogy megismerte.</span><span class="sxs-lookup"><span data-stu-id="70324-450">In this guide, you've learned how to manage Azure Storage with Azure PowerShell.</span></span> <span data-ttu-id="70324-451">Íme, néhány kapcsolódó cikkek és a velük kapcsolatos további források.</span><span class="sxs-lookup"><span data-stu-id="70324-451">Here are some related articles and resources for learning more about them.</span></span>

* [<span data-ttu-id="70324-452">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="70324-452">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="70324-453">Az Azure Storage PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="70324-453">Azure Storage PowerShell Cmdlets</span></span>](/powershell/module/azurerm.storage/#storage)
* [<span data-ttu-id="70324-454">A Windows PowerShell-referencia</span><span class="sxs-lookup"><span data-stu-id="70324-454">Windows PowerShell Reference</span></span>](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
