---
title: "aaaConfigure hello Always On rendelkezésre állási csoport egy Azure virtuális gépen a PowerShell használatával |} Microsoft Docs"
description: "Ez az oktatóanyag hello klasszikus üzembe helyezési modellel létrehozott erőforrások használja. Az Azure PowerShell toocreate Always On rendelkezésre állási csoport használja."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: d4a27e203b2ff299adebec2b010c03422459b3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-always-on-availability-group-on-an-azure-vm-with-powershell"></a><span data-ttu-id="9d87c-104">Hello Always On rendelkezésre állási csoport konfigurálásához egy Azure virtuális gépen a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9d87c-104">Configure hello Always On availability group on an Azure VM with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9d87c-105">Klasszikus: felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="9d87c-105">Classic: UI</span></span>](../classic/portal-sql-alwayson-availability-groups.md)
> * <span data-ttu-id="9d87c-106">[Klasszikus: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span><span class="sxs-lookup"><span data-stu-id="9d87c-106">[Classic: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
</span></span><br/>

<span data-ttu-id="9d87c-107">Mielőtt elkezdené, vegye figyelembe, hogy most hajthatja végre ezt a feladatot az Azure resource manager modellt.</span><span class="sxs-lookup"><span data-stu-id="9d87c-107">Before you begin, consider that you can now complete this task in Azure resource manager model.</span></span> <span data-ttu-id="9d87c-108">Azt javasoljuk, hogy az Azure resource manager modellt új központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9d87c-108">We recommend Azure resource manager model for new deployments.</span></span> <span data-ttu-id="9d87c-109">Lásd: [SQL Server Always On rendelkezésre állási csoportok az Azure virtuális gépeken](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9d87c-109">See [SQL Server Always On availability groups on Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9d87c-110">Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="9d87c-110">We recommend that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="9d87c-111">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9d87c-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9d87c-112">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="9d87c-112">This article covers using hello classic deployment model.</span></span>

<span data-ttu-id="9d87c-113">Az Azure virtuális gépek (VM) segítségével adatbázis rendszergazdák toolower hello költség magas rendelkezésre állású SQL Server rendszert.</span><span class="sxs-lookup"><span data-stu-id="9d87c-113">Azure virtual machines (VMs) can help database administrators toolower hello cost of a high-availability SQL Server system.</span></span> <span data-ttu-id="9d87c-114">Az oktatóanyag bemutatja, hogyan tooimplement rendelkezésre állási csoporthoz az SQL Server Always On-végpontok belül környezetet az Azure használatával.</span><span class="sxs-lookup"><span data-stu-id="9d87c-114">This tutorial shows you how tooimplement an availability group by using SQL Server Always On end-to-end inside an Azure environment.</span></span> <span data-ttu-id="9d87c-115">Hello az oktatóanyag végén hello az SQL Server Always On megoldás az Azure-ban a következő elemek hello áll:</span><span class="sxs-lookup"><span data-stu-id="9d87c-115">At hello end of hello tutorial, your SQL Server Always On solution in Azure will consist of hello following elements:</span></span>

* <span data-ttu-id="9d87c-116">Virtuális hálózat, amely tartalmazza a több alhálózattal, beleértve egy előtér- és háttér-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="9d87c-116">A virtual network that contains multiple subnets, including a front-end and a back-end subnet.</span></span>
* <span data-ttu-id="9d87c-117">Az Active Directory-tartomány a tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="9d87c-117">A domain controller with an Active Directory domain.</span></span>
* <span data-ttu-id="9d87c-118">Két SQL Server virtuális gépen telepített toohello háttér-alhálózathoz és Active Directory-tartományhoz csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-118">Two SQL Server VMs that are deployed toohello back-end subnet and joined toohello Active Directory domain.</span></span>
* <span data-ttu-id="9d87c-119">Egy három csomópontos Windows feladatátvevő fürt hello csomóponttöbbség kvórum modell.</span><span class="sxs-lookup"><span data-stu-id="9d87c-119">A three-node Windows failover cluster with hello Node Majority quorum model.</span></span>
* <span data-ttu-id="9d87c-120">Egy rendelkezésre állási csoportban két szinkron véglegesítésű másodpéldányt a rendelkezésre állási adatbázis.</span><span class="sxs-lookup"><span data-stu-id="9d87c-120">An availability group with two synchronous-commit replicas of an availability database.</span></span>

<span data-ttu-id="9d87c-121">Ebben a forgatókönyvben a jó választás az egyszerűség érdekében az Azure-on, nem pedig a költséghatékonyság vagy egyéb tényezők.</span><span class="sxs-lookup"><span data-stu-id="9d87c-121">This scenario is a good choice for its simplicity on Azure, not for its cost-effectiveness or other factors.</span></span> <span data-ttu-id="9d87c-122">Például minimalizálhatja hello a két-replika rendelkezésre állási csoport toosave a virtuális gépek száma a számítási órák az Azure-ban hello tartományvezérlő hello kvórum tanúsító fájlmegosztást egy két csomópontos feladatátvevő fürt segítségével.</span><span class="sxs-lookup"><span data-stu-id="9d87c-122">For example, you can minimize hello number of VMs for a two-replica availability group toosave on compute hours in Azure by using hello domain controller as hello quorum file share witness in a two-node failover cluster.</span></span> <span data-ttu-id="9d87c-123">Ez a módszer egy, a fenti konfigurációs hello csökkenti a hello virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="9d87c-123">This method reduces hello VM count by one from hello above configuration.</span></span>

<span data-ttu-id="9d87c-124">Ez az oktatóanyag célja, akkor hello lépéseket, amelyek be hello szükséges tooset tooshow megoldást a fenti leírt hello részletei minden egyes lépést kidolgozása nélkül.</span><span class="sxs-lookup"><span data-stu-id="9d87c-124">This tutorial is intended tooshow you hello steps that are required tooset up hello described solution above, without elaborating on hello details of each step.</span></span> <span data-ttu-id="9d87c-125">Ezért ahelyett, hogy nyújtó hello grafikus felhasználói Felülettel konfigurációs lépések, használja a PowerShell parancsfájl-kezelési tootake meg gyorsan a minden egyes lépést.</span><span class="sxs-lookup"><span data-stu-id="9d87c-125">Therefore, instead of providing hello GUI configuration steps, it uses PowerShell scripting tootake you quickly through each step.</span></span> <span data-ttu-id="9d87c-126">Ez az oktatóanyag azt feltételezi, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9d87c-126">This tutorial assumes hello following:</span></span>

* <span data-ttu-id="9d87c-127">Már van Azure-fiókot hello virtuálisgép-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="9d87c-127">You already have an Azure account with hello virtual machine subscription.</span></span>
* <span data-ttu-id="9d87c-128">Hello telepítése [Azure PowerShell-parancsmagok](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9d87c-128">You've installed hello [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>
* <span data-ttu-id="9d87c-129">Már van egy Always On rendelkezésre állási csoportok a helyszíni megoldások alapos ismerete.</span><span class="sxs-lookup"><span data-stu-id="9d87c-129">You already have a solid understanding of Always On availability groups for on-premises solutions.</span></span> <span data-ttu-id="9d87c-130">További információkért lásd: [Always On rendelkezésre állási csoportok (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d87c-130">For more information, see [Always On availability groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).</span></span>

## <a name="connect-tooyour-azure-subscription-and-create-hello-virtual-network"></a><span data-ttu-id="9d87c-131">Csatlakozás Azure-előfizetés tooyour és hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d87c-131">Connect tooyour Azure subscription and create hello virtual network</span></span>
1. <span data-ttu-id="9d87c-132">Egy PowerShell-ablakban a helyi számítógépen hello Azure modul importálása, töltse le a közzétételi beállítások fájl tooyour gép hello és hello letöltött közzétételi beállítások importálása a PowerShell-munkamenet tooyour Azure-előfizetés kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="9d87c-132">In a PowerShell window on your local computer, import hello Azure module, download hello publishing settings file tooyour machine, and connect your PowerShell session tooyour Azure subscription by importing hello downloaded publishing settings.</span></span>

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    <span data-ttu-id="9d87c-133">Hello **Get-AzurePublishSettingsFile** parancs automatikusan az Azure felügyeleti tanúsítványt hoz létre, és letölti, tooyour gép.</span><span class="sxs-lookup"><span data-stu-id="9d87c-133">hello **Get-AzurePublishSettingsFile** command automatically generates a management certificate with Azure and downloads it tooyour machine.</span></span> <span data-ttu-id="9d87c-134">A böngésző automatikusan megnyílik, és felszólító tooenter hello Microsoft-fiók hitelesítő adataival Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="9d87c-134">A browser is automatically opened, and you're prompted tooenter hello Microsoft account credentials for your Azure subscription.</span></span> <span data-ttu-id="9d87c-135">letöltött hello **.publishsettings** fájl tartalmazza az összes hello szükséges információk toomanage az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="9d87c-135">hello downloaded **.publishsettings** file contains all hello information that you need toomanage your Azure subscription.</span></span> <span data-ttu-id="9d87c-136">A fájl tooa helyi könyvtár mentése, után importálnia hello **Import-AzurePublishSettingsFile** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9d87c-136">After saving this file tooa local directory, import it by using hello **Import-AzurePublishSettingsFile** command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9d87c-137">hello .publishsettings fájlt tartalmaz a hitelesítő adatokat (kódolatlan) használt tooadminister, az Azure-előfizetések és -szolgáltatásokra.</span><span class="sxs-lookup"><span data-stu-id="9d87c-137">hello .publishsettings file contains your credentials (unencoded) that are used tooadminister your Azure subscriptions and services.</span></span> <span data-ttu-id="9d87c-138">hello biztonsági szempontból ajánlott a fájl toostore azt ideiglenesen kívül a forrás-könyvtárak (például a hello Libraries\Documents mappában), majd törölje hello importálása után.</span><span class="sxs-lookup"><span data-stu-id="9d87c-138">hello security best practice for this file is toostore it temporarily outside your source directories (for example, in hello Libraries\Documents folder), and then delete it after hello import has finished.</span></span> <span data-ttu-id="9d87c-139">Egy rosszindulatú felhasználó kap hozzáférést toohello .publishsettings fájlt szerkesztése, létrehozása és törlése az Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="9d87c-139">A malicious user who gains access toohello .publishsettings file can edit, create, and delete your Azure services.</span></span>

2. <span data-ttu-id="9d87c-140">Adja meg, hogy azt ismertetjük toocreate a felhőalapú informatikai infrastruktúra változókat.</span><span class="sxs-lookup"><span data-stu-id="9d87c-140">Define a series of variables that you'll use toocreate your cloud IT infrastructure.</span></span>

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    <span data-ttu-id="9d87c-141">Nagy figyelmet toohello tooensure, hogy a parancsok később lesz sikeres, a következő:</span><span class="sxs-lookup"><span data-stu-id="9d87c-141">Pay attention toohello following tooensure that your commands will succeed later:</span></span>

   * <span data-ttu-id="9d87c-142">Változók **$storageAccountName** és **$dcServiceName** egyedinek kell lennie, mert használt tooidentify a felhőalapú társzolgáltatás fiókja és a felhő server, a hello Internet.</span><span class="sxs-lookup"><span data-stu-id="9d87c-142">Variables **$storageAccountName** and **$dcServiceName** must be unique because they're used tooidentify your cloud storage account and cloud server, respectively, on hello Internet.</span></span>
   * <span data-ttu-id="9d87c-143">nevek, amennyit megadott a változók hello **$affinityGroupName** és **$virtualNetworkName** hello virtuális hálózati konfiguráció dokumentum később fogjuk vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="9d87c-143">hello names that you specify for variables **$affinityGroupName** and **$virtualNetworkName** are configured in hello virtual network configuration document that you'll use later.</span></span>
   * <span data-ttu-id="9d87c-144">**$sqlImageName** , amely tartalmazza az SQL Server 2012 Service Pack 1 Enterprise Edition hello Virtuálisgép-lemezkép frissítése hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="9d87c-144">**$sqlImageName** specifies hello updated name of hello VM image that contains SQL Server 2012 Service Pack 1 Enterprise Edition.</span></span>
   * <span data-ttu-id="9d87c-145">Az egyszerűség kedvéért **Contoso! 000** azonos hello hello teljes oktatóprogram során használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="9d87c-145">For simplicity, **Contoso!000** is hello same password that's used throughout hello entire tutorial.</span></span>

3. <span data-ttu-id="9d87c-146">Affinitáscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9d87c-146">Create an affinity group.</span></span>

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. <span data-ttu-id="9d87c-147">Virtuális hálózat létrehozása a konfigurációs fájl importálásával.</span><span class="sxs-lookup"><span data-stu-id="9d87c-147">Create a virtual network by importing a configuration file.</span></span>

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    <span data-ttu-id="9d87c-148">hello konfigurációs fájl tartalmazza a következő XML-dokumentum hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-148">hello configuration file contains hello following XML document.</span></span> <span data-ttu-id="9d87c-149">Röviden, adja meg a virtuális hálózat néven **ContosoNET** nevű hello affinitáscsoportban **ContosoAG**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-149">In brief, it specifies a virtual network called **ContosoNET** in hello affinity group called **ContosoAG**.</span></span> <span data-ttu-id="9d87c-150">Hello címterület rendelkezik **10.10.0.0/16** és két alhálózat **10.10.1.0/24** és **10.10.2.0/24**, amelyeket hello első alhálózat és hátsó alhálózati, illetve.</span><span class="sxs-lookup"><span data-stu-id="9d87c-150">It has hello address space **10.10.0.0/16** and has two subnets, **10.10.1.0/24** and **10.10.2.0/24**, which are hello front subnet and back subnet, respectively.</span></span> <span data-ttu-id="9d87c-151">hello első alhálózat akkor helyezheti el ügyfélalkalmazásokat, például a Microsoft SharePoint.</span><span class="sxs-lookup"><span data-stu-id="9d87c-151">hello front subnet is where you can place client applications such as Microsoft SharePoint.</span></span> <span data-ttu-id="9d87c-152">hello hátsó alhálózat, ahol helyének hello SQL Server virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9d87c-152">hello back subnet is where you'll place hello SQL Server VMs.</span></span> <span data-ttu-id="9d87c-153">Ha módosítja a hello **$affinityGroupName** és **$virtualNetworkName** változók korábban is módosítania kell az alábbi neveket megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-153">If you change hello **$affinityGroupName** and **$virtualNetworkName** variables earlier, you must also change hello corresponding names below.</span></span>

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. <span data-ttu-id="9d87c-154">Hozzon létre egy tárfiókot, hello affinitáscsoport hozott létre, és állítsa be aktuális tárfiók hello az előfizetéshez társított.</span><span class="sxs-lookup"><span data-stu-id="9d87c-154">Create a storage account that's associated with hello affinity group that you created, and set it as hello current storage account in your subscription.</span></span>

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. <span data-ttu-id="9d87c-155">Hello új felhőalapú szolgáltatás és a rendelkezésre állási készlet létrehozása a hello tartományvezérlő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9d87c-155">Create hello domain controller server in hello new cloud service and availability set.</span></span>

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    <span data-ttu-id="9d87c-156">Ezek a parancsok védőeszközön dolgok következő hello:</span><span class="sxs-lookup"><span data-stu-id="9d87c-156">These piped commands do hello following things:</span></span>

   * <span data-ttu-id="9d87c-157">**Új AzureVMConfig** hoz létre a Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9d87c-157">**New-AzureVMConfig** creates a VM configuration.</span></span>
   * <span data-ttu-id="9d87c-158">**Adja hozzá AzureProvisioningConfig** által biztosított hello egy különálló Windows Server konfigurációs paramétereket.</span><span class="sxs-lookup"><span data-stu-id="9d87c-158">**Add-AzureProvisioningConfig** gives hello configuration parameters of a standalone Windows server.</span></span>
   * <span data-ttu-id="9d87c-159">**Adja hozzá AzureDataDisk** szeretné használni a adattárolás Active Directory, a beállítás set tooNone gyorsítótárazás hello hello adatlemezt ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="9d87c-159">**Add-AzureDataDisk** adds hello data disk that you'll use for storing Active Directory data, with hello caching option set tooNone.</span></span>
   * <span data-ttu-id="9d87c-160">**Új AzureVM** hoz létre egy új felhőalapú szolgáltatás, és hoz létre új Azure virtuális gép hello új felhőszolgáltatásban hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-160">**New-AzureVM** creates a new cloud service and creates hello new Azure VM in hello new cloud service.</span></span>

7. <span data-ttu-id="9d87c-161">Hello teljesen kiépítve új virtuális gép toobe várja meg, és töltse le a távoli asztali fájl tooyour munkakönyvtár hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-161">Wait for hello new VM toobe fully provisioned, and download hello remote desktop file tooyour working directory.</span></span> <span data-ttu-id="9d87c-162">Mert a hello új Azure virtuális gép egy hosszú ideig tooprovision, hello `while` hurkot továbbra is toopoll hello új virtuális Gépet, amíg a használatra kész.</span><span class="sxs-lookup"><span data-stu-id="9d87c-162">Because hello new Azure VM takes a long time tooprovision, hello `while` loop continues toopoll hello new VM until it's ready for use.</span></span>

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

<span data-ttu-id="9d87c-163">most már sikeresen kiépítette hello tartományvezérlő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9d87c-163">hello domain controller server is now successfully provisioned.</span></span> <span data-ttu-id="9d87c-164">A következő hello Active Directory-tartományhoz kell majd konfigurálnia a tartomány a tartományvezérlő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9d87c-164">Next, you'll configure hello Active Directory domain on this domain controller server.</span></span> <span data-ttu-id="9d87c-165">Hagyja hello PowerShell ablakban nyissa meg a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9d87c-165">Leave hello PowerShell window open on your local computer.</span></span> <span data-ttu-id="9d87c-166">Azt ismertetjük, hogy újra újabb toocreate hello két SQL Server virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9d87c-166">You'll use it again later toocreate hello two SQL Server VMs.</span></span>

## <a name="configure-hello-domain-controller"></a><span data-ttu-id="9d87c-167">Hello tartományvezérlő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d87c-167">Configure hello domain controller</span></span>
1. <span data-ttu-id="9d87c-168">Kapcsolódás toohello tartományvezérlő kiszolgálót hello távoli asztali fájl megnyitása.</span><span class="sxs-lookup"><span data-stu-id="9d87c-168">Connect toohello domain controller server by launching hello remote desktop file.</span></span> <span data-ttu-id="9d87c-169">Hello machine felügyeleti AzureAdmin felhasználónévvel és jelszóval **Contoso! 000**, amely létrehozásakor adott új virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-169">Use hello machine administrator’s username AzureAdmin and password **Contoso!000**, which you specified when you created hello new VM.</span></span>
2. <span data-ttu-id="9d87c-170">Nyisson meg egy PowerShell-ablakot rendszergazdai módban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-170">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="9d87c-171">Futtassa a következő hello **DCPROMO. EXE** parancs tooset mentése hello **corp.contoso.com** tartományhoz a hello adatkönyvtárak M-meghajtón.</span><span class="sxs-lookup"><span data-stu-id="9d87c-171">Run hello following **DCPROMO.EXE** command tooset up hello **corp.contoso.com** domain, with hello data directories on drive M.</span></span>

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    <span data-ttu-id="9d87c-172">Hello parancs befejezése után a virtuális gép hello automatikusan újraindul.</span><span class="sxs-lookup"><span data-stu-id="9d87c-172">After hello command finishes, hello VM restarts automatically.</span></span>

4. <span data-ttu-id="9d87c-173">Csatlakozzon újra a tartományvezérlő kiszolgálót toohello hello távoli asztali fájl indításával.</span><span class="sxs-lookup"><span data-stu-id="9d87c-173">Connect toohello domain controller server again by launching hello remote desktop file.</span></span> <span data-ttu-id="9d87c-174">Most, jelentkezzen be a **Corp/rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-174">This time, sign in as **CORP\Administrator**.</span></span>
5. <span data-ttu-id="9d87c-175">Nyisson meg egy PowerShell-ablakot rendszergazdai módban, és hello Active Directory PowerShell-modul importálása a következő parancs hello segítségével:</span><span class="sxs-lookup"><span data-stu-id="9d87c-175">Open a PowerShell window in administrator mode, and import hello Active Directory PowerShell module by using hello following command:</span></span>

        Import-Module ActiveDirectory

6. <span data-ttu-id="9d87c-176">Futtassa a következő parancsok tooadd három felhasználók toohello tartomány hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-176">Run hello following commands tooadd three users toohello domain.</span></span>

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    <span data-ttu-id="9d87c-177">**CORP\Install** van használt tooconfigure minden kapcsolódó toohello SQL kiszolgáló-szolgáltatáspéldány, a hello feladatátvevő fürt és a hello rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="9d87c-177">**CORP\Install** is used tooconfigure everything related toohello SQL Server service instances, hello failover cluster, and hello availability group.</span></span> <span data-ttu-id="9d87c-178">**CORP\SQLSvc1** és **CORP\SQLSvc2** hello két SQL Server virtuális gépen hello SQL Server szolgáltatás fiókok használhatók.</span><span class="sxs-lookup"><span data-stu-id="9d87c-178">**CORP\SQLSvc1** and **CORP\SQLSvc2** are used as hello SQL Server service accounts for hello two SQL Server VMs.</span></span>
7. <span data-ttu-id="9d87c-179">Hello következő, futtassa a következő parancsok toogive **CORP\Install** hello engedélyek toocreate számítógép-objektumok hello tartományban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-179">Next, run hello following commands toogive **CORP\Install** hello permissions toocreate computer objects in hello domain.</span></span>

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    <span data-ttu-id="9d87c-180">a fent megadott GUID hello hello GUID hello számítógép objektumtípus.</span><span class="sxs-lookup"><span data-stu-id="9d87c-180">hello GUID specified above is hello GUID for hello computer object type.</span></span> <span data-ttu-id="9d87c-181">Hello **CORP\Install** fiókot kell hello **összes tulajdonság olvasása** és **számítógép-objektumok létrehozása** engedély toocreate hello aktív közvetlen hello feladatátvételi objektumokhoz fürt.</span><span class="sxs-lookup"><span data-stu-id="9d87c-181">hello **CORP\Install** account needs hello **Read All Properties** and **Create Computer Objects** permission toocreate hello Active Direct objects for hello failover cluster.</span></span> <span data-ttu-id="9d87c-182">Hello **összes tulajdonság olvasása** így nem kell toogrant engedély már kap tooCORP\Install alapértelmezés szerint az explicit módon.</span><span class="sxs-lookup"><span data-stu-id="9d87c-182">hello **Read All Properties** permission is already given tooCORP\Install by default, so you don't need toogrant it explicitly.</span></span> <span data-ttu-id="9d87c-183">További információk az engedélyeket, amelyek toocreate hello feladatátvevő fürt szükséges, a következő témakörben: [feladatátvevő fürt részletes útmutató: fiókok konfigurálása az Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d87c-183">For more information on permissions that are needed toocreate hello failover cluster, see [Failover Cluster Step-by-Step Guide: Configuring Accounts in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).</span></span>

    <span data-ttu-id="9d87c-184">Most, hogy befejezte az Active Directory és a felhasználói objektumok hello, akkor hozhat létre két SQL Server virtuális gépen, és csatlakoztassa azokat toothis tartomány.</span><span class="sxs-lookup"><span data-stu-id="9d87c-184">Now that you've finished configuring Active Directory and hello user objects, you'll create two SQL Server VMs and join them toothis domain.</span></span>

## <a name="create-hello-sql-server-vms"></a><span data-ttu-id="9d87c-185">Hello SQL Server virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d87c-185">Create hello SQL Server VMs</span></span>
1. <span data-ttu-id="9d87c-186">Továbbra is toouse hello PowerShell ablak, amely meg van nyitva, a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9d87c-186">Continue toouse hello PowerShell window that's open on your local computer.</span></span> <span data-ttu-id="9d87c-187">Adja meg a következő további változókat hello:</span><span class="sxs-lookup"><span data-stu-id="9d87c-187">Define hello following additional variables:</span></span>

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    <span data-ttu-id="9d87c-188">IP-cím hello **10.10.0.4** általában hozzá van rendelve a toohello hello a létrehozott első virtuális gép **10.10.0.0/16** alhálózat az Azure virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="9d87c-188">hello IP address **10.10.0.4** is typically assigned toohello first VM that you create in hello **10.10.0.0/16** subnet of your Azure virtual network.</span></span> <span data-ttu-id="9d87c-189">Ellenőrizze, hogy ez az a tartományvezérlő kiszolgálót hello címe futtatásával **IPCONFIG**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-189">You should verify that this is hello address of your domain controller server by running **IPCONFIG**.</span></span>
2. <span data-ttu-id="9d87c-190">Először a következő védőeszközön parancsok toocreate hello futtatási hello hello feladatátvevő fürtben, a virtuális gép nevű **ContosoQuorum**:</span><span class="sxs-lookup"><span data-stu-id="9d87c-190">Run hello following piped commands toocreate hello first VM in hello failover cluster, named **ContosoQuorum**:</span></span>

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    <span data-ttu-id="9d87c-191">Vegye figyelembe a fenti hello parancs vonatkozó hello következőket:</span><span class="sxs-lookup"><span data-stu-id="9d87c-191">Note hello following regarding hello command above:</span></span>

   * <span data-ttu-id="9d87c-192">**Új AzureVMConfig** hoz létre egy Virtuálisgép-konfiguráció hello kívánt rendelkezésre állási készlet neve.</span><span class="sxs-lookup"><span data-stu-id="9d87c-192">**New-AzureVMConfig** creates a VM configuration with hello desired availability set name.</span></span> <span data-ttu-id="9d87c-193">hello további virtuális gépek jön létre hello azonos rendelkezésre állási csoport neve, így azok van csatlakoztatva toohello azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="9d87c-193">hello subsequent VMs will be created with hello same availability set name so that they're joined toohello same availability set.</span></span>
   * <span data-ttu-id="9d87c-194">**Adja hozzá AzureProvisioningConfig** illesztések hello VM toohello Active Directory-tartományban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="9d87c-194">**Add-AzureProvisioningConfig** joins hello VM toohello Active Directory domain that you created.</span></span>
   * <span data-ttu-id="9d87c-195">**Set-AzureSubnet** helyek VM hello hello hátsó alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="9d87c-195">**Set-AzureSubnet** places hello VM in hello back subnet.</span></span>
   * <span data-ttu-id="9d87c-196">**Új AzureVM** hoz létre egy új felhőalapú szolgáltatás, és hoz létre új Azure virtuális gép hello új felhőszolgáltatásban hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-196">**New-AzureVM** creates a new cloud service and creates hello new Azure VM in hello new cloud service.</span></span> <span data-ttu-id="9d87c-197">Hello **DnsSettings** paraméter határozza meg, hogy hello DNS-kiszolgáló, a hello hello új felhőalapú szolgáltatás kiszolgálója hello IP-címmel rendelkezik **10.10.0.4**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-197">hello **DnsSettings** parameter specifies that hello DNS server for hello servers in hello new cloud service has hello IP address **10.10.0.4**.</span></span> <span data-ttu-id="9d87c-198">Ez az IP-címe hello hello tartományvezérlő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9d87c-198">This is hello IP address of hello domain controller server.</span></span> <span data-ttu-id="9d87c-199">Ez a paraméter szükséges tooenable sikeresen hello az új virtuális gépek hello cloud service toojoin toohello Active Directory tartományban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-199">This parameter is needed tooenable hello new VMs in hello cloud service toojoin toohello Active Directory domain successfully.</span></span> <span data-ttu-id="9d87c-200">Ez a paraméter nélkül manuálisan be kell hello IPv4-beállításokkal a virtuális gép toouse hello tartományvezérlő kiszolgálót hello elsődleges DNS-kiszolgálóként után hello virtuális gép ki van építve, és majd a hello VM toohello Active Directory-tartományhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="9d87c-200">Without this parameter, you must manually set hello IPv4 settings in your VM toouse hello domain controller server as hello primary DNS server after hello VM is provisioned, and then join hello VM toohello Active Directory domain.</span></span>
3. <span data-ttu-id="9d87c-201">Futtatási hello következő védőeszközön parancsok toocreate hello SQL Server virtuális gépen, nevű **ContosoSQL1** és **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-201">Run hello following piped commands toocreate hello SQL Server VMs, named **ContosoSQL1** and **ContosoSQL2**.</span></span>

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    <span data-ttu-id="9d87c-202">Vegye figyelembe a fenti hello parancsok vonatkozó hello következőket:</span><span class="sxs-lookup"><span data-stu-id="9d87c-202">Note hello following regarding hello commands above:</span></span>

   * <span data-ttu-id="9d87c-203">**Új AzureVMConfig** használ hello rendelkezésre állási készlet neve megegyezik a hello tartományvezérlő kiszolgálót, és használja az SQL Server 2012 Service Pack 1 Enterprise Edition-lemezképet hello virtuálisgép-katalógus hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-203">**New-AzureVMConfig** uses hello same availability set name as hello domain controller server, and uses hello SQL Server 2012 Service Pack 1 Enterprise Edition image in hello virtual machine gallery.</span></span> <span data-ttu-id="9d87c-204">Operációs rendszer lemez tooread gyorsítótárazás csak (nincs írási gyorsítótárazást) hello állítja.</span><span class="sxs-lookup"><span data-stu-id="9d87c-204">It also sets hello operating system disk tooread-caching only (no write caching).</span></span> <span data-ttu-id="9d87c-205">Azt javasoljuk, hogy az áttelepített hello adatbázis fájlok tooa külön adatlemez, hogy csatolása toohello virtuális gép, és állítson be nincs olvasási vagy írási gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="9d87c-205">We recommend that you migrate hello database files tooa separate data disk that you attach toohello VM, and configure it with no read or write caching.</span></span> <span data-ttu-id="9d87c-206">Hello következő ajánlott dolog azonban tooremove írási gyorsítótárazást az hello operációsrendszer-lemez, mert nem távolítható el, olvassa el hello operációsrendszer-lemez gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="9d87c-206">However, hello next best thing is tooremove write caching on hello operating system disk because you can't remove read caching on hello operating system disk.</span></span>
   * <span data-ttu-id="9d87c-207">**Adja hozzá AzureProvisioningConfig** illesztések hello VM toohello Active Directory-tartományban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="9d87c-207">**Add-AzureProvisioningConfig** joins hello VM toohello Active Directory domain that you created.</span></span>
   * <span data-ttu-id="9d87c-208">**Set-AzureSubnet** helyek VM hello hello hátsó alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="9d87c-208">**Set-AzureSubnet** places hello VM in hello back subnet.</span></span>
   * <span data-ttu-id="9d87c-209">**Adja hozzá AzureEndpoint** beveszi hozzáférés végpontok, így az ügyfélalkalmazások férhetnek hozzá a hello Internet ezeket az SQL Server services-példányok.</span><span class="sxs-lookup"><span data-stu-id="9d87c-209">**Add-AzureEndpoint** adds access endpoints so that client applications can access these SQL Server services instances on hello Internet.</span></span> <span data-ttu-id="9d87c-210">Különböző portok tooContosoSQL1 és ContosoSQL2 kap.</span><span class="sxs-lookup"><span data-stu-id="9d87c-210">Different ports are given tooContosoSQL1 and ContosoSQL2.</span></span>
   * <span data-ttu-id="9d87c-211">**Új AzureVM** hoz létre új SQL Server virtuális gép hello a hello ugyanaz a felhőalapú szolgáltatás, ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="9d87c-211">**New-AzureVM** creates hello new SQL Server VM in hello same cloud service as ContosoQuorum.</span></span> <span data-ttu-id="9d87c-212">Be kell jelölnie hello virtuális gépek ugyanazt a felhőalapú szolgáltatás, ha azt szeretné, a toobe hello hello azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="9d87c-212">You must place hello VMs in hello same cloud service if you want them toobe in hello same availability set.</span></span>
4. <span data-ttu-id="9d87c-213">Várjon, amíg minden virtuális gép toobe teljesen kiépítve pedig minden virtuális gép toodownload a távoli asztali fájl tooyour működő könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="9d87c-213">Wait for each VM toobe fully provisioned and for each VM toodownload its remote desktop file tooyour working directory.</span></span> <span data-ttu-id="9d87c-214">Hello `for` hurok váltás hello három új virtuális gépek és hello legfelső szintű kerek zárójeleket belül hello parancsok végrehajtása a esetében.</span><span class="sxs-lookup"><span data-stu-id="9d87c-214">hello `for` loop cycles through hello three new VMs and executes hello commands inside hello top-level curly brackets for each of them.</span></span>

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until hello VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    <span data-ttu-id="9d87c-215">most már kiépített hello SQL Server virtuális gépen, és fut, de van telepítve az SQL Server alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="9d87c-215">hello SQL Server VMs are now provisioned and running, but they're installed with SQL Server with default options.</span></span>

## <a name="initialize-hello-failover-cluster-vms"></a><span data-ttu-id="9d87c-216">Hello feladatátvevő fürt virtuális gépek inicializálása</span><span class="sxs-lookup"><span data-stu-id="9d87c-216">Initialize hello failover cluster VMs</span></span>
<span data-ttu-id="9d87c-217">Ebben a szakaszban kell toomodify hello három kiszolgálót, amelyen le fogja hello feladatátvevő fürt és hello SQL Server telepítése.</span><span class="sxs-lookup"><span data-stu-id="9d87c-217">In this section, you need toomodify hello three servers that you'll use in hello failover cluster and hello SQL Server installation.</span></span> <span data-ttu-id="9d87c-218">Konkrétan:</span><span class="sxs-lookup"><span data-stu-id="9d87c-218">Specifically:</span></span>

* <span data-ttu-id="9d87c-219">Minden kiszolgáló: tooinstall hello kell **feladatátvételi fürtszolgáltatás** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9d87c-219">All servers: You need tooinstall hello **Failover Clustering** feature.</span></span>
* <span data-ttu-id="9d87c-220">Minden kiszolgáló: tooadd kell **CORP\Install** hello gépként **rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-220">All servers: You need tooadd **CORP\Install** as hello machine **administrator**.</span></span>
* <span data-ttu-id="9d87c-221">ContosoSQL1 és ContosoSQL2 csak: tooadd kell **CORP\Install** , egy **sysadmin** hello alapértelmezett adatbázis szerepköréhez.</span><span class="sxs-lookup"><span data-stu-id="9d87c-221">ContosoSQL1 and ContosoSQL2 only: You need tooadd **CORP\Install** as a **sysadmin** role in hello default database.</span></span>
* <span data-ttu-id="9d87c-222">ContosoSQL1 és ContosoSQL2 csak: tooadd kell **NT AUTHORITY\System** , a bejelentkezés az alábbi engedélyek használata hello:</span><span class="sxs-lookup"><span data-stu-id="9d87c-222">ContosoSQL1 and ContosoSQL2 only: You need tooadd **NT AUTHORITY\System** as a sign-in with hello following permissions:</span></span>

  * <span data-ttu-id="9d87c-223">Az ALTER bármely rendelkezésre állási csoport</span><span class="sxs-lookup"><span data-stu-id="9d87c-223">Alter any availability group</span></span>
  * <span data-ttu-id="9d87c-224">Csatlakozás SQL</span><span class="sxs-lookup"><span data-stu-id="9d87c-224">Connect SQL</span></span>
  * <span data-ttu-id="9d87c-225">Kiszolgáló állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="9d87c-225">View server state</span></span>
* <span data-ttu-id="9d87c-226">ContosoSQL1 és ContosoSQL2 csak: hello **TCP** protokoll már engedélyezve van az SQL Server virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="9d87c-226">ContosoSQL1 and ContosoSQL2 only: hello **TCP** protocol is already enabled on hello SQL Server VM.</span></span> <span data-ttu-id="9d87c-227">Azonban továbbra is szükség tooopen hello tűzfal az SQL Server távoli hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="9d87c-227">However, you still need tooopen hello firewall for remote access of SQL Server.</span></span>

<span data-ttu-id="9d87c-228">Most hamarosan kész toostart.</span><span class="sxs-lookup"><span data-stu-id="9d87c-228">Now, you're ready toostart.</span></span> <span data-ttu-id="9d87c-229">Kezdve **ContosoQuorum**, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9d87c-229">Beginning with **ContosoQuorum**, follow hello steps below:</span></span>

1. <span data-ttu-id="9d87c-230">Csatlakozás túl**ContosoQuorum** hello távoli asztali fájlok indításával.</span><span class="sxs-lookup"><span data-stu-id="9d87c-230">Connect too**ContosoQuorum** by launching hello remote desktop files.</span></span> <span data-ttu-id="9d87c-231">Adjon hello gép rendszergazdai jogosultságú felhasználónevet **AzureAdmin** és a jelszó **Contoso! 000**, a megadott hello virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="9d87c-231">Use hello machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created hello VMs.</span></span>
2. <span data-ttu-id="9d87c-232">Győződjön meg arról, hogy hello számítógépek rendelkeznek sikeresen csatlakoztatva lett túl**corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-232">Verify that hello computers have been successfully joined too**corp.contoso.com**.</span></span>
3. <span data-ttu-id="9d87c-233">Várjon, amíg hello SQL Server telepítési toofinish futó automatikus hello inicializálási feladat folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="9d87c-233">Wait for hello SQL Server installation toofinish running hello automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="9d87c-234">Nyisson meg egy PowerShell-ablakot rendszergazdai módban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-234">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="9d87c-235">Hello Windows feladatátvételi fürtszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="9d87c-235">Install hello Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="9d87c-236">Adja hozzá **CORP\Install** helyi rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9d87c-236">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="9d87c-237">Jelentkezzen ki ContosoQuorum.</span><span class="sxs-lookup"><span data-stu-id="9d87c-237">Sign out of ContosoQuorum.</span></span> <span data-ttu-id="9d87c-238">Befejezte a kiszolgáló most.</span><span class="sxs-lookup"><span data-stu-id="9d87c-238">You're done with this server now.</span></span>

        logoff.exe

<span data-ttu-id="9d87c-239">A következő inicializálása **ContosoSQL1** és **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-239">Next, initialize **ContosoSQL1** and **ContosoSQL2**.</span></span> <span data-ttu-id="9d87c-240">Kövesse hello lépéseket, mindkét SQL Server virtuális gépek azonos.</span><span class="sxs-lookup"><span data-stu-id="9d87c-240">Follow hello steps below, which are identical for both SQL Server VMs.</span></span>

1. <span data-ttu-id="9d87c-241">Kapcsolódás toohello két SQL Server VMs hello távoli asztali fájlok megnyitása.</span><span class="sxs-lookup"><span data-stu-id="9d87c-241">Connect toohello two SQL Server VMs by launching hello remote desktop files.</span></span> <span data-ttu-id="9d87c-242">Adjon hello gép rendszergazdai jogosultságú felhasználónevet **AzureAdmin** és a jelszó **Contoso! 000**, a megadott hello virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="9d87c-242">Use hello machine administrator’s username **AzureAdmin** and password **Contoso!000**, which you specified when you created hello VMs.</span></span>
2. <span data-ttu-id="9d87c-243">Győződjön meg arról, hogy hello számítógépek rendelkeznek sikeresen csatlakoztatva lett túl**corp.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-243">Verify that hello computers have been successfully joined too**corp.contoso.com**.</span></span>
3. <span data-ttu-id="9d87c-244">Várjon, amíg hello SQL Server telepítési toofinish futó automatikus hello inicializálási feladat folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="9d87c-244">Wait for hello SQL Server installation toofinish running hello automated initialization tasks before proceeding.</span></span>
4. <span data-ttu-id="9d87c-245">Nyisson meg egy PowerShell-ablakot rendszergazdai módban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-245">Open a PowerShell window in administrator mode.</span></span>
5. <span data-ttu-id="9d87c-246">Hello Windows feladatátvételi fürtszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="9d87c-246">Install hello Windows Failover Clustering feature.</span></span>

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. <span data-ttu-id="9d87c-247">Adja hozzá **CORP\Install** helyi rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9d87c-247">Add **CORP\Install** as a local administrator.</span></span>

        net localgroup administrators "CORP\Install" /Add
7. <span data-ttu-id="9d87c-248">Importálás hello SQL-kiszolgáló PowerShell-szolgáltatóban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-248">Import hello SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. <span data-ttu-id="9d87c-249">Adja hozzá **CORP\Install** , hello SysAdmin (rendszergazda) szerepkör hello alapértelmezett SQL Server-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="9d87c-249">Add **CORP\Install** as hello sysadmin role for hello default SQL Server instance.</span></span>

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. <span data-ttu-id="9d87c-250">Adja hozzá **NT AUTHORITY\System** , a bejelentkezés a fent leírt három hello engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="9d87c-250">Add **NT AUTHORITY\System** as a sign-in with hello three permissions described above.</span></span>

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. <span data-ttu-id="9d87c-251">Nyissa meg az SQL Server távoli hozzáféréshez hello tűzfalat.</span><span class="sxs-lookup"><span data-stu-id="9d87c-251">Open hello firewall for remote access of SQL Server.</span></span>

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. <span data-ttu-id="9d87c-252">Jelentkezzen ki mindkét virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="9d87c-252">Sign out of both VMs.</span></span>

         logoff.exe

<span data-ttu-id="9d87c-253">Végül hamarosan kész tooconfigure hello rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="9d87c-253">Finally, you're ready tooconfigure hello availability group.</span></span> <span data-ttu-id="9d87c-254">Hello SQL-kiszolgáló PowerShell-szolgáltatóban tooperform összes hello működik, a használt **ContosoSQL1**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-254">You'll use hello SQL Server PowerShell Provider tooperform all of hello work on **ContosoSQL1**.</span></span>

## <a name="configure-hello-availability-group"></a><span data-ttu-id="9d87c-255">Hello rendelkezésre állási csoport konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d87c-255">Configure hello availability group</span></span>
1. <span data-ttu-id="9d87c-256">Csatlakozás túl**ContosoSQL1** újra hello távoli asztali fájlok indításával.</span><span class="sxs-lookup"><span data-stu-id="9d87c-256">Connect too**ContosoSQL1** again by launching hello remote desktop files.</span></span> <span data-ttu-id="9d87c-257">Hello gép fiókkal történő bejelentkezés helyett használatával írja alá **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-257">Instead of signing in by using hello machine account, sign in by using **CORP\Install**.</span></span>
2. <span data-ttu-id="9d87c-258">Nyisson meg egy PowerShell-ablakot rendszergazdai módban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-258">Open a PowerShell window in administrator mode.</span></span>
3. <span data-ttu-id="9d87c-259">Adja meg a következő változók hello:</span><span class="sxs-lookup"><span data-stu-id="9d87c-259">Define hello following variables:</span></span>

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. <span data-ttu-id="9d87c-260">Importálás hello SQL-kiszolgáló PowerShell-szolgáltatóban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-260">Import hello SQL Server PowerShell Provider.</span></span>

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. <span data-ttu-id="9d87c-261">Hello SQL Server szolgáltatás fiókjának ContosoSQL1 tooCORP\SQLSvc1 módosítása.</span><span class="sxs-lookup"><span data-stu-id="9d87c-261">Change hello SQL Server service account for ContosoSQL1 tooCORP\SQLSvc1.</span></span>

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. <span data-ttu-id="9d87c-262">Hello SQL Server szolgáltatás fiókjának ContosoSQL2 tooCORP\SQLSvc2 módosítása.</span><span class="sxs-lookup"><span data-stu-id="9d87c-262">Change hello SQL Server service account for ContosoSQL2 tooCORP\SQLSvc2.</span></span>

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. <span data-ttu-id="9d87c-263">Töltse le **CreateAzureFailoverCluster.ps1** a [feladatátvevő fürt létrehozása az Always On rendelkezésre állási csoportok Azure virtuális gép](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello helyi munkakönyvtár.</span><span class="sxs-lookup"><span data-stu-id="9d87c-263">Download **CreateAzureFailoverCluster.ps1** from [Create Failover Cluster for Always On Availability Groups in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello local working directory.</span></span> <span data-ttu-id="9d87c-264">A parancsfájl toohelp működő feladatátvevő fürt létrehozása fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9d87c-264">You'll use this script toohelp you create a functional failover cluster.</span></span> <span data-ttu-id="9d87c-265">Fontos információk a Windows feladatátvételi fürtszolgáltatás és együttműködését hello Azure hálózati, lásd: [az SQL Server Azure virtuális gépek magas rendelkezésre állási és vészhelyreállítási helyreállítási](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9d87c-265">For important information on how Windows Failover Clustering interacts with hello Azure network, see [High availability and disaster recovery for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>
8. <span data-ttu-id="9d87c-266">Tooyour munkakönyvtár megváltoztatására, és hozzon létre hello feladatátvevő fürt letöltött hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="9d87c-266">Change tooyour working directory and create hello failover cluster with hello downloaded script.</span></span>

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. <span data-ttu-id="9d87c-267">Engedélyezze az Always On rendelkezésre állási csoportok hello alapértelmezett SQL Server-példányok a **ContosoSQL1** és **ContosoSQL2**.</span><span class="sxs-lookup"><span data-stu-id="9d87c-267">Enable Always On availability groups for hello default SQL Server instances on **ContosoSQL1** and **ContosoSQL2**.</span></span>

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. <span data-ttu-id="9d87c-268">Hozzon létre egy biztonsági mentési könyvtárat, és adja meg az SQL Server szolgáltatásfiókok hello engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="9d87c-268">Create a backup directory and grant permissions for hello SQL Server service accounts.</span></span> <span data-ttu-id="9d87c-269">A könyvtár tooprepare hello rendelkezésre állási adatbázis hello másodlagos másodpéldányon fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9d87c-269">You'll use this directory tooprepare hello availability database on hello secondary replica.</span></span>

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. <span data-ttu-id="9d87c-270">Adatbázis létrehozása a **ContosoSQL1** nevű **MyDB1**, teljes biztonsági mentés és a napló biztonsági mentését is igénybe vehet, és visszaállítja ezeket **ContosoSQL2** a hello **WITH NORECOVERY** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9d87c-270">Create a database on **ContosoSQL1** called **MyDB1**, take both a full backup and a log backup, and restore them on **ContosoSQL2** with hello **WITH NORECOVERY** option.</span></span>

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. <span data-ttu-id="9d87c-271">Az SQL Server VMs hello hello rendelkezésre állási csoport végpontokat hoz létre, és adja meg a hello megfelelő engedélyeket hello végpontokon.</span><span class="sxs-lookup"><span data-stu-id="9d87c-271">Create hello availability group endpoints on hello SQL Server VMs and set hello proper permissions on hello endpoints.</span></span>

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct1]" -ServerInstance $server2
13. <span data-ttu-id="9d87c-272">Hozzon létre hello rendelkezésre állási másodpéldányok.</span><span class="sxs-lookup"><span data-stu-id="9d87c-272">Create hello availability replicas.</span></span>

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. <span data-ttu-id="9d87c-273">Végezetül hozza létre a hello rendelkezésre állási csoport és a csatlakozás hello másodlagos replika toohello a rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="9d87c-273">Finally, create hello availability group and join hello secondary replica toohello availability group.</span></span>

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a><span data-ttu-id="9d87c-274">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d87c-274">Next steps</span></span>
<span data-ttu-id="9d87c-275">Ön már most sikeresen megvalósítva SQL Server Always On rendelkezésre állási csoport létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="9d87c-275">You've now successfully implemented SQL Server Always On by creating an availability group in Azure.</span></span> <span data-ttu-id="9d87c-276">a rendelkezésre állási csoport egy figyelő tooconfigure lásd [egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="9d87c-276">tooconfigure a listener for this availability group, see [Configure an ILB listener for Always On availability groups in Azure](../classic/ps-sql-int-listener.md).</span></span>

<span data-ttu-id="9d87c-277">Egyéb Azure-ban az SQL Server használatával kapcsolatos információkért lásd: [Azure virtuális gépeken futó SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9d87c-277">For other information about using SQL Server in Azure, see [SQL Server on Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
