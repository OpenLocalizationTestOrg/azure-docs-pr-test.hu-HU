---
title: "egy SQL Server virtuális gép (klasszikus) Azure PowerShell aaaCreate |} Microsoft Docs"
description: "Lépéseket és a PowerShell-parancsfájlok biztosít az Azure virtuális gép létrehozása az SQL Server virtuális gép a gyűjtemény lemezképei. Ez a témakör hello klasszikus üzembe helyezési módot használ."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: b14d5d9bc192fa0a21126395ee20ffd06b3bf47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a><span data-ttu-id="3a0f0-104">Egy SQL Server rendszerű virtuális gép (klasszikus) Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="3a0f0-104">Provision a SQL Server virtual machine using Azure PowerShell (Classic)</span></span>

<span data-ttu-id="3a0f0-105">Ez a cikk a hogyan toocreate SQL Server virtuális gépen az Azure használatával hello PowerShell-parancsmagok lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-105">This article provides steps for how toocreate a SQL Server virtual machine in Azure by using hello PowerShell cmdlets.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3a0f0-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3a0f0-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="3a0f0-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="3a0f0-109">Ez a témakör hello erőforrás-kezelő verziója: [egy SQL Server rendszerű virtuális gép Azure PowerShell Resource Manager használatával](../sql/virtual-machines-windows-ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-109">For hello Resource Manager version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).</span></span>

### <a name="install-and-configure-powershell"></a><span data-ttu-id="3a0f0-110">PowerShell telepítése és konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="3a0f0-110">Install and configure PowerShell:</span></span>
1. <span data-ttu-id="3a0f0-111">Ha nem rendelkezik Azure-fiókkal, az [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/) biztosít.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-111">If you do not have an Azure account, visit [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
2. <span data-ttu-id="3a0f0-112">[Töltse le és telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-112">[Download and install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>
3. <span data-ttu-id="3a0f0-113">Indítsa el a Windows Powershellt, valamint elérheti az Azure-előfizetés tooyour hello **Add-AzureAccount** parancsot.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-113">Start Windows PowerShell, and connect it tooyour Azure subscription with hello **Add-AzureAccount** command.</span></span>

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a><span data-ttu-id="3a0f0-114">Határozza meg a cél Azure-régió</span><span class="sxs-lookup"><span data-stu-id="3a0f0-114">Determine your target Azure region</span></span>

<span data-ttu-id="3a0f0-115">Az SQL Server virtuális gépen egy felhőalapú szolgáltatás, amely egy adott Azure-régióban található fog üzemelni.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-115">Your SQL Server Virtual Machine will be hosted in a cloud service that resides a specific Azure region.</span></span> <span data-ttu-id="3a0f0-116">hello következő lépések segítségével meg toodetermine a régió, a tárfiók és a felhőszolgáltatás, amely jelzi a hello rest hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-116">hello following steps help you toodetermine your region, storage account, and cloud service that will be used for hello rest of hello tutorial.</span></span>

1. <span data-ttu-id="3a0f0-117">Határozza meg, amelyet toouse toohost az SQL Server virtuális gép hello adatközpont.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-117">Determine hello data center that you want toouse toohost your SQL Server VM.</span></span> <span data-ttu-id="3a0f0-118">a következő PowerShell-paranccsal hello rendelkezésre álló terület neveinek listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-118">hello following PowerShell command displays a list of available region names.</span></span>

   ```powershell
   (Get-AzureLocation).Name
   ```

2. <span data-ttu-id="3a0f0-119">Miután az elsődleges hely állapította meg, állítsa be a nevű változó **$dcLocation** toothat régióban.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-119">Once you've identified your preferred location, set a variable named **$dcLocation** toothat region.</span></span> <span data-ttu-id="3a0f0-120">Például a készlet túl hello régió parancs a következő hello "USA keleti régiója":</span><span class="sxs-lookup"><span data-stu-id="3a0f0-120">For example, hello following command sets hello region too"East US":</span></span>

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a><span data-ttu-id="3a0f0-121">Az előfizetés és a tárolási fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="3a0f0-121">Set your subscription and storage account</span></span>

1. <span data-ttu-id="3a0f0-122">Határozza meg, hogy hello Azure-előfizetés hello új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-122">Determine hello Azure subscription you will use for hello new virtual machine.</span></span>

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. <span data-ttu-id="3a0f0-123">Rendelje hozzá a cél Azure-előfizetés toohello **$subscr** változó.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-123">Assign your target Azure subscription toohello **$subscr** variable.</span></span> <span data-ttu-id="3a0f0-124">Ezután állítsa be a jelenlegi Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-124">Then set this as your current Azure subscription.</span></span>

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. <span data-ttu-id="3a0f0-125">Ezután ellenőrizze a meglévő tárfiókok.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-125">Then check for existing storage accounts.</span></span> <span data-ttu-id="3a0f0-126">hello következő parancsfájlja felsorolja az összes tárfiók, amely szerepel a kiválasztott régióban:</span><span class="sxs-lookup"><span data-stu-id="3a0f0-126">hello following script displays all storage accounts that exist in your chosen region:</span></span>

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > <span data-ttu-id="3a0f0-127">Új tárfiók szükséges, ha először létre kell hoznia egy kisbetű minden tárfiók neve a New-AzureStorageAccount hello paranccsal beállítani, mint például a következő hello:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span><span class="sxs-lookup"><span data-stu-id="3a0f0-127">If you require a new storage account, first create an all-lower-case storage account name with hello New-AzureStorageAccount command as in hello following example: `New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span></span>

4. <span data-ttu-id="3a0f0-128">Rendelje hozzá a hello cél tárolási fiók neve toohello **$staccount**.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-128">Assign hello target storage account name toohello **$staccount**.</span></span> <span data-ttu-id="3a0f0-129">Ezután **Set-AzureSubscription** tooset hello előfizetés és az aktuális tárfiók.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-129">Then use **Set-AzureSubscription** tooset hello subscription and current storage account.</span></span>

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a><span data-ttu-id="3a0f0-130">Egy SQL Server virtuálisgép-lemezkép kiválasztása</span><span class="sxs-lookup"><span data-stu-id="3a0f0-130">Select a SQL Server virtual machine image</span></span>

1. <span data-ttu-id="3a0f0-131">Ismerje meg az elérhető SQL Server virtuális gépek rendszerkép hello gyűjteményből hello listája.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-131">Find out hello list of available SQL Server virtual machines images from hello gallery.</span></span> <span data-ttu-id="3a0f0-132">Ezeket a lemezképeket mindegyik rendelkezik egy **ImageFamily** "SQL" kezdetű tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-132">These images all have an **ImageFamily** property that starts with "SQL".</span></span> <span data-ttu-id="3a0f0-133">hello következő jeleníti meg hello kép termékcsalád elérhető tooyou előre telepített SQL Server rendelkező lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-133">hello following query displays hello image family available tooyou that have SQL Server preinstalled.</span></span>

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. <span data-ttu-id="3a0f0-134">Ha megtalálta hello virtuálisgép-lemezkép termékcsalád, lehet több közzétett lemezképet a családban.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-134">When you find hello  virtual machine image family, there could be multiple published images in this family.</span></span> <span data-ttu-id="3a0f0-135">Használjon hello alábbi parancsfájl toofind hello legújabb közzétett virtuális géphez tartozó kép neve a kiválasztott kép családba (például **SQL Server 2016 Windows Server 2012 R2 RTM Enterprise**):</span><span class="sxs-lookup"><span data-stu-id="3a0f0-135">Use hello following script toofind hello latest published virtual machine image name for your selected image family (such as **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2**):</span></span>

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-hello-virtual-machine"></a><span data-ttu-id="3a0f0-136">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a0f0-136">Create hello virtual machine</span></span>

<span data-ttu-id="3a0f0-137">Végezetül hozza létre a hello virtuális gépet, a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="3a0f0-137">Finally, create hello virtual machine with PowerShell:</span></span>

1. <span data-ttu-id="3a0f0-138">Hozzon létre egy felhőalapú szolgáltatás toohost hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-138">Create a cloud service toohost hello new VM.</span></span> <span data-ttu-id="3a0f0-139">Vegye figyelembe, hogy az is lehetséges toouse egy meglévő felhőszolgáltatáshoz helyette.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-139">Note that it is also possible toouse an existing cloud service instead.</span></span> <span data-ttu-id="3a0f0-140">Hozzon létre egy új változót **$svcname** hello rövid névvel hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-140">Create a new variable **$svcname** with hello short name of hello cloud service.</span></span>

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. <span data-ttu-id="3a0f0-141">Adja meg a hello virtuális gép nevét és méretét.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-141">Specify hello virtual machine name and a size.</span></span> <span data-ttu-id="3a0f0-142">További információ a virtuálisgép-méretek: [Azure virtuálisgép-méretek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-142">For more information about virtual machine sizes, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see hello link toohello other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. <span data-ttu-id="3a0f0-143">Adja meg a hello helyi rendszergazda fiókot és jelszót.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-143">Specify hello local administrator account and password.</span></span>

   ```powershell
   $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. <span data-ttu-id="3a0f0-144">Futtassa a következő parancsfájl toocreate hello virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-144">Run hello following script toocreate hello virtual machine.</span></span>

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> <span data-ttu-id="3a0f0-145">További magyarázat és konfigurációs beállítások: hello **összeállítása a parancskészlethez** szakasz [használata Azure PowerShell toocreate és előre konfigurálása a Windows-alapú virtuális gépek](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-145">For additional explanation and configuration options, see hello **Build your command set** section in [Use Azure PowerShell toocreate and preconfigure Windows-based Virtual Machines](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="example-powershell-script"></a><span data-ttu-id="3a0f0-146">Példa PowerShell-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="3a0f0-146">Example PowerShell script</span></span>

<span data-ttu-id="3a0f0-147">hello következő parancsfájl egy példát a teljes parancsfájl, amely létrehoz egy **SQL Server 2016 Windows Server 2012 R2 RTM Enterprise** virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-147">hello following script provides an example of a complete script that creates a **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2** virtual machine.</span></span> <span data-ttu-id="3a0f0-148">Ha ezt a parancsfájlt használja, akkor testre kell szabnia hello kezdeti változók hello előző témakörben leírt lépések alapján.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-148">If you use this script, you must customize hello initial variables based on hello previous steps in this topic.</span></span>

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set hello current subscription and storage account
# Comment out hello New-AzureStorageAccount line if hello account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select hello most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create hello new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create hello VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create hello SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a><span data-ttu-id="3a0f0-149">Csatlakozzon a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="3a0f0-149">Connect with remote desktop</span></span>

1. <span data-ttu-id="3a0f0-150">Hozzon létre hello RDP-fájlok hello aktuális felhasználó Dokumentumok mappa toolaunch ezen virtuális gépek toocomplete beállítása:</span><span class="sxs-lookup"><span data-stu-id="3a0f0-150">Create hello RDP files in hello current user's document folder toolaunch these virtual machines toocomplete setup:</span></span>

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. <span data-ttu-id="3a0f0-151">Hello Dokumentumok mappájába indítsa el a hello RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-151">In hello documents directory, launch hello RDP file.</span></span> <span data-ttu-id="3a0f0-152">Csatlakozás hello rendszergazda felhasználónevet és jelszót adott meg a korábban (például ha a felhasználónév VMAdmin, adja meg a "\VMAdmin" hello felhasználói és hello jelszót).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-152">Connect with hello administrator user name and password provided earlier (for example, if your user name was VMAdmin, specify "\VMAdmin" as hello user and provide hello password).</span></span>

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-hello-configuration-of-hello-sql-server-machine-for-remote-access"></a><span data-ttu-id="3a0f0-153">SQL Server számítógép a távoli hozzáféréshez hello teljes hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3a0f0-153">Complete hello configuration of hello SQL Server Machine for remote access</span></span>

<span data-ttu-id="3a0f0-154">A bejelentkezés után a toohello számítógép a távoli asztal konfigurálása hello utasításait SQL Serveren [egy Azure virtuális gép az SQL Server-kapcsolat beállításának lépései](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-154">After logging on toohello machine with remote desktop, configure SQL Server based on hello instructions in [Steps for configuring SQL Server connectivity in an Azure VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a0f0-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a0f0-155">Next steps</span></span>

<span data-ttu-id="3a0f0-156">További utasítások hello a PowerShell használatával a virtuális gépek rendszerbe állításához található [virtual machines – dokumentáció](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-156">You can find additional instructions for provisioning virtual machines with PowerShell in hello [virtual machines documentation](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="3a0f0-157">Sok esetben hello következő lépésre toomigrate az adatbázisok toothis új SQL Server virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-157">In many cases, hello next step is toomigrate your databases toothis new SQL Server VM.</span></span> <span data-ttu-id="3a0f0-158">Adatbázis-áttelepítési útmutatóért lásd: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-158">For database migration guidance, see [Migrating a Database tooSQL Server on an Azure VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="3a0f0-159">Ha is szeretné használni az Azure portál toocreate SQL virtuális gépek hello, lásd: [Azure SQL Server virtuális gépek kiépítése](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-159">If you're also interested in using hello Azure portal toocreate SQL Virtual Machines, see [Provisioning a SQL Server Virtual Machine on Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="3a0f0-160">Vegye figyelembe, hogy, hogy ajánlott Resource Manager modellt, nem pedig hello klasszikus modellt használja a PowerShell témakör bemutatja, hogyan hello portálon keresztül akkor hoz létre a virtuális gépek hello hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3a0f0-160">Note that hello tutorial that walks you through hello portal creates VMs using hello recommended Resource Manager model, rather than hello classic model used in this PowerShell topic.</span></span>

<span data-ttu-id="3a0f0-161">Ezenkívül toothese erőforrások, javasoljuk, hogy tekintse át [más témakörök kapcsolódó SQL Server Azure virtuális gépek toorunning](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3a0f0-161">In addition toothese resources, we recommend that you review [other topics related toorunning SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
