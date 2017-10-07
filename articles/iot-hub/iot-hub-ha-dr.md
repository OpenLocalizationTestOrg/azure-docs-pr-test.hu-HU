---
title: "az IoT-központ magas rendelkezésre állási és vészhelyreállítási helyreállítási aaaAzure |} Microsoft Docs"
description: "Hello Azure és az IoT-központ funkciókat, amelyek segítenek toobuild magas rendelkezésre állású Azure IoT-megoldások vész-helyreállítási képességekkel írja le."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: ae320e58-aa20-45b9-abdc-fa4faae8e6dd
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: elioda
ms.openlocfilehash: 74e1fa1682258819ffb3fd84eb79e42fc6e4120f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a><span data-ttu-id="56c7f-103">Az IoT-központ magas rendelkezésre állás és katasztrófa-helyreállítás</span><span class="sxs-lookup"><span data-stu-id="56c7f-103">IoT Hub high availability and disaster recovery</span></span>
<span data-ttu-id="56c7f-104">Az Azure-szolgáltatások, mint az IoT-központ biztosítja a magas rendelkezésre ÁLLÁS létszámcsökkentések hello Azure-régiót szinten bármit tennie kellene hello megoldás által igényelt nélkül használja.</span><span class="sxs-lookup"><span data-stu-id="56c7f-104">As an Azure service, IoT Hub provides high availability (HA) using redundancies at hello Azure region level, without any additional work required by hello solution.</span></span> <span data-ttu-id="56c7f-105">hello Microsoft Azure platform is készít a vész-helyreállítási képességek vagy kereszt-régiónkénti elérhetőség megoldások szolgáltatások toohelp.</span><span class="sxs-lookup"><span data-stu-id="56c7f-105">hello Microsoft Azure platform also includes features toohelp you build solutions with disaster recovery (DR) capabilities or cross-region availability.</span></span> <span data-ttu-id="56c7f-106">Ha tooprovide globális, kereszt-régió magas rendelkezésre állás eszközöket vagy felhasználókat, tervezése és készítse elő a megoldások Azure vész-Helyreállítási funkciók tootake kihasználásához.</span><span class="sxs-lookup"><span data-stu-id="56c7f-106">If you want tooprovide global, cross-region high availability for devices or users, design and prepare your solutions tootake advantage of these Azure DR features.</span></span> <span data-ttu-id="56c7f-107">hello cikk [Azure üzleti folytonossági műszaki útmutatót](../resiliency/resiliency-technical-guidance.md) hello beépített szolgáltatásai az Azure-ban az üzletmenet folytonosságának és vész-Helyreállítási ismerteti.</span><span class="sxs-lookup"><span data-stu-id="56c7f-107">hello article [Azure Business Continuity Technical Guidance](../resiliency/resiliency-technical-guidance.md) describes hello built-in features in Azure for business continuity and DR.</span></span> <span data-ttu-id="56c7f-108">Hello [vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások] [ Disaster recovery and high availability for Azure applications] papír stratégiák architektúra-útmutatót biztosít az Azure-alkalmazások tooachieve magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="56c7f-108">hello [Disaster recovery and high availability for Azure applications][Disaster recovery and high availability for Azure applications] paper provides architecture guidance on strategies for Azure applications tooachieve HA and DR.</span></span>

## <a name="azure-iot-hub-dr"></a><span data-ttu-id="56c7f-109">Az Azure IoT Hub DR</span><span class="sxs-lookup"><span data-stu-id="56c7f-109">Azure IoT Hub DR</span></span>
<span data-ttu-id="56c7f-110">Ezenkívül toointra-régió magas rendelkezésre ÁLLÁSÚ, IoT-központ megvalósítja feladatátvételi mechanizmusok vész-helyreállítási hello felhasználói beavatkozást nem igénylő.</span><span class="sxs-lookup"><span data-stu-id="56c7f-110">In addition toointra-region HA, IoT Hub implements failover mechanisms for disaster recovery that require no intervention from hello user.</span></span> <span data-ttu-id="56c7f-111">Az IoT Hub vész-Helyreállítási önálló kezdeményezett, és a helyreállítási idő célkitűzése (RTO) rendelkezik a 2 – 26 óra és helyreállításipont-céljai (Rpo) a következő hello.</span><span class="sxs-lookup"><span data-stu-id="56c7f-111">IoT Hub DR is self-initiated and has a recovery time objective (RTO) of 2-26 hours, and hello following recovery point objectives (RPOs).</span></span>

