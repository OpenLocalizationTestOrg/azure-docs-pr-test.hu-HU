---
title: "A Microsoft Azure-ban a személyes adatok védelme |} Microsoft Docs"
description: "Első cikk a cikkek segítséget a személyes adatok védelme az Azure használatával"
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
ms.openlocfilehash: dfb046374397c8a19672ce6b67741903fff6e178
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-in-microsoft-azure"></a><span data-ttu-id="8a04b-103">A Microsoft Azure-ban a személyes adatok védelme</span><span class="sxs-lookup"><span data-stu-id="8a04b-103">Protect personal data in Microsoft Azure</span></span>

<span data-ttu-id="8a04b-104">Ez a cikk be, amelyek segítenek cikkek személyes adatok védelme az Azure biztonsági technológiák és szolgáltatások segítségével.</span><span class="sxs-lookup"><span data-stu-id="8a04b-104">This article introduces a series of articles that help you use Azure security technologies and services to protect personal data.</span></span> <span data-ttu-id="8a04b-105">Ez az a kulcsfontosságú követelmény az számos vállalati és iparági megfelelési és irányítási kezdeményezések.</span><span class="sxs-lookup"><span data-stu-id="8a04b-105">This is a key requirement for many corporate and industry compliance and governance initiatives.</span></span> <span data-ttu-id="8a04b-106">A forgatókönyv probléma utasítás és a vállalati célok ide tartoznak.</span><span class="sxs-lookup"><span data-stu-id="8a04b-106">The scenario, problem statement and company goals are included here.</span></span>

## <a name="scenario-and-problem-statement"></a><span data-ttu-id="8a04b-107">Forgatókönyv és probléma fényében</span><span class="sxs-lookup"><span data-stu-id="8a04b-107">Scenario and problem statement</span></span>

<span data-ttu-id="8a04b-108">Egy nagy körutazás cég, az Amerikai Egyesült Államokban telephelyének bővíti a műveleteket a mediterrán, Adriai, és Balti tengerek, valamint a Brit-szigetekre útvonalak biztosítani.</span><span class="sxs-lookup"><span data-stu-id="8a04b-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="8a04b-109">Támogatás ezeket, több kisebb körutazás sorok Olaszország, Németországban, Dánia és az Egyesült szerzett</span><span class="sxs-lookup"><span data-stu-id="8a04b-109">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span>

<span data-ttu-id="8a04b-110">A vállalat a Microsoft Azure használ a felhőben tárolt vállalati adatokat.</span><span class="sxs-lookup"><span data-stu-id="8a04b-110">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="8a04b-111">Ide tartozhat alkalmazott és/vagy ügyféladatok, mint:</span><span class="sxs-lookup"><span data-stu-id="8a04b-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="8a04b-112">címek</span><span class="sxs-lookup"><span data-stu-id="8a04b-112">addresses</span></span>
- <span data-ttu-id="8a04b-113">telefonszámok</span><span class="sxs-lookup"><span data-stu-id="8a04b-113">phone numbers</span></span>
- <span data-ttu-id="8a04b-114">azonosító számokat</span><span class="sxs-lookup"><span data-stu-id="8a04b-114">tax identification numbers</span></span>
- <span data-ttu-id="8a04b-115">orvosi adatok</span><span class="sxs-lookup"><span data-stu-id="8a04b-115">medical information</span></span>
- <span data-ttu-id="8a04b-116">Hitelkártya-adatokhoz</span><span class="sxs-lookup"><span data-stu-id="8a04b-116">credit card information</span></span>

<span data-ttu-id="8a04b-117">A vállalati adatok azokat, hogy szükség lenne rá részlegek számára elérhető tétele közben kell védeni az alkalmazottak és a felhasználói adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="8a04b-117">The company must protect the privacy of employee and customer data while making data accessible to those departments that need it.</span></span> <span data-ttu-id="8a04b-118">(például a bérszámfejtési és foglalások részlegek)</span><span class="sxs-lookup"><span data-stu-id="8a04b-118">(such as payroll and reservations departments)</span></span>

## <a name="company-goals"></a><span data-ttu-id="8a04b-119">Vállalati célok</span><span class="sxs-lookup"><span data-stu-id="8a04b-119">Company goals</span></span> 

- <span data-ttu-id="8a04b-120">Adatforrások, amelyek tartalmazzák a személyes adatok titkosítását, ha a felhőben található.</span><span class="sxs-lookup"><span data-stu-id="8a04b-120">Data sources that contain personal data are encrypted when residing in cloud storage.</span></span>

