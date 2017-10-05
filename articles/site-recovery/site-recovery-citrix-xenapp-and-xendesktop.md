---
title: "Egy Azure Site Recovery segítségével többrétegű Citrix XenDesktop és XenApp telepítési replikálni |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan védelme és helyreállítása Azure Site Recovery segítségével Citrix XenDesktop és XenApp telepítések."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: ponatara
ms.openlocfilehash: dc064352b1841ff346b705dc63186b12d79350b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a><span data-ttu-id="219f3-103">Egy Azure Site Recovery segítségével többrétegű Citrix XenApp és XenDesktop telepítési replikálása</span><span class="sxs-lookup"><span data-stu-id="219f3-103">Replicate a multi-tier Citrix XenApp and XenDesktop deployment using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="219f3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="219f3-104">Overview</span></span>

<span data-ttu-id="219f3-105">Citrix XenDesktop egy asztali virtualizálási megoldás, amely asztali környezetek és alkalmazások ondemand-szolgáltatásként juttatja bárhonnan bármely felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="219f3-105">Citrix XenDesktop is a desktop virtualization solution that delivers desktops and applications as an ondemand service to any user, anywhere.</span></span> <span data-ttu-id="219f3-106">A FlexCast kézbesítési technológia XenDesktop gyorsan és biztonságosan biztosíthat alkalmazásokhoz és az asztali a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="219f3-106">With FlexCast delivery technology, XenDesktop can quickly and securely deliver applications and desktops to users.</span></span>
<span data-ttu-id="219f3-107">Ma, Citrix XenApp nem minden vész helyreállítási képességeket biztosítják.</span><span class="sxs-lookup"><span data-stu-id="219f3-107">Today, Citrix XenApp does not provide any disaster recovery capabilities.</span></span>

<span data-ttu-id="219f3-108">Egy jó vész-helyreállítási megoldást lehetővé kell tennie, hogy a fenti körül helyreállítási tervek modellezési összetett alkalmazási architektúrákban és is képesek legyenek kezelni az alkalmazás-hozzárendelések megoldás ezért biztosítása, hogy az egy kattintással indítható megtekinthet egy alacsonyabb RTO vezető katasztrófa esetén különböző rétegek közötti testreszabott lépéseket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="219f3-108">A good disaster recovery solution, should allow modeling of recovery plans around the above complex application architectures and also have the ability to add customized steps to handle application mappings between various tiers hence providing a single-click sure shot solution in the event of a disaster leading to a lower RTO.</span></span>

<span data-ttu-id="219f3-109">Ez a dokumentum részletes útmutatást a vész-helyreállítási megoldást a helyszíni Hyper-V és a VMware vSphere platformokon Citrix XenApp telepítések készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="219f3-109">This document provides a step-by-step guidance for building a disaster recovery solution for your on-premises Citrix XenApp deployments on Hyper-V and VMware vSphere platforms.</span></span> <span data-ttu-id="219f3-110">Ez a dokumentum is ismerteti, hogyan hajthat végre egy feladatátvételi tesztet (vész-helyreállítási részletezési) és a nem tervezett feladatátvételt az Azure-ban, a helyreállítási terv által támogatott konfigurációk és az előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="219f3-110">This document also describes how to perform a test failover(disaster recovery drill) and unplanned failover to Azure using recovery plans, the supported configurations and prerequisites.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="219f3-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="219f3-111">Prerequisites</span></span>

<span data-ttu-id="219f3-112">Megkezdése előtt győződjön meg arról, hogy a következő:</span><span class="sxs-lookup"><span data-stu-id="219f3-112">Before you start, make sure you understand the following:</span></span>

