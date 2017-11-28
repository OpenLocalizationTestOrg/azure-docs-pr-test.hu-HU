---
title: aaaSizing adatai egy VNETET az Azure Remoteappban |} Microsoft Docs
description: "Hello IP-cím követelmények a virtuális hálózaton futó Azure RemoteApp megismerése"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b6e1c4ba-0236-42b2-bced-69353bf211be
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: f98b831af32c41740b258d122b3e18765be08d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a><span data-ttu-id="e3771-103">Egy VNETET az Azure Remoteappban méretezési információi</span><span class="sxs-lookup"><span data-stu-id="e3771-103">Sizing information for a VNET in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e3771-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="e3771-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="e3771-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="e3771-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="e3771-106">Azure RemoteApp virtuális hálózatot (VNET) használja, használja a RemoteApp és az hello alhálózaton belüli IP-címek.</span><span class="sxs-lookup"><span data-stu-id="e3771-106">When you use Azure RemoteApp with a virtual network (VNET), RemoteApp uses IP addresses within hello subnet.</span></span> <span data-ttu-id="e3771-107">A RemoteApp szolgáltatás hello méretezési, meg kell, hogy rendelkezik-e elegendő RemoteApp virtuális gépeknél érhető el IP-cím az alhálózat tooensure.</span><span class="sxs-lookup"><span data-stu-id="e3771-107">Based on hello scale of your RemoteApp service, you need tooensure that your subnet has enough IP addresses available for RemoteApp virtual machines.</span></span> <span data-ttu-id="e3771-108">A olvasható méretezési útmutató nem tökéletes megoldás adott hogyan forog fel dinamikusan a RemoteApp és forog le egy gyűjteményen belül virtuális gépeket, miközben nyújt segítséget az alhálózat tartományának becslése.</span><span class="sxs-lookup"><span data-stu-id="e3771-108">While this sizing guidance isn’t perfect given how RemoteApp dynamically spins up and spins down virtual machines within a collection, it will help you estimate your subnet range.</span></span> <span data-ttu-id="e3771-109">Ez különösen fontos, mivel a RemoteApp szolgáltatás a VNETEN belül helyezkedik el, miután RemoteApp eltávolítása nélkül nem növelhető hello alhálózat méretét.</span><span class="sxs-lookup"><span data-stu-id="e3771-109">This is especially important as, once a RemoteApp service is placed in a VNET, you cannot increase hello subnet size without removing RemoteApp.</span></span>

<span data-ttu-id="e3771-110">Mindegyik RemoteApp-gyűjteményt, amelyet az toorun maximális kapacitással 100 elérhető IP-címek kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="e3771-110">For each RemoteApp collection that you want toorun at maximum capacity, you should have 100 IP addresses available.</span></span> <span data-ttu-id="e3771-111">Például ha hello Standard csomag egy RemoteApp-gyűjteménnyel rendelkezik, és azt szeretné, hogy toohave hello legfeljebb 500 felhasználónak, kell gyűjteménynek 100 IP-címet.</span><span class="sxs-lookup"><span data-stu-id="e3771-111">For example, if you have one RemoteApp collection in hello Standard plan and you want toohave hello maximum 500 users, you should have 100 IP addresses for that collection.</span></span> <span data-ttu-id="e3771-112">Ehhez hasonlóan kell 100 IP-címek egy RemoteApp-gyűjtemény, amely rendelkezik 800 felhasználók alapszintű hello.</span><span class="sxs-lookup"><span data-stu-id="e3771-112">Similarly, you need 100 IP addresses for a RemoteApp collection in hello Basic plan that has 800 users.</span></span> <span data-ttu-id="e3771-113">Ha azt tervezi, toohave kevesebb felhasználók (kisebb maximális hello), csökkentheti a gyűjteményenként szükséges hello IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="e3771-113">If you plan toohave fewer users (less than hello maximum), you can reduce hello IP addresses needed per collection.</span></span> <span data-ttu-id="e3771-114">hello alhálózati minimális méretkövetelményt 30 IP-címek (/ 27).</span><span class="sxs-lookup"><span data-stu-id="e3771-114">hello minimum subnet size requirement is 30 IP addresses (/27).</span></span>

<span data-ttu-id="e3771-115">Tekintse meg a következő információk toomake meg arról, hogy a virtuális hálózat van konfigurálva, és működik-e propertly hello:</span><span class="sxs-lookup"><span data-stu-id="e3771-115">Check out hello following information toomake sure your VNET is configured and working propertly:</span></span>

* [<span data-ttu-id="e3771-116">Személyes virtuális hálózat tooan Azure virtuális hálózat áttelepítése</span><span class="sxs-lookup"><span data-stu-id="e3771-116">Migrate from a personal VNET tooan Azure VNET</span></span>](remoteapp-migratevnet.md)
* [<span data-ttu-id="e3771-117">Az Azure RemoteApp hello Azure VNET toouse ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="e3771-117">Validate hello Azure VNET toouse with Azure RemoteApp</span></span>](remoteapp-vnet.md)

