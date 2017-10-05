---
title: "Dolgot ajánlott biztonsági eljárások az eszközök internetes |} Microsoft Docs"
description: "A cikk a Microsoft Internet dolgot ajánlott biztonsági eljárások és általános javaslatok válogatott listáját tartalmazza."
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
ms.openlocfilehash: 8efc0053458e338ac1afe98d9ce970c1d5cbfa81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="internet-of-things-security-best-practices"></a><span data-ttu-id="67348-103">Eszközök internetes dolgot ajánlott biztonsági eljárások</span><span class="sxs-lookup"><span data-stu-id="67348-103">Internet of Things Security Best Practices</span></span>
<span data-ttu-id="67348-104">A az eszközök internetes hálózatát (IoT) infrastruktúra védelmének biztosítása a felhasználók az IoT-megoldások szerepet játszó kritikus vállalkozás.</span><span class="sxs-lookup"><span data-stu-id="67348-104">Securing the Internet of Things (IoT) infrastructure is a critical undertaking for anyone involved with IoT solutions.</span></span> <span data-ttu-id="67348-105">Az érintett eszközök száma és az elosztott jellege ezeket az eszközöket biztonsági esemény kapcsolatos hibát okoz az IoT-eszközök millióira hatása nem triviális, és széleskörű hatással lehet.</span><span class="sxs-lookup"><span data-stu-id="67348-105">Because of the number of devices involved and the distributed nature of these devices, the impact a security event related to compromise of millions of IoT devices is non-trivial and can have widespread impact.</span></span>

<span data-ttu-id="67348-106">Emiatt az IoT biztonsági kell a biztonsági jellegű megközelítést.</span><span class="sxs-lookup"><span data-stu-id="67348-106">For this reason, IoT security needs a security-in-depth approach.</span></span> <span data-ttu-id="67348-107">Biztonságos kell a felhőben és a privát és nyilvános hálózatokon átvitel során.</span><span class="sxs-lookup"><span data-stu-id="67348-107">Data needs to be secure in the cloud and as it moves over private and public networks.</span></span> <span data-ttu-id="67348-108">Módszerek kell, hogy magukat az IoT-eszközök biztonságosan kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="67348-108">Methods need to be in place to securely provision the IoT devices themselves.</span></span> <span data-ttu-id="67348-109">Minden egyes rétegben, eszköz, a hálózathoz, a háttér-felhőbe erős biztonságot biztosítékok kell.</span><span class="sxs-lookup"><span data-stu-id="67348-109">Each layer, from device, to network, to cloud back-end needs strong security assurances.</span></span>

<span data-ttu-id="67348-110">Az IoT-ajánlott eljárások a következő módon csoportosíthatók:</span><span class="sxs-lookup"><span data-stu-id="67348-110">IoT best practices can be categorized in the following way:</span></span>

* <span data-ttu-id="67348-111">Az IoT-hardvergyártó vagy integráló</span><span class="sxs-lookup"><span data-stu-id="67348-111">IoT hardware manufacturer or integrator</span></span>
* <span data-ttu-id="67348-112">Az IoT-megoldás fejlesztői</span><span class="sxs-lookup"><span data-stu-id="67348-112">IoT solution developer</span></span>
* <span data-ttu-id="67348-113">Az IoT-megoldás deployer</span><span class="sxs-lookup"><span data-stu-id="67348-113">IoT solution deployer</span></span>
* <span data-ttu-id="67348-114">Az IoT-megoldás operátor</span><span class="sxs-lookup"><span data-stu-id="67348-114">IoT solution operator</span></span>

