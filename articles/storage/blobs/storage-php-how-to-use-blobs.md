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
# <a name="how-to-use-blob-storage-from-php"></a><span data-ttu-id="df722-103">Php-ből a blob storage használata</span><span class="sxs-lookup"><span data-stu-id="df722-103">How to use blob storage from PHP</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="df722-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="df722-104">Overview</span></span>
<span data-ttu-id="df722-105">Az Azure Blob Storage egy olyan szolgáltatás, amely a strukturálatlan adatokat objektumként/blobként tárolja a felhőben.</span><span class="sxs-lookup"><span data-stu-id="df722-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="df722-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="df722-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="df722-107">A Blob Storage más néven objektumtárnak is hívható.</span><span class="sxs-lookup"><span data-stu-id="df722-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="df722-108">Ez az útmutató bemutatja, hogyan hajthat végre a szolgáltatást az Azure blob szolgáltatást használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="df722-108">This guide shows you how to perform common scenarios using the Azure blob service.</span></span> <span data-ttu-id="df722-109">A mintákat a PHP és -felhasználási nyelven íródtak a [Azure SDK for PHP][download].</span><span class="sxs-lookup"><span data-stu-id="df722-109">The samples are written in PHP and use the [Azure SDK for PHP][download].</span></span> <span data-ttu-id="df722-110">Az ismertetett forgatókönyvek **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="df722-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="df722-111">A blobok további információkért lásd: a [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="df722-111">For more information on blobs, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="df722-112">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="df722-112">Create a PHP application</span></span>
<span data-ttu-id="df722-113">Egyetlen követelménye, amely hozzáfér az Azure blob szolgáltatáshoz PHP-alkalmazások létrehozásának, a hivatkozási osztályok az Azure SDK-ban a php a kód.</span><span class="sxs-lookup"><span data-stu-id="df722-113">The only requirement for creating a PHP application that accesses the Azure blob service is the referencing of classes in the Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="df722-114">A fejlesztői eszközök hozhat létre az alkalmazás, például a Jegyzettömbbel.</span><span class="sxs-lookup"><span data-stu-id="df722-114">You can use any development tools to create your application, including Notepad.</span></span>

<span data-ttu-id="df722-115">Ebben az útmutatóban használhatja helyben vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó PHP-alkalmazások belül nevű szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="df722-115">In this guide, you use service features, which can be called within a PHP application locally or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="df722-116">Az Azure Ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="df722-116">Get the Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a><span data-ttu-id="df722-117">Állítsa be az alkalmazását, a blob szolgáltatás eléréséhez</span><span class="sxs-lookup"><span data-stu-id="df722-117">Configure your application to access the blob service</span></span>
<span data-ttu-id="df722-118">Az Azure blob szolgáltatás API-kat használ, meg kell:</span><span class="sxs-lookup"><span data-stu-id="df722-118">To use the Azure blob service APIs, you need to:</span></span>

1. <span data-ttu-id="df722-119">A robotot fájl használatával hivatkozik a [require_once] utasítást, és</span><span class="sxs-lookup"><span data-stu-id="df722-119">Reference the autoloader file using the [require_once] statement, and</span></span>
2. <span data-ttu-id="df722-120">Bármely osztályok segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="df722-120">Reference any classes you might use.</span></span>

<span data-ttu-id="df722-121">A következő példa bemutatja, hogyan a robotot fájl és a hivatkozás a **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="df722-121">The following example shows how to include the autoloader file and reference the **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="df722-122">Ebben a cikkben szereplő példák azt feltételezik, hogy telepítette az Azure-szerkesztő segítségével, a PHP Klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="df722-122">The examples in this article assume you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="df722-123">Ha a könyvtárak manuálisan telepítette, hivatkoznia kell a `WindowsAzure.php` robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="df722-123">If you installed the libraries manually, you need to reference the `WindowsAzure.php` autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="df722-124">Az alábbi példákban a `require_once` utasítás mindig megjelennek, de csak a szükséges végrehajtandó például osztályok hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="df722-124">In the examples below, the `require_once` statement will be shown always, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="df722-125">Az Azure storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="df722-125">Set up an Azure storage connection</span></span>
<span data-ttu-id="df722-126">Egy Azure blob szolgáltatásügyfél példányt létrehozni, először egy érvényes kapcsolati karakterláncot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="df722-126">To instantiate an Azure blob service client, you must first have a valid connection string.</span></span> <span data-ttu-id="df722-127">A blob szolgáltatás kapcsolati karakterláncának formátuma:</span><span class="sxs-lookup"><span data-stu-id="df722-127">The format for the blob service connection string is:</span></span>

