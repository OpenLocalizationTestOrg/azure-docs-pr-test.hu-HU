---
title: "Azure-infrastruktúra forgatókönyv aaaExample |} Microsoft Docs"
description: "További tudnivalók hello legfontosabb tervezési és megvalósítási irányelvek Azure-ban egy példa infrastruktúra üzembe helyezéséhez."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd6b6e904404bea0b5be37dfce6d60039daae636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a><span data-ttu-id="9c0ac-103">Windows virtuális gépek Azure-infrastruktúra bemutató példa</span><span class="sxs-lookup"><span data-stu-id="9c0ac-103">Example Azure infrastructure walkthrough for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="9c0ac-104">Ez a cikk végigvezeti egy példa infrastruktúrát létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="9c0ac-105">A Microsoft részletességi összegyűjti az összes hello irányelvek és elnevezési konvenciókat, a rendelkezésre állási csoportok, a virtuális hálózatok és terheléselosztók döntések egyszerű online tároló-infrastruktúrák tervezése, és ténylegesen telepítése az a virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="9c0ac-105">We detail designing an infrastructure for a simple on-line store that brings together all hello guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="9c0ac-106">Példa munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="9c0ac-106">Example workload</span></span>
<span data-ttu-id="9c0ac-107">Az Adventure Works Cycles toobuild egy online áruház-alkalmazás, amely áll az Azure-ban szeretne:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-107">Adventure Works Cycles wants toobuild an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="9c0ac-108">Két előtér olyan webes réteg hello ügyfelet futtató IIS-kiszolgálókkal</span><span class="sxs-lookup"><span data-stu-id="9c0ac-108">Two IIS servers running hello client front-end in a web tier</span></span>
* <span data-ttu-id="9c0ac-109">Két IIS-kiszolgálókkal, adatokhoz és egy alkalmazás szint kérelmek feldolgozása</span><span class="sxs-lookup"><span data-stu-id="9c0ac-109">Two IIS servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="9c0ac-110">Két Microsoft SQL Server-példány AlwaysOn rendelkezésre állási csoportokkal (két SQL-kiszolgáló és a legtöbb csomópont tanúsító) adatbázis-rétegből termék adatok és a rendeléseket tárolásához</span><span class="sxs-lookup"><span data-stu-id="9c0ac-110">Two Microsoft SQL Server instances with AlwaysOn availability groups (two SQL Servers and a majority node witness) for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="9c0ac-111">Két felhasználói fiókok és a szállítók egy hitelesítési réteg Active Directory-tartományvezérlők</span><span class="sxs-lookup"><span data-stu-id="9c0ac-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="9c0ac-112">Két alhálózat összes hello kiszolgálók találhatók:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-112">All hello servers are located in two subnets:</span></span>
  * <span data-ttu-id="9c0ac-113">a webkiszolgálók hello előtér-alhálózat</span><span class="sxs-lookup"><span data-stu-id="9c0ac-113">a front-end subnet for hello web servers</span></span> 
  * <span data-ttu-id="9c0ac-114">hello alkalmazás-kiszolgálókat, az SQL-fürt és a tartományvezérlők háttér-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="9c0ac-114">a back-end subnet for hello application servers, SQL cluster, and domain controllers</span></span>

