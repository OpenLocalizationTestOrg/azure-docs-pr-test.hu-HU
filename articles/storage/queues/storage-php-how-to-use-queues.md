---
title: "php-ből a Queue storage aaaHow toouse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure Queue storage szolgáltatás toocreate és a delete várólisták és a Beszúrás, get, és törli az üzenetet. Minták PHP nyelven íródtak."
documentationcenter: php
services: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7582b208-4851-4489-a74a-bb952569f55b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 8daabcc9b3b4de121a309f21bb3325242cff06f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-php"></a><span data-ttu-id="fbec9-104">Hogyan toouse a Queue storage php-ből</span><span class="sxs-lookup"><span data-stu-id="fbec9-104">How toouse Queue storage from PHP</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="fbec9-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fbec9-105">Overview</span></span>
<span data-ttu-id="fbec9-106">Ez az útmutató bemutatja, hogyan tooperform gyakori helyzetek használatával hello Azure Queue storage szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fbec9-106">This guide will show you how tooperform common scenarios by using hello Azure Queue storage service.</span></span> <span data-ttu-id="fbec9-107">hello minták íródtak keresztül osztályok hello Windows SDK-t a PHP.</span><span class="sxs-lookup"><span data-stu-id="fbec9-107">hello samples are written via classes from hello Windows SDK for PHP.</span></span> <span data-ttu-id="fbec9-108">hello kezelt forgatókönyvek például a Beszúrás, megtekintésekor, lekérését, és törli az üzenetsor-üzeneteket, valamint létrehozása és törlése várólisták.</span><span class="sxs-lookup"><span data-stu-id="fbec9-108">hello covered scenarios include inserting, peeking, getting, and deleting queue messages, as well as creating and deleting queues.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="fbec9-109">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbec9-109">Create a PHP application</span></span>
<span data-ttu-id="fbec9-110">csak a követelmény a PHP-alkalmazások létrehozása, amely hozzáfér az Azure Queue storage hello hello való hivatkozás hello Azure SDK-t az osztályok a php a kód.</span><span class="sxs-lookup"><span data-stu-id="fbec9-110">hello only requirement for creating a PHP application that accesses Azure Queue storage is hello referencing of classes from hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="fbec9-111">A fejlesztői eszközök toocreate használhatja az alkalmazás, például a Jegyzettömbbel.</span><span class="sxs-lookup"><span data-stu-id="fbec9-111">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="fbec9-112">Ez az útmutató a várólista tárolási szolgáltatásokkal, hogy a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó hívható fogja használni.</span><span class="sxs-lookup"><span data-stu-id="fbec9-112">In this guide, you will use Queue storage features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="fbec9-113">Hello Azure Ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="fbec9-113">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="fbec9-114">Az alkalmazás tooaccess várólista-tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fbec9-114">Configure your application tooaccess Queue storage</span></span>
<span data-ttu-id="fbec9-115">API-k toouse hello Azure Queue storage kell:</span><span class="sxs-lookup"><span data-stu-id="fbec9-115">toouse hello APIs for Azure Queue storage, you need to:</span></span>

1. <span data-ttu-id="fbec9-116">Hello robotjába referenciafájlban hello segítségével [require_once] utasítást.</span><span class="sxs-lookup"><span data-stu-id="fbec9-116">Reference hello autoloader file by using hello [require_once] statement.</span></span>
2. <span data-ttu-id="fbec9-117">Bármely olyan osztállyal, amelynek segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="fbec9-117">Reference any classes that you might use.</span></span>

<span data-ttu-id="fbec9-118">hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="fbec9-118">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="fbec9-119">Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette-e az hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával.</span><span class="sxs-lookup"><span data-stu-id="fbec9-119">This example (and other examples in this article) assumes that you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="fbec9-120">Ha manuálisan telepítette a hello szalagtárak, szüksége lesz a tooreference hello `WindowsAzure.php` robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="fbec9-120">If you installed hello libraries manually, you will need tooreference hello `WindowsAzure.php` autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

<span data-ttu-id="fbec9-121">Hello a következő példa a hello `require_once` utasítás mindig megjelennek, de csak a szükséges hello példa tooexecute hello osztályok lesz hivatkozva.</span><span class="sxs-lookup"><span data-stu-id="fbec9-121">In hello examples below, hello `require_once` statement will be shown always, but only hello classes that are necessary for hello example tooexecute will be referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="fbec9-122">Az Azure storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="fbec9-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="fbec9-123">az Azure Queue storage ügyfél tooinstantiate, először rendelkeznie kell érvényes kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="fbec9-123">tooinstantiate an Azure Queue storage client, you must first have a valid connection string.</span></span> <span data-ttu-id="fbec9-124">hello hello várólista szolgáltatás kapcsolati karakterláncának formátuma az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="fbec9-124">hello format for hello queue service connection string is as follows.</span></span>

