---
title: "az Azure IoT peremhálózati aaaOverview |} Microsoft Docs"
description: "Hello architekturális alapfogalmak Azure IoT peremhálózati például átjárók, a modulok és a brókerek ismerteti."
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
ms.openlocfilehash: 32debc0d4f40cfd7f2cce7cf8c76b12ec18ee2dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a><span data-ttu-id="41d32-103">Az Azure IoT peremhálózati architekturális fogalmak</span><span class="sxs-lookup"><span data-stu-id="41d32-103">Azure IoT Edge architectural concepts</span></span>

<span data-ttu-id="41d32-104">Mielőtt bármely mintakód vizsgálja meg, vagy hozzon létre saját IoT Edge használata mező, tisztában kell lennie hello alapvető fogalmakat, amelyek IoT peremhálózati hello architektúrájának.</span><span class="sxs-lookup"><span data-stu-id="41d32-104">Before you examine any sample code or create your own field gateway using IoT Edge, you should understand hello key concepts that underpin hello architecture of IoT Edge.</span></span>

## <a name="iot-edge-modules"></a><span data-ttu-id="41d32-105">Az IoT-Edge-modulok</span><span class="sxs-lookup"><span data-stu-id="41d32-105">IoT Edge modules</span></span>

<span data-ttu-id="41d32-106">Átjáró használjon létrehozásával és összeállításakor Azure IoT peremhálózati *IoT peremhálózati modulok*.</span><span class="sxs-lookup"><span data-stu-id="41d32-106">You build a gateway with Azure IoT Edge by creating and assembling *IoT Edge modules*.</span></span> <span data-ttu-id="41d32-107">Modulok használata *üzenetek* tooexchange adatok egymás mellett.</span><span class="sxs-lookup"><span data-stu-id="41d32-107">Modules use *messages* tooexchange data with each other.</span></span> <span data-ttu-id="41d32-108">Egy modul üzenetet kap, néhány műveletet végez, opcionálisan átalakítja az új üzenetbe, és majd közzéteszi azokat az egyéb modulok tooprocess.</span><span class="sxs-lookup"><span data-stu-id="41d32-108">A module receives a message, performs some action on it, optionally transforms it into a new message, and then publishes it for other modules tooprocess.</span></span> <span data-ttu-id="41d32-109">Egyes modulok esetleg kizárólag új üzeneteket állítanak elő, és nem dolgoznak fel beérkező üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="41d32-109">Some modules might only produce new messages and never process incoming messages.</span></span> <span data-ttu-id="41d32-110">Modulok láncolata adatokat feldolgozó folyamat egyes átalakítás végrehajtja a hello adatokat, hogy a folyamat egy pont a modul hoz létre.</span><span class="sxs-lookup"><span data-stu-id="41d32-110">A chain of modules creates a data processing pipeline with each module performing a transformation on hello data at one point in that pipeline.</span></span>

![Az átjáró moduljainak láncba rendezése az Azure IoT Edge szolgáltatással][1]

<span data-ttu-id="41d32-112">IoT peremhálózati hello a következő összetevőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="41d32-112">IoT Edge contains hello following components:</span></span>

* <span data-ttu-id="41d32-113">Előzetes írásbeli modulokat, amelyek a közös átjáró.</span><span class="sxs-lookup"><span data-stu-id="41d32-113">Pre-written modules that perform common gateway functions.</span></span>
* <span data-ttu-id="41d32-114">a fejlesztői hello felületek toowrite egyéni modulok használatához.</span><span class="sxs-lookup"><span data-stu-id="41d32-114">hello interfaces a developer can use toowrite custom modules.</span></span>
* <span data-ttu-id="41d32-115">hello infrastruktúra szükséges toodeploy, és hajtson végre modulokat.</span><span class="sxs-lookup"><span data-stu-id="41d32-115">hello infrastructure necessary toodeploy and run a set of modules.</span></span>

<span data-ttu-id="41d32-116">hello SDK absztrakciós réteget biztosít, amely lehetővé teszi toobuild átjárók toorun a különböző operációs rendszerek és platformokon.</span><span class="sxs-lookup"><span data-stu-id="41d32-116">hello SDK provides an abstraction layer that enables you toobuild gateways toorun on various operating systems and platforms.</span></span>

