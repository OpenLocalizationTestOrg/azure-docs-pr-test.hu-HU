---
title: "rendelkezésre állás az SAP HANA Azure virtuális gépek (VM) aaaHigh |} Microsoft Docs"
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
ms.openlocfilehash: dcb9bb70594f9d97f8a888cec76300bcbe0bf1ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a><span data-ttu-id="0633f-103">Magas rendelkezésre állású SAP HANA az Azure virtuális gépek (VM)</span><span class="sxs-lookup"><span data-stu-id="0633f-103">High Availability of SAP HANA on Azure Virtual Machines (VMs)</span></span>

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

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

<span data-ttu-id="0633f-113">A helyszíni vagy HANA replikációs használhatja, vagy használja a megosztott tároló tooestablish magas rendelkezésre állású SAP Hana.</span><span class="sxs-lookup"><span data-stu-id="0633f-113">On-premises, you can use either HANA System Replication or use shared storage tooestablish high availability for SAP HANA.</span></span>
<span data-ttu-id="0633f-114">Sajnos jelenleg csak a támogatott HANA replikációs beállítása az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="0633f-114">We currently only support setting up HANA System Replication on Azure.</span></span> <span data-ttu-id="0633f-115">SAP HANA-replikáció egy főcsomópont és legalább egy alárendelt csomópont áll.</span><span class="sxs-lookup"><span data-stu-id="0633f-115">SAP HANA Replication consists of one master node and at least one slave node.</span></span> <span data-ttu-id="0633f-116">Adatok hello fő csomóponton módosítások toohello toohello alárendelt csomópontok replikálja a szinkron vagy aszinkron módon.</span><span class="sxs-lookup"><span data-stu-id="0633f-116">Changes toohello data on hello master node are replicated toohello slave nodes synchronously or asynchronously.</span></span>

<span data-ttu-id="0633f-117">Ez a cikk ismerteti, hogyan toodeploy hello virtuális gépek, virtuális gépek hello konfigurálása, hello fürt keretrendszer telepítése, telepítése és konfigurálása SAP HANA replikációs.</span><span class="sxs-lookup"><span data-stu-id="0633f-117">This article describes how toodeploy hello virtual machines, configure hello virtual machines, install hello cluster framework, install and configure SAP HANA System Replication.</span></span>
<span data-ttu-id="0633f-118">Hello például konfigurációk telepítési parancsok stb példányszámának 03 és HANA rendszer azonosító HDB szolgál.</span><span class="sxs-lookup"><span data-stu-id="0633f-118">In hello example configurations, installation commands etc. instance number 03 and HANA System ID HDB is used.</span></span>

<span data-ttu-id="0633f-119">Olvassa el a következő SAP megjegyzések és által írt cikkeket először hello</span><span class="sxs-lookup"><span data-stu-id="0633f-119">Read hello following SAP Notes and papers first</span></span>

* <span data-ttu-id="0633f-120">SAP Megjegyzés [1928533], amelynek van:</span><span class="sxs-lookup"><span data-stu-id="0633f-120">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="0633f-121">Hello SAP szoftver központi telepítése által támogatott Azure Virtuálisgép-méretek listáját</span><span class="sxs-lookup"><span data-stu-id="0633f-121">List of Azure VM sizes that are supported for hello deployment of SAP software</span></span>
  * <span data-ttu-id="0633f-122">Az Azure Virtuálisgép-méretek fontos készletkapacitás információival</span><span class="sxs-lookup"><span data-stu-id="0633f-122">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="0633f-123">Támogatott SAP szoftver, és az operációs rendszer és az adatbázis kombinációját</span><span class="sxs-lookup"><span data-stu-id="0633f-123">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="0633f-124">A Windows és a Microsoft Azure Linux szükséges SAP kernel verziója</span><span class="sxs-lookup"><span data-stu-id="0633f-124">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>
* <span data-ttu-id="0633f-125">SAP Megjegyzés [2015553] a SAP-támogatott SAP szoftverek központi telepítése az Azure-ban szükséges előfeltételeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0633f-125">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="0633f-126">SAP Megjegyzés [2205917] javasolt a SUSE Linux Enterprise Server operációs rendszer beállításait az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="0633f-126">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="0633f-127">SAP Megjegyzés [1944799] SUSE Linux Enterprise Server SAP HANA-irányelvek rendelkezik az SAP-alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="0633f-127">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="0633f-128">SAP Megjegyzés [2178632] tartalmaz részletes információkat az Azure-ban SAP jelentett összes figyelési metrikákat.</span><span class="sxs-lookup"><span data-stu-id="0633f-128">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="0633f-129">SAP Megjegyzés [2191498] hello szükséges SAP állomás ügynök verziója Linux az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0633f-129">SAP Note [2191498] has hello required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="0633f-130">SAP Megjegyzés [2243692] SAP Azure Linux licenceléssel kapcsolatos információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0633f-130">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="0633f-131">SAP Megjegyzés [1984787] SUSE Linux Enterprise Server 12 vonatkozó általános információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0633f-131">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="0633f-132">SAP Megjegyzés [1999351] hello Azure fokozott Figyelőbővítmény az SAP további információkat.</span><span class="sxs-lookup"><span data-stu-id="0633f-132">SAP Note [1999351] has additional troubleshooting information for hello Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="0633f-133">[SAP közösségi WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) rendelkezik az összes szükséges SAP megjegyzések Linux.</span><span class="sxs-lookup"><span data-stu-id="0633f-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="0633f-134">[Azure virtuális gépek tervezési és megvalósítási az SAP Linux rendszeren][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="0633f-134">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="0633f-135">[Az Azure virtuális gépek telepítése az SAP, Linux (Ez a cikk)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="0633f-135">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="0633f-136">[Az SAP Linux Azure virtuális gépek DBMS-telepítés][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="0633f-136">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="0633f-137">[SAP HANA SR teljesítmény optimalizált forgatókönyv] [ suse-hana-ha-guide] hello útmutató tartalmazza az összes szükséges információk tooset helyszíni SAP HANA replikációs fel.</span><span class="sxs-lookup"><span data-stu-id="0633f-137">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide] hello guide contains all required information tooset up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="0633f-138">Ez az útmutató használja kiindulópontként.</span><span class="sxs-lookup"><span data-stu-id="0633f-138">Use this guide as a baseline.</span></span>

## <a name="deploying-linux"></a><span data-ttu-id="0633f-139">Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="0633f-139">Deploying Linux</span></span>

