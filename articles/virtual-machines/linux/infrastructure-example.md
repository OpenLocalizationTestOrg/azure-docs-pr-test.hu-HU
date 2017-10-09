---
title: "Azure-infrastruktúra forgatókönyv aaaExample |} Microsoft Docs"
description: "További tudnivalók hello legfontosabb tervezési és megvalósítási irányelvek Azure-ban egy példa infrastruktúra üzembe helyezéséhez."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 281fc2c0-b533-45fa-81a3-728c0049c73d
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6b307c0112203aa83e1fada81966fc265397a40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-linux-vms"></a><span data-ttu-id="55476-103">Linux virtuális gépek Azure-infrastruktúra bemutató példa</span><span class="sxs-lookup"><span data-stu-id="55476-103">Example Azure infrastructure walkthrough for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="55476-104">Ez a cikk végigvezeti egy példa infrastruktúrát létrehozására.</span><span class="sxs-lookup"><span data-stu-id="55476-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="55476-105">A Microsoft részletességi összegyűjti az összes hello irányelvek és elnevezési konvenciókat, a rendelkezésre állási csoportok, a virtuális hálózatok és terheléselosztók döntések egyszerű online tároló-infrastruktúrák tervezése, és ténylegesen telepítése az a virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="55476-105">We detail designing an infrastructure for a simple on-line store that brings together all hello guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="55476-106">Példa munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="55476-106">Example workload</span></span>
<span data-ttu-id="55476-107">Az Adventure Works Cycles toobuild egy online áruház-alkalmazás, amely áll az Azure-ban szeretne:</span><span class="sxs-lookup"><span data-stu-id="55476-107">Adventure Works Cycles wants toobuild an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="55476-108">Előtér olyan webes réteg hello-ügyféllel rendelkező két nginx-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="55476-108">Two nginx servers running hello client front-end in a web tier</span></span>
* <span data-ttu-id="55476-109">Adatok és egy alkalmazás szint kérelmek feldolgozása két nginx-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="55476-109">Two nginx servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="55476-110">Két MongoDB kiszolgálók szilánkos fürt része egy adatbázis-rétegből termék adatok és a rendeléseket tárolásához</span><span class="sxs-lookup"><span data-stu-id="55476-110">Two MongoDB servers part of a sharded cluster for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="55476-111">Két felhasználói fiókok és a szállítók egy hitelesítési réteg Active Directory-tartományvezérlők</span><span class="sxs-lookup"><span data-stu-id="55476-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="55476-112">Két alhálózat összes hello kiszolgálók találhatók:</span><span class="sxs-lookup"><span data-stu-id="55476-112">All hello servers are located in two subnets:</span></span>
  * <span data-ttu-id="55476-113">a webkiszolgálók hello előtér-alhálózat</span><span class="sxs-lookup"><span data-stu-id="55476-113">a front-end subnet for hello web servers</span></span> 
  * <span data-ttu-id="55476-114">hello alkalmazás-kiszolgálókat, a MongoDB-fürt és a tartományvezérlők háttér-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="55476-114">a back-end subnet for hello application servers, MongoDB cluster, and domain controllers</span></span>