| <span data-ttu-id="56c7f-112">Funkció</span><span class="sxs-lookup"><span data-stu-id="56c7f-112">Functionality</span></span> | <span data-ttu-id="56c7f-113">A HELYREÁLLÍTÁSI IDŐKORLÁT</span><span class="sxs-lookup"><span data-stu-id="56c7f-113">RPO</span></span> |
| --- | --- |
| <span data-ttu-id="56c7f-114">A beállításjegyzék és a kommunikációs műveletek a szolgáltatás rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="56c7f-114">Service availability for registry and communication operations</span></span> |<span data-ttu-id="56c7f-115">CName adatvesztés</span><span class="sxs-lookup"><span data-stu-id="56c7f-115">Possible CName loss</span></span> |
| <span data-ttu-id="56c7f-116">Azonosító adataihoz az identitásjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="56c7f-116">Identity data in identity registry</span></span> |<span data-ttu-id="56c7f-117">0-5 perc adatvesztés</span><span class="sxs-lookup"><span data-stu-id="56c7f-117">0-5 mins data loss</span></span> |
| <span data-ttu-id="56c7f-118">Az eszközről a felhőbe irányuló üzenetek</span><span class="sxs-lookup"><span data-stu-id="56c7f-118">Device-to-cloud messages</span></span> |<span data-ttu-id="56c7f-119">Az összes olvasatlan üzenetek elvesznek.</span><span class="sxs-lookup"><span data-stu-id="56c7f-119">All unread messages are lost</span></span> |
| <span data-ttu-id="56c7f-120">Műveletek üzenetek figyelése</span><span class="sxs-lookup"><span data-stu-id="56c7f-120">Operations monitoring messages</span></span> |<span data-ttu-id="56c7f-121">Az összes olvasatlan üzenetek elvesznek.</span><span class="sxs-lookup"><span data-stu-id="56c7f-121">All unread messages are lost</span></span> |
| <span data-ttu-id="56c7f-122">Felhő-eszközre küldött üzenetek</span><span class="sxs-lookup"><span data-stu-id="56c7f-122">Cloud-to-device messages</span></span> |<span data-ttu-id="56c7f-123">0-5 perc adatvesztés</span><span class="sxs-lookup"><span data-stu-id="56c7f-123">0-5 mins data loss</span></span> |
| <span data-ttu-id="56c7f-124">Felhő eszközre a visszajelzési üzenetsor</span><span class="sxs-lookup"><span data-stu-id="56c7f-124">Cloud-to-device feedback queue</span></span> |<span data-ttu-id="56c7f-125">Az összes olvasatlan üzenetek elvesznek.</span><span class="sxs-lookup"><span data-stu-id="56c7f-125">All unread messages are lost</span></span> |

## <a name="regional-failover-with-iot-hub"></a><span data-ttu-id="56c7f-126">Az IoT-központ regionális feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="56c7f-126">Regional failover with IoT Hub</span></span>
<span data-ttu-id="56c7f-127">Az IoT-megoldások üzembe helyezési topológiájához teljes kezelés Ez a cikk hello hatókörén kívül esik.</span><span class="sxs-lookup"><span data-stu-id="56c7f-127">A complete treatment of deployment topologies in IoT solutions is outside hello scope of this article.</span></span> <span data-ttu-id="56c7f-128">hello cikkből hello *regionális feladatátvétel* magas rendelkezésre állás és a katasztrófa utáni helyreállítás célját hello telepítési modelljét.</span><span class="sxs-lookup"><span data-stu-id="56c7f-128">hello article discusses hello *regional failover* deployment model for hello purpose of high availability and disaster recovery.</span></span>

