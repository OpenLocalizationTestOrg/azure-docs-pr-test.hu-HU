---
title: "Az Azure IoT peremhálózati áttekintése |} Microsoft Docs"
description: "Ismerteti az alapvető architekturális fogalmakat az Azure IoT Edge például átjárók, a modulok és a brókerek."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: ecdd56c91a8fc2011b3d7abe93b9d27c1e1e0bef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a><span data-ttu-id="ad2d2-103">Az Azure IoT peremhálózati architekturális fogalmak</span><span class="sxs-lookup"><span data-stu-id="ad2d2-103">Azure IoT Edge architectural concepts</span></span>

<span data-ttu-id="ad2d2-104">Mielőtt bármely mintakód vizsgálja meg, vagy hozzon létre saját IoT Edge használata mező, tisztában kell lennie az alapvető fogalmakat, amelyek az IoT-biztonsági architektúrája.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-104">Before you examine any sample code or create your own field gateway using IoT Edge, you should understand the key concepts that underpin the architecture of IoT Edge.</span></span>

## <a name="iot-edge-modules"></a><span data-ttu-id="ad2d2-105">Az IoT-Edge-modulok</span><span class="sxs-lookup"><span data-stu-id="ad2d2-105">IoT Edge modules</span></span>

<span data-ttu-id="ad2d2-106">Átjáró használjon létrehozásával és összeállításakor Azure IoT peremhálózati *IoT peremhálózati modulok*.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-106">You build a gateway with Azure IoT Edge by creating and assembling *IoT Edge modules*.</span></span> <span data-ttu-id="ad2d2-107">A modulok *üzenetek* használatával cserélnek adatokat egymással.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-107">Modules use *messages* to exchange data with each other.</span></span> <span data-ttu-id="ad2d2-108">Az egyes modulok üzeneteket fogadnak, végrehajtanak valamilyen műveletet rajtuk, esetleg átalakítják őket új üzenetekké, majd közzéteszik azokat más modulok számára feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-108">A module receives a message, performs some action on it, optionally transforms it into a new message, and then publishes it for other modules to process.</span></span> <span data-ttu-id="ad2d2-109">Egyes modulok esetleg kizárólag új üzeneteket állítanak elő, és nem dolgoznak fel beérkező üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-109">Some modules might only produce new messages and never process incoming messages.</span></span> <span data-ttu-id="ad2d2-110">Modulok láncba rendezésével egy adatfeldolgozó folyamat hozható létre, amelynek minden pontján valamely modul valamilyen módon átalakítja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-110">A chain of modules creates a data processing pipeline with each module performing a transformation on the data at one point in that pipeline.</span></span>

![Az átjáró moduljainak láncba rendezése az Azure IoT Edge szolgáltatással][1]

<span data-ttu-id="ad2d2-112">Az IoT-Edge a következő összetevőkből áll:</span><span class="sxs-lookup"><span data-stu-id="ad2d2-112">IoT Edge contains the following components:</span></span>

* <span data-ttu-id="ad2d2-113">Előzetes írásbeli modulokat, amelyek a közös átjáró.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-113">Pre-written modules that perform common gateway functions.</span></span>
* <span data-ttu-id="ad2d2-114">Felületek, amelyek segítségével a fejlesztők egyedi modulokat hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-114">The interfaces a developer can use to write custom modules.</span></span>
* <span data-ttu-id="ad2d2-115">A modulok üzembe helyezéséhez és futtatásához szükséges infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-115">The infrastructure necessary to deploy and run a set of modules.</span></span>

<span data-ttu-id="ad2d2-116">Az SDK-t, amely lehetővé teszi, és különböző operációs rendszeren futó átjárók létrehozására absztrakciós réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-116">The SDK provides an abstraction layer that enables you to build gateways to run on various operating systems and platforms.</span></span>

![Az Azure IoT Edge absztrakciós rétege][2]

## <a name="messages"></a><span data-ttu-id="ad2d2-118">Üzenetek</span><span class="sxs-lookup"><span data-stu-id="ad2d2-118">Messages</span></span>

<span data-ttu-id="ad2d2-119">Ha úgy képzeljük el, hogy a modulok egymásnak küldözgetnek üzeneteket, könnyen megragadható az átjáró működése mögött rejlő elv, azonban ez nem pontosan így történik.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-119">Although thinking about modules passing messages to each other is a convenient way to conceptualize how a gateway functions, it does not accurately reflect what happens.</span></span> <span data-ttu-id="ad2d2-120">Az IoT-Edge modulok egy broker használatával kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-120">IoT Edge modules use a broker to communicate with each other.</span></span> <span data-ttu-id="ad2d2-121">Modulok üzenetek közzététele az átvitelszervező (például a bus, vagy a közzétételi/előfizetési üzenetkezelési mintát használ), és engedélyezze a broker továbbítani az üzenetet csatlakozik hozzá modulok.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-121">Modules publish messages to the broker (using messaging patterns such as bus, or publish/subscribe) and then let the broker route the message to the modules connected to it.</span></span>

<span data-ttu-id="ad2d2-122">A modul a **Broker_Publish** függvény használatával teszi közzé az üzeneteket a közvetítőn.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-122">A module uses the **Broker_Publish** function to publish a message to the broker.</span></span> <span data-ttu-id="ad2d2-123">A közvetítő egy visszahívási függvény használatával továbbítja az üzeneteket az egyes moduloknak.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-123">The broker delivers messages to a module by invoking a callback function.</span></span> <span data-ttu-id="ad2d2-124">Az üzenetek kulcs/érték tulajdonságokból és tartalmakból állnak, amelyek memóriablokként vannak továbbítva.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-124">A message consists of a set of key/value properties and content passed as a block of memory.</span></span>

![A közvetítő szerepe az Azure IoT Edge-ben][3]

## <a name="message-routing-and-filtering"></a><span data-ttu-id="ad2d2-126">Üzenettovábbítás és -szűrés</span><span class="sxs-lookup"><span data-stu-id="ad2d2-126">Message routing and filtering</span></span>

<span data-ttu-id="ad2d2-127">A megfelelő IoT Edge-modulok üzenetek közvetlen két módja van:</span><span class="sxs-lookup"><span data-stu-id="ad2d2-127">There are two ways to direct messages to the correct IoT Edge modules:</span></span>

* <span data-ttu-id="ad2d2-128">Átadhatók hivatkozásokat tartalmaz, a broker tudja a forrás és a fogadó minden modul az átvitelszervező kell átadni.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-128">You can pass a set of links can be passed to the broker so the broker knows the source and sink for each module.</span></span>
* <span data-ttu-id="ad2d2-129">A modul az üzenet tulajdonságai alapján szűrhet.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-129">A module can filter on the properties of the message.</span></span>

<span data-ttu-id="ad2d2-130">Az egyes moduloknak azonban csak akkor kell foglalkozniuk egy üzenettel, ha azt nekik szánták.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-130">A module should only act upon a message if the message is intended for it.</span></span> <span data-ttu-id="ad2d2-131">Hivatkozások és a hatékony szűrési message üzenet folyamatokat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ad2d2-131">Links and message filtering effectively create a message pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad2d2-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad2d2-132">Next steps</span></span>

<span data-ttu-id="ad2d2-133">Ezek a fogalmak alkalmazott minta futtatása, olvassa el [felfedezés Azure IoT peremhálózati architektúra][lnk-hello-world].</span><span class="sxs-lookup"><span data-stu-id="ad2d2-133">To see these concepts applied in a sample you can run, see [Explore Azure IoT Edge architecture][lnk-hello-world].</span></span>

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md