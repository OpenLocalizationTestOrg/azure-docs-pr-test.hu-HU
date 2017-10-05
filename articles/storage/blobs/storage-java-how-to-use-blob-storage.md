---
title: "Azure Blob storage (object storage) a Java használatával |} Microsoft Docs"
description: "Store unstructured data in the cloud with Azure Blob storage (object storage) (Strukturálatlan adatok tárolása a felhőben Azure Blob Storage-fiókkal (objektumtároló))."
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
ms.openlocfilehash: e4de1bc57adf668f383d1fbaf4a721a61576d2a0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-blob-storage-from-java"></a><span data-ttu-id="71b62-103">How to use Blob storage from Java (A Blob Storage használata Javával)</span><span class="sxs-lookup"><span data-stu-id="71b62-103">How to use Blob storage from Java</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="71b62-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="71b62-104">Overview</span></span>
<span data-ttu-id="71b62-105">Az Azure Blob Storage egy olyan szolgáltatás, amely a strukturálatlan adatokat objektumként/blobként tárolja a felhőben.</span><span class="sxs-lookup"><span data-stu-id="71b62-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="71b62-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="71b62-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="71b62-107">A Blob Storage más néven objektumtárnak is hívható.</span><span class="sxs-lookup"><span data-stu-id="71b62-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="71b62-108">Ez a cikk bemutatja, hogyan hajthat végre a szolgáltatást a Microsoft Azure Blob Storage tárolót használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="71b62-108">This article will show you how to perform common scenarios using the Microsoft Azure Blob storage.</span></span> <span data-ttu-id="71b62-109">A mintákat a Java és -felhasználási nyelven íródtak a [Azure Storage SDK for Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="71b62-109">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="71b62-110">Az ismertetett forgatókönyvek **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="71b62-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="71b62-111">A blobok további információkért lásd: a [lépések](#Next-Steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="71b62-111">For more information on blobs, see the [Next Steps](#Next-Steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="71b62-112">Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="71b62-112">An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="71b62-113">További információkért lásd: a [Azure Storage SDK for Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="71b62-113">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>
>
>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="71b62-114">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="71b62-114">Create a Java application</span></span>
<span data-ttu-id="71b62-115">Ez a cikk tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.</span><span class="sxs-lookup"><span data-stu-id="71b62-115">In this article, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="71b62-116">Ehhez szüksége lesz a Java fejlesztői készlet (JDK) telepítése, és hozzon létre egy Azure Storage-fiókot az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="71b62-116">To do so, you will need to install the Java Development Kit (JDK) and create an Azure Storage account in your Azure subscription.</span></span> <span data-ttu-id="71b62-117">Ha ezzel végzett, akkor győződjön meg arról, hogy a fejlesztési rendszer megfelel-e a minimális követelményeit és függőségeit, amelyek szerepelnek a [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="71b62-117">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="71b62-118">Ha a rendszer megfelel ezeknek a követelményeknek, akkor is kövesse letöltése és telepítése az Azure Storage szalagtárak Java ebből a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="71b62-118">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="71b62-119">Ezek a feladatok befejezése után lesz ebben a cikkben a példák használó Java-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="71b62-119">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-blob-storage"></a><span data-ttu-id="71b62-120">Állítsa be az alkalmazását, a Blob storage eléréséhez</span><span class="sxs-lookup"><span data-stu-id="71b62-120">Configure your application to access Blob storage</span></span>
<span data-ttu-id="71b62-121">Adja hozzá a következő importálási utasításokat a felső részén a Java-fájlt, amelyre az Azure Storage API-k használata a blobokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="71b62-121">Add the following import statements to the top of the Java file where you want to use the Azure Storage APIs to access blobs.</span></span>

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="71b62-122">Egy Azure Storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="71b62-122">Set up an Azure Storage connection string</span></span>
<span data-ttu-id="71b62-123">Egy Azure Storage-ügyfél egy tárolási kapcsolati karakterlánc végpontok és adatok szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="71b62-123">An Azure Storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="71b62-124">Ha egy ügyfél-alkalmazás fut, meg kell adnia a tárolási kapcsolati karakterlánc a következő formátumban a tárfiók nevével, és a tárfiók elsődleges elérési kulcs szerepel az [Azure-portálon](https://portal.azure.com) a a *AccountName* és *AccountKey* értékek.</span><span class="sxs-lookup"><span data-stu-id="71b62-124">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="71b62-125">A következő példa bemutatja, hogyan deklarálhatnak ahhoz, hogy a kapcsolati karakterlánc statikus mezőben.</span><span class="sxs-lookup"><span data-stu-id="71b62-125">The following example shows how you can declare a static field to hold the connection string.</span></span>

```java
// Define the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="71b62-126">Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja a konfigurációs fájlban, *ServiceConfiguration.cscfg*, és hívja érhető el a **RoleEnvironment.getConfigurationSettings** metódust.</span><span class="sxs-lookup"><span data-stu-id="71b62-126">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="71b62-127">Az alábbi példában a kapcsolati karakterláncot, lekérdezi a **beállítás** nevű elem *StorageConnectionString* a szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="71b62-127">The following example gets the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file.</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="71b62-128">A következő minták azt feltételezik, hogy használt két módszer közül egyik beolvasni a tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="71b62-128">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="71b62-129">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="71b62-129">Create a container</span></span>
<span data-ttu-id="71b62-130">A **CloudBlobClient** objektum lehetővé teszi, hogy a tárolók és blobok hivatkozási objektumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="71b62-130">A **CloudBlobClient** object lets you get reference objects for containers and blobs.</span></span> <span data-ttu-id="71b62-131">Az alábbi kód létrehoz egy **CloudBlobClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="71b62-131">The following code creates a **CloudBlobClient** object.</span></span>

> [!NOTE]
> <span data-ttu-id="71b62-132">Nincsenek további módon hozhat létre **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a a [Azure Storage ügyfél SDK-dokumentáció].</span><span class="sxs-lookup"><span data-stu-id="71b62-132">There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="71b62-133">Használja a **CloudBlobClient** hivatkozás a tárolóhoz használni kívánt objektumot.</span><span class="sxs-lookup"><span data-stu-id="71b62-133">Use the **CloudBlobClient** object to get a reference to the container you want to use.</span></span> <span data-ttu-id="71b62-134">A tároló hozhat létre, ha az nem létezik a **createIfNotExists** metódus, amely más módon fog visszaadni a meglévő tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="71b62-134">You can create the container if it doesn't exist with the **createIfNotExists** method, which will otherwise return the existing container.</span></span> <span data-ttu-id="71b62-135">Alapértelmezés szerint az új tároló sajátja, ezért meg kell adnia a tárelérési kulcsát (úgy, ahogy korábban) a blobok letöltését az ebben a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="71b62-135">By default, the new container is private, so you must specify your storage access key (as you did earlier) to download blobs from this container.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference to a container.
    // The container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create the container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a><span data-ttu-id="71b62-136">Választható lehetőség: A tárolók nyilvános hozzáférés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="71b62-136">Optional: Configure a container for public access</span></span>
<span data-ttu-id="71b62-137">Egy tároló engedélyek privát hozzáférést alapértelmezés szerint vannak konfigurálva, de könnyen konfigurálható egy tároló engedélyt, hogy az összes felhasználó számára az interneten lévő nyilvános, csak olvasható hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="71b62-137">A container's permissions are configured for private access by default, but you can easily configure a container's permissions to allow public, read-only access for all users on the Internet:</span></span>

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="71b62-138">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="71b62-138">Upload a blob into a container</span></span>
<span data-ttu-id="71b62-139">Fájl feltöltése a blob, beolvasni a tároló hivatkozását, és kérjen le egy blobhivatkozást segítségével.</span><span class="sxs-lookup"><span data-stu-id="71b62-139">To upload a file to a blob, get a container reference and use it to get a blob reference.</span></span> <span data-ttu-id="71b62-140">Miután egy blobhivatkozást, a feltöltési meghívásával beolvasott a blobhivatkozást bármilyen streamet feltölthet.</span><span class="sxs-lookup"><span data-stu-id="71b62-140">Once you have a blob reference, you can upload any stream by calling upload on the blob reference.</span></span> <span data-ttu-id="71b62-141">Ez a művelet létrehozza a blobot, ha nem létezik, vagy felülírja, ellenkező esetben.</span><span class="sxs-lookup"><span data-stu-id="71b62-141">This operation will create the blob if it doesn't exist, or overwrite it if it does.</span></span> <span data-ttu-id="71b62-142">A következő példakód ezt mutatja be, és feltételezi, hogy a tároló már létezik.</span><span class="sxs-lookup"><span data-stu-id="71b62-142">The following code sample shows this, and assumes that the container has already been created.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define the path to a local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite the "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="71b62-143">A tárolóban lévő blobok listázása</span><span class="sxs-lookup"><span data-stu-id="71b62-143">List the blobs in a container</span></span>
<span data-ttu-id="71b62-144">A tárolóban lévő blobok listázásához először kapnak a tároló hivatkozását, mint amilyet egy blob feltöltése.</span><span class="sxs-lookup"><span data-stu-id="71b62-144">To list the blobs in a container, first get a container reference like you did to upload a blob.</span></span> <span data-ttu-id="71b62-145">Használhatja a tároló **listBlobs** metódust egy **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="71b62-145">You can use the container's **listBlobs** method with a **for** loop.</span></span> <span data-ttu-id="71b62-146">A következő kódot a konzolba a tárolóban lévő minden egyes blob Uri kimenete.</span><span class="sxs-lookup"><span data-stu-id="71b62-146">The following code outputs the Uri of each blob in a container to the console.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within the container and output the URI to each of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="71b62-147">Vegye figyelembe, hogy a blobok elérési út adatait a név lehet neve.</span><span class="sxs-lookup"><span data-stu-id="71b62-147">Note that you can name blobs with path information in their names.</span></span> <span data-ttu-id="71b62-148">Ez egy olyan virtuális könyvtárstruktúrát hoz létre, amelyet egy hagyományos fájlrendszerhez hasonlóan rendszerezhet és bejárhat.</span><span class="sxs-lookup"><span data-stu-id="71b62-148">This creates a virtual directory structure that you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="71b62-149">Ne feledje, hogy a könyvtárstruktúra csupán virtuális – a Blob Storage szolgáltatásban elérhető erőforrások kizárólag a tárolók és a blobok.</span><span class="sxs-lookup"><span data-stu-id="71b62-149">Note that the directory structure is virtual only - the only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="71b62-150">Azonban az ügyféloldali kódtár kínál a **CloudBlobDirectory** objektum tekintse meg a virtuális könyvtárra, és így blobokkal munkavégzés folyamatainak egyszerűsítéséhez.</span><span class="sxs-lookup"><span data-stu-id="71b62-150">However, the client library offers a **CloudBlobDirectory** object to refer to a virtual directory and simplify the process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="71b62-151">Lehet például a "photos", amelyben blobok "rootphoto1", "2010/photo1", "2010/photo2" nevű és "2011/photo1" lehet, hogy feltöltése nevű tárolót.</span><span class="sxs-lookup"><span data-stu-id="71b62-151">For example, you could have a container named "photos", in which you might upload blobs named "rootphoto1", "2010/photo1", "2010/photo2", and "2011/photo1".</span></span> <span data-ttu-id="71b62-152">Ez hozna létre a virtuális könyvtár "2010" és "2011" a "photos" tárolóban.</span><span class="sxs-lookup"><span data-stu-id="71b62-152">This would create the virtual directories "2010" and "2011" within the "photos" container.</span></span> <span data-ttu-id="71b62-153">A hívás esetén **listBlobs** a "photos" tárolóban, a gyűjtemény adott vissza fogja tartalmazni **CloudBlobDirectory** és **CloudBlob** objektumokból a könyvtárak és a legfelső szinten található blobokat.</span><span class="sxs-lookup"><span data-stu-id="71b62-153">When you call **listBlobs** on the "photos" container, the collection returned will contain **CloudBlobDirectory** and **CloudBlob** objects representing the directories and blobs contained at the top level.</span></span> <span data-ttu-id="71b62-154">Ebben az esetben "2010" és "2011" könyvtárak, valamint fénykép "rootphoto1" vissza.</span><span class="sxs-lookup"><span data-stu-id="71b62-154">In this case, directories "2010" and "2011", as well as photo "rootphoto1" would be returned.</span></span> <span data-ttu-id="71b62-155">Használhatja a **instanceof** operátor segítségével különböztetheti meg egymástól ezeket az objektumokat.</span><span class="sxs-lookup"><span data-stu-id="71b62-155">You can use the **instanceof** operator to distinguish these objects.</span></span>

<span data-ttu-id="71b62-156">Ha szükséges, átviheti paramétereit, és a **listBlobs** metódust a **Listblobs** paraméter beállítása: igaz.</span><span class="sxs-lookup"><span data-stu-id="71b62-156">Optionally, you can pass in parameters to the **listBlobs** method with the **useFlatBlobListing** parameter set to true.</span></span> <span data-ttu-id="71b62-157">Ennek eredményeképpen az összes blobjának ad vissza, függetlenül attól, könyvtár.</span><span class="sxs-lookup"><span data-stu-id="71b62-157">This will result in every blob being returned, regardless of directory.</span></span> <span data-ttu-id="71b62-158">További információkért lásd: **CloudBlobContainer.listBlobs** a a [Azure Storage ügyfél SDK-dokumentáció].</span><span class="sxs-lookup"><span data-stu-id="71b62-158">For more information, see **CloudBlobContainer.listBlobs** in the [Azure Storage Client SDK Reference].</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="71b62-159">Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="71b62-159">Download a blob</span></span>
<span data-ttu-id="71b62-160">Blobok letöltéséhez ugyanazokat a lépéseket egy blob feltöltése ahhoz, hogy kérjen le egy blobhivatkozást hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="71b62-160">To download blobs, follow the same steps as you did for uploading a blob in order to get a blob reference.</span></span> <span data-ttu-id="71b62-161">A feltöltést példában feltöltése a blob-objektum neve.</span><span class="sxs-lookup"><span data-stu-id="71b62-161">In the uploading example, you called upload on the blob object.</span></span> <span data-ttu-id="71b62-162">A következő példában át a blob tartalmát egy stream objektumra, mint a letöltési hívás egy **FileOutputStream** használható megőrizni a blobot egy helyi fájlba.</span><span class="sxs-lookup"><span data-stu-id="71b62-162">In the following example, call download to transfer the blob contents to a stream object such as a **FileOutputStream** that you can use to persist the blob to a local file.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in the container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If the item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download the item and save it to a file with the same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a><span data-ttu-id="71b62-163">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="71b62-163">Delete a blob</span></span>
<span data-ttu-id="71b62-164">Törli a blob, a get egy blobhivatkozást, és a hívás **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="71b62-164">To delete a blob, get a blob reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference to a blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete the blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a><span data-ttu-id="71b62-165">A blob-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="71b62-165">Delete a blob container</span></span>
<span data-ttu-id="71b62-166">Végül egy blob-tároló törléséhez lekérése blob tároló hivatkozását, és hívás **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="71b62-166">Finally, to delete a blob container, get a blob container reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete the blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="71b62-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71b62-167">Next steps</span></span>
<span data-ttu-id="71b62-168">Most, hogy megismerte a Blob storage alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.</span><span class="sxs-lookup"><span data-stu-id="71b62-168">Now that you've learned the basics of Blob storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="71b62-169">[Az Azure Storage Java SDK][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="71b62-169">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="71b62-170">[Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage ügyfél SDK-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="71b62-170">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="71b62-171">[Az Azure Storage REST API-n][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="71b62-171">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="71b62-172">[Az Azure Storage csapat blogja][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="71b62-172">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="71b62-173">További információ: [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="71b62-173">For more information, see also [Azure for Java developers](/java/azure).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
