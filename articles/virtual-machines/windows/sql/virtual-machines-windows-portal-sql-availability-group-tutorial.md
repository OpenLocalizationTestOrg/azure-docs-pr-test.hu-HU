---
title: "Kiszolgáló rendelkezésre állási csoportok – Azure virtuális gépek - oktatóanyag aaaSQL |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan toocreate egy SQL Server mindig a rendelkezésre állási csoport Azure virtuális gépeken."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 65b4210b0f851828a32a02053b03e4b8d469ba4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a><span data-ttu-id="24b19-103">Konfigurálás mindig a rendelkezésre állási csoport az Azure virtuális gép manuálisan</span><span class="sxs-lookup"><span data-stu-id="24b19-103">Configure Always On Availability Group in Azure VM manually</span></span>

<span data-ttu-id="24b19-104">Ez az oktatóanyag bemutatja, hogyan toocreate egy SQL Server mindig a rendelkezésre állási csoport Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="24b19-104">This tutorial shows how toocreate a SQL Server Always On Availability Group on Azure Virtual Machines.</span></span> <span data-ttu-id="24b19-105">hello teljes oktatóanyag egy rendelkezésre állási csoportban két SQL-kiszolgálón az adatbázis-replikát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="24b19-105">hello complete tutorial creates an Availability Group with a database replica on two SQL Servers.</span></span>

<span data-ttu-id="24b19-106">**Becsült idő**: toocomplete körülbelül 30 percet vesz igénybe, ha hello előfeltételek teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="24b19-106">**Time estimate**: Takes about 30 minutes toocomplete once hello prerequisites are met.</span></span>

<span data-ttu-id="24b19-107">hello ábra szemlélteti a hello oktatóanyag felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="24b19-107">hello diagram illustrates what you build in hello tutorial.</span></span>

