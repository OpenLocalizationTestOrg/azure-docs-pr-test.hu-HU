---
title: "Blob storage (object storage) Ruby az használata |} Microsoft Docs"
description: "Store unstructured data in the cloud with Azure Blob storage (object storage) (Strukturálatlan adatok tárolása a felhőben Azure Blob Storage-fiókkal (objektumtároló))."
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: d27cf1594d6a31a746ca85b5c3184f8a5dbbaa54
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-blob-storage-from-ruby"></a><span data-ttu-id="d4165-103">How to use Blob storage from Ruby (A Blob Storage használata Rubyval)</span><span class="sxs-lookup"><span data-stu-id="d4165-103">How to use Blob storage from Ruby</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="d4165-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d4165-104">Overview</span></span>
<span data-ttu-id="d4165-105">Az Azure Blob Storage egy olyan szolgáltatás, amely a strukturálatlan adatokat objektumként/blobként tárolja a felhőben.</span><span class="sxs-lookup"><span data-stu-id="d4165-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="d4165-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="d4165-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="d4165-107">A Blob Storage más néven objektumtárnak is hívható.</span><span class="sxs-lookup"><span data-stu-id="d4165-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="d4165-108">Ez az útmutató bemutatja, hogyan hajthat végre a Blob storage használatával gyakori forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="d4165-108">This guide will show you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="d4165-109">A minták íródtak, a Ruby API használatával.</span><span class="sxs-lookup"><span data-stu-id="d4165-109">The samples are written using the Ruby API.</span></span> <span data-ttu-id="d4165-110">Az ismertetett forgatókönyvek **feltöltése, listázása, letöltése,** és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="d4165-110">The scenarios covered include **uploading, listing, downloading,** and **deleting** blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="d4165-111">Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4165-111">Create a Ruby application</span></span>
<span data-ttu-id="d4165-112">Ruby-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d4165-112">Create a Ruby application.</span></span> <span data-ttu-id="d4165-113">Útmutatásért lásd: [Ruby sínek webalkalmazás egy Azure virtuális gépen](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span><span class="sxs-lookup"><span data-stu-id="d4165-113">For instructions, see [Ruby on Rails Web application on an Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="d4165-114">Állítsa be az alkalmazását tároló elérésére</span><span class="sxs-lookup"><span data-stu-id="d4165-114">Configure your application to access Storage</span></span>
<span data-ttu-id="d4165-115">Azure Storage használatához szüksége töltse le és használja a Ruby azure csomagot, amely tartalmaz egy kényelmi szalagtár szerepel, amely a többi tárolási szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="d4165-115">To use Azure Storage, you need to download and use the Ruby azure package, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="d4165-116">RubyGems használja a csomag beszerzése</span><span class="sxs-lookup"><span data-stu-id="d4165-116">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="d4165-117">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="d4165-117">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="d4165-118">Írja be a "gem telepítése azure" gem és függőségeinek telepítéséhez a parancsablakban.</span><span class="sxs-lookup"><span data-stu-id="d4165-118">Type "gem install azure" in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="d4165-119">A csomag importálása</span><span class="sxs-lookup"><span data-stu-id="d4165-119">Import the package</span></span>
<span data-ttu-id="d4165-120">Kedvenc szövegszerkesztőjével használ, a Ruby, hol szeretne használni a tárolási fájl elejéhez adja hozzá a következő:</span><span class="sxs-lookup"><span data-stu-id="d4165-120">Using your favorite text editor, add the following to the top of the Ruby file where you intend to use storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="d4165-121">Egy Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="d4165-121">Set up an Azure Storage Connection</span></span>
<span data-ttu-id="d4165-122">Az azure-moduljának a környezeti változókat olvassák **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_ACCESS_KEY** az Azure storage-fiókhoz való kapcsolódáshoz szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="d4165-122">The azure module will read the environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="d4165-123">Ha ezek a környezeti változók nem, meg kell adnia a fiók használata előtt **Azure::Blob::BlobService** az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="d4165-123">If these environment variables are not set, you must specify the account information before using **Azure::Blob::BlobService** with the following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="d4165-124">Ezek az értékek beolvasása klasszikus vagy erőforrás-kezelő storage-fiókot az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="d4165-124">To obtain these values from a classic or Resource Manager storage account in the Azure portal:</span></span>

