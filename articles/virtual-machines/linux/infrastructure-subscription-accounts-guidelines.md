---
title: "Előfizetés és a Linux virtuális gépek az Azure-fiók |} Microsoft Docs"
description: "További tudnivalók a főbb tervezési és megvalósítási irányelvek előfizetések és az Azure-fiókok."
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
ms.openlocfilehash: 19695a9960d8e8f0dfca4bf0ca10761fe6ae7ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a><span data-ttu-id="96b22-103">Linux virtuális gépek Azure előfizetés és a fiókok irányelvek</span><span class="sxs-lookup"><span data-stu-id="96b22-103">Azure subscription and accounts guidelines for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="96b22-104">Ez a cikk foglalkozik az előfizetés és a fiókok kezelése, a környezet megközelítési alapos ismerete, és a felhasználói bázis növekszik.</span><span class="sxs-lookup"><span data-stu-id="96b22-104">This article focuses on understanding how to approach subscription and account management as your environment and user base grows.</span></span>

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a><span data-ttu-id="96b22-105">Implementációs segédlet előfizetések és-fiókok</span><span class="sxs-lookup"><span data-stu-id="96b22-105">Implementation guidelines for subscriptions and accounts</span></span>
<span data-ttu-id="96b22-106">Döntéseket:</span><span class="sxs-lookup"><span data-stu-id="96b22-106">Decisions:</span></span>

* <span data-ttu-id="96b22-107">Milyen előfizetések és fiókokat kell az informatikai munkaterhelés vagy az infrastruktúra tárolására beállított?</span><span class="sxs-lookup"><span data-stu-id="96b22-107">What set of subscriptions and accounts do you need to host your IT workload or infrastructure?</span></span>
* <span data-ttu-id="96b22-108">Hogyan szeretné bontani a hierarchiában a szervezet megfelelően?</span><span class="sxs-lookup"><span data-stu-id="96b22-108">How to break down the hierarchy to fit your organization?</span></span>

<span data-ttu-id="96b22-109">Feladatok:</span><span class="sxs-lookup"><span data-stu-id="96b22-109">Tasks:</span></span>

* <span data-ttu-id="96b22-110">Adja meg a logikai szervezeti hierarchia, szerint szeretné egy előfizetési szintről a kezeléshez.</span><span class="sxs-lookup"><span data-stu-id="96b22-110">Define your logical organization hierarchy as you would like to manage it from a subscription level.</span></span>
* <span data-ttu-id="96b22-111">Ezt a logikai hierarchiát megfeleltetéséhez szükséges fiókok és minden egyes fiókkal előfizetések megadása.</span><span class="sxs-lookup"><span data-stu-id="96b22-111">To match this logical hierarchy, define the accounts required and subscriptions under each account.</span></span>
* <span data-ttu-id="96b22-112">Hozzon létre az előfizetések és az elnevezési fiókokat.</span><span class="sxs-lookup"><span data-stu-id="96b22-112">Create the set of subscriptions and accounts using your naming convention.</span></span>

## <a name="subscriptions-and-accounts"></a><span data-ttu-id="96b22-113">Előfizetések és fiókok</span><span class="sxs-lookup"><span data-stu-id="96b22-113">Subscriptions and accounts</span></span>
<span data-ttu-id="96b22-114">Azure használatához szükség van egy vagy több Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="96b22-114">To work with Azure, you need one or more Azure subscriptions.</span></span> <span data-ttu-id="96b22-115">Erőforrások – például virtuális gépek (VM), illetve ezek előfizetések szerepel virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="96b22-115">Resources like virtual machines (VMs) or virtual networks exist in of those subscriptions.</span></span>

* <span data-ttu-id="96b22-116">A vállalati ügyfelek általában rendelkeznek a vállalati beléptetési, amely a childwindow-erőforrás a hierarchiában, és egy vagy több fiókot társítva.</span><span class="sxs-lookup"><span data-stu-id="96b22-116">Enterprise customers typically have an Enterprise Enrollment, which is the top-most resource in the hierarchy, and is associated to one or more accounts.</span></span>
* <span data-ttu-id="96b22-117">A felhasználók és az ügyfelek a vállalati beléptetési nélkül a childwindow-erőforrást az a fiók.</span><span class="sxs-lookup"><span data-stu-id="96b22-117">For consumers and customers without an Enterprise Enrollment, the top-most resource is the account.</span></span>
* <span data-ttu-id="96b22-118">Előfizetések tartoznak fiókok, és egy vagy több előfizetés fiókonként is lehet.</span><span class="sxs-lookup"><span data-stu-id="96b22-118">Subscriptions are associated to accounts, and there can be one or more subscriptions per account.</span></span> <span data-ttu-id="96b22-119">Az Azure rekordok számlázási adatok az előfizetés szintjén.</span><span class="sxs-lookup"><span data-stu-id="96b22-119">Azure records billing information at the subscription level.</span></span>

<span data-ttu-id="96b22-120">A fiók vagy előfizetés kapcsolat két hierarchiaszintekkel legfeljebb miatt fontos az elnevezési fiókok és-előfizetések számlázási igényeihez igazítani.</span><span class="sxs-lookup"><span data-stu-id="96b22-120">Due to the limit of two hierarchy levels on the Account/Subscription relationship, it is important to align the naming convention of accounts and subscriptions to the billing needs.</span></span> <span data-ttu-id="96b22-121">Például ha egy globális vállalat Azure használ, akkor előfordulhat, hogy úgy a egy fiók régiónként, és előfizetések felügyelt rendelkezik a terület szint:</span><span class="sxs-lookup"><span data-stu-id="96b22-121">For instance, if a global company uses Azure, they might choose to have one account per region, and have subscriptions managed at the region level:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

<span data-ttu-id="96b22-122">Például használhatja az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="96b22-122">For instance, you might use the following structure:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

<span data-ttu-id="96b22-123">Egy régiót úgy dönt, hogy egy adott csoporthoz tartozó egynél több előfizetéssel rendelkezik, ha az elnevezési oly módon, a felesleges adatokat a fiók vagy előfizetés nevét a kódolni kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="96b22-123">If a region decides to have more than one subscription associated to a particular group, the naming convention should incorporate a way to encode the extra data on either the account or the subscription name.</span></span> <span data-ttu-id="96b22-124">Ez a szervezet lehetővé teszi, hogy massaging elszámolási adatok a hierarchia új szintek során számlázási jelentések létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="96b22-124">This organization allows massaging billing data to generate the new levels of hierarchy during billing reports:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

<span data-ttu-id="96b22-125">Az alábbi példában a szervezet volt látható:</span><span class="sxs-lookup"><span data-stu-id="96b22-125">The organization could look like the following example:</span></span>

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

<span data-ttu-id="96b22-126">A nagyvállalati szerződéssel nyújtunk részletes számlázási egy letölthető fájlt egy olyan fiók, vagy az összes fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="96b22-126">We provide detailed billing via a downloadable file for a single account, or for all accounts in an enterprise agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96b22-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96b22-127">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

