---
title: "Mi a teendő, egy Azure virtuális hálózatot érintő Azure-szolgáltatások becsukódjon |} Microsoft Docs"
description: "Ismerje meg, mi a teendő az Azure-szolgáltatások becsukódjon érintő Azure virtuális hálózatok."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: 4e125406d2e798138c45e3fbbf61a610afab69fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="virtual-network--business-continuity"></a><span data-ttu-id="3aac0-103">Virtuális hálózat – az üzletmenet folytonossága</span><span class="sxs-lookup"><span data-stu-id="3aac0-103">Virtual Network – Business Continuity</span></span>
## <a name="overview"></a><span data-ttu-id="3aac0-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3aac0-104">Overview</span></span>
<span data-ttu-id="3aac0-105">Egy virtuális hálózatot (VNet) logikai reprezentációjává a hálózaton, a felhőben.</span><span class="sxs-lookup"><span data-stu-id="3aac0-105">A Virtual Network (VNet) is a logical representation of your network in the cloud.</span></span> <span data-ttu-id="3aac0-106">Lehetővé teszi a saját privát IP-címtér meghatározását, és a hálózati szegmenseket alhálózatokra.</span><span class="sxs-lookup"><span data-stu-id="3aac0-106">It allows you to define your own private IP address space and segment the network into subnets.</span></span> <span data-ttu-id="3aac0-107">Vnetek egy megbízhatósági kapcsolat határán a számítási erőforrásokat, például az Azure virtuális gépek és Felhőszolgáltatások (webes/munkavégző szerepkörök) futtatására szolgál.</span><span class="sxs-lookup"><span data-stu-id="3aac0-107">VNets serves as a trust boundary to host your compute resources such as Azure Virtual Machines and Cloud Services (web/worker roles).</span></span> <span data-ttu-id="3aac0-108">Egy VNet lehetővé teszi, hogy a közvetlen saját integrációs csomaggal folytatott kommunikációhoz a benne tárolt erőforrások között.</span><span class="sxs-lookup"><span data-stu-id="3aac0-108">A VNet allows direct private IP communication between the resources hosted in it.</span></span> <span data-ttu-id="3aac0-109">Egy virtuális hálózatot is lehet társítani egy helyi hálózaton keresztül, például a VPN-átjáró vagy ExpressRoute a hibrid lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="3aac0-109">A Virtual Network can also be linked to an on-premises network through one of the hybrid options such as a VPN Gateway or ExpressRoute.</span></span>

<span data-ttu-id="3aac0-110">A virtuális hálózat egy régiót hatókörén belül jön létre.</span><span class="sxs-lookup"><span data-stu-id="3aac0-110">A VNet is created within the scope of a region.</span></span> <span data-ttu-id="3aac0-111">Ugyanazt a címtartományt, két különböző régiókban Vnetek hozhat létre (azaz Velünk East és Velünk nyugati, de nem csatlakoztassa őket egy másik közvetlenül).</span><span class="sxs-lookup"><span data-stu-id="3aac0-111">You can create VNets with same address space in two different regions (i.e. US East and US West but cannot connect them to one another directly).</span></span> 

## <a name="business-continuity"></a><span data-ttu-id="3aac0-112">Üzleti folyamatok fenntarthatósága</span><span class="sxs-lookup"><span data-stu-id="3aac0-112">Business Continuity</span></span>
<span data-ttu-id="3aac0-113">Előfordulhat, hogy több különböző módon, hogy az alkalmazás sikerült-e működni.</span><span class="sxs-lookup"><span data-stu-id="3aac0-113">There could be several different ways that your application could be disrupted.</span></span> <span data-ttu-id="3aac0-114">Egy adott régió sikerült teljesen vágható természeti katasztrófa vagy több eszközök vagy szolgáltatások egy miatti részleges katasztrófa miatt.</span><span class="sxs-lookup"><span data-stu-id="3aac0-114">A given region could be completely cut off due to a natural disaster or a partial disaster due to a failure of multiple devices/services.</span></span> <span data-ttu-id="3aac0-115">A virtuális hálózat szolgáltatás gyakorolt hatás nem azonos a helyzetekben.</span><span class="sxs-lookup"><span data-stu-id="3aac0-115">The impact on the VNet service is different in each of these situations.</span></span>

<span data-ttu-id="3aac0-116">**K: Mit kell tennie egy teljes régióban kimaradás esetén? például ha egy régiót teljesen levágási természeti katasztrófa miatt? Mi történik a virtuális hálózatok régióban üzemeltetett?**</span><span class="sxs-lookup"><span data-stu-id="3aac0-116">**Q: What do you do in the event of an outage to an entire region? i.e. if a region is completely cutoff due to a natural disaster? What happens to the Virtual Networks hosted in the region?**</span></span>