1. <span data-ttu-id="d4165-125">Jelentkezzen be az [Azure portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d4165-125">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d4165-126">Nyissa meg a használni kívánt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d4165-126">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="d4165-127">Kattintson a beállítások panelen kattintson a jobb **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="d4165-127">In the Settings blade on the right, click **Access Keys**.</span></span>
4. <span data-ttu-id="d4165-128">A elérési kulcsok paneljén megjelenő láthatja, a hozzáférési kulcs 1 és 2 elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d4165-128">In the Access keys blade that appears, you'll see the access key 1 and access key 2.</span></span> <span data-ttu-id="d4165-129">Ezek bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="d4165-129">You can use either of these.</span></span>
5. <span data-ttu-id="d4165-130">A Másolás ikonra a kulcs másolása a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="d4165-130">Click the copy icon to copy the key to the clipboard.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="d4165-131">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4165-131">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="d4165-132">A **Azure::Blob::BlobService** objektum lehetővé teszi, hogy a tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="d4165-132">The **Azure::Blob::BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="d4165-133">Egy tároló létrehozásához használja a **létrehozása\_container()** metódust.</span><span class="sxs-lookup"><span data-stu-id="d4165-133">To create a container, use the **create\_container()** method.</span></span>

<span data-ttu-id="d4165-134">Az alábbi példakód létrehoz egy tárolót vagy kiírja a hiba, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="d4165-134">The following code example creates a container or prints the error if there is any.</span></span>

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

<span data-ttu-id="d4165-135">Ha azt szeretné, hogy a fájlok a tároló nyilvános, beállíthatja a tároló engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="d4165-135">If you want to make the files in the container public, you can set the container's permissions.</span></span>

<span data-ttu-id="d4165-136">Csak módosíthatja a <strong>létrehozása\_container()</strong> hívásakor adja át a **: nyilvános\_hozzáférés\_szint** lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="d4165-136">You can just modify the <strong>create\_container()</strong> call to pass the **:public\_access\_level** option:</span></span>

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

<span data-ttu-id="d4165-137">Az érvényes értékek a **: nyilvános\_hozzáférés\_szint** lehetőség van:</span><span class="sxs-lookup"><span data-stu-id="d4165-137">Valid values for the **:public\_access\_level** option are:</span></span>

* <span data-ttu-id="d4165-138">**BLOB:** nyilvános olvasási hozzáférés a blobok határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d4165-138">**blob:** Specifies public read access for blobs.</span></span> <span data-ttu-id="d4165-139">Blobadatok található olvasható névtelen kérelem keresztül, de adatai nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="d4165-139">Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="d4165-140">Ügyfelek névtelen kérelem keresztül a tárolóban található blobok nem tudja felsorolni.</span><span class="sxs-lookup"><span data-stu-id="d4165-140">Clients cannot enumerate blobs within the container via anonymous request.</span></span>
* <span data-ttu-id="d4165-141">**tároló:** határozza meg a tároló és a blob adatait a teljes nyilvános olvasási hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="d4165-141">**container:** Specifies full public read access for container and blob data.</span></span> <span data-ttu-id="d4165-142">Ügyfelek névtelen kérelem keresztül a tárolóban található blobok enumerálása, de nem tudja felsorolni a tárolók a tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="d4165-142">Clients can enumerate blobs within the container via anonymous request, but cannot enumerate containers within the storage account.</span></span>

<span data-ttu-id="d4165-143">Azt is megteheti, módosíthatja a nyilvános hozzáférési szint, a tároló használatával **beállítása\_tároló\_acl()** metódus nyilvános hozzáférés szintjének beállításához.</span><span class="sxs-lookup"><span data-stu-id="d4165-143">Alternatively, you can modify the public access level of a container by using **set\_container\_acl()** method to specify the public access level.</span></span>

<span data-ttu-id="d4165-144">Az alábbi példakód módosítja a nyilvános hozzáférés szintet **tároló**:</span><span class="sxs-lookup"><span data-stu-id="d4165-144">The following code example changes the public access level to **container**:</span></span>

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="d4165-145">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="d4165-145">Upload a blob into a container</span></span>
<span data-ttu-id="d4165-146">Tartalom feltöltése a blob, használja a **létrehozása\_blokk\_blob()** metódussal a blob, hozzon létre egy fájl vagy karakterlánc használják a blob tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d4165-146">To upload content to a blob, use the **create\_block\_blob()** method to create the blob, use a file or string as the content of the blob.</span></span>

<span data-ttu-id="d4165-147">Az alábbi kód feltölti **test.png** , egy új blob neve "lemezkép-blob" tárolóban.</span><span class="sxs-lookup"><span data-stu-id="d4165-147">The following code uploads the file **test.png** as a new blob named "image-blob" in the container.</span></span>

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="d4165-148">A tárolóban lévő blobok listázása</span><span class="sxs-lookup"><span data-stu-id="d4165-148">List the blobs in a container</span></span>
<span data-ttu-id="d4165-149">A tárolók kilistázhatja **list_containers()** metódust.</span><span class="sxs-lookup"><span data-stu-id="d4165-149">To list the containers, use **list_containers()** method.</span></span>
<span data-ttu-id="d4165-150">A tárolóban lévő blobok listázása, használja a **lista\_blobs()** metódust.</span><span class="sxs-lookup"><span data-stu-id="d4165-150">To list the blobs within a container, use **list\_blobs()** method.</span></span>

<span data-ttu-id="d4165-151">Ennek kimenete a fiókhoz tartozó összes tároló összes blobjának URL-címei.</span><span class="sxs-lookup"><span data-stu-id="d4165-151">This outputs the urls of all the blobs in all the containers for the account.</span></span>

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a><span data-ttu-id="d4165-152">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="d4165-152">Download blobs</span></span>
<span data-ttu-id="d4165-153">Blobok letöltéséhez használjon a **beolvasása\_blob()** metódusának segítéségével lekérheti a tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d4165-153">To download blobs, use the **get\_blob()** method to retrieve the contents.</span></span>

<span data-ttu-id="d4165-154">Az alábbi példakód mutatja be, használatával **beolvasása\_blob()** töltse le a "lemezkép-blob" tartalmát és egy helyi fájlba írja.</span><span class="sxs-lookup"><span data-stu-id="d4165-154">The following code example demonstrates using **get\_blob()** to download the contents of "image-blob" and write it to a local file.</span></span>

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a><span data-ttu-id="d4165-155">Blobok törléséhez</span><span class="sxs-lookup"><span data-stu-id="d4165-155">Delete a Blob</span></span>
<span data-ttu-id="d4165-156">Végezetül blob törléséhez használja a **törlése\_blob()** metódust.</span><span class="sxs-lookup"><span data-stu-id="d4165-156">Finally, to delete a blob, use the **delete\_blob()** method.</span></span> <span data-ttu-id="d4165-157">Az alábbi példakód bemutatja, hogyan blobok törléséhez.</span><span class="sxs-lookup"><span data-stu-id="d4165-157">The following code example demonstrates how to delete a blob.</span></span>

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a><span data-ttu-id="d4165-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d4165-158">Next steps</span></span>
<span data-ttu-id="d4165-159">Tájékozódhat az összetettebb tárolási feladatok elvégzéséről, az alábbi hivatkozásokat követve:</span><span class="sxs-lookup"><span data-stu-id="d4165-159">To learn about more complex storage tasks, follow these links:</span></span>

* [<span data-ttu-id="d4165-160">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="d4165-160">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="d4165-161">[Rubyhoz készült Azure SDK](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="d4165-161">[Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>
* [<span data-ttu-id="d4165-162">Adatátvitel az AzCopy parancssori segédprogrammal</span><span class="sxs-lookup"><span data-stu-id="d4165-162">Transfer data with the AzCopy Command-Line Utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

