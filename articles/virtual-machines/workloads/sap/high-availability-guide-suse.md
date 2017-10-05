---
title: "Az Azure virtuális gépek magas rendelkezésre állás a SAP NetWeaver a SUSE Linux Enterprise Server SAP alkalmazásokhoz |} Microsoft Docs"
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
ms.openlocfilehash: 16e09797926f29bc18cb05671c986c74f9c2d4f8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a><span data-ttu-id="143ee-103">Magas rendelkezésre állás a SAP NetWeaver a SUSE Linux Enterprise Server Azure virtuális gépeken az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="143ee-103">High availability for SAP NetWeaver on Azure VMs on SUSE Linux Enterprise Server for SAP applications</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

<span data-ttu-id="143ee-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span><span class="sxs-lookup"><span data-stu-id="143ee-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span></span>
<span data-ttu-id="143ee-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span><span class="sxs-lookup"><span data-stu-id="143ee-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span></span>
<span data-ttu-id="143ee-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="143ee-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
<span data-ttu-id="143ee-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="143ee-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
<span data-ttu-id="143ee-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="143ee-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="143ee-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span><span class="sxs-lookup"><span data-stu-id="143ee-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span></span>
<span data-ttu-id="143ee-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="143ee-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>
<span data-ttu-id="143ee-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span><span class="sxs-lookup"><span data-stu-id="143ee-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span></span>
<span data-ttu-id="143ee-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="143ee-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

<span data-ttu-id="143ee-113">Ez a cikk ismerteti, hogyan telepítse a virtuális gépeket, a virtuális gépet állíthat be, a fürt-keretrendszer telepítése és magas rendelkezésre állású SAP NetWeaver 7.50 rendszert telepíteni.</span><span class="sxs-lookup"><span data-stu-id="143ee-113">This article describes how to deploy the virtual machines, configure the virtual machines, install the cluster framework and install a highly available SAP NetWeaver 7.50 system.</span></span>
<span data-ttu-id="143ee-114">A példa konfigurációkban telepítési parancsok stb. Asc példányszámának 00, SSZON példányszámának 02 és SAP rendszer azonosító NWS szolgál.</span><span class="sxs-lookup"><span data-stu-id="143ee-114">In the example configurations, installation commands etc. ASCS instance number 00, ERS instance number 02 and SAP System ID NWS is used.</span></span> <span data-ttu-id="143ee-115">A példában szereplő erőforrások (például virtuális gépek, virtuális hálózatok) nevei azt feltételezik, használja a [sablon összevont] [ template-converged] SAP rendszer azonosító NWS az erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="143ee-115">The names of the resources (for example virtual machines, virtual networks) in the example assume that you have used the [converged template][template-converged] with SAP system ID NWS to create the resources.</span></span>

<span data-ttu-id="143ee-116">Olvassa el a következő SAP megjegyzések és által írt cikkeket először</span><span class="sxs-lookup"><span data-stu-id="143ee-116">Read the following SAP Notes and papers first</span></span>

* <span data-ttu-id="143ee-117">SAP Megjegyzés [1928533], amelynek van:</span><span class="sxs-lookup"><span data-stu-id="143ee-117">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="143ee-118">Az SAP szoftver központi telepítése támogatott Azure Virtuálisgép-méretek listáját</span><span class="sxs-lookup"><span data-stu-id="143ee-118">List of Azure VM sizes that are supported for the deployment of SAP software</span></span>
  * <span data-ttu-id="143ee-119">Az Azure Virtuálisgép-méretek fontos készletkapacitás információival</span><span class="sxs-lookup"><span data-stu-id="143ee-119">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="143ee-120">Támogatott SAP szoftver, és az operációs rendszer és az adatbázis kombinációját</span><span class="sxs-lookup"><span data-stu-id="143ee-120">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="143ee-121">A Windows és a Microsoft Azure Linux szükséges SAP kernel verziója</span><span class="sxs-lookup"><span data-stu-id="143ee-121">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="143ee-122">SAP Megjegyzés [2015553] a SAP-támogatott SAP szoftverek központi telepítése az Azure-ban szükséges előfeltételeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="143ee-122">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="143ee-123">SAP Megjegyzés [2205917] javasolt a SUSE Linux Enterprise Server operációs rendszer beállításait az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="143ee-123">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="143ee-124">SAP Megjegyzés [1944799] SUSE Linux Enterprise Server SAP HANA-irányelvek rendelkezik az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="143ee-124">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="143ee-125">SAP Megjegyzés [2178632] tartalmaz részletes információkat az Azure-ban SAP jelentett összes figyelési metrikákat.</span><span class="sxs-lookup"><span data-stu-id="143ee-125">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="143ee-126">SAP Megjegyzés [2191498] vannak a szükséges SAP gazdagép-ügynök verziója Linux az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="143ee-126">SAP Note [2191498] has the required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="143ee-127">SAP Megjegyzés [2243692] SAP Azure Linux licenceléssel kapcsolatos információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="143ee-127">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="143ee-128">SAP Megjegyzés [1984787] SUSE Linux Enterprise Server 12 vonatkozó általános információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="143ee-128">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="143ee-129">SAP Megjegyzés [1999351] további információkat talál az Azure fokozott Figyelőbővítmény az SAP rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="143ee-129">SAP Note [1999351] has additional troubleshooting information for the Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="143ee-130">[SAP közösségi WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) rendelkezik az összes szükséges SAP megjegyzések Linux.</span><span class="sxs-lookup"><span data-stu-id="143ee-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="143ee-131">[Azure virtuális gépek tervezési és megvalósítási az SAP Linux rendszeren][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="143ee-131">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="143ee-132">[Az Azure virtuális gépek telepítése az SAP, Linux (Ez a cikk)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="143ee-132">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="143ee-133">[Az SAP Linux Azure virtuális gépek DBMS-telepítés][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="143ee-133">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="143ee-134">[SAP HANA SR teljesítményre optimalizált forgatókönyv][suse-hana-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="143ee-134">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide]</span></span>  
  <span data-ttu-id="143ee-135">Az útmutató a helyszíni SAP HANA replikációs beállítása az összes szükséges információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="143ee-135">The guide contains all required information to set up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="143ee-136">Ez az útmutató használja kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="143ee-136">Use this guide as a baseline.</span></span>
* <span data-ttu-id="143ee-137">[Magas rendelkezésre álló NFS tár DRBD és támasztja] [ suse-drbd-guide] az útmutató a magas rendelkezésre állású NFS-kiszolgáló beállítása az összes szükséges információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="143ee-137">[Highly Available NFS Storage with DRBD and Pacemaker][suse-drbd-guide] The guide contains all required information to set up a highly available NFS server.</span></span> <span data-ttu-id="143ee-138">Ez az útmutató használja kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="143ee-138">Use this guide as a baseline.</span></span>


## <a name="overview"></a><span data-ttu-id="143ee-139">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="143ee-139">Overview</span></span>

<span data-ttu-id="143ee-140">Magas rendelkezésre állás eléréséhez SAP NetWeaver az NFS-kiszolgáló szükséges.</span><span class="sxs-lookup"><span data-stu-id="143ee-140">To achieve high availability, SAP NetWeaver requires an NFS server.</span></span> <span data-ttu-id="143ee-141">Az NFS-kiszolgáló egy külön fürtben lett konfigurálva, és több SAP-rendszerek által használható.</span><span class="sxs-lookup"><span data-stu-id="143ee-141">The NFS server is configured in a separate cluster and can be used by multiple SAP systems.</span></span>

![SAP NetWeaver magas rendelkezésre állás – Áttekintés](./media/high-availability-guide-suse/img_001.png)

<span data-ttu-id="143ee-143">Az NFS-kiszolgáló, a SAP NetWeaver ASC, a SAP NetWeaver SCS, a SAP NetWeaver SSZON és az SAP HANA-adatbázisból virtuális állomásnév és a virtuális IP-címek használata.</span><span class="sxs-lookup"><span data-stu-id="143ee-143">The NFS server, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS and the SAP HANA database use virtual hostname and virtual IP addresses.</span></span> <span data-ttu-id="143ee-144">Az Azure a terheléselosztó virtuális IP-cím szükséges.</span><span class="sxs-lookup"><span data-stu-id="143ee-144">On Azure, a load balancer is required to use a virtual IP address.</span></span> <span data-ttu-id="143ee-145">Az alábbi lista a terheléselosztó-konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="143ee-145">The following list shows the configuration of the load balancer.</span></span>

