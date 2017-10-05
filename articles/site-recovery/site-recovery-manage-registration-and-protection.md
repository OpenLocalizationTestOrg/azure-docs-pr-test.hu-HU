---
title: "Távolítsa el a kiszolgálókat, és tiltsa le a védelmet |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan a Site Recovery-tároló kiszolgálók regisztrációját, és tiltsa le a védelmet a virtuális gépek és fizikai kiszolgálók."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: raynew
ms.openlocfilehash: 43f92a35dc9b04584badd1c9f1152470246b5012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="remove-servers-and-disable-protection"></a><span data-ttu-id="3839b-103">Kiszolgálók eltávolítása és a védelem letiltása</span><span class="sxs-lookup"><span data-stu-id="3839b-103">Remove servers and disable protection</span></span>

<span data-ttu-id="3839b-104">Az üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia hozzájárul az Azure Site Recovery szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="3839b-104">The Azure Site Recovery service contributes to your business continuity and disaster recovery (BCDR) strategy.</span></span> <span data-ttu-id="3839b-105">A szolgáltatás koordinálja a replikációt, a feladatátvételi és a helyreállítási virtuális gépek és fizikai kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="3839b-105">The service orchestrates replication, failover and recovery of virtual machines and physical servers.</span></span> <span data-ttu-id="3839b-106">A gépeket az Azure-ba, vagy egy másodlagos helyszíni adatközpontba replikálhatja.</span><span class="sxs-lookup"><span data-stu-id="3839b-106">Machines can be replicated to Azure, or to a secondary on-premises data center.</span></span> <span data-ttu-id="3839b-107">A szolgáltatás gyors áttekintéséhez olvassa el az [Azure Site Recovery szolgáltatás mibenlétével foglalkozó](site-recovery-overview.md) cikket.</span><span class="sxs-lookup"><span data-stu-id="3839b-107">For a quick overview read [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>

<span data-ttu-id="3839b-108">Ez a cikk ismerteti az Azure portálon Recovery Services-tároló kiszolgálók regisztrációját, és tiltsa le a védelmet a Site Recovery által védett gépek.</span><span class="sxs-lookup"><span data-stu-id="3839b-108">This article describes how to unregister servers from a Recovery Services vault in the Azure portal, and how to disable protection for machines protected by Site Recovery.</span></span>

<span data-ttu-id="3839b-109">Megjegyzéseit vagy kérdéseit a cikk alján, vagy az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.</span><span class="sxs-lookup"><span data-stu-id="3839b-109">Post any comments or questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="unregister-a-connected-configuration-server"></a><span data-ttu-id="3839b-110">A csatlakoztatott konfigurációs kiszolgáló regisztrációját</span><span class="sxs-lookup"><span data-stu-id="3839b-110">Unregister a connected configuration server</span></span>

<span data-ttu-id="3839b-111">Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók replikálása az Azure-ba, az alábbiak szerint tudja törölni a tárolóból egy csatlakoztatott konfigurációs kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="3839b-111">If you replicate VMware VMs or Windows/Linux physical servers to Azure, you can unregister a connected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="3839b-112">Tiltsa le a gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="3839b-112">Disable machine protection.</span></span> <span data-ttu-id="3839b-113">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-113">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="3839b-114">Szüntesse meg a szabályzatoknak.</span><span class="sxs-lookup"><span data-stu-id="3839b-114">Disassociate any policies.</span></span> <span data-ttu-id="3839b-115">A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **replikációs házirendek**, kattintson duplán a tartozó házirend.</span><span class="sxs-lookup"><span data-stu-id="3839b-115">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="3839b-116">Kattintson a jobb gombbal a kiszolgáló > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="3839b-116">Right-click the configuration server > **Disassociate**.</span></span>
3. <span data-ttu-id="3839b-117">Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="3839b-117">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="3839b-118">A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal a kiszolgáló > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-118">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click the server > **Delete**.</span></span>
4. <span data-ttu-id="3839b-119">A konfigurációs kiszolgáló törlése.</span><span class="sxs-lookup"><span data-stu-id="3839b-119">Delete the configuration server.</span></span>
5. <span data-ttu-id="3839b-120">Manuálisan távolítsa el a fő célkiszolgálón futó mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a konfigurációs kiszolgálón futó).</span><span class="sxs-lookup"><span data-stu-id="3839b-120">Manually uninstall the Mobility service running on the master target server (this will be either a separate server, or running on the configuration server).</span></span>
6. <span data-ttu-id="3839b-121">Távolítsa el a további folyamat kiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="3839b-121">Uninstall any additional process servers.</span></span>
7. <span data-ttu-id="3839b-122">Távolítsa el a konfigurációs kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="3839b-122">Uninstall the configuration server.</span></span>
8. <span data-ttu-id="3839b-123">A konfigurációs kiszolgálón, távolítsa el a MySQL Site Recovery által telepített példányát.</span><span class="sxs-lookup"><span data-stu-id="3839b-123">On the configuration server, uninstall the instance of MySQL that was installed by Site Recovery.</span></span>
9. <span data-ttu-id="3839b-124">A konfigurációs kiszolgáló beállításjegyzékében a kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="3839b-124">In the registry of the configuration server delete the key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-unconnected-configuration-server"></a><span data-ttu-id="3839b-125">A frissíthető konfigurációs kiszolgáló regisztrációját</span><span class="sxs-lookup"><span data-stu-id="3839b-125">Unregister a unconnected configuration server</span></span>

