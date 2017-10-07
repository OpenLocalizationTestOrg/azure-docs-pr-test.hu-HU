---
title: a Queue storage a Java aaaHow toouse |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure Queue szolgáltatás toocreate és a delete várólisták és a Beszúrás, get, és törli az üzenetet. A Java nyelven írt minták."
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
ms.openlocfilehash: 297f89c9d21a38d2b4a5f4346f66f59f9d487010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-java"></a><span data-ttu-id="d1e6f-104">Hogyan toouse a Queue storage a Javával</span><span class="sxs-lookup"><span data-stu-id="d1e6f-104">How toouse Queue storage from Java</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="d1e6f-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d1e6f-105">Overview</span></span>
<span data-ttu-id="d1e6f-106">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Queue storage szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-106">This guide will show you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="d1e6f-107">hello minták Java nyelven íródtak, és használja a hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="d1e6f-107">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="d1e6f-108">hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozás** és **törlése** várólisták.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating** and **deleting** queues.</span></span> <span data-ttu-id="d1e6f-109">A várólisták további információkért lásd: hello [további lépések](#Next-Steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-109">For more information on queues, see hello [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="d1e6f-110">Megjegyzés: Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-110">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="d1e6f-111">További információkért lásd: hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="d1e6f-111">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="d1e6f-112">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1e6f-112">Create a Java application</span></span>
<span data-ttu-id="d1e6f-113">Ez az útmutató a tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-113">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="d1e6f-114">toodo tooinstall kell tehát hello Java fejlesztői készlet (JDK), és hozzon létre egy Azure storage-fiókot az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-114">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="d1e6f-115">Ha ezzel végzett, szüksége lesz a fejlesztői rendszerhez megfelelő hello minimális követelményeit és függőségeit, amelyek hello vannak felsorolva tooverify [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-115">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="d1e6f-116">Ha a rendszer megfelel ezeknek a követelményeknek, letöltése és telepítése hello Azure Storage-könyvtárakban Java ebből a rendszeren hello utasításokat kövesse.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-116">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="d1e6f-117">E feladatok befejezése után fog tudni toocreate ebben a cikkben hello példák használó Java-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-117">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="d1e6f-118">Az alkalmazás tooaccess várólista-tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d1e6f-118">Configure your application tooaccess queue storage</span></span>
<span data-ttu-id="d1e6f-119">Adja hozzá a következő importálási utasítások toohello felső részén hello Java fájl toouse az Azure storage API-k tooaccess várólisták hello:</span><span class="sxs-lookup"><span data-stu-id="d1e6f-119">Add hello following import statements toohello top of hello Java file where you want toouse Azure storage APIs tooaccess queues:</span></span>

```java
// Include hello following imports toouse queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="d1e6f-120">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="d1e6f-120">Setup an Azure storage connection string</span></span>
<span data-ttu-id="d1e6f-121">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="d1e6f-122">Ha egy ügyfél-alkalmazás fut, adja meg a hello tárolási kapcsolati karakterlánc formátuma a következő, a tárfiók nevére hello segítségével hello majd hello felsorolt hello tárfiók elsődleges elérési kulcsát hello [Azure Portal](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-122">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="d1e6f-123">Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="d1e6f-123">This example shows how you can declare a static field toohold hello connection string:</span></span>

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="d1e6f-124">Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja hello szolgáltatás konfigurációs fájljában, *ServiceConfiguration.cscfg*, és egy hívás toohello az elérhető  **RoleEnvironment.getConfigurationSettings** metódust.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-124">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="d1e6f-125">Íme egy példa a hello kapcsolati karakterláncának beolvasása egy **beállítás** nevű elem *StorageConnectionString* hello szolgáltatás konfigurációs fájljában:</span><span class="sxs-lookup"><span data-stu-id="d1e6f-125">Here's an example of getting hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="d1e6f-126">hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-126">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="d1e6f-127">Útmutató: a várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1e6f-127">How to: Create a queue</span></span>
<span data-ttu-id="d1e6f-128">A **CloudQueueClient** objektum lehetővé teszi a várólisták hivatkozási objektumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-128">A **CloudQueueClient** object lets you get reference objects for queues.</span></span> <span data-ttu-id="d1e6f-129">hello alábbi kód létrehoz egy **CloudQueueClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-129">hello following code creates a **CloudQueueClient** object.</span></span> <span data-ttu-id="d1e6f-130">(Megjegyzés: vannak további lehetőségek toocreate **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a hello [Azure Storage-ügyfél SDK-dokumentáció].)</span><span class="sxs-lookup"><span data-stu-id="d1e6f-130">(Note: There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].)</span></span>

<span data-ttu-id="d1e6f-131">Használjon hello **CloudQueueClient** tooget toouse kívánt referencia toohello várólista-objektum.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-131">Use hello **CloudQueueClient** object tooget a reference toohello queue you want toouse.</span></span> <span data-ttu-id="d1e6f-132">Hello várólista hozhat létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-132">You can create hello queue if it doesn't exist.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create hello queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference tooa queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create hello queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-tooa-queue"></a><span data-ttu-id="d1e6f-133">Útmutató: egy üzenetsor tooa hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d1e6f-133">How to: Add a message tooa queue</span></span>
<span data-ttu-id="d1e6f-134">tooinsert egy létező üzenetsorba, az üzenet először létre kell hoznia egy új **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-134">tooinsert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="d1e6f-135">Ezután hívja hello **addMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-135">Next, call hello **addMessage** method.</span></span> <span data-ttu-id="d1e6f-136">A **CloudQueueMessage** is létrehozható egy karakterláncból (UTF-8 formátumban), illetve egy bájttömböt.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-136">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a byte array.</span></span> <span data-ttu-id="d1e6f-137">Íme, amely létrehoz egy üzenetsort (Ha még nem létezik) és a Beszúrás üdvözlőüzenetére "Hello, World" code.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-137">Here is code which creates a queue (if it doesn't exist) and inserts hello message "Hello, World".</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create hello queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="d1e6f-138">Hogyan: betekintés a következő köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="d1e6f-138">How to: Peek at hello next message</span></span>
<span data-ttu-id="d1e6f-139">Eltávolítása hello meghívásával anélkül is bepillanthat hello betekintés a várólista elejére hello **peekMessage**.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-139">You can peek at hello message in hello front of a queue without removing it from hello queue by calling **peekMessage**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at hello next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output hello message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="d1e6f-140">Hogyan: hello aszinkron üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="d1e6f-140">How to: Change hello contents of a queued message</span></span>
<span data-ttu-id="d1e6f-141">Módosíthatja egy üzenet helyben hello várólista hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-141">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="d1e6f-142">Üdvözlőüzenetére jelöl, használhatja a munkafeladat hello szolgáltatás tooupdate hello állapotának.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-142">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="d1e6f-143">a következő kód hello hello üzenetsor frissíti az új tartalommal, és a készletek hello látható időtúllépés tooextend további 60 másodperccel.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-143">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="d1e6f-144">Hello üdvözlőüzenetére társított feladat állapotát menti, és lehetőséget ad a hello ügyfél üdvözlőüzenetére dolgozik egy másik perces toocontinue.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-144">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="d1e6f-145">Ez a módszer tootrack többlépéses munkafolyamatokat üzenetsor üzenetein anélkül, hogy toostart keresztül hello kezdődően, ha a folyamat valamelyik lépése toohardware-vagy szoftverhiba miatt nem használható.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-145">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="d1e6f-146">Általában tartja az újrapróbálkozások számát is, és ha hello üzenetet a rendszer ismét megkísérli több mint  *n*  időpontokat, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-146">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="d1e6f-147">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-147">This protects against a message that triggers an application error each time it is processed.</span></span>

<span data-ttu-id="d1e6f-148">hello következő kód a minta keresések hello üzenetsorukat keresztül, megkeresi a hello első üzenetet, amely megfelel a "Hello, World" hello tartalom, majd módosítja a tartalom üdvözlőüzenetére kilép.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-148">hello following code sample searches through hello queue of messages, locates hello first message that matches "Hello, World" for hello content, then modifies hello message content and exits.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // hello maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through hello messages in hello queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify hello content of hello first matching message.
            message.setMessageContent("Updated contents.");
            // Set it toobe visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update hello message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="d1e6f-149">Azt is megteheti hello következő példakód frissíti csak első látható üdvözlőüzenetére hello várólistán.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-149">Alternatively, hello following code sample updates just hello first visible message on hello queue.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve hello first visible message in hello queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify hello message content.
        message.setMessageContent("Updated contents.");
        // Set it toobe visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update hello message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="d1e6f-150">Hogyan: hello várólista hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="d1e6f-150">How to: Get hello queue length</span></span>
<span data-ttu-id="d1e6f-151">A várólistában lévő üzenetek hello számának becslése kérheti le.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-151">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="d1e6f-152">Hello **downloadAttributes** módszert kéri hello Queue szolgáltatás több aktuális értékhez, beleértve az üzenetek számának vannak a várólistán lévő számát.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-152">hello **downloadAttributes** method asks hello Queue service for several current values, including a count of how many messages are in a queue.</span></span> <span data-ttu-id="d1e6f-153">hello száma nem csak hozzávetőleges, mert az üzenetek hozzáadására vagy törlődik, miután hello Queue szolgáltatás válaszol-e tooyour kérelmet.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-153">hello count is only approximate because messages can be added or removed after hello Queue service responds tooyour request.</span></span> <span data-ttu-id="d1e6f-154">Hello **getApproximateMessageCount** metódus visszaadja hello által lekért legutóbbi értéket hello hívás túl**downloadAttributes**, hello Queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-154">hello **getApproximateMessageCount** method returns hello last value retrieved by hello call too**downloadAttributes**, without calling hello Queue service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download hello approximate message count from hello server.
    queue.downloadAttributes();

    // Retrieve hello newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display hello queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="d1e6f-155">Hogyan: hello a következő üzenetet created</span><span class="sxs-lookup"><span data-stu-id="d1e6f-155">How to: Dequeue hello next message</span></span>
<span data-ttu-id="d1e6f-156">A kód dequeues egy üzenetet az üzenetsorból két lépésben.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-156">Your code dequeues a message from a queue in two steps.</span></span> <span data-ttu-id="d1e6f-157">A hívás esetén **retrieveMessage**, a sorhoz hello a következő üzenetet kapott.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-157">When you call **retrieveMessage**, you get hello next message in a queue.</span></span> <span data-ttu-id="d1e6f-158">Az üzenet **retrieveMessage** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-158">A message returned from **retrieveMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="d1e6f-159">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-159">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="d1e6f-160">toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-160">toofinish removing hello message from hello queue, you must also call **deleteMessage**.</span></span> <span data-ttu-id="d1e6f-161">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-161">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="d1e6f-162">A kód hívások **deleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-162">Your code calls **deleteMessage** right after hello message has been processed.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve hello first visible message in hello queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process hello message in less than 30 seconds, and then delete hello message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a><span data-ttu-id="d1e6f-163">További beállítások üzenetmozgatót üzenetek</span><span class="sxs-lookup"><span data-stu-id="d1e6f-163">Additional options for dequeuing messages</span></span>
<span data-ttu-id="d1e6f-164">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-164">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="d1e6f-165">Először is kaphat az üzenetkötegek (felfelé too32).</span><span class="sxs-lookup"><span data-stu-id="d1e6f-165">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="d1e6f-166">Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-166">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span>

<span data-ttu-id="d1e6f-167">hello alábbi példakód hello **retrieveMessages** metódus tooget 20 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-167">hello following code example uses hello **retrieveMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="d1e6f-168">Ezután minden üzenetet használatával feldolgozza a **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-168">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="d1e6f-169">Beállítja a hello láthatatlansági időtúllépés toofive perc (300 másodperc) minden egyes üzenet esetében is.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-169">It also sets hello invisibility timeout toofive minutes (300 seconds) for each message.</span></span> <span data-ttu-id="d1e6f-170">Ugyanaz a hello üzenetek vegye figyelembe, hogy hello öt perc elindítja az összes idő, így ha öt perc telt túl hello hívás**retrieveMessages**, nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-170">Note that hello five minutes starts for all messages at hello same time, so when five minutes have passed since hello call too**retrieveMessages**, any messages which have not been deleted will become visible again.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-hello-queues"></a><span data-ttu-id="d1e6f-171">Hogyan: hello várólisták felsorolása</span><span class="sxs-lookup"><span data-stu-id="d1e6f-171">How to: List hello queues</span></span>
<span data-ttu-id="d1e6f-172">hello aktuális várólisták, hívás hello listája tooobtain **CloudQueueClient.listQueues()** metódus, amely visszaadható gyűjteménye **CloudQueue** objektumok.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-172">tooobtain a list of hello current queues, call hello **CloudQueueClient.listQueues()** method, which will return a collection of **CloudQueue** objects.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through hello collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="d1e6f-173">Útmutató: a várólista törlése</span><span class="sxs-lookup"><span data-stu-id="d1e6f-173">How to: Delete a queue</span></span>
<span data-ttu-id="d1e6f-174">toodelete várólista és köszönőüzenetei minden benne tárolt, hívás hello **deleteIfExists** hello metódusa **CloudQueue** objektum.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-174">toodelete a queue and all hello messages contained in it, call hello **deleteIfExists** method on hello **CloudQueue** object.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete hello queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="d1e6f-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1e6f-175">Next steps</span></span>
<span data-ttu-id="d1e6f-176">Most, hogy megismerte a queue storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="d1e6f-176">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="d1e6f-177">[Az Azure Storage Java SDK][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="d1e6f-177">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="d1e6f-178">[Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage-ügyfél SDK-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="d1e6f-178">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="d1e6f-179">[Az Azure Storage szolgáltatások REST API-ja][Azure Storage Services REST API]</span><span class="sxs-lookup"><span data-stu-id="d1e6f-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span></span>
* <span data-ttu-id="d1e6f-180">[Az Azure Storage csapat blogja][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="d1e6f-180">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage-ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
