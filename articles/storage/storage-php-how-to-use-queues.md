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
# <a name="how-to-use-queue-storage-from-php"></a>How to use Queue storage from PHP (A Queue Storage használata PHP-val)
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan hajthat végre a gyakori forgatókönyvek az Azure Queue storage szolgáltatás használatával. A minták íródtak osztályok keresztül a Windows SDK-ból a PHP. A kezelt forgatókönyvek például a Beszúrás, megtekintésekor, lekérését, és törli az üzenetsor-üzeneteket, valamint létrehozása és törlése várólisták.

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása
A PHP-alkalmazások Azure Queue storage hozzáférő létrehozása egyetlen követelménye, a hivatkozási osztályok az Azure SDK-ból a php a kód. A fejlesztői eszközök hozhat létre az alkalmazás, például a Jegyzettömbbel.

Ez az útmutató a várólista tárolási szolgáltatásokkal, hogy a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó hívható fogja használni.

## <a name="get-the-azure-client-libraries"></a>Az Azure Ügyfélkódtárai beolvasása
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-queue-storage"></a>Állítsa be az alkalmazását, a Queue storage eléréséhez
Az API-kat az Azure Queue storage használandó kell:

1. Hivatkozzon a robotot használatával a [require_once] utasítást.
2. Bármely olyan osztállyal, amelynek segítségével lehet hivatkozni.

A következő példa bemutatja, hogyan a robotot fájl és a hivatkozás a **ServicesBuilder** osztály.

> [!NOTE]
> Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette az Azure-szerkesztő segítségével, a PHP Klienskódtárak segítségével. Ha a könyvtárak manuálisan telepítette, szüksége lesz való hivatkozáshoz a `WindowsAzure.php` robotjába fájlt.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

Az alábbi példákban a `require_once` utasítás mindig megjelennek, de csak a szükséges végrehajtandó például osztályok lesz hivatkozva.

## <a name="set-up-an-azure-storage-connection"></a>Az Azure storage-kapcsolat beállítása
Az Azure Queue storage ügyfél példányosítható, először egy érvényes kapcsolati karakterláncot kell rendelkeznie. A várólista-szolgáltatás kapcsolati karakterláncának formátuma a következő.

Élő szolgáltatások elérésére:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Az emulátor tárolóinak eléréséhez:

```php
UseDevelopmentStorage=true
```

Bármely Azure szolgáltatás ügyfele létrehozásához kell használnia a **ServicesBuilder** osztály. Az alábbi módszerek bármelyikét használhatja:

* Átadni a kapcsolati karakterlánc közvetlenül.
* Használjon **CloudConfigurationManager (CCM)** ellenőrizze a kapcsolati karakterlánc több külső forrás:
  * Alapértelmezés szerint mellékelt egy külső forrás támogatása – környezeti változókat.
  * Új forrásból történő kiterjesztésével adhat hozzá a **ConnectionStringSource** osztály.

Az itt leírt példák a kapcsolati karakterlánc közvetlenül fog adható át.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Üzenetsor létrehozása
A **QueueRestProxy** objektum lehetővé teszi, hogy a várólista létrehozása a **createQueue** metódust. A várólista létrehozásakor az a várakozási beállítások adhatók meg, de ez így nincs szükség. (Az alábbi példa bemutatja, hogyan metaadatok beállítani a várólista.)

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
> A metaadatok kulcsok nagybetűk közötti különbségtételt szabályozó nem támaszkodhat. Minden kulcs olvasható kisbetűs a szolgáltatásból.
> 
> 

## <a name="add-a-message-to-a-queue"></a>A várólista üzenet hozzáadása
A várólista adhat hozzá egy üzenetet, **QueueRestProxy -> createMessage**. A módszer veszi a várólista nevét, a szöveges üzenet és az üzenet beállításokat. (nem kötelező).

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