<span data-ttu-id="3839b-126">Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók replikálása az Azure-ba, az alábbiak szerint tudja törölni a tárolóból egy frissíthető konfigurációs kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="3839b-126">If you replicate VMware VMs or Windows/Linux physical servers to Azure, you can unregister an unconnected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="3839b-127">Tiltsa le a gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="3839b-127">Disable machine protection.</span></span> <span data-ttu-id="3839b-128">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-128">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span> <span data-ttu-id="3839b-129">Válassza ki **gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="3839b-129">Select **Stop managing the machine**.</span></span>
2. <span data-ttu-id="3839b-130">Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="3839b-130">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="3839b-131">A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal a kiszolgáló > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-131">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click the server > **Delete**.</span></span>
3. <span data-ttu-id="3839b-132">A konfigurációs kiszolgáló törlése.</span><span class="sxs-lookup"><span data-stu-id="3839b-132">Delete the configuration server.</span></span>
4. <span data-ttu-id="3839b-133">Manuálisan távolítsa el a fő célkiszolgálón futó mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a konfigurációs kiszolgálón futó).</span><span class="sxs-lookup"><span data-stu-id="3839b-133">Manually uninstall the Mobility service running on the master target server (this will be either a separate server, or running on the configuration server).</span></span>
5. <span data-ttu-id="3839b-134">Távolítsa el a további folyamat kiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="3839b-134">Uninstall any additional process servers.</span></span>
6. <span data-ttu-id="3839b-135">Távolítsa el a konfigurációs kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="3839b-135">Uninstall the configuration server.</span></span>
7. <span data-ttu-id="3839b-136">A konfigurációs kiszolgálón, távolítsa el a MySQL Site Recovery által telepített példányát.</span><span class="sxs-lookup"><span data-stu-id="3839b-136">On the configuration server, uninstall the instance of MySQL that was installed by Site Recovery.</span></span>
8. <span data-ttu-id="3839b-137">A konfigurációs kiszolgáló beállításjegyzékében a kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="3839b-137">In the registry of the configuration server delete the key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-connected-vmm-server"></a><span data-ttu-id="3839b-138">Egy csatlakoztatott VMM-kiszolgáló regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="3839b-138">Unregister a connected VMM server</span></span>

