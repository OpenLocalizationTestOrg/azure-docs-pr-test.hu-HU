---
title: "A Java a Queue storage használata |} Microsoft Docs"
description: "Útmutató az Azure Queue szolgáltatás segítségével hozza létre, és törli az üzenetsorok, és helyezze, get, és törli az üzenetet. A Java nyelven írt minták."
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 68cecc8e-38c9-4a24-99e8-cb722bc63cf9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 03faea986221453d1862ff0f0d6d1ec21f92f3bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-queue-storage-from-java"></a><span data-ttu-id="12bcb-104">How to use Queue Storage from Java (A Queue Storage használata Javával)</span><span class="sxs-lookup"><span data-stu-id="12bcb-104">How to use Queue storage from Java</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="12bcb-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="12bcb-105">Overview</span></span>
<span data-ttu-id="12bcb-106">Ez az útmutató bemutatja, hogyan hajthat végre a szolgáltatást az Azure Queue storage szolgáltatást használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="12bcb-106">This guide will show you how to perform common scenarios using the Azure Queue storage service.</span></span> <span data-ttu-id="12bcb-107">A mintákat a Java és -felhasználási nyelven íródtak a [Azure Storage SDK for Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="12bcb-107">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="12bcb-108">Az ismertetett forgatókönyvek **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek várólistára, valamint **létrehozása** és **törlése** várólisták.</span><span class="sxs-lookup"><span data-stu-id="12bcb-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating** and **deleting** queues.</span></span> <span data-ttu-id="12bcb-109">A várólisták további információkért lásd: a [további lépések](#Next-Steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="12bcb-109">For more information on queues, see the [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="12bcb-110">Megjegyzés: Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="12bcb-110">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="12bcb-111">További információkért lásd: a [Azure Storage SDK for Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="12bcb-111">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="12bcb-112">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="12bcb-112">Create a Java application</span></span>
<span data-ttu-id="12bcb-113">Ez az útmutató a tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.</span><span class="sxs-lookup"><span data-stu-id="12bcb-113">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="12bcb-114">Ehhez szüksége lesz a Java fejlesztői készlet (JDK) telepítése, és hozzon létre egy Azure storage-fiókot az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="12bcb-114">To do so, you will need to install the Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="12bcb-115">Ha ezzel végzett, akkor győződjön meg arról, hogy a fejlesztési rendszer megfelel-e a minimális követelményeit és függőségeit, amelyek szerepelnek a [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="12bcb-115">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="12bcb-116">Ha a rendszer megfelel ezeknek a követelményeknek, akkor is kövesse letöltése és telepítése az Azure Storage szalagtárak Java ebből a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="12bcb-116">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="12bcb-117">Ezek a feladatok befejezése után lesz ebben a cikkben a példák használó Java-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="12bcb-117">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-queue-storage"></a><span data-ttu-id="12bcb-118">Állítsa be az alkalmazását, a queue storage eléréséhez</span><span class="sxs-lookup"><span data-stu-id="12bcb-118">Configure your application to access queue storage</span></span>
<span data-ttu-id="12bcb-119">Vegye fel a következő importálási utasításokat a felső részén a Java-fájlt, amelyre az Azure storage API-k várólisták eléréséhez használható:</span><span class="sxs-lookup"><span data-stu-id="12bcb-119">Add the following import statements to the top of the Java file where you want to use Azure storage APIs to access queues:</span></span>

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="12bcb-120">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="12bcb-120">Setup an Azure storage connection string</span></span>
<span data-ttu-id="12bcb-121">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc végpontok és adatok szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="12bcb-121">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="12bcb-122">Ha egy ügyfél-alkalmazás fut, meg kell adnia a tárolási kapcsolati karakterlánc a következő formátumban a tárfiók nevével, és a tárfiók elsődleges elérési kulcs szerepel az [Azure Portal](https://portal.azure.com) a a *AccountName* és *AccountKey* értékek.</span><span class="sxs-lookup"><span data-stu-id="12bcb-122">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="12bcb-123">Ez a példa bemutatja, hogyan deklarálhatnak ahhoz, hogy a kapcsolati karakterlánc statikus mezőben:</span><span class="sxs-lookup"><span data-stu-id="12bcb-123">This example shows how you can declare a static field to hold the connection string:</span></span>

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="12bcb-124">Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja a konfigurációs fájlban, *ServiceConfiguration.cscfg*, és hívja érhető el a **RoleEnvironment.getConfigurationSettings** metódust.</span><span class="sxs-lookup"><span data-stu-id="12bcb-124">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="12bcb-125">Íme egy példa a kapcsolati karakterlánc beolvasása egy **beállítás** nevű elem *StorageConnectionString* a konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="12bcb-125">Here's an example of getting the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="12bcb-126">A következő minták azt feltételezik, hogy használt két módszer közül egyik beolvasni a tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="12bcb-126">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="12bcb-127">Útmutató: a várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="12bcb-127">How to: Create a queue</span></span>
<span data-ttu-id="12bcb-128">A **CloudQueueClient** objektum lehetővé teszi a várólisták hivatkozási objektumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="12bcb-128">A **CloudQueueClient** object lets you get reference objects for queues.</span></span> <span data-ttu-id="12bcb-129">Az alábbi kód létrehoz egy **CloudQueueClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="12bcb-129">The following code creates a **CloudQueueClient** object.</span></span> <span data-ttu-id="12bcb-130">(Megjegyzés: további módon létrehozásához **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a a [Azure Storage ügyfél SDK-dokumentáció].)</span><span class="sxs-lookup"><span data-stu-id="12bcb-130">(Note: There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].)</span></span>

<span data-ttu-id="12bcb-131">Használja a **CloudQueueClient** hivatkozás a használni kívánt várólista-objektum.</span><span class="sxs-lookup"><span data-stu-id="12bcb-131">Use the **CloudQueueClient** object to get a reference to the queue you want to use.</span></span> <span data-ttu-id="12bcb-132">A várólista hozhat létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="12bcb-132">You can create the queue if it doesn't exist.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-to-a-queue"></a><span data-ttu-id="12bcb-133">Útmutató: üzenet várólista hozzáadása</span><span class="sxs-lookup"><span data-stu-id="12bcb-133">How to: Add a message to a queue</span></span>
<span data-ttu-id="12bcb-134">Ha üzenetet szeretne beszúrni egy létező üzenetsorba, először hozzon létre egy **CloudQueueMessage** elemet.</span><span class="sxs-lookup"><span data-stu-id="12bcb-134">To insert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="12bcb-135">Ezután hívja a **addMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="12bcb-135">Next, call the **addMessage** method.</span></span> <span data-ttu-id="12bcb-136">A **CloudQueueMessage** is létrehozható egy karakterláncból (UTF-8 formátumban), illetve egy bájttömböt.</span><span class="sxs-lookup"><span data-stu-id="12bcb-136">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a byte array.</span></span> <span data-ttu-id="12bcb-137">Itt található kód létrehoz egy üzenetsort (Ha még nem létezik), és szúrja be a "Hello, World" üzenet.</span><span class="sxs-lookup"><span data-stu-id="12bcb-137">Here is code which creates a queue (if it doesn't exist) and inserts the message "Hello, World".</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="12bcb-138">Útmutató: a következő üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="12bcb-138">How to: Peek at the next message</span></span>
<span data-ttu-id="12bcb-139">Is bepillanthat, hogy egy sor elején található üzenetbe anélkül, hogy eltávolítaná az üzenetsorból meghívásával **peekMessage**.</span><span class="sxs-lookup"><span data-stu-id="12bcb-139">You can peek at the message in the front of a queue without removing it from the queue by calling **peekMessage**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="12bcb-140">Útmutató: az üzenetsorban található üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="12bcb-140">How to: Change the contents of a queued message</span></span>
<span data-ttu-id="12bcb-141">Egy üzenetet tartalmát helyben, az üzenetsorban módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="12bcb-141">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="12bcb-142">Ha az üzenet munkafeladatot jelöl, ezzel a funkcióval frissítheti a munkafeladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="12bcb-142">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="12bcb-143">Az alábbi kód frissíti az üzenetsorban található üzenetet az új tartalommal, és a láthatósági időkorlátot további 60 másodperccel bővíti.</span><span class="sxs-lookup"><span data-stu-id="12bcb-143">The following code updates the queue message with new contents, and sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="12bcb-144">Elmenti az üzenethez társított feladat állapotát, és az ügyfél számára további egy percet biztosít az üzenet használatának folytatására.</span><span class="sxs-lookup"><span data-stu-id="12bcb-144">This saves the state of work associated with the message, and gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="12bcb-145">Ezzel a technikával többlépéses munkafolyamatokat is nyomon követhet az üzenetsor üzenetein anélkül, hogy újra kéne kezdenie, ha a folyamat valamelyik lépése hardver- vagy szoftverhiba miatt meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="12bcb-145">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="12bcb-146">A rendszer általában nyilvántartja az újrapróbálkozások számát, és ha az üzenettel *n* alkalomnál többször próbálkoznak, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="12bcb-146">Typically, you would keep a retry count as well, and if the message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="12bcb-147">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="12bcb-147">This protects against a message that triggers an application error each time it is processed.</span></span>

<span data-ttu-id="12bcb-148">A következő kód a minta keresésre a várólistában lévő üzenetek, megkeresi az első üzenet, amely a "Hello, World" megegyezik a tartalomhoz, majd módosítja a tartalom üzenet, és kilép.</span><span class="sxs-lookup"><span data-stu-id="12bcb-148">The following code sample searches through the queue of messages, locates the first message that matches "Hello, World" for the content, then modifies the message content and exits.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="12bcb-149">A következő példakód azt is megteheti, csak az első látható üzenet a várólistában frissíti.</span><span class="sxs-lookup"><span data-stu-id="12bcb-149">Alternatively, the following code sample updates just the first visible message on the queue.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="12bcb-150">Útmutató: az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="12bcb-150">How to: Get the queue length</span></span>
<span data-ttu-id="12bcb-151">Megbecsülheti egy üzenetsorban található üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="12bcb-151">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="12bcb-152">A **downloadAttributes** módszert kéri a Queue szolgáltatás több aktuális értékhez, beleértve az üzenetek számának vannak a várólistán lévő számát.</span><span class="sxs-lookup"><span data-stu-id="12bcb-152">The **downloadAttributes** method asks the Queue service for several current values, including a count of how many messages are in a queue.</span></span> <span data-ttu-id="12bcb-153">A szám nem csak hozzávetőleges, mert az üzenetek hozzáadására vagy törlődik, miután a Queue szolgáltatás válaszol a kérésre.</span><span class="sxs-lookup"><span data-stu-id="12bcb-153">The count is only approximate because messages can be added or removed after the Queue service responds to your request.</span></span> <span data-ttu-id="12bcb-154">A **getApproximateMessageCount** metódus hívása által lekért legutóbbi értéket ad vissza **downloadAttributes**, a Queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="12bcb-154">The **getApproximateMessageCount** method returns the last value retrieved by the call to **downloadAttributes**, without calling the Queue service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-the-next-message"></a><span data-ttu-id="12bcb-155">Útmutató: a következő üzenet created</span><span class="sxs-lookup"><span data-stu-id="12bcb-155">How to: Dequeue the next message</span></span>
<span data-ttu-id="12bcb-156">A kód dequeues egy üzenetet az üzenetsorból két lépésben.</span><span class="sxs-lookup"><span data-stu-id="12bcb-156">Your code dequeues a message from a queue in two steps.</span></span> <span data-ttu-id="12bcb-157">A hívás esetén **retrieveMessage**, a következő üzenetet kap a sorhoz.</span><span class="sxs-lookup"><span data-stu-id="12bcb-157">When you call **retrieveMessage**, you get the next message in a queue.</span></span> <span data-ttu-id="12bcb-158">Az üzenet **retrieveMessage** ebből a várólistából üzeneteket olvasó többi kód láthatatlanná válik.</span><span class="sxs-lookup"><span data-stu-id="12bcb-158">A message returned from **retrieveMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="12bcb-159">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="12bcb-159">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="12bcb-160">Szeretné távolítani az üzenetet az üzenetsorból, meg kell is hívni **deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="12bcb-160">To finish removing the message from the queue, you must also call **deleteMessage**.</span></span> <span data-ttu-id="12bcb-161">Az üzenetek kétlépéses eltávolítása lehetővé teszi, hogy ha a kód hardver- vagy szoftverhiba miatt nem tud feldolgozni egy üzenetet, a kód egy másik példánya megkaphassa ugyanazt az üzenetet, és újra megpróbálkozhasson a feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="12bcb-161">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="12bcb-162">A kód hívások **deleteMessage** jobb gombbal az üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="12bcb-162">Your code calls **deleteMessage** right after the message has been processed.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a><span data-ttu-id="12bcb-163">További beállítások üzenetmozgatót üzenetek</span><span class="sxs-lookup"><span data-stu-id="12bcb-163">Additional options for dequeuing messages</span></span>
<span data-ttu-id="12bcb-164">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="12bcb-164">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="12bcb-165">Az első lehetőség az üzenetkötegek (legfeljebb 32) lekérése.</span><span class="sxs-lookup"><span data-stu-id="12bcb-165">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="12bcb-166">A második lehetőség az, hogy beállít egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak lehetősége van hosszabb vagy rövidebb idő alatt teljesen feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="12bcb-166">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span>

<span data-ttu-id="12bcb-167">Az alábbi példakód a **retrieveMessages** metódus használatával kérje le a 20 üzenetet egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="12bcb-167">The following code example uses the **retrieveMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="12bcb-168">Ezután minden üzenetet használatával feldolgozza a **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="12bcb-168">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="12bcb-169">Azt is meghatározza a láthatatlansági időkorlátot minden üzenethez öt percre (300 másodperc).</span><span class="sxs-lookup"><span data-stu-id="12bcb-169">It also sets the invisibility timeout to five minutes (300 seconds) for each message.</span></span> <span data-ttu-id="12bcb-170">Vegye figyelembe, hogy az öt perc elindítja az összes üzenet egyszerre, így ha öt perc telt hívása **retrieveMessages**, nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="12bcb-170">Note that the five minutes starts for all messages at the same time, so when five minutes have passed since the call to **retrieveMessages**, any messages which have not been deleted will become visible again.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-queues"></a><span data-ttu-id="12bcb-171">Útmutató: a várólisták felsorolása</span><span class="sxs-lookup"><span data-stu-id="12bcb-171">How to: List the queues</span></span>
<span data-ttu-id="12bcb-172">Az aktuális várólisták listájának beszerzése, hívja meg a **CloudQueueClient.listQueues()** metódus, amely visszaadható gyűjteménye **CloudQueue** objektumok.</span><span class="sxs-lookup"><span data-stu-id="12bcb-172">To obtain a list of the current queues, call the **CloudQueueClient.listQueues()** method, which will return a collection of **CloudQueue** objects.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="12bcb-173">Útmutató: a várólista törlése</span><span class="sxs-lookup"><span data-stu-id="12bcb-173">How to: Delete a queue</span></span>
<span data-ttu-id="12bcb-174">Egy üzenetsor és a benne tárolt összes üzenet törléséhez hívja meg a **deleteIfExists** metódust a **CloudQueue** objektum.</span><span class="sxs-lookup"><span data-stu-id="12bcb-174">To delete a queue and all the messages contained in it, call the **deleteIfExists** method on the **CloudQueue** object.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="12bcb-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12bcb-175">Next steps</span></span>
<span data-ttu-id="12bcb-176">Most, hogy megismerte a queue storage alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.</span><span class="sxs-lookup"><span data-stu-id="12bcb-176">Now that you've learned the basics of queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="12bcb-177">[Az Azure Storage Java SDK][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="12bcb-177">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="12bcb-178">[Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage ügyfél SDK-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="12bcb-178">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="12bcb-179">[Az Azure Storage szolgáltatások REST API-ja][Azure Storage Services REST API]</span><span class="sxs-lookup"><span data-stu-id="12bcb-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span></span>
* <span data-ttu-id="12bcb-180">[Az Azure Storage csapat blogja][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="12bcb-180">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
<span data-ttu-id="12bcb-181">[Azure Storage ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/</span><span class="sxs-lookup"><span data-stu-id="12bcb-181">[Azure Storage Client SDK Reference]: http://dl.windowsazure.com/storage/javadoc/</span></span>
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
