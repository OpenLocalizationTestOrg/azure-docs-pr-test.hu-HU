---
title: "egy Azure Site Recovery segítségével többrétegű Citrix XenDesktop és XenApp telepítési aaaReplicate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooprotect és helyreállítás Citrix XenDesktop és XenApp üzembe helyezése Azure Site Recovery."
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
ms.openlocfilehash: c4ea9f95f91c585cdcf9d776b02c0967f4c16ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a><span data-ttu-id="2f234-103">Egy Azure Site Recovery segítségével többrétegű Citrix XenApp és XenDesktop telepítési replikálása</span><span class="sxs-lookup"><span data-stu-id="2f234-103">Replicate a multi-tier Citrix XenApp and XenDesktop deployment using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="2f234-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2f234-104">Overview</span></span>

<span data-ttu-id="2f234-105">Citrix XenDesktop egy olyan asztali virtualizálási megoldás, letölti a asztali környezetek és alkalmazások ondemand szolgáltatás tooany-felhasználóként, bárhonnan.</span><span class="sxs-lookup"><span data-stu-id="2f234-105">Citrix XenDesktop is a desktop virtualization solution that delivers desktops and applications as an ondemand service tooany user, anywhere.</span></span> <span data-ttu-id="2f234-106">A FlexCast kézbesítési technológia XenDesktop gyorsan és biztonságosan biztosíthat alkalmazások és asztali számítógépek toousers.</span><span class="sxs-lookup"><span data-stu-id="2f234-106">With FlexCast delivery technology, XenDesktop can quickly and securely deliver applications and desktops toousers.</span></span>
<span data-ttu-id="2f234-107">Ma, Citrix XenApp nem minden vész helyreállítási képességeket biztosítják.</span><span class="sxs-lookup"><span data-stu-id="2f234-107">Today, Citrix XenApp does not provide any disaster recovery capabilities.</span></span>

<span data-ttu-id="2f234-108">Egy jó vész-helyreállítási megoldást kell modellezési helyreállítási tervek hello körül fent összetett alkalmazási architektúrákban engedélyezése és hello teszi testre szabott tooadd lépéseket toohandle alkalmazástársítások ezért biztosító különböző rétegek közötti is rendelkeznek egy egy kattintással meg arról, hogy megoldás létrehozása a hello eseményeket, ami tooa katasztrófa RTO csökkentéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f234-108">A good disaster recovery solution, should allow modeling of recovery plans around hello above complex application architectures and also have hello ability tooadd customized steps toohandle application mappings between various tiers hence providing a single-click sure shot solution in hello event of a disaster leading tooa lower RTO.</span></span>

<span data-ttu-id="2f234-109">Ez a dokumentum részletes útmutatást a vész-helyreállítási megoldást a helyszíni Hyper-V és a VMware vSphere platformokon Citrix XenApp telepítések készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f234-109">This document provides a step-by-step guidance for building a disaster recovery solution for your on-premises Citrix XenApp deployments on Hyper-V and VMware vSphere platforms.</span></span> <span data-ttu-id="2f234-110">A dokumentum is ismerteti, hogyan tooperform (vész-helyreállítási részletezési) feladatátvételi tesztet, és nem tervezett feladatátvétel tooAzure helyreállítási tervek, hello támogatott konfigurációk és előfeltételek használatával.</span><span class="sxs-lookup"><span data-stu-id="2f234-110">This document also describes how tooperform a test failover(disaster recovery drill) and unplanned failover tooAzure using recovery plans, hello supported configurations and prerequisites.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="2f234-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2f234-111">Prerequisites</span></span>

<span data-ttu-id="2f234-112">Megkezdése előtt győződjön meg arról, hogy hello következő:</span><span class="sxs-lookup"><span data-stu-id="2f234-112">Before you start, make sure you understand hello following:</span></span>