<span data-ttu-id="0633f-140">SUSE Linux Enterprise Server hello erőforrás ügynök SAP Hana tartalmazza az SAP-alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="0633f-140">hello resource agent for SAP HANA is included in SUSE Linux Enterprise Server for SAP Applications.</span></span>
<span data-ttu-id="0633f-141">hello Azure piactér SUSE Linux Enterprise Server SAP alkalmazások 12 a saját (Bring Your saját előfizetés), amelyeket felhasználhat toodeploy új virtuális gépek a kép tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0633f-141">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 with BYOS (Bring Your Own Subscription) that you can use toodeploy new virtual machines.</span></span>

### <a name="manual-deployment"></a><span data-ttu-id="0633f-142">Manuális telepítése</span><span class="sxs-lookup"><span data-stu-id="0633f-142">Manual Deployment</span></span>

1. <span data-ttu-id="0633f-143">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-143">Create a Resource Group</span></span>
1. <span data-ttu-id="0633f-144">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-144">Create a Virtual Network</span></span>
1. <span data-ttu-id="0633f-145">Két Storage-fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-145">Create two Storage Accounts</span></span>
1. <span data-ttu-id="0633f-146">Egy rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-146">Create an Availability Set</span></span>  
   <span data-ttu-id="0633f-147">Készlet maximális frissítési tartomány</span><span class="sxs-lookup"><span data-stu-id="0633f-147">Set max update domain</span></span>
1. <span data-ttu-id="0633f-148">Hozzon létre egy terhelés-kiegyenlítő (belső)</span><span class="sxs-lookup"><span data-stu-id="0633f-148">Create a Load Balancer (internal)</span></span>  
   <span data-ttu-id="0633f-149">Válassza ki a fenti lépést a virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="0633f-149">Select VNET of step above</span></span>
1. <span data-ttu-id="0633f-150">1 virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-150">Create Virtual Machine 1</span></span>  
   <span data-ttu-id="0633f-151">https://Portal.Azure.com/#Create/SUSE-byos.sles-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="0633f-151">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="0633f-152">SLES SAP alkalmazások 12 SP1 (saját)</span><span class="sxs-lookup"><span data-stu-id="0633f-152">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="0633f-153">Válassza ki a Tárfiók 1</span><span class="sxs-lookup"><span data-stu-id="0633f-153">Select Storage Account 1</span></span>  
   <span data-ttu-id="0633f-154">A rendelkezésre állási csoport kiválasztása</span><span class="sxs-lookup"><span data-stu-id="0633f-154">Select Availability Set</span></span>  
1. <span data-ttu-id="0633f-155">2. virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-155">Create Virtual Machine 2</span></span>  
   <span data-ttu-id="0633f-156">https://Portal.Azure.com/#Create/SUSE-byos.sles-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="0633f-156">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="0633f-157">SLES SAP alkalmazások 12 SP1 (saját)</span><span class="sxs-lookup"><span data-stu-id="0633f-157">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="0633f-158">Válassza ki a Tárfiók 2</span><span class="sxs-lookup"><span data-stu-id="0633f-158">Select Storage Account 2</span></span>   
   <span data-ttu-id="0633f-159">A rendelkezésre állási csoport kiválasztása</span><span class="sxs-lookup"><span data-stu-id="0633f-159">Select Availability Set</span></span>  
1. <span data-ttu-id="0633f-160">Adatlemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0633f-160">Add Data Disks</span></span>
1. <span data-ttu-id="0633f-161">Terheléselosztó hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0633f-161">Configure hello load balancer</span></span>
    1. <span data-ttu-id="0633f-162">Előtérbeli IP-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-162">Create a frontend IP pool</span></span>
        1. <span data-ttu-id="0633f-163">Nyissa meg a hello terheléselosztó előtérbeli IP-készlet között, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-163">Open hello load balancer, select frontend IP pool and click Add</span></span>
        1. <span data-ttu-id="0633f-164">Adja meg a hello hello új előtérbeli IP-készlet neve (például hana-előtér)</span><span class="sxs-lookup"><span data-stu-id="0633f-164">Enter hello name of hello new frontend IP pool (for example hana-frontend)</span></span>
       1. <span data-ttu-id="0633f-165">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-165">Click OK</span></span>
        1. <span data-ttu-id="0633f-166">Hello új előtérbeli IP-készlet létrehozása után jegyezze fel az IP-címe</span><span class="sxs-lookup"><span data-stu-id="0633f-166">After hello new frontend IP pool is created, write down its IP address</span></span>
    1. <span data-ttu-id="0633f-167">Háttér-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-167">Create a backend pool</span></span>
        1. <span data-ttu-id="0633f-168">Nyissa meg a terheléselosztó hello, válassza ki a háttérkészlet, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-168">Open hello load balancer, select backend pools and click Add</span></span>
        1. <span data-ttu-id="0633f-169">Adjon meg hello hello új háttérkészlet (például hana-háttérrendszer)</span><span class="sxs-lookup"><span data-stu-id="0633f-169">Enter hello name of hello new backend pool (for example hana-backend)</span></span>
        1. <span data-ttu-id="0633f-170">Kattintson a Hozzáadás gombra a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="0633f-170">Click Add a virtual machine</span></span>
        1. <span data-ttu-id="0633f-171">Válassza ki a rendelkezésre állási csoport a korábban létrehozott hello</span><span class="sxs-lookup"><span data-stu-id="0633f-171">Select hello Availability Set you created earlier</span></span>
        1. <span data-ttu-id="0633f-172">Válassza ki a hello virtuális gépek hello SAP HANA-fürt</span><span class="sxs-lookup"><span data-stu-id="0633f-172">Select hello virtual machines of hello SAP HANA cluster</span></span>
        1. <span data-ttu-id="0633f-173">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-173">Click OK</span></span>
    1. <span data-ttu-id="0633f-174">Hozzon létre egy állapotmintáihoz</span><span class="sxs-lookup"><span data-stu-id="0633f-174">Create a health probe</span></span>
       1. <span data-ttu-id="0633f-175">Nyissa meg a terheléselosztó hello, válassza ki a állapotteljesítmény, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-175">Open hello load balancer, select health probes and click Add</span></span>
        1. <span data-ttu-id="0633f-176">Adjon meg hello hello új állapotmintáihoz (például hana-hp)</span><span class="sxs-lookup"><span data-stu-id="0633f-176">Enter hello name of hello new health probe (for example hana-hp)</span></span>
        1. <span data-ttu-id="0633f-177">Válassza ki a TCP protokoll, port 625**03**, időköz 5 és sérült küszöbérték 2</span><span class="sxs-lookup"><span data-stu-id="0633f-177">Select TCP as protocol, port 625**03**, keep Interval 5 and Unhealthy threshold 2</span></span>
        1. <span data-ttu-id="0633f-178">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-178">Click OK</span></span>
    1. <span data-ttu-id="0633f-179">Terheléselosztási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-179">Create load balancing rules</span></span>
        1. <span data-ttu-id="0633f-180">Nyissa meg a terheléselosztó hello, válassza ki a terheléselosztási szabályok, kattintson a Hozzáadás gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-180">Open hello load balancer, select load balancing rules and click Add</span></span>
        1. <span data-ttu-id="0633f-181">Adja meg a hello új terheléselosztási szabály hello nevét (például hana-lb-3**03**15)</span><span class="sxs-lookup"><span data-stu-id="0633f-181">Enter hello name of hello new load balancer rule (for example hana-lb-3**03**15)</span></span>
        1. <span data-ttu-id="0633f-182">SELECT hello front-end IP-címet, a háttérkészlet és a rendszerállapot mintavételi, korábban létrehozott (például hana-előtér)</span><span class="sxs-lookup"><span data-stu-id="0633f-182">Select hello frontend IP address, backend pool and health probe you created earlier (for example hana-frontend)</span></span>
        1. <span data-ttu-id="0633f-183">Tartsa a TCP protokoll, adja meg azt a portot 3**03**15</span><span class="sxs-lookup"><span data-stu-id="0633f-183">Keep protocol TCP, enter port 3**03**15</span></span>
        1. <span data-ttu-id="0633f-184">Üresjárati időtúllépés too30 perc növelése</span><span class="sxs-lookup"><span data-stu-id="0633f-184">Increase idle timeout too30 minutes</span></span>
       1. <span data-ttu-id="0633f-185">**Győződjön meg arról, hogy tooenable fix IP-Címek**</span><span class="sxs-lookup"><span data-stu-id="0633f-185">**Make sure tooenable Floating IP**</span></span>
        1. <span data-ttu-id="0633f-186">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-186">Click OK</span></span>
        1. <span data-ttu-id="0633f-187">Ismételje meg a hello fent port 3**03**17</span><span class="sxs-lookup"><span data-stu-id="0633f-187">Repeat hello steps above for port 3**03**17</span></span>