<span data-ttu-id="3839b-139">Ajánlott eljárásként azt javasoljuk, hogy a VMM-kiszolgáló regisztrációjának törlése Azure való csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="3839b-139">As a best practice, we recommend that you unregister the VMM server when it's connected to Azure.</span></span> <span data-ttu-id="3839b-140">Ez biztosítja, hogy a VMM-kiszolgáló (és más VMM-kiszolgálókon és párosított) beállításainak megtisztítva megfelelően.</span><span class="sxs-lookup"><span data-stu-id="3839b-140">This ensures that settings on the VMM servers (and on other VMM servers with paired clouds) are cleaned up properly.</span></span> <span data-ttu-id="3839b-141">Frissíthető kiszolgáló csak akkor törlése, ha probléma van a végleges megfeleléssel összefüggő.</span><span class="sxs-lookup"><span data-stu-id="3839b-141">You should only remove an unconnected server if there's a permanent issue with connectivity.</span></span> <span data-ttu-id="3839b-142">Ha a VMM-kiszolgáló nincs csatlakoztatva, szüksége lesz manuálisan a beállításokat egy parancsfájlt futtathat.</span><span class="sxs-lookup"><span data-stu-id="3839b-142">If the VMM server isn’t connected, you will need to manually run a script to clean up settings.</span></span>

1. <span data-ttu-id="3839b-143">Állítsa le a felhőket a VMM-kiszolgálón el szeretné távolítani a virtuális gépek replikálása.</span><span class="sxs-lookup"><span data-stu-id="3839b-143">Stop replicating VMs in clouds on the VMM server you want to remove.</span></span>
2. <span data-ttu-id="3839b-144">Törölje a törölni kívánt VMM-kiszolgálón felhők által használt hálózati leképezések.</span><span class="sxs-lookup"><span data-stu-id="3839b-144">Delete any network mappings used by clouds on the VMM server you want to delete.</span></span> <span data-ttu-id="3839b-145">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-145">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click the network mapping > **Delete**.</span></span>
3. <span data-ttu-id="3839b-146">Felhőből a VMM-kiszolgálón szeretné eltávolítani a replikációs házirend társítását.</span><span class="sxs-lookup"><span data-stu-id="3839b-146">Disassociate replication policies from clouds on the VMM server you want to remove.</span></span>  <span data-ttu-id="3839b-147">A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a tartozó házirend.</span><span class="sxs-lookup"><span data-stu-id="3839b-147">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="3839b-148">Kattintson a jobb gombbal a felhő > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="3839b-148">Right-click the cloud > **Disassociate**.</span></span>
4. <span data-ttu-id="3839b-149">Törölje a VMM-kiszolgáló vagy az aktív VMM-csomópont.</span><span class="sxs-lookup"><span data-stu-id="3839b-149">Delete the VMM server or active VMM node.</span></span> <span data-ttu-id="3839b-150">A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal a kiszolgáló > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-150">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click the server > **Delete**.</span></span>
5. <span data-ttu-id="3839b-151">Távolítsa el manuálisan a VMM-kiszolgálón a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="3839b-151">Uninstall the Provider manually on the VMM server.</span></span> <span data-ttu-id="3839b-152">Ha rendelkezik fürttel, távolítsa el az összes csomópontot.</span><span class="sxs-lookup"><span data-stu-id="3839b-152">If you have a cluster, remove from all nodes.</span></span>
6. <span data-ttu-id="3839b-153">Ha az Azure-bA replikál, manuálisan távolítsa el a Microsoft a Recovery Services agent Hyper-V gazdagépek a törölt felhőkben.</span><span class="sxs-lookup"><span data-stu-id="3839b-153">If you're replicating to Azure, manually remove the Microsoft Recovery Services agent from Hyper-V hosts in the deleted clouds.</span></span>



### <a name="unregister-an-unconnected-vmm-server"></a><span data-ttu-id="3839b-154">Az frissíthető a VMM-kiszolgáló regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="3839b-154">Unregister an unconnected VMM server</span></span>