![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a><span data-ttu-id="24b19-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="24b19-109">Prerequisites</span></span>

<span data-ttu-id="24b19-110">hello oktatóanyag feltételezi, hogy rendelkezik-e az SQL Server Always On rendelkezésre állási csoportokkal kapcsolatos alapvető ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="24b19-110">hello tutorial assumes you have a basic understanding of SQL Server Always On Availability Groups.</span></span> <span data-ttu-id="24b19-111">Ha további tájékoztatásra van szüksége, tekintse meg [az mindig a rendelkezésre állási csoportok áttekintése (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span><span class="sxs-lookup"><span data-stu-id="24b19-111">If you need more information, see [Overview of Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span></span>

<span data-ttu-id="24b19-112">hello következő táblázat sorolja fel, hogy az oktatóanyag elindítása előtt kell toocomplete hello Előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="24b19-112">hello following table lists hello prerequisites that you need toocomplete before starting this tutorial:</span></span>

|  |<span data-ttu-id="24b19-113">Követelmény</span><span class="sxs-lookup"><span data-stu-id="24b19-113">Requirement</span></span> |<span data-ttu-id="24b19-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="24b19-114">Description</span></span> |
|----- |----- |----- |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | <span data-ttu-id="24b19-116">Két SQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="24b19-116">Two SQL Servers</span></span> | <span data-ttu-id="24b19-117">-Az Azure rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="24b19-117">- In an Azure availability set</span></span> <br/> <span data-ttu-id="24b19-118">-Egyetlen tartományban</span><span class="sxs-lookup"><span data-stu-id="24b19-118">- In a single domain</span></span> <br/> <span data-ttu-id="24b19-119">-A Feladatátvételi fürtszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="24b19-119">- With Failover Clustering feature installed</span></span> |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| <span data-ttu-id="24b19-121">Windows Server</span><span class="sxs-lookup"><span data-stu-id="24b19-121">Windows Server</span></span> | <span data-ttu-id="24b19-122">A fürt tanúsító fájlmegosztás</span><span class="sxs-lookup"><span data-stu-id="24b19-122">File share for cluster witness</span></span> |  
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="24b19-124">SQL Server-szolgáltatásfiók</span><span class="sxs-lookup"><span data-stu-id="24b19-124">SQL Server service account</span></span> | <span data-ttu-id="24b19-125">Tartományi fiók</span><span class="sxs-lookup"><span data-stu-id="24b19-125">Domain account</span></span> |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="24b19-127">SQL Server Agent szolgáltatásfiók használatával</span><span class="sxs-lookup"><span data-stu-id="24b19-127">SQL Server Agent service account</span></span> | <span data-ttu-id="24b19-128">Tartományi fiók</span><span class="sxs-lookup"><span data-stu-id="24b19-128">Domain account</span></span> |  
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="24b19-130">Tűzfal portjainak megnyitása</span><span class="sxs-lookup"><span data-stu-id="24b19-130">Firewall ports open</span></span> | <span data-ttu-id="24b19-131">-SQL kiszolgáló: **1433** az alapértelmezett példány esetén</span><span class="sxs-lookup"><span data-stu-id="24b19-131">- SQL Server: **1433** for default instance</span></span> <br/> <span data-ttu-id="24b19-132">-Adatbázis-tükrözési végpontját: **5022** vagy minden elérhető port</span><span class="sxs-lookup"><span data-stu-id="24b19-132">- Database mirroring endpoint: **5022** or any available port</span></span> <br/> <span data-ttu-id="24b19-133">-Azure terheléselosztói mintavétel: **59999** vagy minden elérhető port</span><span class="sxs-lookup"><span data-stu-id="24b19-133">- Azure load balancer probe: **59999** or any available port</span></span> |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="24b19-135">Adja hozzá a Feladatátvételi fürtszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="24b19-135">Add Failover Clustering Feature</span></span> | <span data-ttu-id="24b19-136">Mindkét SQL Server szükség erre a szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="24b19-136">Both SQL Servers require this feature</span></span> |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="24b19-138">Telepítési tartományi fiók</span><span class="sxs-lookup"><span data-stu-id="24b19-138">Installation domain account</span></span> | <span data-ttu-id="24b19-139">-Minden egyes SQL Server helyi rendszergazdája</span><span class="sxs-lookup"><span data-stu-id="24b19-139">- Local administrator on each SQL Server</span></span> <br/> <span data-ttu-id="24b19-140">– SQL Server SysAdmin (rendszergazda) rögzített kiszolgálói szerepkör az SQL Server minden példányának tagja</span><span class="sxs-lookup"><span data-stu-id="24b19-140">- Member of SQL Server sysadmin fixed server role for each instance of SQL Server</span></span>  |


<span data-ttu-id="24b19-141">Hello oktatóanyag elkezdéséhez kell túl[végezze el az Azure virtuális gépek létrehozását a Always On rendelkezésre állási csoportok használatának előfeltételei](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span><span class="sxs-lookup"><span data-stu-id="24b19-141">Before you begin hello tutorial, you need too[Complete prerequisites for creating Always On Availability Groups in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span></span> <span data-ttu-id="24b19-142">Ha az Előfeltételek már megadta, de is ugorhat túl[fürt létrehozása](#CreateCluster).</span><span class="sxs-lookup"><span data-stu-id="24b19-142">If these prerequisites are completed already, you can jump too[Create Cluster](#CreateCluster).</span></span>


<span data-ttu-id="24b19-143"><!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="24b19-143"><!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## Create hello cluster</span></span>

<span data-ttu-id="24b19-144">Hello előfeltételeinek teljesítésekor, miután hello első lépése toocreate tartalmazó Windows Server feladatátvevő fürt két SQL-kiszolgálója és a tanúsító kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="24b19-144">After hello prerequisites are completed, hello first step is toocreate a Windows Server Failover Cluster that includes two SQL Severs and a witness server.</span></span>  

1. <span data-ttu-id="24b19-145">RDP toohello első SQL Server SQL Server-kiszolgálók és a hello tanúsító kiszolgáló egyik rendszergazdájának tartományi fiókkal.</span><span class="sxs-lookup"><span data-stu-id="24b19-145">RDP toohello first SQL Server using a domain account that is an administrator on both SQL Servers and hello witness server.</span></span>

   >[!TIP]
   ><span data-ttu-id="24b19-146">Ha követte hello [Előfeltételek dokumentumát](virtual-machines-windows-portal-sql-availability-group-prereq.md), nevű létrehozott **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="24b19-146">If you followed hello [prerequisites document](virtual-machines-windows-portal-sql-availability-group-prereq.md), you created an account called **CORP\Install**.</span></span> <span data-ttu-id="24b19-147">Használja ezt a fiókot.</span><span class="sxs-lookup"><span data-stu-id="24b19-147">Use this account.</span></span>

2. <span data-ttu-id="24b19-148">A hello **Kiszolgálókezelő** jelölje be az irányítópult **eszközök**, és kattintson a **Feladatátvevőfürt-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-148">In hello **Server Manager** dashboard, select **Tools**, and then click **Failover Cluster Manager**.</span></span>
3. <span data-ttu-id="24b19-149">Hello bal oldali ablaktáblában kattintson a jobb gombbal **Feladatátvevőfürt-kezelő**, és kattintson a **hozzon létre egy fürtöt**.</span><span class="sxs-lookup"><span data-stu-id="24b19-149">In hello left pane, right-click **Failover Cluster Manager**, and then click **Create a Cluster**.</span></span>
   <span data-ttu-id="24b19-150">![Fürt létrehozása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span><span class="sxs-lookup"><span data-stu-id="24b19-150">![Create Cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span></span>
4. <span data-ttu-id="24b19-151">A fürt létrehozása varázsló hello, hozzon létre egy egy csomópontos fürtre lépéseit a következő táblázat hello hello beállításokkal hello lapok végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="24b19-151">In hello Create Cluster Wizard, create a one-node cluster by stepping through hello pages with hello settings in hello following table:</span></span>

   | <span data-ttu-id="24b19-152">Lap</span><span class="sxs-lookup"><span data-stu-id="24b19-152">Page</span></span> | <span data-ttu-id="24b19-153">Beállítások</span><span class="sxs-lookup"><span data-stu-id="24b19-153">Settings</span></span> |
   | --- | --- |
   | <span data-ttu-id="24b19-154">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="24b19-154">Before You Begin</span></span> |<span data-ttu-id="24b19-155">Alapértelmezések használata</span><span class="sxs-lookup"><span data-stu-id="24b19-155">Use defaults</span></span> |
   | <span data-ttu-id="24b19-156">Kiszolgálók kiválasztása</span><span class="sxs-lookup"><span data-stu-id="24b19-156">Select Servers</span></span> |<span data-ttu-id="24b19-157">Első SQL-kiszolgáló neve a típus hello **kiszolgáló nevének megadása** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-157">Type hello first SQL Server name in **Enter server name** and click **Add**.</span></span> |
   | <span data-ttu-id="24b19-158">Érvényesítési figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="24b19-158">Validation Warning</span></span> |<span data-ttu-id="24b19-159">Válassza ki **szám I nem igényel Microsoft-támogatást ehhez a fürthöz, és ezért nem kívánja toorun hello Érvényesítési tesztek. Ha a Tovább gombra kattintva folytatni annak létrehozását hello fürt**.</span><span class="sxs-lookup"><span data-stu-id="24b19-159">Select **No. I do not require support from Microsoft for this cluster, and therefore do not want toorun hello validation tests. When I click Next, continue Creating hello cluster**.</span></span> |
   | <span data-ttu-id="24b19-160">Administering hello fürt hozzáférési pont</span><span class="sxs-lookup"><span data-stu-id="24b19-160">Access Point for Administering hello Cluster</span></span> |<span data-ttu-id="24b19-161">Írja be a fürt nevét, például **SQLAGCluster1** a **fürtnév**.</span><span class="sxs-lookup"><span data-stu-id="24b19-161">Type a cluster name, for example **SQLAGCluster1** in **Cluster Name**.</span></span>|
   | <span data-ttu-id="24b19-162">Megerősítés</span><span class="sxs-lookup"><span data-stu-id="24b19-162">Confirmation</span></span> |<span data-ttu-id="24b19-163">Használhatja az alapértelmezett értékeket, kivéve, ha a tárolóhelyek használata.</span><span class="sxs-lookup"><span data-stu-id="24b19-163">Use defaults unless you are using Storage Spaces.</span></span> <span data-ttu-id="24b19-164">Lásd a táblázat utáni megjegyzést hello.</span><span class="sxs-lookup"><span data-stu-id="24b19-164">See hello note following this table.</span></span> |

### <a name="set-hello-cluster-ip-address"></a><span data-ttu-id="24b19-165">Hello fürt IP-cím beállítása</span><span class="sxs-lookup"><span data-stu-id="24b19-165">Set hello cluster IP address</span></span>

1. <span data-ttu-id="24b19-166">A **Feladatátvevőfürt-kezelő**, görgessen lefelé, túl**fürt alapvető erőforrásai** , és bontsa ki a hello fürt adatait.</span><span class="sxs-lookup"><span data-stu-id="24b19-166">In **Failover Cluster Manager**, scroll down too**Cluster Core Resources** and expand hello cluster details.</span></span> <span data-ttu-id="24b19-167">Mindkét hello kell megjelennie **neve** és hello **IP-cím** hello erőforrások **sikertelen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="24b19-167">You should see both hello **Name** and hello **IP Address** resources in hello **Failed** state.</span></span> <span data-ttu-id="24b19-168">hello IP-cím erőforrás nem kell online állapotba, mert hello fürt hozzá van rendelve a hello azonos IP-maga hello gépként cím, ezért a ismétlődő címet.</span><span class="sxs-lookup"><span data-stu-id="24b19-168">hello IP address resource cannot be brought online because hello cluster is assigned hello same IP address as hello machine itself, therefore it is a duplicate address.</span></span>

2. <span data-ttu-id="24b19-169">Nem sikerült, kattintson a jobb gombbal hello **IP-cím** erőforrás, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="24b19-169">Right-click hello failed **IP Address** resource, and then click **Properties**.</span></span>

   ![Fürt tulajdonságai](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. <span data-ttu-id="24b19-171">Válassza ki **statikus IP-cím** , és adja meg egy címet alhálózatból hello SQL Server esetén hello cím szövegmezőbe.</span><span class="sxs-lookup"><span data-stu-id="24b19-171">Select **Static IP Address** and specify an available address from subnet where hello SQL Server is in hello Address text box.</span></span> <span data-ttu-id="24b19-172">Kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="24b19-172">Then, click **OK**.</span></span>
4. <span data-ttu-id="24b19-173">A hello **fürt alapvető erőforrásai** szakaszt, kattintson a jobb gombbal a fürt nevét, és kattintson **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-173">In hello **Cluster Core Resources** section, right-click cluster name and click **Bring Online**.</span></span> <span data-ttu-id="24b19-174">Ezután Várjon, amíg az mindkét erőforrás online.</span><span class="sxs-lookup"><span data-stu-id="24b19-174">Then, wait until both resources are online.</span></span> <span data-ttu-id="24b19-175">Hello fürt hálózatnév-erőforrás online állapotba kerül, amikor új AD-számítógépfiók frissíti hello tartományvezérlő kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="24b19-175">When hello cluster name resource comes online, it updates hello DC server with a new AD computer account.</span></span> <span data-ttu-id="24b19-176">Az AD fiók toorun hello később a rendelkezésre állási csoport fürtözött szolgáltatást használja.</span><span class="sxs-lookup"><span data-stu-id="24b19-176">Use this AD account toorun hello Availability Group clustered service later.</span></span>

### <span data-ttu-id="24b19-177"><a name="addNode"></a>Adja hozzá más SQL Server toocluster hello</span><span class="sxs-lookup"><span data-stu-id="24b19-177"><a name="addNode"></a>Add hello other SQL Server toocluster</span></span>

<span data-ttu-id="24b19-178">Adja hozzá más SQL Server toohello fürt hello.</span><span class="sxs-lookup"><span data-stu-id="24b19-178">Add hello other SQL Server toohello cluster.</span></span>

1. <span data-ttu-id="24b19-179">A hello böngésző konzolfáján kattintson a jobb gombbal a hello fürt, majd kattintson **csomópont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="24b19-179">In hello browser tree, right-click hello cluster and click **Add Node**.</span></span>

    ![Fürt csomópont toohello hozzáadása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. <span data-ttu-id="24b19-181">A hello **csomópont hozzáadása varázsló**, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-181">In hello **Add Node Wizard**, click **Next**.</span></span> <span data-ttu-id="24b19-182">A hello **kiszolgálók kiválasztása** lapon hello második SQL Server.</span><span class="sxs-lookup"><span data-stu-id="24b19-182">In hello **Select Servers** page, add hello second SQL Server.</span></span> <span data-ttu-id="24b19-183">Hello server típusnévnek **kiszolgáló nevének megadása** majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-183">Type hello server name in **Enter server name** and then click **Add**.</span></span> <span data-ttu-id="24b19-184">Amikor elkészült, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-184">When you are done, click **Next**.</span></span>

1. <span data-ttu-id="24b19-185">A hello **érvényesítési figyelmeztetés** kattintson **nem** (éles forgatókönyv esetében végre kell hajtania hello ellenőrző teszteket).</span><span class="sxs-lookup"><span data-stu-id="24b19-185">In hello **Validation Warning** page, click **No** (in a production scenario you should perform hello validation tests).</span></span> <span data-ttu-id="24b19-186">Ezután kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-186">Then, click **Next**.</span></span>

8. <span data-ttu-id="24b19-187">A hello **megerősítő** lapon törölje a jelet hello jelölőnégyzetet a tárolóhelyek használata **adja hozzá az összes megfelelő tárolót toohello fürtöt.**</span><span class="sxs-lookup"><span data-stu-id="24b19-187">In hello **Confirmation** page if you are using Storage Spaces, clear hello checkbox labeled **Add all eligible storage toohello cluster.**</span></span>

   ![Vegye fel a csomópont megerősítése](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   ><span data-ttu-id="24b19-189">Ha a tárolóhelyek használ, és nem, törölje a jelet **adja hozzá az összes megfelelő tárolót toohello fürtöt**, a Windows hello virtuális lemezek szervezőről hello fürtszolgáltatás folyamat során.</span><span class="sxs-lookup"><span data-stu-id="24b19-189">If you are using Storage Spaces and do not uncheck **Add all eligible storage toohello cluster**, Windows detaches hello virtual disks during hello clustering process.</span></span> <span data-ttu-id="24b19-190">Ennek eredményeképpen csak akkor jelennek meg a Lemezkezelés vagy Explorer hello tárolóhelyek hello fürt el lesznek távolítva, és objektumkörnyezetben a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="24b19-190">As a result, they do not appear in Disk Manager or Explorer until hello storage spaces are removed from hello cluster and reattached using PowerShell.</span></span> <span data-ttu-id="24b19-191">A tárolóhelyek több lemez toostorage készletek felsorolását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="24b19-191">Storage Spaces groups multiple disks in toostorage pools.</span></span> <span data-ttu-id="24b19-192">További információkért lásd: [tárolóhelyek](https://technet.microsoft.com/library/hh831739).</span><span class="sxs-lookup"><span data-stu-id="24b19-192">For more information, see [Storage Spaces](https://technet.microsoft.com/library/hh831739).</span></span>

1. <span data-ttu-id="24b19-193">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-193">Click **Next**.</span></span>

1. <span data-ttu-id="24b19-194">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-194">Click **Finish**.</span></span>

   <span data-ttu-id="24b19-195">Feladatátvevőfürt-kezelő jeleníti meg, hogy a fürt egy új csomóponttal és sorolja fel azokat hello **csomópontok** tároló.</span><span class="sxs-lookup"><span data-stu-id="24b19-195">Failover Cluster Manager shows that your cluster has a new node and lists it in hello **Nodes** container.</span></span>

10. <span data-ttu-id="24b19-196">Jelentkezzen ki a távoli asztali munkamenetgazda hello.</span><span class="sxs-lookup"><span data-stu-id="24b19-196">Log out of hello remote desktop session.</span></span>

### <a name="add-a-cluster-quorum-file-share"></a><span data-ttu-id="24b19-197">A fürt kvórum fájlmegosztás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24b19-197">Add a cluster quorum file share</span></span>

<span data-ttu-id="24b19-198">Ebben a példában hello Windows fürt használja, a fájl megosztási toocreate a fürt kvórumát.</span><span class="sxs-lookup"><span data-stu-id="24b19-198">In this example hello Windows cluster uses a file share toocreate a cluster quorum.</span></span> <span data-ttu-id="24b19-199">Ez az oktatóanyag egy csomópont- és fájlmegosztástöbbség kvórum használja.</span><span class="sxs-lookup"><span data-stu-id="24b19-199">This tutorial uses a Node and File Share Majority quorum.</span></span> <span data-ttu-id="24b19-200">További információkért lásd: [a feladatátvevő fürtök kvórumkonfigurációinak ismertetése](http://technet.microsoft.com/library/cc731739.aspx).</span><span class="sxs-lookup"><span data-stu-id="24b19-200">For more information, see [Understanding Quorum Configurations in a Failover Cluster](http://technet.microsoft.com/library/cc731739.aspx).</span></span>

1. <span data-ttu-id="24b19-201">Csatlakozás toohello fájl megosztási tanúsító tagkiszolgáló egy távoli asztali munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="24b19-201">Connect toohello file share witness member server with a remote desktop session.</span></span>

1. <span data-ttu-id="24b19-202">A **Kiszolgálókezelő**, kattintson a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="24b19-202">On **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="24b19-203">Nyissa meg **számítógép-kezelés**.</span><span class="sxs-lookup"><span data-stu-id="24b19-203">Open **Computer Management**.</span></span>

1. <span data-ttu-id="24b19-204">Kattintson a **megosztott mappák**.</span><span class="sxs-lookup"><span data-stu-id="24b19-204">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="24b19-205">Kattintson a jobb gombbal **megosztások**, és kattintson a **új megosztás...** .</span><span class="sxs-lookup"><span data-stu-id="24b19-205">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="24b19-207">Használjon **megosztott mappa létrehozása varázsló** toocreate egy megosztást.</span><span class="sxs-lookup"><span data-stu-id="24b19-207">Use **Create a Shared Folder Wizard** toocreate a share.</span></span>

1. <span data-ttu-id="24b19-208">A **mappa elérési útja**, kattintson a **Tallózás** , és keresse meg, vagy hozzon létre egy hello megosztott mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="24b19-208">On **Folder Path**, click **Browse** and locate or create a path for hello shared folder.</span></span> <span data-ttu-id="24b19-209">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-209">Click **Next**.</span></span>

1. <span data-ttu-id="24b19-210">A **nevét, leírását és beállítások** hello nevével és elérési ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="24b19-210">In **Name, Description, and Settings** verify hello share name and path.</span></span> <span data-ttu-id="24b19-211">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-211">Click **Next**.</span></span>

1. <span data-ttu-id="24b19-212">A **megosztott mappákra vonatkozó engedélyek** beállítása **engedélyek testreszabását**.</span><span class="sxs-lookup"><span data-stu-id="24b19-212">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="24b19-213">Kattintson a **egyéni...** .</span><span class="sxs-lookup"><span data-stu-id="24b19-213">Click **Custom...**.</span></span>

1. <span data-ttu-id="24b19-214">A **engedélyek testreszabása**, kattintson a **hozzáadása...** .</span><span class="sxs-lookup"><span data-stu-id="24b19-214">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="24b19-215">Győződjön meg arról, hogy hello használt fiók toocreate hello fürt teljes körű vezérléssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="24b19-215">Make sure that hello account used toocreate hello cluster has full control.</span></span>

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. <span data-ttu-id="24b19-217">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-217">Click **OK**.</span></span>

1. <span data-ttu-id="24b19-218">A **megosztott mappákra vonatkozó engedélyek**, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="24b19-218">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="24b19-219">Kattintson a **Befejezés** újra.</span><span class="sxs-lookup"><span data-stu-id="24b19-219">Click **Finish** again.</span></span>  

1. <span data-ttu-id="24b19-220">Jelentkezzen ki hello kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="24b19-220">Log out of hello server</span></span>

### <a name="configure-cluster-quorum"></a><span data-ttu-id="24b19-221">A fürtkvórum konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24b19-221">Configure cluster quorum</span></span>

<span data-ttu-id="24b19-222">Következő lépésként állítsa hello fürt kvóruma.</span><span class="sxs-lookup"><span data-stu-id="24b19-222">Next, set hello cluster quorum.</span></span>

1. <span data-ttu-id="24b19-223">Csatlakozzon a távoli asztal toohello első fürtcsomópontra.</span><span class="sxs-lookup"><span data-stu-id="24b19-223">Connect toohello first cluster node with remote desktop.</span></span>

1. <span data-ttu-id="24b19-224">A **Feladatátvevőfürt-kezelő**, jobb gombbal az hello fürt túl**további műveletek**, és kattintson a **fürt kvórumbeállításainak megadása...** .</span><span class="sxs-lookup"><span data-stu-id="24b19-224">In **Failover Cluster Manager**, right-click hello cluster, point too**More Actions**, and click **Configure Cluster Quorum Settings...**.</span></span>

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. <span data-ttu-id="24b19-226">A **fürtkvórum beállítása varázslót**, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-226">In **Configure Cluster Quorum Wizard**, click **Next**.</span></span>

1. <span data-ttu-id="24b19-227">A **kvórumbeállítások kijelölése**, válassza a **hello kvórum tanúsítójának kijelölése**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-227">In **Select Quorum Configuration Option**, choose **Select hello quorum witness**, and click **Next**.</span></span>

1. <span data-ttu-id="24b19-228">A **kvórum Tanúsítójának kijelölése**, kattintson a **konfigurálása egy tanúsító fájlmegosztást**.</span><span class="sxs-lookup"><span data-stu-id="24b19-228">On **Select Quorum Witness**, click **Configure a file share witness**.</span></span>

   >[!TIP]
   ><span data-ttu-id="24b19-229">Windows Server 2016 felhő tanúsító támogatja.</span><span class="sxs-lookup"><span data-stu-id="24b19-229">Windows Server 2016 supports a cloud witness.</span></span> <span data-ttu-id="24b19-230">Ha úgy dönt, hogy az ilyen típusú tanúsító, nem kell egy fájl tanúsító fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="24b19-230">If you choose this type of witness, you do not need a file share witness.</span></span> <span data-ttu-id="24b19-231">További információkért lásd: [központi telepítése a feladatátvevő fürt tanúsító felhő](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="24b19-231">For more information, see [Deploy a cloud witness for a Failover Cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span> <span data-ttu-id="24b19-232">Ez az oktatóanyag egy tanúsító fájlmegosztást, amely korábbi operációs rendszerek által támogatott használja.</span><span class="sxs-lookup"><span data-stu-id="24b19-232">This tutorial uses a file share witness, which is supported by previous operating systems.</span></span>

1. <span data-ttu-id="24b19-233">A **tanúsító fájlmegosztás**, létrehozott hello megosztás típus hello elérési útja.</span><span class="sxs-lookup"><span data-stu-id="24b19-233">On **Configure File Share Witness**, type hello path for hello share you created.</span></span> <span data-ttu-id="24b19-234">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-234">Click **Next**.</span></span>

1. <span data-ttu-id="24b19-235">Ellenőrizze, hogy hello beállítások **megerősítő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-235">Verify hello settings on **Confirmation**.</span></span> <span data-ttu-id="24b19-236">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-236">Click **Next**.</span></span>

1. <span data-ttu-id="24b19-237">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-237">Click **Finish**.</span></span>

<span data-ttu-id="24b19-238">hello fürt alapvető erőforrásai vannak konfigurálva a tanúsító fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="24b19-238">hello cluster core resources are configured with a file share witness.</span></span>

## <a name="enable-availability-groups"></a><span data-ttu-id="24b19-239">Rendelkezésre állási csoportok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="24b19-239">Enable Availability Groups</span></span>

<span data-ttu-id="24b19-240">Következő lépésként engedélyezze az hello **AlwaysOn rendelkezésre állási csoportok** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="24b19-240">Next, enable hello **AlwaysOn Availability Groups** feature.</span></span> <span data-ttu-id="24b19-241">Hajtsa végre ezeket a lépéseket, a két SQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="24b19-241">Do these steps on both SQL Servers.</span></span>

1. <span data-ttu-id="24b19-242">A hello **Start** indítsa el a jobb **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="24b19-242">From hello **Start** screen, launch **SQL Server Configuration Manager**.</span></span>
2. <span data-ttu-id="24b19-243">Hello böngésző konzolfáján kattintson **SQL Server Services**, majd kattintson a jobb gombbal a hello **SQL Server (MSSQLSERVER)** szolgáltatást, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="24b19-243">In hello browser tree, click **SQL Server Services**, then right-click hello **SQL Server (MSSQLSERVER)** service and click **Properties**.</span></span>
3. <span data-ttu-id="24b19-244">Kattintson a hello **AlwaysOn magas rendelkezésre állású** lapra, majd válasszon **engedélyezze az AlwaysOn rendelkezésre állási csoportokat**, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="24b19-244">Click hello **AlwaysOn High Availability** tab, then select **Enable AlwaysOn Availability Groups**, as follows:</span></span>

    ![Engedélyezze az AlwaysOn rendelkezésre állási csoportok](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. <span data-ttu-id="24b19-246">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-246">Click **Apply**.</span></span> <span data-ttu-id="24b19-247">Kattintson a **OK** hello előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="24b19-247">Click **OK** in hello pop-up dialog.</span></span>

5. <span data-ttu-id="24b19-248">Hello SQL Server-szolgáltatás újraindításához.</span><span class="sxs-lookup"><span data-stu-id="24b19-248">Restart hello SQL Server service.</span></span>

<span data-ttu-id="24b19-249">Ismételje meg ezeket a lépéseket a hello más SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="24b19-249">Repeat these steps on hello other SQL Server.</span></span>

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for hello database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for hello instance of SQL Server that is used toosynchronize hello database replicas in hello Availability Groups on that instance.

On both SQL Servers, open hello firewall for hello TCP port for hello database mirroring endpoint.

1. On hello first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In hello left pane, select **Inbound Rules**. On hello right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For hello port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In hello **Action** page, keep **Allow hello connection** selected and click **Next**.
6. In hello **Profile** page, accept hello default settings and click **Next**.
7. In hello **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in hello **Name** text box, then click **Finish**.

Repeat these steps on hello second SQL Server.
-------------------------->

## <a name="create-a-database-on-hello-first-sql-server"></a><span data-ttu-id="24b19-250">Adatbázis létrehozása a hello első SQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="24b19-250">Create a database on hello first SQL Server</span></span>

1. <span data-ttu-id="24b19-251">Indítási hello RDP fájl toohello első SQL-kiszolgálót egy tartományi fiók, amely tagja a sysadmin (rendszergazda) rögzített kiszolgálói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="24b19-251">Launch hello RDP file toohello first SQL Server with a domain account that is a member of sysadmin fixed server role.</span></span>
1. <span data-ttu-id="24b19-252">Nyissa meg az SQL Server Management Studio eszközt, és csatlakozzon a toohello első SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="24b19-252">Open SQL Server Management Studio and connect toohello first SQL Server.</span></span>
7. <span data-ttu-id="24b19-253">A **Object Explorer**, kattintson a jobb gombbal **adatbázisok** kattintson **új adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="24b19-253">In **Object Explorer**, right-click **Databases** and click **New Database**.</span></span>
8. <span data-ttu-id="24b19-254">A **adatbázisnév**, típus **MyDB1**, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="24b19-254">In **Database name**, type **MyDB1**, then click **OK**.</span></span>

### <span data-ttu-id="24b19-255"><a name="backupshare"></a>Hozzon létre egy biztonsági mentési megosztást</span><span class="sxs-lookup"><span data-stu-id="24b19-255"><a name="backupshare"></a> Create a backup share</span></span>

1. <span data-ttu-id="24b19-256">Az első SQL-kiszolgálót hello **Kiszolgálókezelő**, kattintson a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="24b19-256">On hello first SQL Server in **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="24b19-257">Nyissa meg **számítógép-kezelés**.</span><span class="sxs-lookup"><span data-stu-id="24b19-257">Open **Computer Management**.</span></span>

1. <span data-ttu-id="24b19-258">Kattintson a **megosztott mappák**.</span><span class="sxs-lookup"><span data-stu-id="24b19-258">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="24b19-259">Kattintson a jobb gombbal **megosztások**, és kattintson a **új megosztás...** .</span><span class="sxs-lookup"><span data-stu-id="24b19-259">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="24b19-261">Használjon **megosztott mappa létrehozása varázsló** toocreate egy megosztást.</span><span class="sxs-lookup"><span data-stu-id="24b19-261">Use **Create a Shared Folder Wizard** toocreate a share.</span></span>

1. <span data-ttu-id="24b19-262">A **mappa elérési útja**, kattintson a **Tallózás** , és keresse meg, vagy hozzon létre egy hello adatbázis biztonsági mentési megosztott mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="24b19-262">On **Folder Path**, click **Browse** and locate or create a path for hello database backup shared folder.</span></span> <span data-ttu-id="24b19-263">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-263">Click **Next**.</span></span>

1. <span data-ttu-id="24b19-264">A **nevét, leírását és beállítások** hello nevével és elérési ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="24b19-264">In **Name, Description, and Settings** verify hello share name and path.</span></span> <span data-ttu-id="24b19-265">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-265">Click **Next**.</span></span>

1. <span data-ttu-id="24b19-266">A **megosztott mappákra vonatkozó engedélyek** beállítása **engedélyek testreszabását**.</span><span class="sxs-lookup"><span data-stu-id="24b19-266">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="24b19-267">Kattintson a **egyéni...** .</span><span class="sxs-lookup"><span data-stu-id="24b19-267">Click **Custom...**.</span></span>

1. <span data-ttu-id="24b19-268">A **engedélyek testreszabása**, kattintson a **hozzáadása...** .</span><span class="sxs-lookup"><span data-stu-id="24b19-268">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="24b19-269">Győződjön meg arról, hogy hello SQL Server és SQL Server Agent szolgáltatásfiókokat mindkét kiszolgáló teljes hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="24b19-269">Make sure that hello SQL Server and SQL Server Agent service accounts for both servers have full control.</span></span>

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. <span data-ttu-id="24b19-271">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-271">Click **OK**.</span></span>

1. <span data-ttu-id="24b19-272">A **megosztott mappákra vonatkozó engedélyek**, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="24b19-272">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="24b19-273">Kattintson a **Befejezés** újra.</span><span class="sxs-lookup"><span data-stu-id="24b19-273">Click **Finish** again.</span></span>  

### <a name="take-a-full-backup-of-hello-database"></a><span data-ttu-id="24b19-274">Teljes mentés hello adatbázis készítése</span><span class="sxs-lookup"><span data-stu-id="24b19-274">Take a full backup of hello database</span></span>

<span data-ttu-id="24b19-275">Hello új adatbázis tooinitialize hello naplólánca mentése tooback van szüksége.</span><span class="sxs-lookup"><span data-stu-id="24b19-275">You need tooback up hello new database tooinitialize hello log chain.</span></span> <span data-ttu-id="24b19-276">Ha nem ad meg hello új adatbázis biztonsági másolatát, akkor egy rendelkezésre állási csoportban nem szerepelhet.</span><span class="sxs-lookup"><span data-stu-id="24b19-276">If you do not take a backup of hello new database, it cannot be included in an Availability Group.</span></span>

1. <span data-ttu-id="24b19-277">A **Object Explorer**, jobb gombbal az hello adatbázis túl**feladatok...** , kattintson a **készítsen biztonsági másolatot**.</span><span class="sxs-lookup"><span data-stu-id="24b19-277">In **Object Explorer**, right-click hello database, point too**Tasks...**, click **Back Up**.</span></span>

1. <span data-ttu-id="24b19-278">Kattintson a **OK** tootake a teljes biztonsági mentés toohello alapértelmezett biztonsági másolat helyét.</span><span class="sxs-lookup"><span data-stu-id="24b19-278">Click **OK** tootake a full backup toohello default backup location.</span></span>

## <a name="create-hello-availability-group"></a><span data-ttu-id="24b19-279">Hello rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="24b19-279">Create hello Availability Group</span></span>
<span data-ttu-id="24b19-280">Most már készen áll a rendelkezésre állási csoport használata a következő hello lépések tooconfigure áll:</span><span class="sxs-lookup"><span data-stu-id="24b19-280">You are now ready tooconfigure an Availability Group using hello following steps:</span></span>

* <span data-ttu-id="24b19-281">Adatbázis létrehozása a hello első SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="24b19-281">Create a database on hello first SQL Server.</span></span>
* <span data-ttu-id="24b19-282">Teljes biztonsági mentés és a tranzakciós napló biztonsági mentési hello adatbázis is igénybe vehet</span><span class="sxs-lookup"><span data-stu-id="24b19-282">Take both a full backup and a transaction log backup of hello database</span></span>
* <span data-ttu-id="24b19-283">Visszaállítás hello teljes és a napló biztonsági mentések toohello hello az SQL Server második **NORECOVERY** beállítás</span><span class="sxs-lookup"><span data-stu-id="24b19-283">Restore hello full and log backups toohello second SQL Server with hello **NORECOVERY** option</span></span>
* <span data-ttu-id="24b19-284">Hello rendelkezésre állási csoport létrehozása (**AG1**) szinkron véglegesítésre, automatikus feladatátvétel és olvasható másodlagos másodpéldányokra</span><span class="sxs-lookup"><span data-stu-id="24b19-284">Create hello Availability Group (**AG1**) with synchronous commit, automatic failover, and readable secondary replicas</span></span>

### <a name="create-hello-availability-group"></a><span data-ttu-id="24b19-285">Hello rendelkezésre állási csoport létrehozása:</span><span class="sxs-lookup"><span data-stu-id="24b19-285">Create hello Availability Group:</span></span>

1. <span data-ttu-id="24b19-286">A távoli asztali munkamenetgazda toohello első SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="24b19-286">On remote desktop session toohello first SQL Server.</span></span> <span data-ttu-id="24b19-287">A **Object Explorer** szolgáltatáshoz az ssms, kattintson a jobb gombbal **AlwaysOn magas rendelkezésre állású** kattintson **új rendelkezésre állási csoport varázsló**.</span><span class="sxs-lookup"><span data-stu-id="24b19-287">In **Object Explorer** in SSMS, right-click **AlwaysOn High Availability** and click **New Availability Group Wizard**.</span></span>

    ![Új rendelkezésre állási csoport varázsló indítása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. <span data-ttu-id="24b19-289">A hello **bemutatása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-289">In hello **Introduction** page, click **Next**.</span></span> <span data-ttu-id="24b19-290">A hello **adja meg a rendelkezésre állási csoport nevének** írja be például a rendelkezésre állási csoportban hello nevét **AG1**, a **rendelkezésre állási csoport nevének**.</span><span class="sxs-lookup"><span data-stu-id="24b19-290">In hello **Specify Availability Group Name** page, type a name for hello Availability Group, for example **AG1**, in **Availability group name**.</span></span> <span data-ttu-id="24b19-291">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-291">Click **Next**.</span></span>

    ![Új rendelkezésre állási csoport varázsló, adja meg a rendelkezésre állási csoport nevét](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. <span data-ttu-id="24b19-293">A hello **válasszon adatbázisok** lapon válassza ki az adatbázist, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-293">In hello **Select Databases** page, select your database and click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="24b19-294">hello adatbázis egy rendelkezésre állási csoport hello előfeltételei megfelel, mert legalább egy teljes biztonsági mentést készített hello kívánt elsődleges replikán.</span><span class="sxs-lookup"><span data-stu-id="24b19-294">hello database meets hello prerequisites for an Availability Group because you have taken at least one full backup on hello intended primary replica.</span></span>

   ![Új rendelkezésre állási csoport varázsló, válassza ki az adatbázisokat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. <span data-ttu-id="24b19-296">A hello **replikák megadása** kattintson **adja hozzá a replika**.</span><span class="sxs-lookup"><span data-stu-id="24b19-296">In hello **Specify Replicas** page, click **Add Replica**.</span></span>

   ![Új rendelkezésre állási csoport varázsló, adja meg a replikák](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. <span data-ttu-id="24b19-298">Hello **tooServer csatlakozás** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="24b19-298">hello **Connect tooServer** dialog pops up.</span></span> <span data-ttu-id="24b19-299">Második kiszolgáló hello hello típusnév **kiszolgálónév**.</span><span class="sxs-lookup"><span data-stu-id="24b19-299">Type hello name of hello second server in **Server name**.</span></span> <span data-ttu-id="24b19-300">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-300">Click **Connect**.</span></span>

   <span data-ttu-id="24b19-301">Vissza a hello **replikák megadása** lapon meg kell jelennie hello második kiszolgáló felsorolt **rendelkezésre állási másodpéldányok**.</span><span class="sxs-lookup"><span data-stu-id="24b19-301">Back in hello **Specify Replicas** page, you should now see hello second server listed in **Availability Replicas**.</span></span> <span data-ttu-id="24b19-302">Hello replikákat az alábbiak szerint konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="24b19-302">Configure hello replicas as follows.</span></span>

   ![Új rendelkezésre állási csoport varázsló, adja meg a replikák (kész)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. <span data-ttu-id="24b19-304">Kattintson a **végpontok** toosee hello adatbázis-tükrözési végpontját a rendelkezésre állási csoport számára.</span><span class="sxs-lookup"><span data-stu-id="24b19-304">Click **Endpoints** toosee hello database mirroring endpoint for this Availability Group.</span></span> <span data-ttu-id="24b19-305">Ugyanaz a hello beállításakor használt port használata hello [adatbázis-tükrözési végpont tűzfalszabályt](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span><span class="sxs-lookup"><span data-stu-id="24b19-305">Use hello same port that you used when you set hello [firewall rule for database mirroring endpoints](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span></span>

    ![Új rendelkezésre állási csoport varázsló kezdeti adatszinkronizálás kiválasztása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. <span data-ttu-id="24b19-307">A hello **kezdeti adatszinkronizálás kiválasztása** lapon, hogy melyik **teljes** , és adja meg egy megosztott hálózati helyre.</span><span class="sxs-lookup"><span data-stu-id="24b19-307">In hello **Select Initial Data Synchronization** page, select **Full** and specify a shared network location.</span></span> <span data-ttu-id="24b19-308">A hello helyéhez, használja a hello [létrehozott biztonsági másolatokat tároló megosztáson](#backupshare).</span><span class="sxs-lookup"><span data-stu-id="24b19-308">For hello location, use hello [backup share that you created](#backupshare).</span></span> <span data-ttu-id="24b19-309">Hello példa volt, a **\\\\\<első SQL-kiszolgáló\>\Backup\**.</span><span class="sxs-lookup"><span data-stu-id="24b19-309">In hello example it was, **\\\\\<First SQL Server\>\Backup\**.</span></span> <span data-ttu-id="24b19-310">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-310">Click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="24b19-311">Teljes szinkronizálás teljes mentést készít hello hello első példányát az SQL Server-adatbázisba, és visszaállítja azt toohello második példányát.</span><span class="sxs-lookup"><span data-stu-id="24b19-311">Full synchronization takes a full backup of hello database on hello first instance of SQL Server and restores it toohello second instance.</span></span> <span data-ttu-id="24b19-312">Nagy adatbázisok esetén a teljes szinkronizálás nem ajánlott, mert hosszú ideig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="24b19-312">For large databases, full synchronization is not recommended because it may take a long time.</span></span> <span data-ttu-id="24b19-313">Manuálisan hello adatbázis biztonsági másolatát, és állítja vissza a megadott idő csökkentése érdekében `NO RECOVERY`.</span><span class="sxs-lookup"><span data-stu-id="24b19-313">You can reduce this time by manually taking a backup of hello database and restoring it with `NO RECOVERY`.</span></span> <span data-ttu-id="24b19-314">Ha hello adatbázis már vissza a `NO RECOVERY` hello az SQL Server rendelkezésre állási csoport hello konfigurálása előtt a második, válassza a **csak összekapcsolás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-314">If hello database is already restored with `NO RECOVERY` on hello second SQL Server before configuring hello Availability Group, choose **Join only**.</span></span> <span data-ttu-id="24b19-315">Ha azt szeretné tootake hello biztonsági mentés hello rendelkezésre állási csoport konfigurálása után, **kezdeti adatszinkronizálás kihagyása**.</span><span class="sxs-lookup"><span data-stu-id="24b19-315">If you want tootake hello backup after configuring hello Availability Group, choose **Skip initial data synchronization**.</span></span>

    ![Új rendelkezésre állási csoport varázsló kezdeti adatszinkronizálás kiválasztása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. <span data-ttu-id="24b19-317">A hello **érvényesítési** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="24b19-317">In hello **Validation** page, click **Next**.</span></span> <span data-ttu-id="24b19-318">Ezen a lapon a következő kép hasonló toohello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="24b19-318">This page should look similar toohello following image:</span></span>

    ![Új rendelkezésre állási csoport varázsló, érvényesítése](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    ><span data-ttu-id="24b19-320">Mivel nem állított be egy rendelkezésre állási csoport figyelőjének, nincs hello figyelő konfigurációs vonatkozó figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="24b19-320">There is a warning for hello listener configuration because you have not configured an Availability Group listener.</span></span> <span data-ttu-id="24b19-321">Mivel az Azure virtuális gépeken hello Azure terheléselosztó létrehozása után létrehozhat hello figyelő, kihagyhatja ezt a figyelmeztetést.</span><span class="sxs-lookup"><span data-stu-id="24b19-321">You can ignore this warning because on Azure virtual machines you create hello listener after creating hello Azure load balancer.</span></span>

10. <span data-ttu-id="24b19-322">A hello **összegzés** kattintson **Befejezés**, akkor várjon, amíg a hello varázsló konfigurálja hello új rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="24b19-322">In hello **Summary** page, click **Finish**, then wait while hello wizard configures hello new Availability Group.</span></span> <span data-ttu-id="24b19-323">A hello **folyamatban** lap, kattintson **további részleteket** tooview hello részletes folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="24b19-323">In hello **Progress** page, you can click **More details** tooview hello detailed progress.</span></span> <span data-ttu-id="24b19-324">Hello varázsló befejezése után vizsgálja meg a hello **eredmények** lap tooverify, amely hello rendelkezésre állási csoport sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="24b19-324">Once hello wizard is finished, inspect hello **Results** page tooverify that hello Availability Group is successfully created.</span></span>

     ![Új rendelkezésre állási csoport varázsló, annak az eredménye](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. <span data-ttu-id="24b19-326">Kattintson a **Bezárás** tooexit hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="24b19-326">Click **Close** tooexit hello wizard.</span></span>

### <a name="check-hello-availability-group"></a><span data-ttu-id="24b19-327">Hello rendelkezésre állási csoport ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="24b19-327">Check hello Availability Group</span></span>

1. <span data-ttu-id="24b19-328">A **Object Explorer**, bontsa ki a **AlwaysOn magas rendelkezésre állású**, majd bontsa ki a **rendelkezésre állási csoportok**.</span><span class="sxs-lookup"><span data-stu-id="24b19-328">In **Object Explorer**, expand **AlwaysOn High Availability**, then expand **Availability Groups**.</span></span> <span data-ttu-id="24b19-329">Most látnia kell hello új rendelkezésre állási csoport ebben a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="24b19-329">You should now see hello new Availability Group in this container.</span></span> <span data-ttu-id="24b19-330">Kattintson a jobb gombbal a hello rendelkezésre állási csoportot, és kattintson a **irányítópult megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="24b19-330">Right-click hello Availability Group and click **Show Dashboard**.</span></span>

   ![Rendelkezésre állási csoport Irányítópult megjelenítése](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   <span data-ttu-id="24b19-332">A **AlwaysOn irányítópult** toothis hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="24b19-332">Your **AlwaysOn Dashboard** should look similar toothis.</span></span>

   ![Rendelkezésre állási csoport Irányítópult](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   <span data-ttu-id="24b19-334">Hello replikák hello feladatátvételi mód az egyes replika- és hello szinkronizálási tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="24b19-334">You can see hello replicas, hello failover mode of each replica and hello synchronization state.</span></span>

2. <span data-ttu-id="24b19-335">A **Feladatátvevőfürt-kezelő**, kattintson arra a fürtre.</span><span class="sxs-lookup"><span data-stu-id="24b19-335">In **Failover Cluster Manager**, click your cluster.</span></span> <span data-ttu-id="24b19-336">Válassza ki **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="24b19-336">Select **Roles**.</span></span> <span data-ttu-id="24b19-337">hello rendelkezésre állási csoport nevének használt hello fürtön a szerepkör megadása.</span><span class="sxs-lookup"><span data-stu-id="24b19-337">hello Availability Group name you used is a role on hello cluster.</span></span> <span data-ttu-id="24b19-338">Rendelkezésre állási csoport nem rendelkezik ügyfél-kommunikációhoz, IP-címet, mivel egy figyelő nincs konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="24b19-338">That Availability Group does not have an IP address for client connections, because you did not configure a listener.</span></span> <span data-ttu-id="24b19-339">Miután létrehozta az Azure terheléselosztó hello figyelő konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="24b19-339">You will configure hello listener after you create an Azure load balancer.</span></span>

   ![A Feladatátvevőfürt-kezelő AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > <span data-ttu-id="24b19-341">Ne próbáljon toofail hello rendelkezésre állási csoport a Feladatátvevőfürt-kezelő hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="24b19-341">Do not try toofail over hello Availability Group from hello Failover Cluster Manager.</span></span> <span data-ttu-id="24b19-342">Az összes feladatátvételi műveleteket kell elvégezni belül **AlwaysOn irányítópult** szolgáltatáshoz az ssms.</span><span class="sxs-lookup"><span data-stu-id="24b19-342">All failover operations should be performed from within **AlwaysOn Dashboard** in SSMS.</span></span> <span data-ttu-id="24b19-343">További információkért lásd: [Using korlátozásai hello Feladatátvevőfürt-kezelő rendelkezésre állási csoportokkal](https://msdn.microsoft.com/library/ff929171.aspx).</span><span class="sxs-lookup"><span data-stu-id="24b19-343">For more information, see [Restrictions on Using hello Failover Cluster Manager with Availability Groups](https://msdn.microsoft.com/library/ff929171.aspx).</span></span>
    >

<span data-ttu-id="24b19-344">Ezen a ponton rendelkezik egy rendelkezésre állási csoporthoz az SQL Server két példánya replikával.</span><span class="sxs-lookup"><span data-stu-id="24b19-344">At this point, you have an Availability Group with replicas on two instances of SQL Server.</span></span> <span data-ttu-id="24b19-345">Áthelyezheti hello rendelkezésre állási csoport példányai között.</span><span class="sxs-lookup"><span data-stu-id="24b19-345">You can move hello Availability Group between instances.</span></span> <span data-ttu-id="24b19-346">Toohello rendelkezésre állási csoport nem csatlakoztatható még, mert nem rendelkezik egy figyelő.</span><span class="sxs-lookup"><span data-stu-id="24b19-346">You cannot connect toohello Availability Group yet because you do not have a listener.</span></span> <span data-ttu-id="24b19-347">Az Azure virtuális gépeken hello figyelő terheléselosztó szükséges.</span><span class="sxs-lookup"><span data-stu-id="24b19-347">In Azure virtual machines, hello listener requires a load balancer.</span></span> <span data-ttu-id="24b19-348">következő lépés hello toocreate hello terheléselosztó az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="24b19-348">hello next step is toocreate hello load balancer in Azure.</span></span>

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a><span data-ttu-id="24b19-349">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="24b19-349">Create an Azure load balancer</span></span>

<span data-ttu-id="24b19-350">Azure virtuális gépeken futó SQL Server rendelkezésre állási csoport terheléselosztó szükséges.</span><span class="sxs-lookup"><span data-stu-id="24b19-350">On Azure virtual machines, a SQL Server Availability Group requires a load balancer.</span></span> <span data-ttu-id="24b19-351">hello terheléselosztó hello rendelkezésre állási csoport figyelőjének hello IP-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="24b19-351">hello load balancer holds hello IP address for hello Availability Group listener.</span></span> <span data-ttu-id="24b19-352">Ez a szakasz összefoglalja, hogyan toocreate hello terheléselosztó a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="24b19-352">This section summarizes how toocreate hello load balancer in hello Azure portal.</span></span>

1. <span data-ttu-id="24b19-353">A hello Azure-portálon, válassza a toohello erőforráscsoport, ahol az SQL-kiszolgálók, és kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-353">In hello Azure portal, go toohello resource group where your SQL Servers are and click **+ Add**.</span></span>
2. <span data-ttu-id="24b19-354">Keresse meg **terheléselosztó**.</span><span class="sxs-lookup"><span data-stu-id="24b19-354">Search for **Load Balancer**.</span></span> <span data-ttu-id="24b19-355">Válassza ki a Microsoft által kiadott hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="24b19-355">Choose hello load balancer published by Microsoft.</span></span>

   ![A Feladatátvevőfürt-kezelő AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  <span data-ttu-id="24b19-357">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="24b19-357">Click **Create**.</span></span>
3. <span data-ttu-id="24b19-358">A következő paraméterek hello terheléselosztóhoz hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="24b19-358">Configure hello following parameters for hello load balancer.</span></span>

   | <span data-ttu-id="24b19-359">Beállítás</span><span class="sxs-lookup"><span data-stu-id="24b19-359">Setting</span></span> | <span data-ttu-id="24b19-360">Mező</span><span class="sxs-lookup"><span data-stu-id="24b19-360">Field</span></span> |
   | --- | --- |
   | <span data-ttu-id="24b19-361">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="24b19-361">**Name**</span></span> |<span data-ttu-id="24b19-362">Szöveg esetében használható hello terheléselosztóhoz, például **sqlLB**.</span><span class="sxs-lookup"><span data-stu-id="24b19-362">Use a text name for hello load balancer, for example **sqlLB**.</span></span> |
   | <span data-ttu-id="24b19-363">**Típus**</span><span class="sxs-lookup"><span data-stu-id="24b19-363">**Type**</span></span> |<span data-ttu-id="24b19-364">Belső</span><span class="sxs-lookup"><span data-stu-id="24b19-364">Internal</span></span> |
   | <span data-ttu-id="24b19-365">**Virtuális hálózat**</span><span class="sxs-lookup"><span data-stu-id="24b19-365">**Virtual network**</span></span> |<span data-ttu-id="24b19-366">Azure-beli virtuális hálózat hello hello nevét használja.</span><span class="sxs-lookup"><span data-stu-id="24b19-366">Use hello name of hello Azure virtual network.</span></span> |
   | <span data-ttu-id="24b19-367">**Alhálózat**</span><span class="sxs-lookup"><span data-stu-id="24b19-367">**Subnet**</span></span> |<span data-ttu-id="24b19-368">Hello név használata, amely a virtuális gép hello hello alhálózat szerepel.</span><span class="sxs-lookup"><span data-stu-id="24b19-368">Use hello name of hello subnet that hello virtual machine is in.</span></span>  |
   | <span data-ttu-id="24b19-369">**IP-cím hozzárendelése**</span><span class="sxs-lookup"><span data-stu-id="24b19-369">**IP address assignment**</span></span> |<span data-ttu-id="24b19-370">Statikus</span><span class="sxs-lookup"><span data-stu-id="24b19-370">Static</span></span> |
   | <span data-ttu-id="24b19-371">**IP-cím**</span><span class="sxs-lookup"><span data-stu-id="24b19-371">**IP address**</span></span> |<span data-ttu-id="24b19-372">Egy alhálózatból címet használja.</span><span class="sxs-lookup"><span data-stu-id="24b19-372">Use an available address from subnet.</span></span> |
   | <span data-ttu-id="24b19-373">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="24b19-373">**Subscription**</span></span> |<span data-ttu-id="24b19-374">Használja az azonos hello előfizetés hello virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="24b19-374">Use hello same subscription as hello virtual machine.</span></span> |
   | <span data-ttu-id="24b19-375">**Hely**</span><span class="sxs-lookup"><span data-stu-id="24b19-375">**Location**</span></span> |<span data-ttu-id="24b19-376">Használjon hello azonos helyen hello virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="24b19-376">Use hello same location as hello virtual machine.</span></span> |

   <span data-ttu-id="24b19-377">az Azure portál panel hello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="24b19-377">hello Azure portal blade should look like this:</span></span>

   ![Terheléselosztó létrehozása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. <span data-ttu-id="24b19-379">Kattintson a **létrehozása**, toocreate hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="24b19-379">Click **Create**, toocreate hello load balancer.</span></span>

<span data-ttu-id="24b19-380">tooconfigure hello terheléselosztó kell toocreate háttérkészlet, a mintavételi és a set hello terheléselosztási szabályok.</span><span class="sxs-lookup"><span data-stu-id="24b19-380">tooconfigure hello load balancer, you need toocreate a backend pool, a probe, and set hello load balancing rules.</span></span> <span data-ttu-id="24b19-381">Hajtsa végre ezeket a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="24b19-381">Do these in hello Azure portal.</span></span>

### <a name="add-backend-pool"></a><span data-ttu-id="24b19-382">Háttérkészlet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24b19-382">Add backend pool</span></span>

1. <span data-ttu-id="24b19-383">A hello Azure-portálon válassza a tooyour rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="24b19-383">In hello Azure portal, go tooyour availability group.</span></span> <span data-ttu-id="24b19-384">Szükség lehet toorefresh hello nézet toosee az újonnan létrehozott hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="24b19-384">You might need toorefresh hello view toosee hello newly created load balancer.</span></span>

   ![Terheléselosztó erőforráscsoportban található](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. <span data-ttu-id="24b19-386">Kattintson a hello terheléselosztóhoz, majd **háttérkészletek**, és kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-386">Click hello load balancer, click **Backend pools**, and click **+Add**.</span></span> <span data-ttu-id="24b19-387">Az alábbiak szerint állíthatja hello háttérkészlet:</span><span class="sxs-lookup"><span data-stu-id="24b19-387">Set hello backend pool as follows:</span></span>

   | <span data-ttu-id="24b19-388">Beállítás</span><span class="sxs-lookup"><span data-stu-id="24b19-388">Setting</span></span> | <span data-ttu-id="24b19-389">Leírás</span><span class="sxs-lookup"><span data-stu-id="24b19-389">Description</span></span> | <span data-ttu-id="24b19-390">Példa</span><span class="sxs-lookup"><span data-stu-id="24b19-390">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="24b19-391">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="24b19-391">**Name**</span></span> | <span data-ttu-id="24b19-392">Adjon meg egy szöveges nevet</span><span class="sxs-lookup"><span data-stu-id="24b19-392">Type a text name</span></span> | <span data-ttu-id="24b19-393">SQLLBBE</span><span class="sxs-lookup"><span data-stu-id="24b19-393">SQLLBBE</span></span>
   | <span data-ttu-id="24b19-394">**Társított**</span><span class="sxs-lookup"><span data-stu-id="24b19-394">**Associated to**</span></span> | <span data-ttu-id="24b19-395">Válassza ki a listából</span><span class="sxs-lookup"><span data-stu-id="24b19-395">Pick from list</span></span> | <span data-ttu-id="24b19-396">Rendelkezésre állási csoport</span><span class="sxs-lookup"><span data-stu-id="24b19-396">Availability set</span></span>
   | <span data-ttu-id="24b19-397">**A rendelkezésre állási csoport**</span><span class="sxs-lookup"><span data-stu-id="24b19-397">**Availability set**</span></span> | <span data-ttu-id="24b19-398">Használjon, amelyek az SQL Server VMs hello rendelkezésre állási készlet nevét</span><span class="sxs-lookup"><span data-stu-id="24b19-398">Use a name of hello availability set that your SQL Server VMs are in</span></span> | <span data-ttu-id="24b19-399">sqlAvailabilitySet</span><span class="sxs-lookup"><span data-stu-id="24b19-399">sqlAvailabilitySet</span></span> |
   | <span data-ttu-id="24b19-400">**Virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="24b19-400">**Virtual machines**</span></span> |<span data-ttu-id="24b19-401">a két hello Azure SQL Server virtuális gép neve</span><span class="sxs-lookup"><span data-stu-id="24b19-401">hello two Azure SQL Server VM names</span></span> | <span data-ttu-id="24b19-402">SQL Server-0, SQL Server-1</span><span class="sxs-lookup"><span data-stu-id="24b19-402">sqlserver-0, sqlserver-1</span></span>

1. <span data-ttu-id="24b19-403">Írja be a hello háttérkészlethez hello nevét.</span><span class="sxs-lookup"><span data-stu-id="24b19-403">Type hello name for hello back end pool.</span></span>

1. <span data-ttu-id="24b19-404">Kattintson a **+ adja hozzá a virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="24b19-404">Click **+ Add a virtual machine**.</span></span>

1. <span data-ttu-id="24b19-405">Hello rendelkezésre állási csoportot válassza a hello rendelkezésre állási csoportban, hogy az SQL-kiszolgálók hello.</span><span class="sxs-lookup"><span data-stu-id="24b19-405">For hello availability set, choose hello availability set that hello SQL Servers are in.</span></span>

1. <span data-ttu-id="24b19-406">A virtuális gépek tartalmazzák mind a hello SQL Server-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="24b19-406">For virtual machines, include both of hello SQL Servers.</span></span> <span data-ttu-id="24b19-407">Tanúsító fájlmegosztás hello fájlkiszolgáló nem tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="24b19-407">Do not include hello file share witness server.</span></span>

1. <span data-ttu-id="24b19-408">Kattintson a **OK** toocreate hello háttérkészlet.</span><span class="sxs-lookup"><span data-stu-id="24b19-408">Click **OK** toocreate hello backend pool.</span></span>

### <a name="set-hello-probe"></a><span data-ttu-id="24b19-409">Set hello mintavétel</span><span class="sxs-lookup"><span data-stu-id="24b19-409">Set hello probe</span></span>

1. <span data-ttu-id="24b19-410">Kattintson a hello terheléselosztóhoz, majd **állapot-mintavételi csomagjai**, és kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-410">Click hello load balancer, click **Health probes**, and click **+Add**.</span></span>

1. <span data-ttu-id="24b19-411">Az alábbiak szerint állíthatja hello állapotmintáihoz:</span><span class="sxs-lookup"><span data-stu-id="24b19-411">Set hello health probe as follows:</span></span>

   | <span data-ttu-id="24b19-412">Beállítás</span><span class="sxs-lookup"><span data-stu-id="24b19-412">Setting</span></span> | <span data-ttu-id="24b19-413">Leírás</span><span class="sxs-lookup"><span data-stu-id="24b19-413">Description</span></span> | <span data-ttu-id="24b19-414">Példa</span><span class="sxs-lookup"><span data-stu-id="24b19-414">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="24b19-415">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="24b19-415">**Name**</span></span> | <span data-ttu-id="24b19-416">Szöveg</span><span class="sxs-lookup"><span data-stu-id="24b19-416">Text</span></span> | <span data-ttu-id="24b19-417">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="24b19-417">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="24b19-418">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="24b19-418">**Protocol**</span></span> | <span data-ttu-id="24b19-419">Válassza ki a TCP</span><span class="sxs-lookup"><span data-stu-id="24b19-419">Choose TCP</span></span> | <span data-ttu-id="24b19-420">TCP</span><span class="sxs-lookup"><span data-stu-id="24b19-420">TCP</span></span> |
   | <span data-ttu-id="24b19-421">**Port**</span><span class="sxs-lookup"><span data-stu-id="24b19-421">**Port**</span></span> | <span data-ttu-id="24b19-422">Minden nem használt portot</span><span class="sxs-lookup"><span data-stu-id="24b19-422">Any unused port</span></span> | <span data-ttu-id="24b19-423">59999</span><span class="sxs-lookup"><span data-stu-id="24b19-423">59999</span></span> |
   | <span data-ttu-id="24b19-424">**Időköz**</span><span class="sxs-lookup"><span data-stu-id="24b19-424">**Interval**</span></span>  | <span data-ttu-id="24b19-425">hello időn közötti mintavételi kísérletek másodpercben.</span><span class="sxs-lookup"><span data-stu-id="24b19-425">hello amount of time between probe attempts in seconds</span></span> |<span data-ttu-id="24b19-426">5</span><span class="sxs-lookup"><span data-stu-id="24b19-426">5</span></span> |
   | <span data-ttu-id="24b19-427">**Sérült küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="24b19-427">**Unhealthy threshold**</span></span> | <span data-ttu-id="24b19-428">hello egymást követő sikertelen mintavételek egy virtuális gép toobe megfelelő állapotúnak számít bekövetkező száma</span><span class="sxs-lookup"><span data-stu-id="24b19-428">hello number of consecutive probe failures that must occur for a virtual machine toobe considered unhealthy</span></span>  | <span data-ttu-id="24b19-429">2</span><span class="sxs-lookup"><span data-stu-id="24b19-429">2</span></span> |

1. <span data-ttu-id="24b19-430">Kattintson a **OK** tooset hello állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="24b19-430">Click **OK** tooset hello health probe.</span></span>

### <a name="set-hello-load-balancing-rules"></a><span data-ttu-id="24b19-431">Hello terheléselosztási szabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="24b19-431">Set hello load balancing rules</span></span>

1. <span data-ttu-id="24b19-432">Kattintson a hello terheléselosztóhoz, majd **terheléselosztási szabályok**, és kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="24b19-432">Click hello load balancer, click **Load balancing rules**, and click **+Add**.</span></span>

1. <span data-ttu-id="24b19-433">Hello terheléselosztási szabályok az alábbiak szerint állítsa be.</span><span class="sxs-lookup"><span data-stu-id="24b19-433">Set hello load balancing rules as follows.</span></span>
   | <span data-ttu-id="24b19-434">Beállítás</span><span class="sxs-lookup"><span data-stu-id="24b19-434">Setting</span></span> | <span data-ttu-id="24b19-435">Leírás</span><span class="sxs-lookup"><span data-stu-id="24b19-435">Description</span></span> | <span data-ttu-id="24b19-436">Példa</span><span class="sxs-lookup"><span data-stu-id="24b19-436">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="24b19-437">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="24b19-437">**Name**</span></span> | <span data-ttu-id="24b19-438">Szöveg</span><span class="sxs-lookup"><span data-stu-id="24b19-438">Text</span></span> | <span data-ttu-id="24b19-439">SQLAlwaysOnEndPointListener</span><span class="sxs-lookup"><span data-stu-id="24b19-439">SQLAlwaysOnEndPointListener</span></span> |
   | <span data-ttu-id="24b19-440">**Előtérbeli IP-cím**</span><span class="sxs-lookup"><span data-stu-id="24b19-440">**Frontend IP address**</span></span> | <span data-ttu-id="24b19-441">Válasszon címet</span><span class="sxs-lookup"><span data-stu-id="24b19-441">Choose an address</span></span> |<span data-ttu-id="24b19-442">Hello terheléselosztó létrehozásakor létrehozott hello címet használja.</span><span class="sxs-lookup"><span data-stu-id="24b19-442">Use hello address that you created when you created hello load balancer.</span></span> |
   | <span data-ttu-id="24b19-443">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="24b19-443">**Protocol**</span></span> | <span data-ttu-id="24b19-444">Válassza ki a TCP</span><span class="sxs-lookup"><span data-stu-id="24b19-444">Choose TCP</span></span> |<span data-ttu-id="24b19-445">TCP</span><span class="sxs-lookup"><span data-stu-id="24b19-445">TCP</span></span> |
   | <span data-ttu-id="24b19-446">**Port**</span><span class="sxs-lookup"><span data-stu-id="24b19-446">**Port**</span></span> | <span data-ttu-id="24b19-447">Az SQL Server-példány hello hello port használatára</span><span class="sxs-lookup"><span data-stu-id="24b19-447">Use hello port for hello SQL Server instance</span></span> | <span data-ttu-id="24b19-448">1433</span><span class="sxs-lookup"><span data-stu-id="24b19-448">1433</span></span> |
   | <span data-ttu-id="24b19-449">**Háttér-Port**</span><span class="sxs-lookup"><span data-stu-id="24b19-449">**Backend Port**</span></span> | <span data-ttu-id="24b19-450">Ha a fix IP-Címek értéke a közvetlen kiszolgálói visszatérési nem használja ezt a mezőt</span><span class="sxs-lookup"><span data-stu-id="24b19-450">This field is not used when Floating IP is set for direct server return</span></span> | <span data-ttu-id="24b19-451">1433</span><span class="sxs-lookup"><span data-stu-id="24b19-451">1433</span></span> |
   | <span data-ttu-id="24b19-452">**Hálózatfigyelő**</span><span class="sxs-lookup"><span data-stu-id="24b19-452">**Probe**</span></span> |<span data-ttu-id="24b19-453">hello mintavételhez megadott hello neve</span><span class="sxs-lookup"><span data-stu-id="24b19-453">hello name you specified for hello probe</span></span> | <span data-ttu-id="24b19-454">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="24b19-454">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="24b19-455">**Munkamenet megőrzését**</span><span class="sxs-lookup"><span data-stu-id="24b19-455">**Session Persistence**</span></span> | <span data-ttu-id="24b19-456">Legördülő lista</span><span class="sxs-lookup"><span data-stu-id="24b19-456">Drop down list</span></span> | <span data-ttu-id="24b19-457">**Egyik sem**</span><span class="sxs-lookup"><span data-stu-id="24b19-457">**None**</span></span> |
   | <span data-ttu-id="24b19-458">**Üresjárati időtúllépés**</span><span class="sxs-lookup"><span data-stu-id="24b19-458">**Idle Timeout**</span></span> | <span data-ttu-id="24b19-459">Nyissa meg a percet tookeep a TCP-kapcsolat</span><span class="sxs-lookup"><span data-stu-id="24b19-459">Minutes tookeep a TCP connection open</span></span> | <span data-ttu-id="24b19-460">4</span><span class="sxs-lookup"><span data-stu-id="24b19-460">4</span></span> |
   | <span data-ttu-id="24b19-461">**Lebegőpontos IP (közvetlen kiszolgálói válasz)**</span><span class="sxs-lookup"><span data-stu-id="24b19-461">**Floating IP (direct server return)**</span></span> | |<span data-ttu-id="24b19-462">Engedélyezve</span><span class="sxs-lookup"><span data-stu-id="24b19-462">Enabled</span></span> |

   > [!WARNING]
   > <span data-ttu-id="24b19-463">A közvetlen kiszolgálói válasz érték létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="24b19-463">Direct server return is set during creation.</span></span> <span data-ttu-id="24b19-464">Nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="24b19-464">It cannot be changed.</span></span>

1. <span data-ttu-id="24b19-465">Kattintson a **OK** tooset hello terheléselosztási szabályok.</span><span class="sxs-lookup"><span data-stu-id="24b19-465">Click **OK** tooset hello load balancing rules.</span></span>

## <span data-ttu-id="24b19-466"><a name="configure-listener"></a>Hello figyelő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24b19-466"><a name="configure-listener"></a> Configure hello listener</span></span>

<span data-ttu-id="24b19-467">hello következő lépésként toodo tooconfigure egy rendelkezésre állási csoport figyelőjének hello feladatátvevő fürtön.</span><span class="sxs-lookup"><span data-stu-id="24b19-467">hello next thing toodo is tooconfigure an Availability Group listener on hello failover cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="24b19-468">Ez az oktatóanyag bemutatja, hogyan toocreate egyetlen figyelő - egy ILB IP-cím.</span><span class="sxs-lookup"><span data-stu-id="24b19-468">This tutorial shows how toocreate a single listener - with one ILB IP address.</span></span> <span data-ttu-id="24b19-469">toocreate egy vagy több IP-címeket használ egy vagy több figyelői lásd [figyelő rendelkezésre állási csoport létrehozása és a terheléselosztó |} Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="24b19-469">toocreate one or more listeners using one or more IP addresses, see [Create Availability Group listener and load balancer | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a><span data-ttu-id="24b19-470">Set figyelő port</span><span class="sxs-lookup"><span data-stu-id="24b19-470">Set listener port</span></span>

<span data-ttu-id="24b19-471">Az SQL Server Management Studio hello figyelő port beállítása.</span><span class="sxs-lookup"><span data-stu-id="24b19-471">In SQL Server Management Studio, set hello listener port.</span></span>

1. <span data-ttu-id="24b19-472">Indítsa el az SQL Server Management Studio eszközt, és csatlakozzon a toohello elsődleges másodpéldány.</span><span class="sxs-lookup"><span data-stu-id="24b19-472">Launch SQL Server Management Studio and connect toohello primary replica.</span></span>

1. <span data-ttu-id="24b19-473">Keresse meg a túl**AlwaysOn magas rendelkezésre állású** | **rendelkezésre állási csoportok** | **rendelkezésre állási csoport figyelői**.</span><span class="sxs-lookup"><span data-stu-id="24b19-473">Navigate too**AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span>

1. <span data-ttu-id="24b19-474">Most látnia kell a Feladatátvevőfürt-kezelő létrehozta hello figyelő nevét.</span><span class="sxs-lookup"><span data-stu-id="24b19-474">You should now see hello listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="24b19-475">Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="24b19-475">Right-click hello listener name and click **Properties**.</span></span>

1. <span data-ttu-id="24b19-476">A hello **Port** hello rendelkezésre állási csoport figyelőjének hello portszáma hello segítségével adja meg a korábban használt $EndpointPort (1433 volt hello alapértelmezett), majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="24b19-476">In hello **Port** box, specify hello port number for hello Availability Group listener by using hello $EndpointPort you used earlier (1433 was hello default), then click **OK**.</span></span>

<span data-ttu-id="24b19-477">Most már rendelkezik egy SQL Server rendelkezésre állási csoport erőforrás-kezelő módban futó Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="24b19-477">You now have a SQL Server Availability Group in Azure virtual machines running in Resource Manager mode.</span></span>

## <a name="test-connection-toolistener"></a><span data-ttu-id="24b19-478">Teszt kapcsolat toolistener</span><span class="sxs-lookup"><span data-stu-id="24b19-478">Test connection toolistener</span></span>

<span data-ttu-id="24b19-479">tootest hello kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="24b19-479">tootest hello connection:</span></span>

1. <span data-ttu-id="24b19-480">RDP tooa hello az SQL Server azonos virtuális hálózat, de nem saját hello replika.</span><span class="sxs-lookup"><span data-stu-id="24b19-480">RDP tooa SQL Server that is in hello same virtual network, but does not own hello replica.</span></span> <span data-ttu-id="24b19-481">Használhat más SQL Server hello fürt hello.</span><span class="sxs-lookup"><span data-stu-id="24b19-481">You can use hello other SQL Server in hello cluster.</span></span>

1. <span data-ttu-id="24b19-482">Használjon **sqlcmd** segédprogram tootest hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="24b19-482">Use **sqlcmd** utility tootest hello connection.</span></span> <span data-ttu-id="24b19-483">Például a következő parancsfájl hello hoz létre egy **sqlcmd** kapcsolat toohello elsődleges replika hello figyelő Windows-hitelesítés használatával:</span><span class="sxs-lookup"><span data-stu-id="24b19-483">For example, hello following script establishes a **sqlcmd** connection toohello primary replica through hello listener with Windows authentication:</span></span>

    ```
    sqlcmd -S <listenerName> -E
    ```

    <span data-ttu-id="24b19-484">Ha hello figyelő eltérő portot használ hello alapértelmezett portot (1433), adja meg a hello portot hello kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="24b19-484">If hello listener is using a port other than hello default port (1433), specify hello port in hello connection string.</span></span> <span data-ttu-id="24b19-485">Például hello következő sqlcmd paranccsal összekapcsolja az tooa figyelési port 1435:</span><span class="sxs-lookup"><span data-stu-id="24b19-485">For example, hello following sqlcmd command connects tooa listener at port 1435:</span></span>

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="24b19-486">hello SQLCMD kapcsolat automatikusan csatlakozik a toowhichever példány SQL Server-gazdagépek hello elsődleges replika.</span><span class="sxs-lookup"><span data-stu-id="24b19-486">hello SQLCMD connection automatically connects toowhichever instance of SQL Server hosts hello primary replica.</span></span>

> [!TIP]
> <span data-ttu-id="24b19-487">Győződjön meg arról, hogy a megadott hello port a hello tűzfalat mindkét SQL Server nyitva-e.</span><span class="sxs-lookup"><span data-stu-id="24b19-487">Make sure that hello port you specify is open on hello firewall of both SQL Servers.</span></span> <span data-ttu-id="24b19-488">Mindkét kiszolgáló egy bejövő forgalomra vonatkozó szabály hello használt TCP-port szükséges.</span><span class="sxs-lookup"><span data-stu-id="24b19-488">Both servers require an inbound rule for hello TCP port that you use.</span></span> <span data-ttu-id="24b19-489">További információkért lásd: [hozzáadása vagy szerkesztése tűzfalszabály](http://technet.microsoft.com/library/cc753558.aspx).</span><span class="sxs-lookup"><span data-stu-id="24b19-489">For more information, see [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx).</span></span>
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by hello way” info, an Important is info users need toocomplete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is hello second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note hello format for documenting hello UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult toomaintain. Highlight areas you are referring tooin red.*-->

<!--**No. of steps**: *Make sure hello number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want toogo on.*-->

## <a name="next-steps"></a><span data-ttu-id="24b19-490">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24b19-490">Next steps</span></span>

- <span data-ttu-id="24b19-491">[Adja hozzá az IP cím tooa terheléselosztó egy második rendelkezésre állási csoport](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span><span class="sxs-lookup"><span data-stu-id="24b19-491">[Add an IP address tooa load balancer for a second availability group](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span></span>
