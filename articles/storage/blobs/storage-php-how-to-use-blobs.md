---
title: "aaaHow toouse a blob storage (object storage) php-ből |} Microsoft Docs"
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
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
ms.openlocfilehash: 2e77415519b38007652e3ea372da531b3a97c5d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-php"></a>Hogyan toouse blob-tároló php-ből
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A BLOB storage is az említett tooas objektum tároló.

Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure blob szolgáltatás. hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP][download]. hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat. A blobok további információkért lásd: hello [további lépések](#next-steps) szakasz.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása
egyetlen, aki hozzáfér hello Azure blob szolgáltatáshoz PHP-alkalmazások létrehozására vonatkozó követelmény az hello hello való hivatkozás hello Azure SDK-t az osztályokat a php a kód. A fejlesztői eszközök toocreate használhatja az alkalmazás, például a Jegyzettömbbel.

Ebben az útmutatóban használhatja helyben vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó PHP-alkalmazások belül nevű szolgáltatása.

## <a name="get-hello-azure-client-libraries"></a>Hello Azure Ügyfélkódtárai beolvasása
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-blob-service"></a>Az alkalmazás tooaccess hello blob szolgáltatás konfigurálása
toouse hello Azure blob szolgáltatás API-k, kell:

1. Hello robotjába referenciafájlban hello segítségével [require_once] utasítást, és
2. Bármely osztályok segítségével lehet hivatkozni.

hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServicesBuilder** osztály.

> [!NOTE]
> Ebben a cikkben a hello példák feltételezik, hogy telepítette hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával. Ha manuálisan telepítette a hello szalagtárak, meg kell-e tooreference hello `WindowsAzure.php` robotjába fájlt.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Hello a következő példa a hello `require_once` utasítás mindig megjelennek, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.

## <a name="set-up-an-azure-storage-connection"></a>Az Azure storage-kapcsolat beállítása
egy Azure blob szolgáltatásügyfél tooinstantiate, először rendelkeznie kell érvényes kapcsolati karakterláncot. hello hello blob szolgáltatás kapcsolati karakterláncának formátuma:

Élő szolgáltatások elérésére:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Hello storage emulator eléréséhez:

```php
UseDevelopmentStorage=true
```

toocreate bármely Azure szolgáltatás ügyfele toouse hello kell **ServicesBuilder** osztály. A következőket teheti:

* átadni hello kapcsolati karakterlánc közvetlenül tooit vagy
* hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:
  * Alapértelmezés szerint az tartalmaz egy külső adatforrás - környezeti változók támogatása.
  * Hozzáadhat új források hello kiterjesztésével **ConnectionStringSource** osztály.

Itt vázolt hello példákért hello kapcsolati karakterlánc közvetlenül fog adható át.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Tároló létrehozása
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** objektum lehetővé teszi, hogy hozzon létre egy blob-tároló hello **createContainer** metódust. A tároló létrehozásakor hello tárolóra vonatkozó beállítások adhatók meg, de ez így nincs szükség. (hello az alábbi példa bemutatja, hogyan férhetnek hozzá a tooset hello tároló a szabálygyűjtemény (ACL) és a tároló metaadatait.)

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
// proxys can enumerate blobs within hello container via anonymous
// request, but cannot enumerate containers within hello storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within hello container via
// anonymous request.
// If this value is not specified in hello request, container data is
// private toohello account owner.
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

Hívása **setPublicAccess (PublicAccessType::CONTAINER\_és\_BLOBOK)** teszi hello tároló és a blob adatainak elérése névtelen kérésekkel érhető el. Hívása **setPublicAccess(PublicAccessType::BLOBS_ONLY)** teszi csak blob-adatok elérése névtelen kérésekkel érhető el. Tároló hozzáférés-vezérlési listák kapcsolatos további információkért lásd: [Set tároló hozzáférés-vezérlési lista (REST API-t)][container-acl].

Blob szolgáltatás hibakódokkal kapcsolatban további információkért lásd: [Blob hibakódjai][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
egy fájlt egy blobba, használjon hello tooupload **BlobRestProxy -> createBlockBlob** metódust. Ez a művelet hello blob hoz létre, ha nem létezik, vagy felülírja, ha támogatja. hello az alábbi Kódpélda feltételezi, hogy adott hello tároló már létre lett hozva, és használja [fopen] [ fopen] tooopen hello fájl adatfolyamként.

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

Vegye figyelembe, hogy a fenti példa hello feltölt egy blobot adatfolyamként. Azonban egy blobot is feltölthetők használ, például hello karakterláncból [fájl\_beolvasása\_tartalma] [ file_get_contents] függvény. toodo a hello előző mintát használja, módosítsa `$content = fopen("c:\myfile.txt", "r");` túl`$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello BLOB a tárolóban lévő hello használata **BlobRestProxy -> listBlobs** metódust egy **foreach** hurok tooloop hello eredményén keresztül. hello alábbira neve hello minden egyes blob a tárolóban lévő kimenetként és az URI toohello böngésző jeleníti meg.

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
toodownload blob, hívás hello **BlobRestProxy -> getBlob** módszer, majd hívás hello **getContentStream** hello eredő metódusa **GetBlobResult** objektum.

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

Vegye figyelembe, hogy a fenti hello példa lekérdezi egy blobot egy adatfolyam-erőforrásként (hello alapértelmezés). Azonban használhat hello [adatfolyam\_beolvasása\_tartalma] [ stream-get-contents] függvény tooconvert hello adatfolyam tooa karakterláncot adott vissza.

## <a name="delete-a-blob"></a>Blob törlése
egy blob toodelete átadni hello tároló nevének és a blob neve túl**BlobRestProxy -> deleteBlob**.

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
Végezetül a toodelete egy blob tároló átadni hello tároló neve túl**BlobRestProxy -> deleteContainer**.

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
Most, hogy megismerte az Azure blob szolgáltatás hello hello alapjait, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.

* A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)
* Lásd: hello [PHP blokk blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
* Lásd: hello [PHP lap blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
* [Adatátvitel az AzCopy parancssori segédprogram hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