<span data-ttu-id="fbec9-125">Élő szolgáltatások elérésére:</span><span class="sxs-lookup"><span data-stu-id="fbec9-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="fbec9-126">Hello emulátor tárolóinak eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="fbec9-126">For accessing hello emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="fbec9-127">toocreate bármely Azure szolgáltatás ügyfele toouse hello kell **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="fbec9-127">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="fbec9-128">Hello a következő módszerek bármelyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="fbec9-128">You can use either of hello following techniques:</span></span>

* <span data-ttu-id="fbec9-129">Hello kapcsolatot adjon át közvetlenül tooit karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="fbec9-129">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="fbec9-130">Használjon **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:</span><span class="sxs-lookup"><span data-stu-id="fbec9-130">Use **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="fbec9-131">Alapértelmezés szerint mellékelt egy külső forrás támogatása – környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="fbec9-131">By default, it comes with support for one external source—environmental variables.</span></span>
  * <span data-ttu-id="fbec9-132">Hozzáadhat új források hello kiterjesztésével **ConnectionStringSource** osztály.</span><span class="sxs-lookup"><span data-stu-id="fbec9-132">You can add new sources by extending hello **ConnectionStringSource** class.</span></span>

<span data-ttu-id="fbec9-133">Itt vázolt hello példákért hello kapcsolati karakterlánc közvetlenül fog adható át.</span><span class="sxs-lookup"><span data-stu-id="fbec9-133">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="fbec9-134">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbec9-134">Create a queue</span></span>
<span data-ttu-id="fbec9-135">A **QueueRestProxy** objektum lehetővé teszi, hogy létrehoz egy sort hello segítségével **createQueue** metódust.</span><span class="sxs-lookup"><span data-stu-id="fbec9-135">A **QueueRestProxy** object lets you create a queue by using hello **createQueue** method.</span></span> <span data-ttu-id="fbec9-136">A várólista létrehozásakor hello várólista beállítások adhatók meg, de ez így nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="fbec9-136">When creating a queue, you can set options on hello queue, but doing so is not required.</span></span> <span data-ttu-id="fbec9-137">(az alábbi példa hogyan hello tooset metaadatait a várólistát.)</span><span class="sxs-lookup"><span data-stu-id="fbec9-137">(hello example below shows how tooset metadata on a queue.)</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateQueueOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// OPTIONAL: Set queue metadata.
$createQueueOptions = new CreateQueueOptions();
$createQueueOptions->addMetaData("key1", "value1");
$createQueueOptions->addMetaData("key2", "value2");

