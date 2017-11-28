---
title: az Azure Storage Azure PowerShell aaaUsing |} Microsoft Docs
description: "Megtudhatja, hogyan toouse hello Azure Storage toocreate Azure PowerShell-parancsmagok és a storage-fiókok; kezelése blobok, táblák, üzenetsorok és fájlok; használata konfigurálja és tárolási analitika lekérdezése, és a közös hozzáférésű jogosultságkód létrehozása."
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
ms.openlocfilehash: 17d638e741911ceafb9777d5c2fce7bfe533e50c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a><span data-ttu-id="b91b3-103">Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)</span><span class="sxs-lookup"><span data-stu-id="b91b3-103">Using Azure PowerShell with Azure Storage</span></span>
## <a name="overview"></a><span data-ttu-id="b91b3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b91b3-104">Overview</span></span>
<span data-ttu-id="b91b3-105">Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure, a Windows PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="b91b3-105">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="b91b3-106">Ez egy feladatalapú parancshéj és parancsnyelv, amely kifejezetten rendszerfelügyeleti célra készült.</span><span class="sxs-lookup"><span data-stu-id="b91b3-106">It is a task-based command-line shell and scripting language designed especially for system administration.</span></span> <span data-ttu-id="b91b3-107">A PowerShell-lel könnyen szabályozhatja és hello felügyelete az Azure-szolgáltatások és alkalmazások automatizálása.</span><span class="sxs-lookup"><span data-stu-id="b91b3-107">With PowerShell, you can easily control and automate hello administration of your Azure services and applications.</span></span> <span data-ttu-id="b91b3-108">Például használhatja hello parancsmagok tooperform hello hello keresztül végzett feladatok azonos [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b91b3-108">For example, you can use hello cmdlets tooperform hello same tasks that you can perform through hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="b91b3-109">Ez az útmutató azt fogja feltárja hogyan toouse hello [Azure tárolási parancsmagok](/powershell/module/azurerm.storage/#storage) tooperform számos fejlesztését és a felügyeleti feladatot az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b91b3-109">In this guide, we'll explore how toouse hello [Azure Storage Cmdlets](/powershell/module/azurerm.storage/#storage) tooperform a variety of development and administration tasks with Azure Storage.</span></span>

<span data-ttu-id="b91b3-110">Ez az útmutató feltételezi, hogy rendelkezik tapasztalattal használatával [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) és [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-110">This guide assumes that you have prior experience using [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) and [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span></span> <span data-ttu-id="b91b3-111">hello az útmutató számos parancsfájlt toodemonstrate hello használata az Azure Storage PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b91b3-111">hello guide provides a number of scripts toodemonstrate hello usage of PowerShell with Azure Storage.</span></span> <span data-ttu-id="b91b3-112">Minden parancsprogram futtatása előtt a konfiguráció alapján hello parancsfájl-változókat frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="b91b3-112">You should update hello script variables based on your configuration before running each script.</span></span>

<span data-ttu-id="b91b3-113">hello első rész a jelen útmutató az Azure Storage és a PowerShell kiadványok biztosít.</span><span class="sxs-lookup"><span data-stu-id="b91b3-113">hello first section in this guide provides a quick glance at Azure Storage and PowerShell.</span></span> <span data-ttu-id="b91b3-114">Részletes információk és utasítások, indítsa el a hello [az Azure Storage Azure PowerShell használatára vonatkozó Előfeltételek](#prerequisites-for-using-azure-powershell-with-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="b91b3-114">For detailed information and instructions, start from hello [Prerequisites for using Azure PowerShell with Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span></span>

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a><span data-ttu-id="b91b3-115">Ismerkedés az Azure Storage és a PowerShell 5 perc</span><span class="sxs-lookup"><span data-stu-id="b91b3-115">Getting started with Azure Storage and PowerShell in 5 minutes</span></span>
<span data-ttu-id="b91b3-116">Ez a szakasz bemutatja, hogyan tooaccess Azure Storage PowerShell 5 perc múlva.</span><span class="sxs-lookup"><span data-stu-id="b91b3-116">This section shows you how tooaccess Azure Storage via PowerShell in 5 minutes.</span></span>

<span data-ttu-id="b91b3-117">**Új tooAzure:** a Microsoft Azure-előfizetés és az adott előfizetéshez tartozó Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="b91b3-117">**New tooAzure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="b91b3-118">Az Azure megvásárlási lehetőségeinek információkért lásd: [ingyenes](https://azure.microsoft.com/pricing/free-trial/), [beszerzési lehetőségek](https://azure.microsoft.com/pricing/purchase-options/), és [ajánlatok](https://azure.microsoft.com/pricing/member-offers/) (az MSDN, a Microsoft Partner Network, és a BizSpark és egyéb Microsoft programok tagjai).</span><span class="sxs-lookup"><span data-stu-id="b91b3-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="b91b3-119">Lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure-előfizetések további információt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="b91b3-120">**Miután létrehozta a Microsoft Azure-előfizetésre és -fiókra:**</span><span class="sxs-lookup"><span data-stu-id="b91b3-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="b91b3-121">Töltse le és telepítse legújabb hello [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span><span class="sxs-lookup"><span data-stu-id="b91b3-121">Download and install hello latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span></span>
2. <span data-ttu-id="b91b3-122">Kezdő Windows PowerShell integrált parancsfájlkezelés környezet (ISE): A helyi számítógépen, váltson toohello **Start** menü.</span><span class="sxs-lookup"><span data-stu-id="b91b3-122">Start Windows PowerShell Integrated Scripting Environment (ISE): In your local computer, go toohello **Start** menu.</span></span> <span data-ttu-id="b91b3-123">Típus **felügyeleti eszközök** toorun kattintson azt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-123">Type **Administrative Tools** and click toorun it.</span></span> <span data-ttu-id="b91b3-124">A hello **felügyeleti eszközök** ablak, kattintson a jobb gombbal **Windows PowerShell ISE**, kattintson a **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-124">In hello **Administrative Tools** window, right-click **Windows PowerShell ISE**, click **Run as Administrator**.</span></span>
3. <span data-ttu-id="b91b3-125">A **Windows PowerShell ISE**, kattintson a **fájl** > **új** toocreate egy új parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-125">In **Windows PowerShell ISE**, click **File** > **New** toocreate a new script file.</span></span>
4. <span data-ttu-id="b91b3-126">Most lesz ad egy egyszerű parancsprogram, amely bemutatja az alapvető PowerShell parancsok tooaccess Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b91b3-126">Now, we'll give you a simple script that shows basic PowerShell commands tooaccess Azure Storage.</span></span> <span data-ttu-id="b91b3-127">hello parancsfájl először kéri az Azure-fiók hitelesítő adatait tooadd az Azure-fiók toohello helyi PowerShell környezetben.</span><span class="sxs-lookup"><span data-stu-id="b91b3-127">hello script will first ask your Azure account credentials tooadd your Azure account toohello local PowerShell environment.</span></span> <span data-ttu-id="b91b3-128">Ezután hello parancsfájl hello alapértelmezett Azure-előfizetéssel, és hozzon létre egy új tárfiókot az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b91b3-128">Then, hello script will set hello default Azure subscription and create a new storage account in Azure.</span></span> <span data-ttu-id="b91b3-129">A következő hello parancsfájl hozzon létre egy új tárolót az új tárfiókot, és töltse fel a meglévő lemezkép fájl (blob) toothat tároló.</span><span class="sxs-lookup"><span data-stu-id="b91b3-129">Next, hello script will create a new container in this new storage account and upload an existing image file (blob) toothat container.</span></span> <span data-ttu-id="b91b3-130">Hello parancsfájl megjeleníti az adott tároló összes BLOB, miután egy új célkönyvtáron létrehozza a helyi számítógépen, és hello képfájl letöltése.</span><span class="sxs-lookup"><span data-stu-id="b91b3-130">After hello script lists all blobs in that container, it will create a new destination directory in your local computer and download hello image file.</span></span>
5. <span data-ttu-id="b91b3-131">A következő kódot a szakasz hello, válassza ki hello parancsfájl hello megjegyzések közötti **#begin** és **#end**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-131">In hello following code section, select hello script between hello remarks **#begin** and **#end**.</span></span> <span data-ttu-id="b91b3-132">Nyomja le a CTRL + C toocopy azt toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="b91b3-132">Press CTRL+C toocopy it toohello clipboard.</span></span>

    ```powershell
    # begin
    # Update with hello name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name tooyour new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name tooyour new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account toohello local PowerShell environment.
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
       
    # Download blobs from hello container:
    # Get a reference tooa list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create hello destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into hello local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. <span data-ttu-id="b91b3-133">A **Windows PowerShell ISE**, nyomja le a CTRL + V toocopy hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="b91b3-133">In **Windows PowerShell ISE**, press CTRL+V toocopy hello script.</span></span> <span data-ttu-id="b91b3-134">Kattintson a **fájl** > **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-134">Click **File** > **Save**.</span></span> <span data-ttu-id="b91b3-135">A hello **Mentés másként** párbeszédpanelen, a típusnév hello hello parancsfájl, például a "mystoragescript."</span><span class="sxs-lookup"><span data-stu-id="b91b3-135">In hello **Save As** dialog window, type hello name of hello script file, such as "mystoragescript."</span></span> <span data-ttu-id="b91b3-136">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b91b3-136">Click **Save**.</span></span>
7. <span data-ttu-id="b91b3-137">Most tooupdate hello parancsfájl-változókat a konfigurációs beállítások alapján van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b91b3-137">Now, you need tooupdate hello script variables based on your configuration settings.</span></span> <span data-ttu-id="b91b3-138">Frissítenie kell a hello **$SubscriptionName** változó a saját előfizetésével.</span><span class="sxs-lookup"><span data-stu-id="b91b3-138">You must update hello **$SubscriptionName** variable with your own subscription.</span></span> <span data-ttu-id="b91b3-139">Tartsa hello más változók a parancsfájl hello, vagy igény szerint frissítse azokat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-139">You can keep hello other variables as specified in hello script or update them as you wish.</span></span>
   
   * <span data-ttu-id="b91b3-140">**$SubscriptionName:** ezt a változót a saját előfizetés nevével kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="b91b3-140">**$SubscriptionName:** You must update this variable with your own subscription name.</span></span> <span data-ttu-id="b91b3-141">Kövesse a következő három módon toolocate hello nevét az előfizetésben hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="b91b3-141">Follow one of hello following three ways toolocate hello name of your subscription:</span></span>
     
    <span data-ttu-id="b91b3-142">a.</span><span class="sxs-lookup"><span data-stu-id="b91b3-142">a.</span></span> <span data-ttu-id="b91b3-143">A **Windows PowerShell ISE**, kattintson a **fájl** > **új** toocreate egy új parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-143">In **Windows PowerShell ISE**, click **File** > **New** toocreate a new script file.</span></span> <span data-ttu-id="b91b3-144">Másolás hello alábbi parancsfájl-toohello új parancsfájlt, és kattintson **Debug** > **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-144">Copy hello following script toohello new script file and click **Debug** > **Run**.</span></span> <span data-ttu-id="b91b3-145">hello következő parancsfájlt a rendszer először kérje meg az Azure-fiók hitelesítő adatait tooadd az Azure-fiók toohello helyi PowerShell környezet és ezután meg lehet jeleníteni hello előfizetéseket, amelyek csatlakoztatott toohello helyi PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="b91b3-145">hello following script will first ask your Azure account credentials tooadd your Azure account toohello local PowerShell environment and then show all hello subscriptions that are connected toohello local PowerShell session.</span></span> <span data-ttu-id="b91b3-146">Jegyezze fel az hello előfizetés hello neve, amelyet az toouse az oktatóanyag során:</span><span class="sxs-lookup"><span data-stu-id="b91b3-146">Take a note of hello name of hello subscription that you want toouse while following this tutorial:</span></span>
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    <span data-ttu-id="b91b3-147">b.</span><span class="sxs-lookup"><span data-stu-id="b91b3-147">b.</span></span> <span data-ttu-id="b91b3-148">toolocate, és másolja az előfizetés neve hello [Azure-portálon](https://portal.azure.com), a központ menü a bal oldali hello hello, kattintson a **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-148">toolocate and copy your subscription name in hello [Azure portal](https://portal.azure.com), in hello Hub menu on hello left, click **Subscriptions**.</span></span> <span data-ttu-id="b91b3-149">Másolja a megjeleníteni kívánt toouse hello parancsfájlok futtatása az útmutató során előfizetés hello neve.</span><span class="sxs-lookup"><span data-stu-id="b91b3-149">Copy hello name of subscription that you want toouse while running hello scripts in this guide.</span></span>
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    <span data-ttu-id="b91b3-151">c.</span><span class="sxs-lookup"><span data-stu-id="b91b3-151">c.</span></span> <span data-ttu-id="b91b3-152">toolocate, és másolja az előfizetés neve hello [klasszikus Azure portál](https://manage.windowsazure.com/), görgessen lefelé, és kattintson a **beállítások** a bal oldalán található hello portal hello.</span><span class="sxs-lookup"><span data-stu-id="b91b3-152">toolocate and copy your subscription name in hello [Azure Classic Portal](https://manage.windowsazure.com/), scroll down and click **Settings** on hello left side of hello portal.</span></span> <span data-ttu-id="b91b3-153">Kattintson a **előfizetések** toosee az előfizetések listája.</span><span class="sxs-lookup"><span data-stu-id="b91b3-153">Click **Subscriptions** toosee a list of your subscriptions.</span></span> <span data-ttu-id="b91b3-154">Másolja ki toouse az útmutatóban megadott hello parancsfájlok futtatása közben előfizetést hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b91b3-154">Copy hello name of subscription that you want toouse while running hello scripts given in this guide.</span></span>
     
     ![klasszikus Azure portál](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * <span data-ttu-id="b91b3-156">**$StorageAccountName:** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b91b3-156">**$StorageAccountName:** Use hello given name in hello script or enter a new name for your storage account.</span></span> <span data-ttu-id="b91b3-157">**Fontos:** hello tárfiókja nevére hello Azure egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b91b3-157">**Important:** hello name of hello storage account must be unique in Azure.</span></span> <span data-ttu-id="b91b3-158">Az kisbetűnek kell lennie, túl!</span><span class="sxs-lookup"><span data-stu-id="b91b3-158">It must be lowercase, too!</span></span>
   * <span data-ttu-id="b91b3-159">**$Location:** hello parancsfájlban használja a megadott "USA nyugati régiója" hello, vagy válasszon más Azure helyeken, például az USA keleti régiója, Észak-Európa, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="b91b3-159">**$Location:** Use hello given "West US" in hello script or choose other Azure locations, such as East US, North Europe, and so on.</span></span>
   * <span data-ttu-id="b91b3-160">**$ContainerName:** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tároló.</span><span class="sxs-lookup"><span data-stu-id="b91b3-160">**$ContainerName:** Use hello given name in hello script or enter a new name for your container.</span></span>
   * <span data-ttu-id="b91b3-161">**$ImageToUpload:** , mint a helyi számítógépen, adja meg egy elérési utat tooa kép: "C:\Images\HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="b91b3-161">**$ImageToUpload:** Enter a path tooa picture on your local computer, such as: "C:\Images\HelloWorld.png".</span></span>
   * <span data-ttu-id="b91b3-162">**$DestinationFolder:** adjon meg egy elérési utat tooa helyi könyvtár toostore fájlokat töltött le az Azure Storage, például: "C:\DownloadImages".</span><span class="sxs-lookup"><span data-stu-id="b91b3-162">**$DestinationFolder:** Enter a path tooa local directory toostore files downloaded from Azure Storage, such as: "C:\DownloadImages".</span></span>
8. <span data-ttu-id="b91b3-163">Miután frissítette a parancsfájl-változókat hello hello "mystoragescript.ps1" fájlban, kattintson a **fájl** > **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-163">After updating hello script variables in hello "mystoragescript.ps1" file, click **File** > **Save**.</span></span> <span data-ttu-id="b91b3-164">Kattintson a **Debug** > **futtatása** vagy nyomja le az ENTER **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="b91b3-164">Then, click **Debug** > **Run** or press **F5** toorun hello script.</span></span>  

<span data-ttu-id="b91b3-165">Hello parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött hello lemezképfájlt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-165">After hello script runs, you should have a local destination folder that includes hello downloaded image file.</span></span> <span data-ttu-id="b91b3-166">a következő képernyőkép hello látható egy példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="b91b3-166">hello following screenshot shows an example output:</span></span>

![Blobok letöltése](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> <span data-ttu-id="b91b3-168">hello "Bevezetés az Azure Storage és a PowerShell 5 percben" című szakaszt a megadott gyors bevezetés hogyan toouse az Azure Storage Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b91b3-168">hello "Getting started with Azure Storage and PowerShell in 5 minutes" section provided a quick introduction on how toouse Azure PowerShell with Azure Storage.</span></span> <span data-ttu-id="b91b3-169">Részletes információk és utasítások javasoljuk, a következő szakaszok tooread hello.</span><span class="sxs-lookup"><span data-stu-id="b91b3-169">For detailed information and instructions, we encourage you tooread hello following sections.</span></span>
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a><span data-ttu-id="b91b3-170">Az Azure Storage Azure PowerShell használatának előfeltételei</span><span class="sxs-lookup"><span data-stu-id="b91b3-170">Prerequisites for using Azure PowerShell with Azure Storage</span></span>
<span data-ttu-id="b91b3-171">Szüksége van egy Azure előfizetésre és -fiókra toorun hello PowerShell-parancsmagok ebben az útmutatóban megadott fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="b91b3-171">You need an Azure subscription and account toorun hello PowerShell cmdlets given in this guide, as described above.</span></span>

<span data-ttu-id="b91b3-172">Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure, a Windows PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="b91b3-172">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="b91b3-173">A telepítése és beállítása az Azure PowerShell információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b91b3-173">For information on installing and setting up Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="b91b3-174">Azt javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul az útmutató használata előtt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-174">We recommend that you download and install or upgrade toohello latest Azure PowerShell module before using this guide.</span></span>

<span data-ttu-id="b91b3-175">Hello szabványos Windows PowerShell konzol vagy a Windows PowerShell integrált parancsfájlkezelési környezet (ISE) hello hello parancsmagokat futtathat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-175">You can run hello cmdlets in hello standard Windows PowerShell console or hello Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="b91b3-176">Például tooopen **Windows PowerShell ISE**nyissa meg toohello Start menüben, írja be a felügyeleti eszközök és toorun kattintson azt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-176">For example, tooopen **Windows PowerShell ISE**, go toohello Start menu, type Administrative Tools and click toorun it.</span></span> <span data-ttu-id="b91b3-177">Hello felügyeleti eszközök ablakban kattintson a jobb gombbal a Windows PowerShell ISE, kattintson a Futtatás rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b91b3-177">In hello Administrative Tools window, right-click Windows PowerShell ISE, click Run as Administrator.</span></span>

## <a name="how-toomanage-storage-accounts-in-azure"></a><span data-ttu-id="b91b3-178">Hogyan toomanage tárfiókot az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b91b3-178">How toomanage storage accounts in Azure</span></span>

<span data-ttu-id="b91b3-179">Vessen egy pillantást a PowerShell-lel Azure storage-fiók kezelése</span><span class="sxs-lookup"><span data-stu-id="b91b3-179">Let's take a look at managing storage accounts in Azure with PowerShell</span></span>

### <a name="how-tooset-a-default-azure-subscription"></a><span data-ttu-id="b91b3-180">Hogyan tooset egy alapértelmezett Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="b91b3-180">How tooset a default Azure subscription</span></span>
<span data-ttu-id="b91b3-181">Azure Storage Azure PowerShell használatával, kell tooauthenticate az ügyfél-környezet Azure az Azure Active Directory-hitelesítés vagy Tanúsítványalapú hitelesítés használatával toomanage.</span><span class="sxs-lookup"><span data-stu-id="b91b3-181">toomanage Azure Storage using Azure PowerShell, you need tooauthenticate your client environment with Azure via Azure Active Directory authentication or certificate-based authentication.</span></span> <span data-ttu-id="b91b3-182">Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-182">For detailed information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tutorial.</span></span> <span data-ttu-id="b91b3-183">Ez az útmutató hello Azure Active Directory-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="b91b3-183">This guide uses hello Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="b91b3-184">A Windows PowerShell ISE írja be a következő parancs tooadd hello az Azure-fiók toohello helyi PowerShell környezetben:</span><span class="sxs-lookup"><span data-stu-id="b91b3-184">In Windows PowerShell ISE, type hello following command tooadd your Azure account toohello local PowerShell environment:</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="b91b3-185">Hello "tooMicrosoft Azure bejelentkezés", típus hello e-mail címét és a fiókhoz társított jelszót.</span><span class="sxs-lookup"><span data-stu-id="b91b3-185">In hello "Sign in tooMicrosoft Azure" window, type hello email address and password associated with your account.</span></span> <span data-ttu-id="b91b3-186">Azure hitelesíti és menti a hello hitelesítő adatokat, és majd a hello ablak bezárása.</span><span class="sxs-lookup"><span data-stu-id="b91b3-186">Azure authenticates and saves hello credential information, and then closes hello window.</span></span>

3. <span data-ttu-id="b91b3-187">Ezután futtassa a következő parancs tooview hello Azure fiókok helyi PowerShell-környezetében, és ellenőrizze, hogy szerepel-e a fiók hello:</span><span class="sxs-lookup"><span data-stu-id="b91b3-187">Next, run hello following command tooview hello Azure accounts in your local PowerShell environment, and verify that your account is listed:</span></span>
   
    ```powershell
    Get-AzureAccount
    ```
4. <span data-ttu-id="b91b3-188">Ezután futtassa a következő parancsmag tooview hello hello előfizetéseket, amelyek csatlakoztatott toohello helyi PowerShell-munkamenetet, és ellenőrizze, hogy szerepel-e az előfizetés:</span><span class="sxs-lookup"><span data-stu-id="b91b3-188">Then, run hello following cmdlet tooview all hello subscriptions that are connected toohello local PowerShell session, and verify that your subscription is listed:</span></span>

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. <span data-ttu-id="b91b3-189">Azure-előfizetésre, az alapértelmezett tooset hello válassza-AzureSubscription parancsmag futtatásához:</span><span class="sxs-lookup"><span data-stu-id="b91b3-189">tooset a default Azure subscription, run hello Select-AzureSubscription cmdlet:</span></span>

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. <span data-ttu-id="b91b3-190">Ellenőrizze a hello alapértelmezett előfizetés neve hello hello Get-AzureSubscription parancsmag futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b91b3-190">Verify hello name of hello default subscription by running hello Get-AzureSubscription cmdlet:</span></span>

    ```powershell
    Get-AzureSubscription -Default
    ```

7. <span data-ttu-id="b91b3-191">Futtassa az toosee összes hello elérhető PowerShell-parancsmagok az Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="b91b3-191">toosee all hello available PowerShell cmdlets for Azure Storage, run:</span></span>
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-toocreate-a-new-azure-storage-account"></a><span data-ttu-id="b91b3-192">Hogyan toocreate egy új Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="b91b3-192">How toocreate a new Azure storage account</span></span>
<span data-ttu-id="b91b3-193">az Azure storage toouse, szüksége lesz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b91b3-193">toouse Azure storage, you will need a storage account.</span></span> <span data-ttu-id="b91b3-194">Létrehozhat egy új Azure-tárfiókot, a számítógép tooconnect tooyour előfizetés konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="b91b3-194">You can create a new Azure storage account after you have configured your computer tooconnect tooyour subscription.</span></span>

1. <span data-ttu-id="b91b3-195">Futtassa a hello Get-AzureLocation parancsmag toofind összes hello elérhető adatközpont-helyeinek:</span><span class="sxs-lookup"><span data-stu-id="b91b3-195">Run hello Get-AzureLocation cmdlet toofind all hello available datacenter locations:</span></span>

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. <span data-ttu-id="b91b3-196">Ezt követően futtassa hello New-AzureStorageAccount parancsmag toocreate egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b91b3-196">Next, run hello New-AzureStorageAccount cmdlet toocreate a new storage account.</span></span> <span data-ttu-id="b91b3-197">hello alábbi példa létrehoz egy új tárfiókot hello "USA nyugati régiója" adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="b91b3-197">hello following example creates a new storage account in hello "West US" datacenter.</span></span>
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> <span data-ttu-id="b91b3-198">a tárfiók nevére hello Azure belül egyedieknek kell lenniük, és kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b91b3-198">hello name of your storage account must be unique within Azure and must be lowercase.</span></span> <span data-ttu-id="b91b3-199">Az elnevezési konvenciókat és korlátozások, lásd: [kapcsolatos Azure Storage-fiókok](../storage-create-storage-account.md) és [elnevezési és hivatkozó tárolók, Blobok és metaadatok](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-199">For naming conventions and restrictions, see [About Azure Storage Accounts](../storage-create-storage-account.md) and [Naming and Referencing Containers, Blobs, and Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span></span>
> 
> 

### <a name="how-tooset-a-default-azure-storage-account"></a><span data-ttu-id="b91b3-200">Hogyan tooset egy alapértelmezett Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="b91b3-200">How tooset a default Azure storage account</span></span>
<span data-ttu-id="b91b3-201">Az előfizetés több tárfiókot is lehet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-201">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="b91b3-202">Válassza ki az egyiket, és állítsa be úgy, mint hello alapértelmezett tárfiókot az összes tárolási hello parancsai hello azonos PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="b91b3-202">You can choose one of them and set it as hello default storage account for all hello storage commands in hello same PowerShell session.</span></span> <span data-ttu-id="b91b3-203">Ez lehetővé teszi toorun hello Azure PowerShell tárolási parancsok hello adattároló-környezet explicit megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b91b3-203">This enables you toorun hello Azure PowerShell storage commands without specifying hello storage context explicitly.</span></span>

1. <span data-ttu-id="b91b3-204">egy alapértelmezett tárfiókot az előfizetéshez tartozó tooset, hello Set-AzureSubscription parancsmag is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b91b3-204">tooset a default storage account for your subscription, you can run hello Set-AzureSubscription cmdlet.</span></span>

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. <span data-ttu-id="b91b3-205">Ezután futtassa a Get-AzureSubscription hello parancsmag tooverify, hogy hello tárfiók kapcsolódik az alapértelmezett előfizetéses fiókba.</span><span class="sxs-lookup"><span data-stu-id="b91b3-205">Next, run hello Get-AzureSubscription cmdlet tooverify that hello storage account is associated with your default subscription account.</span></span> <span data-ttu-id="b91b3-206">Ez a parancs visszaadja a hello előfizetés tulajdonságai hello az aktuális előfizetésben többek között az aktuális tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b91b3-206">This command returns hello subscription properties on hello current subscription including its current storage account.</span></span>

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-toolist-all-azure-storage-accounts-in-a-subscription"></a><span data-ttu-id="b91b3-207">Hogyan toolist minden Azure tárfiókok egy előfizetésben található</span><span class="sxs-lookup"><span data-stu-id="b91b3-207">How toolist all Azure storage accounts in a subscription</span></span>
<span data-ttu-id="b91b3-208">Minden Azure-előfizetés mentése too100 tárfiókok rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-208">Each Azure subscription can have up too100 storage accounts.</span></span> <span data-ttu-id="b91b3-209">Hello legfrissebb információk korlátozások: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-209">For hello most up-to-date information on limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="b91b3-210">Futtassa a következő parancsmag toofind hello nevét és hello aktuális előfizetés tárfiókjai hello állapotának hello:</span><span class="sxs-lookup"><span data-stu-id="b91b3-210">Run hello following cmdlet toofind out hello name and status of hello storage accounts in hello current subscription:</span></span>

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-toocreate-an-azure-storage-context"></a><span data-ttu-id="b91b3-211">Hogyan toocreate egy Azure storage-környezetben</span><span class="sxs-lookup"><span data-stu-id="b91b3-211">How toocreate an Azure storage context</span></span>
<span data-ttu-id="b91b3-212">Az Azure storage-környezet egy objektum PowerShell tooencapsulate hello tároló hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b91b3-212">Azure storage context is an object in PowerShell tooencapsulate hello storage credentials.</span></span> <span data-ttu-id="b91b3-213">Tárolási környezetet használ a következő parancsmag futtatása közben lehetővé teszi, hogy Ön tooauthenticate kérelmét hello tárfiók és a hozzáférési kulcs explicit megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b91b3-213">Using a storage context while running any subsequent cmdlet enables you tooauthenticate your request without specifying hello storage account and its access key explicitly.</span></span> <span data-ttu-id="b91b3-214">Adattároló-környezet az sokféleképpen, például a tárolási fiók nevét és hívóbetűjét, a közös hozzáférésű jogosultságkód (SAS) jogkivonatot, a kapcsolati karakterlánc használatával hozhat létre vagy névtelen.</span><span class="sxs-lookup"><span data-stu-id="b91b3-214">You can create a storage context in many ways, such as using storage account name and access key, shared access signature (SAS) token, connection string, or anonymous.</span></span> <span data-ttu-id="b91b3-215">További információkért lásd: [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span><span class="sxs-lookup"><span data-stu-id="b91b3-215">For more information, see [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span></span>  

<span data-ttu-id="b91b3-216">Adattároló-környezet használja a következő három módon toocreate hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="b91b3-216">Use one of hello following three ways toocreate a storage context:</span></span>

* <span data-ttu-id="b91b3-217">Futtassa a hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) parancsmag toofind kimenő hello elsődleges tárelérési kulcs az Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b91b3-217">Run hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet toofind out hello primary storage access key for your Azure storage account.</span></span> <span data-ttu-id="b91b3-218">Ezután hívja hello [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) parancsmag toocreate adattároló-környezet:</span><span class="sxs-lookup"><span data-stu-id="b91b3-218">Next, call hello [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet toocreate a storage context:</span></span>

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* <span data-ttu-id="b91b3-219">Az Azure storage-tároló megosztott hozzáférési aláírást jogkivonat előállítása és toocreate adattároló-környezet használhatja:</span><span class="sxs-lookup"><span data-stu-id="b91b3-219">Generate a shared access signature token for an Azure storage container and use it toocreate a storage context:</span></span>

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    <span data-ttu-id="b91b3-220">További információkért lásd: [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) és [használatával megosztott hozzáférési aláírásokkal (SAS)](../storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-220">For more information, see [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) and [Using Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md).</span></span>

* <span data-ttu-id="b91b3-221">Bizonyos esetekben érdemes lehet toospecify hello Szolgáltatásvégpontok egy új adattároló-környezet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b91b3-221">In some cases, you may want toospecify hello service endpoints when you create a new storage context.</span></span> <span data-ttu-id="b91b3-222">Erre akkor lehet szükség, ha egy egyéni tartománynevet a tárfiók hello Blob szolgáltatásban regisztrált vagy a közös hozzáférésű jogosultságkód toouse kívánt tárolási erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b91b3-222">This might be necessary when you have registered a custom domain name for your storage account with hello Blob service or you want toouse a shared access signature for accessing storage resources.</span></span> <span data-ttu-id="b91b3-223">Hello szolgáltatás végpontokat a kapcsolódási karakterláncban és az új adattároló-környezet toocreate alább látható:</span><span class="sxs-lookup"><span data-stu-id="b91b3-223">Set hello service endpoints in a connection string and use it toocreate a new storage context as shown below:</span></span>

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

<span data-ttu-id="b91b3-224">További információt a tooconfigure egy tárolási kapcsolati karakterlánc, lásd: [kapcsolati karakterláncok konfigurálása](../storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-224">For more information on how tooconfigure a storage connection string, see [Configuring Connection Strings](../storage-configure-connection-string.md).</span></span>

<span data-ttu-id="b91b3-225">Most, hogy a számítógépet, és megtanulta, hogyan toomanage előfizetések és a storage-fiókok az Azure PowerShell, lépjen a toohello hogyan toomanage Azure blobok következő szakasz toolearn és blob-pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="b91b3-225">Now that you have set up your computer and learned how toomanage subscriptions and storage accounts using Azure PowerShell, go toohello next section toolearn how toomanage Azure blobs and blob snapshots.</span></span>

### <a name="how-tooretrieve-and-regenerate-azure-storage-keys"></a><span data-ttu-id="b91b3-226">Hogyan tooretrieve és újragenerálása az Azure storage-kulcsok</span><span class="sxs-lookup"><span data-stu-id="b91b3-226">How tooretrieve and regenerate Azure storage keys</span></span>
<span data-ttu-id="b91b3-227">Egy Azure Storage-fiók két kulcsait tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b91b3-227">An Azure Storage account comes with two account keys.</span></span> <span data-ttu-id="b91b3-228">A következő parancsmag tooretrieve hello használhatja a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-228">You can use hello following cmdlet tooretrieve your keys.</span></span>

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

<span data-ttu-id="b91b3-229">A következő parancsmag tooretrieve egy adott kulcs hello használata.</span><span class="sxs-lookup"><span data-stu-id="b91b3-229">Use hello following cmdlet tooretrieve a specific key.</span></span> <span data-ttu-id="b91b3-230">Érvényes értékek: az elsődleges és másodlagos.</span><span class="sxs-lookup"><span data-stu-id="b91b3-230">Valid values are Primary and Secondary.</span></span>  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

<span data-ttu-id="b91b3-231">Ha azt szeretné, hogy tooregenerate a kulcs, használja a következő parancsmag hello.</span><span class="sxs-lookup"><span data-stu-id="b91b3-231">If you would like tooregenerate your keys, use hello following cmdlet.</span></span> <span data-ttu-id="b91b3-232">KeyType – az érvényes értékek: "Elsődleges" és "Másodlagos"</span><span class="sxs-lookup"><span data-stu-id="b91b3-232">Valid values for -KeyType are "Primary" and "Secondary"</span></span>

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-toomanage-azure-blobs"></a><span data-ttu-id="b91b3-233">Hogyan toomanage Azure blobok</span><span class="sxs-lookup"><span data-stu-id="b91b3-233">How toomanage Azure blobs</span></span>
<span data-ttu-id="b91b3-234">Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.</span><span class="sxs-lookup"><span data-stu-id="b91b3-234">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="b91b3-235">Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Blob Storage szolgáltatással kapcsolatos fogalmak.</span><span class="sxs-lookup"><span data-stu-id="b91b3-235">This section assumes that you are already familiar with hello Azure Blob Storage Service concepts.</span></span> <span data-ttu-id="b91b3-236">Részletes információkért lásd: [az Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-236">For detailed information, see [Get started with Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="how-toocreate-a-container"></a><span data-ttu-id="b91b3-237">Hogyan toocreate tárolója</span><span class="sxs-lookup"><span data-stu-id="b91b3-237">How toocreate a container</span></span>
<span data-ttu-id="b91b3-238">Az Azure storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b91b3-238">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="b91b3-239">Egy személyes tárolót hello New-AzureStorageContainer parancsmaggal hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="b91b3-239">You can create a private container using hello New-AzureStorageContainer cmdlet:</span></span>

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> <span data-ttu-id="b91b3-240">A névtelen olvasási hozzáférés három szintje van: **ki**, **Blob**, és **tároló**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-240">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="b91b3-241">tooprevent névtelen hozzáférés tooblobs, set hello engedély paraméter túl**ki**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-241">tooprevent anonymous access tooblobs, set hello Permission parameter too**Off**.</span></span> <span data-ttu-id="b91b3-242">Alapértelmezés szerint a hello új tároló privát, és csak a fiók tulajdonosának hello keresztül elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="b91b3-242">By default, hello new container is private and can be accessed only by hello account owner.</span></span> <span data-ttu-id="b91b3-243">tooallow névtelen nyilvános olvasási hozzáférés tooblob erőforrásokat, de nem toocontainer metaadatok vagy toohello listája hello tárolóban lévő blobok, hello engedély paraméter értéke túl**Blob**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-243">tooallow anonymous public read access tooblob resources, but not toocontainer metadata or toohello list of blobs in hello container, set hello Permission parameter too**Blob**.</span></span> <span data-ttu-id="b91b3-244">tooallow teljes nyilvános olvasási tooblob erőforrások eléréséhez, a tároló metaadatait, és a hello tárolóban lévő blobok hello listája, hello engedély paraméter értéke túl**tároló**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-244">tooallow full public read access tooblob resources, container metadata, and hello list of blobs in hello container, set hello Permission parameter too**Container**.</span></span> <span data-ttu-id="b91b3-245">További információkért lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-245">For more information, see [Manage anonymous read access toocontainers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>
> 
> 

### <a name="how-tooupload-a-blob-into-a-container"></a><span data-ttu-id="b91b3-246">Hogyan tooupload blob egy tárolóba</span><span class="sxs-lookup"><span data-stu-id="b91b3-246">How tooupload a blob into a container</span></span>
<span data-ttu-id="b91b3-247">Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-247">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="b91b3-248">További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-248">For more information, see [Understanding Block Blobs, Append BLobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="b91b3-249">tooa tárolóban lévő blobok tooupload, hello használható [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-249">tooupload blobs in tooa container, you can use hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span></span> <span data-ttu-id="b91b3-250">Alapértelmezés szerint ez a parancs hello helyi fájlok tooa blokkblob feltöltését.</span><span class="sxs-lookup"><span data-stu-id="b91b3-250">By default, this command uploads hello local files tooa block blob.</span></span> <span data-ttu-id="b91b3-251">toospecify hello típus hello BLOB, hello - BlobType paraméterrel is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b91b3-251">toospecify hello type for hello blob, you can use hello -BlobType parameter.</span></span>

<span data-ttu-id="b91b3-252">hello alábbi példa fut hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) parancsmag tooget összes hello hello megadott mappában található fájlokat, és majd azokat továbbítja toohello a következő parancsmag használatával hello csővezeték-kezelőt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-252">hello following example runs hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet tooget all hello files in hello specified folder, and then passes them toohello next cmdlet by using hello pipeline operator.</span></span> <span data-ttu-id="b91b3-253">Hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) parancsmag feltölt hello helyi fájlok tooyour tároló:</span><span class="sxs-lookup"><span data-stu-id="b91b3-253">hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet uploads hello local files tooyour container:</span></span>

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-toodownload-blobs-from-a-container"></a><span data-ttu-id="b91b3-254">Hogyan toodownload blobok egy tárolójából.</span><span class="sxs-lookup"><span data-stu-id="b91b3-254">How toodownload blobs from a container</span></span>
<span data-ttu-id="b91b3-255">hello a következő példa bemutatja, hogyan toodownload blobok a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="b91b3-255">hello following example demonstrates how toodownload blobs from a container.</span></span> <span data-ttu-id="b91b3-256">hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, ide tartozik az hello tárfiók neve és az elsődleges elérési kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="b91b3-256">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its primary access key.</span></span> <span data-ttu-id="b91b3-257">Ezt követően hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-257">Then, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span></span> <span data-ttu-id="b91b3-258">Ezután a hello példában hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) parancsmag toodownload blobok hello helyi cél mappába.</span><span class="sxs-lookup"><span data-stu-id="b91b3-258">Next, hello example uses hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet toodownload blobs into hello local destination folder.</span></span>

```powershell
#Define hello variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-toocopy-blobs-from-one-storage-container-tooanother"></a><span data-ttu-id="b91b3-259">Hogyan toocopy egy tárolási tároló tooanother a blobok</span><span class="sxs-lookup"><span data-stu-id="b91b3-259">How toocopy blobs from one storage container tooanother</span></span>
<span data-ttu-id="b91b3-260">Átmásolhatja BLOB storage-fiókok és régiók közötti aszinkron módon.</span><span class="sxs-lookup"><span data-stu-id="b91b3-260">You can copy blobs across storage accounts and regions asynchronously.</span></span> <span data-ttu-id="b91b3-261">hello következő példa bemutatja, hogyan toocopy blobok a egy tárolási tároló tooanother két különböző tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="b91b3-261">hello following example demonstrates how toocopy blobs from one storage container tooanother in two different storage accounts.</span></span> <span data-ttu-id="b91b3-262">hello példa először beállítja a forrás és cél storage-fiókok változókat, és létrehoz egy adattároló-környezet az egyes fiókok számára.</span><span class="sxs-lookup"><span data-stu-id="b91b3-262">hello example first sets variables for source and destination storage accounts, and then creates a storage context for each account.</span></span> <span data-ttu-id="b91b3-263">A következő hello példa átmásolja blobok hello forrás tároló toohello céltárolója hello segítségével [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-263">Next, hello example copies blobs from hello source container toohello destination container using hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span></span> <span data-ttu-id="b91b3-264">hello példa feltételezi, hogy hello forrás és cél tárfiók és tároló már létezik.</span><span class="sxs-lookup"><span data-stu-id="b91b3-264">hello example assumes that hello source and destination storage accounts and containers already exist.</span></span>

```powershell
#Define hello source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define hello destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference tooblobs in hello source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container tooanother.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

<span data-ttu-id="b91b3-265">Vegye figyelembe, hogy az ebben a példában egy aszinkron másolatot végez.</span><span class="sxs-lookup"><span data-stu-id="b91b3-265">Note that this example performs an asynchronous copy.</span></span> <span data-ttu-id="b91b3-266">Minden egyes példányra hello állapotának figyelheti hello futtatásával [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-266">You can monitor hello status of each copy by running hello [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span></span>

### <a name="how-toocopy-blobs-from-a-secondary-location"></a><span data-ttu-id="b91b3-267">Hogyan toocopy blobok egy másodlagos helyen</span><span class="sxs-lookup"><span data-stu-id="b91b3-267">How toocopy blobs from a secondary location</span></span>
<span data-ttu-id="b91b3-268">Blobok RA-GRS-engedélyezett fiók hello a másodlagos helyre másolhatja.</span><span class="sxs-lookup"><span data-stu-id="b91b3-268">You can copy blobs from hello secondary location of a RA-GRS-enabled account.</span></span>

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-toodelete-a-blob"></a><span data-ttu-id="b91b3-269">Hogyan toodelete blob</span><span class="sxs-lookup"><span data-stu-id="b91b3-269">How toodelete a blob</span></span>
<span data-ttu-id="b91b3-270">toodelete blob, először kérjen le egy blobhivatkozást, és hívhatja hello Remove-AzureStorageBlob parancsmag rajta.</span><span class="sxs-lookup"><span data-stu-id="b91b3-270">toodelete a blob, first get a blob reference and then call hello Remove-AzureStorageBlob cmdlet on it.</span></span> <span data-ttu-id="b91b3-271">a következő példa hello egy adott tárolóban lévő összes hello blobok törlése.</span><span class="sxs-lookup"><span data-stu-id="b91b3-271">hello following example deletes all hello blobs in a given container.</span></span> <span data-ttu-id="b91b3-272">hello példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-272">hello example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="b91b3-273">A következő hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatása hello [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) parancsmag tooremove BLOB az Azure storage-tárolójából.</span><span class="sxs-lookup"><span data-stu-id="b91b3-273">Next, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet tooremove blobs from a container in Azure storage.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooall hello blobs in hello container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-toomanage-azure-blob-snapshots"></a><span data-ttu-id="b91b3-274">Hogyan toomanage Azure blob-pillanatképek</span><span class="sxs-lookup"><span data-stu-id="b91b3-274">How toomanage Azure blob snapshots</span></span>
<span data-ttu-id="b91b3-275">Azure lehetővé teszi a pillanatkép létrehozása a blob.</span><span class="sxs-lookup"><span data-stu-id="b91b3-275">Azure lets you create a snapshot of a blob.</span></span> <span data-ttu-id="b91b3-276">Pillanatkép egy blobot, amely egy adott időben csak olvasható verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="b91b3-276">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="b91b3-277">Pillanatkép létrehozása, azt is kell olvasni, másolja, vagy törölni, de nem módosított.</span><span class="sxs-lookup"><span data-stu-id="b91b3-277">Once a snapshot has been created, it can be read, copied, or deleted, but not modified.</span></span> <span data-ttu-id="b91b3-278">Pillanatképek adja meg egy módon tooback blob be időben a jelenleg megjelenített.</span><span class="sxs-lookup"><span data-stu-id="b91b3-278">Snapshots provide a way tooback up a blob as it appears at a moment in time.</span></span> <span data-ttu-id="b91b3-279">További információkért lásd: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-279">For more information, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span>

### <a name="how-toocreate-a-blob-snapshot"></a><span data-ttu-id="b91b3-280">Hogyan toocreate blob pillanatkép</span><span class="sxs-lookup"><span data-stu-id="b91b3-280">How toocreate a blob snapshot</span></span>
<span data-ttu-id="b91b3-281">egy BLOB, snaphot toocreate először kérjen le egy blobhivatkozást, és majd meghívják a hello `ICloudBlob.CreateSnapshot` metódust.</span><span class="sxs-lookup"><span data-stu-id="b91b3-281">toocreate a snaphot of a blob, first get a blob reference and then call hello `ICloudBlob.CreateSnapshot` method on it.</span></span> <span data-ttu-id="b91b3-282">hello alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-282">hello following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="b91b3-283">A következő hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatása hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metódus toocreate pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-283">Next, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method toocreate a snapshot.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of hello blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-toolist-a-blobs-snapshots"></a><span data-ttu-id="b91b3-284">Hogyan toolist blob pillanatképek</span><span class="sxs-lookup"><span data-stu-id="b91b3-284">How toolist a blob's snapshots</span></span>
<span data-ttu-id="b91b3-285">Egy BLOB kívánt annyi pillanatképeket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b91b3-285">You can create as many snapshots as you want for a blob.</span></span> <span data-ttu-id="b91b3-286">A blob tootrack tartozó hello pillanatképet listázhatja a jelenlegi pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="b91b3-286">You can list hello snapshots associated with your blob tootrack your current snapshots.</span></span> <span data-ttu-id="b91b3-287">hello alábbi példában egy előre meghatározott blob és hívások hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag toolist hello pillanatképek készítése, hogy a blob.</span><span class="sxs-lookup"><span data-stu-id="b91b3-287">hello following example uses a predefined blob and calls hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet toolist hello snapshots of that blob.</span></span>  

```powershell
#Define hello blob name.
$BlobName = "yourblobname"

#List hello snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-toocopy-a-snapshot-of-a-blob"></a><span data-ttu-id="b91b3-288">Hogyan toocopy blob pillanatképe</span><span class="sxs-lookup"><span data-stu-id="b91b3-288">How toocopy a snapshot of a blob</span></span>
<span data-ttu-id="b91b3-289">A blob toorestore hello pillanatfelvétel pillanatkép másolhatja.</span><span class="sxs-lookup"><span data-stu-id="b91b3-289">You can copy a snapshot of a blob toorestore hello snapshot.</span></span> <span data-ttu-id="b91b3-290">Részletes információk és korlátozások: [egy pillanatképet készíteni egy Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-290">For detailed information and restrictions, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span> <span data-ttu-id="b91b3-291">hello alábbi példa először beállítja a változókat a tárfiók, és létrehoz egy adattároló-környezet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-291">hello following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="b91b3-292">A következő hello példa hello tároló és a blob változók határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b91b3-292">Next, hello example defines hello container and blob name variables.</span></span> <span data-ttu-id="b91b3-293">hello példa kéri le egy blobhivatkozást, hello segítségével [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) parancsmag és futtatása hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metódus toocreate pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-293">hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method toocreate a snapshot.</span></span> <span data-ttu-id="b91b3-294">Ezt követően hello példa fut hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) parancsmag toocopy hello pillanatképe a hello ICloudBlob objektummal hello forrás BLOB blob.</span><span class="sxs-lookup"><span data-stu-id="b91b3-294">Then, hello example runs hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet toocopy hello snapshot of a blob using hello ICloudBlob object for hello source blob.</span></span> <span data-ttu-id="b91b3-295">Lehet, hogy tooupdate hello változók a konfiguráció alapján futó hello példa előtt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-295">Be sure tooupdate hello variables based on your configuration before running hello example.</span></span> <span data-ttu-id="b91b3-296">Vegye figyelembe, hogy a következő példa hello feltételezi, hogy hello forrás és cél tárolók, és hello forrás blob már létezik.</span><span class="sxs-lookup"><span data-stu-id="b91b3-296">Note that hello following example assumes that hello source and destination containers, and hello source blob already exist.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define hello variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy hello snapshot tooanother container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

<span data-ttu-id="b91b3-297">Most, hogy megtanulhatta, hogyan toomanage Azure-blobok és az Azure PowerShell pillanatképek blob, nyissa meg toohello következő szakasz toolearn hogyan toomanage táblák, várólisták és fájlokat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-297">Now that you have learned how toomanage Azure blobs and blob snapshots with Azure PowerShell, go toohello next section toolearn how toomanage tables, queues, and files.</span></span>

## <a name="how-toomanage-azure-tables-and-table-entities"></a><span data-ttu-id="b91b3-298">Hogyan toomanage Azure táblázatok és a tábla az entitások</span><span class="sxs-lookup"><span data-stu-id="b91b3-298">How toomanage Azure tables and table entities</span></span>
<span data-ttu-id="b91b3-299">Az Azure Table storage szolgáltatás egy NoSQL-adattár, amelynek segítségével toostore és lekérdezés hatalmas strukturált, nem relációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-299">Azure Table storage service is a NoSQL datastore, which you can use toostore and query huge sets of structured, non-relational data.</span></span> <span data-ttu-id="b91b3-300">hello hello szolgáltatás fő összetevőit táblák, entitásokat és tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b91b3-300">hello main components of hello service are tables, entities, and properties.</span></span> <span data-ttu-id="b91b3-301">A tábla az entitások gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="b91b3-301">A table is a collection of entities.</span></span> <span data-ttu-id="b91b3-302">Egy entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="b91b3-302">An entity is a set of properties.</span></span> <span data-ttu-id="b91b3-303">Minden entitás legfeljebb too252 tulajdonságait, amelyek minden név-érték párok tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-303">Each entity can have up too252 properties, which are all name-value pairs.</span></span> <span data-ttu-id="b91b3-304">Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Table Storage szolgáltatással kapcsolatos fogalmak.</span><span class="sxs-lookup"><span data-stu-id="b91b3-304">This section assumes that you are already familiar with hello Azure Table Storage Service concepts.</span></span> <span data-ttu-id="b91b3-305">Részletes információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx) és [Ismerkedés az Azure Table storage használatának .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-305">For detailed information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx) and [Get started with Azure Table storage using .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md).</span></span>

<span data-ttu-id="b91b3-306">A következő alszakaszokat hello megtudhatja, hogyan toomanage Azure Table storage szolgáltatást Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-306">In hello following subsections, you'll learn how toomanage Azure Table storage service using Azure PowerShell.</span></span> <span data-ttu-id="b91b3-307">hello tárgyalt forgatókönyvekben szerepel a **létrehozása**, **törlése**, és **beolvasása** **táblák**, valamint **hozzáadása**, **lekérdezése**, és **tábla entitások törlése**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-307">hello scenarios covered include **creating**, **deleting**, and **retrieving** **tables**, as well as **adding**, **querying**, and **deleting table entities**.</span></span>

### <a name="how-toocreate-a-table"></a><span data-ttu-id="b91b3-308">Hogyan toocreate tábla</span><span class="sxs-lookup"><span data-stu-id="b91b3-308">How toocreate a table</span></span>
<span data-ttu-id="b91b3-309">Minden táblának kell lennie az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="b91b3-309">Every table must reside in an Azure storage account.</span></span> <span data-ttu-id="b91b3-310">hello következő példa bemutatja, hogyan toocreate Azure Storage-táblázathoz.</span><span class="sxs-lookup"><span data-stu-id="b91b3-310">hello following example demonstrates how toocreate a table in Azure Storage.</span></span> <span data-ttu-id="b91b3-311">hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-311">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="b91b3-312">Hello használja a következő [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmag toocreate Azure Storage-táblázathoz.</span><span class="sxs-lookup"><span data-stu-id="b91b3-312">Next, it uses hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet toocreate a table in Azure Storage.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-tooretrieve-a-table"></a><span data-ttu-id="b91b3-313">Hogyan tooretrieve tábla</span><span class="sxs-lookup"><span data-stu-id="b91b3-313">How tooretrieve a table</span></span>
<span data-ttu-id="b91b3-314">Lekérdezi, és egy vagy az összes tábla tárfiókokban beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b91b3-314">You can query and retrieve one or all tables in a Storage account.</span></span> <span data-ttu-id="b91b3-315">hello következő példa bemutatja, hogyan egy adott táblához használatával tooretrieve hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-315">hello following example demonstrates how tooretrieve a given table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span>

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

<span data-ttu-id="b91b3-316">Meghívja a hello Get-AzureStorageTable parancsmag paraméterek nélkül, ha a tárfiók lekérdezi összes storage-táblákat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-316">If you call hello Get-AzureStorageTable cmdlet without any parameters, it gets all storage tables for a Storage account.</span></span>

### <a name="how-toodelete-a-table"></a><span data-ttu-id="b91b3-317">Hogyan toodelete tábla</span><span class="sxs-lookup"><span data-stu-id="b91b3-317">How toodelete a table</span></span>
<span data-ttu-id="b91b3-318">Törölheti a tábla a tárfiókból hello segítségével [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-318">You can delete a table from a storage account by using hello [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span></span>  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-toomanage-table-entities"></a><span data-ttu-id="b91b3-319">Hogyan toomanage tábla entitások</span><span class="sxs-lookup"><span data-stu-id="b91b3-319">How toomanage table entities</span></span>
<span data-ttu-id="b91b3-320">Jelenleg az Azure PowerShell nem biztosít parancsmagok toomanage táblaentitásokat közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="b91b3-320">Currently, Azure PowerShell does not provide cmdlets toomanage table entities directly.</span></span> <span data-ttu-id="b91b3-321">táblaentitásokat tooperform műveleteket, használhatja hello megadott hello osztályok [Azure Storage ügyféloldali kódtára a .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-321">tooperform operations on table entities, you can use hello classes given in hello [Azure Storage Client Library for .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span></span>

#### <a name="how-tooadd-table-entities"></a><span data-ttu-id="b91b3-322">Hogyan tooadd tábla entitások</span><span class="sxs-lookup"><span data-stu-id="b91b3-322">How tooadd table entities</span></span>
<span data-ttu-id="b91b3-323">egy entitás tooa tábla tooadd először létre kell hoznia egy objektum, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="b91b3-323">tooadd an entity tooa table, first create an object that defines your entity properties.</span></span> <span data-ttu-id="b91b3-324">Egy entitás legfeljebb too255 tulajdonságokat, beleértve a 3 rendszertulajdonsággal tartalmazhat: **PartitionKey**, **RowKey**, és **időbélyeg**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-324">An entity can have up too255 properties, including 3 system properties: **PartitionKey**, **RowKey**, and **Timestamp**.</span></span> <span data-ttu-id="b91b3-325">Ön felelőssége lesz beszúrva, és hello értékek frissítése **PartitionKey** és **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-325">You are responsible for inserting and updating hello values of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="b91b3-326">hello kiszolgáló kezeli hello értékének **időbélyeg**, amely nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="b91b3-326">hello server manages hello value of **Timestamp**, which cannot be modified.</span></span> <span data-ttu-id="b91b3-327">Együtt hello **PartitionKey** és **RowKey** egyedi módon azonosítja az egy táblázaton belüli összes entitás.</span><span class="sxs-lookup"><span data-stu-id="b91b3-327">Together hello **PartitionKey** and **RowKey** uniquely identify every entity within a table.</span></span>

* <span data-ttu-id="b91b3-328">**PartitionKey**: hello partíció hello entitás tárolt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b91b3-328">**PartitionKey**: Determines hello partition that hello entity is stored in.</span></span>
* <span data-ttu-id="b91b3-329">**RowKey**: hello entitás hello partíción belül egyedi azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="b91b3-329">**RowKey**: Uniquely identifies hello entity within hello partition.</span></span>

<span data-ttu-id="b91b3-330">Be too252 egyéni tulajdonságait, hogy egy entitás adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="b91b3-330">You may define up too252 custom properties for an entity.</span></span> <span data-ttu-id="b91b3-331">További információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-331">For more information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="b91b3-332">hello következő példa bemutatja, hogyan tooadd entitások tooa tábla.</span><span class="sxs-lookup"><span data-stu-id="b91b3-332">hello following example demonstrates how tooadd entities tooa table.</span></span> <span data-ttu-id="b91b3-333">hello példa bemutatja, hogyan tooretrieve hello alkalmazott tábla, és vegyen fel több entitás.</span><span class="sxs-lookup"><span data-stu-id="b91b3-333">hello example shows how tooretrieve hello employee table and add several entities into it.</span></span> <span data-ttu-id="b91b3-334">Először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-334">First, it establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="b91b3-335">Ezt követően lekérdezi a megadott tábla hello hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-335">Next, it retrieves hello given table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="b91b3-336">Ha hello tábla nem létezik, hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) parancsmag használt toocreate Azure Storage-táblázathoz.</span><span class="sxs-lookup"><span data-stu-id="b91b3-336">If hello table does not exist, hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet is used toocreate a table in Azure Storage.</span></span> <span data-ttu-id="b91b3-337">A következő hello példa minden entitás partíció- és sorkulcsa megadásával határozza meg Add-entitás tooadd entitások toohello tábla egyéni függvény.</span><span class="sxs-lookup"><span data-stu-id="b91b3-337">Next, hello example defines a custom function Add-Entity tooadd entities toohello table by specifying each entity's partition and row key.</span></span> <span data-ttu-id="b91b3-338">Add-entitás hello függvény hívások hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a hello [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) osztály toocreate entitásobjektumra.</span><span class="sxs-lookup"><span data-stu-id="b91b3-338">hello Add-Entity function calls hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on hello [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) class toocreate an entity object.</span></span> <span data-ttu-id="b91b3-339">Később, a hello példa meghívja a hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) ez entity objektum tooadd metódust azt tooa tábla.</span><span class="sxs-lookup"><span data-stu-id="b91b3-339">Later, hello example calls hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) method on this entity object tooadd it tooa table.</span></span>

```powershell
#Function Add-Entity: Adds an employee entity tooa table.
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

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve hello table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities tooa table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-tooquery-table-entities"></a><span data-ttu-id="b91b3-340">Hogyan tooquery tábla entitások</span><span class="sxs-lookup"><span data-stu-id="b91b3-340">How tooquery table entities</span></span>
<span data-ttu-id="b91b3-341">egy tábla tooquery hello használata [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="b91b3-341">tooquery a table, use hello [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) class.</span></span> <span data-ttu-id="b91b3-342">hello alábbi példa feltételezi, hogy már futtatását hello hogyan megadott hello parancsfájl tooadd entitások című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="b91b3-342">hello following example assumes that you've already run hello script given in hello How tooadd entities section of this guide.</span></span> <span data-ttu-id="b91b3-343">hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárolási adataival, ide tartozik az hello tárfiók neve és a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b91b3-343">hello example first establishes a connection tooAzure Storage using hello storage context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="b91b3-344">Ezt követően próbálja meg tooretrieve korábban létrehozott hello "alkalmazottak" tábla hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-344">Next, it tries tooretrieve hello previously created "Employees" table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="b91b3-345">Hívása hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a hello Microsoft.WindowsAzure.Storage.Table.TableQuery osztály új objektumot hoz létre lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b91b3-345">Calling hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on hello Microsoft.WindowsAzure.Storage.Table.TableQuery class creates a new query object.</span></span> <span data-ttu-id="b91b3-346">hello például egy "ID" oszlop, amelynek értéke 1 megadott egy karakterlánc-szűrővel rendelkező hello entitásokat keresi.</span><span class="sxs-lookup"><span data-stu-id="b91b3-346">hello example looks for hello entities that have an 'ID' column whose value is 1 as specified in a string filter.</span></span> <span data-ttu-id="b91b3-347">Részletes információkért lásd: [lekérdezése táblákat és entitásokat](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-347">For detailed information, see [Querying Tables and Entities](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span></span> <span data-ttu-id="b91b3-348">Ez a lekérdezés futtatásakor hello szűrési feltételeknek megfelelő összes entitásokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b91b3-348">When you run this query, it returns all entities that match hello filter criteria.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference tooa table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns tooselect.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute hello query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with hello table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-toodelete-table-entities"></a><span data-ttu-id="b91b3-349">Hogyan toodelete tábla entitások</span><span class="sxs-lookup"><span data-stu-id="b91b3-349">How toodelete table entities</span></span>
<span data-ttu-id="b91b3-350">A partíció- és sorfejlécek kulcsokkal entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="b91b3-350">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="b91b3-351">hello alábbi példa feltételezi, hogy már futtatását hello hogyan megadott hello parancsfájl tooadd entitások című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="b91b3-351">hello following example assumes that you've already run hello script given in hello How tooadd entities section of this guide.</span></span> <span data-ttu-id="b91b3-352">hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárolási adataival, ide tartozik az hello tárfiók neve és a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b91b3-352">hello example first establishes a connection tooAzure Storage using hello storage context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="b91b3-353">Ezt követően próbálja meg tooretrieve korábban létrehozott hello "alkalmazottak" tábla hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-353">Next, it tries tooretrieve hello previously created "Employees" table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="b91b3-354">Ha hello tábla létezik, hello példa meghívja a hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metódus tooretrieve entitás a partíció- és sorfejlécek értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="b91b3-354">If hello table exists, hello example calls hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) method tooretrieve an entity based on its partition and row key values.</span></span> <span data-ttu-id="b91b3-355">Így továbbítsa hello entitás toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metódus toodelete.</span><span class="sxs-lookup"><span data-stu-id="b91b3-355">Then, pass hello entity toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) method toodelete.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If hello table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together hello PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete hello entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-toomanage-azure-queues-and-queue-messages"></a><span data-ttu-id="b91b3-356">Hogyan toomanage Azure várólisták és üzenetek várólistára</span><span class="sxs-lookup"><span data-stu-id="b91b3-356">How toomanage Azure queues and queue messages</span></span>
<span data-ttu-id="b91b3-357">Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához.</span><span class="sxs-lookup"><span data-stu-id="b91b3-357">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="b91b3-358">Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Queue Storage szolgáltatás fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-358">This section assumes that you are already familiar with hello Azure Queue Storage Service concepts.</span></span> <span data-ttu-id="b91b3-359">Részletes információkért lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-359">For detailed information, see [Get started with Azure Queue storage using .NET](../storage-dotnet-how-to-use-queues.md).</span></span>

<span data-ttu-id="b91b3-360">Ez a szakasz bemutatja, hogyan toomanage Azure Queue storage szolgáltatás Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-360">This section will show you how toomanage Azure Queue storage service using Azure PowerShell.</span></span> <span data-ttu-id="b91b3-361">hello tárgyalt forgatókönyvekben szerepel a **beszúrása** és **törlése** üzenetek, várólista, valamint **létrehozása**, **törlése**, és **sorok beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="b91b3-361">hello scenarios covered include **inserting** and **deleting** queue messages, as well as **creating**, **deleting**, and **retrieving queues**.</span></span>

### <a name="how-toocreate-a-queue"></a><span data-ttu-id="b91b3-362">Hogyan toocreate várólista</span><span class="sxs-lookup"><span data-stu-id="b91b3-362">How toocreate a queue</span></span>
<span data-ttu-id="b91b3-363">hello alábbi példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-363">hello following example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="b91b3-364">Ezután meghívja [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) parancsmag toocreate "queuename" nevű várólista.</span><span class="sxs-lookup"><span data-stu-id="b91b3-364">Next, it calls [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet toocreate a queue named 'queuename'.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

<span data-ttu-id="b91b3-365">Azure Queue szolgáltatás elnevezési konvencióira vonatkozó információkért lásd: [elnevezési üzenetsorok és metaadatok](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-365">For information on naming conventions for Azure Queue Service, see [Naming Queues and Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

### <a name="how-tooretrieve-a-queue"></a><span data-ttu-id="b91b3-366">Hogyan tooretrieve várólista</span><span class="sxs-lookup"><span data-stu-id="b91b3-366">How tooretrieve a queue</span></span>
<span data-ttu-id="b91b3-367">Lekérdezi, és egy konkrét várólistába helyezi vagy a tárfiókban lévő összes hello várólisták listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b91b3-367">You can query and retrieve a specific queue or a list of all hello queues in a Storage account.</span></span> <span data-ttu-id="b91b3-368">hello következő példa bemutatja, hogy a megadott várólista használatával tooretrieve hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-368">hello following example demonstrates how tooretrieve a specified queue using hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span></span>

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

<span data-ttu-id="b91b3-369">Ha meghívja a hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) parancsmag-paraméterek nélkül, akkor minden hello várólista listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="b91b3-369">If you call hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet without any parameters, it gets a list of all hello queues.</span></span>

### <a name="how-toodelete-a-queue"></a><span data-ttu-id="b91b3-370">Hogyan toodelete várólista</span><span class="sxs-lookup"><span data-stu-id="b91b3-370">How toodelete a queue</span></span>
<span data-ttu-id="b91b3-371">toodelete várólista és köszönőüzenetei minden benne tárolt, hívás hello Remove-AzureStorageQueue parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-371">toodelete a queue and all hello messages contained in it, call hello Remove-AzureStorageQueue cmdlet.</span></span> <span data-ttu-id="b91b3-372">hello a következő példa bemutatja, hogyan a megadott várólista használatával toodelete hello Remove-AzureStorageQueue parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-372">hello following example shows how toodelete a specified queue using hello Remove-AzureStorageQueue cmdlet.</span></span>

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-tooinsert-a-message-into-a-queue"></a><span data-ttu-id="b91b3-373">Hogyan tooinsert egy várólistára üzenet</span><span class="sxs-lookup"><span data-stu-id="b91b3-373">How tooinsert a message into a queue</span></span>
<span data-ttu-id="b91b3-374">tooinsert egy létező üzenetsorba, az üzenet először létre kell hoznia egy új példányát hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="b91b3-374">tooinsert a message into an existing queue, first create a new instance of hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="b91b3-375">Ezután hívja hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="b91b3-375">Next, call hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method.</span></span> <span data-ttu-id="b91b3-376">Egy CloudQueueMessage egy karakterláncból (UTF-8 formátumban), illetve egy bájttömböt hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="b91b3-376">A CloudQueueMessage can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="b91b3-377">hello a következő példa bemutatja, hogyan tooadd üzenet-várólista tooa.</span><span class="sxs-lookup"><span data-stu-id="b91b3-377">hello following example demonstrates how tooadd message tooa queue.</span></span> <span data-ttu-id="b91b3-378">hello példa először hoz létre a kapcsolat tooAzure tárolási hello tárfiók környezetét, beleértve hello a tárfiók neve vagy a hozzáférési kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-378">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="b91b3-379">Ezt követően lekérdezi a megadott várólista hello hello segítségével [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b91b3-379">Next, it retrieves hello specified queue using hello [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span></span> <span data-ttu-id="b91b3-380">Ha hello várólista létezik, hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) parancsmag használt toocreate hello példányának [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="b91b3-380">If hello queue exists, hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet is used toocreate an instance of hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="b91b3-381">Később, a hello példa meghívja a hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) ezen üzenet objektum tooadd metódust azt tooa várólista.</span><span class="sxs-lookup"><span data-stu-id="b91b3-381">Later, hello example calls hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method on this message object tooadd it tooa queue.</span></span> <span data-ttu-id="b91b3-382">Itt található kód, amely lekérdezi a várólista, és szúrja be a "MessageInfo" üdvözlőüzenetére:</span><span class="sxs-lookup"><span data-stu-id="b91b3-382">Here is code which retrieves a queue and inserts hello message 'MessageInfo':</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If hello queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of hello CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message toohello queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-toode-queue-at-hello-next-message"></a><span data-ttu-id="b91b3-383">Hogyan toode-várólista hello a következő üzenet</span><span class="sxs-lookup"><span data-stu-id="b91b3-383">How toode-queue at hello next message</span></span>
<span data-ttu-id="b91b3-384">A kód két lépésben távolít el egy üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="b91b3-384">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="b91b3-385">Hello hívás esetén [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metódus, a hibaüzenet hello tovább a sorhoz.</span><span class="sxs-lookup"><span data-stu-id="b91b3-385">When you call hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) method, you get hello next message in a queue.</span></span> <span data-ttu-id="b91b3-386">Az üzenet **GetMessage** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="b91b3-386">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="b91b3-387">toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="b91b3-387">toofinish removing hello message from hello queue, you must also call hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) method.</span></span> <span data-ttu-id="b91b3-388">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="b91b3-388">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="b91b3-389">A kód hívások **DeleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="b91b3-389">Your code calls **DeleteMessage** right after hello message has been processed.</span></span>

```powershell
# Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get hello message object from hello queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete hello message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-toomanage-azure-file-shares-and-files"></a><span data-ttu-id="b91b3-390">Hogyan toomanage az Azure file közösen használja, és a fájlok</span><span class="sxs-lookup"><span data-stu-id="b91b3-390">How toomanage Azure file shares and files</span></span>
<span data-ttu-id="b91b3-391">Az Azure File storage közös tárterületet hello szabványos SMB protokollt használó alkalmazások számára biztosít.</span><span class="sxs-lookup"><span data-stu-id="b91b3-391">Azure File storage offers shared storage for applications using hello standard SMB protocol.</span></span> <span data-ttu-id="b91b3-392">Microsoft Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások hozzáférhetnek a fájladatok olyan megosztáson található hello File storage API- vagy Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-392">Microsoft Azure virtual machines and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share via hello File storage API or Azure PowerShell.</span></span>

<span data-ttu-id="b91b3-393">További információ az Azure File storage: [Ismerkedés az Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) és [File szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span><span class="sxs-lookup"><span data-stu-id="b91b3-393">For more information on Azure File storage, see [Get started with Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) and [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span></span>

## <a name="how-tooset-and-query-storage-analytics"></a><span data-ttu-id="b91b3-394">Hogyan tooset és lekérdezés tárolási analitika</span><span class="sxs-lookup"><span data-stu-id="b91b3-394">How tooset and query storage analytics</span></span>
<span data-ttu-id="b91b3-395">Használhat [Azure Storage Analytics](../storage-analytics.md) toocollect metrikák az Azure storage-fiókok és a naplóadatok kérelmekkel kapcsolatos küldött tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b91b3-395">You can use [Azure Storage Analytics](../storage-analytics.md) toocollect metrics for your Azure storage accounts and log data about requests sent tooyour storage account.</span></span> <span data-ttu-id="b91b3-396">Tárolási metrikák toomonitor hello állapotát egy tárfiók és tároló naplózási toodiagnose használja, és a problémák elhárítása a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b91b3-396">You can use storage metrics toomonitor hello health of a storage account, and storage logging toodiagnose and troubleshoot issues with your storage account.</span></span> <span data-ttu-id="b91b3-397">Használatával figyelését be tudja állítani az Azure portál vagy a Windows PowerShell hello, vagy programozott módon hello a storage ügyféloldali kódtára.</span><span class="sxs-lookup"><span data-stu-id="b91b3-397">You can configure monitoring using hello Azure portal or Windows PowerShell, or programmatically using hello storage client library.</span></span> <span data-ttu-id="b91b3-398">Tárolási naplózási kiszolgálóoldali történik, és lehetővé teszi a sikeres és a sikertelen kérelmek a tárfiókban lévő toorecord részleteit.</span><span class="sxs-lookup"><span data-stu-id="b91b3-398">Storage logging happens server-side and enables you toorecord details for both successful and failed requests in your storage account.</span></span> <span data-ttu-id="b91b3-399">Ezek a naplók engedélyezése olvasási, írási és törlési műveleteket a táblák, üzenetsorok, blobok, valamint hello okok miatt a sikertelen kérelmek toosee részleteit.</span><span class="sxs-lookup"><span data-stu-id="b91b3-399">These logs enable you toosee details of read, write, and delete operations against your tables, queues, and blobs as well as hello reasons for failed requests.</span></span>

<span data-ttu-id="b91b3-400">Hogyan powershellel, tooenable és nézet Storage Metrics data: toolearn [hogyan tooenable Storage Metrics PowerShell-lel](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span><span class="sxs-lookup"><span data-stu-id="b91b3-400">toolearn how tooenable and view Storage Metrics data using PowerShell, see [How tooenable Storage Metrics using PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span></span>

<span data-ttu-id="b91b3-401">Hogyan powershellel, tooenable és lekérése tárolási naplózási adatait: toolearn [hogyan tooenable tároló-naplózás PowerShell-lel](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) és [keresése a naplózás tárolási naplóadatok](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span><span class="sxs-lookup"><span data-stu-id="b91b3-401">toolearn how tooenable and retrieve Storage Logging data using PowerShell, see [How tooenable Storage Logging using PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) and [Finding your Storage Logging log data](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span></span>
<span data-ttu-id="b91b3-402">A Storage Metrics és tootroubleshoot tárolási problémák naplózási tároló részletes információkért lásd: [figyelés felismerése és hibaelhárítása a Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-402">For detailed information on using Storage Metrics and Storage Logging tootroubleshoot storage issues, see [Monitoring, Diagnosing, and Troubleshooting Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span></span>

## <a name="how-toomanage-shared-access-signature-sas-and-stored-access-policy"></a><span data-ttu-id="b91b3-403">Hogyan toomanage közös hozzáférésű Jogosultságkód (SAS) és a hozzáférési házirendben tárolt</span><span class="sxs-lookup"><span data-stu-id="b91b3-403">How toomanage Shared Access Signature (SAS) and Stored Access Policy</span></span>
<span data-ttu-id="b91b3-404">Közös hozzáférésű jogosultságkód hello biztonsági modellt az Azure Storage használó alkalmazások fontos részét képezik.</span><span class="sxs-lookup"><span data-stu-id="b91b3-404">Shared access signatures are an important part of hello security model for any application using Azure Storage.</span></span> <span data-ttu-id="b91b3-405">Azok a megtekinthetik a korlátozott engedélyekkel tooyour tárolási fiók tooclients, amely nem rendelkezhet hello kulcsára.</span><span class="sxs-lookup"><span data-stu-id="b91b3-405">They are useful for providing limited permissions tooyour storage account tooclients that should not have hello account key.</span></span> <span data-ttu-id="b91b3-406">Alapértelmezés szerint csak hello tulajdonosa hello tárfiók elérhessék blobot, táblát és üzenetsort, hogy a fiókon belül.</span><span class="sxs-lookup"><span data-stu-id="b91b3-406">By default, only hello owner of hello storage account may access blobs, tables, and queues within that account.</span></span> <span data-ttu-id="b91b3-407">Ha a szolgáltatás vagy alkalmazás toomake ezen erőforrások elérhető tooother ügyfelek a hozzáférési kulcs megosztása nélkül, három lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="b91b3-407">If your service or application needs toomake these resources available tooother clients without sharing your access key, you have three options:</span></span>

* <span data-ttu-id="b91b3-408">Állítsa be a tároló engedélyeit toopermit névtelen olvasási hozzáférés toohello tároló és a benne található blobokat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-408">Set a container's permissions toopermit anonymous read access toohello container and its blobs.</span></span> <span data-ttu-id="b91b3-409">Ez nem engedélyezett táblák vagy olyan várólisták esetében.</span><span class="sxs-lookup"><span data-stu-id="b91b3-409">This is not allowed for tables or queues.</span></span>
* <span data-ttu-id="b91b3-410">Használjon egy közös hozzáférésű jogosultságkódot, amely engedélyezi a korlátozott hozzáférésű jogok toocontainers, blobokat, üzenetsorokat és táblákat egy adott időtartam alatt.</span><span class="sxs-lookup"><span data-stu-id="b91b3-410">Use a shared access signature that grants restricted access rights toocontainers, blobs, queues, and tables for a specific time interval.</span></span>
* <span data-ttu-id="b91b3-411">A tárolt hozzáférési házirend tooobtain egy közös hozzáférésű jogosultságkód szabályozhatják további szintet használja, egy tárolót vagy a benne található blobokat, egy sor vagy tábla.</span><span class="sxs-lookup"><span data-stu-id="b91b3-411">Use a stored access policy tooobtain an additional level of control over shared access signatures for a container or its blobs, for a queue, or for a table.</span></span> <span data-ttu-id="b91b3-412">hello tárolt házirend lehetővé teszi toochange hello kezdési ideje, a lejárat időpontjának vagy az engedélyek az aláírás, vagy toorevoke azt követően, hogy ki van állítva.</span><span class="sxs-lookup"><span data-stu-id="b91b3-412">hello stored access policy allows you toochange hello start time, expiry time, or permissions for a signature, or toorevoke it after it has been issued.</span></span>

<span data-ttu-id="b91b3-413">A közös hozzáférésű jogosultságkód lehet két űrlap egyikében:</span><span class="sxs-lookup"><span data-stu-id="b91b3-413">A shared access signature can be in one of two forms:</span></span>

* <span data-ttu-id="b91b3-414">**Ad hoc biztonsági Társítások**: egy alkalmi SAS, a hello kezdési ideje, a lejárat időpontjának létrehozásakor, és a hello SAS engedélyeinek összes adhatók meg hello SAS URI-t.</span><span class="sxs-lookup"><span data-stu-id="b91b3-414">**Ad hoc SAS**: When you create an ad hoc SAS, hello start time, expiry time, and permissions for hello SAS are all specified on hello SAS URI.</span></span> <span data-ttu-id="b91b3-415">Az ilyen típusú SAS lehet létrehozni egy tárolót, a blob, tábla, vagy várólista, és nem visszavonható.</span><span class="sxs-lookup"><span data-stu-id="b91b3-415">This type of SAS may be created on a container, blob, table, or queue and it is non-revocable.</span></span>
* <span data-ttu-id="b91b3-416">**SAS tárolt hozzáférési házirenddel**: A tárolt házirend van definiálva az erőforrás-tárolónak egy blob-tároló, táblázat vagy várólista - és használhatja azt toomanage megkötések legalább egy közös hozzáférésű jogosultságkódokat.</span><span class="sxs-lookup"><span data-stu-id="b91b3-416">**SAS with stored access policy**: A stored access policy is defined on a resource container a blob container, table, or queue - and you can use it toomanage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="b91b3-417">Ha SAS-kód társítja a tárolt házirend, hello SAS hello korlátozásokat örököl - hello start idő, a lejárati idő és a tárolt hello hozzáférési házirend definiált - engedélyek.</span><span class="sxs-lookup"><span data-stu-id="b91b3-417">When you associate a SAS with a stored access policy, hello SAS inherits hello constraints - hello start time, expiry time, and permissions - defined for hello stored access policy.</span></span> <span data-ttu-id="b91b3-418">Ez a biztonsági Társítások típus visszavonható.</span><span class="sxs-lookup"><span data-stu-id="b91b3-418">This type of SAS is revocable.</span></span>

<span data-ttu-id="b91b3-419">További információkért lásd: [használatával megosztott hozzáférési aláírásokkal (SAS)](../storage-dotnet-shared-access-signature-part-1.md) és [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-419">For more information, see [Using Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md) and [Manage anonymous read access toocontainers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>

<span data-ttu-id="b91b3-420">A következő szakaszokban hello megtudhatja, hogyan toocreate egy megosztott hozzáférési aláírást token és tárolt hozzáférési házirend Azure-táblákban.</span><span class="sxs-lookup"><span data-stu-id="b91b3-420">In hello next sections, you will learn how toocreate a shared access signature token and stored access policy for Azure tables.</span></span> <span data-ttu-id="b91b3-421">Az Azure PowerShell hasonló parancsmagokat tárolók, blobok és várólisták is kínál.</span><span class="sxs-lookup"><span data-stu-id="b91b3-421">Azure PowerShell provides similar cmdlets for containers, blobs, and queues as well.</span></span> <span data-ttu-id="b91b3-422">Ebben a szakaszban toorun hello parancsfájlok letöltése hello [Azure PowerShell verziója 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b91b3-422">toorun hello scripts in this section, download hello [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) or later.</span></span>

### <a name="how-toocreate-a-policy-based-shared-access-signature-token"></a><span data-ttu-id="b91b3-423">Hogyan csoportházirend-alapú toocreate közös hozzáférésű Jogosultságkód jogkivonat</span><span class="sxs-lookup"><span data-stu-id="b91b3-423">How toocreate a policy-based Shared Access Signature token</span></span>
<span data-ttu-id="b91b3-424">Hello New-AzureStorageTableStoredAccessPolicy parancsmag toocreate egy új tárolt házirend használja.</span><span class="sxs-lookup"><span data-stu-id="b91b3-424">Use hello New-AzureStorageTableStoredAccessPolicy cmdlet toocreate a new stored access policy.</span></span> <span data-ttu-id="b91b3-425">Majd, meghívják a hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmag toocreate egy új csoportházirend-alapú megosztott aláírás jogkivonatot egy Azure Storage tábla.</span><span class="sxs-lookup"><span data-stu-id="b91b3-425">Then, call hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate a new policy-based shared access signature token for an Azure Storage table.</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-toocreate-an-ad-hoc-non-revocable-shared-access-signature-token"></a><span data-ttu-id="b91b3-426">Hogyan toocreate egy ad hoc (nem visszavonható) közös hozzáférésű Jogosultságkód-jogkivonatot</span><span class="sxs-lookup"><span data-stu-id="b91b3-426">How toocreate an ad hoc (non-revocable) Shared Access Signature token</span></span>
<span data-ttu-id="b91b3-427">Használjon hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) parancsmag toocreate új alkalmi (nem visszavonható) közös hozzáférésű Jogosultságkód tokent egy Azure Storage-tábla a(z):</span><span class="sxs-lookup"><span data-stu-id="b91b3-427">Use hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate a new ad hoc (non-revocable) Shared Access Signature token for an Azure Storage table:</span></span>

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-toocreate-a-stored-access-policy"></a><span data-ttu-id="b91b3-428">Hogyan toocreate a tárolt házirend</span><span class="sxs-lookup"><span data-stu-id="b91b3-428">How toocreate a stored access policy</span></span>
<span data-ttu-id="b91b3-429">Használja az Azure Storage tábla hello New-AzureStorageTableStoredAccessPolicy parancsmag toocreate egy új tárolt házirend:</span><span class="sxs-lookup"><span data-stu-id="b91b3-429">Use hello New-AzureStorageTableStoredAccessPolicy cmdlet toocreate a new stored access policy for an Azure Storage table:</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-tooupdate-a-stored-access-policy"></a><span data-ttu-id="b91b3-430">Hogyan tooupdate a tárolt házirend</span><span class="sxs-lookup"><span data-stu-id="b91b3-430">How tooupdate a stored access policy</span></span>
<span data-ttu-id="b91b3-431">Egy Azure Storage tábla hello Set-AzureStorageTableStoredAccessPolicy parancsmag tooupdate egy meglévő tárolt hozzáférési házirendet használja:</span><span class="sxs-lookup"><span data-stu-id="b91b3-431">Use hello Set-AzureStorageTableStoredAccessPolicy cmdlet tooupdate an existing stored access policy for an Azure Storage table:</span></span>

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-toodelete-a-stored-access-policy"></a><span data-ttu-id="b91b3-432">Hogyan toodelete a tárolt házirend</span><span class="sxs-lookup"><span data-stu-id="b91b3-432">How toodelete a stored access policy</span></span>
<span data-ttu-id="b91b3-433">Használja a Remove-AzureStorageTableStoredAccessPolicy hello parancsmag toodelete egy tárolt házirend egy Azure Storage táblán:</span><span class="sxs-lookup"><span data-stu-id="b91b3-433">Use hello Remove-AzureStorageTableStoredAccessPolicy cmdlet toodelete a stored access policy on an Azure Storage table:</span></span>

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-toouse-azure-storage-for-us-government-and-azure-china"></a><span data-ttu-id="b91b3-434">Hogyan toouse Azure Storage az Amerikai Egyesült Államok kormánya és Azure Kínában</span><span class="sxs-lookup"><span data-stu-id="b91b3-434">How toouse Azure Storage for U.S. government and Azure China</span></span>
<span data-ttu-id="b91b3-435">Környezetet az Azure például egy Microsoft Azure-ban független telepítését [Azure Government az Amerikai Egyesült Államok kormánya](https://azure.microsoft.com/features/gov/), [globális Azure AzureCloud](https://portal.azure.com), és [AzureChinaCloud Azure Kínában a 21Vianet által működtetett](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="b91b3-435">An Azure environment is an independent deployment of Microsoft Azure, such as [Azure Government for U.S. government](https://azure.microsoft.com/features/gov/), [AzureCloud for global Azure](https://portal.azure.com), and [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span> <span data-ttu-id="b91b3-436">Új Azure-környezetek Egyesült kormányzati és Azure Kína telepítése.</span><span class="sxs-lookup"><span data-stu-id="b91b3-436">You can deploy new Azure environments for U.S government and Azure China.</span></span>

<span data-ttu-id="b91b3-437">Azure Storage a AzureChinaCloud toouse, kell toocreate AzureChinaCloud társított adattároló-környezet.</span><span class="sxs-lookup"><span data-stu-id="b91b3-437">toouse Azure Storage with AzureChinaCloud, you need toocreate a storage context that is associated with AzureChinaCloud.</span></span> <span data-ttu-id="b91b3-438">Kövesse a lépéseket tooget használatba:</span><span class="sxs-lookup"><span data-stu-id="b91b3-438">Follow these steps tooget you started:</span></span>

1. <span data-ttu-id="b91b3-439">Futtassa a hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag toosee hello érhető el az Azure-környezetek:</span><span class="sxs-lookup"><span data-stu-id="b91b3-439">Run hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello available Azure environments:</span></span>
   
    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="b91b3-440">Egy Azure Kína fiók tooWindows PowerShell hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b91b3-440">Add an Azure China account tooWindows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. <span data-ttu-id="b91b3-441">Adattároló-környezet AzureChinaCloud fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="b91b3-441">Create a storage context for an AzureChinaCloud account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

<span data-ttu-id="b91b3-442">Azure Storage toouse rendelkező [USA Az Azure Government](https://azure.microsoft.com/features/gov/), érdemes egy új környezet, és ezután hozzon létre egy új adattároló-környezet ebben a környezetben:</span><span class="sxs-lookup"><span data-stu-id="b91b3-442">toouse Azure Storage with [U.S. Azure Government](https://azure.microsoft.com/features/gov/), you should define a new environment and then create a new storage context with this environment:</span></span>

1. <span data-ttu-id="b91b3-443">Futtassa a hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) parancsmag toosee hello érhető el az Azure-környezetek:</span><span class="sxs-lookup"><span data-stu-id="b91b3-443">Run hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello available Azure environments:</span></span>

    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="b91b3-444">Egy Azure Amerikai Egyesült államokbeli kormányzati fiók tooWindows PowerShell hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b91b3-444">Add an Azure US Government account tooWindows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. <span data-ttu-id="b91b3-445">Adattároló-környezet AzureUSGovernment fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="b91b3-445">Create a storage context for an AzureUSGovernment account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
<span data-ttu-id="b91b3-446">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b91b3-446">For more information, see:</span></span>

* <span data-ttu-id="b91b3-447">[A Microsoft Azure Government – útmutató fejlesztőknek](../../azure-government/documentation-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="b91b3-447">[Microsoft Azure Government Developer Guide](../../azure-government/documentation-government-developer-guide.md).</span></span>
* [<span data-ttu-id="b91b3-448">Kínai szolgáltatás egy alkalmazás létrehozásakor különbségek áttekintése</span><span class="sxs-lookup"><span data-stu-id="b91b3-448">Overview of Differences When Creating an Application on China Service</span></span>](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a><span data-ttu-id="b91b3-449">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b91b3-449">Next Steps</span></span>
<span data-ttu-id="b91b3-450">Az útmutatóban, hogy megismerte hogyan toomanage Azure Storage Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b91b3-450">In this guide, you've learned how toomanage Azure Storage with Azure PowerShell.</span></span> <span data-ttu-id="b91b3-451">Íme, néhány kapcsolódó cikkek és a velük kapcsolatos további források.</span><span class="sxs-lookup"><span data-stu-id="b91b3-451">Here are some related articles and resources for learning more about them.</span></span>

* [<span data-ttu-id="b91b3-452">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="b91b3-452">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="b91b3-453">Az Azure Storage PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="b91b3-453">Azure Storage PowerShell Cmdlets</span></span>](/powershell/module/azurerm.storage/#storage)
* [<span data-ttu-id="b91b3-454">A Windows PowerShell-referencia</span><span class="sxs-lookup"><span data-stu-id="b91b3-454">Windows PowerShell Reference</span></span>](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How toomanage storage accounts in Azure]: #manageaccount
[How tooset a default Azure subscription]: #setdefsub
[How toocreate a new Azure storage account]: #createaccount
[How tooset a default Azure storage account]: #defaultaccount
[How toolist all Azure storage accounts in a subscription]: #listaccounts
[How toocreate an Azure storage context]: #createctx
[How toomanage Azure blobs and blob snapshots]: #manageblobs
[How toocreate a container]: #container
[How tooupload a blob into a container]: #uploadblob
[How toodownload blobs from a container]: #downblob
[How toocopy blobs from one storage container tooanother]: #copyblob
[How toodelete a blob]: #deleteblob
[How toomanage Azure blob snapshots]: #manageshots
[How toocreate a blob snapshot]: #createshot
[How toolist snapshots of a blob]: #listshot
[How toocopy a snapshot of a blob]: #copyshot
[How toomanage Azure tables and table entities]: #managetables
[How toocreate a table]: #createtable
[How tooretrieve a table]: #gettable
[How toodelete a table]: #remtable
[How toomanage table entities]: #mngentity
[How tooadd table entities]: #addentity
[How tooquery table entities]: #queryentity
[How toodelete table entities]: #deleteentity
[How toomanage Azure queues and queue messages]: #managequeues
[How toocreate a queue]: #createqueue
[How tooretrieve a queue]: #getqueue
[How toodelete a queue]: #remqueue
[How toomanage queue messages]: #mngqueuemsg
[How tooinsert a message into a queue]: #addqueuemsg
[How toode-queue at hello next message]: #dequeuemsg
[How toomanage Azure file shares and files]: #files
[How tooset and query storage analytics]: #stganalytics
[How toomanage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How toouse Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
