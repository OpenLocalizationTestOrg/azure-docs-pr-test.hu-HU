---
title: "Prémium szintű Azure Storage használata az SQL Server |} Microsoft Docs"
description: "Ez a cikk a klasszikus üzembe helyezési modellel létrehozott erőforrást használ, és a prémium szintű Azure Storage használata az Azure virtuális gépeken futó SQL Server útmutatást ad."
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: jroth
ms.openlocfilehash: 6790db207fc7ec8a4b1546ef07c97ef30abe9513
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a><span data-ttu-id="074c8-103">Az Azure Premium Storage és az SQL Server együttes használata virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="074c8-103">Use Azure Premium Storage with SQL Server on Virtual Machines</span></span>
## <a name="overview"></a><span data-ttu-id="074c8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="074c8-104">Overview</span></span>
<span data-ttu-id="074c8-105">[Prémium szintű Storage](../../../storage/common/storage-premium-storage.md) tárhelyet biztosít alacsony késéssel és magas teljesítmény IO következő generációja van.</span><span class="sxs-lookup"><span data-stu-id="074c8-105">[Azure Premium Storage](../../../storage/common/storage-premium-storage.md) is the next generation of storage that provides low latency and high throughput IO.</span></span> <span data-ttu-id="074c8-106">A kulcs IO igényes munkaterhelések, például az SQL Server IaaS a legjobban [virtuális gépek](https://azure.microsoft.com/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="074c8-106">It works best for key IO intensive workloads, such as SQL Server on IaaS [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="074c8-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="074c8-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="074c8-108">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="074c8-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="074c8-109">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="074c8-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="074c8-110">Ez a cikk ismerteti a tervezési és a prémium szintű Storage SQL Servert futtató virtuális gép áttelepítését.</span><span class="sxs-lookup"><span data-stu-id="074c8-110">This article provides planning and guidance for migrating a Virtual Machine running SQL Server to use Premium Storage.</span></span> <span data-ttu-id="074c8-111">Ez magában foglalja az Azure-infrastruktúra (hálózati, tárolási) és a vendég Windows virtuális gép lépéseket.</span><span class="sxs-lookup"><span data-stu-id="074c8-111">This includes Azure infrastructure (networking, storage) and guest Windows VM steps.</span></span> <span data-ttu-id="074c8-112">Példa a [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) továbbfejlesztett helyi SSD-tárhelyre PowerShell előnyeinek kihasználása érdekében nagyobb virtuális gépek áthelyezése a teljes átfogó végpontok közötti áttelepítését mutatja.</span><span class="sxs-lookup"><span data-stu-id="074c8-112">The example in the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) shows a full comprehensive end to end migration of how to move larger VMs to take advantage of improved local SSD storage with PowerShell.</span></span>

<span data-ttu-id="074c8-113">Fontos tudni, hogy terjesztése prémium szintű Azure Storage az infrastruktúra-szolgáltatási virtuális gépeken futó SQL Server-végpontok közötti folyamatán.</span><span class="sxs-lookup"><span data-stu-id="074c8-113">It is important to understand the end-to-end process of utilizing Azure Premium Storage with SQL Server on IAAS VMs.</span></span> <span data-ttu-id="074c8-114">Ehhez a következőket:</span><span class="sxs-lookup"><span data-stu-id="074c8-114">This includes:</span></span>

* <span data-ttu-id="074c8-115">Prémium szintű Storage használatának előfeltételei azonosítása.</span><span class="sxs-lookup"><span data-stu-id="074c8-115">Identification of the prerequisites to use Premium Storage.</span></span>
* <span data-ttu-id="074c8-116">Prémium szintű Storage IaaS az SQL Server telepítése az új központi telepítéseknél példát.</span><span class="sxs-lookup"><span data-stu-id="074c8-116">Examples of deploying SQL Server on IaaS to Premium Storage for new deployments.</span></span>
* <span data-ttu-id="074c8-117">Önálló kiszolgálók és a üzembe helyezése SQL Always On rendelkezésre állási csoportok áttelepítése meglévő telepítés példát.</span><span class="sxs-lookup"><span data-stu-id="074c8-117">Examples of migrating existing deployments, both stand-alone servers and deployments using SQL Always On Availability Groups.</span></span>
* <span data-ttu-id="074c8-118">Lehetséges áttelepítési módszer.</span><span class="sxs-lookup"><span data-stu-id="074c8-118">Possible migration approaches.</span></span>
* <span data-ttu-id="074c8-119">Az áttelepítés végrehajtásának egy meglévő Always On Azure, a Windows és az SQL Server lépéseket bemutató teljes-végpontok példa.</span><span class="sxs-lookup"><span data-stu-id="074c8-119">Full end-to-end example showing Azure, Windows, and SQL Server steps for the migration of an existing Always On implementation.</span></span>

<span data-ttu-id="074c8-120">Tekintse meg az SQL Server Azure virtuális gépek további háttérinformációkat [SQL Server Azure virtuális gépek](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="074c8-120">For more background information on SQL Server in Azure Virtual Machines, see [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

<span data-ttu-id="074c8-121">**Szerző:** Daniel Sol **műszaki véleményezők:** Luis Carlos Vargas hering, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span><span class="sxs-lookup"><span data-stu-id="074c8-121">**Author:** Daniel Sol **Technical Reviewers:** Luis Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span></span>

## <a name="prerequisites-for-premium-storage"></a><span data-ttu-id="074c8-122">Prémium szintű Storage előfeltételei</span><span class="sxs-lookup"><span data-stu-id="074c8-122">Prerequisites for Premium Storage</span></span>
<span data-ttu-id="074c8-123">Prémium szintű Storage használatával több előfeltételei van.</span><span class="sxs-lookup"><span data-stu-id="074c8-123">There are several prerequisites for using Premium Storage.</span></span>

### <a name="machine-size"></a><span data-ttu-id="074c8-124">Mérete</span><span class="sxs-lookup"><span data-stu-id="074c8-124">Machine size</span></span>
<span data-ttu-id="074c8-125">Prémium szintű Storage használatához szüksége lesz DS adatsorozat virtuális gépek (VM) használja.</span><span class="sxs-lookup"><span data-stu-id="074c8-125">For using Premium Storage you will need to use DS series Virtual Machines (VM).</span></span> <span data-ttu-id="074c8-126">DS adatsorozat gépek nem használta a felhőszolgáltatásban előtt, ha törli a meglévő virtuális Gépet, a csatlakoztatott lemezek tároljuk, és majd új felhőalapú szolgáltatás létrehozása előtt újra létrehozni a virtuális gép méreteként DS * szerepkör.</span><span class="sxs-lookup"><span data-stu-id="074c8-126">If you have not used DS Series machines in your cloud service before, you must delete the existing VM, keep the attached disks, and then create a new cloud service before recreating the VM as DS* role size.</span></span> <span data-ttu-id="074c8-127">További információ a virtuálisgép-méretek: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="074c8-127">For more information on Virtual Machine sizes, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="cloud-services"></a><span data-ttu-id="074c8-128">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="074c8-128">Cloud services</span></span>
<span data-ttu-id="074c8-129">Csak használata virtuális gépek DS * prémium szintű Storage az új felhőalapú szolgáltatás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="074c8-129">You can only use DS* VMs with Premium Storage when they are created in a new cloud service.</span></span> <span data-ttu-id="074c8-130">Ha az SQL Server Always On használ az Azure-ban, az mindig a figyelő az Azure belső vagy külső terheléselosztási terheléselosztó IP-cím egy felhőalapú szolgáltatás társított fog hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="074c8-130">If you are using SQL Server Always On in Azure, the Always On Listener will refer to the Azure Internal or External Load Balancer IP address that is associated with a cloud service.</span></span> <span data-ttu-id="074c8-131">Ez a cikk foglalkozik, hogyan telepítheti át az ebben a forgatókönyvben rendelkezésre állásának a.</span><span class="sxs-lookup"><span data-stu-id="074c8-131">This article focuses on how to migrate while maintaining availability in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="074c8-132">DS * kell a első virtuális gép, amely az új felhőalapú szolgáltatás van telepítve.</span><span class="sxs-lookup"><span data-stu-id="074c8-132">A DS* Series must be the first VM that is deployed to the new Cloud Service.</span></span>
>
>

### <a name="regional-vnets"></a><span data-ttu-id="074c8-133">Regionális VNETEK</span><span class="sxs-lookup"><span data-stu-id="074c8-133">Regional VNETS</span></span>
<span data-ttu-id="074c8-134">Virtuális gépek DS * kell konfigurálnia a virtuális hálózatot (VNET) kell lennie a regionális a virtuális gépeket üzemeltet.</span><span class="sxs-lookup"><span data-stu-id="074c8-134">For DS* VMs you must configure the Virtual Network (VNET) hosting your VMs to be regional.</span></span> <span data-ttu-id="074c8-135">A "szélesebb formája" a virtuális hálózat lehetővé teszi a nagyobb virtuális gépek építhető ki más fürtöket, és engedélyezheti a köztük folyó kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="074c8-135">This “widens” the VNET is to allow the larger VMs to be provisioned in other clusters and allow communication between them.</span></span> <span data-ttu-id="074c8-136">Az alábbi képernyőfelvételen látható a kijelölt hely regionális Vnetek látható, mivel az első eredmény azt mutatja, a "keskeny" virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="074c8-136">In the following screenshot, the highlighted Location shows regional VNETs, whereas the first result shows a “narrow” VNET.</span></span>

![RegionalVNET][1]

<span data-ttu-id="074c8-138">A Microsoft támogatási jegy áttelepíteni egy regionális vnetre is növelheti, Microsoft olyan módosítást, majd a hálózati konfigurációban AffinityGroup-tulajdonság módosításához a regionális Vnetek, az áttelepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="074c8-138">You can raise a Microsoft support ticket to migrate to a regional VNET, Microsoft will make a change, then to complete the migration to regional VNETs, change the property AffinityGroup in the network configuration.</span></span> <span data-ttu-id="074c8-139">Először exportálja a PowerShell a hálózati konfigurációt, és lecseréli a **AffinityGroup** tulajdonságot a **VirtualNetworkSite** elem egy **hely** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="074c8-139">First export the Network Configuration in PowerShell, and then replace the **AffinityGroup** property in the **VirtualNetworkSite** element with a **Location** property.</span></span> <span data-ttu-id="074c8-140">Adja meg `Location = XXXX` ahol `XXXX` egy Azure-régióban van.</span><span class="sxs-lookup"><span data-stu-id="074c8-140">Specify `Location = XXXX` where `XXXX` is an Azure region.</span></span> <span data-ttu-id="074c8-141">Importálja az új konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="074c8-141">Then import the new configuration.</span></span>

<span data-ttu-id="074c8-142">Például annak eldöntéséhez, hogy a következő VNET konfigurációját:</span><span class="sxs-lookup"><span data-stu-id="074c8-142">For example, considering the following VNET configuration:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

<span data-ttu-id="074c8-143">Helyezze át a egy regionális VNETRE, Nyugat-Európában, módosítsa a konfigurációt a következő:</span><span class="sxs-lookup"><span data-stu-id="074c8-143">To move this to a regional VNET in West Europe, change the configuration to the following:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a><span data-ttu-id="074c8-144">Tárfiókok</span><span class="sxs-lookup"><span data-stu-id="074c8-144">Storage accounts</span></span>
<span data-ttu-id="074c8-145">Akkor hozzon létre egy új tárfiókot, amely prémium szintű Storage van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="074c8-145">You will need to create a new storage account that is configured for Premium Storage.</span></span> <span data-ttu-id="074c8-146">Figyelje meg, hogy a storage-fiók nem egyedi virtuális merevlemezek, a prémium szintű Storage használata van beállítva azonban a DS * adatsorozat virtuális gépek használatakor csatolhat a VHD-k a prémium és standard szintű tárolást fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="074c8-146">Note that the use of Premium Storage is set at the storage account, not on individual VHDs, however when using a DS* Series VM you can attach VHD’s from Premium and Standard Storage accounts.</span></span> <span data-ttu-id="074c8-147">A érdemes lehet, hogy lehet, ha nem szeretné helyezni a az operációs rendszer virtuális Merevlemezt a prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="074c8-147">You may consider this if you do not want to place the OS VHD on to the Premium Storage account.</span></span>

<span data-ttu-id="074c8-148">A következő **New-AzureStorageAccountPowerShell** parancsot a "Premium_LRS" **típus** hoz létre a prémium szintű Storage-fiókok:</span><span class="sxs-lookup"><span data-stu-id="074c8-148">The following **New-AzureStorageAccountPowerShell** command with the "Premium_LRS" **Type** creates a Premium Storage Account:</span></span>

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a><span data-ttu-id="074c8-149">Virtuális merevlemezek gyorsítótár beállításai</span><span class="sxs-lookup"><span data-stu-id="074c8-149">VHDs Cache Settings</span></span>
<span data-ttu-id="074c8-150">Lemezek, amelyek részei a prémium szintű Storage-fiók létrehozása közötti fő különbség a lemezgyorsítótár-beállítás.</span><span class="sxs-lookup"><span data-stu-id="074c8-150">The main difference between creating disks that are part of a Premium Storage account is the disk cache setting.</span></span> <span data-ttu-id="074c8-151">Használata javasolt az SQL Server adatmennyiség lemezek azt "**olvasási gyorsítótárazás**".</span><span class="sxs-lookup"><span data-stu-id="074c8-151">For SQL Server Data volume disks it is recommended that you use ‘**Read Caching**’.</span></span> <span data-ttu-id="074c8-152">A tranzakció naplózási kötetek, a lemezgyorsítótár-beállítás kell állítható be "**nincs**".</span><span class="sxs-lookup"><span data-stu-id="074c8-152">For Transaction log volumes, the disk cache setting should be set to ‘**None**’.</span></span> <span data-ttu-id="074c8-153">Ez eltér a javaslatok, Standard szintű Storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="074c8-153">This is different from the recommendations for Standard Storage accounts.</span></span>

<span data-ttu-id="074c8-154">A virtuális merevlemezek csatolást követően a gyorsítótár-beállítása nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="074c8-154">Once the VHDs have been attached, the cache setting cannot be altered.</span></span> <span data-ttu-id="074c8-155">Válassza le, majd újra csatlakoztatja a VHD-t egy frissített gyorsítótár beállítású kellene.</span><span class="sxs-lookup"><span data-stu-id="074c8-155">You would need to detach and reattach the VHD with an updated cache setting.</span></span>

### <a name="windows-storage-spaces"></a><span data-ttu-id="074c8-156">Windows tárolóhelyek</span><span class="sxs-lookup"><span data-stu-id="074c8-156">Windows storage spaces</span></span>
<span data-ttu-id="074c8-157">Használhat [Windows tárolóhelyek](https://technet.microsoft.com/library/hh831739.aspx) úgy, ahogy az előző standard szintű Storage, ez lehetővé teszi, hogy át egy virtuális Gépet, amely már van okhoz tárolóhelyek.</span><span class="sxs-lookup"><span data-stu-id="074c8-157">You can use [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) as you did with previous Standard Storage, this will allow you to migrate a VM that is already utilizing Storage Spaces.</span></span> <span data-ttu-id="074c8-158">Példa [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (9-es és az utána lépés) való kigyűjtésére, majd importálja a virtuális gép több csatlakoztatott virtuális merevlemezek és a Powershell-kódot mutatja be.</span><span class="sxs-lookup"><span data-stu-id="074c8-158">The example in [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (step 9 and forward) demonstrates the Powershell code to extract and import a VM with multiple attached VHDs.</span></span>

<span data-ttu-id="074c8-159">Tárolókészletek használt szabványos Azure storage-fiók átviteli sebesség növelése és a késés csökkentésére.</span><span class="sxs-lookup"><span data-stu-id="074c8-159">Storage Pools were used with Standard Azure storage account to enhance throughput and reduce latency.</span></span> <span data-ttu-id="074c8-160">Bizonyára hasznosnak találja érték a prémium szintű Storage Tárolókészletek tesztelése az új központi telepítéseknél, de nagyobb fokú összetettségével jár tárolási telepítés adnak hozzá.</span><span class="sxs-lookup"><span data-stu-id="074c8-160">You might find value in testing Storage Pools with Premium Storage for new deployments, but they do add additional complexity with storage setup.</span></span>

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a><span data-ttu-id="074c8-161">Annak ellenőrzése, mely Azure virtuális lemezek leképezés tárolókészletek</span><span class="sxs-lookup"><span data-stu-id="074c8-161">How to find which Azure Virtual Disks map to storage pools</span></span>
<span data-ttu-id="074c8-162">Mivel másik gyorsítótármappa beállítás javaslatok csatolt VHD-k, dönthet, másolja a VHD-k a prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="074c8-162">As there are different cache setting recommendations for attached VHDs, you might decide to copy the VHDs to a Premium Storage account.</span></span> <span data-ttu-id="074c8-163">Azonban ha, csatlakoztassa újra őket az új virtuális gép DS adatsorozat, szükség lehet a gyorsítótár beállításainak módosításához.</span><span class="sxs-lookup"><span data-stu-id="074c8-163">However, when you reattach them to the new DS series VM, you might need to alter the cache settings.</span></span> <span data-ttu-id="074c8-164">A prémium szintű Storage ajánlott gyorsítótár beállításai, ha az SQL-adatfájlok és naplófájlok (nem pedig egy virtuális Merevlemezt, amely egyaránt tartalmaz) külön virtuális merevlemezek alkalmazandó egyszerűbb.</span><span class="sxs-lookup"><span data-stu-id="074c8-164">It is simpler to apply the Premium Storage recommended cache settings when you have separate VHDs for the SQL Data files and log files (rather than a single VHD that contains both).</span></span>

> [!NOTE]
> <span data-ttu-id="074c8-165">Ha SQL Server adatainak és naplókönyvtárainak fájlokat ugyanazon a köteten, a gyorsítótár lehetőséget választja a IO hozzáférési minták a adatbázis munkaterhelések függ.</span><span class="sxs-lookup"><span data-stu-id="074c8-165">If you have SQL Server data and log files on the same volume, the caching option you choose depends on the IO access patterns for your database workloads.</span></span> <span data-ttu-id="074c8-166">Csak tesztelési is bemutatják, milyen gyorsítótárazás esetén ajánlott ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="074c8-166">Only testing can demonstrate which caching option is best for this scenario.</span></span>
>
>

<span data-ttu-id="074c8-167">Azonban, amelyek össze több virtuális merevlemezzel, akkor tekintse meg a Windows a tárolóhelyek használata a eredeti parancsfájlok azonosításához, amelyek csatlakoztatott virtuális merevlemezek olyan milyen adott készletben, így után beállíthatja a gyorsítótár beállításait ennek megfelelően az egyes lemezek.</span><span class="sxs-lookup"><span data-stu-id="074c8-167">However, if you are using Windows Storage Spaces which are made up of multiple VHDs you will need to look at your original scripts to identify which attached VHDs are in what specific pool, so you can then set the cache settings accordingly for each disk.</span></span>

<span data-ttu-id="074c8-168">Nincs elérhető mutatjuk be, amely a VHD-k leképezi a tárolókészlet eredeti parancsfájlt, ha az alábbi lépések segítségével határozza meg a lemez tárolási készlet leképezési.</span><span class="sxs-lookup"><span data-stu-id="074c8-168">If you do not have original script available to show you which VHDs map to the storage pool, you can use the following steps to determine the disk/storage pool mapping.</span></span>

<span data-ttu-id="074c8-169">Az egyes lemezek tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="074c8-169">For each disk, use the following steps:</span></span>

1. <span data-ttu-id="074c8-170">Virtuális gép és csatlakoztatott lemezek listájának beszerzése a **Get-AzureVM** parancs:</span><span class="sxs-lookup"><span data-stu-id="074c8-170">Get list of disks attached to VM with the **Get-AzureVM** command:</span></span>

    <span data-ttu-id="074c8-171">Get-AzureVM - ServiceName <servicename> -név <vmname> |} Get-AzureDataDisk</span><span class="sxs-lookup"><span data-stu-id="074c8-171">Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk</span></span>
2. <span data-ttu-id="074c8-172">Jegyezze fel a Diskname és a logikai Egységet.</span><span class="sxs-lookup"><span data-stu-id="074c8-172">Note the Diskname and LUN.</span></span>

    ![DisknameAndLUN][2]
3. <span data-ttu-id="074c8-174">Távoli asztali kapcsolatot a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="074c8-174">Remote desktop into the VM.</span></span> <span data-ttu-id="074c8-175">Ezután lépjen **számítógép-kezelés** | **Eszközkezelő** | **lemezmeghajtók**.</span><span class="sxs-lookup"><span data-stu-id="074c8-175">Then go to **Computer Management** | **Device Manager** | **Disk Drives**.</span></span> <span data-ttu-id="074c8-176">Nézze meg a "Microsoft virtuális lemezek" tulajdonságairól</span><span class="sxs-lookup"><span data-stu-id="074c8-176">Look at the properties of each of the ‘Microsoft Virtual Disks’</span></span>

    ![VirtualDiskProperties][3]
4. <span data-ttu-id="074c8-178">Itt a LUN számot egy hivatkozást a LUN számot, ha a virtuális merevlemez csatolását virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="074c8-178">The LUN number here is a reference to the LUN number you specify when attaching the VHD to the VM.</span></span>
5. <span data-ttu-id="074c8-179">Az a "Microsoft virtuális lemez" Ugrás a **részletek** lapon ezt a a **tulajdonság** listájában keresse fel **illesztőprogram kulcs**.</span><span class="sxs-lookup"><span data-stu-id="074c8-179">For the ‘Microsoft Virtual Disk’ go to the **Details** tab, then in the **Property** list, go to **Driver Key**.</span></span> <span data-ttu-id="074c8-180">Az a **érték**, vegye figyelembe a **eltolás**, ez az az alábbi képernyőképen 0002.</span><span class="sxs-lookup"><span data-stu-id="074c8-180">In the **Value**, note the **Offset**, which is 0002 in the following screenshot.</span></span> <span data-ttu-id="074c8-181">A 0002 azt jelzi, hogy a Fizikailemez2, amely a tárolókészlet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="074c8-181">The 0002 denotes the PhysicalDisk2 that the storage pool references.</span></span>

    ![VirtualDiskPropertyDetails][4]
6. <span data-ttu-id="074c8-183">Minden egyes tárolókészlethez memóriakép el a társított lemezekkel:</span><span class="sxs-lookup"><span data-stu-id="074c8-183">For each storage pool, dump out the associated disks:</span></span>

    <span data-ttu-id="074c8-184">Get-StoragePool - FriendlyName AMS1pooldata |} Get-PhysicalDisk</span><span class="sxs-lookup"><span data-stu-id="074c8-184">Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk</span></span>

    ![GetStoragePool][5]

<span data-ttu-id="074c8-186">Most már használhatja ezt az információt, rendelje hozzá a VHD-k csatolva tárolókészletek a fizikai lemezek.</span><span class="sxs-lookup"><span data-stu-id="074c8-186">Now you can use this information to associate attached VHDs to Physical Disks in Storage Pools.</span></span>

<span data-ttu-id="074c8-187">Virtuális merevlemezek tárolókészletek a fizikai lemezek leképezése után, majd is leválasztani és keresztül másolja őket a prémium szintű Storage-fiók, majd csatolja őket a megfelelő gyorsítótár-beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="074c8-187">Once you have mapped VHDs to Physical Disks in Storage Pools you can then detach and copy them over to a Premium Storage account, then attach them with the correct cache setting.</span></span> <span data-ttu-id="074c8-188">Lásd: a példában a [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), 8 – 12 lépést.</span><span class="sxs-lookup"><span data-stu-id="074c8-188">Please see the example in the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steps 8 through 12.</span></span> <span data-ttu-id="074c8-189">A lépések bemutatják, hogyan bontsa ki a virtuális gép csatlakoztatott virtuális merevlemez lemezkonfigurációt CSV-fájlba, másolja a VHD-k, a lemez konfigurációs gyorsítótár beállításainak módosításához és végül telepítse újra a virtuális gép sorozataként DS VM a csatlakoztatott lemezeket.</span><span class="sxs-lookup"><span data-stu-id="074c8-189">These steps show how to extract a VM-attached VHD disk configuration to a CSV file, copy the VHDs, alter the disk configuration cache settings, and finally redeploy the VM as a DS series VM with all the attached disks.</span></span>

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a><span data-ttu-id="074c8-190">Virtuális gép tárolási sávszélesség és a virtuális merevlemez tárolási teljesítmény</span><span class="sxs-lookup"><span data-stu-id="074c8-190">VM storage bandwidth and VHD storage throughput</span></span>
<span data-ttu-id="074c8-191">A tárolási teljesítményt mértékét megadott DS * Virtuálisgép-méret és a virtuális merevlemez méretét.</span><span class="sxs-lookup"><span data-stu-id="074c8-191">The amount of storage performance depends on the DS* VM size specified and the VHD sizes.</span></span> <span data-ttu-id="074c8-192">A virtuális gépek rendelkeznek, különböző juttatások csatolt VHD-k számát és a maximális sávszélesség (MB/s) támogatja azokat.</span><span class="sxs-lookup"><span data-stu-id="074c8-192">The VMs have different allowances for the number of VHDs that can be attached and the maximum bandwidth they will support (MB/s).</span></span> <span data-ttu-id="074c8-193">Tekintse meg az adott sávszélesség számok [virtuális gépek és Felhőszolgáltatások mérete az Azure-](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="074c8-193">For the specific bandwidth numbers, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="074c8-194">Nagyobb IOPS mérete nagyobb a érhetők el.</span><span class="sxs-lookup"><span data-stu-id="074c8-194">Increased IOPS are achieved with larger disk sizes.</span></span> <span data-ttu-id="074c8-195">Ez akkor érdemes megfontolni, amikor az áttelepítési útvonalának.</span><span class="sxs-lookup"><span data-stu-id="074c8-195">You should consider this when you think about your migration path.</span></span> <span data-ttu-id="074c8-196">További információkért [lásd a táblázatot IOPS és lemeztípusok](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="074c8-196">For details, [see the table for IOPS and Disk Types](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span></span>

<span data-ttu-id="074c8-197">Mindemellett érdemes lehet megfontolnia, virtuális gépek támogatják az összes csatolt lemezek különböző maximális lemez sávszélesség rendelkezik-e.</span><span class="sxs-lookup"><span data-stu-id="074c8-197">Finally, consider that VMs have different maximum disk bandwidths they will support for all disks attached.</span></span> <span data-ttu-id="074c8-198">Nagy terhelés a maximális sávszélesség álljon rendelkezésre a Virtuálisgép-szerepkör méretéhez sikerült telítsük.</span><span class="sxs-lookup"><span data-stu-id="074c8-198">Under high load, you could saturate the maximum disk bandwidth available for that VM role size.</span></span> <span data-ttu-id="074c8-199">Például egy Standard_DS14 támogatja legfeljebb 512 MB/s. három P30 lemezzel, ezért a virtuális lemez sávszélességét telítsük sikerült.</span><span class="sxs-lookup"><span data-stu-id="074c8-199">For example a Standard_DS14 will support up to 512MB/s; therefore, with three P30 disks you could saturate the disk bandwidth of the VM.</span></span> <span data-ttu-id="074c8-200">De ebben a példában az átviteli sebesség korlátja sikerült túllépve, attól függően, hogy olvasási és írási IOs kombinációját.</span><span class="sxs-lookup"><span data-stu-id="074c8-200">But in this example, the throughput limit could be exceeded depending on the mix of read and write IOs.</span></span>

## <a name="new-deployments"></a><span data-ttu-id="074c8-201">Új központi telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="074c8-201">New deployments</span></span>
<span data-ttu-id="074c8-202">A következő két szakaszok bemutatják, hogyan telepítheti az SQL Server VMs prémium szintű Storage.</span><span class="sxs-lookup"><span data-stu-id="074c8-202">The next two sections demonstrate how you can deploy SQL Server VMs to Premium Storage.</span></span> <span data-ttu-id="074c8-203">Ahogy korábban említettük, nem feltétlenül kell elhelyezni az operációsrendszer-lemezképet, a prémium szintű storage.</span><span class="sxs-lookup"><span data-stu-id="074c8-203">As mentioned before, you do not necessarily need to place the OS disk onto Premium storage.</span></span> <span data-ttu-id="074c8-204">Akkor célszerű használni, ha bármely intenzív IO munkaterhelések helyezze az operációs rendszer virtuális merevlemez szándékos volt ehhez.</span><span class="sxs-lookup"><span data-stu-id="074c8-204">You might choose to do this if you are intending to place any intensive IO workloads on the OS VHD.</span></span>

<span data-ttu-id="074c8-205">Az első példa bemutatja, meglévő Azure-gyűjtemény lemezképei használatával.</span><span class="sxs-lookup"><span data-stu-id="074c8-205">The first example demonstrates utilizing existing Azure Gallery Images.</span></span> <span data-ttu-id="074c8-206">A második példában, hogy rendelkezik egy meglévő szabványos tárfiókban lévő egyéni VM-lemezkép használata.</span><span class="sxs-lookup"><span data-stu-id="074c8-206">The second example shows how to use a custom VM image that you have in an existing Standard storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="074c8-207">Ezek a példák feltételezik, hogy már létrehozta a regionális virtuális Hálózatot.</span><span class="sxs-lookup"><span data-stu-id="074c8-207">These examples assume that you have already created a Regional VNET.</span></span>
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a><span data-ttu-id="074c8-208">Prémium szintű Storage gyűjtemény lemezképpel új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-208">Create a new VM with Premium Storage with Gallery Image</span></span>
<span data-ttu-id="074c8-209">Az alábbi példa bemutatja, hogyan helyezze el az operációs rendszer virtuális merevlemez prémium szintű storage, és a prémium szintű Storage-VHD csatolása.</span><span class="sxs-lookup"><span data-stu-id="074c8-209">The example below shows how to place the OS VHD onto premium storage and attach Premium Storage VHDs.</span></span> <span data-ttu-id="074c8-210">Azonban az operációsrendszer-lemezképet is helyez egy standard szintű tárfiókot, és majd csatolja a VHD-k, amelyek tárolása a prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="074c8-210">However, you can also place the OS disk in a Standard Storage account and then attach VHDs that reside in a Premium Storage account.</span></span> <span data-ttu-id="074c8-211">Mindkét forgatókönyvet egy.</span><span class="sxs-lookup"><span data-stu-id="074c8-211">Both scenarios are demonstrated.</span></span>

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a><span data-ttu-id="074c8-212">1. lépés: A prémium szintű Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-212">Step 1: Create a Premium Storage Account</span></span>
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a><span data-ttu-id="074c8-213">2. lépés: Új felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-213">Step 2: Create a New Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a><span data-ttu-id="074c8-214">3. lépés: Lefoglalni egy felhőalapú szolgáltatás VIP (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="074c8-214">Step 3: Reserve a Cloud Service VIP (Optional)</span></span>
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a><span data-ttu-id="074c8-215">4. lépés:, Hozzon létre egy Virtuálisgép-tároló</span><span class="sxs-lookup"><span data-stu-id="074c8-215">Step 4: Create a VM Container</span></span>
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a><span data-ttu-id="074c8-216">5. lépés: az operációs rendszer virtuális Merevlemezt a Standard vagy prémium szintű Storage helyezi el.</span><span class="sxs-lookup"><span data-stu-id="074c8-216">Step 5: Placing OS VHD on Standard or Premium Storage</span></span>
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a><span data-ttu-id="074c8-217">6. lépés: Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-217">Step 6: Create VM</span></span>
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a><span data-ttu-id="074c8-218">A prémium szintű Storage egyéni lemezképként az új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-218">Create a new VM to use Premium Storage with a custom image</span></span>
<span data-ttu-id="074c8-219">Ebben a forgatókönyvben azt mutatja be, melyekben egy standard szintű tárfiókot található meglévő testre szabott lemezképet.</span><span class="sxs-lookup"><span data-stu-id="074c8-219">This scenario demonstrates where you have existing customized images that reside in a Standard Storage account.</span></span> <span data-ttu-id="074c8-220">Ahogy azt korábban említettük, ha el szeretné-e helyezni az operációs rendszer virtuális Merevlemezt a prémium szintű Storage kell, hogy létezik-e a lemezkép másolása a standard szintű tárfiók, és helyezze át a prémium szintű Storage használat előtt.</span><span class="sxs-lookup"><span data-stu-id="074c8-220">As mentioned if you want to place the OS VHD on Premium Storage you will need to copy the image that exists in the Standard Storage account and transfer them to a Premium Storage before it can be used.</span></span> <span data-ttu-id="074c8-221">Ha rendelkezik helyszíni kép, érdemes is ezt a módszert másolja, amely közvetlenül a prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="074c8-221">If you have an image on-premises, you can also use this method to copy that directly to the Premium Storage account.</span></span>

#### <a name="step-1-create-storage-account"></a><span data-ttu-id="074c8-222">1. lépés: Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-222">Step 1: Create Storage Account</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a><span data-ttu-id="074c8-223">2. lépés a felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-223">Step 2 Create Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a><span data-ttu-id="074c8-224">3. lépés: A meglévő kép használata</span><span class="sxs-lookup"><span data-stu-id="074c8-224">Step 3: Use existing image</span></span>
<span data-ttu-id="074c8-225">Egy meglévő lemezképet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="074c8-225">You can use an existing image.</span></span> <span data-ttu-id="074c8-226">Is [igénybe vehet egy meglévő számítógép lemezképét](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="074c8-226">Or, you can [take an image of an existing machine](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="074c8-227">Vegye figyelembe a gépet, akkor kép nem kell Tartományi * géphez.</span><span class="sxs-lookup"><span data-stu-id="074c8-227">Note the machine you image does not have to be DS* machine.</span></span> <span data-ttu-id="074c8-228">Miután a lemezképet, a következőket mutatják be a prémium szintű Storage-fiókkal, és másolja a **Start-AzureStorageBlobCopy** PowerShell-parancsmag segítségével.</span><span class="sxs-lookup"><span data-stu-id="074c8-228">Once you have the image, the following steps show how to copy it to the Premium Storage account with the **Start-AzureStorageBlobCopy** PowerShell commandlet.</span></span>

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a><span data-ttu-id="074c8-229">4. lépés: Másolat Blob Storage-fiókok között</span><span class="sxs-lookup"><span data-stu-id="074c8-229">Step 4: Copy Blob between Storage Accounts</span></span>
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a><span data-ttu-id="074c8-230">5. lépés: Rendszeresen ellenőrizze a példány állapotát:</span><span class="sxs-lookup"><span data-stu-id="074c8-230">Step 5: Regularly check copy status:</span></span>
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a><span data-ttu-id="074c8-231">6. lépés: Lemezt Azure előfizetés-tárház kép lemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="074c8-231">Step 6: Add Image disk to Azure disk Repository in Subscription</span></span>
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> <span data-ttu-id="074c8-232">Előfordulhat, hogy annak ellenére, hogy a állapotjelentések sikeres, sikertelen továbbra is megjelenik bérleti lemezhiba.</span><span class="sxs-lookup"><span data-stu-id="074c8-232">You may find that even though the status reports as success, you could still get a disk lease error.</span></span> <span data-ttu-id="074c8-233">Ebben az esetben várjon körülbelül 10 percet.</span><span class="sxs-lookup"><span data-stu-id="074c8-233">In this case, wait about 10 minutes.</span></span>
>
>

#### <a name="step-7--build-the-vm"></a><span data-ttu-id="074c8-234">7. lépés: A virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-234">Step 7:  Build the VM</span></span>
<span data-ttu-id="074c8-235">Itt hoz létre a virtuális Gépet a lemezkép és a VHD-k két Premium Storage:</span><span class="sxs-lookup"><span data-stu-id="074c8-235">Here you are building the VM from your image and attaching two Premium Storage VHDs:</span></span>

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a><span data-ttu-id="074c8-236">Always On rendelkezésre állási csoportok nem használó meglévő központi telepítések</span><span class="sxs-lookup"><span data-stu-id="074c8-236">Existing deployments that do not use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="074c8-237">A meglévő telepítések esetében először tekintse meg a [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="074c8-237">For existing deployments, first see the [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="074c8-238">Nincsenek az Always On rendelkezésre állási csoportok és azok, amelyek nem használó SQL Server-telepítések kapcsolatos szempontokat.</span><span class="sxs-lookup"><span data-stu-id="074c8-238">There are different considerations for SQL Server deployments that do not use Always On Availability Groups and those that do.</span></span> <span data-ttu-id="074c8-239">Ha nem használják a mindig bekapcsolva, és rendelkezik egy meglévő önálló SQL Server, prémium szintű Storage frissíthet egy új felhőalapú szolgáltatás és a tárolási fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="074c8-239">If you are not using Always On and have an existing standalone SQL Server, you can upgrade to Premium Storage by using a new cloud service and storage account.</span></span> <span data-ttu-id="074c8-240">Vegye figyelembe a következő lehetőségeket:</span><span class="sxs-lookup"><span data-stu-id="074c8-240">Consider the following options:</span></span>

* <span data-ttu-id="074c8-241">**Hozzon létre egy új SQL Server virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="074c8-241">**Create a new SQL Server VM**.</span></span> <span data-ttu-id="074c8-242">Új SQL Server virtuális gép egy prémium szintű Storage-fiókot használó hozhat létre, az új központi megfelelően.</span><span class="sxs-lookup"><span data-stu-id="074c8-242">You can create a new SQL Server VM that uses a Premium Storage account, as documented in New Deployments.</span></span> <span data-ttu-id="074c8-243">Majd készítsen biztonsági másolatot, és az SQL Server-konfigurációs és felhasználói adatbázisok visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="074c8-243">Then backup and restore your SQL Server configuration and user databases.</span></span> <span data-ttu-id="074c8-244">Az alkalmazás kell frissíteni, hogy az új SQL Server hivatkoznak, ha azt kívül és belül is hozzáférnek.</span><span class="sxs-lookup"><span data-stu-id="074c8-244">The application will need to be updated to reference the new SQL Server if it is being accessed internally or externally.</span></span> <span data-ttu-id="074c8-245">Minden "kívül db" objektumok másolja, mintha csinált (SxS) SQL Server párhuzamos áttelepítés kellene.</span><span class="sxs-lookup"><span data-stu-id="074c8-245">You would need to copy all ‘out of db’ objects as if you were doing a Side by Side (SxS) SQL Server migration.</span></span> <span data-ttu-id="074c8-246">Ez magában foglalja az objektumok, például a bejelentkezési adatok, a tanúsítványokat és a csatolt kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="074c8-246">This includes objects such as logins, certificates, and linked servers.</span></span>
* <span data-ttu-id="074c8-247">**Telepítse át a meglévő SQL Server virtuális**.</span><span class="sxs-lookup"><span data-stu-id="074c8-247">**Migrate an existing SQL Server VM**.</span></span> <span data-ttu-id="074c8-248">Ehhez szükséges az SQL Server virtuális gép offline állapotba helyezése, majd áthelyezte azt egy új felhőalapú szolgáltatás, mely tartalmazza, másolja a csatlakoztatott virtuális merevlemezek mindegyikét a prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="074c8-248">This will require taking the SQL Server VM offline, then transferring it to a new cloud service, which includes copying all of its attached VHDs to the Premium Storage account.</span></span> <span data-ttu-id="074c8-249">A virtuális gép online állapotba kerül, ha az alkalmazás a kiszolgáló állomásneve, mielőtt hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="074c8-249">When the VM comes online, the application will reference the server host name as before.</span></span> <span data-ttu-id="074c8-250">Vegye figyelembe, hogy a meglévő lemez mérete befolyásolja a teljesítményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="074c8-250">Be aware that the size of the existing disk will affect the performance characteristics.</span></span> <span data-ttu-id="074c8-251">Például egy 400 GB lemezterület lekérdezi kerekíti egy P20.</span><span class="sxs-lookup"><span data-stu-id="074c8-251">For example, a 400 GB disk gets rounded up to a P20.</span></span> <span data-ttu-id="074c8-252">Ha tudja, hogy nincs szüksége a lemez teljesítménye akkor képes a virtuális Gépet a DS adatsorozat virtuális gépként hozza létre, és csatolja a prémium szintű Storage VHD-k az mérete/teljesítmény specifikáció van szüksége.</span><span class="sxs-lookup"><span data-stu-id="074c8-252">If you know that you do not require that disk performance, then you could recreate the VM as a DS Series VM, and attach Premium Storage VHDs of the size/performance specification you require.</span></span> <span data-ttu-id="074c8-253">Ezután leválasztása sikerült, és csatlakoztassa újra az SQL-adatbázis a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="074c8-253">Then you could detach and reattach the SQL DB files.</span></span>

> [!NOTE]
> <span data-ttu-id="074c8-254">Érdemes figyelembe méretétől függően a mérete, a virtuális merevlemez a lemezek másolásának azt jelenti, hogy prémium szintű tároló lemez típusát esnek, ez határozza meg a lemez teljesítménye megadását.</span><span class="sxs-lookup"><span data-stu-id="074c8-254">When copying the VHD disks you should be aware of the size, depending on the size will mean what Premium Storage Disk type they fall into, this determines disk performance specification.</span></span> <span data-ttu-id="074c8-255">Mentés a legközelebbi lemezre round Azure lesz mérete, így ha egy 400 GB lemezterület, ez kerekíti az egy P20.</span><span class="sxs-lookup"><span data-stu-id="074c8-255">Azure will round up to the nearest disk size, so if you have a 400GB disk, this will be rounded up to a P20.</span></span> <span data-ttu-id="074c8-256">Attól függően, hogy a meglévő IO-követelményeket az operációs rendszer virtuális merevlemez nincs szükség lehet át ezt a prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="074c8-256">Depending on your existing IO requirements of the OS VHD, you might not need to migrate this to a Premium Storage account.</span></span>
>
>

<span data-ttu-id="074c8-257">Ha az SQL Server külsőleg érhető el, a cloud service VIP változik.</span><span class="sxs-lookup"><span data-stu-id="074c8-257">If your SQL Server is accessed externally, then the cloud service VIP will change.</span></span> <span data-ttu-id="074c8-258">Is kell frissítés végpontok, a hozzáférés-vezérlési listák és a DNS-beállításait.</span><span class="sxs-lookup"><span data-stu-id="074c8-258">You will also have to update end points, ACLs, and DNS settings.</span></span>

## <a name="existing-deployments-that-use-always-on-availability-groups"></a><span data-ttu-id="074c8-259">Always On rendelkezésre állási csoportok használó meglévő központi telepítések</span><span class="sxs-lookup"><span data-stu-id="074c8-259">Existing deployments that use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="074c8-260">A meglévő telepítések esetében először tekintse meg a [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="074c8-260">For existing deployments, first see the [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="074c8-261">Kezdetben ebben a szakaszban követően áttekintjük hogyan Always On Azure hálózatkezelési működjön.</span><span class="sxs-lookup"><span data-stu-id="074c8-261">Initially in this section we will look at how Always On interacts with Azure Networking.</span></span> <span data-ttu-id="074c8-262">Azt fogja majd lebontva két olyan eset az áttelepítés: áttelepítéseket, ahol is megengedett némi állásidővel, és áttelepítéseket, ahol minimális állásidővel kell elérni.</span><span class="sxs-lookup"><span data-stu-id="074c8-262">We will then break down migrations in to two scenarios: migrations where some downtime can be tolerated and migrations where you must achieve minimal downtime.</span></span>

<span data-ttu-id="074c8-263">A helyszíni SQL Server Always On rendelkezésre állási csoportok használatára a figyelő a helyi, amely egy virtuális DNS-nevet egy IP-cím, egy vagy több SQL Server-kiszolgálók között megosztott együtt regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="074c8-263">On-premises SQL Server Always On Availability Groups use a Listener on-premises that registers a virtual DNS name along with an IP address that is shared between one or more SQL Servers.</span></span> <span data-ttu-id="074c8-264">Ha az ügyfelek csatlakoznak a figyelő az elsődleges SQL-kiszolgáló IP-keresztül halad.</span><span class="sxs-lookup"><span data-stu-id="074c8-264">When clients connect they are routed through the listener IP to the Primary SQL Server.</span></span> <span data-ttu-id="074c8-265">Ez az a kiszolgáló, amely a mindig az IP-erőforrás tulajdonosa adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="074c8-265">This is the server that owns the Always On IP resource at that time.</span></span>

![A DeploymentsUseAlways][6]

<span data-ttu-id="074c8-267">A Microsoft Azure lehet hozzárendelni egy hálózati Adaptert a virtuális Gépen csak egy IP-címet így az ugyanazon a helyszínen, mint absztrakciós réteget eléréséhez Azure használja az IP-cím, amely hozzá van rendelve a belső/külső terheléselosztó (ILB/ELB).</span><span class="sxs-lookup"><span data-stu-id="074c8-267">In Microsoft Azure you can have only one IP address assigned to a NIC on the VM, so in order to achieve the same layer of abstraction as on-premises, Azure utilizes the IP address that is assigned to the Internal/External Load Balancers (ILB/ELB).</span></span> <span data-ttu-id="074c8-268">Az IP-erőforrás, amelyet a kiszolgálók között, a ILB-/ ELB azonos IP-van beállítva.</span><span class="sxs-lookup"><span data-stu-id="074c8-268">The IP resource that is shared between the servers is set to the same IP as the ILB/ELB.</span></span> <span data-ttu-id="074c8-269">Ez a DNS-ben közzétett és ügyfélforgalmat a ILB-/ ELB azt az SQL-kiszolgáló elsődleges replikára továbbítja.</span><span class="sxs-lookup"><span data-stu-id="074c8-269">This is published in the DNS, and client traffic is passed through the ILB/ELB to the Primary SQL Server replica.</span></span> <span data-ttu-id="074c8-270">A ILB-/ ELB tudja, melyik SQL Server elsődleges óta mintavételt számára, hogy megvizsgálja a mindig az IP-erőforrást használ.</span><span class="sxs-lookup"><span data-stu-id="074c8-270">The ILB/ELB knows which SQL Server is primary since it uses probes to probe the Always On IP resource.</span></span> <span data-ttu-id="074c8-271">Az előző példában azt mintavétel az üzembe helyezett ELB/ILB által hivatkozott végpont minden csomópont, a attól válaszol az elsődleges SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="074c8-271">In the previous example, it probes each node that has an endpoint referenced by the ELB/ILB, whichever responds is the Primary SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="074c8-272">A Példánynak és üzembe helyezett ELB mindkét rendelt adott Azure cloud Service szolgáltatásra, ezért bármely áttelepülés a felhőbe az Azure-ban lesz valószínűleg azt jelenti, hogy a Load Balancer IP változik.</span><span class="sxs-lookup"><span data-stu-id="074c8-272">The ILB and ELB are both assigned to a particular Azure cloud service, therefore any cloud migration in Azure will most likely mean that the Load Balancer IP will change.</span></span>
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a><span data-ttu-id="074c8-273">Áttelepítése mindig a központi telepítések engedélyezhetik bizonyos időre leállítást</span><span class="sxs-lookup"><span data-stu-id="074c8-273">Migrating Always On deployments that can allow some downtime</span></span>
<span data-ttu-id="074c8-274">Nincsenek mindig a központi telepítései áttelepítésének, amely lehetővé teszi bizonyos időre leállítást két stratégiák:</span><span class="sxs-lookup"><span data-stu-id="074c8-274">There are two strategies to migrate Always On deployments that allow for some downtime:</span></span>

1. <span data-ttu-id="074c8-275">**Több másodlagos replika hozzáadása egy meglévő mindig a fürthöz**</span><span class="sxs-lookup"><span data-stu-id="074c8-275">**Add more secondary replicas to an existing Always On Cluster**</span></span>
2. <span data-ttu-id="074c8-276">**Új mindig a fürt áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="074c8-276">**Migrate to a new Always On Cluster**</span></span>

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a><span data-ttu-id="074c8-277">1. Több másodlagos replika hozzáadása egy meglévő mindig a fürthöz</span><span class="sxs-lookup"><span data-stu-id="074c8-277">1. Add more Secondary Replicas to an Existing Always On Cluster</span></span>
<span data-ttu-id="074c8-278">Egyik stratégia, hogy több másodlagos adatbázis hozzáadása az Always On rendelkezésre állási csoportnak.</span><span class="sxs-lookup"><span data-stu-id="074c8-278">One strategy is to add more secondaries to the Always On Availability Group.</span></span> <span data-ttu-id="074c8-279">Meg kell vennie ezeket az új felhőalapú szolgáltatás, és frissítse a figyelő az új load balancer IP-cím.</span><span class="sxs-lookup"><span data-stu-id="074c8-279">You need to add these into a new cloud service and update the listener with the new load balancer IP.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="074c8-280">Állásidő pontok:</span><span class="sxs-lookup"><span data-stu-id="074c8-280">Points of downtime:</span></span>
* <span data-ttu-id="074c8-281">A fürt ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="074c8-281">Cluster Validation.</span></span>
* <span data-ttu-id="074c8-282">Új másodlagos adatbázis-tesztelési mindig a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="074c8-282">Testing Always On failovers for New Secondaries.</span></span>

<span data-ttu-id="074c8-283">Használata Windows Tárolókészletek a virtuális Gépen belül magasabb IO átviteli sebesség eléréséhez, akkor a rendszer offline állapotra állítja a teljes fürt ellenőrzése során.</span><span class="sxs-lookup"><span data-stu-id="074c8-283">If you are using Windows Storage Pools within the VM for higher IO throughput, then these will be taken offline during a Full Cluster Validation.</span></span> <span data-ttu-id="074c8-284">A teszttel ellenőrizheti, ha a csomópontok hozzáadása a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="074c8-284">The validation test is required when you add nodes to the cluster.</span></span> <span data-ttu-id="074c8-285">A teszt futtatása szükséges idő változhat, így kell tesztelje, hogy mennyi ideig Ez eltarthat egy megközelítőleges időpont, amikor a beolvasandó reprezentatív tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="074c8-285">The time it takes to run the test can vary, so you should test this in your representative test environment to get an approximate time of how long this will take.</span></span>

<span data-ttu-id="074c8-286">Idő, ahol végezheti el kézi feladatátvételre és tesztelési chaos csomóponton az újonnan hozzáadott magas rendelkezésre állású mindig a funkciók a várt módon kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="074c8-286">You should provision time where you can perform manual failover and chaos testing on the newly added nodes to ensure Always On High Availability functions as expected.</span></span>

![DeploymentUseAlways On2][7]

> [!NOTE]
> <span data-ttu-id="074c8-288">A Tárolókészletek helyének SQL Server összes példányát le kell állítani az érvényesítés futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="074c8-288">You should stop all instances of SQL Server where the Storage Pools are used before the Validation runs.</span></span>
>
> ##### <a name="high-level-steps"></a><span data-ttu-id="074c8-289">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="074c8-289">High-level steps</span></span>
>

1. <span data-ttu-id="074c8-290">Hozzon létre új felhőszolgáltatás csatlakoztatott prémium szintű Storage két új SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="074c8-290">Create two new SQL Servers in new cloud service with attached Premium Storage.</span></span>
2. <span data-ttu-id="074c8-291">Másolja át a teljes biztonsági mentést, és állítsa vissza a **NORECOVERY**.</span><span class="sxs-lookup"><span data-stu-id="074c8-291">Copy over FULL backups and restore with **NORECOVERY**.</span></span>
3. <span data-ttu-id="074c8-292">Másolja át a "kívül felhasználói DB" függő objektumok, például a bejelentkezéseket stb.</span><span class="sxs-lookup"><span data-stu-id="074c8-292">Copy over ‘out of user DB’ dependent objects, such as logins etc.</span></span>
4. <span data-ttu-id="074c8-293">Létrehozhat új egy új belső Load Balancer (ILB), illetve egy külső Load Balancer (ELB) használja, majd állítsa be a terhelés eloszlik végpontok mindkét új csomópontjának.</span><span class="sxs-lookup"><span data-stu-id="074c8-293">Create new a new Internal Load Balancer (ILB) or use an External Load Balancer (ELB), and then set up Load Balanced Endpoints on both new nodes.</span></span>

   > [!NOTE]
   > <span data-ttu-id="074c8-294">Ellenőrizze minden csomópont van a megfelelő végpont-konfiguráció, a folytatás előtt</span><span class="sxs-lookup"><span data-stu-id="074c8-294">Check all Nodes have the correct Endpoint configuration before you continue</span></span>
   >
   >
5. <span data-ttu-id="074c8-295">Állítsa le a felhasználó vagy alkalmazás-hozzáférés az SQL Server (ha Tárolókészletek használata).</span><span class="sxs-lookup"><span data-stu-id="074c8-295">Stop User/Application Access to the SQL Server (if using Storage Pools).</span></span>
6. <span data-ttu-id="074c8-296">SQL Server adatbázismotor-szolgáltatások leállítása az összes olyan csomóponton, (ha Tárolókészletek használata).</span><span class="sxs-lookup"><span data-stu-id="074c8-296">Stop SQL Server Engine Services on All Nodes (if using Storage Pools).</span></span>
7. <span data-ttu-id="074c8-297">A fürt, és futtassa teljes ellenőrzést új csomópontok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="074c8-297">Add new Nodes to cluster and run full validation.</span></span>
8. <span data-ttu-id="074c8-298">Miután az ellenőrzés nem jelez hibát, indítsa el az összes SQL Server szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="074c8-298">Once Validation is successful, start all SQL Server Services.</span></span>
9. <span data-ttu-id="074c8-299">Tranzakciós naplók biztonsági mentése és visszaállítása felhasználói adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="074c8-299">Backup Transaction logs, and restore user databases.</span></span>
10. <span data-ttu-id="074c8-300">Vegyen fel új csomópontok az Always On rendelkezésre állási csoportnak, és helyezze el a replikációs **szinkron**.</span><span class="sxs-lookup"><span data-stu-id="074c8-300">Add new nodes into the Always On Availability Group and place replication into **Synchronous**.</span></span>
11. <span data-ttu-id="074c8-301">Adja hozzá az IP-cím erőforrás, az új felhőalapú szolgáltatás ILB-/ ELB Powershellen keresztül, az Always On többhelyes példa alapján a [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="074c8-301">Add the IP address resource of the new Cloud Service ILB/ELB through PowerShell for Always On based on the Multi-site example in the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span> <span data-ttu-id="074c8-302">A Windows fürtszolgáltatás, állítsa be a **lehetséges tulajdonosok** , a **IP-cím** erőforrás a régi új csomóponttal.</span><span class="sxs-lookup"><span data-stu-id="074c8-302">In Windows clustering, set the **Possible owners** of the **IP Address** resource to the new nodes old.</span></span> <span data-ttu-id="074c8-303">A "Hozzáadás IP-cím erőforrás ugyanazon az alhálózaton" című része a [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="074c8-303">See the ‘Adding IP Address Resource on Same Subnet’ section of the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
12. <span data-ttu-id="074c8-304">Feladatátvétel az új csomópontok egyikére.</span><span class="sxs-lookup"><span data-stu-id="074c8-304">Failover to one of the new nodes.</span></span>
13. <span data-ttu-id="074c8-305">Ellenőrizze az új csomópontok automatikus feladatátvételi partnerként és a teszt feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="074c8-305">Make the new nodes Auto Failover Partners and test failovers.</span></span>
14. <span data-ttu-id="074c8-306">Távolítsa el az eredeti csomópont a rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="074c8-306">Remove original nodes from Availability Group.</span></span>

##### <a name="advantages"></a><span data-ttu-id="074c8-307">Előnyei</span><span class="sxs-lookup"><span data-stu-id="074c8-307">Advantages</span></span>
* <span data-ttu-id="074c8-308">Új SQL-kiszolgálók lehet tesztelni (SQL Server alkalmazás) előtt mindig a történő hozzáadásuk.</span><span class="sxs-lookup"><span data-stu-id="074c8-308">New SQL Servers can be tested (SQL Server and Application) before they are added to Always On.</span></span>
* <span data-ttu-id="074c8-309">Módosítsa a Virtuálisgép-méretet, és testre szabhatja a tárolót az pontos igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="074c8-309">You can change the VM size and customize the storage to your exact requirements.</span></span> <span data-ttu-id="074c8-310">Azonban hasznos lenne azonos tartani az SQL teljes elérési utat.</span><span class="sxs-lookup"><span data-stu-id="074c8-310">However, it would be beneficial to keep all the SQL file paths the same.</span></span>
* <span data-ttu-id="074c8-311">Az adatbázis biztonsági mentések átvitele a másodlagos replikák elkezdésének szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="074c8-311">You can control when the transfer of the DB backups to the Secondary Replicas are started.</span></span> <span data-ttu-id="074c8-312">Ez eltér az Azure használatával **Start-AzureStorageBlobCopy** parancsmag VHD-k, másolása, mert ez egy aszinkron másolatot.</span><span class="sxs-lookup"><span data-stu-id="074c8-312">This differs from using Azure **Start-AzureStorageBlobCopy** commandlet to copy VHDs, because that is an asynchronous copy.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="074c8-313">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="074c8-313">Disadvantages</span></span>
* <span data-ttu-id="074c8-314">Windows Tárolókészletek használata esetén nincs fürt állásidő az új további csomópontokat a teljes fürt ellenőrzése során.</span><span class="sxs-lookup"><span data-stu-id="074c8-314">When using Windows Storage Pools, there is Cluster downtime during the Full Cluster Validation for the new additional nodes.</span></span>
* <span data-ttu-id="074c8-315">Attól függően, hogy az SQL Server verziója és a másodlagos replikák meglévő számát akkor nem feltétlenül adhat hozzá további másodlagos replikák meglévő másodlagos adatbázis eltávolítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="074c8-315">Depending on the SQL Server Version and the existing number of secondary replicas, you might not be able to add more secondary replicas without removing existing secondaries.</span></span>
* <span data-ttu-id="074c8-316">Előfordulhat, hogy hosszú SQL adatátviteli idő a másodlagos adatbázis beállítása közben.</span><span class="sxs-lookup"><span data-stu-id="074c8-316">There could be long SQL data transfer time while setting up the secondaries.</span></span>
* <span data-ttu-id="074c8-317">Nincs további költség nélkül az áttelepítés során előfordulhat, hogy a párhuzamosan futó új gépek.</span><span class="sxs-lookup"><span data-stu-id="074c8-317">There is additional cost during migration while you have new machines running in parallel.</span></span>

#### <a name="2-migrate-to-a-new-always-on-cluster"></a><span data-ttu-id="074c8-318">2. Új mindig a fürt áttelepítése</span><span class="sxs-lookup"><span data-stu-id="074c8-318">2. Migrate to a new Always On Cluster</span></span>
<span data-ttu-id="074c8-319">Egy másik olyan stratégia, hogy hozzon létre egy új mindig a fürtöt új csomópontot az új felhőalapú szolgáltatás, és majd irányítsa át az ügyfelek számára azt.</span><span class="sxs-lookup"><span data-stu-id="074c8-319">Another strategy is to create a brand new Always On Cluster with brand new nodes in new cloud service and then redirect the clients to use it.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="074c8-320">Állásidő pontok</span><span class="sxs-lookup"><span data-stu-id="074c8-320">Points of downtime</span></span>
<span data-ttu-id="074c8-321">Alkalmazások és a felhasználók az új mindig a figyelő az átvitel során, nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="074c8-321">There is downtime when you transfer applications and users to the new Always On listener.</span></span> <span data-ttu-id="074c8-322">A leállás függ:</span><span class="sxs-lookup"><span data-stu-id="074c8-322">The downtime depends on:</span></span>

* <span data-ttu-id="074c8-323">Végső tranzakciónapló biztonsági mentései új kiszolgálókon lévő adatbázisok visszaállítása szükséges idő.</span><span class="sxs-lookup"><span data-stu-id="074c8-323">The time taken to restore final transaction log backups to databases on new servers.</span></span>
* <span data-ttu-id="074c8-324">Ügyfél-alkalmazások új mindig a figyelő az frissítéséhez szükséges idő.</span><span class="sxs-lookup"><span data-stu-id="074c8-324">The time taken to update client applications to use new Always On listener.</span></span>

##### <a name="advantages"></a><span data-ttu-id="074c8-325">Előnyei</span><span class="sxs-lookup"><span data-stu-id="074c8-325">Advantages</span></span>
* <span data-ttu-id="074c8-326">SQL Server, a tényleges éles környezetben lehet tesztelni és operációs rendszer a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="074c8-326">You can test the actual production environment, SQL Server, and OS build changes.</span></span>
* <span data-ttu-id="074c8-327">Lehetősége van a tárolási testreszabásához és potenciálisan a virtuális gép méretének csökkentésére.</span><span class="sxs-lookup"><span data-stu-id="074c8-327">You have the option to customize the storage and to potentially reduce size of VM.</span></span> <span data-ttu-id="074c8-328">Emiatt költségek csökkentéséhez.</span><span class="sxs-lookup"><span data-stu-id="074c8-328">This could result in cost reduction.</span></span>
* <span data-ttu-id="074c8-329">A SQL Server build vagy a verziójával frissítheti a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="074c8-329">You can update your SQL Server build or version during this process.</span></span> <span data-ttu-id="074c8-330">Az operációs rendszer is lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="074c8-330">You can also upgrade the Operating System.</span></span>
* <span data-ttu-id="074c8-331">Az előző mindig a fürt teli visszaállítás céljaként működhet.</span><span class="sxs-lookup"><span data-stu-id="074c8-331">The previous Always On Cluster can act as a solid rollback target.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="074c8-332">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="074c8-332">Disadvantages</span></span>
* <span data-ttu-id="074c8-333">Módosítsa a figyelő a DNS-nevét, ha azt szeretné, hogy mindkét fut egyszerre mindig a fürtök kell.</span><span class="sxs-lookup"><span data-stu-id="074c8-333">You need to change the DNS name of the listener if you want both Always On clusters running simultaneously.</span></span> <span data-ttu-id="074c8-334">Ezzel hozzáad adminisztrációs terhet az áttelepítés során, az ügyfél alkalmazás karakterláncok tükröznie kell az új figyelő nevét.</span><span class="sxs-lookup"><span data-stu-id="074c8-334">This adds administration overhead during the migration as client application strings must reflect the new Listener name.</span></span>
* <span data-ttu-id="074c8-335">Meg kell valósítani a szinkronizálási mechanizmus, hogy továbbra is a végleges szinkronizálást követelmények áttelepítés előtt minimalizálása érdekében a lehető legközelebb a két környezet közötti.</span><span class="sxs-lookup"><span data-stu-id="074c8-335">You must implement a synchronization mechanism between the two environments to keep them as close as possible to minimize the final synchronization requirements before migration.</span></span>
* <span data-ttu-id="074c8-336">Hiba kerül költség az áttelepítés során előfordulhat, hogy az új futtató környezet.</span><span class="sxs-lookup"><span data-stu-id="074c8-336">There is added cost during migration while you have the new environment running.</span></span>

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a><span data-ttu-id="074c8-337">Áttelepítése mindig a központi telepítések a minimális állásidő érdekében</span><span class="sxs-lookup"><span data-stu-id="074c8-337">Migrating Always On Deployments for minimal downtime</span></span>
<span data-ttu-id="074c8-338">Nincsenek áttelepítése mindig a központi telepítés két stratégiák a minimális állásidő érdekében:</span><span class="sxs-lookup"><span data-stu-id="074c8-338">There are two strategies for migrating Always On deployments for minimal downtime:</span></span>

1. <span data-ttu-id="074c8-339">**Egy létező másodlagos használják: egy helyen**</span><span class="sxs-lookup"><span data-stu-id="074c8-339">**Utilize an Existing Secondary: Single-Site**</span></span>
2. <span data-ttu-id="074c8-340">**Meglévő másodlagos másodpéldányt használják: többhelyes**</span><span class="sxs-lookup"><span data-stu-id="074c8-340">**Utilize Existing Secondary Replica(s): Multi-Site**</span></span>

#### <a name="1-utilize-an-existing-secondary-single-site"></a><span data-ttu-id="074c8-341">1. Egy létező másodlagos használják: egy helyen</span><span class="sxs-lookup"><span data-stu-id="074c8-341">1. Utilize an existing secondary: Single-Site</span></span>
<span data-ttu-id="074c8-342">A minimális állásidő érdekében egyik stratégia, hogy egy meglévő felhőalapú másodlagos igénybe vehet, és eltávolítja az aktuális felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="074c8-342">One strategy for minimal downtime is to take an existing cloud secondary and remove it from the current cloud service.</span></span> <span data-ttu-id="074c8-343">Ezután másolja a virtuális merevlemezek az új prémium szintű Storage-fiókot, és a virtuális gép létrehozása az új felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="074c8-343">Then copy the VHDs to the new Premium Storage account, and create the VM in the new cloud service.</span></span> <span data-ttu-id="074c8-344">A figyelő a fürtszolgáltatás és a feladatátvételi frissíteni.</span><span class="sxs-lookup"><span data-stu-id="074c8-344">Then update the listener in clustering and failover.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="074c8-345">Állásidő pontok</span><span class="sxs-lookup"><span data-stu-id="074c8-345">Points of downtime</span></span>
* <span data-ttu-id="074c8-346">Amikor a végső csomópontja az elosztott terhelésű végpont, nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="074c8-346">There is downtime when you update the final node with the Load Balanced endpoint.</span></span>
* <span data-ttu-id="074c8-347">Az ügyfél újracsatlakozás ügyfél és a DNS-konfigurációtól függően előfordulhat, hogy később.</span><span class="sxs-lookup"><span data-stu-id="074c8-347">Your client reconnection might be delayed depending on your client/DNS configuration.</span></span>
* <span data-ttu-id="074c8-348">Ha választja, a fürt mindig a csoport offline állapotba ki az IP-címek felcserélése, nincs további állásidő.</span><span class="sxs-lookup"><span data-stu-id="074c8-348">There is additional downtime if you choose to take the Always On Cluster group offline to swap out the IP addresses.</span></span> <span data-ttu-id="074c8-349">Ez a hozzáadott IP-cím erőforrás egy OR függőségi és a lehetséges tulajdonosok használatával elkerülheti a.</span><span class="sxs-lookup"><span data-stu-id="074c8-349">You can avoid this by using an OR dependency and Possible Owners for the added IP Address resource.</span></span> <span data-ttu-id="074c8-350">A "Hozzáadás IP-cím erőforrás ugyanazon az alhálózaton" című része a [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="074c8-350">See the ‘Adding IP Address Resource on Same Subnet’ section of the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>

> [!NOTE]
> <span data-ttu-id="074c8-351">Ha azt szeretné, hogy a hozzáadott részt vesz a partnerként mindig a feladatátvevő csomópont kell egy hivatkozást a terhelés eloszlik beállítása az Azure végpont hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="074c8-351">When you want the added node to partake in as an Always On Failover Partner, you need to add an Azure Endpoint with a reference to the Load Balanced Set.</span></span> <span data-ttu-id="074c8-352">Amikor futtatja a **Add-AzureEndpoint** ehhez parancsot, nyissa meg a jelenlegi kapcsolatok maradjon, de a figyelő új kapcsolatot nem fogja tudni hozható létre, amíg a terheléselosztó frissítette.</span><span class="sxs-lookup"><span data-stu-id="074c8-352">When you run the **Add-AzureEndpoint** command to do this, current connections to remain open, but new connections to the listener will not be able to be established until the load balancer has updated.</span></span> <span data-ttu-id="074c8-353">A tesztelés ez fordult elő az elmúlt 90-120seconds, ez kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="074c8-353">In testing this was seen to last 90-120seconds, this should be tested.</span></span>
>
>

##### <a name="advantages"></a><span data-ttu-id="074c8-354">Előnyei</span><span class="sxs-lookup"><span data-stu-id="074c8-354">Advantages</span></span>
* <span data-ttu-id="074c8-355">Nincsenek további az áttelepítés során felmerülő költség.</span><span class="sxs-lookup"><span data-stu-id="074c8-355">No extra cost incurred during migration.</span></span>
* <span data-ttu-id="074c8-356">-Az-egyhez áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="074c8-356">A one-to-one migration.</span></span>
* <span data-ttu-id="074c8-357">Csökkentett összetettségét.</span><span class="sxs-lookup"><span data-stu-id="074c8-357">Reduced complexity.</span></span>
* <span data-ttu-id="074c8-358">Lehetővé teszi, hogy a megnövekedett IOPS a prémium szintű Storage SKU.</span><span class="sxs-lookup"><span data-stu-id="074c8-358">Allows for increased IOPS from Premium Storage SKUs.</span></span> <span data-ttu-id="074c8-359">Ha a lemezt leválasztani a virtuális gép, és másolja az új felhőalapú szolgáltatás, a 3. fél eszköz a virtuális merevlemez méretét, magasabb teljesítmények biztosító használható.</span><span class="sxs-lookup"><span data-stu-id="074c8-359">When the disks are detached from the VM and copied to the new cloud service, a 3rd party tool can be used to increase the VHD size, which provides higher throughputs.</span></span> <span data-ttu-id="074c8-360">Virtuális merevlemez mérete növekszik, lásd: a [fórum vitafórum](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span><span class="sxs-lookup"><span data-stu-id="074c8-360">For increasing VHD sizes, see this [forum discussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="074c8-361">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="074c8-361">Disadvantages</span></span>
* <span data-ttu-id="074c8-362">Nincs magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási átmenetileg megszakad az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="074c8-362">There is a temporary loss of HA and DR during migration.</span></span>
* <span data-ttu-id="074c8-363">Ez ugyanis egy 1:1-áttelepítés, a minimális virtuális gép mérete által támogatott virtuális merevlemezek, a száma, ezért nem lehet a virtuális gépek downsize használni fog.</span><span class="sxs-lookup"><span data-stu-id="074c8-363">As this is a 1:1 migration, you will have to use a minimum VM size that will support your number of VHDs, so you might not be able to downsize your VMs.</span></span>
* <span data-ttu-id="074c8-364">Ebben a forgatókönyvben szeretné használni az Azure **Start-AzureStorageBlobCopy** parancsmag, amely aszinkron.</span><span class="sxs-lookup"><span data-stu-id="074c8-364">This scenario would use the Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="074c8-365">Nincs nincs SLA-t a Másolás befejezése.</span><span class="sxs-lookup"><span data-stu-id="074c8-365">There is no SLA on copy completion.</span></span> <span data-ttu-id="074c8-366">A példányok idő változó, amíg ez függ a várakozási sorban is függ a mennyiségű adatot továbbít.</span><span class="sxs-lookup"><span data-stu-id="074c8-366">The time of the copies varies, while this depends on wait in queue it will also depend on the amount of data to transfer.</span></span> <span data-ttu-id="074c8-367">A másolási idő növeli a Ha az adatátvitelt lesz egy másik Azure-adatközponthoz, amely támogatja a prémium szintű Storage egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="074c8-367">The copy time increases if the transfer is going to another Azure data center that supports Premium Storage in another region.</span></span> <span data-ttu-id="074c8-368">Ha csak 2 csomópontok, inkább egy lehetséges megoldás, abban az esetben a Másolás tovább tart, mint a tesztelés.</span><span class="sxs-lookup"><span data-stu-id="074c8-368">If you just have 2 nodes, consider a possible mitigation in case the copy takes longer than in testing.</span></span> <span data-ttu-id="074c8-369">Ez magában foglalhatja a következő ötleteket.</span><span class="sxs-lookup"><span data-stu-id="074c8-369">This could include the following ideas.</span></span>
  * <span data-ttu-id="074c8-370">Adja hozzá egy ideiglenes 3. az SQL Server-csomópont a magas rendelkezésre ÁLLÁSÚ egyeztetett állásidővel az áttelepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="074c8-370">Add a temporary 3rd SQL Server node for HA before the migration with agreed downtime.</span></span>
  * <span data-ttu-id="074c8-371">Futtassa az áttelepítés Azure ütemezett karbantartás kívül.</span><span class="sxs-lookup"><span data-stu-id="074c8-371">Run the migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="074c8-372">Győződjön meg arról, a fürt kvóruma megfelelően konfigurálta-e.</span><span class="sxs-lookup"><span data-stu-id="074c8-372">Ensure you have configured your cluster quorum correctly.</span></span>  

##### <a name="high-level-steps"></a><span data-ttu-id="074c8-373">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="074c8-373">High-level steps</span></span>
<span data-ttu-id="074c8-374">Ez a dokumentum nem bemutatása teljes végpont példa, azonban a [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) részletesen is javítható, ha ezt elvégezni.</span><span class="sxs-lookup"><span data-stu-id="074c8-374">This document does not demonstrate a complete end to end example, however the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) provides details that can be leveraged to perform this.</span></span>

![MinimalDowntime][8]

* <span data-ttu-id="074c8-376">Gyűjtse össze a lemezkonfigurációt, és távolítsa el a csomópont (ne törölje a csatolt VHD-k).</span><span class="sxs-lookup"><span data-stu-id="074c8-376">Gather disk configuration, and remove the node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="074c8-377">Prémium szintű Storage-fiók létrehozása, és másolja a standard szintű tárfiók VHD-k</span><span class="sxs-lookup"><span data-stu-id="074c8-377">Create a Premium Storage account and copy VHDs from the Standard Storage account</span></span>
* <span data-ttu-id="074c8-378">Új felhőalapú szolgáltatás létrehozása, és telepítse újra a virtuális gép SQL2, amely a felhőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="074c8-378">Create new cloud service and redeploy the SQL2 VM in that cloud service.</span></span> <span data-ttu-id="074c8-379">A másolt eredeti operációs rendszer virtuális Merevlemezt használ, és a másolt VHD-virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="074c8-379">Create the VM using the copied original OS VHD and attaching the copied VHDs.</span></span>
* <span data-ttu-id="074c8-380">Konfigurálja a ILB / ELB és végpont-hozzáadáshoz.</span><span class="sxs-lookup"><span data-stu-id="074c8-380">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="074c8-381">Frissítés figyelő egyike:</span><span class="sxs-lookup"><span data-stu-id="074c8-381">Update Listener by either:</span></span>
  * <span data-ttu-id="074c8-382">A mindig a csoport offline állapotba helyezése, és a mindig a figyelő frissítése új ILB és üzembe helyezett ELB IP-cím.</span><span class="sxs-lookup"><span data-stu-id="074c8-382">Taking the Always On Group offline and updating the Always On Listener with new ILB / ELB IP address.</span></span>
  * <span data-ttu-id="074c8-383">Vagy az IP-cím erőforrás az új felhőalapú szolgáltatás ILB-/ ELB Powershellen keresztül történő Windows-fürtszolgáltatás hozzáadására.</span><span class="sxs-lookup"><span data-stu-id="074c8-383">Or adding the IP address resource of new Cloud Service ILB/ELB through PowerShell into Windows clustering.</span></span> <span data-ttu-id="074c8-384">Ezután állítsa be az IP-cím erőforrás lehetséges tulajdonosainak áttelepített csomóponthoz, SQL2, és ez állítja be a hálózati név vagy függőséghez.</span><span class="sxs-lookup"><span data-stu-id="074c8-384">Then set the Possible owners of the IP Address resource to the migrated node, SQL2, and set this as OR dependency in the Network Name.</span></span> <span data-ttu-id="074c8-385">A "Hozzáadás IP-cím erőforrás ugyanazon az alhálózaton" című része a [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="074c8-385">See the ‘Adding IP Address Resource on Same Subnet’ section of the [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
* <span data-ttu-id="074c8-386">Ellenőrizze a konfigurációs DNS propagálási az ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="074c8-386">Check DNS configuration/propogation to the clients.</span></span>
* <span data-ttu-id="074c8-387">Telepítse át az sql1 számítógép virtuális gép, és nyissa meg a 2 – 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="074c8-387">Migrate SQL1 VM, and go through steps 2 – 4.</span></span>
* <span data-ttu-id="074c8-388">Lépéseket 5ii használata, majd adja hozzá az sql1 számítógép lehetséges tulajdonosaként a hozzáadott IP-cím erőforrás</span><span class="sxs-lookup"><span data-stu-id="074c8-388">If using steps 5ii, then add SQL1 as a Possible Owner for the added IP Address Resource</span></span>
* <span data-ttu-id="074c8-389">Feladatátvétel tesztelése.</span><span class="sxs-lookup"><span data-stu-id="074c8-389">Test failovers.</span></span>

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a><span data-ttu-id="074c8-390">2. Meglévő másodlagos másodpéldányt használják: többhelyes</span><span class="sxs-lookup"><span data-stu-id="074c8-390">2. Utilize existing secondary replica(s): Multi-Site</span></span>
<span data-ttu-id="074c8-391">Ha több Azure-adatközpontban (DC) vannak olyan csomópontok, vagy ha hibrid környezettel rendelkezik, majd egy mindig a konfigurációt használhatja ebben a környezetben az állásidő minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="074c8-391">If you have nodes in more than one Azure datacenter (DC) or if you have a hybrid environment, then you can use an Always On configuration in this environment to minimize downtime.</span></span>

<span data-ttu-id="074c8-392">A megoldás, a mindig bekapcsolva szinkronizálás átállítása szinkron a helyszíni vagy Azure a másodlagos Tartományvezérlőt, majd feladatátvétel keresztül az adott SQL Server.</span><span class="sxs-lookup"><span data-stu-id="074c8-392">The approach is to change the Always On synchronization to Synchronous for the on-premises or secondary Azure DC, and then failover over to that SQL Server.</span></span> <span data-ttu-id="074c8-393">Ezután másolja a VHD-k a prémium szintű Storage-fiókra, és telepítse újra a gépet az új felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="074c8-393">Then copy the VHDs to a Premium Storage account, and redeploy the machine into a new cloud service.</span></span> <span data-ttu-id="074c8-394">A figyelő frissítése, és ezután a feladat-visszavételt.</span><span class="sxs-lookup"><span data-stu-id="074c8-394">Update the listener, and then fail back.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="074c8-395">Állásidő pontok</span><span class="sxs-lookup"><span data-stu-id="074c8-395">Points of downtime</span></span>
<span data-ttu-id="074c8-396">A leállás áll a feladatátvételt a másodlagos tartományvezérlő és vissza időt.</span><span class="sxs-lookup"><span data-stu-id="074c8-396">The downtime consists of the time to failover to the alternative DC and back.</span></span> <span data-ttu-id="074c8-397">Is függ, az ügyfél és a DNS-konfiguráció, és később, az ügyfél lehet újracsatlakozni.</span><span class="sxs-lookup"><span data-stu-id="074c8-397">It also depends on your client/DNS configuration and your client reconnection may be delayed.</span></span>
<span data-ttu-id="074c8-398">Vegye figyelembe a következő példa egy hibrid mindig a konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="074c8-398">Consider the following example of a hybrid Always On configuration:</span></span>

![MultiSite1][9]

##### <a name="advantages"></a><span data-ttu-id="074c8-400">Előnyei</span><span class="sxs-lookup"><span data-stu-id="074c8-400">Advantages</span></span>
* <span data-ttu-id="074c8-401">Úgy használhatja a meglévő infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="074c8-401">You can utilize existing infrastructure.</span></span>
* <span data-ttu-id="074c8-402">Lehetősége van a frissítés előtti először a vész-Helyreállítási Azure rendszerű tartományvezérlőn történő tárolása az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="074c8-402">You have the option to pre-upgrade the Azure storage on the DR Azure DC first.</span></span>
* <span data-ttu-id="074c8-403">A vész-Helyreállítási Azure DC tárolási újra kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="074c8-403">The DR Azure DC storage can be reconfigured.</span></span>
* <span data-ttu-id="074c8-404">Nincs legalább két feladatátvételi teszteket kivéve az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="074c8-404">There is a minimum of two failovers during migration, excluding test failovers.</span></span>
* <span data-ttu-id="074c8-405">Nem kell áthelyezni az SQL Server-adatok biztonsági mentése és visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="074c8-405">You do not need to move SQL Server data with backup and restore.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="074c8-406">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="074c8-406">Disadvantages</span></span>
* <span data-ttu-id="074c8-407">Attól függően, hogy az ügyfél-hozzáférési az SQL-kiszolgálón lehet nagyobb késéseket amikor SQL Server egy másik tartományvezérlő futtatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="074c8-407">Depending on client access to SQL Server, there might be increased latency when SQL Server is running in an alternative DC to the application.</span></span>
* <span data-ttu-id="074c8-408">A másolási idő a VHD-k prémium szintű Storage hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="074c8-408">The copy time of VHDs to Premium storage could be long.</span></span> <span data-ttu-id="074c8-409">Ez befolyásolhatja a döntést kell-e őrizni a csomópont a rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="074c8-409">This might affect your decision on whether to keep the node in the Availability Group.</span></span> <span data-ttu-id="074c8-410">Megfontolandó szempontok a amikor napló intenzív feladata betölti az áttelepítés során futnak, mivel az elsődleges csomópont kell tartani a árvává tranzakciókat a tranzakciónaplóban.</span><span class="sxs-lookup"><span data-stu-id="074c8-410">Consider this for when log intensive work loads are running during the migration is required, since the Primary node will have to keep the unreplicated transactions in its transaction log.</span></span> <span data-ttu-id="074c8-411">Ezért ez sikerült jelentősen megnő.</span><span class="sxs-lookup"><span data-stu-id="074c8-411">Therefore this could grow significantly.</span></span>
* <span data-ttu-id="074c8-412">Ebben a forgatókönyvben szeretné használni az Azure **Start-AzureStorageBlobCopy** parancsmag, amely aszinkron.</span><span class="sxs-lookup"><span data-stu-id="074c8-412">This scenario would use the Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="074c8-413">Nincs nincs SLA befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="074c8-413">There is no SLA on completion.</span></span> <span data-ttu-id="074c8-414">A példányok idő változik, ez függ a várakozási sorban, miközben is függ a mennyiségű adatot továbbít.</span><span class="sxs-lookup"><span data-stu-id="074c8-414">The time of the copies varies, while this depends on wait in queue, it will also depend on the amount of data to transfer.</span></span> <span data-ttu-id="074c8-415">Ezért csak egy csomópont van a 2. adatközpontban, abban az esetben a Másolás tovább tart, mint a tesztelés megoldás lépéseket kell tennie.</span><span class="sxs-lookup"><span data-stu-id="074c8-415">Therefore you just have one node in your 2nd data center, you should take mitigation steps in case the copy takes longer than in testing.</span></span> <span data-ttu-id="074c8-416">Ez magában foglalhatja a következő ötleteket.</span><span class="sxs-lookup"><span data-stu-id="074c8-416">This could include the following ideas.</span></span>
  * <span data-ttu-id="074c8-417">Adja hozzá a 2. az SQL ideiglenes csomópontot a magas rendelkezésre ÁLLÁSÚ egyeztetett állásidővel az áttelepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="074c8-417">Add a temporary 2nd SQL node for HA before the migration with agreed downtime.</span></span>
  * <span data-ttu-id="074c8-418">Futtassa az áttelepítés Azure ütemezett karbantartás kívül.</span><span class="sxs-lookup"><span data-stu-id="074c8-418">Run the migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="074c8-419">Győződjön meg arról, a fürt kvóruma megfelelően konfigurálta-e.</span><span class="sxs-lookup"><span data-stu-id="074c8-419">Ensure you have configured your cluster quorum correctly.</span></span>

<span data-ttu-id="074c8-420">Ez a forgatókönyv feltételezi, hogy rendelkezik saját telepítési dokumentált, és tudja, hogyan hozzá van rendelve a tárhely optimális gyorsítótár beállításainak módosításához.</span><span class="sxs-lookup"><span data-stu-id="074c8-420">This scenario assumes that you have documented your install and know how the storage is mapped in order to make changes for optimal disk cache settings.</span></span>

##### <a name="high-level-steps"></a><span data-ttu-id="074c8-421">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="074c8-421">High-level steps</span></span>
![Multisite2][10]

* <span data-ttu-id="074c8-423">Ellenőrizze a helyszíni / Azure-tartományvezérlő az SQL Server elsődleges alternatív, és azt a más automatikus feladatátvételi Partner (AFP).</span><span class="sxs-lookup"><span data-stu-id="074c8-423">Make the on-premises / alternate Azure DC the SQL Server Primary, and make it the other Auto Failover Partner (AFP).</span></span>
* <span data-ttu-id="074c8-424">Lemez konfigurációs adatokat gyűjtsön a SQL2, és távolítsa el a csomópont (ne törölje a csatolt VHD-k).</span><span class="sxs-lookup"><span data-stu-id="074c8-424">Gather disk configuration information from SQL2, and remove the node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="074c8-425">Prémium szintű Storage-fiók létrehozása, és másolja a standard szintű tárfiók VHD-k.</span><span class="sxs-lookup"><span data-stu-id="074c8-425">Create a Premium Storage account and copy VHDs from the Standard Storage account.</span></span>
* <span data-ttu-id="074c8-426">Hozzon létre egy új felhőalapú szolgáltatás, és hozzon létre a SQL2 virtuális Gépet a díjak tárolólemezek csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="074c8-426">Create a new cloud service and create the SQL2 VM with its Premiums Storage disks attached.</span></span>
* <span data-ttu-id="074c8-427">Konfigurálja a ILB / ELB és végpont-hozzáadáshoz.</span><span class="sxs-lookup"><span data-stu-id="074c8-427">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="074c8-428">Frissítse a mindig a figyelő új ILB / ELB IP cím és a teszt feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="074c8-428">Update the Always On Listener with new ILB / ELB IP address and test failover.</span></span>
* <span data-ttu-id="074c8-429">Ellenőrizze a DNS-beállításait.</span><span class="sxs-lookup"><span data-stu-id="074c8-429">Check the DNS configuration.</span></span>
* <span data-ttu-id="074c8-430">Módosítsa a AFP SQL2, majd telepítse át az sql1 számítógép és végrehajtania 2 – 5. lépéseket.</span><span class="sxs-lookup"><span data-stu-id="074c8-430">Change the AFP to SQL2, and then migrate SQL1 and go through steps 2 – 5.</span></span>
* <span data-ttu-id="074c8-431">Feladatátvétel tesztelése.</span><span class="sxs-lookup"><span data-stu-id="074c8-431">Test failovers.</span></span>
* <span data-ttu-id="074c8-432">Váltás a AFP SQL1 és SQL2</span><span class="sxs-lookup"><span data-stu-id="074c8-432">Switch the AFP back to SQL1 and SQL2</span></span>

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a><span data-ttu-id="074c8-433">Függelék: A prémium szintű Storage mindig a fürt egy többhelyes áttelepítése</span><span class="sxs-lookup"><span data-stu-id="074c8-433">Appendix: Migrating a Multisite Always On Cluster to Premium Storage</span></span>
<span data-ttu-id="074c8-434">Ez a témakör további része a többhelyes mindig a fürt átalakítani a prémium szintű Storage egy részletes példát biztosít.</span><span class="sxs-lookup"><span data-stu-id="074c8-434">The remainder of this topic provides a detailed example of converting a multi-site Always On cluster to Premium storage.</span></span> <span data-ttu-id="074c8-435">Is átalakítja a figyelő a külső terheléselosztással (ELB) egy belső terheléselosztón (ILB).</span><span class="sxs-lookup"><span data-stu-id="074c8-435">It also converts the Listener from using an external load balancer (ELB) to an internal load balancer (ILB).</span></span>

### <a name="environment"></a><span data-ttu-id="074c8-436">Környezet</span><span class="sxs-lookup"><span data-stu-id="074c8-436">Environment</span></span>
* <span data-ttu-id="074c8-437">2 KB-os Windows 12 / 2 KB-os SQL 12</span><span class="sxs-lookup"><span data-stu-id="074c8-437">Windows 2k12 / SQL 2k12</span></span>
* <span data-ttu-id="074c8-438">SP 1 DB fájlok</span><span class="sxs-lookup"><span data-stu-id="074c8-438">1 DB Files on SP</span></span>
* <span data-ttu-id="074c8-439">Tárolókészletek csomópontonként x 2</span><span class="sxs-lookup"><span data-stu-id="074c8-439">2 x Storage Pools per Node</span></span>

![Appendix1][11]

### <a name="vm"></a><span data-ttu-id="074c8-441">VIRTUÁLIS GÉP:</span><span class="sxs-lookup"><span data-stu-id="074c8-441">VM:</span></span>
<span data-ttu-id="074c8-442">Ebben a példában fogjuk áthelyezése egy üzembe helyezett ELB ILB bemutatása.</span><span class="sxs-lookup"><span data-stu-id="074c8-442">In this example we are going to demonstrate moving from an ELB to ILB.</span></span> <span data-ttu-id="074c8-443">Üzembe helyezett ELB volt elérhető ILB, mielőtt, így ez azt jelenti, hogy hogyan lehet váltani a az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="074c8-443">ELB was available before ILB, so this shows how to switch to this during the migration.</span></span>

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a><span data-ttu-id="074c8-445">Előkészítő lépések: Csatlakozás-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="074c8-445">Pre Steps: Connect to Subscription</span></span>
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a><span data-ttu-id="074c8-446">1. lépés: Új Tárfiók létrehozása, és a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="074c8-446">Step 1: Create New Storage Account and Cloud Service</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a><span data-ttu-id="074c8-447">2. lépés: Az erőforrás engedélyezett hibák növelése<Optional></span><span class="sxs-lookup"><span data-stu-id="074c8-447">Step 2: Increase the permitted failures on resources <Optional></span></span>
<span data-ttu-id="074c8-448">Bizonyos erőforrások, amelyek az Always On rendelkezésre állási csoportnak nincsenek korlátozások a hány sikertelen végrehajtása esetén, amelyek adott időszakban, amikor a Fürtszolgáltatás megpróbálja újraindítani az erőforráscsoport fordul elő.</span><span class="sxs-lookup"><span data-stu-id="074c8-448">On certain resources that belong to your Always On Availability Group there are limits on how many failures that can occur in a period, where the cluster service will attempt to restart the resource group.</span></span> <span data-ttu-id="074c8-449">Növeli a miközben meg van útmutató alapján ez az eljárás óta Ha ezt elmulasztja manuálisan eseményindító és feladatátvételi feladatátvételek állítja le ezt a határt hamarosan kaphat gépek ajánlott.</span><span class="sxs-lookup"><span data-stu-id="074c8-449">It is recommended you increase this whilst you are walking through this procedure, since if you don’t manually failover and trigger failovers by shutting down machines you can get close to this limit.</span></span>

<span data-ttu-id="074c8-450">A hiba támogatást ehhez a Feladatátvevőfürt-kezelőben, kétszeres körültekintő lenne keresse fel a mindig bekapcsolva erőforráscsoport tulajdonságait:</span><span class="sxs-lookup"><span data-stu-id="074c8-450">It would be prudent to double the failure allowance, to do this in Failover Cluster Manager, go to the Properties of the Always On resource group:</span></span>

![Appendix3][13]

<span data-ttu-id="074c8-452">Módosítsa a maximális hibák 6.</span><span class="sxs-lookup"><span data-stu-id="074c8-452">Change the Maximum Failures to 6.</span></span>

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a><span data-ttu-id="074c8-453">3. lépés: IP-cím hozzáadása erőforrás fürt csoport<Optional></span><span class="sxs-lookup"><span data-stu-id="074c8-453">Step 3: Addition IP Address resource for Cluster Group <Optional></span></span>
<span data-ttu-id="074c8-454">Ha csak egy IP-cím a fürt csoport, és ez a felhő alhálózati igazodik, ügyeljen arra, ha véletlenül kapcsolat nélküli minden fürtcsomópontnak a felhőben, majd a fürt IP-erőforrás a hálózaton, és a fürt hálózati neve nem lesz képes online állapotba.</span><span class="sxs-lookup"><span data-stu-id="074c8-454">If you have only one IP address for the Cluster Group and this is aligned to the cloud subnet, beware, if you accidentally take offline all cluster nodes in the cloud on that network then the Cluster IP resource and Cluster Network Name will not be able to come online.</span></span> <span data-ttu-id="074c8-455">Esetén ez megakadályozza majd frissítések a fürt más erőforrásai.</span><span class="sxs-lookup"><span data-stu-id="074c8-455">In the event of this it will prevent updates to other cluster resources.</span></span>

#### <a name="step-4-dns-configuration"></a><span data-ttu-id="074c8-456">4. lépés: DNS-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="074c8-456">Step 4: DNS configuration</span></span>
<span data-ttu-id="074c8-457">Zavartalan végrehajtásához átmenet attól függ, hogyan DNS folyamatban van szükség, és frissíti.</span><span class="sxs-lookup"><span data-stu-id="074c8-457">To implement a smooth transition depends on how DNS is being utilized and updated.</span></span>
<span data-ttu-id="074c8-458">Mindig telepítve van, ha létrehoz egy Windows fürterőforrás-csoporthoz, ha Feladatátvevőfürt-kezelő megnyitásához jelenik meg, hogy legalább három erőforrások lesz, a kettő, hogy a dokumentum hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="074c8-458">When Always On is installed, it creates a Windows Cluster Resource group, if you open Failover Cluster Manager, you will see that at a minimum it will have three resources, the two that the document refers to are:</span></span>

* <span data-ttu-id="074c8-459">Virtuális hálózat neve (VNN) – Ez az a DNS-nevét, hogy az ügyfélszámítógépek csatlakozhatnak, ha szeretne csatlakozni az SQL Server Always On keresztül.</span><span class="sxs-lookup"><span data-stu-id="074c8-459">Virtual Network Name (VNN) – This is the DNS name that client connect to when wanting to connect to SQL Servers via Always On.</span></span>
* <span data-ttu-id="074c8-460">IP-cím erőforrás – Ez a VNN társított IP-cím, több is van, és többhelyes konfigurációban hely/alhálózatot / IP-cím lesz.</span><span class="sxs-lookup"><span data-stu-id="074c8-460">IP Address Resource – This is the IP address that associated with the VNN, you can have more than one, and in a multisite configuration you will have an IP address per site/subnet.</span></span>

<span data-ttu-id="074c8-461">Ha a Kapcsolódás az SQL Server, az SQL Server Client illesztőprogram lekéri a DNS-rekordokat, a figyelő társított, és csatlakozni próbálnak minden mindig a hozzárendelt IP-cím, alatt arról lesz szó néhány befolyásoló tényezők is ez.</span><span class="sxs-lookup"><span data-stu-id="074c8-461">When connecting to SQL Server, the SQL Server Client driver will retrieve the DNS records associated with the listener and try to connect to each Always On associated IP address, below we discuss some factors that can influence this.</span></span>

<span data-ttu-id="074c8-462">A figyelő neve társított egyidejű DNS-rekordok száma attól függ, nem csak kapcsolódó, IP-címek száma, de a "RegisterAllIpProviders'setting a Feladatátvételi fürtszolgáltatás az Always ON VNN erőforrás.</span><span class="sxs-lookup"><span data-stu-id="074c8-462">The number of concurrent DNS records that are associated with the listener name depends not only on the number of IP addresses associated, but the ‘RegisterAllIpProviders’setting in Failover Clustering for the Always ON VNN resource.</span></span>

<span data-ttu-id="074c8-463">Ha telepít mindig az Azure-ban különböző lépést a figyelő és IP-címek létrehozása, kell manuálisan konfigurálnia a "RegisterAllIpProviders' 1, ez eltér a egy helyszíni mindig a telepítés már állítottak 1.</span><span class="sxs-lookup"><span data-stu-id="074c8-463">When you deploy Always On in Azure there are different steps to create the Listener and IP Addresses, you have to manually configure the ‘RegisterAllIpProviders’ to 1, this is different to an on-premises Always On deployment where it is already set to 1.</span></span>

<span data-ttu-id="074c8-464">Ha a "RegisterAllIpProviders" értéke 0, majd csak akkor jelenik meg egy a figyelő társított DNS-rekordot a DNS-ben:</span><span class="sxs-lookup"><span data-stu-id="074c8-464">If ‘RegisterAllIpProviders’ is 0, then you will only see one DNS record in DNS associated with the Listener:</span></span>

![Appendix4][14]

<span data-ttu-id="074c8-466">Ha 'RegisterAllIpProviders' 1:</span><span class="sxs-lookup"><span data-stu-id="074c8-466">If ‘RegisterAllIpProviders’ is 1:</span></span>

![Appendix5][15]

<span data-ttu-id="074c8-468">Az alábbi kódot fogja írassa ki a VNN beállítások és állítsa be az Ön, Megjegyzés: a változtatás érvénybe léptetéséhez szüksége lesz a VNN offline állapotba, és kapcsolja vissza online-e véve a figyelő offline állapotban, amely az ügyfél-kapcsolódási problémákat.</span><span class="sxs-lookup"><span data-stu-id="074c8-468">The code below will dump out the VNN settings and set it for you, please note, for the change to take effect you will need to take the VNN offline and turn it back online, this taking the Listener offline causing client connectivity disruption.</span></span>

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

<span data-ttu-id="074c8-469">Egy későbbi lépésben áttelepítési kell frissíteni a mindig bekapcsolva figyelő frissített IP-címet, amely a használatával hivatkozik a terheléselosztó Ez magában foglalja az IP-cím erőforrás eltávolítása és hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="074c8-469">In a later migration step you will need to update the Always On listener with an updated IP address that will reference a load balancer, this will involve an IP Address resource removal and addition.</span></span> <span data-ttu-id="074c8-470">Az IP-frissítés után ellenőrizze az új IP-címet a DNS-zónában frissítették, és, hogy az ügyfelek frissítése a helyi gyorsítótárban kell.</span><span class="sxs-lookup"><span data-stu-id="074c8-470">After the IP update, you need to ensure the new IP address has been updated in DNS Zone and that the clients are updating their local DNS cache.</span></span>

<span data-ttu-id="074c8-471">Ha az ügyfelek egy másik hálózati szegmensben találhatók, és egy másik DNS-kiszolgáló hivatkozik, akkor fontolja meg, mi történik, DNS-zóna átviteli kapcsolatos az áttelepítés során az alkalmazás csatlakozzon újra, akkor fog megtörténni által korlátozott legalább bármely a zóna átviteli idő a figyelő új IP-címet.</span><span class="sxs-lookup"><span data-stu-id="074c8-471">If your clients reside in a different network segment and reference a different DNS server, you need to consider what happens about DNS Zone Transfer during the migration, as the application reconnect time will be constrained by at least the Zone Transfer Time of any new IP addresses for the listener.</span></span> <span data-ttu-id="074c8-472">Ha itt időkorlát alatt, érdemes tárgyalja, és a Windows-csoportok egy növekményes zónaletöltés kényszerítése, és a DNS-állomásrekord is helyezheti a egy alacsonyabb élettartam (TTL), ezért az ügyfelek frissítése.</span><span class="sxs-lookup"><span data-stu-id="074c8-472">If you are under time constraint here, you should discuss and test forcing an incremental zone transfer with your Windows teams, and also put the DNS host record to a lower Time To Live (TTL), so the clients update.</span></span> <span data-ttu-id="074c8-473">További információkért lásd: [növekményes zónaátvitelek](https://technet.microsoft.com/library/cc958973.aspx) és [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span><span class="sxs-lookup"><span data-stu-id="074c8-473">For more information, see [Incremental Zone Transfers](https://technet.microsoft.com/library/cc958973.aspx) and [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span></span>

<span data-ttu-id="074c8-474">Az élettartam a DNS-rekord, a figyelő az Always On Azure társított alapértelmezés az 1200-as másodperc.</span><span class="sxs-lookup"><span data-stu-id="074c8-474">By default the TTL for DNS Record that is associated with the Listener in Always On in Azure is 1200 seconds.</span></span> <span data-ttu-id="074c8-475">Kezdésként érdemes lehet csökkenteni Ez a korlátozás ahhoz, hogy az ügyfelek az áttelepítés során frissítse a DNS a frissített IP-cím a figyelő időpontja esetén.</span><span class="sxs-lookup"><span data-stu-id="074c8-475">You may wish to reduce this if you are under time constraint during your migration to ensure the clients update their DNS with the updated IP address for the listener.</span></span> <span data-ttu-id="074c8-476">Tekintse meg, és a konfiguráció módosítása a VNN konfigurációjának kimenő okai:</span><span class="sxs-lookup"><span data-stu-id="074c8-476">You can see and modify the configuration by dumping out the configuration of the VNN:</span></span>

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

<span data-ttu-id="074c8-477">Vegye figyelembe, minél kisebb a "HostRecordTTL", a DNS-forgalom nagyobb mennyiségű történik.</span><span class="sxs-lookup"><span data-stu-id="074c8-477">Please note, the lower the ‘HostRecordTTL’, a higher amount of DNS traffic will occur.</span></span>

##### <a name="client-application-settings"></a><span data-ttu-id="074c8-478">Ügyfél-Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="074c8-478">Client application settings</span></span>
<span data-ttu-id="074c8-479">Ha az SQL-ügyfélalkalmazást támogatja a .net 4.5 SQLClient, akkor használhatja "MULTISUBNETFAILOVER = TRUE" kulcsszót, ez ajánlott kell alkalmazni, mivel lehetővé teszi a gyorsabb kapcsolat SQL Always On rendelkezésre állási csoportnak a feladatátvétel során.</span><span class="sxs-lookup"><span data-stu-id="074c8-479">If your SQL client application supports the .Net 4.5 SQLClient, then you can use ‘MULTISUBNETFAILOVER=TRUE’ keyword, this is recommended to be applied as it allows for faster connection to SQL Always On Availability Group during failover.</span></span> <span data-ttu-id="074c8-480">A mindig bekapcsolva figyelő párhuzamosan társított összes IP-címek keresztül enumerálása, és átadta szigorúbb TCP-kapcsolat újrapróbálkozási sebességét.</span><span class="sxs-lookup"><span data-stu-id="074c8-480">It enumerates through all IP addresses associated with the Always On listener in parallel, and performs a more aggressive TCP connection retry speed during a failover.</span></span>

<span data-ttu-id="074c8-481">A fenti beállításokat kapcsolatos további információkért lásd: [MultiSubnetFailover kulcsszó és a kapcsolódó szolgáltatások](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span><span class="sxs-lookup"><span data-stu-id="074c8-481">For more information regarding the settings above, please see [MultiSubnetFailover Keyword and Associated Features](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span></span> <span data-ttu-id="074c8-482">Lásd még: [SqlClient támogatja a magas rendelkezésre állási, vészhelyreállítási](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="074c8-482">Also see [SqlClient Support for High Availability, Disaster Recovery](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span></span>

#### <a name="step-5-cluster-quorum-settings"></a><span data-ttu-id="074c8-483">5. lépés: Fürt kvórumbeállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="074c8-483">Step 5: Cluster quorum settings</span></span>
<span data-ttu-id="074c8-484">Legalább egy SQL Server le egyszerre tart kívánja, módosítania kell a fürt Kvórum beállítása, 2-csomópontokkal fájl megosztási tanúsító (FSW) használata, állítsa be a kvórum csomóponttöbbség engedélyezése és felhasználását a dinamikus szavazás , és ezek lehetővé teszik a egyetlen csomópontot állandó marad.</span><span class="sxs-lookup"><span data-stu-id="074c8-484">As you are going to be taking out at least one SQL Server down at a time, you should modify the cluster quorum setting, if using File Share Witness (FSW) with 2 nodes, you should set the quorum to allow node majority and utilize dynamic voting, and this is to allow for a single node to remain standing.</span></span>

    Set-ClusterQuorum -NodeMajority  

<span data-ttu-id="074c8-485">Kezelését és konfigurálását a fürtkvórum további információkért lásd: [konfigurálása és kezelése a Windows Server 2012 feladatátvevő fürt kvórum](https://technet.microsoft.com/library/jj612870.aspx).</span><span class="sxs-lookup"><span data-stu-id="074c8-485">For more information on managing and configuring the cluster quorum, please see [Configure and Manage the Quorum in a Windows Server 2012 Failover Cluster](https://technet.microsoft.com/library/jj612870.aspx).</span></span>

#### <a name="step-6-extract-existing-endpoints-and-acls"></a><span data-ttu-id="074c8-486">6. lépés: Bontsa ki a meglévő végpontok és hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="074c8-486">Step 6: Extract Existing Endpoints and ACLs</span></span>
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

<span data-ttu-id="074c8-487">Mentse azokat a szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="074c8-487">Save these to a text file.</span></span>

#### <a name="step-7-change-failover-partners-and-replication-modes"></a><span data-ttu-id="074c8-488">7. lépés: Feladatátvételi partnerként és replikációs mód módosítása</span><span class="sxs-lookup"><span data-stu-id="074c8-488">Step 7: Change Failover Partners and Replication Modes</span></span>
<span data-ttu-id="074c8-489">Ha 2-nél több SQL-kiszolgáló, "Szinkron" módosítsa a feladatátvételt egy másik másodlagos egy másik tartományvezérlő vagy a helyszíni és teszik az automatikus feladatátvételi Partner (AFP) kell, ez pedig, magas rendelkezésre ÁLLÁSÚ karbantartása, míg a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="074c8-489">If you have more than 2 SQL Servers, you should change the failover of another secondary in another DC or on-premises to ‘Synchronous’ and make it an Automatic Failover Partner (AFP), this is so you maintain HA whilst you are making changes.</span></span> <span data-ttu-id="074c8-490">Ezt megteheti a TSQL használatával is módosíthatja, ha a szolgáltatáshoz az SSMS:</span><span class="sxs-lookup"><span data-stu-id="074c8-490">You can do this through TSQL of modify though SSMS:</span></span>

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a><span data-ttu-id="074c8-492">8. lépés: Másodlagos virtuális gép eltávolítása a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="074c8-492">Step 8: Remove Secondary VM from cloud service</span></span>
<span data-ttu-id="074c8-493">Meg kell lennie áttelepítésének tervezése felhő másodlagos csomópont először, ha jelenleg az elsődleges, akkor kell kezdeményezni kézi feladatátvételre.</span><span class="sxs-lookup"><span data-stu-id="074c8-493">You should be planning to migrate a cloud secondary node first, if this is currently primary, you should initiate a manual failover.</span></span>

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="074c8-494">9. lépés: Lemez gyorsítótárazási beállítások a CSV-fájlban, és mentse</span><span class="sxs-lookup"><span data-stu-id="074c8-494">Step 9: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="074c8-495">A adatkötetek ezeket kell megadni csak OLVASHATÓ.</span><span class="sxs-lookup"><span data-stu-id="074c8-495">For data volumes these should be set to READONLY.</span></span>

<span data-ttu-id="074c8-496">TLOG kötetek ezeket érdemes lehet nincs értékűre állítani.</span><span class="sxs-lookup"><span data-stu-id="074c8-496">For TLOG volumes these should be set to NONE.</span></span>

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a><span data-ttu-id="074c8-498">10. lépés: Másolat VHD-k</span><span class="sxs-lookup"><span data-stu-id="074c8-498">Step 10: Copy VHDS</span></span>
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



<span data-ttu-id="074c8-499">A virtuális merevlemezek a prémium szintű Storage-fiók példány állapotát ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="074c8-499">You can check the copy status of the VHDs to the Premium Storage account:</span></span>

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

<span data-ttu-id="074c8-501">Várjon, amíg a fenti sikeres fájl rögzíti.</span><span class="sxs-lookup"><span data-stu-id="074c8-501">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="074c8-502">Az egyes blobok információt:</span><span class="sxs-lookup"><span data-stu-id="074c8-502">For information for individual blobs:</span></span>

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a><span data-ttu-id="074c8-503">11. lépés: az operációs rendszer Register lemez</span><span class="sxs-lookup"><span data-stu-id="074c8-503">Step 11: Register OS disk</span></span>
    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a><span data-ttu-id="074c8-504">12. lépés: Importálása másodlagos új felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="074c8-504">Step 12: Import secondary into new cloud service</span></span>
<span data-ttu-id="074c8-505">Az alábbi kódot is használ a hozzáadott itt importálhatja a gép és a retainable VIP használja.</span><span class="sxs-lookup"><span data-stu-id="074c8-505">The code below also uses the added option here you can import the machine and use the retainable VIP.</span></span>

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="074c8-506">13. lépés: Hozható létre ILB új felhő Svc, vegye fel a terhelés eloszlik a végpontok és hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="074c8-506">Step 13: Create ILB on New Cloud Svc, Add Load Balanced Endpoints and ACLs</span></span>
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a><span data-ttu-id="074c8-507">14. lépés: Mindig a frissítése.</span><span class="sxs-lookup"><span data-stu-id="074c8-507">Step 14: Update Always On</span></span>
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

<span data-ttu-id="074c8-509">Most távolítsa el a régi felhőalapú szolgáltatás IP-címet.</span><span class="sxs-lookup"><span data-stu-id="074c8-509">Now remove the old cloud service IP Address.</span></span>

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a><span data-ttu-id="074c8-511">15. lépés: A DNS-frissítési ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="074c8-511">Step 15: DNS update check</span></span>
<span data-ttu-id="074c8-512">Most DNS-kiszolgálók ellenőrizze az SQL Server ügyfél hálózatokon és győződjön meg arról, hogy a fürtszolgáltatás hozzáadta a felesleges állomásrekord a hozzáadott IP-cím.</span><span class="sxs-lookup"><span data-stu-id="074c8-512">You should now check DNS Servers on your SQL Server client networks and make sure that clustering has added the extra host record for the added IP address.</span></span> <span data-ttu-id="074c8-513">Ha ezeket a DNS-kiszolgálók nem frissítették, fontolja meg a DNS zónaletöltés kényszerítése, és győződjön meg arról, hogy az ügyfelek nem alhálózati képesek mindkét mindig az IP-címekhez, ez így nem kell várnia az automatikus DNS-replikáció az.</span><span class="sxs-lookup"><span data-stu-id="074c8-513">If those DNS servers have not updated, consider forcing a DNS Zone transfer and ensure that the clients in there subnet are able to resolve to both Always On IP Addresses, this is so you do not need to wait for automatic DNS replication.</span></span>

#### <a name="step-16-reconfigure-always-on"></a><span data-ttu-id="074c8-514">16. lépés: Mindig a újrakonfigurálása</span><span class="sxs-lookup"><span data-stu-id="074c8-514">Step 16: Reconfigure Always On</span></span>
<span data-ttu-id="074c8-515">Ezen a ponton várja meg a másodlagos teljesen szinkronizálja újra a helyi csomóponthoz és szinkron replikáció csomópont váltani, és lehetővé teszi a AFP áttelepített csomóponton.</span><span class="sxs-lookup"><span data-stu-id="074c8-515">At this point you wait for the secondary that node that was migrated to fully resynchronize with the on-premises node and switch to synchronous replication node and make it the AFP.</span></span>  

#### <a name="step-17-migrate-second-node"></a><span data-ttu-id="074c8-516">17. lépés: A második csomópont áttelepítése</span><span class="sxs-lookup"><span data-stu-id="074c8-516">Step 17: Migrate second node</span></span>
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="074c8-517">18. lépés: Lemez gyorsítótárazási beállítások a CSV-fájlban, és mentse</span><span class="sxs-lookup"><span data-stu-id="074c8-517">Step 18: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="074c8-518">A adatkötetek ezeket kell megadni csak OLVASHATÓ.</span><span class="sxs-lookup"><span data-stu-id="074c8-518">For data volumes these should be set to READONLY.</span></span>

<span data-ttu-id="074c8-519">TLOG kötetek ezeket érdemes lehet nincs értékűre állítani.</span><span class="sxs-lookup"><span data-stu-id="074c8-519">For TLOG volumes these should be set to NONE.</span></span>

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a><span data-ttu-id="074c8-521">19. lépés: A másodlagos csomópont új független Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="074c8-521">Step 19: Create New Independent Storage Account for Secondary Node</span></span>
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a><span data-ttu-id="074c8-522">20. lépés: Másolat VHD-k</span><span class="sxs-lookup"><span data-stu-id="074c8-522">Step 20: Copy VHDS</span></span>
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


<span data-ttu-id="074c8-523">A VHD-másolat állapota ellenőrizze, hogy minden virtuális merevlemez: ForEach (a $diskobjects $disk) {$lun = $disk. LUN $vhdname $disk.vhdname $cacheoption = = $disk. HostCaching $disklabel = $disk. Lemezcímke $diskName = $disk. DiskName</span><span class="sxs-lookup"><span data-stu-id="074c8-523">You can check the VHD copy status for all VHDs: ForEach ($disk in $diskobjects) { $lun = $disk.Lun $vhdname = $disk.vhdname $cacheoption = $disk.HostCaching $disklabel = $disk.DiskLabel $diskName = $disk.DiskName</span></span>

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

<span data-ttu-id="074c8-525">Várjon, amíg a fenti sikeres fájl rögzíti.</span><span class="sxs-lookup"><span data-stu-id="074c8-525">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="074c8-526">Az egyes blobok információt:</span><span class="sxs-lookup"><span data-stu-id="074c8-526">For information for individual blobs:</span></span>

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a><span data-ttu-id="074c8-527">21. lépés: az operációs rendszer Register lemez</span><span class="sxs-lookup"><span data-stu-id="074c8-527">Step 21: Register OS disk</span></span>
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="074c8-528">22. lépés: Adja hozzá a terhelés eloszlik a végpontok és hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="074c8-528">Step 22: Add Load Balanced Endpoints and ACLs</span></span>
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a><span data-ttu-id="074c8-529">23. lépés: Feladatátvételi teszt</span><span class="sxs-lookup"><span data-stu-id="074c8-529">Step 23: Test failover</span></span>
<span data-ttu-id="074c8-530">Ha most hagyja, az áttelepített csomópont szinkronizálja a helyszíni mindig a csomóponttal, elhelyezéséhez szinkron replikáció módra, és várjon, amíg azt szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="074c8-530">You should now let the migrated node synchronize with the on-premises Always On node, place it in to synchronous replication mode and wait until it is synchronized.</span></span> <span data-ttu-id="074c8-531">Ezután az első csomópontot a helyszínen a feladatátvétel áttelepíti, vagyis a AFP.</span><span class="sxs-lookup"><span data-stu-id="074c8-531">Then failover from on-prem to the first node migrated, which is the AFP.</span></span> <span data-ttu-id="074c8-532">Után, amely működött, módosítsa a AFP áttelepített utolsó csomópontja.</span><span class="sxs-lookup"><span data-stu-id="074c8-532">Once that has worked, change the last migrated node to the AFP.</span></span>

<span data-ttu-id="074c8-533">Feladatátvételi tesztek összes csomópontok között kell, és bár chaos tesztek annak biztosítása érdekében feladatátvételek work várt, egy időben manor futnak.</span><span class="sxs-lookup"><span data-stu-id="074c8-533">You should test failovers between all nodes and run though chaos tests to ensure failovers work as expected and in a timely manor.</span></span>

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a><span data-ttu-id="074c8-534">24. lépés: Helyezze vissza fürt kvórumbeállításainak megadása / DNS-élettartam / feladatátvételi Pntrs / szinkronizálási beállítások</span><span class="sxs-lookup"><span data-stu-id="074c8-534">Step 24: Put back cluster quorum settings / DNS TTL / Failover Pntrs / Sync Settings</span></span>
##### <a name="adding-ip-address-resource-on-same-subnet"></a><span data-ttu-id="074c8-535">Azonos alhálózatban lévő IP-cím erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="074c8-535">Adding IP Address Resource on Same Subnet</span></span>
<span data-ttu-id="074c8-536">Ha csak 2 SQL-kiszolgálóval rendelkezik, és szeretne áttérni őket az új felhőalapú szolgáltatás, de szeretné, hogy továbbra is ugyanazon az alhálózaton, elkerülheti a figyelő offline véve törli az eredeti mindig az IP-címet, és adja hozzá az új IP-cím.</span><span class="sxs-lookup"><span data-stu-id="074c8-536">If you have only 2 SQL Servers and want to migrate them to a new cloud service, but want to keep them on the same subnet, you can avoid taking the listener offline to delete the original Always On IP Address and add the New IP Address.</span></span> <span data-ttu-id="074c8-537">Ha a virtuális gépeket telepít át egy másik alhálózat nem kell ehhez, mert egy további fürthálózat, alhálózaton hivatkozhat lesz.</span><span class="sxs-lookup"><span data-stu-id="074c8-537">If you are migrating the VMs to another subnet you will not need to do this as there will be an additional cluster network that will reference that subnet.</span></span>

<span data-ttu-id="074c8-538">Miután a áttelepített másodlagos felvet és hozzáadni az új IP-cím erőforrás a meglévő elsődleges feladatátvétel előtt az új felhőalapú szolgáltatás, ezeket a lépéseket belül a Feladatátvevőfürt-kezelőt kell tennie:</span><span class="sxs-lookup"><span data-stu-id="074c8-538">Once you have brought up the migrated secondary and added in the new IP Address resource for the new cloud service before failover the existing Primary, you should take these steps within the Cluster Failover Manager:</span></span>

<span data-ttu-id="074c8-539">Adja hozzá az IP-cím, tekintse meg a [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), 14 lépésben.</span><span class="sxs-lookup"><span data-stu-id="074c8-539">To add in IP Address, see the [Appendix](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), step 14.</span></span>

1. <span data-ttu-id="074c8-540">Az aktuális IP-cím erőforrás az alábbi példában "dansqlams4" a "Meglévő elsődleges SQL Server", a lehetséges tulajdonos módosítása:</span><span class="sxs-lookup"><span data-stu-id="074c8-540">For the current IP Address resource, change the possible owner to ‘Existing Primary SQL Server’, in the example below, ‘dansqlams4’:</span></span>

    ![Appendix13][23]
2. <span data-ttu-id="074c8-542">Az új IP-cím erőforrás "Áttelepítve másodlagos SQL-kiszolgáló", az alábbi példában "dansqlams5" lehetséges tulajdonos módosítása:</span><span class="sxs-lookup"><span data-stu-id="074c8-542">For the new IP Address resource, change the possible owner to ‘Migrated secondary SQL Server’, in the example below, ‘dansqlams5’:</span></span>

    ![Appendix14][24]
3. <span data-ttu-id="074c8-544">Miután ezt állítja be a következőket teheti feladatátvevő, és az utolsó csomópont áttelepítésekor a lehetséges tulajdonosok módosítani kell úgy, hogy a csomópont lehetséges tulajdonosa meg van adva:</span><span class="sxs-lookup"><span data-stu-id="074c8-544">Once this is set you can failover, and when the last node is migrated the Possible Owners should be edited so that node is added as a Possible Owner:</span></span>

    ![Appendix15][25]

## <a name="additional-resources"></a><span data-ttu-id="074c8-546">További források</span><span class="sxs-lookup"><span data-stu-id="074c8-546">Additional resources</span></span>
* [<span data-ttu-id="074c8-547">Prémium szintű Azure Storage</span><span class="sxs-lookup"><span data-stu-id="074c8-547">Azure Premium Storage</span></span>](../../../storage/common/storage-premium-storage.md)
* [<span data-ttu-id="074c8-548">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="074c8-548">Virtual Machines</span></span>](https://azure.microsoft.com/services/virtual-machines/)
* [<span data-ttu-id="074c8-549">SQL Server Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="074c8-549">SQL Server in Azure Virtual Machines</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
