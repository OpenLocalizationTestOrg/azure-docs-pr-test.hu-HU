---
title: "aaaSecure a az eszközök internetes hálózatát (IoT) az Azure-ban |} Microsoft Docs"
description: " Azure internetes hálózata (IoT) szolgáltatások széles skálájával képességeket kínálnak. Ez a cikk segítségével megismerheti, hogyan toosecure az IoT-megoldások Azure-ban. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: b6cb2ea1c1facada854fb52c55066f34a8289e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-overview"></a><span data-ttu-id="c9aa9-104">Az eszközök internetes hálózatát biztonsági áttekintése</span><span class="sxs-lookup"><span data-stu-id="c9aa9-104">Internet of Things security overview</span></span>
<span data-ttu-id="c9aa9-105">Azure internetes hálózata (IoT) szolgáltatások széles skálájával képességeket kínálnak.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-105">Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="c9aa9-106">Ezekkel a vállalati szintű szolgáltatásokkal a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="c9aa9-106">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="c9aa9-107">Adatok gyűjtése eszközökről</span><span class="sxs-lookup"><span data-stu-id="c9aa9-107">Collect data from devices</span></span>
* <span data-ttu-id="c9aa9-108">Streamek elemzése mozgás közben</span><span class="sxs-lookup"><span data-stu-id="c9aa9-108">Analyze data streams in-motion</span></span>
* <span data-ttu-id="c9aa9-109">Nagy adatkészletek tárolása és lekérdezése</span><span class="sxs-lookup"><span data-stu-id="c9aa9-109">Store and query large data sets</span></span>
* <span data-ttu-id="c9aa9-110">Valós idejű és előzményadatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="c9aa9-110">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="c9aa9-111">Integrálás irodai háttérrendszerekkel</span><span class="sxs-lookup"><span data-stu-id="c9aa9-111">Integrate with back-office systems</span></span>

<span data-ttu-id="c9aa9-112">Ezek a képességek, Azure IoT Suite csomagok együtt toodeliver több Azure szolgáltatások egyéni kiterjesztéseket tartalmazó előkonfigurált megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-112">toodeliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as preconfigured solutions.</span></span> <span data-ttu-id="c9aa9-113">Ezek előkonfigurált megoldásokat olyan alap megvalósítások annak a közös IoT megoldást, amelyek segítenek tooreduce hello időt, toodeliver az IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-113">These preconfigured solutions are base implementations of common IoT solution patterns that help tooreduce hello time you take toodeliver your IoT solutions.</span></span> <span data-ttu-id="c9aa9-114">Hello IoT szoftverfejlesztői készletek használ, testre szabhatja és a megoldások toomeet kiterjesztése a saját követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-114">Using hello IoT software development kits, you can customize and extend these solutions toomeet your own requirements.</span></span> <span data-ttu-id="c9aa9-115">Ezeket a megoldásokat példaként vagy sablonként is használhatja az új IoT-megoldások fejlesztésekor.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="c9aa9-116">hello Azure IoT suite egy hatékony megoldás, az IoT-igényeinek.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-116">hello Azure IoT suite is a powerful solution for your IoT needs.</span></span> <span data-ttu-id="c9aa9-117">Célszerű upmost fontos, hogy az IoT-megoldások a biztonságot szem előtt tartva hello indítás készültek.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-117">However, it’s of upmost importance that your IoT solutions are designed with security in mind from hello start.</span></span> <span data-ttu-id="c9aa9-118">Az IoT-eszközök számát puszta hello, mert minden biztonsági esemény vehet a széles körű esemény jelentős következményekkel.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-118">Because of hello sheer number of IoT devices, any security incident can quickly become a widespread event with significant consequences.</span></span>

<span data-ttu-id="c9aa9-119">tisztában toohelp hogyan toosecure hello a következő információkat kell az IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-119">toohelp you understand how toosecure your IoT solutions, we have hello following information.</span></span>

## <a name="security-architecture"></a><span data-ttu-id="c9aa9-120">Biztonsági architektúra</span><span class="sxs-lookup"><span data-stu-id="c9aa9-120">Security architecture</span></span>
<span data-ttu-id="c9aa9-121">A rendszer tervezésekor fontos toounderstand hello potenciális fenyegetések toothat rendszer, és adjon hozzá megfelelő védelmekkel ennek megfelelően módon hello rendszer terveztek és tervezett.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-121">When designing a system, it is important toounderstand hello potential threats toothat system, and add appropriate defenses accordingly, as hello system is designed and architected.</span></span> <span data-ttu-id="c9aa9-122">Fontos toodesign hello hello start terméket a biztonságot szem előtt tartva, mert megértése, hogyan lehet egy támadó, képes toocompromise a rendszer segít, gondoskodjon arról, hogy megfelelő megoldást hello elejétől helyen.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-122">It is important toodesign hello product from hello start with security in mind because understanding how an attacker might be able toocompromise a system helps make sure appropriate mitigations are in place from hello beginning.</span></span>

