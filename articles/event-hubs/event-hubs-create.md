---
title: az Azure event hubs aaaCreate |} Microsoft Docs
description: "Hozzon létre egy Azure Event Hubs névtér és az eseményközpontok hello Azure-portál használatával"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: sethm
ms.openlocfilehash: 9a8b7711e2ca7d112e24be19353d43c365ff6935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-hello-azure-portal"></a>Hozzon létre egy Event Hubs névtér és az eseményközpontok hello Azure-portál használatával

## <a name="create-an-event-hubs-namespace"></a>Az Event Hubs-névtér létrehozása
1. Jelentkezzen be toohello [Azure-portálon][Azure portal], és kattintson **új** hello: bal felső részén üdvözlő képernyőt.
1. Kattintson a **az eszközök internetes hálózatát**, és kattintson a **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. A hello **névtér létrehozása** panelen adja meg a névtér nevét. hello rendszer azonnal ellenőrzi toosee, ha hello neve érhető el.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Miután meggyőződött arról hello név nem érhető el, válassza ki azt a hello tarifacsomag (alapszintű vagy Standard). Válassza ki, egy Azure-előfizetéssel, erőforráscsoportot és helyet az erőforrás-toocreate hello. 
1. Kattintson a **létrehozása** toocreate hello névtér. Előfordulhat, hogy toowait hello toofully rendelkezés hello rendszererőforrásokat néhány percig.
2. Hello portál névterek, kattintson az újonnan létrehozott névtér hello.
2. Kattintson a **megosztott elérési házirendek**, és kattintson a **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. Kattintson a hello Másolás gombra toocopy hello **RootManageSharedAccessKey** kapcsolati karakterlánc toohello vágólapra. Mentse ezt a kapcsolati karakterláncot egy ideiglenes helyre, például a Jegyzettömbben toouse később.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Eseményközpont létrehozása

1. Hello Event Hubs névtér, kattintson az újonnan létrehozott hello névtér.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. Hello névtér paneljén kattintson **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. Hello hello panel felső részén kattintson **adja hozzá az Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Adja meg az eseményközpont nevét, majd kattintson az **létrehozása**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)

Az eseményközpont megtörtént, és rendelkezik hello kapcsolati karakterláncokkal toosend kell, és képes eseményeket fogadni.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs toolearn látogasson el ezeket a hivatkozásokat:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Event Hubs API overview](event-hubs-api-overview.md) (Event Hubs API – áttekintés)

[Azure portal]: https://portal.azure.com/