### <a name="nfs-server"></a><span data-ttu-id="143ee-146">NFS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="143ee-146">NFS Server</span></span>
* <span data-ttu-id="143ee-147">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="143ee-147">Frontend configuration</span></span>
  * <span data-ttu-id="143ee-148">IP-cím 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="143ee-148">IP address 10.0.0.4</span></span>
* <span data-ttu-id="143ee-149">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="143ee-149">Backend configuration</span></span>
  * <span data-ttu-id="143ee-150">Az összes virtuális gépet, amely az NFS-fürt részét kell képezniük elsődleges hálózati illesztők csatlakozik</span><span class="sxs-lookup"><span data-stu-id="143ee-150">Connected to primary network interfaces of all virtual machines that should be part of the NFS cluster</span></span>
* <span data-ttu-id="143ee-151">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="143ee-151">Probe Port</span></span>
  * <span data-ttu-id="143ee-152">Port 61000</span><span class="sxs-lookup"><span data-stu-id="143ee-152">Port 61000</span></span>
* <span data-ttu-id="143ee-153">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="143ee-153">Loadbalancing rules</span></span>
  * <span data-ttu-id="143ee-154">2049 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-154">2049 TCP</span></span> 
  * <span data-ttu-id="143ee-155">2049 UDP</span><span class="sxs-lookup"><span data-stu-id="143ee-155">2049 UDP</span></span>

### <a name="ascs"></a><span data-ttu-id="143ee-156">(A) SCS</span><span class="sxs-lookup"><span data-stu-id="143ee-156">(A)SCS</span></span>
* <span data-ttu-id="143ee-157">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="143ee-157">Frontend configuration</span></span>
  * <span data-ttu-id="143ee-158">IP-cím 10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="143ee-158">IP address 10.0.0.10</span></span>
