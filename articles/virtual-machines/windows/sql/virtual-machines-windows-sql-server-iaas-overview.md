---
title: "az SQL Server Azure virtuális gépeken aaaOverview |} Microsoft Docs"
description: "További információk a hogyan toorun teljes SQL Server kiadásai Azure virtuális gépeken. Közvetlen kapcsolatok tooall SQL Server Virtuálisgép-lemezképek és a kapcsolódó tartalom kapják meg."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: 07be567c76f4435961592fc0872fe41cd45bd79d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-sql-server-on-azure-virtual-machines"></a><span data-ttu-id="0b846-104">Az SQL Server használata az Azure Virtual Machines szolgáltatásban – áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b846-104">Overview of SQL Server on Azure Virtual Machines</span></span>
<span data-ttu-id="0b846-105">Ez a témakör ismerteti a beállításokat az SQL Servert futtató Azure virtuális gépek (VM), valamint [tooportal képek hivatkozásait](#option-1-create-a-sql-vm-with-per-minute-licensing) és áttekintését [gyakori feladatok](#manage-your-sql-vm).</span><span class="sxs-lookup"><span data-stu-id="0b846-105">This topic describes your options for running SQL Server on Azure virtual machines (VMs), along with [links tooportal images](#option-1-create-a-sql-vm-with-per-minute-licensing) and an overview of [common tasks](#manage-your-sql-vm).</span></span>

> [!NOTE]
> <span data-ttu-id="0b846-106">Ha már ismeri az SQL Server, és most szeretné, hogy hogyan toodeploy egy, az SQL Server virtuális gép: toosee [egy SQL Server rendszerű virtuális gép az Azure-portálon hello](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-106">If you're already familiar with SQL Server and just want toosee how toodeploy a SQL Server VM, see [Provision a SQL Server virtual machine in hello Azure portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="0b846-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b846-107">Overview</span></span>
<span data-ttu-id="0b846-108">Ha egy adatbázis-rendszergazda vagy egy fejlesztő, Azure virtuális gépek egy módon toomove forgathatók a helyszíni SQL Server számítási feladatok és alkalmazások toohello felhő adja meg.</span><span class="sxs-lookup"><span data-stu-id="0b846-108">If you are a database administrator or a developer, Azure VMs provide a way toomove your on-premises SQL Server workloads and applications toohello Cloud.</span></span> <span data-ttu-id="0b846-109">hello következő videó műszaki áttekintést nyújt az SQL Server Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="0b846-109">hello following video provides a technical overview of SQL Server Azure VMs.</span></span>

> [!VIDEO https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016/player]
> 
> 

<span data-ttu-id="0b846-110">hello videó ismerteti a következő területeken hello:</span><span class="sxs-lookup"><span data-stu-id="0b846-110">hello video covers hello following areas:</span></span>

| <span data-ttu-id="0b846-111">Time</span><span class="sxs-lookup"><span data-stu-id="0b846-111">Time</span></span> | <span data-ttu-id="0b846-112">Terület</span><span class="sxs-lookup"><span data-stu-id="0b846-112">Area</span></span> |
| --- | --- |
| <span data-ttu-id="0b846-113">00:21</span><span class="sxs-lookup"><span data-stu-id="0b846-113">00:21</span></span> |<span data-ttu-id="0b846-114">Mik azok az Azure virtuális gépek?</span><span class="sxs-lookup"><span data-stu-id="0b846-114">What are Azure VMs?</span></span> |
| <span data-ttu-id="0b846-115">01:45</span><span class="sxs-lookup"><span data-stu-id="0b846-115">01:45</span></span> |<span data-ttu-id="0b846-116">Biztonság</span><span class="sxs-lookup"><span data-stu-id="0b846-116">Security</span></span> |
| <span data-ttu-id="0b846-117">02:50</span><span class="sxs-lookup"><span data-stu-id="0b846-117">02:50</span></span> |<span data-ttu-id="0b846-118">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="0b846-118">Connectivity</span></span> |
| <span data-ttu-id="0b846-119">03:30</span><span class="sxs-lookup"><span data-stu-id="0b846-119">03:30</span></span> |<span data-ttu-id="0b846-120">Tárolómegbízhatóság és -teljesítmény</span><span class="sxs-lookup"><span data-stu-id="0b846-120">Storage reliability and performance</span></span> |
| <span data-ttu-id="0b846-121">05:20</span><span class="sxs-lookup"><span data-stu-id="0b846-121">05:20</span></span> |<span data-ttu-id="0b846-122">A virtuális gépek mérete</span><span class="sxs-lookup"><span data-stu-id="0b846-122">VM sizes</span></span> |
| <span data-ttu-id="0b846-123">05:54</span><span class="sxs-lookup"><span data-stu-id="0b846-123">05:54</span></span> |<span data-ttu-id="0b846-124">Magas rendelkezésre állás és szolgáltatásszint-szerződés</span><span class="sxs-lookup"><span data-stu-id="0b846-124">High availability and SLA</span></span> |
| <span data-ttu-id="0b846-125">07:30</span><span class="sxs-lookup"><span data-stu-id="0b846-125">07:30</span></span> |<span data-ttu-id="0b846-126">Konfigurációs támogatás</span><span class="sxs-lookup"><span data-stu-id="0b846-126">Configuration support</span></span> |
| <span data-ttu-id="0b846-127">08:00</span><span class="sxs-lookup"><span data-stu-id="0b846-127">08:00</span></span> |<span data-ttu-id="0b846-128">Figyelés</span><span class="sxs-lookup"><span data-stu-id="0b846-128">Monitoring</span></span> |
| <span data-ttu-id="0b846-129">08:32</span><span class="sxs-lookup"><span data-stu-id="0b846-129">08:32</span></span> |<span data-ttu-id="0b846-130">Bemutató: SQL Server 2016 virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b846-130">Demo: Create a SQL Server 2016 VM</span></span> |

> [!NOTE]
> <span data-ttu-id="0b846-131">hello videó az SQL Server 2016 összpontosít, de Azure Virtuálisgép-rendszerképek biztosít az SQL Server 2012, 2014 és 2016 sok verziója.</span><span class="sxs-lookup"><span data-stu-id="0b846-131">hello video focuses on SQL Server 2016, but Azure provides VM images for many versions of SQL Server, including 2012, 2014, and 2016.</span></span> 
> 
> 

## <a name="scenarios"></a><span data-ttu-id="0b846-132">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="0b846-132">Scenarios</span></span>
<span data-ttu-id="0b846-133">Számos oka választása toohost az adatokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0b846-133">There are many reasons that you might choose toohost your data in Azure.</span></span> <span data-ttu-id="0b846-134">Ha az alkalmazás mozgatása tooAzure, ez növeli a tooalso áthelyezés hello teljesítményadatait.</span><span class="sxs-lookup"><span data-stu-id="0b846-134">If your application is moving tooAzure, it improves performance tooalso move hello data.</span></span> <span data-ttu-id="0b846-135">De a használata további előnyökkel is jár.</span><span class="sxs-lookup"><span data-stu-id="0b846-135">But there are other benefits.</span></span> <span data-ttu-id="0b846-136">Automatikusan hozzáférést toomultiple adatközponthoz is a globális jelenlét és katasztrófa-helyreállítás van.</span><span class="sxs-lookup"><span data-stu-id="0b846-136">You automatically have access toomultiple data centers for a global presence and disaster recovery.</span></span> <span data-ttu-id="0b846-137">hello adata is rendkívül biztonságos és tartós.</span><span class="sxs-lookup"><span data-stu-id="0b846-137">hello data is also highly secured and durable.</span></span>

<span data-ttu-id="0b846-138">Az Azure virtuális gépeken futó SQL Server a relációs adatok Azure-ban való tárolásának egyik módja.</span><span class="sxs-lookup"><span data-stu-id="0b846-138">SQL Server running on Azure VMs is one option for storing your relational data in Azure.</span></span> <span data-ttu-id="0b846-139">Ez számos forgatókönyv esetében jó választás.</span><span class="sxs-lookup"><span data-stu-id="0b846-139">It is good choice for several scenarios.</span></span> <span data-ttu-id="0b846-140">Például lehetséges tooan a helyszíni SQL Server számítógép hasonlóan érdemes tooconfigure hello Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="0b846-140">For example, you might want tooconfigure hello Azure VM as similarly as possible tooan on-premises SQL Server machine.</span></span> <span data-ttu-id="0b846-141">Vagy a toorun további alkalmazásokat és szolgáltatásokat a hello ugyanazon adatbázis-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="0b846-141">Or you might want toorun additional applications and services on hello same database server.</span></span> <span data-ttu-id="0b846-142">Két fő erőforrás segítheti abban, hogy még több forgatókönyvet és megfontolást végiggondoljon:</span><span class="sxs-lookup"><span data-stu-id="0b846-142">There are two main resources that can help you think through even more scenarios and considerations:</span></span>

* <span data-ttu-id="0b846-143">[Azure virtuális gépeken futó SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/) hello ajánlott forgatókönyvek áttekintést nyújt az Azure virtuális gépeken futó SQL Server használatával.</span><span class="sxs-lookup"><span data-stu-id="0b846-143">[SQL Server on Azure virtual machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) provides an overview of hello best scenarios for using SQL Server on Azure VMs.</span></span> 
* <span data-ttu-id="0b846-144">A [Felhőalapú SQL Server-verzió választása: Azure SQL Database (PaaS) adatbázis vagy az Azure virtuális gépeken futó SQL Server (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) című témakör részletesen összehasonlítja az SQL Database-t és a virtuális gépeken futó SQL Servert.</span><span class="sxs-lookup"><span data-stu-id="0b846-144">[Choose a cloud SQL Server option: Azure SQL (PaaS) Database or SQL Server on Azure VMs (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) provides a detailed comparison between SQL Database and SQL Server running on a VM.</span></span>

## <a name="create-a-new-sql-vm"></a><span data-ttu-id="0b846-145">Új SQL virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b846-145">Create a new SQL VM</span></span>
<span data-ttu-id="0b846-146">hello következő szakaszokban közvetlen hivatkozások toohello Azure-portálon a hello SQL Server virtuális gép a gyűjtemény lemezképei.</span><span class="sxs-lookup"><span data-stu-id="0b846-146">hello following sections provide direct links toohello Azure portal for hello SQL Server virtual machine gallery images.</span></span> <span data-ttu-id="0b846-147">Attól függően, hogy hello lemezképet választ vagy az SQL Server licencelési költségei perc alapon fizetési is, vagy helyezheti a saját licenc (használata BYOL).</span><span class="sxs-lookup"><span data-stu-id="0b846-147">Depending on hello image you select, you can either pay for SQL Server licensing costs on a per-minute basis, or you can bring your own license (BYOL).</span></span>

<span data-ttu-id="0b846-148">Részletes útmutatás az új SQL virtuális gép létrehozása hello az oktatóanyagban található [egy SQL Server rendszerű virtuális gép az Azure-portálon hello](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-148">Find step-by-step guidance for creating a new SQL VM in hello tutorial, [Provision a SQL Server virtual machine in hello Azure portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="0b846-149">Emellett tekintse át a hello [teljesítmény ajánlott eljárások az SQL Server VMs](virtual-machines-windows-sql-performance.md), amely ismerteti, hogyan tooselect hello megfelelő számítógép méret és más elérhető szolgáltatások kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="0b846-149">Also, review hello [Performance best practices for SQL Server VMs](virtual-machines-windows-sql-performance.md), which explains how tooselect hello appropriate machine size and other features available during provisioning.</span></span>

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a><span data-ttu-id="0b846-150">1. lehetőség: SQL virtuális gép létrehozása percalapú licenceléssel</span><span class="sxs-lookup"><span data-stu-id="0b846-150">Option 1: Create a SQL VM with per-minute licensing</span></span>
<span data-ttu-id="0b846-151">hello alábbi táblázat foglalja össze hello legújabb SQL Server-rendszerképeit hello virtuálisgép-katalógusában.</span><span class="sxs-lookup"><span data-stu-id="0b846-151">hello following table provides a matrix of hello latest SQL Server images in hello virtual machine gallery.</span></span> <span data-ttu-id="0b846-152">Kattintson az összes hivatkozás toobegin egy új SQL virtuális gép létrehozása az operációs rendszerrel, a edition és a megadott verzió.</span><span class="sxs-lookup"><span data-stu-id="0b846-152">Click on any link toobegin creating a new SQL VM with your specified version, edition, and operating system.</span></span> 

> [!TIP]
> <span data-ttu-id="0b846-153">toounderstand hello virtuális gép és az ezeket a lemezképeket díjszabása SQL [útmutatást az SQL Server Azure virtuális gépek díjszabása](virtual-machines-windows-sql-server-pricing-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-153">toounderstand hello VM and SQL pricing for these images, see [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md).</span></span>

| <span data-ttu-id="0b846-154">Verzió</span><span class="sxs-lookup"><span data-stu-id="0b846-154">Version</span></span> | <span data-ttu-id="0b846-155">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="0b846-155">Operating System</span></span> | <span data-ttu-id="0b846-156">Kiadás</span><span class="sxs-lookup"><span data-stu-id="0b846-156">Edition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0b846-157">**SQL Server 2016 SP1**</span><span class="sxs-lookup"><span data-stu-id="0b846-157">**SQL Server 2016 SP1**</span></span> |<span data-ttu-id="0b846-158">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="0b846-158">Windows Server 2016</span></span> |<span data-ttu-id="0b846-159">[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016)</span><span class="sxs-lookup"><span data-stu-id="0b846-159">[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016)</span></span> |
| <span data-ttu-id="0b846-160">**SQL Server 2014 SP2**</span><span class="sxs-lookup"><span data-stu-id="0b846-160">**SQL Server 2014 SP2**</span></span> |<span data-ttu-id="0b846-161">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="0b846-161">Windows Server 2012 R2</span></span> |<span data-ttu-id="0b846-162">[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2)</span><span class="sxs-lookup"><span data-stu-id="0b846-162">[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2)</span></span> |
| <span data-ttu-id="0b846-163">**SQL Server 2012 SP3**</span><span class="sxs-lookup"><span data-stu-id="0b846-163">**SQL Server 2012 SP3**</span></span> |<span data-ttu-id="0b846-164">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="0b846-164">Windows Server 2012 R2</span></span> |<span data-ttu-id="0b846-165">[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)</span><span class="sxs-lookup"><span data-stu-id="0b846-165">[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)</span></span> |

<span data-ttu-id="0b846-166">Ezenkívül toothis lista, más SQL Server-verziók és operációs rendszerek kombinációi érhetők el.</span><span class="sxs-lookup"><span data-stu-id="0b846-166">In addition toothis list, other combinations of SQL Server versions and operating systems are available.</span></span> <span data-ttu-id="0b846-167">Hello Azure-portálon keresztül piactér keresés más lemezképek megkeresése</span><span class="sxs-lookup"><span data-stu-id="0b846-167">Find other images through a marketplace search in hello Azure portal.</span></span> 

## <span data-ttu-id="0b846-168"><a id="BYOL"></a>2. lehetőség: SQL virtuális gép létrehozása meglévő licenchez</span><span class="sxs-lookup"><span data-stu-id="0b846-168"><a id="BYOL"></a> Option 2: Create a SQL VM with an existing license</span></span>
<span data-ttu-id="0b846-169">Saját licencet is használhat (BYOL).</span><span class="sxs-lookup"><span data-stu-id="0b846-169">You can also bring your own license (BYOL).</span></span> <span data-ttu-id="0b846-170">Ebben a forgatókönyvben csak kell fizetnie hello virtuális gép az SQL Server licencelési egyéb költségek nélkül.</span><span class="sxs-lookup"><span data-stu-id="0b846-170">In this scenario, you only pay for hello VM without any additional charges for SQL Server licensing.</span></span> <span data-ttu-id="0b846-171">toouse saját licenc használata hello mátrix SQL Server-verziók, kiadásainak és operációs rendszereinek alábbi.</span><span class="sxs-lookup"><span data-stu-id="0b846-171">toouse your own license, use hello matrix of SQL Server versions, editions, and operating systems below.</span></span> <span data-ttu-id="0b846-172">Hello portálon, ezek előtagot a **{BYOL}**.</span><span class="sxs-lookup"><span data-stu-id="0b846-172">In hello portal, these image names are prefixed with **{BYOL}**.</span></span>

> [!TIP]
> <span data-ttu-id="0b846-173">Saját licence használatával hosszú távon pénzt takaríthat meg a folyamatos éles számítási feladatok esetében.</span><span class="sxs-lookup"><span data-stu-id="0b846-173">Bringing your own license can save you money over time for continuous production workloads.</span></span> <span data-ttu-id="0b846-174">További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-174">For more information, see [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md).</span></span>

| <span data-ttu-id="0b846-175">Verzió</span><span class="sxs-lookup"><span data-stu-id="0b846-175">Version</span></span> | <span data-ttu-id="0b846-176">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="0b846-176">Operating system</span></span> | <span data-ttu-id="0b846-177">Kiadás</span><span class="sxs-lookup"><span data-stu-id="0b846-177">Edition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0b846-178">**SQL Server 2016 SP1**</span><span class="sxs-lookup"><span data-stu-id="0b846-178">**SQL Server 2016 SP1**</span></span> |<span data-ttu-id="0b846-179">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="0b846-179">Windows Server 2016</span></span> |<span data-ttu-id="0b846-180">[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)</span><span class="sxs-lookup"><span data-stu-id="0b846-180">[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)</span></span> |
| <span data-ttu-id="0b846-181">**SQL Server 2014 SP2**</span><span class="sxs-lookup"><span data-stu-id="0b846-181">**SQL Server 2014 SP2**</span></span> |<span data-ttu-id="0b846-182">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="0b846-182">Windows Server 2012 R2</span></span> |<span data-ttu-id="0b846-183">[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2)</span><span class="sxs-lookup"><span data-stu-id="0b846-183">[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2)</span></span> |
| <span data-ttu-id="0b846-184">**SQL Server 2012 SP2**</span><span class="sxs-lookup"><span data-stu-id="0b846-184">**SQL Server 2012 SP2**</span></span> |<span data-ttu-id="0b846-185">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="0b846-185">Windows Server 2012 R2</span></span> |<span data-ttu-id="0b846-186">[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)</span><span class="sxs-lookup"><span data-stu-id="0b846-186">[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard  BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)</span></span> |

<span data-ttu-id="0b846-187">Ezenkívül toothis lista, más SQL Server-verziók és operációs rendszerek kombinációi érhetők el.</span><span class="sxs-lookup"><span data-stu-id="0b846-187">In addition toothis list, other combinations of SQL Server versions and operating systems are available.</span></span> <span data-ttu-id="0b846-188">A piactér keresési keresztül más képek található hello Azure portal (Keresés a "{BYOL} SQL Server").</span><span class="sxs-lookup"><span data-stu-id="0b846-188">Find other images through a marketplace search in hello Azure portal (search for "{BYOL} SQL Server").</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b846-189">toouse BYOL VM-lemezképek, rendelkeznie kell egy nagyvállalati szerződéssel [Azure frissítési garancián keresztüli Licenchordozhatósági](https://azure.microsoft.com/pricing/license-mobility/).</span><span class="sxs-lookup"><span data-stu-id="0b846-189">toouse BYOL VM images, you must have an Enterprise Agreement with [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/).</span></span> <span data-ttu-id="0b846-190">Érvényes licencre is kell hello verziójához/kiadásához, az SQL Server toouse keresi.</span><span class="sxs-lookup"><span data-stu-id="0b846-190">You also need a valid license for hello version/edition of SQL Server you want toouse.</span></span> <span data-ttu-id="0b846-191">Meg kell [adja meg a szükséges BYOL információk tooMicrosoft hello](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) belül **10** a virtuális gép kiépítésétől számított.</span><span class="sxs-lookup"><span data-stu-id="0b846-191">You must [provide hello necessary BYOL information tooMicrosoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) within **10** days of provisioning your VM.</span></span> 

> [!NOTE]
> <span data-ttu-id="0b846-192">Már nem lehetséges toochange hello licencelési egy perc fizetési SQL Server virtuális gép toouse márkája saját licenc.</span><span class="sxs-lookup"><span data-stu-id="0b846-192">It is not possible toochange hello licensing model of a pay-per-minute SQL Server VM toouse your own license.</span></span> <span data-ttu-id="0b846-193">Ebben az esetben létre kell hoznia egy új BYOL VM és az adatbázisok toohello áttelepítése új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="0b846-193">In this case, you must create a new BYOL VM and migrate your databases toohello new VM.</span></span> 

## <a name="manage-your-sql-vm"></a><span data-ttu-id="0b846-194">Az SQL virtuális gép felügyelete</span><span class="sxs-lookup"><span data-stu-id="0b846-194">Manage your SQL VM</span></span>
<span data-ttu-id="0b846-195">Az SQL Server virtuális gép kiépítése után több választható felügyeleti feladatot is végrehajthat.</span><span class="sxs-lookup"><span data-stu-id="0b846-195">After provisioning your SQL Server VM, there are several optional management tasks.</span></span> <span data-ttu-id="0b846-196">Bizonyos szempontokból az SQL Server konfigurálását és felügyeletét pontosan ugyanúgy kell elvégeznie, ahogy egy helyszíni SQL Server-példánnyal tenné.</span><span class="sxs-lookup"><span data-stu-id="0b846-196">In many aspects, you configure and manage SQL Server exactly like you would manage an on-premises SQL Server instance.</span></span> <span data-ttu-id="0b846-197">Azonban az egyes feladatok nem adott tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0b846-197">However, some tasks are specific tooAzure.</span></span> <span data-ttu-id="0b846-198">hello következő szakaszok kiemelnek néhány ilyen területet, és hivatkozások toomore információkat.</span><span class="sxs-lookup"><span data-stu-id="0b846-198">hello following sections highlight some of these areas with links toomore information.</span></span>

### <a name="connect-toohello-vm"></a><span data-ttu-id="0b846-199">Csatlakozás a virtuális gép toohello</span><span class="sxs-lookup"><span data-stu-id="0b846-199">Connect toohello VM</span></span>
<span data-ttu-id="0b846-200">Hello legalapvetőbb felügyeleti lépések egyike tooconnect tooyour SQL Server rendszerű virtuális eszközök, például az SQL Server Management Studio (SSMS) keresztül.</span><span class="sxs-lookup"><span data-stu-id="0b846-200">One of hello most basic management steps is tooconnect tooyour SQL Server VM through tools, such as SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="0b846-201">Hogyan tooconnect tooyour új SQL Server rendszerű virtuális Géphez, lásd: kapcsolatos utasításokat [csatlakozzon az SQL Server virtuális gépet az Azure tooa](virtual-machines-windows-sql-connect.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-201">For instructions on how tooconnect tooyour new SQL Server VM, see [Connect tooa SQL Server Virtual Machine on Azure](virtual-machines-windows-sql-connect.md).</span></span>

### <a name="migrate-your-data"></a><span data-ttu-id="0b846-202">Adatok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="0b846-202">Migrate your data</span></span>
<span data-ttu-id="0b846-203">Ha egy meglévő adatbázist használ, érdemes toomove adott toohello újonnan kiépített SQL virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="0b846-203">If you have an existing database, you'll want toomove that toohello newly provisioned SQL VM.</span></span> <span data-ttu-id="0b846-204">Az áttelepítési lehetőségekért és útmutatásért listájáért lásd: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](virtual-machines-windows-migrate-sql.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-204">For a list of migration options and guidance, see [Migrating a Database tooSQL Server on an Azure VM](virtual-machines-windows-migrate-sql.md).</span></span>

### <a name="configure-high-availability"></a><span data-ttu-id="0b846-205">Magas rendelkezésre állás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0b846-205">Configure high availability</span></span>
<span data-ttu-id="0b846-206">Ha magas rendelkezésre állásra van szüksége, fontolja meg az SQL Server rendelkezésre állási csoportok konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="0b846-206">If you require high availability, consider configuring SQL Server Availability Groups.</span></span> <span data-ttu-id="0b846-207">Ehhez több Azure virtuális gépre van szükség egy virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="0b846-207">This involves multiple Azure VMs in a virtual network.</span></span> <span data-ttu-id="0b846-208">hello Azure-portálon találhat egy sablont, amely beállítja ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0b846-208">hello Azure portal has a template that sets up this configuration for you.</span></span> <span data-ttu-id="0b846-209">További információ: [Configure an AlwaysOn availability group in Azure Resource Manager virtual machines](virtual-machines-windows-portal-sql-alwayson-availability-groups.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (AlwaysOn rendelkezésre állási csoport konfigurálása Azure Resource Manager virtuális gépeken).</span><span class="sxs-lookup"><span data-stu-id="0b846-209">For more information, see [Configure an AlwaysOn availability group in Azure Resource Manager virtual machines](virtual-machines-windows-portal-sql-alwayson-availability-groups.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="0b846-210">Ha azt szeretné, toomanually konfigurálja a rendelkezésre állási csoport és a kapcsolódó figyelőt, lásd: [AlwaysOn rendelkezésre állási csoportok konfigurálása Azure virtuális gép](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-210">If you want toomanually configure your Availability Group and associated listener, see [Configure AlwaysOn Availability Groups in Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

<span data-ttu-id="0b846-211">További magas rendelkezésre állási lehetőségekért lásd: [High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md) (Magas rendelkezésre állás és vészhelyreállítás Azure virtuális gépeken futó SQL Serveren).</span><span class="sxs-lookup"><span data-stu-id="0b846-211">For other high availability considerations, see [High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).</span></span>

### <a name="back-up-your-data"></a><span data-ttu-id="0b846-212">Az adatok biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="0b846-212">Back up your data</span></span>
<span data-ttu-id="0b846-213">Azure virtuális gépek kihasználhatják a [automatikus biztonsági mentés](virtual-machines-windows-sql-automated-backup.md), amely rendszeresen hoz létre a adatbázistár tooblob biztonsági másolatait.</span><span class="sxs-lookup"><span data-stu-id="0b846-213">Azure VMs can take advantage of [Automated Backup](virtual-machines-windows-sql-automated-backup.md), which regularly creates backups of your database tooblob storage.</span></span> <span data-ttu-id="0b846-214">Ezt a technikát manuálisan is alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="0b846-214">You can also manually use this technique.</span></span> <span data-ttu-id="0b846-215">További információ: [Use Azure Storage for SQL Server Backup and Restore](virtual-machines-windows-use-storage-sql-server-backup-restore.md) (Az Azure Storage használata az SQL Server biztonsági mentéséhez és helyreállításához).</span><span class="sxs-lookup"><span data-stu-id="0b846-215">For more information, see [Use Azure Storage for SQL Server Backup and Restore](virtual-machines-windows-use-storage-sql-server-backup-restore.md).</span></span> <span data-ttu-id="0b846-216">A biztonsági mentési és helyreállítási lehetőségek teljes körű áttekintéséért lásd: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md) (Az SQL Server biztonsági mentése és helyreállítása Azure virtuális gépeken).</span><span class="sxs-lookup"><span data-stu-id="0b846-216">For an overview of all backup and restore options, see [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

### <a name="automate-updates"></a><span data-ttu-id="0b846-217">Frissítések automatizálása</span><span class="sxs-lookup"><span data-stu-id="0b846-217">Automate updates</span></span>
<span data-ttu-id="0b846-218">Azure virtuális gépek használható [automatikus javítás](virtual-machines-windows-sql-automated-patching.md) tooschedule automatikusan frissül, fontos windows- és SQL Server telepítésére szolgáló karbantartási időszakot.</span><span class="sxs-lookup"><span data-stu-id="0b846-218">Azure VMs can use [Automated Patching](virtual-machines-windows-sql-automated-patching.md) tooschedule a maintenance window for installing important windows and SQL Server updates automatically.</span></span>

### <a name="customer-experience-improvement-program-ceip"></a><span data-ttu-id="0b846-219">Felhasználói élmény fokozása program (CEIP)</span><span class="sxs-lookup"><span data-stu-id="0b846-219">Customer experience improvement program (CEIP)</span></span>
<span data-ttu-id="0b846-220">hello felhasználói élmény fokozása Program (CEIP) alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="0b846-220">hello Customer Experience Improvement Program (CEIP) is enabled by default.</span></span> <span data-ttu-id="0b846-221">Ilyen időközönként küld jelentések tooMicrosoft toohelp javítása az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b846-221">This periodically sends reports tooMicrosoft toohelp improve SQL Server.</span></span> <span data-ttu-id="0b846-222">Nincs szükség a felhasználói élmény fokozása program, kivéve, ha azt szeretné, hogy toodisable felügyeleti feladat azt kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="0b846-222">There is no management task required with CEIP unless you want toodisable it after provisioning.</span></span> <span data-ttu-id="0b846-223">Testre szabhatja, vagy tiltsa le a felhasználói élmény fokozása program hello toohello VM csatlakozzon a távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="0b846-223">You can customize or disable hello CEIP by connecting toohello VM with remote desktop.</span></span> <span data-ttu-id="0b846-224">Futtassa a hello **SQL Server hiba- és használatai jelentések** segédprogram.</span><span class="sxs-lookup"><span data-stu-id="0b846-224">Then run hello **SQL Server Error and Usage Reporting** utility.</span></span> <span data-ttu-id="0b846-225">Hajtsa végre a hello utasításokat toodisable reporting.</span><span class="sxs-lookup"><span data-stu-id="0b846-225">Follow hello instructions toodisable reporting.</span></span> 

<span data-ttu-id="0b846-226">További információkért lásd: hello hello CEIP szakasza [licencfeltételek elfogadása](https://msdn.microsoft.com/library/ms143343.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="0b846-226">For more information, see hello CEIP section of hello [Accept License Terms](https://msdn.microsoft.com/library/ms143343.aspx) topic.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0b846-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b846-227">Next steps</span></span>

<span data-ttu-id="0b846-228">Az árazással kapcsolatos kérdésekre, lásd: [útmutatást az SQL Server Azure virtuális gépek díjszabása](virtual-machines-windows-sql-server-pricing-guidance.md) és hello [Azure díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).</span><span class="sxs-lookup"><span data-stu-id="0b846-228">For questions about pricing, see [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) and hello [Azure pricing page](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).</span></span> <span data-ttu-id="0b846-229">Válassza ki a cél az SQL Server kiadása hello **operációsrendszer-szoftver** listája.</span><span class="sxs-lookup"><span data-stu-id="0b846-229">Select your target edition of SQL Server in hello **OS/Software** list.</span></span> <span data-ttu-id="0b846-230">Nézze meg, hello árak másképp méretű virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="0b846-230">Then view hello prices for differently sized virtual machines.</span></span>

<span data-ttu-id="0b846-231">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="0b846-231">More question?</span></span> <span data-ttu-id="0b846-232">Először tekintse meg a hello [SQL Server Azure virtuális gépek – gyakori kérdések](virtual-machines-windows-sql-server-iaas-faq.md).</span><span class="sxs-lookup"><span data-stu-id="0b846-232">First, see hello [SQL Server on Azure Virtual Machines FAQ](virtual-machines-windows-sql-server-iaas-faq.md).</span></span> <span data-ttu-id="0b846-233">De is vegye fel a kérdését vagy megjegyzését toohello alsó részén található bármely SQL virtuális gép témakörök toointeract Microsoft és hello Közösséggel.</span><span class="sxs-lookup"><span data-stu-id="0b846-233">But also add your questions or comments toohello bottom of any SQL VM topics toointeract with Microsoft and hello community.</span></span>