<span data-ttu-id="56c7f-129">Egy regionális feladatátvétel modellben hello hátsó end fut megoldás elsősorban egy datacenter helyen, és egy másodlagos IoT-központ háttér, telepítve van egy másik datacenter helyen.</span><span class="sxs-lookup"><span data-stu-id="56c7f-129">In a regional failover model, hello solution back end runs primarily in one datacenter location, and a secondary IoT hub and back end are deployed in another datacenter location.</span></span> <span data-ttu-id="56c7f-130">Ha az elsődleges adatközpont hello hello IoT-központ kimaradás szenved, vagy hello eszköz toohello elsődleges adatközpont hello a hálózati kapcsolat megszakad.</span><span class="sxs-lookup"><span data-stu-id="56c7f-130">If hello IoT hub in hello primary datacenter suffers an outage or hello network connectivity from hello device toohello primary datacenter is interrupted.</span></span> <span data-ttu-id="56c7f-131">Eszközök hello elsődleges átjáró nem érhető el egy másodlagos végpontot használni.</span><span class="sxs-lookup"><span data-stu-id="56c7f-131">Devices use a secondary service endpoint whenever hello primary gateway cannot be reached.</span></span> <span data-ttu-id="56c7f-132">A kereszt-régió feladatátvételi képesség hello megoldás rendelkezésre állási túl hello egy magas rendelkezésre állása növelhető a.</span><span class="sxs-lookup"><span data-stu-id="56c7f-132">With a cross-region failover capability, hello solution availability can be improved beyond hello high availability of a single region.</span></span>

<span data-ttu-id="56c7f-133">Magas szinten, IoT-központ, egy regionális feladatátvétel modell tooimplement hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="56c7f-133">At a high level, tooimplement a regional failover model with IoT Hub, you need hello following:</span></span>

