---
title: "Példa az Azure infrastruktúra forgatókönyv |} Microsoft Docs"
description: "További tudnivalók a főbb tervezési és megvalósítási irányelveket az Azure-ban egy példa infrastruktúra üzembe helyezéséhez."
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
ms.openlocfilehash: 84cefcdb85f1a3c753027e827abde010b461cda7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a><span data-ttu-id="acf60-103">Windows virtuális gépek Azure-infrastruktúra bemutató példa</span><span class="sxs-lookup"><span data-stu-id="acf60-103">Example Azure infrastructure walkthrough for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="acf60-104">Ez a cikk végigvezeti egy példa infrastruktúrát létrehozására.</span><span class="sxs-lookup"><span data-stu-id="acf60-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="acf60-105">A Microsoft részletességi összegyűjti az összes irányelvek és elnevezési konvenciókat, a rendelkezésre állási csoportok, a virtuális hálózatok és terheléselosztók döntések egyszerű online tároló-infrastruktúrák tervezése, és ténylegesen telepítése az a virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="acf60-105">We detail designing an infrastructure for a simple on-line store that brings together all the guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="acf60-106">Példa munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="acf60-106">Example workload</span></span>
<span data-ttu-id="acf60-107">Az Adventure Works Cycles szeretné az Azure-alkotó online áruházbeli alkalmazás létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="acf60-107">Adventure Works Cycles wants to build an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="acf60-108">Egy webes réteghez az előtér-ügyfelet futtató két IIS-kiszolgálókkal</span><span class="sxs-lookup"><span data-stu-id="acf60-108">Two IIS servers running the client front-end in a web tier</span></span>
* <span data-ttu-id="acf60-109">Két IIS-kiszolgálókkal, adatokhoz és egy alkalmazás szint kérelmek feldolgozása</span><span class="sxs-lookup"><span data-stu-id="acf60-109">Two IIS servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="acf60-110">Két Microsoft SQL Server-példány AlwaysOn rendelkezésre állási csoportokkal (két SQL-kiszolgáló és a legtöbb csomópont tanúsító) adatbázis-rétegből termék adatok és a rendeléseket tárolásához</span><span class="sxs-lookup"><span data-stu-id="acf60-110">Two Microsoft SQL Server instances with AlwaysOn availability groups (two SQL Servers and a majority node witness) for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="acf60-111">Két felhasználói fiókok és a szállítók egy hitelesítési réteg Active Directory-tartományvezérlők</span><span class="sxs-lookup"><span data-stu-id="acf60-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="acf60-112">A kiszolgálók két alhálózat találhatók:</span><span class="sxs-lookup"><span data-stu-id="acf60-112">All the servers are located in two subnets:</span></span>
  * <span data-ttu-id="acf60-113">a webkiszolgálók előtér-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="acf60-113">a front-end subnet for the web servers</span></span> 
  * <span data-ttu-id="acf60-114">az alkalmazás-kiszolgálókat, az SQL-fürt és a tartományvezérlők háttér-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="acf60-114">a back-end subnet for the application servers, SQL cluster, and domain controllers</span></span>