![Alkalmazás-infrastruktúra különböző rétegek ábrája](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="55476-116">Bejövő biztonságos webes forgalom kell hello webkiszolgálók közötti kiegyensúlyozott ügyfelek Tallózás hello online áruház.</span><span class="sxs-lookup"><span data-stu-id="55476-116">Incoming secure web traffic must be load-balanced among hello web servers as customers browse hello on-line store.</span></span> <span data-ttu-id="55476-117">HTTP hello formájában forgalom feldolgozása sorrendben kér hello webes kiszolgálók kell lennie terhelésű hello alkalmazáskiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="55476-117">Order processing traffic in hello form of HTTP requests from hello web servers must be load-balanced among hello application servers.</span></span> <span data-ttu-id="55476-118">Emellett a hello infrastruktúra úgy kell megtervezni, a magas rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="55476-118">Additionally, hello infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="55476-119">hello eredményül kapott tervezési tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="55476-119">hello resulting design must incorporate:</span></span>

* <span data-ttu-id="55476-120">Egy Azure-előfizetés és a fiók</span><span class="sxs-lookup"><span data-stu-id="55476-120">An Azure subscription and account</span></span>
* <span data-ttu-id="55476-121">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="55476-121">A single resource group</span></span>
* <span data-ttu-id="55476-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="55476-122">Azure Managed Disks</span></span>
* <span data-ttu-id="55476-123">Két alhálózat virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="55476-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="55476-124">A virtuális gépek hello hasonló szerepet a rendelkezésre állási készletek</span><span class="sxs-lookup"><span data-stu-id="55476-124">Availability sets for hello VMs with a similar role</span></span>
* <span data-ttu-id="55476-125">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="55476-125">Virtual machines</span></span>

<span data-ttu-id="55476-126">Az összes fenti hello kövesse ezeket elnevezési konvenciókat:</span><span class="sxs-lookup"><span data-stu-id="55476-126">All hello above follow these naming conventions:</span></span>

* <span data-ttu-id="55476-127">Adventure Works Cycles használ **[informatikai munkaterhelés]-[hely]-[Azure-erőforrás]** előtagjaként</span><span class="sxs-lookup"><span data-stu-id="55476-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="55476-128">Ebben a példában a "**azos**" (Azure online áruház) van hello IT-munkaterhelést neve és a "**használja**" (USA keleti régiója 2) az hello hely</span><span class="sxs-lookup"><span data-stu-id="55476-128">For this example, "**azos**" (Azure On-line Store) is hello IT workload name and "**use**" (East US 2) is hello location</span></span>
* <span data-ttu-id="55476-129">Virtuális hálózatok használata AZOS-használat-VN**[szám]**</span><span class="sxs-lookup"><span data-stu-id="55476-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="55476-130">Rendelkezésre állási készletek használata azos-használata-,-**[szerepkör]**</span><span class="sxs-lookup"><span data-stu-id="55476-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="55476-131">Virtuális gép nevét használja azos-használata-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="55476-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="55476-132">Azure-előfizetések és fiókok</span><span class="sxs-lookup"><span data-stu-id="55476-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="55476-133">Az Adventure Works Cycles vállalati előfizetésének, az Adventure Works nagyvállalati előfizetéssel, az informatikai munkaterhelés számlázási tooprovide nevű használ.</span><span class="sxs-lookup"><span data-stu-id="55476-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, tooprovide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="55476-134">Storage</span><span class="sxs-lookup"><span data-stu-id="55476-134">Storage</span></span>
<span data-ttu-id="55476-135">Az Adventure Works Cycles határozza meg, hogy azok Azure felügyelt lemezeket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="55476-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="55476-136">Virtuális gépek létrehozásakor rendelkezésre álló tár mindkét tárolórétegek használhatók:</span><span class="sxs-lookup"><span data-stu-id="55476-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="55476-137">**Standard szintű tárolást** hello webkiszolgálók, alkalmazás-kiszolgálókat, és a tartományvezérlők és az adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="55476-137">**Standard storage** for hello web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="55476-138">**Prémium szintű storage** hello MongoDB szilánkos fürt kiszolgálók és az adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="55476-138">**Premium storage** for hello MongoDB sharded cluster servers and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="55476-139">Virtuális hálózat és alhálózatok</span><span class="sxs-lookup"><span data-stu-id="55476-139">Virtual network and subnets</span></span>
<span data-ttu-id="55476-140">Mivel hello virtuális hálózat nem kell a folyamatban lévő kapcsolat toohello Adventure munkahelyi ciklusok a helyi hálózaton, azok lezárását egy kizárólag felhőalapú virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="55476-140">Because hello virtual network does not need ongoing connectivity toohello Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="55476-141">Hozza létre őket egy kizárólag felhőalapú virtuális hálózatot a hello beállításait hello Azure portál segítségével a következő:</span><span class="sxs-lookup"><span data-stu-id="55476-141">They created a cloud-only virtual network with hello following settings using hello Azure portal:</span></span>

* <span data-ttu-id="55476-142">Neve: AZOS-használat-VN01</span><span class="sxs-lookup"><span data-stu-id="55476-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="55476-143">Helye: USA keleti régiója 2. régiója</span><span class="sxs-lookup"><span data-stu-id="55476-143">Location: East US 2</span></span>
* <span data-ttu-id="55476-144">Virtuális hálózat címtere: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="55476-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="55476-145">Első alhálózati:</span><span class="sxs-lookup"><span data-stu-id="55476-145">First subnet:</span></span>
  * <span data-ttu-id="55476-146">Name: előtér</span><span class="sxs-lookup"><span data-stu-id="55476-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="55476-147">Címtér: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="55476-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="55476-148">Második alhálózati:</span><span class="sxs-lookup"><span data-stu-id="55476-148">Second subnet:</span></span>
  * <span data-ttu-id="55476-149">Name: háttér</span><span class="sxs-lookup"><span data-stu-id="55476-149">Name: BackEnd</span></span>
  * <span data-ttu-id="55476-150">Címtér: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="55476-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="55476-151">Rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="55476-151">Availability sets</span></span>
<span data-ttu-id="55476-152">toomaintain magas rendelkezésre állású összes négy rétegből álló az online áruházat, az Adventure Works Cycles vállalat úgy döntött, a négy rendelkezésre állási csoportok:</span><span class="sxs-lookup"><span data-stu-id="55476-152">toomaintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="55476-153">**azos-használja-– weblapként** hello webkiszolgálókhoz</span><span class="sxs-lookup"><span data-stu-id="55476-153">**azos-use-as-web** for hello web servers</span></span>
* <span data-ttu-id="55476-154">**azos-használat-,-alkalmazás** hello alkalmazáskiszolgálók</span><span class="sxs-lookup"><span data-stu-id="55476-154">**azos-use-as-app** for hello application servers</span></span>
* <span data-ttu-id="55476-155">**azos-use-,-adatbázis** hello kiszolgálók hello MongoDB szilánkos fürt</span><span class="sxs-lookup"><span data-stu-id="55476-155">**azos-use-as-db** for hello servers in hello MongoDB sharded cluster</span></span>
* <span data-ttu-id="55476-156">**azos-használatát-szerint – dc** hello tartományvezérlők</span><span class="sxs-lookup"><span data-stu-id="55476-156">**azos-use-as-dc** for hello domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="55476-157">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="55476-157">Virtual machines</span></span>
<span data-ttu-id="55476-158">Az Adventure Works Cycles úgy döntött, a neveket az Azure virtuális gépek a következő hello:</span><span class="sxs-lookup"><span data-stu-id="55476-158">Adventure Works Cycles decided on hello following names for their Azure VMs:</span></span>

* <span data-ttu-id="55476-159">**azos-használat-vm-web01** hello első webkiszolgálón</span><span class="sxs-lookup"><span data-stu-id="55476-159">**azos-use-vm-web01** for hello first web server</span></span>
* <span data-ttu-id="55476-160">**azos-használat-vm-web02** hello második webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="55476-160">**azos-use-vm-web02** for hello second web server</span></span>
* <span data-ttu-id="55476-161">**azos-használat-vm-app01** hello első alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="55476-161">**azos-use-vm-app01** for hello first application server</span></span>
* <span data-ttu-id="55476-162">**azos-használat-vm-app02** hello második alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="55476-162">**azos-use-vm-app02** for hello second application server</span></span>
* <span data-ttu-id="55476-163">**azos-használat-vm-db01** hello első MongoDB kiszolgáló hello fürt</span><span class="sxs-lookup"><span data-stu-id="55476-163">**azos-use-vm-db01** for hello first MongoDB server in hello cluster</span></span>
* <span data-ttu-id="55476-164">**azos-használat-vm-db02** hello második MongoDB kiszolgáló hello fürt</span><span class="sxs-lookup"><span data-stu-id="55476-164">**azos-use-vm-db02** for hello second MongoDB server in hello cluster</span></span>
* <span data-ttu-id="55476-165">**azos-használat-vm-dc01** hello első tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="55476-165">**azos-use-vm-dc01** for hello first domain controller</span></span>
* <span data-ttu-id="55476-166">**azos-használat-vm-dc02** hello második tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="55476-166">**azos-use-vm-dc02** for hello second domain controller</span></span>

<span data-ttu-id="55476-167">Itt található hello létrejövő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="55476-167">Here is hello resulting configuration.</span></span>

![Az Azure-ban telepített alkalmazások végleges infrastruktúra](./media/infrastructure-example/example-config.png)

<span data-ttu-id="55476-169">Ez a konfiguráció a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="55476-169">This configuration incorporates:</span></span>

* <span data-ttu-id="55476-170">Egy kizárólag felhőalapú virtuális hálózatot, két alhálózattal (előtér- és háttérszolgáltatások)</span><span class="sxs-lookup"><span data-stu-id="55476-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="55476-171">Standard és a Premium lemezek használata az Azure Managed lemezek</span><span class="sxs-lookup"><span data-stu-id="55476-171">Azure Managed Disks using both Standard and Premium disks</span></span>
* <span data-ttu-id="55476-172">Négy rendelkezésre állási csoportok esetében egy, az egyes rétegekhez hello online áruház</span><span class="sxs-lookup"><span data-stu-id="55476-172">Four availability sets, one for each tier of hello on-line store</span></span>
* <span data-ttu-id="55476-173">virtuális gépek hello hello négy rétegek</span><span class="sxs-lookup"><span data-stu-id="55476-173">hello virtual machines for hello four tiers</span></span>
* <span data-ttu-id="55476-174">Egy külső elosztott terhelésű készlet hello Internet toohello webkiszolgálók a HTTPS-alapú webes forgalom</span><span class="sxs-lookup"><span data-stu-id="55476-174">An external load balanced set for HTTPS-based web traffic from hello Internet toohello web servers</span></span>
* <span data-ttu-id="55476-175">Belső elosztott terhelésű készlet hello webalkalmazás kiszolgálók toohello kiszolgálók a titkosítatlan webes forgalom</span><span class="sxs-lookup"><span data-stu-id="55476-175">An internal load balanced set for unencrypted web traffic from hello web servers toohello application servers</span></span>
* <span data-ttu-id="55476-176">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="55476-176">A single resource group</span></span>