1. [<span data-ttu-id="2f234-113">A virtuális gép tooAzure replikálása</span><span class="sxs-lookup"><span data-stu-id="2f234-113">Replicating a virtual machine tooAzure</span></span>](site-recovery-vmware-to-azure.md)
1. <span data-ttu-id="2f234-114">Hogyan túl[egy helyreállítási hálózathoz. terv](site-recovery-network-design.md)</span><span class="sxs-lookup"><span data-stu-id="2f234-114">How too[design a recovery network](site-recovery-network-design.md)</span></span>
1. [<span data-ttu-id="2f234-115">Ez a teszt feladatátvételi tooAzure</span><span class="sxs-lookup"><span data-stu-id="2f234-115">Doing a test failover tooAzure</span></span>](site-recovery-test-failover-to-azure.md)
1. [<span data-ttu-id="2f234-116">Ez a feladatátvételi tooAzure</span><span class="sxs-lookup"><span data-stu-id="2f234-116">Doing a failover tooAzure</span></span>](site-recovery-failover.md)
1. <span data-ttu-id="2f234-117">Hogyan túl[tartományvezérlő replikálása](site-recovery-active-directory.md)</span><span class="sxs-lookup"><span data-stu-id="2f234-117">How too[replicate a domain controller](site-recovery-active-directory.md)</span></span>
1. <span data-ttu-id="2f234-118">Hogyan túl[SQL-kiszolgáló replikálása](site-recovery-sql.md)</span><span class="sxs-lookup"><span data-stu-id="2f234-118">How too[replicate SQL Server](site-recovery-sql.md)</span></span>

## <a name="deployment-patterns"></a><span data-ttu-id="2f234-119">Központi telepítés minták</span><span class="sxs-lookup"><span data-stu-id="2f234-119">Deployment patterns</span></span>

<span data-ttu-id="2f234-120">Citrix XenApp és XenDesktop farm jellemzően a következő telepítési minta hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="2f234-120">A Citrix XenApp and XenDesktop farm typically has hello following deployment pattern:</span></span>

<span data-ttu-id="2f234-121">**Telepítési minta**</span><span class="sxs-lookup"><span data-stu-id="2f234-121">**Deployment pattern**</span></span>

<span data-ttu-id="2f234-122">Központi Citrix XenApp és XenDesktop AD DNS-kiszolgálóval, az SQL-adatbázis-kiszolgáló, Citrix kézbesítési vezérlő, kirakat kiszolgálót, XenApp főkiszolgáló (VDA), Citrix XenApp licenckiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2f234-122">Citrix XenApp and XenDesktop deployment with AD DNS server, SQL database server, Citrix Delivery Controller, StoreFront server, XenApp Master (VDA), Citrix XenApp License Server</span></span>

