---
title: "Az Azure File storage Java kidolgozása |} Microsoft Docs"
description: "Útmutató a Java-alkalmazások és adatok tárolásához Azure File storage használó szolgáltatások fejlesztéséhez."
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
ms.openlocfilehash: 16924599e49990265e07f7a58613756d93c46942
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a><span data-ttu-id="6653a-103">Az Azure File storage Java fejlesztése</span><span class="sxs-lookup"><span data-stu-id="6653a-103">Develop for Azure File storage with Java</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="6653a-104">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="6653a-104">About this tutorial</span></span>
<span data-ttu-id="6653a-105">Ez az oktatóanyag mutatni, a Java használatával alkalmazásokhoz vagy szolgáltatásokhoz, tárolhatja a fájljait az Azure File storage segítségével fejlesztéséhez alapjait.</span><span class="sxs-lookup"><span data-stu-id="6653a-105">This tutorial will demonstrate the basics of using Java to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="6653a-106">Ebben az oktatóanyagban a rendszer egyszerű Konzolalkalmazás létrehozása és a Java és az Azure File storage alapvető műveleteket szemléltetik:</span><span class="sxs-lookup"><span data-stu-id="6653a-106">In this tutorial, we will create a simple console application and show how to perform basic actions with Java and Azure File storage:</span></span>

* <span data-ttu-id="6653a-107">Hozzon létre vagy töröljön az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="6653a-107">Create and delete Azure File shares</span></span>
* <span data-ttu-id="6653a-108">Hozzon létre vagy töröljön a könyvtárak</span><span class="sxs-lookup"><span data-stu-id="6653a-108">Create and delete directories</span></span>
* <span data-ttu-id="6653a-109">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="6653a-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="6653a-110">Töltse fel, töltse le és törölje a fájlt</span><span class="sxs-lookup"><span data-stu-id="6653a-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="6653a-111">Mivel előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, akkor lehet egyszerű alkalmazások írását, amelyek a szabványos Java i/o-osztályokat Azure fájlmegosztás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="6653a-111">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard Java I/O classes.</span></span> <span data-ttu-id="6653a-112">Ez a cikk ismerteti, hogyan írhat alkalmazásokat, amelyek használják az Azure Storage Java SDK, amely használja a [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) felvegye a Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="6653a-112">This article will describe how to write applications that use the Azure Storage Java SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="6653a-113">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6653a-113">Create a Java application</span></span>
<span data-ttu-id="6653a-114">A minták összeállítása, szüksége lesz a Java fejlesztői készlet (JDK) és az [Azure Storage SDK for Java] [].</span><span class="sxs-lookup"><span data-stu-id="6653a-114">To build the samples, you will need the Java Development Kit (JDK) and the [Azure Storage SDK for Java][].</span></span> <span data-ttu-id="6653a-115">Meg is létrehozott egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="6653a-115">You should also have created an Azure storage account.</span></span>

## <a name="setup-your-application-to-use-azure-file-storage"></a><span data-ttu-id="6653a-116">A használatára az Azure File storage-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="6653a-116">Setup your application to use Azure File storage</span></span>
<span data-ttu-id="6653a-117">Az Azure storage API-k, vegye fel a következő utasítás célhelyeként a a társzolgáltatás hozzáférni a Java-fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="6653a-117">To use the Azure storage APIs, add the following statement to the top of the Java file where you intend to access the storage service from.</span></span>

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="6653a-118">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="6653a-118">Setup an Azure storage connection string</span></span>
<span data-ttu-id="6653a-119">Azure File storage használatához szüksége az Azure storage-fiókjához.</span><span class="sxs-lookup"><span data-stu-id="6653a-119">To use Azure File storage, you need to connect to your Azure storage account.</span></span> <span data-ttu-id="6653a-120">Az első lépés is konfigurálhat egy kapcsolati karakterláncot, amely csatlakozni a tárfiókhoz fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="6653a-120">The first step would be to configure a connection string which we'll use to connect to your storage account.</span></span> <span data-ttu-id="6653a-121">Nézzük meg egy statikus változót ehhez.</span><span class="sxs-lookup"><span data-stu-id="6653a-121">Let's define a static variable to do that.</span></span>

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> <span data-ttu-id="6653a-122">Cserélje le a tárfiók a tényleges értékek your_storage_account_name és your_storage_account_key.</span><span class="sxs-lookup"><span data-stu-id="6653a-122">Replace your_storage_account_name and your_storage_account_key with the actual values for your storage account.</span></span>
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a><span data-ttu-id="6653a-123">Az Azure storage-fiókhoz való csatlakozást</span><span class="sxs-lookup"><span data-stu-id="6653a-123">Connecting to an Azure storage account</span></span>
<span data-ttu-id="6653a-124">A tárfiók csatlakozni kell használnia a **CloudStorageAccount** objektum átadása egy kapcsolati karakterláncot a **elemezni** metódus.</span><span class="sxs-lookup"><span data-stu-id="6653a-124">To connect to your storage account, you need to use the **CloudStorageAccount** object, passing a connection string to its **parse** method.</span></span>

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