<span data-ttu-id="df722-128">Élő szolgáltatások elérésére:</span><span class="sxs-lookup"><span data-stu-id="df722-128">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="df722-129">A storage emulator eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="df722-129">For accessing the storage emulator:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="df722-130">Bármely Azure szolgáltatás ügyfele létrehozásához kell használnia a **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="df722-130">To create any Azure service client, you need to use the **ServicesBuilder** class.</span></span> <span data-ttu-id="df722-131">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="df722-131">You can:</span></span>

* <span data-ttu-id="df722-132">a kapcsolati karakterláncot adja át a közvetlenül vagy</span><span class="sxs-lookup"><span data-stu-id="df722-132">Pass the connection string directly to it or</span></span>
* <span data-ttu-id="df722-133">használja a **CloudConfigurationManager (CCM)** ellenőrizze a kapcsolati karakterlánc több külső forrás:</span><span class="sxs-lookup"><span data-stu-id="df722-133">Use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="df722-134">Alapértelmezés szerint az tartalmaz egy külső adatforrás - környezeti változók támogatása.</span><span class="sxs-lookup"><span data-stu-id="df722-134">By default, it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="df722-135">Új forrásból történő kiterjesztésével adhat hozzá a **ConnectionStringSource** osztály.</span><span class="sxs-lookup"><span data-stu-id="df722-135">You can add new sources by extending the **ConnectionStringSource** class.</span></span>

<span data-ttu-id="df722-136">Az itt leírt példák a kapcsolati karakterlánc közvetlenül fog adható át.</span><span class="sxs-lookup"><span data-stu-id="df722-136">For the examples outlined here, the connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a><span data-ttu-id="df722-137">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="df722-137">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="df722-138">A **BlobRestProxy** objektum lehetővé teszi, hogy a blob tárolókat hozhat létre a **createContainer** metódust.</span><span class="sxs-lookup"><span data-stu-id="df722-138">A **BlobRestProxy** object lets you create a blob container with the **createContainer** method.</span></span> <span data-ttu-id="df722-139">A tároló létrehozásakor a tárolóra vonatkozó beállítások adhatók meg, de ez így nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="df722-139">When creating a container, you can set options on the container, but doing so is not required.</span></span> <span data-ttu-id="df722-140">(Az alábbi példa bemutatja, hogyan állíthatja be a tároló hozzáférés-vezérlési lista (ACL) és a tároló metaadatait.)</span><span class="sxs-lookup"><span data-stu-id="df722-140">(The example below shows how to set the container access control list (ACL) and container metadata.)</span></span>

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

<span data-ttu-id="df722-141">Hívása **setPublicAccess (PublicAccessType::CONTAINER\_és\_BLOBOK)** elérhetővé válnak a tároló és a blob adatainak elérése névtelen kérésekkel.</span><span class="sxs-lookup"><span data-stu-id="df722-141">Calling **setPublicAccess(PublicAccessType::CONTAINER\_AND\_BLOBS)** makes the container and blob data accessible via anonymous requests.</span></span> <span data-ttu-id="df722-142">Hívása **setPublicAccess(PublicAccessType::BLOBS_ONLY)** teszi csak blob-adatok elérése névtelen kérésekkel érhető el.</span><span class="sxs-lookup"><span data-stu-id="df722-142">Calling **setPublicAccess(PublicAccessType::BLOBS_ONLY)** makes only blob data accessible via anonymous requests.</span></span> <span data-ttu-id="df722-143">Tároló hozzáférés-vezérlési listák kapcsolatos további információkért lásd: [Set tároló hozzáférés-vezérlési lista (REST API-t)][container-acl].</span><span class="sxs-lookup"><span data-stu-id="df722-143">For more information about container ACLs, see [Set container ACL (REST API)][container-acl].</span></span>

<span data-ttu-id="df722-144">Blob szolgáltatás hibakódokkal kapcsolatban további információkért lásd: [Blob hibakódjai][error-codes].</span><span class="sxs-lookup"><span data-stu-id="df722-144">For more information about Blob service error codes, see [Blob Service Error Codes][error-codes].</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="df722-145">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="df722-145">Upload a blob into a container</span></span>
<span data-ttu-id="df722-146">Feltölteni a fájlt egy blobba, használja a **BlobRestProxy -> createBlockBlob** metódust.</span><span class="sxs-lookup"><span data-stu-id="df722-146">To upload a file as a blob, use the **BlobRestProxy->createBlockBlob** method.</span></span> <span data-ttu-id="df722-147">Ez a művelet a blob hoz létre, ha nem létezik, vagy felülírja, ha támogatja.</span><span class="sxs-lookup"><span data-stu-id="df722-147">This operation creates the blob if it doesn't exist, or overwrites it if it does.</span></span> <span data-ttu-id="df722-148">Az alábbi Kódpélda feltételezi, hogy a tároló már létre lett hozva, és használja [fopen] [ fopen] adatfolyamként fájl megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="df722-148">The code example below assumes that the container has already been created and uses [fopen][fopen] to open the file as a stream.</span></span>

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

