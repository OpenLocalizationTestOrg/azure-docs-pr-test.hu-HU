---
title: "Az Oracle vész-helyreállítási forgatókönyv az Azure környezetben áttekintése |} Microsoft Docs"
description: "Oracle Database 12c adatbázis az Azure környezetben vészhelyreállítás"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/2/2017
ms.author: rclaus
ms.openlocfilehash: f17ebb2b74cd7ad872f88483ed7cdb4f239ee069
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a><span data-ttu-id="501ce-103">Oracle Database 12c adatbázis Azure környezetben vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="501ce-103">Disaster recovery for an Oracle Database 12c database in an Azure environment</span></span>

## <a name="assumptions"></a><span data-ttu-id="501ce-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="501ce-104">Assumptions</span></span>

- <span data-ttu-id="501ce-105">Oracle Data Guard tervezési és az Azure-környezetek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="501ce-105">You have an understanding of Oracle Data Guard design and Azure environments.</span></span>


## <a name="goals"></a><span data-ttu-id="501ce-106">Célok</span><span class="sxs-lookup"><span data-stu-id="501ce-106">Goals</span></span>
- <span data-ttu-id="501ce-107">Tervezési a topológia és a konfigurációt, amely a vész-helyreállítási követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="501ce-107">Design the topology and configuration that meet your disaster recovery (DR) requirements.</span></span>

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a><span data-ttu-id="501ce-108">1. forgatókönyv: Elsődleges és a vész-Helyreállítási helyeken az Azure-on</span><span class="sxs-lookup"><span data-stu-id="501ce-108">Scenario 1: Primary and DR sites on Azure</span></span>

<span data-ttu-id="501ce-109">Az ügyfél rendelkezik egy Oracle adatbázis-állítsa be az elsődleges helyen.</span><span class="sxs-lookup"><span data-stu-id="501ce-109">A customer has an Oracle database set up on the primary site.</span></span> <span data-ttu-id="501ce-110">A vész-Helyreállítási hely van egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="501ce-110">A DR site is in a different region.</span></span> <span data-ttu-id="501ce-111">Az ügyfél Oracle Data Guard használja a helyek közötti gyors helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="501ce-111">The customer uses Oracle Data Guard for quick recovery between these sites.</span></span> <span data-ttu-id="501ce-112">Az elsődleges hely is rendelkezik, egy másodlagos adatbázis jelentéskészítéshez és egyéb felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="501ce-112">The primary site also has a secondary database for reporting and other uses.</span></span> 

### <a name="topology"></a><span data-ttu-id="501ce-113">topológia</span><span class="sxs-lookup"><span data-stu-id="501ce-113">Topology</span></span>

<span data-ttu-id="501ce-114">Ez az Azure beállítása összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="501ce-114">Here is a summary of the Azure setup:</span></span>

- <span data-ttu-id="501ce-115">Két hely (elsődleges hely és a vész-Helyreállítási hely)</span><span class="sxs-lookup"><span data-stu-id="501ce-115">Two sites (a primary site and a DR site)</span></span>
- <span data-ttu-id="501ce-116">Két virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="501ce-116">Two virtual networks</span></span>
- <span data-ttu-id="501ce-117">Két adatbázisokkal Oracle Data Guard (elsődleges és készenléti állapot)</span><span class="sxs-lookup"><span data-stu-id="501ce-117">Two Oracle databases with Data Guard (primary and standby)</span></span>
- <span data-ttu-id="501ce-118">Két Oracle adatbázis Golden kapu vagy Data Guard (csak az elsődleges hely)</span><span class="sxs-lookup"><span data-stu-id="501ce-118">Two Oracle databases with Golden Gate or Data Guard (primary site only)</span></span>
- <span data-ttu-id="501ce-119">Két alkalmazás szolgáltatást, egy elsődleges és egy, a vész-Helyreállítási helyen</span><span class="sxs-lookup"><span data-stu-id="501ce-119">Two application services, one primary and one on the DR site</span></span>
- <span data-ttu-id="501ce-120">Egy *rendelkezésre állási csoport* amellyel adatbázis és az alkalmazás szolgáltatás az elsődleges helyen</span><span class="sxs-lookup"><span data-stu-id="501ce-120">An *availability set,* which is used for database and application service on the primary site</span></span>
- <span data-ttu-id="501ce-121">Minden helyen, amely korlátozza a hozzáférést a magánhálózaton, és csak a bejelentkezés egy rendszergazda lehetővé teszi egy jumpbox</span><span class="sxs-lookup"><span data-stu-id="501ce-121">One jumpbox on each site, which restricts access to the private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="501ce-122">A jumpbox, a alkalmazás service, az adatbázis és a VPN-átjáró külön alhálózatokon</span><span class="sxs-lookup"><span data-stu-id="501ce-122">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="501ce-123">NSG kényszeríti az alkalmazás és az adatbázis alhálózatok</span><span class="sxs-lookup"><span data-stu-id="501ce-123">NSG enforced on application and database subnets</span></span>