* <span data-ttu-id="143ee-159">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="143ee-159">Backend configuration</span></span>
  * <span data-ttu-id="143ee-160">Az összes virtuális gépet, amely a (A) részét kell képezniük elsődleges hálózati illesztők csatlakozik SCS/SSZON fürt</span><span class="sxs-lookup"><span data-stu-id="143ee-160">Connected to primary network interfaces of all virtual machines that should be part of the (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="143ee-161">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="143ee-161">Probe Port</span></span>
  * <span data-ttu-id="143ee-162">Port 620**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="143ee-162">Port 620**&lt;nr&gt;**</span></span>
* <span data-ttu-id="143ee-163">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="143ee-163">Loadbalancing rules</span></span>
  * <span data-ttu-id="143ee-164">32**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-164">32**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="143ee-165">36**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-165">36**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="143ee-166">39**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-166">39**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="143ee-167">81-es**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-167">81**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="143ee-168">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-168">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="143ee-169">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-169">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="143ee-170">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-170">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="ers"></a><span data-ttu-id="143ee-171">SSZON</span><span class="sxs-lookup"><span data-stu-id="143ee-171">ERS</span></span>
* <span data-ttu-id="143ee-172">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="143ee-172">Frontend configuration</span></span>
  * <span data-ttu-id="143ee-173">IP-cím 10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="143ee-173">IP address 10.0.0.11</span></span>
* <span data-ttu-id="143ee-174">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="143ee-174">Backend configuration</span></span>
  * <span data-ttu-id="143ee-175">Az összes virtuális gépet, amely a (A) részét kell képezniük elsődleges hálózati illesztők csatlakozik SCS/SSZON fürt</span><span class="sxs-lookup"><span data-stu-id="143ee-175">Connected to primary network interfaces of all virtual machines that should be part of the (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="143ee-176">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="143ee-176">Probe Port</span></span>
  * <span data-ttu-id="143ee-177">Port 621**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="143ee-177">Port 621**&lt;nr&gt;**</span></span>
* <span data-ttu-id="143ee-178">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="143ee-178">Loadbalancing rules</span></span>
  * <span data-ttu-id="143ee-179">33**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-179">33**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="143ee-180">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-180">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="143ee-181">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-181">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="143ee-182">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-182">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="sap-hana"></a><span data-ttu-id="143ee-183">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="143ee-183">SAP HANA</span></span>
* <span data-ttu-id="143ee-184">Előtérbeli konfigurációja</span><span class="sxs-lookup"><span data-stu-id="143ee-184">Frontend configuration</span></span>
  * <span data-ttu-id="143ee-185">10.0.0.12 IP-cím</span><span class="sxs-lookup"><span data-stu-id="143ee-185">IP address 10.0.0.12</span></span>
* <span data-ttu-id="143ee-186">Háttérkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="143ee-186">Backend configuration</span></span>
  * <span data-ttu-id="143ee-187">Az összes virtuális gépet, amely a HANA fürt részét kell képezniük elsődleges hálózati illesztők csatlakozik</span><span class="sxs-lookup"><span data-stu-id="143ee-187">Connected to primary network interfaces of all virtual machines that should be part of the HANA cluster</span></span>
* <span data-ttu-id="143ee-188">Mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="143ee-188">Probe Port</span></span>
  * <span data-ttu-id="143ee-189">Port a 625**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="143ee-189">Port 625**&lt;nr&gt;**</span></span>
* <span data-ttu-id="143ee-190">Terheléselosztás szabályok</span><span class="sxs-lookup"><span data-stu-id="143ee-190">Loadbalancing rules</span></span>
  * <span data-ttu-id="143ee-191">3**&lt;nr&gt;**15 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-191">3**&lt;nr&gt;**15 TCP</span></span>
  * <span data-ttu-id="143ee-192">3**&lt;nr&gt;**17 TCP</span><span class="sxs-lookup"><span data-stu-id="143ee-192">3**&lt;nr&gt;**17 TCP</span></span>

## <a name="setting-up-a-highly-available-nfs-server"></a><span data-ttu-id="143ee-193">Egy magas rendelkezésre állású NFS-kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="143ee-193">Setting up a highly available NFS server</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="143ee-194">Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-194">Deploying Linux</span></span>

<span data-ttu-id="143ee-195">Az Azure piactéren SUSE Linux Enterprise Server SAP alkalmazások 12-es segítségével új virtuális gépek telepítése a kép tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="143ee-195">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use to deploy new virtual machines.</span></span>
<span data-ttu-id="143ee-196">Segítségével a gyors üzembe helyezési sablonok valamelyikét a githubon központi telepítése az összes szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="143ee-196">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="143ee-197">A sablon telepíti, a virtuális gépek, a terheléselosztó hasonló adataival, a rendelkezésre állási csoport stb. Kövesse az alábbi lépéseket a sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="143ee-197">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="143ee-198">Nyissa meg a [SAP fájl server sablon] [ template-file-server] az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="143ee-198">Open the [SAP file server template][template-file-server] in the Azure portal</span></span>   
1. <span data-ttu-id="143ee-199">Adja meg a következő paraméterek</span><span class="sxs-lookup"><span data-stu-id="143ee-199">Enter the following parameters</span></span>
   1. <span data-ttu-id="143ee-200">Erőforrás-előtag</span><span class="sxs-lookup"><span data-stu-id="143ee-200">Resource Prefix</span></span>  
      <span data-ttu-id="143ee-201">Adja meg a használni kívánt előtagot.</span><span class="sxs-lookup"><span data-stu-id="143ee-201">Enter the prefix you want to use.</span></span> <span data-ttu-id="143ee-202">Az érték a telepített erőforrások esetén használatos előtagjaként.</span><span class="sxs-lookup"><span data-stu-id="143ee-202">The value is used as a prefix for the resources that are deployed.</span></span>
   2. <span data-ttu-id="143ee-203">Operációs rendszer típusa</span><span class="sxs-lookup"><span data-stu-id="143ee-203">Os Type</span></span>  
      <span data-ttu-id="143ee-204">Válasszon egyet a Linux terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="143ee-204">Select one of the Linux distributions.</span></span> <span data-ttu-id="143ee-205">Ehhez a példához válassza ki a SLES 12 rendszert</span><span class="sxs-lookup"><span data-stu-id="143ee-205">For this example, select SLES 12</span></span>
   3. <span data-ttu-id="143ee-206">Rendszergazda felhasználónevét és a rendszergazdai jelszó</span><span class="sxs-lookup"><span data-stu-id="143ee-206">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="143ee-207">Új felhasználó jön létre, amely segítségével jelentkezzen be a gépre.</span><span class="sxs-lookup"><span data-stu-id="143ee-207">A new user is created that can be used to log on to the machine.</span></span>
   4. <span data-ttu-id="143ee-208">Alhálózati azonosító</span><span class="sxs-lookup"><span data-stu-id="143ee-208">Subnet Id</span></span>  
      <span data-ttu-id="143ee-209">Az alhálózat, amelyhez a virtuális gépek csatlakoznia kell az azonosítója.</span><span class="sxs-lookup"><span data-stu-id="143ee-209">The ID of the subnet to which the virtual machines should be connected to.</span></span> <span data-ttu-id="143ee-210">Hagyja üresen, ha azt szeretné, hozzon létre egy új virtuális hálózatot, vagy jelölje ki az alhálózatot, a VPN- vagy Express Route virtuális hálózat a virtuális gép és a helyszíni hálózathoz csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="143ee-210">Leave empty if you want to create a new virtual network or select the subnet of your VPN or Express Route virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="143ee-211">Az azonosító általában a következőképpen néz következő**&lt;előfizetés-azonosító&gt;**/resourceGroups/**&lt;erőforráscsoport-név&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuálishálózat-név&gt;**/subnets/**&lt;alhálózat neve&gt;**</span><span class="sxs-lookup"><span data-stu-id="143ee-211">The ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="143ee-212">Telepítés</span><span class="sxs-lookup"><span data-stu-id="143ee-212">Installation</span></span>

<span data-ttu-id="143ee-213">A következő elemek fűzve előtagként vagy **[A]** – az összes csomópont alkalmazandó **[1]** – csak érvényes csomópont 1 vagy **[2]** - csomópont 2 csak érvényes.</span><span class="sxs-lookup"><span data-stu-id="143ee-213">The following items are prefixed with either **[A]** - applicable to all nodes, **[1]** - only applicable to node 1 or **[2]** - only applicable to node 2.</span></span>

1. <span data-ttu-id="143ee-214">**[A]**  SLES frissítése</span><span class="sxs-lookup"><span data-stu-id="143ee-214">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="143ee-215">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-215">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="143ee-216">**[2]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-216">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="143ee-217">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-217">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="143ee-218">**[A]**  Telepítése magas rendelkezésre ÁLLÁSÚ bővítmény</span><span class="sxs-lookup"><span data-stu-id="143ee-218">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="143ee-219">**[A]**  Állomásnév beállítása</span><span class="sxs-lookup"><span data-stu-id="143ee-219">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="143ee-220">DNS-kiszolgálót használjon, vagy módosítsa az/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="143ee-220">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="143ee-221">Ez a példa bemutatja, hogyan használható az/etc/hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="143ee-221">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="143ee-222">Cserélje le az IP-cím és a következő parancsokat az állomásnév</span><span class="sxs-lookup"><span data-stu-id="143ee-222">Replace the IP address and the hostname in the following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="143ee-223">Helyezze be a következő sorokat/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="143ee-223">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="143ee-224">Az IP-cím és a környezet megfelelő állomásnév módosítása</span><span class="sxs-lookup"><span data-stu-id="143ee-224">Change the IP address and hostname to match your environment</span></span>   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. <span data-ttu-id="143ee-225">**[1]**  Fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-225">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="143ee-226">**[2]**  Csomópont hozzáadása a fürthöz</span><span class="sxs-lookup"><span data-stu-id="143ee-226">**[2]** Add node to cluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="143ee-227">**[A]**  Hacluster ugyanazt a jelszót a jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="143ee-227">**[A]** Change hacluster password to the same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="143ee-228">**[A]**  Egyéb átvitelt használ, és adja hozzá a csomópontlista corosync konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="143ee-228">**[A]** Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="143ee-229">Fürt egyébként nem működik.</span><span class="sxs-lookup"><span data-stu-id="143ee-229">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="143ee-230">Vegye fel a következő félkövér tartalmat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="143ee-230">Add the following bold content to the file.</span></span>
   
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

   <span data-ttu-id="143ee-231">Indítsa újra a corosync szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="143ee-231">Then restart the corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="143ee-232">**[A]**  Drbd összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-232">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="143ee-233">**[A]**  Hozzon létre egy partíciót a drbd eszköz</span><span class="sxs-lookup"><span data-stu-id="143ee-233">**[A]** Create a partition for the drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="143ee-234">**[A]**  Létrehozása LVM konfigurációk</span><span class="sxs-lookup"><span data-stu-id="143ee-234">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. <span data-ttu-id="143ee-235">**[A]**  Az NFS drbd eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-235">**[A]** Create the NFS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   <span data-ttu-id="143ee-236">Helyezze be az új drbd eszköz és a kilépési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="143ee-236">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="143ee-237">Hozzon létre a drbd eszközt, és indítsa el</span><span class="sxs-lookup"><span data-stu-id="143ee-237">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="143ee-238">**[1]**  Kihagyása a kezdeti szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="143ee-238">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="143ee-239">**[1]**  Állítsa be az elsődleges csomópont</span><span class="sxs-lookup"><span data-stu-id="143ee-239">**[1]** Set the primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="143ee-240">**[1]**  Várjon, amíg az új drbd eszköz szinkronizált</span><span class="sxs-lookup"><span data-stu-id="143ee-240">**[1]** Wait until the new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="143ee-241">**[1]**  Fájlrendszerek létrehozása a drbd eszközökön</span><span class="sxs-lookup"><span data-stu-id="143ee-241">**[1]** Create file systems on the drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="143ee-242">Fürt keretrendszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-242">Configure Cluster Framework</span></span>

1. <span data-ttu-id="143ee-243">**[1]**  Az alapértelmezett beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="143ee-243">**[1]** Change the default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="143ee-244">**[1]**  NFS drbd eszköz hozzáadása a fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-244">**[1]** Add the NFS drbd device to the cluster configuration</span></span>

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

1. <span data-ttu-id="143ee-245">**[1]**  Az NFS-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-245">**[1]** Create the NFS server</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="143ee-246">**[1]**  Létrehozása az NFS fájlrendszert erőforrások</span><span class="sxs-lookup"><span data-stu-id="143ee-246">**[1]** Create the NFS File System resources</span></span>

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

1. <span data-ttu-id="143ee-247">**[1]**  Létrehozása az NFS exportálása</span><span class="sxs-lookup"><span data-stu-id="143ee-247">**[1]** Create the NFS exports</span></span>

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

1. <span data-ttu-id="143ee-248">**[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi a belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="143ee-248">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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

### <a name="create-stonith-device"></a><span data-ttu-id="143ee-249">STONITH eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-249">Create STONITH device</span></span>

<span data-ttu-id="143ee-250">A STONITH eszköz egy egyszerű szolgáltatást használ, szemben a Microsoft Azure engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="143ee-250">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="143ee-251">Kövesse az alábbi lépéseket egy egyszerű szolgáltatásnév létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="143ee-251">Follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="143ee-252">Ugrás a <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="143ee-252">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="143ee-253">Nyissa meg az Azure Active Directory panelt</span><span class="sxs-lookup"><span data-stu-id="143ee-253">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="143ee-254">Nyissa meg tulajdonságait, és jegyezze fel a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="143ee-254">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="143ee-255">Ez a **bérlőazonosító**.</span><span class="sxs-lookup"><span data-stu-id="143ee-255">This is the **tenant id**.</span></span>
1. <span data-ttu-id="143ee-256">Kattintson az alkalmazás-regisztráció</span><span class="sxs-lookup"><span data-stu-id="143ee-256">Click App registrations</span></span>
1. <span data-ttu-id="143ee-257">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="143ee-257">Click Add</span></span>
1. <span data-ttu-id="143ee-258">Adjon meg egy nevet, válassza ki a "Web app/API" alkalmazástípus, adja meg a bejelentkezési URL-címet (például http://localhost) és kattintson a Létrehozás gombra</span><span class="sxs-lookup"><span data-stu-id="143ee-258">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="143ee-259">A bejelentkezési URL-címet nem használja, és bármilyen érvényes URL-CÍMEK lehetnek</span><span class="sxs-lookup"><span data-stu-id="143ee-259">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="143ee-260">Válassza ki az új alkalmazást, és a beállítások lapon kattintson a kulcsok</span><span class="sxs-lookup"><span data-stu-id="143ee-260">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="143ee-261">Adja meg egy új kulcs leírását, válassza a "Soha nem jár le", és kattintson a Mentés gombra</span><span class="sxs-lookup"><span data-stu-id="143ee-261">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="143ee-262">Jegyezze fel az értéket.</span><span class="sxs-lookup"><span data-stu-id="143ee-262">Write down the Value.</span></span> <span data-ttu-id="143ee-263">Használják a **jelszó** a szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="143ee-263">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="143ee-264">Jegyezze fel az azonosítót.</span><span class="sxs-lookup"><span data-stu-id="143ee-264">Write down the Application Id.</span></span> <span data-ttu-id="143ee-265">A felhasználónév használják (**bejelentkezési azonosító** az alábbi lépéseket a) a szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="143ee-265">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="143ee-266">A szolgáltatás egyszerű nincs engedélye a alapértelmezés szerint az Azure-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="143ee-266">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="143ee-267">Hozzá kell rendelnie a szolgáltatás egyszerű engedélyek indítása és leállítása (felszabadítása) a fürt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="143ee-267">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="143ee-268">Ugrás a https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="143ee-268">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="143ee-269">Nyissa meg az összes erőforrás panel</span><span class="sxs-lookup"><span data-stu-id="143ee-269">Open the All resources blade</span></span>
1. <span data-ttu-id="143ee-270">Válassza ki a virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="143ee-270">Select the virtual machine</span></span>
1. <span data-ttu-id="143ee-271">Kattintson a hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="143ee-271">Click Access control (IAM)</span></span>
1. <span data-ttu-id="143ee-272">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="143ee-272">Click Add</span></span>
1. <span data-ttu-id="143ee-273">Válassza ki a szerepkör tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="143ee-273">Select the role Owner</span></span>
1. <span data-ttu-id="143ee-274">Adja meg az előbb létrehozott alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="143ee-274">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="143ee-275">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="143ee-275">Click OK</span></span>

#### <a name="1-create-the-stonith-devices"></a><span data-ttu-id="143ee-276">**[1]**  STONITH eszközök létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-276">**[1]** Create the STONITH devices</span></span>

<span data-ttu-id="143ee-277">Után szerkeszteni a virtuális gépek engedélyeit, beállíthatja a STONITH eszközök a fürtben.</span><span class="sxs-lookup"><span data-stu-id="143ee-277">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a><span data-ttu-id="143ee-278">**[1]**  STONITH eszköz használatának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-278">**[1]** Enable the use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a><span data-ttu-id="143ee-279">(A) SCS beállítása</span><span class="sxs-lookup"><span data-stu-id="143ee-279">Setting up (A)SCS</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="143ee-280">Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-280">Deploying Linux</span></span>

<span data-ttu-id="143ee-281">Az Azure piactéren SUSE Linux Enterprise Server SAP alkalmazások 12-es segítségével új virtuális gépek telepítése a kép tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="143ee-281">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use to deploy new virtual machines.</span></span> <span data-ttu-id="143ee-282">A Piactéri lemezképhez SAP NetWeaver erőforrás ügynökön tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="143ee-282">The marketplace image contains the resource agent for SAP NetWeaver.</span></span>

<span data-ttu-id="143ee-283">Segítségével a gyors üzembe helyezési sablonok valamelyikét a githubon központi telepítése az összes szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="143ee-283">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="143ee-284">A sablon telepíti, a virtuális gépek, a terheléselosztó hasonló adataival, a rendelkezésre állási csoport stb. Kövesse az alábbi lépéseket a sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="143ee-284">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="143ee-285">Nyissa meg a [ASC/SCS Multi SID sablon] [ template-multisid-xscs] vagy a [sablon összevont] [ template-converged] az Azure-portál a SCS/ASC sablon csak hoz létre a terheléselosztási szabályok SAP NetWeaver ASC/SCS és SSZON példányok (csak Linux), míg az átszervezett is létrejön a terheléselosztási szabályok-adatbázis (például a Microsoft SQL Server vagy az SAP HANA).</span><span class="sxs-lookup"><span data-stu-id="143ee-285">Open the [ASCS/SCS Multi SID template][template-multisid-xscs] or the [converged template][template-converged] on the Azure portal The ASCS/SCS template only creates the load-balancing rules for the SAP NetWeaver ASCS/SCS and ERS (Linux only) instances whereas the converged template also creates the load-balancing rules for a database (for example Microsoft SQL Server or SAP HANA).</span></span> <span data-ttu-id="143ee-286">Ha azt tervezi, hogy az SAP NetWeaver alapú rendszert telepíti, és szeretné telepíteni az adatbázis ugyanazon a gépen, használja a [sablon összevont][template-converged].</span><span class="sxs-lookup"><span data-stu-id="143ee-286">If you plan to install an SAP NetWeaver based system and you also want to install the database on the same machines, use the [converged template][template-converged].</span></span>
1. <span data-ttu-id="143ee-287">Adja meg a következő paraméterek</span><span class="sxs-lookup"><span data-stu-id="143ee-287">Enter the following parameters</span></span>
   1. <span data-ttu-id="143ee-288">Erőforrás-előtag (csak a sablon ASC/SCS Multi SID)</span><span class="sxs-lookup"><span data-stu-id="143ee-288">Resource Prefix (ASCS/SCS Multi SID template only)</span></span>  
      <span data-ttu-id="143ee-289">Adja meg a használni kívánt előtagot.</span><span class="sxs-lookup"><span data-stu-id="143ee-289">Enter the prefix you want to use.</span></span> <span data-ttu-id="143ee-290">Az érték a telepített erőforrások esetén használatos előtagjaként.</span><span class="sxs-lookup"><span data-stu-id="143ee-290">The value is used as a prefix for the resources that are deployed.</span></span>
   3. <span data-ttu-id="143ee-291">SAP rendszerazonosító (csak az átszervezett sablon)</span><span class="sxs-lookup"><span data-stu-id="143ee-291">Sap System Id (converged template only)</span></span>  
      <span data-ttu-id="143ee-292">Adja meg a telepíteni kívánt SAP rendszer SAP rendszer azonosítója.</span><span class="sxs-lookup"><span data-stu-id="143ee-292">Enter the SAP system Id of the SAP system you want to install.</span></span> <span data-ttu-id="143ee-293">Az azonosító előtagjaként szolgál a telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="143ee-293">The Id is used as a prefix for the resources that are deployed.</span></span>
   4. <span data-ttu-id="143ee-294">A készlet típusa</span><span class="sxs-lookup"><span data-stu-id="143ee-294">Stack Type</span></span>  
      <span data-ttu-id="143ee-295">Az SAP NetWeaver verem típusának kiválasztása</span><span class="sxs-lookup"><span data-stu-id="143ee-295">Select the SAP NetWeaver stack type</span></span>
   5. <span data-ttu-id="143ee-296">Operációs rendszer típusa</span><span class="sxs-lookup"><span data-stu-id="143ee-296">Os Type</span></span>  
      <span data-ttu-id="143ee-297">Válasszon egyet a Linux terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="143ee-297">Select one of the Linux distributions.</span></span> <span data-ttu-id="143ee-298">Ehhez a példához válassza ki a SLES 12 saját</span><span class="sxs-lookup"><span data-stu-id="143ee-298">For this example, select SLES 12 BYOS</span></span>
   6. <span data-ttu-id="143ee-299">DB típusa</span><span class="sxs-lookup"><span data-stu-id="143ee-299">Db Type</span></span>  
      <span data-ttu-id="143ee-300">Válassza ki a HANA</span><span class="sxs-lookup"><span data-stu-id="143ee-300">Select HANA</span></span>
   7. <span data-ttu-id="143ee-301">SAP mérete</span><span class="sxs-lookup"><span data-stu-id="143ee-301">Sap System Size</span></span>  
      <span data-ttu-id="143ee-302">Az új rendszer biztosít SAP mennyisége.</span><span class="sxs-lookup"><span data-stu-id="143ee-302">The amount of SAPS the new system provides.</span></span> <span data-ttu-id="143ee-303">Ha nem tudja, hogy hány SAP, a rendszer szükséges, kérje meg a SAP technológia Partner vagy a rendszer integráló</span><span class="sxs-lookup"><span data-stu-id="143ee-303">If you are not sure how many SAPS the system requires, please ask your SAP Technology Partner or System Integrator</span></span>
   8. <span data-ttu-id="143ee-304">Rendszer rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="143ee-304">System Availability</span></span>  
      <span data-ttu-id="143ee-305">Válassza ki a magas rendelkezésre ÁLLÁSÚ</span><span class="sxs-lookup"><span data-stu-id="143ee-305">Select HA</span></span>
   9. <span data-ttu-id="143ee-306">Rendszergazda felhasználónevét és a rendszergazdai jelszó</span><span class="sxs-lookup"><span data-stu-id="143ee-306">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="143ee-307">Új felhasználó jön létre, amely segítségével jelentkezzen be a gépre.</span><span class="sxs-lookup"><span data-stu-id="143ee-307">A new user is created that can be used to log on to the machine.</span></span>
   10. <span data-ttu-id="143ee-308">Alhálózati azonosító</span><span class="sxs-lookup"><span data-stu-id="143ee-308">Subnet Id</span></span>  
   <span data-ttu-id="143ee-309">Az alhálózat, amelyhez a virtuális gépek csatlakoznia kell az azonosítója.</span><span class="sxs-lookup"><span data-stu-id="143ee-309">The ID of the subnet to which the virtual machines should be connected to.</span></span>  <span data-ttu-id="143ee-310">Hagyja üresen, ha azt szeretné, hozzon létre egy új virtuális hálózatot, vagy válasszon ugyanazon az alhálózaton, amely a használni vagy létrehozni az NFS-kiszolgáló központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="143ee-310">Leave empty if you want to create a new virtual network or select the same subnet that you used or created as part of the NFS server deployment.</span></span> <span data-ttu-id="143ee-311">Az azonosító általában a következőképpen néz következő**&lt;előfizetés-azonosító&gt;**/resourceGroups/**&lt;erőforráscsoport-név&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuálishálózat-név&gt;**/subnets/**&lt;alhálózat neve&gt;**</span><span class="sxs-lookup"><span data-stu-id="143ee-311">The ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="143ee-312">Telepítés</span><span class="sxs-lookup"><span data-stu-id="143ee-312">Installation</span></span>

<span data-ttu-id="143ee-313">A következő elemek fűzve előtagként vagy **[A]** – az összes csomópont alkalmazandó **[1]** – csak érvényes csomópont 1 vagy **[2]** - csomópont 2 csak érvényes.</span><span class="sxs-lookup"><span data-stu-id="143ee-313">The following items are prefixed with either **[A]** - applicable to all nodes, **[1]** - only applicable to node 1 or **[2]** - only applicable to node 2.</span></span>

1. <span data-ttu-id="143ee-314">**[A]**  SLES frissítése</span><span class="sxs-lookup"><span data-stu-id="143ee-314">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="143ee-315">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-315">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="143ee-316">**[2]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-316">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="143ee-317">**[1]**  Ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-317">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="143ee-318">**[A]**  Telepítése magas rendelkezésre ÁLLÁSÚ bővítmény</span><span class="sxs-lookup"><span data-stu-id="143ee-318">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="143ee-319">**[A]**  Frissítés SAP erőforrás ügynökök</span><span class="sxs-lookup"><span data-stu-id="143ee-319">**[A]** Update SAP resource agents</span></span>  
   
   <span data-ttu-id="143ee-320">Az erőforrás-ügynökök csomag javítást kell használnia az új konfigurációt, ebben a cikkben ismertetett.</span><span class="sxs-lookup"><span data-stu-id="143ee-320">A patch for the resource-agents package is required to use the new configuration, that is described in this article.</span></span> <span data-ttu-id="143ee-321">Ellenőrizheti, ha a javítás már telepítve van a következő paranccsal</span><span class="sxs-lookup"><span data-stu-id="143ee-321">You can check, if the patch is already installed with the following command</span></span>

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   <span data-ttu-id="143ee-322">A kimeneti hasonlónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="143ee-322">The output should be similar to</span></span>

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   <span data-ttu-id="143ee-323">Ha a grep parancs nem található a IS_ERS paraméter, szeretné-e meg a javítás [a SUSE letöltési oldala](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span><span class="sxs-lookup"><span data-stu-id="143ee-323">If the grep command does not find the IS_ERS parameter, you need to install the patch listed on [the SUSE download page](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span></span>

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. <span data-ttu-id="143ee-324">**[A]**  Állomásnév beállítása</span><span class="sxs-lookup"><span data-stu-id="143ee-324">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="143ee-325">DNS-kiszolgálót használjon, vagy módosítsa az/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="143ee-325">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="143ee-326">Ez a példa bemutatja, hogyan használható az/etc/hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="143ee-326">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="143ee-327">Cserélje le az IP-cím és a következő parancsokat az állomásnév</span><span class="sxs-lookup"><span data-stu-id="143ee-327">Replace the IP address and the hostname in the following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="143ee-328">Helyezze be a következő sorokat/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="143ee-328">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="143ee-329">Az IP-cím és a környezet megfelelő állomásnév módosítása</span><span class="sxs-lookup"><span data-stu-id="143ee-329">Change the IP address and hostname to match your environment</span></span>   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. <span data-ttu-id="143ee-330">**[1]**  Fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-330">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="143ee-331">**[2]**  Csomópont hozzáadása a fürthöz</span><span class="sxs-lookup"><span data-stu-id="143ee-331">**[2]** Add node to cluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="143ee-332">**[A]**  Hacluster ugyanazt a jelszót a jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="143ee-332">**[A]** Change hacluster password to the same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="143ee-333">**[A]**  Egyéb átvitelt használ, és adja hozzá a csomópontlista corosync konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="143ee-333">**[A]** Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="143ee-334">Fürt egyébként nem működik.</span><span class="sxs-lookup"><span data-stu-id="143ee-334">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="143ee-335">Vegye fel a következő félkövér tartalmat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="143ee-335">Add the following bold content to the file.</span></span>
   
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

   <span data-ttu-id="143ee-336">Indítsa újra a corosync szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="143ee-336">Then restart the corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="143ee-337">**[A]**  Drbd összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-337">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="143ee-338">**[A]**  Hozzon létre egy partíciót a drbd eszköz</span><span class="sxs-lookup"><span data-stu-id="143ee-338">**[A]** Create a partition for the drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="143ee-339">**[A]**  Létrehozása LVM konfigurációk</span><span class="sxs-lookup"><span data-stu-id="143ee-339">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. <span data-ttu-id="143ee-340">**[A]**  A SCS drbd eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-340">**[A]** Create the SCS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   <span data-ttu-id="143ee-341">Helyezze be az új drbd eszköz és a kilépési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="143ee-341">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="143ee-342">Hozzon létre a drbd eszközt, és indítsa el</span><span class="sxs-lookup"><span data-stu-id="143ee-342">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. <span data-ttu-id="143ee-343">**[A]**  A SSZON drbd eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-343">**[A]** Create the ERS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   <span data-ttu-id="143ee-344">Helyezze be az új drbd eszköz és a kilépési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="143ee-344">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="143ee-345">Hozzon létre a drbd eszközt, és indítsa el</span><span class="sxs-lookup"><span data-stu-id="143ee-345">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="143ee-346">**[1]**  Kihagyása a kezdeti szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="143ee-346">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="143ee-347">**[1]**  Állítsa be az elsődleges csomópont</span><span class="sxs-lookup"><span data-stu-id="143ee-347">**[1]** Set the primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="143ee-348">**[1]**  Várjon, amíg az új drbd eszköz szinkronizált</span><span class="sxs-lookup"><span data-stu-id="143ee-348">**[1]** Wait until the new drbd devices are synchronized</span></span>

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

1. <span data-ttu-id="143ee-349">**[1]**  Fájlrendszerek létrehozása a drbd eszközökön</span><span class="sxs-lookup"><span data-stu-id="143ee-349">**[1]** Create file systems on the drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="143ee-350">Fürt keretrendszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-350">Configure Cluster Framework</span></span>

<span data-ttu-id="143ee-351">**[1]**  Az alapértelmezett beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="143ee-351">**[1]** Change the default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a><span data-ttu-id="143ee-352">SAP NetWeaver telepítésének előkészítése</span><span class="sxs-lookup"><span data-stu-id="143ee-352">Prepare for SAP NetWeaver installation</span></span>

1. <span data-ttu-id="143ee-353">**[A]**  a megosztott könyvtárak létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-353">**[A]** Create the shared directories</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. <span data-ttu-id="143ee-354">**[A]**  Autofs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-354">**[A]** Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="143ee-355">Fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-355">Create a file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   <span data-ttu-id="143ee-356">Indítsa újra az új fájlmegosztások csatlakoztatása autofs</span><span class="sxs-lookup"><span data-stu-id="143ee-356">Restart autofs to mount the new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="143ee-357">**[A]**  Fájl FELCSERÉLÉSE konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-357">**[A]** Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="143ee-358">Indítsa újra az ügynököt, a módosítás aktiválása</span><span class="sxs-lookup"><span data-stu-id="143ee-358">Restart the Agent to activate the change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a><span data-ttu-id="143ee-359">SAP NetWeaver ASC/SSZON telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-359">Installing SAP NetWeaver ASCS/ERS</span></span>

1. <span data-ttu-id="143ee-360">**[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi a belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="143ee-360">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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

   <span data-ttu-id="143ee-361">Győződjön meg arról, hogy a fürt állapota rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="143ee-361">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="143ee-362">Nem fontos, melyik csomópontján, az erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="143ee-362">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="143ee-363">**[1]**  SAP NetWeaver ASC telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-363">**[1]** Install SAP NetWeaver ASCS</span></span>  

   <span data-ttu-id="143ee-364">SAP NetWeaver ASC telepítse a legfelső szintű használatával egy virtuális állomásnevet, amely a terheléselosztó előtér-konfiguráció a ASC IP-címe például az első csomóponton <b>nws-ASC</b>, <b>10.0.0.10</b> és a példány számát, például használt a mintavétel a terheléselosztó <b>00</b>.</span><span class="sxs-lookup"><span data-stu-id="143ee-364">Install SAP NetWeaver ASCS as root on the first node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ASCS for example <b>nws-ascs</b>, <b>10.0.0.10</b> and the instance number that you used for the probe of the load balancer for example <b>00</b>.</span></span>

   <span data-ttu-id="143ee-365">Használhatja a sapinst paraméter SAPINST_REMOTE_ACCESS_USER sapinst való kapcsolódáshoz nem legfelső szintű felhasználó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="143ee-365">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="143ee-366">**[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi a belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="143ee-366">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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
   # Do you still want to commit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   <span data-ttu-id="143ee-367">Győződjön meg arról, hogy a fürt állapota rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="143ee-367">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="143ee-368">Nem fontos, melyik csomópontján, az erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="143ee-368">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="143ee-369">**[2]**  SAP NetWeaver SSZON telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-369">**[2]** Install SAP NetWeaver ERS</span></span>  

   <span data-ttu-id="143ee-370">SAP NetWeaver SSZON telepítése a második csomópont használatával egy virtuális állomásnevet, amely a terheléselosztó előtér-konfiguráció a SSZON IP-címe például a legfelső szintű <b>nws-sszon</b>, <b>10.0.0.11</b> és a példány számát, például használt a mintavétel a terheléselosztó <b>02</b>.</span><span class="sxs-lookup"><span data-stu-id="143ee-370">Install SAP NetWeaver ERS as root on the second node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ERS for example <b>nws-ers</b>, <b>10.0.0.11</b> and the instance number that you used for the probe of the load balancer for example <b>02</b>.</span></span>

   <span data-ttu-id="143ee-371">Használhatja a sapinst paraméter SAPINST_REMOTE_ACCESS_USER sapinst való kapcsolódáshoz nem legfelső szintű felhasználó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="143ee-371">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > <span data-ttu-id="143ee-372">SWPM SP 20 PL 05 vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="143ee-372">Please use SWPM SP 20 PL 05 or higher.</span></span> <span data-ttu-id="143ee-373">Alacsonyabb verzió nincs beállítva az engedélyeket, és a telepítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="143ee-373">Lower versions do not set the permissions correctly and the installation will fail.</span></span>
   > 

1. <span data-ttu-id="143ee-374">**[1]**  Adaptálása a ASC/SCS és SSZON példány profilok</span><span class="sxs-lookup"><span data-stu-id="143ee-374">**[1]** Adapt the ASCS/SCS and ERS instance profiles</span></span>
 
   * <span data-ttu-id="143ee-375">Asc/SCS profil</span><span class="sxs-lookup"><span data-stu-id="143ee-375">ASCS/SCS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * <span data-ttu-id="143ee-376">SSZON profil</span><span class="sxs-lookup"><span data-stu-id="143ee-376">ERS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. <span data-ttu-id="143ee-377">**[A]**  Keep-Alive konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-377">**[A]** Configure Keep Alive</span></span>

   <span data-ttu-id="143ee-378">Az SAP NetWeaver alkalmazáskiszolgáló és az ASC/SCS közötti kommunikáció áthalad szoftveres terheléselosztóként üzemeljen.</span><span class="sxs-lookup"><span data-stu-id="143ee-378">The communication between the SAP NetWeaver application server and the ASCS/SCS is routed through a software load balancer.</span></span> <span data-ttu-id="143ee-379">A load balancer inaktív kapcsolatok leválasztása után konfigurálható időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="143ee-379">The load balancer disconnects inactive connections after a configurable timeout.</span></span> <span data-ttu-id="143ee-380">Ennek megelőzése szüksége egy paramétert az SAP NetWeaver ASC/SCS profil és a Linux rendszer beállításainak módosítására.</span><span class="sxs-lookup"><span data-stu-id="143ee-380">To prevent this you need to set a parameter in the SAP NetWeaver ASCS/SCS profile and change the Linux system settings.</span></span> <span data-ttu-id="143ee-381">Kérjük, olvassa el [SAP Megjegyzés 1410736] [ 1410736] további információt.</span><span class="sxs-lookup"><span data-stu-id="143ee-381">Please read [SAP Note 1410736][1410736] for more information.</span></span>
   
   <span data-ttu-id="143ee-382">A ASC/SCS profil paraméter célzó/encni/set_so_keepalive már felvették az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="143ee-382">The ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in the last step.</span></span>

   <pre><code> 
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. <span data-ttu-id="143ee-383">**[A]**  Az SAP felhasználók konfigurálásához, a telepítés után</span><span class="sxs-lookup"><span data-stu-id="143ee-383">**[A]** Configure the SAP users after the installation</span></span>
 
   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. <span data-ttu-id="143ee-384">**[1]**  A ASC és SSZON SAP-szolgáltatás hozzáadása a sapservice fájlhoz</span><span class="sxs-lookup"><span data-stu-id="143ee-384">**[1]** Add the ASCS and ERS SAP services to the sapservice file</span></span>

   <span data-ttu-id="143ee-385">Adja hozzá a ASC bejegyzés a második csomópontra történő szolgáltatást, és másolja a SSZON szolgáltatás bejegyzés az első csomópontot.</span><span class="sxs-lookup"><span data-stu-id="143ee-385">Add the ASCS service entry to the second node and copy the ERS service entry to the first node.</span></span>

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. <span data-ttu-id="143ee-386">**[1]**  Az SAP fürterőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-386">**[1]** Create the SAP cluster resources</span></span>

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

   <span data-ttu-id="143ee-387">Győződjön meg arról, hogy a fürt állapota rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="143ee-387">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="143ee-388">Nem fontos, melyik csomópontján, az erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="143ee-388">It is not important on which node the resources are running.</span></span>

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

### <a name="create-stonith-device"></a><span data-ttu-id="143ee-389">STONITH eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-389">Create STONITH device</span></span>

<span data-ttu-id="143ee-390">A STONITH eszköz egy egyszerű szolgáltatást használ, szemben a Microsoft Azure engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="143ee-390">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="143ee-391">Kövesse az alábbi lépéseket egy egyszerű szolgáltatásnév létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="143ee-391">Follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="143ee-392">Ugrás a <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="143ee-392">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="143ee-393">Nyissa meg az Azure Active Directory panelt</span><span class="sxs-lookup"><span data-stu-id="143ee-393">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="143ee-394">Nyissa meg tulajdonságait, és jegyezze fel a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="143ee-394">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="143ee-395">Ez a **bérlőazonosító**.</span><span class="sxs-lookup"><span data-stu-id="143ee-395">This is the **tenant id**.</span></span>
1. <span data-ttu-id="143ee-396">Kattintson az alkalmazás-regisztráció</span><span class="sxs-lookup"><span data-stu-id="143ee-396">Click App registrations</span></span>
1. <span data-ttu-id="143ee-397">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="143ee-397">Click Add</span></span>
1. <span data-ttu-id="143ee-398">Adjon meg egy nevet, válassza ki a "Web app/API" alkalmazástípus, adja meg a bejelentkezési URL-címet (például http://localhost) és kattintson a Létrehozás gombra</span><span class="sxs-lookup"><span data-stu-id="143ee-398">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="143ee-399">A bejelentkezési URL-címet nem használja, és bármilyen érvényes URL-CÍMEK lehetnek</span><span class="sxs-lookup"><span data-stu-id="143ee-399">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="143ee-400">Válassza ki az új alkalmazást, és a beállítások lapon kattintson a kulcsok</span><span class="sxs-lookup"><span data-stu-id="143ee-400">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="143ee-401">Adja meg egy új kulcs leírását, válassza a "Soha nem jár le", és kattintson a Mentés gombra</span><span class="sxs-lookup"><span data-stu-id="143ee-401">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="143ee-402">Jegyezze fel az értéket.</span><span class="sxs-lookup"><span data-stu-id="143ee-402">Write down the Value.</span></span> <span data-ttu-id="143ee-403">Használják a **jelszó** a szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="143ee-403">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="143ee-404">Jegyezze fel az azonosítót.</span><span class="sxs-lookup"><span data-stu-id="143ee-404">Write down the Application Id.</span></span> <span data-ttu-id="143ee-405">A felhasználónév használják (**bejelentkezési azonosító** az alábbi lépéseket a) a szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="143ee-405">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="143ee-406">A szolgáltatás egyszerű nincs engedélye a alapértelmezés szerint az Azure-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="143ee-406">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="143ee-407">Hozzá kell rendelnie a szolgáltatás egyszerű engedélyek indítása és leállítása (felszabadítása) a fürt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="143ee-407">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="143ee-408">Ugrás a https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="143ee-408">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="143ee-409">Nyissa meg az összes erőforrás panel</span><span class="sxs-lookup"><span data-stu-id="143ee-409">Open the All resources blade</span></span>
1. <span data-ttu-id="143ee-410">Válassza ki a virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="143ee-410">Select the virtual machine</span></span>
1. <span data-ttu-id="143ee-411">Kattintson a hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="143ee-411">Click Access control (IAM)</span></span>
1. <span data-ttu-id="143ee-412">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="143ee-412">Click Add</span></span>
1. <span data-ttu-id="143ee-413">Válassza ki a szerepkör tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="143ee-413">Select the role Owner</span></span>
1. <span data-ttu-id="143ee-414">Adja meg az előbb létrehozott alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="143ee-414">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="143ee-415">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="143ee-415">Click OK</span></span>

#### <a name="1-create-the-stonith-devices"></a><span data-ttu-id="143ee-416">**[1]**  STONITH eszközök létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-416">**[1]** Create the STONITH devices</span></span>

<span data-ttu-id="143ee-417">Után szerkeszteni a virtuális gépek engedélyeit, beállíthatja a STONITH eszközök a fürtben.</span><span class="sxs-lookup"><span data-stu-id="143ee-417">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a><span data-ttu-id="143ee-418">**[1]**  STONITH eszköz használatának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-418">**[1]** Enable the use of a STONITH device</span></span>

<span data-ttu-id="143ee-419">Egy STONITH eszköz használatának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="143ee-419">Enable the use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a><span data-ttu-id="143ee-420">A telepítési adatbázis</span><span class="sxs-lookup"><span data-stu-id="143ee-420">Install database</span></span>

<span data-ttu-id="143ee-421">Ebben a példában egy SAP HANA replikációs telepítve és konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="143ee-421">In this example an SAP HANA System Replication is installed and configured.</span></span> <span data-ttu-id="143ee-422">A fürtön, amelyen az SAP NetWeaver ASC/SCS és SSZON SAP HANA fog futni.</span><span class="sxs-lookup"><span data-stu-id="143ee-422">SAP HANA will run in the same cluster as the SAP NetWeaver ASCS/SCS and ERS.</span></span> <span data-ttu-id="143ee-423">SAP HANA egy dedikált fürtön is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="143ee-423">You can also install SAP HANA on a dedicated cluster.</span></span> <span data-ttu-id="143ee-424">Lásd: [magas rendelkezésre állás az SAP HANA Azure virtuális gépek (VM)] [ sap-hana-ha] további információt.</span><span class="sxs-lookup"><span data-stu-id="143ee-424">See [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha] for more information.</span></span>

### <a name="prepare-for-sap-hana-installation"></a><span data-ttu-id="143ee-425">SAP HANA-telepítés előkészítése</span><span class="sxs-lookup"><span data-stu-id="143ee-425">Prepare for SAP HANA installation</span></span>

<span data-ttu-id="143ee-426">Általában javasoljuk LVM kötetek, amelyek adatokat tárolhatnak, és a naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="143ee-426">We generally recommend using LVM for volumes that store data and log files.</span></span> <span data-ttu-id="143ee-427">Tesztelési célokra is beállíthatja az adatok tárolásához és a naplófájl közvetlenül egy normál lemezen.</span><span class="sxs-lookup"><span data-stu-id="143ee-427">For testing purposes, you can also choose to store the data and log file directly on a plain disk.</span></span>

1. <span data-ttu-id="143ee-428">**[A]**  LVM</span><span class="sxs-lookup"><span data-stu-id="143ee-428">**[A]** LVM</span></span>  
   <span data-ttu-id="143ee-429">Az alábbi példa azt feltételezi, hogy, hogy a virtuális gépek rendelkeznek négy adatlemezt csatolni, amelynek használatával hozzon létre két köteteket.</span><span class="sxs-lookup"><span data-stu-id="143ee-429">The example below assumes that the virtual machines have four data disks attached that should be used to create two volumes.</span></span>
   
   <span data-ttu-id="143ee-430">A használni kívánt összes lemez fizikai köteteket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="143ee-430">Create physical volumes for all disks that you want to use.</span></span>
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   <span data-ttu-id="143ee-431">Az adatfájlok kötet csoport, a naplófájlok egy kötet csoport és egy SAP HANA a megosztott könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-431">Create a volume group for the data files, one volume group for the log files and one for the shared directory of SAP HANA</span></span>
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   <span data-ttu-id="143ee-432">A logikai köteteket hozhat létre</span><span class="sxs-lookup"><span data-stu-id="143ee-432">Create the logical volumes</span></span>
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   <span data-ttu-id="143ee-433">A csatlakoztatási könyvtárak létrehozása, és másolja az összes logikai kötet UUID</span><span class="sxs-lookup"><span data-stu-id="143ee-433">Create the mount directories and copy the UUID of all logical volumes</span></span>
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   <span data-ttu-id="143ee-434">A három logikai kötetek autofs bejegyzéseket létrehozni</span><span class="sxs-lookup"><span data-stu-id="143ee-434">Create autofs entries for the three logical volumes</span></span>
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   <span data-ttu-id="143ee-435">Ez a sudo vi /etc/auto.direct sor beszúrása</span><span class="sxs-lookup"><span data-stu-id="143ee-435">Insert this line to sudo vi /etc/auto.direct</span></span>
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   <span data-ttu-id="143ee-436">Csatlakoztassa az új köteteket</span><span class="sxs-lookup"><span data-stu-id="143ee-436">Mount the new volumes</span></span>
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. <span data-ttu-id="143ee-437">**[A]**  Egyszerű lemez</span><span class="sxs-lookup"><span data-stu-id="143ee-437">**[A]** Plain Disks</span></span>  

   <span data-ttu-id="143ee-438">A kis vagy bemutató rendszerek, elhelyezhet egy lemezt a HANA adatainak és naplókönyvtárainak fájlokat.</span><span class="sxs-lookup"><span data-stu-id="143ee-438">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="143ee-439">A következő parancsok /dev/sdc hozza létre a partíciót, és formázza xfs.</span><span class="sxs-lookup"><span data-stu-id="143ee-439">The following commands create a partition on /dev/sdc and format it with xfs.</span></span>
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down the id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   <span data-ttu-id="143ee-440">Ez a /etc/auto.direct sor beszúrása</span><span class="sxs-lookup"><span data-stu-id="143ee-440">Insert this line to /etc/auto.direct</span></span>
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   <span data-ttu-id="143ee-441">A célkönyvtár létrehozása, és csatlakoztassa a lemezt.</span><span class="sxs-lookup"><span data-stu-id="143ee-441">Create the target directory and mount the disk.</span></span>
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a><span data-ttu-id="143ee-442">SAP HANA telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-442">Installing SAP HANA</span></span>

<span data-ttu-id="143ee-443">Az alábbi lépéseket a fejezete 4 alapulnak a [SAP HANA SR teljesítmény optimalizált forgatókönyv útmutató] [ suse-hana-ha-guide] SAP HANA rendszer replikáció telepítését.</span><span class="sxs-lookup"><span data-stu-id="143ee-443">The following steps are based on chapter 4 of the [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] to install SAP HANA System Replication.</span></span> <span data-ttu-id="143ee-444">Olvassa el, mielőtt folytatja a telepítést.</span><span class="sxs-lookup"><span data-stu-id="143ee-444">Please read it before you continue the installation.</span></span>

1. <span data-ttu-id="143ee-445">**[A]**  Hdblcm HANA DVD-ről futtatni</span><span class="sxs-lookup"><span data-stu-id="143ee-445">**[A]** Run hdblcm from the HANA DVD</span></span>
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. <span data-ttu-id="143ee-446">**[A]**  SAP állomás ügynökök frissítése</span><span class="sxs-lookup"><span data-stu-id="143ee-446">**[A]** Upgrade SAP Host Agent</span></span>

   <span data-ttu-id="143ee-447">Töltse le a legfrissebb SAP a gazdagép ügynöke archív a a [SAP Softwarecenter] [ sap-swcenter] és az ügynökök frissítése a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="143ee-447">Download the latest SAP Host Agent archive from the [SAP Softwarecenter][sap-swcenter] and run the following command to upgrade the agent.</span></span> <span data-ttu-id="143ee-448">Cserélje le az archívum mutasson a letöltött fájl elérési útja.</span><span class="sxs-lookup"><span data-stu-id="143ee-448">Replace the path to the archive to point to the file you downloaded.</span></span>
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path to SAP Host Agent SAR&gt;</b> 
   </code></pre>

1. <span data-ttu-id="143ee-449">**[1]**  Létrehozása HANA replikációs (rendszergazdaként)</span><span class="sxs-lookup"><span data-stu-id="143ee-449">**[1]** Create HANA replication (as root)</span></span>  

   <span data-ttu-id="143ee-450">A következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="143ee-450">Run the following command.</span></span> <span data-ttu-id="143ee-451">Ügyeljen arra, hogy félkövér karakterláncok (HANA rendszer azonosító HDB és példány újrahasznosítása 03) cserélje le az értékeket a SAP HANA-telepítés.</span><span class="sxs-lookup"><span data-stu-id="143ee-451">Make sure to replace bold strings (HANA System ID HDB and instance number 03) with the values of your SAP HANA installation.</span></span>
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. <span data-ttu-id="143ee-452">**[A]**  Keystore bejegyzés (rendszergazdaként) létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-452">**[A]** Create keystore entry (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. <span data-ttu-id="143ee-453">**[1]**  Adatbázis biztonsági másolata (rendszergazdaként)</span><span class="sxs-lookup"><span data-stu-id="143ee-453">**[1]** Backup database (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. <span data-ttu-id="143ee-454">**[1]**  Váltani a HANA sapsid felhasználó és az elsődleges hely létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="143ee-454">**[1]** Switch to the HANA sapsid user and create the primary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. <span data-ttu-id="143ee-455">**[2]**  Váltani a HANA sapsid felhasználó és a másodlagos hely létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="143ee-455">**[2]** Switch to the HANA sapsid user and create the secondary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. <span data-ttu-id="143ee-456">**[1]**  Fürterőforrások SAP HANA létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-456">**[1]** Create SAP HANA cluster resources</span></span>

   <span data-ttu-id="143ee-457">Először hozza létre a topológiát.</span><span class="sxs-lookup"><span data-stu-id="143ee-457">First, create the topology.</span></span>
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number and HANA system id
   
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
   
   <span data-ttu-id="143ee-458">Ezután hozzon létre a HANA erőforrások</span><span class="sxs-lookup"><span data-stu-id="143ee-458">Next, create the HANA resources</span></span>
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
    
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

   <span data-ttu-id="143ee-459">Győződjön meg arról, hogy a fürt állapota rendben, és, hogy az összes erőforrás indulnak el.</span><span class="sxs-lookup"><span data-stu-id="143ee-459">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="143ee-460">Nem fontos, melyik csomópontján, az erőforrások futnak.</span><span class="sxs-lookup"><span data-stu-id="143ee-460">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="143ee-461">**[1]**  Az SAP NetWeaver adatbázis-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-461">**[1]** Install the SAP NetWeaver database instance</span></span>

   <span data-ttu-id="143ee-462">A SAP NetWeaver adatbázis-példány telepítését a legfelső szintű használatával egy virtuális állomásnevet, amely a terheléselosztó előtér-konfiguráció az adatbázis IP-címe például <b>nws-db</b> és <b>10.0.0.12</b>.</span><span class="sxs-lookup"><span data-stu-id="143ee-462">Install the SAP NetWeaver database instance as root using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the database for example <b>nws-db</b> and <b>10.0.0.12</b>.</span></span>

   <span data-ttu-id="143ee-463">Használhatja a sapinst paraméter SAPINST_REMOTE_ACCESS_USER sapinst való kapcsolódáshoz nem legfelső szintű felhasználó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="143ee-463">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a><span data-ttu-id="143ee-464">SAP NetWeaver alkalmazáskiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-464">SAP NetWeaver application server installation</span></span>

<span data-ttu-id="143ee-465">Kövesse az alábbi lépéseket az SAP-alkalmazáskiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="143ee-465">Follow these steps to install an SAP application server.</span></span> <span data-ttu-id="143ee-466">A lépések alábbi azt feltételezik, hogy az alkalmazáskiszolgáló egy kiszolgálón telepíti a ASC/SCS és HANA-kiszolgálókról különböző.</span><span class="sxs-lookup"><span data-stu-id="143ee-466">The steps bellow assume that you install the application server on a server different from the ASCS/SCS and HANA servers.</span></span> <span data-ttu-id="143ee-467">Ellenkező esetben egyes (például a gazdagép névfeloldásához konfigurálása) az alábbi lépéseket nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="143ee-467">Otherwise some of the steps below (like configuring host name resolution) are not needed.</span></span>

1. <span data-ttu-id="143ee-468">A telepítő állomásnév</span><span class="sxs-lookup"><span data-stu-id="143ee-468">Setup host name resolution</span></span>    
   <span data-ttu-id="143ee-469">DNS-kiszolgálót használjon, vagy módosítsa az/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="143ee-469">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="143ee-470">Ez a példa bemutatja, hogyan használható az/etc/hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="143ee-470">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="143ee-471">Cserélje le az IP-cím és a következő parancsokat az állomásnév</span><span class="sxs-lookup"><span data-stu-id="143ee-471">Replace the IP address and the hostname in the following commands</span></span>
   ```bash
   sudo vi /etc/hosts
   ```
   <span data-ttu-id="143ee-472">Helyezze be a következő sorokat/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="143ee-472">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="143ee-473">Az IP-cím és a környezet megfelelő állomásnév módosítása</span><span class="sxs-lookup"><span data-stu-id="143ee-473">Change the IP address and hostname to match your environment</span></span>    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of the application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. <span data-ttu-id="143ee-474">A sapmnt könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-474">Create the sapmnt directory</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. <span data-ttu-id="143ee-475">Autofs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-475">Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="143ee-476">Új fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="143ee-476">Create a new file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   <span data-ttu-id="143ee-477">Indítsa újra az új fájlmegosztások csatlakoztatása autofs</span><span class="sxs-lookup"><span data-stu-id="143ee-477">Restart autofs to mount the new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="143ee-478">Lapozófájl konfigurálása</span><span class="sxs-lookup"><span data-stu-id="143ee-478">Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="143ee-479">Indítsa újra az ügynököt, a módosítás aktiválása</span><span class="sxs-lookup"><span data-stu-id="143ee-479">Restart the Agent to activate the change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. <span data-ttu-id="143ee-480">SAP NetWeaver alkalmazáskiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="143ee-480">Install SAP NetWeaver application server</span></span>

   <span data-ttu-id="143ee-481">Elsődleges vagy további SAP NetWeaver alkalmazások kiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="143ee-481">Install a primary or additional SAP NetWeaver applications server.</span></span>

   <span data-ttu-id="143ee-482">Használhatja a sapinst paraméter SAPINST_REMOTE_ACCESS_USER sapinst való kapcsolódáshoz nem legfelső szintű felhasználó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="143ee-482">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="143ee-483">Biztonságos tár SAP HANA frissítése</span><span class="sxs-lookup"><span data-stu-id="143ee-483">Update SAP HANA secure store</span></span>

   <span data-ttu-id="143ee-484">Frissítse a SAP HANA biztonságos tár úgy, hogy a virtuális nevét a replikációs SAP HANA-telepítő mutasson.</span><span class="sxs-lookup"><span data-stu-id="143ee-484">Update the SAP HANA secure store to point to the virtual name of the SAP HANA System Replication setup.</span></span>
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a><span data-ttu-id="143ee-485">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="143ee-485">Next steps</span></span>
* <span data-ttu-id="143ee-486">[Az Azure virtuális gépek tervezési és megvalósítási az SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="143ee-486">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="143ee-487">[Az SAP Azure virtuális gépek telepítése][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="143ee-487">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="143ee-488">[Az SAP Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="143ee-488">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="143ee-489">Magas rendelkezésre állás és az Azure (nagy példány) az SAP HANA vész-helyreállítási terv létrehozásához, lásd: [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="143ee-489">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span>
* <span data-ttu-id="143ee-490">Magas rendelkezésre állás és az Azure virtuális gépeken az SAP HANA vész-helyreállítási terv létrehozásához, lásd: [magas rendelkezésre állás az SAP HANA Azure virtuális gépek (VM)][sap-hana-ha]</span><span class="sxs-lookup"><span data-stu-id="143ee-490">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]</span></span>