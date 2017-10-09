---
title: Blob storage (object storage) Ruby a aaaHow toouse |} Microsoft Docs
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
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
ms.openlocfilehash: 638826777f5a7ae8330fd67cdbb51d5eee1736a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ruby"></a><span data-ttu-id="7570a-103">Hogyan toouse Blob storage-ának Ruby</span><span class="sxs-lookup"><span data-stu-id="7570a-103">How toouse Blob storage from Ruby</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="7570a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7570a-104">Overview</span></span>
<span data-ttu-id="7570a-105">Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="7570a-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="7570a-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="7570a-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="7570a-107">A BLOB storage is az említett tooas objektum tároló.</span><span class="sxs-lookup"><span data-stu-id="7570a-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="7570a-108">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7570a-108">This guide will show you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="7570a-109">hello minták hello Ruby API segítségével készül.</span><span class="sxs-lookup"><span data-stu-id="7570a-109">hello samples are written using hello Ruby API.</span></span> <span data-ttu-id="7570a-110">hello tárgyalt forgatókönyvekben szerepel a **feltöltése, listázása, letöltése,** és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="7570a-110">hello scenarios covered include **uploading, listing, downloading,** and **deleting** blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="7570a-111">Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7570a-111">Create a Ruby application</span></span>
<span data-ttu-id="7570a-112">Ruby-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7570a-112">Create a Ruby application.</span></span> <span data-ttu-id="7570a-113">Útmutatásért lásd: [Ruby sínek webalkalmazás egy Azure virtuális gépen](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span><span class="sxs-lookup"><span data-stu-id="7570a-113">For instructions, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="7570a-114">Az alkalmazás tooaccess tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7570a-114">Configure your application tooaccess Storage</span></span>
<span data-ttu-id="7570a-115">Azure Storage toouse, és van szükség toodownload használata hello Ruby azure csomagot, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7570a-115">toouse Azure Storage, you need toodownload and use hello Ruby azure package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="7570a-116">RubyGems tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="7570a-116">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="7570a-117">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="7570a-117">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="7570a-118">Írja be a "gem telepítése azure" hello parancs ablak tooinstall hello gem és függőségeit.</span><span class="sxs-lookup"><span data-stu-id="7570a-118">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="7570a-119">Hello csomag importálása</span><span class="sxs-lookup"><span data-stu-id="7570a-119">Import hello package</span></span>
<span data-ttu-id="7570a-120">Kedvenc szövegszerkesztőjével használ, adja hozzá a következő hello toouse tárolási célhelyeként Ruby fájl elejéhez toohello hello:</span><span class="sxs-lookup"><span data-stu-id="7570a-120">Using your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="7570a-121">Egy Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="7570a-121">Set up an Azure Storage Connection</span></span>
<span data-ttu-id="7570a-122">hello azure modul hello környezeti változókat olvassák **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_ACCESS_KEY** a információ tooconnect tooyour Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="7570a-122">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="7570a-123">Ha ezek a környezeti változók nem, meg kell adnia hello fiók használata előtt **Azure::Blob::BlobService** a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="7570a-123">If these environment variables are not set, you must specify hello account information before using **Azure::Blob::BlobService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="7570a-124">tooobtain ezeket az értékeket a klasszikus vagy erőforrás-kezelő tárolási fiók hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="7570a-124">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="7570a-125">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7570a-125">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7570a-126">Keresse meg a kívánt toouse toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7570a-126">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="7570a-127">Kattintson a jobb oldali hello hello beállítási paneljén **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="7570a-127">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="7570a-128">Hello elérési kulcsok paneljén megjelenő láthatja, hello hozzáférési kulcs 1 és 2 elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7570a-128">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="7570a-129">Ezek bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="7570a-129">You can use either of these.</span></span>
5. <span data-ttu-id="7570a-130">Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="7570a-130">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

<span data-ttu-id="7570a-131">Ezeket az értékeket a klasszikus tárolási funkciókat biztosító fiókot a klasszikus Azure portálon hello tooobtain:</span><span class="sxs-lookup"><span data-stu-id="7570a-131">tooobtain these values from a classic storage account in hello classic Azure portal:</span></span>

