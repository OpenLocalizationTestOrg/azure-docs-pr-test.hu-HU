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
ms.openlocfilehash: ce8d999d9fafaef0dab48442560d004c034c0804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-python"></a>Hogyan toouse a Queue storage a Python
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Queue storage szolgáltatás. hello minták Python nyelven íródtak, és használja a hello [Microsoft Azure Storage SDK for Python]. hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozása és törlése várólisták**. A várólisták további információkért tekintse meg a toohello [lépések] szakasz.

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Útmutató: A várólista létrehozása
Hello **QueueService** objektum lehetővé teszi, hogy az üzenetsorok. hello alábbi kód létrehoz egy **QueueService** objektum. Adja hozzá a hello következő közelében hello felső bármely Python fájl tooprogrammatically access Azure Storage kívánja:

```python
from azure.storage.queue import QueueService
```

hello alábbi kód létrehoz egy **QueueService** objektum hello tárfiók neve és a fiók kulcsot használ. "Myaccount" és "SajátKulcs" cserélje le a fióknevet és kulcsot.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Útmutató: A várólista üzenet beszúrása
a várólistán, használjon hello üzenet tooinsert **put\_üzenet** hozzon létre egy új üzenetet, és adja hozzá toohello várólista módszert.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-hello-next-message"></a>Útmutató: A következő üzenetet hello betekintés
Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **betekintés\_üzenetek** metódust. Alapértelmezés szerint **betekintés\_üzenetek** lekéri egyetlen üzenetben.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Útmutató: Created üzenetek
A kód eltávolítja az üzenetet az üzenetsorból két lépést. A hívás esetén **beolvasása\_üzenetek**, hello a következő üzenetet a várólistában egyszerre várakozó alapértelmezés szerint beolvasása. Az üzenet **beolvasása\_üzenetek** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik. Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan. toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **törlése\_üzenet**. A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód tooprocess üzenet hardver- vagy szoftverhiba miatt nem sikerül, a kód egy másik példánya is megkaphassa ugyanazt az üzenetet, és próbálkozzon újra. A kód hívások **törlése\_üzenet** jobb gombbal az üdvözlő üzenet feldolgozása után.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.
Először is kaphat az üzenetkötegek (felfelé too32). Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket. hello alábbi példakód a **beolvasása\_üzenetek** metódus tooget 16 üzenetek egy hívásban. Ezután minden üzenetet használatával feldolgozza a hurok. Azt is meghatározza hello láthatatlansági időkorlátot minden üzenethez öt percre.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Útmutató: Módosítsa az sorba állított üzenetek hello tartalma
Módosíthatja egy üzenet helyben hello várólista hello tartalmát. Az üzenet jelöl, használhatja a szolgáltatás tooupdate hello munkafeladat állapotát. hello kódot használja hello **frissítése\_üzenet** metódus tooupdate üzenetet. hello láthatósági időkorlátot too0, ami azt jelenti, az üzenet jelenik meg azonnal, és hello tartalom frissítésekor van beállítva.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-hello-queue-length"></a>How To: Get hello várólistájának hossza
A várólistában lévő üzenetek hello számának becslése kérheti le. A **beolvasása\_várólista\_metaadatok** metódus megkéri a várólista szolgáltatás tooreturn metaadatainak hello várólista hello és hello **approximate_message_count**. hello eredménye csak hozzávetőleges mert üzenetek hozzáadására vagy törlődik, miután a queue szolgáltatás válaszol-e tooyour kérelmet.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Útmutató: A várólista törlése
toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **törlése\_várólista** metódust.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Queue storage alapjait hello, kövesse az alábbi hivatkozások további toolearn.

* [Python fejlesztői központ](/develop/python/)
* [Az Azure Storage-szolgáltatások REST API-ja](http://msdn.microsoft.com/library/azure/dd179355)
* [Az Azure Storage csapat blogja]
* [Microsoft Azure Storage SDK for Python]

[Az Azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python