---
title: "aaaRemove kiszolgálók és a tiltsa le a védelmet |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toounregister kiszolgálót a Site Recovery tároló és a virtuális gépek és fizikai kiszolgálók toodisable védelem."
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
ms.openlocfilehash: 95f20433f782c93685ad4bae93c6bc0e2d2f2356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-servers-and-disable-protection"></a><span data-ttu-id="000ee-103">Kiszolgálók eltávolítása és a védelem letiltása</span><span class="sxs-lookup"><span data-stu-id="000ee-103">Remove servers and disable protection</span></span>

<span data-ttu-id="000ee-104">hello Azure Site Recovery szolgáltatás hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégiát.</span><span class="sxs-lookup"><span data-stu-id="000ee-104">hello Azure Site Recovery service contributes tooyour business continuity and disaster recovery (BCDR) strategy.</span></span> <span data-ttu-id="000ee-105">hello szolgáltatás koordinálja a replikációt, a feladatátvételi és a helyreállítási virtuális gépek és fizikai kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="000ee-105">hello service orchestrates replication, failover and recovery of virtual machines and physical servers.</span></span> <span data-ttu-id="000ee-106">Gépek replikált tooAzure vagy tooa másodlagos helyszíni adatközpontba lehet.</span><span class="sxs-lookup"><span data-stu-id="000ee-106">Machines can be replicated tooAzure, or tooa secondary on-premises data center.</span></span> <span data-ttu-id="000ee-107">A szolgáltatás gyors áttekintéséhez olvassa el az [Azure Site Recovery szolgáltatás mibenlétével foglalkozó](site-recovery-overview.md) cikket.</span><span class="sxs-lookup"><span data-stu-id="000ee-107">For a quick overview read [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>

<span data-ttu-id="000ee-108">Ez a cikk ismerteti, hogyan toounregister kiszolgálókat a Recovery Services hello Azure-portálon a tároló, és hogyan Site Recovery által védett gépek toodisable védelmét.</span><span class="sxs-lookup"><span data-stu-id="000ee-108">This article describes how toounregister servers from a Recovery Services vault in hello Azure portal, and how toodisable protection for machines protected by Site Recovery.</span></span>

<span data-ttu-id="000ee-109">Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="000ee-109">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="unregister-a-connected-configuration-server"></a><span data-ttu-id="000ee-110">A csatlakoztatott konfigurációs kiszolgáló regisztrációját</span><span class="sxs-lookup"><span data-stu-id="000ee-110">Unregister a connected configuration server</span></span>

<span data-ttu-id="000ee-111">Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók tooAzure replikálja, az alábbiak szerint tudja törölni a tárolóból egy csatlakoztatott konfigurációs kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="000ee-111">If you replicate VMware VMs or Windows/Linux physical servers tooAzure, you can unregister a connected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="000ee-112">Tiltsa le a gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="000ee-112">Disable machine protection.</span></span> <span data-ttu-id="000ee-113">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="000ee-113">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="000ee-114">Szüntesse meg a szabályzatoknak.</span><span class="sxs-lookup"><span data-stu-id="000ee-114">Disassociate any policies.</span></span> <span data-ttu-id="000ee-115">A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **replikációs házirendek**, kattintson duplán a hello tartozó házirend.</span><span class="sxs-lookup"><span data-stu-id="000ee-115">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="000ee-116">Kattintson a jobb gombbal hello konfigurációs kiszolgáló > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="000ee-116">Right-click hello configuration server > **Disassociate**.</span></span>
3. <span data-ttu-id="000ee-117">Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="000ee-117">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="000ee-118">A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal hello kiszolgáló > **Törlése**.</span><span class="sxs-lookup"><span data-stu-id="000ee-118">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click hello server > **Delete**.</span></span>
4. <span data-ttu-id="000ee-119">Hello konfigurációs kiszolgáló törlése.</span><span class="sxs-lookup"><span data-stu-id="000ee-119">Delete hello configuration server.</span></span>
5. <span data-ttu-id="000ee-120">Manuálisan távolítsa el a fő célkiszolgáló hello futó hello mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a hello konfigurációs kiszolgálón futó).</span><span class="sxs-lookup"><span data-stu-id="000ee-120">Manually uninstall hello Mobility service running on hello master target server (this will be either a separate server, or running on hello configuration server).</span></span>
6. <span data-ttu-id="000ee-121">Távolítsa el a további folyamat kiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="000ee-121">Uninstall any additional process servers.</span></span>
7. <span data-ttu-id="000ee-122">Távolítsa el a hello konfigurációs kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="000ee-122">Uninstall hello configuration server.</span></span>
8. <span data-ttu-id="000ee-123">Hello konfigurációs kiszolgálón távolítsa el a Site Recovery által telepített MySQL hello példányát.</span><span class="sxs-lookup"><span data-stu-id="000ee-123">On hello configuration server, uninstall hello instance of MySQL that was installed by Site Recovery.</span></span>
9. <span data-ttu-id="000ee-124">Hello beállításjegyzék hello konfigurációs kiszolgáló hello kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="000ee-124">In hello registry of hello configuration server delete hello key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-unconnected-configuration-server"></a><span data-ttu-id="000ee-125">A frissíthető konfigurációs kiszolgáló regisztrációját</span><span class="sxs-lookup"><span data-stu-id="000ee-125">Unregister a unconnected configuration server</span></span>

