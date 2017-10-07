---
title: "a dolgok ajánlott biztonsági eljárások aaaInternet |} Microsoft Docs"
description: "hello a cikk a Microsoft Internet dolgot ajánlott biztonsági eljárások és általános javaslatok válogatott listáját tartalmazza."
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
ms.openlocfilehash: 7ee31c912e8ac230ffa5efcd5b4c2b0b0713584f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-best-practices"></a><span data-ttu-id="78687-103">Eszközök internetes dolgot ajánlott biztonsági eljárások</span><span class="sxs-lookup"><span data-stu-id="78687-103">Internet of Things Security Best Practices</span></span>
<span data-ttu-id="78687-104">Rendelkezésre áll egy kritikus vállalkozás bárki vállalniuk az IoT-megoldások biztonságossá tétele hello az eszközök internetes hálózatát (IoT) infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="78687-104">Securing hello Internet of Things (IoT) infrastructure is a critical undertaking for anyone involved with IoT solutions.</span></span> <span data-ttu-id="78687-105">Hello érintett eszközök száma és hello miatt ezeken az eszközökön hello hatás biztonsági esemény elosztott jellege kapcsolatos, az IoT-eszközök millióira toocompromise nem triviális, és a széleskörű hatással lehet.</span><span class="sxs-lookup"><span data-stu-id="78687-105">Because of hello number of devices involved and hello distributed nature of these devices, hello impact a security event related toocompromise of millions of IoT devices is non-trivial and can have widespread impact.</span></span>

<span data-ttu-id="78687-106">Emiatt az IoT biztonsági kell a biztonsági jellegű megközelítést.</span><span class="sxs-lookup"><span data-stu-id="78687-106">For this reason, IoT security needs a security-in-depth approach.</span></span> <span data-ttu-id="78687-107">Adatok igényeinek toobe biztonságos hello felhőben és, mert privát és nyilvános hálózatokon helyezi át.</span><span class="sxs-lookup"><span data-stu-id="78687-107">Data needs toobe secure in hello cloud and as it moves over private and public networks.</span></span> <span data-ttu-id="78687-108">Metódusok hívására van szükség, hogy a hely toosecurely rendelkezés hello az IoT-eszközök maguk toobe.</span><span class="sxs-lookup"><span data-stu-id="78687-108">Methods need toobe in place toosecurely provision hello IoT devices themselves.</span></span> <span data-ttu-id="78687-109">Minden egyes rétegben, eszköz, toonetwork, toocloud háttér-erős biztonságot biztosítékok kell.</span><span class="sxs-lookup"><span data-stu-id="78687-109">Each layer, from device, toonetwork, toocloud back-end needs strong security assurances.</span></span>

<span data-ttu-id="78687-110">Az IoT-ajánlott eljárások a hello a következő módon csoportosíthatók:</span><span class="sxs-lookup"><span data-stu-id="78687-110">IoT best practices can be categorized in hello following way:</span></span>

* <span data-ttu-id="78687-111">Az IoT-hardvergyártó vagy integráló</span><span class="sxs-lookup"><span data-stu-id="78687-111">IoT hardware manufacturer or integrator</span></span>
* <span data-ttu-id="78687-112">Az IoT-megoldás fejlesztői</span><span class="sxs-lookup"><span data-stu-id="78687-112">IoT solution developer</span></span>
* <span data-ttu-id="78687-113">Az IoT-megoldás deployer</span><span class="sxs-lookup"><span data-stu-id="78687-113">IoT solution deployer</span></span>
* <span data-ttu-id="78687-114">Az IoT-megoldás operátor</span><span class="sxs-lookup"><span data-stu-id="78687-114">IoT solution operator</span></span>

