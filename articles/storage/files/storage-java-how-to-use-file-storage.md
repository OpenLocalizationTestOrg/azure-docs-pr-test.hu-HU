---
title: az Azure File storage Java aaaDevelop |} Microsoft Docs
description: "Ismerje meg, hogyan toodevelop Java-alkalmazások és szolgáltatások Azure File storage toostore használó fájladatok."
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 3bfbfa7f-d378-4fb4-8df3-e0b6fcea5b27
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 05/27/2017
ms.author: robinsh
ms.openlocfilehash: be71a946604da8af0130f101f2eb6135c5e08abd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a><span data-ttu-id="626a4-103">Az Azure File storage Java fejlesztése</span><span class="sxs-lookup"><span data-stu-id="626a4-103">Develop for Azure File storage with Java</span></span>
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="626a4-104">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="626a4-104">About this tutorial</span></span>
<span data-ttu-id="626a4-105">Ez az oktatóanyag mutatni, Java toodevelop alkalmazásokhoz vagy szolgáltatásokhoz, használja az Azure File storage toostore fájladatok használatával hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="626a4-105">This tutorial will demonstrate hello basics of using Java toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="626a4-106">Ebben az oktatóanyagban a rendszer létrehoz egy egyszerű konzolalkalmazásként és megjelenítése hogyan tooperform alapszintű műveleteket az Java és az Azure File storage szolgáltatással:</span><span class="sxs-lookup"><span data-stu-id="626a4-106">In this tutorial, we will create a simple console application and show how tooperform basic actions with Java and Azure File storage:</span></span>

* <span data-ttu-id="626a4-107">Hozzon létre vagy töröljön az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="626a4-107">Create and delete Azure File shares</span></span>
* <span data-ttu-id="626a4-108">Hozzon létre vagy töröljön a könyvtárak</span><span class="sxs-lookup"><span data-stu-id="626a4-108">Create and delete directories</span></span>
* <span data-ttu-id="626a4-109">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="626a4-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="626a4-110">Töltse fel, töltse le és törölje a fájlt</span><span class="sxs-lookup"><span data-stu-id="626a4-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="626a4-111">Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite hello Azure fájlmegosztás hello szabványos Java i/o-osztály használatával elérhető egyszerű alkalmazásokat is.</span><span class="sxs-lookup"><span data-stu-id="626a4-111">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard Java I/O classes.</span></span> <span data-ttu-id="626a4-112">Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello Azure Storage Java SDK, hello használó [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="626a4-112">This article will describe how toowrite applications that use hello Azure Storage Java SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="626a4-113">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="626a4-113">Create a Java application</span></span>
<span data-ttu-id="626a4-114">toobuild hello minták, a Java fejlesztői készlet (JDK) hello és hello [Azure Storage SDK for Java] [-] lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="626a4-114">toobuild hello samples, you will need hello Java Development Kit (JDK) and hello [Azure Storage SDK for Java][].</span></span> <span data-ttu-id="626a4-115">Meg is létrehozott egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="626a4-115">You should also have created an Azure storage account.</span></span>

## <a name="setup-your-application-toouse-azure-file-storage"></a><span data-ttu-id="626a4-116">Az alkalmazás toouse Azure File storage beállítása</span><span class="sxs-lookup"><span data-stu-id="626a4-116">Setup your application toouse Azure File storage</span></span>
<span data-ttu-id="626a4-117">toouse hello az Azure storage API-k, adja hozzá a következő utasítás toohello felső hello Java fájl tooaccess hello storage szolgáltatással való célhelyeként hello.</span><span class="sxs-lookup"><span data-stu-id="626a4-117">toouse hello Azure storage APIs, add hello following statement toohello top of hello Java file where you intend tooaccess hello storage service from.</span></span>

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="626a4-118">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="626a4-118">Setup an Azure storage connection string</span></span>
<span data-ttu-id="626a4-119">Azure File storage toouse, tooconnect tooyour az Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="626a4-119">toouse Azure File storage, you need tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="626a4-120">hello első lépéseként lenne egy kapcsolati karakterláncot, amely fogjuk használni tooconfigure tooconnect tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="626a4-120">hello first step would be tooconfigure a connection string which we'll use tooconnect tooyour storage account.</span></span> <span data-ttu-id="626a4-121">Is határozza meg egy statikus változó toodo, amely.</span><span class="sxs-lookup"><span data-stu-id="626a4-121">Let's define a static variable toodo that.</span></span>

```java
// Configure hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> <span data-ttu-id="626a4-122">Cserélje le your_storage_account_name és your_storage_account_key hello tényleges értékek a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="626a4-122">Replace your_storage_account_name and your_storage_account_key with hello actual values for your storage account.</span></span>
> 
> 

## <a name="connecting-tooan-azure-storage-account"></a><span data-ttu-id="626a4-123">Csatlakozás tooan Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="626a4-123">Connecting tooan Azure storage account</span></span>
<span data-ttu-id="626a4-124">tooconnect tooyour tárfiókot kell toouse hello **CloudStorageAccount** objektum átadása egy kapcsolati karakterlánc tooits **elemezni** metódust.</span><span class="sxs-lookup"><span data-stu-id="626a4-124">tooconnect tooyour storage account, you need toouse hello **CloudStorageAccount** object, passing a connection string tooits **parse** method.</span></span>

```java
// Use hello CloudStorageAccount object tooconnect tooyour storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle hello exception
}
```

<span data-ttu-id="626a4-125">**CloudStorageAccount.parse** jelez, ezért szüksége tooput InvalidKeyException egy azt egy try vagy catch belül blokkolása.</span><span class="sxs-lookup"><span data-stu-id="626a4-125">**CloudStorageAccount.parse** throws an InvalidKeyException so you'll need tooput it inside a try/catch block.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="626a4-126">Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="626a4-126">Create an Azure File share</span></span>
<span data-ttu-id="626a4-127">A fájlok és könyvtárak az Azure File storage találhatók nevű tároló egy **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="626a4-127">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="626a4-128">A tárfiók lehet mértékű megosztások a kapacitásával lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="626a4-128">Your storage account can have as much shares as your account capacity allows.</span></span> <span data-ttu-id="626a4-129">tooobtain hozzáférés tooa megosztást és tartalmát, egy Azure File storage ügyfél toouse kell.</span><span class="sxs-lookup"><span data-stu-id="626a4-129">tooobtain access tooa share and its contents, you need toouse a Azure File storage client.</span></span>

```java
// Create hello Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

