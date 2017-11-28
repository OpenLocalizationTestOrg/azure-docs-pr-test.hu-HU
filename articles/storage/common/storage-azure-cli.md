---
title: "Az Azure parancssori felület 2.0 használatával az Azure Storage |} Microsoft Docs"
description: "Megtudhatja, hogyan használható az Azure parancssori felület (CLI) 2.0 az Azure Storage létrehozása és a storage-fiókok kezelése és a munkahelyi Azure-blobokat és fájlokat. Az Azure CLI 2.0 pythonban írt platformfüggetlen eszköz."
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
ms.openlocfilehash: 8dfa91de25eadb93186d994095f0a0107fe1a9d0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-cli-20-with-azure-storage"></a><span data-ttu-id="93cf3-104">Az Azure parancssori felület 2.0 használatával az Azure Storage</span><span class="sxs-lookup"><span data-stu-id="93cf3-104">Using the Azure CLI 2.0 with Azure Storage</span></span>

<span data-ttu-id="93cf3-105">A nyílt forráskódú, platformok közötti Azure CLI 2.0 parancsokat biztosít az Azure platformon való munkához.</span><span class="sxs-lookup"><span data-stu-id="93cf3-105">The open-source, cross-platform Azure CLI 2.0 provides a set of commands for working with the Azure platform.</span></span> <span data-ttu-id="93cf3-106">Nagy része megtalálható ugyanezeket a funkciókat biztosít a [Azure-portálon](https://portal.azure.com), beleértve a funkciógazdag adatelérési.</span><span class="sxs-lookup"><span data-stu-id="93cf3-106">It provides much of the same functionality found in the [Azure portal](https://portal.azure.com), including rich data access.</span></span>

<span data-ttu-id="93cf3-107">Ebben az útmutatóban megmutatjuk, hogyan használható a [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) működik-e az Azure-tárfiók erőforrásokat több feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="93cf3-107">In this guide, we show you how to use the [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) to perform several tasks working with resources in your Azure Storage account.</span></span> <span data-ttu-id="93cf3-108">Azt javasoljuk, hogy letöltése és telepítése vagy frissítése a legújabb verzióra a CLI 2.0 az útmutató használata előtt.</span><span class="sxs-lookup"><span data-stu-id="93cf3-108">We recommend that you download and install or upgrade to the latest version of the CLI 2.0 before using this guide.</span></span>

<span data-ttu-id="93cf3-109">Az útmutatóban szereplő példák a Bash rendszerhéjat Ubuntu használatát feltételezik, de más platformokon hasonló módon végre kell hajtania.</span><span class="sxs-lookup"><span data-stu-id="93cf3-109">The examples in the guide assume the use of the Bash shell on Ubuntu, but other platforms should perform similarly.</span></span> 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a><span data-ttu-id="93cf3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="93cf3-110">Prerequisites</span></span>
<span data-ttu-id="93cf3-111">Ez az útmutató feltételezi, hogy tudomásul veszi az Azure Storage alapvető fogalmait.</span><span class="sxs-lookup"><span data-stu-id="93cf3-111">This guide assumes that you understand the basic concepts of Azure Storage.</span></span> <span data-ttu-id="93cf3-112">Azt is feltételezi, hogy megfelelnek a fiók létrehozása az Azure és a tárolás szolgáltatás alatt megadott esetén van.</span><span class="sxs-lookup"><span data-stu-id="93cf3-112">It also assumes that you're able to satisfy the account creation requirements that are specified below for Azure and the Storage service.</span></span>

### <a name="accounts"></a><span data-ttu-id="93cf3-113">Fiókok</span><span class="sxs-lookup"><span data-stu-id="93cf3-113">Accounts</span></span>
* <span data-ttu-id="93cf3-114">**Azure-fiók**: Ha még nem rendelkezik Azure-előfizetéssel, [egy ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="93cf3-114">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="93cf3-115">**Tárfiók**: Lásd a [Tudnivalók az Azure Storage-fiókokról](storage-create-storage-account.md) cikk [Tárfiók létrehozása](storage-create-storage-account.md#create-a-storage-account) szakaszát.</span><span class="sxs-lookup"><span data-stu-id="93cf3-115">**Storage account**: See [Create a storage account](storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](storage-create-storage-account.md).</span></span>

### <a name="install-the-azure-cli-20"></a><span data-ttu-id="93cf3-116">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="93cf3-116">Install the Azure CLI 2.0</span></span>

<span data-ttu-id="93cf3-117">Töltse le és telepítse az Azure CLI 2.0 ismertetett lépéseket követve [Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="93cf3-117">Download and install the Azure CLI 2.0 by following the instructions outlined in [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

> [!TIP]
> <span data-ttu-id="93cf3-118">Ha problémája merül fel a a telepítés, tekintse meg a [telepítési hibák elhárítása](/cli/azure/install-az-cli2#installation-troubleshooting) cikkének, és a [telepítése hibaelhárítási](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) útmutató a Githubon.</span><span class="sxs-lookup"><span data-stu-id="93cf3-118">If you have trouble with the installation, check out the [Installation Troubleshooting](/cli/azure/install-az-cli2#installation-troubleshooting) section of the article, and the [Install Troubleshooting](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) guide on GitHub.</span></span>
>

## <a name="working-with-the-cli"></a><span data-ttu-id="93cf3-119">A parancssori felület használata</span><span class="sxs-lookup"><span data-stu-id="93cf3-119">Working with the CLI</span></span>

<span data-ttu-id="93cf3-120">A parancssori felület telepítése után is használhatja a `az` parancsot a parancssori felület (Bash, terminált, parancssor) az Azure parancssori felület parancsait eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="93cf3-120">Once you've installed the CLI, you can use the `az` command in your command-line interface (Bash, Terminal, Command Prompt) to access the Azure CLI commands.</span></span> <span data-ttu-id="93cf3-121">Típus a `az` parancsot az alap parancsokat (a következő egy példa a kimenetre csonkolódtak) teljes listájának megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="93cf3-121">Type the `az` command to see a full list of the base commands (the following example output has been truncated):</span></span>

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome to the cool new Azure CLI!

Here are the base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

<span data-ttu-id="93cf3-122">A parancssori felületen, a parancs végrehajtása `az storage --help` listájára a `storage` alcsoportokat parancsot.</span><span class="sxs-lookup"><span data-stu-id="93cf3-122">In your command-line interface, execute the command `az storage --help` to list the `storage` command subgroups.</span></span> <span data-ttu-id="93cf3-123">A alcsoportokat leírásait nyújt áttekintést. a funkció az Azure parancssori felület biztosít a tároló-erőforrások használata.</span><span class="sxs-lookup"><span data-stu-id="93cf3-123">The descriptions of the subgroups provide an overview of the functionality the Azure CLI provides for working with your storage resources.</span></span>

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
    file     : File shares that use the standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues to effectively scale applications according to traffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-the-cli-to-your-azure-subscription"></a><span data-ttu-id="93cf3-124">A parancssori felület csatlakozni az Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="93cf3-124">Connect the CLI to your Azure subscription</span></span>

<span data-ttu-id="93cf3-125">Az Azure-előfizetés az erőforrások használatához kell először jelentkezik be az Azure-fiókja `az login`.</span><span class="sxs-lookup"><span data-stu-id="93cf3-125">To work with the resources in your Azure subscription, you must first log in to your Azure account with `az login`.</span></span> <span data-ttu-id="93cf3-126">Többféleképpen is bejelentkezhet:</span><span class="sxs-lookup"><span data-stu-id="93cf3-126">There are several ways you can log in:</span></span>

* <span data-ttu-id="93cf3-127">**Interaktív bejelentkezés**:`az login`</span><span class="sxs-lookup"><span data-stu-id="93cf3-127">**Interactive login**: `az login`</span></span>
* <span data-ttu-id="93cf3-128">**Jelentkezzen be a felhasználónevet és jelszót**:`az login -u johndoe@contoso.com -p VerySecret`</span><span class="sxs-lookup"><span data-stu-id="93cf3-128">**Log in with user name and password**: `az login -u johndoe@contoso.com -p VerySecret`</span></span>
  * <span data-ttu-id="93cf3-129">A Microsoft és fiókok számára, hogy a többtényezős hitelesítés használata mellett nem működik.</span><span class="sxs-lookup"><span data-stu-id="93cf3-129">This doesn't work with Microsoft accounts or accounts that use multi-factor authentication.</span></span>
* <span data-ttu-id="93cf3-130">**Jelentkezzen be egy egyszerű**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="93cf3-130">**Log in with a service principal**: `az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span></span>

## <a name="azure-cli-20-sample-script"></a><span data-ttu-id="93cf3-131">Az Azure CLI 2.0 parancsfájlpéldát</span><span class="sxs-lookup"><span data-stu-id="93cf3-131">Azure CLI 2.0 sample script</span></span>

<span data-ttu-id="93cf3-132">Dolgozunk lesz ezután az egy kis héjparancsfájlt, amely néhány alapvető Azure CLI 2.0 parancs interaktív állít ki az Azure Storage-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="93cf3-132">Next, we'll work with a small shell script that issues a few basic Azure CLI 2.0 commands to interact with Azure Storage resources.</span></span> <span data-ttu-id="93cf3-133">A parancsfájl először létrehoz egy új tárolót a tárfiókban lévő, majd feltölt egy már létező fájlt (a blob) tárolóban.</span><span class="sxs-lookup"><span data-stu-id="93cf3-133">The script first creates a new container in your storage account, then uploads an existing file (as a blob) to that container.</span></span> <span data-ttu-id="93cf3-134">Majd felsorolja az összes BLOB a tárolóban, és végül letölti a fájlt a helyi számítógépen, amely megadja egy célra.</span><span class="sxs-lookup"><span data-stu-id="93cf3-134">It then lists all blobs in the container, and finally, downloads the file to a destination on your local computer that you specify.</span></span>

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating the container..."
az storage container create --name $container_name

echo "Uploading the file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing the blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading the file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

<span data-ttu-id="93cf3-135">**Konfigurálja és futtassa a parancsprogramot**</span><span class="sxs-lookup"><span data-stu-id="93cf3-135">**Configure and run the script**</span></span>

1. <span data-ttu-id="93cf3-136">Nyissa meg a kedvenc szövegszerkesztőjével, majd másolja, és az előző parancsfájl illessze be a szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="93cf3-136">Open your favorite text editor, then copy and paste the preceding script into the editor.</span></span>

2. <span data-ttu-id="93cf3-137">Következő lépésként frissítse a parancsfájl-változókat megfelelően a konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="93cf3-137">Next, update the script's variables to reflect your configuration settings.</span></span> <span data-ttu-id="93cf3-138">Cserélje le a következő értékeket a megadott:</span><span class="sxs-lookup"><span data-stu-id="93cf3-138">Replace the following values as specified:</span></span>

   * <span data-ttu-id="93cf3-139">**\<storage_account_name\>**  a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="93cf3-139">**\<storage_account_name\>** The name of your storage account.</span></span>
   * <span data-ttu-id="93cf3-140">**\<storage_account_key\>**  az elsődleges vagy másodlagos elérési kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="93cf3-140">**\<storage_account_key\>** The primary or secondary access key for your storage account.</span></span>
   * <span data-ttu-id="93cf3-141">**\<container_name\>**  A nevet az új tároló létrehozásához, például az "azure-cli-minta-container".</span><span class="sxs-lookup"><span data-stu-id="93cf3-141">**\<container_name\>** A name the new container to create, such as "azure-cli-sample-container".</span></span>
   * <span data-ttu-id="93cf3-142">**\<blob_name\>**  egy nevet a cél BLOB a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="93cf3-142">**\<blob_name\>** A name for the destination blob in the container.</span></span>
   * <span data-ttu-id="93cf3-143">**\<file_to_upload\>**  a helyi számítógépen, például a fájl elérési útját kis "~ / images/HelloWorld.png".</span><span class="sxs-lookup"><span data-stu-id="93cf3-143">**\<file_to_upload\>** The path to small file on your local computer, such as "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="93cf3-144">**\<destination_file\>**  a cél fájl elérési útját, például a "~ / downloadedImage.png".</span><span class="sxs-lookup"><span data-stu-id="93cf3-144">**\<destination_file\>** The destination file path, such as "~/downloadedImage.png".</span></span>

3. <span data-ttu-id="93cf3-145">Miután frissítette a szükséges változók, mentse a parancsfájlt, és zárja be a szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="93cf3-145">After you've updated the necessary variables, save the script and exit your editor.</span></span> <span data-ttu-id="93cf3-146">A következő lépések azt feltételezik, hogy a parancsfájl nevű **my_storage_sample.sh**.</span><span class="sxs-lookup"><span data-stu-id="93cf3-146">The next steps assume you've named your script **my_storage_sample.sh**.</span></span>

4. <span data-ttu-id="93cf3-147">Szükség esetén jelölje meg a parancsfájl végrehajtható, mint:`chmod +x my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="93cf3-147">Mark the script as executable, if necessary: `chmod +x my_storage_sample.sh`</span></span>

5. <span data-ttu-id="93cf3-148">Hajtsa végre a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="93cf3-148">Execute the script.</span></span> <span data-ttu-id="93cf3-149">Például a Bash:`./my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="93cf3-149">For example, in Bash: `./my_storage_sample.sh`</span></span>

<span data-ttu-id="93cf3-150">A következőhöz hasonló kimenetnek kell megjelennie, és a  **\<destination_file\>**  az megjelenjen-e a parancsfájl a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="93cf3-150">You should see output similar to the following, and the **\<destination_file\>** you specified in the script should appear on your local computer.</span></span>

```
Creating the container...
{
  "created": true
}
Uploading the file...
Percent complete: %100.0
Listing the blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading the file...
Name
---------
README.md
Done
```

> [!TIP]
> <span data-ttu-id="93cf3-151">Az előző eredmény **tábla** formátumban.</span><span class="sxs-lookup"><span data-stu-id="93cf3-151">The preceding output is in **table** format.</span></span> <span data-ttu-id="93cf3-152">Megadhat, amely kimeneti megadásával formátumát a `--output` argumentumának a parancssori felület parancsait, vagy állítsa az segítségével globálisan `az configure`.</span><span class="sxs-lookup"><span data-stu-id="93cf3-152">You can specify which output format to use by specifying the `--output` argument in your CLI commands, or set it globally using `az configure`.</span></span>
>

## <a name="manage-storage-accounts"></a><span data-ttu-id="93cf3-153">Tárfiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="93cf3-153">Manage storage accounts</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="93cf3-154">Új tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="93cf3-154">Create a new storage account</span></span>
<span data-ttu-id="93cf3-155">Az Azure Storage használatához tárfiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="93cf3-155">To use Azure Storage, you need a storage account.</span></span> <span data-ttu-id="93cf3-156">A számítógép beállítása után létrehozhat egy új Azure Storage-fiók [csatlakozás az előfizetéshez](#connect-to-your-azure-subscription).</span><span class="sxs-lookup"><span data-stu-id="93cf3-156">You can create a new Azure Storage account after you've configured your computer to [connect to your subscription](#connect-to-your-azure-subscription).</span></span>

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* <span data-ttu-id="93cf3-157">`--location`[Szükséges]: helyét.</span><span class="sxs-lookup"><span data-stu-id="93cf3-157">`--location` [Required]: Location.</span></span> <span data-ttu-id="93cf3-158">Például "USA nyugati régiója".</span><span class="sxs-lookup"><span data-stu-id="93cf3-158">For example, "West US".</span></span>
* <span data-ttu-id="93cf3-159">`--name`[Szükséges]: A tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="93cf3-159">`--name` [Required]: The storage account name.</span></span> <span data-ttu-id="93cf3-160">A névnek kell 3 – 24 karakter hosszúságú lehet, és csak kisbetűs alfanumerikus karaktereket használjon.</span><span class="sxs-lookup"><span data-stu-id="93cf3-160">The name must be 3 to 24 characters in length, and use only lowercase alphanumeric characters.</span></span>
* <span data-ttu-id="93cf3-161">`--resource-group`[Szükséges]: erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="93cf3-161">`--resource-group` [Required]: Name of resource group.</span></span>
* <span data-ttu-id="93cf3-162">`--sku`[Szükséges]: A tárfiók Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="93cf3-162">`--sku` [Required]: The storage account SKU.</span></span> <span data-ttu-id="93cf3-163">Megengedett értékek:</span><span class="sxs-lookup"><span data-stu-id="93cf3-163">Allowed values:</span></span>
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a><span data-ttu-id="93cf3-164">Környezeti változók értékét az alapértelmezett Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="93cf3-164">Set default Azure storage account environment variables</span></span>
<span data-ttu-id="93cf3-165">Az Azure-előfizetéshez több tárfiókot is lehet.</span><span class="sxs-lookup"><span data-stu-id="93cf3-165">You can have multiple storage accounts in your Azure subscription.</span></span> <span data-ttu-id="93cf3-166">Válassza ki az egyiket minden ezt követő tárolási parancs, állítsa be ezen környezeti változókkal:</span><span class="sxs-lookup"><span data-stu-id="93cf3-166">To select one of them to use for all subsequent storage commands, you can set these environment variables:</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="93cf3-167">Egy alapértelmezett tárfiókot beállításához másik úgy, hogy a kapcsolati karakterlánc használatával.</span><span class="sxs-lookup"><span data-stu-id="93cf3-167">Another way to set a default storage account is by using a connection string.</span></span> <span data-ttu-id="93cf3-168">Először a a kapcsolati karakterlánc beolvasása a `show-connection-string` parancs:</span><span class="sxs-lookup"><span data-stu-id="93cf3-168">First, get the connection string with the `show-connection-string` command:</span></span>

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

<span data-ttu-id="93cf3-169">Ezután másolja a kimeneti kapcsolati karakterláncot, és állítsa be a `AZURE_STORAGE_CONNECTION_STRING` környezeti változó (szükség lehet a kapcsolati karakterlánc tegye idézőjelek közé foglalt):</span><span class="sxs-lookup"><span data-stu-id="93cf3-169">Then copy the output connection string and set the `AZURE_STORAGE_CONNECTION_STRING` environment variable (you might need to enclose the connection string in quotes):</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> <span data-ttu-id="93cf3-170">Ez a cikk a következő szakaszok a összes példák feltételezik, hogy beállított a `AZURE_STORAGE_ACCOUNT` és `AZURE_STORAGE_ACCESS_KEY` környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="93cf3-170">All examples in the following sections of this article assume that you've set the `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY` environment variables.</span></span>
>

## <a name="create-and-manage-blobs"></a><span data-ttu-id="93cf3-171">Hozzon létre és blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="93cf3-171">Create and manage blobs</span></span>
<span data-ttu-id="93cf3-172">Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a világon HTTP vagy HTTPS PROTOKOLLON keresztül tárolásához.</span><span class="sxs-lookup"><span data-stu-id="93cf3-172">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="93cf3-173">Ez a szakasz feltételezi, hogy Ön már ismeri a Azure Blob storage fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="93cf3-173">This section assumes that you are already familiar with Azure Blob storage concepts.</span></span> <span data-ttu-id="93cf3-174">Részletes információkért lásd: [az Azure Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](/rest/api/storageservices/blob-service-concepts).</span><span class="sxs-lookup"><span data-stu-id="93cf3-174">For detailed information, see [Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](/rest/api/storageservices/blob-service-concepts).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="93cf3-175">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="93cf3-175">Create a container</span></span>
<span data-ttu-id="93cf3-176">Az Azure storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="93cf3-176">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="93cf3-177">Egy tároló segítségével létrehozható a `az storage container create` parancs:</span><span class="sxs-lookup"><span data-stu-id="93cf3-177">You can create a container by using the `az storage container create` command:</span></span>

```azurecli
az storage container create --name <container_name>
```

<span data-ttu-id="93cf3-178">Beállíthatja három szintjének olvasási hozzáférés egy új tároló az opcionális megadásával `--public-access` argumentum:</span><span class="sxs-lookup"><span data-stu-id="93cf3-178">You can set one of three levels of read access for a new container by specifying the optional  `--public-access` argument:</span></span>

* <span data-ttu-id="93cf3-179">`off`(alapértelmezett): tároló adatokat a fiók tulajdonosának a saját.</span><span class="sxs-lookup"><span data-stu-id="93cf3-179">`off` (default): Container data is private to the account owner.</span></span>
* <span data-ttu-id="93cf3-180">`blob`: Blobok nyilvános olvasási hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="93cf3-180">`blob`: Public read access for blobs.</span></span>
* <span data-ttu-id="93cf3-181">`container`: A teljes tárolóhoz nyilvános olvasási és lista eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="93cf3-181">`container`: Public read and list access to the entire container.</span></span>

<span data-ttu-id="93cf3-182">További információkért lásd: [tárolók és blobok névtelen olvasási hozzáférés kezelése](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="93cf3-182">For more information, see [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>

### <a name="upload-a-blob-to-a-container"></a><span data-ttu-id="93cf3-183">Blob feltöltése tárolóba</span><span class="sxs-lookup"><span data-stu-id="93cf3-183">Upload a blob to a container</span></span>
<span data-ttu-id="93cf3-184">Az Azure Blob storage blokkméretet támogatja, hozzáfűzése, és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="93cf3-184">Azure Blob storage supports block, append, and page blobs.</span></span> <span data-ttu-id="93cf3-185">Töltse fel blobok egy tárolóba használatával a `blob upload` parancs:</span><span class="sxs-lookup"><span data-stu-id="93cf3-185">Upload blobs to a container by using the `blob upload` command:</span></span>

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 <span data-ttu-id="93cf3-186">Alapértelmezés szerint a `blob upload` parancs a lapblobokat, vagy egyéb blokkblobokat fel a *.vhd fájlokat.</span><span class="sxs-lookup"><span data-stu-id="93cf3-186">By default, the `blob upload` command uploads *.vhd files to page blobs, or block blobs otherwise.</span></span> <span data-ttu-id="93cf3-187">Ha feltölt egy blobot egy másik megadásához használja a `--type` argumentum--engedélyezett értékek `append`, `block`, és `page`.</span><span class="sxs-lookup"><span data-stu-id="93cf3-187">To specify another type when you upload a blob, you can use the `--type` argument--allowed values are `append`, `block`, and `page`.</span></span>

 <span data-ttu-id="93cf3-188">A különböző blob típusokon további információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span><span class="sxs-lookup"><span data-stu-id="93cf3-188">For more information on the different blob types, see [Understanding Block Blobs, Append Blobs, and Page Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span></span>


### <a name="download-a-blob-from-a-container"></a><span data-ttu-id="93cf3-189">Blob letöltése tárolóból</span><span class="sxs-lookup"><span data-stu-id="93cf3-189">Download a blob from a container</span></span>
<span data-ttu-id="93cf3-190">Ez a példa bemutatja, hogyan töltheti le a blob egy tárolóba:</span><span class="sxs-lookup"><span data-stu-id="93cf3-190">This example demonstrates how to download a blob from a container:</span></span>

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="93cf3-191">A tárolóban lévő blobok listázása</span><span class="sxs-lookup"><span data-stu-id="93cf3-191">List the blobs in a container</span></span>

<span data-ttu-id="93cf3-192">A tárolóban lévő blobok listázása a [az tárolási blob lista](/cli/azure/storage/blob#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="93cf3-192">List the blobs in a container with the [az storage blob list](/cli/azure/storage/blob#list) command.</span></span>

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a><span data-ttu-id="93cf3-193">Blobok másolása</span><span class="sxs-lookup"><span data-stu-id="93cf3-193">Copy blobs</span></span>
<span data-ttu-id="93cf3-194">Tárfiókokon és régiókon belül vagy azok között aszinkron módon másolhatja át a blobokat.</span><span class="sxs-lookup"><span data-stu-id="93cf3-194">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="93cf3-195">Az alábbi példa azt mutatja be, hogyan másolhat át blobokat egyik tárfiókból a másikba.</span><span class="sxs-lookup"><span data-stu-id="93cf3-195">The following example demonstrates how to copy blobs from one storage account to another.</span></span> <span data-ttu-id="93cf3-196">Először a forrás tárfiókban hozunk létre tárolót, az abban lévő blobok számára pedig nyilvános olvasási hozzáférést adunk.</span><span class="sxs-lookup"><span data-stu-id="93cf3-196">We first create a container in the source storage account, specifying public read-access for its blobs.</span></span> <span data-ttu-id="93cf3-197">Ezt követően feltöltünk egy fájlt a tárolóba, végül átmásoljuk a blobot ebből a tárolóból a céltárfiókban lévő tárolóba.</span><span class="sxs-lookup"><span data-stu-id="93cf3-197">Next, we upload a file to the container, and finally, copy the blob from that container into a container in the destination storage account.</span></span>

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob to container in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

<span data-ttu-id="93cf3-198">A fenti példában a célként megadott tárolót már léteznie kell a cél tárfiókkal a másolási művelet sikeres legyen.</span><span class="sxs-lookup"><span data-stu-id="93cf3-198">In the above example, the destination container must already exist in the destination storage account for the copy operation to succeed.</span></span> <span data-ttu-id="93cf3-199">Ezenkívül a `--source-uri` argumentumban megadott forrásblobnak tartalmaznia kell egy közös hozzáférésű jogosultságkód (SAS-) tokent, vagy nyilvánosnak kell lennie, ahogy ebben a példában is szerepel.</span><span class="sxs-lookup"><span data-stu-id="93cf3-199">Additionally, the source blob specified in the `--source-uri` argument must either include a shared access signature (SAS) token, or be publicly accessible, as in this example.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="93cf3-200">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="93cf3-200">Delete a blob</span></span>
<span data-ttu-id="93cf3-201">Egy blob törléséhez használja a `blob delete` parancs:</span><span class="sxs-lookup"><span data-stu-id="93cf3-201">To delete a blob, use the `blob delete` command:</span></span>

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="93cf3-202">Hozzon létre és fájlmegosztások kezelése</span><span class="sxs-lookup"><span data-stu-id="93cf3-202">Create and manage file shares</span></span>
<span data-ttu-id="93cf3-203">Az Azure File storage közös tárterületet biztosít számára a Server Message Block (SMB) protokollt használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="93cf3-203">Azure File storage offers shared storage for applications using the Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="93cf3-204">A Microsoft Azure virtuális gépek és felhőszolgáltatások, valamint a helyszíni alkalmazások megosztott csatlakoztatott megosztásokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="93cf3-204">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="93cf3-205">A fájlmegosztások és a fájladatok az Azure parancssori felület használatával kezelheti.</span><span class="sxs-lookup"><span data-stu-id="93cf3-205">You can manage file shares and file data via the Azure CLI.</span></span> <span data-ttu-id="93cf3-206">További információ az Azure File storage: [Ismerkedés az Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) vagy [Azure File storage használata Linux](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="93cf3-206">For more information on Azure File storage, see [Get started with Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) or [How to use Azure File storage with Linux](../storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="93cf3-207">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="93cf3-207">Create a file share</span></span>
<span data-ttu-id="93cf3-208">Egy Azure fájlmegosztás egy SMB-fájlmegosztás, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="93cf3-208">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="93cf3-209">Minden könyvtárak és fájlok fájlmegosztást kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="93cf3-209">All directories and files must be created in a file share.</span></span> <span data-ttu-id="93cf3-210">Egy fiók korlátlan számú megosztást tartalmazhat, és a megosztás tud tárolni a fájlokat, a tárfiók a kapacitás határértékekig korlátlan számú.</span><span class="sxs-lookup"><span data-stu-id="93cf3-210">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up to the capacity limits of the storage account.</span></span> <span data-ttu-id="93cf3-211">Az alábbi példa létrehoz egy nevű fájlmegosztás **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="93cf3-211">The following example creates a file share named **myshare**.</span></span>

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="93cf3-212">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="93cf3-212">Create a directory</span></span>
<span data-ttu-id="93cf3-213">Egy könyvtárat biztosít az Azure fájlmegosztások hierarchikus struktúra.</span><span class="sxs-lookup"><span data-stu-id="93cf3-213">A directory provides a hierarchical structure in an Azure file share.</span></span> <span data-ttu-id="93cf3-214">Az alábbi példa létrehoz egy könyvtárat nevű **könyvtárnév** a fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="93cf3-214">The following example creates a directory named **myDir** in the file share.</span></span>

```azurecli
az storage directory create --name myDir --share-name myshare
```

<span data-ttu-id="93cf3-215">Az elérési út például tartalmazhatnak több szintjéről **dir1/dir2**.</span><span class="sxs-lookup"><span data-stu-id="93cf3-215">A directory path can include multiple levels, for example **dir1/dir2**.</span></span> <span data-ttu-id="93cf3-216">Azonban úgy kell beállítania, hogy létezik-e minden szülő-könyvtár egy alkönyvtár létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="93cf3-216">However, you must ensure that all parent directories exist before creating a subdirectory.</span></span> <span data-ttu-id="93cf3-217">Például a következő elérési út **dir1/dir2**, először létre kell hoznia directory **dir1**, majd hozza létre a könyvtár **dir2**.</span><span class="sxs-lookup"><span data-stu-id="93cf3-217">For example, for path **dir1/dir2**, you must first create directory **dir1**, then create directory **dir2**.</span></span>

### <a name="upload-a-local-file-to-a-share"></a><span data-ttu-id="93cf3-218">Egy helyi fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="93cf3-218">Upload a local file to a share</span></span>
<span data-ttu-id="93cf3-219">Az alábbi példa feltölt egy fájlt a **~/temp/samplefile.txt** gyökérben, a **megosztás** fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="93cf3-219">The following example uploads a file from **~/temp/samplefile.txt** to root of the **myshare** file share.</span></span> <span data-ttu-id="93cf3-220">A `--source` argumentum meghatározza a meglévő helyi fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="93cf3-220">The `--source` argument specifies the existing local file to upload.</span></span>

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

<span data-ttu-id="93cf3-221">Mint létrehozni a könyvtárat, megadhatja a megosztás lehet feltölteni a fájlt meglévő címtárhoz a megosztáson belüli belül az elérési út:</span><span class="sxs-lookup"><span data-stu-id="93cf3-221">As with directory creation, you can specify a directory path within the share to upload the file to an existing directory within the share:</span></span>

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

<span data-ttu-id="93cf3-222">A megosztásban található egy fájl akár 1 TB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="93cf3-222">A file in the share can be up to 1 TB in size.</span></span>

### <a name="list-the-files-in-a-share"></a><span data-ttu-id="93cf3-223">Olyan megosztáson található fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="93cf3-223">List the files in a share</span></span>
<span data-ttu-id="93cf3-224">Fájlok és könyvtárak olyan megosztáson található is listázhatja a a `az storage file list` parancs:</span><span class="sxs-lookup"><span data-stu-id="93cf3-224">You can list files and directories in a share by using the `az storage file list` command:</span></span>

```azurecli
# List the files in the root of a share
az storage file list --share-name myshare --output table

# List the files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List the files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a><span data-ttu-id="93cf3-225">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="93cf3-225">Copy files</span></span>      
<span data-ttu-id="93cf3-226">Fájl másolása másik fájlba, egy fájlt egy blobba vagy egy blobot egy fájlba.</span><span class="sxs-lookup"><span data-stu-id="93cf3-226">You can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="93cf3-227">Ha például a fájl másolása másik megosztásban egy könyvtárat:</span><span class="sxs-lookup"><span data-stu-id="93cf3-227">For example, to copy a file to a directory in a different share:</span></span>        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a><span data-ttu-id="93cf3-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="93cf3-228">Next steps</span></span>
<span data-ttu-id="93cf3-229">Az alábbiakban néhány további források további, az Azure CLI 2.0 használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="93cf3-229">Here are some additional resources for learning more about working with the Azure CLI 2.0.</span></span>

* [<span data-ttu-id="93cf3-230">Ismerkedés az Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="93cf3-230">Get started with Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="93cf3-231">Az Azure CLI 2.0 parancsdokumentációja</span><span class="sxs-lookup"><span data-stu-id="93cf3-231">Azure CLI 2.0 command reference</span></span>](/cli/azure)
* [<span data-ttu-id="93cf3-232">Az Azure CLI 2.0 a Githubon</span><span class="sxs-lookup"><span data-stu-id="93cf3-232">Azure CLI 2.0 on GitHub</span></span>](https://github.com/Azure/azure-cli)
