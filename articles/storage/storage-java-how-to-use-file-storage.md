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
ms.openlocfilehash: b50703815daf2c829e7e9a9a4196c31a2b8727e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a>Az Azure File storage Java fejlesztése
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ez az oktatóanyag mutatni, Java toodevelop alkalmazásokhoz vagy szolgáltatásokhoz, használja az Azure File storage toostore fájladatok használatával hello alapjait. Ebben az oktatóanyagban a rendszer létrehoz egy egyszerű konzolalkalmazásként és megjelenítése hogyan tooperform alapszintű műveleteket az Java és az Azure File storage szolgáltatással:

* Hozzon létre vagy töröljön az Azure fájlmegosztások
* Hozzon létre vagy töröljön a könyvtárak
* Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
* Töltse fel, töltse le és törölje a fájlt

> [!Note]  
> Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite hello Azure fájlmegosztás hello szabványos Java i/o-osztály használatával elérhető egyszerű alkalmazásokat is. Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello Azure Storage Java SDK, hello használó [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására.

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása
toobuild hello minták, a Java fejlesztői készlet (JDK) hello és hello [Azure Storage SDK for Java] [-] lesz szüksége. Meg is létrehozott egy Azure storage-fiókot.

## <a name="setup-your-application-toouse-azure-file-storage"></a>Az alkalmazás toouse Azure File storage beállítása
toouse hello az Azure storage API-k, adja hozzá a következő utasítás toohello felső hello Java fájl tooaccess hello storage szolgáltatással való célhelyeként hello.

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Egy Azure storage kapcsolati karakterlánc beállítása
Azure File storage toouse, tooconnect tooyour az Azure storage-fiók szükséges. hello első lépéseként lenne egy kapcsolati karakterláncot, amely fogjuk használni tooconfigure tooconnect tooyour tárfiók. Is határozza meg egy statikus változó toodo, amely.

```java
// Configure hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Cserélje le your_storage_account_name és your_storage_account_key hello tényleges értékek a tárfiók.
> 
> 

## <a name="connecting-tooan-azure-storage-account"></a>Csatlakozás tooan Azure storage-fiók
tooconnect tooyour tárfiókot kell toouse hello **CloudStorageAccount** objektum átadása egy kapcsolati karakterlánc tooits **elemezni** metódust.

```java
// Use hello CloudStorageAccount object tooconnect tooyour storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle hello exception
}
```

**CloudStorageAccount.parse** jelez, ezért szüksége tooput InvalidKeyException egy azt egy try vagy catch belül blokkolása.

## <a name="create-an-azure-file-share"></a>Azure-fájlmegosztás létrehozása
A fájlok és könyvtárak az Azure File storage találhatók nevű tároló egy **megosztás**. A tárfiók lehet mértékű megosztások a kapacitásával lehetővé teszi. tooobtain hozzáférés tooa megosztást és tartalmát, egy Azure File storage ügyfél toouse kell.

```java
// Create hello Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Hello Azure File storage-ügyfelet használ, majd szerezhet be egy hivatkozást tooa megosztást.

```java
// Get a reference toohello file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

tooactually hello megosztás létrehozása, használata hello **createIfNotExists** hello CloudFileShare metódusa.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

Ezen a ponton **megosztása** tartalmazza a referencia-tooa nevű megosztás **sampleshare**.

## <a name="delete-an-azure-file-share"></a>Egy Azure fájlmegosztás törlése
A megosztás törlése hívó hello végezhető el **deleteIfExists** CloudFileShare objektum metódust. Itt látható mintakód, amelyet, amely.

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

## <a name="create-a-directory"></a>Könyvtár létrehozása
Tegyen alkönyvtárat ahelyett, az összes hello gyökérkönyvtárában lévő fájlok tárolási is rendezhetők. Az Azure File storage lehetővé teszi toocreate sok könyvtárak, a fiók fog lehetővé. hello kódot hoz létre a nevű alkönyvtárát **sampledir** hello gyökérkönyvtárban.

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

## <a name="delete-a-directory"></a>Könyvtár törlése
A könyvtár törlése mely viszonylag egyszerű feladat, érdemes megjegyezni, hogy nem törölhető a fájlok továbbra is tartalmazó könyvtár vagy más címtárakban.

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

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
Fájlok és könyvtárak egy megosztáson belüli listájának beszerzésével könnyen történik meghívásával **listFilesAndDirectories** CloudFileDirectory hivatkozással. hello metódus ListFileItem objektumokat, amely akkor is többször listáját adja vissza. Tegyük fel hello alábbira felsorolja a fájlok és könyvtárak hello legfelső szintű könyvtárán belül.

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Fájl feltöltése
Egy megosztás nagyon legalább tartalmaz: hello Azure fájlt, egy gyökérkönyvtár fájlokat tároló is. Ebben a szakaszban megtudhatja, hogyan tooupload egy fájlt a helyi tároló alakzatot hello gyökérkönyvtár megosztás.

hello első lépése a fájl feltöltése tooobtain egy hivatkozás toohello könyvtárat, ahol kell tárolni. Ehhez hívó hello **getRootDirectoryReference** hello megosztás metódusa.

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Most, hogy hello megosztás hivatkozás toohello gyökérkönyvtár, feltöltheti a fájlt a következő kód hello segítségével.

```java
        // Define hello path tooa local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Fájl letöltése
További műveleteket fogja elvégezni szemben Azure File storage gyakori hello egyik toodownload fájlokat. A következő példa hello hello kód SampleFile.txt tölti le, és megjeleníti a tartalmát.

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

## <a name="delete-a-file"></a>Fájl törlése
Közös Azure File storage egy másik művelet a fájl törlése. hello alábbira töröl egy fájlt egy nevű könyvtár tárolt SampleFile.txt nevű **sampledir**.

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

## <a name="next-steps"></a>Következő lépések
Ha szeretné más Azure storage API-kkal kapcsolatos további toolearn, kövesse ezeket a hivatkozásokat.

* [Java fejlesztői központban](http://azure.microsoft.com/develop/java/)
* [Az Azure Storage Java SDK](https://github.com/azure/azure-storage-java)
* [Az Azure Storage SDK for Android](https://github.com/azure/azure-storage-android)
* [Az Azure Storage ügyfél SDK-dokumentáció](http://dl.windowsazure.com/storage/javadoc/)
* [Az Azure Storage-szolgáltatások REST API-ja](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Az Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)
* [Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)
