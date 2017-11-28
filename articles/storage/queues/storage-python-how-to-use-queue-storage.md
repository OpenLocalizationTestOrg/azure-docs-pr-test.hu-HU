---
title: "A Python a Queue storage használata |} Microsoft Docs"
description: "Útmutató: az Azure Queue szolgáltatás Python segítségével hozza létre, és törli az üzenetsorok, és helyezze, beolvasása, és törli az üzenetet."
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: cc0d2da2-379a-4b58-a234-8852b4e3d99d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 963c11acb7939993568a774cd281145a8059b5a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-python"></a><span data-ttu-id="35ca5-103">How to use Queue storage from Python (A Queue Storage használata Pythonnal)</span><span class="sxs-lookup"><span data-stu-id="35ca5-103">How to use Queue storage from Python</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="35ca5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="35ca5-104">Overview</span></span>
<span data-ttu-id="35ca5-105">Ez az útmutató bemutatja, hogyan hajthat végre a szolgáltatást az Azure Queue storage szolgáltatást használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="35ca5-105">This guide shows you how to perform common scenarios using the Azure Queue storage service.</span></span> <span data-ttu-id="35ca5-106">A mintákat a Python és -felhasználási nyelven íródtak a [Microsoft Azure Storage SDK for Python].</span><span class="sxs-lookup"><span data-stu-id="35ca5-106">The samples are written in Python and use the [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="35ca5-107">Az ismertetett forgatókönyvek **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek várólistára, valamint **létrehozása és törlése várólisták**.</span><span class="sxs-lookup"><span data-stu-id="35ca5-107">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span> <span data-ttu-id="35ca5-108">A várólisták további információkért tekintse át a [lépések] részt.</span><span class="sxs-lookup"><span data-stu-id="35ca5-108">For more information on queues, refer to the [Next Steps] section.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a><span data-ttu-id="35ca5-109">Útmutató: A várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="35ca5-109">How To: Create a Queue</span></span>
<span data-ttu-id="35ca5-110">A **QueueService** objektum lehetővé teszi, hogy az üzenetsorok.</span><span class="sxs-lookup"><span data-stu-id="35ca5-110">The **QueueService** object lets you work with queues.</span></span> <span data-ttu-id="35ca5-111">Az alábbi kód létrehoz egy **QueueService** objektum.</span><span class="sxs-lookup"><span data-stu-id="35ca5-111">The following code creates a **QueueService** object.</span></span> <span data-ttu-id="35ca5-112">Adja hozzá a következő tetejénél található minden olyan Python fájlt, amelyben programon keresztüli eléréséhez az Azure Storage kívánja:</span><span class="sxs-lookup"><span data-stu-id="35ca5-112">Add the following near the top of any Python file in which you wish to programmatically access Azure Storage:</span></span>

```python
from azure.storage.queue import QueueService
```

<span data-ttu-id="35ca5-113">Az alábbi kód létrehoz egy **QueueService** objektumba a tárfiók nevét és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="35ca5-113">The following code creates a **QueueService** object using the storage account name and account key.</span></span> <span data-ttu-id="35ca5-114">"Myaccount" és "SajátKulcs" cserélje le a fióknevet és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="35ca5-114">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="35ca5-115">Útmutató: A várólista üzenet beszúrása</span><span class="sxs-lookup"><span data-stu-id="35ca5-115">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="35ca5-116">Üzenet beszúrása egy üzenetsort, használja a **put\_üzenet** módszert, hozzon létre egy új üzenetet, és vegye fel a várólistára.</span><span class="sxs-lookup"><span data-stu-id="35ca5-116">To insert a message into a queue, use the **put\_message** method to create a new message and add it to the queue.</span></span>

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="35ca5-117">Útmutató: A következő üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="35ca5-117">How To: Peek at the Next Message</span></span>
<span data-ttu-id="35ca5-118">Is bepillanthat, hogy egy sor elején található üzenetbe anélkül, hogy eltávolítaná az üzenetsorból meghívásával a **betekintés\_üzenetek** metódust.</span><span class="sxs-lookup"><span data-stu-id="35ca5-118">You can peek at the message in the front of a queue without removing it from the queue by calling the **peek\_messages** method.</span></span> <span data-ttu-id="35ca5-119">Alapértelmezés szerint **betekintés\_üzenetek** lekéri egyetlen üzenetben.</span><span class="sxs-lookup"><span data-stu-id="35ca5-119">By default, **peek\_messages** peeks at a single message.</span></span>

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a><span data-ttu-id="35ca5-120">Útmutató: Created üzenetek</span><span class="sxs-lookup"><span data-stu-id="35ca5-120">How To: Dequeue Messages</span></span>
<span data-ttu-id="35ca5-121">A kód eltávolítja az üzenetet az üzenetsorból két lépést.</span><span class="sxs-lookup"><span data-stu-id="35ca5-121">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="35ca5-122">A hívás esetén **beolvasása\_üzenetek**, a hibaüzenet a következő várólista alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="35ca5-122">When you call **get\_messages**, you get the next message in a queue by default.</span></span> <span data-ttu-id="35ca5-123">Az üzenet **beolvasása\_üzenetek** ebből a várólistából üzeneteket olvasó többi kód láthatatlanná válik.</span><span class="sxs-lookup"><span data-stu-id="35ca5-123">A message returned from **get\_messages** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="35ca5-124">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="35ca5-124">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="35ca5-125">Szeretné távolítani az üzenetet az üzenetsorból, meg kell is hívni **törlése\_üzenet**.</span><span class="sxs-lookup"><span data-stu-id="35ca5-125">To finish removing the message from the queue, you must also call **delete\_message**.</span></span> <span data-ttu-id="35ca5-126">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód nem tudja feldolgozni egy üzenetet, hardver- vagy szoftverhiba miatt, a kód egy másik példánya is megkaphassa ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="35ca5-126">This two-step process of removing a message assures that when your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="35ca5-127">A kód hívások **törlése\_üzenet** jobb gombbal az üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="35ca5-127">Your code calls **delete\_message** right after the message has been processed.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

<span data-ttu-id="35ca5-128">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="35ca5-128">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="35ca5-129">Az első lehetőség az üzenetkötegek (legfeljebb 32) lekérése.</span><span class="sxs-lookup"><span data-stu-id="35ca5-129">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="35ca5-130">A második lehetőség az, hogy beállít egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak lehetősége van hosszabb vagy rövidebb idő alatt teljesen feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="35ca5-130">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="35ca5-131">Az alábbi példakód a **beolvasása\_üzenetek** módszer segítségével 16 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="35ca5-131">The following code example uses the **get\_messages** method to get 16 messages in one call.</span></span> <span data-ttu-id="35ca5-132">Ezután minden üzenetet használatával feldolgozza a hurok.</span><span class="sxs-lookup"><span data-stu-id="35ca5-132">Then it processes each message using a for loop.</span></span> <span data-ttu-id="35ca5-133">Mindemellett a láthatatlansági időkorlátot minden üzenethez öt percre állítja be.</span><span class="sxs-lookup"><span data-stu-id="35ca5-133">It also sets the invisibility timeout to five minutes for each message.</span></span>

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="35ca5-134">Útmutató: Az aszinkron üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="35ca5-134">How To: Change the Contents of a Queued Message</span></span>
<span data-ttu-id="35ca5-135">Egy üzenetet tartalmát helyben, az üzenetsorban módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="35ca5-135">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="35ca5-136">Ha az üzenet munkafeladatot jelöl, ezzel a funkcióval frissítheti a munkafeladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="35ca5-136">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="35ca5-137">Az alábbi által használt kódot a **frissítése\_üzenet** üzenet frissítési módjának.</span><span class="sxs-lookup"><span data-stu-id="35ca5-137">The code below uses the **update\_message** method to update a message.</span></span> <span data-ttu-id="35ca5-138">A láthatósági időkorlátot értéke 0, ami azt jelenti, az üzenet jelenik meg azonnal, és a tartalom frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="35ca5-138">The visibility timeout is set to 0, meaning the message appears immediately and the content is updated.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="35ca5-139">Útmutató: Az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="35ca5-139">How To: Get the Queue Length</span></span>
<span data-ttu-id="35ca5-140">Megbecsülheti egy üzenetsorban található üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="35ca5-140">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="35ca5-141">A **beolvasása\_várólista\_metaadatok** módszert kéri a queue szolgáltatás az üzenetsorral kapcsolatos metaadatok visszaadását és a **approximate_message_count**.</span><span class="sxs-lookup"><span data-stu-id="35ca5-141">The **get\_queue\_metadata** method asks the queue service to return metadata about the queue, and the **approximate_message_count**.</span></span> <span data-ttu-id="35ca5-142">Az eredmény csak azért hozzávetőleges, mert üzenetek hozzáadására vagy törlődik, miután a queue szolgáltatás válaszol a kérésre.</span><span class="sxs-lookup"><span data-stu-id="35ca5-142">The result is only approximate because messages can be added or removed after the queue service responds to your request.</span></span>

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="35ca5-143">Útmutató: A várólista törlése</span><span class="sxs-lookup"><span data-stu-id="35ca5-143">How To: Delete a Queue</span></span>
<span data-ttu-id="35ca5-144">Egy üzenetsor és a benne tárolt összes üzenet törléséhez hívja meg a **törlése\_várólista** metódust.</span><span class="sxs-lookup"><span data-stu-id="35ca5-144">To delete a queue and all the messages contained in it, call the **delete\_queue** method.</span></span>

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a><span data-ttu-id="35ca5-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35ca5-145">Next Steps</span></span>
<span data-ttu-id="35ca5-146">Most, hogy megismerte a Queue storage alapjait, az alábbi hivatkozásokból további.</span><span class="sxs-lookup"><span data-stu-id="35ca5-146">Now that you've learned the basics of Queue storage, follow these links to learn more.</span></span>

* [<span data-ttu-id="35ca5-147">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="35ca5-147">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="35ca5-148">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="35ca5-148">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="35ca5-149">[Az Azure Storage csapat blogja]</span><span class="sxs-lookup"><span data-stu-id="35ca5-149">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="35ca5-150">[Microsoft Azure Storage SDK for Python]</span><span class="sxs-lookup"><span data-stu-id="35ca5-150">[Microsoft Azure Storage SDK for Python]</span></span>

[Az Azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python