1. [<span data-ttu-id="219f3-113">A virtuális gépek replikálása Azure-bA</span><span class="sxs-lookup"><span data-stu-id="219f3-113">Replicating a virtual machine to Azure</span></span>](site-recovery-vmware-to-azure.md)
1. <span data-ttu-id="219f3-114">Hogyan [egy helyreállítási hálózathoz. terv](site-recovery-network-design.md)</span><span class="sxs-lookup"><span data-stu-id="219f3-114">How to [design a recovery network](site-recovery-network-design.md)</span></span>
1. [<span data-ttu-id="219f3-115">A teszt feladatátvétel az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="219f3-115">Doing a test failover to Azure</span></span>](site-recovery-test-failover-to-azure.md)
1. [<span data-ttu-id="219f3-116">A feladatátvétel az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="219f3-116">Doing a failover to Azure</span></span>](site-recovery-failover.md)
1. <span data-ttu-id="219f3-117">Hogyan [tartományvezérlő replikálása](site-recovery-active-directory.md)</span><span class="sxs-lookup"><span data-stu-id="219f3-117">How to [replicate a domain controller](site-recovery-active-directory.md)</span></span>
1. <span data-ttu-id="219f3-118">Hogyan [SQL-kiszolgáló replikálása](site-recovery-sql.md)</span><span class="sxs-lookup"><span data-stu-id="219f3-118">How to [replicate SQL Server](site-recovery-sql.md)</span></span>

## <a name="deployment-patterns"></a><span data-ttu-id="219f3-119">Központi telepítés minták</span><span class="sxs-lookup"><span data-stu-id="219f3-119">Deployment patterns</span></span>

<span data-ttu-id="219f3-120">A Citrix XenApp és XenDesktop farmhoz jellemzően a következő telepítési minta rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="219f3-120">A Citrix XenApp and XenDesktop farm typically has the following deployment pattern:</span></span>

<span data-ttu-id="219f3-121">**Telepítési minta**</span><span class="sxs-lookup"><span data-stu-id="219f3-121">**Deployment pattern**</span></span>

<span data-ttu-id="219f3-122">Központi Citrix XenApp és XenDesktop AD DNS-kiszolgálóval, az SQL-adatbázis-kiszolgáló, Citrix kézbesítési vezérlő, kirakat kiszolgálót, XenApp főkiszolgáló (VDA), Citrix XenApp licenckiszolgáló</span><span class="sxs-lookup"><span data-stu-id="219f3-122">Citrix XenApp and XenDesktop deployment with AD DNS server, SQL database server, Citrix Delivery Controller, StoreFront server, XenApp Master (VDA), Citrix XenApp License Server</span></span>

