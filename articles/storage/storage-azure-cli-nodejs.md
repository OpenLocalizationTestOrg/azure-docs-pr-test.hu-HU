---
title: az Azure Storage Azure CLI 1.0 aaaUsing hello |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure parancssori felület (CLI) 1.0 az Azure Storage toocreate és a storage-fiókok kezelése és az Azure-blobokat és fájlok használatát. hello Azure parancssori felület egy olyan többplatformos eszköz"
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: 786f2be64875f5094f09fd6e4a47532889c3a82f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-10-with-azure-storage"></a><span data-ttu-id="22c37-104">Az Azure Storage hello Azure CLI 1.0 használatával</span><span class="sxs-lookup"><span data-stu-id="22c37-104">Using hello Azure CLI 1.0 with Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="22c37-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="22c37-105">Overview</span></span>

<span data-ttu-id="22c37-106">hello Azure parancssori felület számos nyílt forráskódú, platformok közötti parancsok végzett munka hello Azure platformra.</span><span class="sxs-lookup"><span data-stu-id="22c37-106">hello Azure CLI provides a set of open source, cross-platform commands for working with hello Azure Platform.</span></span> <span data-ttu-id="22c37-107">Nagy részét hello hello található ugyanezeket a funkciókat biztosít [Azure-portálon](https://portal.azure.com) , valamint a gazdag adat-hozzáférési funkciókat.</span><span class="sxs-lookup"><span data-stu-id="22c37-107">It provides much of hello same functionality found in hello [Azure portal](https://portal.azure.com) as well as rich data access functionality.</span></span>

<span data-ttu-id="22c37-108">Ez az útmutató azt fogja feltárja hogyan toouse [Azure parancssori felület (CLI)](../cli-install-nodejs.md) tooperform számos fejlesztését és a felügyeleti feladatot az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="22c37-108">In this guide, we'll explore how toouse [Azure Command-Line Interface (Azure CLI)](../cli-install-nodejs.md) tooperform a variety of development and administration tasks with Azure Storage.</span></span> <span data-ttu-id="22c37-109">Azt javasoljuk, hogy letöltése és telepítése vagy frissítése toohello Azure CLI legújabb az útmutató használata előtt.</span><span class="sxs-lookup"><span data-stu-id="22c37-109">We recommend that you download and install or upgrade toohello latest Azure CLI before using this guide.</span></span>

<span data-ttu-id="22c37-110">Ez az útmutató feltételezi, hogy tudomásul veszi hello Azure Storage alapvető fogalmait.</span><span class="sxs-lookup"><span data-stu-id="22c37-110">This guide assumes that you understand hello basic concepts of Azure Storage.</span></span> <span data-ttu-id="22c37-111">hello az útmutató számos parancsfájlt toodemonstrate hello használata az Azure Storage Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="22c37-111">hello guide provides a number of scripts toodemonstrate hello usage of hello Azure CLI with Azure Storage.</span></span> <span data-ttu-id="22c37-112">Lehet, hogy tooupdate hello parancsfájl-változókat minden parancsprogram futtatása előtt a konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="22c37-112">Be sure tooupdate hello script variables based on your configuration before running each script.</span></span>

> [!NOTE]
> <span data-ttu-id="22c37-113">hello útmutató példákat hello Azure CLI parancs és a parancsfájl klasszikus tárfiókokat.</span><span class="sxs-lookup"><span data-stu-id="22c37-113">hello guide provides hello Azure CLI command and script examples for classic storage accounts.</span></span> <span data-ttu-id="22c37-114">Lásd: [Using hello Azure parancssori felület Mac, Linux és Windows Azure Resource Manager](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) az erőforrás-kezelő storage-fiókok Azure parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="22c37-114">See [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) for Azure CLI commands for Resource Manager storage accounts.</span></span>
>
>

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-hello-azure-cli-in-5-minutes"></a><span data-ttu-id="22c37-115">Ismerkedés az Azure Storage és hello Azure CLI 5 percben</span><span class="sxs-lookup"><span data-stu-id="22c37-115">Get started with Azure Storage and hello Azure CLI in 5 minutes</span></span>
<span data-ttu-id="22c37-116">Ez az útmutató Ubuntu használja példák, de más operációs Rendszeri platformokon hasonló módon végre kell hajtania.</span><span class="sxs-lookup"><span data-stu-id="22c37-116">This guide uses Ubuntu for examples, but other OS platforms should perform similarly.</span></span>

<span data-ttu-id="22c37-117">**Új tooAzure:** a Microsoft Azure-előfizetés és az adott előfizetéshez tartozó Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="22c37-117">**New tooAzure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="22c37-118">Az Azure megvásárlási lehetőségeinek információkért lásd: [ingyenes](https://azure.microsoft.com/pricing/free-trial/), [beszerzési lehetőségek](https://azure.microsoft.com/pricing/purchase-options/), és [ajánlatok](https://azure.microsoft.com/pricing/member-offers/) (az MSDN, a Microsoft Partner Network, és a BizSpark és egyéb Microsoft programok tagjai).</span><span class="sxs-lookup"><span data-stu-id="22c37-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="22c37-119">Lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure-előfizetések további információt.</span><span class="sxs-lookup"><span data-stu-id="22c37-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="22c37-120">**Miután létrehozta a Microsoft Azure-előfizetésre és -fiókra:**</span><span class="sxs-lookup"><span data-stu-id="22c37-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="22c37-121">Töltse le és telepítse az Azure parancssori felület hello utasításokat a következő témakörben ismertetett hello [telepítés hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="22c37-121">Download and install hello Azure CLI following hello instructions outlined in [Install hello Azure CLI](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="22c37-122">Hello Azure parancssori felület telepítése után, fogja tudni toouse hello azure parancsot a parancssori felület (Bash, terminál, parancssor) tooaccess hello Azure parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="22c37-122">Once hello Azure CLI has been installed, you will be able toouse hello azure command from your command-line interface (Bash, Terminal, Command prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="22c37-123">Típus hello _azure_ parancsot, és meg kell jelennie a következő kimeneti hello.</span><span class="sxs-lookup"><span data-stu-id="22c37-123">Type hello _azure_ command and you should see hello following output.</span></span>

    ![Az Azure parancs kimenete][Image1]
3. <span data-ttu-id="22c37-125">Hello parancssori felület, írja be `azure storage` minden toolist az azure storage parancsok hello és hello funkciók hello Azure parancssori Felületet biztosít első benyomást kell szereznie.</span><span class="sxs-lookup"><span data-stu-id="22c37-125">In hello command-line interface, type `azure storage` toolist out all hello azure storage commands and get a first impression of hello functionalities hello Azure CLI provides.</span></span> <span data-ttu-id="22c37-126">Beírhatja a parancsnév **-h** paraméter (például `azure storage share create -h`) parancsszintaxis toosee részleteit.</span><span class="sxs-lookup"><span data-stu-id="22c37-126">You can type command name with **-h** parameter (for example, `azure storage share create -h`) toosee details of command syntax.</span></span>
4. <span data-ttu-id="22c37-127">Most lesz ad egy egyszerű parancsprogram, amely tartalmazza az alapszintű Azure CLI parancsok tooaccess Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="22c37-127">Now, we'll give you a simple script that shows basic Azure CLI commands tooaccess Azure Storage.</span></span> <span data-ttu-id="22c37-128">hello parancsfájl először kérni fogja tooset két változó a tárfiók és a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="22c37-128">hello script will first ask you tooset two variables for your storage account and key.</span></span> <span data-ttu-id="22c37-129">Ezután hello parancsfájl hozzon létre egy új tárolót az új tárfiókot, és töltse fel a meglévő lemezkép fájl (blob) toothat tároló.</span><span class="sxs-lookup"><span data-stu-id="22c37-129">Then, hello script will create a new container in this new storage account and upload an existing image file (blob) toothat container.</span></span> <span data-ttu-id="22c37-130">Miután hello parancsfájl megjeleníti az adott tároló összes BLOB, le fogja tölteni hello kép fájl toohello célkönyvtáron létező hello helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="22c37-130">After hello script lists all blobs in that container, it will download hello image file toohello destination directory which exists on hello local computer.</span></span>

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating hello container..."
    azure storage container create $container_name

    echo "Uploading hello image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing hello blobs..."
    azure storage blob list $container_name

    echo "Downloading hello image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. <span data-ttu-id="22c37-131">A helyi számítógépen nyissa meg az előnyben részesített szövegszerkesztőben (például vim).</span><span class="sxs-lookup"><span data-stu-id="22c37-131">In your local computer, open your preferred text editor (vim for example).</span></span> <span data-ttu-id="22c37-132">Parancsfájl fent hello írja be a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="22c37-132">Type hello above script into your text editor.</span></span>
6. <span data-ttu-id="22c37-133">Most tooupdate hello parancsfájl-változókat a konfigurációs beállítások alapján van szüksége.</span><span class="sxs-lookup"><span data-stu-id="22c37-133">Now, you need tooupdate hello script variables based on your configuration settings.</span></span>

   * <span data-ttu-id="22c37-134">**< Storage_account_name >** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="22c37-134">**<storage_account_name>** Use hello given name in hello script or enter a new name for your storage account.</span></span> <span data-ttu-id="22c37-135">**Fontos:** hello tárfiókja nevére hello Azure egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="22c37-135">**Important:** hello name of hello storage account must be unique in Azure.</span></span> <span data-ttu-id="22c37-136">Az kisbetűnek kell lennie, túl!</span><span class="sxs-lookup"><span data-stu-id="22c37-136">It must be lowercase, too!</span></span>
   * <span data-ttu-id="22c37-137">**< storage_account_key >** hello hozzáférési kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="22c37-137">**<storage_account_key>** hello access key of your storage account.</span></span>
   * <span data-ttu-id="22c37-138">**< Container_name >** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tároló.</span><span class="sxs-lookup"><span data-stu-id="22c37-138">**<container_name>** Use hello given name in hello script or enter a new name for your container.</span></span>
   * <span data-ttu-id="22c37-139">**< Image_to_upload >** , mint a helyi számítógépen, adja meg egy elérési utat tooa kép: "~ / images/HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="22c37-139">**<image_to_upload>** Enter a path tooa picture on your local computer, such as: "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="22c37-140">**< Destination_folder >** adjon meg egy elérési utat tooa helyi könyvtár toostore fájlokat töltött le az Azure Storage, például: "~/downloadImages".</span><span class="sxs-lookup"><span data-stu-id="22c37-140">**<destination_folder>** Enter a path tooa local directory toostore files downloaded from Azure Storage, such as: "~/downloadImages".</span></span>
7. <span data-ttu-id="22c37-141">Hello szükséges változók vim frissítése után nyomja le az billentyűkombinációk `ESC`, `:`, `wq!` toosave hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="22c37-141">After you've updated hello necessary variables in vim, press key combinations `ESC`, `:`, `wq!` toosave hello script.</span></span>
8. <span data-ttu-id="22c37-142">toorun ezt a parancsfájlt, egyszerűen típus hello parancsprogramfájljának nevét hello bash konzolon.</span><span class="sxs-lookup"><span data-stu-id="22c37-142">toorun this script, simply type hello script file name in hello bash console.</span></span> <span data-ttu-id="22c37-143">Ez a parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött hello lemezképfájlt.</span><span class="sxs-lookup"><span data-stu-id="22c37-143">After this script runs, you should have a local destination folder that includes hello downloaded image file.</span></span> <span data-ttu-id="22c37-144">a következő képernyőkép hello látható egy példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="22c37-144">hello following screenshot shows an example output:</span></span>

<span data-ttu-id="22c37-145">Hello parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött hello lemezképfájlt.</span><span class="sxs-lookup"><span data-stu-id="22c37-145">After hello script runs, you should have a local destination folder that includes hello downloaded image file.</span></span>

## <a name="manage-storage-accounts-with-hello-azure-cli"></a><span data-ttu-id="22c37-146">Az Azure CLI hello storage-fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="22c37-146">Manage storage accounts with hello Azure CLI</span></span>
### <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="22c37-147">Csatlakozás Azure-előfizetés tooyour</span><span class="sxs-lookup"><span data-stu-id="22c37-147">Connect tooyour Azure subscription</span></span>
<span data-ttu-id="22c37-148">Hello tárolási parancsok többsége Azure-előfizetés nélkül fog működni, de javasolt tooconnect tooyour hello Azure CLI-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="22c37-148">While most of hello storage commands will work without an Azure subscription, we recommend you tooconnect tooyour subscription from hello Azure CLI.</span></span> <span data-ttu-id="22c37-149">tooconfigure hello Azure CLI toowork az előfizetéséhez, a hello lépésekkel [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="22c37-149">tooconfigure hello Azure CLI toowork with your subscription, follow hello steps in [Connect tooan Azure subscription from hello Azure CLI](../xplat-cli-connect.md).</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="22c37-150">Új tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="22c37-150">Create a new storage account</span></span>
<span data-ttu-id="22c37-151">az Azure storage toouse, szüksége lesz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="22c37-151">toouse Azure storage, you will need a storage account.</span></span> <span data-ttu-id="22c37-152">Létrehozhat egy új Azure-tárfiókot, a számítógép tooconnect tooyour előfizetés konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="22c37-152">You can create a new Azure storage account after you have configured your computer tooconnect tooyour subscription.</span></span>

```azurecli
azure storage account create <account_name>
```

<span data-ttu-id="22c37-153">a tárfiók nevére hello kell 3 és 24 karakter hosszúságúnak és kell használnia csak számokat és kisbetűket tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="22c37-153">hello name of your storage account must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a><span data-ttu-id="22c37-154">Környezeti változók alapértelmezett Azure storage-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="22c37-154">Set a default Azure storage account in environment variables</span></span>
<span data-ttu-id="22c37-155">Az előfizetés több tárfiókot is lehet.</span><span class="sxs-lookup"><span data-stu-id="22c37-155">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="22c37-156">Válassza ki az egyiket, és állítsa be azt az hello környezeti változók minden hello tároló parancsai hello ugyanazt a munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="22c37-156">You can choose one of them and set it in hello environment variables for all hello storage commands in hello same session.</span></span> <span data-ttu-id="22c37-157">Ez lehetővé teszi toorun hello Azure CLI tárolási hello tároló megadása nélkül parancsok fiókot, és kulcs explicit módon.</span><span class="sxs-lookup"><span data-stu-id="22c37-157">This enables you toorun hello Azure CLI storage commands without specifying hello storage account and key explicitly.</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="22c37-158">Egy másik módja tooset alapértelmezett tárfiók kapcsolati karakterláncot használ.</span><span class="sxs-lookup"><span data-stu-id="22c37-158">Another way tooset a default storage account is using connection string.</span></span> <span data-ttu-id="22c37-159">Először is hello kapcsolati karakterlánc lekéréséhez parancs:</span><span class="sxs-lookup"><span data-stu-id="22c37-159">Firstly get hello connection string by command:</span></span>

```azurecli
azure storage account connectionstring show <account_name>
```

<span data-ttu-id="22c37-160">Ezután másolja a hello kimeneti kapcsolati karakterláncot, és állítsa be úgy a tooenvironment változó:</span><span class="sxs-lookup"><span data-stu-id="22c37-160">Then copy hello output connection string and set it tooenvironment variable:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a><span data-ttu-id="22c37-161">Hozzon létre és blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="22c37-161">Create and manage blobs</span></span>
<span data-ttu-id="22c37-162">Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.</span><span class="sxs-lookup"><span data-stu-id="22c37-162">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="22c37-163">Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Blob storage fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="22c37-163">This section assumes that you are already familiar with hello Azure Blob storage concepts.</span></span> <span data-ttu-id="22c37-164">Részletes információkért lásd: [az Azure Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="22c37-164">For detailed information, see [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="22c37-165">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="22c37-165">Create a container</span></span>
<span data-ttu-id="22c37-166">Az Azure storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="22c37-166">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="22c37-167">Létrehozhat egy személyes tárolót hello segítségével `azure storage container create` parancs:</span><span class="sxs-lookup"><span data-stu-id="22c37-167">You can create a private container using hello `azure storage container create` command:</span></span>

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> <span data-ttu-id="22c37-168">A névtelen olvasási hozzáférés három szintje van: **ki**, **Blob**, és **tároló**.</span><span class="sxs-lookup"><span data-stu-id="22c37-168">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="22c37-169">tooprevent névtelen hozzáférés tooblobs, set hello engedély paraméter túl**ki**.</span><span class="sxs-lookup"><span data-stu-id="22c37-169">tooprevent anonymous access tooblobs, set hello Permission parameter too**Off**.</span></span> <span data-ttu-id="22c37-170">Alapértelmezés szerint a hello új tároló privát, és csak a fiók tulajdonosának hello keresztül elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="22c37-170">By default, hello new container is private and can be accessed only by hello account owner.</span></span> <span data-ttu-id="22c37-171">tooallow névtelen nyilvános olvasási hozzáférés tooblob erőforrásokat, de nem toocontainer metaadatok vagy toohello listája hello tárolóban lévő blobok, hello engedély paraméter értéke túl**Blob**.</span><span class="sxs-lookup"><span data-stu-id="22c37-171">tooallow anonymous public read access tooblob resources, but not toocontainer metadata or toohello list of blobs in hello container, set hello Permission parameter too**Blob**.</span></span> <span data-ttu-id="22c37-172">tooallow teljes nyilvános olvasási tooblob erőforrások eléréséhez, a tároló metaadatait, és a hello tárolóban lévő blobok hello listája, hello engedély paraméter értéke túl**tároló**.</span><span class="sxs-lookup"><span data-stu-id="22c37-172">tooallow full public read access tooblob resources, container metadata, and hello list of blobs in hello container, set hello Permission parameter too**Container**.</span></span> <span data-ttu-id="22c37-173">További információkért lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="22c37-173">For more information, see [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md).</span></span>
>
>

### <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="22c37-174">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="22c37-174">Upload a blob into a container</span></span>
<span data-ttu-id="22c37-175">Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="22c37-175">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="22c37-176">További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="22c37-176">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="22c37-177">tooa tárolóban lévő blobok tooupload, hello használható `azure storage blob upload`.</span><span class="sxs-lookup"><span data-stu-id="22c37-177">tooupload blobs in tooa container, you can use hello `azure storage blob upload`.</span></span> <span data-ttu-id="22c37-178">Alapértelmezés szerint ez a parancs hello helyi fájlok tooa blokkblob feltöltését.</span><span class="sxs-lookup"><span data-stu-id="22c37-178">By default, this command uploads hello local files tooa block blob.</span></span> <span data-ttu-id="22c37-179">toospecify hello típus hello BLOB, hello használható `--blobtype` paraméter.</span><span class="sxs-lookup"><span data-stu-id="22c37-179">toospecify hello type for hello blob, you can use hello `--blobtype` parameter.</span></span>

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a><span data-ttu-id="22c37-180">Blobok letöltése a tárolóból</span><span class="sxs-lookup"><span data-stu-id="22c37-180">Download blobs from a container</span></span>
<span data-ttu-id="22c37-181">hello a következő példa bemutatja, hogyan toodownload blobok a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="22c37-181">hello following example demonstrates how toodownload blobs from a container.</span></span>

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a><span data-ttu-id="22c37-182">Blobok másolása</span><span class="sxs-lookup"><span data-stu-id="22c37-182">Copy blobs</span></span>
<span data-ttu-id="22c37-183">Tárfiókokon és régiókon belül vagy azok között aszinkron módon másolhatja át a blobokat.</span><span class="sxs-lookup"><span data-stu-id="22c37-183">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="22c37-184">hello következő példa bemutatja, hogyan toocopy blobok a egy tárolási fiók tooanother.</span><span class="sxs-lookup"><span data-stu-id="22c37-184">hello following example demonstrates how toocopy blobs from one storage account tooanother.</span></span> <span data-ttu-id="22c37-185">Ez a példa azt létrehozni egy tárolót, amelyben blobokat nyilvánosan, is névtelenül érhető el.</span><span class="sxs-lookup"><span data-stu-id="22c37-185">In this sample we create a container where blobs are publicly, anonymously accessible.</span></span>

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

<span data-ttu-id="22c37-186">Ez a minta egy aszinkron másolatot hajt végre.</span><span class="sxs-lookup"><span data-stu-id="22c37-186">This sample performs an asynchronous copy.</span></span> <span data-ttu-id="22c37-187">Figyelheti a hello állapotát minden egyes másolási művelet hello futtatásával `azure storage blob copy show` műveletet.</span><span class="sxs-lookup"><span data-stu-id="22c37-187">You can monitor hello status of each copy operation by running hello `azure storage blob copy show` operation.</span></span>

<span data-ttu-id="22c37-188">Vegye figyelembe, hogy hello forrás URL-címe hello másolási művelet a megadott kell nyilvánosan hozzáférhető, vagy egy (közös hozzáférésű jogosultságkód) SAS-jogkivonatot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="22c37-188">Note that hello source URL provided for hello copy operation must either be publicly accessible, or include a SAS (shared access signature) token.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="22c37-189">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="22c37-189">Delete a blob</span></span>
<span data-ttu-id="22c37-190">toodelete blob, hello az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="22c37-190">toodelete a blob, use hello below command:</span></span>

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="22c37-191">Hozzon létre és fájlmegosztások kezelése</span><span class="sxs-lookup"><span data-stu-id="22c37-191">Create and manage file shares</span></span>
<span data-ttu-id="22c37-192">Az Azure File storage közös tárterületet hello szabványos SMB protokollt használó alkalmazások számára biztosít.</span><span class="sxs-lookup"><span data-stu-id="22c37-192">Azure File storage offers shared storage for applications using hello standard SMB protocol.</span></span> <span data-ttu-id="22c37-193">A Microsoft Azure virtuális gépek és felhőszolgáltatások, valamint a helyszíni alkalmazások megosztott csatlakoztatott megosztásokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="22c37-193">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="22c37-194">A fájlmegosztások és a fájladatok hello Azure CLI segítségével kezelheti.</span><span class="sxs-lookup"><span data-stu-id="22c37-194">You can manage file shares and file data via hello Azure CLI.</span></span> <span data-ttu-id="22c37-195">Az Azure File storage további információkért lásd: [Ismerkedés az Azure File storage on Windows](storage-dotnet-how-to-use-files.md) vagy [hogyan toouse Linux Azure File storage](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="22c37-195">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) or [How toouse Azure File storage with Linux](storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="22c37-196">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22c37-196">Create a file share</span></span>
<span data-ttu-id="22c37-197">Egy Azure fájlmegosztás egy SMB-fájlmegosztás, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="22c37-197">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="22c37-198">Minden könyvtárak és fájlok fájlmegosztást kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="22c37-198">All directories and files must be created in a file share.</span></span> <span data-ttu-id="22c37-199">Egy fiók korlátlan számú megosztást tartalmazhat, és egy megosztási tárolhatók fájlok mentése toohello kapacitáskorlátait hello tárfiók korlátlan számú.</span><span class="sxs-lookup"><span data-stu-id="22c37-199">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up toohello capacity limits of hello storage account.</span></span> <span data-ttu-id="22c37-200">hello alábbi példa létrehoz egy nevű fájlmegosztás **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="22c37-200">hello following example creates a file share named **myshare**.</span></span>

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="22c37-201">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="22c37-201">Create a directory</span></span>
<span data-ttu-id="22c37-202">Egy könyvtár egy választható hierarchikus struktúra biztosít az Azure fájlmegosztások.</span><span class="sxs-lookup"><span data-stu-id="22c37-202">A directory provides an optional hierarchical structure for an Azure file share.</span></span> <span data-ttu-id="22c37-203">hello alábbi példa létrehoz egy könyvtárat nevű **könyvtárnév** hello fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="22c37-203">hello following example creates a directory named **myDir** in hello file share.</span></span>

```azurecli
azure storage directory create myshare myDir
```

<span data-ttu-id="22c37-204">Vegye figyelembe, hogy a könyvtár elérési útja tartalmazhatnak több szintjéről *pl.*, **a / b**.</span><span class="sxs-lookup"><span data-stu-id="22c37-204">Note that directory path can include multiple levels, *e.g.*, **a/b**.</span></span> <span data-ttu-id="22c37-205">Azonban úgy kell beállítania, hogy létezik-e az összes fölérendelt könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="22c37-205">However, you must ensure that all parent directories exist.</span></span> <span data-ttu-id="22c37-206">Például a következő elérési út **a / b**, létre kell hoznia könyvtárat **egy** először, majd hozza létre a könyvtár **b**.</span><span class="sxs-lookup"><span data-stu-id="22c37-206">For example, for path **a/b**, you must create directory **a** first, then create directory **b**.</span></span>

### <a name="upload-a-local-file-toodirectory"></a><span data-ttu-id="22c37-207">Egy helyi fájl toodirectory feltöltése</span><span class="sxs-lookup"><span data-stu-id="22c37-207">Upload a local file toodirectory</span></span>
<span data-ttu-id="22c37-208">hello alábbi példa feltölt egy fájlt a **~/temp/samplefile.txt** toohello **könyvtárnév** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="22c37-208">hello following example uploads a file from **~/temp/samplefile.txt** toohello **myDir** directory.</span></span> <span data-ttu-id="22c37-209">Szerkesztése hello fájl elérési útját, hogy tooa érvényes fájlt a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="22c37-209">Edit hello file path so that it points tooa valid file on your local machine:</span></span>

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

<span data-ttu-id="22c37-210">Figyelje meg, hogy hello megosztáson található, a fájl mentése too1 TB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="22c37-210">Note that a file in hello share can be up too1 TB in size.</span></span>

### <a name="list-hello-files-in-hello-share-root-or-directory"></a><span data-ttu-id="22c37-211">Hello fájlok listázása a hello megosztás legfelső szintű vagy a könyvtár</span><span class="sxs-lookup"><span data-stu-id="22c37-211">List hello files in hello share root or directory</span></span>
<span data-ttu-id="22c37-212">Hello fájlok és alkönyvtárak megosztás legfelső szintű vagy egy könyvtár a következő parancs hello segítségével jeleníthetők meg:</span><span class="sxs-lookup"><span data-stu-id="22c37-212">You can list hello files and subdirectories in a share root or a directory using hello following command:</span></span>

```azurecli
azure storage file list myshare myDir
```

<span data-ttu-id="22c37-213">Vegye figyelembe, hogy hello könyvtárnév hello listázása művelet esetén nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="22c37-213">Note that hello directory name is optional for hello listing operation.</span></span> <span data-ttu-id="22c37-214">Ha nincs megadva, hello parancs hello megosztás hello gyökérkönyvtárában hello tartalmát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22c37-214">If omitted, hello command lists hello contents of hello root directory of hello share.</span></span>

### <a name="copy-files"></a><span data-ttu-id="22c37-215">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="22c37-215">Copy files</span></span>
<span data-ttu-id="22c37-216">Azure CLI 0.9.8-as verzióját kezdve másolhatja egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájlt.</span><span class="sxs-lookup"><span data-stu-id="22c37-216">Beginning with version 0.9.8 of Azure CLI, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="22c37-217">Az alábbiakban bemutatjuk, hogyan tooperform ezek másolása műveletekbe parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="22c37-217">Below we demonstrate how tooperform these copy operations using CLI commands.</span></span> <span data-ttu-id="22c37-218">toocopy fájl toohello új könyvtár:</span><span class="sxs-lookup"><span data-stu-id="22c37-218">toocopy a file toohello new directory:</span></span>

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

<span data-ttu-id="22c37-219">toocopy blob tooa fájl könyvtár:</span><span class="sxs-lookup"><span data-stu-id="22c37-219">toocopy a blob tooa file directory:</span></span>

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a><span data-ttu-id="22c37-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="22c37-220">Next Steps</span></span>

<span data-ttu-id="22c37-221">Azure CLI 1.0 parancsdokumentációja találhat itt tároló-erőforrások használata:</span><span class="sxs-lookup"><span data-stu-id="22c37-221">You can find Azure CLI 1.0 command reference for working with Storage resources here:</span></span>

* [<span data-ttu-id="22c37-222">Az Azure parancssori felület parancsait erőforrás-kezelő módban</span><span class="sxs-lookup"><span data-stu-id="22c37-222">Azure CLI commands in Resource Manager mode</span></span>](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [<span data-ttu-id="22c37-223">Az Azure parancssori felület parancsait Azure szolgáltatásfelügyelet módban</span><span class="sxs-lookup"><span data-stu-id="22c37-223">Azure CLI commands in Azure Service Management mode</span></span>](../cli-install-nodejs.md)

<span data-ttu-id="22c37-224">Akkor is előfordulhat, hogy például a tootry hello [Azure CLI 2.0](storage-azure-cli.md), a következő generációs CLI hello Resource Manager üzembe helyezési modellben való használatra, pythonban írt.</span><span class="sxs-lookup"><span data-stu-id="22c37-224">You may also like tootry hello [Azure CLI 2.0](storage-azure-cli.md), our next-generation CLI written in Python, for use with hello Resource Manager deployment model.</span></span>

[Image1]: ./media/storage-azure-cli/azure_command.png
