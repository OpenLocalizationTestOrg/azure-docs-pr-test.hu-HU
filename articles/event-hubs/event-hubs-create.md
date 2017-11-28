---
title: "Hozzon létre egy Azure event hubs |} Microsoft Docs"
description: "Hozzon létre egy Azure Event Hubs névtér és egy eseményközpontot, az Azure portál használatával"
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
ms.openlocfilehash: 816bf1426704d3391550e80c0700f1b011683a94
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a><span data-ttu-id="ca232-103">Hozzon létre egy Event Hubs névtér és egy eseményközpontot, az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="ca232-103">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="ca232-104">Az Event Hubs-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="ca232-104">Create an Event Hubs namespace</span></span>
1. <span data-ttu-id="ca232-105">Jelentkezzen be az [Azure Portalra][Azure portal], és kattintson az **Új** gombra a képernyő bal felső részén.</span><span class="sxs-lookup"><span data-stu-id="ca232-105">Log on to the [Azure portal][Azure portal], and click **New** at the top left of the screen.</span></span>
1. <span data-ttu-id="ca232-106">Kattintson a **az eszközök internetes hálózatát**, és kattintson a **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="ca232-106">Click **Internet of Things**, and then click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. <span data-ttu-id="ca232-107">A **Névtér létrehozása** panelen adja meg a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="ca232-107">In the **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="ca232-108">A rendszer azonnal ellenőrzi, hogy a név elérhető-e.</span><span class="sxs-lookup"><span data-stu-id="ca232-108">The system immediately checks to see if the name is available.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. <span data-ttu-id="ca232-109">Miután ellenőrizte, hogy a névtér neve elérhető-e, válassza ki a tarifacsomagot (Basic vagy Standard).</span><span class="sxs-lookup"><span data-stu-id="ca232-109">After making sure the namespace name is available, choose the pricing tier (Basic or Standard).</span></span> <span data-ttu-id="ca232-110">Valamint válassza ki azt az Azure-előfizetést, erőforráscsoportot és helyet, amellyel az erőforrást létre kívánja hozni.</span><span class="sxs-lookup"><span data-stu-id="ca232-110">Also, choose an Azure subscription, resource group, and location in which to create the resource.</span></span> 
1. <span data-ttu-id="ca232-111">A névtér létrehozásához kattintson a **Létrehozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="ca232-111">Click **Create** to create the namespace.</span></span> <span data-ttu-id="ca232-112">Előfordulhat, hogy Várjon néhány percet, hogy a rendszer teljesen kiépíteni az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ca232-112">You may have to wait a few minutes for the system to fully provision the resources.</span></span>
2. <span data-ttu-id="ca232-113">A portál névterek listája kattintson az újonnan létrehozott névtérre.</span><span class="sxs-lookup"><span data-stu-id="ca232-113">In the portal list of namespaces, click the newly created namespace.</span></span>
2. <span data-ttu-id="ca232-114">Kattintson a **megosztott elérési házirendek**, és kattintson a **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="ca232-114">Click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. <span data-ttu-id="ca232-115">Kattintson a másolás gombra, hogy a **RootManageSharedAccessKey** kapcsolati karakterláncot a vágólapra másolja.</span><span class="sxs-lookup"><span data-stu-id="ca232-115">Click the copy button to copy the **RootManageSharedAccessKey** connection string to the clipboard.</span></span> <span data-ttu-id="ca232-116">Mentse ezt a kapcsolati karakterláncot egy ideiglenes helyre, például a Jegyzettömbben későbbi használat céljából.</span><span class="sxs-lookup"><span data-stu-id="ca232-116">Save this connection string in a temporary location, such as Notepad, to use later.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a><span data-ttu-id="ca232-117">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="ca232-117">Create an event hub</span></span>

1. <span data-ttu-id="ca232-118">Az Event Hubs névtér, kattintson az újonnan létrehozott névtér.</span><span class="sxs-lookup"><span data-stu-id="ca232-118">In the Event Hubs namespace list, click the newly created namespace.</span></span>      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. <span data-ttu-id="ca232-119">A névtér panelen kattintson az **Event Hubs** elemre.</span><span class="sxs-lookup"><span data-stu-id="ca232-119">In the namespace blade, click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. <span data-ttu-id="ca232-120">A panel tetején kattintson az **Eseményközpont felvétele** parancsra.</span><span class="sxs-lookup"><span data-stu-id="ca232-120">At the top of the blade, click **Add Event Hub**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. <span data-ttu-id="ca232-121">Adja meg az eseményközpont nevét, majd kattintson az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ca232-121">Type a name for your event hub, then click **Create**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub5.png)

<span data-ttu-id="ca232-122">Az eseményközpont megtörtént, és az események küldéséhez és fogadásához szükséges kapcsolati karakterlánccal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ca232-122">Your event hub is now created, and you have the connection strings you need to send and receive events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca232-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ca232-123">Next steps</span></span>
<span data-ttu-id="ca232-124">Az Event Hubs kapcsolatos további információkért látogasson el ezeket a hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="ca232-124">To learn more about Event Hubs, visit these links:</span></span>

* [<span data-ttu-id="ca232-125">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ca232-125">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="ca232-126">Event Hubs API – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ca232-126">Event Hubs API overview</span></span>](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/