* <span data-ttu-id="56c7f-134">**Egy másodlagos IoT hub és útválasztási logikai eszköz**: a szolgáltatás szüneteltetése az elsődleges régióban hello esetben eszközök kell elindítania csatlakozó tooyour másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="56c7f-134">**A secondary IoT hub and device routing logic**: In hello case of a service disruption in your primary region, devices must start connecting tooyour secondary region.</span></span> <span data-ttu-id="56c7f-135">Jellegéből hello állapot-kompatibilis a legtöbb szolgáltatások, esetében gyakori, a megoldás rendszergazdák tootrigger hello közötti régió feladatátvételi folyamat.</span><span class="sxs-lookup"><span data-stu-id="56c7f-135">Given hello state-aware nature of most services involved, it is common for solution administrators tootrigger hello inter-region failover process.</span></span> <span data-ttu-id="56c7f-136">hello legjobb módja toocommunicate hello új végpont toodevices, hello folyamat irányítását megőrzésével toohave őket rendszeresen ellenőrzi a *segítõ* aktuális aktív végpont hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="56c7f-136">hello best way toocommunicate hello new endpoint toodevices, while maintaining control of hello process, is toohave them regularly check a *concierge* service for hello current active endpoint.</span></span> <span data-ttu-id="56c7f-137">hello segítõ szolgáltatás lehet van replikálva, és elérhető-e tartani webalkalmazás DNS-átirányítás technikák segítségével (például az [Azure Traffic Manager][Azure Traffic Manager]).</span><span class="sxs-lookup"><span data-stu-id="56c7f-137">hello concierge service can be a web application that is replicated and kept reachable using DNS-redirection techniques (for example, using [Azure Traffic Manager][Azure Traffic Manager]).</span></span>
* <span data-ttu-id="56c7f-138">**Identitás beállításjegyzék replikációs** -toobe használható, hello másodlagos IoT-központ az összes eszköz identitások csatlakoztatható toohello megoldás kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="56c7f-138">**Identity registry replication** - toobe usable, hello secondary IoT hub must contain all device identities that can connect toohello solution.</span></span> <span data-ttu-id="56c7f-139">hello megoldás kell biztonsági mentések georeplikált eszköz identitások tartása, és toohello másodlagos IoT-központ előtti feltöltésükhöz váltás hello aktív végpontja hello eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="56c7f-139">hello solution should keep geo-replicated backups of device identities, and upload them toohello secondary IoT hub before switching hello active endpoint for hello devices.</span></span> <span data-ttu-id="56c7f-140">hello eszköz identitása exportálási funkció IoT hub akkor hasznos, ebben a környezetben.</span><span class="sxs-lookup"><span data-stu-id="56c7f-140">hello device identity export functionality of IoT Hub is useful in this context.</span></span> <span data-ttu-id="56c7f-141">További információkért lásd: [IoT Hub fejlesztői útmutató - identitásjegyzékhez][IoT Hub developer guide - identity registry].</span><span class="sxs-lookup"><span data-stu-id="56c7f-141">For more information, see [IoT Hub developer guide - identity registry][IoT Hub developer guide - identity registry].</span></span>
* <span data-ttu-id="56c7f-142">**Az egyesítés logika** – amikor ismét elérhetővé hello elsődleges régió válik, minden hello állapotát és hello másodlagos helyen létrehozott adatok áttelepített hátsó toohello elsődleges régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="56c7f-142">**Merging logic** - When hello primary region becomes available again, all hello state and data that have been created in hello secondary site must be migrated back toohello primary region.</span></span> <span data-ttu-id="56c7f-143">Ez állapotának és adatainak többnyire vonatkozik toodevice identitások és az alkalmazás metaadatainak, amely úgy kell egyesíteni hello elsődleges IoT hub és más alkalmazás-specifikus áruházak hello elsődleges régióban.</span><span class="sxs-lookup"><span data-stu-id="56c7f-143">This state and data mostly relates toodevice identities and application metadata, which must be merged with hello primary IoT hub and any other application-specific stores in hello primary region.</span></span> <span data-ttu-id="56c7f-144">toosimplify Ez a lépés idempotent műveleteket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="56c7f-144">toosimplify this step, you should use idempotent operations.</span></span> <span data-ttu-id="56c7f-145">Az Idempotent műveletek hello mellékhatásokkal hello végleges konzisztens terjesztési események, illetve a duplikálás vagy események soron kézbesítését minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="56c7f-145">Idempotent operations minimize hello side-effects from hello eventual consistent distribution of events, and from duplicates or out-of-order delivery of events.</span></span> <span data-ttu-id="56c7f-146">Ezenkívül hello úgy az alkalmazáslogikát lehet tervezett tootolerate lehetséges inkonzisztenciát "némileg", dátum-állapotát.</span><span class="sxs-lookup"><span data-stu-id="56c7f-146">In addition, hello application logic should be designed tootolerate potential inconsistencies or "slightly" out of date-state.</span></span> <span data-ttu-id="56c7f-147">Ez a helyzet akkor következhetnek be toohello további időt hello rendszer túl "javítandó" helyreállításipont-céljai (RPO) alapján.</span><span class="sxs-lookup"><span data-stu-id="56c7f-147">This situation can occur due toohello additional time it takes for hello system too"heal" based on recovery point objectives (RPO).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56c7f-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="56c7f-148">Next steps</span></span>
<span data-ttu-id="56c7f-149">Hajtsa végre a további információk az Azure IoT Hub hivatkozások toolearn:</span><span class="sxs-lookup"><span data-stu-id="56c7f-149">Follow these links toolearn more about Azure IoT Hub:</span></span>

* <span data-ttu-id="56c7f-150">[Ismerkedés az IoT-központok (Útmutató)][lnk-get-started]</span><span class="sxs-lookup"><span data-stu-id="56c7f-150">[Get started with IoT Hubs (Tutorial)][lnk-get-started]</span></span>
* <span data-ttu-id="56c7f-151">[Mi az Azure IoT Hub?][What is Azure IoT Hub?]</span><span class="sxs-lookup"><span data-stu-id="56c7f-151">[What is Azure IoT Hub?][What is Azure IoT Hub?]</span></span>

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md