## <a name="peek-at-the-next-message"></a>Betekintés a következő üzenetbe
Is bepillanthat, hogy egy üzenet (vagy üzenetek) egy sor elején anélkül, hogy eltávolítaná az üzenetsorból meghívásával **QueueRestProxy -> peekMessages**. Alapértelmezés szerint a **peekMessage** metódus egyetlen üzenet ad vissza, de ezt az értéket használatával módosíthatja a **PeekMessagesOptions -> setNumberOfMessages** metódust.

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

## <a name="de-queue-the-next-message"></a>A következő üzenet kivétele az üzenetsorból
A kód eltávolítja az üzenetet az üzenetsorból két lépést. Először hívja **QueueRestProxy -> listMessages**, ami lehetővé teszi az üzenetet az üzenetsorból olvasó többi kód nem látható. Alapértelmezés szerint ez az üzenet marad láthatatlan 30 másodpercig. (Ha az üzenet nem törlődik az ebben az időszakban, azt újra láthatóvá válnak a várólistán.) Szeretné távolítani az üzenetet az üzenetsorból, meg kell hívnia **QueueRestProxy -> deleteMessage**. A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód nem tudja feldolgozni egy üzenetet, hardver- vagy szoftverhiba miatt, a kód egy másik példánya is megkaphassa ugyanazt az üzenetet, és próbálkozzon újra. A kód hívások **deleteMessage** jobb gombbal az üzenet feldolgozása után.

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

## <a name="change-the-contents-of-a-queued-message"></a>Üzenetsorban található üzenet tartalmának módosítása
Módosíthatja a tartalmát egy üzenet helyben a várólistán lévő meghívásával **QueueRestProxy -> updateMessage**. Ha az üzenet munkafeladatot jelöl, ezzel a funkcióval frissítheti a munkafeladat állapotát. Az alábbi kód frissíti az üzenetsorban található üzenetet az új tartalommal, és meghatározza a láthatósági időkorlátot további 60 másodperccel bővíti. Ez az üzenet társított feladat állapotát menti, és az ügyfél folytathatja a munkát az üzenetben további egy percet biztosít. Ezzel a technikával többlépéses munkafolyamatokat is nyomon követhet az üzenetsor üzenetein anélkül, hogy újra kéne kezdenie, ha a folyamat valamelyik lépése hardver- vagy szoftverhiba miatt meghiúsul. A rendszer általában nyilvántartja az újrapróbálkozások számát, és ha az üzenettel *n* alkalomnál többször próbálkoznak, akkor törlődik. Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.

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

## <a name="additional-options-for-de-queuing-messages"></a>Az üzenetek üzenetsorból további beállítások
Két módon, hogy testre szabhatja az üzenetek lekérését egy üzenetsorból. Az első lehetőség az üzenetkötegek (legfeljebb 32) lekérése. Második beállíthat egy hosszabb vagy rövidebb láthatósági időkorlátot hosszabb vagy rövidebb idő teljesen feldolgozni a minden üzenet, amely lehetővé teszi, hogy a kódot. Az alábbi példakód a **getMessages** módszer segítségével 16 üzenetek egy hívásban. Ezután minden üzenetet feldolgozza a használatával egy **a** hurok. Mindemellett a láthatatlansági időkorlátot minden üzenethez öt percre állítja be.

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

## <a name="get-queue-length"></a>Get-várólista hossza
Megbecsülheti egy üzenetsorban található üzenetek számát. A **QueueRestProxy -> getQueueMetadata** módszer a queue szolgáltatás az üzenetsorral kapcsolatos metaadatok visszaadását kéri. Hívja a **getApproximateMessageCount** a visszaadott objektumon módszer lehetővé teszi az üzenetek számának vannak a várólistán lévő számát. A szám nem csak hozzávetőleges, mert az üzenetek hozzáadására vagy törlődik, miután a queue szolgáltatás válaszol a kérésre.

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

## <a name="delete-a-queue"></a>Üzenetsor törlése
Egy üzenetsor és a benne lévő összes üzenet törléséhez hívja meg a **QueueRestProxy -> deleteQueue** metódust.

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

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte az Azure Queue storage alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről:

* Látogasson el a [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/).

További információ: a [PHP fejlesztői központ](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

