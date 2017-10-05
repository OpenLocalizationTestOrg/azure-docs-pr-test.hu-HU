---
title: "Az Azure virtuális gépek (VM) SAP HANA magas rendelkezésre állású |} Microsoft Docs"
description: "Magas rendelkezésre állású SAP HANA az Azure virtuális gépek (VM) létrehozásához."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: 951150e621d21037b0adde7287b9f985290d8d11
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a><span data-ttu-id="ae76a-103">Magas rendelkezésre állású SAP HANA az Azure virtuális gépek (VM)</span><span class="sxs-lookup"><span data-stu-id="ae76a-103">High Availability of SAP HANA on Azure Virtual Machines (VMs)</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

<span data-ttu-id="ae76a-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span><span class="sxs-lookup"><span data-stu-id="ae76a-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span></span>
<span data-ttu-id="ae76a-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span><span class="sxs-lookup"><span data-stu-id="ae76a-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span></span>
<span data-ttu-id="ae76a-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="ae76a-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
<span data-ttu-id="ae76a-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="ae76a-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
<span data-ttu-id="ae76a-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="ae76a-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="ae76a-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span><span class="sxs-lookup"><span data-stu-id="ae76a-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span></span>
<span data-ttu-id="ae76a-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="ae76a-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>
<span data-ttu-id="ae76a-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span><span class="sxs-lookup"><span data-stu-id="ae76a-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span></span>
<span data-ttu-id="ae76a-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="ae76a-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

<span data-ttu-id="ae76a-113">A helyszíni vagy HANA replikációs használhatja, vagy használja a megosztott tárolót SAP HANA a magas rendelkezésre állás érdekében.</span><span class="sxs-lookup"><span data-stu-id="ae76a-113">On-premises, you can use either HANA System Replication or use shared storage to establish high availability for SAP HANA.</span></span>
<span data-ttu-id="ae76a-114">Sajnos jelenleg csak a támogatott HANA replikációs beállítása az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="ae76a-114">We currently only support setting up HANA System Replication on Azure.</span></span> <span data-ttu-id="ae76a-115">SAP HANA-replikáció egy főcsomópont és legalább egy alárendelt csomópont áll.</span><span class="sxs-lookup"><span data-stu-id="ae76a-115">SAP HANA Replication consists of one master node and at least one slave node.</span></span> <span data-ttu-id="ae76a-116">A fő csomóponton levő adatok módosításait replikálva vannak a alárendelt csomópontok szinkron vagy aszinkron módon.</span><span class="sxs-lookup"><span data-stu-id="ae76a-116">Changes to the data on the master node are replicated to the slave nodes synchronously or asynchronously.</span></span>

<span data-ttu-id="ae76a-117">A cikkből megtudhatja, hogyan telepítse a virtuális gépeket, a virtuális gépet állíthat be, a fürt-keretrendszer telepítése, telepítése és konfigurálása a SAP HANA replikációs.</span><span class="sxs-lookup"><span data-stu-id="ae76a-117">This article describes how to deploy the virtual machines, configure the virtual machines, install the cluster framework, install and configure SAP HANA System Replication.</span></span>
<span data-ttu-id="ae76a-118">A példa konfigurációkban telepítési parancsok 03 stb. száma, és HANA rendszer azonosító HDB használatos.</span><span class="sxs-lookup"><span data-stu-id="ae76a-118">In the example configurations, installation commands etc. instance number 03 and HANA System ID HDB is used.</span></span>

<span data-ttu-id="ae76a-119">Olvassa el a következő SAP megjegyzések és által írt cikkeket először</span><span class="sxs-lookup"><span data-stu-id="ae76a-119">Read the following SAP Notes and papers first</span></span>

* <span data-ttu-id="ae76a-120">SAP Megjegyzés [1928533], amelynek van:</span><span class="sxs-lookup"><span data-stu-id="ae76a-120">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="ae76a-121">Az SAP szoftver központi telepítése támogatott Azure Virtuálisgép-méretek listáját</span><span class="sxs-lookup"><span data-stu-id="ae76a-121">List of Azure VM sizes that are supported for the deployment of SAP software</span></span>
  * <span data-ttu-id="ae76a-122">Az Azure Virtuálisgép-méretek fontos készletkapacitás információival</span><span class="sxs-lookup"><span data-stu-id="ae76a-122">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="ae76a-123">Támogatott SAP szoftver, és az operációs rendszer és az adatbázis kombinációját</span><span class="sxs-lookup"><span data-stu-id="ae76a-123">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="ae76a-124">A Windows és a Microsoft Azure Linux szükséges SAP kernel verziója</span><span class="sxs-lookup"><span data-stu-id="ae76a-124">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>
