---
title: "prémium szintű Azure Storage az SQL Server aaaUse |} Microsoft Docs"
description: "Ez a cikk hello klasszikus telepítési modellel létrehozott erőforrást használ, és a prémium szintű Azure Storage használata az Azure virtuális gépeken futó SQL Server útmutatást biztosít."
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
ms.openlocfilehash: 393ea2020b39ea686302ae632e1049935c24af00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a><span data-ttu-id="6ce9c-103">Az Azure Premium Storage és az SQL Server együttes használata virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="6ce9c-103">Use Azure Premium Storage with SQL Server on Virtual Machines</span></span>
## <a name="overview"></a><span data-ttu-id="6ce9c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6ce9c-104">Overview</span></span>
<span data-ttu-id="6ce9c-105">[Prémium szintű Storage](../../../storage/common/storage-premium-storage.md) van hello tárhelyet biztosít alacsony késéssel és magas teljesítmény IO következő generációja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-105">[Azure Premium Storage](../../../storage/common/storage-premium-storage.md) is hello next generation of storage that provides low latency and high throughput IO.</span></span> <span data-ttu-id="6ce9c-106">A kulcs IO igényes munkaterhelések, például az SQL Server IaaS a legjobban [virtuális gépek](https://azure.microsoft.com/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-106">It works best for key IO intensive workloads, such as SQL Server on IaaS [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ce9c-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6ce9c-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="6ce9c-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="6ce9c-110">Ez a cikk ismerteti a tervezési és az SQL Server toouse prémium szintű Storage futtató virtuális gép áttelepítését.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-110">This article provides planning and guidance for migrating a Virtual Machine running SQL Server toouse Premium Storage.</span></span> <span data-ttu-id="6ce9c-111">Ez magában foglalja az Azure-infrastruktúra (hálózati, tárolási) és a vendég Windows virtuális gép lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-111">This includes Azure infrastructure (networking, storage) and guest Windows VM steps.</span></span> <span data-ttu-id="6ce9c-112">hello hello példát [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) bemutatja, hogyan toomove nagyobb virtuális gépek tootake előnyeit továbbfejlesztett teljes átfogó end tooend áttelepítését helyi SSD-tárolóba, a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-112">hello example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) shows a full comprehensive end tooend migration of how toomove larger VMs tootake advantage of improved local SSD storage with PowerShell.</span></span>

<span data-ttu-id="6ce9c-113">Fontos toounderstand hello végpont folyamata terjesztése prémium szintű Azure Storage az infrastruktúra-szolgáltatási virtuális gépeken futó SQL Server is.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-113">It is important toounderstand hello end-to-end process of utilizing Azure Premium Storage with SQL Server on IAAS VMs.</span></span> <span data-ttu-id="6ce9c-114">Ehhez a következőket:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-114">This includes:</span></span>

* <span data-ttu-id="6ce9c-115">Hello Előfeltételek toouse prémium szintű Storage azonosítása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-115">Identification of hello prerequisites toouse Premium Storage.</span></span>
* <span data-ttu-id="6ce9c-116">SQL Server telepítése az infrastruktúra-szolgáltatási tooPremium tárhely az új központi telepítéseknél példát.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-116">Examples of deploying SQL Server on IaaS tooPremium Storage for new deployments.</span></span>
* <span data-ttu-id="6ce9c-117">Önálló kiszolgálók és a üzembe helyezése SQL Always On rendelkezésre állási csoportok áttelepítése meglévő telepítés példát.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-117">Examples of migrating existing deployments, both stand-alone servers and deployments using SQL Always On Availability Groups.</span></span>
* <span data-ttu-id="6ce9c-118">Lehetséges áttelepítési módszer.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-118">Possible migration approaches.</span></span>
* <span data-ttu-id="6ce9c-119">Teljes-végpont például megjelenítő hello áttelepítés végrehajtásának egy meglévő Always On Azure, a Windows és az SQL Server lépései.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-119">Full end-to-end example showing Azure, Windows, and SQL Server steps for hello migration of an existing Always On implementation.</span></span>

<span data-ttu-id="6ce9c-120">Tekintse meg az SQL Server Azure virtuális gépek további háttérinformációkat [SQL Server Azure virtuális gépek](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-120">For more background information on SQL Server in Azure Virtual Machines, see [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

<span data-ttu-id="6ce9c-121">**Szerző:** Daniel Sol **műszaki véleményezők:** Luis Carlos Vargas hering, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-121">**Author:** Daniel Sol **Technical Reviewers:** Luis Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.</span></span>

## <a name="prerequisites-for-premium-storage"></a><span data-ttu-id="6ce9c-122">Prémium szintű Storage előfeltételei</span><span class="sxs-lookup"><span data-stu-id="6ce9c-122">Prerequisites for Premium Storage</span></span>
<span data-ttu-id="6ce9c-123">Prémium szintű Storage használatával több előfeltételei van.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-123">There are several prerequisites for using Premium Storage.</span></span>

### <a name="machine-size"></a><span data-ttu-id="6ce9c-124">Mérete</span><span class="sxs-lookup"><span data-stu-id="6ce9c-124">Machine size</span></span>
<span data-ttu-id="6ce9c-125">Prémium szintű Storage használatához szüksége lesz a toouse DS adatsorozat virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-125">For using Premium Storage you will need toouse DS series Virtual Machines (VM).</span></span> <span data-ttu-id="6ce9c-126">Ha DS adatsorozat gépek nem használta a felhőszolgáltatásban előtt, törölje a meglévő virtuális gép hello, hello csatlakoztatott lemezek tároljuk, és majd új felhőalapú szolgáltatás létrehozása előtt újra létrehozni a virtuális gép méreteként DS * szerepkör hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-126">If you have not used DS Series machines in your cloud service before, you must delete hello existing VM, keep hello attached disks, and then create a new cloud service before recreating hello VM as DS* role size.</span></span> <span data-ttu-id="6ce9c-127">További információ a virtuálisgép-méretek: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-127">For more information on Virtual Machine sizes, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="cloud-services"></a><span data-ttu-id="6ce9c-128">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="6ce9c-128">Cloud services</span></span>
<span data-ttu-id="6ce9c-129">Csak használata virtuális gépek DS * prémium szintű Storage az új felhőalapú szolgáltatás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-129">You can only use DS* VMs with Premium Storage when they are created in a new cloud service.</span></span> <span data-ttu-id="6ce9c-130">Ha az SQL Server Always On használ az Azure-ban, mindig figyelő hello toohello Azure belső vagy a külső terheléselosztási terheléselosztó IP-cím egy felhőalapú szolgáltatás társított vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-130">If you are using SQL Server Always On in Azure, hello Always On Listener will refer toohello Azure Internal or External Load Balancer IP address that is associated with a cloud service.</span></span> <span data-ttu-id="6ce9c-131">Ez a cikk foglalkozik, hogyan ebben a forgatókönyvben rendelkezésre állásának toomigrate.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-131">This article focuses on how toomigrate while maintaining availability in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="6ce9c-132">Első virtuális gép, amely telepített toohello DS * több kell hello új felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-132">A DS* Series must be hello first VM that is deployed toohello new Cloud Service.</span></span>
>
>

### <a name="regional-vnets"></a><span data-ttu-id="6ce9c-133">Regionális VNETEK</span><span class="sxs-lookup"><span data-stu-id="6ce9c-133">Regional VNETS</span></span>
<span data-ttu-id="6ce9c-134">A DS * virtuális gépek hello virtuális hálózatot (VNET) a regionális virtuális gépek toobe üzemeltető kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-134">For DS* VMs you must configure hello Virtual Network (VNET) hosting your VMs toobe regional.</span></span> <span data-ttu-id="6ce9c-135">A "szélesebb formája" hello VNET tooallow hello nagyobb virtuális gépek toobe más fürtök kiépítése és azok közötti kommunikáció lehetővé tételéhez.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-135">This “widens” hello VNET is tooallow hello larger VMs toobe provisioned in other clusters and allow communication between them.</span></span> <span data-ttu-id="6ce9c-136">A következő képernyőkép hello hello kiemelt helyen mutatja regionális Vnetek, mivel hello első eredmény azt mutatja, a "keskeny" VNET.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-136">In hello following screenshot, hello highlighted Location shows regional VNETs, whereas hello first result shows a “narrow” VNET.</span></span>

![RegionalVNET][1]

<span data-ttu-id="6ce9c-138">A Microsoft támogatási jegy toomigrate tooa is növelheti regionális virtuális hálózat, a Microsoft fog olyan módosítást, majd toocomplete hello áttelepítési tooregional Vnetek, módosítsa a hello tulajdonság AffinityGroup hello a hálózati konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-138">You can raise a Microsoft support ticket toomigrate tooa regional VNET, Microsoft will make a change, then toocomplete hello migration tooregional VNETs, change hello property AffinityGroup in hello network configuration.</span></span> <span data-ttu-id="6ce9c-139">Először exportálnia hello PowerShell a hálózati konfigurációt, és lecseréli a hello **AffinityGroup** hello tulajdonság **VirtualNetworkSite** elem egy **hely** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-139">First export hello Network Configuration in PowerShell, and then replace hello **AffinityGroup** property in hello **VirtualNetworkSite** element with a **Location** property.</span></span> <span data-ttu-id="6ce9c-140">Adja meg `Location = XXXX` ahol `XXXX` egy Azure-régióban van.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-140">Specify `Location = XXXX` where `XXXX` is an Azure region.</span></span> <span data-ttu-id="6ce9c-141">Importálja az új konfiguráció hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-141">Then import hello new configuration.</span></span>

<span data-ttu-id="6ce9c-142">Például figyelembe véve a VNET konfigurációját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-142">For example, considering hello following VNET configuration:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

<span data-ttu-id="6ce9c-143">toomove a tooa Nyugat-Európa, a regionális virtuális hálózat módosítása hello konfigurációs toohello a következő:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-143">toomove this tooa regional VNET in West Europe, change hello configuration toohello following:</span></span>

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a><span data-ttu-id="6ce9c-144">Tárfiókok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-144">Storage accounts</span></span>
<span data-ttu-id="6ce9c-145">Szüksége lesz egy új tárfiókot, amelyet a prémium szintű Storage beállított toocreate.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-145">You will need toocreate a new storage account that is configured for Premium Storage.</span></span> <span data-ttu-id="6ce9c-146">Figyelje meg, hogy hello storage-fiók nem egyedi virtuális merevlemezek, a prémium szintű Storage hello használata van beállítva azonban a DS * adatsorozat virtuális gépek használatakor csatolhat a VHD-k a prémium és standard szintű tárolást fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-146">Note that hello use of Premium Storage is set at hello storage account, not on individual VHDs, however when using a DS* Series VM you can attach VHD’s from Premium and Standard Storage accounts.</span></span> <span data-ttu-id="6ce9c-147">Ha nem szeretné, hogy tooplace hello az operációs rendszer virtuális Merevlemezt a prémium szintű Storage-fiók toohello ez foglalkozhat.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-147">You may consider this if you do not want tooplace hello OS VHD on toohello Premium Storage account.</span></span>

<span data-ttu-id="6ce9c-148">hello következő **New-AzureStorageAccountPowerShell** hello "Premium_LRS" parancsot **típus** hoz létre a prémium szintű Storage-fiókok:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-148">hello following **New-AzureStorageAccountPowerShell** command with hello "Premium_LRS" **Type** creates a Premium Storage Account:</span></span>

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a><span data-ttu-id="6ce9c-149">Virtuális merevlemezek gyorsítótár beállításai</span><span class="sxs-lookup"><span data-stu-id="6ce9c-149">VHDs Cache Settings</span></span>
<span data-ttu-id="6ce9c-150">hello fő különbség a prémium szintű Storage-fiókok részét képező lemezek létrehozása, hello lemezgyorsítótár-beállítás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-150">hello main difference between creating disks that are part of a Premium Storage account is hello disk cache setting.</span></span> <span data-ttu-id="6ce9c-151">Használata javasolt az SQL Server adatmennyiség lemezek azt "**olvasási gyorsítótárazás**".</span><span class="sxs-lookup"><span data-stu-id="6ce9c-151">For SQL Server Data volume disks it is recommended that you use ‘**Read Caching**’.</span></span> <span data-ttu-id="6ce9c-152">A tranzakció naplózási kötetek, hello lemezgyorsítótár-beállítás beállításaként túl "**nincs**".</span><span class="sxs-lookup"><span data-stu-id="6ce9c-152">For Transaction log volumes, hello disk cache setting should be set too‘**None**’.</span></span> <span data-ttu-id="6ce9c-153">Ez eltér a szabványos tárfiókok hello javaslatok.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-153">This is different from hello recommendations for Standard Storage accounts.</span></span>

<span data-ttu-id="6ce9c-154">Hello VHD-k csatolást követően hello lemezgyorsítótár-beállítás nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-154">Once hello VHDs have been attached, hello cache setting cannot be altered.</span></span> <span data-ttu-id="6ce9c-155">Ehhez szükséges toodetach, és csatlakoztassa újra a virtuális merevlemez hello frissített gyorsítótár-beállítással.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-155">You would need toodetach and reattach hello VHD with an updated cache setting.</span></span>

### <a name="windows-storage-spaces"></a><span data-ttu-id="6ce9c-156">Windows tárolóhelyek</span><span class="sxs-lookup"><span data-stu-id="6ce9c-156">Windows storage spaces</span></span>
<span data-ttu-id="6ce9c-157">Használhat [Windows tárolóhelyek](https://technet.microsoft.com/library/hh831739.aspx) úgy, ahogy az előző standard szintű Storage, ez lehetővé teszi egy virtuális Gépet, amely már van okhoz tárolóhelyek toomigrate.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-157">You can use [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) as you did with previous Standard Storage, this will allow you toomigrate a VM that is already utilizing Storage Spaces.</span></span> <span data-ttu-id="6ce9c-158">hello példát [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (9-es és az utána lépés) azt mutatja be, hello Powershell kód tooextract, és importálja a virtuális gép több csatlakoztatott virtuális merevlemezek és.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-158">hello example in [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (step 9 and forward) demonstrates hello Powershell code tooextract and import a VM with multiple attached VHDs.</span></span>

<span data-ttu-id="6ce9c-159">Tárolókészletek szabványos Azure tárolási fiók tooenhance átviteli alkalmazott, és a késés csökkentésére.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-159">Storage Pools were used with Standard Azure storage account tooenhance throughput and reduce latency.</span></span> <span data-ttu-id="6ce9c-160">Bizonyára hasznosnak találja érték a prémium szintű Storage Tárolókészletek tesztelése az új központi telepítéseknél, de nagyobb fokú összetettségével jár tárolási telepítés adnak hozzá.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-160">You might find value in testing Storage Pools with Premium Storage for new deployments, but they do add additional complexity with storage setup.</span></span>

#### <a name="how-toofind-which-azure-virtual-disks-map-toostorage-pools"></a><span data-ttu-id="6ce9c-161">Hogyan toofind mely Azure virtuális lemezek toostorage készletek leképezése</span><span class="sxs-lookup"><span data-stu-id="6ce9c-161">How toofind which Azure Virtual Disks map toostorage pools</span></span>
<span data-ttu-id="6ce9c-162">Mivel másik gyorsítótármappa beállítás javaslatok csatolt VHD-k, dönthet úgy, hogy toocopy hello VHD-k tooa prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-162">As there are different cache setting recommendations for attached VHDs, you might decide toocopy hello VHDs tooa Premium Storage account.</span></span> <span data-ttu-id="6ce9c-163">Azonban amikor Ön áttelepítést csatlakoztassa őket újra toohello új DS adatsorozat VM, szükség lehet a tooalter hello gyorsítótár beállításait is.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-163">However, when you reattach them toohello new DS series VM, you might need tooalter hello cache settings.</span></span> <span data-ttu-id="6ce9c-164">Prémium szintű Storage ajánlott gyorsítótár beállításai, ha külön virtuális merevlemezek hello SQL adatok fájlok napló fájlok (helyett és egy virtuális Merevlemezt, amely egyaránt tartalmaz) egyszerűbb tooapply hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-164">It is simpler tooapply hello Premium Storage recommended cache settings when you have separate VHDs for hello SQL Data files and log files (rather than a single VHD that contains both).</span></span>

> [!NOTE]
> <span data-ttu-id="6ce9c-165">Ha SQL Server adatainak és naplókönyvtárainak fájlok hello ugyanazon a köteten, gyorsítótárazási beállítását választja hello hello IO hozzáférési minták az adatbázis-terhelések függ.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-165">If you have SQL Server data and log files on hello same volume, hello caching option you choose depends on hello IO access patterns for your database workloads.</span></span> <span data-ttu-id="6ce9c-166">Csak tesztelési is bemutatják, milyen gyorsítótárazás esetén ajánlott ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-166">Only testing can demonstrate which caching option is best for this scenario.</span></span>
>
>

<span data-ttu-id="6ce9c-167">Azonban használata Windows tárolóhelyek, amelyek össze több VHD-k toolook, szüksége lesz az eredeti parancsfájlok tooidentify, amelyek csatlakoztatott virtuális merevlemezek milyen adott készletben, majd beállíthatja hello gyorsítótár beállításainak ennek megfelelően az egyes lemezek.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-167">However, if you are using Windows Storage Spaces which are made up of multiple VHDs you will need toolook at your original scripts tooidentify which attached VHDs are in what specific pool, so you can then set hello cache settings accordingly for each disk.</span></span>

<span data-ttu-id="6ce9c-168">Ha nem rendelkezik eredeti parancsfájl elérhető tooshow, amely a VHD-k leképezése toohello tárolókészlethez, használhatja a következő lépéseket toodetermine hello lemezegységet/készlet leképezési hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-168">If you do not have original script available tooshow you which VHDs map toohello storage pool, you can use hello following steps toodetermine hello disk/storage pool mapping.</span></span>

<span data-ttu-id="6ce9c-169">Az egyes lemezek lépések hello használata:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-169">For each disk, use hello following steps:</span></span>

1. <span data-ttu-id="6ce9c-170">Lemezek listájának beszerzése a hello tooVM csatolt **Get-AzureVM** parancs:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-170">Get list of disks attached tooVM with hello **Get-AzureVM** command:</span></span>

    <span data-ttu-id="6ce9c-171">Get-AzureVM - ServiceName <servicename> -név <vmname> |} Get-AzureDataDisk</span><span class="sxs-lookup"><span data-stu-id="6ce9c-171">Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk</span></span>
2. <span data-ttu-id="6ce9c-172">Megjegyzés: hello Diskname és a logikai Egységet.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-172">Note hello Diskname and LUN.</span></span>

    ![DisknameAndLUN][2]
3. <span data-ttu-id="6ce9c-174">Távoli asztali kapcsolatot hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-174">Remote desktop into hello VM.</span></span> <span data-ttu-id="6ce9c-175">Keresse meg a túl**számítógép-kezelés** | **Eszközkezelő** | **lemezmeghajtók**.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-175">Then go too**Computer Management** | **Device Manager** | **Disk Drives**.</span></span> <span data-ttu-id="6ce9c-176">Nézze meg az egyes hello "Microsoft virtuális lemezek" hello tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="6ce9c-176">Look at hello properties of each of hello ‘Microsoft Virtual Disks’</span></span>

    ![VirtualDiskProperties][3]
4. <span data-ttu-id="6ce9c-178">Itt hello LUN számot a hivatkozási toohello LUN számot adja meg a hello VHD toohello virtuális gép csatlakoztatása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-178">hello LUN number here is a reference toohello LUN number you specify when attaching hello VHD toohello VM.</span></span>
5. <span data-ttu-id="6ce9c-179">A "Microsoft virtuális lemez" hello go toohello **részletek** fülre, majd a hello **tulajdonság** listában, nyissa meg túl**illesztőprogram kulcs**.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-179">For hello ‘Microsoft Virtual Disk’ go toohello **Details** tab, then in hello **Property** list, go too**Driver Key**.</span></span> <span data-ttu-id="6ce9c-180">A hello **érték**, Megjegyzés hello **eltolás**, vagyis a következő képernyőkép hello 0002.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-180">In hello **Value**, note hello **Offset**, which is 0002 in hello following screenshot.</span></span> <span data-ttu-id="6ce9c-181">hello 0002 tárolási készlet hivatkozások hello hello Fizikailemez2 jelöli.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-181">hello 0002 denotes hello PhysicalDisk2 that hello storage pool references.</span></span>

    ![VirtualDiskPropertyDetails][4]
6. <span data-ttu-id="6ce9c-183">Minden egyes tárolókészlethez kimenő hello memóriakép kapcsolódó lemezek:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-183">For each storage pool, dump out hello associated disks:</span></span>

    <span data-ttu-id="6ce9c-184">Get-StoragePool - FriendlyName AMS1pooldata |} Get-PhysicalDisk</span><span class="sxs-lookup"><span data-stu-id="6ce9c-184">Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk</span></span>

    ![GetStoragePool][5]