<span data-ttu-id="626a4-130">Hello Azure File storage-ügyfelet használ, majd szerezhet be egy hivatkozást tooa megosztást.</span><span class="sxs-lookup"><span data-stu-id="626a4-130">Using hello Azure File storage client, you can then obtain a reference tooa share.</span></span>

```java
// Get a reference toohello file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

<span data-ttu-id="626a4-131">tooactually hello megosztás létrehozása, használata hello **createIfNotExists** hello CloudFileShare metódusa.</span><span class="sxs-lookup"><span data-stu-id="626a4-131">tooactually create hello share, use hello **createIfNotExists** method of hello CloudFileShare object.</span></span>

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

<span data-ttu-id="626a4-132">Ezen a ponton **megosztása** tartalmazza a referencia-tooa nevű megosztás **sampleshare**.</span><span class="sxs-lookup"><span data-stu-id="626a4-132">At this point, **share** holds a reference tooa share named **sampleshare**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="626a4-133">Egy Azure fájlmegosztás törlése</span><span class="sxs-lookup"><span data-stu-id="626a4-133">Delete an Azure File share</span></span>
<span data-ttu-id="626a4-134">A megosztás törlése hívó hello végezhető el **deleteIfExists** CloudFileShare objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="626a4-134">Deleting a share is done by calling hello **deleteIfExists** method on a CloudFileShare object.</span></span> <span data-ttu-id="626a4-135">Itt látható mintakód, amelyet, amely.</span><span class="sxs-lookup"><span data-stu-id="626a4-135">Here's sample code that does that.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference toohello file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a><span data-ttu-id="626a4-136">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="626a4-136">Create a directory</span></span>
<span data-ttu-id="626a4-137">Tegyen alkönyvtárat ahelyett, az összes hello gyökérkönyvtárában lévő fájlok tárolási is rendezhetők.</span><span class="sxs-lookup"><span data-stu-id="626a4-137">You can also organize storage by putting files inside sub-directories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="626a4-138">Az Azure File storage lehetővé teszi toocreate sok könyvtárak, a fiók fog lehetővé.</span><span class="sxs-lookup"><span data-stu-id="626a4-138">Azure File storage allows you toocreate as much directories as your account will allow.</span></span> <span data-ttu-id="626a4-139">hello kódot hoz létre a nevű alkönyvtárát **sampledir** hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="626a4-139">hello code below will create a sub-directory named **sampledir** under hello root directory.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference toohello sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a><span data-ttu-id="626a4-140">Könyvtár törlése</span><span class="sxs-lookup"><span data-stu-id="626a4-140">Delete a directory</span></span>
<span data-ttu-id="626a4-141">A könyvtár törlése mely viszonylag egyszerű feladat, érdemes megjegyezni, hogy nem törölhető a fájlok továbbra is tartalmazó könyvtár vagy más címtárakban.</span><span class="sxs-lookup"><span data-stu-id="626a4-141">Deleting a directory is a fairly simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```java
// Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference toohello directory you want toodelete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete hello directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="626a4-142">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="626a4-142">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="626a4-143">Fájlok és könyvtárak egy megosztáson belüli listájának beszerzésével könnyen történik meghívásával **listFilesAndDirectories** CloudFileDirectory hivatkozással.</span><span class="sxs-lookup"><span data-stu-id="626a4-143">Obtaining a list of files and directories within a share is easily done by calling **listFilesAndDirectories** on a CloudFileDirectory reference.</span></span> <span data-ttu-id="626a4-144">hello metódus ListFileItem objektumokat, amely akkor is többször listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="626a4-144">hello method returns a list of ListFileItem objects which you can iterate on.</span></span> <span data-ttu-id="626a4-145">Tegyük fel hello alábbira felsorolja a fájlok és könyvtárak hello legfelső szintű könyvtárán belül.</span><span class="sxs-lookup"><span data-stu-id="626a4-145">As an example, hello following code will list files and directories inside hello root directory.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a><span data-ttu-id="626a4-146">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="626a4-146">Upload a file</span></span>
<span data-ttu-id="626a4-147">Egy megosztás nagyon legalább tartalmaz: hello Azure fájlt, egy gyökérkönyvtár fájlokat tároló is.</span><span class="sxs-lookup"><span data-stu-id="626a4-147">An Azure File share contains at hello very least, a root directory where files can reside.</span></span> <span data-ttu-id="626a4-148">Ebben a szakaszban megtudhatja, hogyan tooupload egy fájlt a helyi tároló alakzatot hello gyökérkönyvtár megosztás.</span><span class="sxs-lookup"><span data-stu-id="626a4-148">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="626a4-149">hello első lépése a fájl feltöltése tooobtain egy hivatkozás toohello könyvtárat, ahol kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="626a4-149">hello first step in uploading a file is tooobtain a reference toohello directory where it should reside.</span></span> <span data-ttu-id="626a4-150">Ehhez hívó hello **getRootDirectoryReference** hello megosztás metódusa.</span><span class="sxs-lookup"><span data-stu-id="626a4-150">You do this by calling hello **getRootDirectoryReference** method of hello share object.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

