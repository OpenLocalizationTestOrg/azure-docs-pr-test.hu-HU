---
title: a Queue storage a Python aaaHow toouse |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure Queue szolgáltatás a Python toocreate várólisták, törlése és beszúrása, lekérése és törli az üzenetet."
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
ms.openlocfilehash: f4f902a2c314401e5c1768fbc80566c8ba25c058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-python"></a><span data-ttu-id="97da2-103">Hogyan toouse a Queue storage a Python</span><span class="sxs-lookup"><span data-stu-id="97da2-103">How toouse Queue storage from Python</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="97da2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="97da2-104">Overview</span></span>
<span data-ttu-id="97da2-105">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Queue storage szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="97da2-105">This guide shows you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="97da2-106">hello minták Python nyelven íródtak, és használja a hello [Microsoft Azure Storage SDK for Python].</span><span class="sxs-lookup"><span data-stu-id="97da2-106">hello samples are written in Python and use hello [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="97da2-107">hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozása és törlése várólisták**.</span><span class="sxs-lookup"><span data-stu-id="97da2-107">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span> <span data-ttu-id="97da2-108">A várólisták további információkért tekintse meg a toohello [lépések] szakasz.</span><span class="sxs-lookup"><span data-stu-id="97da2-108">For more information on queues, refer toohello [Next Steps] section.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a><span data-ttu-id="97da2-109">Útmutató: A várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="97da2-109">How To: Create a Queue</span></span>
<span data-ttu-id="97da2-110">Hello **QueueService** objektum lehetővé teszi, hogy az üzenetsorok.</span><span class="sxs-lookup"><span data-stu-id="97da2-110">hello **QueueService** object lets you work with queues.</span></span> <span data-ttu-id="97da2-111">hello alábbi kód létrehoz egy **QueueService** objektum.</span><span class="sxs-lookup"><span data-stu-id="97da2-111">hello following code creates a **QueueService** object.</span></span> <span data-ttu-id="97da2-112">Adja hozzá a hello következő közelében hello felső bármely Python fájl tooprogrammatically access Azure Storage kívánja:</span><span class="sxs-lookup"><span data-stu-id="97da2-112">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Azure Storage:</span></span>

```python
from azure.storage.queue import QueueService
```

<span data-ttu-id="97da2-113">hello alábbi kód létrehoz egy **QueueService** objektum hello tárfiók neve és a fiók kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="97da2-113">hello following code creates a **QueueService** object using hello storage account name and account key.</span></span> <span data-ttu-id="97da2-114">"Myaccount" és "SajátKulcs" cserélje le a fióknevet és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="97da2-114">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="97da2-115">Útmutató: A várólista üzenet beszúrása</span><span class="sxs-lookup"><span data-stu-id="97da2-115">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="97da2-116">a várólistán, használjon hello üzenet tooinsert **put\_üzenet** hozzon létre egy új üzenetet, és adja hozzá toohello várólista módszert.</span><span class="sxs-lookup"><span data-stu-id="97da2-116">tooinsert a message into a queue, use hello **put\_message** method to create a new message and add it toohello queue.</span></span>

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="97da2-117">Útmutató: A következő üzenetet hello betekintés</span><span class="sxs-lookup"><span data-stu-id="97da2-117">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="97da2-118">Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **betekintés\_üzenetek** metódust.</span><span class="sxs-lookup"><span data-stu-id="97da2-118">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek\_messages** method.</span></span> <span data-ttu-id="97da2-119">Alapértelmezés szerint **betekintés\_üzenetek** lekéri egyetlen üzenetben.</span><span class="sxs-lookup"><span data-stu-id="97da2-119">By default, **peek\_messages** peeks at a single message.</span></span>

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a><span data-ttu-id="97da2-120">Útmutató: Created üzenetek</span><span class="sxs-lookup"><span data-stu-id="97da2-120">How To: Dequeue Messages</span></span>
<span data-ttu-id="97da2-121">A kód eltávolítja az üzenetet az üzenetsorból két lépést.</span><span class="sxs-lookup"><span data-stu-id="97da2-121">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="97da2-122">A hívás esetén **beolvasása\_üzenetek**, hello a következő üzenetet a várólistában egyszerre várakozó alapértelmezés szerint beolvasása.</span><span class="sxs-lookup"><span data-stu-id="97da2-122">When you call **get\_messages**, you get hello next message in a queue by default.</span></span> <span data-ttu-id="97da2-123">Az üzenet **beolvasása\_üzenetek** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="97da2-123">A message returned from **get\_messages** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="97da2-124">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="97da2-124">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="97da2-125">toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **törlése\_üzenet**.</span><span class="sxs-lookup"><span data-stu-id="97da2-125">toofinish removing hello message from hello queue, you must also call **delete\_message**.</span></span> <span data-ttu-id="97da2-126">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód tooprocess üzenet hardver- vagy szoftverhiba miatt nem sikerül, a kód egy másik példánya is megkaphassa ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="97da2-126">This two-step process of removing a message assures that when your code fails tooprocess a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="97da2-127">A kód hívások **törlése\_üzenet** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="97da2-127">Your code calls **delete\_message** right after hello message has been processed.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

<span data-ttu-id="97da2-128">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="97da2-128">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="97da2-129">Először is kaphat az üzenetkötegek (felfelé too32).</span><span class="sxs-lookup"><span data-stu-id="97da2-129">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="97da2-130">Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="97da2-130">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="97da2-131">hello alábbi példakód a **beolvasása\_üzenetek** metódus tooget 16 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="97da2-131">hello following code example uses the **get\_messages** method tooget 16 messages in one call.</span></span> <span data-ttu-id="97da2-132">Ezután minden üzenetet használatával feldolgozza a hurok.</span><span class="sxs-lookup"><span data-stu-id="97da2-132">Then it processes each message using a for loop.</span></span> <span data-ttu-id="97da2-133">Azt is meghatározza hello láthatatlansági időkorlátot minden üzenethez öt percre.</span><span class="sxs-lookup"><span data-stu-id="97da2-133">It also sets hello invisibility timeout to five minutes for each message.</span></span>

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="97da2-134">Útmutató: Módosítsa az sorba állított üzenetek hello tartalma</span><span class="sxs-lookup"><span data-stu-id="97da2-134">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="97da2-135">Módosíthatja egy üzenet helyben hello várólista hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="97da2-135">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="97da2-136">Az üzenet jelöl, használhatja a szolgáltatás tooupdate hello munkafeladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="97da2-136">If the message represents a work task, you could use this feature tooupdate the status of hello work task.</span></span> <span data-ttu-id="97da2-137">hello kódot használja hello **frissítése\_üzenet** metódus tooupdate üzenetet.</span><span class="sxs-lookup"><span data-stu-id="97da2-137">hello code below uses hello **update\_message** method tooupdate a message.</span></span> <span data-ttu-id="97da2-138">hello láthatósági időkorlátot too0, ami azt jelenti, az üzenet jelenik meg azonnal, és hello tartalom frissítésekor van beállítva.</span><span class="sxs-lookup"><span data-stu-id="97da2-138">hello visibility timeout is set too0, meaning the message appears immediately and hello content is updated.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="97da2-139">How To: Get hello várólistájának hossza</span><span class="sxs-lookup"><span data-stu-id="97da2-139">How To: Get hello Queue Length</span></span>
<span data-ttu-id="97da2-140">A várólistában lévő üzenetek hello számának becslése kérheti le.</span><span class="sxs-lookup"><span data-stu-id="97da2-140">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="97da2-141">A **beolvasása\_várólista\_metaadatok** metódus megkéri a várólista szolgáltatás tooreturn metaadatainak hello várólista hello és hello **approximate_message_count**.</span><span class="sxs-lookup"><span data-stu-id="97da2-141">The **get\_queue\_metadata** method asks hello queue service tooreturn metadata about hello queue, and hello **approximate_message_count**.</span></span> <span data-ttu-id="97da2-142">hello eredménye csak hozzávetőleges mert üzenetek hozzáadására vagy törlődik, miután a queue szolgáltatás válaszol-e tooyour kérelmet.</span><span class="sxs-lookup"><span data-stu-id="97da2-142">hello result is only approximate because messages can be added or removed after the queue service responds tooyour request.</span></span>

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="97da2-143">Útmutató: A várólista törlése</span><span class="sxs-lookup"><span data-stu-id="97da2-143">How To: Delete a Queue</span></span>
<span data-ttu-id="97da2-144">toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **törlése\_várólista** metódust.</span><span class="sxs-lookup"><span data-stu-id="97da2-144">toodelete a queue and all hello messages contained in it, call the **delete\_queue** method.</span></span>

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a><span data-ttu-id="97da2-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97da2-145">Next Steps</span></span>
<span data-ttu-id="97da2-146">Most, hogy megismerte a Queue storage alapjait hello, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="97da2-146">Now that you've learned hello basics of Queue storage, follow these links toolearn more.</span></span>

* [<span data-ttu-id="97da2-147">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="97da2-147">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="97da2-148">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="97da2-148">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="97da2-149">[Az Azure Storage csapat blogja]</span><span class="sxs-lookup"><span data-stu-id="97da2-149">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="97da2-150">[Microsoft Azure Storage SDK for Python]</span><span class="sxs-lookup"><span data-stu-id="97da2-150">[Microsoft Azure Storage SDK for Python]</span></span>

[Az Azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python