- <span data-ttu-id="8a04b-121">Az átvitel során titkosítva van a személyes adatok egyik helyről egy másikra.</span><span class="sxs-lookup"><span data-stu-id="8a04b-121">Personal data that is transferred from one location to another is encrypted while in-transit.</span></span> <span data-ttu-id="8a04b-122">Ez érvényét veszti, ha az adatok áramlanak a virtuális hálózaton vagy az interneten keresztül a vállalati adatközpontban és az Azure felhőalapú között.</span><span class="sxs-lookup"><span data-stu-id="8a04b-122">This is true if the data is traveling across the virtual network or across the Internet between the corporate datacenter and the Azure cloud.</span></span>

- <span data-ttu-id="8a04b-123">Személyes adatok sértetlenségét és bizalmasságát védik a jogosulatlan hozzáférés erős Identitáskezelés és hozzáférés-vezérlési technológiák.</span><span class="sxs-lookup"><span data-stu-id="8a04b-123">Confidentiality and integrity of personal data is protected from unauthorized access by strong identity management and access control technologies.</span></span>

- <span data-ttu-id="8a04b-124">Személyes adatok védve van az adatok biztonsági szabályok megsértésére felügyelet biztonsági réseket és a fenyegetések kitettség.</span><span class="sxs-lookup"><span data-stu-id="8a04b-124">Personal data is protected from exposure via data breach via monitoring for vulnerabilities and threats.</span></span>

- <span data-ttu-id="8a04b-125">Biztonsági állapotát az Azure-szolgáltatásokat, amely tárolja, és a személyes adatok megfelelőségét ellenőrizni kell, hogy azonosítsa a finomhangolási lehetőségeket, amelyekkel jobban megvédheti a személyes adatok.</span><span class="sxs-lookup"><span data-stu-id="8a04b-125">The security state of Azure services that store or transmit personal data is assessed to identify opportunities to better protect personal data.</span></span>

## <a name="data-protection-guidance"></a><span data-ttu-id="8a04b-126">Data protection útmutató</span><span class="sxs-lookup"><span data-stu-id="8a04b-126">Data protection guidance</span></span>

<span data-ttu-id="8a04b-127">A következő cikkek műszaki útmutató útmutatást, amelyek segítségével a fent felsorolt személyes adatok védelem céljainak eléréséhez tartalmaznak:</span><span class="sxs-lookup"><span data-stu-id="8a04b-127">The following articles contain technical how-to guidance that will help you attain the personal data protection goals listed above:</span></span>

- [<span data-ttu-id="8a04b-128">Az Azure titkosítási technológiák</span><span class="sxs-lookup"><span data-stu-id="8a04b-128">Azure encryption technologies</span></span>](protect-personal-data-at-rest.md)

- [<span data-ttu-id="8a04b-129">Az Azure titkosítási technológiák</span><span class="sxs-lookup"><span data-stu-id="8a04b-129">Azure encryption technologies</span></span>](protect-personal-data-in-transit-encryption.md)

- [<span data-ttu-id="8a04b-130">Az Azure identitások és hozzáférések technológiák</span><span class="sxs-lookup"><span data-stu-id="8a04b-130">Azure identity and access technologies</span></span>](protect-personal-data-identity-access-controls.md)

- [<span data-ttu-id="8a04b-131">Az Azure hálózati biztonsági technológiák</span><span class="sxs-lookup"><span data-stu-id="8a04b-131">Azure network security technologies</span></span>](protect-personal-data-network-security.md)

- [<span data-ttu-id="8a04b-132">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="8a04b-132">Azure Security Center</span></span>](protect-personal-data-azure-security-center.md)



## <a name="next-steps"></a><span data-ttu-id="8a04b-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a04b-133">Next steps</span></span>

- [<span data-ttu-id="8a04b-134">Az Azure biztonsági információk hely</span><span class="sxs-lookup"><span data-stu-id="8a04b-134">Azure Security Information Site</span></span>](https://aka.ms/AzureSecInfo)

- [<span data-ttu-id="8a04b-135">A Microsoft biztonsági és adatkezelési központ</span><span class="sxs-lookup"><span data-stu-id="8a04b-135">Microsoft Trust Center</span></span>](https://www.microsoft.com/TrustCenter/default.aspx)

- [<span data-ttu-id="8a04b-136">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="8a04b-136">Azure Security Center</span></span>](https://azure.microsoft.com/services/security-center/)

- [<span data-ttu-id="8a04b-137">Az Azure Security csapat blogja</span><span class="sxs-lookup"><span data-stu-id="8a04b-137">Azure Security Team Blog</span></span>](https://www.azuresecurityorg)

- [<span data-ttu-id="8a04b-138">Azure.com webhelyre Blog - biztonsági</span><span class="sxs-lookup"><span data-stu-id="8a04b-138">Azure.com Blog - Security</span></span>](https://azure.microsoft.com/blog/topics/security/)