<span data-ttu-id="6ce9c-186">Most már használhat ezen információk tooassociate csatolt VHD-k tooPhysical lemezek tárolókészletek.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-186">Now you can use this information tooassociate attached VHDs tooPhysical Disks in Storage Pools.</span></span>

<span data-ttu-id="6ce9c-187">Miután leképezte a VHD-k tooPhysical lemezek tárolókészletek válassza le, majd másolja őket keresztül tooa prémium szintű Storage-fiókot, majd csatolja a hello beállítása helyes gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-187">Once you have mapped VHDs tooPhysical Disks in Storage Pools you can then detach and copy them over tooa Premium Storage account, then attach them with hello correct cache setting.</span></span> <span data-ttu-id="6ce9c-188">Tekintse meg a hello hello példát [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), 8 – 12 lépést.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-188">Please see hello example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steps 8 through 12.</span></span> <span data-ttu-id="6ce9c-189">Ezeket a lépéseket hogyan tooextract virtuális merevlemez virtuális gép csatlakoztatott lemez konfigurációs tooa CSV-fájl, másolja a VHD-k hello, hello lemez konfigurációs gyorsítótár beállításainak módosításához, és végül telepítse újra hello VM, az összes hello DS több virtuális gép csatlakoztatott lemezekkel megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-189">These steps show how tooextract a VM-attached VHD disk configuration tooa CSV file, copy hello VHDs, alter hello disk configuration cache settings, and finally redeploy hello VM as a DS series VM with all hello attached disks.</span></span>

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a><span data-ttu-id="6ce9c-190">Virtuális gép tárolási sávszélesség és a virtuális merevlemez tárolási teljesítmény</span><span class="sxs-lookup"><span data-stu-id="6ce9c-190">VM storage bandwidth and VHD storage throughput</span></span>
<span data-ttu-id="6ce9c-191">hello tárolási teljesítményének mértékét hello DS * Virtuálisgép-méretet megadott és hello virtuális merevlemez méretét.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-191">hello amount of storage performance depends on hello DS* VM size specified and hello VHD sizes.</span></span> <span data-ttu-id="6ce9c-192">hello virtuális gépek különböző támogatás hello számát, amely lehet csatolni, és maximális sávszélesség (MB/s) fog támogatják hello VHD-k számára rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-192">hello VMs have different allowances for hello number of VHDs that can be attached and hello maximum bandwidth they will support (MB/s).</span></span> <span data-ttu-id="6ce9c-193">Hello meghatározott sávszélesség számok, lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-193">For hello specific bandwidth numbers, see [Virtual Machine and Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="6ce9c-194">Nagyobb IOPS mérete nagyobb a érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-194">Increased IOPS are achieved with larger disk sizes.</span></span> <span data-ttu-id="6ce9c-195">Ez akkor érdemes megfontolni, amikor az áttelepítési útvonalának.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-195">You should consider this when you think about your migration path.</span></span> <span data-ttu-id="6ce9c-196">További információkért [hello táblázatban találja az iops-érték és lemeztípusok](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-196">For details, [see hello table for IOPS and Disk Types](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).</span></span>

<span data-ttu-id="6ce9c-197">Mindemellett érdemes lehet megfontolnia, virtuális gépek támogatják az összes csatolt lemezek különböző maximális lemez sávszélesség rendelkezik-e.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-197">Finally, consider that VMs have different maximum disk bandwidths they will support for all disks attached.</span></span> <span data-ttu-id="6ce9c-198">Magas terhelés alatt hello maximális sávszélesség érhető el a Virtuálisgép-szerepkör méretéhez sikerült telítsük.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-198">Under high load, you could saturate hello maximum disk bandwidth available for that VM role size.</span></span> <span data-ttu-id="6ce9c-199">Például egy Standard_DS14 támogatja mentése too512MB/s. három P30 lemezzel ezért hello lemez sávszélesség hello VM telítsük sikerült.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-199">For example a Standard_DS14 will support up too512MB/s; therefore, with three P30 disks you could saturate hello disk bandwidth of hello VM.</span></span> <span data-ttu-id="6ce9c-200">De ebben a példában hello átviteli korlátja sikerült túllépve, attól függően, hogy olvasási és írási IOs hello kombinációját.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-200">But in this example, hello throughput limit could be exceeded depending on hello mix of read and write IOs.</span></span>

## <a name="new-deployments"></a><span data-ttu-id="6ce9c-201">Új központi telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="6ce9c-201">New deployments</span></span>
<span data-ttu-id="6ce9c-202">hello következő két szakaszok bemutatják, hogyan telepítheti az SQL Server VMs tooPremium tároló.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-202">hello next two sections demonstrate how you can deploy SQL Server VMs tooPremium Storage.</span></span> <span data-ttu-id="6ce9c-203">Ahogy korábban említettük, nem feltétlenül kell tooplace hello operációsrendszer-lemez prémium szintű storage-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-203">As mentioned before, you do not necessarily need tooplace hello OS disk onto Premium storage.</span></span> <span data-ttu-id="6ce9c-204">Előfordulhat, hogy toodo ezt választja, ha meg vannak szándékos volt tooplace bármely intenzív IO-munkaterhelések az operációs rendszer virtuális merevlemez hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-204">You might choose toodo this if you are intending tooplace any intensive IO workloads on hello OS VHD.</span></span>

<span data-ttu-id="6ce9c-205">hello első példa bemutatja, meglévő Azure-gyűjtemény lemezképei használatával.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-205">hello first example demonstrates utilizing existing Azure Gallery Images.</span></span> <span data-ttu-id="6ce9c-206">hello második példa bemutatja, hogyan toouse egyéni VM lemezkép, amelyet a meglévő standard szintű tárfiók van.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-206">hello second example shows how toouse a custom VM image that you have in an existing Standard storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="6ce9c-207">Ezek a példák feltételezik, hogy már létrehozta a regionális virtuális Hálózatot.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-207">These examples assume that you have already created a Regional VNET.</span></span>
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a><span data-ttu-id="6ce9c-208">Prémium szintű Storage gyűjtemény lemezképpel új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-208">Create a new VM with Premium Storage with Gallery Image</span></span>
<span data-ttu-id="6ce9c-209">hello az alábbi példa bemutatja, hogyan tooplace az operációs rendszer virtuális merevlemez hello alakzatot prémium szintű storage-e, és csatolja a prémium szintű Storage VHD-k.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-209">hello example below shows how tooplace hello OS VHD onto premium storage and attach Premium Storage VHDs.</span></span> <span data-ttu-id="6ce9c-210">Azonban is hello operációsrendszer-lemezzel helyez egy standard szintű tárfiókot, és majd csatolja a VHD-k, amelyek tárolása a prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-210">However, you can also place hello OS disk in a Standard Storage account and then attach VHDs that reside in a Premium Storage account.</span></span> <span data-ttu-id="6ce9c-211">Mindkét forgatókönyvet egy.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-211">Both scenarios are demonstrated.</span></span>

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a><span data-ttu-id="6ce9c-212">1. lépés: A prémium szintű Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-212">Step 1: Create a Premium Storage Account</span></span>
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a><span data-ttu-id="6ce9c-213">2. lépés: Új felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-213">Step 2: Create a New Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a><span data-ttu-id="6ce9c-214">3. lépés: Lefoglalni egy felhőalapú szolgáltatás VIP (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="6ce9c-214">Step 3: Reserve a Cloud Service VIP (Optional)</span></span>
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a><span data-ttu-id="6ce9c-215">4. lépés:, Hozzon létre egy Virtuálisgép-tároló</span><span class="sxs-lookup"><span data-stu-id="6ce9c-215">Step 4: Create a VM Container</span></span>
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a><span data-ttu-id="6ce9c-216">5. lépés: az operációs rendszer virtuális Merevlemezt a Standard vagy prémium szintű Storage helyezi el.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-216">Step 5: Placing OS VHD on Standard or Premium Storage</span></span>
    #NOTE: Set up subscription and default storage account which will be used tooplace hello OS VHD in

    #If you want tooplace hello OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted tooplace hello OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a><span data-ttu-id="6ce9c-217">6. lépés: Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-217">Step 6: Create VM</span></span>
    #Get list of available SQL Server Images from hello Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember toochange tooDS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks tooVM Config
    #Note hello size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising hello Premium Storage enabled Storage account

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