<span data-ttu-id="000ee-126">Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók tooAzure replikálja, az alábbiak szerint tudja törölni a tárolóból egy frissíthető konfigurációs kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="000ee-126">If you replicate VMware VMs or Windows/Linux physical servers tooAzure, you can unregister an unconnected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="000ee-127">Tiltsa le a gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="000ee-127">Disable machine protection.</span></span> <span data-ttu-id="000ee-128">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="000ee-128">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span> <span data-ttu-id="000ee-129">Válassza ki **hello gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="000ee-129">Select **Stop managing hello machine**.</span></span>
2. <span data-ttu-id="000ee-130">Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="000ee-130">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="000ee-131">A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal hello kiszolgáló > **Törlése**.</span><span class="sxs-lookup"><span data-stu-id="000ee-131">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click hello server > **Delete**.</span></span>
3. <span data-ttu-id="000ee-132">Hello konfigurációs kiszolgáló törlése.</span><span class="sxs-lookup"><span data-stu-id="000ee-132">Delete hello configuration server.</span></span>
4. <span data-ttu-id="000ee-133">Manuálisan távolítsa el a fő célkiszolgáló hello futó hello mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a hello konfigurációs kiszolgálón futó).</span><span class="sxs-lookup"><span data-stu-id="000ee-133">Manually uninstall hello Mobility service running on hello master target server (this will be either a separate server, or running on hello configuration server).</span></span>
5. <span data-ttu-id="000ee-134">Távolítsa el a további folyamat kiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="000ee-134">Uninstall any additional process servers.</span></span>
6. <span data-ttu-id="000ee-135">Távolítsa el a hello konfigurációs kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="000ee-135">Uninstall hello configuration server.</span></span>
7. <span data-ttu-id="000ee-136">Hello konfigurációs kiszolgálón távolítsa el a Site Recovery által telepített MySQL hello példányát.</span><span class="sxs-lookup"><span data-stu-id="000ee-136">On hello configuration server, uninstall hello instance of MySQL that was installed by Site Recovery.</span></span>
8. <span data-ttu-id="000ee-137">Hello beállításjegyzék hello konfigurációs kiszolgáló hello kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="000ee-137">In hello registry of hello configuration server delete hello key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-connected-vmm-server"></a><span data-ttu-id="000ee-138">Egy csatlakoztatott VMM-kiszolgáló regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="000ee-138">Unregister a connected VMM server</span></span>

<span data-ttu-id="000ee-139">Ajánlott eljárásként azt javasoljuk hello VMM-kiszolgáló regisztrációjának törlése, ha tooAzure van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="000ee-139">As a best practice, we recommend that you unregister hello VMM server when it's connected tooAzure.</span></span> <span data-ttu-id="000ee-140">Ez biztosítja, hogy hello VMM-kiszolgáló (és más VMM-kiszolgálókon és párosított) beállításainak megtisztítva megfelelően.</span><span class="sxs-lookup"><span data-stu-id="000ee-140">This ensures that settings on hello VMM servers (and on other VMM servers with paired clouds) are cleaned up properly.</span></span> <span data-ttu-id="000ee-141">Frissíthető kiszolgáló csak akkor törlése, ha probléma van a végleges megfeleléssel összefüggő.</span><span class="sxs-lookup"><span data-stu-id="000ee-141">You should only remove an unconnected server if there's a permanent issue with connectivity.</span></span> <span data-ttu-id="000ee-142">Ha hello VMM-kiszolgáló nincs csatlakoztatva, szüksége lesz toomanually futtatni egy parancsfájlt tooclean beállításokat.</span><span class="sxs-lookup"><span data-stu-id="000ee-142">If hello VMM server isn’t connected, you will need toomanually run a script tooclean up settings.</span></span>