<span data-ttu-id="6653a-125">**CloudStorageAccount.parse** egy InvalidKeyException jelez, ezért kell tenni egy try vagy catch blokkon belül.</span><span class="sxs-lookup"><span data-stu-id="6653a-125">**CloudStorageAccount.parse** throws an InvalidKeyException so you'll need to put it inside a try/catch block.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="6653a-126">Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6653a-126">Create an Azure File share</span></span>
<span data-ttu-id="6653a-127">A fájlok és könyvtárak az Azure File storage találhatók nevű tároló egy **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="6653a-127">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="6653a-128">A tárfiók lehet mértékű megosztások a kapacitásával lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="6653a-128">Your storage account can have as much shares as your account capacity allows.</span></span> <span data-ttu-id="6653a-129">Történő hozzáférés a megosztást és tartalmát, Azure File storage-ügyfél használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="6653a-129">To obtain access to a share and its contents, you need to use a Azure File storage client.</span></span>

```java
// Create the Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

<span data-ttu-id="6653a-130">Az Azure File storage ügyfélprogrammal, majd szerezhet be egy hivatkozást egy megosztásra.</span><span class="sxs-lookup"><span data-stu-id="6653a-130">Using the Azure File storage client, you can then obtain a reference to a share.</span></span>

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

<span data-ttu-id="6653a-131">A megosztás ténylegesen létrehozásához használja a **createIfNotExists** CloudFileShare metódusa.</span><span class="sxs-lookup"><span data-stu-id="6653a-131">To actually create the share, use the **createIfNotExists** method of the CloudFileShare object.</span></span>

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

<span data-ttu-id="6653a-132">Ezen a ponton **megosztása** nevű megosztáshoz egy hivatkozást tartalmaz **sampleshare**.</span><span class="sxs-lookup"><span data-stu-id="6653a-132">At this point, **share** holds a reference to a share named **sampleshare**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="6653a-133">Egy Azure fájlmegosztás törlése</span><span class="sxs-lookup"><span data-stu-id="6653a-133">Delete an Azure File share</span></span>
<span data-ttu-id="6653a-134">Egy megosztás törlése meghívásával történik a **deleteIfExists** CloudFileShare objektum metódus.</span><span class="sxs-lookup"><span data-stu-id="6653a-134">Deleting a share is done by calling the **deleteIfExists** method on a CloudFileShare object.</span></span> <span data-ttu-id="6653a-135">Itt látható mintakód, amelyet, amely.</span><span class="sxs-lookup"><span data-stu-id="6653a-135">Here's sample code that does that.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a><span data-ttu-id="6653a-136">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="6653a-136">Create a directory</span></span>
<span data-ttu-id="6653a-137">Tárolási tegyen alkönyvtárat így ahelyett hogy ezek a gyökérkönyvtárban található fájlok is rendezhetők.</span><span class="sxs-lookup"><span data-stu-id="6653a-137">You can also organize storage by putting files inside sub-directories instead of having all of them in the root directory.</span></span> <span data-ttu-id="6653a-138">Az Azure File storage ugyanennyi könyvtárat fiókját engedélyezi létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="6653a-138">Azure File storage allows you to create as much directories as your account will allow.</span></span> <span data-ttu-id="6653a-139">Az alábbi kódot hoz létre a nevű alkönyvtárát **sampledir** a gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="6653a-139">The code below will create a sub-directory named **sampledir** under the root directory.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a><span data-ttu-id="6653a-140">Könyvtár törlése</span><span class="sxs-lookup"><span data-stu-id="6653a-140">Delete a directory</span></span>
<span data-ttu-id="6653a-141">A könyvtár törlése mely viszonylag egyszerű feladat, érdemes megjegyezni, hogy nem törölhető a fájlok továbbra is tartalmazó könyvtár vagy más címtárakban.</span><span class="sxs-lookup"><span data-stu-id="6653a-141">Deleting a directory is a fairly simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="6653a-142">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="6653a-142">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="6653a-143">Fájlok és könyvtárak egy megosztáson belüli listájának beszerzésével könnyen történik meghívásával **listFilesAndDirectories** CloudFileDirectory hivatkozással.</span><span class="sxs-lookup"><span data-stu-id="6653a-143">Obtaining a list of files and directories within a share is easily done by calling **listFilesAndDirectories** on a CloudFileDirectory reference.</span></span> <span data-ttu-id="6653a-144">A metódus ListFileItem objektumokat, amely akkor is többször listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6653a-144">The method returns a list of ListFileItem objects which you can iterate on.</span></span> <span data-ttu-id="6653a-145">Tegyük fel az alábbi kód felsorolja a fájlok és könyvtárak a legfelső szintű könyvtárán belül.</span><span class="sxs-lookup"><span data-stu-id="6653a-145">As an example, the following code will list files and directories inside the root directory.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a><span data-ttu-id="6653a-146">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="6653a-146">Upload a file</span></span>
<span data-ttu-id="6653a-147">A megosztás tartalmaz legalább egy Azure-fájl, egy gyökérkönyvtár fájlokat tároló is.</span><span class="sxs-lookup"><span data-stu-id="6653a-147">An Azure File share contains at the very least, a root directory where files can reside.</span></span> <span data-ttu-id="6653a-148">Ebben a szakaszban megtudhatja, hogyan feltölteni a fájlt a helyi tároló megosztás gyökérkönyvtárában alakzatot.</span><span class="sxs-lookup"><span data-stu-id="6653a-148">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="6653a-149">Az első lépés egy feltöltés az beszerzése hivatkozni kell a könyvtárban, ahol kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="6653a-149">The first step in uploading a file is to obtain a reference to the directory where it should reside.</span></span> <span data-ttu-id="6653a-150">Ehhez hívja a **getRootDirectoryReference** a megosztás metódusa.</span><span class="sxs-lookup"><span data-stu-id="6653a-150">You do this by calling the **getRootDirectoryReference** method of the share object.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

<span data-ttu-id="6653a-151">Most, hogy a megosztás gyökérkönyvtárában mutató hivatkozás, feltöltheti a fájlt, az alábbi kód használatával.</span><span class="sxs-lookup"><span data-stu-id="6653a-151">Now that you have a reference to the root directory of the share, you can upload a file onto it using the following code.</span></span>

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a><span data-ttu-id="6653a-152">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="6653a-152">Download a file</span></span>
<span data-ttu-id="6653a-153">A gyakoribb műveletek fogja elvégezni szemben Azure File storage egyik fájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="6653a-153">One of the more frequent operations you will perform against Azure File storage is to download files.</span></span> <span data-ttu-id="6653a-154">A következő példában a kód SampleFile.txt tölti le, és megjeleníti a tartalmát.</span><span class="sxs-lookup"><span data-stu-id="6653a-154">In the following example, the code downloads SampleFile.txt and displays its contents.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a><span data-ttu-id="6653a-155">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="6653a-155">Delete a file</span></span>
<span data-ttu-id="6653a-156">Közös Azure File storage egy másik művelet a fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="6653a-156">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="6653a-157">Az alábbi kód töröl egy fájlt egy nevű könyvtár tárolt SampleFile.txt nevű **sampledir**.</span><span class="sxs-lookup"><span data-stu-id="6653a-157">The following code deletes a file named SampleFile.txt stored inside a directory named **sampledir**.</span></span>

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a><span data-ttu-id="6653a-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6653a-158">Next steps</span></span>
<span data-ttu-id="6653a-159">Ha azt szeretné, ha többet szeretne megtudni az egyéb Azure storage API-k, az alábbi hivatkozásokat követve.</span><span class="sxs-lookup"><span data-stu-id="6653a-159">If you would like to learn more about other Azure storage APIs, follow these links.</span></span>

* [<span data-ttu-id="6653a-160">Java fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="6653a-160">Java Developer Center</span></span>](http://azure.microsoft.com/develop/java/)
* [<span data-ttu-id="6653a-161">Az Azure Storage Java SDK</span><span class="sxs-lookup"><span data-stu-id="6653a-161">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="6653a-162">Az Azure Storage SDK for Android</span><span class="sxs-lookup"><span data-stu-id="6653a-162">Azure Storage SDK for Android</span></span>](https://github.com/azure/azure-storage-android)
* [<span data-ttu-id="6653a-163">Az Azure Storage ügyfél SDK-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="6653a-163">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="6653a-164">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="6653a-164">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="6653a-165">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="6653a-165">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="6653a-166">Adatátvitel az AzCopy parancssori segédprogrammal</span><span class="sxs-lookup"><span data-stu-id="6653a-166">Transfer data with the AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)