![Alkalmazás-infrastruktúra különböző rétegek ábrája](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="acf60-116">Bejövő biztonságos webes forgalom kell lennie terhelésű a webkiszolgálók között, az ügyfelek, keresse meg az online áruházból.</span><span class="sxs-lookup"><span data-stu-id="acf60-116">Incoming secure web traffic must be load-balanced among the web servers as customers browse the on-line store.</span></span> <span data-ttu-id="acf60-117">HTTP formájában forgalom feldolgozása sorrendben kéri le a webes kiszolgálók úgy kell meghatározni az alkalmazás-kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="acf60-117">Order processing traffic in the form of HTTP requests from the web servers must be balanced among the application servers.</span></span> <span data-ttu-id="acf60-118">Emellett az infrastruktúra úgy kell megtervezni a magas rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="acf60-118">Additionally, the infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="acf60-119">Az eredményül kapott terv tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="acf60-119">The resulting design must incorporate:</span></span>

* <span data-ttu-id="acf60-120">Egy Azure-előfizetés és a fiók</span><span class="sxs-lookup"><span data-stu-id="acf60-120">An Azure subscription and account</span></span>
* <span data-ttu-id="acf60-121">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="acf60-121">A single resource group</span></span>
* <span data-ttu-id="acf60-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="acf60-122">Azure Managed Disks</span></span>
* <span data-ttu-id="acf60-123">Két alhálózat virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="acf60-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="acf60-124">A hasonló szerepet rendelkező virtuális gépek rendelkezésre állási készletek</span><span class="sxs-lookup"><span data-stu-id="acf60-124">Availability sets for the VMs with a similar role</span></span>
* <span data-ttu-id="acf60-125">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="acf60-125">Virtual machines</span></span>

<span data-ttu-id="acf60-126">Minden a fenti kövesse az alábbi elnevezési konvenciókat:</span><span class="sxs-lookup"><span data-stu-id="acf60-126">All the above follow these naming conventions:</span></span>

* <span data-ttu-id="acf60-127">Adventure Works Cycles használ **[informatikai munkaterhelés]-[hely]-[Azure-erőforrás]** előtagjaként</span><span class="sxs-lookup"><span data-stu-id="acf60-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="acf60-128">Ebben a példában a "**azos**" (Azure online áruház) a az IT-munkaterhelést neve és a "**használja**" (USA keleti régiója 2) az a hely</span><span class="sxs-lookup"><span data-stu-id="acf60-128">For this example, "**azos**" (Azure On-line Store) is the IT workload name and "**use**" (East US 2) is the location</span></span>
* <span data-ttu-id="acf60-129">Virtuális hálózatok használata AZOS-használat-VN**[szám]**</span><span class="sxs-lookup"><span data-stu-id="acf60-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="acf60-130">Rendelkezésre állási készletek használata azos-használata-,-**[szerepkör]**</span><span class="sxs-lookup"><span data-stu-id="acf60-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="acf60-131">Virtuális gép nevét használja azos-használata-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="acf60-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="acf60-132">Azure-előfizetések és fiókok</span><span class="sxs-lookup"><span data-stu-id="acf60-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="acf60-133">Az Adventure Works Cycles vállalati előfizetésének, az Adventure Works nagyvállalati előfizetéssel nevű segítségével adja meg a számlázás informatikai munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="acf60-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, to provide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="acf60-134">Storage</span><span class="sxs-lookup"><span data-stu-id="acf60-134">Storage</span></span>
<span data-ttu-id="acf60-135">Az Adventure Works Cycles határozza meg, hogy azok Azure felügyelt lemezeket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="acf60-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="acf60-136">Virtuális gépek létrehozásakor rendelkezésre álló tár mindkét tárolórétegek használhatók:</span><span class="sxs-lookup"><span data-stu-id="acf60-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="acf60-137">**Standard szintű tárolást** a webkiszolgálók, alkalmazás-kiszolgálókat, és a tartományvezérlők és az adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="acf60-137">**Standard storage** for the web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="acf60-138">**Prémium szintű storage** az SQL Server virtuális gépek és az adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="acf60-138">**Premium storage** for the SQL Server VMs and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="acf60-139">Virtuális hálózat és alhálózatok</span><span class="sxs-lookup"><span data-stu-id="acf60-139">Virtual network and subnets</span></span>
<span data-ttu-id="acf60-140">Mivel a virtuális hálózat nem kell a folyamatban lévő Adventure munkahelyi ciklusok a helyszíni hálózati kapcsolattal, akkor lezárását egy kizárólag felhőalapú virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="acf60-140">Because the virtual network does not need ongoing connectivity to the Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="acf60-141">Hozza létre őket egy kizárólag felhőalapú virtuális hálózatot az Azure portál használatával a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="acf60-141">They created a cloud-only virtual network with the following settings using the Azure portal:</span></span>

* <span data-ttu-id="acf60-142">Neve: AZOS-használat-VN01</span><span class="sxs-lookup"><span data-stu-id="acf60-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="acf60-143">Helye: USA keleti régiója 2. régiója</span><span class="sxs-lookup"><span data-stu-id="acf60-143">Location: East US 2</span></span>
* <span data-ttu-id="acf60-144">Virtuális hálózat címtere: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="acf60-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="acf60-145">Első alhálózati:</span><span class="sxs-lookup"><span data-stu-id="acf60-145">First subnet:</span></span>
  * <span data-ttu-id="acf60-146">Name: előtér</span><span class="sxs-lookup"><span data-stu-id="acf60-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="acf60-147">Címtér: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="acf60-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="acf60-148">Második alhálózati:</span><span class="sxs-lookup"><span data-stu-id="acf60-148">Second subnet:</span></span>
  * <span data-ttu-id="acf60-149">Name: háttér</span><span class="sxs-lookup"><span data-stu-id="acf60-149">Name: BackEnd</span></span>
  * <span data-ttu-id="acf60-150">Címtér: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="acf60-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="acf60-151">Rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="acf60-151">Availability sets</span></span>
<span data-ttu-id="acf60-152">A magas rendelkezésre állású, hogy az online áruház összes négy rétegek fenntartása Adventure Works Cycles úgy döntött, a négy rendelkezésre állási csoportok:</span><span class="sxs-lookup"><span data-stu-id="acf60-152">To maintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="acf60-153">**azos-használja-– weblapként** a webkiszolgálókhoz</span><span class="sxs-lookup"><span data-stu-id="acf60-153">**azos-use-as-web** for the web servers</span></span>
* <span data-ttu-id="acf60-154">**azos-használat-,-alkalmazás** az alkalmazáskiszolgálók</span><span class="sxs-lookup"><span data-stu-id="acf60-154">**azos-use-as-app** for the application servers</span></span>
* <span data-ttu-id="acf60-155">**azos-használat-,-sql** SQL-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="acf60-155">**azos-use-as-sql** for the SQL Servers</span></span>
* <span data-ttu-id="acf60-156">**azos-használatát-szerint – dc** a tartományvezérlők</span><span class="sxs-lookup"><span data-stu-id="acf60-156">**azos-use-as-dc** for the domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="acf60-157">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="acf60-157">Virtual machines</span></span>
<span data-ttu-id="acf60-158">Az Adventure Works Cycles határozott meg a következő neveket az Azure virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="acf60-158">Adventure Works Cycles decided on the following names for their Azure VMs:</span></span>

* <span data-ttu-id="acf60-159">**azos-használat-vm-web01** az első webkiszolgálón</span><span class="sxs-lookup"><span data-stu-id="acf60-159">**azos-use-vm-web01** for the first web server</span></span>
* <span data-ttu-id="acf60-160">**azos-használat-vm-web02** a második webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="acf60-160">**azos-use-vm-web02** for the second web server</span></span>
* <span data-ttu-id="acf60-161">**azos-használat-vm-app01** az első alkalmazás-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="acf60-161">**azos-use-vm-app01** for the first application server</span></span>
* <span data-ttu-id="acf60-162">**azos-használat-vm-app02** a második alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="acf60-162">**azos-use-vm-app02** for the second application server</span></span>
* <span data-ttu-id="acf60-163">**azos-használat-vm-sql01** a fürt első SQL Server-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="acf60-163">**azos-use-vm-sql01** for the first SQL Server server in the cluster</span></span>
* <span data-ttu-id="acf60-164">**azos-használat-vm-sql02** a második SQL Server-kiszolgáló a fürt</span><span class="sxs-lookup"><span data-stu-id="acf60-164">**azos-use-vm-sql02** for the second SQL Server server in the cluster</span></span>
* <span data-ttu-id="acf60-165">**azos-használat-vm-dc01** az első tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="acf60-165">**azos-use-vm-dc01** for the first domain controller</span></span>
* <span data-ttu-id="acf60-166">**azos-használat-vm-dc02** a második tartományvezérlő</span><span class="sxs-lookup"><span data-stu-id="acf60-166">**azos-use-vm-dc02** for the second domain controller</span></span>

<span data-ttu-id="acf60-167">Ez a létrejövő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="acf60-167">Here is the resulting configuration.</span></span>

![Az Azure-ban telepített alkalmazások végleges infrastruktúra](./media/infrastructure-example/example-config.png)

<span data-ttu-id="acf60-169">Ez a konfiguráció a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="acf60-169">This configuration incorporates:</span></span>

* <span data-ttu-id="acf60-170">Egy kizárólag felhőalapú virtuális hálózatot, két alhálózattal (előtér- és háttérszolgáltatások)</span><span class="sxs-lookup"><span data-stu-id="acf60-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="acf60-171">Az Azure Managed lemezek a Standard és a Premium lemezek</span><span class="sxs-lookup"><span data-stu-id="acf60-171">Azure Managed Disks with both Standard and Premium disks</span></span>
* <span data-ttu-id="acf60-172">Négy rendelkezésre állási csoportok esetében egy, az egyes rétegekhez az online áruház</span><span class="sxs-lookup"><span data-stu-id="acf60-172">Four availability sets, one for each tier of the on-line store</span></span>
* <span data-ttu-id="acf60-173">A virtuális gépek négy rétegek</span><span class="sxs-lookup"><span data-stu-id="acf60-173">The virtual machines for the four tiers</span></span>
* <span data-ttu-id="acf60-174">A HTTPS-alapú webes forgalom az internetről webkiszolgálók egy külső elosztott terhelésű készlet</span><span class="sxs-lookup"><span data-stu-id="acf60-174">An external load balanced set for HTTPS-based web traffic from the Internet to the web servers</span></span>
* <span data-ttu-id="acf60-175">Belső elosztott terhelésű készlet alkalmazáskiszolgálókra érkező titkosítatlan webes forgalom</span><span class="sxs-lookup"><span data-stu-id="acf60-175">An internal load balanced set for unencrypted web traffic from the web servers to the application servers</span></span>
* <span data-ttu-id="acf60-176">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="acf60-176">A single resource group</span></span>