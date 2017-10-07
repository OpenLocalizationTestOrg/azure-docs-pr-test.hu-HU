---
title: "aaaProtect személyes adatokat a Microsoft Azure |} Microsoft Docs"
description: "Először a következő cikket: cikkek toohelp sorozata használhatja az Azure tooprotect személyes adatok"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: cbffd3872552cbd0f12539535898c41ecf7789e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-in-microsoft-azure"></a><span data-ttu-id="83987-103">A Microsoft Azure-ban a személyes adatok védelme</span><span class="sxs-lookup"><span data-stu-id="83987-103">Protect personal data in Microsoft Azure</span></span>

<span data-ttu-id="83987-104">Ez a cikk bemutatja a cikkek, amelyek segítenek az Azure biztonsági technológiák és szolgáltatások tooprotect személyes adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="83987-104">This article introduces a series of articles that help you use Azure security technologies and services tooprotect personal data.</span></span> <span data-ttu-id="83987-105">Ez az a kulcsfontosságú követelmény az számos vállalati és iparági megfelelési és irányítási kezdeményezések.</span><span class="sxs-lookup"><span data-stu-id="83987-105">This is a key requirement for many corporate and industry compliance and governance initiatives.</span></span> <span data-ttu-id="83987-106">hello esetben probléma utasítás és a vállalati célok ide tartoznak.</span><span class="sxs-lookup"><span data-stu-id="83987-106">hello scenario, problem statement and company goals are included here.</span></span>

## <a name="scenario-and-problem-statement"></a><span data-ttu-id="83987-107">Forgatókönyv és probléma fényében</span><span class="sxs-lookup"><span data-stu-id="83987-107">Scenario and problem statement</span></span>

<span data-ttu-id="83987-108">A nagy körutazás vállalati telephelyének hello az Amerikai Egyesült Államokban, a műveletek toooffer útvonalak hello mediterrán, Adriai, és Balti tengerek, valamint hello Brit-szigetekre növekszik.</span><span class="sxs-lookup"><span data-stu-id="83987-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="83987-109">toosupport e erőfeszítéseket szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit</span><span class="sxs-lookup"><span data-stu-id="83987-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span>

<span data-ttu-id="83987-110">hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja.</span><span class="sxs-lookup"><span data-stu-id="83987-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="83987-111">Ide tartozhat alkalmazott és/vagy ügyféladatok, mint:</span><span class="sxs-lookup"><span data-stu-id="83987-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="83987-112">címek</span><span class="sxs-lookup"><span data-stu-id="83987-112">addresses</span></span>
- <span data-ttu-id="83987-113">telefonszámok</span><span class="sxs-lookup"><span data-stu-id="83987-113">phone numbers</span></span>
- <span data-ttu-id="83987-114">azonosító számokat</span><span class="sxs-lookup"><span data-stu-id="83987-114">tax identification numbers</span></span>
- <span data-ttu-id="83987-115">orvosi adatok</span><span class="sxs-lookup"><span data-stu-id="83987-115">medical information</span></span>
- <span data-ttu-id="83987-116">Hitelkártya-adatokhoz</span><span class="sxs-lookup"><span data-stu-id="83987-116">credit card information</span></span>

<span data-ttu-id="83987-117">hello vállalati kell védelmében hello az alkalmazottak és a felhasználói adatok szükség lenne rá adatok elérhető toothose osztályai tétele közben.</span><span class="sxs-lookup"><span data-stu-id="83987-117">hello company must protect hello privacy of employee and customer data while making data accessible toothose departments that need it.</span></span> <span data-ttu-id="83987-118">(például a bérszámfejtési és foglalások részlegek)</span><span class="sxs-lookup"><span data-stu-id="83987-118">(such as payroll and reservations departments)</span></span>

## <a name="company-goals"></a><span data-ttu-id="83987-119">Vállalati célok</span><span class="sxs-lookup"><span data-stu-id="83987-119">Company goals</span></span> 

- <span data-ttu-id="83987-120">Adatforrások, amelyek tartalmazzák a személyes adatok titkosítását, ha a felhőben található.</span><span class="sxs-lookup"><span data-stu-id="83987-120">Data sources that contain personal data are encrypted when residing in cloud storage.</span></span>