### <a name="deploy-with-template"></a><span data-ttu-id="0633f-188">Üzembe helyezés sablon használatával</span><span class="sxs-lookup"><span data-stu-id="0633f-188">Deploy with template</span></span>
<span data-ttu-id="0633f-189">Használhat hello gyors üzembe helyezési sablonok a githubon toodeploy minden szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="0633f-189">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="0633f-190">hello sablon telepíti hello virtuális gépek, a terheléselosztó hello, a rendelkezésre állási csoport stb. Kövesse a lépéseket toodeploy hello sablon:</span><span class="sxs-lookup"><span data-stu-id="0633f-190">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="0633f-191">Nyissa meg hello [adatbázis sablon] [ template-multisid-db] vagy hello [sablon összevont] [ template-converged] hello Azure Portal hello adatbázis-sablon csak létrehoz hello terheléselosztási szabályok adatbázis mivel hello átszervezett sablon is létrehozza a terheléselosztási szabályok hello egy ASC/SCS és SSZON (csak Linux) példányát.</span><span class="sxs-lookup"><span data-stu-id="0633f-191">Open hello [database template][template-multisid-db] or hello [converged template][template-converged] on hello Azure Portal hello database template only creates hello load-balancing rules for a database whereas hello converged template also creates hello load-balancing rules for an ASCS/SCS and ERS (Linux only) instance.</span></span> <span data-ttu-id="0633f-192">Ha azt tervezi, hogy az SAP NetWeaver-alapú rendszerek tooinstall, és azt is szeretné tooinstall hello ASC/SCS példányhoz hello azonos gépek, használja a hello [sablon összevont][template-converged].</span><span class="sxs-lookup"><span data-stu-id="0633f-192">If you plan tooinstall an SAP NetWeaver based system and you also want tooinstall hello ASCS/SCS instance on hello same machines, use hello [converged template][template-converged].</span></span>
1. <span data-ttu-id="0633f-193">Adja meg a következő paraméterek hello</span><span class="sxs-lookup"><span data-stu-id="0633f-193">Enter hello following parameters</span></span>
    1. <span data-ttu-id="0633f-194">SAP azonosító</span><span class="sxs-lookup"><span data-stu-id="0633f-194">Sap System Id</span></span>  
       <span data-ttu-id="0633f-195">Adja meg a hello SAP rendszer azonosító hello tooinstall kívánt SAP rendszer.</span><span class="sxs-lookup"><span data-stu-id="0633f-195">Enter hello SAP system Id of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="0633f-196">hello azonosító használandó előtagjaként hello telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="0633f-196">hello Id will be used as a prefix for hello resources that are deployed.</span></span>
    1. <span data-ttu-id="0633f-197">A készlet típusa (csak akkor érvényes, ha hello átszervezett sablont használ)</span><span class="sxs-lookup"><span data-stu-id="0633f-197">Stack Type (only applicable if you use hello converged template)</span></span>  
       <span data-ttu-id="0633f-198">Hello SAP NetWeaver verem típusának kiválasztása</span><span class="sxs-lookup"><span data-stu-id="0633f-198">Select hello SAP NetWeaver stack type</span></span>
    1. <span data-ttu-id="0633f-199">Operációs rendszer típusa</span><span class="sxs-lookup"><span data-stu-id="0633f-199">Os Type</span></span>  
       <span data-ttu-id="0633f-200">Válasszon ki egy hello Linux terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="0633f-200">Select one of hello Linux distributions.</span></span> <span data-ttu-id="0633f-201">Ehhez a példához válassza ki a SLES 12 saját</span><span class="sxs-lookup"><span data-stu-id="0633f-201">For this example, select SLES 12 BYOS</span></span>
    1. <span data-ttu-id="0633f-202">DB típusa</span><span class="sxs-lookup"><span data-stu-id="0633f-202">Db Type</span></span>  
       <span data-ttu-id="0633f-203">Válassza ki a HANA</span><span class="sxs-lookup"><span data-stu-id="0633f-203">Select HANA</span></span>
    1. <span data-ttu-id="0633f-204">SAP mérete</span><span class="sxs-lookup"><span data-stu-id="0633f-204">Sap System Size</span></span>  
       <span data-ttu-id="0633f-205">SAP hello új rendszer hello mennyisége ad meg.</span><span class="sxs-lookup"><span data-stu-id="0633f-205">hello amount of SAPS hello new system will provide.</span></span> <span data-ttu-id="0633f-206">Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló</span><span class="sxs-lookup"><span data-stu-id="0633f-206">If you are not sure how many SAPS hello system will require, please ask your SAP Technology Partner or System Integrator</span></span>
    1. <span data-ttu-id="0633f-207">Rendszer rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="0633f-207">System Availability</span></span>  
       <span data-ttu-id="0633f-208">Válassza ki a magas rendelkezésre ÁLLÁSÚ</span><span class="sxs-lookup"><span data-stu-id="0633f-208">Select HA</span></span>
    1. <span data-ttu-id="0633f-209">Rendszergazda felhasználónevét és a rendszergazdai jelszó</span><span class="sxs-lookup"><span data-stu-id="0633f-209">Admin Username and Admin Password</span></span>  
       <span data-ttu-id="0633f-210">Új felhasználó jön létre, amely lehet használt toolog toohello gépen.</span><span class="sxs-lookup"><span data-stu-id="0633f-210">A new user is created that can be used toolog on toohello machine.</span></span>
    1. <span data-ttu-id="0633f-211">Új vagy meglévő alhálózati</span><span class="sxs-lookup"><span data-stu-id="0633f-211">New Or Existing Subnet</span></span>  
       <span data-ttu-id="0633f-212">Meghatározza, hogy egy új virtuális hálózat és alhálózat kell létrehozni, vagy használjon egy létező alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="0633f-212">Determines whether a new virtual network and subnet should be created or an existing subnet should be used.</span></span> <span data-ttu-id="0633f-213">Ha már van egy virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat, válasszon a meglévő.</span><span class="sxs-lookup"><span data-stu-id="0633f-213">If you already have a virtual network that is connected tooyour on-premises network, select existing.</span></span>
    1. <span data-ttu-id="0633f-214">Alhálózati azonosító</span><span class="sxs-lookup"><span data-stu-id="0633f-214">Subnet Id</span></span>  
    <span data-ttu-id="0633f-215">hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakoznia kell.</span><span class="sxs-lookup"><span data-stu-id="0633f-215">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span> <span data-ttu-id="0633f-216">Válassza ki a VPN- vagy Express Route virtuális hálózati tooconnect hello virtuális gép tooyour a helyszíni hálózat hello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="0633f-216">Select hello subnet of your VPN or Express Route virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="0633f-217">hello azonosítója általában a következőképpen néz következő`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`></span><span class="sxs-lookup"><span data-stu-id="0633f-217">hello ID usually looks like /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`></span></span>

## <a name="setting-up-linux-ha"></a><span data-ttu-id="0633f-218">Beállítása Linux magas rendelkezésre ÁLLÁSÚ</span><span class="sxs-lookup"><span data-stu-id="0633f-218">Setting up Linux HA</span></span>

<span data-ttu-id="0633f-219">a következő elemek hello fűzve előtagként vagy [A] - alkalmazható tooall csomópontok, toonode 2 [1] - 1 vagy a(z) [2] csak érvényes toonode - csak érvényes.</span><span class="sxs-lookup"><span data-stu-id="0633f-219">hello following items are prefixed with either [A] - applicable tooall nodes, [1] - only applicable toonode 1 or [2] - only applicable toonode 2.</span></span>

1. <span data-ttu-id="0633f-220">[A] az SAP - csak a saját regisztrálása SLES toobe képes toouse hello adattárak SLES</span><span class="sxs-lookup"><span data-stu-id="0633f-220">[A] SLES for SAP BYOS only - Register SLES toobe able toouse hello repositories</span></span>
1. <span data-ttu-id="0633f-221">[A] SLES az SAP saját csak - nyilvános felhő modul hozzá lesz adva</span><span class="sxs-lookup"><span data-stu-id="0633f-221">[A] SLES for SAP BYOS only - Add public-cloud module</span></span>
1. <span data-ttu-id="0633f-222">[A] SLES frissítése</span><span class="sxs-lookup"><span data-stu-id="0633f-222">[A] Update SLES</span></span>
    ```bash
    sudo zypper update

    ```

1. <span data-ttu-id="0633f-223">[1] ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0633f-223">[1] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. <span data-ttu-id="0633f-224">[2] ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0633f-224">[2] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa

    # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. <span data-ttu-id="0633f-225">[1] ssh hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0633f-225">[1] Enable ssh access</span></span>
    ```bash
    # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. <span data-ttu-id="0633f-226">[A] magas rendelkezésre ÁLLÁSÚ-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="0633f-226">[A] Install HA extension</span></span>
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. <span data-ttu-id="0633f-227">[A] telepítő lemez elrendezése</span><span class="sxs-lookup"><span data-stu-id="0633f-227">[A] Setup disk layout</span></span>
    1. <span data-ttu-id="0633f-228">LVM</span><span class="sxs-lookup"><span data-stu-id="0633f-228">LVM</span></span>  
    <span data-ttu-id="0633f-229">Általánosságban javasolt toouse LVM kötetek, amelyek adatokat tárolhatnak, és a naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="0633f-229">We generally recommend toouse LVM for volumes that store data and log files.</span></span> <span data-ttu-id="0633f-230">hello az alábbi példa azt feltételezi, hogy hello virtuális gépek rendelkezik-e a négy adatlemezt csatolni, amelyeket használt toocreate két kötet.</span><span class="sxs-lookup"><span data-stu-id="0633f-230">hello example below assumes that hello virtual machines have four data disks attached that should be used toocreate two volumes.</span></span>
        * <span data-ttu-id="0633f-231">Az összes lemezt, amelyet az toouse fizikai köteteket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0633f-231">Create physical volumes for all disks that you want toouse.</span></span>
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * <span data-ttu-id="0633f-232">Hozzon létre egy kötet csoport hello adatfájlok, hello naplófájlok egy kötet csoport és egy hello megosztott könyvtárában SAP HANA</span><span class="sxs-lookup"><span data-stu-id="0633f-232">Create a volume group for hello data files, one volume group for hello log files and one for hello shared directory of SAP HANA</span></span>
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * <span data-ttu-id="0633f-233">Hello logikai köteteket hozhat létre</span><span class="sxs-lookup"><span data-stu-id="0633f-233">Create hello logical volumes</span></span>
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * <span data-ttu-id="0633f-234">Hello csatlakoztatási könyvtárak létrehozása, és másolja az összes logikai kötet UUID hello</span><span class="sxs-lookup"><span data-stu-id="0633f-234">Create hello mount directories and copy hello UUID of all logical volumes</span></span>
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * <span data-ttu-id="0633f-235">Három logikai kötetek hello fstab bejegyzéseket létrehozni</span><span class="sxs-lookup"><span data-stu-id="0633f-235">Create fstab entries for hello three logical volumes</span></span>
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    <span data-ttu-id="0633f-236">A sor túl/etc/fstab beszúrása</span><span class="sxs-lookup"><span data-stu-id="0633f-236">Insert this line too/etc/fstab</span></span>
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * <span data-ttu-id="0633f-237">Csatlakoztassa hello új köteteket</span><span class="sxs-lookup"><span data-stu-id="0633f-237">Mount hello new volumes</span></span>
    <pre><code>
    sudo mount -a
    </code></pre>
    1. <span data-ttu-id="0633f-238">Egyszerű lemez</span><span class="sxs-lookup"><span data-stu-id="0633f-238">Plain Disks</span></span>  
       <span data-ttu-id="0633f-239">A kis vagy bemutató rendszerek, elhelyezhet egy lemezt a HANA adatainak és naplókönyvtárainak fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0633f-239">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="0633f-240">hello következő parancsok /dev/sdc hozza létre a partíciót, és formázza xfs.</span><span class="sxs-lookup"><span data-stu-id="0633f-240">hello following commands create a partition on /dev/sdc and format it with xfs.</span></span>
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-hello-id-of-devsdc1"></a><span data-ttu-id="0633f-241">Írja le /dev/sdc1 hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="0633f-241">write down hello id of /dev/sdc1</span></span>
    <span data-ttu-id="0633f-242">sudo/sbin/blkid sudo vi/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="0633f-242">sudo /sbin/blkid  sudo vi /etc/fstab</span></span>
    ```

    Insert this line too/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create hello target directory and mount hello disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. <span data-ttu-id="0633f-243">[A] állomásnév telepítési állomások</span><span class="sxs-lookup"><span data-stu-id="0633f-243">[A] Setup host name resolution for all hosts</span></span>  
    <span data-ttu-id="0633f-244">DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton.</span><span class="sxs-lookup"><span data-stu-id="0633f-244">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="0633f-245">Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="0633f-245">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="0633f-246">Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév</span><span class="sxs-lookup"><span data-stu-id="0633f-246">Replace hello IP address and hello hostname in hello following commands</span></span>
    ```bash
    sudo vi /etc/hosts
    ```
    <span data-ttu-id="0633f-247">Helyezze be a következő sorokat túl/etc/hosts hello.</span><span class="sxs-lookup"><span data-stu-id="0633f-247">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="0633f-248">Hello IP cím és az állomásnév toomatch a környezet módosítása</span><span class="sxs-lookup"><span data-stu-id="0633f-248">Change hello IP address and hostname toomatch your environment</span></span>    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. <span data-ttu-id="0633f-249">[1] fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="0633f-249">[1] Install Cluster</span></span>
    ```bash
    sudo ha-cluster-init
    
    # Do you want toocontinue anyway? [y/N] -> y
    # Network address toobind too(e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish toouse SBD? [y/N] -> N
    # Do you wish tooconfigure an administration IP? [y/N] -> N
    ```
        
1. <span data-ttu-id="0633f-250">[2] csomópont toocluster hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0633f-250">[2] Add node toocluster</span></span>
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured toostart at system boot.
    # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
    # Do you want toocontinue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. <span data-ttu-id="0633f-251">[A] módosítása hacluster jelszó toohello ugyanazt a jelszót</span><span class="sxs-lookup"><span data-stu-id="0633f-251">[A] Change hacluster password toohello same password</span></span>
    ```bash
    sudo passwd hacluster
    
    ```

1. <span data-ttu-id="0633f-252">[A] konfigurálása corosync toouse egyéb átvitelt, és adja hozzá a csomópontlista.</span><span class="sxs-lookup"><span data-stu-id="0633f-252">[A] Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="0633f-253">Fürt egyébként nem működik.</span><span class="sxs-lookup"><span data-stu-id="0633f-253">Cluster will not work otherwise.</span></span>
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    <span data-ttu-id="0633f-254">Adja hozzá a következő félkövér tartalom toohello fájl hello.</span><span class="sxs-lookup"><span data-stu-id="0633f-254">Add hello following bold content toohello file.</span></span>
    
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

    <span data-ttu-id="0633f-255">Indítsa újra hello corosync szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="0633f-255">Then restart hello corosync service</span></span>

    ```bash
    sudo service corosync restart
    
    ```

1. <span data-ttu-id="0633f-256">[A] HANA magas rendelkezésre ÁLLÁSÚ csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="0633f-256">[A] Install HANA HA packages</span></span>  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a><span data-ttu-id="0633f-257">SAP HANA telepítése</span><span class="sxs-lookup"><span data-stu-id="0633f-257">Installing SAP HANA</span></span>

<span data-ttu-id="0633f-258">Hajtsa végre a hello 4 fejezete [SAP HANA SR teljesítmény optimalizált forgatókönyv útmutató] [ suse-hana-ha-guide] tooinstall SAP HANA replikációs.</span><span class="sxs-lookup"><span data-stu-id="0633f-258">Follow chapter 4 of hello [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] tooinstall SAP HANA System Replication.</span></span>

1. <span data-ttu-id="0633f-259">[A] hdblcm-ről futtatva hello HANA DVD-ről</span><span class="sxs-lookup"><span data-stu-id="0633f-259">[A] Run hdblcm from hello HANA DVD</span></span>
    * <span data-ttu-id="0633f-260">Válassza ki a telepítés-1 ></span><span class="sxs-lookup"><span data-stu-id="0633f-260">Choose installation -> 1</span></span>
    * <span data-ttu-id="0633f-261">Válassza ki a további összetevők telepítésének 1-></span><span class="sxs-lookup"><span data-stu-id="0633f-261">Select additional components for installation -> 1</span></span>
    * <span data-ttu-id="0633f-262">Adja meg a telepítési útvonalat [/ hana/megosztott]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-262">Enter Installation Path [/hana/shared]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-263">Adja meg a helyi állomás nevét [.]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-263">Enter Local Host Name [..]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-264">Szeretné tooadd további állomások toohello rendszer?</span><span class="sxs-lookup"><span data-stu-id="0633f-264">Do you want tooadd additional hosts toohello system?</span></span> <span data-ttu-id="0633f-265">(i/n) [n]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-265">(y/n) [n]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-266">Adja meg az SAP HANA-azonosító:<SID of HANA e.g. HDB></span><span class="sxs-lookup"><span data-stu-id="0633f-266">Enter SAP HANA System ID: <SID of HANA e.g. HDB></span></span>
    * <span data-ttu-id="0633f-267">Adja meg a [00] száma:</span><span class="sxs-lookup"><span data-stu-id="0633f-267">Enter Instance Number [00]:</span></span>   
  <span data-ttu-id="0633f-268">HANA példányszámának.</span><span class="sxs-lookup"><span data-stu-id="0633f-268">HANA Instance number.</span></span> <span data-ttu-id="0633f-269">03 használja, ha követte a fenti példa hello vagy használt hello Azure-sablon</span><span class="sxs-lookup"><span data-stu-id="0633f-269">Use 03 if you used hello Azure Template or followed hello example above</span></span>
    * <span data-ttu-id="0633f-270">Válassza ki az adatbázis-mód / Index [1] adja meg: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-270">Select Database Mode / Enter Index [1]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-271">Válassza ki a rendszer használati / adja meg az Index [4]:</span><span class="sxs-lookup"><span data-stu-id="0633f-271">Select System Usage / Enter Index [4]:</span></span>  
  <span data-ttu-id="0633f-272">Válassza ki a hello rendszer kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="0633f-272">Select hello system Usage</span></span>
    * <span data-ttu-id="0633f-273">Adja meg a helyet, az adatkötetek [/ hana/data/HDB]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-273">Enter Location of Data Volumes [/hana/data/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-274">Adja meg a naplózási kötetek [/ hana/napló/HDB] helyet: ENTER -></span><span class="sxs-lookup"><span data-stu-id="0633f-274">Enter Location of Log Volumes [/hana/log/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-275">Maximális memória kiosztása korlátozása?</span><span class="sxs-lookup"><span data-stu-id="0633f-275">Restrict maximum memory allocation?</span></span> <span data-ttu-id="0633f-276">[n]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-276">[n]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-277">Adja meg a tanúsítvány gazdagép neve a gazdagép "..." []: -> Adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-277">Enter Certificate Host Name For Host '...' [...]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-278">Adja meg a SAP ügynök felhasználói (sapadm) jelszavát:</span><span class="sxs-lookup"><span data-stu-id="0633f-278">Enter SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="0633f-279">SAP ügynök felhasználói (sapadm) jelszó megerősítése:</span><span class="sxs-lookup"><span data-stu-id="0633f-279">Confirm SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="0633f-280">Adja meg a rendszergazda (hdbadm) jelszavát:</span><span class="sxs-lookup"><span data-stu-id="0633f-280">Enter System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="0633f-281">Erősítse meg a rendszergazda (hdbadm) jelszavát:</span><span class="sxs-lookup"><span data-stu-id="0633f-281">Confirm System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="0633f-282">Adja meg a rendszer rendszergazdai kezdőkönyvtár [/ usr/sap/HDB/home]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-282">Enter System Administrator Home Directory [/usr/sap/HDB/home]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-283">Adja meg a rendszer rendszergazdai bejelentkezési rendszerhéj [/ bin/sh]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-283">Enter System Administrator Login Shell [/bin/sh]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-284">Adja meg a rendszergazda felhasználói Azonosítóját [1001]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-284">Enter System Administrator User ID [1001]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-285">Adjon meg azonosító a felhasználói csoport (sapsys) [79]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-285">Enter ID of User Group (sapsys) [79]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-286">Adja meg az adatbázis jelszó (rendszer):</span><span class="sxs-lookup"><span data-stu-id="0633f-286">Enter Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="0633f-287">Adatbázis (rendszer) felhasználói jelszó megerősítése:</span><span class="sxs-lookup"><span data-stu-id="0633f-287">Confirm Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="0633f-288">Számítógép újraindítása után indítsa újra a rendszert?</span><span class="sxs-lookup"><span data-stu-id="0633f-288">Restart system after machine reboot?</span></span> <span data-ttu-id="0633f-289">[n]: -> adjon meg</span><span class="sxs-lookup"><span data-stu-id="0633f-289">[n]: -> ENTER</span></span>
    * <span data-ttu-id="0633f-290">Szeretné toocontinue?</span><span class="sxs-lookup"><span data-stu-id="0633f-290">Do you want toocontinue?</span></span> <span data-ttu-id="0633f-291">(i/n):</span><span class="sxs-lookup"><span data-stu-id="0633f-291">(y/n):</span></span>  
  <span data-ttu-id="0633f-292">Hello összefoglaló, és írja be a y toocontinue</span><span class="sxs-lookup"><span data-stu-id="0633f-292">Validate hello summary and enter y toocontinue</span></span>
1. <span data-ttu-id="0633f-293">[A] frissítési SAP Gazdagépügynöke</span><span class="sxs-lookup"><span data-stu-id="0633f-293">[A] Upgrade SAP Host Agent</span></span>  
  <span data-ttu-id="0633f-294">Hello legújabb SAP a gazdagép ügynöke archív letöltését hello [SAP Softwarecenter] [ sap-swcenter] és futtatási hello parancs tooupgrade hello ügynök következő.</span><span class="sxs-lookup"><span data-stu-id="0633f-294">Download hello latest SAP Host Agent archive from hello [SAP Softwarecenter][sap-swcenter] and run hello following command tooupgrade hello agent.</span></span> <span data-ttu-id="0633f-295">Cserélje le a hello elérési toohello toopoint toohello archívumfájl letöltött.</span><span class="sxs-lookup"><span data-stu-id="0633f-295">Replace hello path toohello archive toopoint toohello file you downloaded.</span></span>
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path tooSAP Host Agent SAR>
    ```

