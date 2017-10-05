---
title: "Php-ből a Queue storage használata |} Microsoft Docs"
description: "Útmutató: az Azure Queue storage szolgáltatás segítségével hozza létre, és törli az üzenetsorok, és helyezze, get, és törli az üzenetet. Minták PHP nyelven íródtak."
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
ms.openlocfilehash: 3c8f799a917cfc9d74412d90f27f2ea8c21265d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-queue-storage-from-php"></a><span data-ttu-id="5abe5-104">How to use Queue storage from PHP (A Queue Storage használata PHP-val)</span><span class="sxs-lookup"><span data-stu-id="5abe5-104">How to use Queue storage from PHP</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="5abe5-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5abe5-105">Overview</span></span>
<span data-ttu-id="5abe5-106">Ez az útmutató bemutatja, hogyan hajthat végre a gyakori forgatókönyvek az Azure Queue storage szolgáltatás használatával.</span><span class="sxs-lookup"><span data-stu-id="5abe5-106">This guide will show you how to perform common scenarios by using the Azure Queue storage service.</span></span> <span data-ttu-id="5abe5-107">A minták íródtak osztályok keresztül a Windows SDK-ból a PHP.</span><span class="sxs-lookup"><span data-stu-id="5abe5-107">The samples are written via classes from the Windows SDK for PHP.</span></span> <span data-ttu-id="5abe5-108">A kezelt forgatókönyvek például a Beszúrás, megtekintésekor, lekérését, és törli az üzenetsor-üzeneteket, valamint létrehozása és törlése várólisták.</span><span class="sxs-lookup"><span data-stu-id="5abe5-108">The covered scenarios include inserting, peeking, getting, and deleting queue messages, as well as creating and deleting queues.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="5abe5-109">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5abe5-109">Create a PHP application</span></span>
<span data-ttu-id="5abe5-110">A PHP-alkalmazások Azure Queue storage hozzáférő létrehozása egyetlen követelménye, a hivatkozási osztályok az Azure SDK-ból a php a kód.</span><span class="sxs-lookup"><span data-stu-id="5abe5-110">The only requirement for creating a PHP application that accesses Azure Queue storage is the referencing of classes from the Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="5abe5-111">A fejlesztői eszközök hozhat létre az alkalmazás, például a Jegyzettömbbel.</span><span class="sxs-lookup"><span data-stu-id="5abe5-111">You can use any development tools to create your application, including Notepad.</span></span>

<span data-ttu-id="5abe5-112">Ez az útmutató a várólista tárolási szolgáltatásokkal, hogy a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó hívható fogja használni.</span><span class="sxs-lookup"><span data-stu-id="5abe5-112">In this guide, you will use Queue storage features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="5abe5-113">Az Azure Ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="5abe5-113">Get the Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-queue-storage"></a><span data-ttu-id="5abe5-114">Állítsa be az alkalmazását, a Queue storage eléréséhez</span><span class="sxs-lookup"><span data-stu-id="5abe5-114">Configure your application to access Queue storage</span></span>
<span data-ttu-id="5abe5-115">Az API-kat az Azure Queue storage használandó kell:</span><span class="sxs-lookup"><span data-stu-id="5abe5-115">To use the APIs for Azure Queue storage, you need to:</span></span>

1. <span data-ttu-id="5abe5-116">Hivatkozzon a robotot használatával a [require_once] utasítást.</span><span class="sxs-lookup"><span data-stu-id="5abe5-116">Reference the autoloader file by using the [require_once] statement.</span></span>
2. <span data-ttu-id="5abe5-117">Bármely olyan osztállyal, amelynek segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="5abe5-117">Reference any classes that you might use.</span></span>

<span data-ttu-id="5abe5-118">A következő példa bemutatja, hogyan a robotot fájl és a hivatkozás a **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="5abe5-118">The following example shows how to include the autoloader file and reference the **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="5abe5-119">Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette az Azure-szerkesztő segítségével, a PHP Klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="5abe5-119">This example (and other examples in this article) assumes that you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="5abe5-120">Ha a könyvtárak manuálisan telepítette, szüksége lesz való hivatkozáshoz a `WindowsAzure.php` robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="5abe5-120">If you installed the libraries manually, you will need to reference the `WindowsAzure.php` autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

