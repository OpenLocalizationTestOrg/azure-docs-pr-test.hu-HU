---
title: "Azure virtuális gépek közötti kapcsolati problémák elhárítása |} Microsoft Docs"
description: "Útmutató az Azure virtuális gépek közötti kapcsolódási problémák megoldásáról."
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
ms.openlocfilehash: fd0f25c07cb3f385d3e8480ce1e33dec1ae0a214
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a><span data-ttu-id="01887-103">Azure virtuális gépek közötti kapcsolati problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="01887-103">Troubleshooting connectivity problems between Azure VMs</span></span>

<span data-ttu-id="01887-104">Azure virtuális gépek közötti kapcsolódási problémák merülhetnek fel.</span><span class="sxs-lookup"><span data-stu-id="01887-104">You might experience connectivity problems between Azure VMs.</span></span> <span data-ttu-id="01887-105">Ez a cikk nyújt hibaelhárítási segítséget nyújtanak a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="01887-105">This article provides troubleshooting steps to help you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a><span data-ttu-id="01887-106">Jelenség</span><span class="sxs-lookup"><span data-stu-id="01887-106">Symptom</span></span>

<span data-ttu-id="01887-107">Egy Azure virtuális gép egy másik Azure virtuális gép nem tud kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="01887-107">An Azure VM cannot connect to another Azure VM.</span></span>

## <a name="troubleshooting-guidance"></a><span data-ttu-id="01887-108">Hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="01887-108">Troubleshooting guidance</span></span> 