try    {
    // Create queue.
    $queueRestProxy->createQueue("myqueue", $createQueueOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> <span data-ttu-id="fbec9-138">A metaadatok kulcsok nagybetűk közötti különbségtételt szabályozó nem támaszkodhat.</span><span class="sxs-lookup"><span data-stu-id="fbec9-138">You should not rely on case sensitivity for metadata keys.</span></span> <span data-ttu-id="fbec9-139">Minden kulcs pedig olvassa a kisbetűs hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fbec9-139">All keys are read from hello service in lowercase.</span></span>
> 
> 

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="fbec9-140">Egy üzenetsor tooa hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbec9-140">Add a message tooa queue</span></span>
<span data-ttu-id="fbec9-141">egy üzenetsor tooa tooadd használja **QueueRestProxy -> createMessage**.</span><span class="sxs-lookup"><span data-stu-id="fbec9-141">tooadd a message tooa queue, use **QueueRestProxy->createMessage**.</span></span> <span data-ttu-id="fbec9-142">hello metódus időt vesz igénybe, hello üzenetsornév, hello üzenet szövege és üzenet beállításokat. (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="fbec9-142">hello method takes hello queue name, hello message text, and message options (which are optional).</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateMessageOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Create message.
    $builder = new ServicesBuilder();
    $queueRestProxy->createMessage("myqueue", "Hello World!");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="peek-at-hello-next-message"></a><span data-ttu-id="fbec9-143">Betekintés a következő köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="fbec9-143">Peek at hello next message</span></span>
<span data-ttu-id="fbec9-144">Is bepillanthat, hogy egy üzenet (vagy üzenetek): a várólista elejére hello nélkül eltávolítása hello meghívásával **QueueRestProxy -> peekMessages**.</span><span class="sxs-lookup"><span data-stu-id="fbec9-144">You can peek at a message (or messages) at hello front of a queue without removing it from hello queue by calling **QueueRestProxy->peekMessages**.</span></span> <span data-ttu-id="fbec9-145">Alapértelmezés szerint hello **peekMessage** metódus egyetlen üzenet ad vissza, de ezt az értéket módosíthatja hello segítségével **PeekMessagesOptions -> setNumberOfMessages** metódust.</span><span class="sxs-lookup"><span data-stu-id="fbec9-145">By default, hello **peekMessage** method returns a single message, but you can change that value by using hello **PeekMessagesOptions->setNumberOfMessages** method.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\PeekMessagesOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// OPTIONAL: Set peek message options.
$message_options = new PeekMessagesOptions();
$message_options->setNumberOfMessages(1); // Default value is 1.

try    {
    $peekMessagesResult = $queueRestProxy->peekMessages("myqueue", $message_options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$messages = $peekMessagesResult->getQueueMessages();

// View messages.
$messageCount = count($messages);
if($messageCount <= 0){
    echo "There are no messages.<br />";
}
else{
    foreach($messages as $message)    {
        echo "Peeked message:<br />";
        echo "Message Id: ".$message->getMessageId()."<br />";
        echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
        echo "Message text: ".$message->getMessageText()."<br /><br />";
    }
}
```

## <a name="de-queue-hello-next-message"></a><span data-ttu-id="fbec9-146">Hello következő üzenet kivétele az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="fbec9-146">De-queue hello next message</span></span>
<span data-ttu-id="fbec9-147">A kód eltávolítja az üzenetet az üzenetsorból két lépést.</span><span class="sxs-lookup"><span data-stu-id="fbec9-147">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="fbec9-148">Először hívja **QueueRestProxy -> listMessages**, amely teszi hello üzenet láthatatlanná tooany egyéb kód, amely éppen olvas hello üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="fbec9-148">First, you call **QueueRestProxy->listMessages**, which makes hello message invisible tooany other code that's reading from hello queue.</span></span> <span data-ttu-id="fbec9-149">Alapértelmezés szerint ez az üzenet marad láthatatlan 30 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="fbec9-149">By default, this message will stay invisible for 30 seconds.</span></span> <span data-ttu-id="fbec9-150">(Ha üdvözlőüzenetére ebben az időszakban nem törlődik, azt újra láthatóvá válnak hello várólistában.) toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell hívnia **QueueRestProxy -> deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="fbec9-150">(If hello message is not deleted in this time period, it will become visible on hello queue again.) toofinish removing hello message from hello queue, you must call **QueueRestProxy->deleteMessage**.</span></span> <span data-ttu-id="fbec9-151">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy amikor a kód sikertelen tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kaphat hello ugyanazon üzenet, majd próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="fbec9-151">This two-step process of removing a message assures that when your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="fbec9-152">A kód hívások **deleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="fbec9-152">Your code calls **deleteMessage** right after hello message has been processed.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueRestProxy->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

/* ---------------------
    Process message.
   --------------------- */

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Delete message.
    $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="change-hello-contents-of-a-queued-message"></a><span data-ttu-id="fbec9-153">Hello aszinkron üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="fbec9-153">Change hello contents of a queued message</span></span>
<span data-ttu-id="fbec9-154">Módosíthatja egy üzenet helyben hello várólista hello tartalmát meghívásával **QueueRestProxy -> updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="fbec9-154">You can change hello contents of a message in-place in hello queue by calling **QueueRestProxy->updateMessage**.</span></span> <span data-ttu-id="fbec9-155">Üdvözlőüzenetére jelöl, használhatja a munkafeladat hello szolgáltatás tooupdate hello állapotának.</span><span class="sxs-lookup"><span data-stu-id="fbec9-155">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="fbec9-156">a következő kód hello hello üzenetsor frissíti az új tartalommal, és további 60 másodperccel beállítja hello látható időtúllépés tooextend.</span><span class="sxs-lookup"><span data-stu-id="fbec9-156">hello following code updates hello queue message with new contents, and it sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="fbec9-157">Ez menti üdvözlőüzenetére társított feladat hello állapotát, és hello ügyfél üdvözlőüzenetére dolgozik egy másik perces toocontinue biztosít.</span><span class="sxs-lookup"><span data-stu-id="fbec9-157">This saves hello state of work that's associated with hello message, and it gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="fbec9-158">Ez a módszer tootrack többlépéses munkafolyamatokat üzenetsor üzenetein anélkül, hogy toostart keresztül hello kezdődően, ha a folyamat valamelyik lépése toohardware-vagy szoftverhiba miatt nem használható.</span><span class="sxs-lookup"><span data-stu-id="fbec9-158">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="fbec9-159">Általában tartja az újrapróbálkozások számát is, és ha hello üzenetet a rendszer ismét megkísérli több mint  *n*  időpontokat, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="fbec9-159">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="fbec9-160">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="fbec9-160">This protects against a message that triggers an application error each time it is processed.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueRestProxy->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

// Define new message properties.
$new_message_text = "New message text.";
$new_visibility_timeout = 5; // Measured in seconds.

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Update message.
    $queueRestProxy->updateMessage("myqueue",
                                $messageId,
                                $popReceipt,
                                $new_message_text,
                                $new_visibility_timeout);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="additional-options-for-de-queuing-messages"></a><span data-ttu-id="fbec9-161">Az üzenetek üzenetsorból további beállítások</span><span class="sxs-lookup"><span data-stu-id="fbec9-161">Additional options for de-queuing messages</span></span>
<span data-ttu-id="fbec9-162">Két módon, hogy testre szabhatja az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="fbec9-162">There are two ways that you can customize message retrieval from a queue.</span></span> <span data-ttu-id="fbec9-163">Először is kaphat az üzenetkötegek (felfelé too32).</span><span class="sxs-lookup"><span data-stu-id="fbec9-163">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="fbec9-164">Második beállíthat egy hosszabb vagy rövidebb láthatósági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="fbec9-164">Second, you can set a longer or shorter visibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="fbec9-165">hello alábbi példakód hello **getMessages** metódus tooget 16 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="fbec9-165">hello following code example uses hello **getMessages** method tooget 16 messages in one call.</span></span> <span data-ttu-id="fbec9-166">Ezután minden üzenetet feldolgozza a használatával egy **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="fbec9-166">Then it processes each message by using a **for** loop.</span></span> <span data-ttu-id="fbec9-167">Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is.</span><span class="sxs-lookup"><span data-stu-id="fbec9-167">It also sets hello invisibility timeout toofive minutes for each message.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\ListMessagesOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Set list message options.
$message_options = new ListMessagesOptions();
$message_options->setVisibilityTimeoutInSeconds(300);
$message_options->setNumberOfMessages(16);

// Get messages.
try{
    $listMessagesResult = $queueRestProxy->listMessages("myqueue",
                                                     $message_options);
    $messages = $listMessagesResult->getQueueMessages();

    foreach($messages as $message){

        /* ---------------------
            Process message.
        --------------------- */

        // Get message Id and pop receipt.
        $messageId = $message->getMessageId();
        $popReceipt = $message->getPopReceipt();

        // Delete message.
        $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="get-queue-length"></a><span data-ttu-id="fbec9-168">Get-várólista hossza</span><span class="sxs-lookup"><span data-stu-id="fbec9-168">Get queue length</span></span>
<span data-ttu-id="fbec9-169">A várólistában lévő üzenetek hello számának becslése kérheti le.</span><span class="sxs-lookup"><span data-stu-id="fbec9-169">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="fbec9-170">Hello **QueueRestProxy -> getQueueMetadata** metódus hello várólista szolgáltatás tooreturn metaadatai tesz kapcsolatban hello várólista.</span><span class="sxs-lookup"><span data-stu-id="fbec9-170">hello **QueueRestProxy->getQueueMetadata** method asks hello queue service tooreturn metadata about hello queue.</span></span> <span data-ttu-id="fbec9-171">Hívása hello **getApproximateMessageCount** hello metódus visszaadott objektum egy adott időszeletben üzenetek számának vannak a várólistán.</span><span class="sxs-lookup"><span data-stu-id="fbec9-171">Calling hello **getApproximateMessageCount** method on hello returned object provides a count of how many messages are in a queue.</span></span> <span data-ttu-id="fbec9-172">hello száma nem csak hozzávetőleges, mert az üzenetek hozzáadására vagy törlődik, miután hello queue szolgáltatás válaszol-e tooyour kérelmet.</span><span class="sxs-lookup"><span data-stu-id="fbec9-172">hello count is only approximate because messages can be added or removed after hello queue service responds tooyour request.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Get queue metadata.
    $queue_metadata = $queueRestProxy->getQueueMetadata("myqueue");
    $approx_msg_count = $queue_metadata->getApproximateMessageCount();
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

echo $approx_msg_count;
```

## <a name="delete-a-queue"></a><span data-ttu-id="fbec9-173">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="fbec9-173">Delete a queue</span></span>
<span data-ttu-id="fbec9-174">toodelete egy üzenetsor és az összes hello üzenetek, hívja hello **QueueRestProxy -> deleteQueue** metódust.</span><span class="sxs-lookup"><span data-stu-id="fbec9-174">toodelete a queue and all hello messages in it, call hello **QueueRestProxy->deleteQueue** method.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Delete queue.
    $queueRestProxy->deleteQueue("myqueue");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a><span data-ttu-id="fbec9-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbec9-175">Next steps</span></span>
<span data-ttu-id="fbec9-176">Most, hogy megismerte hello Azure Queue storage alapjait, hajtsa végre az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn:</span><span class="sxs-lookup"><span data-stu-id="fbec9-176">Now that you've learned hello basics of Azure Queue storage, follow these links toolearn about more complex storage tasks:</span></span>

* <span data-ttu-id="fbec9-177">A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="fbec9-177">Visit hello [Azure Storage Team blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>

<span data-ttu-id="fbec9-178">További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="fbec9-178">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