- <span data-ttu-id="83987-121">Egy hely tooanother a személyes adatok az átvitel során titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="83987-121">Personal data that is transferred from one location tooanother is encrypted while in-transit.</span></span> <span data-ttu-id="83987-122">Ez érvényét veszti, ha hello adatok áramlanak hello virtuális hálózaton vagy hello interneten keresztül hello vállalati adatközpontban és a hello Azure-felhő között.</span><span class="sxs-lookup"><span data-stu-id="83987-122">This is true if hello data is traveling across hello virtual network or across hello Internet between hello corporate datacenter and hello Azure cloud.</span></span>

- <span data-ttu-id="83987-123">Személyes adatok sértetlenségét és bizalmasságát védik a jogosulatlan hozzáférés erős Identitáskezelés és hozzáférés-vezérlési technológiák.</span><span class="sxs-lookup"><span data-stu-id="83987-123">Confidentiality and integrity of personal data is protected from unauthorized access by strong identity management and access control technologies.</span></span>

- <span data-ttu-id="83987-124">Személyes adatok védve van az adatok biztonsági szabályok megsértésére felügyelet biztonsági réseket és a fenyegetések kitettség.</span><span class="sxs-lookup"><span data-stu-id="83987-124">Personal data is protected from exposure via data breach via monitoring for vulnerabilities and threats.</span></span>

- <span data-ttu-id="83987-125">hello Azure-szolgáltatásokat, amely tárolja, és a személyes adatokat biztonsági állapotának értékelik tooidentify lehetőségek toobetter személyes adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="83987-125">hello security state of Azure services that store or transmit personal data is assessed tooidentify opportunities toobetter protect personal data.</span></span>

## <a name="data-protection-guidance"></a><span data-ttu-id="83987-126">Data protection útmutató</span><span class="sxs-lookup"><span data-stu-id="83987-126">Data protection guidance</span></span>

<span data-ttu-id="83987-127">a következő cikkek hello műszaki útmutató-tooguidance, amely segít a fent felsorolt hello személyes adatok védelem céljainak eléréséhez tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="83987-127">hello following articles contain technical how-tooguidance that will help you attain hello personal data protection goals listed above:</span></span>

- [<span data-ttu-id="83987-128">Az Azure titkosítási technológiák</span><span class="sxs-lookup"><span data-stu-id="83987-128">Azure encryption technologies</span></span>](protect-personal-data-at-rest.md)

- [<span data-ttu-id="83987-129">Az Azure titkosítási technológiák</span><span class="sxs-lookup"><span data-stu-id="83987-129">Azure encryption technologies</span></span>](protect-personal-data-in-transit-encryption.md)

- [<span data-ttu-id="83987-130">Az Azure identitások és hozzáférések technológiák</span><span class="sxs-lookup"><span data-stu-id="83987-130">Azure identity and access technologies</span></span>](protect-personal-data-identity-access-controls.md)

- [<span data-ttu-id="83987-131">Az Azure hálózati biztonsági technológiák</span><span class="sxs-lookup"><span data-stu-id="83987-131">Azure network security technologies</span></span>](protect-personal-data-network-security.md)

- [<span data-ttu-id="83987-132">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="83987-132">Azure Security Center</span></span>](protect-personal-data-azure-security-center.md)



## <a name="next-steps"></a><span data-ttu-id="83987-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83987-133">Next steps</span></span>

- [<span data-ttu-id="83987-134">Az Azure biztonsági információk hely</span><span class="sxs-lookup"><span data-stu-id="83987-134">Azure Security Information Site</span></span>](https://aka.ms/AzureSecInfo)

- [<span data-ttu-id="83987-135">A Microsoft biztonsági és adatkezelési központ</span><span class="sxs-lookup"><span data-stu-id="83987-135">Microsoft Trust Center</span></span>](https://www.microsoft.com/TrustCenter/default.aspx)

- [<span data-ttu-id="83987-136">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="83987-136">Azure Security Center</span></span>](https://azure.microsoft.com/services/security-center/)

- [<span data-ttu-id="83987-137">Az Azure Security csapat blogja</span><span class="sxs-lookup"><span data-stu-id="83987-137">Azure Security Team Blog</span></span>](https://www.azuresecurityorg)

- [<span data-ttu-id="83987-138">Azure.com webhelyre Blog - biztonsági</span><span class="sxs-lookup"><span data-stu-id="83987-138">Azure.com Blog - Security</span></span>](https://azure.microsoft.com/blog/topics/security/)
