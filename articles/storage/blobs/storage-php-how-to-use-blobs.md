---
title: "A blob storage (object storage) php-ből használata |} Microsoft Docs"
description: "Store unstructured data in the cloud with Azure Blob storage (object storage) (Strukturálatlan adatok tárolása a felhőben Azure Blob Storage-fiókkal (objektumtároló))."
documentationcenter: php
services: storage
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 4b68844c5d0553eaede3997bf09bff4fe570e850
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-blob-storage-from-php"></a>Php-ből a blob storage használata
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Az Azure Blob Storage egy olyan szolgáltatás, amely a strukturálatlan adatokat objektumként/blobként tárolja a felhőben. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A Blob Storage más néven objektumtárnak is hívható.

Ez az útmutató bemutatja, hogyan hajthat végre a szolgáltatást az Azure blob szolgáltatást használó általános forgatókönyvhöz. A mintákat a PHP és -felhasználási nyelven íródtak a [Azure SDK for PHP][download]. Az ismertetett forgatókönyvek **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat. A blobok további információkért lásd: a [további lépések](#next-steps) szakasz.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása
Egyetlen követelménye, amely hozzáfér az Azure blob szolgáltatáshoz PHP-alkalmazások létrehozásának, a hivatkozási osztályok az Azure SDK-ban a php a kód. A fejlesztői eszközök hozhat létre az alkalmazás, például a Jegyzettömbbel.

Ebben az útmutatóban használhatja helyben vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó PHP-alkalmazások belül nevű szolgáltatása.

## <a name="get-the-azure-client-libraries"></a>Az Azure Ügyfélkódtárai beolvasása
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Állítsa be az alkalmazását, a blob szolgáltatás eléréséhez
Az Azure blob szolgáltatás API-kat használ, meg kell:

1. A robotot fájl használatával hivatkozik a [require_once] utasítást, és
2. Bármely osztályok segítségével lehet hivatkozni.

A következő példa bemutatja, hogyan a robotot fájl és a hivatkozás a **ServicesBuilder** osztály.

> [!NOTE]
> Ebben a cikkben szereplő példák azt feltételezik, hogy telepítette az Azure-szerkesztő segítségével, a PHP Klienskódtárak segítségével. Ha a könyvtárak manuálisan telepítette, hivatkoznia kell a `WindowsAzure.php` robotjába fájlt.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Az alábbi példákban a `require_once` utasítás mindig megjelennek, de csak a szükséges végrehajtandó például osztályok hivatkozott.

## <a name="set-up-an-azure-storage-connection"></a>Az Azure storage-kapcsolat beállítása
Egy Azure blob szolgáltatásügyfél példányt létrehozni, először egy érvényes kapcsolati karakterláncot kell rendelkeznie. A blob szolgáltatás kapcsolati karakterláncának formátuma:

Élő szolgáltatások elérésére:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

A storage emulator eléréséhez:

```php
UseDevelopmentStorage=true
```

Bármely Azure szolgáltatás ügyfele létrehozásához kell használnia a **ServicesBuilder** osztály. A következőket teheti:

* a kapcsolati karakterláncot adja át a közvetlenül vagy
* használja a **CloudConfigurationManager (CCM)** ellenőrizze a kapcsolati karakterlánc több külső forrás:
  * Alapértelmezés szerint az tartalmaz egy külső adatforrás - környezeti változók támogatása.
  * Új forrásból történő kiterjesztésével adhat hozzá a **ConnectionStringSource** osztály.

Az itt leírt példák a kapcsolati karakterlánc közvetlenül fog adható át.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Tároló létrehozása
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** objektum lehetővé teszi, hogy a blob tárolókat hozhat létre a **createContainer** metódust. A tároló létrehozásakor a tárolóra vonatkozó beállítások adhatók meg, de ez így nincs szükség. (Az alábbi példa bemutatja, hogyan állíthatja be a tároló hozzáférés-vezérlési lista (ACL) és a tároló metaadatait.)

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Hívása **setPublicAccess (PublicAccessType::CONTAINER\_és\_BLOBOK)** elérhetővé válnak a tároló és a blob adatainak elérése névtelen kérésekkel. Hívása **setPublicAccess(PublicAccessType::BLOBS_ONLY)** teszi csak blob-adatok elérése névtelen kérésekkel érhető el. Tároló hozzáférés-vezérlési listák kapcsolatos további információkért lásd: [Set tároló hozzáférés-vezérlési lista (REST API-t)][container-acl].

Blob szolgáltatás hibakódokkal kapcsolatban további információkért lásd: [Blob hibakódjai][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
Feltölteni a fájlt egy blobba, használja a **BlobRestProxy -> createBlockBlob** metódust. Ez a művelet a blob hoz létre, ha nem létezik, vagy felülírja, ha támogatja. Az alábbi Kódpélda feltételezi, hogy a tároló már létre lett hozva, és használja [fopen] [ fopen] adatfolyamként fájl megnyitásához.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Vegye figyelembe, hogy a fenti példában feltölt egy blobot adatfolyamként. Azonban egy blob is feltölthetők a karakterlánc használja, mint például a [fájl\_beolvasása\_tartalma] [ file_get_contents] függvény. Ehhez használja a fenti példában, módosítsa `$content = fopen("c:\myfile.txt", "r");` való `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>A tárolóban lévő blobok listázása
A tárolóban lévő blobok listázásához, használja a **BlobRestProxy -> listBlobs** metódust egy **foreach** hurok a hurok a eredményén keresztül. A következő kódot neve minden egyes blob a tárolóban lévő kimenetként és jeleníti meg az URI a böngészőre.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // List blobs.
    $blob_list = $blobRestProxy->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a>Blob letöltése
Töltse le a blob, hívja meg a **BlobRestProxy getBlob ->** módszer, majd hívja a **getContentStream** metódust a kapott **GetBlobResult** objektum.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Vegye figyelembe, hogy a fenti példa blob lekérdezi egy adatfolyam-erőforrásként (alapértelmezés). Azonban használhatja a [adatfolyam\_beolvasása\_tartalma] [ stream-get-contents] működnek, mint a visszaadott stream alakítható át karakterlánccá.

## <a name="delete-a-blob"></a>Blob törlése
Blobok törléséhez, adja át a tároló és a blob neve **BlobRestProxy -> deleteBlob**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Delete blob.
    $blobRestProxy->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a>A blob-tároló törlése
Végül egy blob-tároló törlése, a tároló nevét át **BlobRestProxy -> deleteContainer**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete container.
    $blobRestProxy->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte az Azure blob szolgáltatás alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.

* Látogasson el a [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)
* Tekintse meg a [PHP blokk blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
* Tekintse meg a [PHP lap blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
* [Adatátvitel az AzCopy parancssori segédprogrammal](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

További információ: a [PHP fejlesztői központ](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
