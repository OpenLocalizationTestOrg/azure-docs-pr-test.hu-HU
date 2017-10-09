---
title: "az Azure-alapú környezetben az Oracle katasztrófa utáni helyreállítása aaaOverview |} Microsoft Docs"
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
ms.openlocfilehash: 1fa69e1ba044b46b27695fec92fd9ca82df796f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a><span data-ttu-id="bdc07-103">Oracle Database 12c adatbázis Azure környezetben vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="bdc07-103">Disaster recovery for an Oracle Database 12c database in an Azure environment</span></span>

## <a name="assumptions"></a><span data-ttu-id="bdc07-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bdc07-104">Assumptions</span></span>

- <span data-ttu-id="bdc07-105">Oracle Data Guard tervezési és az Azure-környezetek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bdc07-105">You have an understanding of Oracle Data Guard design and Azure environments.</span></span>


## <a name="goals"></a><span data-ttu-id="bdc07-106">Célok</span><span class="sxs-lookup"><span data-stu-id="bdc07-106">Goals</span></span>
- <span data-ttu-id="bdc07-107">A kialakítási hello topológia és konfigurációt, amely a vész-helyreállítási követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="bdc07-107">Design hello topology and configuration that meet your disaster recovery (DR) requirements.</span></span>

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a><span data-ttu-id="bdc07-108">1. forgatókönyv: Elsődleges és a vész-Helyreállítási helyeken az Azure-on</span><span class="sxs-lookup"><span data-stu-id="bdc07-108">Scenario 1: Primary and DR sites on Azure</span></span>

<span data-ttu-id="bdc07-109">Az ügyfél rendelkezik egy Oracle adatbázis-készlet mentése hello elsődleges helyen.</span><span class="sxs-lookup"><span data-stu-id="bdc07-109">A customer has an Oracle database set up on hello primary site.</span></span> <span data-ttu-id="bdc07-110">A vész-Helyreállítási hely van egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="bdc07-110">A DR site is in a different region.</span></span> <span data-ttu-id="bdc07-111">hello ügyfél Oracle Data Guard használja a helyek közötti gyors helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="bdc07-111">hello customer uses Oracle Data Guard for quick recovery between these sites.</span></span> <span data-ttu-id="bdc07-112">hello elsődleges helyet is rendelkezik, egy másodlagos adatbázis jelentéskészítéshez és egyéb felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="bdc07-112">hello primary site also has a secondary database for reporting and other uses.</span></span> 

### <a name="topology"></a><span data-ttu-id="bdc07-113">topológia</span><span class="sxs-lookup"><span data-stu-id="bdc07-113">Topology</span></span>

<span data-ttu-id="bdc07-114">Íme az Azure beállítása hello összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="bdc07-114">Here is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="bdc07-115">Két hely (elsődleges hely és a vész-Helyreállítási hely)</span><span class="sxs-lookup"><span data-stu-id="bdc07-115">Two sites (a primary site and a DR site)</span></span>
- <span data-ttu-id="bdc07-116">Két virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="bdc07-116">Two virtual networks</span></span>
- <span data-ttu-id="bdc07-117">Két adatbázisokkal Oracle Data Guard (elsődleges és készenléti állapot)</span><span class="sxs-lookup"><span data-stu-id="bdc07-117">Two Oracle databases with Data Guard (primary and standby)</span></span>
- <span data-ttu-id="bdc07-118">Két Oracle adatbázis Golden kapu vagy Data Guard (csak az elsődleges hely)</span><span class="sxs-lookup"><span data-stu-id="bdc07-118">Two Oracle databases with Golden Gate or Data Guard (primary site only)</span></span>
- <span data-ttu-id="bdc07-119">Két alkalmazás szolgáltatást, egy elsődleges és egy hello vész-Helyreállítási helyen</span><span class="sxs-lookup"><span data-stu-id="bdc07-119">Two application services, one primary and one on hello DR site</span></span>
- <span data-ttu-id="bdc07-120">Egy *rendelkezésre állási csoport* hello elsődleges helyen adatbázis és a alkalmazás service szolgál</span><span class="sxs-lookup"><span data-stu-id="bdc07-120">An *availability set,* which is used for database and application service on hello primary site</span></span>
- <span data-ttu-id="bdc07-121">Minden helyen, amely korlátozza a hozzáférést toohello magánhálózati, és csak a bejelentkezés egy rendszergazda lehetővé teszi egy jumpbox</span><span class="sxs-lookup"><span data-stu-id="bdc07-121">One jumpbox on each site, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="bdc07-122">A jumpbox, a alkalmazás service, az adatbázis és a VPN-átjáró külön alhálózatokon</span><span class="sxs-lookup"><span data-stu-id="bdc07-122">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="bdc07-123">NSG kényszeríti az alkalmazás és az adatbázis alhálózatok</span><span class="sxs-lookup"><span data-stu-id="bdc07-123">NSG enforced on application and database subnets</span></span>

