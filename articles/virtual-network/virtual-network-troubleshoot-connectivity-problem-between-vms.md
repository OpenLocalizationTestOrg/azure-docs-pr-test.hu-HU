---
title: "Azure virtuális gépek közötti aaaTroubleshooting kapcsolódási problémái vannak |} Microsoft Docs"
description: "Ismerje meg, hogyan tooTroubleshoot hello Azure virtuális gépek közötti kapcsolódási problémái vannak."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: genli
ms.openlocfilehash: ee0819178dcbee2bf529a495758e6c33f49e085e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a><span data-ttu-id="44cc9-103">Azure virtuális gépek közötti kapcsolati problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="44cc9-103">Troubleshooting connectivity problems between Azure VMs</span></span>

<span data-ttu-id="44cc9-104">Azure virtuális gépek közötti kapcsolódási problémák merülhetnek fel.</span><span class="sxs-lookup"><span data-stu-id="44cc9-104">You might experience connectivity problems between Azure VMs.</span></span> <span data-ttu-id="44cc9-105">Ez a cikk ismerteti a hibaelhárítási lépéseket toohelp a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="44cc9-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a><span data-ttu-id="44cc9-106">Jelenség</span><span class="sxs-lookup"><span data-stu-id="44cc9-106">Symptom</span></span>

<span data-ttu-id="44cc9-107">Egy Azure virtuális gép nem csatlakoztatható tooanother Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="44cc9-107">An Azure VM cannot connect tooanother Azure VM.</span></span>

## <a name="troubleshooting-guidance"></a><span data-ttu-id="44cc9-108">Hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="44cc9-108">Troubleshooting guidance</span></span> 