![Az Azure IoT Edge absztrakciós rétege][2]

## <a name="messages"></a><span data-ttu-id="41d32-118">Üzenetek</span><span class="sxs-lookup"><span data-stu-id="41d32-118">Messages</span></span>

<span data-ttu-id="41d32-119">Bár végezni kapcsolatban benyújtása modulok más üzenetek tooeach egy kényelmes módszert arra tooconceptualize hogyan a átjáró működik, akkor nem tükrözik mi történik.</span><span class="sxs-lookup"><span data-stu-id="41d32-119">Although thinking about modules passing messages tooeach other is a convenient way tooconceptualize how a gateway functions, it does not accurately reflect what happens.</span></span> <span data-ttu-id="41d32-120">IoT biztonsági modulok használata egy broker toocommunicate egymással.</span><span class="sxs-lookup"><span data-stu-id="41d32-120">IoT Edge modules use a broker toocommunicate with each other.</span></span> <span data-ttu-id="41d32-121">Modulok közzététele üzenetek toohello broker (például a bus, vagy a közzétételi/előfizetési üzenetkezelési mintát használ), és hagyja hello broker útvonal hello üzenet toohello modulok csatlakoztatott tooit.</span><span class="sxs-lookup"><span data-stu-id="41d32-121">Modules publish messages toohello broker (using messaging patterns such as bus, or publish/subscribe) and then let hello broker route hello message toohello modules connected tooit.</span></span>

<span data-ttu-id="41d32-122">A modul használja hello **Broker_Publish** toopublish üzenet toohello közvetítő működik.</span><span class="sxs-lookup"><span data-stu-id="41d32-122">A module uses hello **Broker_Publish** function toopublish a message toohello broker.</span></span> <span data-ttu-id="41d32-123">hello broker üzenetek tooa modul által a visszahívási függvény meghívása nyújt.</span><span class="sxs-lookup"><span data-stu-id="41d32-123">hello broker delivers messages tooa module by invoking a callback function.</span></span> <span data-ttu-id="41d32-124">Az üzenetek kulcs/érték tulajdonságokból és tartalmakból állnak, amelyek memóriablokként vannak továbbítva.</span><span class="sxs-lookup"><span data-stu-id="41d32-124">A message consists of a set of key/value properties and content passed as a block of memory.</span></span>

![az Azure IoT Edge Broker hello hello szerepe][3]

## <a name="message-routing-and-filtering"></a><span data-ttu-id="41d32-126">Üzenettovábbítás és -szűrés</span><span class="sxs-lookup"><span data-stu-id="41d32-126">Message routing and filtering</span></span>

<span data-ttu-id="41d32-127">Két módon toodirect üzenetek toohello megfelelő IoT peremhálózati modulok:</span><span class="sxs-lookup"><span data-stu-id="41d32-127">There are two ways toodirect messages toohello correct IoT Edge modules:</span></span>

* <span data-ttu-id="41d32-128">Hivatkozásokat tartalmaz átadhatók függvénynek adható át toohello broker, hello broker tudja hello forrás és a fogadó minden modulhoz.</span><span class="sxs-lookup"><span data-stu-id="41d32-128">You can pass a set of links can be passed toohello broker so hello broker knows hello source and sink for each module.</span></span>
* <span data-ttu-id="41d32-129">A modul végezhet üdvözlőüzenetére hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="41d32-129">A module can filter on hello properties of hello message.</span></span>

<span data-ttu-id="41d32-130">A modul kell csak jár el egy üzenetet üdvözlőüzenetére szól, ha.</span><span class="sxs-lookup"><span data-stu-id="41d32-130">A module should only act upon a message if hello message is intended for it.</span></span> <span data-ttu-id="41d32-131">Hivatkozások és a hatékony szűrési message üzenet folyamatokat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="41d32-131">Links and message filtering effectively create a message pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41d32-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41d32-132">Next steps</span></span>

<span data-ttu-id="41d32-133">toosee ezekről a fogalmakról minta alkalmazott futtatja, lásd: [felfedezés Azure IoT peremhálózati architektúra][lnk-hello-world].</span><span class="sxs-lookup"><span data-stu-id="41d32-133">toosee these concepts applied in a sample you can run, see [Explore Azure IoT Edge architecture][lnk-hello-world].</span></span>

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md