* <span data-ttu-id="ae76a-125">SAP Megjegyzés [2015553] a SAP-támogatott SAP szoftverek központi telepítése az Azure-ban szükséges előfeltételeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ae76a-125">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="ae76a-126">SAP Megjegyzés [2205917] javasolt a SUSE Linux Enterprise Server operációs rendszer beállításait az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="ae76a-126">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="ae76a-127">SAP Megjegyzés [1944799] SUSE Linux Enterprise Server SAP HANA-irányelvek rendelkezik az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="ae76a-127">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="ae76a-128">SAP Megjegyzés [2178632] tartalmaz részletes információkat az Azure-ban SAP jelentett összes figyelési metrikákat.</span><span class="sxs-lookup"><span data-stu-id="ae76a-128">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="ae76a-129">SAP Megjegyzés [2191498] vannak a szükséges SAP gazdagép-ügynök verziója Linux az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ae76a-129">SAP Note [2191498] has the required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="ae76a-130">SAP Megjegyzés [2243692] SAP Azure Linux licenceléssel kapcsolatos információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ae76a-130">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="ae76a-131">SAP Megjegyzés [1984787] SUSE Linux Enterprise Server 12 vonatkozó általános információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ae76a-131">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="ae76a-132">SAP Megjegyzés [1999351] további információkat talál az Azure fokozott Figyelőbővítmény az SAP rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ae76a-132">SAP Note [1999351] has additional troubleshooting information for the Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="ae76a-133">[SAP közösségi WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) rendelkezik az összes szükséges SAP megjegyzések Linux.</span><span class="sxs-lookup"><span data-stu-id="ae76a-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="ae76a-134">[Azure virtuális gépek tervezési és megvalósítási az SAP Linux rendszeren][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="ae76a-134">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="ae76a-135">[Az Azure virtuális gépek telepítése az SAP, Linux (Ez a cikk)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="ae76a-135">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="ae76a-136">[Az SAP Linux Azure virtuális gépek DBMS-telepítés][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="ae76a-136">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="ae76a-137">[SAP HANA SR teljesítmény optimalizált forgatókönyv] [ suse-hana-ha-guide] az útmutató helyszíni SAP HANA replikációs beállítása az összes szükséges információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ae76a-137">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide] The guide contains all required information to set up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="ae76a-138">Ez az útmutató használja kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="ae76a-138">Use this guide as a baseline.</span></span>

## <a name="deploying-linux"></a><span data-ttu-id="ae76a-139">Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="ae76a-139">Deploying Linux</span></span>

<span data-ttu-id="ae76a-140">Az erőforrás-ügynök SAP Hana SUSE Linux Enterprise Server szerepel az SAP-alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="ae76a-140">The resource agent for SAP HANA is included in SUSE Linux Enterprise Server for SAP Applications.</span></span>
<span data-ttu-id="ae76a-141">Az Azure piactéren SUSE Linux Enterprise Server SAP alkalmazások 12 a saját (Bring Your saját előfizetés), amely segítségével új virtuális gépek telepítése a kép tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ae76a-141">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 with BYOS (Bring Your Own Subscription) that you can use to deploy new virtual machines.</span></span>

### <a name="manual-deployment"></a><span data-ttu-id="ae76a-142">Manuális telepítése</span><span class="sxs-lookup"><span data-stu-id="ae76a-142">Manual Deployment</span></span>

1. <span data-ttu-id="ae76a-143">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-143">Create a Resource Group</span></span>
1. <span data-ttu-id="ae76a-144">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-144">Create a Virtual Network</span></span>
1. <span data-ttu-id="ae76a-145">Két Storage-fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-145">Create two Storage Accounts</span></span>
1. <span data-ttu-id="ae76a-146">Egy rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-146">Create an Availability Set</span></span>  
   <span data-ttu-id="ae76a-147">Készlet maximális frissítési tartomány</span><span class="sxs-lookup"><span data-stu-id="ae76a-147">Set max update domain</span></span>
1. <span data-ttu-id="ae76a-148">Hozzon létre egy terhelés-kiegyenlítő (belső)</span><span class="sxs-lookup"><span data-stu-id="ae76a-148">Create a Load Balancer (internal)</span></span>  
   <span data-ttu-id="ae76a-149">Válassza ki a fenti lépést a virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="ae76a-149">Select VNET of step above</span></span>
1. <span data-ttu-id="ae76a-150">1 virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-150">Create Virtual Machine 1</span></span>  
   <span data-ttu-id="ae76a-151">https://Portal.Azure.com/#Create/SUSE-byos.sles-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="ae76a-151">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="ae76a-152">SLES SAP alkalmazások 12 SP1 (saját)</span><span class="sxs-lookup"><span data-stu-id="ae76a-152">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="ae76a-153">Válassza ki a Tárfiók 1</span><span class="sxs-lookup"><span data-stu-id="ae76a-153">Select Storage Account 1</span></span>  
   <span data-ttu-id="ae76a-154">A rendelkezésre állási csoport kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ae76a-154">Select Availability Set</span></span>  
1. <span data-ttu-id="ae76a-155">2. virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-155">Create Virtual Machine 2</span></span>  
   <span data-ttu-id="ae76a-156">https://Portal.Azure.com/#Create/SUSE-byos.sles-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="ae76a-156">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="ae76a-157">SLES SAP alkalmazások 12 SP1 (saját)</span><span class="sxs-lookup"><span data-stu-id="ae76a-157">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="ae76a-158">Válassza ki a Tárfiók 2</span><span class="sxs-lookup"><span data-stu-id="ae76a-158">Select Storage Account 2</span></span>   
   <span data-ttu-id="ae76a-159">A rendelkezésre állási csoport kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ae76a-159">Select Availability Set</span></span>  
1. <span data-ttu-id="ae76a-160">Adatlemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ae76a-160">Add Data Disks</span></span>
1. <span data-ttu-id="ae76a-161">A load balancer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae76a-161">Configure the load balancer</span></span>
    1. <span data-ttu-id="ae76a-162">Előtérbeli IP-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-162">Create a frontend IP pool</span></span>
        1. <span data-ttu-id="ae76a-163">Nyissa meg a terheléselosztó előtérbeli IP-készlet között, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-163">Open the load balancer, select frontend IP pool and click Add</span></span>
        1. <span data-ttu-id="ae76a-164">Adja meg a nevét, az új előtérbeli IP-címtartomány (például hana-előtér)</span><span class="sxs-lookup"><span data-stu-id="ae76a-164">Enter the name of the new frontend IP pool (for example hana-frontend)</span></span>
       1. <span data-ttu-id="ae76a-165">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-165">Click OK</span></span>
        1. <span data-ttu-id="ae76a-166">Az új előtérbeli IP-címkészlet létrehozása után jegyezze fel az IP-címe</span><span class="sxs-lookup"><span data-stu-id="ae76a-166">After the new frontend IP pool is created, write down its IP address</span></span>
    1. <span data-ttu-id="ae76a-167">Háttér-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-167">Create a backend pool</span></span>
        1. <span data-ttu-id="ae76a-168">Nyissa meg a terheléselosztóhoz, válassza ki a háttérkészlet, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-168">Open the load balancer, select backend pools and click Add</span></span>
        1. <span data-ttu-id="ae76a-169">Adja meg az új háttérkészlet (például hana-háttérrendszer)</span><span class="sxs-lookup"><span data-stu-id="ae76a-169">Enter the name of the new backend pool (for example hana-backend)</span></span>
        1. <span data-ttu-id="ae76a-170">Kattintson a Hozzáadás gombra a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="ae76a-170">Click Add a virtual machine</span></span>
        1. <span data-ttu-id="ae76a-171">Válassza ki a korábban létrehozott rendelkezésre állási csoport</span><span class="sxs-lookup"><span data-stu-id="ae76a-171">Select the Availability Set you created earlier</span></span>
        1. <span data-ttu-id="ae76a-172">Válassza ki a virtuális gépeket, a SAP HANA-fürt</span><span class="sxs-lookup"><span data-stu-id="ae76a-172">Select the virtual machines of the SAP HANA cluster</span></span>
        1. <span data-ttu-id="ae76a-173">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-173">Click OK</span></span>
    1. <span data-ttu-id="ae76a-174">Hozzon létre egy állapotmintáihoz</span><span class="sxs-lookup"><span data-stu-id="ae76a-174">Create a health probe</span></span>
       1. <span data-ttu-id="ae76a-175">Nyissa meg a terheléselosztóhoz, válassza ki a állapotteljesítmény, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-175">Open the load balancer, select health probes and click Add</span></span>
        1. <span data-ttu-id="ae76a-176">Adja meg az új állapotmintáihoz (például hana-hp)</span><span class="sxs-lookup"><span data-stu-id="ae76a-176">Enter the name of the new health probe (for example hana-hp)</span></span>
        1. <span data-ttu-id="ae76a-177">Válassza ki a TCP protokoll, port 625**03**, időköz 5 és sérült küszöbérték 2</span><span class="sxs-lookup"><span data-stu-id="ae76a-177">Select TCP as protocol, port 625**03**, keep Interval 5 and Unhealthy threshold 2</span></span>
        1. <span data-ttu-id="ae76a-178">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-178">Click OK</span></span>
    1. <span data-ttu-id="ae76a-179">Terheléselosztási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-179">Create load balancing rules</span></span>
        1. <span data-ttu-id="ae76a-180">Nyissa meg a terheléselosztóhoz, válassza ki a terheléselosztási szabályok, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-180">Open the load balancer, select load balancing rules and click Add</span></span>
        1. <span data-ttu-id="ae76a-181">Adja meg az új terheléselosztási szabály nevét (például hana-lb-3**03**15)</span><span class="sxs-lookup"><span data-stu-id="ae76a-181">Enter the name of the new load balancer rule (for example hana-lb-3**03**15)</span></span>
        1. <span data-ttu-id="ae76a-182">Válassza ki az előtérbeli IP-cím, a háttérkészlet és állapotfigyelő mintavételi, korábban létrehozott (például hana-előtér)</span><span class="sxs-lookup"><span data-stu-id="ae76a-182">Select the frontend IP address, backend pool and health probe you created earlier (for example hana-frontend)</span></span>
        1. <span data-ttu-id="ae76a-183">Tartsa a TCP protokoll, adja meg azt a portot 3**03**15</span><span class="sxs-lookup"><span data-stu-id="ae76a-183">Keep protocol TCP, enter port 3**03**15</span></span>
        1. <span data-ttu-id="ae76a-184">Növelje a 30 perc üresjárati időtúllépés</span><span class="sxs-lookup"><span data-stu-id="ae76a-184">Increase idle timeout to 30 minutes</span></span>
       1. <span data-ttu-id="ae76a-185">**Ügyeljen arra, hogy a fix IP-Címek engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="ae76a-185">**Make sure to enable Floating IP**</span></span>
        1. <span data-ttu-id="ae76a-186">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-186">Click OK</span></span>
        1. <span data-ttu-id="ae76a-187">Ismételje meg a fenti port 3**03**17</span><span class="sxs-lookup"><span data-stu-id="ae76a-187">Repeat the steps above for port 3**03**17</span></span>

### <a name="deploy-with-template"></a><span data-ttu-id="ae76a-188">Üzembe helyezés sablon használatával</span><span class="sxs-lookup"><span data-stu-id="ae76a-188">Deploy with template</span></span>
<span data-ttu-id="ae76a-189">Segítségével a gyors üzembe helyezési sablonok valamelyikét a githubon központi telepítése az összes szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ae76a-189">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="ae76a-190">A sablon telepíti, a virtuális gépek, a terheléselosztó hasonló adataival, a rendelkezésre állási csoport stb. Kövesse az alábbi lépéseket a sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="ae76a-190">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="ae76a-191">Nyissa meg a [adatbázis sablon] [ template-multisid-db] vagy a [sablon összevont] [ template-converged] az Azure portálon található az adatbázis-sablon csak a terheléselosztási szabály létrehozása adatbázis mivel átszervezett is létrejön a terheléselosztási szabályok ASC/SCS és SSZON (csak Linux) példány.</span><span class="sxs-lookup"><span data-stu-id="ae76a-191">Open the [database template][template-multisid-db] or the [converged template][template-converged] on the Azure Portal The database template only creates the load-balancing rules for a database whereas the converged template also creates the load-balancing rules for an ASCS/SCS and ERS (Linux only) instance.</span></span> <span data-ttu-id="ae76a-192">Ha azt tervezi, hogy az SAP NetWeaver alapú rendszert telepíti, és szeretné telepíteni az ASC/SCS-példányt az azonos gépeken, használja a [sablon összevont][template-converged].</span><span class="sxs-lookup"><span data-stu-id="ae76a-192">If you plan to install an SAP NetWeaver based system and you also want to install the ASCS/SCS instance on the same machines, use the [converged template][template-converged].</span></span>
1. <span data-ttu-id="ae76a-193">Adja meg a következő paraméterek</span><span class="sxs-lookup"><span data-stu-id="ae76a-193">Enter the following parameters</span></span>
    1. <span data-ttu-id="ae76a-194">SAP azonosító</span><span class="sxs-lookup"><span data-stu-id="ae76a-194">Sap System Id</span></span>  
       <span data-ttu-id="ae76a-195">Adja meg a telepíteni kívánt SAP rendszer SAP rendszer azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ae76a-195">Enter the SAP system Id of the SAP system you want to install.</span></span> <span data-ttu-id="ae76a-196">Az azonosító az üzembe helyezett erőforrások előtagjaként lesz használható.</span><span class="sxs-lookup"><span data-stu-id="ae76a-196">The Id will be used as a prefix for the resources that are deployed.</span></span>
    1. <span data-ttu-id="ae76a-197">A készlet típusa (csak akkor érvényes, az összevont sablon)</span><span class="sxs-lookup"><span data-stu-id="ae76a-197">Stack Type (only applicable if you use the converged template)</span></span>  
       <span data-ttu-id="ae76a-198">Az SAP NetWeaver verem típusának kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ae76a-198">Select the SAP NetWeaver stack type</span></span>
    1. <span data-ttu-id="ae76a-199">Operációs rendszer típusa</span><span class="sxs-lookup"><span data-stu-id="ae76a-199">Os Type</span></span>  
       <span data-ttu-id="ae76a-200">Válasszon egyet a Linux terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="ae76a-200">Select one of the Linux distributions.</span></span> <span data-ttu-id="ae76a-201">Ehhez a példához válassza ki a SLES 12 saját</span><span class="sxs-lookup"><span data-stu-id="ae76a-201">For this example, select SLES 12 BYOS</span></span>
    1. <span data-ttu-id="ae76a-202">DB típusa</span><span class="sxs-lookup"><span data-stu-id="ae76a-202">Db Type</span></span>  
       <span data-ttu-id="ae76a-203">Válassza ki a HANA</span><span class="sxs-lookup"><span data-stu-id="ae76a-203">Select HANA</span></span>
    1. <span data-ttu-id="ae76a-204">SAP mérete</span><span class="sxs-lookup"><span data-stu-id="ae76a-204">Sap System Size</span></span>  
       <span data-ttu-id="ae76a-205">Az új rendszer nyújtják SAP mennyisége.</span><span class="sxs-lookup"><span data-stu-id="ae76a-205">The amount of SAPS the new system will provide.</span></span> <span data-ttu-id="ae76a-206">Ha nem tudja, a rendszer hány SAP, kérje meg a SAP technológia Partner vagy a rendszer integráló</span><span class="sxs-lookup"><span data-stu-id="ae76a-206">If you are not sure how many SAPS the system will require, please ask your SAP Technology Partner or System Integrator</span></span>
    1. <span data-ttu-id="ae76a-207">Rendszer rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="ae76a-207">System Availability</span></span>  
       <span data-ttu-id="ae76a-208">Válassza ki a magas rendelkezésre ÁLLÁSÚ</span><span class="sxs-lookup"><span data-stu-id="ae76a-208">Select HA</span></span>
    1. <span data-ttu-id="ae76a-209">Rendszergazda felhasználónevét és a rendszergazdai jelszó</span><span class="sxs-lookup"><span data-stu-id="ae76a-209">Admin Username and Admin Password</span></span>  
       <span data-ttu-id="ae76a-210">Új felhasználó jön létre, amely segítségével jelentkezzen be a gépre.</span><span class="sxs-lookup"><span data-stu-id="ae76a-210">A new user is created that can be used to log on to the machine.</span></span>
    1. <span data-ttu-id="ae76a-211">Új vagy meglévő alhálózati</span><span class="sxs-lookup"><span data-stu-id="ae76a-211">New Or Existing Subnet</span></span>  
       <span data-ttu-id="ae76a-212">Meghatározza, hogy egy új virtuális hálózat és alhálózat kell létrehozni, vagy használjon egy létező alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ae76a-212">Determines whether a new virtual network and subnet should be created or an existing subnet should be used.</span></span> <span data-ttu-id="ae76a-213">Ha már van egy virtuális hálózatot, amely a helyszíni hálózathoz csatlakozik, válasszon a meglévő.</span><span class="sxs-lookup"><span data-stu-id="ae76a-213">If you already have a virtual network that is connected to your on-premises network, select existing.</span></span>
    1. <span data-ttu-id="ae76a-214">Alhálózati azonosító</span><span class="sxs-lookup"><span data-stu-id="ae76a-214">Subnet Id</span></span>  
    <span data-ttu-id="ae76a-215">Az alhálózat, amelyhez a virtuális gépek csatlakoznia kell az azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ae76a-215">The ID of the subnet to which the virtual machines should be connected to.</span></span> <span data-ttu-id="ae76a-216">Jelölje ki az alhálózatot, a VPN- vagy Express Route virtuális hálózat a virtuális gép és a helyszíni hálózathoz csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="ae76a-216">Select the subnet of your VPN or Express Route virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="ae76a-217">Az azonosító általában a következőképpen néz következő`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`></span><span class="sxs-lookup"><span data-stu-id="ae76a-217">The ID usually looks like /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`></span></span>

## <a name="setting-up-linux-ha"></a><span data-ttu-id="ae76a-218">Beállítása Linux magas rendelkezésre ÁLLÁSÚ</span><span class="sxs-lookup"><span data-stu-id="ae76a-218">Setting up Linux HA</span></span>

<span data-ttu-id="ae76a-219">A következő elemek fűzve előtagként vagy [A] - alkalmazható az összes csomóponton [1] - 1 vagy [2] - csomópont 2 csak érvényes csomópont csak érvényes.</span><span class="sxs-lookup"><span data-stu-id="ae76a-219">The following items are prefixed with either [A] - applicable to all nodes, [1] - only applicable to node 1 or [2] - only applicable to node 2.</span></span>

1. <span data-ttu-id="ae76a-220">[A] az SAP - csak a saját használhatják a tárolóhelyekkel való regisztrálásához SLES SLES</span><span class="sxs-lookup"><span data-stu-id="ae76a-220">[A] SLES for SAP BYOS only - Register SLES to be able to use the repositories</span></span>
1. <span data-ttu-id="ae76a-221">[A] SLES az SAP saját csak - nyilvános felhő modul hozzá lesz adva</span><span class="sxs-lookup"><span data-stu-id="ae76a-221">[A] SLES for SAP BYOS only - Add public-cloud module</span></span>
1. <span data-ttu-id="ae76a-222">[A] SLES frissítése</span><span class="sxs-lookup"><span data-stu-id="ae76a-222">[A] Update SLES</span></span>
    ```bash
    sudo zypper update

    ```

1. <span data-ttu-id="ae76a-223">[1] ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ae76a-223">[1] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. <span data-ttu-id="ae76a-224">[2] ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ae76a-224">[2] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa

    # insert the public key you copied in the last step into the authorized keys file on the second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. <span data-ttu-id="ae76a-225">[1] ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ae76a-225">[1] Enable ssh access</span></span>
    ```bash
    # insert the public key you copied in the last step into the authorized keys file on the first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. <span data-ttu-id="ae76a-226">[A] magas rendelkezésre ÁLLÁSÚ-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="ae76a-226">[A] Install HA extension</span></span>
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. <span data-ttu-id="ae76a-227">[A] telepítő lemez elrendezése</span><span class="sxs-lookup"><span data-stu-id="ae76a-227">[A] Setup disk layout</span></span>
    1. <span data-ttu-id="ae76a-228">LVM</span><span class="sxs-lookup"><span data-stu-id="ae76a-228">LVM</span></span>  
    <span data-ttu-id="ae76a-229">Általában javasoljuk LVM használandó kötetek, amelyek adatokat tárolhatnak, és a naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="ae76a-229">We generally recommend to use LVM for volumes that store data and log files.</span></span> <span data-ttu-id="ae76a-230">Az alábbi példa azt feltételezi, hogy, hogy a virtuális gépek rendelkeznek négy adatlemezt csatolni, amelynek használatával hozzon létre két köteteket.</span><span class="sxs-lookup"><span data-stu-id="ae76a-230">The example below assumes that the virtual machines have four data disks attached that should be used to create two volumes.</span></span>
        * <span data-ttu-id="ae76a-231">A használni kívánt összes lemez fizikai köteteket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ae76a-231">Create physical volumes for all disks that you want to use.</span></span>
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * <span data-ttu-id="ae76a-232">Az adatfájlok kötet csoport, a naplófájlok egy kötet csoport és egy SAP HANA a megosztott könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-232">Create a volume group for the data files, one volume group for the log files and one for the shared directory of SAP HANA</span></span>
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * <span data-ttu-id="ae76a-233">A logikai köteteket hozhat létre</span><span class="sxs-lookup"><span data-stu-id="ae76a-233">Create the logical volumes</span></span>
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * <span data-ttu-id="ae76a-234">A csatlakoztatási könyvtárak létrehozása, és másolja az összes logikai kötet UUID</span><span class="sxs-lookup"><span data-stu-id="ae76a-234">Create the mount directories and copy the UUID of all logical volumes</span></span>
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * <span data-ttu-id="ae76a-235">A három logikai kötetek között az fstab bejegyzéseket létrehozni</span><span class="sxs-lookup"><span data-stu-id="ae76a-235">Create fstab entries for the three logical volumes</span></span>
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    <span data-ttu-id="ae76a-236">Ez a /etc/fstab sor beszúrása</span><span class="sxs-lookup"><span data-stu-id="ae76a-236">Insert this line to /etc/fstab</span></span>
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * <span data-ttu-id="ae76a-237">Csatlakoztassa az új köteteket</span><span class="sxs-lookup"><span data-stu-id="ae76a-237">Mount the new volumes</span></span>
    <pre><code>
    sudo mount -a
    </code></pre>
    1. <span data-ttu-id="ae76a-238">Egyszerű lemez</span><span class="sxs-lookup"><span data-stu-id="ae76a-238">Plain Disks</span></span>  
       <span data-ttu-id="ae76a-239">A kis vagy bemutató rendszerek, elhelyezhet egy lemezt a HANA adatainak és naplókönyvtárainak fájlokat.</span><span class="sxs-lookup"><span data-stu-id="ae76a-239">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="ae76a-240">A következő parancsok /dev/sdc hozza létre a partíciót, és formázza xfs.</span><span class="sxs-lookup"><span data-stu-id="ae76a-240">The following commands create a partition on /dev/sdc and format it with xfs.</span></span>
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-the-id-of-devsdc1"></a><span data-ttu-id="ae76a-241">Jegyezze fel a /dev/sdc1 azonosítója</span><span class="sxs-lookup"><span data-stu-id="ae76a-241">write down the id of /dev/sdc1</span></span>
    <span data-ttu-id="ae76a-242">sudo/sbin/blkid sudo vi/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="ae76a-242">sudo /sbin/blkid  sudo vi /etc/fstab</span></span>
    ```

    Insert this line to /etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create the target directory and mount the disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. <span data-ttu-id="ae76a-243">[A] állomásnév telepítési állomások</span><span class="sxs-lookup"><span data-stu-id="ae76a-243">[A] Setup host name resolution for all hosts</span></span>  
    <span data-ttu-id="ae76a-244">DNS-kiszolgálót használjon, vagy módosítsa az/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="ae76a-244">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="ae76a-245">Ez a példa bemutatja, hogyan használható az/etc/hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="ae76a-245">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="ae76a-246">Cserélje le az IP-cím és a következő parancsokat az állomásnév</span><span class="sxs-lookup"><span data-stu-id="ae76a-246">Replace the IP address and the hostname in the following commands</span></span>
    ```bash
    sudo vi /etc/hosts
    ```
    <span data-ttu-id="ae76a-247">Helyezze be a következő sorokat/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="ae76a-247">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="ae76a-248">Az IP-cím és a környezet megfelelő állomásnév módosítása</span><span class="sxs-lookup"><span data-stu-id="ae76a-248">Change the IP address and hostname to match your environment</span></span>    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. <span data-ttu-id="ae76a-249">[1] fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="ae76a-249">[1] Install Cluster</span></span>
    ```bash
    sudo ha-cluster-init
    
    # Do you want to continue anyway? [y/N] -> y
    # Network address to bind to (e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish to use SBD? [y/N] -> N
    # Do you wish to configure an administration IP? [y/N] -> N
    ```
        
1. <span data-ttu-id="ae76a-250">[2] csomópont hozzáadása fürthöz</span><span class="sxs-lookup"><span data-stu-id="ae76a-250">[2] Add node to cluster</span></span>
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured to start at system boot.
    # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
    # Do you want to continue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. <span data-ttu-id="ae76a-251">[A] módosítása hacluster jelszó ugyanazt a jelszót</span><span class="sxs-lookup"><span data-stu-id="ae76a-251">[A] Change hacluster password to the same password</span></span>
    ```bash
    sudo passwd hacluster
    
    ```

1. <span data-ttu-id="ae76a-252">[A] egyéb átvitelt használ, és adja hozzá a csomópontlista corosync konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ae76a-252">[A] Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="ae76a-253">Fürt egyébként nem működik.</span><span class="sxs-lookup"><span data-stu-id="ae76a-253">Cluster will not work otherwise.</span></span>
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    <span data-ttu-id="ae76a-254">Vegye fel a következő félkövér tartalmat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="ae76a-254">Add the following bold content to the file.</span></span>
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    <span data-ttu-id="ae76a-255">Indítsa újra a corosync szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ae76a-255">Then restart the corosync service</span></span>

    ```bash
    sudo service corosync restart
    
    ```

1. <span data-ttu-id="ae76a-256">[A] HANA magas rendelkezésre ÁLLÁSÚ csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="ae76a-256">[A] Install HANA HA packages</span></span>  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a><span data-ttu-id="ae76a-257">SAP HANA telepítése</span><span class="sxs-lookup"><span data-stu-id="ae76a-257">Installing SAP HANA</span></span>

<span data-ttu-id="ae76a-258">4 fejezete kövesse a [SAP HANA SR teljesítmény optimalizált forgatókönyv útmutató] [ suse-hana-ha-guide] SAP HANA rendszer replikáció telepítését.</span><span class="sxs-lookup"><span data-stu-id="ae76a-258">Follow chapter 4 of the [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] to install SAP HANA System Replication.</span></span>

1. <span data-ttu-id="ae76a-259">[A] hdblcm futtassa a HANA DVD-ről</span><span class="sxs-lookup"><span data-stu-id="ae76a-259">[A] Run hdblcm from the HANA DVD</span></span>
    * <span data-ttu-id="ae76a-260">Válassza ki a telepítés-1 ></span><span class="sxs-lookup"><span data-stu-id="ae76a-260">Choose installation -> 1</span></span>
    * <span data-ttu-id="ae76a-261">Válassza ki a további összetevők telepítésének 1-></span><span class="sxs-lookup"><span data-stu-id="ae76a-261">Select additional components for installation -> 1</span></span>
    * <span data-ttu-id="ae76a-262">Adja meg a telepítési útvonalat [/ hana/megosztott]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-262">Enter Installation Path [/hana/shared]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-263">Adja meg a helyi állomás nevét [.]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-263">Enter Local Host Name [..]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-264">Végrehajtja a rendszer további gazdagépeket vehet fel?</span><span class="sxs-lookup"><span data-stu-id="ae76a-264">Do you want to add additional hosts to the system?</span></span> <span data-ttu-id="ae76a-265">(i/n) [n]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-265">(y/n) [n]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-266">Adja meg az SAP HANA-azonosító:<SID of HANA e.g. HDB></span><span class="sxs-lookup"><span data-stu-id="ae76a-266">Enter SAP HANA System ID: <SID of HANA e.g. HDB></span></span>
    * <span data-ttu-id="ae76a-267">Adja meg a [00] száma:</span><span class="sxs-lookup"><span data-stu-id="ae76a-267">Enter Instance Number [00]:</span></span>   
  <span data-ttu-id="ae76a-268">HANA példányszámának.</span><span class="sxs-lookup"><span data-stu-id="ae76a-268">HANA Instance number.</span></span> <span data-ttu-id="ae76a-269">03 használja, ha követte a fenti példa vagy használja az Azure-sablon</span><span class="sxs-lookup"><span data-stu-id="ae76a-269">Use 03 if you used the Azure Template or followed the example above</span></span>
    * <span data-ttu-id="ae76a-270">Válassza ki az adatbázis-mód / Index [1] adja meg: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-270">Select Database Mode / Enter Index [1]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-271">Válassza ki a rendszer használati / adja meg az Index [4]:</span><span class="sxs-lookup"><span data-stu-id="ae76a-271">Select System Usage / Enter Index [4]:</span></span>  
  <span data-ttu-id="ae76a-272">A rendszer használati kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ae76a-272">Select the system Usage</span></span>
    * <span data-ttu-id="ae76a-273">Adja meg a helyet, az adatkötetek [/ hana/data/HDB]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-273">Enter Location of Data Volumes [/hana/data/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-274">Adja meg a naplózási kötetek [/ hana/napló/HDB] helyet: ENTER -></span><span class="sxs-lookup"><span data-stu-id="ae76a-274">Enter Location of Log Volumes [/hana/log/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-275">Maximális memória kiosztása korlátozása?</span><span class="sxs-lookup"><span data-stu-id="ae76a-275">Restrict maximum memory allocation?</span></span> <span data-ttu-id="ae76a-276">[n]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-276">[n]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-277">Adja meg a tanúsítvány gazdagép neve a gazdagép "..."</span><span class="sxs-lookup"><span data-stu-id="ae76a-277">Enter Certificate Host Name For Host '...'</span></span> <span data-ttu-id="ae76a-278">[]: -> ADJON MEG</span><span class="sxs-lookup"><span data-stu-id="ae76a-278">[...]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-279">Adja meg a SAP ügynök felhasználói (sapadm) jelszavát:</span><span class="sxs-lookup"><span data-stu-id="ae76a-279">Enter SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="ae76a-280">SAP ügynök felhasználói (sapadm) jelszó megerősítése:</span><span class="sxs-lookup"><span data-stu-id="ae76a-280">Confirm SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="ae76a-281">Adja meg a rendszergazda (hdbadm) jelszavát:</span><span class="sxs-lookup"><span data-stu-id="ae76a-281">Enter System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="ae76a-282">Erősítse meg a rendszergazda (hdbadm) jelszavát:</span><span class="sxs-lookup"><span data-stu-id="ae76a-282">Confirm System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="ae76a-283">Adja meg a rendszer rendszergazdai kezdőkönyvtár [/ usr/sap/HDB/home]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-283">Enter System Administrator Home Directory [/usr/sap/HDB/home]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-284">Adja meg a rendszer rendszergazdai bejelentkezési rendszerhéj [/ bin/sh]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-284">Enter System Administrator Login Shell [/bin/sh]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-285">Adja meg a rendszergazda felhasználói Azonosítóját [1001]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-285">Enter System Administrator User ID [1001]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-286">Adjon meg azonosító a felhasználói csoport (sapsys) [79]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-286">Enter ID of User Group (sapsys) [79]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-287">Adja meg az adatbázis jelszó (rendszer):</span><span class="sxs-lookup"><span data-stu-id="ae76a-287">Enter Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="ae76a-288">Adatbázis (rendszer) felhasználói jelszó megerősítése:</span><span class="sxs-lookup"><span data-stu-id="ae76a-288">Confirm Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="ae76a-289">Számítógép újraindítása után indítsa újra a rendszert?</span><span class="sxs-lookup"><span data-stu-id="ae76a-289">Restart system after machine reboot?</span></span> <span data-ttu-id="ae76a-290">[n]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="ae76a-290">[n]: -> ENTER</span></span>
    * <span data-ttu-id="ae76a-291">Folytatja?</span><span class="sxs-lookup"><span data-stu-id="ae76a-291">Do you want to continue?</span></span> <span data-ttu-id="ae76a-292">(i/n):</span><span class="sxs-lookup"><span data-stu-id="ae76a-292">(y/n):</span></span>  
  <span data-ttu-id="ae76a-293">Az összegzés, és írja be az y gombbal folytathatja</span><span class="sxs-lookup"><span data-stu-id="ae76a-293">Validate the summary and enter y to continue</span></span>
1. <span data-ttu-id="ae76a-294">[A] frissítési SAP Gazdagépügynöke</span><span class="sxs-lookup"><span data-stu-id="ae76a-294">[A] Upgrade SAP Host Agent</span></span>  
  <span data-ttu-id="ae76a-295">Töltse le a legfrissebb SAP a gazdagép ügynöke archív a a [SAP Softwarecenter] [ sap-swcenter] és az ügynökök frissítése a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="ae76a-295">Download the latest SAP Host Agent archive from the [SAP Softwarecenter][sap-swcenter] and run the following command to upgrade the agent.</span></span> <span data-ttu-id="ae76a-296">Cserélje le az archívum mutasson a letöltött fájl elérési útja.</span><span class="sxs-lookup"><span data-stu-id="ae76a-296">Replace the path to the archive to point to the file you downloaded.</span></span>
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

1. <span data-ttu-id="ae76a-297">[1] replikációs HANA létrehozása (a legfelső szintű)</span><span class="sxs-lookup"><span data-stu-id="ae76a-297">[1] Create HANA replication (as root)</span></span>  
    <span data-ttu-id="ae76a-298">A következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="ae76a-298">Run the following command.</span></span> <span data-ttu-id="ae76a-299">Ügyeljen arra, hogy félkövér karakterláncok (HANA rendszer azonosító HDB és példány újrahasznosítása 03) cserélje le az értékeket a SAP HANA-telepítés.</span><span class="sxs-lookup"><span data-stu-id="ae76a-299">Make sure to replace bold strings (HANA System ID HDB and instance number 03) with the values of your SAP HANA installation.</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. <span data-ttu-id="ae76a-300">[A] létrehozása keystore bejegyzés (root)</span><span class="sxs-lookup"><span data-stu-id="ae76a-300">[A] Create keystore entry (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. <span data-ttu-id="ae76a-301">[1] backup database (a legfelső szintű)</span><span class="sxs-lookup"><span data-stu-id="ae76a-301">[1] Backup database (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. <span data-ttu-id="ae76a-302">[1] váltson a sapsid felhasználó (pl. hdbadm), és az elsődleges hely létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ae76a-302">[1] Switch to the sapsid user (for example hdbadm) and create the primary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. <span data-ttu-id="ae76a-303">[2] váltson a sapsid felhasználó (pl. hdbadm), és a másodlagos hely létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ae76a-303">[2] Switch to the sapsid user (for example hdbadm) and create the secondary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a><span data-ttu-id="ae76a-304">Fürt keretrendszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae76a-304">Configure Cluster Framework</span></span>

<span data-ttu-id="ae76a-305">Az alapértelmezett beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="ae76a-305">Change the default settings</span></span>

<pre>
sudo vi crm-defaults.txt
# enter the following to crm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="ae76a-306">Most azt betölteni a fájlt a fürthöz</span><span class="sxs-lookup"><span data-stu-id="ae76a-306">now we load the file to the cluster</span></span>
<span data-ttu-id="ae76a-307">sudo crm terhelés frissítés crm-defaults.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae76a-307">sudo crm configure load update crm-defaults.txt</span></span>
</pre>

### <a name="create-stonith-device"></a><span data-ttu-id="ae76a-308">STONITH eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-308">Create STONITH device</span></span>

<span data-ttu-id="ae76a-309">A STONITH eszköz egy egyszerű szolgáltatást használ, szemben a Microsoft Azure engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ae76a-309">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="ae76a-310">Kérjük, kövesse ezeket a lépéseket egy egyszerű szolgáltatásnév létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ae76a-310">Please follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="ae76a-311">Ugrás a <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="ae76a-311">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="ae76a-312">Nyissa meg az Azure Active Directory panelt</span><span class="sxs-lookup"><span data-stu-id="ae76a-312">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="ae76a-313">Nyissa meg tulajdonságait, és jegyezze fel a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="ae76a-313">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="ae76a-314">Ez a **bérlőazonosító**.</span><span class="sxs-lookup"><span data-stu-id="ae76a-314">This is the **tenant id**.</span></span>
1. <span data-ttu-id="ae76a-315">Kattintson az alkalmazás-regisztráció</span><span class="sxs-lookup"><span data-stu-id="ae76a-315">Click App registrations</span></span>
1. <span data-ttu-id="ae76a-316">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="ae76a-316">Click Add</span></span>
1. <span data-ttu-id="ae76a-317">Adjon meg egy nevet, válassza ki a "Web app/API" alkalmazástípus, adja meg a bejelentkezési URL-címet (például http://localhost) és kattintson a Létrehozás gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-317">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="ae76a-318">A bejelentkezési URL-címet nem használja, és bármilyen érvényes URL-CÍMEK lehetnek</span><span class="sxs-lookup"><span data-stu-id="ae76a-318">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="ae76a-319">Válassza ki az új alkalmazást, és a beállítások lapon kattintson a kulcsok</span><span class="sxs-lookup"><span data-stu-id="ae76a-319">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="ae76a-320">Adja meg egy új kulcs leírását, válassza a "Soha nem jár le", és kattintson a Mentés gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-320">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="ae76a-321">Jegyezze fel az értéket.</span><span class="sxs-lookup"><span data-stu-id="ae76a-321">Write down the Value.</span></span> <span data-ttu-id="ae76a-322">Használják a **jelszó** a szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="ae76a-322">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="ae76a-323">Jegyezze fel az azonosítót.</span><span class="sxs-lookup"><span data-stu-id="ae76a-323">Write down the Application Id.</span></span> <span data-ttu-id="ae76a-324">A felhasználónév használják (**bejelentkezési azonosító** az alábbi lépéseket a) a szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="ae76a-324">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="ae76a-325">A szolgáltatás egyszerű nincs engedélye a alapértelmezés szerint az Azure-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ae76a-325">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="ae76a-326">Hozzá kell rendelnie a szolgáltatás egyszerű engedélyek indítása és leállítása (felszabadítása) a fürt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ae76a-326">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="ae76a-327">Ugrás a https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="ae76a-327">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="ae76a-328">Nyissa meg az összes erőforrás panel</span><span class="sxs-lookup"><span data-stu-id="ae76a-328">Open the All resources blade</span></span>
1. <span data-ttu-id="ae76a-329">Válassza ki a virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="ae76a-329">Select the virtual machine</span></span>
1. <span data-ttu-id="ae76a-330">Kattintson a hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="ae76a-330">Click Access control (IAM)</span></span>
1. <span data-ttu-id="ae76a-331">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="ae76a-331">Click Add</span></span>
1. <span data-ttu-id="ae76a-332">Válassza ki a szerepkör tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="ae76a-332">Select the role Owner</span></span>
1. <span data-ttu-id="ae76a-333">Adja meg az előbb létrehozott alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="ae76a-333">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="ae76a-334">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="ae76a-334">Click OK</span></span>

<span data-ttu-id="ae76a-335">Után szerkeszteni a virtuális gépek engedélyeit, beállíthatja a STONITH eszközök a fürtben.</span><span class="sxs-lookup"><span data-stu-id="ae76a-335">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre>
sudo vi crm-fencing.txt
# enter the following to crm-fencing.txt
# replace the bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="ae76a-336">Most azt betölteni a fájlt a fürthöz</span><span class="sxs-lookup"><span data-stu-id="ae76a-336">now we load the file to the cluster</span></span>
<span data-ttu-id="ae76a-337">sudo crm terhelés frissítés crm-fencing.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae76a-337">sudo crm configure load update crm-fencing.txt</span></span>
</pre>

### <a name="create-sap-hana-resources"></a><span data-ttu-id="ae76a-338">SAP HANA-erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae76a-338">Create SAP HANA resources</span></span>

<pre>
sudo vi crm-saphanatop.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="ae76a-339">Most azt betölteni a fájlt a fürthöz</span><span class="sxs-lookup"><span data-stu-id="ae76a-339">now we load the file to the cluster</span></span>
<span data-ttu-id="ae76a-340">sudo crm terhelés frissítés crm-saphanatop.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae76a-340">sudo crm configure load update crm-saphanatop.txt</span></span>
</pre>

<pre>
sudo vi crm-saphana.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="ae76a-341">Most azt betölteni a fájlt a fürthöz</span><span class="sxs-lookup"><span data-stu-id="ae76a-341">now we load the file to the cluster</span></span>
<span data-ttu-id="ae76a-342">sudo crm terhelés frissítés crm-saphana.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae76a-342">sudo crm configure load update crm-saphana.txt</span></span>
</pre>

### <a name="test-cluster-setup"></a><span data-ttu-id="ae76a-343">Teszt fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="ae76a-343">Test cluster setup</span></span>
<span data-ttu-id="ae76a-344">A következő fejezet ismerteti, hogyan tesztelheti a telepítőt.</span><span class="sxs-lookup"><span data-stu-id="ae76a-344">The following chapter describe how you can test your setup.</span></span> <span data-ttu-id="ae76a-345">Minden teszt feltételezi, hogy a legfelső szintű áll, és az SAP HANA-főkiszolgáló fut a virtuális gép saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="ae76a-345">Every test assumes that you are root and the SAP HANA master is running on the virtual machine saphanavm1.</span></span>

#### <a name="fencing-test"></a><span data-ttu-id="ae76a-346">Teszt kerítésépítés</span><span class="sxs-lookup"><span data-stu-id="ae76a-346">Fencing Test</span></span>

<span data-ttu-id="ae76a-347">A telepítő a kerítés ügynök tesztelheti a hálózati adapternek csomópont saphanavm1 letiltásával.</span><span class="sxs-lookup"><span data-stu-id="ae76a-347">You can test the setup of the fencing agent by disabling the network interface on node saphanavm1.</span></span>

<pre><code>
sudo ifdown eth0
</code></pre>

<span data-ttu-id="ae76a-348">A virtuális gép kell most beolvasása újraindult, vagy attól függően, hogy a fürt konfigurációját, leállt.</span><span class="sxs-lookup"><span data-stu-id="ae76a-348">The virtual machine should now get restarted or stopped depending on your cluster configuration.</span></span>
<span data-ttu-id="ae76a-349">Ha a stonith-művelet kikapcsolva, a virtuális gép leáll, és az erőforrások települnek a futó virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="ae76a-349">If you set the stonith-action to off, the virtual machine will be stopped and the resources are migrated to the running virtual machine.</span></span>

<span data-ttu-id="ae76a-350">Után újra elindítani a virtuális gépet, a SAP HANA-erőforrás indulnak-e a másodlagos el Ha AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="ae76a-350">Once you start the virtual machine again, the SAP HANA resource will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="ae76a-351">Ebben az esetben kell HANA-példány beállítása a másodlagos, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="ae76a-351">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a><span data-ttu-id="ae76a-352">Manuális feladatátvétel tesztelése</span><span class="sxs-lookup"><span data-stu-id="ae76a-352">Testing a manual failover</span></span>

<span data-ttu-id="ae76a-353">Manuális feladatátvétel tesztelheti a csomópont saphanavm1 a támasztja szolgáltatás leállításával.</span><span class="sxs-lookup"><span data-stu-id="ae76a-353">You can test a manual failover by stopping the pacemaker service on node saphanavm1.</span></span>
<pre><code>
service pacemaker stop
</code></pre>

<span data-ttu-id="ae76a-354">A feladatátvétel után újra elindíthatja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ae76a-354">After the failover, you can start the service again.</span></span> <span data-ttu-id="ae76a-355">Az SAP HANA-erőforrás a saphanavm1 nem fog elindulni, mint a másodlagos Ha AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="ae76a-355">The SAP HANA resource on saphanavm1 will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="ae76a-356">Ebben az esetben kell HANA-példány beállítása a másodlagos, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="ae76a-356">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a><span data-ttu-id="ae76a-357">Áttelepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ae76a-357">Testing a migration</span></span>

<span data-ttu-id="ae76a-358">Az SAP HANA-főcsomópont áttelepítése a következő parancs végrehajtása</span><span class="sxs-lookup"><span data-stu-id="ae76a-358">You can migrate the SAP HANA master node by executing the following command</span></span>
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="ae76a-359">Ez át kell telepíteni a fő SAP HANA-csomópont és a virtuális IP-címet saphanavm2 tartalmazó csoport.</span><span class="sxs-lookup"><span data-stu-id="ae76a-359">This should migrate the SAP HANA master node and the group that contains the virtual IP address to saphanavm2.</span></span>
<span data-ttu-id="ae76a-360">Az SAP HANA-erőforrás a saphanavm1 nem fog elindulni, mint a másodlagos Ha AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="ae76a-360">The SAP HANA resource on saphanavm1 will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="ae76a-361">Ebben az esetben kell HANA-példány beállítása a másodlagos, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="ae76a-361">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

<span data-ttu-id="ae76a-362">Az áttelepítés hoz létre a hely contraints, hogy újra kell törölni.</span><span class="sxs-lookup"><span data-stu-id="ae76a-362">The migration creates location contraints that need to be deleted again.</span></span>

<pre><code>
crm configure edited

# delete location contraints that are named like the following contraint. You should have two contraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="ae76a-363">Szükség karbantartása a másodlagos csomópont-erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="ae76a-363">You also need to cleanup the state of the secondary node resource</span></span>

<pre><code>
# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a><span data-ttu-id="ae76a-364">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae76a-364">Next steps</span></span>
* <span data-ttu-id="ae76a-365">[Az Azure virtuális gépek tervezési és megvalósítási az SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="ae76a-365">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="ae76a-366">[Az SAP Azure virtuális gépek telepítése][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="ae76a-366">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="ae76a-367">[Az SAP Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="ae76a-367">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="ae76a-368">Magas rendelkezésre állás és az Azure (nagy példány) az SAP HANA vész-helyreállítási terv létrehozásához, lásd: [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="ae76a-368">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span> 