<span data-ttu-id="67348-115">Ez a cikk összefoglalja [Internet a dolgok ajánlott biztonsági eljárások](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="67348-115">This article summarizes [Internet of Things Security Best Practices](../iot-suite/iot-security-best-practices.md).</span></span> <span data-ttu-id="67348-116">Tekintse meg az adott cikkhez részletes információt.</span><span class="sxs-lookup"><span data-stu-id="67348-116">Please refer to that article for more detailed information.</span></span>

## <a name="iot-hardware-manufacturer-or-integrator"></a><span data-ttu-id="67348-117">Az IoT-hardvergyártó vagy integráló</span><span class="sxs-lookup"><span data-stu-id="67348-117">IoT hardware manufacturer or integrator</span></span>
<span data-ttu-id="67348-118">Ha Ön egy IoT hardver gyártás vagy egy hardveres integráló, kövesse az alábbi gyakorlati tanácsokra:</span><span class="sxs-lookup"><span data-stu-id="67348-118">Follow the best practices below if you are an IoT hardware manufacture or a hardware integrator:</span></span>

* <span data-ttu-id="67348-119">**Hatókör hardvereszközeit úgy, hogy a minimális követelmények**: a hardver tervezési tartalmaznia kell a hardvert, és semmi további művelethez szükséges minimális funkciók.</span><span class="sxs-lookup"><span data-stu-id="67348-119">**Scope hardware to minimum requirements**: the hardware design should include minimum features required for operation of the hardware, and nothing more.</span></span> 
* <span data-ttu-id="67348-120">**Ellenőrizze a hardver igazolása védelmet**: build a mechanizmusok észleléséhez fizikai illetéktelen módosításának hardverek, például a megnyitása az eszköz borítóján eltávolítása egy részét az adott eszköz feloldásához, stb.</span><span class="sxs-lookup"><span data-stu-id="67348-120">**Make hardware tamper proof**: build in mechanisms to detect physical tampering of hardware, such as opening the device cover, removing a part of the device, etc.</span></span> 
* <span data-ttu-id="67348-121">**Biztonságos hardveren körül Build**: Ha [ELÁBÉ](https://en.wikipedia.org/wiki/Cost_of_goods_sold) lehetővé teszik, készíthetnek biztonsági funkciókat, például a biztonságos és titkosított tárolási és a platformmegbízhatósági modul TPM-alapú rendszerindítási funkcióit.</span><span class="sxs-lookup"><span data-stu-id="67348-121">**Build around secure hardware**: if [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) permit, build security features such as secure and encrypted storage and Trusted Platform Module (TPM)-based boot functionality.</span></span>
* <span data-ttu-id="67348-122">**Frissítések biztonságosabbá teheti**: előbb vagy utóbb elkerülhetetlenné belső vezérlőprogram frissítése az eszköz élettartama alatt.</span><span class="sxs-lookup"><span data-stu-id="67348-122">**Make upgrades secure**: upgrading firmware during lifetime of the device is inevitable.</span></span>

## <a name="iot-solution-developer"></a><span data-ttu-id="67348-123">Az IoT-megoldás fejlesztői</span><span class="sxs-lookup"><span data-stu-id="67348-123">IoT solution developer</span></span>
<span data-ttu-id="67348-124">Az IoT-megoldás fejlesztők, kövesse az alábbi gyakorlati tanácsokat:</span><span class="sxs-lookup"><span data-stu-id="67348-124">Follow the best practices below if you are an IoT solution developer:</span></span>

* <span data-ttu-id="67348-125">**Hajtsa végre a biztonságos software development módszert**: ground felfelé továbbléphetnek a kezdetektől a projekt a biztonság egészen a végrehajtási, a tesztelés és a telepítése a biztonságos szoftver fejlesztése igényel.</span><span class="sxs-lookup"><span data-stu-id="67348-125">**Follow secure software development methodology**: developing secure software requires ground-up thinking about security from the inception of the project all the way to its implementation, testing, and deployment.</span></span>
* <span data-ttu-id="67348-126">**A kiválasztott nyílt forráskódú szoftver körültekintően**: gyorsan a megoldások fejlesztése lehetőséget kínál a nyílt forráskódú szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="67348-126">**Choose open source software with care**: open source software provides an opportunity to quickly develop solutions.</span></span>
* <span data-ttu-id="67348-127">**Integrálható, így gondossággal járjanak**: a szoftver biztonsági hiányosságokat számos lehet létrehozni a határ könyvtárakat és API-k.</span><span class="sxs-lookup"><span data-stu-id="67348-127">**Integrate with care**: many of the software security flaws exist at the boundary of libraries and APIs.</span></span> 

## <a name="iot-solution-deployer"></a><span data-ttu-id="67348-128">Az IoT-megoldás deployer</span><span class="sxs-lookup"><span data-stu-id="67348-128">IoT solution deployer</span></span>
<span data-ttu-id="67348-129">Ha Ön egy IoT-megoldás deployer, kövesse az alábbi gyakorlati tanácsokat:</span><span class="sxs-lookup"><span data-stu-id="67348-129">Follow the best practices below if you are an IoT solution deployer:</span></span>

* <span data-ttu-id="67348-130">**Hardver biztonságos telepítése**: IoT központi telepítések szükség lehet a hardvert, hogy nem biztonságos helyen, például a nyilvános szóközt vagy felügyeletlen területi telepíthető.</span><span class="sxs-lookup"><span data-stu-id="67348-130">**Deploy hardware securely**: IoT deployments may require hardware to be deployed in unsecure locations, such as in public spaces or unsupervised locales.</span></span>
* <span data-ttu-id="67348-131">**Hitelesítési kulcsok biztonsága**: a telepítés során minden egyes eszköz eszközazonosítókat igényel, és hozzárendelt hitelesítési kulcsokat, a felhőalapú szolgáltatás által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="67348-131">**Keep authentication keys safe**: during deployment, each device requires device IDs and associated authentication keys generated by the cloud service.</span></span> <span data-ttu-id="67348-132">Ezek a kulcsok fizikailag biztonságos tartsa a telepítés után is.</span><span class="sxs-lookup"><span data-stu-id="67348-132">Keep these keys physically safe even after the deployment.</span></span> <span data-ttu-id="67348-133">Feltört kulcs segítségével egy rosszindulatú eszköz helyettesítő meglévő eszközként.</span><span class="sxs-lookup"><span data-stu-id="67348-133">Any compromised key can be used by a malicious device to masquerade as an existing device.</span></span>

## <a name="iot-solution-operator"></a><span data-ttu-id="67348-134">Az IoT-megoldás operátor</span><span class="sxs-lookup"><span data-stu-id="67348-134">IoT solution operator</span></span>
<span data-ttu-id="67348-135">Ha IoT-megoldás kezelőként, kövesse az alábbi gyakorlati tanácsokat:</span><span class="sxs-lookup"><span data-stu-id="67348-135">Follow the best practices below if you are an IoT solution operator:</span></span>

* <span data-ttu-id="67348-136">**Naprakészen rendszerek**: Győződjön meg arról, eszköz-operációsrendszerek és az összes eszközillesztőt frissítése a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="67348-136">**Keep systems up to date**: ensure device operating systems and all device drivers are updated to the latest versions.</span></span> 
* <span data-ttu-id="67348-137">**Rosszindulatú tevékenységhez elleni**: lehetővé teszi az operációs rendszer, ha minden eszköz operációs rendszere helyezze a legújabb kártevőirtó- és víruskereső-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="67348-137">**Protect against malicious activity**: if the operating system permits, place the latest anti-virus and anti-malware capabilities on each device operating system.</span></span> 
* <span data-ttu-id="67348-138">**Naplózási gyakran**: naplózás IoT infrastruktúra pedig biztonsági kapcsolatos problémák esetén kulcs-biztonsági incidensekre válaszol.</span><span class="sxs-lookup"><span data-stu-id="67348-138">**Audit frequently**: auditing IoT infrastructure for security related issues is key when responding to security incidents.</span></span>
* <span data-ttu-id="67348-139">**Az IoT-infrastruktúra fizikailag védelme**: a legrosszabb biztonsági támadások ellen IoT infrastruktúra fizikai hozzáférés eszközökhöz indul el.</span><span class="sxs-lookup"><span data-stu-id="67348-139">**Physically protect the IoT infrastructure**: the worst security attacks against IoT infrastructure are launched using physical access to devices.</span></span>
* <span data-ttu-id="67348-140">**Felhő hitelesítő adatok védelme**: Felhő hitelesítő adatok konfigurálása és az IoT központi telepítése operációs rendszer valószínűleg a hozzáférést és legegyszerűbben hibát okoz az IoT.</span><span class="sxs-lookup"><span data-stu-id="67348-140">**Protect cloud credentials**: cloud authentication credentials used for configuring and operating an IoT deployment are possibly the easiest way to gain access and compromise an IoT system.</span></span> 

