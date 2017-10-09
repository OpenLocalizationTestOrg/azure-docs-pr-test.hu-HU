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
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-hello-azure-portal"></a><span data-ttu-id="c886e-103">Hozzon létre egy Event Hubs névtér és az eseményközpontok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="c886e-103">Create an Event Hubs namespace and an event hub using hello Azure portal</span></span>

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="c886e-104">Az Event Hubs-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="c886e-104">Create an Event Hubs namespace</span></span>
1. <span data-ttu-id="c886e-105">Jelentkezzen be toohello [Azure-portálon][Azure portal], és kattintson **új** hello: bal felső részén üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="c886e-105">Log on toohello [Azure portal][Azure portal], and click **New** at hello top left of hello screen.</span></span>
1. <span data-ttu-id="c886e-106">Kattintson a **az eszközök internetes hálózatát**, és kattintson a **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="c886e-106">Click **Internet of Things**, and then click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. <span data-ttu-id="c886e-107">A hello **névtér létrehozása** panelen adja meg a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="c886e-107">In hello **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="c886e-108">hello rendszer azonnal ellenőrzi toosee, ha hello neve érhető el.</span><span class="sxs-lookup"><span data-stu-id="c886e-108">hello system immediately checks toosee if hello name is available.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. <span data-ttu-id="c886e-109">Miután meggyőződött arról hello név nem érhető el, válassza ki azt a hello tarifacsomag (alapszintű vagy Standard).</span><span class="sxs-lookup"><span data-stu-id="c886e-109">After making sure hello namespace name is available, choose hello pricing tier (Basic or Standard).</span></span> <span data-ttu-id="c886e-110">Válassza ki, egy Azure-előfizetéssel, erőforráscsoportot és helyet az erőforrás-toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="c886e-110">Also, choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> 
1. <span data-ttu-id="c886e-111">Kattintson a **létrehozása** toocreate hello névtér.</span><span class="sxs-lookup"><span data-stu-id="c886e-111">Click **Create** toocreate hello namespace.</span></span> <span data-ttu-id="c886e-112">Előfordulhat, hogy toowait hello toofully rendelkezés hello rendszererőforrásokat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="c886e-112">You may have toowait a few minutes for hello system toofully provision hello resources.</span></span>
2. <span data-ttu-id="c886e-113">Hello portál névterek, kattintson az újonnan létrehozott névtér hello.</span><span class="sxs-lookup"><span data-stu-id="c886e-113">In hello portal list of namespaces, click hello newly created namespace.</span></span>
2. <span data-ttu-id="c886e-114">Kattintson a **megosztott elérési házirendek**, és kattintson a **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="c886e-114">Click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. <span data-ttu-id="c886e-115">Kattintson a hello Másolás gombra toocopy hello **RootManageSharedAccessKey** kapcsolati karakterlánc toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="c886e-115">Click hello copy button toocopy hello **RootManageSharedAccessKey** connection string toohello clipboard.</span></span> <span data-ttu-id="c886e-116">Mentse ezt a kapcsolati karakterláncot egy ideiglenes helyre, például a Jegyzettömbben toouse később.</span><span class="sxs-lookup"><span data-stu-id="c886e-116">Save this connection string in a temporary location, such as Notepad, toouse later.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a><span data-ttu-id="c886e-117">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="c886e-117">Create an event hub</span></span>

1. <span data-ttu-id="c886e-118">Hello Event Hubs névtér, kattintson az újonnan létrehozott hello névtér.</span><span class="sxs-lookup"><span data-stu-id="c886e-118">In hello Event Hubs namespace list, click hello newly created namespace.</span></span>      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. <span data-ttu-id="c886e-119">Hello névtér paneljén kattintson **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="c886e-119">In hello namespace blade, click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. <span data-ttu-id="c886e-120">Hello hello panel felső részén kattintson **adja hozzá az Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="c886e-120">At hello top of hello blade, click **Add Event Hub**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. <span data-ttu-id="c886e-121">Adja meg az eseményközpont nevét, majd kattintson az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c886e-121">Type a name for your event hub, then click **Create**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub5.png)

<span data-ttu-id="c886e-122">Az eseményközpont megtörtént, és rendelkezik hello kapcsolati karakterláncokkal toosend kell, és képes eseményeket fogadni.</span><span class="sxs-lookup"><span data-stu-id="c886e-122">Your event hub is now created, and you have hello connection strings you need toosend and receive events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c886e-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c886e-123">Next steps</span></span>
<span data-ttu-id="c886e-124">További információ az Event Hubs toolearn látogasson el ezeket a hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="c886e-124">toolearn more about Event Hubs, visit these links:</span></span>

* [<span data-ttu-id="c886e-125">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="c886e-125">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* <span data-ttu-id="c886e-126">[Event Hubs API overview](event-hubs-api-overview.md) (Event Hubs API – áttekintés)</span><span class="sxs-lookup"><span data-stu-id="c886e-126">[Event Hubs API overview](event-hubs-api-overview.md)</span></span>

[Azure portal]: https://portal.azure.com/