![Telepítési minta 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a><span data-ttu-id="219f3-124">Webhely-helyreállítási támogatás</span><span class="sxs-lookup"><span data-stu-id="219f3-124">Site Recovery support</span></span>

<span data-ttu-id="219f3-125">Ebben a cikkben céljából vSphere 6.0 által felügyelt VMware virtuális gépeken a Citrix központi telepítések / System Center VMM 2012 R2 használt vész-Helyreállítási beállítása.</span><span class="sxs-lookup"><span data-stu-id="219f3-125">For the purpose of this article, Citrix deployments on VMware virtual machines managed by vSphere 6.0 / System Center VMM 2012 R2 were used to setup DR.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="219f3-126">Forrása és célja</span><span class="sxs-lookup"><span data-stu-id="219f3-126">Source and target</span></span>

<span data-ttu-id="219f3-127">**A forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="219f3-127">**Scenario**</span></span> | <span data-ttu-id="219f3-128">**Egy másodlagos helyre**</span><span class="sxs-lookup"><span data-stu-id="219f3-128">**To a secondary site**</span></span> | <span data-ttu-id="219f3-129">**Az Azure-bA**</span><span class="sxs-lookup"><span data-stu-id="219f3-129">**To Azure**</span></span>
--- | --- | ---
<span data-ttu-id="219f3-130">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="219f3-130">**Hyper-V**</span></span> | <span data-ttu-id="219f3-131">Nincs a hatókörben</span><span class="sxs-lookup"><span data-stu-id="219f3-131">Not in scope</span></span> | <span data-ttu-id="219f3-132">Igen</span><span class="sxs-lookup"><span data-stu-id="219f3-132">Yes</span></span>
<span data-ttu-id="219f3-133">**VMware**</span><span class="sxs-lookup"><span data-stu-id="219f3-133">**VMware**</span></span> | <span data-ttu-id="219f3-134">Nincs a hatókörben</span><span class="sxs-lookup"><span data-stu-id="219f3-134">Not in scope</span></span> | <span data-ttu-id="219f3-135">Igen</span><span class="sxs-lookup"><span data-stu-id="219f3-135">Yes</span></span>
<span data-ttu-id="219f3-136">**Fizikai kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="219f3-136">**Physical server**</span></span> | <span data-ttu-id="219f3-137">Nincs a hatókörben</span><span class="sxs-lookup"><span data-stu-id="219f3-137">Not in scope</span></span> | <span data-ttu-id="219f3-138">Igen</span><span class="sxs-lookup"><span data-stu-id="219f3-138">Yes</span></span>

### <a name="versions"></a><span data-ttu-id="219f3-139">Verziók</span><span class="sxs-lookup"><span data-stu-id="219f3-139">Versions</span></span>
<span data-ttu-id="219f3-140">Futó Hyper-V vagy VMware virtuális gépek vagy fizikai kiszolgálók, ügyfelek XenApp összetevőit is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="219f3-140">Customers can deploy XenApp components as Virtual Machines running on Hyper-V or VMware or as Physical Servers.</span></span> <span data-ttu-id="219f3-141">Az Azure Site Recovery megvédheti a fizikai és virtuális telepítések az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="219f3-141">Azure Site Recovery can protect both physical and virtual deployments to Azure.</span></span>
<span data-ttu-id="219f3-142">XenApp Észrevételek 7.7 vagy újabb támogatott Azure-ban, mert csak ezen verziói telepítések esetén is a feladatátvételt az Azure-bA vész-helyreállítási vagy áttelepítési.</span><span class="sxs-lookup"><span data-stu-id="219f3-142">Since XenApp 7.7 or later is supported in Azure, only deployments with these versions can be failed over to Azure for Disaster Recovery or migration.</span></span>

### <a name="things-to-keep-in-mind"></a><span data-ttu-id="219f3-143">Vegye figyelembe a következőkre</span><span class="sxs-lookup"><span data-stu-id="219f3-143">Things to keep in mind</span></span>

1. <span data-ttu-id="219f3-144">Védelem és helyreállítás a helyszíni központi telepítések segítségével a kiszolgálói operációs rendszer képes biztosítani a XenApp gépek közzétett alkalmazást, és XenApp közzétett asztalok használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="219f3-144">Protection and recovery of on-premises deployments using Server OS machines to deliver XenApp published apps and XenApp published desktops is supported.</span></span>

2. <span data-ttu-id="219f3-145">Védelmi és helyreállítási a helyszíni központi telepítések asztali VDI-ügyfél képes biztosítani a virtuális asztalok, beleértve a Windows 10 operációs rendszer asztali gépek használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="219f3-145">Protection and recovery of on-premises deployments using desktop OS machines to deliver Desktop VDI for client virtual desktops, including Windows 10, is not supported.</span></span> <span data-ttu-id="219f3-146">Ennek oka az, így nem támogatja a visszaállítást OS'es asztali gépek.</span><span class="sxs-lookup"><span data-stu-id="219f3-146">This is because ASR does not support the recovery of machines with desktop OS’es.</span></span>  <span data-ttu-id="219f3-147">Emellett néhány ügyfél virtuális asztali tökéletesíthető (pl..</span><span class="sxs-lookup"><span data-stu-id="219f3-147">Also, some client virtual desktop flavours (eg.</span></span> <span data-ttu-id="219f3-148">Windows 7) még nem támogatottak a licencelés az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="219f3-148">Windows 7) are not yet supported for licensing in Azure.</span></span> <span data-ttu-id="219f3-149">[Itt részletesen tájékozódhat](https://azure.microsoft.com/pricing/licensing-faq/) az ügyfél/kiszolgáló asztali környezeteinek Azure-ban történő licenceléséről.</span><span class="sxs-lookup"><span data-stu-id="219f3-149">[Learn More](https://azure.microsoft.com/pricing/licensing-faq/) about licensing for client/server desktops in Azure.</span></span>

3.  <span data-ttu-id="219f3-150">Az Azure Site Recovery nem replikálja, és védeni a meglévő helyszíni MCS vagy PVS klónok.</span><span class="sxs-lookup"><span data-stu-id="219f3-150">Azure Site Recovery cannot replicate and protect existing on-premises MCS or PVS clones.</span></span>
<span data-ttu-id="219f3-151">Létre kell hoznia a klónok használatával Azure RM kiépítése a kézbesítési vezérlőről.</span><span class="sxs-lookup"><span data-stu-id="219f3-151">You need to recreate these clones using Azure RM provisioning from Delivery controller.</span></span>

4. <span data-ttu-id="219f3-152">NetScaler nem látható el védelemmel, Azure Site Recovery segítségével NetScaler freebsd rendszerű alapul, és az Azure Site Recovery nem támogatja az operációs rendszer freebsd rendszerű védelméről.</span><span class="sxs-lookup"><span data-stu-id="219f3-152">NetScaler cannot be protected using Azure Site Recovery as NetScaler is based on FreeBSD and Azure Site Recovery does not support protection of FreeBSD OS.</span></span> <span data-ttu-id="219f3-153">Kellene központi telepítése és konfigurálása az Azure piactéren új NetScaler berendezések Azure feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="219f3-153">You would need to deploy and configure a new NetScaler appliance from Azure Market place after failover to Azure.</span></span>


## <a name="replicating-virtual-machines"></a><span data-ttu-id="219f3-154">Virtuális gépek replikálása</span><span class="sxs-lookup"><span data-stu-id="219f3-154">Replicating virtual machines</span></span>

<span data-ttu-id="219f3-155">A Citrix XenApp központi telepítés a következő összetevőket kell replikáció és a helyreállítás engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="219f3-155">The following components of the Citrix XenApp deployment need to be protected to enable replication and recovery.</span></span>

* <span data-ttu-id="219f3-156">AD DNS-kiszolgáló védelme</span><span class="sxs-lookup"><span data-stu-id="219f3-156">Protection of AD DNS server</span></span>
* <span data-ttu-id="219f3-157">Az SQL server-adatbázis védelme</span><span class="sxs-lookup"><span data-stu-id="219f3-157">Protection of SQL database server</span></span>
* <span data-ttu-id="219f3-158">Citrix kézbesítési vezérlő védelme</span><span class="sxs-lookup"><span data-stu-id="219f3-158">Protection of Citrix Delivery Controller</span></span>
* <span data-ttu-id="219f3-159">Kirakat server védelméről.</span><span class="sxs-lookup"><span data-stu-id="219f3-159">Protection of StoreFront server.</span></span>
* <span data-ttu-id="219f3-160">Védelmi XenApp főkiszolgáló (VDA)</span><span class="sxs-lookup"><span data-stu-id="219f3-160">Protection of XenApp Master (VDA)</span></span>
* <span data-ttu-id="219f3-161">Citrix XenApp licenckiszolgáló védelme</span><span class="sxs-lookup"><span data-stu-id="219f3-161">Protection of Citrix XenApp License Server</span></span>


<span data-ttu-id="219f3-162">**AD DNS-kiszolgáló replikáció**</span><span class="sxs-lookup"><span data-stu-id="219f3-162">**AD DNS server replication**</span></span>

<span data-ttu-id="219f3-163">Tekintse meg [védelme Active Directory és az Azure Site Recovery DNS](site-recovery-active-directory.md) kapcsolatos replikálódik, és a tartományvezérlő beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="219f3-163">Please refer to [Protect Active Directory and DNS with Azure Site Recovery](site-recovery-active-directory.md) on guidance for replicating and configuring a domain controller in Azure.</span></span>

<span data-ttu-id="219f3-164">**SQL Server adatbázisreplikáció**</span><span class="sxs-lookup"><span data-stu-id="219f3-164">**SQL database Server replication**</span></span>

<span data-ttu-id="219f3-165">Tekintse meg [SQL Server védelme az SQL Server vészhelyreállítási és az Azure Site Recovery](site-recovery-sql.md) részletes műszaki útmutatást az SQL Server-kiszolgálók védelme érdekében ajánlott beállításai.</span><span class="sxs-lookup"><span data-stu-id="219f3-165">Please refer to [Protect SQL Server with SQL Server disaster recovery and Azure Site Recovery](site-recovery-sql.md) for detailed technical guidance on the recommended options for protecting SQL servers.</span></span>

<span data-ttu-id="219f3-166">Hajtsa végre a [Ez az útmutató](site-recovery-vmware-to-azure.md) elindítani, a többi összetevő virtuális gépek replikálása Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="219f3-166">Follow [this guidance](site-recovery-vmware-to-azure.md) to start replicating the other component virtual machines to Azure.</span></span>

![XenApp összetevők védelme](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

<span data-ttu-id="219f3-168">**Számítási és hálózati beállítások**</span><span class="sxs-lookup"><span data-stu-id="219f3-168">**Compute and Network Settings**</span></span>

<span data-ttu-id="219f3-169">Miután a gépek védelmének (állapotüzenet látható, a replikált elemek "Protected"), a számítási és hálózati beállításokat kell megadni.</span><span class="sxs-lookup"><span data-stu-id="219f3-169">After the machines are protected (status shows as “Protected” under Replicated Items), the Compute and Network settings need to be configured.</span></span>
<span data-ttu-id="219f3-170">A számítás és hálózat > számítási tulajdonságok, megadhatja, hogy az Azure virtuális gép nevét és a cél méretét.</span><span class="sxs-lookup"><span data-stu-id="219f3-170">In Compute and Network > Compute properties, you can specify the Azure VM name and target size.</span></span>
<span data-ttu-id="219f3-171">A név ahhoz Azure-követelményeknek, ha szeretné, hogy módosítható.</span><span class="sxs-lookup"><span data-stu-id="219f3-171">Modify the name to comply with Azure requirements if you need to.</span></span> <span data-ttu-id="219f3-172">Is megtekintheti, és a cél hálózati alhálózat és az Azure virtuális géphez rendelendő IP-cím kapcsolatos információval.</span><span class="sxs-lookup"><span data-stu-id="219f3-172">You can also view and add information about the target network, subnet, and IP address that will be assigned to the Azure VM.</span></span>

<span data-ttu-id="219f3-173">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="219f3-173">Note the following:</span></span>

* <span data-ttu-id="219f3-174">A cél IP-címe beállítható.</span><span class="sxs-lookup"><span data-stu-id="219f3-174">You can set the target IP address.</span></span> <span data-ttu-id="219f3-175">Ha nem ad meg címet, a gép, amelynek a feladatait átadja, a DHCP-t fogja használni.</span><span class="sxs-lookup"><span data-stu-id="219f3-175">If you don't provide an address, the failed over machine will use DHCP.</span></span> <span data-ttu-id="219f3-176">Ha egy címet, amely nem használható feladatátadásra, a feladatátvétel nem fog működni.</span><span class="sxs-lookup"><span data-stu-id="219f3-176">If you set an address that isn't available at failover, the failover won't work.</span></span> <span data-ttu-id="219f3-177">A cél IP-címe feladatátvételi tesztre is használható, amennyiben a cím elérhető a feladatátvételi teszt hálózatában.</span><span class="sxs-lookup"><span data-stu-id="219f3-177">The same target IP address can be used for test failover if the address is available in the test failover network.</span></span>

* <span data-ttu-id="219f3-178">Az AD és a DNS-kiszolgáló esetén, megtartja a helyi cím megadását teszi lehetővé ugyanazt a címet az Azure virtuális hálózat DNS-kiszolgálóként.</span><span class="sxs-lookup"><span data-stu-id="219f3-178">For the AD/DNS server, retaining the on-premises address lets you specify the same address as the DNS server for the Azure Virtual network.</span></span>

<span data-ttu-id="219f3-179">A hálózati adapterek számát a cél virtuális gépek mérete határozza meg, a következők szerint:</span><span class="sxs-lookup"><span data-stu-id="219f3-179">The number of network adapters is dictated by the size you specify for the target virtual machine, as follows:</span></span>

*   <span data-ttu-id="219f3-180">Ha a forrásgépen működő hálózati adapterek száma kisebb vagy egyenlő a célgép méretéhez engedélyezett adapterek számával, a célon ugyanannyi adapter fog működni, mint a forráson.</span><span class="sxs-lookup"><span data-stu-id="219f3-180">If the number of network adapters on the source machine is less than or equal to the number of adapters allowed for the target machine size, then the target will have the same number of adapters as the source.</span></span>
*   <span data-ttu-id="219f3-181">Ha a forrás virtuális gépek adaptereinek száma meghaladja a célmérethez engedélyezett maximumot, a rendszer a célmérethez engedélyezett maximális számot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="219f3-181">If the number of adapters for the source virtual machine exceeds the number allowed for the target size then the target size maximum will be used.</span></span>
* <span data-ttu-id="219f3-182">Ha például a forrásgépen két hálózati adapter működik, és a célgép mérete négy adapter használatát teszi lehetővé, a célgépen két adapter fog működni.</span><span class="sxs-lookup"><span data-stu-id="219f3-182">For example, if a source machine has two network adapters and the target machine size supports four, the target machine will have two adapters.</span></span> <span data-ttu-id="219f3-183">Ha azonban a forrásgépen két adapter működik, de a cél csupán egy adaptert támogat, akkor a célgépen is csak egy adapter fog működni.</span><span class="sxs-lookup"><span data-stu-id="219f3-183">If the source machine has two adapters but the supported target size only supports one then the target machine will have only one adapter.</span></span>
*   <span data-ttu-id="219f3-184">Ha a virtuális gépnek több hálózati adapter lesz az összes csatlakoznak az ugyanazon a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="219f3-184">If the virtual machine has multiple network adapters they will all connect to the same network.</span></span>
*   <span data-ttu-id="219f3-185">Ha a virtuális gépnek több hálózati adaptert, majd az elsőt a listán látható lesz a virtuális gépet az Azure alapértelmezett hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="219f3-185">If the virtual machine has multiple network adapters, then the first one shown in the list becomes the Default network adapter in the Azure virtual machine.</span></span>


## <a name="creating-a-recovery-plan"></a><span data-ttu-id="219f3-186">Helyreállítási terv létrehozása</span><span class="sxs-lookup"><span data-stu-id="219f3-186">Creating a recovery plan</span></span>

<span data-ttu-id="219f3-187">A XenApp összetevő virtuális gépek replikáció engedélyezése után a következő lépés a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="219f3-187">After replication is enabled for the XenApp component VMs, the next step is to create a recovery plan.</span></span>
<span data-ttu-id="219f3-188">A helyreállítási terv csoportok együtt a virtuális gépek a feladatátvétel és helyreállítás hasonló követelményei.</span><span class="sxs-lookup"><span data-stu-id="219f3-188">A recovery plan groups together virtual machines with similar requirements for failover and recovery.</span></span>  

<span data-ttu-id="219f3-189">**A helyreállítási terv létrehozásának lépései**</span><span class="sxs-lookup"><span data-stu-id="219f3-189">**Steps to create a recovery plan**</span></span>

1. <span data-ttu-id="219f3-190">A XenApp összetevő virtuális gépek hozzáadása a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="219f3-190">Add the XenApp component virtual machines in the Recovery Plan.</span></span>
2. <span data-ttu-id="219f3-191">Kattintson a helyreállítási terv -> + helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="219f3-191">Click Recovery Plans -> + Recovery Plan.</span></span> <span data-ttu-id="219f3-192">Adja meg a helyreállítási terv egy egyszerűen elsajátítható nevét.</span><span class="sxs-lookup"><span data-stu-id="219f3-192">Provide an intuitive name for the recovery plan.</span></span>
3. <span data-ttu-id="219f3-193">A VMware virtuális gépek: forrás kiválasztása VMware folyamatkiszolgálót, mint a Microsoft Azure cél, és üzembe helyezési modellel, az erőforrás-kezelő, majd kattintson a elemeket választhat ki.</span><span class="sxs-lookup"><span data-stu-id="219f3-193">For VMware virtual machines: Select source as VMware process server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items.</span></span>
4. <span data-ttu-id="219f3-194">A Hyper-V virtuális gépek: válassza ki a forrás VMM-kiszolgálóként, Microsoft Azure és a telepítési modell, erőforrás-kezelő cél és kattintson az elemeket választhat ki és válassza ki a XenApp telepítési virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="219f3-194">For Hyper-V virtual machines: Select source as VMM server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items and then select the XenApp deployment VMs.</span></span>

### <a name="adding-virtual-machines-to-failover-groups"></a><span data-ttu-id="219f3-195">Virtuális gépek feladatátvételi csoportok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="219f3-195">Adding virtual machines to failover groups</span></span>

<span data-ttu-id="219f3-196">Adja hozzá az adott indítási sorrendje, parancsprogramok és manuális műveletek feladatátvételi csoportokat a helyreállítási terv testre szabható.</span><span class="sxs-lookup"><span data-stu-id="219f3-196">Recovery plans can be customized to add failover groups for specific startup order, scripts or manual actions.</span></span> <span data-ttu-id="219f3-197">A következő csoportok kell felvenni a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="219f3-197">The following groups need to be added to the recovery plan.</span></span>

1. <span data-ttu-id="219f3-198">Feladatátvételi csoport1: AD DNS</span><span class="sxs-lookup"><span data-stu-id="219f3-198">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="219f3-199">Feladatátvételi csoport2: SQL Server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="219f3-199">Failover Group2: SQL Server VMs</span></span>
2. <span data-ttu-id="219f3-200">Feladatátvételi 3: VDA fő kép VM</span><span class="sxs-lookup"><span data-stu-id="219f3-200">Failover Group3: VDA Master Image VM</span></span>
3. <span data-ttu-id="219f3-201">Feladatátvételi 4: Kézbesítési vezérlő és kirakat server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="219f3-201">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>


### <a name="adding-scripts-to-the-recovery-plan"></a><span data-ttu-id="219f3-202">A helyreállítási terv parancsprogramokat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="219f3-202">Adding scripts to the recovery plan</span></span>

<span data-ttu-id="219f3-203">Parancsfájlokat lehet futtatni előtt vagy után egy adott csoport a helyreállítási tervet.</span><span class="sxs-lookup"><span data-stu-id="219f3-203">Scripts can be run before or after a specific group in a recovery plan.</span></span> <span data-ttu-id="219f3-204">Manuális műveletek lehet is része és feladatátvétel során.</span><span class="sxs-lookup"><span data-stu-id="219f3-204">Manual actions can be also be included and performed during failover.</span></span>

<span data-ttu-id="219f3-205">A testre szabott helyreállítási tervet a következőképpen néz az alábbi:</span><span class="sxs-lookup"><span data-stu-id="219f3-205">The customized recovery plan looks like the below:</span></span>

1. <span data-ttu-id="219f3-206">Feladatátvételi csoport1: AD DNS</span><span class="sxs-lookup"><span data-stu-id="219f3-206">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="219f3-207">Feladatátvételi csoport2: SQL Server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="219f3-207">Failover Group2: SQL Server VMs</span></span>
3. <span data-ttu-id="219f3-208">Feladatátvételi 3: VDA fő kép VM</span><span class="sxs-lookup"><span data-stu-id="219f3-208">Failover Group3: VDA Master Image VM</span></span>

   >[!NOTE]     
   ><span data-ttu-id="219f3-209">Csak egy helyszíni XenApp lépéseket, 4, 6, 7 tartalmazó manuális vagy parancsfájl műveletek alkalmazhatók > MCS/PVS katalógusok környezetben.</span><span class="sxs-lookup"><span data-stu-id="219f3-209">Steps 4, 6 and 7 containing manual or script actions are applicable to only an on-premises XenApp >environment with MCS/PVS catalogs.</span></span>

4. <span data-ttu-id="219f3-210">3. csoport kézi vagy parancsfájl művelet: fő VDA VM a fő VDA VM leállítása, ha a feladatátvételt az Azure-ba kerül, a futó állapotot.</span><span class="sxs-lookup"><span data-stu-id="219f3-210">Group 3 Manual or script action: Shutdown master VDA VM The Master VDA VM when failed over to Azure will be in a running state.</span></span> <span data-ttu-id="219f3-211">Hozzon létre új MCS katalógusok Azure ARM használatával, a fő VDA VM szükség lehet leállítva (de lefoglalt) állapotát.</span><span class="sxs-lookup"><span data-stu-id="219f3-211">To create new MCS catalogs using Azure ARM hosting, the master VDA VM is required to be in Stopped (de allocated) state.</span></span> <span data-ttu-id="219f3-212">Állítsa le a virtuális gép Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="219f3-212">Shutdown the VM from Azure Portal.</span></span>

5. <span data-ttu-id="219f3-213">Feladatátvételi 4: Kézbesítési vezérlő és kirakat server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="219f3-213">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>
6. <span data-ttu-id="219f3-214">3 manuális vagy parancsfájl 1 művelet:</span><span class="sxs-lookup"><span data-stu-id="219f3-214">Group3 manual or script action 1:</span></span>

    <span data-ttu-id="219f3-215">***Azure RM állomás kapcsolat hozzáadása***</span><span class="sxs-lookup"><span data-stu-id="219f3-215">***Add Azure RM host connection***</span></span>

    <span data-ttu-id="219f3-216">Kézbesítési vezérlő gép kiépítéséhez új MCS katalógusok az Azure-ban Azure ARM-állomás kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="219f3-216">Create Azure ARM host connection in Delivery Controller machine to provision new MCS catalogs in Azure.</span></span> <span data-ttu-id="219f3-217">Kövesse a lépéseket, ez a [cikk](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span><span class="sxs-lookup"><span data-stu-id="219f3-217">Follow the steps as explained in this [article](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span></span>

7. <span data-ttu-id="219f3-218">3 manuális vagy parancsfájl művelet 2:</span><span class="sxs-lookup"><span data-stu-id="219f3-218">Group3 manual or script action 2:</span></span>

    <span data-ttu-id="219f3-219">***Hozza létre az MCS katalógusok az Azure-ban***</span><span class="sxs-lookup"><span data-stu-id="219f3-219">***Re-create MCS Catalogs in Azure***</span></span>

    <span data-ttu-id="219f3-220">A meglévő MCS vagy PVS klónok az elsődleges helyen nem replikálja az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="219f3-220">The existing MCS or PVS clones on the primary site will not be replicated to Azure.</span></span> <span data-ttu-id="219f3-221">Létre kell hoznia ezeket a replikált fő VDA és a kézbesítési vezérlőről kiépítés Azure ARM klónok. Kövesse a lépéseket, ez a [cikk](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) MCS katalógusok létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="219f3-221">You need to recreate these clones using the replicated master VDA and Azure ARM provisioning from Delivery controller.Follow the steps as explained in this [article](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) to create MCS catalogs in Azure.</span></span>

![Helyreállítási terv XenApp összetevők](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   ><span data-ttu-id="219f3-223">Parancsfájlokat, [hely](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) frissíteni a DNS és az új IP-címek a keresztül > virtuális gépek vagy csatolni egy terhelés-kiegyenlítő a feladatait átadó virtuális gép, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="219f3-223">You can use scripts at [location](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) to update the DNS with the new IPs of the failed over >virtual machines or to attach a load balancer on the failed over virtual machine, if needed.</span></span>


## <a name="doing-a-test-failover"></a><span data-ttu-id="219f3-224">A teszt feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="219f3-224">Doing a test failover</span></span>

<span data-ttu-id="219f3-225">Hajtsa végre a [Ez az útmutató](site-recovery-test-failover-to-azure.md) feladatátvételi teszt végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="219f3-225">Follow [this guidance](site-recovery-test-failover-to-azure.md) to do a test failover.</span></span>

![Helyreállítási terv](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a><span data-ttu-id="219f3-227">A feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="219f3-227">Doing a failover</span></span>

<span data-ttu-id="219f3-228">Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) Ha feladatátvételt végez.</span><span class="sxs-lookup"><span data-stu-id="219f3-228">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="219f3-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="219f3-229">Next steps</span></span>

<span data-ttu-id="219f3-230">Is [további](https://aka.ms/citrix-xenapp-xendesktop-with-asr) kapcsolatos Citrix XenApp és XenDesktop a központi telepítések Ez a dokumentum replikálása.</span><span class="sxs-lookup"><span data-stu-id="219f3-230">You can [learn more](https://aka.ms/citrix-xenapp-xendesktop-with-asr) about replicating Citrix XenApp and XenDesktop deployments  in this white paper.</span></span> <span data-ttu-id="219f3-231">Nézze meg az útmutató a [replikálja más alkalmazások](site-recovery-workload.md) Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="219f3-231">Look at the guidance to [replicate other applications](site-recovery-workload.md) using Site Recovery.</span></span>
