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
# <a name="how-toouse-blob-storage-from-java"></a>Hogyan toouse Blob storage-ának Java
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Áttekintés
Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A BLOB storage is az említett tooas objektum tároló.

Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Microsoft Azure Blob Storage tárolóban. hello minták Java nyelven íródtak, és használja a hello [Azure Storage SDK for Java][Azure Storage SDK for Java]. hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat. A blobok további információkért lásd: hello [lépések](#Next-Steps) szakasz.

> [!NOTE]
> Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön. További információkért lásd: hello [Azure Storage SDK for Android][Azure Storage SDK for Android].
>
>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása
Ez a cikk tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.

toodo tooinstall kell tehát hello Java fejlesztői készlet (JDK), és hozzon létre egy Azure Storage-fiókot az Azure-előfizetéshez. Ha ezzel végzett, szüksége lesz a fejlesztői rendszerhez megfelelő hello minimális követelményeit és függőségeit, amelyek hello vannak felsorolva tooverify [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából. Ha a rendszer megfelel ezeknek a követelményeknek, letöltése és telepítése hello Azure Storage-könyvtárakban Java ebből a rendszeren hello utasításokat kövesse. E feladatok befejezése után fog tudni toocreate ebben a cikkben hello példák használó Java-alkalmazások.

## <a name="configure-your-application-tooaccess-blob-storage"></a>Az alkalmazás tooaccess Blob-tároló konfigurálása
Adja hozzá a következő importálási utasítások toohello felső részén hello Java fájl toouse hello Azure Storage API-k tooaccess blobok hello.

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Egy Azure Storage kapcsolati karakterlánc beállítása
Egy Azure Storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatokat felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat. Ha egy ügyfél-alkalmazás fut, adja meg a hello tárolási kapcsolati karakterlánc formátuma a következő, a tárfiók nevére hello segítségével hello majd hello felsorolt hello tárfiók elsődleges elérési kulcsát hello [Azure-portálon](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket. hello következő példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot.

```java
// Define hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja hello szolgáltatás konfigurációs fájljában, *ServiceConfiguration.cscfg*, és egy hívás toohello az elérhető  **RoleEnvironment.getConfigurationSettings** metódust. hello alábbi példa lekérdezi hello származó kapcsolati karakterlánc egy **beállítás** nevű elem *StorageConnectionString* hello szolgáltatás konfigurációs fájljában.

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.

## <a name="create-a-container"></a>Tároló létrehozása
A **CloudBlobClient** objektum lehetővé teszi, hogy a tárolók és blobok hivatkozási objektumok beolvasása. hello alábbi kód létrehoz egy **CloudBlobClient** objektum.

> [!NOTE]
> Nincsenek további lehetőségek toocreate **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a hello [Azure Storage ügyfél SDK-dokumentáció].
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Használjon hello **CloudBlobClient** tooget egy hivatkozás toohello tároló toouse kívánt objektumot. Hello tároló hozhat létre, ha még nem létezik a hello **createIfNotExists** metódus, amely egyébként visszaadható hello meglévő tárolóhoz. Alapértelmezés szerint hello új tároló sajátja, ezért meg kell adnia a tárelérési kulcsát (úgy, ahogy korábban) toodownload blobok ebből a tárolóból.

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

### <a name="optional-configure-a-container-for-public-access"></a>Választható lehetőség: A tárolók nyilvános hozzáférés konfigurálása
Egy tároló engedélyek privát hozzáférést alapértelmezés szerint vannak konfigurálva, de egy tároló engedélyek tooallow nyilvános, csak olvasható hozzáférést minden felhasználó számára könnyen konfigurálható hello Internet:

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in hello permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set hello permissions on hello container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
tooupload fájl tooa blob, szerezze be a tároló hivatkozását, és tooget egy blobhivatkozást használják. Miután egy blobhivatkozást, a feltöltési meghívásával beolvasott hello blobhivatkozást bármilyen streamet feltölthet. Ez a művelet létrehoz hello blob nem létezik, vagy felülírja, ha létezik. a következő példakód hello ezt mutatja be, és feltételezi, hogy hello tároló már létre van hozva.

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

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását, ahogyan azt tette tooupload blob. Hello tárolót is használhatja **listBlobs** metódust egy **a** hurok. hello alábbira kimenete hello URI-címe, minden egyes blob tároló toohello konzolban.

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

Vegye figyelembe, hogy a blobok elérési út adatait a név lehet neve. Ez egy olyan virtuális könyvtárstruktúrát hoz létre, amelyet egy hagyományos fájlrendszerhez hasonlóan rendszerezhet és bejárhat. Vegye figyelembe, hogy csak virtuális könyvtárstruktúrát hello – hello csak forrásanyag is elérhető a Blob Storage tárolóban tárolók és blobok. Azonban hello ügyféloldali kódtár kínál a **CloudBlobDirectory** toorefer tooa virtuális könyvtár objektum és az ezzel a módszerrel blobokkal hello folyamat leegyszerűsítése érdekében.

Lehet például a "photos", amelyben blobok "rootphoto1", "2010/photo1", "2010/photo2" nevű és "2011/photo1" lehet, hogy feltöltése nevű tárolót. Ez akkor hello virtuális könyvtárak létrehozása "2010" és "2011" hello "photos" tárolóban. A hívás esetén **listBlobs** hello "photos" tárolóban, a visszaadott hello gyűjtemény fogja tartalmazni **CloudBlobDirectory** és **CloudBlob** hello objektumokból illetve hello felső szinten található blobokat. Ebben az esetben "2010" és "2011" könyvtárak, valamint fénykép "rootphoto1" vissza. Használhatja a hello **instanceof** operátor toodistinguish ezeket az objektumokat.

Ha szükséges, átviheti paraméterek toohello **listBlobs** hello metódus **Listblobs** paraméterkészlet tootrue. Ennek eredményeképpen az összes blobjának ad vissza, függetlenül attól, könyvtár. További információkért lásd: **CloudBlobContainer.listBlobs** a hello [Azure Storage ügyfél SDK-dokumentáció].

## <a name="download-a-blob"></a>Blob letöltése
toodownload blobok, az alábbi hello azonos lépések sorrendben tooget egy blobhivatkozást a blob feltöltése hasonló módon. Hello példa feltöltése, a feltöltési hello blob objektum neve. A következő példa hello, hívja a letöltési tootransfer hello blob tartalma tooa adatfolyam-objektum többek között a **FileOutputStream** használható toopersist hello tooa helyi fájlját.

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

## <a name="delete-a-blob"></a>Blob törlése
egy blob toodelete lekérése blob hivatkoznak, és hívja meg **deleteIfExists**.

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

## <a name="delete-a-blob-container"></a>A blob-tároló törlése
Végezetül toodelete egy blob tároló lekérése blob tároló hivatkozását, és hívás **deleteIfExists**.

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

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Blob storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.

* [Az Azure Storage Java SDK][Azure Storage SDK for Java]
* [Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage ügyfél SDK-dokumentáció]
* [Az Azure Storage REST API-n][Azure Storage REST API]
* [Az Azure Storage csapat blogja][Azure Storage Team Blog]

További információkért lásd még: hello [Java fejlesztői központ](/develop/java/).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