<span data-ttu-id="5abe5-121">Az alábbi példákban a `require_once` utasítás mindig megjelennek, de csak a szükséges végrehajtandó például osztályok lesz hivatkozva.</span><span class="sxs-lookup"><span data-stu-id="5abe5-121">In the examples below, the `require_once` statement will be shown always, but only the classes that are necessary for the example to execute will be referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="5abe5-122">Az Azure storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="5abe5-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="5abe5-123">Az Azure Queue storage ügyfél példányosítható, először egy érvényes kapcsolati karakterláncot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="5abe5-123">To instantiate an Azure Queue storage client, you must first have a valid connection string.</span></span> <span data-ttu-id="5abe5-124">A várólista-szolgáltatás kapcsolati karakterláncának formátuma a következő.</span><span class="sxs-lookup"><span data-stu-id="5abe5-124">The format for the queue service connection string is as follows.</span></span>

<span data-ttu-id="5abe5-125">Élő szolgáltatások elérésére:</span><span class="sxs-lookup"><span data-stu-id="5abe5-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="5abe5-126">Az emulátor tárolóinak eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="5abe5-126">For accessing the emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="5abe5-127">Bármely Azure szolgáltatás ügyfele létrehozásához kell használnia a **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="5abe5-127">To create any Azure service client, you need to use the **ServicesBuilder** class.</span></span> <span data-ttu-id="5abe5-128">Az alábbi módszerek bármelyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="5abe5-128">You can use either of the following techniques:</span></span>

* <span data-ttu-id="5abe5-129">Átadni a kapcsolati karakterlánc közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="5abe5-129">Pass the connection string directly to it.</span></span>
* <span data-ttu-id="5abe5-130">Használjon **CloudConfigurationManager (CCM)** ellenőrizze a kapcsolati karakterlánc több külső forrás:</span><span class="sxs-lookup"><span data-stu-id="5abe5-130">Use **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="5abe5-131">Alapértelmezés szerint mellékelt egy külső forrás támogatása – környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="5abe5-131">By default, it comes with support for one external source—environmental variables.</span></span>
  * <span data-ttu-id="5abe5-132">Új forrásból történő kiterjesztésével adhat hozzá a **ConnectionStringSource** osztály.</span><span class="sxs-lookup"><span data-stu-id="5abe5-132">You can add new sources by extending the **ConnectionStringSource** class.</span></span>