### <a name="create-a-new-vm-toouse-premium-storage-with-a-custom-image"></a><span data-ttu-id="6ce9c-218">Hozzon létre egy új virtuális gép toouse prémium szintű Storage egyéni kép</span><span class="sxs-lookup"><span data-stu-id="6ce9c-218">Create a new VM toouse Premium Storage with a custom image</span></span>
<span data-ttu-id="6ce9c-219">Ebben a forgatókönyvben azt mutatja be, melyekben egy standard szintű tárfiókot található meglévő testre szabott lemezképet.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-219">This scenario demonstrates where you have existing customized images that reside in a Standard Storage account.</span></span> <span data-ttu-id="6ce9c-220">Ahogy azt korábban említettük, ha azt szeretné, tooplace hello az operációs rendszer virtuális Merevlemezt a prémium szintű Storage toocopy kell hello lemezképet, Standard szintű tárfiók hello szerepel, és helyezze tooa prémium szintű Storage használat előtt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-220">As mentioned if you want tooplace hello OS VHD on Premium Storage you will need toocopy hello image that exists in hello Standard Storage account and transfer them tooa Premium Storage before it can be used.</span></span> <span data-ttu-id="6ce9c-221">Ha rendelkezik helyszíni kép, érdemes a metódus toocopy is használhatja, amely közvetlenül toohello prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-221">If you have an image on-premises, you can also use this method toocopy that directly toohello Premium Storage account.</span></span>