1. [<span data-ttu-id="44cc9-109">Ellenőrizze, hogy ha a hálózati adapter helytelenül van konfigurálva</span><span class="sxs-lookup"><span data-stu-id="44cc9-109">Check if NIC is misconfigured</span></span>](#step-1-check-if-nic-is-misconfigured)
2. [<span data-ttu-id="44cc9-110">Ha hálózati forgalmát blokkolja NSG-t vagy UDR ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="44cc9-110">Check if network traffic is blocked by NSG or UDR</span></span>](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [<span data-ttu-id="44cc9-111">Ha hálózati forgalmát blokkolja a virtuális gép tűzfal ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="44cc9-111">Check if network traffic is blocked by VM firewall</span></span>](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [<span data-ttu-id="44cc9-112">Ellenőrizze, hogy virtuális gép alkalmazás vagy szolgáltatás hello portot figyel</span><span class="sxs-lookup"><span data-stu-id="44cc9-112">Check whether VM app or service is listening on hello port</span></span>](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [<span data-ttu-id="44cc9-113">Ellenőrizze, hogy hello probléma okozza-e SNAT</span><span class="sxs-lookup"><span data-stu-id="44cc9-113">Check whether hello problem is caused by SNAT</span></span>](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [<span data-ttu-id="44cc9-114">Ellenőrizze, hogy forgalmat blokkolja az ACL-ek hello klasszikus virtuális gép</span><span class="sxs-lookup"><span data-stu-id="44cc9-114">Check whether traffic is blocked by ACLs for hello classic VM</span></span>](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [<span data-ttu-id="44cc9-115">Ellenőrizze, hogy hello végpont kerül létrehozásra az hello klasszikus virtuális gép</span><span class="sxs-lookup"><span data-stu-id="44cc9-115">Check whether hello endpoint is created for hello classic VM</span></span>](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [<span data-ttu-id="44cc9-116">Nem lehet tooconnect tooa VM hálózati megosztáson</span><span class="sxs-lookup"><span data-stu-id="44cc9-116">Unable tooconnect tooa VM network share</span></span>](#step-8-unable-to-connect-to-a-vm-network-share)
9. [<span data-ttu-id="44cc9-117">Régión Vnet-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="44cc9-117">Inter-Vnet connectivity</span></span>](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a><span data-ttu-id="44cc9-118">Hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="44cc9-118">Troubleshooting steps</span></span>

<span data-ttu-id="44cc9-119">Kövesse a lépéseket tootroubleshoot hello probléma.</span><span class="sxs-lookup"><span data-stu-id="44cc9-119">Follow these steps tootroubleshoot hello problem.</span></span> <span data-ttu-id="44cc9-120">Ellenőrizze, hogy hello a probléma nem oldódik lépések után.</span><span class="sxs-lookup"><span data-stu-id="44cc9-120">Check whether hello problem is resolved after each step.</span></span> 

### <a name="step-1-check-if-nic-is-misconfigured"></a><span data-ttu-id="44cc9-121">1. lépés: Ellenőrizze, hogy ha a hálózati adapter helytelenül van konfigurálva</span><span class="sxs-lookup"><span data-stu-id="44cc9-121">Step 1: Check if NIC is misconfigured</span></span>

<span data-ttu-id="44cc9-122">Hajtsa végre a [hogyan tooreset a hálózati kapcsolat az Azure virtuális gépekhez Windows](../virtual-machines/windows/reset-network-interface.md).</span><span class="sxs-lookup"><span data-stu-id="44cc9-122">Follow [How tooreset network interface for Azure Windows VM](../virtual-machines/windows/reset-network-interface.md).</span></span> 

<span data-ttu-id="44cc9-123">Ha a probléma hello hello hálózati illesztőt (NIC) módosítása után következik be, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="44cc9-123">If hello problem occurs after you modify hello network interface (NIC), follow these steps:</span></span>

<span data-ttu-id="44cc9-124">**A hálózati adapter Mulit virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="44cc9-124">**Mulit-NIC VMs**</span></span>

1. <span data-ttu-id="44cc9-125">Adja hozzá a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="44cc9-125">Add a NIC.</span></span>
2. <span data-ttu-id="44cc9-126">Javítsa ki a hello problémákat hello hibás, vagy távolítsa el a hálózati adapter hibás hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="44cc9-126">Fix hello problems in hello bad NIC or remove bad NIC.</span></span>  <span data-ttu-id="44cc9-127">Majd readd hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="44cc9-127">Then readd hello NIC.</span></span>

<span data-ttu-id="44cc9-128">További információkért lásd: [Hozzáadás hálózati illesztők tooor távolítsa el a virtuális gépek](virtual-network-network-interface-vm.md).</span><span class="sxs-lookup"><span data-stu-id="44cc9-128">For more information, see [Add network interfaces tooor remove from virtual machines](virtual-network-network-interface-vm.md).</span></span>

<span data-ttu-id="44cc9-129">**Single-NIC VM**</span><span class="sxs-lookup"><span data-stu-id="44cc9-129">**Single-NIC VM**</span></span> 

- [<span data-ttu-id="44cc9-130">Telepítse újra a Windows virtuális gép</span><span class="sxs-lookup"><span data-stu-id="44cc9-130">Redeploy Windows VM</span></span>](../virtual-machines/windows/redeploy-to-new-node.md)
- [<span data-ttu-id="44cc9-131">Telepítse újra a Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="44cc9-131">Redeploy Linux VM</span></span>](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a><span data-ttu-id="44cc9-132">2. lépés: Ellenőrizze, hogy ha hálózati forgalmát blokkolja NSG-t vagy UDR</span><span class="sxs-lookup"><span data-stu-id="44cc9-132">Step 2: Check if network traffic is blocked by NSG or UDR</span></span>

<span data-ttu-id="44cc9-133">Használatára [hálózati figyelő IP folyamata ellenőrizze](../network-watcher/network-watcher-ip-flow-verify-overview.md) és [NSG folyamata naplózás](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, ha a hálózati biztonsági csoport vagy felhasználói útvonalat, amely ütközik az adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="44cc9-133">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span>

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a><span data-ttu-id="44cc9-134">3. lépés: Ellenőrizze, hogy ha hálózati forgalmát blokkolja a virtuális gép tűzfal</span><span class="sxs-lookup"><span data-stu-id="44cc9-134">Step 3: Check if network traffic is blocked by VM firewall</span></span>

<span data-ttu-id="44cc9-135">Tiltsa le a hello tűzfal, és tesztelje a hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="44cc9-135">Disable hello firewall, and then test hello result.</span></span> <span data-ttu-id="44cc9-136">Hello probléma megoldódott, ha a tűzfal hello hello-beállítások érvényesítése, és engedélyezze újra.</span><span class="sxs-lookup"><span data-stu-id="44cc9-136">If hello problem is resolved, validate hello settings in hello firewall and re-enable.</span></span>

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-hello-port"></a><span data-ttu-id="44cc9-137">4. lépés: Ellenőrizze, hogy virtuális gép alkalmazás vagy szolgáltatás hello portot figyel</span><span class="sxs-lookup"><span data-stu-id="44cc9-137">Step 4: Check whether VM app or service is listening on hello port</span></span>

<span data-ttu-id="44cc9-138">Hogy a virtuális gép alkalmazás vagy szolgáltatás hello porton figyel a következő módszerek toocheck hello egyikét használhatja</span><span class="sxs-lookup"><span data-stu-id="44cc9-138">You can use one of hello following methods toocheck whether VM Application or Service is listening on hello port</span></span>

- <span data-ttu-id="44cc9-139">A következő futtatási hello toocheck parancsokat, hogy hello server adott portot figyel.</span><span class="sxs-lookup"><span data-stu-id="44cc9-139">Run hello following commands toocheck whether hello server is listening on that port.</span></span>

<span data-ttu-id="44cc9-140">**Windows rendszerű virtuális gép**</span><span class="sxs-lookup"><span data-stu-id="44cc9-140">**Windows VM**</span></span>

    netstat –ano

<span data-ttu-id="44cc9-141">**Linux rendszerű virtuális gép**</span><span class="sxs-lookup"><span data-stu-id="44cc9-141">**Linux VM**</span></span>

    netstat -l

- <span data-ttu-id="44cc9-142">Futtassa a hello **Telnet** hello VM tooitself tootest hello port parancs.</span><span class="sxs-lookup"><span data-stu-id="44cc9-142">Run hello **Telnet** command on hello VM tooitself tootest hello port.</span></span> <span data-ttu-id="44cc9-143">Hello teszt sikertelen, alkalmazás vagy szolgáltatás nincs beállított toolisten hello porton.</span><span class="sxs-lookup"><span data-stu-id="44cc9-143">If hello test fails, application or service is not configured toolisten on hello port.</span></span>

### <a name="step-5-check-whether-hello-problem-is-caused-by-snat"></a><span data-ttu-id="44cc9-144">5. lépés: Ellenőrizze, hogy hello probléma okozza-e SNAT</span><span class="sxs-lookup"><span data-stu-id="44cc9-144">Step 5: Check whether hello problem is caused by SNAT</span></span>

<span data-ttu-id="44cc9-145">Bizonyos esetekben hello VM mögé kerül, amely az Azure-on kívüli erőforrások Load balance megoldást.</span><span class="sxs-lookup"><span data-stu-id="44cc9-145">In some scenarios, hello VM is placed behind a Load balance solution that has a dependency on resources outside of Azure.</span></span> <span data-ttu-id="44cc9-146">Ezekben az esetekben ha időszakos kapcsolattal problémák hello probléma okozhatja [SNAT port Erőforrásfogyás](../load-balancer/load-balancer-outbound-connections.md).</span><span class="sxs-lookup"><span data-stu-id="44cc9-146">In these scenarios, if you experience intermittent connection problems, hello problem may be caused by [SNAT port exhaustion](../load-balancer/load-balancer-outbound-connections.md).</span></span> <span data-ttu-id="44cc9-147">tooresolve hello probléma, az egyes virtuális gépekhez, amely VIP (vagy a klasszikus ILPIP) létrehozása hello terheléselosztó mögött, és az NSG-t vagy a hozzáférés-vezérlési lista biztonságáról.</span><span class="sxs-lookup"><span data-stu-id="44cc9-147">tooresolve hello issue, create a VIP (or ILPIP for classic) for each VM that is behind hello Load balancer and secure with NSG or ACL.</span></span> 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-hello-classic-vm"></a><span data-ttu-id="44cc9-148">6. lépés: Ellenőrizze hogy forgalmat blokkolja az ACL-ek hello klasszikus virtuális gép</span><span class="sxs-lookup"><span data-stu-id="44cc9-148">Step 6: Check whether traffic is blocked by ACLs for hello classic VM</span></span>

<span data-ttu-id="44cc9-149">Hozzáférés-vezérlési Listában lehetővé teszi a hello képességét tooselectively engedély, vagy letilthatja a forgalmat a virtuális gép végpont.</span><span class="sxs-lookup"><span data-stu-id="44cc9-149">An ACL provides hello ability tooselectively permit or deny traffic for a virtual machine endpoint.</span></span> <span data-ttu-id="44cc9-150">További információkért lásd: [kezelése hello ACL a végpont](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span><span class="sxs-lookup"><span data-stu-id="44cc9-150">For more information, see [Manage hello ACL on an endpoint](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span></span>

### <a name="step-7-check-whether-hello-endpoint-is-created-for-hello-classic-vm"></a><span data-ttu-id="44cc9-151">7. lépés: Ellenőrizze, hogy hello végpont kerül létrehozásra az hello klasszikus virtuális gép</span><span class="sxs-lookup"><span data-stu-id="44cc9-151">Step 7: Check whether hello endpoint is created for hello classic VM</span></span>

<span data-ttu-id="44cc9-152">Az Azure-ban hello klasszikus üzembe helyezési modellel létrehozott összes virtuális gép automatikusan kommunikálhatnak más virtuális gépekkel hello a magánhálózat-csatornán keresztül azonos felhőalapú szolgáltatás, vagy a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="44cc9-152">All VMs that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="44cc9-153">Azonban más virtuális hálózatokon lévő számítógépek számára szükséges végpontok toodirect hello bejövő hálózati forgalom tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="44cc9-153">However, computers on other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="44cc9-154">További információkért lásd: [hogyan mentése végpontok tooset](../virtual-machines/windows/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="44cc9-154">For more information, see [How tooset up endpoints](../virtual-machines/windows/classic/setup-endpoints.md).</span></span>

### <a name="step-8-unable-tooconnect-tooa-vm-network-share"></a><span data-ttu-id="44cc9-155">8. lépés: Nem tooconnect tooa VM hálózati megosztáson</span><span class="sxs-lookup"><span data-stu-id="44cc9-155">Step 8: Unable tooconnect tooa VM network share</span></span>

<span data-ttu-id="44cc9-156">Ha nem tooconnect tooa VM hálózati megosztást, hello probléma okozhatja hello VM hálózati adapter nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="44cc9-156">If you are unable tooconnect tooa VM network share, hello problem can be caused by unavailable NICs in hello VM.</span></span> <span data-ttu-id="44cc9-157">toodelete nem érhető el a hálózati adapterek hello című [hogyan toodelete hello nem érhető el a hálózati adapterek](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span><span class="sxs-lookup"><span data-stu-id="44cc9-157">toodelete hello unavailable NICs, see [How toodelete hello unavailable NICs](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span></span>

### <a name="step-9-inter-vnet-connectivity"></a><span data-ttu-id="44cc9-158">9. lépés: Virtuális közötti Vnet-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="44cc9-158">Step 9: Inter-Vnet connectivity</span></span>

<span data-ttu-id="44cc9-159">Használatára [hálózati figyelő IP folyamata ellenőrizze](../network-watcher/network-watcher-ip-flow-verify-overview.md) és [NSG folyamata naplózás](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, ha a hálózati biztonsági csoport vagy felhasználói útvonalat, amely ütközik az adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="44cc9-159">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span> <span data-ttu-id="44cc9-160">Az Inter-virtuális hálózat konfigurációt is ellenőrizheti [Itt](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span><span class="sxs-lookup"><span data-stu-id="44cc9-160">You can also validate your Inter-Vnet configuration [here](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span></span>

### <a name="need-help-contact-support"></a><span data-ttu-id="44cc9-161">Segítség</span><span class="sxs-lookup"><span data-stu-id="44cc9-161">Need help?</span></span> <span data-ttu-id="44cc9-162">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="44cc9-162">Contact support.</span></span>
<span data-ttu-id="44cc9-163">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.</span><span class="sxs-lookup"><span data-stu-id="44cc9-163">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