1. [<span data-ttu-id="01887-109">Ellenőrizze, hogy ha a hálózati adapter helytelenül van konfigurálva</span><span class="sxs-lookup"><span data-stu-id="01887-109">Check if NIC is misconfigured</span></span>](#step-1-check-if-nic-is-misconfigured)
2. [<span data-ttu-id="01887-110">Ha hálózati forgalmát blokkolja NSG-t vagy UDR ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="01887-110">Check if network traffic is blocked by NSG or UDR</span></span>](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [<span data-ttu-id="01887-111">Ha hálózati forgalmát blokkolja a virtuális gép tűzfal ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="01887-111">Check if network traffic is blocked by VM firewall</span></span>](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [<span data-ttu-id="01887-112">Ellenőrizze, hogy virtuális gép alkalmazást vagy szolgáltatást a portot figyel</span><span class="sxs-lookup"><span data-stu-id="01887-112">Check whether VM app or service is listening on the port</span></span>](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [<span data-ttu-id="01887-113">Ellenőrizze, hogy a probléma okozza-e SNAT</span><span class="sxs-lookup"><span data-stu-id="01887-113">Check whether the problem is caused by SNAT</span></span>](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [<span data-ttu-id="01887-114">Ellenőrizze, hogy forgalmat blokkolja hozzáférés-vezérlési listákat a klasszikus virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="01887-114">Check whether traffic is blocked by ACLs for the classic VM</span></span>](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [<span data-ttu-id="01887-115">Ellenőrizze, hogy a végpont jön létre a klasszikus virtuális gép</span><span class="sxs-lookup"><span data-stu-id="01887-115">Check whether the endpoint is created for the classic VM</span></span>](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [<span data-ttu-id="01887-116">Nem sikerült csatlakozni a virtuális gép hálózati megosztáson</span><span class="sxs-lookup"><span data-stu-id="01887-116">Unable to connect to a VM network share</span></span>](#step-8-unable-to-connect-to-a-vm-network-share)
9. [<span data-ttu-id="01887-117">Régión Vnet-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="01887-117">Inter-Vnet connectivity</span></span>](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a><span data-ttu-id="01887-118">Hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="01887-118">Troubleshooting steps</span></span>

<span data-ttu-id="01887-119">Kövesse az alábbi lépéseket a probléma elhárításához.</span><span class="sxs-lookup"><span data-stu-id="01887-119">Follow these steps to troubleshoot the problem.</span></span> <span data-ttu-id="01887-120">Ellenőrizze, hogy a probléma nem oldódik lépések után.</span><span class="sxs-lookup"><span data-stu-id="01887-120">Check whether the problem is resolved after each step.</span></span> 

### <a name="step-1-check-if-nic-is-misconfigured"></a><span data-ttu-id="01887-121">1. lépés: Ellenőrizze, hogy ha a hálózati adapter helytelenül van konfigurálva</span><span class="sxs-lookup"><span data-stu-id="01887-121">Step 1: Check if NIC is misconfigured</span></span>

<span data-ttu-id="01887-122">Hajtsa végre a [hálózati kapcsolat visszaállítása az Azure virtuális gépekhez Windows](../virtual-machines/windows/reset-network-interface.md).</span><span class="sxs-lookup"><span data-stu-id="01887-122">Follow [How to reset network interface for Azure Windows VM](../virtual-machines/windows/reset-network-interface.md).</span></span> 

<span data-ttu-id="01887-123">Ha a probléma akkor fordul elő, a hálózati kártya (NIC) módosítása után, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="01887-123">If the problem occurs after you modify the network interface (NIC), follow these steps:</span></span>

<span data-ttu-id="01887-124">**A hálózati adapter Mulit virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="01887-124">**Mulit-NIC VMs**</span></span>

1. <span data-ttu-id="01887-125">Adja hozzá a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="01887-125">Add a NIC.</span></span>
2. <span data-ttu-id="01887-126">Hárítsa el a problémákat a hibás hálózati adapter, vagy távolítsa el a hibás hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="01887-126">Fix the problems in the bad NIC or remove bad NIC.</span></span>  <span data-ttu-id="01887-127">Majd readd a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="01887-127">Then readd the NIC.</span></span>

<span data-ttu-id="01887-128">További információkért lásd: [adja hozzá a hálózati kapcsolatokat, vagy távolítsa el a virtuális gépek](virtual-network-network-interface-vm.md).</span><span class="sxs-lookup"><span data-stu-id="01887-128">For more information, see [Add network interfaces to or remove from virtual machines](virtual-network-network-interface-vm.md).</span></span>

<span data-ttu-id="01887-129">**Single-NIC VM**</span><span class="sxs-lookup"><span data-stu-id="01887-129">**Single-NIC VM**</span></span> 

- [<span data-ttu-id="01887-130">Telepítse újra a Windows virtuális gép</span><span class="sxs-lookup"><span data-stu-id="01887-130">Redeploy Windows VM</span></span>](../virtual-machines/windows/redeploy-to-new-node.md)
- [<span data-ttu-id="01887-131">Telepítse újra a Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="01887-131">Redeploy Linux VM</span></span>](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a><span data-ttu-id="01887-132">2. lépés: Ellenőrizze, hogy ha hálózati forgalmát blokkolja NSG-t vagy UDR</span><span class="sxs-lookup"><span data-stu-id="01887-132">Step 2: Check if network traffic is blocked by NSG or UDR</span></span>

<span data-ttu-id="01887-133">Használatára [hálózati figyelő IP folyamata ellenőrizze](../network-watcher/network-watcher-ip-flow-verify-overview.md) és [NSG folyamata naplózás](../network-watcher/network-watcher-nsg-flow-logging-overview.md) azonosítását, ha egy hálózati biztonsági csoport vagy a felhasználói útvonalat, amely ütközik az adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="01887-133">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) to identify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span>

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a><span data-ttu-id="01887-134">3. lépés: Ellenőrizze, hogy ha hálózati forgalmát blokkolja a virtuális gép tűzfal</span><span class="sxs-lookup"><span data-stu-id="01887-134">Step 3: Check if network traffic is blocked by VM firewall</span></span>

<span data-ttu-id="01887-135">Tiltsa le a tűzfalat, és tesztelje az eredmény.</span><span class="sxs-lookup"><span data-stu-id="01887-135">Disable the firewall, and then test the result.</span></span> <span data-ttu-id="01887-136">Ha a probléma megoldódott, a tűzfal beállításainak ellenőrzéséhez, és engedélyezze újra.</span><span class="sxs-lookup"><span data-stu-id="01887-136">If the problem is resolved, validate the settings in the firewall and re-enable.</span></span>

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a><span data-ttu-id="01887-137">4. lépés: Ellenőrizze, hogy virtuális gép alkalmazást vagy szolgáltatást a portot figyel</span><span class="sxs-lookup"><span data-stu-id="01887-137">Step 4: Check whether VM app or service is listening on the port</span></span>

<span data-ttu-id="01887-138">Segítségével a következő módszerek egyikével ellenőrizze, hogy virtuális gép alkalmazást vagy szolgáltatást a portot figyel</span><span class="sxs-lookup"><span data-stu-id="01887-138">You can use one of the following methods to check whether VM Application or Service is listening on the port</span></span>

- <span data-ttu-id="01887-139">Futtassa a következő parancsok futtatásával ellenőrizze, hogy a kiszolgáló adott portot figyel.</span><span class="sxs-lookup"><span data-stu-id="01887-139">Run the following commands to check whether the server is listening on that port.</span></span>

<span data-ttu-id="01887-140">**Windows rendszerű virtuális gép**</span><span class="sxs-lookup"><span data-stu-id="01887-140">**Windows VM**</span></span>

    netstat –ano

<span data-ttu-id="01887-141">**Linux rendszerű virtuális gép**</span><span class="sxs-lookup"><span data-stu-id="01887-141">**Linux VM**</span></span>

    netstat -l

- <span data-ttu-id="01887-142">Futtassa a **Telnet** parancs a virtuális gép fel saját maga a port teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="01887-142">Run the **Telnet** command on the VM to itself to test the port.</span></span> <span data-ttu-id="01887-143">A teszt sikertelen, alkalmazás vagy szolgáltatás beállítása nem lehet figyelni a porton.</span><span class="sxs-lookup"><span data-stu-id="01887-143">If the test fails, application or service is not configured to listen on the port.</span></span>

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a><span data-ttu-id="01887-144">5. lépés: Ellenőrizze, hogy a probléma okozza-e SNAT</span><span class="sxs-lookup"><span data-stu-id="01887-144">Step 5: Check whether the problem is caused by SNAT</span></span>

<span data-ttu-id="01887-145">Bizonyos esetekben a virtuális gép el van helyezve a terhelés elosztása megoldás, amely az Azure-on kívüli erőforrások mögött.</span><span class="sxs-lookup"><span data-stu-id="01887-145">In some scenarios, the VM is placed behind a Load balance solution that has a dependency on resources outside of Azure.</span></span> <span data-ttu-id="01887-146">Ezekben az esetekben időszakos kapcsolódási problémák léptek fel, ha a probléma oka lehet [SNAT port Erőforrásfogyás](../load-balancer/load-balancer-outbound-connections.md).</span><span class="sxs-lookup"><span data-stu-id="01887-146">In these scenarios, if you experience intermittent connection problems, the problem may be caused by [SNAT port exhaustion](../load-balancer/load-balancer-outbound-connections.md).</span></span> <span data-ttu-id="01887-147">A probléma megoldásához, VIP (vagy létrehozása a klasszikus ILPIP) az egyes virtuális gépek, amelyek a terheléselosztó mögött van, és az NSG-t vagy a hozzáférés-vezérlési lista biztonságáról.</span><span class="sxs-lookup"><span data-stu-id="01887-147">To resolve the issue, create a VIP (or ILPIP for classic) for each VM that is behind the Load balancer and secure with NSG or ACL.</span></span> 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a><span data-ttu-id="01887-148">6. lépés: Ellenőrizze, hogy forgalmat blokkolja hozzáférés-vezérlési listákat a klasszikus virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="01887-148">Step 6: Check whether traffic is blocked by ACLs for the classic VM</span></span>

<span data-ttu-id="01887-149">Hozzáférés-vezérlési Listában lehetővé teszi a szelektív módon engedélyezheti vagy megtagadhatja a forgalom egy virtuális gép végpont számára.</span><span class="sxs-lookup"><span data-stu-id="01887-149">An ACL provides the ability to selectively permit or deny traffic for a virtual machine endpoint.</span></span> <span data-ttu-id="01887-150">További információkért lásd: [végponti ACL kezelése](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span><span class="sxs-lookup"><span data-stu-id="01887-150">For more information, see [Manage the ACL on an endpoint](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span></span>

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a><span data-ttu-id="01887-151">7. lépés: Ellenőrizze, hogy a végpont jön létre a klasszikus virtuális gép</span><span class="sxs-lookup"><span data-stu-id="01887-151">Step 7: Check whether the endpoint is created for the classic VM</span></span>

<span data-ttu-id="01887-152">Az Azure klasszikus telepítési modellel létrehozott összes virtuális gép automatikusan kommunikálhat más virtuális gépekkel a felhőalapú szolgáltatás- vagy virtuális hálózati magánhálózati csatornán keresztül.</span><span class="sxs-lookup"><span data-stu-id="01887-152">All VMs that you create in Azure using the classic deployment model can automatically communicate over a private network channel with other virtual machines in the same cloud service or virtual network.</span></span> <span data-ttu-id="01887-153">Azonban más virtuális hálózatokon lévő számítógépek számára szükséges végpontok át tudja irányítani a bejövő hálózati forgalmat a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="01887-153">However, computers on other virtual networks require endpoints to direct the inbound network traffic to a virtual machine.</span></span> <span data-ttu-id="01887-154">További információkért lásd: [végpontok beállítása](../virtual-machines/windows/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="01887-154">For more information, see [How to set up endpoints](../virtual-machines/windows/classic/setup-endpoints.md).</span></span>

### <a name="step-8-unable-to-connect-to-a-vm-network-share"></a><span data-ttu-id="01887-155">8. lépés: Nem sikerült csatlakozni a virtuális gép hálózati megosztáson</span><span class="sxs-lookup"><span data-stu-id="01887-155">Step 8: Unable to connect to a VM network share</span></span>

<span data-ttu-id="01887-156">Ha nem sikerül kapcsolódni a virtuális gép hálózati megosztásra, a problémát okozhatja a virtuális hálózati adapter nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="01887-156">If you are unable to connect to a VM network share, the problem can be caused by unavailable NICs in the VM.</span></span> <span data-ttu-id="01887-157">Törölje a nem elérhető hálózati adapterek, lásd: [az elérhető hálózati adapterek törlése](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span><span class="sxs-lookup"><span data-stu-id="01887-157">To delete the unavailable NICs, see [How to delete the unavailable NICs](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span></span>

### <a name="step-9-inter-vnet-connectivity"></a><span data-ttu-id="01887-158">9. lépés: Virtuális közötti Vnet-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="01887-158">Step 9: Inter-Vnet connectivity</span></span>

<span data-ttu-id="01887-159">Használatára [hálózati figyelő IP folyamata ellenőrizze](../network-watcher/network-watcher-ip-flow-verify-overview.md) és [NSG folyamata naplózás](../network-watcher/network-watcher-nsg-flow-logging-overview.md) azonosítását, ha egy hálózati biztonsági csoport vagy a felhasználói útvonalat, amely ütközik az adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="01887-159">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) to identify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span> <span data-ttu-id="01887-160">Az Inter-virtuális hálózat konfigurációt is ellenőrizheti [Itt](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span><span class="sxs-lookup"><span data-stu-id="01887-160">You can also validate your Inter-Vnet configuration [here](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span></span>

### <a name="need-help-contact-support"></a><span data-ttu-id="01887-161">Segítség</span><span class="sxs-lookup"><span data-stu-id="01887-161">Need help?</span></span> <span data-ttu-id="01887-162">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="01887-162">Contact support.</span></span>
<span data-ttu-id="01887-163">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="01887-163">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>