---
title: "Virtuális gépek magas rendelkezésre állás a SUSE Linux Enterprise Server SAP NetWeaver az SAP-alkalmazásokból aaaAzure |} Microsoft Docs"
description: "Magas rendelkezésre állású útmutatója az SAP NetWeaver a SUSE Linux Enterprise Server SAP-alkalmazásokból"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: e944103df92d5ffec9196189f138e25972bea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a><span data-ttu-id="d0887-103">Magas rendelkezésre állás a SAP NetWeaver a SUSE Linux Enterprise Server Azure virtuális gépeken az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="d0887-103">High availability for SAP NetWeaver on Azure VMs on SUSE Linux Enterprise Server for SAP applications</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

<span data-ttu-id="d0887-113">Ez a cikk ismerteti, hogyan toodeploy hello virtuális gépek, virtuális gépek hello konfigurálása, hello fürt keretrendszer telepítése és egy magas rendelkezésre állású SAP NetWeaver 7.50 rendszer telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d0887-113">This article describes how toodeploy hello virtual machines, configure hello virtual machines, install hello cluster framework and install a highly available SAP NetWeaver 7.50 system.</span></span>
<span data-ttu-id="d0887-114">Hello például konfigurációk telepítési parancsok stb. Asc példányszámának 00, SSZON példányszámának 02 és SAP rendszer azonosító NWS szolgál.</span><span class="sxs-lookup"><span data-stu-id="d0887-114">In hello example configurations, installation commands etc. ASCS instance number 00, ERS instance number 02 and SAP System ID NWS is used.</span></span> <span data-ttu-id="d0887-115">hello nevek hello erőforrások (például virtuális gépek, virtuális hálózatok) hello példa feltételezi hello használt [sablon összevont] [ template-converged] SAP rendszer azonosító NWS toocreate hello erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="d0887-115">hello names of hello resources (for example virtual machines, virtual networks) in hello example assume that you have used hello [converged template][template-converged] with SAP system ID NWS toocreate hello resources.</span></span>

<span data-ttu-id="d0887-116">Olvassa el a következő SAP megjegyzések és által írt cikkeket először hello</span><span class="sxs-lookup"><span data-stu-id="d0887-116">Read hello following SAP Notes and papers first</span></span>

* <span data-ttu-id="d0887-117">SAP Megjegyzés [1928533], amelynek van:</span><span class="sxs-lookup"><span data-stu-id="d0887-117">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="d0887-118">Hello SAP szoftver központi telepítése által támogatott Azure Virtuálisgép-méretek listáját</span><span class="sxs-lookup"><span data-stu-id="d0887-118">List of Azure VM sizes that are supported for hello deployment of SAP software</span></span>
  * <span data-ttu-id="d0887-119">Az Azure Virtuálisgép-méretek fontos készletkapacitás információival</span><span class="sxs-lookup"><span data-stu-id="d0887-119">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="d0887-120">Támogatott SAP szoftver, és az operációs rendszer és az adatbázis kombinációját</span><span class="sxs-lookup"><span data-stu-id="d0887-120">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="d0887-121">A Windows és a Microsoft Azure Linux szükséges SAP kernel verziója</span><span class="sxs-lookup"><span data-stu-id="d0887-121">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="d0887-122">SAP Megjegyzés [2015553] a SAP-támogatott SAP szoftverek központi telepítése az Azure-ban szükséges előfeltételeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d0887-122">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="d0887-123">SAP Megjegyzés [2205917] javasolt a SUSE Linux Enterprise Server operációs rendszer beállításait az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="d0887-123">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="d0887-124">SAP Megjegyzés [1944799] SUSE Linux Enterprise Server SAP HANA-irányelvek rendelkezik az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="d0887-124">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="d0887-125">SAP Megjegyzés [2178632] tartalmaz részletes információkat az Azure-ban SAP jelentett összes figyelési metrikákat.</span><span class="sxs-lookup"><span data-stu-id="d0887-125">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="d0887-126">SAP Megjegyzés [2191498] hello szükséges SAP állomás ügynök verziója Linux az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d0887-126">SAP Note [2191498] has hello required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="d0887-127">SAP Megjegyzés [2243692] SAP Azure Linux licenceléssel kapcsolatos információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d0887-127">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="d0887-128">SAP Megjegyzés [1984787] SUSE Linux Enterprise Server 12 vonatkozó általános információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d0887-128">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="d0887-129">SAP Megjegyzés [1999351] hello Azure fokozott Figyelőbővítmény az SAP további információkat.</span><span class="sxs-lookup"><span data-stu-id="d0887-129">SAP Note [1999351] has additional troubleshooting information for hello Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="d0887-130">[SAP közösségi WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) rendelkezik az összes szükséges SAP megjegyzések Linux.</span><span class="sxs-lookup"><span data-stu-id="d0887-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="d0887-131">[Azure virtuális gépek tervezési és megvalósítási az SAP Linux rendszeren][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="d0887-131">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="d0887-132">[Az Azure virtuális gépek telepítése az SAP, Linux (Ez a cikk)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="d0887-132">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="d0887-133">[Az SAP Linux Azure virtuális gépek DBMS-telepítés][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="d0887-133">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="d0887-134">[SAP HANA SR teljesítményre optimalizált forgatókönyv][suse-hana-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="d0887-134">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide]</span></span>  
  <span data-ttu-id="d0887-135">hello útmutató tartalmazza az összes szükséges információk tooset helyszíni SAP HANA replikációs fel.</span><span class="sxs-lookup"><span data-stu-id="d0887-135">hello guide contains all required information tooset up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="d0887-136">Ez az útmutató használja kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="d0887-136">Use this guide as a baseline.</span></span>
* <span data-ttu-id="d0887-137">[Magas rendelkezésre álló NFS tár DRBD és támasztja] [ suse-drbd-guide] hello útmutató tartalmazza az összes szükséges információt tooset NFS magas rendelkezésre állású kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d0887-137">[Highly Available NFS Storage with DRBD and Pacemaker][suse-drbd-guide] hello guide contains all required information tooset up a highly available NFS server.</span></span> <span data-ttu-id="d0887-138">Ez az útmutató használja kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="d0887-138">Use this guide as a baseline.</span></span>


## <a name="overview"></a><span data-ttu-id="d0887-139">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d0887-139">Overview</span></span>

<span data-ttu-id="d0887-140">tooachieve magas rendelkezésre állású, SAP NetWeaver az NFS-kiszolgáló szükséges.</span><span class="sxs-lookup"><span data-stu-id="d0887-140">tooachieve high availability, SAP NetWeaver requires an NFS server.</span></span> <span data-ttu-id="d0887-141">hello NFS-kiszolgáló egy külön fürtben lett konfigurálva, és több SAP-rendszerek által használható.</span><span class="sxs-lookup"><span data-stu-id="d0887-141">hello NFS server is configured in a separate cluster and can be used by multiple SAP systems.</span></span>

![SAP NetWeaver magas rendelkezésre állás – Áttekintés](./media/high-availability-guide-suse/img_001.png)

<span data-ttu-id="d0887-143">hello NFS-kiszolgáló, a SAP NetWeaver ASC, SAP NetWeaver SCS, SAP NetWeaver SSZON és hello SAP HANA-adatbázisból virtuális állomásnév és a virtuális IP-címek használata.</span><span class="sxs-lookup"><span data-stu-id="d0887-143">hello NFS server, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS and hello SAP HANA database use virtual hostname and virtual IP addresses.</span></span> <span data-ttu-id="d0887-144">A Azure-ban egy terhelés-kiegyenlítő szükség toouse egy virtuális IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d0887-144">On Azure, a load balancer is required toouse a virtual IP address.</span></span> <span data-ttu-id="d0887-145">hello alábbi lista mutatja azokat hello terheléselosztó hello konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="d0887-145">hello following list shows hello configuration of hello load balancer.</span></span>

### <a name="nfs-server"></a><span data-ttu-id="d0887-146">NFS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d0887-146">NFS Server</span></span>
* <span data-ttu-id="d0887-147">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d0887-147">Frontend configuration</span></span>
  * <span data-ttu-id="d0887-148">IP-cím 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="d0887-148">IP address 10.0.0.4</span></span>
* <span data-ttu-id="d0887-149">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="d0887-149">Backend configuration</span></span>
  * <span data-ttu-id="d0887-150">Az összes virtuális gépet, amely hello NFS fürt részét kell képezniük tooprimary hálózati adapterek csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="d0887-150">Connected tooprimary network interfaces of all virtual machines that should be part of hello NFS cluster</span></span>
* <span data-ttu-id="d0887-151">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="d0887-151">Probe Port</span></span>
  * <span data-ttu-id="d0887-152">Port 61000</span><span class="sxs-lookup"><span data-stu-id="d0887-152">Port 61000</span></span>
* <span data-ttu-id="d0887-153">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="d0887-153">Loadbalancing rules</span></span>
  * <span data-ttu-id="d0887-154">2049 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-154">2049 TCP</span></span> 
  * <span data-ttu-id="d0887-155">2049 UDP</span><span class="sxs-lookup"><span data-stu-id="d0887-155">2049 UDP</span></span>