1. <span data-ttu-id="000ee-143">Állítsa le a virtuális gépek VMM-kiszolgálónak tooremove hello felhőkben replikálása.</span><span class="sxs-lookup"><span data-stu-id="000ee-143">Stop replicating VMs in clouds on hello VMM server you want tooremove.</span></span>
2. <span data-ttu-id="000ee-144">Törölje a felhő hello toodelete kívánt VMM-kiszolgáló által használt hálózati leképezések.</span><span class="sxs-lookup"><span data-stu-id="000ee-144">Delete any network mappings used by clouds on hello VMM server you want toodelete.</span></span> <span data-ttu-id="000ee-145">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés hello >  **Törlés**.</span><span class="sxs-lookup"><span data-stu-id="000ee-145">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click hello network mapping > **Delete**.</span></span>
3. <span data-ttu-id="000ee-146">Szüntesse meg a VMM-kiszolgálónak tooremove hello felhőből replikációs házirendek.</span><span class="sxs-lookup"><span data-stu-id="000ee-146">Disassociate replication policies from clouds on hello VMM server you want tooremove.</span></span>  <span data-ttu-id="000ee-147">A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a kapcsolódó hello házirend.</span><span class="sxs-lookup"><span data-stu-id="000ee-147">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="000ee-148">Kattintson a jobb gombbal a hello felhő > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="000ee-148">Right-click hello cloud > **Disassociate**.</span></span>
4. <span data-ttu-id="000ee-149">Hello VMM-kiszolgáló vagy az aktív VMM-csomópont törlése.</span><span class="sxs-lookup"><span data-stu-id="000ee-149">Delete hello VMM server or active VMM node.</span></span> <span data-ttu-id="000ee-150">A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal hello server >  **Törlés**.</span><span class="sxs-lookup"><span data-stu-id="000ee-150">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click hello server > **Delete**.</span></span>
5. <span data-ttu-id="000ee-151">Távolítsa el manuálisan a VMM-kiszolgáló hello szolgáltató hello.</span><span class="sxs-lookup"><span data-stu-id="000ee-151">Uninstall hello Provider manually on hello VMM server.</span></span> <span data-ttu-id="000ee-152">Ha rendelkezik fürttel, távolítsa el az összes csomópontot.</span><span class="sxs-lookup"><span data-stu-id="000ee-152">If you have a cluster, remove from all nodes.</span></span>
6. <span data-ttu-id="000ee-153">TooAzure replikál, hello Microsoft Recovery Services Agent ügynököt manuálisan távolítsa el a Hyper-V gazdagépekről törölt hello felhőkben.</span><span class="sxs-lookup"><span data-stu-id="000ee-153">If you're replicating tooAzure, manually remove hello Microsoft Recovery Services agent from Hyper-V hosts in hello deleted clouds.</span></span>



### <a name="unregister-an-unconnected-vmm-server"></a><span data-ttu-id="000ee-154">Az frissíthető a VMM-kiszolgáló regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="000ee-154">Unregister an unconnected VMM server</span></span>

