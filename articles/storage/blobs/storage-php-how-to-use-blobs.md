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
# <a name="how-toouse-blob-storage-from-php"></a><span data-ttu-id="a911c-103">Hogyan toouse blob-tároló php-ből</span><span class="sxs-lookup"><span data-stu-id="a911c-103">How toouse blob storage from PHP</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="a911c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a911c-104">Overview</span></span>
<span data-ttu-id="a911c-105">Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="a911c-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="a911c-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="a911c-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="a911c-107">A BLOB storage is az említett tooas objektum tároló.</span><span class="sxs-lookup"><span data-stu-id="a911c-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="a911c-108">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure blob szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a911c-108">This guide shows you how tooperform common scenarios using hello Azure blob service.</span></span> <span data-ttu-id="a911c-109">hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP][download].</span><span class="sxs-lookup"><span data-stu-id="a911c-109">hello samples are written in PHP and use hello [Azure SDK for PHP][download].</span></span> <span data-ttu-id="a911c-110">hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="a911c-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="a911c-111">A blobok további információkért lásd: hello [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="a911c-111">For more information on blobs, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="a911c-112">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a911c-112">Create a PHP application</span></span>
<span data-ttu-id="a911c-113">egyetlen, aki hozzáfér hello Azure blob szolgáltatáshoz PHP-alkalmazások létrehozására vonatkozó követelmény az hello hello való hivatkozás hello Azure SDK-t az osztályokat a php a kód.</span><span class="sxs-lookup"><span data-stu-id="a911c-113">hello only requirement for creating a PHP application that accesses hello Azure blob service is hello referencing of classes in hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="a911c-114">A fejlesztői eszközök toocreate használhatja az alkalmazás, például a Jegyzettömbbel.</span><span class="sxs-lookup"><span data-stu-id="a911c-114">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="a911c-115">Ebben az útmutatóban használhatja helyben vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó PHP-alkalmazások belül nevű szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="a911c-115">In this guide, you use service features, which can be called within a PHP application locally or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="a911c-116">Hello Azure Ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="a911c-116">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-blob-service"></a><span data-ttu-id="a911c-117">Az alkalmazás tooaccess hello blob szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a911c-117">Configure your application tooaccess hello blob service</span></span>
<span data-ttu-id="a911c-118">toouse hello Azure blob szolgáltatás API-k, kell:</span><span class="sxs-lookup"><span data-stu-id="a911c-118">toouse hello Azure blob service APIs, you need to:</span></span>

1. <span data-ttu-id="a911c-119">Hello robotjába referenciafájlban hello segítségével [require_once] utasítást, és</span><span class="sxs-lookup"><span data-stu-id="a911c-119">Reference hello autoloader file using hello [require_once] statement, and</span></span>
2. <span data-ttu-id="a911c-120">Bármely osztályok segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="a911c-120">Reference any classes you might use.</span></span>

<span data-ttu-id="a911c-121">hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="a911c-121">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="a911c-122">Ebben a cikkben a hello példák feltételezik, hogy telepítette hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával.</span><span class="sxs-lookup"><span data-stu-id="a911c-122">hello examples in this article assume you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="a911c-123">Ha manuálisan telepítette a hello szalagtárak, meg kell-e tooreference hello `WindowsAzure.php` robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="a911c-123">If you installed hello libraries manually, you need tooreference hello `WindowsAzure.php` autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="a911c-124">Hello a következő példa a hello `require_once` utasítás mindig megjelennek, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="a911c-124">In hello examples below, hello `require_once` statement will be shown always, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="a911c-125">Az Azure storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="a911c-125">Set up an Azure storage connection</span></span>
<span data-ttu-id="a911c-126">egy Azure blob szolgáltatásügyfél tooinstantiate, először rendelkeznie kell érvényes kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a911c-126">tooinstantiate an Azure blob service client, you must first have a valid connection string.</span></span> <span data-ttu-id="a911c-127">hello hello blob szolgáltatás kapcsolati karakterláncának formátuma:</span><span class="sxs-lookup"><span data-stu-id="a911c-127">hello format for hello blob service connection string is:</span></span>

<span data-ttu-id="a911c-128">Élő szolgáltatások elérésére:</span><span class="sxs-lookup"><span data-stu-id="a911c-128">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="a911c-129">Hello storage emulator eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="a911c-129">For accessing hello storage emulator:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="a911c-130">toocreate bármely Azure szolgáltatás ügyfele toouse hello kell **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="a911c-130">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="a911c-131">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="a911c-131">You can:</span></span>

* <span data-ttu-id="a911c-132">átadni hello kapcsolati karakterlánc közvetlenül tooit vagy</span><span class="sxs-lookup"><span data-stu-id="a911c-132">Pass hello connection string directly tooit or</span></span>
* <span data-ttu-id="a911c-133">hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:</span><span class="sxs-lookup"><span data-stu-id="a911c-133">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="a911c-134">Alapértelmezés szerint az tartalmaz egy külső adatforrás - környezeti változók támogatása.</span><span class="sxs-lookup"><span data-stu-id="a911c-134">By default, it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="a911c-135">Hozzáadhat új források hello kiterjesztésével **ConnectionStringSource** osztály.</span><span class="sxs-lookup"><span data-stu-id="a911c-135">You can add new sources by extending hello **ConnectionStringSource** class.</span></span>

<span data-ttu-id="a911c-136">Itt vázolt hello példákért hello kapcsolati karakterlánc közvetlenül fog adható át.</span><span class="sxs-lookup"><span data-stu-id="a911c-136">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a><span data-ttu-id="a911c-137">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="a911c-137">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="a911c-138">A **BlobRestProxy** objektum lehetővé teszi, hogy hozzon létre egy blob-tároló hello **createContainer** metódust.</span><span class="sxs-lookup"><span data-stu-id="a911c-138">A **BlobRestProxy** object lets you create a blob container with hello **createContainer** method.</span></span> <span data-ttu-id="a911c-139">A tároló létrehozásakor hello tárolóra vonatkozó beállítások adhatók meg, de ez így nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="a911c-139">When creating a container, you can set options on hello container, but doing so is not required.</span></span> <span data-ttu-id="a911c-140">(hello az alábbi példa bemutatja, hogyan férhetnek hozzá a tooset hello tároló a szabálygyűjtemény (ACL) és a tároló metaadatait.)</span><span class="sxs-lookup"><span data-stu-id="a911c-140">(hello example below shows how tooset hello container access control list (ACL) and container metadata.)</span></span>

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

<span data-ttu-id="a911c-141">Hívása **setPublicAccess (PublicAccessType::CONTAINER\_és\_BLOBOK)** teszi hello tároló és a blob adatainak elérése névtelen kérésekkel érhető el.</span><span class="sxs-lookup"><span data-stu-id="a911c-141">Calling **setPublicAccess(PublicAccessType::CONTAINER\_AND\_BLOBS)** makes hello container and blob data accessible via anonymous requests.</span></span> <span data-ttu-id="a911c-142">Hívása **setPublicAccess(PublicAccessType::BLOBS_ONLY)** teszi csak blob-adatok elérése névtelen kérésekkel érhető el.</span><span class="sxs-lookup"><span data-stu-id="a911c-142">Calling **setPublicAccess(PublicAccessType::BLOBS_ONLY)** makes only blob data accessible via anonymous requests.</span></span> <span data-ttu-id="a911c-143">Tároló hozzáférés-vezérlési listák kapcsolatos további információkért lásd: [Set tároló hozzáférés-vezérlési lista (REST API-t)][container-acl].</span><span class="sxs-lookup"><span data-stu-id="a911c-143">For more information about container ACLs, see [Set container ACL (REST API)][container-acl].</span></span>

<span data-ttu-id="a911c-144">Blob szolgáltatás hibakódokkal kapcsolatban további információkért lásd: [Blob hibakódjai][error-codes].</span><span class="sxs-lookup"><span data-stu-id="a911c-144">For more information about Blob service error codes, see [Blob Service Error Codes][error-codes].</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="a911c-145">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="a911c-145">Upload a blob into a container</span></span>
<span data-ttu-id="a911c-146">egy fájlt egy blobba, használjon hello tooupload **BlobRestProxy -> createBlockBlob** metódust.</span><span class="sxs-lookup"><span data-stu-id="a911c-146">tooupload a file as a blob, use hello **BlobRestProxy->createBlockBlob** method.</span></span> <span data-ttu-id="a911c-147">Ez a művelet hello blob hoz létre, ha nem létezik, vagy felülírja, ha támogatja.</span><span class="sxs-lookup"><span data-stu-id="a911c-147">This operation creates hello blob if it doesn't exist, or overwrites it if it does.</span></span> <span data-ttu-id="a911c-148">hello az alábbi Kódpélda feltételezi, hogy adott hello tároló már létre lett hozva, és használja [fopen] [ fopen] tooopen hello fájl adatfolyamként.</span><span class="sxs-lookup"><span data-stu-id="a911c-148">hello code example below assumes that hello container has already been created and uses [fopen][fopen] tooopen hello file as a stream.</span></span>

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

<span data-ttu-id="a911c-149">Vegye figyelembe, hogy a fenti példa hello feltölt egy blobot adatfolyamként.</span><span class="sxs-lookup"><span data-stu-id="a911c-149">Note that hello previous sample uploads a blob as a stream.</span></span> <span data-ttu-id="a911c-150">Azonban egy blobot is feltölthetők használ, például hello karakterláncból [fájl\_beolvasása\_tartalma] [ file_get_contents] függvény.</span><span class="sxs-lookup"><span data-stu-id="a911c-150">However, a blob can also be uploaded as a string using, for example, hello [file\_get\_contents][file_get_contents] function.</span></span> <span data-ttu-id="a911c-151">toodo a hello előző mintát használja, módosítsa `$content = fopen("c:\myfile.txt", "r");` túl`$content = file_get_contents("c:\myfile.txt");`.</span><span class="sxs-lookup"><span data-stu-id="a911c-151">toodo this using hello previous sample, change `$content = fopen("c:\myfile.txt", "r");` too`$content = file_get_contents("c:\myfile.txt");`.</span></span>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="a911c-152">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="a911c-152">List hello blobs in a container</span></span>
<span data-ttu-id="a911c-153">toolist hello BLOB a tárolóban lévő hello használata **BlobRestProxy -> listBlobs** metódust egy **foreach** hurok tooloop hello eredményén keresztül.</span><span class="sxs-lookup"><span data-stu-id="a911c-153">toolist hello blobs in a container, use hello **BlobRestProxy->listBlobs** method with a **foreach** loop tooloop through hello result.</span></span> <span data-ttu-id="a911c-154">hello alábbira neve hello minden egyes blob a tárolóban lévő kimenetként és az URI toohello böngésző jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a911c-154">hello following code displays hello name of each blob as output in a container and displays its URI toohello browser.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="a911c-155">Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="a911c-155">Download a blob</span></span>
<span data-ttu-id="a911c-156">toodownload blob, hívás hello **BlobRestProxy -> getBlob** módszer, majd hívás hello **getContentStream** hello eredő metódusa **GetBlobResult** objektum.</span><span class="sxs-lookup"><span data-stu-id="a911c-156">toodownload a blob, call hello **BlobRestProxy->getBlob** method, then call hello **getContentStream** method on hello resulting **GetBlobResult** object.</span></span>

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

<span data-ttu-id="a911c-157">Vegye figyelembe, hogy a fenti hello példa lekérdezi egy blobot egy adatfolyam-erőforrásként (hello alapértelmezés).</span><span class="sxs-lookup"><span data-stu-id="a911c-157">Note that hello example above gets a blob as a stream resource (hello default behavior).</span></span> <span data-ttu-id="a911c-158">Azonban használhat hello [adatfolyam\_beolvasása\_tartalma] [ stream-get-contents] függvény tooconvert hello adatfolyam tooa karakterláncot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="a911c-158">However, you can use hello [stream\_get\_contents][stream-get-contents] function tooconvert hello returned stream tooa string.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="a911c-159">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="a911c-159">Delete a blob</span></span>
<span data-ttu-id="a911c-160">egy blob toodelete átadni hello tároló nevének és a blob neve túl**BlobRestProxy -> deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="a911c-160">toodelete a blob, pass hello container name and blob name too**BlobRestProxy->deleteBlob**.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="a911c-161">A blob-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="a911c-161">Delete a blob container</span></span>
<span data-ttu-id="a911c-162">Végezetül a toodelete egy blob tároló átadni hello tároló neve túl**BlobRestProxy -> deleteContainer**.</span><span class="sxs-lookup"><span data-stu-id="a911c-162">Finally, toodelete a blob container, pass hello container name too**BlobRestProxy->deleteContainer**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a911c-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a911c-163">Next steps</span></span>
<span data-ttu-id="a911c-164">Most, hogy megismerte az Azure blob szolgáltatás hello hello alapjait, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="a911c-164">Now that you've learned hello basics of hello Azure blob service, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="a911c-165">A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="a911c-165">Visit hello [Azure Storage team blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="a911c-166">Lásd: hello [PHP blokk blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="a911c-166">See hello [PHP block blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span></span>
* <span data-ttu-id="a911c-167">Lásd: hello [PHP lap blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="a911c-167">See hello [PHP page blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span></span>
* [<span data-ttu-id="a911c-168">Adatátvitel az AzCopy parancssori segédprogram hello</span><span class="sxs-lookup"><span data-stu-id="a911c-168">Transfer data with hello AzCopy Command-Line Utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

<span data-ttu-id="a911c-169">További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="a911c-169">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