![A vész-Helyreállítási topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a><span data-ttu-id="501ce-125">2. forgatókönyv: Helyszíni elsődleges hely és az Azure-on vész-Helyreállítási hely</span><span class="sxs-lookup"><span data-stu-id="501ce-125">Scenario 2: Primary site on-premises and DR site on Azure</span></span>

<span data-ttu-id="501ce-126">Az ügyfél rendelkezik egy helyszíni Oracle adatbázis-beállítások (elsődleges helyen).</span><span class="sxs-lookup"><span data-stu-id="501ce-126">A customer has an on-premises Oracle database setup (primary site).</span></span> <span data-ttu-id="501ce-127">A vész-Helyreállítási hely van, az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="501ce-127">A DR site is on Azure.</span></span> <span data-ttu-id="501ce-128">Oracle Data Guard szolgál a helyek közötti gyors helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="501ce-128">Oracle Data Guard is used for quick recovery between these sites.</span></span> <span data-ttu-id="501ce-129">Az elsődleges hely is rendelkezik, egy másodlagos adatbázis jelentéskészítéshez és egyéb felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="501ce-129">The primary site also has a secondary database for reporting and other uses.</span></span> 

<span data-ttu-id="501ce-130">Kétféleképpen a telepítés.</span><span class="sxs-lookup"><span data-stu-id="501ce-130">There are two approaches for this setup.</span></span>

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-the-firewall"></a><span data-ttu-id="501ce-131">1. módszer: A helyszíni és az Azure, nyitott TCP-portok a tűzfalon igénylő közti közvetlen kapcsolatokon keresztül</span><span class="sxs-lookup"><span data-stu-id="501ce-131">Approach 1: Direct connections between on-premises and Azure, requiring open TCP ports on the firewall</span></span> 

<span data-ttu-id="501ce-132">Közvetlen kapcsolatok nem ajánlott, mert a TCP-portok a külvilág szolgáltatnak.</span><span class="sxs-lookup"><span data-stu-id="501ce-132">We don't recommend direct connections because they expose the TCP ports to the outside world.</span></span>

#### <a name="topology"></a><span data-ttu-id="501ce-133">topológia</span><span class="sxs-lookup"><span data-stu-id="501ce-133">Topology</span></span>

<span data-ttu-id="501ce-134">Az alábbiakban olvashat az Azure beállítása összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="501ce-134">Following is a summary of the Azure setup:</span></span>

- <span data-ttu-id="501ce-135">Egy vész-Helyreállítási hely</span><span class="sxs-lookup"><span data-stu-id="501ce-135">One DR site</span></span> 
- <span data-ttu-id="501ce-136">Egy virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="501ce-136">One virtual network</span></span>
- <span data-ttu-id="501ce-137">A Data Guard (aktív) egy Oracle-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="501ce-137">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="501ce-138">A vész-Helyreállítási helyen egy alkalmazásszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="501ce-138">One application service on the DR site</span></span>
- <span data-ttu-id="501ce-139">Egy jumpbox, amely korlátozza a hozzáférést a magánhálózaton, és csak lehetővé teszi a bejelentkezés rendszergazda</span><span class="sxs-lookup"><span data-stu-id="501ce-139">One jumpbox, which restricts access to the private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="501ce-140">A jumpbox, a alkalmazás service, az adatbázis és a VPN-átjáró külön alhálózatokon</span><span class="sxs-lookup"><span data-stu-id="501ce-140">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="501ce-141">NSG kényszeríti az alkalmazás és az adatbázis alhálózatok</span><span class="sxs-lookup"><span data-stu-id="501ce-141">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="501ce-142">Egy NSG házirend/szabály, amely engedélyezi a bejövő TCP-portot 1521 (vagy egy felhasználó által definiált port)</span><span class="sxs-lookup"><span data-stu-id="501ce-142">An NSG policy/rule to allow inbound TCP port 1521 (or a user-defined port)</span></span>
- <span data-ttu-id="501ce-143">Egy NSG/házirendszabály korlátozása csak az IP-cím/címek helyszíni (DB vagy alkalmazás) a virtuális hálózat eléréséhez</span><span class="sxs-lookup"><span data-stu-id="501ce-143">An NSG policy/rule to restrict only the IP address/addresses on-premises (DB or application) to access the virtual network</span></span>

![A vész-Helyreállítási topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a><span data-ttu-id="501ce-145">2. módszer: Telephelyek közötti VPN</span><span class="sxs-lookup"><span data-stu-id="501ce-145">Approach 2: Site-to-site VPN</span></span>
<span data-ttu-id="501ce-146">Telephelyek közötti VPN jobb megközelítés.</span><span class="sxs-lookup"><span data-stu-id="501ce-146">Site-to-site VPN is a better approach.</span></span> <span data-ttu-id="501ce-147">VPN-en beállításával kapcsolatos további információkért lásd: [virtuális hálózat létrehozása a parancssori felület használatával telephelyek közötti VPN-kapcsolattal rendelkező](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="501ce-147">For more information about setting up a VPN, see [Create a virtual network with a Site-to-Site VPN connection using CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span>

#### <a name="topology"></a><span data-ttu-id="501ce-148">topológia</span><span class="sxs-lookup"><span data-stu-id="501ce-148">Topology</span></span>

<span data-ttu-id="501ce-149">Az alábbiakban olvashat az Azure beállítása összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="501ce-149">Following is a summary of the Azure setup:</span></span>

- <span data-ttu-id="501ce-150">Egy vész-Helyreállítási hely</span><span class="sxs-lookup"><span data-stu-id="501ce-150">One DR site</span></span> 
- <span data-ttu-id="501ce-151">Egy virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="501ce-151">One virtual network</span></span> 
- <span data-ttu-id="501ce-152">A Data Guard (aktív) egy Oracle-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="501ce-152">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="501ce-153">A vész-Helyreállítási helyen egy alkalmazásszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="501ce-153">One application service on the DR site</span></span>
- <span data-ttu-id="501ce-154">Egy jumpbox, amely korlátozza a hozzáférést a magánhálózaton, és csak lehetővé teszi a bejelentkezés rendszergazda</span><span class="sxs-lookup"><span data-stu-id="501ce-154">One jumpbox, which restricts access to the private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="501ce-155">Külön alhálózatokon vannak a jumpbox, a szolgáltatás, az adatbázis és a VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="501ce-155">A jumpbox, application service, database, and VPN gateway are on separate subnets</span></span>
- <span data-ttu-id="501ce-156">NSG kényszeríti az alkalmazás és az adatbázis alhálózatok</span><span class="sxs-lookup"><span data-stu-id="501ce-156">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="501ce-157">A helyszíni és az Azure közötti pont-pont VPN-kapcsolat</span><span class="sxs-lookup"><span data-stu-id="501ce-157">Site-to-site VPN connection between on-premises and Azure</span></span>

![A vész-Helyreállítási topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a><span data-ttu-id="501ce-159">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="501ce-159">Additional reading</span></span>

- [<span data-ttu-id="501ce-160">Tervezése és megvalósítása Oracle-adatbázishoz az Azure</span><span class="sxs-lookup"><span data-stu-id="501ce-160">Design and implement an Oracle database on Azure</span></span>](oracle-design.md)
- [<span data-ttu-id="501ce-161">Oracle Data Guard konfigurálása</span><span class="sxs-lookup"><span data-stu-id="501ce-161">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="501ce-162">Oracle Golden kapu beállítása</span><span class="sxs-lookup"><span data-stu-id="501ce-162">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="501ce-163">Oracle biztonsági mentés és helyreállítás</span><span class="sxs-lookup"><span data-stu-id="501ce-163">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)


## <a name="next-steps"></a><span data-ttu-id="501ce-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="501ce-164">Next steps</span></span>

- [<span data-ttu-id="501ce-165">Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="501ce-165">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="501ce-166">Virtuális gép telepítése az Azure parancssori felület minták felfedezése</span><span class="sxs-lookup"><span data-stu-id="501ce-166">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