1. <span data-ttu-id="0633f-296">[1] replikációs HANA létrehozása (a legfelső szintű)</span><span class="sxs-lookup"><span data-stu-id="0633f-296">[1] Create HANA replication (as root)</span></span>  
    <span data-ttu-id="0633f-297">Futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="0633f-297">Run hello following command.</span></span> <span data-ttu-id="0633f-298">Győződjön meg arról, hogy tooreplace félkövér karakterláncok (HANA rendszer azonosító HDB és példány újrahasznosítása 03) a SAP HANA-telepítés hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="0633f-298">Make sure tooreplace bold strings (HANA System ID HDB and instance number 03) with hello values of your SAP HANA installation.</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. <span data-ttu-id="0633f-299">[A] létrehozása keystore bejegyzés (root)</span><span class="sxs-lookup"><span data-stu-id="0633f-299">[A] Create keystore entry (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. <span data-ttu-id="0633f-300">[1] backup database (a legfelső szintű)</span><span class="sxs-lookup"><span data-stu-id="0633f-300">[1] Backup database (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. <span data-ttu-id="0633f-301">[1] felhasználóváltás toohello sapsid (például hdbadm), és hozzon létre hello elsődleges hely.</span><span class="sxs-lookup"><span data-stu-id="0633f-301">[1] Switch toohello sapsid user (for example hdbadm) and create hello primary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. <span data-ttu-id="0633f-302">[2] váltson toohello sapsid felhasználói (például hdbadm), és hello másodlagos hely létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0633f-302">[2] Switch toohello sapsid user (for example hdbadm) and create hello secondary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a><span data-ttu-id="0633f-303">Fürt keretrendszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0633f-303">Configure Cluster Framework</span></span>

<span data-ttu-id="0633f-304">Hello alapértelmezett beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="0633f-304">Change hello default settings</span></span>

<pre>
sudo vi crm-defaults.txt
# enter hello following toocrm-defaults.txt
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

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="0633f-305">most hello toohello fájlfürt betöltés</span><span class="sxs-lookup"><span data-stu-id="0633f-305">now we load hello file toohello cluster</span></span>
<span data-ttu-id="0633f-306">sudo crm terhelés frissítés crm-defaults.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0633f-306">sudo crm configure load update crm-defaults.txt</span></span>
</pre>

### <a name="create-stonith-device"></a><span data-ttu-id="0633f-307">STONITH eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-307">Create STONITH device</span></span>

<span data-ttu-id="0633f-308">hello STONITH eszköz által használt egy egyszerű tooauthorize Microsoft Azure ellen.</span><span class="sxs-lookup"><span data-stu-id="0633f-308">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="0633f-309">Kérjük, kövesse ezeket a lépéseket toocreate egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0633f-309">Please follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="0633f-310">Nyissa meg túl<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="0633f-310">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="0633f-311">Nyissa meg hello Azure Active Directory panel</span><span class="sxs-lookup"><span data-stu-id="0633f-311">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="0633f-312">Nyissa meg tooProperties, és írja le hello Directory azonosítóját. Ez a hello **bérlőazonosító**.</span><span class="sxs-lookup"><span data-stu-id="0633f-312">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="0633f-313">Kattintson az alkalmazás-regisztráció</span><span class="sxs-lookup"><span data-stu-id="0633f-313">Click App registrations</span></span>
1. <span data-ttu-id="0633f-314">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="0633f-314">Click Add</span></span>
1. <span data-ttu-id="0633f-315">Adjon meg egy nevet, válassza ki a "Web app/API" alkalmazástípus, adja meg a bejelentkezési URL-címet (például http://localhost) és kattintson a Létrehozás gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-315">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="0633f-316">hello bejelentkezési URL-címet nem használja, és bármilyen érvényes URL-CÍMEK lehetnek</span><span class="sxs-lookup"><span data-stu-id="0633f-316">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="0633f-317">Válassza ki az új alkalmazás hello és kattintson a kulcsok hello-beállítások lap</span><span class="sxs-lookup"><span data-stu-id="0633f-317">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="0633f-318">Adja meg egy új kulcs leírását, válassza a "Soha nem jár le", és kattintson a Mentés gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-318">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="0633f-319">Írja le hello érték.</span><span class="sxs-lookup"><span data-stu-id="0633f-319">Write down hello Value.</span></span> <span data-ttu-id="0633f-320">Hello használják **jelszó** a hello szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="0633f-320">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="0633f-321">Írja le hello azonosítót. Hello felhasználónév, a rendszer (**bejelentkezési azonosító** hello lépéseket a) a hello szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="0633f-321">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="0633f-322">hello szolgáltatás egyszerű engedélyek tooaccess az Azure-erőforrások alapértelmezés szerint nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0633f-322">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="0633f-323">Toogive hello szolgáltatás egyszerű engedélyek toostart van szüksége, és állítsa (felszabadítása) hello fürt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="0633f-323">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="0633f-324">Nyissa meg toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="0633f-324">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="0633f-325">Nyissa meg az összes erőforrás panel hello</span><span class="sxs-lookup"><span data-stu-id="0633f-325">Open hello All resources blade</span></span>
1. <span data-ttu-id="0633f-326">Válassza ki a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="0633f-326">Select hello virtual machine</span></span>
1. <span data-ttu-id="0633f-327">Kattintson a hozzáférés-vezérlés (IAM)</span><span class="sxs-lookup"><span data-stu-id="0633f-327">Click Access control (IAM)</span></span>
1. <span data-ttu-id="0633f-328">Kattintson az Add (Hozzáadás) parancsra</span><span class="sxs-lookup"><span data-stu-id="0633f-328">Click Add</span></span>
1. <span data-ttu-id="0633f-329">Válassza ki a hello szerepkör tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="0633f-329">Select hello role Owner</span></span>
1. <span data-ttu-id="0633f-330">Adja meg az előbb létrehozott hello alkalmazás hello neve</span><span class="sxs-lookup"><span data-stu-id="0633f-330">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="0633f-331">Kattintson az OK gombra</span><span class="sxs-lookup"><span data-stu-id="0633f-331">Click OK</span></span>

<span data-ttu-id="0633f-332">Után szerkeszteni hello engedélyek hello virtuális gépekhez, beállíthatja a hello STONITH eszközök hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="0633f-332">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre>
sudo vi crm-fencing.txt
# enter hello following toocrm-fencing.txt
# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="0633f-333">most hello toohello fájlfürt betöltés</span><span class="sxs-lookup"><span data-stu-id="0633f-333">now we load hello file toohello cluster</span></span>
<span data-ttu-id="0633f-334">sudo crm terhelés frissítés crm-fencing.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0633f-334">sudo crm configure load update crm-fencing.txt</span></span>
</pre>

### <a name="create-sap-hana-resources"></a><span data-ttu-id="0633f-335">SAP HANA-erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="0633f-335">Create SAP HANA resources</span></span>

<pre>
sudo vi crm-saphanatop.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number and HANA system id
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

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="0633f-336">most hello toohello fájlfürt betöltés</span><span class="sxs-lookup"><span data-stu-id="0633f-336">now we load hello file toohello cluster</span></span>
<span data-ttu-id="0633f-337">sudo crm terhelés frissítés crm-saphanatop.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0633f-337">sudo crm configure load update crm-saphanatop.txt</span></span>
</pre>

<pre>
sudo vi crm-saphana.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
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

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="0633f-338">most hello toohello fájlfürt betöltés</span><span class="sxs-lookup"><span data-stu-id="0633f-338">now we load hello file toohello cluster</span></span>
<span data-ttu-id="0633f-339">sudo crm terhelés frissítés crm-saphana.txt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0633f-339">sudo crm configure load update crm-saphana.txt</span></span>
</pre>

### <a name="test-cluster-setup"></a><span data-ttu-id="0633f-340">Teszt fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="0633f-340">Test cluster setup</span></span>
<span data-ttu-id="0633f-341">a következő fejezet hello ismertetik, hogyan tesztelheti a telepítőt.</span><span class="sxs-lookup"><span data-stu-id="0633f-341">hello following chapter describe how you can test your setup.</span></span> <span data-ttu-id="0633f-342">Minden teszt feltételezi, hogy a legfelső szintű áll, és hello SAP HANA fő fut a virtuális gép saphanavm1 hello.</span><span class="sxs-lookup"><span data-stu-id="0633f-342">Every test assumes that you are root and hello SAP HANA master is running on hello virtual machine saphanavm1.</span></span>

#### <a name="fencing-test"></a><span data-ttu-id="0633f-343">Teszt kerítésépítés</span><span class="sxs-lookup"><span data-stu-id="0633f-343">Fencing Test</span></span>

<span data-ttu-id="0633f-344">A csomópont saphanavm1 hello hálózati adapter letiltásával hello beállítása hello kerítés ügynök tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="0633f-344">You can test hello setup of hello fencing agent by disabling hello network interface on node saphanavm1.</span></span>

<pre><code>
sudo ifdown eth0
</code></pre>

<span data-ttu-id="0633f-345">hello virtuális gép kell most beolvasása újraindult, vagy attól függően, hogy a fürt konfigurációját, leállt.</span><span class="sxs-lookup"><span data-stu-id="0633f-345">hello virtual machine should now get restarted or stopped depending on your cluster configuration.</span></span>
<span data-ttu-id="0633f-346">Ha hello stonith műveleti toooff, hello virtuális gép leáll és hello erőforrások áttelepített toohello virtuális gépet futtat.</span><span class="sxs-lookup"><span data-stu-id="0633f-346">If you set hello stonith-action toooff, hello virtual machine will be stopped and hello resources are migrated toohello running virtual machine.</span></span>

<span data-ttu-id="0633f-347">Hello virtuális gép ismételt futtatása, ha hello SAP HANA-erőforrás sikertelen lesz toostart másodlagosként Ha AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="0633f-347">Once you start hello virtual machine again, hello SAP HANA resource will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="0633f-348">Ebben az esetben szüksége tooconfigure hello HANA példányához, másodlagos a következő hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0633f-348">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a><span data-ttu-id="0633f-349">Manuális feladatátvétel tesztelése</span><span class="sxs-lookup"><span data-stu-id="0633f-349">Testing a manual failover</span></span>

<span data-ttu-id="0633f-350">Manuális feladatátvétel tesztelheti a csomópont saphanavm1 hello támasztja szolgáltatás leállításával.</span><span class="sxs-lookup"><span data-stu-id="0633f-350">You can test a manual failover by stopping hello pacemaker service on node saphanavm1.</span></span>
<pre><code>
service pacemaker stop
</code></pre>

<span data-ttu-id="0633f-351">Hello feladatátvétel után elindíthatja hello szolgáltatást újra.</span><span class="sxs-lookup"><span data-stu-id="0633f-351">After hello failover, you can start hello service again.</span></span> <span data-ttu-id="0633f-352">hello saphanavm1 SAP HANA erőforrás sikertelen lesz toostart másodlagosként Ha AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="0633f-352">hello SAP HANA resource on saphanavm1 will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="0633f-353">Ebben az esetben szüksége tooconfigure hello HANA példányához, másodlagos a következő hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0633f-353">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a><span data-ttu-id="0633f-354">Áttelepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0633f-354">Testing a migration</span></span>

<span data-ttu-id="0633f-355">Hello SAP HANA-főcsomópont telepíthet át a következő hello a következő parancs futtatásával</span><span class="sxs-lookup"><span data-stu-id="0633f-355">You can migrate hello SAP HANA master node by executing hello following command</span></span>
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="0633f-356">Ez kell telepítenie a hello SAP HANA-főcsomópont és hello virtuális IP-cím toosaphanavm2 tartalmazó hello csoport.</span><span class="sxs-lookup"><span data-stu-id="0633f-356">This should migrate hello SAP HANA master node and hello group that contains hello virtual IP address toosaphanavm2.</span></span>
<span data-ttu-id="0633f-357">hello saphanavm1 SAP HANA erőforrás sikertelen lesz toostart másodlagosként Ha AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="0633f-357">hello SAP HANA resource on saphanavm1 will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="0633f-358">Ebben az esetben szüksége tooconfigure hello HANA példányához, másodlagos a következő hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0633f-358">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

<span data-ttu-id="0633f-359">hello áttelepítés törölt újra toobe igénylő hely contraints hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0633f-359">hello migration creates location contraints that need toobe deleted again.</span></span>

<pre><code>
crm configure edited

# delete location contraints that are named like hello following contraint. You should have two contraints, one for hello SAP HANA resource and one for hello IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="0633f-360">Szükség hello másodlagos csomópont erőforrás toocleanup hello állapotát</span><span class="sxs-lookup"><span data-stu-id="0633f-360">You also need toocleanup hello state of hello secondary node resource</span></span>

<pre><code>
# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a><span data-ttu-id="0633f-361">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0633f-361">Next steps</span></span>
* <span data-ttu-id="0633f-362">[Az Azure virtuális gépek tervezési és megvalósítási az SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="0633f-362">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="0633f-363">[Az SAP Azure virtuális gépek telepítése][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="0633f-363">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="0633f-364">[Az SAP Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="0633f-364">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="0633f-365">Hogyan tooestablish magas rendelkezésre állású és az Azure (nagy példányokat), az SAP HANA vész-helyreállítási terv: toolearn [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="0633f-365">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span> 
