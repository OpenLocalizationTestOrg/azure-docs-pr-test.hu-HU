---
title: a Queue storage aaaIntroduction tooAzure |} Microsoft Docs
description: "A Queue storage bemutatása tooAzure"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: robinsh
ms.openlocfilehash: 669effedff7beedde8a119c350a2a70edafedcf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooqueues"></a>Bevezetés tooQueues

Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához. Egyetlen üzenetsor mentése too64 KB méretű is lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.

## <a name="common-uses"></a>Gyakori használati mód

A Queue Storage gyakori használati módjai:

* A munkahelyi tooprocess várakozó aszinkron módon létrehozása
* Üzenetek átadása egy Azure webes szerepkör tooan Azure feldolgozói szerepkörnek

## <a name="queue-service-concepts"></a>A Queue szolgáltatás alapfogalmai

Queue szolgáltatás hello hello a következő összetevőket tartalmazza:

![Várólista fogalmak](./media/storage-queues-introduction/queue1.png)

* **URL-formátum:** várólisták, amelyek megcímezhető hello URL-cím formátuma a következő használatával:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    a következő URL-cím hello hello ábra egyik üzenetsora címek:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **A tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Az Azure Storage méretezhetőségi és teljesítménycéljai).

* **Üzenetsor:** Az üzenetsorok üzenetek készleteit tartalmazzák. Az összes üzenetnek üzenetsorban kell lennie. Vegye figyelembe, hogy hello várólista neve csak kisbetűket. Az üzenetsorok elnevezésével kapcsolatos információkat lásd: [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Üzenetsorok és metaadatok elnevezése).

* **Üzenet:** egy üzenet jelenik meg, tetszőleges méretű a too64 KB fel. hello maximális időtartam, egy üzenet maradhat hello sorban hét nap.

## <a name="next-steps"></a>Következő lépések

* [Tárfiók létrehozása](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Bevezetés az üzenetsorok .NET használatával](storage-dotnet-how-to-use-queues.md)