1. <span data-ttu-id="7570a-132">Jelentkezzen be toohello [klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="7570a-132">Log in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="7570a-133">Keresse meg a kívánt toouse toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7570a-133">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="7570a-134">Kattintson a **ELÉRÉSI kulcsok kezelése** hello aljához hello navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="7570a-134">Click **MANAGE ACCESS KEYS** at hello bottom of hello navigation pane.</span></span>
4. <span data-ttu-id="7570a-135">Hello előugró párbeszédpanelen látható hello tárfiók neve, az elsődleges elérési kulcsát és a másodlagos elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7570a-135">In hello pop-up dialog, you'll see hello storage account name, primary access key and secondary access key.</span></span> <span data-ttu-id="7570a-136">Hozzáférési kulcs hello elsődleges vagy másodlagos is hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7570a-136">For access key, you can use either hello primary one or hello secondary one.</span></span>
5. <span data-ttu-id="7570a-137">Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="7570a-137">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="7570a-138">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="7570a-138">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="7570a-139">Hello **Azure::Blob::BlobService** objektum lehetővé teszi, hogy a tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="7570a-139">hello **Azure::Blob::BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="7570a-140">egy tároló toocreate hello használata **létrehozása\_container()** metódust.</span><span class="sxs-lookup"><span data-stu-id="7570a-140">toocreate a container, use hello **create\_container()** method.</span></span>

<span data-ttu-id="7570a-141">hello alábbi példakód létrehoz egy tárolót vagy nyomtatása hello hiba, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="7570a-141">hello following code example creates a container or prints hello error if there is any.</span></span>

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

<span data-ttu-id="7570a-142">Ha azt szeretné toomake hello fájlok hello tárolóban nyilvános, beállíthatja a hello tároló engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="7570a-142">If you want toomake hello files in hello container public, you can set hello container's permissions.</span></span>

<span data-ttu-id="7570a-143">Csak módosíthatja hello <strong>létrehozása\_container()</strong> hívás toopass hello **: nyilvános\_hozzáférés\_szint** lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="7570a-143">You can just modify hello <strong>create\_container()</strong> call toopass hello **:public\_access\_level** option:</span></span>

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

<span data-ttu-id="7570a-144">Hello értékek **: nyilvános\_hozzáférés\_szint** lehetőség van:</span><span class="sxs-lookup"><span data-stu-id="7570a-144">Valid values for hello **:public\_access\_level** option are:</span></span>

* <span data-ttu-id="7570a-145">**BLOB:** nyilvános olvasási hozzáférés a blobok határozza meg.</span><span class="sxs-lookup"><span data-stu-id="7570a-145">**blob:** Specifies public read access for blobs.</span></span> <span data-ttu-id="7570a-146">Blobadatok található olvasható névtelen kérelem keresztül, de adatai nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="7570a-146">Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="7570a-147">Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok nem tudja felsorolni.</span><span class="sxs-lookup"><span data-stu-id="7570a-147">Clients cannot enumerate blobs within hello container via anonymous request.</span></span>
* <span data-ttu-id="7570a-148">**tároló:** határozza meg a tároló és a blob adatait a teljes nyilvános olvasási hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="7570a-148">**container:** Specifies full public read access for container and blob data.</span></span> <span data-ttu-id="7570a-149">Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok enumerálása, de nem tudja felsorolni a tárolók hello tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="7570a-149">Clients can enumerate blobs within hello container via anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="7570a-150">Azt is megteheti, módosíthatja az hello nyilvános hozzáférési szint a tároló használatával **beállítása\_tároló\_acl()** toospecify hello nyilvános hozzáférés szintet.</span><span class="sxs-lookup"><span data-stu-id="7570a-150">Alternatively, you can modify hello public access level of a container by using **set\_container\_acl()** method toospecify hello public access level.</span></span>

<span data-ttu-id="7570a-151">hello az alábbi kódpéldát módosítások hello nyilvános hozzáférési szint túl**tároló**:</span><span class="sxs-lookup"><span data-stu-id="7570a-151">hello following code example changes hello public access level too**container**:</span></span>

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="7570a-152">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="7570a-152">Upload a blob into a container</span></span>
<span data-ttu-id="7570a-153">tooupload tartalom tooa blob, használjon hello **létrehozása\_blokk\_blob()** metódus toocreate hello blob, egy fájl vagy karakterlánc-hello BLOB hello tartalmat.</span><span class="sxs-lookup"><span data-stu-id="7570a-153">tooupload content tooa blob, use hello **create\_block\_blob()** method toocreate hello blob, use a file or string as hello content of hello blob.</span></span>

<span data-ttu-id="7570a-154">hello alábbira feltölt hello fájlt **test.png** "lemezkép-blob" hello tárolóban nevű új blob-ként.</span><span class="sxs-lookup"><span data-stu-id="7570a-154">hello following code uploads hello file **test.png** as a new blob named "image-blob" in hello container.</span></span>

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="7570a-155">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="7570a-155">List hello blobs in a container</span></span>
<span data-ttu-id="7570a-156">toolist hello tárolók, használjon **list_containers()** metódust.</span><span class="sxs-lookup"><span data-stu-id="7570a-156">toolist hello containers, use **list_containers()** method.</span></span>
<span data-ttu-id="7570a-157">toolist hello BLOB a tárolóban lévő használja **lista\_blobs()** metódust.</span><span class="sxs-lookup"><span data-stu-id="7570a-157">toolist hello blobs within a container, use **list\_blobs()** method.</span></span>

<span data-ttu-id="7570a-158">Ennek kimenete hello hello fiók összes hello tárolókban lévő összes hello BLOB URL-címei.</span><span class="sxs-lookup"><span data-stu-id="7570a-158">This outputs hello urls of all hello blobs in all hello containers for hello account.</span></span>

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a><span data-ttu-id="7570a-159">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="7570a-159">Download blobs</span></span>
<span data-ttu-id="7570a-160">toodownload blobs használata hello **beolvasása\_blob()** metódus tooretrieve hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="7570a-160">toodownload blobs, use hello **get\_blob()** method tooretrieve hello contents.</span></span>

<span data-ttu-id="7570a-161">hello alábbi példakód bemutatja használatával **beolvasása\_blob()** toodownload hello "lemezkép-blob" tartalmát és azokra írják tooa helyi fájlt.</span><span class="sxs-lookup"><span data-stu-id="7570a-161">hello following code example demonstrates using **get\_blob()** toodownload hello contents of "image-blob" and write it tooa local file.</span></span>

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a><span data-ttu-id="7570a-162">Blobok törléséhez</span><span class="sxs-lookup"><span data-stu-id="7570a-162">Delete a Blob</span></span>
<span data-ttu-id="7570a-163">Végezetül toodelete blob, használja a hello **törlése\_blob()** metódust.</span><span class="sxs-lookup"><span data-stu-id="7570a-163">Finally, toodelete a blob, use hello **delete\_blob()** method.</span></span> <span data-ttu-id="7570a-164">hello következő kódrészlet példa bemutatja, hogyan toodelete blob.</span><span class="sxs-lookup"><span data-stu-id="7570a-164">hello following code example demonstrates how toodelete a blob.</span></span>

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a><span data-ttu-id="7570a-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7570a-165">Next steps</span></span>
<span data-ttu-id="7570a-166">toolearn kapcsolatos összetettebb tárolási feladatok elvégzéséről az alábbi hivatkozásokat követve:</span><span class="sxs-lookup"><span data-stu-id="7570a-166">toolearn about more complex storage tasks, follow these links:</span></span>

* [<span data-ttu-id="7570a-167">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="7570a-167">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="7570a-168">[Rubyhoz készült Azure SDK](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="7570a-168">[Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>
* [<span data-ttu-id="7570a-169">Adatátvitel az AzCopy parancssori segédprogram hello</span><span class="sxs-lookup"><span data-stu-id="7570a-169">Transfer data with hello AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)