![Hello DR topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a><span data-ttu-id="bdc07-125">2. forgatókönyv: Helyszíni elsődleges hely és az Azure-on vész-Helyreállítási hely</span><span class="sxs-lookup"><span data-stu-id="bdc07-125">Scenario 2: Primary site on-premises and DR site on Azure</span></span>

<span data-ttu-id="bdc07-126">Az ügyfél rendelkezik egy helyszíni Oracle adatbázis-beállítások (elsődleges helyen).</span><span class="sxs-lookup"><span data-stu-id="bdc07-126">A customer has an on-premises Oracle database setup (primary site).</span></span> <span data-ttu-id="bdc07-127">A vész-Helyreállítási hely van, az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="bdc07-127">A DR site is on Azure.</span></span> <span data-ttu-id="bdc07-128">Oracle Data Guard szolgál a helyek közötti gyors helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="bdc07-128">Oracle Data Guard is used for quick recovery between these sites.</span></span> <span data-ttu-id="bdc07-129">hello elsődleges helyet is rendelkezik, egy másodlagos adatbázis jelentéskészítéshez és egyéb felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="bdc07-129">hello primary site also has a secondary database for reporting and other uses.</span></span> 

<span data-ttu-id="bdc07-130">Kétféleképpen a telepítés.</span><span class="sxs-lookup"><span data-stu-id="bdc07-130">There are two approaches for this setup.</span></span>

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-hello-firewall"></a><span data-ttu-id="bdc07-131">1. módszer: A helyszíni és az Azure, a hello tűzfal nyitott TCP-portok igénylő közti közvetlen kapcsolatokon keresztül</span><span class="sxs-lookup"><span data-stu-id="bdc07-131">Approach 1: Direct connections between on-premises and Azure, requiring open TCP ports on hello firewall</span></span> 

<span data-ttu-id="bdc07-132">Közvetlen kapcsolatok nem ajánlott, mivel ezek az hello TCP-portok toohello globális kívül.</span><span class="sxs-lookup"><span data-stu-id="bdc07-132">We don't recommend direct connections because they expose hello TCP ports toohello outside world.</span></span>

#### <a name="topology"></a><span data-ttu-id="bdc07-133">topológia</span><span class="sxs-lookup"><span data-stu-id="bdc07-133">Topology</span></span>

<span data-ttu-id="bdc07-134">Az alábbiakban olvashat az Azure beállítása hello összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="bdc07-134">Following is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="bdc07-135">Egy vész-Helyreállítási hely</span><span class="sxs-lookup"><span data-stu-id="bdc07-135">One DR site</span></span> 
- <span data-ttu-id="bdc07-136">Egy virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="bdc07-136">One virtual network</span></span>
- <span data-ttu-id="bdc07-137">A Data Guard (aktív) egy Oracle-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="bdc07-137">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="bdc07-138">A vész-Helyreállítási hello helyen egy alkalmazásszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="bdc07-138">One application service on hello DR site</span></span>
- <span data-ttu-id="bdc07-139">Egy jumpbox, amely korlátozza a hozzáférést toohello magánhálózati, és csak lehetővé teszi a bejelentkezés egy rendszergazda</span><span class="sxs-lookup"><span data-stu-id="bdc07-139">One jumpbox, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="bdc07-140">A jumpbox, a alkalmazás service, az adatbázis és a VPN-átjáró külön alhálózatokon</span><span class="sxs-lookup"><span data-stu-id="bdc07-140">A jumpbox, application service, database, and VPN gateway on separate subnets</span></span>
- <span data-ttu-id="bdc07-141">NSG kényszeríti az alkalmazás és az adatbázis alhálózatok</span><span class="sxs-lookup"><span data-stu-id="bdc07-141">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="bdc07-142">Az NSG házirendszabály/tooallow bejövő TCP-port 1521 (vagy egy felhasználó által definiált port)</span><span class="sxs-lookup"><span data-stu-id="bdc07-142">An NSG policy/rule tooallow inbound TCP port 1521 (or a user-defined port)</span></span>
- <span data-ttu-id="bdc07-143">Az NSG házirendszabály/toorestrict csak hello IP-cím/címek helyszíni (DB vagy alkalmazás) tooaccess hello virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="bdc07-143">An NSG policy/rule toorestrict only hello IP address/addresses on-premises (DB or application) tooaccess hello virtual network</span></span>

![Hello DR topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a><span data-ttu-id="bdc07-145">2. módszer: Telephelyek közötti VPN</span><span class="sxs-lookup"><span data-stu-id="bdc07-145">Approach 2: Site-to-site VPN</span></span>
<span data-ttu-id="bdc07-146">Telephelyek közötti VPN jobb megközelítés.</span><span class="sxs-lookup"><span data-stu-id="bdc07-146">Site-to-site VPN is a better approach.</span></span> <span data-ttu-id="bdc07-147">VPN-en beállításával kapcsolatos további információkért lásd: [virtuális hálózat létrehozása a parancssori felület használatával telephelyek közötti VPN-kapcsolattal rendelkező](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="bdc07-147">For more information about setting up a VPN, see [Create a virtual network with a Site-to-Site VPN connection using CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span>

#### <a name="topology"></a><span data-ttu-id="bdc07-148">topológia</span><span class="sxs-lookup"><span data-stu-id="bdc07-148">Topology</span></span>

<span data-ttu-id="bdc07-149">Az alábbiakban olvashat az Azure beállítása hello összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="bdc07-149">Following is a summary of hello Azure setup:</span></span>

- <span data-ttu-id="bdc07-150">Egy vész-Helyreállítási hely</span><span class="sxs-lookup"><span data-stu-id="bdc07-150">One DR site</span></span> 
- <span data-ttu-id="bdc07-151">Egy virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="bdc07-151">One virtual network</span></span> 
- <span data-ttu-id="bdc07-152">A Data Guard (aktív) egy Oracle-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="bdc07-152">One Oracle database with Data Guard (active)</span></span>
- <span data-ttu-id="bdc07-153">A vész-Helyreállítási hello helyen egy alkalmazásszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="bdc07-153">One application service on hello DR site</span></span>
- <span data-ttu-id="bdc07-154">Egy jumpbox, amely korlátozza a hozzáférést toohello magánhálózati, és csak lehetővé teszi a bejelentkezés egy rendszergazda</span><span class="sxs-lookup"><span data-stu-id="bdc07-154">One jumpbox, which restricts access toohello private network and only allows sign-in by an administrator</span></span>
- <span data-ttu-id="bdc07-155">Külön alhálózatokon vannak a jumpbox, a szolgáltatás, az adatbázis és a VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="bdc07-155">A jumpbox, application service, database, and VPN gateway are on separate subnets</span></span>
- <span data-ttu-id="bdc07-156">NSG kényszeríti az alkalmazás és az adatbázis alhálózatok</span><span class="sxs-lookup"><span data-stu-id="bdc07-156">NSG enforced on application and database subnets</span></span>
- <span data-ttu-id="bdc07-157">A helyszíni és az Azure közötti pont-pont VPN-kapcsolat</span><span class="sxs-lookup"><span data-stu-id="bdc07-157">Site-to-site VPN connection between on-premises and Azure</span></span>

![Hello DR topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a><span data-ttu-id="bdc07-159">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="bdc07-159">Additional reading</span></span>

- [<span data-ttu-id="bdc07-160">Tervezése és megvalósítása Oracle-adatbázishoz az Azure</span><span class="sxs-lookup"><span data-stu-id="bdc07-160">Design and implement an Oracle database on Azure</span></span>](oracle-design.md)
- [<span data-ttu-id="bdc07-161">Oracle Data Guard konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bdc07-161">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="bdc07-162">Oracle Golden kapu beállítása</span><span class="sxs-lookup"><span data-stu-id="bdc07-162">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="bdc07-163">Oracle biztonsági mentés és helyreállítás</span><span class="sxs-lookup"><span data-stu-id="bdc07-163">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)


## <a name="next-steps"></a><span data-ttu-id="bdc07-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdc07-164">Next steps</span></span>

- [<span data-ttu-id="bdc07-165">Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="bdc07-165">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="bdc07-166">Virtuális gép telepítése az Azure parancssori felület minták felfedezése</span><span class="sxs-lookup"><span data-stu-id="bdc07-166">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
