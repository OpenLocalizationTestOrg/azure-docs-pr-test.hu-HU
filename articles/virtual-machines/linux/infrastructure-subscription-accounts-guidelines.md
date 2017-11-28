---
title: "aaaSubscription és a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Tudnivalók: hello legfontosabb tervezési és megvalósítási előfizetések és a fiókok az Azure-on."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19343826-7eef-42a1-98be-4ec65b0f377a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9025a40783c008310ebd0f674deb4a9001ae974a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a><span data-ttu-id="2aca5-103">Linux virtuális gépek Azure előfizetés és a fiókok irányelvek</span><span class="sxs-lookup"><span data-stu-id="2aca5-103">Azure subscription and accounts guidelines for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="2aca5-104">Ez a cikk foglalkozik a megértése, hogyan tooapproach előfizetés és a fiókok kezelése, a környezet és a felhasználói bázis növekszik.</span><span class="sxs-lookup"><span data-stu-id="2aca5-104">This article focuses on understanding how tooapproach subscription and account management as your environment and user base grows.</span></span>

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a><span data-ttu-id="2aca5-105">Implementációs segédlet előfizetések és-fiókok</span><span class="sxs-lookup"><span data-stu-id="2aca5-105">Implementation guidelines for subscriptions and accounts</span></span>
<span data-ttu-id="2aca5-106">Döntéseket:</span><span class="sxs-lookup"><span data-stu-id="2aca5-106">Decisions:</span></span>

* <span data-ttu-id="2aca5-107">Milyen set előfizetések és a fiókok van szüksége toohost az informatikai munkaterhelés vagy az infrastruktúra?</span><span class="sxs-lookup"><span data-stu-id="2aca5-107">What set of subscriptions and accounts do you need toohost your IT workload or infrastructure?</span></span>
* <span data-ttu-id="2aca5-108">Hogyan toobreak hello hierarchia toofit le a szervezet?</span><span class="sxs-lookup"><span data-stu-id="2aca5-108">How toobreak down hello hierarchy toofit your organization?</span></span>

<span data-ttu-id="2aca5-109">Feladatok:</span><span class="sxs-lookup"><span data-stu-id="2aca5-109">Tasks:</span></span>

* <span data-ttu-id="2aca5-110">Adja meg a logikai szervezeti hierarchia szerint szeretné toomanage azt egy előfizetés szintjéről.</span><span class="sxs-lookup"><span data-stu-id="2aca5-110">Define your logical organization hierarchy as you would like toomanage it from a subscription level.</span></span>
* <span data-ttu-id="2aca5-111">toomatch ezt a logikai hierarchiát, adja meg a szükséges hello fiókjaihoz és előfizetéseihez minden egyes fiókkal.</span><span class="sxs-lookup"><span data-stu-id="2aca5-111">toomatch this logical hierarchy, define hello accounts required and subscriptions under each account.</span></span>
* <span data-ttu-id="2aca5-112">Hozzon létre hello előfizetések és az elnevezési fiókokat.</span><span class="sxs-lookup"><span data-stu-id="2aca5-112">Create hello set of subscriptions and accounts using your naming convention.</span></span>

## <a name="subscriptions-and-accounts"></a><span data-ttu-id="2aca5-113">Előfizetések és fiókok</span><span class="sxs-lookup"><span data-stu-id="2aca5-113">Subscriptions and accounts</span></span>
<span data-ttu-id="2aca5-114">az Azure-ral toowork, szüksége van egy vagy több Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="2aca5-114">toowork with Azure, you need one or more Azure subscriptions.</span></span> <span data-ttu-id="2aca5-115">Erőforrások – például virtuális gépek (VM), illetve ezek előfizetések szerepel virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="2aca5-115">Resources like virtual machines (VMs) or virtual networks exist in of those subscriptions.</span></span>

* <span data-ttu-id="2aca5-116">A vállalati ügyfelek a vállalati beléptetési, amely hello childwindow-erőforrás hello hierarchiában, és társított tooone vagy további fiókok általában rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="2aca5-116">Enterprise customers typically have an Enterprise Enrollment, which is hello top-most resource in hello hierarchy, and is associated tooone or more accounts.</span></span>
* <span data-ttu-id="2aca5-117">A felhasználók és az ügyfelek a vállalati beléptetési nélkül a hello childwindow-erőforrás az hello fiók.</span><span class="sxs-lookup"><span data-stu-id="2aca5-117">For consumers and customers without an Enterprise Enrollment, hello top-most resource is hello account.</span></span>
* <span data-ttu-id="2aca5-118">Előfizetések társított tooaccounts, és egy vagy több előfizetés fiókonként is lehet.</span><span class="sxs-lookup"><span data-stu-id="2aca5-118">Subscriptions are associated tooaccounts, and there can be one or more subscriptions per account.</span></span> <span data-ttu-id="2aca5-119">Az Azure rekordok számlázási adatokat hello előfizetés szintjén.</span><span class="sxs-lookup"><span data-stu-id="2aca5-119">Azure records billing information at hello subscription level.</span></span>

<span data-ttu-id="2aca5-120">Lejáró hello/előfizetés kapcsolat két hierarchiaszintekkel toohello korlátot fontos tooalign hello elnevezési konvenciót kell számlázási fiókjaihoz és előfizetéseihez toohello a.</span><span class="sxs-lookup"><span data-stu-id="2aca5-120">Due toohello limit of two hierarchy levels on hello Account/Subscription relationship, it is important tooalign hello naming convention of accounts and subscriptions toohello billing needs.</span></span> <span data-ttu-id="2aca5-121">Például ha egy globális vállalat Azure használ, előfordulhat, hogy választ toohave egy fiók régiónként, és előfizetések felügyelt rendelkezik régió szint hello:</span><span class="sxs-lookup"><span data-stu-id="2aca5-121">For instance, if a global company uses Azure, they might choose toohave one account per region, and have subscriptions managed at hello region level:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

<span data-ttu-id="2aca5-122">Használhatja például a következő struktúra hello:</span><span class="sxs-lookup"><span data-stu-id="2aca5-122">For instance, you might use hello following structure:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

<span data-ttu-id="2aca5-123">Ha egy régió egyetlen előfizetéssel társított tooa adott csoport nagyobb toohave úgy dönt, hello elnevezési konvenciót kell tartalmaznia egy módon tooencode hello hello fiókkal vagy hello előfizetés nevét a további adatokat.</span><span class="sxs-lookup"><span data-stu-id="2aca5-123">If a region decides toohave more than one subscription associated tooa particular group, hello naming convention should incorporate a way tooencode hello extra data on either hello account or hello subscription name.</span></span> <span data-ttu-id="2aca5-124">A szervezet engedélyezi a számlázási toogenerate hello új szintek hierarchia massaging számlázási jelentések során:</span><span class="sxs-lookup"><span data-stu-id="2aca5-124">This organization allows massaging billing data toogenerate hello new levels of hierarchy during billing reports:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

<span data-ttu-id="2aca5-125">hello szervezet a következő példa hello nézhet:</span><span class="sxs-lookup"><span data-stu-id="2aca5-125">hello organization could look like hello following example:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

<span data-ttu-id="2aca5-126">A nagyvállalati szerződéssel nyújtunk részletes számlázási egy letölthető fájlt egy olyan fiók, vagy az összes fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="2aca5-126">We provide detailed billing via a downloadable file for a single account, or for all accounts in an enterprise agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2aca5-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2aca5-127">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