<span data-ttu-id="df722-149">Vegye figyelembe, hogy a fenti példában feltölt egy blobot adatfolyamként.</span><span class="sxs-lookup"><span data-stu-id="df722-149">Note that the previous sample uploads a blob as a stream.</span></span> <span data-ttu-id="df722-150">Azonban egy blob is feltölthetők a karakterlánc használja, mint például a [fájl\_beolvasása\_tartalma] [ file_get_contents] függvény.</span><span class="sxs-lookup"><span data-stu-id="df722-150">However, a blob can also be uploaded as a string using, for example, the [file\_get\_contents][file_get_contents] function.</span></span> <span data-ttu-id="df722-151">Ehhez használja a fenti példában, módosítsa `$content = fopen("c:\myfile.txt", "r");` való `$content = file_get_contents("c:\myfile.txt");`.</span><span class="sxs-lookup"><span data-stu-id="df722-151">To do this using the previous sample, change `$content = fopen("c:\myfile.txt", "r");` to `$content = file_get_contents("c:\myfile.txt");`.</span></span>

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="df722-152">A tárolóban lévő blobok listázása</span><span class="sxs-lookup"><span data-stu-id="df722-152">List the blobs in a container</span></span>
<span data-ttu-id="df722-153">A tárolóban lévő blobok listázásához, használja a **BlobRestProxy -> listBlobs** metódust egy **foreach** hurok a hurok a eredményén keresztül.</span><span class="sxs-lookup"><span data-stu-id="df722-153">To list the blobs in a container, use the **BlobRestProxy->listBlobs** method with a **foreach** loop to loop through the result.</span></span> <span data-ttu-id="df722-154">A következő kódot neve minden egyes blob a tárolóban lévő kimenetként és jeleníti meg az URI a böngészőre.</span><span class="sxs-lookup"><span data-stu-id="df722-154">The following code displays the name of each blob as output in a container and displays its URI to the browser.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="df722-155">Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="df722-155">Download a blob</span></span>
<span data-ttu-id="df722-156">Töltse le a blob, hívja meg a **BlobRestProxy getBlob ->** módszer, majd hívja a **getContentStream** metódust a kapott **GetBlobResult** objektum.</span><span class="sxs-lookup"><span data-stu-id="df722-156">To download a blob, call the **BlobRestProxy->getBlob** method, then call the **getContentStream** method on the resulting **GetBlobResult** object.</span></span>

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

<span data-ttu-id="df722-157">Vegye figyelembe, hogy a fenti példa blob lekérdezi egy adatfolyam-erőforrásként (alapértelmezés).</span><span class="sxs-lookup"><span data-stu-id="df722-157">Note that the example above gets a blob as a stream resource (the default behavior).</span></span> <span data-ttu-id="df722-158">Azonban használhatja a [adatfolyam\_beolvasása\_tartalma] [ stream-get-contents] működnek, mint a visszaadott stream alakítható át karakterlánccá.</span><span class="sxs-lookup"><span data-stu-id="df722-158">However, you can use the [stream\_get\_contents][stream-get-contents] function to convert the returned stream to a string.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="df722-159">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="df722-159">Delete a blob</span></span>
<span data-ttu-id="df722-160">Blobok törléséhez, adja át a tároló és a blob neve **BlobRestProxy -> deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="df722-160">To delete a blob, pass the container name and blob name to **BlobRestProxy->deleteBlob**.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="df722-161">A blob-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="df722-161">Delete a blob container</span></span>
<span data-ttu-id="df722-162">Végül egy blob-tároló törlése, a tároló nevét át **BlobRestProxy -> deleteContainer**.</span><span class="sxs-lookup"><span data-stu-id="df722-162">Finally, to delete a blob container, pass the container name to **BlobRestProxy->deleteContainer**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="df722-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df722-163">Next steps</span></span>
<span data-ttu-id="df722-164">Most, hogy megismerte az Azure blob szolgáltatás alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.</span><span class="sxs-lookup"><span data-stu-id="df722-164">Now that you've learned the basics of the Azure blob service, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="df722-165">Látogasson el a [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="df722-165">Visit the [Azure Storage team blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="df722-166">Tekintse meg a [PHP blokk blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="df722-166">See the [PHP block blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span></span>
* <span data-ttu-id="df722-167">Tekintse meg a [PHP lap blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="df722-167">See the [PHP page blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span></span>
* [<span data-ttu-id="df722-168">Adatátvitel az AzCopy parancssori segédprogrammal</span><span class="sxs-lookup"><span data-stu-id="df722-168">Transfer data with the AzCopy Command-Line Utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

<span data-ttu-id="df722-169">További információ: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="df722-169">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
