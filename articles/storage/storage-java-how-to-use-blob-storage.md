---
title: Azure Blob storage (object storage) a Java aaaHow toouse |} Microsoft Docs
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: java
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 2e223b38-92de-4c2f-9254-346374545d32
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 6dd6efdf688161c32b9d73a65fa7f3a98f2fad74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-java"></a><span data-ttu-id="c5204-103">Hogyan toouse Blob storage-ának Java</span><span class="sxs-lookup"><span data-stu-id="c5204-103">How toouse Blob storage from Java</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="c5204-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c5204-104">Overview</span></span>
<span data-ttu-id="c5204-105">Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="c5204-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="c5204-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="c5204-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="c5204-107">A BLOB storage is az említett tooas objektum tároló.</span><span class="sxs-lookup"><span data-stu-id="c5204-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="c5204-108">Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Microsoft Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c5204-108">This article will show you how tooperform common scenarios using hello Microsoft Azure Blob storage.</span></span> <span data-ttu-id="c5204-109">hello minták Java nyelven íródtak, és használja a hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="c5204-109">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="c5204-110">hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="c5204-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="c5204-111">A blobok további információkért lásd: hello [lépések](#Next-Steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c5204-111">For more information on blobs, see hello [Next Steps](#Next-Steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="c5204-112">Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="c5204-112">An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="c5204-113">További információkért lásd: hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="c5204-113">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>
>
>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="c5204-114">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5204-114">Create a Java application</span></span>
<span data-ttu-id="c5204-115">Ez a cikk tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.</span><span class="sxs-lookup"><span data-stu-id="c5204-115">In this article, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="c5204-116">toodo tooinstall kell tehát hello Java fejlesztői készlet (JDK), és hozzon létre egy Azure Storage-fiókot az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c5204-116">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure Storage account in your Azure subscription.</span></span> <span data-ttu-id="c5204-117">Ha ezzel végzett, szüksége lesz a fejlesztői rendszerhez megfelelő hello minimális követelményeit és függőségeit, amelyek hello vannak felsorolva tooverify [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="c5204-117">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="c5204-118">Ha a rendszer megfelel ezeknek a követelményeknek, letöltése és telepítése hello Azure Storage-könyvtárakban Java ebből a rendszeren hello utasításokat kövesse.</span><span class="sxs-lookup"><span data-stu-id="c5204-118">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="c5204-119">E feladatok befejezése után fog tudni toocreate ebben a cikkben hello példák használó Java-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c5204-119">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-blob-storage"></a><span data-ttu-id="c5204-120">Az alkalmazás tooaccess Blob-tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c5204-120">Configure your application tooaccess Blob storage</span></span>
<span data-ttu-id="c5204-121">Adja hozzá a következő importálási utasítások toohello felső részén hello Java fájl toouse hello Azure Storage API-k tooaccess blobok hello.</span><span class="sxs-lookup"><span data-stu-id="c5204-121">Add hello following import statements toohello top of hello Java file where you want toouse hello Azure Storage APIs tooaccess blobs.</span></span>

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="c5204-122">Egy Azure Storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="c5204-122">Set up an Azure Storage connection string</span></span>
<span data-ttu-id="c5204-123">Egy Azure Storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatokat felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c5204-123">An Azure Storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="c5204-124">Ha egy ügyfél-alkalmazás fut, adja meg a hello tárolási kapcsolati karakterlánc formátuma a következő, a tárfiók nevére hello segítségével hello majd hello felsorolt hello tárfiók elsődleges elérési kulcsát hello [Azure-portálon](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket.</span><span class="sxs-lookup"><span data-stu-id="c5204-124">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="c5204-125">hello következő példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="c5204-125">hello following example shows how you can declare a static field toohold hello connection string.</span></span>

```java
// Define hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="c5204-126">Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja hello szolgáltatás konfigurációs fájljában, *ServiceConfiguration.cscfg*, és egy hívás toohello az elérhető  **RoleEnvironment.getConfigurationSettings** metódust.</span><span class="sxs-lookup"><span data-stu-id="c5204-126">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="c5204-127">hello alábbi példa lekérdezi hello származó kapcsolati karakterlánc egy **beállítás** nevű elem *StorageConnectionString* hello szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="c5204-127">hello following example gets hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file.</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="c5204-128">hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="c5204-128">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="c5204-129">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5204-129">Create a container</span></span>
<span data-ttu-id="c5204-130">A **CloudBlobClient** objektum lehetővé teszi, hogy a tárolók és blobok hivatkozási objektumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c5204-130">A **CloudBlobClient** object lets you get reference objects for containers and blobs.</span></span> <span data-ttu-id="c5204-131">hello alábbi kód létrehoz egy **CloudBlobClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="c5204-131">hello following code creates a **CloudBlobClient** object.</span></span>

> [!NOTE]
> <span data-ttu-id="c5204-132">Nincsenek további lehetőségek toocreate **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a hello [Azure Storage ügyfél SDK-dokumentáció].</span><span class="sxs-lookup"><span data-stu-id="c5204-132">There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="c5204-133">Használjon hello **CloudBlobClient** tooget egy hivatkozás toohello tároló toouse kívánt objektumot.</span><span class="sxs-lookup"><span data-stu-id="c5204-133">Use hello **CloudBlobClient** object tooget a reference toohello container you want toouse.</span></span> <span data-ttu-id="c5204-134">Hello tároló hozhat létre, ha még nem létezik a hello **createIfNotExists** metódus, amely egyébként visszaadható hello meglévő tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="c5204-134">You can create hello container if it doesn't exist with hello **createIfNotExists** method, which will otherwise return hello existing container.</span></span> <span data-ttu-id="c5204-135">Alapértelmezés szerint hello új tároló sajátja, ezért meg kell adnia a tárelérési kulcsát (úgy, ahogy korábban) toodownload blobok ebből a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="c5204-135">By default, hello new container is private, so you must specify your storage access key (as you did earlier) toodownload blobs from this container.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference tooa container.
    // hello container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create hello container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a><span data-ttu-id="c5204-136">Választható lehetőség: A tárolók nyilvános hozzáférés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c5204-136">Optional: Configure a container for public access</span></span>
<span data-ttu-id="c5204-137">Egy tároló engedélyek privát hozzáférést alapértelmezés szerint vannak konfigurálva, de egy tároló engedélyek tooallow nyilvános, csak olvasható hozzáférést minden felhasználó számára könnyen konfigurálható hello Internet:</span><span class="sxs-lookup"><span data-stu-id="c5204-137">A container's permissions are configured for private access by default, but you can easily configure a container's permissions tooallow public, read-only access for all users on hello Internet:</span></span>

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in hello permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set hello permissions on hello container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="c5204-138">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="c5204-138">Upload a blob into a container</span></span>
<span data-ttu-id="c5204-139">tooupload fájl tooa blob, szerezze be a tároló hivatkozását, és tooget egy blobhivatkozást használják.</span><span class="sxs-lookup"><span data-stu-id="c5204-139">tooupload a file tooa blob, get a container reference and use it tooget a blob reference.</span></span> <span data-ttu-id="c5204-140">Miután egy blobhivatkozást, a feltöltési meghívásával beolvasott hello blobhivatkozást bármilyen streamet feltölthet.</span><span class="sxs-lookup"><span data-stu-id="c5204-140">Once you have a blob reference, you can upload any stream by calling upload on hello blob reference.</span></span> <span data-ttu-id="c5204-141">Ez a művelet létrehoz hello blob nem létezik, vagy felülírja, ha létezik.</span><span class="sxs-lookup"><span data-stu-id="c5204-141">This operation will create hello blob if it doesn't exist, or overwrite it if it does.</span></span> <span data-ttu-id="c5204-142">a következő példakód hello ezt mutatja be, és feltételezi, hogy hello tároló már létre van hozva.</span><span class="sxs-lookup"><span data-stu-id="c5204-142">hello following code sample shows this, and assumes that hello container has already been created.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define hello path tooa local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite hello "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="c5204-143">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="c5204-143">List hello blobs in a container</span></span>
<span data-ttu-id="c5204-144">toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását, ahogyan azt tette tooupload blob.</span><span class="sxs-lookup"><span data-stu-id="c5204-144">toolist hello blobs in a container, first get a container reference like you did tooupload a blob.</span></span> <span data-ttu-id="c5204-145">Hello tárolót is használhatja **listBlobs** metódust egy **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="c5204-145">You can use hello container's **listBlobs** method with a **for** loop.</span></span> <span data-ttu-id="c5204-146">hello alábbira kimenete hello URI-címe, minden egyes blob tároló toohello konzolban.</span><span class="sxs-lookup"><span data-stu-id="c5204-146">hello following code outputs hello Uri of each blob in a container toohello console.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within hello container and output hello URI tooeach of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="c5204-147">Vegye figyelembe, hogy a blobok elérési út adatait a név lehet neve.</span><span class="sxs-lookup"><span data-stu-id="c5204-147">Note that you can name blobs with path information in their names.</span></span> <span data-ttu-id="c5204-148">Ez egy olyan virtuális könyvtárstruktúrát hoz létre, amelyet egy hagyományos fájlrendszerhez hasonlóan rendszerezhet és bejárhat.</span><span class="sxs-lookup"><span data-stu-id="c5204-148">This creates a virtual directory structure that you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="c5204-149">Vegye figyelembe, hogy csak virtuális könyvtárstruktúrát hello – hello csak forrásanyag is elérhető a Blob Storage tárolóban tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="c5204-149">Note that hello directory structure is virtual only - hello only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="c5204-150">Azonban hello ügyféloldali kódtár kínál a **CloudBlobDirectory** toorefer tooa virtuális könyvtár objektum és az ezzel a módszerrel blobokkal hello folyamat leegyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="c5204-150">However, hello client library offers a **CloudBlobDirectory** object toorefer tooa virtual directory and simplify hello process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="c5204-151">Lehet például a "photos", amelyben blobok "rootphoto1", "2010/photo1", "2010/photo2" nevű és "2011/photo1" lehet, hogy feltöltése nevű tárolót.</span><span class="sxs-lookup"><span data-stu-id="c5204-151">For example, you could have a container named "photos", in which you might upload blobs named "rootphoto1", "2010/photo1", "2010/photo2", and "2011/photo1".</span></span> <span data-ttu-id="c5204-152">Ez akkor hello virtuális könyvtárak létrehozása "2010" és "2011" hello "photos" tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c5204-152">This would create hello virtual directories "2010" and "2011" within hello "photos" container.</span></span> <span data-ttu-id="c5204-153">A hívás esetén **listBlobs** hello "photos" tárolóban, a visszaadott hello gyűjtemény fogja tartalmazni **CloudBlobDirectory** és **CloudBlob** hello objektumokból illetve hello felső szinten található blobokat.</span><span class="sxs-lookup"><span data-stu-id="c5204-153">When you call **listBlobs** on hello "photos" container, hello collection returned will contain **CloudBlobDirectory** and **CloudBlob** objects representing hello directories and blobs contained at hello top level.</span></span> <span data-ttu-id="c5204-154">Ebben az esetben "2010" és "2011" könyvtárak, valamint fénykép "rootphoto1" vissza.</span><span class="sxs-lookup"><span data-stu-id="c5204-154">In this case, directories "2010" and "2011", as well as photo "rootphoto1" would be returned.</span></span> <span data-ttu-id="c5204-155">Használhatja a hello **instanceof** operátor toodistinguish ezeket az objektumokat.</span><span class="sxs-lookup"><span data-stu-id="c5204-155">You can use hello **instanceof** operator toodistinguish these objects.</span></span>

<span data-ttu-id="c5204-156">Ha szükséges, átviheti paraméterek toohello **listBlobs** hello metódus **Listblobs** paraméterkészlet tootrue.</span><span class="sxs-lookup"><span data-stu-id="c5204-156">Optionally, you can pass in parameters toohello **listBlobs** method with hello **useFlatBlobListing** parameter set tootrue.</span></span> <span data-ttu-id="c5204-157">Ennek eredményeképpen az összes blobjának ad vissza, függetlenül attól, könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c5204-157">This will result in every blob being returned, regardless of directory.</span></span> <span data-ttu-id="c5204-158">További információkért lásd: **CloudBlobContainer.listBlobs** a hello [Azure Storage ügyfél SDK-dokumentáció].</span><span class="sxs-lookup"><span data-stu-id="c5204-158">For more information, see **CloudBlobContainer.listBlobs** in hello [Azure Storage Client SDK Reference].</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="c5204-159">Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="c5204-159">Download a blob</span></span>
<span data-ttu-id="c5204-160">toodownload blobok, az alábbi hello azonos lépések sorrendben tooget egy blobhivatkozást a blob feltöltése hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="c5204-160">toodownload blobs, follow hello same steps as you did for uploading a blob in order tooget a blob reference.</span></span> <span data-ttu-id="c5204-161">Hello példa feltöltése, a feltöltési hello blob objektum neve.</span><span class="sxs-lookup"><span data-stu-id="c5204-161">In hello uploading example, you called upload on hello blob object.</span></span> <span data-ttu-id="c5204-162">A következő példa hello, hívja a letöltési tootransfer hello blob tartalma tooa adatfolyam-objektum többek között a **FileOutputStream** használható toopersist hello tooa helyi fájlját.</span><span class="sxs-lookup"><span data-stu-id="c5204-162">In hello following example, call download tootransfer hello blob contents tooa stream object such as a **FileOutputStream** that you can use toopersist hello blob tooa local file.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in hello container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If hello item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download hello item and save it tooa file with hello same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a><span data-ttu-id="c5204-163">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="c5204-163">Delete a blob</span></span>
<span data-ttu-id="c5204-164">egy blob toodelete lekérése blob hivatkoznak, és hívja meg **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="c5204-164">toodelete a blob, get a blob reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference tooa blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete hello blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a><span data-ttu-id="c5204-165">A blob-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="c5204-165">Delete a blob container</span></span>
<span data-ttu-id="c5204-166">Végezetül toodelete egy blob tároló lekérése blob tároló hivatkozását, és hívás **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="c5204-166">Finally, toodelete a blob container, get a blob container reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete hello blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="c5204-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5204-167">Next steps</span></span>
<span data-ttu-id="c5204-168">Most, hogy megismerte a Blob storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="c5204-168">Now that you've learned hello basics of Blob storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="c5204-169">[Az Azure Storage Java SDK][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="c5204-169">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="c5204-170">[Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage ügyfél SDK-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="c5204-170">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="c5204-171">[Az Azure Storage REST API-n][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="c5204-171">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="c5204-172">[Az Azure Storage csapat blogja][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="c5204-172">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="c5204-173">További információkért lásd még: hello [Java fejlesztői központ](/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="c5204-173">For more information, see also hello [Java Developer Center](/develop/java/).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