<span data-ttu-id="78687-115">Ez a cikk összefoglalja [Internet a dolgok ajánlott biztonsági eljárások](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="78687-115">This article summarizes [Internet of Things Security Best Practices](../iot-suite/iot-security-best-practices.md).</span></span> <span data-ttu-id="78687-116">További részletes információt toothat cikkben tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="78687-116">Please refer toothat article for more detailed information.</span></span>

## <a name="iot-hardware-manufacturer-or-integrator"></a><span data-ttu-id="78687-117">Az IoT-hardvergyártó vagy integráló</span><span class="sxs-lookup"><span data-stu-id="78687-117">IoT hardware manufacturer or integrator</span></span>
<span data-ttu-id="78687-118">Kövesse az alábbi gyakorlati tanácsok hello egy IoT hardver gyártás vagy egy hardveres integráló:</span><span class="sxs-lookup"><span data-stu-id="78687-118">Follow hello best practices below if you are an IoT hardware manufacture or a hardware integrator:</span></span>

* <span data-ttu-id="78687-119">**Hatókör toominimum hardverkövetelmények**: hello hardver tervezési hello hardver, és semmi további művelethez szükséges minimális funkciók kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="78687-119">**Scope hardware toominimum requirements**: hello hardware design should include minimum features required for operation of hello hardware, and nothing more.</span></span> 
* <span data-ttu-id="78687-120">**Ellenőrizze a hardver igazolása védelmet**: build a mechanizmusok toodetect fizikai illetéktelen módosításának hardverek, például a megnyitása hello eszköz borítóján eltávolítása egy részét hello eszköz stb.</span><span class="sxs-lookup"><span data-stu-id="78687-120">**Make hardware tamper proof**: build in mechanisms toodetect physical tampering of hardware, such as opening hello device cover, removing a part of hello device, etc.</span></span> 
* <span data-ttu-id="78687-121">**Biztonságos hardveren körül Build**: Ha [ELÁBÉ](https://en.wikipedia.org/wiki/Cost_of_goods_sold) lehetővé teszik, készíthetnek biztonsági funkciókat, például a biztonságos és titkosított tárolási és a platformmegbízhatósági modul TPM-alapú rendszerindítási funkcióit.</span><span class="sxs-lookup"><span data-stu-id="78687-121">**Build around secure hardware**: if [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) permit, build security features such as secure and encrypted storage and Trusted Platform Module (TPM)-based boot functionality.</span></span>
* <span data-ttu-id="78687-122">**Frissítések biztonságosabbá teheti**: előbb vagy utóbb elkerülhetetlenné belső vezérlőprogram frissítése hello eszköz élettartama alatt.</span><span class="sxs-lookup"><span data-stu-id="78687-122">**Make upgrades secure**: upgrading firmware during lifetime of hello device is inevitable.</span></span>

## <a name="iot-solution-developer"></a><span data-ttu-id="78687-123">Az IoT-megoldás fejlesztői</span><span class="sxs-lookup"><span data-stu-id="78687-123">IoT solution developer</span></span>
<span data-ttu-id="78687-124">Kövesse az alábbi gyakorlati tanácsok hello egy IoT-megoldás fejlesztők:</span><span class="sxs-lookup"><span data-stu-id="78687-124">Follow hello best practices below if you are an IoT solution developer:</span></span>

* <span data-ttu-id="78687-125">**Hajtsa végre a biztonságos software development módszert**: biztonságos szoftver fejlesztése ground up gondolat biztonsági hello kezdetektől hello projekt összes hello módon tooits végrehajtására, tesztelése és telepítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="78687-125">**Follow secure software development methodology**: developing secure software requires ground-up thinking about security from hello inception of hello project all hello way tooits implementation, testing, and deployment.</span></span>
* <span data-ttu-id="78687-126">**A kiválasztott nyílt forráskódú szoftver körültekintően**: nyílt forráskódú szoftvereket lehetőséget kínál a tooquickly megoldások fejlesztése.</span><span class="sxs-lookup"><span data-stu-id="78687-126">**Choose open source software with care**: open source software provides an opportunity tooquickly develop solutions.</span></span>
* <span data-ttu-id="78687-127">**Integrálható, így gondossággal járjanak**: hello szoftver biztonsági hiányosságokat számos hello határt a könyvtárakat és API-k lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="78687-127">**Integrate with care**: many of hello software security flaws exist at hello boundary of libraries and APIs.</span></span> 

## <a name="iot-solution-deployer"></a><span data-ttu-id="78687-128">Az IoT-megoldás deployer</span><span class="sxs-lookup"><span data-stu-id="78687-128">IoT solution deployer</span></span>
<span data-ttu-id="78687-129">Ha Ön egy IoT-megoldás deployer, kövesse a hello ajánlott eljárásokat az alábbi:</span><span class="sxs-lookup"><span data-stu-id="78687-129">Follow hello best practices below if you are an IoT solution deployer:</span></span>

* <span data-ttu-id="78687-130">**Hardver biztonságos telepítése**: IoT-telepítések nem biztonságos helyen, például a nyilvános szóközt vagy felügyeletlen területi telepített hardver toobe lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="78687-130">**Deploy hardware securely**: IoT deployments may require hardware toobe deployed in unsecure locations, such as in public spaces or unsupervised locales.</span></span>
* <span data-ttu-id="78687-131">**Hitelesítési kulcsok biztonsága**: a telepítés során minden eszköz eszközazonosítókat igényel, és társított hello felhőalapú szolgáltatás által létrehozott hitelesítési kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="78687-131">**Keep authentication keys safe**: during deployment, each device requires device IDs and associated authentication keys generated by hello cloud service.</span></span> <span data-ttu-id="78687-132">Tartsa meg ezeket a kulcsokat fizikailag biztonságos hello telepítés után is.</span><span class="sxs-lookup"><span data-stu-id="78687-132">Keep these keys physically safe even after hello deployment.</span></span> <span data-ttu-id="78687-133">Feltört kulcs egy meglévő eszközt egy rosszindulatú eszköz toomasquerade is használhatják.</span><span class="sxs-lookup"><span data-stu-id="78687-133">Any compromised key can be used by a malicious device toomasquerade as an existing device.</span></span>

## <a name="iot-solution-operator"></a><span data-ttu-id="78687-134">Az IoT-megoldás operátor</span><span class="sxs-lookup"><span data-stu-id="78687-134">IoT solution operator</span></span>
<span data-ttu-id="78687-135">Ha az IoT-megoldás kezelőként, kövesse a hello ajánlott eljárásokat az alábbi:</span><span class="sxs-lookup"><span data-stu-id="78687-135">Follow hello best practices below if you are an IoT solution operator:</span></span>

* <span data-ttu-id="78687-136">**Az egyes rendszerek toodate be**: Győződjön meg arról, eszköz-operációsrendszerek és az összes eszközillesztőt frissített toohello legújabb verziója.</span><span class="sxs-lookup"><span data-stu-id="78687-136">**Keep systems up toodate**: ensure device operating systems and all device drivers are updated toohello latest versions.</span></span> 
* <span data-ttu-id="78687-137">**Rosszindulatú tevékenységhez elleni**: Ha engedélyezi az hello operációs rendszer, helyezze hello legújabb víruskereső és kártevőirtó funkciókat minden eszköz operációs rendszere.</span><span class="sxs-lookup"><span data-stu-id="78687-137">**Protect against malicious activity**: if hello operating system permits, place hello latest anti-virus and anti-malware capabilities on each device operating system.</span></span> 
* <span data-ttu-id="78687-138">**Naplózási gyakran**: naplózás IoT infrastruktúra pedig biztonsági kapcsolatos problémák esetén kulcs toosecurity incidensek válaszol.</span><span class="sxs-lookup"><span data-stu-id="78687-138">**Audit frequently**: auditing IoT infrastructure for security related issues is key when responding toosecurity incidents.</span></span>
* <span data-ttu-id="78687-139">**Fizikailag a hello IoT-infrastruktúra védelméhez**: hello legrosszabb IoT infrastruktúra elleni támadások fizikai hozzáférés toodevices indul el.</span><span class="sxs-lookup"><span data-stu-id="78687-139">**Physically protect hello IoT infrastructure**: hello worst security attacks against IoT infrastructure are launched using physical access toodevices.</span></span>
* <span data-ttu-id="78687-140">**Felhő hitelesítő adatok védelme**: Felhő hitelesítő adatok konfigurálása és az IoT központi telepítése operációs valószínűleg hello legegyszerűbb módja toogain hozzáférés, és az IoT-rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="78687-140">**Protect cloud credentials**: cloud authentication credentials used for configuring and operating an IoT deployment are possibly hello easiest way toogain access and compromise an IoT system.</span></span> 