<span data-ttu-id="626a4-151">Most, hogy hello megosztás hivatkozás toohello gyökérkönyvtár, feltöltheti a fájlt a következő kód hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="626a4-151">Now that you have a reference toohello root directory of hello share, you can upload a file onto it using hello following code.</span></span>

```java
        // Define hello path tooa local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a><span data-ttu-id="626a4-152">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="626a4-152">Download a file</span></span>
<span data-ttu-id="626a4-153">További műveleteket fogja elvégezni szemben Azure File storage gyakori hello egyik toodownload fájlokat.</span><span class="sxs-lookup"><span data-stu-id="626a4-153">One of hello more frequent operations you will perform against Azure File storage is toodownload files.</span></span> <span data-ttu-id="626a4-154">A következő példa hello hello kód SampleFile.txt tölti le, és megjeleníti a tartalmát.</span><span class="sxs-lookup"><span data-stu-id="626a4-154">In hello following example, hello code downloads SampleFile.txt and displays its contents.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference toohello directory that contains hello file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference toohello file you want toodownload
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write hello contents of hello file toohello console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a><span data-ttu-id="626a4-155">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="626a4-155">Delete a file</span></span>
<span data-ttu-id="626a4-156">Közös Azure File storage egy másik művelet a fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="626a4-156">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="626a4-157">hello alábbira töröl egy fájlt egy nevű könyvtár tárolt SampleFile.txt nevű **sampledir**.</span><span class="sxs-lookup"><span data-stu-id="626a4-157">hello following code deletes a file named SampleFile.txt stored inside a directory named **sampledir**.</span></span>

```java
// Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference toohello directory where hello file toobe deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a><span data-ttu-id="626a4-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="626a4-158">Next steps</span></span>
<span data-ttu-id="626a4-159">Ha szeretné más Azure storage API-kkal kapcsolatos további toolearn, kövesse ezeket a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="626a4-159">If you would like toolearn more about other Azure storage APIs, follow these links.</span></span>

* <span data-ttu-id="626a4-160">[Azure Java-fejlesztőknek](/java/azure)/)</span><span class="sxs-lookup"><span data-stu-id="626a4-160">[Azure for Java developers](/java/azure)/)</span></span>
* [<span data-ttu-id="626a4-161">Az Azure Storage Java SDK</span><span class="sxs-lookup"><span data-stu-id="626a4-161">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="626a4-162">Az Azure Storage SDK for Android</span><span class="sxs-lookup"><span data-stu-id="626a4-162">Azure Storage SDK for Android</span></span>](https://github.com/azure/azure-storage-android)
* [<span data-ttu-id="626a4-163">Az Azure Storage ügyfél SDK-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="626a4-163">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="626a4-164">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="626a4-164">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="626a4-165">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="626a4-165">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="626a4-166">[Adatátvitel az AzCopy parancssori segédprogram hello](../common/storage-use-azcopy.md* [Troubleshooting Azure File storage problems - Windows](storage-troubleshoot-windows-file-connection-problems.md)
)</span><span class="sxs-lookup"><span data-stu-id="626a4-166">[Transfer data with hello AzCopy Command-Line Utility](../common/storage-use-azcopy.md* [Troubleshooting Azure File storage problems - Windows](storage-troubleshoot-windows-file-connection-problems.md)
)</span></span>