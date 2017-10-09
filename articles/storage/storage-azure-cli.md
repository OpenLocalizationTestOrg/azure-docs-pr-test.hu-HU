---
title: az Azure Storage Azure CLI 2.0 aaaUsing hello |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure parancssori felület (CLI) 2.0 az Azure Storage toocreate és a storage-fiókok kezelése és az Azure-blobokat és fájlok használatát. hello Azure CLI 2.0 pythonban írt platformfüggetlen eszköz."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: marsma
ms.openlocfilehash: 38402373dcd87f1ef05471a02353c77d58f1a9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-20-with-azure-storage"></a><span data-ttu-id="8ca41-104">Az Azure Storage hello Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="8ca41-104">Using hello Azure CLI 2.0 with Azure Storage</span></span>

<span data-ttu-id="8ca41-105">hello nyílt forráskódú, platformok közötti Azure CLI 2.0 parancsokat biztosít a hello Azure platformon való munkához.</span><span class="sxs-lookup"><span data-stu-id="8ca41-105">hello open-source, cross-platform Azure CLI 2.0 provides a set of commands for working with hello Azure platform.</span></span> <span data-ttu-id="8ca41-106">Nagy részét hello hello található ugyanezeket a funkciókat biztosít [Azure-portálon](https://portal.azure.com), beleértve a funkciógazdag adatelérési.</span><span class="sxs-lookup"><span data-stu-id="8ca41-106">It provides much of hello same functionality found in hello [Azure portal](https://portal.azure.com), including rich data access.</span></span>

<span data-ttu-id="8ca41-107">Az útmutató azt mutatja be toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform működik-e az Azure-tárfiók erőforrásokat több feladatot.</span><span class="sxs-lookup"><span data-stu-id="8ca41-107">In this guide, we show you how toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform several tasks working with resources in your Azure Storage account.</span></span> <span data-ttu-id="8ca41-108">Azt javasoljuk, hogy letöltése és telepítése vagy frissítése toohello hello CLI 2.0 legújabb verzióját az útmutató használata előtt.</span><span class="sxs-lookup"><span data-stu-id="8ca41-108">We recommend that you download and install or upgrade toohello latest version of hello CLI 2.0 before using this guide.</span></span>

<span data-ttu-id="8ca41-109">hello útmutatóban hello példák feltételezik hello Ubuntu Bash rendszerhéjat hello használatát, de más platformokon hasonló módon végre kell hajtania.</span><span class="sxs-lookup"><span data-stu-id="8ca41-109">hello examples in hello guide assume hello use of hello Bash shell on Ubuntu, but other platforms should perform similarly.</span></span> 

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a><span data-ttu-id="8ca41-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8ca41-110">Prerequisites</span></span>
<span data-ttu-id="8ca41-111">Ez az útmutató feltételezi, hogy tudomásul veszi hello Azure Storage alapvető fogalmait.</span><span class="sxs-lookup"><span data-stu-id="8ca41-111">This guide assumes that you understand hello basic concepts of Azure Storage.</span></span> <span data-ttu-id="8ca41-112">Azt is feltételezi, hogy Ön képes toosatisfy hello létrehozása követelmények az Azure és a Storage szolgáltatás hello alatt megadott.</span><span class="sxs-lookup"><span data-stu-id="8ca41-112">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Storage service.</span></span>

### <a name="accounts"></a><span data-ttu-id="8ca41-113">Fiókok</span><span class="sxs-lookup"><span data-stu-id="8ca41-113">Accounts</span></span>
* <span data-ttu-id="8ca41-114">**Azure-fiók**: Ha még nem rendelkezik Azure-előfizetéssel, [egy ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="8ca41-114">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="8ca41-115">**Tárfiók**: Lásd a [Tudnivalók az Azure Storage-fiókokról](../storage/storage-create-storage-account.md) cikk [Tárfiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) szakaszát.</span><span class="sxs-lookup"><span data-stu-id="8ca41-115">**Storage account**: See [Create a storage account](../storage/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/storage-create-storage-account.md).</span></span>

### <a name="install-hello-azure-cli-20"></a><span data-ttu-id="8ca41-116">Hello Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="8ca41-116">Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="8ca41-117">Töltse le és telepítse az Azure CLI 2.0 hello hello ismertetett lépéseket követve [Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="8ca41-117">Download and install hello Azure CLI 2.0 by following hello instructions outlined in [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

> [!TIP]
> <span data-ttu-id="8ca41-118">Ha problémája merül fel a hello telepítés, tekintse meg a hello [telepítési hibák elhárítása](/cli/azure/install-az-cli2#installation-troubleshooting) hello cikk és hello [telepítése hibaelhárítási](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) útmutató a Githubon.</span><span class="sxs-lookup"><span data-stu-id="8ca41-118">If you have trouble with hello installation, check out hello [Installation Troubleshooting](/cli/azure/install-az-cli2#installation-troubleshooting) section of hello article, and hello [Install Troubleshooting](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) guide on GitHub.</span></span>
>

## <a name="working-with-hello-cli"></a><span data-ttu-id="8ca41-119">Hello CLI használata</span><span class="sxs-lookup"><span data-stu-id="8ca41-119">Working with hello CLI</span></span>

<span data-ttu-id="8ca41-120">Hello parancssori felület telepítése után is használhatja hello `az` parancsot a parancssori felület (Bash, terminált, parancssor) tooaccess hello Azure parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="8ca41-120">Once you've installed hello CLI, you can use hello `az` command in your command-line interface (Bash, Terminal, Command Prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="8ca41-121">Típus hello `az` parancs toosee hello alap parancsokat (a következő egy példa a kimenetre hello csonkolódtak) teljes listáját:</span><span class="sxs-lookup"><span data-stu-id="8ca41-121">Type hello `az` command toosee a full list of hello base commands (hello following example output has been truncated):</span></span>

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome toohello cool new Azure CLI!

Here are hello base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

<span data-ttu-id="8ca41-122">A parancssori felület hello parancs végrehajtása `az storage --help` toolist hello `storage` alcsoportokat parancsot.</span><span class="sxs-lookup"><span data-stu-id="8ca41-122">In your command-line interface, execute hello command `az storage --help` toolist hello `storage` command subgroups.</span></span> <span data-ttu-id="8ca41-123">hello alcsoportokat hello leírását az nyújt áttekintést hello funkció hello Azure parancssori Felületet biztosít a tároló-erőforrások használata.</span><span class="sxs-lookup"><span data-stu-id="8ca41-123">hello descriptions of hello subgroups provide an overview of hello functionality hello Azure CLI provides for working with your storage resources.</span></span>

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use hello standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues tooeffectively scale applications according tootraffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-hello-cli-tooyour-azure-subscription"></a><span data-ttu-id="8ca41-124">Csatlakozás hello CLI tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="8ca41-124">Connect hello CLI tooyour Azure subscription</span></span>

<span data-ttu-id="8ca41-125">az Azure-előfizetéshez erőforrásokat hello toowork, kell először jelentkezik be a tooyour az Azure-fiók `az login`.</span><span class="sxs-lookup"><span data-stu-id="8ca41-125">toowork with hello resources in your Azure subscription, you must first log in tooyour Azure account with `az login`.</span></span> <span data-ttu-id="8ca41-126">Többféleképpen is bejelentkezhet:</span><span class="sxs-lookup"><span data-stu-id="8ca41-126">There are several ways you can log in:</span></span>

* <span data-ttu-id="8ca41-127">**Interaktív bejelentkezés**:`az login`</span><span class="sxs-lookup"><span data-stu-id="8ca41-127">**Interactive login**: `az login`</span></span>
* <span data-ttu-id="8ca41-128">**Jelentkezzen be a felhasználónevet és jelszót**:`az login -u johndoe@contoso.com -p VerySecret`</span><span class="sxs-lookup"><span data-stu-id="8ca41-128">**Log in with user name and password**: `az login -u johndoe@contoso.com -p VerySecret`</span></span>
  * <span data-ttu-id="8ca41-129">A Microsoft és fiókok számára, hogy a többtényezős hitelesítés használata mellett nem működik.</span><span class="sxs-lookup"><span data-stu-id="8ca41-129">This doesn't work with Microsoft accounts or accounts that use multi-factor authentication.</span></span>
* <span data-ttu-id="8ca41-130">**Jelentkezzen be egy egyszerű**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="8ca41-130">**Log in with a service principal**: `az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span></span>

## <a name="azure-cli-20-sample-script"></a><span data-ttu-id="8ca41-131">Az Azure CLI 2.0 parancsfájlpéldát</span><span class="sxs-lookup"><span data-stu-id="8ca41-131">Azure CLI 2.0 sample script</span></span>

<span data-ttu-id="8ca41-132">Dolgozunk lesz ezután az egy kis héjparancsfájlt, amely néhány alapvető Azure CLI 2.0 parancsok toointeract állít ki az Azure Storage-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="8ca41-132">Next, we'll work with a small shell script that issues a few basic Azure CLI 2.0 commands toointeract with Azure Storage resources.</span></span> <span data-ttu-id="8ca41-133">hello parancsfájl először létrehoz egy új tárolót a tárfiókban lévő, majd feltölti a meglévő fájlt (a blob) toothat tároló.</span><span class="sxs-lookup"><span data-stu-id="8ca41-133">hello script first creates a new container in your storage account, then uploads an existing file (as a blob) toothat container.</span></span> <span data-ttu-id="8ca41-134">Majd felsorolja az összes BLOB hello tárolóban, és végül letölti a hello fájl tooa cél a helyi számítógépen, amely akkor adja meg.</span><span class="sxs-lookup"><span data-stu-id="8ca41-134">It then lists all blobs in hello container, and finally, downloads hello file tooa destination on your local computer that you specify.</span></span>

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating hello container..."
az storage container create --name $container_name

echo "Uploading hello file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing hello blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading hello file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

<span data-ttu-id="8ca41-135">**Konfigurálja és hello parancsprogrammal**</span><span class="sxs-lookup"><span data-stu-id="8ca41-135">**Configure and run hello script**</span></span>

1. <span data-ttu-id="8ca41-136">Nyissa meg a kedvenc szövegszerkesztőjével, majd másolja, és illessze be a parancsfájl megelőző be hello szerkesztő hello.</span><span class="sxs-lookup"><span data-stu-id="8ca41-136">Open your favorite text editor, then copy and paste hello preceding script into hello editor.</span></span>

2. <span data-ttu-id="8ca41-137">Következő lépésként frissítse hello parancsfájl változók tooreflect a konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="8ca41-137">Next, update hello script's variables tooreflect your configuration settings.</span></span> <span data-ttu-id="8ca41-138">Cserélje le a következő értékeket a megadott hello:</span><span class="sxs-lookup"><span data-stu-id="8ca41-138">Replace hello following values as specified:</span></span>

   * <span data-ttu-id="8ca41-139">**\<storage_account_name\>**  hello a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="8ca41-139">**\<storage_account_name\>** hello name of your storage account.</span></span>
   * <span data-ttu-id="8ca41-140">**\<storage_account_key\>**  hello elsődleges vagy másodlagos elérési kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="8ca41-140">**\<storage_account_key\>** hello primary or secondary access key for your storage account.</span></span>
   * <span data-ttu-id="8ca41-141">**\<container_name\>**  egy nevet az új tároló toocreate, például az "azure-cli-minta-container" hello.</span><span class="sxs-lookup"><span data-stu-id="8ca41-141">**\<container_name\>** A name hello new container toocreate, such as "azure-cli-sample-container".</span></span>
   * <span data-ttu-id="8ca41-142">**\<blob_name\>**  hello cél blob hello tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="8ca41-142">**\<blob_name\>** A name for hello destination blob in hello container.</span></span>
   * <span data-ttu-id="8ca41-143">**\<file_to_upload\>**  toosmall-fájl elérési útja a helyi számítógépen, például a hello "~ / images/HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="8ca41-143">**\<file_to_upload\>** hello path toosmall file on your local computer, such as "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="8ca41-144">**\<destination_file\>**  hello a cél elérési utat, például a "~ / downloadedImage.png".</span><span class="sxs-lookup"><span data-stu-id="8ca41-144">**\<destination_file\>** hello destination file path, such as "~/downloadedImage.png".</span></span>

3. <span data-ttu-id="8ca41-145">Hello szükséges változók frissítése után mentse hello parancsfájlt, és zárja be a szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="8ca41-145">After you've updated hello necessary variables, save hello script and exit your editor.</span></span> <span data-ttu-id="8ca41-146">hello következő lépések azt feltételezik, hogy a parancsfájl nevű **my_storage_sample.sh**.</span><span class="sxs-lookup"><span data-stu-id="8ca41-146">hello next steps assume you've named your script **my_storage_sample.sh**.</span></span>

4. <span data-ttu-id="8ca41-147">Szükség esetén jelölje meg hello parancsfájl végrehajtható, mint:`chmod +x my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="8ca41-147">Mark hello script as executable, if necessary: `chmod +x my_storage_sample.sh`</span></span>

5. <span data-ttu-id="8ca41-148">Hello parancsfájlok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="8ca41-148">Execute hello script.</span></span> <span data-ttu-id="8ca41-149">Például a Bash:`./my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="8ca41-149">For example, in Bash: `./my_storage_sample.sh`</span></span>

<span data-ttu-id="8ca41-150">Kell kimeneti hasonló toohello következő, és hello  **\<destination_file\>**  hello megadott parancsfájl megjelenjen-e a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8ca41-150">You should see output similar toohello following, and hello **\<destination_file\>** you specified in hello script should appear on your local computer.</span></span>

```
Creating hello container...
{
  "created": true
}
Uploading hello file...
Percent complete: %100.0
Listing hello blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading hello file...
Name
---------
README.md
Done
```

> [!TIP]
> <span data-ttu-id="8ca41-151">hello előző kimeneti van **tábla** formátumban.</span><span class="sxs-lookup"><span data-stu-id="8ca41-151">hello preceding output is in **table** format.</span></span> <span data-ttu-id="8ca41-152">Megadhatja, amely a kimeneti formátum toouse hello megadásával `--output` argumentumának a parancssori felület parancsait, vagy állítsa az segítségével globálisan `az configure`.</span><span class="sxs-lookup"><span data-stu-id="8ca41-152">You can specify which output format toouse by specifying hello `--output` argument in your CLI commands, or set it globally using `az configure`.</span></span>
>

## <a name="manage-storage-accounts"></a><span data-ttu-id="8ca41-153">Tárfiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="8ca41-153">Manage storage accounts</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="8ca41-154">Új tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ca41-154">Create a new storage account</span></span>
<span data-ttu-id="8ca41-155">Azure Storage toouse, tároló-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="8ca41-155">toouse Azure Storage, you need a storage account.</span></span> <span data-ttu-id="8ca41-156">A számítógép túl beállítása után létrehozhat egy új Azure Storage-fiók[tooyour előfizetés csatlakozás](#connect-to-your-azure-subscription).</span><span class="sxs-lookup"><span data-stu-id="8ca41-156">You can create a new Azure Storage account after you've configured your computer too[connect tooyour subscription](#connect-to-your-azure-subscription).</span></span>

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* <span data-ttu-id="8ca41-157">`--location`[Szükséges]: helyét.</span><span class="sxs-lookup"><span data-stu-id="8ca41-157">`--location` [Required]: Location.</span></span> <span data-ttu-id="8ca41-158">Például "USA nyugati régiója".</span><span class="sxs-lookup"><span data-stu-id="8ca41-158">For example, "West US".</span></span>
* <span data-ttu-id="8ca41-159">`--name`[Szükséges]: hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="8ca41-159">`--name` [Required]: hello storage account name.</span></span> <span data-ttu-id="8ca41-160">hello neve 3 too24 karakter hosszúságúnak kell, és csak kisbetűs alfanumerikus karaktereket használjon.</span><span class="sxs-lookup"><span data-stu-id="8ca41-160">hello name must be 3 too24 characters in length, and use only lowercase alphanumeric characters.</span></span>
* <span data-ttu-id="8ca41-161">`--resource-group`[Szükséges]: erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="8ca41-161">`--resource-group` [Required]: Name of resource group.</span></span>
* <span data-ttu-id="8ca41-162">`--sku`[Szükséges]: hello tárfiók Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="8ca41-162">`--sku` [Required]: hello storage account SKU.</span></span> <span data-ttu-id="8ca41-163">Megengedett értékek:</span><span class="sxs-lookup"><span data-stu-id="8ca41-163">Allowed values:</span></span>
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a><span data-ttu-id="8ca41-164">Környezeti változók értékét az alapértelmezett Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="8ca41-164">Set default Azure storage account environment variables</span></span>
<span data-ttu-id="8ca41-165">Az Azure-előfizetéshez több tárfiókot is lehet.</span><span class="sxs-lookup"><span data-stu-id="8ca41-165">You can have multiple storage accounts in your Azure subscription.</span></span> <span data-ttu-id="8ca41-166">az egyik legyen tooselect toouse minden ezt követő tárolási parancsok, ezek a környezeti változók állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="8ca41-166">tooselect one of them toouse for all subsequent storage commands, you can set these environment variables:</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="8ca41-167">Egy másik módja tooset alapértelmezett tárfiók kapcsolati karakterlánc használatával van.</span><span class="sxs-lookup"><span data-stu-id="8ca41-167">Another way tooset a default storage account is by using a connection string.</span></span> <span data-ttu-id="8ca41-168">Először a hello hello kapcsolati karakterlánc beolvasása `show-connection-string` parancs:</span><span class="sxs-lookup"><span data-stu-id="8ca41-168">First, get hello connection string with hello `show-connection-string` command:</span></span>

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

<span data-ttu-id="8ca41-169">Majd a Másolás hello kimeneti kapcsolati karakterláncot, és állítsa be a hello `AZURE_STORAGE_CONNECTION_STRING` környezeti változó (szükség lehet tooenclose hello kapcsolati karakterlánc idézőjelben):</span><span class="sxs-lookup"><span data-stu-id="8ca41-169">Then copy hello output connection string and set hello `AZURE_STORAGE_CONNECTION_STRING` environment variable (you might need tooenclose hello connection string in quotes):</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> <span data-ttu-id="8ca41-170">A következő részekben a cikk hello összes példák feltételezik, hogy beállított hello `AZURE_STORAGE_ACCOUNT` és `AZURE_STORAGE_ACCESS_KEY` környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="8ca41-170">All examples in hello following sections of this article assume that you've set hello `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY` environment variables.</span></span>
>

## <a name="create-and-manage-blobs"></a><span data-ttu-id="8ca41-171">Hozzon létre és blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="8ca41-171">Create and manage blobs</span></span>
<span data-ttu-id="8ca41-172">Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.</span><span class="sxs-lookup"><span data-stu-id="8ca41-172">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="8ca41-173">Ez a szakasz feltételezi, hogy Ön már ismeri a Azure Blob storage fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="8ca41-173">This section assumes that you are already familiar with Azure Blob storage concepts.</span></span> <span data-ttu-id="8ca41-174">Részletes információkért lásd: [az Azure Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](/rest/api/storageservices/blob-service-concepts).</span><span class="sxs-lookup"><span data-stu-id="8ca41-174">For detailed information, see [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](/rest/api/storageservices/blob-service-concepts).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="8ca41-175">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ca41-175">Create a container</span></span>
<span data-ttu-id="8ca41-176">Az Azure storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8ca41-176">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="8ca41-177">Egy tároló hello segítségével létrehozható `az storage container create` parancs:</span><span class="sxs-lookup"><span data-stu-id="8ca41-177">You can create a container by using hello `az storage container create` command:</span></span>

```azurecli
az storage container create --name <container_name>
```

<span data-ttu-id="8ca41-178">Beállíthatja egy három szintje olvasási hozzáférés egy új tároló választható hello megadásával `--public-access` argumentum:</span><span class="sxs-lookup"><span data-stu-id="8ca41-178">You can set one of three levels of read access for a new container by specifying hello optional  `--public-access` argument:</span></span>

* <span data-ttu-id="8ca41-179">`off`(alapértelmezett): tároló adata titkos toohello fiók tulajdonosának.</span><span class="sxs-lookup"><span data-stu-id="8ca41-179">`off` (default): Container data is private toohello account owner.</span></span>
* <span data-ttu-id="8ca41-180">`blob`: Blobok nyilvános olvasási hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="8ca41-180">`blob`: Public read access for blobs.</span></span>
* <span data-ttu-id="8ca41-181">`container`: Nyilvános olvasási és lista hozzáférés toohello teljes tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="8ca41-181">`container`: Public read and list access toohello entire container.</span></span>

<span data-ttu-id="8ca41-182">További információkért lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="8ca41-182">For more information, see [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md).</span></span>

### <a name="upload-a-blob-tooa-container"></a><span data-ttu-id="8ca41-183">Töltse fel a blobtárolóban tooa</span><span class="sxs-lookup"><span data-stu-id="8ca41-183">Upload a blob tooa container</span></span>
<span data-ttu-id="8ca41-184">Az Azure Blob storage blokkméretet támogatja, hozzáfűzése, és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="8ca41-184">Azure Blob storage supports block, append, and page blobs.</span></span> <span data-ttu-id="8ca41-185">Feltöltés tooa tároló blobok hello segítségével `blob upload` parancs:</span><span class="sxs-lookup"><span data-stu-id="8ca41-185">Upload blobs tooa container by using hello `blob upload` command:</span></span>

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 <span data-ttu-id="8ca41-186">Alapértelmezés szerint hello `blob upload` parancs tölt *.vhd fájlok toopage blobot, vagy egyéb blokkblobokat.</span><span class="sxs-lookup"><span data-stu-id="8ca41-186">By default, hello `blob upload` command uploads *.vhd files toopage blobs, or block blobs otherwise.</span></span> <span data-ttu-id="8ca41-187">toospecify egy másik típusa egy blob feltöltése, használhatja a hello `--type` argumentum--engedélyezett értékek `append`, `block`, és `page`.</span><span class="sxs-lookup"><span data-stu-id="8ca41-187">toospecify another type when you upload a blob, you can use hello `--type` argument--allowed values are `append`, `block`, and `page`.</span></span>

 <span data-ttu-id="8ca41-188">A hello másik blob típusok további információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span><span class="sxs-lookup"><span data-stu-id="8ca41-188">For more information on hello different blob types, see [Understanding Block Blobs, Append Blobs, and Page Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span></span>


### <a name="download-a-blob-from-a-container"></a><span data-ttu-id="8ca41-189">Blob letöltése tárolóból</span><span class="sxs-lookup"><span data-stu-id="8ca41-189">Download a blob from a container</span></span>
<span data-ttu-id="8ca41-190">Ez a példa bemutatja, hogyan toodownload tárolókból blob:</span><span class="sxs-lookup"><span data-stu-id="8ca41-190">This example demonstrates how toodownload a blob from a container:</span></span>

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="8ca41-191">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="8ca41-191">List hello blobs in a container</span></span>

<span data-ttu-id="8ca41-192">A tárolóhoz hello hello blobok listázása [az tárolási blob lista](/cli/azure/storage/blob#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="8ca41-192">List hello blobs in a container with hello [az storage blob list](/cli/azure/storage/blob#list) command.</span></span>

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a><span data-ttu-id="8ca41-193">Blobok másolása</span><span class="sxs-lookup"><span data-stu-id="8ca41-193">Copy blobs</span></span>
<span data-ttu-id="8ca41-194">Tárfiókokon és régiókon belül vagy azok között aszinkron módon másolhatja át a blobokat.</span><span class="sxs-lookup"><span data-stu-id="8ca41-194">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="8ca41-195">hello következő példa bemutatja, hogyan toocopy blobok a egy tárolási fiók tooanother.</span><span class="sxs-lookup"><span data-stu-id="8ca41-195">hello following example demonstrates how toocopy blobs from one storage account tooanother.</span></span> <span data-ttu-id="8ca41-196">Azt először létre kell hoznia egy tárolót hello forrás tárfiókot, a benne található blobokat nyilvános olvasási hozzáférés megadása.</span><span class="sxs-lookup"><span data-stu-id="8ca41-196">We first create a container in hello source storage account, specifying public read-access for its blobs.</span></span> <span data-ttu-id="8ca41-197">A következő azt feltöltése egy fájl toohello tárolót, és végül másolási hello blob a tárolóban a cél tárfiókkal hello egy tárolóba.</span><span class="sxs-lookup"><span data-stu-id="8ca41-197">Next, we upload a file toohello container, and finally, copy hello blob from that container into a container in hello destination storage account.</span></span>

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob toocontainer in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account toodestination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

<span data-ttu-id="8ca41-198">A fenti példa hello hello céltárolója már léteznie kell hello cél tárfiókkal hello másolási művelet toosucceed.</span><span class="sxs-lookup"><span data-stu-id="8ca41-198">In hello above example, hello destination container must already exist in hello destination storage account for hello copy operation toosucceed.</span></span> <span data-ttu-id="8ca41-199">Emellett hello forrás blob hello megadott `--source-uri` argumentumot kell egy közös hozzáférésű jogosultságkód (SAS) jogkivonatot, vagy a nyilvánosan hozzáférhető, ebben a példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="8ca41-199">Additionally, hello source blob specified in hello `--source-uri` argument must either include a shared access signature (SAS) token, or be publicly accessible, as in this example.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="8ca41-200">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="8ca41-200">Delete a blob</span></span>
<span data-ttu-id="8ca41-201">egy blob toodelete hello használata `blob delete` parancs:</span><span class="sxs-lookup"><span data-stu-id="8ca41-201">toodelete a blob, use hello `blob delete` command:</span></span>

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="8ca41-202">Hozzon létre és fájlmegosztások kezelése</span><span class="sxs-lookup"><span data-stu-id="8ca41-202">Create and manage file shares</span></span>
<span data-ttu-id="8ca41-203">Az Azure File storage közös tárterületet hello Server Message Block (SMB) protokollt használó alkalmazások számára biztosít.</span><span class="sxs-lookup"><span data-stu-id="8ca41-203">Azure File storage offers shared storage for applications using hello Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="8ca41-204">A Microsoft Azure virtuális gépek és felhőszolgáltatások, valamint a helyszíni alkalmazások megosztott csatlakoztatott megosztásokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="8ca41-204">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="8ca41-205">A fájlmegosztások és a fájladatok hello Azure CLI segítségével kezelheti.</span><span class="sxs-lookup"><span data-stu-id="8ca41-205">You can manage file shares and file data via hello Azure CLI.</span></span> <span data-ttu-id="8ca41-206">Az Azure File storage további információkért lásd: [Ismerkedés az Azure File storage on Windows](storage-dotnet-how-to-use-files.md) vagy [hogyan toouse Linux Azure File storage](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8ca41-206">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) or [How toouse Azure File storage with Linux](storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="8ca41-207">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ca41-207">Create a file share</span></span>
<span data-ttu-id="8ca41-208">Egy Azure fájlmegosztás egy SMB-fájlmegosztás, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8ca41-208">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="8ca41-209">Minden könyvtárak és fájlok fájlmegosztást kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8ca41-209">All directories and files must be created in a file share.</span></span> <span data-ttu-id="8ca41-210">Egy fiók korlátlan számú megosztást tartalmazhat, és egy megosztási tárolhatók fájlok mentése toohello kapacitáskorlátait hello tárfiók korlátlan számú.</span><span class="sxs-lookup"><span data-stu-id="8ca41-210">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up toohello capacity limits of hello storage account.</span></span> <span data-ttu-id="8ca41-211">hello alábbi példa létrehoz egy nevű fájlmegosztás **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="8ca41-211">hello following example creates a file share named **myshare**.</span></span>

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="8ca41-212">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ca41-212">Create a directory</span></span>
<span data-ttu-id="8ca41-213">Egy könyvtárat biztosít az Azure fájlmegosztások hierarchikus struktúra.</span><span class="sxs-lookup"><span data-stu-id="8ca41-213">A directory provides a hierarchical structure in an Azure file share.</span></span> <span data-ttu-id="8ca41-214">hello alábbi példa létrehoz egy könyvtárat nevű **könyvtárnév** hello fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="8ca41-214">hello following example creates a directory named **myDir** in hello file share.</span></span>

```azurecli
az storage directory create --name myDir --share-name myshare
```

<span data-ttu-id="8ca41-215">Az elérési út például tartalmazhatnak több szintjéről **dir1/dir2**.</span><span class="sxs-lookup"><span data-stu-id="8ca41-215">A directory path can include multiple levels, for example **dir1/dir2**.</span></span> <span data-ttu-id="8ca41-216">Azonban úgy kell beállítania, hogy létezik-e minden szülő-könyvtár egy alkönyvtár létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="8ca41-216">However, you must ensure that all parent directories exist before creating a subdirectory.</span></span> <span data-ttu-id="8ca41-217">Például a következő elérési út **dir1/dir2**, először létre kell hoznia directory **dir1**, majd hozza létre a könyvtár **dir2**.</span><span class="sxs-lookup"><span data-stu-id="8ca41-217">For example, for path **dir1/dir2**, you must first create directory **dir1**, then create directory **dir2**.</span></span>

### <a name="upload-a-local-file-tooa-share"></a><span data-ttu-id="8ca41-218">Helyi tooa fájlmegosztás feltöltése</span><span class="sxs-lookup"><span data-stu-id="8ca41-218">Upload a local file tooa share</span></span>
<span data-ttu-id="8ca41-219">hello alábbi példa feltölt egy fájlt a **~/temp/samplefile.txt** a hello tooroot **megosztás** fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="8ca41-219">hello following example uploads a file from **~/temp/samplefile.txt** tooroot of hello **myshare** file share.</span></span> <span data-ttu-id="8ca41-220">Hello `--source` argumentum hello meglévő helyi fájl tooupload határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8ca41-220">hello `--source` argument specifies hello existing local file tooupload.</span></span>

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

<span data-ttu-id="8ca41-221">Csakúgy, mint létrehozni a könyvtárat, adja meg hello megosztás tooupload hello fájl tooan meglévő címtárhoz az hello megosztáson belül az elérési út:</span><span class="sxs-lookup"><span data-stu-id="8ca41-221">As with directory creation, you can specify a directory path within hello share tooupload hello file tooan existing directory within hello share:</span></span>

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

<span data-ttu-id="8ca41-222">Hello megosztáson található, a fájl mentése too1 TB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="8ca41-222">A file in hello share can be up too1 TB in size.</span></span>

### <a name="list-hello-files-in-a-share"></a><span data-ttu-id="8ca41-223">A megosztási hello fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="8ca41-223">List hello files in a share</span></span>
<span data-ttu-id="8ca41-224">Fájlok és könyvtárak olyan megosztáson található is listázhatja hello segítségével `az storage file list` parancs:</span><span class="sxs-lookup"><span data-stu-id="8ca41-224">You can list files and directories in a share by using hello `az storage file list` command:</span></span>

```azurecli
# List hello files in hello root of a share
az storage file list --share-name myshare --output table

# List hello files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List hello files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a><span data-ttu-id="8ca41-225">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="8ca41-225">Copy files</span></span>      
<span data-ttu-id="8ca41-226">Egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájl másolhatja.</span><span class="sxs-lookup"><span data-stu-id="8ca41-226">You can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="8ca41-227">Ha például toocopy különböző megosztásban található fájl tooa könyvtár:</span><span class="sxs-lookup"><span data-stu-id="8ca41-227">For example, toocopy a file tooa directory in a different share:</span></span>        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a><span data-ttu-id="8ca41-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ca41-228">Next steps</span></span>
<span data-ttu-id="8ca41-229">Az alábbiakban néhány további források további hello Azure CLI 2.0 használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8ca41-229">Here are some additional resources for learning more about working with hello Azure CLI 2.0.</span></span>

* [<span data-ttu-id="8ca41-230">Ismerkedés az Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8ca41-230">Get started with Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="8ca41-231">Az Azure CLI 2.0 parancsdokumentációja</span><span class="sxs-lookup"><span data-stu-id="8ca41-231">Azure CLI 2.0 command reference</span></span>](/cli/azure)
* [<span data-ttu-id="8ca41-232">Az Azure CLI 2.0 a Githubon</span><span class="sxs-lookup"><span data-stu-id="8ca41-232">Azure CLI 2.0 on GitHub</span></span>](https://github.com/Azure/azure-cli)