<span data-ttu-id="5abe5-133">Az itt leírt példák a kapcsolati karakterlánc közvetlenül fog adható át.</span><span class="sxs-lookup"><span data-stu-id="5abe5-133">For the examples outlined here, the connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="5abe5-134">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="5abe5-134">Create a queue</span></span>
<span data-ttu-id="5abe5-135">A **QueueRestProxy** objektum lehetővé teszi, hogy a várólista létrehozása a **createQueue** metódust.</span><span class="sxs-lookup"><span data-stu-id="5abe5-135">A **QueueRestProxy** object lets you create a queue by using the **createQueue** method.</span></span> <span data-ttu-id="5abe5-136">A várólista létrehozásakor az a várakozási beállítások adhatók meg, de ez így nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="5abe5-136">When creating a queue, you can set options on the queue, but doing so is not required.</span></span> <span data-ttu-id="5abe5-137">(Az alábbi példa bemutatja, hogyan metaadatok beállítani a várólista.)</span><span class="sxs-lookup"><span data-stu-id="5abe5-137">(The example below shows how to set metadata on a queue.)</span></span>

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
> <span data-ttu-id="5abe5-138">A metaadatok kulcsok nagybetűk közötti különbségtételt szabályozó nem támaszkodhat.</span><span class="sxs-lookup"><span data-stu-id="5abe5-138">You should not rely on case sensitivity for metadata keys.</span></span> <span data-ttu-id="5abe5-139">Minden kulcs olvasható kisbetűs a szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="5abe5-139">All keys are read from the service in lowercase.</span></span>
> 
> 

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="5abe5-140">A várólista üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5abe5-140">Add a message to a queue</span></span>
<span data-ttu-id="5abe5-141">A várólista adhat hozzá egy üzenetet, **QueueRestProxy -> createMessage**.</span><span class="sxs-lookup"><span data-stu-id="5abe5-141">To add a message to a queue, use **QueueRestProxy->createMessage**.</span></span> <span data-ttu-id="5abe5-142">A módszer veszi a várólista nevét, a szöveges üzenet és az üzenet beállításokat. (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="5abe5-142">The method takes the queue name, the message text, and message options (which are optional).</span></span>

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

## <a name="peek-at-the-next-message"></a><span data-ttu-id="5abe5-143">Betekintés a következő üzenetbe</span><span class="sxs-lookup"><span data-stu-id="5abe5-143">Peek at the next message</span></span>
<span data-ttu-id="5abe5-144">Is bepillanthat, hogy egy üzenet (vagy üzenetek) egy sor elején anélkül, hogy eltávolítaná az üzenetsorból meghívásával **QueueRestProxy -> peekMessages**.</span><span class="sxs-lookup"><span data-stu-id="5abe5-144">You can peek at a message (or messages) at the front of a queue without removing it from the queue by calling **QueueRestProxy->peekMessages**.</span></span> <span data-ttu-id="5abe5-145">Alapértelmezés szerint a **peekMessage** metódus egyetlen üzenet ad vissza, de ezt az értéket használatával módosíthatja a **PeekMessagesOptions -> setNumberOfMessages** metódust.</span><span class="sxs-lookup"><span data-stu-id="5abe5-145">By default, the **peekMessage** method returns a single message, but you can change that value by using the **PeekMessagesOptions->setNumberOfMessages** method.</span></span>

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

## <a name="de-queue-the-next-message"></a><span data-ttu-id="5abe5-146">A következő üzenet kivétele az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="5abe5-146">De-queue the next message</span></span>
<span data-ttu-id="5abe5-147">A kód eltávolítja az üzenetet az üzenetsorból két lépést.</span><span class="sxs-lookup"><span data-stu-id="5abe5-147">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="5abe5-148">Először hívja **QueueRestProxy -> listMessages**, ami lehetővé teszi az üzenetet az üzenetsorból olvasó többi kód nem látható.</span><span class="sxs-lookup"><span data-stu-id="5abe5-148">First, you call **QueueRestProxy->listMessages**, which makes the message invisible to any other code that's reading from the queue.</span></span> <span data-ttu-id="5abe5-149">Alapértelmezés szerint ez az üzenet marad láthatatlan 30 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="5abe5-149">By default, this message will stay invisible for 30 seconds.</span></span> <span data-ttu-id="5abe5-150">(Ha az üzenet nem törlődik az ebben az időszakban, azt újra láthatóvá válnak a várólistán.) Szeretné távolítani az üzenetet az üzenetsorból, meg kell hívnia **QueueRestProxy -> deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="5abe5-150">(If the message is not deleted in this time period, it will become visible on the queue again.) To finish removing the message from the queue, you must call **QueueRestProxy->deleteMessage**.</span></span> <span data-ttu-id="5abe5-151">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód nem tudja feldolgozni egy üzenetet, hardver- vagy szoftverhiba miatt, a kód egy másik példánya is megkaphassa ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="5abe5-151">This two-step process of removing a message assures that when your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="5abe5-152">A kód hívások **deleteMessage** jobb gombbal az üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="5abe5-152">Your code calls **deleteMessage** right after the message has been processed.</span></span>

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

## <a name="change-the-contents-of-a-queued-message"></a><span data-ttu-id="5abe5-153">Üzenetsorban található üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="5abe5-153">Change the contents of a queued message</span></span>
<span data-ttu-id="5abe5-154">Módosíthatja a tartalmát egy üzenet helyben a várólistán lévő meghívásával **QueueRestProxy -> updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="5abe5-154">You can change the contents of a message in-place in the queue by calling **QueueRestProxy->updateMessage**.</span></span> <span data-ttu-id="5abe5-155">Ha az üzenet munkafeladatot jelöl, ezzel a funkcióval frissítheti a munkafeladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="5abe5-155">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="5abe5-156">Az alábbi kód frissíti az üzenetsorban található üzenetet az új tartalommal, és meghatározza a láthatósági időkorlátot további 60 másodperccel bővíti.</span><span class="sxs-lookup"><span data-stu-id="5abe5-156">The following code updates the queue message with new contents, and it sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="5abe5-157">Ez az üzenet társított feladat állapotát menti, és az ügyfél folytathatja a munkát az üzenetben további egy percet biztosít.</span><span class="sxs-lookup"><span data-stu-id="5abe5-157">This saves the state of work that's associated with the message, and it gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="5abe5-158">Ezzel a technikával többlépéses munkafolyamatokat is nyomon követhet az üzenetsor üzenetein anélkül, hogy újra kéne kezdenie, ha a folyamat valamelyik lépése hardver- vagy szoftverhiba miatt meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="5abe5-158">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="5abe5-159">A rendszer általában nyilvántartja az újrapróbálkozások számát, és ha az üzenettel *n* alkalomnál többször próbálkoznak, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="5abe5-159">Typically, you would keep a retry count as well, and if the message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="5abe5-160">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="5abe5-160">This protects against a message that triggers an application error each time it is processed.</span></span>

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

## <a name="additional-options-for-de-queuing-messages"></a><span data-ttu-id="5abe5-161">Az üzenetek üzenetsorból további beállítások</span><span class="sxs-lookup"><span data-stu-id="5abe5-161">Additional options for de-queuing messages</span></span>
<span data-ttu-id="5abe5-162">Két módon, hogy testre szabhatja az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="5abe5-162">There are two ways that you can customize message retrieval from a queue.</span></span> <span data-ttu-id="5abe5-163">Az első lehetőség az üzenetkötegek (legfeljebb 32) lekérése.</span><span class="sxs-lookup"><span data-stu-id="5abe5-163">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="5abe5-164">Második beállíthat egy hosszabb vagy rövidebb láthatósági időkorlátot hosszabb vagy rövidebb idő teljesen feldolgozni a minden üzenet, amely lehetővé teszi, hogy a kódot.</span><span class="sxs-lookup"><span data-stu-id="5abe5-164">Second, you can set a longer or shorter visibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="5abe5-165">Az alábbi példakód a **getMessages** módszer segítségével 16 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="5abe5-165">The following code example uses the **getMessages** method to get 16 messages in one call.</span></span> <span data-ttu-id="5abe5-166">Ezután minden üzenetet feldolgozza a használatával egy **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="5abe5-166">Then it processes each message by using a **for** loop.</span></span> <span data-ttu-id="5abe5-167">Mindemellett a láthatatlansági időkorlátot minden üzenethez öt percre állítja be.</span><span class="sxs-lookup"><span data-stu-id="5abe5-167">It also sets the invisibility timeout to five minutes for each message.</span></span>

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

## <a name="get-queue-length"></a><span data-ttu-id="5abe5-168">Get-várólista hossza</span><span class="sxs-lookup"><span data-stu-id="5abe5-168">Get queue length</span></span>
<span data-ttu-id="5abe5-169">Megbecsülheti egy üzenetsorban található üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="5abe5-169">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="5abe5-170">A **QueueRestProxy -> getQueueMetadata** módszer a queue szolgáltatás az üzenetsorral kapcsolatos metaadatok visszaadását kéri.</span><span class="sxs-lookup"><span data-stu-id="5abe5-170">The **QueueRestProxy->getQueueMetadata** method asks the queue service to return metadata about the queue.</span></span> <span data-ttu-id="5abe5-171">Hívja a **getApproximateMessageCount** a visszaadott objektumon módszer lehetővé teszi az üzenetek számának vannak a várólistán lévő számát.</span><span class="sxs-lookup"><span data-stu-id="5abe5-171">Calling the **getApproximateMessageCount** method on the returned object provides a count of how many messages are in a queue.</span></span> <span data-ttu-id="5abe5-172">A szám nem csak hozzávetőleges, mert az üzenetek hozzáadására vagy törlődik, miután a queue szolgáltatás válaszol a kérésre.</span><span class="sxs-lookup"><span data-stu-id="5abe5-172">The count is only approximate because messages can be added or removed after the queue service responds to your request.</span></span>

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

## <a name="delete-a-queue"></a><span data-ttu-id="5abe5-173">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="5abe5-173">Delete a queue</span></span>
<span data-ttu-id="5abe5-174">Egy üzenetsor és a benne lévő összes üzenet törléséhez hívja meg a **QueueRestProxy -> deleteQueue** metódust.</span><span class="sxs-lookup"><span data-stu-id="5abe5-174">To delete a queue and all the messages in it, call the **QueueRestProxy->deleteQueue** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5abe5-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5abe5-175">Next steps</span></span>
<span data-ttu-id="5abe5-176">Most, hogy megismerte az Azure Queue storage alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről:</span><span class="sxs-lookup"><span data-stu-id="5abe5-176">Now that you've learned the basics of Azure Queue storage, follow these links to learn about more complex storage tasks:</span></span>

* <span data-ttu-id="5abe5-177">Látogasson el a [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="5abe5-177">Visit the [Azure Storage Team blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>

<span data-ttu-id="5abe5-178">További információ: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="5abe5-178">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
<span data-ttu-id="5abe5-179">[require_once]: http://www.php.net/manual/en/function.require-once.php</span><span class="sxs-lookup"><span data-stu-id="5abe5-179">[require_once]: http://www.php.net/manual/en/function.require-once.php</span></span>
[Azure Portal]: https://portal.azure.com

