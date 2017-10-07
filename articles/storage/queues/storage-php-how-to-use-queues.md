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
# <a name="how-toouse-queue-storage-from-php"></a>Hogyan toouse a Queue storage php-ből
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform gyakori helyzetek használatával hello Azure Queue storage szolgáltatás. hello minták íródtak keresztül osztályok hello Windows SDK-t a PHP. hello kezelt forgatókönyvek például a Beszúrás, megtekintésekor, lekérését, és törli az üzenetsor-üzeneteket, valamint létrehozása és törlése várólisták.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása
csak a követelmény a PHP-alkalmazások létrehozása, amely hozzáfér az Azure Queue storage hello hello való hivatkozás hello Azure SDK-t az osztályok a php a kód. A fejlesztői eszközök toocreate használhatja az alkalmazás, például a Jegyzettömbbel.

Ez az útmutató a várólista tárolási szolgáltatásokkal, hogy a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó hívható fogja használni.

## <a name="get-hello-azure-client-libraries"></a>Hello Azure Ügyfélkódtárai beolvasása
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-queue-storage"></a>Az alkalmazás tooaccess várólista-tároló konfigurálása
API-k toouse hello Azure Queue storage kell:

1. Hello robotjába referenciafájlban hello segítségével [require_once] utasítást.
2. Bármely olyan osztállyal, amelynek segítségével lehet hivatkozni.

hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServicesBuilder** osztály.

> [!NOTE]
> Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette-e az hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával. Ha manuálisan telepítette a hello szalagtárak, szüksége lesz a tooreference hello `WindowsAzure.php` robotjába fájlt.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

Hello a következő példa a hello `require_once` utasítás mindig megjelennek, de csak a szükséges hello példa tooexecute hello osztályok lesz hivatkozva.

## <a name="set-up-an-azure-storage-connection"></a>Az Azure storage-kapcsolat beállítása
az Azure Queue storage ügyfél tooinstantiate, először rendelkeznie kell érvényes kapcsolati karakterláncot. hello hello várólista szolgáltatás kapcsolati karakterláncának formátuma az alábbiak szerint.

Élő szolgáltatások elérésére:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Hello emulátor tárolóinak eléréséhez:

```php
UseDevelopmentStorage=true
```

toocreate bármely Azure szolgáltatás ügyfele toouse hello kell **ServicesBuilder** osztály. Hello a következő módszerek bármelyikét használhatja:

* Hello kapcsolatot adjon át közvetlenül tooit karakterlánc.
* Használjon **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:
  * Alapértelmezés szerint mellékelt egy külső forrás támogatása – környezeti változókat.
  * Hozzáadhat új források hello kiterjesztésével **ConnectionStringSource** osztály.

Itt vázolt hello példákért hello kapcsolati karakterlánc közvetlenül fog adható át.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Üzenetsor létrehozása
A **QueueRestProxy** objektum lehetővé teszi, hogy létrehoz egy sort hello segítségével **createQueue** metódust. A várólista létrehozásakor hello várólista beállítások adhatók meg, de ez így nincs szükség. (az alábbi példa hogyan hello tooset metaadatait a várólistát.)

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
> A metaadatok kulcsok nagybetűk közötti különbségtételt szabályozó nem támaszkodhat. Minden kulcs pedig olvassa a kisbetűs hello szolgáltatást.
> 
> 

## <a name="add-a-message-tooa-queue"></a>Egy üzenetsor tooa hozzáadása
egy üzenetsor tooa tooadd használja **QueueRestProxy -> createMessage**. hello metódus időt vesz igénybe, hello üzenetsornév, hello üzenet szövege és üzenet beállításokat. (nem kötelező).

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

## <a name="peek-at-hello-next-message"></a>Betekintés a következő köszönőüzenetei
Is bepillanthat, hogy egy üzenet (vagy üzenetek): a várólista elejére hello nélkül eltávolítása hello meghívásával **QueueRestProxy -> peekMessages**. Alapértelmezés szerint hello **peekMessage** metódus egyetlen üzenet ad vissza, de ezt az értéket módosíthatja hello segítségével **PeekMessagesOptions -> setNumberOfMessages** metódust.

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

## <a name="de-queue-hello-next-message"></a>Hello következő üzenet kivétele az üzenetsorból
A kód eltávolítja az üzenetet az üzenetsorból két lépést. Először hívja **QueueRestProxy -> listMessages**, amely teszi hello üzenet láthatatlanná tooany egyéb kód, amely éppen olvas hello üzenetsorból. Alapértelmezés szerint ez az üzenet marad láthatatlan 30 másodpercig. (Ha üdvözlőüzenetére ebben az időszakban nem törlődik, azt újra láthatóvá válnak hello várólistában.) toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell hívnia **QueueRestProxy -> deleteMessage**. A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy amikor a kód sikertelen tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kaphat hello ugyanazon üzenet, majd próbálkozzon újra. A kód hívások **deleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.

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

## <a name="change-hello-contents-of-a-queued-message"></a>Hello aszinkron üzenet tartalmának módosítása
Módosíthatja egy üzenet helyben hello várólista hello tartalmát meghívásával **QueueRestProxy -> updateMessage**. Üdvözlőüzenetére jelöl, használhatja a munkafeladat hello szolgáltatás tooupdate hello állapotának. a következő kód hello hello üzenetsor frissíti az új tartalommal, és további 60 másodperccel beállítja hello látható időtúllépés tooextend. Ez menti üdvözlőüzenetére társított feladat hello állapotát, és hello ügyfél üdvözlőüzenetére dolgozik egy másik perces toocontinue biztosít. Ez a módszer tootrack többlépéses munkafolyamatokat üzenetsor üzenetein anélkül, hogy toostart keresztül hello kezdődően, ha a folyamat valamelyik lépése toohardware-vagy szoftverhiba miatt nem használható. Általában tartja az újrapróbálkozások számát is, és ha hello üzenetet a rendszer ismét megkísérli több mint  *n*  időpontokat, akkor törlődik. Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.

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
Két módon, hogy testre szabhatja az üzenetek lekérését egy üzenetsorból. Először is kaphat az üzenetkötegek (felfelé too32). Második beállíthat egy hosszabb vagy rövidebb láthatósági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket. hello alábbi példakód hello **getMessages** metódus tooget 16 üzenetek egy hívásban. Ezután minden üzenetet feldolgozza a használatával egy **a** hurok. Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is.

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
A várólistában lévő üzenetek hello számának becslése kérheti le. Hello **QueueRestProxy -> getQueueMetadata** metódus hello várólista szolgáltatás tooreturn metaadatai tesz kapcsolatban hello várólista. Hívása hello **getApproximateMessageCount** hello metódus visszaadott objektum egy adott időszeletben üzenetek számának vannak a várólistán. hello száma nem csak hozzávetőleges, mert az üzenetek hozzáadására vagy törlődik, miután hello queue szolgáltatás válaszol-e tooyour kérelmet.

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
toodelete egy üzenetsor és az összes hello üzenetek, hívja hello **QueueRestProxy -> deleteQueue** metódust.

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
Most, hogy megismerte hello Azure Queue storage alapjait, hajtsa végre az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn:

* A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/).

További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