### <a name="ascs"></a><span data-ttu-id="d0887-156">(A) SCS</span><span class="sxs-lookup"><span data-stu-id="d0887-156">(A)SCS</span></span>
* <span data-ttu-id="d0887-157">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d0887-157">Frontend configuration</span></span>
  * <span data-ttu-id="d0887-158">IP-cím 10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="d0887-158">IP address 10.0.0.10</span></span>
* <span data-ttu-id="d0887-159">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="d0887-159">Backend configuration</span></span>
  * <span data-ttu-id="d0887-160">Az összes virtuális gépet, amely hello (A) SCS/SSZON fürt részét kell képezniük tooprimary csatlakoztatott hálózati illesztők</span><span class="sxs-lookup"><span data-stu-id="d0887-160">Connected tooprimary network interfaces of all virtual machines that should be part of hello (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="d0887-161">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="d0887-161">Probe Port</span></span>
  * <span data-ttu-id="d0887-162">Port 620**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="d0887-162">Port 620**&lt;nr&gt;**</span></span>
* <span data-ttu-id="d0887-163">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="d0887-163">Loadbalancing rules</span></span>
  * <span data-ttu-id="d0887-164">32**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-164">32**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="d0887-165">36**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-165">36**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="d0887-166">39**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-166">39**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="d0887-167">81-es**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-167">81**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="d0887-168">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-168">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="d0887-169">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-169">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="d0887-170">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-170">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="ers"></a><span data-ttu-id="d0887-171">SSZON</span><span class="sxs-lookup"><span data-stu-id="d0887-171">ERS</span></span>
* <span data-ttu-id="d0887-172">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d0887-172">Frontend configuration</span></span>
  * <span data-ttu-id="d0887-173">IP-cím 10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="d0887-173">IP address 10.0.0.11</span></span>
* <span data-ttu-id="d0887-174">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="d0887-174">Backend configuration</span></span>
  * <span data-ttu-id="d0887-175">Az összes virtuális gépet, amely hello (A) SCS/SSZON fürt részét kell képezniük tooprimary csatlakoztatott hálózati illesztők</span><span class="sxs-lookup"><span data-stu-id="d0887-175">Connected tooprimary network interfaces of all virtual machines that should be part of hello (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="d0887-176">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="d0887-176">Probe Port</span></span>
  * <span data-ttu-id="d0887-177">Port 621**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="d0887-177">Port 621**&lt;nr&gt;**</span></span>
* <span data-ttu-id="d0887-178">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="d0887-178">Loadbalancing rules</span></span>
  * <span data-ttu-id="d0887-179">33**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-179">33**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="d0887-180">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-180">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="d0887-181">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-181">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="d0887-182">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-182">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="sap-hana"></a><span data-ttu-id="d0887-183">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="d0887-183">SAP HANA</span></span>
* <span data-ttu-id="d0887-184">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d0887-184">Frontend configuration</span></span>
  * <span data-ttu-id="d0887-185">10.0.0.12 IP-cím</span><span class="sxs-lookup"><span data-stu-id="d0887-185">IP address 10.0.0.12</span></span>
* <span data-ttu-id="d0887-186">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="d0887-186">Backend configuration</span></span>
  * <span data-ttu-id="d0887-187">Az összes virtuális gépet, amely hello HANA fürt részét kell képezniük tooprimary hálózati adapterek csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="d0887-187">Connected tooprimary network interfaces of all virtual machines that should be part of hello HANA cluster</span></span>
* <span data-ttu-id="d0887-188">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="d0887-188">Probe Port</span></span>
  * <span data-ttu-id="d0887-189">Port a 625**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="d0887-189">Port 625**&lt;nr&gt;**</span></span>
* <span data-ttu-id="d0887-190">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="d0887-190">Loadbalancing rules</span></span>
  * <span data-ttu-id="d0887-191">3**&lt;nr&gt;**15 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-191">3**&lt;nr&gt;**15 TCP</span></span>
  * <span data-ttu-id="d0887-192">3**&lt;nr&gt;**17 TCP</span><span class="sxs-lookup"><span data-stu-id="d0887-192">3**&lt;nr&gt;**17 TCP</span></span>

## <a name="setting-up-a-highly-available-nfs-server"></a><span data-ttu-id="d0887-193">Egy magas rendelkezésre állású NFS-kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="d0887-193">Setting up a highly available NFS server</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="d0887-194">Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-194">Deploying Linux</span></span>

<span data-ttu-id="d0887-195">hello Azure piactér SUSE Linux Enterprise Server SAP alkalmazások 12 használható toodeploy új virtuális gépek a kép tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0887-195">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use toodeploy new virtual machines.</span></span>
<span data-ttu-id="d0887-196">Használhat hello gyors üzembe helyezési sablonok a githubon toodeploy minden szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d0887-196">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="d0887-197">hello sablon telepíti hello virtuális gépek, a terheléselosztó hello, a rendelkezésre állási csoport stb. Kövesse a lépéseket toodeploy hello sablon:</span><span class="sxs-lookup"><span data-stu-id="d0887-197">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="d0887-198">Nyissa meg hello [SAP fájl server sablon] [ template-file-server] a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d0887-198">Open hello [SAP file server template][template-file-server] in hello Azure portal</span></span>   
1. <span data-ttu-id="d0887-199">Adja meg a következő paraméterek hello</span><span class="sxs-lookup"><span data-stu-id="d0887-199">Enter hello following parameters</span></span>
   1. <span data-ttu-id="d0887-200">Erőforrás-előtag</span><span class="sxs-lookup"><span data-stu-id="d0887-200">Resource Prefix</span></span>  
      <span data-ttu-id="d0887-201">Adja meg a kívánt toouse hello előtag.</span><span class="sxs-lookup"><span data-stu-id="d0887-201">Enter hello prefix you want toouse.</span></span> <span data-ttu-id="d0887-202">hello érték előtagjaként hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="d0887-202">hello value is used as a prefix for hello resources that are deployed.</span></span>
   2. <span data-ttu-id="d0887-203">Operációs rendszer típusa</span><span class="sxs-lookup"><span data-stu-id="d0887-203">Os Type</span></span>  
      <span data-ttu-id="d0887-204">Válasszon ki egy hello Linux terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="d0887-204">Select one of hello Linux distributions.</span></span> <span data-ttu-id="d0887-205">Ehhez a példához válassza ki a SLES 12 rendszert</span><span class="sxs-lookup"><span data-stu-id="d0887-205">For this example, select SLES 12</span></span>
   3. <span data-ttu-id="d0887-206">Rendszergazda felhasználónevét és a rendszergazdai jelszó</span><span class="sxs-lookup"><span data-stu-id="d0887-206">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="d0887-207">Új felhasználó jön létre, amely lehet használt toolog toohello gépen.</span><span class="sxs-lookup"><span data-stu-id="d0887-207">A new user is created that can be used toolog on toohello machine.</span></span>
   4. <span data-ttu-id="d0887-208">Alhálózati azonosító</span><span class="sxs-lookup"><span data-stu-id="d0887-208">Subnet Id</span></span>  
      <span data-ttu-id="d0887-209">hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakoznia kell.</span><span class="sxs-lookup"><span data-stu-id="d0887-209">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span> <span data-ttu-id="d0887-210">Hagyja üresen, ha szeretné, hogy egy új virtuális hálózat toocreate, vagy jelölje ki a VPN- vagy Express Route virtuális hálózati tooconnect hello virtuális gép tooyour a helyszíni hálózat hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="d0887-210">Leave empty if you want toocreate a new virtual network or select hello subnet of your VPN or Express Route virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="d0887-211">hello azonosítója általában a következőképpen néz következő**&lt;előfizetés-azonosító&gt;**/resourceGroups/**&lt;erőforráscsoport-név&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuálishálózat-név&gt;**/subnets/**&lt;alhálózat neve&gt;**</span><span class="sxs-lookup"><span data-stu-id="d0887-211">hello ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="d0887-212">Telepítés</span><span class="sxs-lookup"><span data-stu-id="d0887-212">Installation</span></span>

<span data-ttu-id="d0887-213">hello következő elemek fűzve előtagként vagy **[A]** -alkalmazható tooall csomópontok **[1]** -csak a megfelelő toonode 1 vagy **[2]** -csak a megfelelő toonode 2.</span><span class="sxs-lookup"><span data-stu-id="d0887-213">hello following items are prefixed with either **[A]** - applicable tooall nodes, **[1]** - only applicable toonode 1 or **[2]** - only applicable toonode 2.</span></span>

1. <span data-ttu-id="d0887-214">**[A]**  SLES frissítése</span><span class="sxs-lookup"><span data-stu-id="d0887-214">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="d0887-215">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-215">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="d0887-216">**[2]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-216">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="d0887-217">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-217">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="d0887-218">**[A]**  Telepítése magas rendelkezésre ÁLLÁSÚ bővítmény</span><span class="sxs-lookup"><span data-stu-id="d0887-218">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="d0887-219">**[A]**  Állomásnév beállítása</span><span class="sxs-lookup"><span data-stu-id="d0887-219">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="d0887-220">DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d0887-220">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="d0887-221">Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="d0887-221">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="d0887-222">Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév</span><span class="sxs-lookup"><span data-stu-id="d0887-222">Replace hello IP address and hello hostname in hello following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="d0887-223">Helyezze be a következő sorokat túl/etc/hosts hello.</span><span class="sxs-lookup"><span data-stu-id="d0887-223">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="d0887-224">Hello IP cím és az állomásnév toomatch a környezet módosítása</span><span class="sxs-lookup"><span data-stu-id="d0887-224">Change hello IP address and hostname toomatch your environment</span></span>   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. <span data-ttu-id="d0887-225">**[1]**  Fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-225">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="d0887-226">**[2]**  Csomópont toocluster hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0887-226">**[2]** Add node toocluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="d0887-227">**[A]**  Módosítása hacluster jelszó toohello ugyanazt a jelszót</span><span class="sxs-lookup"><span data-stu-id="d0887-227">**[A]** Change hacluster password toohello same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="d0887-228">**[A]**  Corosync toouse más átviteli konfigurálása, és adja hozzá a csomópontlista.</span><span class="sxs-lookup"><span data-stu-id="d0887-228">**[A]** Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="d0887-229">Fürt egyébként nem működik.</span><span class="sxs-lookup"><span data-stu-id="d0887-229">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="d0887-230">Adja hozzá a következő félkövér tartalom toohello fájl hello.</span><span class="sxs-lookup"><span data-stu-id="d0887-230">Add hello following bold content toohello file.</span></span>
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-nfs-0</b>
      ring0_addr:10.0.0.5
     }
     node {
      # IP address of <b>prod-nfs-1</b>
      ring0_addr:10.0.0.6
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   <span data-ttu-id="d0887-231">Indítsa újra hello corosync szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d0887-231">Then restart hello corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="d0887-232">**[A]**  Drbd összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-232">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="d0887-233">**[A]**  Hozzon létre egy partíciót hello drbd eszköz</span><span class="sxs-lookup"><span data-stu-id="d0887-233">**[A]** Create a partition for hello drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="d0887-234">**[A]**  Létrehozása LVM konfigurációk</span><span class="sxs-lookup"><span data-stu-id="d0887-234">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. <span data-ttu-id="d0887-235">**[A]**  Létrehozása hello NFS drbd eszköz</span><span class="sxs-lookup"><span data-stu-id="d0887-235">**[A]** Create hello NFS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   <span data-ttu-id="d0887-236">Helyezze be a hello új drbd eszköz és a kilépési hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-236">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_nfs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>prod-nfs-0</b> {
         address   <b>10.0.0.5</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
      on <b>prod-nfs-1</b> {
         address   <b>10.0.0.6</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="d0887-237">Hello drbd eszköz létrehozása, és indítsa el</span><span class="sxs-lookup"><span data-stu-id="d0887-237">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="d0887-238">**[1]**  Kihagyása a kezdeti szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="d0887-238">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="d0887-239">**[1]**  Set hello elsődleges csomópont</span><span class="sxs-lookup"><span data-stu-id="d0887-239">**[1]** Set hello primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="d0887-240">**[1]**  Várjon, amíg a hello új drbd eszköz szinkronizált</span><span class="sxs-lookup"><span data-stu-id="d0887-240">**[1]** Wait until hello new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="d0887-241">**[1]**  Fájlrendszerek hozható létre hello drbd eszközök</span><span class="sxs-lookup"><span data-stu-id="d0887-241">**[1]** Create file systems on hello drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="d0887-242">Fürt keretrendszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-242">Configure Cluster Framework</span></span>

1. <span data-ttu-id="d0887-243">**[1]**  Hello alapértelmezett beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="d0887-243">**[1]** Change hello default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="d0887-244">**[1]**  Hozzáadás hello NFS drbd eszköz toohello fürtkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="d0887-244">**[1]** Add hello NFS drbd device toohello cluster configuration</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_nfs drbd_<b>NWS</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="d0887-245">**[1]**  Létrehozása hello NFS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d0887-245">**[1]** Create hello NFS server</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="d0887-246">**[1]**  Hello NFS fájlrendszert erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-246">**[1]** Create hello NFS File System resources</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive fs_<b>NWS</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NWS</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# group g-<b>NWS</b>_nfs fs_<b>NWS</b>_sapmnt

   crm(live)configure# order o-<b>NWS</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NWS</b>_nfs:promote g-<b>NWS</b>_nfs:start
   
   crm(live)configure# colocation col-<b>NWS</b>_nfs_on_drbd inf: \
     g-<b>NWS</b>_nfs ms-drbd_<b>NWS</b>_nfs:Master

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="d0887-247">**[1]**  Hello NFS kivitel létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-247">**[1]** Create hello NFS exports</span></span>

   <pre><code>
   sudo mkdir /srv/nfs/<b>NWS</b>/sidsys
   sudo mkdir /srv/nfs/<b>NWS</b>/sapmntsid
   sudo mkdir /srv/nfs/<b>NWS</b>/trans

   sudo crm configure

   crm(live)configure# primitive exportfs_<b>NWS</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NWS</b>" \
     options="rw,no_root_squash" \
     clientspec="*" fsid=0 \
     wait_for_leasetime_on_stop=true \
     op monitor interval="30s"

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add exportfs_<b>NWS</b>

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="d0887-248">**[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi hello belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="d0887-248">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive vip_<b>NWS</b>_nfs IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_nfs anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 610<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add nc_<b>NWS</b>_nfs
   crm(live)configure# modgroup g-<b>NWS</b>_nfs add vip_<b>NWS</b>_nfs

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

### <a name="create-stonith-device"></a><span data-ttu-id="d0887-249">STONITH eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-249">Create STONITH device</span></span>

<span data-ttu-id="d0887-250">hello STONITH eszköz által használt egy egyszerű tooauthorize Microsoft Azure ellen.</span><span class="sxs-lookup"><span data-stu-id="d0887-250">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="d0887-251">Kövesse ezeket a lépéseket toocreate egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d0887-251">Follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="d0887-252">Nyissa meg túl<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="d0887-252">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="d0887-253">Nyissa meg hello Azure Active Directory panel</span><span class="sxs-lookup"><span data-stu-id="d0887-253">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="d0887-254">Nyissa meg tooProperties, és írja le hello Directory azonosítóját. Ez a hello **bérlőazonosító**.</span><span class="sxs-lookup"><span data-stu-id="d0887-254">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="d0887-255">Kattintson az alkalmazás-regisztráció</span><span class="sxs-lookup"><span data-stu-id="d0887-255">Click App registrations</span></span>
1. <span data-ttu-id="d0887-256">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="d0887-256">Click Add</span></span>
1. <span data-ttu-id="d0887-257">Adjon meg egy nevet, válassza ki a "Web app/API" alkalmazástípus, adja meg a bejelentkezési URL-címet (például http://localhost) és kattintson a Létrehozás gombra</span><span class="sxs-lookup"><span data-stu-id="d0887-257">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="d0887-258">hello bejelentkezési URL-címet nem használja, és bármilyen érvényes URL-CÍMEK lehetnek</span><span class="sxs-lookup"><span data-stu-id="d0887-258">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="d0887-259">Válassza ki az új alkalmazás hello és kattintson a kulcsok hello-beállítások lap</span><span class="sxs-lookup"><span data-stu-id="d0887-259">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="d0887-260">Adja meg egy új kulcs leírását, válassza a "Soha nem jár le", és kattintson a Mentés gombra</span><span class="sxs-lookup"><span data-stu-id="d0887-260">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="d0887-261">Írja le hello érték.</span><span class="sxs-lookup"><span data-stu-id="d0887-261">Write down hello Value.</span></span> <span data-ttu-id="d0887-262">Hello használják **jelszó** a hello szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="d0887-262">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="d0887-263">Írja le hello azonosítót. Hello felhasználónév, a rendszer (**bejelentkezési azonosító** hello lépéseket a) a hello szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="d0887-263">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="d0887-264">hello szolgáltatás egyszerű engedélyek tooaccess az Azure-erőforrások alapértelmezés szerint nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d0887-264">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="d0887-265">Toogive hello szolgáltatás egyszerű engedélyek toostart van szüksége, és állítsa (felszabadítása) hello fürt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d0887-265">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="d0887-266">Nyissa meg toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="d0887-266">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="d0887-267">Nyissa meg az összes erőforrás panel hello</span><span class="sxs-lookup"><span data-stu-id="d0887-267">Open hello All resources blade</span></span>
1. <span data-ttu-id="d0887-268">Válassza ki a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="d0887-268">Select hello virtual machine</span></span>
1. <span data-ttu-id="d0887-269">Kattintson a hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="d0887-269">Click Access control (IAM)</span></span>
1. <span data-ttu-id="d0887-270">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="d0887-270">Click Add</span></span>
1. <span data-ttu-id="d0887-271">Válassza ki a hello szerepkör tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="d0887-271">Select hello role Owner</span></span>
1. <span data-ttu-id="d0887-272">Adja meg az előbb létrehozott hello alkalmazás hello neve</span><span class="sxs-lookup"><span data-stu-id="d0887-272">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="d0887-273">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="d0887-273">Click OK</span></span>

#### <a name="1-create-hello-stonith-devices"></a><span data-ttu-id="d0887-274">**[1]**  Hello STONITH eszközök létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-274">**[1]** Create hello STONITH devices</span></span>

<span data-ttu-id="d0887-275">Után szerkeszteni hello engedélyek hello virtuális gépekhez, beállíthatja a hello STONITH eszközök hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="d0887-275">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a><span data-ttu-id="d0887-276">**[1]**  STONITH eszköz hello használatának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-276">**[1]** Enable hello use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a><span data-ttu-id="d0887-277">(A) SCS beállítása</span><span class="sxs-lookup"><span data-stu-id="d0887-277">Setting up (A)SCS</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="d0887-278">Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-278">Deploying Linux</span></span>

<span data-ttu-id="d0887-279">hello Azure piactér SUSE Linux Enterprise Server SAP alkalmazások 12 használható toodeploy új virtuális gépek a kép tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0887-279">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use toodeploy new virtual machines.</span></span> <span data-ttu-id="d0887-280">hello Piactéri lemezképhez hello erőforrás ügynök az SAP NetWeaver tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0887-280">hello marketplace image contains hello resource agent for SAP NetWeaver.</span></span>

<span data-ttu-id="d0887-281">Használhat hello gyors üzembe helyezési sablonok a githubon toodeploy minden szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d0887-281">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="d0887-282">hello sablon telepíti hello virtuális gépek, a terheléselosztó hello, a rendelkezésre állási csoport stb. Kövesse a lépéseket toodeploy hello sablon:</span><span class="sxs-lookup"><span data-stu-id="d0887-282">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="d0887-283">Nyissa meg hello [ASC/SCS Multi SID sablon] [ template-multisid-xscs] vagy hello [sablon összevont] [ template-converged] hello Azure portál hello ASC/SCS sablon csak hello terheléselosztási szabályok hello SAP NetWeaver ASC/SCS és SSZON (csak Linux) példányok hoz létre, mivel hello átszervezett sablon is (például a Microsoft SQL Server vagy az SAP HANA) adatbázis hello terheléselosztási szabályokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d0887-283">Open hello [ASCS/SCS Multi SID template][template-multisid-xscs] or hello [converged template][template-converged] on hello Azure portal hello ASCS/SCS template only creates hello load-balancing rules for hello SAP NetWeaver ASCS/SCS and ERS (Linux only) instances whereas hello converged template also creates hello load-balancing rules for a database (for example Microsoft SQL Server or SAP HANA).</span></span> <span data-ttu-id="d0887-284">Ha azt tervezi, hogy az SAP NetWeaver-alapú rendszerek tooinstall és tooinstall hello adatbázis is érdemes a hello azonos gépek, használja a hello [sablon összevont][template-converged].</span><span class="sxs-lookup"><span data-stu-id="d0887-284">If you plan tooinstall an SAP NetWeaver based system and you also want tooinstall hello database on hello same machines, use hello [converged template][template-converged].</span></span>
1. <span data-ttu-id="d0887-285">Adja meg a következő paraméterek hello</span><span class="sxs-lookup"><span data-stu-id="d0887-285">Enter hello following parameters</span></span>
   1. <span data-ttu-id="d0887-286">Erőforrás-előtag (csak a sablon ASC/SCS Multi SID)</span><span class="sxs-lookup"><span data-stu-id="d0887-286">Resource Prefix (ASCS/SCS Multi SID template only)</span></span>  
      <span data-ttu-id="d0887-287">Adja meg a kívánt toouse hello előtag.</span><span class="sxs-lookup"><span data-stu-id="d0887-287">Enter hello prefix you want toouse.</span></span> <span data-ttu-id="d0887-288">hello érték előtagjaként hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="d0887-288">hello value is used as a prefix for hello resources that are deployed.</span></span>
   3. <span data-ttu-id="d0887-289">SAP rendszerazonosító (csak az átszervezett sablon)</span><span class="sxs-lookup"><span data-stu-id="d0887-289">Sap System Id (converged template only)</span></span>  
      <span data-ttu-id="d0887-290">Adja meg a hello SAP rendszer azonosító hello tooinstall kívánt SAP rendszer.</span><span class="sxs-lookup"><span data-stu-id="d0887-290">Enter hello SAP system Id of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="d0887-291">hello azonosító előtagjaként hello telepített erőforrások esetén használható.</span><span class="sxs-lookup"><span data-stu-id="d0887-291">hello Id is used as a prefix for hello resources that are deployed.</span></span>
   4. <span data-ttu-id="d0887-292">A készlet típusa</span><span class="sxs-lookup"><span data-stu-id="d0887-292">Stack Type</span></span>  
      <span data-ttu-id="d0887-293">Hello SAP NetWeaver verem típusának kiválasztása</span><span class="sxs-lookup"><span data-stu-id="d0887-293">Select hello SAP NetWeaver stack type</span></span>
   5. <span data-ttu-id="d0887-294">Operációs rendszer típusa</span><span class="sxs-lookup"><span data-stu-id="d0887-294">Os Type</span></span>  
      <span data-ttu-id="d0887-295">Válasszon ki egy hello Linux terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="d0887-295">Select one of hello Linux distributions.</span></span> <span data-ttu-id="d0887-296">Ehhez a példához válassza ki a SLES 12 saját</span><span class="sxs-lookup"><span data-stu-id="d0887-296">For this example, select SLES 12 BYOS</span></span>
   6. <span data-ttu-id="d0887-297">DB típusa</span><span class="sxs-lookup"><span data-stu-id="d0887-297">Db Type</span></span>  
      <span data-ttu-id="d0887-298">Válassza ki a HANA</span><span class="sxs-lookup"><span data-stu-id="d0887-298">Select HANA</span></span>
   7. <span data-ttu-id="d0887-299">SAP mérete</span><span class="sxs-lookup"><span data-stu-id="d0887-299">Sap System Size</span></span>  
      <span data-ttu-id="d0887-300">SAP hello új rendszer hello mennyisége biztosít.</span><span class="sxs-lookup"><span data-stu-id="d0887-300">hello amount of SAPS hello new system provides.</span></span> <span data-ttu-id="d0887-301">Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló</span><span class="sxs-lookup"><span data-stu-id="d0887-301">If you are not sure how many SAPS hello system requires, please ask your SAP Technology Partner or System Integrator</span></span>
   8. <span data-ttu-id="d0887-302">Rendszer rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="d0887-302">System Availability</span></span>  
      <span data-ttu-id="d0887-303">Válassza ki a magas rendelkezésre ÁLLÁSÚ</span><span class="sxs-lookup"><span data-stu-id="d0887-303">Select HA</span></span>
   9. <span data-ttu-id="d0887-304">Rendszergazda felhasználónevét és a rendszergazdai jelszó</span><span class="sxs-lookup"><span data-stu-id="d0887-304">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="d0887-305">Új felhasználó jön létre, amely lehet használt toolog toohello gépen.</span><span class="sxs-lookup"><span data-stu-id="d0887-305">A new user is created that can be used toolog on toohello machine.</span></span>
   10. <span data-ttu-id="d0887-306">Alhálózati azonosító</span><span class="sxs-lookup"><span data-stu-id="d0887-306">Subnet Id</span></span>  
   <span data-ttu-id="d0887-307">hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakoznia kell.</span><span class="sxs-lookup"><span data-stu-id="d0887-307">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span>  <span data-ttu-id="d0887-308">Hagyja üresen, ha azt szeretné, hogy egy új virtuális hálózat toocreate, vagy válasszon hello ugyanazon az alhálózaton, amely használja, vagy hozza létre hello NFS-kiszolgáló központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="d0887-308">Leave empty if you want toocreate a new virtual network or select hello same subnet that you used or created as part of hello NFS server deployment.</span></span> <span data-ttu-id="d0887-309">hello azonosítója általában a következőképpen néz következő**&lt;előfizetés-azonosító&gt;**/resourceGroups/**&lt;erőforráscsoport-név&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuálishálózat-név&gt;**/subnets/**&lt;alhálózat neve&gt;**</span><span class="sxs-lookup"><span data-stu-id="d0887-309">hello ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="d0887-310">Telepítés</span><span class="sxs-lookup"><span data-stu-id="d0887-310">Installation</span></span>

<span data-ttu-id="d0887-311">hello következő elemek fűzve előtagként vagy **[A]** -alkalmazható tooall csomópontok **[1]** -csak a megfelelő toonode 1 vagy **[2]** -csak a megfelelő toonode 2.</span><span class="sxs-lookup"><span data-stu-id="d0887-311">hello following items are prefixed with either **[A]** - applicable tooall nodes, **[1]** - only applicable toonode 1 or **[2]** - only applicable toonode 2.</span></span>

1. <span data-ttu-id="d0887-312">**[A]**  SLES frissítése</span><span class="sxs-lookup"><span data-stu-id="d0887-312">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="d0887-313">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-313">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="d0887-314">**[2]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-314">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="d0887-315">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-315">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="d0887-316">**[A]**  Telepítése magas rendelkezésre ÁLLÁSÚ bővítmény</span><span class="sxs-lookup"><span data-stu-id="d0887-316">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="d0887-317">**[A]**  Frissítés SAP erőforrás ügynökök</span><span class="sxs-lookup"><span data-stu-id="d0887-317">**[A]** Update SAP resource agents</span></span>  
   
   <span data-ttu-id="d0887-318">Egy javítást a hello erőforrás-ügynökök csomag szükséges toouse hello új konfigurációt, ebben a cikkben ismertetett.</span><span class="sxs-lookup"><span data-stu-id="d0887-318">A patch for hello resource-agents package is required toouse hello new configuration, that is described in this article.</span></span> <span data-ttu-id="d0887-319">Ellenőrizheti, ha hello javítás már telepítve van a következő parancs hello</span><span class="sxs-lookup"><span data-stu-id="d0887-319">You can check, if hello patch is already installed with hello following command</span></span>

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   <span data-ttu-id="d0887-320">hello kimeneti hasonlónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d0887-320">hello output should be similar to</span></span>

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   <span data-ttu-id="d0887-321">Hello grep parancs nem található hello IS_ERS paraméter, ha szüksége van-e a felsorolt tooinstall hello javítás [hello SUSE letöltési oldala](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span><span class="sxs-lookup"><span data-stu-id="d0887-321">If hello grep command does not find hello IS_ERS parameter, you need tooinstall hello patch listed on [hello SUSE download page](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span></span>

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. <span data-ttu-id="d0887-322">**[A]**  Állomásnév beállítása</span><span class="sxs-lookup"><span data-stu-id="d0887-322">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="d0887-323">DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d0887-323">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="d0887-324">Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="d0887-324">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="d0887-325">Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév</span><span class="sxs-lookup"><span data-stu-id="d0887-325">Replace hello IP address and hello hostname in hello following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="d0887-326">Helyezze be a következő sorokat túl/etc/hosts hello.</span><span class="sxs-lookup"><span data-stu-id="d0887-326">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="d0887-327">Hello IP cím és az állomásnév toomatch a környezet módosítása</span><span class="sxs-lookup"><span data-stu-id="d0887-327">Change hello IP address and hostname toomatch your environment</span></span>   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. <span data-ttu-id="d0887-328">**[1]**  Fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-328">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="d0887-329">**[2]**  Csomópont toocluster hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0887-329">**[2]** Add node toocluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="d0887-330">**[A]**  Módosítása hacluster jelszó toohello ugyanazt a jelszót</span><span class="sxs-lookup"><span data-stu-id="d0887-330">**[A]** Change hacluster password toohello same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="d0887-331">**[A]**  Corosync toouse más átviteli konfigurálása, és adja hozzá a csomópontlista.</span><span class="sxs-lookup"><span data-stu-id="d0887-331">**[A]** Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="d0887-332">Fürt egyébként nem működik.</span><span class="sxs-lookup"><span data-stu-id="d0887-332">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="d0887-333">Adja hozzá a következő félkövér tartalom toohello fájl hello.</span><span class="sxs-lookup"><span data-stu-id="d0887-333">Add hello following bold content toohello file.</span></span>
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>nws-cl-0</b>
      ring0_addr:     10.0.0.14
     }
     node {
      # IP address of <b>nws-cl-1</b>
      ring0_addr:     10.0.0.13
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   <span data-ttu-id="d0887-334">Indítsa újra hello corosync szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d0887-334">Then restart hello corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="d0887-335">**[A]**  Drbd összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-335">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="d0887-336">**[A]**  Hozzon létre egy partíciót hello drbd eszköz</span><span class="sxs-lookup"><span data-stu-id="d0887-336">**[A]** Create a partition for hello drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="d0887-337">**[A]**  Létrehozása LVM konfigurációk</span><span class="sxs-lookup"><span data-stu-id="d0887-337">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. <span data-ttu-id="d0887-338">**[A]**  Létrehozása hello SCS drbd eszköz</span><span class="sxs-lookup"><span data-stu-id="d0887-338">**[A]** Create hello SCS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   <span data-ttu-id="d0887-339">Helyezze be a hello új drbd eszköz és a kilépési hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-339">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_ascs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="d0887-340">Hello drbd eszköz létrehozása, és indítsa el</span><span class="sxs-lookup"><span data-stu-id="d0887-340">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. <span data-ttu-id="d0887-341">**[A]**  Létrehozása hello SSZON drbd eszköz</span><span class="sxs-lookup"><span data-stu-id="d0887-341">**[A]** Create hello ERS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   <span data-ttu-id="d0887-342">Helyezze be a hello új drbd eszköz és a kilépési hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-342">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_ers {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="d0887-343">Hello drbd eszköz létrehozása, és indítsa el</span><span class="sxs-lookup"><span data-stu-id="d0887-343">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="d0887-344">**[1]**  Kihagyása a kezdeti szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="d0887-344">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="d0887-345">**[1]**  Set hello elsődleges csomópont</span><span class="sxs-lookup"><span data-stu-id="d0887-345">**[1]** Set hello primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="d0887-346">**[1]**  Várjon, amíg a hello új drbd eszköz szinkronizált</span><span class="sxs-lookup"><span data-stu-id="d0887-346">**[1]** Wait until hello new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:93991268 nr:0 dw:93991268 dr:93944920 al:383 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 1: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:6047920 nr:0 dw:6047920 dr:6039112 al:34 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 2: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:5142732 nr:0 dw:5142732 dr:5133924 al:30 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="d0887-347">**[1]**  Fájlrendszerek hozható létre hello drbd eszközök</span><span class="sxs-lookup"><span data-stu-id="d0887-347">**[1]** Create file systems on hello drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="d0887-348">Fürt keretrendszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-348">Configure Cluster Framework</span></span>

<span data-ttu-id="d0887-349">**[1]**  Hello alapértelmezett beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="d0887-349">**[1]** Change hello default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a><span data-ttu-id="d0887-350">SAP NetWeaver telepítésének előkészítése</span><span class="sxs-lookup"><span data-stu-id="d0887-350">Prepare for SAP NetWeaver installation</span></span>

1. <span data-ttu-id="d0887-351">**[A]**  Létrehozása hello megosztott könyvtárak</span><span class="sxs-lookup"><span data-stu-id="d0887-351">**[A]** Create hello shared directories</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. <span data-ttu-id="d0887-352">**[A]**  Autofs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-352">**[A]** Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="d0887-353">Fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-353">Create a file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   <span data-ttu-id="d0887-354">Indítsa újra a autofs toomount hello új megosztások</span><span class="sxs-lookup"><span data-stu-id="d0887-354">Restart autofs toomount hello new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="d0887-355">**[A]**  Fájl FELCSERÉLÉSE konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-355">**[A]** Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="d0887-356">Indítsa újra a hello ügynök tooactivate hello módosítása</span><span class="sxs-lookup"><span data-stu-id="d0887-356">Restart hello Agent tooactivate hello change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a><span data-ttu-id="d0887-357">SAP NetWeaver ASC/SSZON telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-357">Installing SAP NetWeaver ASCS/ERS</span></span>

1. <span data-ttu-id="d0887-358">**[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi hello belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="d0887-358">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm node standby <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ASCS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ascs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ASCS drbd_<b>NWS</b>_ASCS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ASCS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/usr/sap/<b>NWS</b>/ASCS<b>00</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.10</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   crm(live)configure# group g-<b>NWS</b>_ASCS nc_<b>NWS</b>_ASCS vip_<b>NWS</b>_ASCS fs_<b>NWS</b>_ASCS \
      meta resource-stickiness=3000

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ASCS inf: \
     ms-drbd_<b>NWS</b>_ASCS:promote g-<b>NWS</b>_ASCS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ASCS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ASCS:Master g-<b>NWS</b>_ASCS
   
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   <span data-ttu-id="d0887-359">Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="d0887-359">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="d0887-360">Nem fontos a csomópont hello erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="d0887-360">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Node nws-cl-1: standby
   # <b>Online: [ nws-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      Stopped: [ nws-cl-1 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   </code></pre>

1. <span data-ttu-id="d0887-361">**[1]**  SAP NetWeaver ASC telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-361">**[1]** Install SAP NetWeaver ASCS</span></span>  

   <span data-ttu-id="d0887-362">SAP NetWeaver ASC telepítse a legfelső szintű használatával egy virtuális állomásnevet, amely hello terheléselosztó előtér-konfiguráció hello ASC toohello IP-címe például hello első csomóponton <b>nws-ASC</b>, <b>10.0.0.10</b>és például használt hello mintavétel hello terheléselosztó hello példányszámának <b>00</b>.</span><span class="sxs-lookup"><span data-stu-id="d0887-362">Install SAP NetWeaver ASCS as root on hello first node using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello ASCS for example <b>nws-ascs</b>, <b>10.0.0.10</b> and hello instance number that you used for hello probe of hello load balancer for example <b>00</b>.</span></span>

   <span data-ttu-id="d0887-363">Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d0887-363">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="d0887-364">**[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi hello belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="d0887-364">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm node standby <b>nws-cl-0</b>
   sudo crm node online <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ERS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ers" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ERS drbd_<b>NWS</b>_ERS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ERS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/usr/sap/<b>NWS</b>/ERS<b>02</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.11</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0

   crm(live)configure# group g-<b>NWS</b>_ERS nc_<b>NWS</b>_ERS vip_<b>NWS</b>_ERS fs_<b>NWS</b>_ERS

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ERS inf: \
     ms-drbd_<b>NWS</b>_ERS:promote g-<b>NWS</b>_ERS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ERS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ERS:Master g-<b>NWS</b>_ERS
   
   crm(live)configure# commit
   # WARNING: Resources nc_NWS_ASCS,nc_NWS_ERS,nc_NWS_nfs violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want toocommit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   <span data-ttu-id="d0887-365">Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="d0887-365">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="d0887-366">Nem fontos a csomópont hello erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="d0887-366">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Node <b>nws-cl-0: standby</b>
   # <b>Online: [ nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   </code></pre>

1. <span data-ttu-id="d0887-367">**[2]**  SAP NetWeaver SSZON telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-367">**[2]** Install SAP NetWeaver ERS</span></span>  

   <span data-ttu-id="d0887-368">SAP NetWeaver SSZON telepítése hello a második csomópont használatával egy virtuális állomásnevet, amely hello terheléselosztó előtér-konfiguráció hello SSZON toohello IP-címe például a legfelső szintű <b>nws-sszon</b>, <b>10.0.0.11</b> és például használt hello mintavétel hello terheléselosztó hello példányszámának <b>02</b>.</span><span class="sxs-lookup"><span data-stu-id="d0887-368">Install SAP NetWeaver ERS as root on hello second node using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello ERS for example <b>nws-ers</b>, <b>10.0.0.11</b> and hello instance number that you used for hello probe of hello load balancer for example <b>02</b>.</span></span>

   <span data-ttu-id="d0887-369">Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d0887-369">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > <span data-ttu-id="d0887-370">SWPM SP 20 PL 05 vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="d0887-370">Please use SWPM SP 20 PL 05 or higher.</span></span> <span data-ttu-id="d0887-371">Korábbi verziók nem hello engedélyek helyesen beállítva, és hello telepítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="d0887-371">Lower versions do not set hello permissions correctly and hello installation will fail.</span></span>
   > 

1. <span data-ttu-id="d0887-372">**[1]**  Hello ASC/SCS és SSZON példány profilok igazítja</span><span class="sxs-lookup"><span data-stu-id="d0887-372">**[1]** Adapt hello ASCS/SCS and ERS instance profiles</span></span>
 
   * <span data-ttu-id="d0887-373">Asc/SCS profil</span><span class="sxs-lookup"><span data-stu-id="d0887-373">ASCS/SCS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change hello restart command tooa start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add hello keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * <span data-ttu-id="d0887-374">SSZON profil</span><span class="sxs-lookup"><span data-stu-id="d0887-374">ERS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. <span data-ttu-id="d0887-375">**[A]**  Keep-Alive konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-375">**[A]** Configure Keep Alive</span></span>

   <span data-ttu-id="d0887-376">SAP NetWeaver alkalmazáskiszolgáló hello és az ASC/SCS hello hello kommunikációját szoftveres terheléselosztóként üzemeljen keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="d0887-376">hello communication between hello SAP NetWeaver application server and hello ASCS/SCS is routed through a software load balancer.</span></span> <span data-ttu-id="d0887-377">hello terheléselosztó inaktív kapcsolatok leválasztása után konfigurálható időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="d0887-377">hello load balancer disconnects inactive connections after a configurable timeout.</span></span> <span data-ttu-id="d0887-378">tooprevent ez tooset hello SAP NetWeaver ASC/SCS profil egyik paraméterének kell és hello Linux rendszer beállításainak módosítására.</span><span class="sxs-lookup"><span data-stu-id="d0887-378">tooprevent this you need tooset a parameter in hello SAP NetWeaver ASCS/SCS profile and change hello Linux system settings.</span></span> <span data-ttu-id="d0887-379">Kérjük, olvassa el [SAP Megjegyzés 1410736] [ 1410736] további információt.</span><span class="sxs-lookup"><span data-stu-id="d0887-379">Please read [SAP Note 1410736][1410736] for more information.</span></span>
   
   <span data-ttu-id="d0887-380">hello ASC/SCS profil paraméter célzó/encni/set_so_keepalive már felvették hello utolsó lépésében megadja.</span><span class="sxs-lookup"><span data-stu-id="d0887-380">hello ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in hello last step.</span></span>

   <pre><code> 
   # Change hello Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. <span data-ttu-id="d0887-381">**[A]**  Hello SAP felhasználók konfigurálása hello telepítése után</span><span class="sxs-lookup"><span data-stu-id="d0887-381">**[A]** Configure hello SAP users after hello installation</span></span>
 
   <pre><code>
   # Add sidadm toohello haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. <span data-ttu-id="d0887-382">**[1]**  Hello ASC és SSZON SAP szolgáltatások toohello sapservice fájl</span><span class="sxs-lookup"><span data-stu-id="d0887-382">**[1]** Add hello ASCS and ERS SAP services toohello sapservice file</span></span>

   <span data-ttu-id="d0887-383">Adja hozzá a hello ASC szolgáltatás bejegyzés toohello második csomópontnak, illetve másolási hello SSZON szolgáltatás bejegyzés toohello első csomópontnak.</span><span class="sxs-lookup"><span data-stu-id="d0887-383">Add hello ASCS service entry toohello second node and copy hello ERS service entry toohello first node.</span></span>

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. <span data-ttu-id="d0887-384">**[1]**  Hello SAP-fürterőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-384">**[1]** Create hello SAP cluster resources</span></span>

   <pre><code>
   sudo crm configure property maintenance-mode="true"

   sudo crm configure

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ASCS<b>00</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ERS<b>02</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000

   crm(live)configure# modgroup g-<b>NWS</b>_ASCS add rsc_sap_<b>NWS</b>_ASCS<b>00</b>
   crm(live)configure# modgroup g-<b>NWS</b>_ERS add rsc_sap_<b>NWS</b>_ERS<b>02</b>

   crm(live)configure# colocation col_sap_<b>NWS</b>_no_both -5000: g-<b>NWS</b>_ERS g-<b>NWS</b>_ASCS
   crm(live)configure# location loc_sap_<b>NWS</b>_failover_to_ers rsc_sap_<b>NWS</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NWS</b> eq 1
   crm(live)configure# order ord_sap_<b>NWS</b>_first_start_ascs Optional: rsc_sap_<b>NWS</b>_ASCS<b>00</b>:start rsc_sap_<b>NWS</b>_ERS<b>02</b>:stop symmetrical=false

   crm(live)configure# commit
   crm(live)configure# exit

   sudo crm configure property maintenance-mode="false"
   sudo crm node online <b>nws-cl-0</b>
   </code></pre>

   <span data-ttu-id="d0887-385">Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="d0887-385">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="d0887-386">Nem fontos a csomópont hello erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="d0887-386">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Online: <b>[ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   </code></pre>

### <a name="create-stonith-device"></a><span data-ttu-id="d0887-387">STONITH eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-387">Create STONITH device</span></span>

<span data-ttu-id="d0887-388">hello STONITH eszköz által használt egy egyszerű tooauthorize Microsoft Azure ellen.</span><span class="sxs-lookup"><span data-stu-id="d0887-388">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="d0887-389">Kövesse ezeket a lépéseket toocreate egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d0887-389">Follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="d0887-390">Nyissa meg túl<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="d0887-390">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="d0887-391">Nyissa meg hello Azure Active Directory panel</span><span class="sxs-lookup"><span data-stu-id="d0887-391">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="d0887-392">Nyissa meg tooProperties, és írja le hello Directory azonosítóját. Ez a hello **bérlőazonosító**.</span><span class="sxs-lookup"><span data-stu-id="d0887-392">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="d0887-393">Kattintson az alkalmazás-regisztráció</span><span class="sxs-lookup"><span data-stu-id="d0887-393">Click App registrations</span></span>
1. <span data-ttu-id="d0887-394">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="d0887-394">Click Add</span></span>
1. <span data-ttu-id="d0887-395">Adjon meg egy nevet, válassza ki a "Web app/API" alkalmazástípus, adja meg a bejelentkezési URL-címet (például http://localhost) és kattintson a Létrehozás gombra</span><span class="sxs-lookup"><span data-stu-id="d0887-395">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="d0887-396">hello bejelentkezési URL-címet nem használja, és bármilyen érvényes URL-CÍMEK lehetnek</span><span class="sxs-lookup"><span data-stu-id="d0887-396">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="d0887-397">Válassza ki az új alkalmazás hello és kattintson a kulcsok hello-beállítások lap</span><span class="sxs-lookup"><span data-stu-id="d0887-397">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="d0887-398">Adja meg egy új kulcs leírását, válassza a "Soha nem jár le", és kattintson a Mentés gombra</span><span class="sxs-lookup"><span data-stu-id="d0887-398">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="d0887-399">Írja le hello érték.</span><span class="sxs-lookup"><span data-stu-id="d0887-399">Write down hello Value.</span></span> <span data-ttu-id="d0887-400">Hello használják **jelszó** a hello szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="d0887-400">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="d0887-401">Írja le hello azonosítót. Hello felhasználónév, a rendszer (**bejelentkezési azonosító** hello lépéseket a) a hello szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="d0887-401">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="d0887-402">hello szolgáltatás egyszerű engedélyek tooaccess az Azure-erőforrások alapértelmezés szerint nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d0887-402">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="d0887-403">Toogive hello szolgáltatás egyszerű engedélyek toostart van szüksége, és állítsa (felszabadítása) hello fürt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d0887-403">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="d0887-404">Nyissa meg toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="d0887-404">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="d0887-405">Nyissa meg az összes erőforrás panel hello</span><span class="sxs-lookup"><span data-stu-id="d0887-405">Open hello All resources blade</span></span>
1. <span data-ttu-id="d0887-406">Válassza ki a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="d0887-406">Select hello virtual machine</span></span>
1. <span data-ttu-id="d0887-407">Kattintson a hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="d0887-407">Click Access control (IAM)</span></span>
1. <span data-ttu-id="d0887-408">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="d0887-408">Click Add</span></span>
1. <span data-ttu-id="d0887-409">Válassza ki a hello szerepkör tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="d0887-409">Select hello role Owner</span></span>
1. <span data-ttu-id="d0887-410">Adja meg az előbb létrehozott hello alkalmazás hello neve</span><span class="sxs-lookup"><span data-stu-id="d0887-410">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="d0887-411">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="d0887-411">Click OK</span></span>

#### <a name="1-create-hello-stonith-devices"></a><span data-ttu-id="d0887-412">**[1]**  Hello STONITH eszközök létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-412">**[1]** Create hello STONITH devices</span></span>

<span data-ttu-id="d0887-413">Után szerkeszteni hello engedélyek hello virtuális gépekhez, beállíthatja a hello STONITH eszközök hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="d0887-413">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a><span data-ttu-id="d0887-414">**[1]**  STONITH eszköz hello használatának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-414">**[1]** Enable hello use of a STONITH device</span></span>

<span data-ttu-id="d0887-415">Hello STONITH eszköz használatának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d0887-415">Enable hello use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a><span data-ttu-id="d0887-416">A telepítési adatbázis</span><span class="sxs-lookup"><span data-stu-id="d0887-416">Install database</span></span>

<span data-ttu-id="d0887-417">Ebben a példában egy SAP HANA replikációs telepítve és konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="d0887-417">In this example an SAP HANA System Replication is installed and configured.</span></span> <span data-ttu-id="d0887-418">SAP HANA hello azonos hello SAP NetWeaver ASC/SCS és SSZON fürtöt fog futni.</span><span class="sxs-lookup"><span data-stu-id="d0887-418">SAP HANA will run in hello same cluster as hello SAP NetWeaver ASCS/SCS and ERS.</span></span> <span data-ttu-id="d0887-419">SAP HANA egy dedikált fürtön is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="d0887-419">You can also install SAP HANA on a dedicated cluster.</span></span> <span data-ttu-id="d0887-420">Lásd: [magas rendelkezésre állás az SAP HANA Azure virtuális gépek (VM)] [ sap-hana-ha] további információt.</span><span class="sxs-lookup"><span data-stu-id="d0887-420">See [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha] for more information.</span></span>

### <a name="prepare-for-sap-hana-installation"></a><span data-ttu-id="d0887-421">SAP HANA-telepítés előkészítése</span><span class="sxs-lookup"><span data-stu-id="d0887-421">Prepare for SAP HANA installation</span></span>

<span data-ttu-id="d0887-422">Általában javasoljuk LVM kötetek, amelyek adatokat tárolhatnak, és a naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="d0887-422">We generally recommend using LVM for volumes that store data and log files.</span></span> <span data-ttu-id="d0887-423">Tesztelési célokra toostore hello adatainak és naplókönyvtárainak közvetlenül egy egyszerű lemezen fájl is kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="d0887-423">For testing purposes, you can also choose toostore hello data and log file directly on a plain disk.</span></span>

1. <span data-ttu-id="d0887-424">**[A]**  LVM</span><span class="sxs-lookup"><span data-stu-id="d0887-424">**[A]** LVM</span></span>  
   <span data-ttu-id="d0887-425">hello az alábbi példa azt feltételezi, hogy hello virtuális gépek rendelkezik-e a négy adatlemezt csatolni, amelyeket használt toocreate két kötet.</span><span class="sxs-lookup"><span data-stu-id="d0887-425">hello example below assumes that hello virtual machines have four data disks attached that should be used toocreate two volumes.</span></span>
   
   <span data-ttu-id="d0887-426">Az összes lemezt, amelyet az toouse fizikai köteteket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="d0887-426">Create physical volumes for all disks that you want toouse.</span></span>
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   <span data-ttu-id="d0887-427">Hozzon létre egy kötet csoport hello adatfájlok, hello naplófájlok egy kötet csoport és egy hello megosztott könyvtárában SAP HANA</span><span class="sxs-lookup"><span data-stu-id="d0887-427">Create a volume group for hello data files, one volume group for hello log files and one for hello shared directory of SAP HANA</span></span>
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   <span data-ttu-id="d0887-428">Hello logikai köteteket hozhat létre</span><span class="sxs-lookup"><span data-stu-id="d0887-428">Create hello logical volumes</span></span>
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   <span data-ttu-id="d0887-429">Hello csatlakoztatási könyvtárak létrehozása, és másolja az összes logikai kötet UUID hello</span><span class="sxs-lookup"><span data-stu-id="d0887-429">Create hello mount directories and copy hello UUID of all logical volumes</span></span>
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   <span data-ttu-id="d0887-430">Három logikai kötetek hello autofs bejegyzéseket létrehozni</span><span class="sxs-lookup"><span data-stu-id="d0887-430">Create autofs entries for hello three logical volumes</span></span>
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   <span data-ttu-id="d0887-431">A sor toosudo vi /etc/auto.direct beszúrása</span><span class="sxs-lookup"><span data-stu-id="d0887-431">Insert this line toosudo vi /etc/auto.direct</span></span>
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   <span data-ttu-id="d0887-432">Csatlakoztassa hello új köteteket</span><span class="sxs-lookup"><span data-stu-id="d0887-432">Mount hello new volumes</span></span>
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. <span data-ttu-id="d0887-433">**[A]**  Egyszerű lemez</span><span class="sxs-lookup"><span data-stu-id="d0887-433">**[A]** Plain Disks</span></span>  

   <span data-ttu-id="d0887-434">A kis vagy bemutató rendszerek, elhelyezhet egy lemezt a HANA adatainak és naplókönyvtárainak fájlokat.</span><span class="sxs-lookup"><span data-stu-id="d0887-434">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="d0887-435">hello következő parancsok /dev/sdc hozza létre a partíciót, és formázza xfs.</span><span class="sxs-lookup"><span data-stu-id="d0887-435">hello following commands create a partition on /dev/sdc and format it with xfs.</span></span>
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down hello id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   <span data-ttu-id="d0887-436">A sor too/etc/auto.direct beszúrása</span><span class="sxs-lookup"><span data-stu-id="d0887-436">Insert this line too/etc/auto.direct</span></span>
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   <span data-ttu-id="d0887-437">Hello célkönyvtár és azt csatlakoztatja a hello lemez.</span><span class="sxs-lookup"><span data-stu-id="d0887-437">Create hello target directory and mount hello disk.</span></span>
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a><span data-ttu-id="d0887-438">SAP HANA telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-438">Installing SAP HANA</span></span>

<span data-ttu-id="d0887-439">hello lépések alapuló hello 4 fejezete [SAP HANA SR teljesítmény optimalizált forgatókönyv útmutató] [ suse-hana-ha-guide] tooinstall SAP HANA replikációs.</span><span class="sxs-lookup"><span data-stu-id="d0887-439">hello following steps are based on chapter 4 of hello [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] tooinstall SAP HANA System Replication.</span></span> <span data-ttu-id="d0887-440">Olvassa el, mielőtt hello telepítés folytatásához.</span><span class="sxs-lookup"><span data-stu-id="d0887-440">Please read it before you continue hello installation.</span></span>

1. <span data-ttu-id="d0887-441">**[A]**  Hdblcm hello HANA DVD-ről futtatva</span><span class="sxs-lookup"><span data-stu-id="d0887-441">**[A]** Run hdblcm from hello HANA DVD</span></span>
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. <span data-ttu-id="d0887-442">**[A]**  SAP állomás ügynökök frissítése</span><span class="sxs-lookup"><span data-stu-id="d0887-442">**[A]** Upgrade SAP Host Agent</span></span>

   <span data-ttu-id="d0887-443">Hello legújabb SAP a gazdagép ügynöke archív letöltését hello [SAP Softwarecenter] [ sap-swcenter] és futtatási hello parancs tooupgrade hello ügynök következő.</span><span class="sxs-lookup"><span data-stu-id="d0887-443">Download hello latest SAP Host Agent archive from hello [SAP Softwarecenter][sap-swcenter] and run hello following command tooupgrade hello agent.</span></span> <span data-ttu-id="d0887-444">Cserélje le a hello elérési toohello toopoint toohello archívumfájl letöltött.</span><span class="sxs-lookup"><span data-stu-id="d0887-444">Replace hello path toohello archive toopoint toohello file you downloaded.</span></span>
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path tooSAP Host Agent SAR&gt;</b> 
   </code></pre>

1. <span data-ttu-id="d0887-445">**[1]**  Létrehozása HANA replikációs (rendszergazdaként)</span><span class="sxs-lookup"><span data-stu-id="d0887-445">**[1]** Create HANA replication (as root)</span></span>  

   <span data-ttu-id="d0887-446">Futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="d0887-446">Run hello following command.</span></span> <span data-ttu-id="d0887-447">Győződjön meg arról, hogy tooreplace félkövér karakterláncok (HANA rendszer azonosító HDB és példány újrahasznosítása 03) a SAP HANA-telepítés hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="d0887-447">Make sure tooreplace bold strings (HANA System ID HDB and instance number 03) with hello values of your SAP HANA installation.</span></span>
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. <span data-ttu-id="d0887-448">**[A]**  Keystore bejegyzés (rendszergazdaként) létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-448">**[A]** Create keystore entry (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. <span data-ttu-id="d0887-449">**[1]**  Adatbázis biztonsági másolata (rendszergazdaként)</span><span class="sxs-lookup"><span data-stu-id="d0887-449">**[1]** Backup database (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. <span data-ttu-id="d0887-450">**[1]**  Toohello HANA sapsid felhasználói váltson, és hozzon létre hello elsődleges hely.</span><span class="sxs-lookup"><span data-stu-id="d0887-450">**[1]** Switch toohello HANA sapsid user and create hello primary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. <span data-ttu-id="d0887-451">**[2]**  Toohello HANA sapsid felhasználói váltson, és hello másodlagos hely létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d0887-451">**[2]** Switch toohello HANA sapsid user and create hello secondary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. <span data-ttu-id="d0887-452">**[1]**  Fürterőforrások SAP HANA létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-452">**[1]** Create SAP HANA cluster resources</span></span>

   <span data-ttu-id="d0887-453">Először hozzon létre hello topológia.</span><span class="sxs-lookup"><span data-stu-id="d0887-453">First, create hello topology.</span></span>
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number and HANA system id
   
   crm(live)configure# primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b>   ocf:suse:SAPHanaTopology \
     operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"
    
   crm(live)configure# clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>
   
   <span data-ttu-id="d0887-454">Ezután hozzon létre hello HANA erőforrások</span><span class="sxs-lookup"><span data-stu-id="d0887-454">Next, create hello HANA resources</span></span>
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
    
   crm(live)configure# primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
    
   crm(live)configure# ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
    
   crm(live)configure# primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
     meta target-role="Started" is-managed="true" \ 
     operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
     op monitor interval="10s" timeout="20s" \ 
     params ip="<b>10.0.0.12</b>" 

   crm(live)configure# primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
     op monitor timeout=20s interval=10 depth=0 

   crm(live)configure# group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  

   crm(live)configure# order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   <span data-ttu-id="d0887-455">Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="d0887-455">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="d0887-456">Nem fontos a csomópont hello erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="d0887-456">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # <b>Online: [ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Clone Set: cln_SAPHanaTopology_HDB_HDB03 [rsc_SAPHanaTopology_HDB_HDB03]
   #      <b>Started: [ nws-cl-0 nws-cl-1 ]</b>
   #  Master/Slave Set: msl_SAPHana_HDB_HDB03 [rsc_SAPHana_HDB_HDB03]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g_ip_HDB_HDB03
   #      rsc_ip_HDB_HDB03   (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      rsc_nc_HDB_HDB03   (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   # rsc_st_azure_1  (stonith:fence_azure_arm):      <b>Started nws-cl-0</b>
   # rsc_st_azure_2  (stonith:fence_azure_arm):      <b>Started nws-cl-1</b>
   </code></pre>

1. <span data-ttu-id="d0887-457">**[1]**  Telepítés hello SAP NetWeaver adatbázispéldány</span><span class="sxs-lookup"><span data-stu-id="d0887-457">**[1]** Install hello SAP NetWeaver database instance</span></span>

   <span data-ttu-id="d0887-458">Telepítés hello SAP NetWeaver adatbázispéldány legfelső szintű használatával egy virtuális állomásnevet, amely hello terheléselosztó előtér-konfiguráció hello adatbázis toohello IP-címe például <b>nws-db</b> és <b>10.0.0.12</b>.</span><span class="sxs-lookup"><span data-stu-id="d0887-458">Install hello SAP NetWeaver database instance as root using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello database for example <b>nws-db</b> and <b>10.0.0.12</b>.</span></span>

   <span data-ttu-id="d0887-459">Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d0887-459">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a><span data-ttu-id="d0887-460">SAP NetWeaver alkalmazáskiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-460">SAP NetWeaver application server installation</span></span>

<span data-ttu-id="d0887-461">Kövesse ezeket a lépéseket tooinstall SAP alkalmazáskiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d0887-461">Follow these steps tooinstall an SAP application server.</span></span> <span data-ttu-id="d0887-462">hello lépéseket alábbi azt feltételezik, hogy hello alkalmazáskiszolgáló hello ASC/SCS eltérő kiszolgálóra és HANA kiszolgálók telepíteni.</span><span class="sxs-lookup"><span data-stu-id="d0887-462">hello steps bellow assume that you install hello application server on a server different from hello ASCS/SCS and HANA servers.</span></span> <span data-ttu-id="d0887-463">Ellenkező esetben egyes (például a gazdagép névfeloldásához konfigurálása) alatt hello lépések nem szükségesek.</span><span class="sxs-lookup"><span data-stu-id="d0887-463">Otherwise some of hello steps below (like configuring host name resolution) are not needed.</span></span>

1. <span data-ttu-id="d0887-464">A telepítő állomásnév</span><span class="sxs-lookup"><span data-stu-id="d0887-464">Setup host name resolution</span></span>    
   <span data-ttu-id="d0887-465">DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d0887-465">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="d0887-466">Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="d0887-466">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="d0887-467">Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév</span><span class="sxs-lookup"><span data-stu-id="d0887-467">Replace hello IP address and hello hostname in hello following commands</span></span>
   ```bash
   sudo vi /etc/hosts
   ```
   <span data-ttu-id="d0887-468">Helyezze be a következő sorokat túl/etc/hosts hello.</span><span class="sxs-lookup"><span data-stu-id="d0887-468">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="d0887-469">Hello IP cím és az állomásnév toomatch a környezet módosítása</span><span class="sxs-lookup"><span data-stu-id="d0887-469">Change hello IP address and hostname toomatch your environment</span></span>    
    
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of hello application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. <span data-ttu-id="d0887-470">Hello sapmnt könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-470">Create hello sapmnt directory</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. <span data-ttu-id="d0887-471">Autofs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-471">Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="d0887-472">Új fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0887-472">Create a new file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   <span data-ttu-id="d0887-473">Indítsa újra a autofs toomount hello új megosztások</span><span class="sxs-lookup"><span data-stu-id="d0887-473">Restart autofs toomount hello new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="d0887-474">Lapozófájl konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0887-474">Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="d0887-475">Indítsa újra a hello ügynök tooactivate hello módosítása</span><span class="sxs-lookup"><span data-stu-id="d0887-475">Restart hello Agent tooactivate hello change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. <span data-ttu-id="d0887-476">SAP NetWeaver alkalmazáskiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="d0887-476">Install SAP NetWeaver application server</span></span>

   <span data-ttu-id="d0887-477">Elsődleges vagy további SAP NetWeaver alkalmazások kiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="d0887-477">Install a primary or additional SAP NetWeaver applications server.</span></span>

   <span data-ttu-id="d0887-478">Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d0887-478">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="d0887-479">Biztonságos tár SAP HANA frissítése</span><span class="sxs-lookup"><span data-stu-id="d0887-479">Update SAP HANA secure store</span></span>

   <span data-ttu-id="d0887-480">Frissítés hello SAP HANA biztonságos tárolására toopoint toohello virtuális nevét hello SAP HANA replikációs beállítás.</span><span class="sxs-lookup"><span data-stu-id="d0887-480">Update hello SAP HANA secure store toopoint toohello virtual name of hello SAP HANA System Replication setup.</span></span>
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a><span data-ttu-id="d0887-481">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0887-481">Next steps</span></span>
* <span data-ttu-id="d0887-482">[Az Azure virtuális gépek tervezési és megvalósítási az SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="d0887-482">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="d0887-483">[Az SAP Azure virtuális gépek telepítése][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="d0887-483">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="d0887-484">[Az SAP Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="d0887-484">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="d0887-485">Hogyan tooestablish magas rendelkezésre állású és az Azure (nagy példányokat), az SAP HANA vész-helyreállítási terv: toolearn [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="d0887-485">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span>
* <span data-ttu-id="d0887-486">Hogyan tooestablish magas rendelkezésre állású és az Azure virtuális gépeken, a SAP HANA vész-helyreállítási terv: toolearn [magas rendelkezésre állás az SAP HANA Azure virtuális gépek (VM)][sap-hana-ha]</span><span class="sxs-lookup"><span data-stu-id="d0887-486">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]</span></span>