<span data-ttu-id="c9aa9-123">Megismerheti az IoT-biztonsági architektúrája olvasásával [Internet a dolgok biztonsági architektúrája](../iot-suite/iot-security-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="c9aa9-123">You can learn about IoT security architecture by reading [Internet of Things Security Architecture](../iot-suite/iot-security-architecture.md).</span></span>

<span data-ttu-id="c9aa9-124">Ez a cikk ismerteti, amelyek a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="c9aa9-124">This article discusses hello following topics:</span></span>

* [<span data-ttu-id="c9aa9-125">A fenyegetések modellezése biztonsági kezdődik</span><span class="sxs-lookup"><span data-stu-id="c9aa9-125">Security Starts with a Threat Model</span></span>](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [<span data-ttu-id="c9aa9-126">Az IoT biztonsági</span><span class="sxs-lookup"><span data-stu-id="c9aa9-126">Security in IoT</span></span>](../iot-suite/iot-security-architecture.md#security-in-iot)
* [<span data-ttu-id="c9aa9-127">A fenyegetés modellezési hello Azure IoT-Referenciaarchitektúra</span><span class="sxs-lookup"><span data-stu-id="c9aa9-127">Threat Modeling hello Azure IoT Reference Architecture</span></span>](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-hello-ground-up"></a><span data-ttu-id="c9aa9-128">Biztonsági szabad hello a</span><span class="sxs-lookup"><span data-stu-id="c9aa9-128">Security from hello ground up</span></span>
<span data-ttu-id="c9aa9-129">hello IoT kockázatot egyedi biztonsági, adatvédelmi és megfelelőségi kihívást toobusinesses világszerte.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-129">hello IoT poses unique security, privacy, and compliance challenges toobusinesses worldwide.</span></span> <span data-ttu-id="c9aa9-130">Hagyományos számítógépes technológia, ahol a problémák szoftver, és hogyan megvalósított köré szerveződik, eltérően IoT vonatkozik, mi történik, ha hello számítógépes és hello fizikai világot átszervezését.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-130">Unlike traditional cyber technology where these issues revolve around software and how it is implemented, IoT concerns what happens when hello cyber and hello physical worlds converge.</span></span> <span data-ttu-id="c9aa9-131">Az IoT-megoldások védelmének biztosítása az eszközök, az adott eszközöket és a hello felhő és a biztonságos adatvédelem hello felhőben feldolgozás és a tárolás során közötti biztonságos kapcsolat biztonságos kiépítése igényel.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-131">Protecting IoT solutions requires ensuring secure provisioning of devices, secure connectivity between these devices and hello cloud, and secure data protection in hello cloud during processing and storage.</span></span> <span data-ttu-id="c9aa9-132">Eszközök fejlesztésének ilyen funkció, azonban a következők: erőforrás-korlátozott eszközök, központi telepítések földrajzi eloszlása és megoldást belül számos eszközön.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-132">Working against such functionality, however, are resource-constrained devices, geographic distribution of deployments, and many devices within a solution.</span></span>

<span data-ttu-id="c9aa9-133">Áttekintheti, hogyan toohandle biztonsági olvasásával évi [az eszközök internetes hálózatát biztonsági szabad hello a](../iot-suite/securing-iot-ground-up.md).</span><span class="sxs-lookup"><span data-stu-id="c9aa9-133">You can learn how toohandle security in these areas by reading [Internet of Things security from hello ground up](../iot-suite/securing-iot-ground-up.md).</span></span>

<span data-ttu-id="c9aa9-134">hello cikkből hello a következő témaköröket:</span><span class="sxs-lookup"><span data-stu-id="c9aa9-134">hello article discusses hello following topics:</span></span>

* [<span data-ttu-id="c9aa9-135">Biztonságos infrastruktúra hello szabad</span><span class="sxs-lookup"><span data-stu-id="c9aa9-135">Secure infrastructure from hello ground up</span></span>](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [<span data-ttu-id="c9aa9-136">A Microsoft Azure – a vállalati biztonságos IoT-infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="c9aa9-136">Microsoft Azure – secure IoT infrastructure for your business</span></span>](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a><span data-ttu-id="c9aa9-137">Ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="c9aa9-137">Best Practices</span></span>
<span data-ttu-id="c9aa9-138">Az IoT-infrastruktúra védelmének biztosítása megköveteli a szigorú jellegű biztonsági stratégia.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-138">Securing an IoT infrastructure requires a rigorous security-in-depth strategy.</span></span> <span data-ttu-id="c9aa9-139">Biztonságossá tétele a felhasználók az adatok integritását, miközben átvitel közben keresztüli hello nyilvános internet, toosecurely létesítési eszközök, az egyes rétegekhez nagyobb biztonság alkot hello felhőben adatok hello átfogó infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="c9aa9-139">From securing data in hello cloud, protecting data integrity while in transit over hello public internet, toosecurely provisioning devices, each layer builds greater security assurance in hello overall infrastructure.</span></span>

<span data-ttu-id="c9aa9-140">Megismerheti az eszközök internetes hálózatát biztonsági gyakorlati tanácsok olvasásával [az eszközök internetes hálózatát ajánlott biztonsági eljárások](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="c9aa9-140">You can learn about Internet of Things security best practices by reading [Internet of Things security best practices](../iot-suite/iot-security-best-practices.md).</span></span>

<span data-ttu-id="c9aa9-141">hello cikkből hello a következő témaköröket:</span><span class="sxs-lookup"><span data-stu-id="c9aa9-141">hello article discusses hello following topics:</span></span>

* [<span data-ttu-id="c9aa9-142">Az IoT hardver gyártója/integráló</span><span class="sxs-lookup"><span data-stu-id="c9aa9-142">IoT hardware manufacturer/integrator</span></span>](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [<span data-ttu-id="c9aa9-143">Az IoT-megoldás fejlesztői</span><span class="sxs-lookup"><span data-stu-id="c9aa9-143">IoT solution developer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [<span data-ttu-id="c9aa9-144">Az IoT-megoldás deployer</span><span class="sxs-lookup"><span data-stu-id="c9aa9-144">IoT solution deployer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [<span data-ttu-id="c9aa9-145">Az IoT-megoldás operátor</span><span class="sxs-lookup"><span data-stu-id="c9aa9-145">IoT solution operator</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