1. <span data-ttu-id="000ee-155">Állítsa le a virtuális gépek VMM-kiszolgálónak tooremove hello felhőkben replikálása.</span><span class="sxs-lookup"><span data-stu-id="000ee-155">Stop replicating VMs in clouds on hello VMM server you want tooremove.</span></span>
2. <span data-ttu-id="000ee-156">A felhők, amelyet az toodelete hello VMM-kiszolgáló által használt hálózati leképezéseket törölhet.</span><span class="sxs-lookup"><span data-stu-id="000ee-156">Delete any network mappings used by clouds on hello VMM server that you want toodelete.</span></span> <span data-ttu-id="000ee-157">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés hello >  **Törlés**.</span><span class="sxs-lookup"><span data-stu-id="000ee-157">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click hello network mapping > **Delete**.</span></span>
3. <span data-ttu-id="000ee-158">Vegye figyelembe a VMM-kiszolgáló hello hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="000ee-158">Note hello ID of hello VMM server.</span></span>
4. <span data-ttu-id="000ee-159">Szüntesse meg a VMM-kiszolgálónak tooremove hello felhőből replikációs házirendek.</span><span class="sxs-lookup"><span data-stu-id="000ee-159">Disassociate replication policies from clouds on hello VMM server you want tooremove.</span></span>  <span data-ttu-id="000ee-160">A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a kapcsolódó hello házirend.</span><span class="sxs-lookup"><span data-stu-id="000ee-160">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="000ee-161">Kattintson a jobb gombbal a hello felhő > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="000ee-161">Right-click hello cloud > **Disassociate**.</span></span>
5. <span data-ttu-id="000ee-162">Törölje a hello VMM-kiszolgáló vagy az aktív csomópontra.</span><span class="sxs-lookup"><span data-stu-id="000ee-162">Delete hello VMM server or active node.</span></span> <span data-ttu-id="000ee-163">A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal hello server >  **Törlés**.</span><span class="sxs-lookup"><span data-stu-id="000ee-163">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click hello server > **Delete**.</span></span>
6. <span data-ttu-id="000ee-164">Töltse le és futtassa a hello [karbantartási parancsprogramot](http://aka.ms/asr-cleanup-script-vmm) hello VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="000ee-164">Download and run hello [cleanup script](http://aka.ms/asr-cleanup-script-vmm) on hello VMM server.</span></span> <span data-ttu-id="000ee-165">Nyissa meg a Powershellt a hello **Futtatás rendszergazdaként** beállítás, toochange hello végrehajtási házirend hello alapértelmezett (LocalMachine) hatókör.</span><span class="sxs-lookup"><span data-stu-id="000ee-165">Open PowerShell with hello **Run as Administrator** option, toochange hello execution policy for hello default (LocalMachine) scope.</span></span> <span data-ttu-id="000ee-166">Hello parancsfájlban adja meg a hello Azonosítóját a VMM-kiszolgálónak tooremove hello.</span><span class="sxs-lookup"><span data-stu-id="000ee-166">In hello script, specify hello ID of hello VMM server you want tooremove.</span></span> <span data-ttu-id="000ee-167">hello parancsfájl regisztrációs és felhőpárosítási információk hello kiszolgálóról távolítja el.</span><span class="sxs-lookup"><span data-stu-id="000ee-167">hello script removes registration and cloud pairing information from hello server.</span></span>
5. <span data-ttu-id="000ee-168">Bármely más kiszolgálókon a VMM-felhő van párosítva a VMM-kiszolgálónak tooremove hello felhők tartalmazó hello karbantartási parancsprogramot fut.</span><span class="sxs-lookup"><span data-stu-id="000ee-168">Run hello cleanup script on any other VMM servers that contain clouds that are paired with clouds on hello VMM server you want tooremove.</span></span>
6. <span data-ttu-id="000ee-169">Futtassa a hello karbantartási parancsprogramot a bármely más passzív VMM-fürtcsomóponton, amely hello szolgáltató telepítve van.</span><span class="sxs-lookup"><span data-stu-id="000ee-169">Run hello  cleanup script on any other passive VMM cluster nodes that have hello Provider installed.</span></span>
7. <span data-ttu-id="000ee-170">Távolítsa el manuálisan a VMM-kiszolgáló hello szolgáltató hello.</span><span class="sxs-lookup"><span data-stu-id="000ee-170">Uninstall hello Provider manually on hello VMM server.</span></span> <span data-ttu-id="000ee-171">Ha rendelkezik fürttel, távolítsa el az összes csomópontot.</span><span class="sxs-lookup"><span data-stu-id="000ee-171">If you have a cluster, remove from all nodes.</span></span>
8. <span data-ttu-id="000ee-172">Ha tooAzure replikálása, akkor eltávolíthatja hello Microsoft Recovery Services Agent ügynököt Hyper-V gazdagépek törölt hello felhőkben.</span><span class="sxs-lookup"><span data-stu-id="000ee-172">If you replicating tooAzure, you can remove hello Microsoft Recovery Services agent from Hyper-V hosts in hello deleted clouds.</span></span>

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a><span data-ttu-id="000ee-173">A Hyper-V gazdagépet a Hyper-V hely regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="000ee-173">Unregister a Hyper-V host in a Hyper-V Site</span></span>

<span data-ttu-id="000ee-174">A VMM által nem felügyelt Hyper-V-gazdagépek egy Hyper-V helyre gyűjti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="000ee-174">Hyper-V hosts that aren't managed by VMM are gathered into a Hyper-V site.</span></span> <span data-ttu-id="000ee-175">Távolítsa el a gazdagép egy Hyper-V hely az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="000ee-175">Remove a host in a Hyper-V site as follows:</span></span>

1. <span data-ttu-id="000ee-176">Tiltsa le a hello gazdagépen található Hyper-V virtuális gépek replikációját.</span><span class="sxs-lookup"><span data-stu-id="000ee-176">Disable replication for Hyper-V VMs located on hello host.</span></span>
2. <span data-ttu-id="000ee-177">Házirendek hello Hyper-V hely társítását.</span><span class="sxs-lookup"><span data-stu-id="000ee-177">Disassociate policies for hello Hyper-V site.</span></span> <span data-ttu-id="000ee-178">A **Site Recovery-infrastruktúra** > **a Hyper-V helyek** >  **replikációs házirendek**, kattintson duplán a kapcsolódó hello házirend.</span><span class="sxs-lookup"><span data-stu-id="000ee-178">In **Site Recovery Infrastructure** > **For Hyper-V Sites** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="000ee-179">Kattintson a jobb gombbal hello hely > **Disassociate**.</span><span class="sxs-lookup"><span data-stu-id="000ee-179">Right-click hello site > **Disassociate**.</span></span>
3. <span data-ttu-id="000ee-180">Hyper-V-gazdagépek törlése.</span><span class="sxs-lookup"><span data-stu-id="000ee-180">Delete Hyper-V hosts.</span></span> <span data-ttu-id="000ee-181">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V-gazdagépek**, kattintson a jobb gombbal hello server >  **Törlés**.</span><span class="sxs-lookup"><span data-stu-id="000ee-181">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Hosts**, right-click hello server > **Delete**.</span></span>
4. <span data-ttu-id="000ee-182">Hello Hyper-V hely törlése után minden gazdagép el lettek távolítva belőle.</span><span class="sxs-lookup"><span data-stu-id="000ee-182">Delete hello Hyper-V site after all hosts have been removed from it.</span></span> <span data-ttu-id="000ee-183">A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V helyek**, kattintson a jobb gombbal hello hely >  **Törlés**.</span><span class="sxs-lookup"><span data-stu-id="000ee-183">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Sites**, right-click hello site > **Delete**.</span></span>
5. <span data-ttu-id="000ee-184">Futtassa a parancsfájlt minden Hyper-V állomáson, amelyről eltávolította a következő hello.</span><span class="sxs-lookup"><span data-stu-id="000ee-184">Run hello following script on each Hyper-V host that you removed.</span></span> <span data-ttu-id="000ee-185">hello parancsfájl hello futtató kiszolgáló beállításain a szükségtelenné vált, és megszünteti azt a hello tárolójából.</span><span class="sxs-lookup"><span data-stu-id="000ee-185">hello script cleans up settings on hello server, and unregisters it from hello vault.</span></span>


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run hello script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove hello old Azure Site Recovery Provider related properties. Do you want toocontinue (Y/N) ?"
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
                "Stopping hello Azure Site Recovery service..."
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

            # First retrive all hello certificates toobe deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete hello certs
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



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a><span data-ttu-id="000ee-186">Tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="000ee-186">Disable protection for a VMware VM or physical server</span></span>

1. <span data-ttu-id="000ee-187">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="000ee-187">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="000ee-188">A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:</span><span class="sxs-lookup"><span data-stu-id="000ee-188">In **Remove Machine**, select one of these options:</span></span>
    - <span data-ttu-id="000ee-189">**Tiltsa le a védelmet (ajánlott) hello gép**.</span><span class="sxs-lookup"><span data-stu-id="000ee-189">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="000ee-190">Használja ezt a beállítást toostop hello gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="000ee-190">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="000ee-191">Webhely-helyreállítási beállításai automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="000ee-191">Site Recovery settings will be cleaned up automatically.</span></span> <span data-ttu-id="000ee-192">A következő körülmények között hello ezt a lehetőséget csak akkor jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="000ee-192">You will only see this option in hello following circumstances:</span></span>
        - <span data-ttu-id="000ee-193">**Hello méretű kötet már átméretezte**– során egy kötet hello virtuális gép kritikus állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="000ee-193">**You've resized hello VM volume**—When you resize a volume hello virtual machine goes into a critical state.</span></span> <span data-ttu-id="000ee-194">Válassza ezt a beállítást toodisables védelmének biztosítását, ugyanakkor az Azure-ban a helyreállítási pontok megőrzése.</span><span class="sxs-lookup"><span data-stu-id="000ee-194">Select this option toodisables protection while retaining recovery points in Azure.</span></span> <span data-ttu-id="000ee-195">Ha engedélyezi a hello gép védelmét, hello átméretezték, ezért hello kötet fognak átvitt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="000ee-195">When you enable protection for hello machine again, hello data for hello resized volume will be transferred tooAzure.</span></span>
        - <span data-ttu-id="000ee-196">**Nemrég futtatását feladatátvevő**– futtatását követően egy feladatátvételi tootest a környezetében, válassza ki a beállítás toostart újra védelme a helyszíni gépeket.</span><span class="sxs-lookup"><span data-stu-id="000ee-196">**You've recently run a failover**—After you've run a failover tootest your environment, select this option toostart protecting on-premises machines again.</span></span> <span data-ttu-id="000ee-197">Letiltja a virtuális gépeken, és akkor szüksége tooenable védelmi őket újra.</span><span class="sxs-lookup"><span data-stu-id="000ee-197">It disables each virtual machine, and then you need tooenable protection for them again.</span></span> <span data-ttu-id="000ee-198">Ez a beállítás letiltása hello gép nincs hatással a hello replika virtuális gép az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="000ee-198">Disabling hello machine with this setting doesn't affect hello replica virtual machine in Azure.</span></span> <span data-ttu-id="000ee-199">Ne távolítsa el a mobilitási szolgáltatás hello hello gépről.</span><span class="sxs-lookup"><span data-stu-id="000ee-199">Don't uninstall hello Mobility service from hello machine.</span></span>
    - <span data-ttu-id="000ee-200">**Hello gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="000ee-200">**Stop managing hello machine**.</span></span> <span data-ttu-id="000ee-201">Ha ezt a beállítást, hello gép csak eltávolítja a hello tárolójából.</span><span class="sxs-lookup"><span data-stu-id="000ee-201">If you select this option, hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="000ee-202">A helyszíni hello gép védelmi beállításai nem érinti.</span><span class="sxs-lookup"><span data-stu-id="000ee-202">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="000ee-203">tooremove beállításait hello gépen, és tooremove hello gép hello Azure-előfizetéssel, kell tooclean hello beállítások hello mobilitási szolgáltatás eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="000ee-203">tooremove settings on hello machine, and tooremove hello machine from hello Azure subscription, you need tooclean hello settings by uninstalling hello Mobility service.</span></span>

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a><span data-ttu-id="000ee-204">Tiltsa le a védelmet a Hyper-V virtuális gépek VMM-felhőben</span><span class="sxs-lookup"><span data-stu-id="000ee-204">Disable protection for a Hyper-V VM in a VMM cloud</span></span>

1. <span data-ttu-id="000ee-205">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="000ee-205">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="000ee-206">A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:</span><span class="sxs-lookup"><span data-stu-id="000ee-206">In **Remove Machine**, select one of these options:</span></span>

    - <span data-ttu-id="000ee-207">**Tiltsa le a védelmet (ajánlott) hello gép**.</span><span class="sxs-lookup"><span data-stu-id="000ee-207">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="000ee-208">Használja ezt a beállítást toostop hello gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="000ee-208">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="000ee-209">Webhely-helyreállítási beállításai automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="000ee-209">Site Recovery settings will be cleaned up automatically.</span></span>
    - <span data-ttu-id="000ee-210">**Hello gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="000ee-210">**Stop managing hello machine**.</span></span> <span data-ttu-id="000ee-211">Ha ezt a beállítást, hello gép csak eltávolítja a hello tárolójából.</span><span class="sxs-lookup"><span data-stu-id="000ee-211">If you select this option, hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="000ee-212">A helyszíni hello gép védelmi beállításai nem érinti.</span><span class="sxs-lookup"><span data-stu-id="000ee-212">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="000ee-213">tooremove beállítások hello gépen, és tooremove hello gép hello Azure-előfizetéssel, kell tooclean hello-beállítások mentése manuálisan, a hello utasításokat az alábbi.</span><span class="sxs-lookup"><span data-stu-id="000ee-213">tooremove settings on hello machine, and tooremove hello machine from hello Azure subscription, you need tooclean hello settings up manually, using hello instructions below.</span></span> <span data-ttu-id="000ee-214">Vegye figyelembe, hogy ha toodelete hello virtuális gép és annak merevlemezei választja, akkor fog törlődni hello célhelyet.</span><span class="sxs-lookup"><span data-stu-id="000ee-214">Note that if you select toodelete hello virtual machine and its hard disks, they'll be removed from hello target location.</span></span>

### <a name="clean-up-protection-settings---replication-tooa-secondary-vmm-site"></a><span data-ttu-id="000ee-215">Védelmi beállítások - replikáció tooa VMM másodlagos hely törlése</span><span class="sxs-lookup"><span data-stu-id="000ee-215">Clean up protection settings - replication tooa secondary VMM site</span></span>

<span data-ttu-id="000ee-216">Ha a kiválasztott **hello gép kezelésének leállítása** és replikálása tooa másodlagos helyet, a parancsfájl futtatása a hello elsődleges kiszolgáló tooclean hello elsődleges virtuális gép hello beállításokat.</span><span class="sxs-lookup"><span data-stu-id="000ee-216">If you selected **Stop managing hello machine** and you replicating tooa secondary site, run this script on hello primary server tooclean up hello settings for hello primary virtual machine.</span></span> <span data-ttu-id="000ee-217">Hello VMM-konzolon kattintson a hello PowerShell gomb tooopen hello VMM PowerShell-konzolban.</span><span class="sxs-lookup"><span data-stu-id="000ee-217">In hello VMM console click hello PowerShell button tooopen hello VMM PowerShell console.</span></span> <span data-ttu-id="000ee-218">Cserélje le a virtuális gép nevének hello SQLVM1.</span><span class="sxs-lookup"><span data-stu-id="000ee-218">Replace SQLVM1 with hello name of your virtual machine.</span></span>

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="000ee-219">Hello másodlagos VMM-kiszolgálón futtassa a parancsfájl tooclean hello másodlagos virtuális gép hello beállításokat:</span><span class="sxs-lookup"><span data-stu-id="000ee-219">On hello secondary VMM server run this script tooclean up hello settings for hello secondary virtual machine:</span></span>

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. <span data-ttu-id="000ee-220">VMM-kiszolgálón hello másodlagos hello virtuális gépek hello Hyper-V gazdakiszolgálón, frissítése, hogy hello másodlagos virtuális gép lekérdezi észlelt újra hello VMM-konzolon.</span><span class="sxs-lookup"><span data-stu-id="000ee-220">On hello secondary VMM server, refresh hello virtual machines on hello Hyper-V host server, so that hello secondary VM gets detected again in hello VMM console.</span></span>
4. <span data-ttu-id="000ee-221">hello a fenti lépéseket hello a replikációs beállításokat a VMM-kiszolgáló hello törlődnek.</span><span class="sxs-lookup"><span data-stu-id="000ee-221">hello above steps clear up hello replication settings on hello VMM server.</span></span> <span data-ttu-id="000ee-222">Ha azt szeretné, hogy hello virtuális gép, futtassa a következő parancsfájl bizony hello toostop replikálása hello az elsődleges és másodlagos virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="000ee-222">If you want toostop replication for hello virtual machine, run hello following script oh hello primary and secondary VMs.</span></span> <span data-ttu-id="000ee-223">Cserélje le a virtuális gép nevének hello SQLVM1.</span><span class="sxs-lookup"><span data-stu-id="000ee-223">Replace SQLVM1 with hello name of your virtual machine.</span></span>

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-tooazure"></a><span data-ttu-id="000ee-224">Védelmi beállításokat - replikáció tooAzure</span><span class="sxs-lookup"><span data-stu-id="000ee-224">Clean up protection settings - replication tooAzure</span></span>

1. <span data-ttu-id="000ee-225">Ha a kiválasztott **hello gép kezelésének leállítása** és tooAzure replikálni, futtassa ezt a parancsfájlt a forráskiszolgálón hello VMM PowerShell használatával hello VMM-konzolról.</span><span class="sxs-lookup"><span data-stu-id="000ee-225">If you selected **Stop managing hello machine** and you replicate tooAzure, run this script on hello source VMM server, using PowerShell from hello VMM console.</span></span>
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="000ee-226">hello a fenti lépéseket hello a replikációs beállításokat a VMM-kiszolgáló hello törölje.</span><span class="sxs-lookup"><span data-stu-id="000ee-226">hello above steps clear hello replication settings on hello VMM server.</span></span> <span data-ttu-id="000ee-227">hello Hyper-V gazdakiszolgálón futó hello virtuális gép replikálása toostop futtassa ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="000ee-227">toostop replication for hello virtual machine running on hello Hyper-V host server, run this script.</span></span> <span data-ttu-id="000ee-228">Cserélje le SQLVM1 hello nevét, valamint a virtuális gép, host01.contoso.com hello nevű hello Hyper-V gazdagép-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="000ee-228">Replace SQLVM1 with hello name of your virtual machine, and host01.contoso.com with hello name of hello Hyper-V host server.</span></span>

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a><span data-ttu-id="000ee-229">Tiltsa le a védelmet a Hyper-V virtuális gépek a Hyper-V hely</span><span class="sxs-lookup"><span data-stu-id="000ee-229">Disable protection for a Hyper-V VM in a Hyper-V Site</span></span>

<span data-ttu-id="000ee-230">Ez az eljárás használható, ha a Hyper-V virtuális gépek tooAzure nélkül egy VMM-kiszolgáló replikál.</span><span class="sxs-lookup"><span data-stu-id="000ee-230">Use this procedure if you're replicating Hyper-V VMs tooAzure without a VMM server.</span></span>

1. <span data-ttu-id="000ee-231">A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.</span><span class="sxs-lookup"><span data-stu-id="000ee-231">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="000ee-232">A **eltávolítása gép**, kiválaszthatja az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="000ee-232">In **Remove Machine**, you can select hello following options:</span></span>

   - <span data-ttu-id="000ee-233">**Tiltsa le a védelmet (ajánlott) hello gép**.</span><span class="sxs-lookup"><span data-stu-id="000ee-233">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="000ee-234">Használja ezt a beállítást toostop hello gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="000ee-234">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="000ee-235">Webhely-helyreállítási beállításai automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="000ee-235">Site Recovery settings will be cleaned up automatically.</span></span>
   - <span data-ttu-id="000ee-236">**Hello gép kezelésének leállítása**.</span><span class="sxs-lookup"><span data-stu-id="000ee-236">**Stop managing hello machine**.</span></span> <span data-ttu-id="000ee-237">Ha ezt a beállítást hello gép csak törlődni fog a hello tárolójából.</span><span class="sxs-lookup"><span data-stu-id="000ee-237">If you select this option hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="000ee-238">A helyszíni hello gép védelmi beállításai nem érinti.</span><span class="sxs-lookup"><span data-stu-id="000ee-238">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="000ee-239">tooremove beállítások hello gépen, és tooremove hello a virtuális gép hello Azure-előfizetéssel, kell tooclean hello-beállítások mentése manuálisan.</span><span class="sxs-lookup"><span data-stu-id="000ee-239">tooremove settings on hello machine, and tooremove hello virtual machine from hello Azure subscription, you need tooclean hello settings up manually.</span></span> <span data-ttu-id="000ee-240">Ha ki kell választania toodelete hello virtuális gép és annak merevlemezei, akkor azok törlődnek a hello célhelyet.</span><span class="sxs-lookup"><span data-stu-id="000ee-240">If you select toodelete hello virtual machine and its hard disks they will be removed from hello target location.</span></span>
3. <span data-ttu-id="000ee-241">Ha a kiválasztott **hello gép kezelésének leállítása**, futtassa a parancsfájl hello forrás Hyper-V gazdakiszolgálón, tooremove hello virtuális gép replikálása.</span><span class="sxs-lookup"><span data-stu-id="000ee-241">If you selected **Stop managing hello machine**, run this script on hello source Hyper-V host server, tooremove replication for hello virtual machine.</span></span> <span data-ttu-id="000ee-242">Cserélje le a virtuális gép nevének hello SQLVM1.</span><span class="sxs-lookup"><span data-stu-id="000ee-242">Replace SQLVM1 with hello name of your virtual machine.</span></span>

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