![Telepítési minta 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a><span data-ttu-id="2f234-124">Webhely-helyreállítási támogatás</span><span class="sxs-lookup"><span data-stu-id="2f234-124">Site Recovery support</span></span>

<span data-ttu-id="2f234-125">Ez a cikk hello célra vSphere 6.0 által felügyelt VMware virtuális gépeken a Citrix központi telepítések / System Center VMM 2012 R2 volt használt toosetup vész-Helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="2f234-125">For hello purpose of this article, Citrix deployments on VMware virtual machines managed by vSphere 6.0 / System Center VMM 2012 R2 were used toosetup DR.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="2f234-126">Forrása és célja</span><span class="sxs-lookup"><span data-stu-id="2f234-126">Source and target</span></span>

<span data-ttu-id="2f234-127">**A forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="2f234-127">**Scenario**</span></span> | <span data-ttu-id="2f234-128">**másodlagos hely tooa**</span><span class="sxs-lookup"><span data-stu-id="2f234-128">**tooa secondary site**</span></span> | <span data-ttu-id="2f234-129">**tooAzure**</span><span class="sxs-lookup"><span data-stu-id="2f234-129">**tooAzure**</span></span>
--- | --- | ---
<span data-ttu-id="2f234-130">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="2f234-130">**Hyper-V**</span></span> | <span data-ttu-id="2f234-131">Nincs a hatókörben</span><span class="sxs-lookup"><span data-stu-id="2f234-131">Not in scope</span></span> | <span data-ttu-id="2f234-132">Igen</span><span class="sxs-lookup"><span data-stu-id="2f234-132">Yes</span></span>
<span data-ttu-id="2f234-133">**VMware**</span><span class="sxs-lookup"><span data-stu-id="2f234-133">**VMware**</span></span> | <span data-ttu-id="2f234-134">Nincs a hatókörben</span><span class="sxs-lookup"><span data-stu-id="2f234-134">Not in scope</span></span> | <span data-ttu-id="2f234-135">Igen</span><span class="sxs-lookup"><span data-stu-id="2f234-135">Yes</span></span>
<span data-ttu-id="2f234-136">**Fizikai kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="2f234-136">**Physical server**</span></span> | <span data-ttu-id="2f234-137">Nincs a hatókörben</span><span class="sxs-lookup"><span data-stu-id="2f234-137">Not in scope</span></span> | <span data-ttu-id="2f234-138">Igen</span><span class="sxs-lookup"><span data-stu-id="2f234-138">Yes</span></span>

### <a name="versions"></a><span data-ttu-id="2f234-139">Verziók</span><span class="sxs-lookup"><span data-stu-id="2f234-139">Versions</span></span>
<span data-ttu-id="2f234-140">Futó Hyper-V vagy VMware virtuális gépek vagy fizikai kiszolgálók, ügyfelek XenApp összetevőit is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="2f234-140">Customers can deploy XenApp components as Virtual Machines running on Hyper-V or VMware or as Physical Servers.</span></span> <span data-ttu-id="2f234-141">Az Azure Site Recovery mindkét fizikai és virtuális központi telepítések tooAzure védhet.</span><span class="sxs-lookup"><span data-stu-id="2f234-141">Azure Site Recovery can protect both physical and virtual deployments tooAzure.</span></span>
<span data-ttu-id="2f234-142">Az Azure-ban XenApp Észrevételek 7.7 vagy újabb támogatott, mert csak ezen verziói telepítések esetén feladatátvételre tooAzure vész-helyreállítási vagy áttelepítési.</span><span class="sxs-lookup"><span data-stu-id="2f234-142">Since XenApp 7.7 or later is supported in Azure, only deployments with these versions can be failed over tooAzure for Disaster Recovery or migration.</span></span>

### <a name="things-tookeep-in-mind"></a><span data-ttu-id="2f234-143">Dolgot tookeep szem előtt</span><span class="sxs-lookup"><span data-stu-id="2f234-143">Things tookeep in mind</span></span>

1. <span data-ttu-id="2f234-144">Védelem és helyreállítás a helyszíni kiszolgáló OS gépek toodeliver használatával XenApp közzétett alkalmazást, és XenApp közzétett asztalok központi telepítések esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="2f234-144">Protection and recovery of on-premises deployments using Server OS machines toodeliver XenApp published apps and XenApp published desktops is supported.</span></span>

2. <span data-ttu-id="2f234-145">Védelmi és helyreállítási a helyszíni központi telepítések ügyfél virtuális asztalok, beleértve a Windows 10 asztali operációs rendszer gépek toodeliver asztali VDI használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2f234-145">Protection and recovery of on-premises deployments using desktop OS machines toodeliver Desktop VDI for client virtual desktops, including Windows 10, is not supported.</span></span> <span data-ttu-id="2f234-146">Ennek oka az, így nem támogatja az asztallal OS'es gépek hello helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="2f234-146">This is because ASR does not support hello recovery of machines with desktop OS’es.</span></span>  <span data-ttu-id="2f234-147">Emellett néhány ügyfél virtuális asztali tökéletesíthető (pl..</span><span class="sxs-lookup"><span data-stu-id="2f234-147">Also, some client virtual desktop flavours (eg.</span></span> <span data-ttu-id="2f234-148">Windows 7) még nem támogatottak a licencelés az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2f234-148">Windows 7) are not yet supported for licensing in Azure.</span></span> <span data-ttu-id="2f234-149">[Itt részletesen tájékozódhat](https://azure.microsoft.com/pricing/licensing-faq/) az ügyfél/kiszolgáló asztali környezeteinek Azure-ban történő licenceléséről.</span><span class="sxs-lookup"><span data-stu-id="2f234-149">[Learn More](https://azure.microsoft.com/pricing/licensing-faq/) about licensing for client/server desktops in Azure.</span></span>

3.  <span data-ttu-id="2f234-150">Az Azure Site Recovery nem replikálja, és védeni a meglévő helyszíni MCS vagy PVS klónok.</span><span class="sxs-lookup"><span data-stu-id="2f234-150">Azure Site Recovery cannot replicate and protect existing on-premises MCS or PVS clones.</span></span>
<span data-ttu-id="2f234-151">Ezek klónok használatával Azure RM kiépítése a kézbesítési vezérlőről kell toorecreate.</span><span class="sxs-lookup"><span data-stu-id="2f234-151">You need toorecreate these clones using Azure RM provisioning from Delivery controller.</span></span>

4. <span data-ttu-id="2f234-152">NetScaler nem látható el védelemmel, Azure Site Recovery segítségével NetScaler freebsd rendszerű alapul, és az Azure Site Recovery nem támogatja az operációs rendszer freebsd rendszerű védelméről.</span><span class="sxs-lookup"><span data-stu-id="2f234-152">NetScaler cannot be protected using Azure Site Recovery as NetScaler is based on FreeBSD and Azure Site Recovery does not support protection of FreeBSD OS.</span></span> <span data-ttu-id="2f234-153">Ehhez toodeploy kell, és az Azure piactéren új NetScaler berendezések konfigurálása után feladatátvételi tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2f234-153">You would need toodeploy and configure a new NetScaler appliance from Azure Market place after failover tooAzure.</span></span>


## <a name="replicating-virtual-machines"></a><span data-ttu-id="2f234-154">Virtuális gépek replikálása</span><span class="sxs-lookup"><span data-stu-id="2f234-154">Replicating virtual machines</span></span>

<span data-ttu-id="2f234-155">a következő hello Citrix XenApp telepítési összetevői hello védett toobe tooenable replikációs és helyreállítási van szükség.</span><span class="sxs-lookup"><span data-stu-id="2f234-155">hello following components of hello Citrix XenApp deployment need toobe protected tooenable replication and recovery.</span></span>

* <span data-ttu-id="2f234-156">AD DNS-kiszolgáló védelme</span><span class="sxs-lookup"><span data-stu-id="2f234-156">Protection of AD DNS server</span></span>
* <span data-ttu-id="2f234-157">Az SQL server-adatbázis védelme</span><span class="sxs-lookup"><span data-stu-id="2f234-157">Protection of SQL database server</span></span>
* <span data-ttu-id="2f234-158">Citrix kézbesítési vezérlő védelme</span><span class="sxs-lookup"><span data-stu-id="2f234-158">Protection of Citrix Delivery Controller</span></span>
* <span data-ttu-id="2f234-159">Kirakat server védelméről.</span><span class="sxs-lookup"><span data-stu-id="2f234-159">Protection of StoreFront server.</span></span>
* <span data-ttu-id="2f234-160">Védelmi XenApp főkiszolgáló (VDA)</span><span class="sxs-lookup"><span data-stu-id="2f234-160">Protection of XenApp Master (VDA)</span></span>
* <span data-ttu-id="2f234-161">Citrix XenApp licenckiszolgáló védelme</span><span class="sxs-lookup"><span data-stu-id="2f234-161">Protection of Citrix XenApp License Server</span></span>


<span data-ttu-id="2f234-162">**AD DNS-kiszolgáló replikáció**</span><span class="sxs-lookup"><span data-stu-id="2f234-162">**AD DNS server replication**</span></span>

<span data-ttu-id="2f234-163">Tekintse meg a túl[védelme Active Directory és az Azure Site Recovery DNS](site-recovery-active-directory.md) kapcsolatos replikálódik, és a tartományvezérlő beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2f234-163">Please refer too[Protect Active Directory and DNS with Azure Site Recovery](site-recovery-active-directory.md) on guidance for replicating and configuring a domain controller in Azure.</span></span>

<span data-ttu-id="2f234-164">**SQL Server adatbázisreplikáció**</span><span class="sxs-lookup"><span data-stu-id="2f234-164">**SQL database Server replication**</span></span>

<span data-ttu-id="2f234-165">Tekintse meg a túl[SQL Server védelme az SQL Server vészhelyreállítási és az Azure Site Recovery](site-recovery-sql.md) hello részletes műszaki útmutatást ajánlott az SQL kiszolgáló védelmére irányuló lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="2f234-165">Please refer too[Protect SQL Server with SQL Server disaster recovery and Azure Site Recovery](site-recovery-sql.md) for detailed technical guidance on hello recommended options for protecting SQL servers.</span></span>

<span data-ttu-id="2f234-166">Hajtsa végre a [Ez az útmutató](site-recovery-vmware-to-azure.md) toostart replikálása hello más összetevő virtuális gépek tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2f234-166">Follow [this guidance](site-recovery-vmware-to-azure.md) toostart replicating hello other component virtual machines tooAzure.</span></span>

![XenApp összetevők védelme](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

<span data-ttu-id="2f234-168">**Számítási és hálózati beállítások**</span><span class="sxs-lookup"><span data-stu-id="2f234-168">**Compute and Network Settings**</span></span>

<span data-ttu-id="2f234-169">Után a védett gépek hello (állapotát jeleníti meg, "Protected" replikált elemek alapján), hello számítási, és meg kell toobe konfigurált hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2f234-169">After hello machines are protected (status shows as “Protected” under Replicated Items), hello Compute and Network settings need toobe configured.</span></span>
<span data-ttu-id="2f234-170">A számítás és hálózat > számítási tulajdonságok, megadhatja a hello Azure virtuális gép nevét és a cél méretét.</span><span class="sxs-lookup"><span data-stu-id="2f234-170">In Compute and Network > Compute properties, you can specify hello Azure VM name and target size.</span></span>
<span data-ttu-id="2f234-171">Ha szeretné, módosítsa a hello neve toocomply az Azure-követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="2f234-171">Modify hello name toocomply with Azure requirements if you need to.</span></span> <span data-ttu-id="2f234-172">Is megtekintheti, és adja hozzá a hello célhálózat, alhálózati és toohello Azure virtuális Géphez rendelendő IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2f234-172">You can also view and add information about hello target network, subnet, and IP address that will be assigned toohello Azure VM.</span></span>

<span data-ttu-id="2f234-173">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="2f234-173">Note hello following:</span></span>

* <span data-ttu-id="2f234-174">Beállíthat hello cél IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2f234-174">You can set hello target IP address.</span></span> <span data-ttu-id="2f234-175">Ha nem ad meg egy címet, a gép a feladatátvételt hello DHCP fogja használni.</span><span class="sxs-lookup"><span data-stu-id="2f234-175">If you don't provide an address, hello failed over machine will use DHCP.</span></span> <span data-ttu-id="2f234-176">Ha egy címet, amely nem használható feladatátadásra, hello feladatátvétel nem fog működni.</span><span class="sxs-lookup"><span data-stu-id="2f234-176">If you set an address that isn't available at failover, hello failover won't work.</span></span> <span data-ttu-id="2f234-177">cél IP-cím a feladatátvételi teszthez használható, ha hello címkészletében nem áll rendelkezésre a feladatátvételi teszt hálózatában hello hello.</span><span class="sxs-lookup"><span data-stu-id="2f234-177">hello same target IP address can be used for test failover if hello address is available in hello test failover network.</span></span>

* <span data-ttu-id="2f234-178">Hello AD és a DNS-kiszolgáló esetén hello megőrzése a helyi cím segítségével azonos cím hello hello Azure virtuális hálózat hello DNS-kiszolgálóként adja meg.</span><span class="sxs-lookup"><span data-stu-id="2f234-178">For hello AD/DNS server, retaining hello on-premises address lets you specify hello same address as hello DNS server for hello Azure Virtual network.</span></span>

<span data-ttu-id="2f234-179">hálózati adapterek száma hello hello mérete határozza meg hello cél virtuális gépre, az alábbiak szerint határozza meg:</span><span class="sxs-lookup"><span data-stu-id="2f234-179">hello number of network adapters is dictated by hello size you specify for hello target virtual machine, as follows:</span></span>

*   <span data-ttu-id="2f234-180">Ha hálózati adapterek hello forrásgépen hello száma kisebb vagy egyenlő, mint hello cél gép méretéhez engedélyezett adapterek toohello számát, majd hello cél lesz hello ugyanannyi adapter hello forrásaként.</span><span class="sxs-lookup"><span data-stu-id="2f234-180">If hello number of network adapters on hello source machine is less than or equal toohello number of adapters allowed for hello target machine size, then hello target will have hello same number of adapters as hello source.</span></span>
*   <span data-ttu-id="2f234-181">Ha hello hello forrás virtuális gép adaptereinek száma meghaladja hello engedélyezett hello célméretet majd hello cél maximális fogja használni.</span><span class="sxs-lookup"><span data-stu-id="2f234-181">If hello number of adapters for hello source virtual machine exceeds hello number allowed for hello target size then hello target size maximum will be used.</span></span>
* <span data-ttu-id="2f234-182">Például ha a forrásgépen két hálózati adaptert, és hello célgép mérete négy támogatja, akkor hello célgépen két adapter fog működni.</span><span class="sxs-lookup"><span data-stu-id="2f234-182">For example, if a source machine has two network adapters and hello target machine size supports four, hello target machine will have two adapters.</span></span> <span data-ttu-id="2f234-183">Ha hello forrásgépen két adapter, azonban a hello támogatott célméretet csak egy majd hello célgépen csak egy adapter fog működni.</span><span class="sxs-lookup"><span data-stu-id="2f234-183">If hello source machine has two adapters but hello supported target size only supports one then hello target machine will have only one adapter.</span></span>
*   <span data-ttu-id="2f234-184">Ha a virtuális gépnek több hálózati adapter hello lesz az összes csatlakoznak toohello ugyanazon a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="2f234-184">If hello virtual machine has multiple network adapters they will all connect toohello same network.</span></span>
*   <span data-ttu-id="2f234-185">Ha hello virtuális gépnek több hálózati adaptert, majd hello hello listában megjelenő első egy lesz hello alapértelmezett hálózati adapter hello Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2f234-185">If hello virtual machine has multiple network adapters, then hello first one shown in hello list becomes hello Default network adapter in hello Azure virtual machine.</span></span>


## <a name="creating-a-recovery-plan"></a><span data-ttu-id="2f234-186">Helyreállítási terv létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f234-186">Creating a recovery plan</span></span>

<span data-ttu-id="2f234-187">Után a replikáció hello XenApp összetevő virtuális gépek engedélyezve van, a hello következő lépésre toocreate helyreállítási tervet.</span><span class="sxs-lookup"><span data-stu-id="2f234-187">After replication is enabled for hello XenApp component VMs, hello next step is toocreate a recovery plan.</span></span>
<span data-ttu-id="2f234-188">A helyreállítási terv csoportok együtt a virtuális gépek a feladatátvétel és helyreállítás hasonló követelményei.</span><span class="sxs-lookup"><span data-stu-id="2f234-188">A recovery plan groups together virtual machines with similar requirements for failover and recovery.</span></span>  

<span data-ttu-id="2f234-189">**A helyreállítási terv lépéseket toocreate**</span><span class="sxs-lookup"><span data-stu-id="2f234-189">**Steps toocreate a recovery plan**</span></span>

1. <span data-ttu-id="2f234-190">Helyreállítási terv hello hello XenApp összetevő virtuális gépek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2f234-190">Add hello XenApp component virtual machines in hello Recovery Plan.</span></span>
2. <span data-ttu-id="2f234-191">Kattintson a helyreállítási terv -> + helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="2f234-191">Click Recovery Plans -> + Recovery Plan.</span></span> <span data-ttu-id="2f234-192">Adjon meg a helyreállítási terv hello intuitív nevet.</span><span class="sxs-lookup"><span data-stu-id="2f234-192">Provide an intuitive name for hello recovery plan.</span></span>
3. <span data-ttu-id="2f234-193">A VMware virtuális gépek: forrás kiválasztása VMware folyamatkiszolgálót, mint a Microsoft Azure cél, és üzembe helyezési modellel, az erőforrás-kezelő, majd kattintson a elemeket választhat ki.</span><span class="sxs-lookup"><span data-stu-id="2f234-193">For VMware virtual machines: Select source as VMware process server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items.</span></span>
4. <span data-ttu-id="2f234-194">A Hyper-V virtuális gépek: válassza ki a forrás VMM-kiszolgálóként, megcélozni a Microsoft Azure és a telepítési modell, erőforrás-kezelő és kattintson a elemeket választhat ki majd hello XenApp telepítési virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2f234-194">For Hyper-V virtual machines: Select source as VMM server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items and then select hello XenApp deployment VMs.</span></span>

### <a name="adding-virtual-machines-toofailover-groups"></a><span data-ttu-id="2f234-195">Virtuális gépek toofailover csoportok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f234-195">Adding virtual machines toofailover groups</span></span>

<span data-ttu-id="2f234-196">A helyreállítási terv testreszabott tooadd feladatátvételi csoportok az adott indítási sorrend, parancsfájlok, illetve manuális műveletek is lehet.</span><span class="sxs-lookup"><span data-stu-id="2f234-196">Recovery plans can be customized tooadd failover groups for specific startup order, scripts or manual actions.</span></span> <span data-ttu-id="2f234-197">a következő csoportok hello kell toobe hozzáadott toohello helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="2f234-197">hello following groups need toobe added toohello recovery plan.</span></span>

1. <span data-ttu-id="2f234-198">Feladatátvételi csoport1: AD DNS</span><span class="sxs-lookup"><span data-stu-id="2f234-198">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="2f234-199">Feladatátvételi csoport2: SQL Server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="2f234-199">Failover Group2: SQL Server VMs</span></span>
2. <span data-ttu-id="2f234-200">Feladatátvételi 3: VDA fő kép VM</span><span class="sxs-lookup"><span data-stu-id="2f234-200">Failover Group3: VDA Master Image VM</span></span>
3. <span data-ttu-id="2f234-201">Feladatátvételi 4: Kézbesítési vezérlő és kirakat server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="2f234-201">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>


### <a name="adding-scripts-toohello-recovery-plan"></a><span data-ttu-id="2f234-202">A parancsfájlok toohello helyreállítási terv hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f234-202">Adding scripts toohello recovery plan</span></span>

<span data-ttu-id="2f234-203">Parancsfájlokat lehet futtatni előtt vagy után egy adott csoport a helyreállítási tervet.</span><span class="sxs-lookup"><span data-stu-id="2f234-203">Scripts can be run before or after a specific group in a recovery plan.</span></span> <span data-ttu-id="2f234-204">Manuális műveletek lehet is része és feladatátvétel során.</span><span class="sxs-lookup"><span data-stu-id="2f234-204">Manual actions can be also be included and performed during failover.</span></span>

<span data-ttu-id="2f234-205">hello testreszabott helyreállítási tervben az alábbi hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="2f234-205">hello customized recovery plan looks like hello below:</span></span>

1. <span data-ttu-id="2f234-206">Feladatátvételi csoport1: AD DNS</span><span class="sxs-lookup"><span data-stu-id="2f234-206">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="2f234-207">Feladatátvételi csoport2: SQL Server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="2f234-207">Failover Group2: SQL Server VMs</span></span>
3. <span data-ttu-id="2f234-208">Feladatátvételi 3: VDA fő kép VM</span><span class="sxs-lookup"><span data-stu-id="2f234-208">Failover Group3: VDA Master Image VM</span></span>

   >[!NOTE]     
   ><span data-ttu-id="2f234-209">4, 6, 7 tartalmazó manuális vagy parancsfájl műveletek lépésekre vonatkozó tooonly egy helyszíni XenApp > MCS/PVS katalógusok környezetben.</span><span class="sxs-lookup"><span data-stu-id="2f234-209">Steps 4, 6 and 7 containing manual or script actions are applicable tooonly an on-premises XenApp >environment with MCS/PVS catalogs.</span></span>

4. <span data-ttu-id="2f234-210">3. csoport kézi vagy parancsfájl művelet: leállítási fő VDA VM hello átadja a feladatokat tooAzure fő VDA virtuális gép futó állapotban lesz.</span><span class="sxs-lookup"><span data-stu-id="2f234-210">Group 3 Manual or script action: Shutdown master VDA VM hello Master VDA VM when failed over tooAzure will be in a running state.</span></span> <span data-ttu-id="2f234-211">toocreate új MCS katalógusok Azure ARM használatával, hello fő VDA VM szükség toobe a Leállítva (de lefoglalt) állapotát.</span><span class="sxs-lookup"><span data-stu-id="2f234-211">toocreate new MCS catalogs using Azure ARM hosting, hello master VDA VM is required toobe in Stopped (de allocated) state.</span></span> <span data-ttu-id="2f234-212">Leállítás hello VM Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="2f234-212">Shutdown hello VM from Azure Portal.</span></span>

5. <span data-ttu-id="2f234-213">Feladatátvételi 4: Kézbesítési vezérlő és kirakat server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="2f234-213">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>
6. <span data-ttu-id="2f234-214">3 manuális vagy parancsfájl 1 művelet:</span><span class="sxs-lookup"><span data-stu-id="2f234-214">Group3 manual or script action 1:</span></span>

    <span data-ttu-id="2f234-215">***Azure RM állomás kapcsolat hozzáadása***</span><span class="sxs-lookup"><span data-stu-id="2f234-215">***Add Azure RM host connection***</span></span>

    <span data-ttu-id="2f234-216">Hozzon létre Azure ARM-gazdagép kapcsolati kézbesítési vezérlő gép tooprovision új MCS katalógusok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2f234-216">Create Azure ARM host connection in Delivery Controller machine tooprovision new MCS catalogs in Azure.</span></span> <span data-ttu-id="2f234-217">Hello lépésekkel, ez a [cikk](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span><span class="sxs-lookup"><span data-stu-id="2f234-217">Follow hello steps as explained in this [article](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span></span>

7. <span data-ttu-id="2f234-218">3 manuális vagy parancsfájl művelet 2:</span><span class="sxs-lookup"><span data-stu-id="2f234-218">Group3 manual or script action 2:</span></span>

    <span data-ttu-id="2f234-219">***Hozza létre az MCS katalógusok az Azure-ban***</span><span class="sxs-lookup"><span data-stu-id="2f234-219">***Re-create MCS Catalogs in Azure***</span></span>

    <span data-ttu-id="2f234-220">az MCS vagy PVS klónok hello elsődleges hely meglévő hello nem lesz replikált tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2f234-220">hello existing MCS or PVS clones on hello primary site will not be replicated tooAzure.</span></span> <span data-ttu-id="2f234-221">Ezek segítségével hello klónok replikálva fő VDA és Azure ARM-kiépítés kézbesítési vezérlőből toorecreate van szüksége. Hello lépésekkel, ez a [cikk](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate MCS katalogizálása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2f234-221">You need toorecreate these clones using hello replicated master VDA and Azure ARM provisioning from Delivery controller.Follow hello steps as explained in this [article](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate MCS catalogs in Azure.</span></span>

![Helyreállítási terv XenApp összetevők](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   ><span data-ttu-id="2f234-223">A parancsfájlokat [hely](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS az új IP-címek hello feladatátvételt hello > virtuális gépek vagy tooattach hello a terheléselosztó nem sikerült átadó virtuális gép, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="2f234-223">You can use scripts at [location](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS with hello new IPs of hello failed over >virtual machines or tooattach a load balancer on hello failed over virtual machine, if needed.</span></span>


## <a name="doing-a-test-failover"></a><span data-ttu-id="2f234-224">A teszt feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="2f234-224">Doing a test failover</span></span>

<span data-ttu-id="2f234-225">Hajtsa végre a [Ez az útmutató](site-recovery-test-failover-to-azure.md) toodo feladatátvételi tesztet.</span><span class="sxs-lookup"><span data-stu-id="2f234-225">Follow [this guidance](site-recovery-test-failover-to-azure.md) toodo a test failover.</span></span>

![Helyreállítási terv](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a><span data-ttu-id="2f234-227">A feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="2f234-227">Doing a failover</span></span>

<span data-ttu-id="2f234-228">Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) Ha feladatátvételt végez.</span><span class="sxs-lookup"><span data-stu-id="2f234-228">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f234-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f234-229">Next steps</span></span>

<span data-ttu-id="2f234-230">Is [további](https://aka.ms/citrix-xenapp-xendesktop-with-asr) kapcsolatos Citrix XenApp és XenDesktop a központi telepítések Ez a dokumentum replikálása.</span><span class="sxs-lookup"><span data-stu-id="2f234-230">You can [learn more](https://aka.ms/citrix-xenapp-xendesktop-with-asr) about replicating Citrix XenApp and XenDesktop deployments  in this white paper.</span></span> <span data-ttu-id="2f234-231">Tekintse meg hello útmutatást túl[replikálja más alkalmazások](site-recovery-workload.md) Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="2f234-231">Look at hello guidance too[replicate other applications](site-recovery-workload.md) using Site Recovery.</span></span>