1. <span data-ttu-id="3839b-155">Állítsa le a felhőket a VMM-kiszolgálón el szeretné távolítani a virtuális gépek replikálása.</span><span class="sxs-lookup"><span data-stu-id="3839b-155">Stop replicating VMs in clouds on the VMM server you want to remove.</span></span>
2. <span data-ttu-id="3839b-156">Törölje a VMM-kiszolgálón, amely a törölni kívánt felhők által használt hálózati leképezések.</span><span class="sxs-lookup"><span data-stu-id="3839b-156">Delete any network mappings used by clouds on the VMM server that you want to delete.</span></span> <span data-ttu-id="3839b-157">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-157">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click the network mapping > **Delete**.</span></span>
3. <span data-ttu-id="3839b-158">Vegye figyelembe a VMM-kiszolgáló Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="3839b-158">Note the ID of the VMM server.</span></span>
4. <span data-ttu-id="3839b-159">Felhőből a VMM-kiszolgálón szeretné eltávolítani a replikációs házirend társítását.</span><span class="sxs-lookup"><span data-stu-id="3839b-159">Disassociate replication policies from clouds on the VMM server you want to remove.</span></span>  <span data-ttu-id="3839b-160">A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a tartozó házirend.</span><span class="sxs-lookup"><span data-stu-id="3839b-160">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="3839b-161">Kattintson a jobb gombbal a felhő > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="3839b-161">Right-click the cloud > **Disassociate**.</span></span>
5. <span data-ttu-id="3839b-162">Törölje a VMM-kiszolgáló vagy az aktív csomópontra.</span><span class="sxs-lookup"><span data-stu-id="3839b-162">Delete the VMM server or active node.</span></span> <span data-ttu-id="3839b-163">A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal a kiszolgáló > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-163">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click the server > **Delete**.</span></span>
6. <span data-ttu-id="3839b-164">Töltse le és futtassa a [karbantartási parancsprogramot](http://aka.ms/asr-cleanup-script-vmm) a VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="3839b-164">Download and run the [cleanup script](http://aka.ms/asr-cleanup-script-vmm) on the VMM server.</span></span> <span data-ttu-id="3839b-165">Nyissa meg a PowerShell használata a **Futtatás rendszergazdaként** kapcsoló, a végrehajtási házirend az alapértelmezett (LocalMachine) hatókör módosítására.</span><span class="sxs-lookup"><span data-stu-id="3839b-165">Open PowerShell with the **Run as Administrator** option, to change the execution policy for the default (LocalMachine) scope.</span></span> <span data-ttu-id="3839b-166">A parancsfájl adja meg az Azonosítót, a VMM-kiszolgáló el szeretné távolítani.</span><span class="sxs-lookup"><span data-stu-id="3839b-166">In the script, specify the ID of the VMM server you want to remove.</span></span> <span data-ttu-id="3839b-167">A parancsfájl a regisztrációs és felhőpárosítási adatait a kiszolgálóról távolítja el.</span><span class="sxs-lookup"><span data-stu-id="3839b-167">The script removes registration and cloud pairing information from the server.</span></span>
5. <span data-ttu-id="3839b-168">Futtassa a karbantartási parancsprogramot bármely más VMM-kiszolgálókon, amelyek tartalmazzák a felhők eltávolítani kívánt VMM-kiszolgálón van párosítva-felhő.</span><span class="sxs-lookup"><span data-stu-id="3839b-168">Run the cleanup script on any other VMM servers that contain clouds that are paired with clouds on the VMM server you want to remove.</span></span>
6. <span data-ttu-id="3839b-169">Futtassa a karbantartási parancsprogramot a bármely más passzív VMM-fürtcsomóponton, hogy a szolgáltató telepítve.</span><span class="sxs-lookup"><span data-stu-id="3839b-169">Run the  cleanup script on any other passive VMM cluster nodes that have the Provider installed.</span></span>
7. <span data-ttu-id="3839b-170">Távolítsa el manuálisan a VMM-kiszolgálón a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="3839b-170">Uninstall the Provider manually on the VMM server.</span></span> <span data-ttu-id="3839b-171">Ha rendelkezik fürttel, távolítsa el az összes csomópontot.</span><span class="sxs-lookup"><span data-stu-id="3839b-171">If you have a cluster, remove from all nodes.</span></span>
8. <span data-ttu-id="3839b-172">Ha Ön replikálása Azure-ba, eltávolíthatja a Microsoft Recovery Services Agent ügynököt a Hyper-V gazdagépekről, a törölt felhőkben.</span><span class="sxs-lookup"><span data-stu-id="3839b-172">If you replicating to Azure, you can remove the Microsoft Recovery Services agent from Hyper-V hosts in the deleted clouds.</span></span>

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a><span data-ttu-id="3839b-173">A Hyper-V gazdagépet a Hyper-V hely regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="3839b-173">Unregister a Hyper-V host in a Hyper-V Site</span></span>

<span data-ttu-id="3839b-174">A VMM által nem felügyelt Hyper-V-gazdagépek egy Hyper-V helyre gyűjti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="3839b-174">Hyper-V hosts that aren't managed by VMM are gathered into a Hyper-V site.</span></span> <span data-ttu-id="3839b-175">Távolítsa el a gazdagép egy Hyper-V hely az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3839b-175">Remove a host in a Hyper-V site as follows:</span></span>

1. <span data-ttu-id="3839b-176">Tiltsa le a replikációt a gazdagépen található Hyper-V virtuális gépek esetén.</span><span class="sxs-lookup"><span data-stu-id="3839b-176">Disable replication for Hyper-V VMs located on the host.</span></span>
2. <span data-ttu-id="3839b-177">Szüntesse meg a Hyper-V hely házirendek.</span><span class="sxs-lookup"><span data-stu-id="3839b-177">Disassociate policies for the Hyper-V site.</span></span> <span data-ttu-id="3839b-178">A **Site Recovery-infrastruktúra** > **a Hyper-V helyek** >  **replikációs házirendek**, kattintson duplán a tartozó házirend.</span><span class="sxs-lookup"><span data-stu-id="3839b-178">In **Site Recovery Infrastructure** > **For Hyper-V Sites** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="3839b-179">Kattintson a jobb gombbal a hely > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="3839b-179">Right-click the site > **Disassociate**.</span></span>
3. <span data-ttu-id="3839b-180">Hyper-V-gazdagépek törlése.</span><span class="sxs-lookup"><span data-stu-id="3839b-180">Delete Hyper-V hosts.</span></span> <span data-ttu-id="3839b-181">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V-gazdagépek**, kattintson a jobb gombbal a kiszolgáló > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-181">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Hosts**, right-click the server > **Delete**.</span></span>
4. <span data-ttu-id="3839b-182">Hyper-V hely törlése után minden gazdagép el lettek távolítva belőle.</span><span class="sxs-lookup"><span data-stu-id="3839b-182">Delete the Hyper-V site after all hosts have been removed from it.</span></span> <span data-ttu-id="3839b-183">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V helyek**, kattintson a jobb gombbal a hely > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-183">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Sites**, right-click the site > **Delete**.</span></span>
5. <span data-ttu-id="3839b-184">Futtassa az alábbi parancsfájlt minden Hyper-V állomáson eltávolított.</span><span class="sxs-lookup"><span data-stu-id="3839b-184">Run the following script on each Hyper-V host that you removed.</span></span> <span data-ttu-id="3839b-185">A parancsfájl a szükségtelenné vált beállításait a kiszolgálón, és megszünteti azt a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="3839b-185">The script cleans up settings on the server, and unregisters it from the vault.</span></span>


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a><span data-ttu-id="3839b-186">Tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="3839b-186">Disable protection for a VMware VM or physical server</span></span>

1. <span data-ttu-id="3839b-187">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-187">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="3839b-188">A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:</span><span class="sxs-lookup"><span data-stu-id="3839b-188">In **Remove Machine**, select one of these options:</span></span>
    - <span data-ttu-id="3839b-189">**(Ajánlott) a gép védelmének letiltása**.</span><span class="sxs-lookup"><span data-stu-id="3839b-189">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="3839b-190">Ez a beállítás használatával állítsa le a gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="3839b-190">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="3839b-191">Webhely-helyreállítási beállításai automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="3839b-191">Site Recovery settings will be cleaned up automatically.</span></span> <span data-ttu-id="3839b-192">Ez a beállítás a következő körülmények között csak akkor jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3839b-192">You will only see this option in the following circumstances:</span></span>
        - <span data-ttu-id="3839b-193">**Már átméretezte a virtuális gép kötet**– Ha azt szeretné, hogy a kötet a virtuális gép állapota kritikus állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="3839b-193">**You've resized the VM volume**—When you resize a volume the virtual machine goes into a critical state.</span></span> <span data-ttu-id="3839b-194">Válassza ezt a beállítást, letiltja a védelem során az Azure-ban a helyreállítási pontok megőrzése.</span><span class="sxs-lookup"><span data-stu-id="3839b-194">Select this option to disables protection while retaining recovery points in Azure.</span></span> <span data-ttu-id="3839b-195">Ha engedélyezi a gép védelmét, az átméretezett kötet adatait Azure használatával lesz áthelyezve.</span><span class="sxs-lookup"><span data-stu-id="3839b-195">When you enable protection for the machine again, the data for the resized volume will be transferred to Azure.</span></span>
        - <span data-ttu-id="3839b-196">**Nemrég futtatását feladatátvevő**– egy feladatátvételi teszt a környezet futtatása után válassza ezt a beállítást, a helyszíni gépeket újra védelmének megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="3839b-196">**You've recently run a failover**—After you've run a failover to test your environment, select this option to start protecting on-premises machines again.</span></span> <span data-ttu-id="3839b-197">Letiltja a virtuális gépeken, és ezután engedélyeznie kell azokat újra védelmét.</span><span class="sxs-lookup"><span data-stu-id="3839b-197">It disables each virtual machine, and then you need to enable protection for them again.</span></span> <span data-ttu-id="3839b-198">Letiltja a gépet, amelynek ez a beállítás nincs hatással a replika virtuális gép az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3839b-198">Disabling the machine with this setting doesn't affect the replica virtual machine in Azure.</span></span> <span data-ttu-id="3839b-199">Ne távolítsa el a mobilitási szolgáltatást a számítógépről.</span><span class="sxs-lookup"><span data-stu-id="3839b-199">Don't uninstall the Mobility service from the machine.</span></span>
    - <span data-ttu-id="3839b-200">**Gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="3839b-200">**Stop managing the machine**.</span></span> <span data-ttu-id="3839b-201">Ha ezt a beállítást, a gép csak törlődni fognak a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="3839b-201">If you select this option, the machine will only be removed from the vault.</span></span> <span data-ttu-id="3839b-202">A helyszíni védelmi beállítások a gép nem érinti.</span><span class="sxs-lookup"><span data-stu-id="3839b-202">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="3839b-203">Távolítsa el a számítógép beállításait, és távolítsa el a gépet az Azure-előfizetés, kell tisztítsa meg a beállításokat a mobilitási szolgáltatás eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="3839b-203">To remove settings on the machine, and to remove the machine from the Azure subscription, you need to clean the settings by uninstalling the Mobility service.</span></span>

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a><span data-ttu-id="3839b-204">Tiltsa le a védelmet a Hyper-V virtuális gépek VMM-felhőben</span><span class="sxs-lookup"><span data-stu-id="3839b-204">Disable protection for a Hyper-V VM in a VMM cloud</span></span>

1. <span data-ttu-id="3839b-205">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-205">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="3839b-206">A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:</span><span class="sxs-lookup"><span data-stu-id="3839b-206">In **Remove Machine**, select one of these options:</span></span>

    - <span data-ttu-id="3839b-207">**(Ajánlott) a gép védelmének letiltása**.</span><span class="sxs-lookup"><span data-stu-id="3839b-207">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="3839b-208">Ez a beállítás használatával állítsa le a gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="3839b-208">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="3839b-209">Webhely-helyreállítási beállításai automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="3839b-209">Site Recovery settings will be cleaned up automatically.</span></span>
    - <span data-ttu-id="3839b-210">**Gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="3839b-210">**Stop managing the machine**.</span></span> <span data-ttu-id="3839b-211">Ha ezt a beállítást, a gép csak törlődni fognak a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="3839b-211">If you select this option, the machine will only be removed from the vault.</span></span> <span data-ttu-id="3839b-212">A helyszíni védelmi beállítások a gép nem érinti.</span><span class="sxs-lookup"><span data-stu-id="3839b-212">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="3839b-213">Távolítsa el a számítógép beállításait, és távolítsa el a gépet az Azure-előfizetés, meg kell törölnie a beállítások manuálisan, az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="3839b-213">To remove settings on the machine, and to remove the machine from the Azure subscription, you need to clean the settings up manually, using the instructions below.</span></span> <span data-ttu-id="3839b-214">Vegye figyelembe, hogy törölje a virtuális gépet és annak merevlemezei válassza ki, ha azok fogja eltávolítani a célhelyről.</span><span class="sxs-lookup"><span data-stu-id="3839b-214">Note that if you select to delete the virtual machine and its hard disks, they'll be removed from the target location.</span></span>

### <a name="clean-up-protection-settings---replication-to-a-secondary-vmm-site"></a><span data-ttu-id="3839b-215">Védelmi beállítások - replikációt, hogy a VMM másodlagos hely törlése</span><span class="sxs-lookup"><span data-stu-id="3839b-215">Clean up protection settings - replication to a secondary VMM site</span></span>

<span data-ttu-id="3839b-216">Ha a kiválasztott **gép kezelésének leállítása** és Ön replikálása egy másodlagos helyre, és futtassa ezt a parancsfájlt a beállításokat az elsődleges virtuális gép az elsődleges kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="3839b-216">If you selected **Stop managing the machine** and you replicating to a secondary site, run this script on the primary server to clean up the settings for the primary virtual machine.</span></span> <span data-ttu-id="3839b-217">A VMM-konzolon kattintson a PowerShell gombra a VMM PowerShell-konzol megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="3839b-217">In the VMM console click the PowerShell button to open the VMM PowerShell console.</span></span> <span data-ttu-id="3839b-218">SQLVM1 cserélje le a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="3839b-218">Replace SQLVM1 with the name of your virtual machine.</span></span>

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="3839b-219">A másodlagos VMM-kiszolgálón futtassa ezt a parancsfájlt a másodlagos virtuális gép beállításait:</span><span class="sxs-lookup"><span data-stu-id="3839b-219">On the secondary VMM server run this script to clean up the settings for the secondary virtual machine:</span></span>

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. <span data-ttu-id="3839b-220">A másodlagos VMM-kiszolgálón frissítse a virtuális gépek a Hyper-V gazdagép-kiszolgálón, hogy a másodlagos virtuális gép újra a VMM-konzol lekérdezi észlelte.</span><span class="sxs-lookup"><span data-stu-id="3839b-220">On the secondary VMM server, refresh the virtual machines on the Hyper-V host server, so that the secondary VM gets detected again in the VMM console.</span></span>
4. <span data-ttu-id="3839b-221">A fenti lépések kapcsolja ki a replikációs beállításokat a VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="3839b-221">The above steps clear up the replication settings on the VMM server.</span></span> <span data-ttu-id="3839b-222">Ha le kívánja állítani a virtuális gép replikálását, futtassa a következő parancsfájlt bizony az elsődleges és másodlagos virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="3839b-222">If you want to stop replication for the virtual machine, run the following script oh the primary and secondary VMs.</span></span> <span data-ttu-id="3839b-223">SQLVM1 cserélje le a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="3839b-223">Replace SQLVM1 with the name of your virtual machine.</span></span>

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-to-azure"></a><span data-ttu-id="3839b-224">Védelmi beállításokat - replikáció az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="3839b-224">Clean up protection settings - replication to Azure</span></span>

1. <span data-ttu-id="3839b-225">Ha a kiválasztott **gép kezelésének leállítása** és replikálja az Azure-ba, a parancsfájl futtatásával a forrás VMM-kiszolgálón, a PowerShell használatával a VMM-konzolról.</span><span class="sxs-lookup"><span data-stu-id="3839b-225">If you selected **Stop managing the machine** and you replicate to Azure, run this script on the source VMM server, using PowerShell from the VMM console.</span></span>
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="3839b-226">A fenti lépések törölje a replikációs beállításokat a VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="3839b-226">The above steps clear the replication settings on the VMM server.</span></span> <span data-ttu-id="3839b-227">Állítsa le a replikációt a virtuális gép a Hyper-V kiszolgálón futó, futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="3839b-227">To stop replication for the virtual machine running on the Hyper-V host server, run this script.</span></span> <span data-ttu-id="3839b-228">SQLVM1 cserélje le a virtuális gép, és a host01.contoso.com nevű, a Hyper-V gazdakiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="3839b-228">Replace SQLVM1 with the name of your virtual machine, and host01.contoso.com with the name of the Hyper-V host server.</span></span>

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a><span data-ttu-id="3839b-229">Tiltsa le a védelmet a Hyper-V virtuális gépek a Hyper-V hely</span><span class="sxs-lookup"><span data-stu-id="3839b-229">Disable protection for a Hyper-V VM in a Hyper-V Site</span></span>

<span data-ttu-id="3839b-230">Ez az eljárás használható, ha az Azure Hyper-V virtuális gépek a VMM-kiszolgáló nem replikál.</span><span class="sxs-lookup"><span data-stu-id="3839b-230">Use this procedure if you're replicating Hyper-V VMs to Azure without a VMM server.</span></span>

1. <span data-ttu-id="3839b-231">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3839b-231">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="3839b-232">A **eltávolítása gép**, a következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="3839b-232">In **Remove Machine**, you can select the following options:</span></span>

   - <span data-ttu-id="3839b-233">**(Ajánlott) a gép védelmének letiltása**.</span><span class="sxs-lookup"><span data-stu-id="3839b-233">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="3839b-234">Ez a beállítás használatával állítsa le a gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="3839b-234">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="3839b-235">Webhely-helyreállítási beállításai automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="3839b-235">Site Recovery settings will be cleaned up automatically.</span></span>
   - <span data-ttu-id="3839b-236">**Gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="3839b-236">**Stop managing the machine**.</span></span> <span data-ttu-id="3839b-237">Ha ezt a beállítást a gép csak törlődni fognak a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="3839b-237">If you select this option the machine will only be removed from the vault.</span></span> <span data-ttu-id="3839b-238">A helyszíni védelmi beállítások a gép nem érinti.</span><span class="sxs-lookup"><span data-stu-id="3839b-238">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="3839b-239">Távolítsa el a számítógép beállításait, és távolítsa el a virtuális gépet az Azure-előfizetés, tisztítása a beállításokat manuálisan kell.</span><span class="sxs-lookup"><span data-stu-id="3839b-239">To remove settings on the machine, and to remove the virtual machine from the Azure subscription, you need to clean the settings up manually.</span></span> <span data-ttu-id="3839b-240">Ha szeretné, törölje a virtuális gépet és annak merevlemezei, akkor azok törlődnek a célhelyről.</span><span class="sxs-lookup"><span data-stu-id="3839b-240">If you select to delete the virtual machine and its hard disks they will be removed from the target location.</span></span>
3. <span data-ttu-id="3839b-241">Ha a kiválasztott **gép kezelésének leállítása**, futtassa ezt a parancsfájlt a forráskiszolgálón a Hyper-V gazdagép, eltávolítja a virtuális gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="3839b-241">If you selected **Stop managing the machine**, run this script on the source Hyper-V host server, to remove replication for the virtual machine.</span></span> <span data-ttu-id="3839b-242">SQLVM1 cserélje le a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="3839b-242">Replace SQLVM1 with the name of your virtual machine.</span></span>

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