![Alkalmazás-infrastruktúra különböző rétegek ábrája](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="9c0ac-116">Bejövő biztonságos webes forgalom kell hello webkiszolgálók közötti kiegyensúlyozott ügyfelek Tallózás hello online áruház.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-116">Incoming secure web traffic must be load-balanced among hello web servers as customers browse hello on-line store.</span></span> <span data-ttu-id="9c0ac-117">Rendezés feldolgozási forgalom hello képernyőn a HTTP-kérések hello webkiszolgálóról kiszolgálók mérlegelendő hello alkalmazáskiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-117">Order processing traffic in hello form of HTTP requests from hello web servers must be balanced among hello application servers.</span></span> <span data-ttu-id="9c0ac-118">Emellett a hello infrastruktúra úgy kell megtervezni, a magas rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-118">Additionally, hello infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="9c0ac-119">hello eredményül kapott tervezési tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-119">hello resulting design must incorporate:</span></span>

* <span data-ttu-id="9c0ac-120">Egy Azure-előfizetés és a fiók</span><span class="sxs-lookup"><span data-stu-id="9c0ac-120">An Azure subscription and account</span></span>
* <span data-ttu-id="9c0ac-121">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="9c0ac-121">A single resource group</span></span>
* <span data-ttu-id="9c0ac-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="9c0ac-122">Azure Managed Disks</span></span>
* <span data-ttu-id="9c0ac-123">Két alhálózat virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="9c0ac-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="9c0ac-124">A virtuális gépek hello hasonló szerepet a rendelkezésre állási készletek</span><span class="sxs-lookup"><span data-stu-id="9c0ac-124">Availability sets for hello VMs with a similar role</span></span>
* <span data-ttu-id="9c0ac-125">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="9c0ac-125">Virtual machines</span></span>

<span data-ttu-id="9c0ac-126">Az összes fenti hello kövesse ezeket elnevezési konvenciókat:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-126">All hello above follow these naming conventions:</span></span>

* <span data-ttu-id="9c0ac-127">Adventure Works Cycles használ **[informatikai munkaterhelés]-[hely]-[Azure-erőforrás]** előtagjaként</span><span class="sxs-lookup"><span data-stu-id="9c0ac-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="9c0ac-128">Ebben a példában a "**azos**" (Azure online áruház) van hello IT-munkaterhelést neve és a "**használja**" (USA keleti régiója 2) az hello hely</span><span class="sxs-lookup"><span data-stu-id="9c0ac-128">For this example, "**azos**" (Azure On-line Store) is hello IT workload name and "**use**" (East US 2) is hello location</span></span>
* <span data-ttu-id="9c0ac-129">Virtuális hálózatok használata AZOS-használat-VN**[szám]**</span><span class="sxs-lookup"><span data-stu-id="9c0ac-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="9c0ac-130">Rendelkezésre állási készletek használata azos-használata-,-**[szerepkör]**</span><span class="sxs-lookup"><span data-stu-id="9c0ac-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="9c0ac-131">Virtuális gép nevét használja azos-használata-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="9c0ac-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="9c0ac-132">Azure-előfizetések és fiókok</span><span class="sxs-lookup"><span data-stu-id="9c0ac-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="9c0ac-133">Az Adventure Works Cycles vállalati előfizetésének, az Adventure Works nagyvállalati előfizetéssel, az informatikai munkaterhelés számlázási tooprovide nevű használ.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, tooprovide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="9c0ac-134">Storage</span><span class="sxs-lookup"><span data-stu-id="9c0ac-134">Storage</span></span>
<span data-ttu-id="9c0ac-135">Az Adventure Works Cycles határozza meg, hogy azok Azure felügyelt lemezeket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="9c0ac-136">Virtuális gépek létrehozásakor rendelkezésre álló tár mindkét tárolórétegek használhatók:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="9c0ac-137">**Standard szintű tárolást** hello webkiszolgálók, alkalmazás-kiszolgálókat, és a tartományvezérlők és az adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-137">**Standard storage** for hello web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="9c0ac-138">**Prémium szintű storage** hello SQL Server virtuális gépek és az adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-138">**Premium storage** for hello SQL Server VMs and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="9c0ac-139">Virtuális hálózat és alhálózatok</span><span class="sxs-lookup"><span data-stu-id="9c0ac-139">Virtual network and subnets</span></span>
<span data-ttu-id="9c0ac-140">Mivel hello virtuális hálózat nem kell a folyamatban lévő kapcsolat toohello Adventure munkahelyi ciklusok a helyi hálózaton, azok lezárását egy kizárólag felhőalapú virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-140">Because hello virtual network does not need ongoing connectivity toohello Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="9c0ac-141">Hozza létre őket egy kizárólag felhőalapú virtuális hálózatot a hello beállításait hello Azure portál segítségével a következő:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-141">They created a cloud-only virtual network with hello following settings using hello Azure portal:</span></span>

* <span data-ttu-id="9c0ac-142">Neve: AZOS-használat-VN01</span><span class="sxs-lookup"><span data-stu-id="9c0ac-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="9c0ac-143">Helye: USA keleti régiója 2. régiója</span><span class="sxs-lookup"><span data-stu-id="9c0ac-143">Location: East US 2</span></span>
* <span data-ttu-id="9c0ac-144">Virtuális hálózat címtere: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="9c0ac-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="9c0ac-145">Első alhálózati:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-145">First subnet:</span></span>
  * <span data-ttu-id="9c0ac-146">Name: előtér</span><span class="sxs-lookup"><span data-stu-id="9c0ac-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="9c0ac-147">Címtér: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="9c0ac-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="9c0ac-148">Második alhálózati:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-148">Second subnet:</span></span>
  * <span data-ttu-id="9c0ac-149">Name: háttér</span><span class="sxs-lookup"><span data-stu-id="9c0ac-149">Name: BackEnd</span></span>
  * <span data-ttu-id="9c0ac-150">Címtér: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="9c0ac-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="9c0ac-151">Rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="9c0ac-151">Availability sets</span></span>
<span data-ttu-id="9c0ac-152">toomaintain magas rendelkezésre állású összes négy rétegből álló az online áruházat, az Adventure Works Cycles vállalat úgy döntött, a négy rendelkezésre állási csoportok:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-152">toomaintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="9c0ac-153">**azos-használja-– weblapként** hello webkiszolgálókhoz</span><span class="sxs-lookup"><span data-stu-id="9c0ac-153">**azos-use-as-web** for hello web servers</span></span>
* <span data-ttu-id="9c0ac-154">**azos-használat-,-alkalmazás** hello alkalmazáskiszolgálók</span><span class="sxs-lookup"><span data-stu-id="9c0ac-154">**azos-use-as-app** for hello application servers</span></span>
* <span data-ttu-id="9c0ac-155">**azos-használat-,-sql** hello SQL Server-kiszolgálók számára</span><span class="sxs-lookup"><span data-stu-id="9c0ac-155">**azos-use-as-sql** for hello SQL Servers</span></span>
* <span data-ttu-id="9c0ac-156">**azos-használatát-szerint – dc** hello tartományvezérlők</span><span class="sxs-lookup"><span data-stu-id="9c0ac-156">**azos-use-as-dc** for hello domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="9c0ac-157">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="9c0ac-157">Virtual machines</span></span>
<span data-ttu-id="9c0ac-158">Az Adventure Works Cycles úgy döntött, a neveket az Azure virtuális gépek a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-158">Adventure Works Cycles decided on hello following names for their Azure VMs:</span></span>

* <span data-ttu-id="9c0ac-159">**azos-használat-vm-web01** hello első webkiszolgálón</span><span class="sxs-lookup"><span data-stu-id="9c0ac-159">**azos-use-vm-web01** for hello first web server</span></span>
* <span data-ttu-id="9c0ac-160">**azos-használat-vm-web02** hello második webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9c0ac-160">**azos-use-vm-web02** for hello second web server</span></span>
* <span data-ttu-id="9c0ac-161">**azos-használat-vm-app01** hello első alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9c0ac-161">**azos-use-vm-app01** for hello first application server</span></span>
* <span data-ttu-id="9c0ac-162">**azos-használat-vm-app02** hello második alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9c0ac-162">**azos-use-vm-app02** for hello second application server</span></span>
* <span data-ttu-id="9c0ac-163">**azos-használat-vm-sql01** hello első SQL Server kiszolgáló hello fürt</span><span class="sxs-lookup"><span data-stu-id="9c0ac-163">**azos-use-vm-sql01** for hello first SQL Server server in hello cluster</span></span>
* <span data-ttu-id="9c0ac-164">**azos-használat-vm-sql02** hello második SQL Server kiszolgáló hello fürt</span><span class="sxs-lookup"><span data-stu-id="9c0ac-164">**azos-use-vm-sql02** for hello second SQL Server server in hello cluster</span></span>
* <span data-ttu-id="9c0ac-165">**azos-használat-vm-dc01** hello első tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="9c0ac-165">**azos-use-vm-dc01** for hello first domain controller</span></span>
* <span data-ttu-id="9c0ac-166">**azos-használat-vm-dc02** hello második tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="9c0ac-166">**azos-use-vm-dc02** for hello second domain controller</span></span>

<span data-ttu-id="9c0ac-167">Itt található hello létrejövő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9c0ac-167">Here is hello resulting configuration.</span></span>

![Az Azure-ban telepített alkalmazások végleges infrastruktúra](./media/infrastructure-example/example-config.png)

<span data-ttu-id="9c0ac-169">Ez a konfiguráció a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9c0ac-169">This configuration incorporates:</span></span>

* <span data-ttu-id="9c0ac-170">Egy kizárólag felhőalapú virtuális hálózatot, két alhálózattal (előtér- és háttérszolgáltatások)</span><span class="sxs-lookup"><span data-stu-id="9c0ac-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="9c0ac-171">Az Azure Managed lemezek a Standard és a Premium lemezek</span><span class="sxs-lookup"><span data-stu-id="9c0ac-171">Azure Managed Disks with both Standard and Premium disks</span></span>
* <span data-ttu-id="9c0ac-172">Négy rendelkezésre állási csoportok esetében egy, az egyes rétegekhez hello online áruház</span><span class="sxs-lookup"><span data-stu-id="9c0ac-172">Four availability sets, one for each tier of hello on-line store</span></span>
* <span data-ttu-id="9c0ac-173">virtuális gépek hello hello négy rétegek</span><span class="sxs-lookup"><span data-stu-id="9c0ac-173">hello virtual machines for hello four tiers</span></span>
* <span data-ttu-id="9c0ac-174">Egy külső elosztott terhelésű készlet hello Internet toohello webkiszolgálók a HTTPS-alapú webes forgalom</span><span class="sxs-lookup"><span data-stu-id="9c0ac-174">An external load balanced set for HTTPS-based web traffic from hello Internet toohello web servers</span></span>
* <span data-ttu-id="9c0ac-175">Belső elosztott terhelésű készlet hello webalkalmazás kiszolgálók toohello kiszolgálók a titkosítatlan webes forgalom</span><span class="sxs-lookup"><span data-stu-id="9c0ac-175">An internal load balanced set for unencrypted web traffic from hello web servers toohello application servers</span></span>
* <span data-ttu-id="9c0ac-176">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="9c0ac-176">A single resource group</span></span>