#### <a name="step-1-create-storage-account"></a><span data-ttu-id="6ce9c-222">1. lépés: Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-222">Step 1: Create Storage Account</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a><span data-ttu-id="6ce9c-223">2. lépés a felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-223">Step 2 Create Cloud Service</span></span>
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a><span data-ttu-id="6ce9c-224">3. lépés: A meglévő kép használata</span><span class="sxs-lookup"><span data-stu-id="6ce9c-224">Step 3: Use existing image</span></span>
<span data-ttu-id="6ce9c-225">Egy meglévő lemezképet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-225">You can use an existing image.</span></span> <span data-ttu-id="6ce9c-226">Is [igénybe vehet egy meglévő számítógép lemezképét](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-226">Or, you can [take an image of an existing machine](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="6ce9c-227">Megjegyzés: hello géphez rendszerképet készítene nincs toobe DS * gép.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-227">Note hello machine you image does not have toobe DS* machine.</span></span> <span data-ttu-id="6ce9c-228">Miután hello kép, hello módját a következő lépéseket megjelenítése toocopy, prémium szintű Storage-fiókkal és hello toohello **Start-AzureStorageBlobCopy** PowerShell-parancsmag segítségével.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-228">Once you have hello image, hello following steps show how toocopy it toohello Premium Storage account with hello **Start-AzureStorageBlobCopy** PowerShell commandlet.</span></span>

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for hello storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a><span data-ttu-id="6ce9c-229">4. lépés: Másolat Blob Storage-fiókok között</span><span class="sxs-lookup"><span data-stu-id="6ce9c-229">Step 4: Copy Blob between Storage Accounts</span></span>
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a><span data-ttu-id="6ce9c-230">5. lépés: Rendszeresen ellenőrizze a példány állapotát:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-230">Step 5: Regularly check copy status:</span></span>
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-tooazure-disk-repository-in-subscription"></a><span data-ttu-id="6ce9c-231">6. lépés: Kép tooAzure lemezről lemezre tárház hozzáadása az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="6ce9c-231">Step 6: Add Image disk tooAzure disk Repository in Subscription</span></span>
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> <span data-ttu-id="6ce9c-232">Előfordulhat, hogy annak ellenére, hogy hello állapotáról szóló jelentések sikeres, sikertelen továbbra is megjelenik bérleti lemezhiba.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-232">You may find that even though hello status reports as success, you could still get a disk lease error.</span></span> <span data-ttu-id="6ce9c-233">Ebben az esetben várjon körülbelül 10 percet.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-233">In this case, wait about 10 minutes.</span></span>
>
>

#### <a name="step-7--build-hello-vm"></a><span data-ttu-id="6ce9c-234">7. lépés: Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-234">Step 7:  Build hello VM</span></span>
<span data-ttu-id="6ce9c-235">Itt készítésekor hello VM a lemezkép és a VHD-k két Premium Storage:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-235">Here you are building hello VM from your image and attaching two Premium Storage VHDs:</span></span>

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need toobe a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use tooDS Series VM
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

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a><span data-ttu-id="6ce9c-236">Always On rendelkezésre állási csoportok nem használó meglévő központi telepítések</span><span class="sxs-lookup"><span data-stu-id="6ce9c-236">Existing deployments that do not use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="6ce9c-237">A meglévő telepítések esetében először lásd: hello [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-237">For existing deployments, first see hello [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="6ce9c-238">Nincsenek az Always On rendelkezésre állási csoportok és azok, amelyek nem használó SQL Server-telepítések kapcsolatos szempontokat.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-238">There are different considerations for SQL Server deployments that do not use Always On Availability Groups and those that do.</span></span> <span data-ttu-id="6ce9c-239">Ha nem használják a mindig bekapcsolva, és rendelkezik egy meglévő önálló SQL Server, frissítheti tooPremium tárolási új felhőalapú szolgáltatás és a tárolási fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-239">If you are not using Always On and have an existing standalone SQL Server, you can upgrade tooPremium Storage by using a new cloud service and storage account.</span></span> <span data-ttu-id="6ce9c-240">Vegye figyelembe az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-240">Consider hello following options:</span></span>

* <span data-ttu-id="6ce9c-241">**Hozzon létre egy új SQL Server virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-241">**Create a new SQL Server VM**.</span></span> <span data-ttu-id="6ce9c-242">Új SQL Server virtuális gép egy prémium szintű Storage-fiókot használó hozhat létre, az új központi megfelelően.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-242">You can create a new SQL Server VM that uses a Premium Storage account, as documented in New Deployments.</span></span> <span data-ttu-id="6ce9c-243">Majd készítsen biztonsági másolatot, és az SQL Server-konfigurációs és felhasználói adatbázisok visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-243">Then backup and restore your SQL Server configuration and user databases.</span></span> <span data-ttu-id="6ce9c-244">hello alkalmazásnak szüksége lesz frissítve toobe tooreference hello új SQL-kiszolgálót, ha azt kívül és belül is hozzáférnek.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-244">hello application will need toobe updated tooreference hello new SQL Server if it is being accessed internally or externally.</span></span> <span data-ttu-id="6ce9c-245">Kellene toocopy minden "kívül db" objektumot, ha korábban végzett (SxS) SQL Server párhuzamos áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-245">You would need toocopy all ‘out of db’ objects as if you were doing a Side by Side (SxS) SQL Server migration.</span></span> <span data-ttu-id="6ce9c-246">Ez magában foglalja az objektumok, például a bejelentkezési adatok, a tanúsítványokat és a csatolt kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-246">This includes objects such as logins, certificates, and linked servers.</span></span>
* <span data-ttu-id="6ce9c-247">**Telepítse át a meglévő SQL Server virtuális**.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-247">**Migrate an existing SQL Server VM**.</span></span> <span data-ttu-id="6ce9c-248">Ehhez szükséges hello SQL Server virtuális gép offline állapotba helyezése, majd áthelyezte azt tooa új felhőalapú szolgáltatás, beleértve a csatlakoztatott virtuális merevlemezek toohello prémium szintű Storage-fiók összes másolása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-248">This will require taking hello SQL Server VM offline, then transferring it tooa new cloud service, which includes copying all of its attached VHDs toohello Premium Storage account.</span></span> <span data-ttu-id="6ce9c-249">Virtuális gép hello online állapotba kerül, ha hello alkalmazás hello állomásnevét, mielőtt hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-249">When hello VM comes online, hello application will reference hello server host name as before.</span></span> <span data-ttu-id="6ce9c-250">Vegye figyelembe, hogy a meglévő lemez hello hello mérete hatással lesz a hello teljesítményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-250">Be aware that hello size of hello existing disk will affect hello performance characteristics.</span></span> <span data-ttu-id="6ce9c-251">Például egy 400 GB lemezterület tooa P20 kerekíti lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-251">For example, a 400 GB disk gets rounded up tooa P20.</span></span> <span data-ttu-id="6ce9c-252">Ha tudja, hogy nincs szüksége a lemez teljesítménye akkor lehetett hello VM DS adatsorozat virtuális gépként hozza létre, és csatolja a prémium szintű Storage VHD-k hello mérete/teljesítmény specifikáció van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-252">If you know that you do not require that disk performance, then you could recreate hello VM as a DS Series VM, and attach Premium Storage VHDs of hello size/performance specification you require.</span></span> <span data-ttu-id="6ce9c-253">Ezután sikerült leválasztani a, és csatlakoztassa újra hello SQL-adatbázis a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-253">Then you could detach and reattach hello SQL DB files.</span></span>

> [!NOTE]
> <span data-ttu-id="6ce9c-254">Érdemes figyelembe hello méretű hello méretétől függően hello VHD lemezek másolásának azt jelenti, hogy prémium szintű tároló lemez típusát esnek, ez határozza meg a lemez teljesítménye megadását.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-254">When copying hello VHD disks you should be aware of hello size, depending on hello size will mean what Premium Storage Disk type they fall into, this determines disk performance specification.</span></span> <span data-ttu-id="6ce9c-255">Lemez legközelebbi toohello mentése round Azure lesz mérete, így ha egy 400 GB lemezterület, ez kerekíti tooa P20.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-255">Azure will round up toohello nearest disk size, so if you have a 400GB disk, this will be rounded up tooa P20.</span></span> <span data-ttu-id="6ce9c-256">Attól függően, hogy a meglévő IO-követelményeket a hello az operációs rendszer virtuális merevlemez előfordulhat, nem kell toomigrate a tooa prémium szintű Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-256">Depending on your existing IO requirements of hello OS VHD, you might not need toomigrate this tooa Premium Storage account.</span></span>
>
>

<span data-ttu-id="6ce9c-257">Az SQL Server külsőleg érhető el, ha hello cloud service VIP változik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-257">If your SQL Server is accessed externally, then hello cloud service VIP will change.</span></span> <span data-ttu-id="6ce9c-258">Ki is tooupdate végpontok, a hozzáférés-vezérlési listák és a DNS-beállításait.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-258">You will also have tooupdate end points, ACLs, and DNS settings.</span></span>

## <a name="existing-deployments-that-use-always-on-availability-groups"></a><span data-ttu-id="6ce9c-259">Always On rendelkezésre állási csoportok használó meglévő központi telepítések</span><span class="sxs-lookup"><span data-stu-id="6ce9c-259">Existing deployments that use Always On Availability Groups</span></span>
> [!NOTE]
> <span data-ttu-id="6ce9c-260">A meglévő telepítések esetében először lásd: hello [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-260">For existing deployments, first see hello [Prerequisites](#prerequisites-for-premium-storage) section of this topic.</span></span>
>
>

<span data-ttu-id="6ce9c-261">Kezdetben ebben a szakaszban követően áttekintjük hogyan Always On Azure hálózatkezelési működjön.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-261">Initially in this section we will look at how Always On interacts with Azure Networking.</span></span> <span data-ttu-id="6ce9c-262">Azt fogja majd lebontva áttelepítések tootwo forgatókönyvekben: áttelepítéseket, ahol is megengedett némi állásidővel, és áttelepítéseket, ahol minimális állásidővel kell elérni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-262">We will then break down migrations in tootwo scenarios: migrations where some downtime can be tolerated and migrations where you must achieve minimal downtime.</span></span>

<span data-ttu-id="6ce9c-263">A helyszíni SQL Server Always On rendelkezésre állási csoportok használatára a figyelő a helyi, amely egy virtuális DNS-nevet egy IP-cím, egy vagy több SQL Server-kiszolgálók között megosztott együtt regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-263">On-premises SQL Server Always On Availability Groups use a Listener on-premises that registers a virtual DNS name along with an IP address that is shared between one or more SQL Servers.</span></span> <span data-ttu-id="6ce9c-264">Ha az ügyfelek hello figyelő IP toohello elsődleges SQL-kiszolgálón keresztül halad.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-264">When clients connect they are routed through hello listener IP toohello Primary SQL Server.</span></span> <span data-ttu-id="6ce9c-265">Ez az adott időpontban mindig az IP-erőforrás hello birtokló hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-265">This is hello server that owns hello Always On IP resource at that time.</span></span>

![A DeploymentsUseAlways][6]

<span data-ttu-id="6ce9c-267">A Microsoft Azure-ban akkor is csak egy IP cím tooa hálózati adapter a hello VM, tehát a rendelés tooachieve hello azonos, a helyszíni absztrakciós réteget, Azure hello IP-címet, amely hozzá van rendelve toohello belső/külső terheléselosztó (ILB/ELB) használja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-267">In Microsoft Azure you can have only one IP address assigned tooa NIC on hello VM, so in order tooachieve hello same layer of abstraction as on-premises, Azure utilizes hello IP address that is assigned toohello Internal/External Load Balancers (ILB/ELB).</span></span> <span data-ttu-id="6ce9c-268">hello kiszolgálók között megosztott hello IP-erőforrás értéke toohello azonos IP mint hello ILB-/ ELB.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-268">hello IP resource that is shared between hello servers is set toohello same IP as hello ILB/ELB.</span></span> <span data-ttu-id="6ce9c-269">Ez a hello DNS közzé van téve, és ügyfélforgalmat továbbítja a hello ILB-/ ELB toohello elsődleges SQL-kiszolgáló replika.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-269">This is published in hello DNS, and client traffic is passed through hello ILB/ELB toohello Primary SQL Server replica.</span></span> <span data-ttu-id="6ce9c-270">hello ILB-/ ELB tudja, melyik SQL Server elsődleges óta mintavételt tooprobe hello mindig az IP-erőforrást használ.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-270">hello ILB/ELB knows which SQL Server is primary since it uses probes tooprobe hello Always On IP resource.</span></span> <span data-ttu-id="6ce9c-271">Hello előző példában azt vizsgálat hello ELB/ILB által hivatkozott végpont minden csomópont, a amelyik reagál hello elsődleges SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-271">In hello previous example, it probes each node that has an endpoint referenced by hello ELB/ILB, whichever responds is hello Primary SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="6ce9c-272">hello ILB és üzembe helyezett ELB mindkét rendelt tooa adott Azure-felhőszolgáltatásban, ezért bármely áttelepülés a felhőbe az Azure-ban lesz valószínűleg jelenti azt, hogy hello Load Balancer IP változik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-272">hello ILB and ELB are both assigned tooa particular Azure cloud service, therefore any cloud migration in Azure will most likely mean that hello Load Balancer IP will change.</span></span>
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a><span data-ttu-id="6ce9c-273">Áttelepítése mindig a központi telepítések engedélyezhetik bizonyos időre leállítást</span><span class="sxs-lookup"><span data-stu-id="6ce9c-273">Migrating Always On deployments that can allow some downtime</span></span>
<span data-ttu-id="6ce9c-274">Számos két stratégiák toomigrate mindig a központi telepítések lehetővé teszik a bizonyos időre leállítást.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-274">There are two strategies toomigrate Always On deployments that allow for some downtime:</span></span>

1. <span data-ttu-id="6ce9c-275">**Több másodlagos replika tooan meglévő mindig a fürt hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="6ce9c-275">**Add more secondary replicas tooan existing Always On Cluster**</span></span>
2. <span data-ttu-id="6ce9c-276">**Telepítse át a tooa új mindig a fürt**</span><span class="sxs-lookup"><span data-stu-id="6ce9c-276">**Migrate tooa new Always On Cluster**</span></span>

#### <a name="1-add-more-secondary-replicas-tooan-existing-always-on-cluster"></a><span data-ttu-id="6ce9c-277">1. Több másodlagos replika tooan hozzáadása meglévő mindig a fürt</span><span class="sxs-lookup"><span data-stu-id="6ce9c-277">1. Add more Secondary Replicas tooan Existing Always On Cluster</span></span>
<span data-ttu-id="6ce9c-278">Egy stratégia tooadd van több másodlagos adatbázist toohello Always On rendelkezésre állási csoportnak.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-278">One strategy is tooadd more secondaries toohello Always On Availability Group.</span></span> <span data-ttu-id="6ce9c-279">Új felhőalapú szolgáltatás be ezeket tooadd kell, és frissítse az hello figyelő hello új load balancer IP.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-279">You need tooadd these into a new cloud service and update hello listener with hello new load balancer IP.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="6ce9c-280">Állásidő pontok:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-280">Points of downtime:</span></span>
* <span data-ttu-id="6ce9c-281">A fürt ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-281">Cluster Validation.</span></span>
* <span data-ttu-id="6ce9c-282">Új másodlagos adatbázis-tesztelési mindig a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-282">Testing Always On failovers for New Secondaries.</span></span>

<span data-ttu-id="6ce9c-283">Ha használ Tárolókészletek Windows hello VM belül IO nagyobb átviteli teljesítményt, akkor a rendszer offline állapotra állítja a teljes fürt ellenőrzése során.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-283">If you are using Windows Storage Pools within hello VM for higher IO throughput, then these will be taken offline during a Full Cluster Validation.</span></span> <span data-ttu-id="6ce9c-284">hello teszttel ellenőrizheti, ha a csomópontok toohello fürt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-284">hello validation test is required when you add nodes toohello cluster.</span></span> <span data-ttu-id="6ce9c-285">hello időt toorun hello teszt eltérőek lehetnek, így kell tesztelje a reprezentatív tesztelési környezetben tooget, hogy mennyi ideig Ez eltarthat egy megközelítőleges időpont, amikor.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-285">hello time it takes toorun hello test can vary, so you should test this in your representative test environment tooget an approximate time of how long this will take.</span></span>

<span data-ttu-id="6ce9c-286">Ha kézi feladatátvételre végezheti el, és chaos tesztelés hello az újonnan hozzáadott csomópontok tooensure magas rendelkezésre állású mindig a Funkciók, a várt kell kiépíteni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-286">You should provision time where you can perform manual failover and chaos testing on hello newly added nodes tooensure Always On High Availability functions as expected.</span></span>

![DeploymentUseAlways On2][7]

> [!NOTE]
> <span data-ttu-id="6ce9c-288">Ha hello Tárolókészletek hello érvényesítés futtatása előtt használt SQL Server összes példányát le kell állítani.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-288">You should stop all instances of SQL Server where hello Storage Pools are used before hello Validation runs.</span></span>
>
> ##### <a name="high-level-steps"></a><span data-ttu-id="6ce9c-289">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="6ce9c-289">High-level steps</span></span>
>

1. <span data-ttu-id="6ce9c-290">Hozzon létre új felhőszolgáltatás csatlakoztatott prémium szintű Storage két új SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-290">Create two new SQL Servers in new cloud service with attached Premium Storage.</span></span>
2. <span data-ttu-id="6ce9c-291">Másolja át a teljes biztonsági mentést, és állítsa vissza a **NORECOVERY**.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-291">Copy over FULL backups and restore with **NORECOVERY**.</span></span>
3. <span data-ttu-id="6ce9c-292">Másolja át a "kívül felhasználói DB" függő objektumok, például a bejelentkezéseket stb.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-292">Copy over ‘out of user DB’ dependent objects, such as logins etc.</span></span>
4. <span data-ttu-id="6ce9c-293">Létrehozhat új egy új belső Load Balancer (ILB), illetve egy külső Load Balancer (ELB) használja, majd állítsa be a terhelés eloszlik végpontok mindkét új csomópontjának.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-293">Create new a new Internal Load Balancer (ILB) or use an External Load Balancer (ELB), and then set up Load Balanced Endpoints on both new nodes.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6ce9c-294">Ellenőrizze minden csomópont hello megfelelő végpont-konfiguráció van, a folytatás előtt</span><span class="sxs-lookup"><span data-stu-id="6ce9c-294">Check all Nodes have hello correct Endpoint configuration before you continue</span></span>
   >
   >
5. <span data-ttu-id="6ce9c-295">Állítsa le a felhasználó vagy alkalmazás-hozzáférés toohello SQL Server (ha Tárolókészletek használata).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-295">Stop User/Application Access toohello SQL Server (if using Storage Pools).</span></span>
6. <span data-ttu-id="6ce9c-296">SQL Server adatbázismotor-szolgáltatások leállítása az összes olyan csomóponton, (ha Tárolókészletek használata).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-296">Stop SQL Server Engine Services on All Nodes (if using Storage Pools).</span></span>
7. <span data-ttu-id="6ce9c-297">Adja hozzá az új csomópontok toocluster, és futtassa teljes ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-297">Add new Nodes toocluster and run full validation.</span></span>
8. <span data-ttu-id="6ce9c-298">Miután az ellenőrzés nem jelez hibát, indítsa el az összes SQL Server szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-298">Once Validation is successful, start all SQL Server Services.</span></span>
9. <span data-ttu-id="6ce9c-299">Tranzakciós naplók biztonsági mentése és visszaállítása felhasználói adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-299">Backup Transaction logs, and restore user databases.</span></span>
10. <span data-ttu-id="6ce9c-300">Vegyen fel új csomópontok hello Always On rendelkezésre állási csoportnak, és helyezze el a replikációs **szinkron**.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-300">Add new nodes into hello Always On Availability Group and place replication into **Synchronous**.</span></span>
11. <span data-ttu-id="6ce9c-301">Hello IP-cím hozzáadása hello cím erőforrása új felhőalapú szolgáltatás ILB-/ ELB a PowerShell segítségével az Always On alapján hello többhelyes példát hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-301">Add hello IP address resource of hello new Cloud Service ILB/ELB through PowerShell for Always On based on hello Multi-site example in hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span> <span data-ttu-id="6ce9c-302">A Windows fürtszolgáltatás, állítsa be a hello **lehetséges tulajdonosok** a hello **IP-cím** erőforrás toohello új csomópontok régi.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-302">In Windows clustering, set hello **Possible owners** of hello **IP Address** resource toohello new nodes old.</span></span> <span data-ttu-id="6ce9c-303">Című rész hello "IP-cím erőforrás hozzáadása ugyanazon az alhálózaton" hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-303">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
12. <span data-ttu-id="6ce9c-304">Feladatátvételi tooone hello új csomópontok.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-304">Failover tooone of hello new nodes.</span></span>
13. <span data-ttu-id="6ce9c-305">Ellenőrizze a hello új csomópontok automatikus feladatátvételi partnerként és feladatátvételi tesztek.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-305">Make hello new nodes Auto Failover Partners and test failovers.</span></span>
14. <span data-ttu-id="6ce9c-306">Távolítsa el az eredeti csomópont a rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-306">Remove original nodes from Availability Group.</span></span>

##### <a name="advantages"></a><span data-ttu-id="6ce9c-307">Előnyei</span><span class="sxs-lookup"><span data-stu-id="6ce9c-307">Advantages</span></span>
* <span data-ttu-id="6ce9c-308">Új SQL-kiszolgálók lehet tesztelni (SQL Server alkalmazás) tooAlways a Hozzáadás előtt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-308">New SQL Servers can be tested (SQL Server and Application) before they are added tooAlways On.</span></span>
* <span data-ttu-id="6ce9c-309">Hello VM Oldalméret módosítása oly módon, és testre szabhatja hello tárolási tooyour pontos követelményeit.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-309">You can change hello VM size and customize hello storage tooyour exact requirements.</span></span> <span data-ttu-id="6ce9c-310">Azonban hasznos tookeep lenne hello SQL fájl görbékhez hello azonos.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-310">However, it would be beneficial tookeep all hello SQL file paths hello same.</span></span>
* <span data-ttu-id="6ce9c-311">Hello DB biztonsági mentések toohello másodlagos replikák hello átruházása elkezdésének szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-311">You can control when hello transfer of hello DB backups toohello Secondary Replicas are started.</span></span> <span data-ttu-id="6ce9c-312">Ez eltér az Azure használatával **Start-AzureStorageBlobCopy** parancsmag toocopy VHD-k, mert ez egy aszinkron másolatot.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-312">This differs from using Azure **Start-AzureStorageBlobCopy** commandlet toocopy VHDs, because that is an asynchronous copy.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="6ce9c-313">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-313">Disadvantages</span></span>
* <span data-ttu-id="6ce9c-314">Windows Tárolókészletek használata esetén nincs fürt állásidő hello teljes fürtérvényesítési hello új további csomópontok során.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-314">When using Windows Storage Pools, there is Cluster downtime during hello Full Cluster Validation for hello new additional nodes.</span></span>
* <span data-ttu-id="6ce9c-315">Attól függően, hogy az SQL Server verziója hello és hello meglévő másodlagos replikák száma, akkor előfordulhat, hogy nem tud tooadd több másodlagos replika meglévő másodlagos adatbázis eltávolítása nélkül lehet.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-315">Depending on hello SQL Server Version and hello existing number of secondary replicas, you might not be able tooadd more secondary replicas without removing existing secondaries.</span></span>
* <span data-ttu-id="6ce9c-316">Előfordulhat, hogy hosszú SQL adatátviteli idő hello másodlagos beállítása közben.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-316">There could be long SQL data transfer time while setting up hello secondaries.</span></span>
* <span data-ttu-id="6ce9c-317">Nincs további költség nélkül az áttelepítés során előfordulhat, hogy a párhuzamosan futó új gépek.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-317">There is additional cost during migration while you have new machines running in parallel.</span></span>

#### <a name="2-migrate-tooa-new-always-on-cluster"></a><span data-ttu-id="6ce9c-318">2. Telepítse át a tooa új mindig a fürt</span><span class="sxs-lookup"><span data-stu-id="6ce9c-318">2. Migrate tooa new Always On Cluster</span></span>
<span data-ttu-id="6ce9c-319">Egy másik olyan stratégia toocreate egy teljesen új mindig a fürt új csomóponttal rendelkező új felhőalapú szolgáltatás és az átirányítási hello ügyfelek toouse azt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-319">Another strategy is toocreate a brand new Always On Cluster with brand new nodes in new cloud service and then redirect hello clients toouse it.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="6ce9c-320">Állásidő pontok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-320">Points of downtime</span></span>
<span data-ttu-id="6ce9c-321">Alkalmazások és a felhasználók toohello új mindig a figyelő-átvitel során, nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-321">There is downtime when you transfer applications and users toohello new Always On listener.</span></span> <span data-ttu-id="6ce9c-322">hello állásidő függ:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-322">hello downtime depends on:</span></span>

* <span data-ttu-id="6ce9c-323">hello idő toorestore végső tranzakciós napló biztonsági mentések toodatabases új kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-323">hello time taken toorestore final transaction log backups toodatabases on new servers.</span></span>
* <span data-ttu-id="6ce9c-324">hello igénybe vett idő tooupdate ügyfél alkalmazások toouse új mindig a figyelő.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-324">hello time taken tooupdate client applications toouse new Always On listener.</span></span>

##### <a name="advantages"></a><span data-ttu-id="6ce9c-325">Előnyei</span><span class="sxs-lookup"><span data-stu-id="6ce9c-325">Advantages</span></span>
* <span data-ttu-id="6ce9c-326">Hello tényleges éles környezetben, SQL Server, tesztelheti, és operációs rendszer a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-326">You can test hello actual production environment, SQL Server, and OS build changes.</span></span>
* <span data-ttu-id="6ce9c-327">Hello beállítás toocustomize hello tárhellyel rendelkező és toopotentially csökkentheti a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-327">You have hello option toocustomize hello storage and toopotentially reduce size of VM.</span></span> <span data-ttu-id="6ce9c-328">Emiatt költségek csökkentéséhez.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-328">This could result in cost reduction.</span></span>
* <span data-ttu-id="6ce9c-329">A SQL Server build vagy a verziójával frissítheti a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-329">You can update your SQL Server build or version during this process.</span></span> <span data-ttu-id="6ce9c-330">Operációs rendszer hello is lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-330">You can also upgrade hello Operating System.</span></span>
* <span data-ttu-id="6ce9c-331">hello előző mindig a fürt működhet teli visszaállítás céljaként.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-331">hello previous Always On Cluster can act as a solid rollback target.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="6ce9c-332">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-332">Disadvantages</span></span>
* <span data-ttu-id="6ce9c-333">Ha azt szeretné, hogy mindkét fut egyszerre mindig a fürtök szüksége hello figyelő toochange hello DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-333">You need toochange hello DNS name of hello listener if you want both Always On clusters running simultaneously.</span></span> <span data-ttu-id="6ce9c-334">Adminisztrációs terhet hello az áttelepítés során ez biztosítja a, az ügyfél alkalmazás karakterláncok tükröznie kell hello új figyelő nevét.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-334">This adds administration overhead during hello migration as client application strings must reflect hello new Listener name.</span></span>
* <span data-ttu-id="6ce9c-335">Meg kell valósítani a szinkronizálási mechanizmus hello azokat a lehetséges toominimize hello végleges szinkronizálást követelmény áttelepítés előtt zárja be két környezetek tookeep között.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-335">You must implement a synchronization mechanism between hello two environments tookeep them as close as possible toominimize hello final synchronization requirements before migration.</span></span>
* <span data-ttu-id="6ce9c-336">Hiba kerül költség az áttelepítés során közben fut hello új környezettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-336">There is added cost during migration while you have hello new environment running.</span></span>

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a><span data-ttu-id="6ce9c-337">Áttelepítése mindig a központi telepítések a minimális állásidő érdekében</span><span class="sxs-lookup"><span data-stu-id="6ce9c-337">Migrating Always On Deployments for minimal downtime</span></span>
<span data-ttu-id="6ce9c-338">Nincsenek áttelepítése mindig a központi telepítés két stratégiák a minimális állásidő érdekében:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-338">There are two strategies for migrating Always On deployments for minimal downtime:</span></span>

1. <span data-ttu-id="6ce9c-339">**Egy létező másodlagos használják: egy helyen**</span><span class="sxs-lookup"><span data-stu-id="6ce9c-339">**Utilize an Existing Secondary: Single-Site**</span></span>
2. <span data-ttu-id="6ce9c-340">**Meglévő másodlagos másodpéldányt használják: többhelyes**</span><span class="sxs-lookup"><span data-stu-id="6ce9c-340">**Utilize Existing Secondary Replica(s): Multi-Site**</span></span>

#### <a name="1-utilize-an-existing-secondary-single-site"></a><span data-ttu-id="6ce9c-341">1. Egy létező másodlagos használják: egy helyen</span><span class="sxs-lookup"><span data-stu-id="6ce9c-341">1. Utilize an existing secondary: Single-Site</span></span>
<span data-ttu-id="6ce9c-342">A minimális állásidő érdekében egy stratégia tootake egy létező másodlagos felhőben, és távolítsa el az aktuális felhőszolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-342">One strategy for minimal downtime is tootake an existing cloud secondary and remove it from hello current cloud service.</span></span> <span data-ttu-id="6ce9c-343">Ezután másolja a VHD-k toohello új prémium szintű Storage-fiók hello, és hozzon létre hello VM hello új felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-343">Then copy hello VHDs toohello new Premium Storage account, and create hello VM in hello new cloud service.</span></span> <span data-ttu-id="6ce9c-344">Módosítsa a fürtszolgáltatás és a feladatátvételi hello figyelő.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-344">Then update hello listener in clustering and failover.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="6ce9c-345">Állásidő pontok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-345">Points of downtime</span></span>
* <span data-ttu-id="6ce9c-346">Amikor a végső csomópontja hello hello az elosztott terhelésű végpont, nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-346">There is downtime when you update hello final node with hello Load Balanced endpoint.</span></span>
* <span data-ttu-id="6ce9c-347">Az ügyfél újracsatlakozás ügyfél és a DNS-konfigurációtól függően előfordulhat, hogy később.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-347">Your client reconnection might be delayed depending on your client/DNS configuration.</span></span>
* <span data-ttu-id="6ce9c-348">Ha úgy dönt, hogy tootake hello mindig fürt csoport offline tooswap kimenő hello IP-címek, nincs további állásidő.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-348">There is additional downtime if you choose tootake hello Always On Cluster group offline tooswap out hello IP addresses.</span></span> <span data-ttu-id="6ce9c-349">Egy vagy függőség használatával elkerülheti ezt, és lehetséges tulajdonosainak hello hozzáadott IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-349">You can avoid this by using an OR dependency and Possible Owners for hello added IP Address resource.</span></span> <span data-ttu-id="6ce9c-350">Című rész hello "IP-cím erőforrás hozzáadása ugyanazon az alhálózaton" hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-350">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>

> [!NOTE]
> <span data-ttu-id="6ce9c-351">Ha azt szeretné, hogy hello hozzáadott csomópont toopartake a partnerként mindig a feladatátvétel, tooadd a terhelés eloszlik beállítása hivatkozás toohello Azure végpont kell.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-351">When you want hello added node toopartake in as an Always On Failover Partner, you need tooadd an Azure Endpoint with a reference toohello Load Balanced Set.</span></span> <span data-ttu-id="6ce9c-352">Hello futtatásakor **Add-AzureEndpoint** toodo nyissa meg a, aktuális kapcsolatok tooremain parancsot, de új kapcsolatok toohello figyelő nem lesz képes toobe mindaddig, amíg hello terheléselosztó frissült.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-352">When you run hello **Add-AzureEndpoint** command toodo this, current connections tooremain open, but new connections toohello listener will not be able toobe established until hello load balancer has updated.</span></span> <span data-ttu-id="6ce9c-353">A tesztelés látott toolast 90-120seconds volt, ez kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-353">In testing this was seen toolast 90-120seconds, this should be tested.</span></span>
>
>

##### <a name="advantages"></a><span data-ttu-id="6ce9c-354">Előnyei</span><span class="sxs-lookup"><span data-stu-id="6ce9c-354">Advantages</span></span>
* <span data-ttu-id="6ce9c-355">Nincsenek további az áttelepítés során felmerülő költség.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-355">No extra cost incurred during migration.</span></span>
* <span data-ttu-id="6ce9c-356">-Az-egyhez áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-356">A one-to-one migration.</span></span>
* <span data-ttu-id="6ce9c-357">Csökkentett összetettségét.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-357">Reduced complexity.</span></span>
* <span data-ttu-id="6ce9c-358">Lehetővé teszi, hogy a megnövekedett IOPS a prémium szintű Storage SKU.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-358">Allows for increased IOPS from Premium Storage SKUs.</span></span> <span data-ttu-id="6ce9c-359">Amikor hello lemezek hello VM leválasztani és toohello új felhőalapú szolgáltatás, másolja a 3. fél eszköz lehet magasabb teljesítmények biztosít használt tooincrease hello virtuális merevlemez méretét.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-359">When hello disks are detached from hello VM and copied toohello new cloud service, a 3rd party tool can be used tooincrease hello VHD size, which provides higher throughputs.</span></span> <span data-ttu-id="6ce9c-360">Virtuális merevlemez mérete növekszik, lásd: a [fórum vitafórum](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-360">For increasing VHD sizes, see this [forum discussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="6ce9c-361">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-361">Disadvantages</span></span>
* <span data-ttu-id="6ce9c-362">Nincs magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási átmenetileg megszakad az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-362">There is a temporary loss of HA and DR during migration.</span></span>
* <span data-ttu-id="6ce9c-363">Mivel ez egy 1:1 áttelepítési, egy minimális Virtuálisgép-méretet, amely támogatja a virtuális merevlemezeket, számát, nem feltétlenül tudja toodownsize a virtuális gépek toouse fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-363">As this is a 1:1 migration, you will have toouse a minimum VM size that will support your number of VHDs, so you might not be able toodownsize your VMs.</span></span>
* <span data-ttu-id="6ce9c-364">Ebben a forgatókönyvben használna hello Azure **Start-AzureStorageBlobCopy** parancsmag, amely aszinkron.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-364">This scenario would use hello Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="6ce9c-365">Nincs nincs SLA-t a Másolás befejezése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-365">There is no SLA on copy completion.</span></span> <span data-ttu-id="6ce9c-366">hello példányok hello idő változó, amíg ez függ a várakozási sorban is függ adatok tootransfer hello mennyisége.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-366">hello time of hello copies varies, while this depends on wait in queue it will also depend on hello amount of data tootransfer.</span></span> <span data-ttu-id="6ce9c-367">hello ideje növekszik, ha hello átviteli tooanother Azure-adatközponthoz, amely támogatja a prémium szintű Storage egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-367">hello copy time increases if hello transfer is going tooanother Azure data center that supports Premium Storage in another region.</span></span> <span data-ttu-id="6ce9c-368">Ha csak 2 csomópontok, inkább egy lehetséges megoldás, ha hello másolási tovább tart, mint a tesztelés.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-368">If you just have 2 nodes, consider a possible mitigation in case hello copy takes longer than in testing.</span></span> <span data-ttu-id="6ce9c-369">Ez magában foglalhatja a következő ötletek hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-369">This could include hello following ideas.</span></span>
  * <span data-ttu-id="6ce9c-370">Ideiglenes 3. az SQL Server csomópont hozzáadása a magas rendelkezésre ÁLLÁSÚ egyeztetett állásidővel hello áttelepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-370">Add a temporary 3rd SQL Server node for HA before hello migration with agreed downtime.</span></span>
  * <span data-ttu-id="6ce9c-371">Hello áttelepítéshez Azure ütemezett karbantartás kívül.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-371">Run hello migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="6ce9c-372">Győződjön meg arról, a fürt kvóruma megfelelően konfigurálta-e.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-372">Ensure you have configured your cluster quorum correctly.</span></span>  

##### <a name="high-level-steps"></a><span data-ttu-id="6ce9c-373">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="6ce9c-373">High-level steps</span></span>
<span data-ttu-id="6ce9c-374">Ez a dokumentum nem bemutatása teljes end tooend példában azonban hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) ismerteti, amelyek alkalmazhatók tooperform lehetnek ez.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-374">This document does not demonstrate a complete end tooend example, however hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) provides details that can be leveraged tooperform this.</span></span>

![MinimalDowntime][8]

* <span data-ttu-id="6ce9c-376">Összefog lemez konfigurációját, és távolítsa el hello csomópont (ne törölje a csatolt VHD-k).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-376">Gather disk configuration, and remove hello node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="6ce9c-377">Prémium szintű Storage-fiók létrehozása és virtuális merevlemezek másolása hello standard szintű tárfiók</span><span class="sxs-lookup"><span data-stu-id="6ce9c-377">Create a Premium Storage account and copy VHDs from hello Standard Storage account</span></span>
* <span data-ttu-id="6ce9c-378">Új felhőalapú szolgáltatás létrehozása, és telepítse újra a hello SQL2 virtuális gép található, amely a felhőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-378">Create new cloud service and redeploy hello SQL2 VM in that cloud service.</span></span> <span data-ttu-id="6ce9c-379">Hozzon létre virtuális gép hello hello használatával másolja az eredeti operációs rendszer virtuális merevlemez és a kapcsolódó hello másolja a VHD-k.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-379">Create hello VM using hello copied original OS VHD and attaching hello copied VHDs.</span></span>
* <span data-ttu-id="6ce9c-380">Konfigurálja a ILB / ELB és végpont-hozzáadáshoz.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-380">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="6ce9c-381">Frissítés figyelő egyike:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-381">Update Listener by either:</span></span>
  * <span data-ttu-id="6ce9c-382">Véve hello mindig csoport offline állapotú, és a frissítési hello mindig a figyelő az új ILB és üzembe helyezett ELB IP-cím.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-382">Taking hello Always On Group offline and updating hello Always On Listener with new ILB / ELB IP address.</span></span>
  * <span data-ttu-id="6ce9c-383">Vagy hello IP-cím erőforrás az új felhőalapú szolgáltatás ILB-/ ELB Powershellen keresztül történő Windows-fürtszolgáltatás hozzáadására.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-383">Or adding hello IP address resource of new Cloud Service ILB/ELB through PowerShell into Windows clustering.</span></span> <span data-ttu-id="6ce9c-384">Majd set hello lehetséges tulajdonosainak hello IP-cím erőforrás toohello csomópont, SQL2, át, és állítsa a hello hálózatnév vagy függőségként.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-384">Then set hello Possible owners of hello IP Address resource toohello migrated node, SQL2, and set this as OR dependency in hello Network Name.</span></span> <span data-ttu-id="6ce9c-385">Című rész hello "IP-cím erőforrás hozzáadása ugyanazon az alhálózaton" hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-385">See hello ‘Adding IP Address Resource on Same Subnet’ section of hello [Appendix](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).</span></span>
* <span data-ttu-id="6ce9c-386">Ellenőrizze a DNS-konfiguráció/propagálási toohello ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-386">Check DNS configuration/propogation toohello clients.</span></span>
* <span data-ttu-id="6ce9c-387">Telepítse át az sql1 számítógép virtuális gép, és nyissa meg a 2 – 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-387">Migrate SQL1 VM, and go through steps 2 – 4.</span></span>
* <span data-ttu-id="6ce9c-388">Ha lépéseket 5ii használ, majd adja hozzá az sql1 számítógép ennek hello lehetséges tulajdonosa hozzá IP-cím erőforrás</span><span class="sxs-lookup"><span data-stu-id="6ce9c-388">If using steps 5ii, then add SQL1 as a Possible Owner for hello added IP Address Resource</span></span>
* <span data-ttu-id="6ce9c-389">Feladatátvétel tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-389">Test failovers.</span></span>

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a><span data-ttu-id="6ce9c-390">2. Meglévő másodlagos másodpéldányt használják: többhelyes</span><span class="sxs-lookup"><span data-stu-id="6ce9c-390">2. Utilize existing secondary replica(s): Multi-Site</span></span>
<span data-ttu-id="6ce9c-391">Ha több Azure-adatközpontban (DC) vannak olyan csomópontok, vagy ha hibrid környezettel rendelkezik, majd egy mindig a konfigurációt használhatja a környezet toominimize állásidőt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-391">If you have nodes in more than one Azure datacenter (DC) or if you have a hybrid environment, then you can use an Always On configuration in this environment toominimize downtime.</span></span>

<span data-ttu-id="6ce9c-392">hello megoldás toochange hello mindig a szinkronizálási tooSynchronous hello a helyszíni vagy Azure a másodlagos Tartományvezérlőt, majd feladatátvétel, SQL Server toothat keresztül.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-392">hello approach is toochange hello Always On synchronization tooSynchronous for hello on-premises or secondary Azure DC, and then failover over toothat SQL Server.</span></span> <span data-ttu-id="6ce9c-393">Hello VHD-k tooa prémium szintű Storage-fiók másolja, és az új felhőalapú szolgáltatás hello gép újbóli üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-393">Then copy hello VHDs tooa Premium Storage account, and redeploy hello machine into a new cloud service.</span></span> <span data-ttu-id="6ce9c-394">Hello figyelő frissítése, és ezután a feladat-visszavételt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-394">Update hello listener, and then fail back.</span></span>

##### <a name="points-of-downtime"></a><span data-ttu-id="6ce9c-395">Állásidő pontok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-395">Points of downtime</span></span>
<span data-ttu-id="6ce9c-396">hello állásidő hello idő toofailover toohello alternatív DC és biztonsági áll.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-396">hello downtime consists of hello time toofailover toohello alternative DC and back.</span></span> <span data-ttu-id="6ce9c-397">Is függ, az ügyfél és a DNS-konfiguráció, és később, az ügyfél lehet újracsatlakozni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-397">It also depends on your client/DNS configuration and your client reconnection may be delayed.</span></span>
<span data-ttu-id="6ce9c-398">Vegye figyelembe a következő példa egy hibrid mindig a konfiguráció hello:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-398">Consider hello following example of a hybrid Always On configuration:</span></span>

![MultiSite1][9]

##### <a name="advantages"></a><span data-ttu-id="6ce9c-400">Előnyei</span><span class="sxs-lookup"><span data-stu-id="6ce9c-400">Advantages</span></span>
* <span data-ttu-id="6ce9c-401">Úgy használhatja a meglévő infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-401">You can utilize existing infrastructure.</span></span>
* <span data-ttu-id="6ce9c-402">Hello beállítás toopre frissítési hello az Azure storage először a vész-Helyreállítási Azure DC hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-402">You have hello option toopre-upgrade hello Azure storage on hello DR Azure DC first.</span></span>
* <span data-ttu-id="6ce9c-403">DR Azure DC tárolási hello újra kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-403">hello DR Azure DC storage can be reconfigured.</span></span>
* <span data-ttu-id="6ce9c-404">Nincs legalább két feladatátvételi teszteket kivéve az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-404">There is a minimum of two failovers during migration, excluding test failovers.</span></span>
* <span data-ttu-id="6ce9c-405">Nem kell toomove SQL Server-adatok biztonsági mentése és visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-405">You do not need toomove SQL Server data with backup and restore.</span></span>

##### <a name="disadvantages"></a><span data-ttu-id="6ce9c-406">Hátrányok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-406">Disadvantages</span></span>
* <span data-ttu-id="6ce9c-407">Attól függően, hogy az ügyfél-hozzáférési tooSQL kiszolgáló előfordulhat nagyobb késéseket SQL Server egy másik tartományvezérlő toohello alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-407">Depending on client access tooSQL Server, there might be increased latency when SQL Server is running in an alternative DC toohello application.</span></span>
* <span data-ttu-id="6ce9c-408">virtuális merevlemezek tooPremium tárolási hello ideje hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-408">hello copy time of VHDs tooPremium storage could be long.</span></span> <span data-ttu-id="6ce9c-409">Ez befolyásolhatja a döntést e tookeep hello hello rendelkezésre állási csoport csomópontja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-409">This might affect your decision on whether tookeep hello node in hello Availability Group.</span></span> <span data-ttu-id="6ce9c-410">Megfontolandó szempontok a amikor napló intenzív feladata terhelések hello az áttelepítés során futnak, mivel hello elsődleges csomópontot a tranzakciónaplóban árvává tranzakciók hello tookeep kell.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-410">Consider this for when log intensive work loads are running during hello migration is required, since hello Primary node will have tookeep hello unreplicated transactions in its transaction log.</span></span> <span data-ttu-id="6ce9c-411">Ezért ez sikerült jelentősen megnő.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-411">Therefore this could grow significantly.</span></span>
* <span data-ttu-id="6ce9c-412">Ebben a forgatókönyvben használna hello Azure **Start-AzureStorageBlobCopy** parancsmag, amely aszinkron.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-412">This scenario would use hello Azure **Start-AzureStorageBlobCopy** commandlet, which is asynchronous.</span></span> <span data-ttu-id="6ce9c-413">Nincs nincs SLA befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-413">There is no SLA on completion.</span></span> <span data-ttu-id="6ce9c-414">hello hello példányok idő változik, ez függ a várakozási sorban, miközben az adatok tootransfer hello mennyisége is függ.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-414">hello time of hello copies varies, while this depends on wait in queue, it will also depend on hello amount of data tootransfer.</span></span> <span data-ttu-id="6ce9c-415">Ezért csak egy csomópont van a 2. adatközpontban, abban az esetben hello másolási tovább tart, mint a tesztelés megoldás lépéseket kell tennie.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-415">Therefore you just have one node in your 2nd data center, you should take mitigation steps in case hello copy takes longer than in testing.</span></span> <span data-ttu-id="6ce9c-416">Ez magában foglalhatja a következő ötletek hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-416">This could include hello following ideas.</span></span>
  * <span data-ttu-id="6ce9c-417">Adja hozzá a 2. az SQL ideiglenes csomópontot a magas rendelkezésre ÁLLÁSÚ egyeztetett állásidővel hello áttelepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-417">Add a temporary 2nd SQL node for HA before hello migration with agreed downtime.</span></span>
  * <span data-ttu-id="6ce9c-418">Hello áttelepítéshez Azure ütemezett karbantartás kívül.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-418">Run hello migration outside of Azure scheduled maintenance.</span></span>
  * <span data-ttu-id="6ce9c-419">Győződjön meg arról, a fürt kvóruma megfelelően konfigurálta-e.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-419">Ensure you have configured your cluster quorum correctly.</span></span>

<span data-ttu-id="6ce9c-420">Ebben a forgatókönyvben feltételezi, hogy rendelkezik saját telepítési dokumentált, és tudja, hogyan le van képezve hello tárolási a rendelés toomake optimális gyorsítótár beállítások módosításait.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-420">This scenario assumes that you have documented your install and know how hello storage is mapped in order toomake changes for optimal disk cache settings.</span></span>

##### <a name="high-level-steps"></a><span data-ttu-id="6ce9c-421">Magas szintű lépései</span><span class="sxs-lookup"><span data-stu-id="6ce9c-421">High-level steps</span></span>
![Multisite2][10]

* <span data-ttu-id="6ce9c-423">Győződjön meg a helyszíni hello / Azure DC hello SQL Server elsődleges alternatív és könnyebben hello más automatikus feladatátvételi Partner (AFP).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-423">Make hello on-premises / alternate Azure DC hello SQL Server Primary, and make it hello other Auto Failover Partner (AFP).</span></span>
* <span data-ttu-id="6ce9c-424">Lemez konfigurációs adatokat gyűjt a SQL2, és távolítsa el hello csomópont (ne törölje a csatolt VHD-k).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-424">Gather disk configuration information from SQL2, and remove hello node (do not delete attached VHDs).</span></span>
* <span data-ttu-id="6ce9c-425">Prémium szintű Storage-fiók létrehozása, és másolja a VHD-k a hello standard szintű tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-425">Create a Premium Storage account and copy VHDs from hello Standard Storage account.</span></span>
* <span data-ttu-id="6ce9c-426">Hozzon létre egy új felhőalapú szolgáltatás, és hozzon létre a kapcsolódó díjakat tárolólemezek hello SQL2 virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-426">Create a new cloud service and create hello SQL2 VM with its Premiums Storage disks attached.</span></span>
* <span data-ttu-id="6ce9c-427">Konfigurálja a ILB / ELB és végpont-hozzáadáshoz.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-427">Configure ILB / ELB and add Endpoints.</span></span>
* <span data-ttu-id="6ce9c-428">Frissítés hello mindig a figyelő az új ILB / ELB IP cím és a teszt feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-428">Update hello Always On Listener with new ILB / ELB IP address and test failover.</span></span>
* <span data-ttu-id="6ce9c-429">Hello DNS konfigurációjának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-429">Check hello DNS configuration.</span></span>
* <span data-ttu-id="6ce9c-430">Hello AFP tooSQL2, módosítsa, majd telepítse át az sql1 számítógép és végrehajtania 2 – 5. lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-430">Change hello AFP tooSQL2, and then migrate SQL1 and go through steps 2 – 5.</span></span>
* <span data-ttu-id="6ce9c-431">Feladatátvétel tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-431">Test failovers.</span></span>
* <span data-ttu-id="6ce9c-432">Váltson vissza tooSQL1 hello AFP és SQL2</span><span class="sxs-lookup"><span data-stu-id="6ce9c-432">Switch hello AFP back tooSQL1 and SQL2</span></span>

## <a name="appendix-migrating-a-multisite-always-on-cluster-toopremium-storage"></a><span data-ttu-id="6ce9c-433">A függelék: Egy mindig a fürtözött tooPremium tároló áttelepítése</span><span class="sxs-lookup"><span data-stu-id="6ce9c-433">Appendix: Migrating a Multisite Always On Cluster tooPremium Storage</span></span>
<span data-ttu-id="6ce9c-434">Ez a témakör további része hello példát egy részletes egy többhelyes mindig a tooPremium fürttároló alakítása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-434">hello remainder of this topic provides a detailed example of converting a multi-site Always On cluster tooPremium storage.</span></span> <span data-ttu-id="6ce9c-435">Egy külső terheléselosztó (ELB) tooan belső terheléselosztón (ILB) használatával a hello figyelő is alakítja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-435">It also converts hello Listener from using an external load balancer (ELB) tooan internal load balancer (ILB).</span></span>

### <a name="environment"></a><span data-ttu-id="6ce9c-436">Környezet</span><span class="sxs-lookup"><span data-stu-id="6ce9c-436">Environment</span></span>
* <span data-ttu-id="6ce9c-437">2 KB-os Windows 12 / 2 KB-os SQL 12</span><span class="sxs-lookup"><span data-stu-id="6ce9c-437">Windows 2k12 / SQL 2k12</span></span>
* <span data-ttu-id="6ce9c-438">SP 1 DB fájlok</span><span class="sxs-lookup"><span data-stu-id="6ce9c-438">1 DB Files on SP</span></span>
* <span data-ttu-id="6ce9c-439">Tárolókészletek csomópontonként x 2</span><span class="sxs-lookup"><span data-stu-id="6ce9c-439">2 x Storage Pools per Node</span></span>

![Appendix1][11]

### <a name="vm"></a><span data-ttu-id="6ce9c-441">VIRTUÁLIS GÉP:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-441">VM:</span></span>
<span data-ttu-id="6ce9c-442">Ebben a példában fogjuk tér át egy üzembe helyezett ELB tooILB toodemonstrate.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-442">In this example we are going toodemonstrate moving from an ELB tooILB.</span></span> <span data-ttu-id="6ce9c-443">Üzembe helyezett ELB volt elérhető ILB, mielőtt, így ez azt jelenti, hogy hogyan tooswitch toothis során hello áttelepítési.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-443">ELB was available before ILB, so this shows how tooswitch toothis during hello migration.</span></span>

![Appendix2][12]

### <a name="pre-steps-connect-toosubscription"></a><span data-ttu-id="6ce9c-445">Előkészítő lépések: Csatlakozás tooSubscription</span><span class="sxs-lookup"><span data-stu-id="6ce9c-445">Pre Steps: Connect tooSubscription</span></span>
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a><span data-ttu-id="6ce9c-446">1. lépés: Új Tárfiók létrehozása, és a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6ce9c-446">Step 1: Create New Storage Account and Cloud Service</span></span>
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where hello vm toomigrate resides
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

#### <a name="step-2-increase-hello-permitted-failures-on-resources-optional"></a><span data-ttu-id="6ce9c-447">2. lépés: Növelje hello hibák engedélyezett erőforrások<Optional></span><span class="sxs-lookup"><span data-stu-id="6ce9c-447">Step 2: Increase hello permitted failures on resources <Optional></span></span>
<span data-ttu-id="6ce9c-448">Bizonyos erőforrások tooyour mindig a rendelkezésre állási csoporthoz tartozó nincsenek korlátozások a hány sikertelen végrehajtása esetén, amelyek adott időszakban, ahol a hello Fürtszolgáltatás megkísérli toorestart hello erőforráscsoport fordul elő.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-448">On certain resources that belong tooyour Always On Availability Group there are limits on how many failures that can occur in a period, where hello cluster service will attempt toorestart hello resource group.</span></span> <span data-ttu-id="6ce9c-449">Ajánlott növeli a miközben érdekében keresztül ez az eljárás, mert ellenkező esetben kézzel eseményindító és feladatátvételi feladatátvételek által gépek leállítása Bezárás toothis korlát kérheti le.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-449">It is recommended you increase this whilst you are walking through this procedure, since if you don’t manually failover and trigger failovers by shutting down machines you can get close toothis limit.</span></span>

<span data-ttu-id="6ce9c-450">Volna kell körültekintő toodouble hello hiba támogatás, toodo Ez a Feladatátvevőfürt-kezelőben, nyissa meg hello mindig az erőforráscsoport toohello tulajdonságainak:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-450">It would be prudent toodouble hello failure allowance, toodo this in Failover Cluster Manager, go toohello Properties of hello Always On resource group:</span></span>

![Appendix3][13]

<span data-ttu-id="6ce9c-452">Hello maximális hibák too6 módosítása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-452">Change hello Maximum Failures too6.</span></span>

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a><span data-ttu-id="6ce9c-453">3. lépés: IP-cím hozzáadása erőforrás fürt csoport<Optional></span><span class="sxs-lookup"><span data-stu-id="6ce9c-453">Step 3: Addition IP Address resource for Cluster Group <Optional></span></span>
<span data-ttu-id="6ce9c-454">Ha hello fürtcsoport csak egy IP-címe és igazított toohello felhő alhálózati, ügyeljen arra, ha véletlenül kapcsolat nélküli hello felhőalapú a fürt összes csomópontján, hogy hálózati majd hello fürt IP- és hálózati fürtnév nem lesz képes toocome online.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-454">If you have only one IP address for hello Cluster Group and this is aligned toohello cloud subnet, beware, if you accidentally take offline all cluster nodes in hello cloud on that network then hello Cluster IP resource and Cluster Network Name will not be able toocome online.</span></span> <span data-ttu-id="6ce9c-455">A hello Ez megakadályozza az esemény tooother fürterőforrások frissíti.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-455">In hello event of this it will prevent updates tooother cluster resources.</span></span>

#### <a name="step-4-dns-configuration"></a><span data-ttu-id="6ce9c-456">4. lépés: DNS-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6ce9c-456">Step 4: DNS configuration</span></span>
<span data-ttu-id="6ce9c-457">tooimplement zökkenőmentes átmenet, attól függ, hogyan DNS folyamatban van szükség, és frissíti.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-457">tooimplement a smooth transition depends on how DNS is being utilized and updated.</span></span>
<span data-ttu-id="6ce9c-458">Mindig telepítve van, ha létrehoz egy Windows fürterőforrás-csoporthoz, ha a Feladatátvevőfürt-kezelő megnyitásához láthatja, hogy legalább három erőforrások lesz, a dokumentum hello két hello tooare hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-458">When Always On is installed, it creates a Windows Cluster Resource group, if you open Failover Cluster Manager, you will see that at a minimum it will have three resources, hello two that hello document refers tooare:</span></span>

* <span data-ttu-id="6ce9c-459">Virtuális hálózat neve (VNN) – Ez az hello DNS-nevet, hogy az ügyfél toowhen tooconnect tooSQL mindig a kiszolgálókra, aki a csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-459">Virtual Network Name (VNN) – This is hello DNS name that client connect toowhen wanting tooconnect tooSQL Servers via Always On.</span></span>
* <span data-ttu-id="6ce9c-460">IP-cím erőforrás – Ez az IP-címe, amelyhez társítva hello VNN hello, több is van, és többhelyes konfigurációban hely/alhálózatot / IP-cím lesz.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-460">IP Address Resource – This is hello IP address that associated with hello VNN, you can have more than one, and in a multisite configuration you will have an IP address per site/subnet.</span></span>

<span data-ttu-id="6ce9c-461">Amikor csatlakozó tooSQL Server, SQL Server ügyfél-illesztőprogram hello lekéri hello figyelő társított hello DNS-rekordokat, és próbálja tooconnect tooeach mindig a hozzárendelt IP-cím, alább néhány befolyásoló tényezők is ez tárgyaljuk.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-461">When connecting tooSQL Server, hello SQL Server Client driver will retrieve hello DNS records associated with hello listener and try tooconnect tooeach Always On associated IP address, below we discuss some factors that can influence this.</span></span>

<span data-ttu-id="6ce9c-462">hello hello figyelőjének nevével társított párhuzamos DNS-rekordok száma attól függ, nem csak hello társított IP-címek száma, de hello "RegisterAllIpProviders'setting a Feladatátvételi fürtszolgáltatás hello Always ON VNN erőforrás számára.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-462">hello number of concurrent DNS records that are associated with hello listener name depends not only on hello number of IP addresses associated, but hello ‘RegisterAllIpProviders’setting in Failover Clustering for hello Always ON VNN resource.</span></span>

<span data-ttu-id="6ce9c-463">Always On Azure telepítésekor más lépéseket toocreate hello figyelő és IP-címek, hello "RegisterAllIpProviders" too1 konfigurálása toomanually rendelkezik, ez különböző tooan helyi mindig a központi már állítottak too1.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-463">When you deploy Always On in Azure there are different steps toocreate hello Listener and IP Addresses, you have toomanually configure hello ‘RegisterAllIpProviders’ too1, this is different tooan on-premises Always On deployment where it is already set too1.</span></span>

<span data-ttu-id="6ce9c-464">Ha a "RegisterAllIpProviders" értéke 0, majd csak akkor jelenik meg egy DNS-rekordot a DNS-ben társított hello figyelő:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-464">If ‘RegisterAllIpProviders’ is 0, then you will only see one DNS record in DNS associated with hello Listener:</span></span>

![Appendix4][14]

<span data-ttu-id="6ce9c-466">Ha 'RegisterAllIpProviders' 1:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-466">If ‘RegisterAllIpProviders’ is 1:</span></span>

![Appendix5][15]

<span data-ttu-id="6ce9c-468">hello kódot fog kimenő hello dump VNN beállításait, és állítsa be az Ön, adjon megjegyzés, hello tootake hello VNN offline állapotba, és kapcsolja hálózatra újra kell tootake effektus módosítása, ez véve hello figyelő offline okozó ügyfél kapcsolat megszűnésének.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-468">hello code below will dump out hello VNN settings and set it for you, please note, for hello change tootake effect you will need tootake hello VNN offline and turn it back online, this taking hello Listener offline causing client connectivity disruption.</span></span>

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

<span data-ttu-id="6ce9c-469">Egy későbbi lépésben áttelepítési tooupdate hello mindig a figyelő frissített IP-címet, amely a használatával hivatkozik a terheléselosztó kell, ez magában foglalja az IP-cím erőforrás eltávolítása és hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-469">In a later migration step you will need tooupdate hello Always On listener with an updated IP address that will reference a load balancer, this will involve an IP Address resource removal and addition.</span></span> <span data-ttu-id="6ce9c-470">Hello IP frissítés után kell tooensure hello új IP-címet a DNS-zóna frissítették, és hogy hello ügyfelek frissítése a helyi gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-470">After hello IP update, you need tooensure hello new IP address has been updated in DNS Zone and that hello clients are updating their local DNS cache.</span></span>

<span data-ttu-id="6ce9c-471">Ha az ügyfelek egy másik hálózati szegmensben találhatók, és egy másik DNS-kiszolgáló hivatkozik, mi történik, DNS-zóna átviteli kapcsolatos hello az áttelepítés során, hello alkalmazás újracsatlakozás idő fog korlátozott tooconsider kell által legalább hello zóna átviteli idő bármely új IP-címek hello figyelője.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-471">If your clients reside in a different network segment and reference a different DNS server, you need tooconsider what happens about DNS Zone Transfer during hello migration, as hello application reconnect time will be constrained by at least hello Zone Transfer Time of any new IP addresses for hello listener.</span></span> <span data-ttu-id="6ce9c-472">Ha itt időkorlát alatt, érdemes tárgyalja, és a Windows-csoportok egy növekményes zónaletöltés kényszerítése, és helyezheti is hello DNS állomás rekord tooa alacsonyabb idő tooLive (TTL), így hello ügyfelek frissítése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-472">If you are under time constraint here, you should discuss and test forcing an incremental zone transfer with your Windows teams, and also put hello DNS host record tooa lower Time tooLive (TTL), so hello clients update.</span></span> <span data-ttu-id="6ce9c-473">További információkért lásd: [növekményes zónaátvitelek](https://technet.microsoft.com/library/cc958973.aspx) és [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-473">For more information, see [Incremental Zone Transfers](https://technet.microsoft.com/library/cc958973.aspx) and [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).</span></span>

<span data-ttu-id="6ce9c-474">Alapértelmezett hello TTL-t, a DNS-rekord társított hello mindig az Azure-ban a figyelő az 1200-as másodperc.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-474">By default hello TTL for DNS Record that is associated with hello Listener in Always On in Azure is 1200 seconds.</span></span> <span data-ttu-id="6ce9c-475">Kezdésként érdemes lehet tooreduce ez Ha frissítésekor az áttelepítési tooensure hello ügyfelek hello figyelő DNS-frissített hello IP-címmel vannak időkorlát alatt.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-475">You may wish tooreduce this if you are under time constraint during your migration tooensure hello clients update their DNS with hello updated IP address for hello listener.</span></span> <span data-ttu-id="6ce9c-476">Megtekintheti és hello konfigurációjának módosítása okai kimenő hello VNN hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-476">You can see and modify hello configuration by dumping out hello configuration of hello VNN:</span></span>

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

<span data-ttu-id="6ce9c-477">Vegye figyelembe, hello alacsonyabb hello "HostRecordTTL", a DNS-forgalom nagyobb mennyiségű történik.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-477">Please note, hello lower hello ‘HostRecordTTL’, a higher amount of DNS traffic will occur.</span></span>

##### <a name="client-application-settings"></a><span data-ttu-id="6ce9c-478">Ügyfél-Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="6ce9c-478">Client application settings</span></span>
<span data-ttu-id="6ce9c-479">Ha az SQL client alkalmazás támogatja a .net 4.5 hello SQLClient, akkor használhatja "MULTISUBNETFAILOVER = TRUE" kulcsszót, az ajánlott toobe szerint lehetővé teszi a gyorsabb kapcsolat tooSQL Always On rendelkezésre állási csoportnak a feladatátvétel során.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-479">If your SQL client application supports hello .Net 4.5 SQLClient, then you can use ‘MULTISUBNETFAILOVER=TRUE’ keyword, this is recommended toobe applied as it allows for faster connection tooSQL Always On Availability Group during failover.</span></span> <span data-ttu-id="6ce9c-480">Hello mindig a figyelő párhuzamosan társított összes IP-címek keresztül enumerálása, és átadta szigorúbb TCP-kapcsolat újrapróbálkozási sebességét.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-480">It enumerates through all IP addresses associated with hello Always On listener in parallel, and performs a more aggressive TCP connection retry speed during a failover.</span></span>

<span data-ttu-id="6ce9c-481">A fenti hello-beállítások kapcsolatos további információkért lásd: [MultiSubnetFailover kulcsszó és a kapcsolódó szolgáltatások](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-481">For more information regarding hello settings above, please see [MultiSubnetFailover Keyword and Associated Features](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span></span> <span data-ttu-id="6ce9c-482">Lásd még: [SqlClient támogatja a magas rendelkezésre állási, vészhelyreállítási](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-482">Also see [SqlClient Support for High Availability, Disaster Recovery](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).</span></span>

#### <a name="step-5-cluster-quorum-settings"></a><span data-ttu-id="6ce9c-483">5. lépés: Fürt kvórumbeállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-483">Step 5: Cluster quorum settings</span></span>
<span data-ttu-id="6ce9c-484">Mivel legalább egy SQL Server le egyszerre véve toobe fog, módosítsa hello fürt kvórumbeállításokat, ha 2 csomópontok fájl megosztási tanúsító (FSW) használ, meg kell hello kvórum tooallow csomóponttöbbség beállítása és felhasználását dinamikus szavazás, és ezek a tooallow egy egycsomópontos tooremain állandó.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-484">As you are going toobe taking out at least one SQL Server down at a time, you should modify hello cluster quorum setting, if using File Share Witness (FSW) with 2 nodes, you should set hello quorum tooallow node majority and utilize dynamic voting, and this is tooallow for a single node tooremain standing.</span></span>

    Set-ClusterQuorum -NodeMajority  

<span data-ttu-id="6ce9c-485">További információ a kezelése és hello a fürtkvórum konfigurálása, lásd: [konfigurálása és kezelése a Windows Server 2012 feladatátvevő fürt kvórum hello](https://technet.microsoft.com/library/jj612870.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ce9c-485">For more information on managing and configuring hello cluster quorum, please see [Configure and Manage hello Quorum in a Windows Server 2012 Failover Cluster](https://technet.microsoft.com/library/jj612870.aspx).</span></span>

#### <a name="step-6-extract-existing-endpoints-and-acls"></a><span data-ttu-id="6ce9c-486">6. lépés: Bontsa ki a meglévő végpontok és hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="6ce9c-486">Step 6: Extract Existing Endpoints and ACLs</span></span>
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for hello Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

<span data-ttu-id="6ce9c-487">Ezek tooa szöveges fájlt mentse.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-487">Save these tooa text file.</span></span>

#### <a name="step-7-change-failover-partners-and-replication-modes"></a><span data-ttu-id="6ce9c-488">7. lépés: Feladatátvételi partnerként és replikációs mód módosítása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-488">Step 7: Change Failover Partners and Replication Modes</span></span>
<span data-ttu-id="6ce9c-489">Ha 2-nél több SQL-kiszolgálóval rendelkezik, meg kell változtatni a másik tartományvezérlő egy másik másodlagos hello feladatátvételi vagy helyszíni too'Synchronous', és teszi az automatikus feladatátvételi Partner (AFP), ez a helyzet magas rendelkezésre ÁLLÁSÚ karbantartása, míg a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-489">If you have more than 2 SQL Servers, you should change hello failover of another secondary in another DC or on-premises too‘Synchronous’ and make it an Automatic Failover Partner (AFP), this is so you maintain HA whilst you are making changes.</span></span> <span data-ttu-id="6ce9c-490">Ezt megteheti a TSQL használatával is módosíthatja, ha a szolgáltatáshoz az SSMS:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-490">You can do this through TSQL of modify though SSMS:</span></span>

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a><span data-ttu-id="6ce9c-492">8. lépés: Másodlagos virtuális gép eltávolítása a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6ce9c-492">Step 8: Remove Secondary VM from cloud service</span></span>
<span data-ttu-id="6ce9c-493">Kell egy felhőalapú másodlagos csomópont toomigrate tervezési először, ha jelenleg az elsődleges, akkor kell kezdeményezni kézi feladatátvételre.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-493">You should be planning toomigrate a cloud secondary node first, if this is currently primary, you should initiate a manual failover.</span></span>

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

    #Check disk config, make sure below returns hello disks associated with hello VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="6ce9c-494">9. lépés: Lemez gyorsítótárazási beállítások a CSV-fájlban, és mentse</span><span class="sxs-lookup"><span data-stu-id="6ce9c-494">Step 9: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="6ce9c-495">A adatkötetek ezek tooREADONLY kell állítani.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-495">For data volumes these should be set tooREADONLY.</span></span>

<span data-ttu-id="6ce9c-496">TLOG kötetek ezek tooNONE kell állítani.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-496">For TLOG volumes these should be set tooNONE.</span></span>

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a><span data-ttu-id="6ce9c-498">10. lépés: Másolat VHD-k</span><span class="sxs-lookup"><span data-stu-id="6ce9c-498">Step 10: Copy VHDS</span></span>
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
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



<span data-ttu-id="6ce9c-499">Hello példány állapotának hello VHD-k toohello prémium szintű Storage-fiók ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-499">You can check hello copy status of hello VHDs toohello Premium Storage account:</span></span>

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

<span data-ttu-id="6ce9c-501">Várjon, amíg a fenti sikeres fájl rögzíti.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-501">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="6ce9c-502">Az egyes blobok információt:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-502">For information for individual blobs:</span></span>

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a><span data-ttu-id="6ce9c-503">11. lépés: az operációs rendszer Register lemez</span><span class="sxs-lookup"><span data-stu-id="6ce9c-503">Step 11: Register OS disk</span></span>
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

#### <a name="step-12-import-secondary-into-new-cloud-service"></a><span data-ttu-id="6ce9c-504">12. lépés: Importálása másodlagos új felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6ce9c-504">Step 12: Import secondary into new cloud service</span></span>
<span data-ttu-id="6ce9c-505">hello kódot is használ hello hozzá alább beállítást itt meg lehet hello gép importálni és használni hello retainable VIP.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-505">hello code below also uses hello added option here you can import hello machine and use hello retainable VIP.</span></span>

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember toochange tooXIO
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

    ###Attaching disks tooa VM during a deploy tooa new cloud service and new storage account is different from just attaching VHDs toojust a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="6ce9c-506">13. lépés: Hozható létre ILB új felhő Svc, vegye fel a terhelés eloszlik a végpontok és hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="6ce9c-506">Step 13: Create ILB on New Cloud Svc, Add Load Balanced Endpoints and ACLs</span></span>
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

#### <a name="step-14-update-always-on"></a><span data-ttu-id="6ce9c-507">14. lépés: Mindig a frissítése.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-507">Step 14: Update Always On</span></span>
    #Code toobe executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # hello azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency tooListener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

<span data-ttu-id="6ce9c-509">Hello régi felhőalapú szolgáltatás IP-cím eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-509">Now remove hello old cloud service IP Address.</span></span>

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a><span data-ttu-id="6ce9c-511">15. lépés: A DNS-frissítési ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6ce9c-511">Step 15: DNS update check</span></span>
<span data-ttu-id="6ce9c-512">Most DNS-kiszolgálók ellenőrizze az SQL Server ügyfél hálózatokon és győződjön meg arról, hogy a fürtszolgáltatás hozzáadta hello hello extra állomásrekordot hozzáadott IP-címet.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-512">You should now check DNS Servers on your SQL Server client networks and make sure that clustering has added hello extra host record for hello added IP address.</span></span> <span data-ttu-id="6ce9c-513">Ha ezeket a DNS-kiszolgálók nem frissítették, fontolja meg a DNS zónaletöltés kényszerítése, és győződjön meg arról, hogy hello ott alhálózaton lévő ügyfelek képesek tooresolve tooboth mindig az IP-címek, ez így nem kell toowait a automatikus DNS-replikáció az.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-513">If those DNS servers have not updated, consider forcing a DNS Zone transfer and ensure that hello clients in there subnet are able tooresolve tooboth Always On IP Addresses, this is so you do not need toowait for automatic DNS replication.</span></span>

#### <a name="step-16-reconfigure-always-on"></a><span data-ttu-id="6ce9c-514">16. lépés: Mindig a újrakonfigurálása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-514">Step 16: Reconfigure Always On</span></span>
<span data-ttu-id="6ce9c-515">Ezen a ponton, de a áttelepített toofully csomóponton újraszinkronizálása hello helyi csomóponthoz, és toosynchronous replikációs csomópont váltson, és könnyebben hello AFP másodlagos hello várja.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-515">At this point you wait for hello secondary that node that was migrated toofully resynchronize with hello on-premises node and switch toosynchronous replication node and make it hello AFP.</span></span>  

#### <a name="step-17-migrate-second-node"></a><span data-ttu-id="6ce9c-516">17. lépés: A második csomópont áttelepítése</span><span class="sxs-lookup"><span data-stu-id="6ce9c-516">Step 17: Migrate second node</span></span>
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

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a><span data-ttu-id="6ce9c-517">18. lépés: Lemez gyorsítótárazási beállítások a CSV-fájlban, és mentse</span><span class="sxs-lookup"><span data-stu-id="6ce9c-517">Step 18: Change disk caching settings in CSV file and save</span></span>
<span data-ttu-id="6ce9c-518">A adatkötetek ezek tooREADONLY kell állítani.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-518">For data volumes these should be set tooREADONLY.</span></span>

<span data-ttu-id="6ce9c-519">TLOG kötetek ezek tooNONE kell állítani.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-519">For TLOG volumes these should be set tooNONE.</span></span>

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a><span data-ttu-id="6ce9c-521">19. lépés: A másodlagos csomópont új független Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-521">Step 19: Create New Independent Storage Account for Secondary Node</span></span>
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset hello storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a><span data-ttu-id="6ce9c-522">20. lépés: Másolat VHD-k</span><span class="sxs-lookup"><span data-stu-id="6ce9c-522">Step 20: Copy VHDS</span></span>
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
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


<span data-ttu-id="6ce9c-523">Hello VHD-másolat állapota ellenőrizze, hogy minden virtuális merevlemez: ForEach (a $diskobjects $disk) {$lun = $disk. LUN $vhdname $disk.vhdname $cacheoption = = $disk. HostCaching $disklabel = $disk. Lemezcímke $diskName = $disk. DiskName</span><span class="sxs-lookup"><span data-stu-id="6ce9c-523">You can check hello VHD copy status for all VHDs: ForEach ($disk in $diskobjects) { $lun = $disk.Lun $vhdname = $disk.vhdname $cacheoption = $disk.HostCaching $disklabel = $disk.DiskLabel $diskName = $disk.DiskName</span></span>

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

<span data-ttu-id="6ce9c-525">Várjon, amíg a fenti sikeres fájl rögzíti.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-525">Wait until all these are recorded as success.</span></span>

<span data-ttu-id="6ce9c-526">Az egyes blobok információt:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-526">For information for individual blobs:</span></span>

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a><span data-ttu-id="6ce9c-527">21. lépés: az operációs rendszer Register lemez</span><span class="sxs-lookup"><span data-stu-id="6ce9c-527">Step 21: Register OS disk</span></span>
    #change storage account toohello new XIO storage account
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

    #Join tooexisting Avaiability Set

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

    ###This is different toojust a straight cloud service change
    #note if you do not have a disk label hello command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a><span data-ttu-id="6ce9c-528">22. lépés: Adja hozzá a terhelés eloszlik a végpontok és hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="6ce9c-528">Step 22: Add Load Balanced Endpoints and ACLs</span></span>
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in hello Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a><span data-ttu-id="6ce9c-529">23. lépés: Feladatátvételi teszt</span><span class="sxs-lookup"><span data-stu-id="6ce9c-529">Step 23: Test failover</span></span>
<span data-ttu-id="6ce9c-530">Ha hagyja hello áttelepített csomópont szinkronizálja a helyszíni hello mindig a csomóponttal, helyezze toosynchronous replikációs mód, és várjon, amíg azt szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-530">You should now let hello migrated node synchronize with hello on-premises Always On node, place it in toosynchronous replication mode and wait until it is synchronized.</span></span> <span data-ttu-id="6ce9c-531">Ezután feladatátvétel helyszíni toohello első csomópontot az áttelepíti, amely hello AFP.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-531">Then failover from on-prem toohello first node migrated, which is hello AFP.</span></span> <span data-ttu-id="6ce9c-532">Amennyiben rendelkezik működőképes, módosítás hello legutóbbi áttelepítésük óta csomópont toohello AFP.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-532">Once that has worked, change hello last migrated node toohello AFP.</span></span>

<span data-ttu-id="6ce9c-533">Feladatátvételi tesztek összes csomópontok között kell, és bár tooensure feladatátvételek működhet chaos tesztek várt, egy időben manor futnak.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-533">You should test failovers between all nodes and run though chaos tests tooensure failovers work as expected and in a timely manor.</span></span>

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a><span data-ttu-id="6ce9c-534">24. lépés: Helyezze vissza fürt kvórumbeállításainak megadása / DNS-élettartam / feladatátvételi Pntrs / szinkronizálási beállítások</span><span class="sxs-lookup"><span data-stu-id="6ce9c-534">Step 24: Put back cluster quorum settings / DNS TTL / Failover Pntrs / Sync Settings</span></span>
##### <a name="adding-ip-address-resource-on-same-subnet"></a><span data-ttu-id="6ce9c-535">Azonos alhálózatban lévő IP-cím erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6ce9c-535">Adding IP Address Resource on Same Subnet</span></span>
<span data-ttu-id="6ce9c-536">Ha csak 2 SQL-kiszolgálókhoz, és toomigrate szeretné őket tooa új felhőalapú szolgáltatás, de nem szeretnének tookeep azokat a hello ugyanazon az alhálózaton, elkerülheti a mindig offline toodelete hello eredeti hello figyelő véve az IP-címet, és adja hozzá az új IP-cím hello.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-536">If you have only 2 SQL Servers and want toomigrate them tooa new cloud service, but want tookeep them on hello same subnet, you can avoid taking hello listener offline toodelete hello original Always On IP Address and add hello New IP Address.</span></span> <span data-ttu-id="6ce9c-537">Ha az áttelepítés hello virtuális gépek tooanother alhálózati nincs szükség van egy toodo ez nem lesznek egy fürt további hálózati alhálózaton fog hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-537">If you are migrating hello VMs tooanother subnet you will not need toodo this as there will be an additional cluster network that will reference that subnet.</span></span>

<span data-ttu-id="6ce9c-538">Miután léptetik hello másodlagos át, és előtt feladatátvételi hello meglévő elsődleges hozzáadott új IP-cím erőforrás hello hello új felhőalapú szolgáltatás, ezeket a lépéseket a fürt Feladatátvevőfürt-kezelő hello belül kell végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-538">Once you have brought up hello migrated secondary and added in hello new IP Address resource for hello new cloud service before failover hello existing Primary, you should take these steps within hello Cluster Failover Manager:</span></span>

<span data-ttu-id="6ce9c-539">az IP-cím tooadd lásd: hello [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), 14 lépésben.</span><span class="sxs-lookup"><span data-stu-id="6ce9c-539">tooadd in IP Address, see hello [Appendix](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), step 14.</span></span>

1. <span data-ttu-id="6ce9c-540">Az aktuális IP-cím erőforrás hello, módosítsa a hello lehetséges tulajdonos too'Existing elsődleges SQL Server ", az alábbi"dansqlams4"hello példa:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-540">For hello current IP Address resource, change hello possible owner too‘Existing Primary SQL Server’, in hello example below, ‘dansqlams4’:</span></span>

    ![Appendix13][23]
2. <span data-ttu-id="6ce9c-542">Hello új IP-cím erőforráson, módosítsa a hello lehetséges tulajdonos too'Migrated másodlagos SQL-kiszolgáló ", az alábbi"dansqlams5"hello példa:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-542">For hello new IP Address resource, change hello possible owner too‘Migrated secondary SQL Server’, in hello example below, ‘dansqlams5’:</span></span>

    ![Appendix14][24]
3. <span data-ttu-id="6ce9c-544">Miután ezt állítja be a következőket teheti feladatátvevő, és hello utolsó csomópontja áttelepítésekor hello lehetséges tulajdonosok módosítani kell úgy, hogy a csomópont lehetséges tulajdonosa meg van adva:</span><span class="sxs-lookup"><span data-stu-id="6ce9c-544">Once this is set you can failover, and when hello last node is migrated hello Possible Owners should be edited so that node is added as a Possible Owner:</span></span>

    ![Appendix15][25]

## <a name="additional-resources"></a><span data-ttu-id="6ce9c-546">További források</span><span class="sxs-lookup"><span data-stu-id="6ce9c-546">Additional resources</span></span>
* [<span data-ttu-id="6ce9c-547">Prémium szintű Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6ce9c-547">Azure Premium Storage</span></span>](../../../storage/common/storage-premium-storage.md)
* [<span data-ttu-id="6ce9c-548">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="6ce9c-548">Virtual Machines</span></span>](https://azure.microsoft.com/services/virtual-machines/)
* [<span data-ttu-id="6ce9c-549">SQL Server Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="6ce9c-549">SQL Server in Azure Virtual Machines</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

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