<span data-ttu-id="3aac0-117">V: a virtuális hálózat és az érintett régióban erőforrás elérhetetlen maradjon az időre, amíg a szolgáltatás megszűnésének.</span><span class="sxs-lookup"><span data-stu-id="3aac0-117">A: The Virtual Network and the resources in the affected region remains inaccessible during the time of the service disruption.</span></span>

![Egyszerű virtuális hálózati diagramja](./media/virtual-network-disaster-recovery-guidance/vnet.png)

<span data-ttu-id="3aac0-119">**K: milyen lehetőségeket újra létre kell hoznia az azonos virtuális hálózatban egy másik régióban?**</span><span class="sxs-lookup"><span data-stu-id="3aac0-119">**Q: What can I to do re-create the same Virtual Network in a different region?**</span></span>

<span data-ttu-id="3aac0-120">V: virtual Network (VNet) viszonylag könnyű erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3aac0-120">A: Virtual Network (VNet) is fairly lightweight resource.</span></span> <span data-ttu-id="3aac0-121">Hozzon létre egy Vnetet ugyanazt a címtartományt egy másik régióban Azure API-hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="3aac0-121">You can invoke Azure APIs to create a VNet with the same address space in a different region.</span></span> <span data-ttu-id="3aac0-122">Újra létre kell hoznia, az érintett régió szerepel ugyanabban a környezetben, hogy helyezze újra üzembe a Felhőszolgáltatásokat (webes/munkavégző szerepkörök) és a virtuális gépek volt az API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="3aac0-122">To re-create the same environment that was present in the affected region, you have to make API calls to re-deploy your Cloud Services (web/worker roles) and Virtual Machines that you had.</span></span> <span data-ttu-id="3aac0-123">Akkor is rendelkezik majd lépett fel a VPN-átjáró és a helyszíni hálózat csatlakozni, ha a helyi kapcsolat (például egy hibrid telepítés).</span><span class="sxs-lookup"><span data-stu-id="3aac0-123">You will also have to spin up a VPN Gateway and connect to your on-premises network if you had on-premises connectivity (such as in a hybrid deployment).</span></span>

<span data-ttu-id="3aac0-124">Egy VNet létrehozására vonatkozó utasításokat találhatók [Itt](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="3aac0-124">The instructions for creating a VNet are found [here](virtual-networks-create-vnet-arm-pportal.md).</span></span> 

<span data-ttu-id="3aac0-125">**K: egy replikát készít a virtuális hálózat egy adott régióban lehet hozza létre újra egy másik régióban időben?**</span><span class="sxs-lookup"><span data-stu-id="3aac0-125">**Q: Can a replica of a VNet in a given region be re-created in another region ahead of time?**</span></span>

<span data-ttu-id="3aac0-126">A: Igen két Vnetek időben két különböző régiókban azonos privát IP-címtér és erőforrások használatával is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="3aac0-126">A: Yes, you can create two VNets using the same private IP address space and resources in two different regions ahead of time.</span></span> <span data-ttu-id="3aac0-127">Ha az ügyfél internetre irányuló szolgáltatások, a Vneten belül lett üzemeltet, azok sikerült állította be Traffic Manager földrajzi-útvonal forgalmát a régiót, amelyben aktív.</span><span class="sxs-lookup"><span data-stu-id="3aac0-127">If a customer was hosting internet facing services in the VNet, they could have set up Traffic Manager to geo-route traffic to the region that is active.</span></span> <span data-ttu-id="3aac0-128">Azonban az ügyfél nem tud csatlakozni két Vnetek a helyszíni hálózathoz ugyanazt a címtartományt, útválasztási problémák okozna.</span><span class="sxs-lookup"><span data-stu-id="3aac0-128">However, a customer cannot connect two VNets with the same address space to their on-premises network as it would cause routing issues.</span></span> <span data-ttu-id="3aac0-129">Egy olyan vészhelyzet esetén, és egy Vnetet egy régióban megszűnését az ügyfél csatlakozhatnak a rendelkezésre álló terület egyéb VNet-címterek a helyszíni hálózat megfelelő.</span><span class="sxs-lookup"><span data-stu-id="3aac0-129">At the time of a disaster and loss of a VNet in one region, a customer can connect the other VNet in the available region with matching address space to their on-premises network.</span></span>

<span data-ttu-id="3aac0-130">Egy VNet létrehozására vonatkozó utasításokat találhatók [Itt](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="3aac0-130">The instructions for creating a VNet are found [here](virtual-networks-create-vnet-arm-pportal.md).</span></span>

